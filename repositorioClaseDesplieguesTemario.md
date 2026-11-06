Repositorio clase Despliegues
→ git@github.com:monium/gitanddocs.git

(Sacar toda la documentacion del repo y crear documentacion local completa y super detallada, con ejemplo, explicaciones de todos los elementos, usos, definiciones, ubicaciones de elementos, analizar el codigo y explicaciones de todas las carpetas del repositorio del profesor, sacar infor completa y detallada de TODO. Usando tambien la informacion de la pagina del profesor con todo el temario dado y por dar *https://dpl-f1facc.gitlab.io/*. Mirar en todas las ramas del repo tambien y mirar los issues y pull request en mi github para ver que cosas estan por implementar y NO hacerlo para publicarlo en el repo del profe sino para implementarlo en la documentacion MIA de mi repositoriop de despliegues. Haz lo mismo con los commits, ver los cambios del repositorio para entender cambios que han ocurrido, estado del principio y actual por si estos cambios pueden explicarse con alguna parte del temario. "Esto se cambio porque al principio se iba a hacer una cosa por el temario y para cosas futuras relacionadas con el temario se hizo esto")
(Revisar las actividades hechas para ver lo que hemos hecho con ssh y nginx)
(IR poco a poco un dia haciendo todas las practicas de la pagina del profesor *https://dpl-f1facc.gitlab.io/* de los temas 1 y 2

1:

	Práctica guiada — Ramas, merges y flujos de trabajo

	Práctica guiada — CI/CD con GitHub Actions

2:

	Práctica guiada — nginx en contenedor
	
	Práctica guiada — Calculadora Python con nginx y Gunicorn

	Práctica guiada — Calculadora PHP con nginx y PHP-FPM

	Práctica guiada — Red, logs y diagnóstico

	Práctica guiada — Terraform y AWS

3:

	Práctica guiada — Server blocks (hosts virtuales)

	Práctica guiada — HTTPS con certificado autofirmado -> Posible actividad entregable para la semana que viene

	Práctica guiada — Módulos de nginx: procesado de imágenes con

	Práctica guiada — Server blocks con autenticación

	Práctica guiada — HTTPS con autenticación

	Práctica guiada — HTTP/2 y HTTP/3 en nginx

(Si alguna ya se ha pedido para entregar a clase la saltamos. Por tenerlas como documentacion, si se pueden hacer dentro del repositorio mio las hacemos, cada una con su carpeta o zona y cuando tengamos que hacer una actividad entregable lo sacamos y hacemos poco a poco como hemos echo las otras dos ya entregadas(con pasos poco a poco y commits y readme.md) y hacemos un repo separado como hemos hecho)
)
(Hacer todo esto para tener un .md del temario completo en el repositorio mio personal de despliegues. Añadir a la automatizacion que tambien se comprube el repo del profesor para ver si hubieron cambios y traerlos a la documentacion)

(El texto a continuacion es de explicaciones de clase. Adaptar al temario nuevo)

Oracle Cloud

-> file /etc/hosts

## ORACLE OCI
	ip1 máquina 1
	ip2 máquina 2

ssh nombre(sistema de la maquina)@nombre(de la maquina)
enter pass: xxxxx

nombre@nombre:~$

kamal -> automatizar a la máquinas remotas.
    • Entra por ssh y si no lo tienes lo instala.

descarga en el anfitrión para podes desplegar solo
descargar rubi con rbenv

crear carpeta

clonar repo profe. El proyecto esta en /tasks-api/ dentor del repo.
kamal init

en el repo se añadio JWT en el repositorio

sniffing el intercambio de llaves de acceso y cifrado hash

explicar jwt y como funciona en el repositorio. Explica en detalle cada parte de su proceso y componentes, JWT DEBUGGER, los datos que salen y decir como fucniona todo. 

swt debugger, decoder, encoder

Generate example -> muchos algoritmos de hashing

Decoded header
json
	alg: HS256
	typ: JWT

Decoded payload
json
	sub: 1234567890
	name: "John Doe"
	admin: true
	iat: 1516239022
	exp: 1516939022

jwt signature verification
>_Secret
	a-string-secrete-adt-least-256-bits-long

Valid JWT
Sifnature Verified / Signature ...

nos genera un tocken y lo guardamos.

Lo mismo con la private key >_sign jwt Private key

esto sirve para: En la web tasks del repo, ahora para hacer los metodos como POST /auth/register/ -> curl POST -H -H -d info en json -> (sin esguridad scl en nginx sale la info en crudo. Hay que cifrarla. HTTP vs HTTPS) (no se debe guardar contraseña en bbdd en el back tal cual. Se guarda hasheada y cifrada a tope. EN el caso del profe usa *ARGON* para cifrar). el POST devuelve id, username y created_at pero no la password. 
tras guardarse vamos al otro endpoint de inicio de sesion. curl POST -H -H -d en crudo de nuevo. Si los hashes son iguales -> Access token: xxxxxxxxxx.....xxxxxx.  Si no -> Usuario o contraseña incorrecto

Como se usa el token? Lo copiamos, -> Insomnia(por ejemplo) -> POST -> Headers -> Authorization Bearer: xxxxxxxxxx.....xxxxxx
creamos varias tareas. -> GET -> salen las tareas.

**Migracion** /alembic/
se creo originalmente el esquema sin la tabla de users en el repo del profe pero como vemos hemos hecho un POST a user, por lo que se implemento posteriormente la tabla. Esto puede pasar con tablas, relaciones, etc. 

Ahora la app está en produccion por lo que ahora no se puede crear un nuevo esquema, de ahi vienen las *migraciones*. 

nuevos metodos en models.py sobre la migracion.

Simplemente se hace un nuevo create table con la nueva tabla y se añaden columnas de fk en la tablas que estarán relacionadas con la nueva tabla. 
Pero ¿que pasa con las tareas que estaban antes de que hicieramos la creacion de users y de la autenticacion que no tienen user asociado?
Plantear el nuevo sistema y preguntar que hacer, proponer opciones.
	1 - Partir de que la tareas eran del admin, haciendolo manualmente con altertables y select las tablas con fk null y asignarsela
	2 - Preguntar a cada user a quien le pertenecen las tareas y hacer los mismo, alter tables e inserts.
	3 - Borrar y partir de cero
	4 - 
	5 - 
	6 - 

En python hay una herramienta que permite hacer migraciones automaticamente (alembic). Crea versiones de cada estado del squema y bbdd. Ejemplo, 001_base_tasks, 002_add_users_add_users_and_taks_owner, etc... Para poblar los datos si hay que decir como hacerlo.

EN el caso del repo del profe, al estar en Kamal, al desplegar se hace el upgrade solo leyendo el codigo. SI falla, se configura para volver hacer un downgrade y volver a una version de la bbdd estable. Todo esto hecho con Kamal y alembic. 

**Como se hace la migracion**
bootstrap_credentials
	lo que se hizo en este caso es darle las tareas anteriores al admin. 
	variables de entorno de user y password hasheada

create_ootstrap_user
	donde se crea e inserta llamando a *prepare_bootstrap* donde estan los datos preparados para crearlos e insertarlos

prepare_bootstrap
	obtener credenciales
	hashear

upgrade	
	settea la info al user ya terminando la migracion llamando a los otros helper
	se crean la tabla user
	se añade la columna a task
