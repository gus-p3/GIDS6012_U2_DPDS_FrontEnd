# Ejercicio5Cliente (FrontEnd) — Upgrade a Java 21 y Configuración CI/CD en Jenkins

**Asignatura:** Gestión del Proceso de Desarrollo de Software (GIDS6102 - Unidad 2)  
**Estudiante:** Gustavo Mendoza  
**Microservicio:** FrontEnd (`Ejercicio5Cliente` - Interfaz Web Spring Boot + Thymeleaf + Bootstrap)

---

## 1. Ligas de GitHub (Repositorios Listos para Utilizar)

* **Repositorio FrontEnd (`Ejercicio5Cliente`):** [https://github.com/gus-p3/GIDS6012_U2_DPDS_FrontEnd.git](https://github.com/gus-p3/GIDS6012_U2_DPDS_FrontEnd.git)
* **Repositorio BackEnd (`Ejercicio5MyAPIGestor`):** [https://github.com/gus-p3/GIDS6012_U2_DPDS_BackEnd.git](https://github.com/gus-p3/GIDS6012_U2_DPDS_BackEnd.git)

---

## 2. Configuración de Jenkins (JDK 21 y Maven)

El servidor de Integración Continua (Jenkins) fue actualizado y configurado para operar nativamente sobre **JDK 21** y compilar ambos microservicios con **Apache Maven 3.9.x**:

* **Contenedor Jenkins:** `jenkins/jenkins:jdk21` (`OpenJDK Runtime Environment Temurin-21.0.12.1+1 LTS`)
* **Configuración de JDK en Jenkins (`Manage Jenkins -> Tools -> JDK installations`):**
  * **Name:** `JDK21`
  * **JAVA_HOME:** `/opt/java/openjdk`
* **Configuración de Maven en Jenkins (`Manage Jenkins -> Tools -> Maven installations`):**
  * **Name:** `M3`
  * **Versión:** `Maven 3.9.0` (Instalación automática desde Apache, compatible con compilación en Java 21 y Spring Boot 3.3.4).

### Configuraciones XML aplicadas en Jenkins:
```xml
<!-- /var/jenkins_home/config.xml (JDK 21) -->
<jdks>
  <jdk>
    <name>JDK21</name>
    <home>/opt/java/openjdk</home>
    <properties/>
  </jdk>
</jdks>

<!-- /var/jenkins_home/hudson.tasks.Maven.xml (Maven 3.9.0) -->
<hudson.tasks.Maven_-DescriptorImpl>
  <installations>
    <hudson.tasks.Maven_-MavenInstallation>
      <name>M3</name>
      <properties>
        <hudson.tools.InstallSourceProperty>
          <installers>
            <hudson.tasks.Maven_-MavenInstaller>
              <id>3.9.0</id>
            </hudson.tasks.Maven_-MavenInstaller>
          </installers>
        </hudson.tools.InstallSourceProperty>
      </properties>
    </hudson.tasks.Maven_-MavenInstallation>
  </installations>
</hudson.tasks.Maven_-DescriptorImpl>
```

---

## 3. Evidencia de Archivos `pom.xml` (Estado Original vs. Estado Actualizado)

### 3.1. Microservicio FrontEnd (`Ejercicio5Cliente`)

#### A) `pom.xml` en su estado normal (Original - Java 11 / Spring Boot 2.7.17)
*(Archivo de referencia conservado en [`pom.original.xml`](./pom.original.xml))*

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.7.17</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.mx.cbtis</groupId>
	<artifactId>Ejercicio5Cliente</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>Ejercicio5Cliente</name>
	<description>Interfaz del Gestor de Proyectos</description>
	<properties>
		<java.version>11</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		
		<dependency>
		    <groupId>org.webjars</groupId>
		    <artifactId>bootstrap</artifactId>
		    <version>5.1.3</version> 
		</dependency>

	</dependencies>
	
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
	
</project>
```

#### B) `pom.xml` con su actualización (Actualizado - Java 21 / Spring Boot 3.3.4)
*(Archivo activo en [`pom.xml`](./pom.xml))*

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>3.3.4</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.mx.cbtis</groupId>
	<artifactId>Ejercicio5Cliente</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>Ejercicio5Cliente</name>
	<description>Interfaz del Gestor de Proyectos</description>
	<properties>
		<java.version>21</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-validation</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		
		<dependency>
		    <groupId>org.webjars</groupId>
		    <artifactId>bootstrap</artifactId>
		    <version>5.3.3</version> 
		</dependency>

	</dependencies>
	
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
	
</project>
```

---

### 3.2. Microservicio BackEnd (`Ejercicio5MyAPIGestor`)

#### A) `pom.xml` en su estado normal (Original - Java 11 / Spring Boot 2.7.17)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.7.17</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.mx.cbtis</groupId>
	<artifactId>Ejercicio5MyAPIGestor</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>Ejercicio5MyAPIGestor</name>
	<description>Proyecto My API Rest</description>
	<properties>
		<java.version>11</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.mariadb.jdbc</groupId>
			<artifactId>mariadb-java-client</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```

#### B) `pom.xml` con su actualización (Actualizado - Java 21 / Spring Boot 3.3.4)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>3.3.4</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.mx.cbtis</groupId>
	<artifactId>Ejercicio5MyAPIGestor</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>Ejercicio5MyAPIGestor</name>
	<description>Proyecto My API Rest</description>
	<properties>
		<java.version>21</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.mariadb.jdbc</groupId>
			<artifactId>mariadb-java-client</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```

---

## 4. Documento Explicativo: ¿Qué cambió en la actualización?

| Componente / Elemento | Estado Original (Antes) | Estado Actualizado (Después) | Justificación Técnica del Cambio |
| :--- | :--- | :--- | :--- |
| **Versión de Java (`<java.version>`)** | `11` (Java 11 LTS) | `21` (Java 21 LTS) | Actualización del compilador (`maven-compiler-plugin` source/target release 21) para aprovechar soporte LTS moderno, mejoras de rendimiento y compatibilidad con Spring Boot 3.x. |
| **Spring Boot Parent (`spring-boot-starter-parent`)** | `2.7.17` | `3.3.4` | Spring Boot `2.7.x` llegó a su fin de soporte (EOL) y no soporta nativamente el bytecode de Java 21 en todas sus librerías. Spring Boot `3.3.4` ofrece soporte oficial completo para **JDK 21**. |
| **Migración de `javax.*` a `jakarta.*` (Jakarta EE 10)** | `javax.persistence.*` / `javax.validation.*` | `jakarta.persistence.*` / `jakarta.validation.*` | Al subir de Spring Boot 2.7 a 3.3.4, las entidades JPA (`Proyectos`, `Tareas`, `Usuario`, `Asignaciones`, `Comentarios`) y validaciones migraron del namespace `javax.*` (Java EE 8) a `jakarta.*` (Jakarta EE 10). |
| **Dependencias FrontEnd (`Ejercicio5Cliente`)** | Bootstrap `5.1.3` | Bootstrap `5.3.3` + `spring-boot-starter-validation` | Se actualizó WebJars Bootstrap a `5.3.3` y se incorporó `spring-boot-starter-validation` para validación de formularios compatible con Jakarta Bean Validation 3.0. |
| **Entorno de Integración Continua (Jenkins)** | Imagen `jenkins/jenkins:jdk17` | Imagen `jenkins/jenkins:jdk21` + Maven `3.9.0` | Se migró el contenedor de Jenkins a `jdk21` (`/opt/java/openjdk`) y se configuró Apache Maven `3.9.0` para evitar errores `Fatal error compiling: invalid target release: 21`. |
