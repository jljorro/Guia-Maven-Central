# Cómo publicar en Central Repository

En esta guia se describen los pasos necesarios para publicar en *Central repository* una librería para Java o Android y que otros usuarios puedan importarla usando Maven o Grandle.

## Paso 1: Elegir identificador de grupo

El primero de los pasos es elegir un identificador de grupo (`groupId` en Maven) para identificar de donde provienen las librerías que publicamos. Este identificador se obtiene a partir de un domio del cual seamos propietarios. Por ejemplo, si queremos hacer el identificador para una asociación o empresa, podemos obtenerlo a partir de un dominio que tengan como:

`es.ucm.fdi.gaia`

Por otro lado, si lo hacemos para un desarrollador individual y este no dispone de dominio, se puede utilizar el dominio de su repositorio privado. Por ejemplo:

`com.github.jljorro`

## Paso 2: Solicitar acceso a OSSRH

A continuación, debemos solicitar acceso para poder publica en Maven Central. Para ello nos dirigiremos a la página de [*Issues de Sonatype*](https://issues.sonatype.org/secure/CreateIssue.jspa?issuetype=21&pid=10134). En este caso, si tenemos una cuenta procederemos a pulsar sobre el enlace **log in** de la ventana de error. En caso de que no tengamos cuenta, iremos al enlace **sing up**. Cuando creemos una cuenta nos pedirá la siguiente información:

- Correo electrónico
- Nombre completo
- Nombre de usuario
- Contraseña

Una vez que hayamos entrado, nos permitirá rellenar el formulario para solicitar acceso al repositorio de OSSRH. Veremos un formulario como el que aparece a continuación:

![Screenshot_2019-05-14 Create Issue - Sonatype JIRA](/Users/jljorro/Desktop/Central Repository/Screenshot_2019-05-14 Create Issue - Sonatype JIRA.png)

En este formulario tenemos que rellenar la siguiente información:

- **Summary** Una pequeña descripción de que tipo de proyectos vamos a subir.
- **Description** En este aparatado explicaremos con más detalle que tipo de proyectos vamos a subir al repositorio.
- **Group Id** Identificador del grupo que hemos seleccionado en el paso anterior.
- **Project URL** Dominio donde se encuentra  el proyecto. La opción más sencilla es añadir la URL del repositorio con el que estamos trabajando.
- **SCM url** Podemos añadir el mismo dominio que el de la opción anterior.
- **Username(s)** Nombre de usuario o usuarios que van a tener acceso al proyecto.

Cuando hayamos completado esta información, pulsamos sobrte el botón **Create** para crear el *Issue* en el sistema de Sonatype. Ahora debemos esperar a que nos notifique a través del email que nos han creado correctamente nuestro repositorio con nuestro identificador de grupo. Este proceso puede tardar unas horas.

## Paso 3: Preparar el proyecto para publicar

Para poder publicar en *Maven Central* nuestro proyecto tiene que cumplir unos requisitos. Vamos a ir viendo cada uno de ellos.

### Javadocs y ficheros fuente

Los proyectos que se suben a *Maven Central* deben permitir el acceso a los Javadocs que se hayan generado y a los fichero fuente del proyecto. En caso de que no se pueda añadir las fuentes o el Javadoc, es necesario crear unos vacios para poder subirlos.

### Firmar los ficheros

Todos los ficheros que se suban tienen que firmarse con **GPG/PGP** y con un fichero de tipo `.asc` que contenga la firma por cada fichero que subiremos.

### Metadatos en el fichero `pom`

El fichero `pom` debe contener toda la información que describa correctamente el proyecto. Este fichero se encuentra dentro de nuestro proyecto Java. La información que debe aparecer en dicho fichero es:

- **Coordenadas del proyecto** Es decir, el identificador del grupo, el identificador del proyecto y la versión del proyecto. Un ejemplo sería:

  ```xml
  <groupId>es.ucm.fdi.gaia</groupId>
  <artifactId>RecoLibry-Core</artifactId>
  <version>0.0.1</version>
  ```

- **Nombre del proyecto, una pequeña descripción y una URL** Debemos especificar el nombre completo del proyecto, una descripción que explique brevemente su objetivo y una URL donde se encuentre documentación, o la propia aplicación. Por ejemplo:

- ```xml
  <name>RecoLibry-Core</name>
  <description>A JAVA framework to make recommender systems.</description>
  <url>https://github.com/jljorro/RecoLibry-Core</url>
  ```

- **Información de la licencia** Añadir que tipo de licencia está asociada con el proyecto. Por ejemplo si tenemos una licencia GNU-GPL v3.0, deberíamos añadir lo siguiente:

- ```xml
  <licenses>
    <license>
      <name>The GNU Lesser General Public License, Version 3.0</name>
      <url>http://www.gnu.org/licenses/lgpl-3.0.txt</url>
      <distribution>repo</distribution>
    </license>
  </licenses>
  ```

- **Información de los desarrolladores** Incluiremos la información de los desarrolladores que han participado en el proyecto. Estos desarrolladores deben tener un link, que perfectamente puede ser su perfil en GitHub. Un ejemplo sería:

- ```xml
  <developers>
  	<developer>
  		<name>Jose L. Jorro-Aragoneses</name>
  		<email>jljorro@email.net</email>
  		<organizationUrl>https://github.com/jljorro/</organizationUrl>
  	</developer>
  </developers>
  ```

- **Información SMC** Es necesario especificar la conexión con tu sistema de control de código (en inglés *source control system* SMC). Las etiquetas que hay que completar son:

  - `connection` Conexión de solo lectura.
  - `developerConnection` Conexión de lectura y escritura.
  - `url` Un dirección web donde se encuentra el front end de tu sistema SMC.

  Un ejemplo sería basado en GitHub sería:

  ```xml
  <scm>
  	<connection>scm:git:git://github.com/jljorro/RecoLibry-Core.git</connection>
  	<developerConnection>scm:git:ssh://github.com:jljorro/RecoLibry-Core.git</developerConnection>
  	<url>http://github.com/jljorro/RecoLibry-Core/tree/master</url>
  </scm>
  ```

El ejemplo completo del fichero `pom` sería:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.github.jljorro</groupId>
    <artifactId>RecoLibry-Core</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>

    <name>RecoLibry-Core</name>
    <description>A JAVA framework to make recommender systems.</description>
    <url>https://github.com/jljorro/RecoLibry-Core</url>

    <licenses>
        <license>
            <name>The GNU Lesser General Public License, Version 3.0</name>
            <url>http://www.gnu.org/licenses/lgpl-3.0.txt</url>
            <distribution>repo</distribution>
        </license>
    </licenses>

    <developers>
        <developer>
            <name>Jose L. Jorro-Aragoneses</name>
            <email>jljorro@ucm.es</email>
            <organizationUrl>https://github.com/jljorro/</organizationUrl>
        </developer>
    </developers>

    <scm>
        <connection>scm:git:git://github.com/jljorro/RecoLibry-Core.git</connection>
        <developerConnection>scm:git:ssh://github.com:jljorro/RecoLibry-Core.git</developerConnection>
        <url>http://github.com/jljorro/RecoLibry-Core/tree/master</url>
    </scm>
  ...
</project>
```

## Paso 4: Firmar los archivos

Cómo se ha explicado en el paso anterior, uno de los requisitos para poder publicar un proyecto, es que los ficheros deben estar firmados usando **GPG/PGP**. En este [enlace](https://central.sonatype.org/pages/working-with-pgp-signatures.html) pueden ver cómo generar las claves y firmar sus ficheros. Aunque, en la siguiente sección veremos como hacer este paso automáticamente.

## Paso 5: Desplegar en OSSRH usando Apache Maven

En este paso vamos a ver cómo configurar nuestro fichero `pom` para se creen los ficheros necesarios, se firmen y se publique automáticamente en Central Repository. Esta forma es la que se recomineda en Sonatype.

**Nota:** Para poder firmar automáticamente, es necesario que tengamos un cliente GPG instalado en nuestro equipo y que esté dentro del path del sistema operativo, ya que se llamara como un comando desde Maven.

### Autentificación de la distribución y acceso a su Manager

Usando lo plugin de despliegue de Maven, es necesario incluir una sección `distribuionManagement` en el fichero `pom`. Esta sección será como la siguiente:

```xml
<distributionManagement>
  <snapshotRepository>
    <id>ossrh</id>
    <url>https://oss.sonatype.org/content/repositories/snapshots</url>
  </snapshotRepository>
  <repository>
    <id>ossrh</id>
    <url>https://oss.sonatype.org/service/local/staging/deploy/maven2/</url>
  </repository>
</distributionManagement>
```

Usando esta opción es necesario añadir la información de acceso a OSSRH. Para ello tenemos que incluir el nombre de usuario y la contraseña del Paso 2 en el fichero `settings.xml` de Maven. Lo que hay que incluir es lo siguiente:

```xml
<settings>
  <servers>
    <server>
      <id>ossrh</id>
      <username>your-jira-id</username>
      <password>your-jira-pwd</password>
    </server>
  </servers>
</settings>
```

### Generar jar con Javadoc y ficheros fuentes

Para generar los jar que contienen el Javadoc y la fuente del código, se debe configurar estos parámetros en el plugin de Maven:

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-source-plugin</artifactId>
      <version>2.2.1</version>
      <executions>
        <execution>
          <id>attach-sources</id>
          <goals>
            <goal>jar-no-fork</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-javadoc-plugin</artifactId>
      <version>2.9.1</version>
      <executions>
        <execution>
          <id>attach-javadocs</id>
          <goals>
            <goal>jar</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
```

### Firmar los componentes con GPG

Añadimos el plugin GPG de Maven con la siguiente configuración:

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-gpg-plugin</artifactId>
      <version>1.5</version>
      <executions>
        <execution>
          <id>sign-artifacts</id>
          <phase>verify</phase>
          <goals>
            <goal>sign</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
```

Las credenciales de GPG en el sistema tienen que estar accesibles en el fichero `settings.xml`. Sin embargo, se puede configurar el comando GPG dentro del fichero `pom.xml`:

```xml
<settings>
  <profiles>
    <profile>
      <id>ossrh</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <properties>
        <gpg.executable>gpg2</gpg.executable>
        <gpg.passphrase>the_pass_phrase</gpg.passphrase>
      </properties>
    </profile>
  </profiles>
</settings>
```

### Despliegue en OSSRH

La última parte consiste en lanzar nuestro projecto en el repositorio Central. Para ello, solo hay que añadir el pluguin al fichero `pom.xml`:

```xml
<build>
<plugins>
...
  <plugin>
    <groupId>org.sonatype.plugins</groupId>
    <artifactId>nexus-staging-maven-plugin</artifactId>
    <version>1.6.7</version>
    <extensions>true</extensions>
    <configuration>
       <serverId>ossrh</serverId>
       <nexusUrl>https://oss.sonatype.org/</nexusUrl>
       <autoReleaseAfterClose>true</autoReleaseAfterClose>
    </configuration>
  </plugin>
```

Una vez completado esto, cuando se lance una versión definitiva (es decir, el número de versión no finaliza con -SNAPSHOT) solo se tiene que ejecutar el siguiente comando y dicha versión se desplegará automáticamente en el respositorio central:

```
mvn clean deploy
```

Con eso tendríamos nuestro proyecto subido al Central Repository de Maven.