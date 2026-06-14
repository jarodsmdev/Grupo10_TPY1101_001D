# ERS — Especificación de Requisitos de Software

## SIFA: Sistema Integrado de Fiscalización Automatizada

| Versión | Fecha      | Autor       | Descripción                       |
| ------- | ---------- | ----------- | --------------------------------- |
| 1.0     | Junio 2026 | Equipo SIFA | Versión inicial del documento ERS |

---

## Índice

1. [Introducción](#1-introducción)
2. [Descripción General del Proyecto](#2-descripción-general-del-proyecto)
3. [Actores del Sistema](#3-actores-del-sistema)
4. [Requisitos Funcionales](#4-requisitos-funcionales)
5. [Requisitos No Funcionales](#5-requisitos-no-funcionales)
6. [Épicas e Historias de Usuario](#6-épicas-e-historias-de-usuario)
7. [Arquitectura del Sistema](#7-arquitectura-del-sistema)
8. [Diagramas del Sistema](#8-diagramas-del-sistema)
9. [Mockups y Prototipos](#9-mockups-y-prototipos)
10. [Glosario](#10-glosario)

---

## 1. Introducción

### 1.1 Propósito

El presente documento constituye la **Especificación de Requisitos de Software (ERS)** para el proyecto **SIFA — Sistema Integrado de Fiscalización Automatizada**, desarrollado para la **I. Municipalidad de El Quisco**. Este documento describe de manera completa el comportamiento esperado del sistema, sus restricciones, actores involucrados y la interacción entre sus componentes.

### 1.2 Alcance

SIFA es un ecosistema de microservicios cuyo objetivo es digitalizar y automatizar el proceso de fiscalización vehicular mediante:

- Detección de patentes con inteligencia artificial (YOLO + OCR)
- Gestión de infracciones en terreno desde una aplicación móvil
- Emisión de citaciones del Juzgado de Policía Local (JPL)
- Panel web de administración, supervisión y reportes

El sistema está dirigido a fiscalizadores municipales, administrativos JPL, supervisores y administradores del sistema.

### 1.3 Definiciones y Siglas

| Término  | Significado                                         |
| -------- | --------------------------------------------------- |
| **SIFA** | Sistema Integrado de Fiscalización Automatizada     |
| **JPL**  | Juzgado de Policía Local                            |
| **YOLO** | You Only Look Once (modelo de detección de objetos) |
| **OCR**  | Optical Character Recognition                       |
| **PPU**  | Placa Patente Única                                 |
| **JWT**  | JSON Web Token                                      |
| **RBAC** | Role-Based Access Control                           |
| **API**  | Application Programming Interface                   |
| **IaC**  | Infrastructure as Code                              |
| **KPI**  | Key Performance Indicator                           |

### 1.4 Referencias

| Documentos                                                                                     |
| ---------------------------------------------------------------------------------------------- |
| [Visión del proyecto + 4 pilares](Docs%20Scrum/Visión%20del%20proyecto%20+%204%20pilares.docx) |
| [Análisis del caso](Docs%20Scrum/Analisis%20del%20caso.docx)                                   |
| [Requisitos Funcionales](Docs%20Scrum/Requisitos%20Funcionales.docx)                           |
| [Requisitos No Funcionales](Docs%20Scrum/Requisitos%20No%20Funcionales.docx)                   |
| [Épicas y user stories](Docs%20Scrum/Épicas,%20historias%20de%20usuario%20y%20sprints.docx)    |
| [Roles y acciones](Docs%20Scrum/Roles%20y%20acciones.docx)                                     |
| [Diagrama de arquitectura](Diagramas%20Scrum/Diagrama%20de%20arquitectura.pdf)                 |
| [Diagrama de casos de uso](Diagramas%20Scrum/Diagrama%20de%20casos%20de%20uso.pdf)             |
| [Diagrama de componentes](Diagramas%20Scrum/Diagrama%20de%20componentes.pdf)                   |
| [Diagrama de flujo](Diagramas%20Scrum/Diagrama%20de%20flujo.pdf)                               |
| [Esquema BD](Diagramas%20Scrum/Esquema_BD.pdf)                                                 |
| [Mapa de Impacto](Diagramas%20Scrum/)                                                          |
| [Mapa Historias de Usuario](Diagramas%20Scrum/User%20Story%20Mapping.pdf)                      |
| ##                                                                                             |

## 2. Descripción General del Proyecto

### 2.1 Problema Actual

El proceso de fiscalización vehicular en la I. Municipalidad de El Quisco es actualmente **manual, lento, inseguro y propenso a errores**. Los fiscalizadores utilizan talonarios de papel físicos (Formulario N.º 00443) para registrar infracciones, lo que genera:

- Riesgo de seguridad para los fiscalizadores (larga exposición en la vía pública)
- Errores de información por escritura manual
- Multas invalidadas por falta de pruebas sólidas
- Sobrecarga administrativa en el JPL
- Flujo de información lento y sin trazabilidad digital

### 2.2 Solución Propuesta

SIFA propone un ecosistema digital integrado que permite:

1. **Captura rápida:** El fiscalizador fotografía la patente desde la app móvil
2. **Detección automática:** IA reconoce la patente en menos de 3 segundos
3. **One-Tap:** Registro de infracción en menos de 30 segundos
4. **Gestión centralizada:** Dashboard web para revisión y aprobación
5. **Citación digital:** Generación de PDF oficial con firma y QR

### 2.3 Los 4 Pilares de SIFA

| Pilar                                           | Descripción                                                                                                            |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Seguridad y Resiliencia del Funcionario**     | Minimizar el tiempo de exposición del fiscalizador en la vía pública. Ciclo de fiscalización reducido a < 30 segundos. |
| **Eficiencia Operativa y Digitalización**       | Eliminar talonarios de papel y cuellos de botella administrativos. Recepción en tiempo real en el Dashboard Web.       |
| **Probidad y Transparencia Administrativa**     | Registros de auditoría inmutables, coordenadas GPS automáticas y timestamps para respaldo legal irrefutable.           |
| **Interoperabilidad e Integración Tecnológica** | Módulo de exportación masiva a CSV/XML compatibles con estándares CAS Chile.                                           |

### 2.5 Tecnología del Sistema

| Área                | Tecnologías                                                                     |
| ------------------- | ------------------------------------------------------------------------------- |
| **Backend**         | Java 17/21, Spring Boot 3, Spring Cloud Gateway, Spring Security, JPA/Hibernate |
| **IA / OCR**        | Python, FastAPI, YOLO, Tesseract OCR, OpenCV                                    |
| **Frontend Web**    | React 18, Vite, Tailwind CSS, Recharts, Leaflet, jsPDF                          |
| **Mobile**          | Kotlin, Jetpack Compose, CameraX, Retrofit                                      |
| **Base de Datos**   | MySQL 8+                                                                        |
| **Infraestructura** | Terraform, AWS (EC2, VPC, S3, NAT Gateway, DynamoDB)                            |
| **Contenedores**    | Docker, Docker Compose                                                          |
| **CI/CD**           | GitHub Actions                                                                  |

### 2.6 Repositorios del Ecosistema

**Backend:**

| Servicio            | Tecnología                       | Puerto  | Repositorio                                                              |
| ------------------- | -------------------------------- | ------- | ------------------------------------------------------------------------ |
| API Gateway         | Spring Cloud Gateway / Java 17   | `:9000` | [BEsifaAPIGateway](https://github.com/jarodsmdev/BEsifaAPIGateway)       |
| Auth Service        | Spring Boot 3 / Java 21 / MySQL  | `:8081` | [BEsifaAuthService](https://github.com/jarodsmdev/BEsifaAuthService)     |
| Core Service        | Spring Boot 3 / Java 21 / MySQL  | `:8083` | [BEsifaCoreService](https://github.com/Andythe20/BEsifaCoreService)      |
| YOLO Plate Detector | FastAPI / Python / Tesseract OCR | `:8001` | [sifaPlateDetectorBE](https://github.com/jarodsmdev/sifaPlateDetectorBE) |

**Frontend:**

| Aplicación          | Tecnología                         | Repositorio                                                    |
| ------------------- | ---------------------------------- | -------------------------------------------------------------- |
| Dashboard Web SIFA  | React 18 / Vite / Tailwind CSS     | [SIFA_Dashboard](https://github.com/Nicolas-15/SIFA_Dashboard) |
| SIFA GO (App Móvil) | Android / Kotlin / Jetpack Compose | [sifa_go](https://github.com/Andythe20/sifa_go)                |

**Infraestructura:**

| Proyecto             | Herramienta         | Repositorio                                                                |
| -------------------- | ------------------- | -------------------------------------------------------------------------- |
| Infraestructura SIFA | Terraform / AWS     | [TerraformProyectSIFA](https://github.com/jarodsmdev/TerraformProyectSIFA) |
| MySQL en EC2         | Terraform / AWS EC2 | [Terraform_EC2_MySQL](https://github.com/jarodsmdev/Terraform_EC2_MySQL)   |

---

## 3. Actores del Sistema

### 3.1 Mapa de Actores

![Mapa de actores](Diagramas%20Scrum/imgs/Mapa%20de%20actores_page-0001.jpg)

_Ver diagrama original:_ [Mapa de actores](Diagramas%20Scrum/Mapa%20de%20actores.pdf)

### 3.2 Descripción de Actores

| Actor                  | Descripción                                                                             | Acceso                       | Sistema                                                                                                                                                                       |
| ---------------------- | --------------------------------------------------------------------------------------- | ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Fiscalizador**       | Inspector municipal que opera en terreno. Registra infracciones mediante la app móvil.  | SIFA GO (App Móvil)          | Login con credenciales + huella digital, captura de patente vía IA, corrección manual, fotos, GPS, timestamp, consulta de datos vehículo, emisión de infracción, modo offline |
| **Administrativo JPL** | Personal administrativo del Juzgado de Policía Local que revisa y procesa infracciones. | SIFA Control (Dashboard Web) | Ver bandeja de infracciones, ver detalle completo, aceptar/rechazar infracciones, generar PDF de citación, exportar datos                                                     |
| **Supervisor**         | Supervisor que monitorea el desempeño de fiscalizadores y genera reportes.              | SIFA Control (Dashboard Web) | Ver listado de inspecciones, ver fiscalizadores activos/inactivos, dashboard de métricas, mapas de calor, reportes de productividad                                           |
| **Administrador**      | Administrador del sistema con acceso completo a toda la funcionalidad.                  | SIFA Control (Dashboard Web) | Todos los permisos de Supervisor + crear usuarios (asignar roles), ver tabla de auditoría, configuración del sistema                                                          |

### 3.3 Matriz de Roles y Permisos (RBAC)

| Rol                              | App Móvil | Dashboard | Crear Usuarios | Aceptar/Rechazar | Ver Métricas | Ver Auditoría |
| -------------------------------- | --------- | --------- | -------------- | ---------------- | ------------ | ------------- |
| `USER_APP` (Fiscalizador)        | ✓         | ✗         | ✗              | ✗                | ✗            | ✗             |
| `ADMIN_JPL` (Administrativo JPL) | ✗         | ✓         | ✗              | ✓                | ✗            | ✗             |
| `SUPERVISOR` (Supervisor)        | ✗         | ✓         | ✗              | ✗                | ✓            | ✓ (limitado)  |
| `USER_ADMIN` (Administrador)     | ✗         | ✓         | ✓              | ✓                | ✓            | ✓ (completo)  |

---

## 4. Requisitos Funcionales

_Ver Documento:_ [Requisistos Funcionales](Docs%20Scrum/Requisitos%20Funcionales.docx)

### 4.1 Aplicación Móvil (SIFA GO)

| ID        | Nombre                                    | Descripción                                                                                                             | Prioridad |
| --------- | ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | --------- |
| **RF-01** | Inicio de sesión seguro                   | El sistema debe permitir al fiscalizador iniciar sesión con RUT/usuario y contraseña.                                   | Alta      |
| **RF-02** | Autenticación biométrica                  | El sistema debe permitir el uso de huella digital para el inicio de sesión.                                             | Media     |
| **RF-03** | Captura de imagen vehicular               | El sistema debe permitir capturar una imagen del vehículo en terreno.                                                   | Alta      |
| **RF-04** | Confirmación de captura                   | El sistema debe permitir confirmar la imagen capturada o repetirla si no es adecuada.                                   | Alta      |
| **RF-05** | Reconocimiento automático de patente      | El sistema debe reconocer automáticamente la patente del vehículo usando IA (YOLO + OCR).                               | Alta      |
| **RF-06** | Validación y corrección manual de patente | El sistema debe permitir validar y/o corregir manualmente la patente detectada.                                         | Alta      |
| **RF-07** | Registro automático de GPS                | El sistema debe registrar automáticamente la ubicación GPS de la infracción.                                            | Alta      |
| **RF-08** | Registro automático de fecha y hora       | El sistema debe registrar automáticamente la fecha y hora del evento (timestamp).                                       | Alta      |
| **RF-09** | Adjuntar evidencia fotográfica            | El sistema debe permitir adjuntar evidencia fotográfica a la infracción.                                                | Alta      |
| **RF-10** | Consulta de datos del vehículo            | El sistema debe consultar información del vehículo (marca, modelo, año, color) desde una base de datos o API.           | Alta      |
| **RF-11** | Emisión de infracción                     | El sistema debe permitir cursar una infracción si los documentos del vehículo presentan problemas o hay alerta de robo. | Alta      |
| **RF-12** | Envío de infracción al servidor           | El sistema debe enviar la información de la infracción al servidor en tiempo real o diferido (modo offline/online).     | Alta      |
| **RF-13** | Confirmación de registro exitoso          | El sistema debe mostrar una confirmación de registro exitoso al fiscalizador.                                           | Alta      |
| **RF-14** | Modo offline                              | El sistema debe permitir operar en condiciones de baja conectividad (almacenamiento local temporal).                    | Alta      |
| **RF-15** | Firma digital del fiscalizador            | El sistema debe permitir subir la imagen de la firma del fiscalizador para incluirla en la citación digital.            | Media     |

### 4.2 Plataforma Web (SIFA Control)

| ID        | Nombre                               | Descripción                                                                                                                                                                       | Prioridad |
| --------- | ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| **RF-16** | Inicio de sesión con roles           | El sistema debe permitir el inicio de sesión mediante credenciales y JWT, restringiendo acceso y vistas según el rol del usuario (Administrador, Supervisor, Administrativo JPL). | Alta      |
| **RF-17** | Gestión de usuarios                  | El sistema debe permitir al Administrador crear nuevos usuarios, asignando obligatoriamente un rol y tipo de cuenta.                                                              | Alta      |
| **RF-18** | Bandeja centralizada de infracciones | El sistema debe mostrar una bandeja centralizada con las infracciones recibidas desde terreno, indicando su estado.                                                               | Alta      |
| **RF-19** | Búsqueda y filtrado de infracciones  | El sistema debe permitir buscar y filtrar infracciones por criterios avanzados (rango de fechas, patente, estado, fiscalizador).                                                  | Alta      |
| **RF-20** | Detalle completo de infracción       | El sistema debe permitir ver el detalle completo de una infracción, incluyendo evidencia fotográfica, coordenadas GPS en mapa y datos del vehículo.                               | Alta      |
| **RF-21** | Cambio de estado de infracción       | El sistema debe permitir al Administrativo JPL cambiar el estado de la infracción (Aceptar/Rechazar), solicitando una razón opcional para el rechazo.                             | Alta      |
| **RF-22** | Generación de PDF de citación        | El sistema debe permitir exportar un certificado de infracción validado a PDF, incluyendo la firma digital transparente del fiscalizador y un código QR de validación.            | Alta      |
| **RF-23** | Dashboard de métricas                | El sistema debe mostrar un panel de métricas con estadísticas visuales de las fiscalizaciones y ubicación en tiempo real de los fiscalizadores en terreno.                        | Alta      |
| **RF-24** | Listado general de inspecciones      | El sistema debe mostrar un listado general de todas las inspecciones (lecturas de patentes) realizadas, con opciones de búsqueda y filtro por fecha.                              | Media     |
| **RF-25** | Listado de fiscalizadores            | El sistema debe mostrar un listado de fiscalizadores registrados, indicando sus detalles y estado (activo/inactivo).                                                              | Media     |
| **RF-26** | Reportes operacionales               | El sistema debe permitir generar reportes operacionales (mapa de calor de infracciones, productividad por fiscalizador) y exportarlos como PDF.                                   | Media     |
| **RF-27** | Tabla de auditoría                   | El sistema debe mantener y mostrar una tabla de auditoría con registros inalterables de las acciones realizadas por los usuarios, visible solo para el Administrador.             | Alta      |

### 4.3 Backend y Servicios

| ID        | Nombre                                     | Descripción                                                                               | Prioridad |
| --------- | ------------------------------------------ | ----------------------------------------------------------------------------------------- | --------- |
| **RF-28** | Recepción y almacenamiento de infracciones | El sistema debe recibir y almacenar las infracciones enviadas desde la aplicación móvil.  | Alta      |
| **RF-29** | Almacenamiento de evidencia fotográfica    | El sistema debe almacenar la evidencia fotográfica asociada a cada infracción.            | Alta      |
| **RF-30** | Registro de auditoría                      | El sistema debe mantener un registro de auditoría (log) de todas las acciones realizadas. | Alta      |
| **RF-31** | API REST                                   | El sistema debe exponer una API REST para la comunicación entre componentes.              | Alta      |
| **RF-32** | Procesamiento de imágenes con IA           | El sistema debe procesar imágenes a través de un servicio de IA para detectar patentes.   | Alta      |
| **RF-33** | Validación de integridad de datos          | El sistema debe validar la integridad de los datos recibidos.                             | Alta      |

### 4.4 Integración y Exportación

| ID        | Nombre                             | Descripción                                                                                        | Prioridad |
| --------- | ---------------------------------- | -------------------------------------------------------------------------------------------------- | --------- |
| **RF-34** | Exportación de archivos            | El sistema debe generar archivos de exportación en diferentes formatos (CSV, PDF).                 | Media     |
| **RF-35** | Exportación masiva de infracciones | El sistema debe permitir la exportación masiva de infracciones.                                    | Media     |
| **RF-36** | Integración con servicios externos | El sistema debe permitir la integración con servicios externos para consulta de datos vehiculares. | Media     |

---

## 5. Requisitos No Funcionales

_Ver Documento:_ [Requisitos No Funcionales](Docs%20Scrum/Requisitos%20No%20Funcionales.docx)

### 5.1 Rendimiento

| ID         | Nombre                              | Descripción                                                                                                    | Métrica         |
| ---------- | ----------------------------------- | -------------------------------------------------------------------------------------------------------------- | --------------- |
| **RNF-01** | Tiempo de reconocimiento de patente | El reconocimiento de patente debe procesarse en un máximo de 3 segundos.                                       | < 3 s           |
| **RNF-02** | Tiempo de respuesta de API          | El sistema debe responder a las solicitudes del usuario en menos de 2 segundos en condiciones normales.        | < 2 s           |
| **RNF-03** | Concurrencia                        | El sistema debe soportar múltiples usuarios concurrentes sin degradación significativa del rendimiento.        | Sin degradación |
| **RNF-04** | Tiempo de envío de infracción       | El envío de infracción desde la app móvil al servidor no debe exceder los 5 segundos con conectividad estable. | < 5 s           |
| **RNF-05** | Ciclo de fiscalización              | El sistema debe permitir registrar una infracción en menos de 30 segundos.                                     | < 30 s          |

### 5.2 Disponibilidad

| ID         | Nombre                     | Descripción                                                                                      |
| ---------- | -------------------------- | ------------------------------------------------------------------------------------------------ |
| **RNF-06** | Disponibilidad del sistema | El sistema debe estar disponible al menos el 99% del tiempo.                                     |
| **RNF-07** | Copias de seguridad        | El sistema debe contar con mecanismos automáticos de respaldo de información.                    |
| **RNF-08** | Modo offline               | La app móvil debe permitir operación sin conexión, sincronizando datos cuando haya conectividad. |

### 5.3 Seguridad

| ID         | Nombre                                   | Descripción                                                           |
| ---------- | ---------------------------------------- | --------------------------------------------------------------------- |
| **RNF-09** | Autenticación JWT                        | El sistema debe implementar autenticación segura mediante tokens JWT. |
| **RNF-10** | Control de acceso basado en roles (RBAC) | El sistema debe restringir el acceso según el rol del usuario.        |
| **RNF-11** | Trazabilidad (Audit Trail)               | El sistema debe registrar accesos y acciones realizadas.              |
| **RNF-12** | Protección de datos                      | El sistema debe proteger los datos contra accesos no autorizados.     |
| **RNF-13** | Cifrado en tránsito                      | El sistema debe utilizar HTTPS/SSL para todas las comunicaciones.     |

### 5.4 Usabilidad

| ID         | Nombre                | Descripción                                                                          |
| ---------- | --------------------- | ------------------------------------------------------------------------------------ |
| **RNF-14** | Interfaz intuitiva    | La app móvil debe tener una interfaz simple e intuitiva.                             |
| **RNF-15** | Minimización de pasos | El sistema debe minimizar la cantidad de pasos necesarios para completar una acción. |

### 5.5 Escalabilidad

| ID         | Nombre                   | Descripción                                                                     |
| ---------- | ------------------------ | ------------------------------------------------------------------------------- |
| **RNF-16** | Escalabilidad horizontal | El sistema debe permitir escalamiento horizontal en la nube.                    |
| **RNF-17** | Crecimiento de usuarios  | El sistema debe soportar el crecimiento de usuarios sin afectar el rendimiento. |

### 5.6 Mantenibilidad

| ID         | Nombre                       | Descripción                                                                                |
| ---------- | ---------------------------- | ------------------------------------------------------------------------------------------ |
| **RNF-18** | Arquitectura modular         | El sistema debe desarrollarse con una arquitectura modular de microservicios.              |
| **RNF-19** | Código versionado            | El código debe estar documentado y versionado en un repositorio.                           |
| **RNF-20** | Actualización de componentes | El sistema debe permitir la actualización de componentes sin afectar la operación general. |

### 5.7 Compatibilidad

| ID         | Nombre               | Descripción                                                                 |
| ---------- | -------------------- | --------------------------------------------------------------------------- |
| **RNF-21** | Compatibilidad móvil | La app móvil debe ser compatible con dispositivos Android.                  |
| **RNF-22** | Compatibilidad web   | El sistema web debe ser compatible con navegadores modernos (Chrome, Edge). |

### 5.8 Integridad de Datos

| ID         | Nombre                   | Descripción                                                                       |
| ---------- | ------------------------ | --------------------------------------------------------------------------------- |
| **RNF-23** | Integridad de datos      | El sistema debe garantizar la integridad y consistencia de los datos almacenados. |
| **RNF-24** | Prevención de duplicados | El sistema debe prevenir registros duplicados de infracciones.                    |

---

## 6. Épicas e Historias de Usuario

_Ver Documento:_ [Épicas e Historias de Usuario](Docs%20Scrum/Épicas,%20historias%20de%20usuario%20y%20sprints.docx)

### 6.1 Épicas del Proyecto

| Épica   | Nombre                                                | Descripción                                                                                          |
| ------- | ----------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **E00** | Planificación, Diseño y Configuración Inicial         | Centraliza la definición de las bases estructurales, visuales y técnicas antes de la implementación. |
| **E01** | Aplicación Móvil de Fiscalización (Módulo de Terreno) | Centraliza el desarrollo del cliente móvil Kotlin para fiscalizadores.                               |
| **E02** | Motor de Inteligencia Artificial y OCR                | Se enfoca exclusivamente en el microservicio Python de visión computacional.                         |
| **E03** | Dashboard Web de Gestión                              | Cubre el portal administrativo React para el personal JPL y supervisores.                            |
| **E04** | Núcleo de Servicios y Persistencia (Core Backend)     | Representa la columna vertebral del sistema en Java/Spring Boot y base de datos MySQL.               |
| **E06** | Infraestructura Cloud y DevOps                        | Cubre el despliegue y automatización del ciclo de vida del software en AWS.                          |

### 6.2 Historias de Usuario

| ID   | Épica | Nombre                                     | Descripción                                                                                            |
| ---- | ----- | ------------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| HU40 | E00   | Diseño de Arquitectura y Diagramas UML     | Como equipo, queremos diseñar la arquitectura y diagramas UML para tener una visión clara del sistema. |
| HU41 | E00   | Modelamiento Lógico de Datos (MER)         | Como equipo, queremos modelar la base de datos para asegurar una persistencia eficiente.               |
| HU42 | E00   | Prototipado de Alta Fidelidad (UI/UX)      | Como equipo, queremos prototipar la interfaz para validar el diseño con los stakeholders.              |
| HU43 | E00   | Levantamiento de Entornos                  | Como equipo, queremos configurar los entornos de desarrollo para comenzar la implementación.           |
| HU44 | E00   | Definición de Estándares y Git Flow        | Como equipo, queremos definir estándares de código y flujo Git para mantener consistencia.             |
| HU45 | E00   | Documentación SCRUM                        | Como equipo, queremos crear la documentación SCRUM para seguir la metodología ágil.                    |
| HU01 | E01   | Acceso Seguro (login + huella)             | Como fiscalizador, quiero iniciar sesión de forma segura en la app móvil para acceder al sistema.      |
| HU02 | E01   | Reconocimiento Automático de Patentes      | Como fiscalizador, quiero que la app reconozca automáticamente la patente del vehículo mediante IA.    |
| HU03 | E01   | Validación y Corrección de Datos           | Como fiscalizador, quiero validar y corregir manualmente la patente detectada.                         |
| HU04 | E01   | Registro Automatizado de Contexto          | Como fiscalizador, quiero que el sistema registre automáticamente GPS y timestamp.                     |
| HU05 | E01   | Consulta de Antecedentes en Tiempo Real    | Como fiscalizador, quiero consultar datos del vehículo en tiempo real.                                 |
| HU06 | E01   | Captura de Evidencia Fotográfica           | Como fiscalizador, quiero capturar múltiples fotos como evidencia de la infracción.                    |
| HU07 | E01   | Emisión de la Infracción                   | Como fiscalizador, quiero emitir la infracción con un solo toque (One-Tap).                            |
| HU08 | E01   | Sincronización y Modo Offline              | Como fiscalizador, quiero que la app funcione sin conexión y sincronice después.                       |
| HU09 | E01   | Confirmación de Registro Exitoso           | Como fiscalizador, quiero recibir confirmación de que la infracción se registró correctamente.         |
| HU21 | E01   | Envío de imagen al sistema                 | Como fiscalizador, quiero enviar la imagen de la patente al sistema para su procesamiento.             |
| HU10 | E02   | Detección de PPU con YOLO                  | Como sistema, quiero detectar la placa patente única usando el modelo YOLO.                            |
| HU11 | E02   | Extracción de Texto mediante OCR           | Como sistema, quiero extraer el texto de la patente usando Tesseract OCR.                              |
| HU12 | E02   | Endpoint de Procesamiento de Imágenes      | Como sistema, quiero exponer un endpoint FastAPI para procesar imágenes.                               |
| HU13 | E02   | Optimización de Tiempo de Respuesta        | Como sistema, quiero optimizar la detección para que responda en menos de 3 segundos.                  |
| HU14 | E02   | Validación de Confianza del Reconocimiento | Como sistema, quiero mostrar un puntaje de confianza del reconocimiento realizado.                     |
| HU15 | E03   | Acceso Administrativo al Portal            | Como administrativo JPL, quiero iniciar sesión en el portal web para gestionar infracciones.           |
| HU16 | E03   | Listado de Infracciones                    | Como administrativo JPL, quiero ver un listado de infracciones con filtros avanzados.                  |
| HU17 | E03   | Auditoría y Exportación de Infracción      | Como administrativo JPL, quiero exportar una infracción a PDF con QR y firma digital.                  |
| HU18 | E03   | Gestión de Estados (Aceptación/Rechazo)    | Como administrativo JPL, quiero aceptar o rechazar infracciones con razón opcional.                    |
| HU20 | E03   | Panel de Supervisión y Métricas            | Como supervisor, quiero ver un dashboard con métricas y KPIs del sistema.                              |
| HU31 | E03   | Vista de Fiscalizadores del Sistema        | Como supervisor, quiero ver el listado de fiscalizadores y su estado.                                  |
| HU33 | E03   | Auditoría y Reportes de Infracciones       | Como supervisor, quiero generar reportes (mapas de calor, productividad).                              |
| HU47 | E03   | Creación de Usuarios                       | Como administrador, quiero crear usuarios y asignarles roles.                                          |
| HU48 | E03   | Consulta de Auditorías del Sistema         | Como administrador, quiero consultar la tabla de auditoría del sistema.                                |
| HU19 | E04   | Firma del Fiscalizador                     | Como sistema, quiero asociar la identidad JWT del fiscalizador a cada infracción.                      |
| HU22 | E04   | Persistencia Relacional de Infracciones    | Como sistema, quiero almacenar infracciones en una base de datos relacional.                           |
| HU23 | E04   | Gestión de Transacciones de Infracción     | Como sistema, quiero gestionar las transacciones de infracción vía API Core.                           |
| HU24 | E04   | Log de Auditoría y Probidad                | Como sistema, quiero implementar un registro de auditoría inmutable.                                   |
| HU25 | E04   | Control de Acceso Basado en Roles          | Como sistema, quiero implementar RBAC para restringir accesos.                                         |
| HU26 | E04   | Orquestación de Evidencia Multimedia       | Como sistema, quiero almacenar evidencia fotográfica en S3/local.                                      |
| HU27 | E04   | Interfaz para Dashboard Web                | Como sistema, quiero exponer endpoints REST para el Dashboard Web.                                     |
| HU30 | E04   | Autenticación mediante Tokens JWT          | Como sistema, quiero autenticar usuarios mediante tokens JWT.                                          |
| HU46 | E04   | API Gateway Centralizado                   | Como sistema, quiero implementar un API Gateway como punto único de entrada.                           |
| HU34 | E06   | Provisionamiento de Servidores EC2         | Como equipo, queremos provisionar servidores EC2 para los microservicios.                              |
| HU35 | E06   | Configuración de Base de Datos MySQL       | Como equipo, queremos configurar una base de datos MySQL administrada.                                 |
| HU36 | E06   | Almacenamiento en S3                       | Como equipo, queremos configurar un bucket S3 para evidencia multimedia.                               |
| HU37 | E06   | Tuberías CI/CD                             | Como equipo, queremos implementar tuberías CI/CD con GitHub Actions.                                   |
| HU38 | E06   | Seguridad SSL/TLS                          | Como equipo, queremos configurar HTTPS para todas las comunicaciones.                                  |
| HU32 | E06   | Cifrado de Información en Tránsito         | Como equipo, queremos cifrar la información en tránsito entre servicios.                               |
| HU39 | E06   | Monitoreo y Escalabilidad                  | Como equipo, queremos implementar monitoreo y escalabilidad del sistema.                               |

### 6.3 Planificación de Sprints

| Sprint       | Nombre                            | Duración        | Objetivo                                                               |
| ------------ | --------------------------------- | --------------- | ---------------------------------------------------------------------- |
| **Sprint 1** | Planificación y Blueprint Técnico | 22 Mar - 5 Abr  | Documentación, diagramas, prototipos y artefactos SCRUM                |
| **Sprint 2** | Core Backend (Local)              | 27 Mar - 12 Abr | Entornos, base de datos, JWT, RBAC, API Gateway                        |
| **Sprint 3** | IA y Visión Computacional         | 13 Abr - 19 Abr | Microservicio YOLO + OCR de detección de patentes                      |
| **Sprint 4** | App Móvil - Captura y Perfil      | 20 Abr - 28 Abr | App móvil: login, cámara, integración detección                        |
| **Sprint 5** | Registro Multimodal               | 29 Abr - 9 May  | Flujo completo móvil: GPS, fotos, consulta vehículo, infracción        |
| **Sprint 6** | Dashboard Web - Gestión           | 10 May - 19 May | Dashboard: login, listado infracciones, PDF, gestión estados, usuarios |
| **Sprint 7** | Supervisión, Métricas y Reportes  | 20 May - 30 May | Dashboard métricas, vista fiscalizadores, reportes, mapas de calor     |
| **Sprint 8** | Infraestructura Cloud y DevOps    | 31 May - 9 Jun  | Despliegue AWS, CI/CD, SSL, hardening                                  |
| **Sprint 9** | Calidad y Auditoría Final         | 10 Jun - 16 Jun | QA, logs auditoría, monitoreo, documentación final                     |

---

## 7. Arquitectura del Sistema

### 7.1 Diagrama de Arquitectura

![Diagrama de Arquitectura](Diagramas%20Scrum/imgs/Diagrama%20de%20arquitectura_page-0001.jpg)

_Ver diagrama original:_ [Diagrama de Arquitectura](Diagramas%20Scrum/Diagrama%20de%20arquitectura.pdf)

### 7.2 Estilo Arquitectónico

**Microservicios (Cliente-Servidor)** con 3 capas:

1. **Capa Cliente:** App móvil Android (Kotlin/Jetpack Compose) + Dashboard Web (React/Vite)
2. **Capa de Servicios:** API Gateway (Spring Cloud), Auth Service (Spring Boot), Core Service (Spring Boot), Plate Detector (FastAPI/Python)
3. **Capa de Datos:** Bases de datos MySQL (Auth DB + Core DB), AWS S3 para almacenamiento de imágenes

### 7.3 Topología de Red

```
                        ┌─────────────────────┐
                        │   SIFA GO (Mobile)  │
                        │  Android / Kotlin   │
                        └─────────┬───────────┘
                                  │
┌──────────────┐          ┌───────▼───────────┐          ┌──────────────────┐
│   SIFA Web   │          │     API Gateway   │          │  Plate Detector  │
│  Dashboard   │◄────────►│   Spring Cloud    │◄────────►│   YOLO + OCR     │
│  React/Vite  │          │   Gateway :9000   │          │   FastAPI :8001  │
└──────────────┘          └───────┬───────────┘          └──────────────────┘
                                  │
          ┌───────────────────────┼───────────────────────┐
          │                       │                       │
          ▼                       ▼                       ▼
┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐
│  Auth Service    │   │   Core Service   │   │    MySQL DBs     │
│  Spring Boot 3   │   │  Spring Boot 3   │   │  (Auth + Core)   │
│  :8081           │   │  :8083           │   │                  │
└──────────────────┘   └──────────────────┘   └──────────────────┘
                         ▲
                         │
          ┌──────────────┴──────────────┐
          │     AWS Cloud (Terraform)   │
          │  EC2, VPC, S3, NAT, EIP    │
          └─────────────────────────────┘
```

### 7.4 Flujo de Funcionamiento

1. **Captura:** El fiscalizador usa **SIFA GO** (móvil) para fotografiar una patente.
2. **Detección:** La imagen se envía al **YOLO Plate Detector**, que devuelve el texto de la patente (< 3 segundos).
3. **Consulta:** SIFA GO consulta el **Core Service** (vía API Gateway) para obtener datos del vehículo (marca, modelo, año, color).
4. **Validación:** El fiscalizador decide si cursar o no la infracción.
5. **Evidencia:** Fotos adicionales, coordenadas GPS y timestamp se adjuntan automáticamente.
6. **Infracción:** Se registra la infracción con "One-Tap" (ciclo total < 30 segundos). A prueba de fallos: integridad transaccional "All or Nothing".
7. **Gestión:** Las infracciones aparecen en tiempo real en el **Dashboard Web** (SIFA Control), donde el Administrativo JPL las revisa, acepta o rechaza.
8. **Citación:** Al aceptar, se genera un **PDF** oficial (con código QR y firma digital del fiscalizador) listo para impresión.
9. **Reportes:** Los supervisores pueden generar mapas de calor, reportes de productividad y ver estado de fiscalizadores en tiempo real.

### 7.5 Diagrama de Componentes

![Diagrama de Componentes](Diagramas%20Scrum/imgs/Diagrama%20de%20componentes_page-0001.jpg)

_Ver diagrama original:_ [Diagrama de Componentes](Diagramas%20Scrum/Diagrama%20de%20componentes.pdf)

### 7.6 Diagrama de Casos de Uso

![Diagrama de Casos de Uso](Diagramas%20Scrum/imgs/Diagrama%20de%20casos%20de%20uso_page-0001.jpg)

_Ver diagrama original:_ [Diagrama de Casos de Uso](Diagramas%20Scrum/Diagrama%20de%20casos%20de%20uso.pdf)

### 7.7 Diagrama de Flujo

![Diagrama de Flujo](Diagramas%20Scrum/imgs/Diagrama%20de%20flujo_page-0001.jpg)

_Ver diagrama original:_ [Diagrama de Flujo](Diagramas%20Scrum/Diagrama%20de%20flujo.pdf)

### 7.8 Esquema de Base de Datos

![Esquema de Base de Datos](Diagramas%20Scrum/imgs/Esquema_BD.png)

_Ver diagrama original:_ [Esquema de Base de Datos](Diagramas%20Scrum/Esquema_BD.pdf)

Entidades principales:

- **Usuarios:** id, rut, nombre, email, password_hash, rol, activo, creado_en
- **Roles:** id, nombre, descripción
- **Fiscalizadores:** id, user_id, firma_url, activo, latitud, longitud, ultimo_heartbeat
- **Vehículos:** id, patente, marca, modelo, año, color, propietario
- **Infracciones:** id, patente, vehiculo_id, fiscalizador_id, estado, fecha_hora, latitud, longitud, direccion, observacion, creado_en
- **Evidencias:** id, infraccion_id, tipo (foto/gps), url, fecha_hora
- **Auditoría:** id, usuario_id, accion, entidad, entidad_id, detalle, timestamp
- **Tipos de Infracción:** id, nombre, descripción, monto, activo

---

## 8. Diagramas del Sistema

Los siguientes diagramas se encuentran disponibles en formato PDF dentro del repositorio:

| Diagrama                 | Archivo                                                                                                              |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| Diagrama de Arquitectura | [Documentación/Diagramas Scrum/Diagrama de arquitectura.pdf](Diagramas%20Scrum/Diagrama%20de%20arquitectura.pdf)     |
| Diagrama de Casos de Uso | [Documentación/Diagramas Scrum/Diagrama de casos de uso.pdf](Diagramas%20Scrum/Diagrama%20de%20casos%20de%20uso.pdf) |
| Diagrama de Componentes  | [Documentación/Diagramas Scrum/Diagrama de componentes.pdf](Diagramas%20Scrum/Diagrama%20de%20componentes.pdf)       |
| Diagrama de Flujo        | [Documentación/Diagramas Scrum/Diagrama de flujo.pdf](Diagramas%20Scrum/Diagrama%20de%20flujo.pdf)                   |
| Esquema de Base de Datos | [Documentación/Diagramas Scrum/Esquema_BD.pdf](Diagramas%20Scrum/Esquema_BD.pdf)                                     |
| Impact Mapping           | [Documentación/Diagramas Scrum/Impact Mapping.pdf](Diagramas%20Scrum/Impact%20Mapping.pdf)                           |
| Mapa de Actores          | [Documentación/Diagramas Scrum/Mapa de actores.pdf](Diagramas%20Scrum/Mapa%20de%20actores.pdf)                       |
| User Story Mapping       | [Documentación/Diagramas Scrum/User Story Mapping.pdf](Diagramas%20Scrum/User%20Story%20Mapping.pdf)                 |

### Impact Mapping

![Placeholder: Impact Mapping](Diagramas%20Scrum/imgs/Impact%20Mapping_page-0001.jpg)

_Ver diagrama original:_ [Impact Mapping](Diagramas%20Scrum/Impact%20Mapping.pdf)

### User Story Mapping

![Placeholder: User Story Mapping](Diagramas%20Scrum/imgs/User%20Story%20Mapping_page-0001.jpg)

_Ver diagrama original:_ [User Story Mapping](Diagramas%20Scrum/User%20Story%20Mapping.pdf)

---

## 9. Mockups y Prototipos

### 9.1 Mockup Aplicación Móvil

![Placeholder: Mockup App Móvil SIFA GO](Mockups/Mockup%20App%20Móvil.png)

_Ver mockup original en:_ [Mockup App Móvil SIFA GO](Mockups/Mockup%20App%20Móvil.png)

### 9.2 Mockup Aplicación Web

![Placeholder: Mockup Aplicación Web](Mockups/Mockup%20web01.jpg)

![Placeholder: Mockup Aplicación Web](Mockups/Mockup%20web02.jpg)

_Ver mockup original en:_

- [Mockup Aplicación Web page 1](Mockups/Mockup%20web01.jpg)
- [Mockup Aplicación Web page 2](Mockups/Mockup%20web02.jpg)

### 9.3 Pantallas Principales

| Pantalla                   | Descripción                                                      | Usuario          |
| -------------------------- | ---------------------------------------------------------------- | ---------------- |
| Login Móvil                | Inicio de sesión con RUT + contraseña y opción de huella digital | Fiscalizador     |
| Captura de Patente         | Vista de cámara con detección automática de patente              | Fiscalizador     |
| Confirmación de Infracción | Resumen de datos: patente, vehículo, GPS, fotos                  | Fiscalizador     |
| Dashboard Web              | Panel con KPIs, mapa de calor y métricas en tiempo real          | Supervisor/Admin |
| Bandeja de Infracciones    | Listado de infracciones con filtros y estados                    | Admin JPL        |
| Detalle de Infracción      | Vista completa con evidencia, mapa y datos del vehículo          | Admin JPL        |
| Gestión de Usuarios        | CRUD de usuarios con asignación de roles                         | Administrador    |
| Reportes                   | Generación de reportes operacionales y exportación PDF           | Supervisor       |

---

## 10. Glosario

| Término           | Definición                                                                             |
| ----------------- | -------------------------------------------------------------------------------------- |
| **Fiscalizador**  | Inspector municipal que opera en terreno fiscalizando vehículos.                       |
| **Infracción**    | Registro de una falta de tránsito cometida por un vehículo.                            |
| **Patente (PPU)** | Placa Patente Única que identifica un vehículo.                                        |
| **Citación JPL**  | Documento oficial que cita al infractor al Juzgado de Policía Local.                   |
| **JPL**           | Juzgado de Policía Local, tribunal encargado de juzgar infracciones de tránsito.       |
| **YOLO**          | Modelo de visión computacional para detección de objetos en tiempo real.               |
| **OCR**           | Reconocimiento óptico de caracteres para extraer texto de imágenes.                    |
| **One-Tap**       | Flujo de un solo toque para registrar una infracción rápidamente.                      |
| **RBAC**          | Control de acceso basado en roles (Role-Based Access Control).                         |
| **JWT**           | JSON Web Token, estándar para autenticación stateless.                                 |
| **API Gateway**   | Punto único de entrada que enruta solicitudes a microservicios.                        |
| **Terraform**     | Herramienta IaC para provisionar infraestructura cloud.                                |
| **Audit Log**     | Registro inmutable de todas las acciones realizadas en el sistema.                     |
| **Heartbeat**     | Señal periódica enviada por la app móvil para indicar que el fiscalizador está activo. |

---

## Anexos

### A. KPIs y Métricas del Proyecto

| KPI                                     | Objetivo                                          |
| --------------------------------------- | ------------------------------------------------- |
| Precisión de reconocimiento de patentes | ≥ 85% en entornos controlados                     |
| Tiempo de registro de infracción        | ≤ 10 segundos (ideal) / ≤ 30 segundos (requisito) |
| Disponibilidad del sistema              | ≥ 99%                                             |
| Correctitud de exportación de datos     | 100% de casos de prueba                           |
| Tiempo de respuesta de IA               | < 3 segundos                                      |
| Tiempo de respuesta de API              | < 2 segundos                                      |
| Umbral de confianza YOLO                | > 75%                                             |
| Tamaño de imagen optimizado             | < 5 MB por foto                                   |
| Heartbeat (telemetría fiscalizador)     | Cada 3 minutos                                    |

### B. Gestión de Riesgos

| Riesgo                                | Impacto | Probabilidad | Mitigación                            |
| ------------------------------------- | ------- | ------------ | ------------------------------------- |
| Baja precisión del modelo IA          | Alto    | Media        | Dataset adicional, pruebas iterativas |
| Falta de dataset de patentes chilenas | Alto    | Alta         | Datasets públicos + generación manual |
| Problemas de integración              | Alto    | Media        | Pruebas de integración tempranas      |
| Limitaciones de tiempo                | Alto    | Alta         | Priorización MVP                      |
| Fallas en servicios cloud             | Medio   | Baja         | Servicios AWS administrados           |
| Curva de aprendizaje tecnológica      | Medio   | Media        | Distribución de tareas por expertise  |
| Problemas de conectividad en terreno  | Medio   | Alta         | Almacenamiento local temporal         |

### C. Enlaces de Interés

- **Figma (UI/UX):** https://www.figma.com/design/lPnmPA1nVhkHiIynNSZC1I/SIFA
- **Canva (Diagramas):** https://www.canva.com/design/DAHETfLsYtY/CRJ4o-yosxk7U3FmVpG3VA/edit
- **Jira (SCRUM):** https://sifa-proyect.atlassian.net/jira/software/projects/SCRUM/boards/1
- **Miro (Arquitectura):** [Tablero Miro](https://miro.com/welcomeonboard/NGJXeTJZb2l6dC9zWm43NVFzWWpVZVRYM2lMMzBJeUxwSE9NNm9pMGZWRUlxWGF4VnViaEl5aFJNK2MwTDc5Z3VCK05DWDZ2TnNEZDduRHdxai9VSWlidTZRK2Z1L2hVMk1VUTZhL0ViR0NWL2lXaktHaE5BZU1iVFdqT0pxam5BS2NFM01kcUNFSnM0d3FEN050ekl3PT0hdjE=)
- **Mockup Móvil:** `Documentación/Mockups/Mockup App Móvil.png`

---

**Fin del documento ERS — SIFA v1.0**
