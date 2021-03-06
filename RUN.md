## Requisitos para ejecutar el servicio

* IDE compatible con proyectos MAVEN
* Java 11 (JDK11)
* Por default se usa la base de datos h2. Si desea usar PostgreSQL, lea [PostgreSQL.md](PostgreSQL.md)


## Clonar proyecto

Debe clonar el proyecto de (Es un repositorio autenticado, consultar el URL para `clone` en):

```shell
$ git clone XXXXXX
$ cd XXXX
```

## Ejecución Rápida
Si ya se tienen instaladas las librerías Joko necesarias (joko-utils y [security](https://github.com/jokoframework/security): pasos
de instalacion más abajo) entonces se puede proceder a empaquetar el proyecto (Ej: `mvn package`) y luego: 

### Opción 1: Ejecución Con Docker
La forma más simple de levantar el proyecto es con la utilización de Docker, ejecutando
el siguiente comando dentro del proyecto:
`docker-compose up`

### Opción 2: Ejecución Sin Docker
Se debe ejecutar lo siguiente dentro del proyecto:
```
cd target/
java -jar nombreDelArchivoJar.jar
```

## Otras Configuraciones
El proyecto posee un conjunto de scripts que nos permiten automatizar el ciclo
 de vida de la base de datos. Con esto se puede crear fácilmente toda la BD 
 desde la linea de comandos. Para actualizar hay que seguir los siguientes 
 pasos:
  
 ### Step 1) Crear el directorio PROFILE_DIR
 El directorio de profile contiene el archivo application.properties con la 
 configuración necesaria para lanzar la aplicación spring-boot.
Esto permite tener diferenciados los ambientes de ejecución.

 La convencion utilizada es tener un directorio, dentro del cual existan 
 varios PROFILE_DIR según se requiera. Por ejemplo:
 ```shell
 /opt/starter-kit/dev
 /opt/starter-kit/qa
 ```
 
 En el anterior ejemplo existen dos PROFILE_DIR dentro del joko-demo, el 
 primero para development y el segundo con datos de quality assurance.
 
 Para referirse al PROFILE_DIR como parametro se usa "ext.prop.dir".
 
 Obs.: Un archivo de ejemplo para el application.properties se encuentra en 
 `src/main/resources/application.properties.examples`
  
  ### Step 2) Instalar librerías Joko
	
Clonar los proyectos (no dentro de la misma carpeta backend o PROFILE)
	
https://github.com/jokoframework/joko-utils

https://github.com/jokoframework/security

Entrar en el directorio de cada proyecto y hacer lo siguiente:

-Para Joko-utils solo ejecutar `mvn install`, en caso de tener problemas descargando las dependencias ejecute
 
 `mvn install -Ddownloader.quick.query.timestamp=false`

-Para [security](https://github.com/jokoframework/security) hay que entrar al proyecto en github y seguir sus instrucciones de instalacion. Esto deja instaladas las librerías que son dependencias para el Backend.

### Step 3 Corren con Maven

Una vez hechos estos cambios, debemos definir y exportar la siguiente variable de entorno:

```shell
  $ export SPRING_CONFIG_LOCATION=/opt/starter-kit/dev/application.properties
```

Posteriormente, podemos correr el proyecto como una aplicación de Spring Boot, o con la siguiente línea de comando (se requiere maven instalado).

```shell
  $ mvn -Dext.prop.dir=/opt/starter-kit/dev spring-boot:run
```


El usuario/password default que se crea con la base de datos, es admin/123456

El proyecto usa por default la base de datos Embedded h2. 

STS
----
Para poder levantar la aplicación desde un IDE, se debe añadir el parámetro 
como argumento de la VM `-Dspring.config.location=file://` 
 con la ruta al archivo  application.properties. 
 
 Por ejemplo en el IDE  Eclipse-STS  ir hasta debug/debug-configuracion,  añadir una nueva configuración e ir hasta la pestaña (x)Arguments Luego insertar el parámetro en el campo 'VM arguments'. 
 
 Ejemplo:

    -Dspring.config.location=file:///opt/starter-kit/application.properties -Dext
    .prop.dir=/opt/starter-kit


La mayoría de los IDEs soportan ejecución de aplicaciones tipo Spring Boot o 
permiten configurar ejecuciones customizadas de maven.

### Swagger API
El proyecto cuenta con documentación del API accesible desde el swagger-ui.
URI al swagger desde maquina HOST:

  http://localhost:8080/swagger-ui.html
	
OBS. Si se desea abrir la pagina desde algún Windows u otro SO interno:

  http://"IP DE LA MAQUINA HOST":8080/swagger-ui.html
