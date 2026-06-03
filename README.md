# SIFA — Sistema Integrado de Fiscalización Automatizada

**SIFA** es un ecosistema de microservicios desarrollado para la **I. Municipalidad de El Quisco**, cuyo objetivo es digitalizar y automatizar el proceso de fiscalización vehicular mediante detección de patentes con inteligencia artificial, gestión de infracciones, y emisión de citaciones del Juzgado de Policía Local (JPL).

Este repositorio centraliza la documentación y los enlaces de todos los microservicios que componen el proyecto, desarrollado como evaluación para el ramo **Taller Aplicado de Programación TPY1101-001D** de la **Escuela de Informática de DUOC UC**.

---

## Equipo de Desarrollo

| Integrante | Rol | GitHub |
|------------|-----|--------|
| **Leonel Briones Palacios** | Backend & Infraestructura | [@jarodsmdev](https://github.com/jarodsmdev) |
| **Nicolás Alejandro López Plaza** | Frontend Web | [@Nicolas-15](https://github.com/Nicolas-15) |
| **Andrés Vicente Ortega Suazo** | App Móvil & Core Service | [@Andythe20](https://github.com/Andythe20) |

---

## Arquitectura del Sistema

```
                         ┌─────────────────────┐
                         │   SIFA GO (Mobile)  │
                         │  Android / Kotlin   │
                         └─────────┬───────────┘
                                   │
┌──────────────┐          ┌───────▼───────────┐          ┌──────────────────┐
│   SIFA Web   │          │     API Gateway   │          │  Plate Detector  │
│  Dashboard   │◄────────►│   Spring Cloud    │◄────────►│   YOLO + OCR     │
│  React / Vite│          │   Gateway :9000   │          │   FastAPI :8001  │
└──────────────┘          └───────┬───────────┘          └──────────────────┘
                                  │
          ┌───────────────────────┼───────────────────────┐
          │                       │                       │
          ▼                       ▼                       ▼
┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐
│  Auth Service    │   │   Core Service   │   │    MySQL DBs     │
│  Spring Boot     │   │  Spring Boot     │   │  (Auth + Core)   │
│  :8081           │   │  :8083           │   │                  │
└──────────────────┘   └──────────────────┘   └──────────────────┘
                         ▲
                         │
          ┌──────────────┴──────────────┐
          │     AWS Cloud (Terraform)   │
          │  EC2, VPC, S3, NAT, EIP    │
          └─────────────────────────────┘
```

---

## Repositorios del Ecosistema

### Backend

| Servicio | Tecnología | Puerto | Repositorio |
|----------|-----------|--------|-------------|
| **API Gateway** | Spring Cloud Gateway / Java 17 | `:9000` | [BEsifaAPIGateway](https://github.com/jarodsmdev/BEsifaAPIGateway) |
| **Auth Service** | Spring Boot 3 / Java 21 / MySQL | `:8081` | [BEsifaAuthService](https://github.com/jarodsmdev/BEsifaAuthService) |
| **Core Service** | Spring Boot 3 / Java 21 / MySQL | `:8083` | [BEsifaCoreService](https://github.com/Andythe20/BEsifaCoreService) |
| **YOLO Plate Detector** | FastAPI / Python / Tesseract OCR | `:8001` | [sifaPlateDetectorBE](https://github.com/jarodsmdev/sifaPlateDetectorBE) |

### Frontend

| Aplicación | Tecnología | Repositorio |
|-----------|-----------|-------------|
| **Dashboard Web SIFA** | React 18 / Vite / Tailwind CSS | [SIFA_Dashboard](https://github.com/Nicolas-15/SIFA_Dashboard) |
| **SIFA GO (App Móvil)** | Android / Kotlin / Jetpack Compose | [sifa_go](https://github.com/Andythe20/sifa_go) |

### Infraestructura Cloud

| Proyecto | Herramienta | Repositorio |
|---------|------------|-------------|
| **Infraestructura SIFA** | Terraform / AWS (EC2, VPC, S3, NAT) | [TerraformProyectSIFA](https://github.com/jarodsmdev/TerraformProyectSIFA) |
| **MySQL en EC2** | Terraform / AWS EC2 | [Terraform_EC2_MySQL](https://github.com/jarodsmdev/Terraform_EC2_MySQL) |

---

## Descripción de Componentes

### 1. API Gateway (`BEsifaAPIGateway`)
Punto único de entrada para toda la arquitectura. Implementado con **Spring Cloud Gateway**, valida tokens JWT localmente, inyecta headers de auditoría (`X-Auth-User`, `X-Auth-Token-Valid`) y redirige el tráfico a los microservicios internos según la ruta.

### 2. Auth Service (`BEsifaAuthService`)
Microservicio de autenticación basado en **Spring Boot 3 + Spring Security + JWT**. Gestiona registro, login y roles de usuario (Administrador, Supervisor, Administrativo JPL, Fiscalizador). Persiste en una base de datos **MySQL** independiente.

### 3. Core Service (`BEsifaCoreService`)
Microservicio central que gestiona infracciones, evidencias fotográficas (imágenes encriptadas) y citaciones JPL. No valida JWT directamente; recibe la identidad del usuario desde el Gateway mediante headers. Es el cerebro de la lógica de negocio del sistema.

### 4. YOLO Plate Detector (`sifaPlateDetectorBE`)
Servicio de **visión artificial** desarrollado en **FastAPI**. Utiliza un modelo **YOLO** para detectar patentes vehiculares en imágenes y **Tesseract OCR** para extraer el texto. Corre en un contenedor Docker y expone un endpoint `POST /detect`.

### 5. Dashboard Web SIFA (`SIFA_Dashboard`)
Panel administrativo construido con **React 18 + Vite + Tailwind CSS**. Incluye:
- KPIs en tiempo real, mapas de calor con Leaflet
- Gestión de infracciones con carrusel de evidencias
- Catálogo de tipos de infracción (CRUD)
- Administración de usuarios con validación de RUT chileno
- Generación de documentos PDF (empadronado JPL)
- Control de acceso basado en roles (RBAC)

### 6. SIFA GO (`sifa_go`)
Aplicación móvil nativa **Android** en **Kotlin + Jetpack Compose** para fiscalizadores en terreno. Captura fotografías de patentes mediante la **Cámara**, las envía al servicio YOLO para su procesamiento y prepara el flujo de generación de multas.

### 7. Terraform SIFA Cloud (`TerraformProyectSIFA`)
Infraestructura como código (IaC) para desplegar el ecosistema en **AWS**. Define VPC con subnets públicas y privadas, instancias EC2, NAT Gateway, Security Groups, bucket S3 para imágenes y backend remoto de Terraform con S3 + DynamoDB.

### 8. Terraform EC2 MySQL (`Terraform_EC2_MySQL`)
Módulo adicional de Terraform para desplegar una instancia EC2 con **MySQL** preconfigurado, ideal para entornos de desarrollo o bases de datos auxiliares.

---

## Flujo de Funcionamiento

1. **Captura:** El fiscalizador usa **SIFA GO** (móvil) para fotografiar una patente.
2. **Detección:** La imagen se envía al **YOLO Plate Detector**, que devuelve el texto de la patente.
3. **Consulta:** SIFA GO consulta el **Core Service** (vía API Gateway) para obtener datos del vehículo.
4. **Validación:** El fiscalizador decide si cursar o no la infracción.
5. **Gestión:** Las infracciones ingresadas se visualizan en el **Dashboard Web**, donde Administrativos JPL las revisan, aceptan o rechazan.
6. **Citación:** Al aceptar una infracción, se genera un **PDF** oficial de empadronamiento listo para impresión.

---

## Stack Tecnológico General

| Área | Tecnologías |
|------|------------|
| **Backend** | Java 17/21, Spring Boot 3, Spring Cloud Gateway, Spring Security, JPA |
| **IA / OCR** | Python, FastAPI, YOLO, Tesseract OCR |
| **Frontend Web** | React 18, Vite, Tailwind CSS, Recharts, Leaflet, jsPDF |
| **Mobile** | Kotlin, Jetpack Compose, CameraX, Retrofit |
| **Base de Datos** | MySQL 8+ |
| **Infraestructura** | Terraform, AWS (EC2, VPC, S3, NAT Gateway, DynamoDB) |
| **Contenedores** | Docker, Docker Compose |

---

## Estructura de este Repositorio

```
GITHUB_ENTREGA/
├── README.md                  ← Este archivo
├── Documentación/
│   ├── Diagramas Scrum/
│   ├── Docs Scrum/
│   ├── Mockups/
│   └── README.md
├── Gestión/
│   ├── 1.1.2 Documento de registro...
│   └── integrantes.txt
└── Producto/
    └── README.md
```

---

## Recursos del Proyecto

- **Diseño UI/UX (Figma):** [Ver prototipos](https://www.figma.com/design/lPnmPA1nVhkHiIynNSZC1I/SIFA)
- **Diagramas del Sistema (Canva):** [Ver diagramas](https://www.canva.com/design/DAHETfLsYtY/CRJ4o-yosxk7U3FmVpG3VA/edit)
- **Tablero SCRUM (Jira):** [Ver tablero](https://sifa-proyect.atlassian.net/jira/software/projects/SCRUM/boards/1)
- **Arquitectura y Planeación (Miro):** [Ver tablero](https://miro.com/welcomeonboard/NGJXeTJZb2l6dC9zWm43NVFzWWpVZVRYM2lMMzBJeUxwSE9NNm9pMGZWRUlxWGF4VnViaEl5aFJNK2MwTDc5Z3VCK05DWDZ2TnNEZDduRHdxai9VSWlidTZRK2Z1L2hVMk1VUTZhL0ViR0NWL2lXaktHaE5BZU1iVFdqT0pxam5BS2NFM01kcUNFSnM0d3FEN050ekl3PT0hdjE=)

---

## ¿Cómo empezar?

Cada repositorio contiene su propio README con instrucciones detalladas de instalación y ejecución. Para levantar el sistema completo en local:

1. Clona cada repositorio listado arriba
2. Sigue las instrucciones de cada README (requisitos, variables de entorno, etc.)
3. Usa **Docker Compose** donde esté disponible para simplificar el despliegue
4. Asegúrate de que el **API Gateway** esté corriendo para conectar los servicios

---

## Licencia

Proyecto desarrollado con fines académicos para la **Escuela de Informática de DUOC UC**.
