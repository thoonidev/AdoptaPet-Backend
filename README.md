# AdoptaPet — Backend

API REST para gestionar la adopción de mascotas: desde la publicación de animales y el registro de adoptantes hasta la evaluación de solicitudes y la entrega de la mascota.

## Funcionalidades

- Gestión de mascotas con fotografías, adoptantes y trabajadores.
- Registro e inicio de sesión, con roles de administrador, trabajador y adoptante.
- Registro, aprobación, rechazo, cancelación y seguimiento de solicitudes de adopción.
- Consulta de disponibilidad y programación o reprogramación de entregas.
- Generación de documentos de adopción, carga de actas firmadas y reporte general en PDF.

## Tecnologías

Java 17 · Spring Boot 3.3.4 · Spring Data JPA · Spring Security · MySQL · Maven · Apache PDFBox · Lombok

## Instalación y configuración

Requiere **JDK 17** y **MySQL**. El repositorio incluye Maven Wrapper.

```bash
git clone https://github.com/thoonidev/AdoptaPet-Backend.git
cd AdoptaPet-Backend
```

1. Ejecuta `src/main/resources/bd_adopcion.sql` en MySQL para crear las tablas y cargar los datos de ejemplo. **El script elimina y vuelve a crear `bd_adopcion` si ya existe.**
2. Copia `src/main/resources/application-example.properties` como `application.properties` en la misma carpeta.
3. Ajusta la conexión con tus credenciales locales:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/bd_adopcion
spring.datasource.username=tu_usuario
spring.datasource.password=tu_password
```

El archivo de configuración local está excluido de Git. El origen permitido para el frontend es `http://localhost:4200`; si necesitas cambiarlo, revisa `WebConfig.java` y las anotaciones `@CrossOrigin` de los controladores.

## Ejecución

En Windows (PowerShell):

```powershell
.\mvnw.cmd spring-boot:run
```

En Linux o macOS:

```bash
./mvnw spring-boot:run
```

El servidor inicia en `http://localhost:8080`. Las fotografías y los documentos se guardan en las carpetas locales `fotos_mascotas/` y `uploads/`, creadas al cargar archivos. El límite por archivo es de 10 MB.

## Rutas principales

| Ruta | Uso |
| --- | --- |
| `POST /auth/register` | Registro de adoptantes. |
| `POST /auth/login` | Validación de credenciales (`username` y `password`). |
| `GET /mascota` | Consulta pública de mascotas. |
| `/solicitud` | Solicitudes, disponibilidad, entregas y documentos. |
| `/adoptante` | Consulta y gestión del estado de adoptantes. |
| `/trabajador` | Administración de trabajadores. |
| `GET /reporte/general/pdf` | Reporte general para administradores. |

Las rutas protegidas utilizan **HTTP Basic** con usuario y contraseña; `/auth/login` no emite un token JWT. Los permisos dependen del rol del usuario.

## Comandos

| Comando (Windows) | Descripción |
| --- | --- |
| `.\mvnw.cmd test` | Ejecuta la prueba de carga del contexto de Spring. |
| `.\mvnw.cmd clean package` | Compila, ejecuta las pruebas y genera el JAR en `target/`. |

En Linux o macOS, sustituye `.\mvnw.cmd` por `./mvnw`. Las pruebas requieren la configuración local y acceso a MySQL.
