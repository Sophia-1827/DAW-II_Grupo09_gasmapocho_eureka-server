
# Gas Mapocho - Eureka Server

Este repositorio contiene el **Eureka Server** del proyecto Gas Mapocho, que estamos desarrollando para el curso de Desarrollo de Aplicaciones Web II de Cibertec.

Este servidor nos ayuda a organizar la comunicación entre los diferentes microservicios del sistema. Su función principal es llevar un registro de los servicios que están disponibles, para que puedan encontrarse entre ellos sin tener que utilizar direcciones fijas.

## Tecnologías utilizadas

Para este proyecto estamos utilizando:

- Java 25
- Spring Boot 4.1.1
- Spring Cloud 2025.1.3
- Netflix Eureka Server
- Maven
- Docker

## ¿Para qué sirve Eureka Server?

Como Gas Mapocho está dividido en varios microservicios, necesitamos una manera de saber cuáles están funcionando y dónde se encuentran.

Para eso utilizamos Eureka Server.

Cuando iniciamos un microservicio que tiene configurado Eureka Client, este puede registrarse en el servidor indicando su nombre y dirección.

Así, los demás servicios pueden encontrarlo utilizando su nombre en lugar de depender de una dirección IP específica.

Esto nos facilita la integración de los diferentes módulos del proyecto.

## ¿Cómo funciona?

Eureka Server funciona como un registro central de servicios.

Por ejemplo, cuando iniciamos los microservicios de usuarios, balones o movimientos, estos pueden registrarse automáticamente en Eureka.

El servidor mantiene la información de los servicios registrados y permite consultar cuáles se encuentran disponibles.

Además, cuenta con un panel web desde el que podemos revisar los servicios registrados.

## Requisitos

Para ejecutar el proyecto necesitamos:

- JDK 25
- Git
- Maven o el Maven Wrapper incluido
- Docker (opcional)

Este servidor no necesita una base de datos para realizar su función principal de descubrimiento de servicios.

## Instalación

Primero clonamos el repositorio:

```bash
git clone https://github.com/Sophia-1827/DAW-II_Grupo09_gasmapocho_eureka-server.git
```

Ingresamos a la carpeta del proyecto:

```bash
cd DAW-II_Grupo09_gasmapocho_eureka-server
```

Luego compilamos:

```bash
.\mvnw.cmd clean install
```

Finalmente, iniciamos el servidor:

```bash
.\mvnw.cmd spring-boot:run
```

Los comandos anteriores están pensados para Windows. En Linux o macOS podemos utilizar `./mvnw`.

## Configuración

Eureka Server utiliza normalmente el puerto **8761**, que también es el puerto expuesto en el Dockerfile del proyecto.

Una configuración básica para el servidor sería:

```yaml
server:
  port: 8761

spring:
  application:
    name: eureka-server

eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
```

Esta configuración permite que Eureka funcione como servidor independiente, sin intentar registrarse a sí mismo como cliente.

Debemos revisar que estos valores coincidan con la configuración utilizada en el proyecto.

## Panel de Eureka

Una vez iniciado el servidor en el puerto 8761, podemos ingresar desde el navegador a:

```text
http://localhost:8761
```

En este panel podemos revisar información como:

- Los microservicios registrados.
- El estado de las instancias disponibles.
- Los nombres de los servicios.
- Las direcciones y puertos de las instancias.

Si todavía no hemos iniciado los demás microservicios, es normal que la lista aparezca vacía.

## Ejecución con Docker

El proyecto también cuenta con un Dockerfile que permite compilar y ejecutar el servidor dentro de un contenedor.

Primero construimos la imagen:

```bash
docker build -t gasmapocho-eureka-server .
```

Luego iniciamos el contenedor:

```bash
docker run -p 8761:8761 gasmapocho-eureka-server
```

Después podemos acceder al panel utilizando el mismo puerto, siempre que la aplicación esté configurada para escucharlo.

## Relación con los demás microservicios

Eureka Server es uno de los componentes principales de la infraestructura de Gas Mapocho.

Está pensado para trabajar con los siguientes servicios:

- **API Gateway:** utiliza el descubrimiento de servicios para localizar los microservicios.
- **Config Server:** puede registrarse en Eureka para ser localizado por otros servicios.
- **Microservicio de Usuarios:** maneja los usuarios del sistema.
- **Microservicio de Balones:** administra el catálogo de balones de gas.
- **Microservicio de Movimientos:** registra las entradas y salidas del inventario.
- **Microservicio de Reportes:** consulta información de otros módulos para preparar reportes.

Cada microservicio debe contar con su propia configuración de Eureka Client para poder registrarse correctamente.

## Orden de ejecución

Al momento de levantar el sistema completo, es recomendable iniciar primero Eureka Server.

Después podemos iniciar los demás servicios de infraestructura y finalmente los microservicios del proyecto.

De esta manera, cuando los servicios intenten registrarse, Eureka ya estará disponible para recibirlos.

## Pruebas

El proyecto incluye Spring Boot Starter Test para realizar pruebas.

Podemos ejecutarlas utilizando:

```bash
.\mvnw.cmd test
```

También podemos comprobar manualmente el funcionamiento desde el panel de Eureka, verificando que los microservicios aparezcan registrados cuando estén en ejecución.

## Curso

**Desarrollo de Aplicaciones Web II**

**Instituto:** Cibertec

**Grupo:** 09

**Proyecto:** Gas Mapocho
