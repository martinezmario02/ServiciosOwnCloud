# Práctica 1: Despliegue de servicio ownCloud

**Mario Martínez Sánchez**

## Entorno de desarrollo y producción
Para el desarrollo y despliegue de la práctica se ha utilizado el siguiente entorno:

- **Sistema operativo:** Ubuntu
- **Gestor de contenedores:** Docker 
- **Orquestador de contenedores:** Docker Compose 
- **Proxy inverso:** HAProxy:latest
- **Servicios adicionales:**
  - MariaDB:latest (Base de datos)
  - OpenLDAP:latest (Servicio de directorio)
  - Redis:latest (Almacenamiento en caché)
  - OwnCloud:latest (Almacenamiento en la nube)

## Descripción de la práctica
El objetivo de esta práctica es desplegar y administrar varios servicios en red dentro de un entorno virtualizado utilizando Docker y Docker Compose. Se han configurado dos escenarios:

## Escenario 1: Pequeña empresa o departamento
Este escenario está diseñado para empresas con un número de usuarios pequeño (hasta 150) y almacenamiento local de hasta 10TB.  

### Arquitectura y servicios implementados:
- **OwnCloud:** Servicio de almacenamiento y compartición de archivos con acceso vía web.
- **MariaDB:** Base de datos para OwnCloud.
- **Redis:** Sistema de almacenamiento en caché para optimizar el rendimiento.
- **LDAP:** Servicio de autenticación de usuarios centralizado.
- **Almacenamiento:** Local en la máquina anfitriona.

### Características clave:
- **Disponibilidad:** No hay redundancia, si un componente falla, el servicio se interrumpe.

## Escenario 2: Empresa mediana
Este escenario simula una infraestructura más robusta para una empresa de hasta 1,000 usuarios y almacenamiento de hasta 200TB.

### Arquitectura y servicios implementados:
- **Balanceo de carga (HAProxy):** Distribuye tráfico entre los servidores de aplicación.
- **OwnCloud:** Servicio web de almacenamiento y compartición de archivos.
- **MariaDB:** Implementado con replicación para alta disponibilidad.
- **Redis:** Servicio de caché para mejorar el rendimiento.
- **LDAP:** Servicio de autenticación de usuarios.
- **Servidor NFS**: Servidor para almacenamiento centralizado.
- **Almacenamiento:** Simulado con almacenamiento local.

### Características clave:
- **Alta disponibilidad:** Replicación de la base de datos para tolerancia a fallos.
- **Balanceo de carga:** HAProxy distribuye peticiones de OwnCloud.
  
Se han automatizado los despliegues y configuraciones mediante `compose.yml` y archivos de configuración específicos.

## Servicios desplegados y configuración
### Base de datos (MariaDB)
- Se han configurado dos contenedores: `mariadb-primary` y `mariadb-replica`.
- Se ha establecido replicación entre ambas instancias para garantizar disponibilidad.

### OpenLDAP
- Se ha desplegado un servidor OpenLDAP con un esquema de autenticación estándar.
- Se han creado usuarios y grupos en el directorio.

### HAProxy
- Se ha utilizado HAProxy para distribuir tráfico entre servicios.
- Configurado para balanceo de carga en OwnCloud.

### OwnCloud
- Implementado como servicio de almacenamiento en la nube.
- Integrado con OpenLDAP para la autenticación de usuarios.

## Instrucciones de despliegue
### Escenario 1 y Escenario 2 desplegados con Docker Compose
Para iniciar los servicios, se ejecutará el siguiente comando: `docker compose up`

Podremos iniciar sesión en OwnCloud con los siguientes usuarios:
- Usuario administrador:
  - Nombre de usuario: admin
  - Contraseña: adminpassword
- Usuario normal:
  - Nombre de usuario: martinezmario
  - Contraseña: martinezmario

### Escenario 1 desplegado con Kubernetes
Para iniciar los servicios, se ejecutarán las siguientes órdenes:
- `kubectl apply -f mariadb-deployment.yaml`
- `kubectl apply -f redis-deployment.yaml`
- `kubectl apply -f ldap-deployment.yaml`
- `kubectl apply -f owncloud-deployment.yaml`
- `kubectl apply -f services.yaml`

Tras esto, obtenemos el enlace de OwnCloud: `minikube service owncloud-service --url`

Podremos iniciar sesión en OwnCloud con el siguiente usuario:
- Usuario administrador:
  - Nombre de usuario: admin
  - Contraseña: password

## Conclusiones
Esta práctica ha permitido desplegar y gestionar servicios en red con Docker en dos escenarios empresariales.

En el Escenario 1, se configuró una infraestructura básica con OwnCloud, MariaDB, Redis y OpenLDAP en una sola máquina, adecuada para pequeñas empresas con menor tolerancia a fallos.

En el Escenario 2, se mejoró la disponibilidad con HAProxy para balanceo de carga y replicación de MariaDB, adaptándose a empresas medianas con mayores requisitos de continuidad.

La práctica demuestra cómo los contenedores permiten construir infraestructuras flexibles y escalables según las necesidades empresariales. 

## Referencias bibliográficas y recursos utilizados
- https://github.com/cbergmeir/cc2425/tree/main/practice1
- https://doc.owncloud.com/server/next/admin_manual/installation/deployment_recommendations.html
- https://www.youtube.com/watch?v=Jd0JImHj3fk

# Enlace a Drive al proyecto completo (con imágenes):
- https://drive.google.com/file/d/1IXoJHOibWpiHX-0H2qIdiVC-0gV4HdZ6/view?usp=sharing 
