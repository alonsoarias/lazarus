# Documento de Especificación y Diseño (DED)
# Sistema de Gestión Académico-Administrativa "Lazarus"

**Versión:** 1.0  
**Fecha:** 11 de mayo de 2025  
**Autor:** [Nombre del Consultor/Arquitecto]  
**Cliente:** Instituciones Educativas Colombianas  
**Desarrollador Principal:** Alonso Arias

---

## Índice

1. [Introducción](#1-introducción)
   1. [Propósito del documento](#11-propósito-del-documento)
   2. [Alcance del sistema](#12-alcance-del-sistema)
   3. [Definiciones, acrónimos y abreviaturas](#13-definiciones-acrónimos-y-abreviaturas)
   4. [Referencias](#14-referencias)
   5. [Visión general del documento](#15-visión-general-del-documento)

2. [Descripción general](#2-descripción-general)
   1. [Perspectiva del producto](#21-perspectiva-del-producto)
   2. [Funciones del producto](#22-funciones-del-producto)
   3. [Características de los usuarios](#23-características-de-los-usuarios)
   4. [Restricciones](#24-restricciones)
   5. [Suposiciones y dependencias](#25-suposiciones-y-dependencias)

3. [Contexto y Alcance](#3-contexto-y-alcance)
   1. [Nombre clave y branding](#31-nombre-clave-y-branding)
   2. [Modelo de licenciamiento](#32-modelo-de-licenciamiento)
   3. [Segmento de clientes objetivo](#33-segmento-de-clientes-objetivo)
   4. [Stack tecnológico](#34-stack-tecnológico)
   5. [Proceso de instalación](#35-proceso-de-instalación)
   6. [Modelo de desarrollo](#36-modelo-de-desarrollo)
   7. [Arquitectura general](#37-arquitectura-general)

4. [Modelo de MVP por Plugins](#4-modelo-de-mvp-por-plugins)
   1. [Estrategia de releases](#41-estrategia-de-releases)
   2. [Detalle de plugins por release](#42-detalle-de-plugins-por-release)
   3. [Roadmap visual](#43-roadmap-visual)

5. [Roles y Permisos](#5-roles-y-permisos)
   1. [Modelo de roles](#51-modelo-de-roles)
   2. [Permisos por rol](#52-permisos-por-rol)
   3. [Matriz CRUD×Rol](#53-matriz-crudrol)
   4. [Gestión y exportación de roles](#54-gestión-y-exportación-de-roles)

6. [Plugins y Requisitos Funcionales](#6-plugins-y-requisitos-funcionales)
   1. [Core](#61-core)
   2. [Installer](#62-installer)
   3. [RBAC-Extended](#63-rbac-extended)
   4. [Académico-Basic](#64-académico-basic)
   5. [Boletines](#65-boletines)
   6. [License-Manager](#66-license-manager)
   7. [Payments](#67-payments)
   8. [Analytics](#68-analytics)
   9. [Certificaciones](#69-certificaciones)
   10. [Migrator](#610-migrator)
   11. [LMS-Bridge](#611-lms-bridge)
   12. [Observador](#612-observador)

7. [Requisitos No Funcionales](#7-requisitos-no-funcionales)
   1. [Rendimiento](#71-rendimiento)
   2. [Seguridad](#72-seguridad)
   3. [Disponibilidad](#73-disponibilidad)
   4. [Escalabilidad](#74-escalabilidad)
   5. [Usabilidad](#75-usabilidad)
   6. [Mantenibilidad](#76-mantenibilidad)
   7. [Portabilidad](#77-portabilidad)
   8. [Compatibilidad](#78-compatibilidad)

8. [Normativa y Cumplimiento](#8-normativa-y-cumplimiento)
   1. [Marco legal colombiano](#81-marco-legal-colombiano)
   2. [Protección de datos personales](#82-protección-de-datos-personales)
   3. [Integraciones oficiales](#83-integraciones-oficiales)
   4. [Estándares técnicos obligatorios](#84-estándares-técnicos-obligatorios)

9. [Diseño Técnico](#9-diseño-técnico)
   1. [Arquitectura del sistema](#91-arquitectura-del-sistema)
   2. [Diseño de base de datos](#92-diseño-de-base-de-datos)
   3. [Diseño de APIs](#93-diseño-de-apis)
   4. [Diseño de seguridad](#94-diseño-de-seguridad)
   5. [Diseño de escalabilidad](#95-diseño-de-escalabilidad)
   6. [Diagramas UML](#96-diagramas-uml)

10. [Historias de Usuario](#10-historias-de-usuario)
    1. [Historias de Core](#101-historias-de-core)
    2. [Historias por plugin](#102-historias-por-plugin)

11. [Plan de Pruebas](#11-plan-de-pruebas)
    1. [Estrategia de pruebas](#111-estrategia-de-pruebas)
    2. [Tipos de pruebas](#112-tipos-de-pruebas)
    3. [Matriz de pruebas por plugin](#113-matriz-de-pruebas-por-plugin)

12. [Estrategia de Migración](#12-estrategia-de-migración)
    1. [Proceso de migración](#121-proceso-de-migración)
    2. [Herramientas de migración](#122-herramientas-de-migración)
    3. [Validación de datos migrados](#123-validación-de-datos-migrados)

13. [Plan de Capacitación](#13-plan-de-capacitación)
    1. [Usuarios administradores](#131-usuarios-administradores)
    2. [Usuarios finales](#132-usuarios-finales)
    3. [Recursos de autoservicio](#133-recursos-de-autoservicio)

14. [Cronograma de Desarrollo](#14-cronograma-de-desarrollo)
    1. [Resumen de sprints](#141-resumen-de-sprints)
    2. [Cronograma detallado de 24 meses](#142-cronograma-detallado-de-24-meses)
    3. [Hitos principales](#143-hitos-principales)

15. [Anexos Técnicos](#15-anexos-técnicos)
    1. [Ejemplo de archivo .env](#151-ejemplo-de-archivo-env)
    2. [Plantilla my.cnf optimizada](#152-plantilla-mycnf-optimizada)
    3. [Estructura archivo .lzm](#153-estructura-archivo-lzm)
    4. [Configuración webhook WooCommerce](#154-configuración-webhook-woocommerce)
    5. [Plantilla de roles JSON](#155-plantilla-de-roles-json)
    6. [Esquema manifest.json para plugins](#156-esquema-manifestjson-para-plugins)

---

## 1. Introducción

### 1.1 Propósito del documento

Este Documento de Especificación y Diseño (DED) tiene como propósito definir de manera exhaustiva y autocontenida los requisitos, arquitectura, diseño técnico y plan de implementación del sistema de gestión académico-administrativa "Lazarus" para instituciones educativas colombianas. El documento servirá como guía principal para el desarrollo del sistema, proporcionando una visión clara de todos los componentes, restricciones y expectativas, facilitando así la implementación incremental por un solo desarrollador.

### 1.2 Alcance del sistema

Lazarus es un sistema modular de gestión académico-administrativa diseñado específicamente para colegios colombianos. El sistema contempla:

- Gestión académica (calificaciones, boletines, certificados)
- Administración de roles y permisos extensible
- Compatibilidad multi-sede y multi-institución
- Integración con sistemas oficiales colombianos
- Modelo de licenciamiento flexible (SaaS y On-Premise)
- Arquitectura basada en plugins para extensibilidad

El sistema está destinado a instituciones de educación básica y media, tanto públicas como privadas, contemplando diferentes calendarios académicos (A, B y flexibles) y modalidades (presencial, virtual, a distancia).

El proyecto contempla un periodo de desarrollo de 48 meses por un único desarrollador, con entregas incrementales de funcionalidad a través de plugins que permitan un uso temprano del sistema mientras se continúa el desarrollo de funcionalidades avanzadas.

#### Dentro del alcance:

- Sistema core multi-tenancy con aislamiento de datos por institución
- Proceso de instalación simplificado ("One-Click" y CLI)
- Gestión académica básica (años académicos, periodos, cursos, asignaturas)
- Sistema de calificaciones flexible y adaptable a diferentes modelos evaluativos
- Generación de boletines y certificados oficiales
- Gestión de licencias y pagos por documentos
- Análisis estadístico y sistema de alertas
- Herramientas de migración de datos
- Integración con sistemas de aprendizaje (Moodle)
- Observador del estudiante y seguimiento disciplinario
- Conformidad con normativa educativa colombiana

#### Fuera del alcance:

- Sistema contable completo (solo se incluye gestión básica de pagos)
- Gestión de nómina de personal
- Administración de recursos físicos de la institución
- Plataforma LMS propia (se integra con existentes)
- Aplicaciones móviles nativas (la interfaz será responsive)
- Sistema de alimentación escolar
- Horario automatizado (solo gestión de asignación académica)

### 1.3 Definiciones, acrónimos y abreviaturas

- **DED**: Documento de Especificación y Diseño
- **MVP**: Producto Mínimo Viable
- **SaaS**: Software como Servicio
- **On-Premise**: Implementación local en servidores del cliente
- **JWT**: JSON Web Token
- **RBAC**: Control de Acceso Basado en Roles
- **LMS**: Sistema de Gestión de Aprendizaje
- **CRUD**: Crear, Leer, Actualizar, Eliminar
- **MFA/2FA**: Autenticación de Múltiples Factores/Dos Factores
- **API**: Interfaz de Programación de Aplicaciones
- **SA**: Super-Administrador
- **AC**: Admin-Cliente
- **AI**: Admin-Institución
- **AS**: Admin-Sede

### 1.4 Referencias

- Ley General de Educación (Ley 115 de 1994)
- Decreto 1860 de 1994
- Decreto 1290 de 2009
- Decreto 1075 de 2015
- Ley 594 de 2000 (Ley General de Archivos)
- Ley 1581 de 2012 y Decreto 1377 de 2013 (Protección de Datos Personales)
- Ley 87 de 1993 (Control Interno)
- ISO/IEC 27001 (Seguridad de la Información)
- WCAG 2.1 AA (Accesibilidad Web)

### 1.5 Visión general del documento

Este documento está estructurado en 15 secciones que abarcan desde la introducción y contexto hasta los anexos técnicos. Incluye especificaciones detalladas de todos los componentes del sistema, requisitos funcionales y no funcionales, diagramas técnicos, matrices de permisos, cronograma de implementación, y otros aspectos cruciales para el desarrollo exitoso del sistema.

## 2. Descripción general

### 2.1 Perspectiva del producto

Lazarus es un sistema independiente pero interoperable con otros sistemas educativos colombianos. Está diseñado como una solución integral que puede funcionar de forma autónoma o complementando otras herramientas mediante integraciones. Su arquitectura modular basada en plugins permite a las instituciones activar solo las funcionalidades necesarias según sus necesidades específicas.

El sistema se posiciona como una alternativa moderna y adaptable a las soluciones existentes en el mercado colombiano, con ventajas en términos de flexibilidad, cumplimiento normativo y adaptabilidad a diferentes tipos de instituciones educativas.

### 2.2 Funciones del producto

El sistema Lazarus ofrece las siguientes funciones principales:

- **Gestión académica**: Registro y administración de calificaciones, generación de boletines, certificados y diplomas.
- **Control de usuarios**: Sistema avanzado de roles y permisos adaptable a la estructura organizacional de cada institución.
- **Multi-tenancy**: Soporte para múltiples instituciones y sedes con aislamiento de datos.
- **Analítica educativa**: Indicadores de rendimiento académico y alertas tempranas.
- **Gestión documental**: Generación y administración de documentos académicos oficiales.
- **Integración con LMS**: Puente con sistemas de gestión de aprendizaje como Moodle.
- **Cumplimiento normativo**: Adaptación a las regulaciones colombianas para instituciones educativas.
- **Licenciamiento flexible**: Modelos SaaS y On-Premise con migración bidireccional.

### 2.3 Características de los usuarios

Lazarus está diseñado para ser utilizado por diversos actores del ecosistema educativo:

- **Administradores de sistema**: Usuarios técnicos que gestionan la plataforma a nivel general.
- **Directivos institucionales**: Rectores, coordinadores y directivos que administran la institución.
- **Personal administrativo**: Secretarias, personal financiero que gestiona procesos administrativos.
- **Docentes**: Profesores que registran calificaciones y observaciones de estudiantes.
- **Estudiantes**: Usuarios que consultan sus calificaciones y documentos académicos.
- **Acudientes**: Padres o tutores que acceden a la información académica de sus hijos/tutelados.
- **Invitados**: Usuarios con acceso limitado a información pública o compartida.

### 2.4 Restricciones

El desarrollo e implementación de Lazarus está sujeto a las siguientes restricciones:

- **Recursos de desarrollo**: Un solo desarrollador (Alonso Arias) con disponibilidad de 11 horas semanales.
- **Plazo de desarrollo**: 24 meses para el desarrollo completo del sistema.
- **Tecnología**: Stack tecnológico predefinido (Laravel 10 LTS, MariaDB, etc.).
- **Normativa colombiana**: Cumplimiento obligatorio de leyes y decretos educativos.
- **Protección de datos**: Requisitos estrictos de seguridad y privacidad de información sensible.
- **Infraestructura variable**: Debe funcionar en entornos con conectividad limitada o intermitente.

### 2.5 Suposiciones y dependencias

El diseño de Lazarus se basa en las siguientes suposiciones y dependencias:

- Las instituciones educativas tienen acceso a servidores que cumplen con los requisitos mínimos.
- Los usuarios administradores tienen conocimientos técnicos básicos para la instalación On-Premise.
- Existe compatibilidad entre las versiones del stack tecnológico seleccionado.
- Los sistemas oficiales (SIMAT, ICFES-PRISMA, etc.) mantienen sus interfaces de integración actuales.
- El marco normativo colombiano no sufre cambios drásticos durante el desarrollo.

## 3. Contexto y Alcance

### 3.1 Nombre clave y branding

**Nombre clave**: Lazarus

El nombre "Lazarus" evoca el concepto de revitalización y nueva vida, simbolizando la modernización de los procesos educativos tradicionales. Este nombre será utilizado tanto internamente en el desarrollo como en la comercialización del producto.

Elementos de branding a desarrollar:
- Logo principal y variantes
- Paleta de colores institucional
- Tipografía del sistema
- Iconografía consistente en interfaz

### 3.2 Modelo de licenciamiento

El sistema Lazarus implementa un modelo de licenciamiento dual:

**Modelo SaaS (Software como Servicio)**:
- Autenticación mediante clave JWT
- Facturación recurrente (mensual/anual)
- Almacenamiento en la nube gestionada por el proveedor
- Actualizaciones automáticas
- Soporte técnico incluido según nivel de suscripción

**Modelo On-Premise (Local)**:
- Licencia validada mediante archivo `.lzl`
- Pago único con mantenimiento anual opcional
- Instalación en servidores propios de la institución
- Actualizaciones manuales o programadas
- Soporte técnico según contrato de mantenimiento

**Migración bidireccional**:
- Transición SaaS → On-Premise y viceversa
- Proceso operado exclusivamente por el proveedor
- Preservación de integridad de datos
- Tiempo de migración garantizado ≤ 48 horas
- Verificación de consistencia post-migración

### 3.3 Segmento de clientes objetivo

El sistema está diseñado para atender a los siguientes segmentos de instituciones educativas colombianas:

**Por naturaleza jurídica**:
- Colegios públicos (oficiales)
- Colegios privados
- Colegios de administración mixta (concesiones)

**Por estructura organizacional**:
- Instituciones unisede
- Instituciones multi-sede
- Redes/grupos educativos

**Por calendario académico**:
- Calendario A (febrero-noviembre)
- Calendario B (agosto-junio)
- Calendarios flexibles o especiales

**Por modalidad educativa**:
- Educación presencial tradicional
- Educación virtual
- Educación a distancia
- Modelos híbridos

**Por niveles educativos**:
- Preescolar
- Básica primaria
- Básica secundaria
- Media académica y técnica
- Programas de educación para adultos

### 3.4 Stack tecnológico

El desarrollo de Lazarus se basará en las siguientes tecnologías:

**Backend**:
- Laravel 10 LTS (framework PHP)
- MariaDB ≥ 10.6 (base de datos relacional)
- Redis (caché y colas)
- stancl/tenancy (multi-tenancy - database-per-tenant)
- nWidart/modules (arquitectura modular para plugins)

**Frontend**:
- Livewire 3.x / Inertia.js (interfaces reactivas)
- TailwindCSS (framework CSS)
- Alpine.js (interactividad frontend mínima)
- HTMX (mejoras progresivas)

**Infraestructura**:
- Docker (contenedores para desarrollo y despliegue)
- Nginx (servidor web)
- Let's Encrypt (certificados SSL automáticos)
- Laravel Horizon (gestión de colas)
- Laravel Telescope (debugging - solo desarrollo)

**Herramientas de desarrollo**:
- PHPStan (análisis estático)
- Laravel Pint (formateador de código)
- PHPUnit (pruebas unitarias)
- Laravel Dusk (pruebas end-to-end)
- GitHub Actions (CI/CD)

### 3.5 Proceso de instalación

El sistema Lazarus ofrece dos métodos principales de instalación:

**Instalación "One-Click" (wizard web)**:
1. Subida de archivos a servidor
2. Acceso a URL de instalación
3. Verificación automática de requisitos del servidor
4. Formulario web para configuración de:
   - Datos de conexión a base de datos
   - Información de la institución
   - Configuración de correo electrónico
   - Ajustes de seguridad básicos
   - Zona horaria y configuración regional
5. Creación automática de bases de datos y archivos de configuración
6. Generación automática del archivo `.env` con los valores introducidos
7. Creación de usuario Super-Administrador
8. Activación de licencia (SaaS/On-Premise)
9. Redirección al dashboard inicial

**Instalación mediante CLI**:
1. Clonación/descarga de repositorio
2. Ejecución de script de instalación
   ```bash
   php artisan lazarus:install
   ```
3. Configuración mediante prompts interactivos o archivo de configuración
4. Verificación automática de dependencias
5. Migración y semilla de base de datos
6. Generación de claves de aplicación
7. Configuración de tareas programadas (cron)

Ambos métodos de instalación incluirán:
- Verificación de compatibilidad de versiones
- Comprobación de extensiones PHP requeridas
- Validación de permisos de archivos y directorios
- Creación automática de archivos de configuración

### 3.6 Modelo de desarrollo

El desarrollo de Lazarus seguirá estas directrices:

**Recursos humanos**:
- Un solo desarrollador principal (Alonso Arias)
- Dedicación de 11 horas semanales
- Periodo de desarrollo: 48 meses

**Metodología**:
- Desarrollo incremental basado en plugins
- Sprints de 2-3 semanas
- Pruebas continuas (TDD cuando sea aplicable)
- Releases pequeños y frecuentes
- Retrospectivas mensuales de avance

**Gestión del código**:
- Control de versiones con Git (GitHub)
- Ramas feature/hotfix/release
- Convención de commits semánticos
- Documentación inline del código (PHPDoc)
- Revisión estática con GitHub Actions

**Priorización**:
- Enfoque en MVP funcional por cada plugin
- Desarrollo secuencial de plugins según roadmap
- Pruebas de usuarios clave al final de cada release

### 3.7 Arquitectura general

El sistema Lazarus se construirá sobre una arquitectura modular con las siguientes características:

**Arquitectura por capas**:
- Capa de presentación (interfaces web, API)
- Capa de aplicación (controladores, servicios)
- Capa de dominio (modelos, lógica de negocio)
- Capa de infraestructura (base de datos, servicios externos)

**Modelo de plugins**:
- Núcleo mínimo indispensable (core)
- Funcionalidades adicionales como plugins independientes
- Cada plugin activable/desactivable a nivel de:
  - Cliente (multi-institución)
  - Institución
  - Sede

**Multi-tenancy**:
- Aislamiento a nivel de base de datos (database-per-tenant)
- Separación lógica de datos por cliente/institución/sede
- Esquema compartido pero datos aislados
- Dominios/subdominios configurables por tenant:
  - Dominio principal para la institución (ej: iecolmerced.edu.co)
  - Subdominios opcionales para cada sede (ej: lacaldera.iecolmerced.edu.co)
  - Dominios personalizados opcionales por sede (ej: sedelacaldera.edu.co)
  - Redirecciones y aliases configurables

**Escalabilidad**:
- Diseño para crecimiento horizontal y vertical
- Optimización para entornos limitados (inicial)
- Preparación para clustering (futuro)

## 4. Modelo de MVP por Plugins

### 4.1 Estrategia de releases

El desarrollo de Lazarus seguirá una estrategia de releases incrementales, donde cada versión entrega un conjunto coherente de funcionalidades empaquetadas como plugins. Esta estrategia permite:

1. Obtener feedback temprano y continuo de los usuarios
2. Distribuir la carga de desarrollo de manera equilibrada
3. Priorizar funcionalidades críticas para generar valor rápidamente
4. Mantener un ritmo sostenible para un solo desarrollador
5. Facilitar la detección y corrección de problemas tempranamente

Cada release está planificado para ofrecer un MVP (Producto Mínimo Viable) con funcionalidad completa aunque limitada en alcance. Esto permite que cada versión sea potencialmente utilizable en entornos reales.

### 4.2 Detalle de plugins por release

A continuación, se detalla cada release con los plugins correspondientes y su objetivo principal:

#### Release 0.1 - Fundamentos del sistema
**Plugins**: core, installer, rbac-extended
**Objetivo**: Establecer la infraestructura multi-tenant, el sistema de roles y el proceso de instalación.
**Funcionalidades clave**:
- Estructura multi-tenant con aislamiento de datos
- Wizard de instalación web y CLI
- Sistema extensible de roles y permisos
- Panel de administración base
- Gestión de usuarios y perfiles básicos

#### Release 0.2 - Gestión académica básica
**Plugins**: academico-basic, boletines
**Objetivo**: Implementar la gestión de calificaciones y generación de boletines preliminares.
**Funcionalidades clave**:
- Configuración de períodos académicos
- Definición de asignaturas y áreas
- Registro de calificaciones
- Generación de boletines preliminares con marca de agua
- Configuración de escala de evaluación

#### Release 0.3 - Comercialización
**Plugins**: license-manager, payments
**Objetivo**: Establecer mecanismos de licenciamiento y pagos.
**Funcionalidades clave**:
- Creación y validación de licencias (JWT y .lzl)
- Integración con WooCommerce para checkout
- Portal de cliente para gestión de licencias
- Marcado de documentos con costo
- Métodos de pago (Stripe, PSE, transferencia)

#### Release 0.4 - Analítica educativa
**Plugins**: analytics
**Objetivo**: Proporcionar estadísticas y alertas para la toma de decisiones.
**Funcionalidades clave**:
- Dashboard con indicadores académicos
- Detección de estudiantes en riesgo
- Tendencias de rendimiento por curso/área
- Alertas automatizadas
- Exportación de reportes estadísticos

#### Release 0.5 - Documentación oficial
**Plugins**: certificaciones
**Objetivo**: Gestionar la emisión de documentos académicos oficiales.
**Funcionalidades clave**:
- Generación de diplomas y actas
- Plantillas personalizables para documentos
- Sistema de sellos y firmas digitales
- Validación de documentos emitidos
- Pagos por documento (opcional)

#### Release 0.6 - Migración de datos
**Plugins**: migrator v1
**Objetivo**: Facilitar la exportación e importación de datos entre instancias.
**Funcionalidades clave**:
- Formato de archivo .lzm para migración
- Exportación/importación de datos
- Transferencia online segura
- Herramientas de CLI para migración
- Cifrado AES-256 para archivos de migración

#### Release 0.7 - Integración con LMS
**Plugins**: lms-bridge (beta)
**Objetivo**: Sincronizar datos con sistemas de gestión de aprendizaje.
**Funcionalidades clave**:
- Sincronización de usuarios con Moodle
- Enrolamiento automático en cursos
- Importación de calificaciones desde Moodle
- Configuración de mapeo de escalas de evaluación
- Progresión de sincronía (batch → tiempo real)

### 4.3 Roadmap visual

```
   2025                                                               2027
   May   Jul   Sep   Nov   Ene   Mar   May   Jul   Sep   Nov   Ene   Mar   May
   |     |     |     |     |     |     |     |     |     |     |     |     |
[0.1]-------->[0.2]-------->[0.3]-------->[0.4]-------->[0.5]-------->[0.6]--->[0.7]
 |            |             |             |             |             |         |
 |            |             |             |             |             |         |
 v            v             v             v             v             v         v
Core          Académico     License       Analytics     Certificac.   Migrator  LMS
Installer     Boletines     Payments                                            Bridge
RBAC
```

## 5. Roles y Permisos

### 5.1 Modelo de roles

El sistema Lazarus implementa un modelo de Control de Acceso Basado en Roles (RBAC) extensible, inspirado en el sistema de Moodle pero ampliado para adaptarse a las necesidades específicas de las instituciones educativas colombianas.

#### Jerarquía de roles predefinidos

1. **Super-Administrador (SA)**
   - Nivel: Sistema
   - Descripción: Control total sobre todas las funcionalidades del sistema y todas las instituciones.
   - Responsabilidades: Gestión de licencias, configuración global, auditoría del sistema.

2. **Admin-Cliente (AC)**
   - Nivel: Cliente (grupo de instituciones)
   - Descripción: Administra todas las instituciones asociadas a un cliente específico.
   - Responsabilidades: Gestión de instituciones, configuraciones a nivel de cliente, reportes consolidados.

3. **Admin-Institución (AI)**
   - Nivel: Institución
   - Descripción: Administra una institución completa incluyendo todas sus sedes.
   - Responsabilidades: Configuración institucional, gestión de sedes, políticas académicas.

4. **Admin-Sede (AS)**
   - Nivel: Sede
   - Descripción: Administra una sede específica de una institución.
   - Responsabilidades: Gestión específica de la sede, configuraciones locales.

5. **Directivo**
   - Nivel: Institución/Sede
   - Descripción: Rol para rectores, coordinadores y otros directivos académicos.
   - Responsabilidades: Supervisión académica, aprobación de procesos, consulta global.

6. **Docente**
   - Nivel: Asignaturas asignadas
   - Descripción: Profesores responsables de asignaturas específicas.
   - Responsabilidades: Registro de calificaciones, observaciones, asistencia.

7. **Secretaría**
   - Nivel: Institución/Sede
   - Descripción: Personal administrativo de secretaría académica.
   - Responsabilidades: Gestión documental, certificados, matrículas.

8. **Finanzas**
   - Nivel: Institución/Sede
   - Descripción: Personal administrativo de área financiera.
   - Responsabilidades: Gestión de pagos, facturación, reportes financieros.

9. **Estudiante**
   - Nivel: Individual
   - Descripción: Alumno matriculado en la institución.
   - Responsabilidades: Consulta de calificaciones, comunicación, solicitudes.

10. **Acudiente**
    - Nivel: Estudiantes asociados
    - Descripción: Padre, madre o tutor legal de estudiantes.
    - Responsabilidades: Seguimiento académico, comunicación, autorizaciones.

11. **Invitado**
    - Nivel: Limitado
    - Descripción: Usuario con acceso restringido a información pública.
    - Responsabilidades: Visualización de información autorizada.

12. **Servicio Alimentación**
    - Nivel: Institución/Sede
    - Descripción: Personal encargado del servicio de alimentación escolar.
    - Responsabilidades: Gestión de menús, control de asistencia, inventario alimentario.

13. **Bibliotecario**
    - Nivel: Institución/Sede
    - Descripción: Personal encargado de biblioteca y recursos educativos.
    - Responsabilidades: Gestión de préstamos, catalogación, inventario bibliográfico.

14. **Psicorientador**
    - Nivel: Institución/Sede
    - Descripción: Personal de apoyo psicológico y orientación.
    - Responsabilidades: Acompañamiento estudiantil, reportes confidenciales.

15. **Enfermería**
    - Nivel: Institución/Sede
    - Descripción: Personal de atención en salud escolar.
    - Responsabilidades: Registro de atenciones, gestión de información médica.

#### Características del sistema RBAC

- **Creación de roles personalizados**: Capacidad de crear roles adicionales específicos para cada institución, con permisos personalizados y adaptados a su estructura organizacional.

- **Roles compuestos**: Capacidad de combinar permisos de múltiples roles en uno nuevo.

- **Herencia de roles**: Estructura jerárquica donde roles superiores heredan permisos de inferiores y roles personalizados pueden heredar de roles predefinidos.

- **Arquetipos de rol**: Plantillas predefinidas para facilitar la creación de nuevos roles frecuentes (ej. Coordinador Académico, Director de Grupo, etc.).

- **Roles contextuales**: Roles que se aplican solo en contextos específicos (ej. Director de Grupo solo para un grupo específico).

- **Restricciones temporales**: Activación/desactivación de roles por periodo (ej. jurado de evaluación solo durante periodo de evaluación).

- **Delegación**: Capacidad de asignar temporalmente permisos específicos a otros usuarios.

- **Auditoría completa**: Registro detallado de asignación, modificación y uso de roles y permisos.

### 5.2 Permisos por rol

Los permisos en Lazarus están organizados en categorías funcionales y pueden ser asignados de forma granular a cada rol. Cada permiso sigue una nomenclatura de `[módulo].[recurso].[acción]`.

#### Categorías de permisos

1. **Sistema** (`system.*`)
   - Configuración global
   - Gestión de plugins
   - Registro de auditoría
   - Monitoreo y mantenimiento

2. **Administración** (`admin.*`)
   - Gestión de usuarios
   - Configuración de roles
   - Parámetros institucionales
   - Calendarios académicos

3. **Académico** (`academic.*`)
   - Planes de estudio
   - Cursos y asignaturas
   - Evaluaciones
   - Reportes académicos

4. **Documental** (`document.*`)
   - Boletines
   - Certificados
   - Constancias
   - Actas y resoluciones

5. **Comunicación** (`communication.*`)
   - Notificaciones
   - Mensajería
   - Anuncios
   - Tablones de comunicados

6. **Financiero** (`financial.*`)
   - Pagos
   - Facturación
   - Reportes financieros
   - Conciliaciones

#### Acciones permitidas

Cada recurso puede tener las siguientes acciones asociadas:

- `view`: Visualizar
- `create`: Crear nuevos registros
- `update`: Modificar registros existentes
- `delete`: Eliminar registros
- `approve`: Aprobar/autorizar
- `export`: Exportar datos
- `import`: Importar datos
- `print`: Imprimir documentos oficiales
- `configure`: Modificar configuraciones
- `assign`: Asignar a otros usuarios

### 5.3 Matriz CRUD×Rol

La siguiente matriz representa los permisos básicos CRUD (Crear, Leer, Actualizar, Eliminar) por cada rol predefinido en el sistema para los recursos principales. Esta matriz se expandirá a nivel de plugin, donde cada plugin deberá definir su propia matriz de permisos.

| Recurso                  | SA | AC | AI | AS | Directivo | Docente | Secretaría | Finanzas | Estudiante | Acudiente | Invitado |
|--------------------------|----|----|----|----|-----------|---------|------------|----------|------------|-----------|----------|
| **Usuarios**             | CRUD | CRUD | CRUD | R | R | - | CR | - | - | - | - |
| **Roles**                | CRUD | R | R | - | - | - | - | - | - | - | - |
| **Instituciones**        | CRUD | CRUD | R | - | R | - | - | - | - | - | - |
| **Sedes**                | CRUD | CRUD | CRUD | R | R | - | R | - | - | - | - |
| **Años académicos**      | CRUD | R | CRUD | R | R | R | R | - | R | R | - |
| **Periodos**             | CRUD | R | CRUD | R | CRUD | R | R | - | R | R | - |
| **Áreas**                | CRUD | R | CRUD | R | CRUD | R | R | - | R | R | - |
| **Asignaturas**          | CRUD | R | CRUD | CRUD | CRUD | R | R | - | R | R | - |
| **Cursos**               | CRUD | R | CRUD | CRUD | CRUD | R | R | - | R | R | - |
| **Estudiantes**          | CRUD | R | CRUD | CRUD | R | R | CRUD | R | R | R | - |
| **Calificaciones**       | CRUD | R | R | R | CR | CRUD | R | - | R | R | - |
| **Boletines**            | CRUD | R | CR | R | CR | R | CRUD | - | R | R | - |
| **Certificados**         | CRUD | R | CR | R | CR | - | CRUD | R | R | R | - |
| **Observador**           | CRUD | R | CR | CR | CRUD | CR | R | - | R | R | - |
| **Licencias**            | CRUD | R | - | - | - | - | - | - | - | - | - |
| **Plugins**              | CRUD | R | - | - | - | - | - | - | - | - | - |
| **Pagos**                | CRUD | R | R | R | - | - | R | CRUD | R | R | - |
| **Config. sistema**      | CRUD | - | - | - | - | - | - | - | - | - | - |
| **Config. institución**  | CRUD | CRUD | CRUD | - | R | - | - | - | - | - | - |
| **Config. sede**         | CRUD | CRUD | CRUD | CRUD | R | - | - | - | - | - | - |
| **Reportes analíticos**  | CRUD | CRUD | CRUD | R | CRUD | R | R | R | - | - | - |
| **Impersonación**        | CRUD | CR | CR | CR | - | - | - | - | - | - | - |

Leyenda:
- C: Crear
- R: Leer
- U: Actualizar
- D: Eliminar
- -: Sin acceso

#### Notas importantes sobre permisos:

1. **Contexto de permiso**: Los permisos están limitados al ámbito del rol:
   - AC: Solo sobre sus instituciones asignadas
   - AI: Solo sobre su institución y sedes
   - AS: Solo sobre su sede específica
   - Docente: Solo sobre sus estudiantes/cursos asignados
   - Acudiente: Solo sobre estudiantes asociados como acudiente

2. **Restricciones adicionales**:
   - Secretaría puede crear usuarios estudiantes y actualizar datos básicos, pero no modificar roles
   - Directivos pueden aprobar/revisar calificaciones pero no registrarlas directamente
   - Docentes solo pueden modificar calificaciones dentro del periodo activo y para sus asignaturas
   - La gestión de usuarios está limitada por nivel jerárquico (ningún rol puede gestionar usuarios de nivel superior)

3. **Herencia de permisos**:
   - Los permisos se acumulan de arriba hacia abajo en la jerarquía de roles
   - Roles superiores heredan todos los permisos de roles inferiores
   - Cuando un rol tiene múltiples padres, hereda todos los permisos de cada uno

#### Permisos para funcionalidad "Ingresar como" (Impersonación)

La funcionalidad de impersonación ("Ingresar como") permite a ciertos roles administrativos asumir temporalmente la identidad de otros usuarios para propósitos de soporte, diagnóstico o capacitación. Esta funcionalidad está estrictamente controlada y auditada:

| Rol | Capacidad de impersonación |
|-----|----------------------------|
| SA (Super-Administrador) | Puede impersonar cualquier usuario en el sistema |
| AC (Admin-Cliente) | Puede impersonar usuarios dentro de sus instituciones asignadas |
| AI (Admin-Institución) | Puede impersonar usuarios dentro de su institución |
| AS (Admin-Sede) | Puede impersonar usuarios dentro de su sede específica |
| Otros roles | No tienen capacidad de impersonación |

**Controles de seguridad para impersonación**:

1. **Transición directa**: Cambio inmediato al usuario seleccionado sin autenticación adicional
2. **Registro detallado**: Toda sesión de impersonación queda registrada en logs de auditoría inmutables
3. **Banners de notificación**: La interfaz muestra claramente que se está en modo de impersonación
4. **Duración flexible**: El usuario puede mantener la sesión de impersonación mientras sea necesario
5. **Retorno sencillo**: Botón "Volver a mi usuario" que restaura la sesión original sin reautenticación
6. **Límites jerárquicos**: Ningún rol puede impersonar a usuarios de nivel superior en la jerarquía
7. **Límites funcionales**: Durante la impersonación hay restricciones en acciones críticas configurables por institución
8. **JWT anidado**: Implementación técnica mediante tokens JWT anidados que preservan la identidad original

### 5.4 Gestión y exportación de roles

Lazarus incluirá un sistema avanzado para la gestión y exportación de configuraciones de roles:

#### Funcionalidades de gestión

- **Clonación de roles**: Capacidad de duplicar roles existentes para crear variantes
- **Roles compuestos**: Combinación de múltiples roles en uno
- **Auditoría de cambios**: Registro de modificaciones a roles y permisos
- **Vista comparativa**: Herramienta visual para comparar permisos entre roles
- **Simulación**: Capacidad de "ver como" otro rol para verificar permisos

#### Exportación e importación

- **Formato JSON**: Estructura estandarizada para representar roles y permisos
- **Exportación selectiva**: Capacidad de exportar roles individuales o conjuntos
- **Importación con validación**: Verificación de integridad y conflictos
- **Plantillas predefinidas**: Conjuntos de roles optimizados para diferentes tipos de instituciones

#### Ejemplo de estructura JSON para roles

```json
{
  "role": {
    "name": "Coordinador Académico",
    "slug": "coordinador-academico",
    "description": "Rol para coordinadores académicos con permisos de supervisión",
    "parent_role": "directivo",
    "level": "sede",
    "scope_restriction": true
  },
  "permissions": [
    {
      "name": "academic.courses.view",
      "granted": true,
      "conditions": null
    },
    {
      "name": "academic.courses.update",
      "granted": true,
      "conditions": {
        "sede_id": "@current_sede_id"
      }
    },
    {
      "name": "academic.grades.approve",
      "granted": true,
      "conditions": {
        "sede_id": "@current_sede_id"
      }
    }
    // Más permisos...
  ],
  "metadata": {
    "created_at": "2025-01-15T10:30:00Z",
    "updated_at": "2025-02-20T14:15:00Z",
    "created_by": "user@example.com",
    "version": "1.2"
  }
}
```

## 6. Plugins y Requisitos Funcionales

### 6.1 Core

El plugin Core constituye el núcleo fundamental del sistema Lazarus, proporcionando la infraestructura base sobre la cual se construyen todos los demás plugins. Implementa las funcionalidades esenciales que no pueden ser desactivadas y que son requeridas por cualquier otro componente del sistema.

#### Requisitos Funcionales

| ID     | Requisito | Descripción | Prioridad |
|--------|-----------|-------------|-----------|
| CORE-1 | Multi-tenancy base | Implementación de arquitectura multi-tenant usando stancl/tenancy con aislamiento a nivel de base de datos. | Alta |
| CORE-2 | Gestión básica de usuarios | Sistema CRUD para usuarios con información básica, estado, y credenciales. | Alta |
| CORE-3 | Sistema base de roles | Implementación de roles básicos (sin extensiones del plugin rbac-extended). | Alta |
| CORE-4 | Dashboard principal | Panel administrativo con widgets configurables según rol de usuario. | Media |
| CORE-5 | Sistema de notificaciones | Infraestructura para notificaciones en aplicación, email y SMS. | Media |
| CORE-6 | Gestión de instituciones | CRUD para instituciones educativas con información básica y configuraciones. | Alta |
| CORE-7 | Gestión de sedes | CRUD para sedes asociadas a instituciones con geolocalización y contacto. | Alta |
| CORE-8 | Logs y auditoría | Registro inmutable de acciones críticas con usuario, fecha y cambios. | Alta |
| CORE-9 | Sistema de configuración | Infraestructura para gestionar parámetros de configuración por nivel (sistema, institución, sede). | Media |
| CORE-10 | API básica | Endpoints REST para funcionalidades core con autenticación OAuth2/JWT. | Baja |
| CORE-11 | Internacionalización | Soporte para múltiples idiomas (español, inglés, portugués). | Baja |
| CORE-12 | Plugin manager | Sistema para gestionar la activación/desactivación de plugins y sus dependencias. | Alta |

#### Restricciones y Consideraciones Técnicas

- El core debe mantener un tamaño mínimo, delegando funcionalidades específicas a plugins
- Debe implementar interfaces y contratos claros para la extensión por plugins
- La base de datos core debe minimizar dependencias circulares
- Debe funcionar correctamente en modo degradado si plugins fallan
- Incorporará mecanismos de cache distribuido para optimizar rendimiento multi-tenant

### 6.2 Installer

El plugin Installer proporciona las herramientas necesarias para la instalación y configuración inicial del sistema Lazarus, tanto en modo web como a través de CLI, asegurando una experiencia fluida para administradores.

#### Requisitos Funcionales

| ID     | Requisito | Descripción | Prioridad |
|--------|-----------|-------------|-----------|
| INS-1 | Wizard web de instalación | Interfaz paso a paso para instalación y configuración inicial con verificación de requisitos. | Alta |
| INS-2 | Instalación por CLI | Comando artisan para instalación automatizada con opciones para entornos sin interfaz gráfica. | Alta |
| INS-3 | Verificación de requisitos | Comprobación automática de versiones PHP, extensiones, permisos y configuraciones de servidor. | Alta |
| INS-4 | Creación de bases de datos | Utilidad para crear y configurar bases de datos principales y de tenants automáticamente. | Alta |
| INS-5 | Carga de datos iniciales | Importación de catálogos base (departamentos, ciudades, tipos de documentos, etc.). | Media |
| INS-6 | Configuración de correo | Asistente para configurar y probar la funcionalidad de correo electrónico. | Media |
| INS-7 | Creación de usuario admin | Formulario para establecer las credenciales del primer usuario super-administrador. | Alta |
| INS-8 | Configuración de entorno | Generación automática de archivo .env con valores optimizados según el entorno. | Alta |
| INS-9 | Validación post-instalación | Verificación de integridad del sistema tras la instalación y reporte de estado. | Media |
| INS-10 | Actualizador | Herramienta para actualizar el sistema a nuevas versiones preservando datos y configuraciones. | Media |

#### Restricciones y Consideraciones Técnicas

- El instalador debe funcionar en entornos con recursos limitados
- Debe proporcionar mensajes de error claros y orientados a soluciones
- La instalación debe ser reanudable en caso de interrupciones
- Debe implementar medidas de seguridad para prevenir instalaciones no autorizadas
- Debe verificar y sugerir optimizaciones para el servidor web y base de datos

### 6.3 RBAC-Extended

El plugin RBAC-Extended expande las capacidades básicas de roles y permisos del core, proporcionando un sistema avanzado de Control de Acceso Basado en Roles con alta granularidad y flexibilidad para adaptarse a diferentes estructuras organizacionales educativas.

#### Requisitos Funcionales

| ID     | Requisito | Descripción | Prioridad |
|--------|-----------|-------------|-----------|
| RB-1 | Arquetipos de roles | Sistema de plantillas para facilitar la creación de roles típicos en instituciones educativas. | Alta |
| RB-2 | Importación/exportación de roles | Funcionalidad para exportar e importar configuraciones de roles en formato JSON. | Alta |
| RB-3 | UI granular de permisos | Interfaz para asignación detallada de permisos con agrupación visual y búsqueda. | Alta |
| RB-4 | Herencia de roles | Implementación de jerarquía de roles donde los superiores heredan permisos de los inferiores. | Alta |
| RB-5 | Roles compuestos | Capacidad para crear roles que combinan permisos de múltiples roles existentes. | Media |
| RB-6 | Restricción por ámbito | Limitación de permisos según ámbito (sistema, cliente, institución, sede). | Alta |
| RB-7 | Permisos condicionales | Asignación de permisos basados en condiciones dinámicas (ej. solo para cursos asignados). | Media |
| RB-8 | Delegación temporal | Capacidad para asignar temporalmente permisos específicos a otros usuarios. | Baja |
| RB-9 | Auditoría de cambios | Registro detallado de modificaciones a roles y permisos con capacidad de revertir. | Media |
| RB-10 | Simulación de roles | Herramienta "ver como" para probar navegación y accesos desde la perspectiva de otro rol. | Baja |

#### Restricciones y Consideraciones Técnicas

- Debe integrarse sin conflictos con el sistema básico de roles del core
- La interfaz de gestión debe balancear usabilidad con potencia para administradores
- Debe optimizar consultas de permisos para minimizar impacto en rendimiento
- Debe implementar caché de permisos con invalidación selectiva
- Debe mantener compatibilidad hacia atrás con cada actualización

### 6.4 Académico-Basic

El plugin Académico-Basic implementa las funcionalidades fundamentales para la gestión académica en instituciones educativas, cubriendo la configuración de períodos, cursos, asignaturas y el registro de calificaciones.

#### Requisitos Funcionales

| ID     | Requisito | Descripción | Prioridad |
|--------|-----------|-------------|-----------|
| AB-1 | Configuración de año académico | Definición de años escolares con fechas de inicio/fin y estado. | Alta |
| AB-2 | Gestión de períodos | Configuración de períodos académicos con porcentajes, fechas y estados. | Alta |
| AB-3 | Áreas y asignaturas | CRUD para áreas académicas y sus asignaturas asociadas con intensidad horaria. | Alta |
| AB-4 | Cursos y grupos | Gestión de cursos (grados) y sus grupos con director de grupo y cupo máximo. | Alta |
| AB-5 | Matrícula de estudiantes | Proceso de asignación de estudiantes a cursos con control de cupos y estados. | Alta |
| AB-6 | Asignación académica | Asignación de docentes a asignaturas por curso/grupo con horario. | Alta |
| AB-7 | Escala de evaluación | Configuración flexible de escalas de calificación con rangos y desempeños. | Alta |
| AB-8 | Registro de calificaciones | Interfaz para docentes para ingresar calificaciones por período y asignatura. | Alta |
| AB-9 | Logros e indicadores | Gestión de logros e indicadores de evaluación por asignatura y período. | Media |
| AB-10 | Promedios y cálculos | Cálculo automático de promedios por período, área y final según configuración. | Alta |
| AB-11 | Asistencia básica | Registro de asistencia con justificaciones y reportes básicos. | Media |
| AB-12 | Importación/exportación | Herramientas para importar/exportar datos académicos en formatos estándar. | Media |

#### Restricciones y Consideraciones Técnicas

- Debe soportar múltiples modelos de evaluación (numérica, conceptual, mixta)
- Debe implementar bloqueo optimista para evitar conflictos en edición concurrente
- Las calificaciones deben tener un historial de cambios auditable
- Debe optimizarse para operación con conectividad intermitente (modo offline)
- Debe implementar validaciones de datos según normativa educativa colombiana

### 6.5 Boletines

El plugin Boletines proporciona la funcionalidad para generar, visualizar y gestionar los boletines académicos de los estudiantes, cumpliendo con los requisitos normativos y proporcionando características de seguridad y personalización.

#### Requisitos Funcionales

| ID     | Requisito | Descripción | Prioridad |
|--------|-----------|-------------|-----------|
| BO-1 | Boletines preliminares | Generación de boletines con marca de agua para revisión sin posibilidad de descarga. | Alta |
| BO-2 | Boletines oficiales | Emisión de boletines oficiales para acudientes con controles de seguridad. | Alta |
| BO-3 | Plantillas personalizables | Editor de plantillas para boletines con variables dinámicas y formato institucional. | Alta |
| BO-4 | Control de acceso por edad | Restricción de acceso a boletines según edad del estudiante (menores/mayores). | Alta |
| BO-5 | Registro de auditoría | Registro detallado de generación, visualización y descarga de boletines. | Alta |
| BO-6 | Firma digital básica | Incorporación de firmas digitalizadas de directivos en documentos oficiales. | Media |
| BO-7 | Observaciones por estudiante | Inclusión de observaciones generales y específicas en boletines. | Alta |
| BO-8 | Estadísticas comparativas | Inclusión de gráficos y datos comparativos de rendimiento. | Media |
| BO-9 | Visualización por periodo | Opción de generar boletines por periodo individual o consolidados. | Alta |
| BO-10 | Exportación masiva | Generación batch de boletines para múltiples estudiantes/cursos. | Media |
| BO-11 | Notificaciones | Alertas a acudientes sobre disponibilidad de boletines. | Baja |
| BO-12 | QR de verificación | Código QR para verificación de autenticidad del documento. | Media |

#### Restricciones y Consideraciones Técnicas

- Los boletines generados deben cumplir con los requisitos del Decreto 1290
- Debe optimizarse el rendimiento para generación masiva de documentos
- Debe implementar protección contra manipulación de documentos
- Debe mantener compatibilidad con diversos navegadores y dispositivos
- Debe implementar mecanismos para controlar el tamaño de los archivos generados

### 6.6 License-Manager

El plugin License-Manager implementa el sistema de licenciamiento de Lazarus, gestionando la creación, validación y administración de licencias tanto en modelo SaaS (JWT) como On-Premise (archivo .lzl).

#### Requisitos Funcionales

| ID     | Requisito | Descripción | Prioridad |
|--------|-----------|-------------|-----------|
| LM-1 | Creación de licencias | Interfaz administrativa para generar licencias JWT y archivos .lzl con parámetros específicos. | Alta |
| LM-2 | Validación de licencias | Sistema de verificación automática de licencias con manejo de errores y notificaciones. | Alta |
| LM-3 | Integración WooCommerce | Webhook para activación automática de licencias tras compras en WooCommerce. | Alta |
| LM-4 | Portal de cliente | Interfaz para que clientes gestionen sus licencias, sedes e instituciones. | Alta |
| LM-5 | Auditoría de licencias | Registro detallado de eventos relacionados con licencias (creación, activación, caducidad). | Alta |
| LM-6 | Licencias por módulos | Soporte para activar/desactivar plugins específicos según licencia adquirida. | Alta |
| LM-7 | Renovación automática | Proceso para renovar licencias automáticamente con notificaciones previas. | Media |
| LM-8 | Límites por licencia | Control de límites de usuarios, estudiantes, instituciones o sedes por licencia. | Alta |
| LM-9 | Transferencia de licencias | Proceso supervisado para transferir licencias entre servidores/instancias. | Media |
| LM-10 | Degradación graciosa | Comportamiento controlado cuando la licencia expira o tiene problemas. | Alta |
| LM-11 | Licencias de prueba | Generación de licencias temporales para demostración o evaluación. | Media |
| LM-12 | Reportes de licencias | Informes sobre estado de licencias, vencimientos y uso. | Media |

#### Restricciones y Consideraciones Técnicas

- Las licencias deben implementar criptografía asimétrica para prevenir falsificaciones
- El sistema debe funcionar sin conexión permanente a internet (verificación periódica)
- Debe manejar adecuadamente relojes de servidor desincronizados
- Debe prevenir manipulación de fechas del sistema para extender licencias
- Todas las operaciones críticas deben ser auditadas de forma inmutable

### 6.7 Payments

El plugin Payments proporciona la infraestructura para gestionar pagos por documentos y servicios dentro del sistema Lazarus, permitiendo a las instituciones monetizar ciertos aspectos de su operación académica.

#### Requisitos Funcionales

| ID     | Requisito | Descripción | Prioridad |
|--------|-----------|-------------|-----------|
| PM-1 | Marcar documentos con costo | Capacidad para establecer costos a certificados y documentos específicos. | Alta |
| PM-2 | Múltiples métodos de pago | Integración con Stripe, PSE, y registro de transferencias bancarias manuales. | Alta |
| PM-3 | Checkout integrado | Flujo de pago dentro del sistema con facturación básica. | Alta |
| PM-4 | Conciliación de pagos | Herramientas para verificar y conciliar pagos realizados. | Alta |
| PM-5 | Historial de transacciones | Registro detallado de todas las transacciones con estados y referencias. | Alta |
| PM-6 | Notificaciones de pago | Alertas automáticas sobre pagos recibidos, pendientes o fallidos. | Media |
| PM-7 | Reportes financieros | Informes sobre ingresos, documentos pagados y pendientes. | Media |
| PM-8 | Gestión de precios | Configuración de precios por tipo de documento y descuentos. | Alta |
| PM-9 | Exenciones de pago | Marcado de usuarios o categorías exentas de ciertos pagos. | Media |
| PM-10 | Verificación de pagos | Sistema para validar que un documento ha sido pagado antes de su emisión. | Alta |
| PM-11 | Recibos y comprobantes | Generación de comprobantes de pago para usuarios. | Alta |
| PM-12 | API de conciliación | Endpoints para integración con sistemas contables externos. | Baja |

#### Restricciones y Consideraciones Técnicas

- Debe cumplir con regulaciones colombianas sobre pagos electrónicos
- Debe implementar seguridad PCI-DSS para manejo de datos de tarjetas
- Debe mantener registro inmutable de transacciones para auditoría
- Debe implementar mecanismos anti-fraude básicos
- Debe optimizarse para minimizar fallos en flujos de pago incompletos

### 6.8 Analytics

El plugin Analytics proporciona herramientas de análisis de datos educativos, permitiendo a las instituciones obtener insights sobre el rendimiento académico, identificar patrones y recibir alertas sobre situaciones que requieren atención.

#### Requisitos Funcionales

| ID     | Requisito | Descripción | Prioridad |
|--------|-----------|-------------|-----------|
| AN-1 | Indicadores académicos | Dashboard con métricas clave de rendimiento académico por curso, área y asignatura. | Alta |
| AN-2 | Detección de riesgo de deserción | Algoritmos para identificar estudiantes en riesgo de deserción según patrones. | Alta |
| AN-3 | Sistema de alertas | Notificaciones automáticas sobre situaciones críticas que requieren atención. | Alta |
| AN-4 | Reportes comparativos | Comparación de rendimiento entre períodos, años, sedes o instituciones. | Media |
| AN-5 | Seguimiento de progreso | Visualización de evolución académica de estudiantes a lo largo del tiempo. | Alta |
| AN-6 | Análisis de asistencia | Patrones de inasistencia y su correlación con rendimiento académico. | Media |
| AN-7 | Exportación de datos | Exportación de reportes y datos analíticos en formatos estándar (XLS, CSV, PDF). | Media |
| AN-8 | Visualizaciones interactivas | Gráficos y visualizaciones dinámicas para exploración de datos. | Media |
| AN-9 | Predicciones básicas | Proyecciones simples de tendencias basadas en datos históricos. | Baja |
| AN-10 | Reportes programados | Generación y envío automático de reportes según calendario configurado. | Baja |
| AN-11 | KPIs institucionales | Indicadores clave de desempeño institucional configurables. | Media |
| AN-12 | Análisis de cohortes | Seguimiento de grupos de estudiantes a lo largo de su trayectoria académica. | Baja |

#### Restricciones y Consideraciones Técnicas

- Debe optimizarse para minimizar impacto en rendimiento del sistema principal
- Debe implementar agregaciones y preprocesamiento para consultas eficientes
- Debe respetar políticas de privacidad y anonimización según configuración
- Debe escalar apropiadamente con grandes volúmenes de datos históricos
- Visualizaciones deben ser responsivas y accesibles en diversos dispositivos

### 6.9 Certificaciones

El plugin Certificaciones gestiona la emisión de documentos oficiales como diplomas, actas y certificados, con controles de seguridad, plantillas personalizables y registro auditable.

#### Requisitos Funcionales

| ID     | Requisito | Descripción | Prioridad |
|--------|-----------|-------------|-----------|
| CR-1 | Generación de diplomas | Creación de diplomas oficiales con datos del estudiante e institución. | Alta |
| CR-2 | Actas de grado | Emisión de actas de grado individuales y consolidadas. | Alta |
| CR-3 | Plantillas personalizables | Editor de plantillas para documentos oficiales con variables dinámicas. | Alta |
| CR-4 | Sellos y firmas | Gestión de sellos institucionales y firmas de directivos para documentos. | Alta |
| CR-5 | Numeración controlada | Sistema de numeración secuencial y controlada para documentos oficiales. | Alta |
| CR-6 | Validación de documentos | Mecanismo para verificar autenticidad de documentos emitidos. | Alta |
| CR-7 | Constancias personalizadas | Generación de constancias configurables según necesidades. | Media |
| CR-8 | Certificados de notas | Emisión de certificados detallados de calificaciones por períodos o años. | Alta |
| CR-9 | Control de re-impresión | Registro y control de reimpresiones con justificación. | Alta |
| CR-10 | Documentos a demanda | Solicitud y emisión de documentos por demanda (con integración de pagos). | Media |
| CR-11 | Libro de registro | Libro digital de registro de documentos oficiales emitidos. | Media |
| CR-12 | Marcas de seguridad | Inclusión de elementos de seguridad en documentos (fondos, microimpresión). | Baja |

#### Restricciones y Consideraciones Técnicas

- Debe cumplir con requisitos legales de documentación educativa colombiana
- Debe implementar seguridad para prevenir falsificaciones de documentos
- Debe optimizarse para impresión de alta calidad
- Debe mantener registro auditable de todos los documentos emitidos
- Debe incluir mecanismos de respaldo para prevenir pérdida de registros

### 6.10 Migrator

El plugin Migrator proporciona herramientas para la migración de datos entre diferentes instancias de Lazarus, facilitando la transición entre modelos SaaS y On-Premise o la consolidación/separación de instituciones.

#### Requisitos Funcionales

| ID     | Requisito | Descripción | Prioridad |
|--------|-----------|-------------|-----------|
| MIG-1 | Formato de archivo .lzm | Definición e implementación del formato de archivo de migración Lazarus. | Alta |
| MIG-2 | Exportación de datos | Herramienta para exportar datos selectivos o completos a archivo .lzm. | Alta |
| MIG-3 | Importación de datos | Sistema para importar datos desde archivo .lzm con validación y resolución de conflictos. | Alta |
| MIG-4 | Transferencia online | Mecanismo para migración directa entre instancias a través de API segura. | Media |
| MIG-5 | Herramientas CLI | Comandos artisan para operaciones de migración automatizadas o programadas. | Alta |
| MIG-6 | Cifrado AES-256 | Implementación de cifrado fuerte para archivos de migración. | Alta |
| MIG-7 | Migración selectiva | Capacidad para seleccionar qué componentes o datos migrar específicamente. | Media |
| MIG-8 | Validación previa | Análisis previo de compatibilidad y potenciales conflictos antes de migración. | Alta |
| MIG-9 | Registro de migraciones | Historial detallado de migraciones realizadas con resultados y errores. | Alta |
| MIG-10 | Rollback de migración | Capacidad para revertir una migración fallida o problemática. | Media |
| MIG-11 | Programación de migraciones | Calendarización de procesos de migración automáticos. | Baja |
| MIG-12 | Reportes de migración | Informes detallados post-migración con estadísticas y resultados. | Media |

#### Restricciones y Consideraciones Técnicas

- Debe manejar adecuadamente grandes volúmenes de datos
- Debe implementar verificación de integridad de datos migrados
- Debe considerar compatibilidad entre diferentes versiones del sistema
- Debe implementar timeouts extensos para operaciones prolongadas
- Debe proveer información detallada de progreso durante migraciones largas

### 6.11 LMS-Bridge

El plugin LMS-Bridge implementa la sincronización bidireccional entre Lazarus y sistemas de gestión de aprendizaje como Moodle, permitiendo mantener alineados usuarios, cursos y calificaciones.

#### Requisitos Funcionales

| ID     | Requisito | Descripción | Prioridad |
|--------|-----------|-------------|-----------|
| LMS-1 | Sincronización de usuarios | Creación y actualización bidireccional de usuarios entre Lazarus y Moodle. | Alta |
| LMS-2 | Sincronización de enrolamientos | Mantenimiento automático de inscripciones a cursos según asignación académica. | Alta |
| LMS-3 | Importación de calificaciones | Transferencia de calificaciones desde Moodle hacia Lazarus. | Alta |
| LMS-4 | Mapeo de cursos | Configuración de correspondencia entre cursos/asignaturas y cursos Moodle. | Alta |
| LMS-5 | Mapeo de escalas | Conversión configurable entre escalas de evaluación de ambos sistemas. | Alta |
| LMS-6 | Sincronización batch | Proceso programado de sincronización completa a intervalos configurados. | Alta |
| LMS-7 | Sincronización en tiempo real | Eventos y webhooks para sincronización inmediata de cambios críticos. | Media |
| LMS-8 | Resolución de conflictos | Herramientas para detectar y resolver discrepancias entre sistemas. | Media |
| LMS-9 | Panel de monitoreo | Interfaz para supervisar estado de sincronización y resolver problemas. | Media |
| LMS-10 | Autenticación SSO | Inicio de sesión único entre Lazarus y Moodle con tokens JWT. | Baja |
| LMS-11 | Logging detallado | Registro granular de operaciones de sincronización para troubleshooting. | Alta |
| LMS-12 | Sincronización selectiva | Capacidad para sincronizar selectivamente elementos específicos. | Media |

#### Restricciones y Consideraciones Técnicas

- Debe ser compatible con Moodle 4.x y versiones posteriores
- Debe manejar adecuadamente interrupciones en la conectividad
- Debe implementar mecanismos de retry con backoff para fallos
- Debe optimizarse para minimizar carga en ambos sistemas
- Debe contemplar diversos escenarios de configuración de Moodle

### 6.12 Observador

El plugin Observador implementa la gestión del registro de seguimiento comportamental y disciplinario de los estudiantes, cumpliendo con los requisitos normativos y garantizando adecuados niveles de confidencialidad según rol.

#### Requisitos Funcionales

| ID     | Requisito | Descripción | Prioridad |
|--------|-----------|-------------|-----------|
| OB-1 | Registro disciplinario | Sistema para documentar situaciones disciplinarias según clasificación normativa. | Alta |
| OB-2 | Vistas según rol | Interfaces diferenciadas según rol con distintos niveles de acceso a información. | Alta |
| OB-3 | Confidencialidad selectiva | Marcado de registros como confidenciales con acceso restringido. | Alta |
| OB-4 | Seguimiento de casos | Trazabilidad de situaciones disciplinarias con estado y resolución. | Alta |
| OB-5 | Clasificación normativa | Categorización según tipos I, II y III del decreto 1965 de 2013. | Alta |
| OB-6 | Descargos y evidencias | Registro de descargos de estudiantes y carga de evidencias relacionadas. | Media |
| OB-7 | Actas de compromiso | Generación de actas de compromiso con seguimiento de cumplimiento. | Media |
| OB-8 | Notificaciones a acudientes | Sistema de alertas a padres/acudientes sobre situaciones disciplinarias. | Media |
| OB-9 | Estadísticas comportamentales | Reportes y análisis de tendencias disciplinarias por estudiante, curso o institución. | Media |
| OB-10 | Observaciones positivas | Registro de reconocimientos y méritos de estudiantes. | Media |
| OB-11 | Comité de convivencia | Gestión de casos para comité con actas y seguimiento. | Media |
| OB-12 | Observaciones periódicas | Evaluaciones comportamentales por período con descriptores configurables. | Alta |

#### Restricciones y Consideraciones Técnicas

- Debe cumplir estrictamente con normativa de manejo de información sensible
- Debe implementar controles de acceso granulares basados en roles
- Debe mantener registro auditable de accesos a información confidencial
- Debe permitir exportación selectiva para reportes a autoridades
- Debe balancear necesidades de registro con protección de menores

## 7. Requisitos No Funcionales

### 7.1 Rendimiento

| ID | Requisito | Descripción | Criterio de aceptación |
|----|-----------|-------------|------------------------|
| RNF-P1 | Tiempo de respuesta | El sistema debe responder en tiempos aceptables incluso bajo carga. | Tiempo de respuesta < 2s para el 95% de las operaciones bajo carga normal. |
| RNF-P2 | Carga concurrente | El sistema debe soportar múltiples usuarios concurrentes. | Soporte mínimo de 100 usuarios concurrentes por instancia con degradación < 30%. |
| RNF-P3 | Tiempo de generación de documentos | Los documentos deben generarse en tiempos razonables. | Boletines individuales < 5s, lotes de hasta 30 boletines < 60s. |
| RNF-P4 | Eficiencia en consultas | Las consultas de base de datos deben optimizarse para rendimiento. | Ninguna consulta individual debe tomar > 1s en completarse. |
| RNF-P5 | Carga inicial de página | El tiempo de carga inicial debe ser optimizado. | Tiempo de carga inicial < 3s con conexión de 1Mbps. |
| RNF-P6 | Consumo de recursos | El uso de recursos del servidor debe ser eficiente. | Uso de RAM < 1GB para instalación básica con 50 usuarios activos. |

### 7.2 Seguridad

| ID | Requisito | Descripción | Criterio de aceptación |
|----|-----------|-------------|------------------------|
| RNF-S1 | Autenticación segura | Implementación de autenticación robusta para todos los usuarios. | Contraseñas hasheadas, políticas de complejidad, protección contra ataques de fuerza bruta. |
| RNF-S2 | Autenticación de dos factores | Soporte para 2FA en roles críticos. | 2FA funcional para roles SA, AC, AI usando TOTP. |
| RNF-S3 | Cifrado de datos sensibles | La información sensible debe almacenarse cifrada. | Campos sensibles cifrados con AES-256-GCM. |
| RNF-S4 | Auditoría de seguridad | Registro de eventos de seguridad inmutable. | Log de auditoría completo para acciones críticas con usuario, fecha, IP. |
| RNF-S5 | Protección contra vulnerabilidades web | Implementación de defensas contra vulnerabilidades OWASP Top 10. | Protección verificada contra XSS, CSRF, SQL Injection, etc. |
| RNF-S6 | Seguridad en comunicaciones | Toda transmisión de datos debe ser segura. | TLS 1.2+ obligatorio, certificados válidos, HSTS. |
| RNF-S7 | Integridad de plugins | Verificación de integridad de plugins instalados. | Verificación de hash SHA-256 para todos los archivos de plugins. |
| RNF-S8 | Protección de sesiones | Manejo seguro de sesiones de usuario. | Regeneración de ID de sesión en login, timeouts configurables, cookie seguras. |

### 7.3 Disponibilidad

| ID | Requisito | Descripción | Criterio de aceptación |
|----|-----------|-------------|------------------------|
| RNF-D1 | Tiempo de actividad | El sistema debe mantener alta disponibilidad. | Uptime > 99.5% en modelo SaaS, documentación para lograr alto uptime en On-Premise. |
| RNF-D2 | Recuperación ante fallos | Capacidad para recuperarse de fallos inesperados. | Tiempo de recuperación < 15 minutos tras fallo no catastrófico. |
| RNF-D3 | Degradación controlada | Comportamiento predecible bajo condiciones no óptimas. | Funcionalidades críticas operativas aún con servicios secundarios caídos. |
| RNF-D4 | Mantenimiento sin interrupción | Capacidad para actualizar con mínima interrupción. | Actualizaciones de plugins sin downtime, actualizaciones core con < 30 minutos de interrupción. |
| RNF-D5 | Backups automáticos | Respaldo periódico de datos críticos. | Backups completos diarios, incrementales cada 6 horas, retención 30 días. |
| RNF-D6 | Monitoreo de salud | Sistema de monitoreo del estado de componentes críticos. | Dashboard de estado con alertas configurables para administradores. |

### 7.4 Escalabilidad

| ID | Requisito | Descripción | Criterio de aceptación |
|----|-----------|-------------|------------------------|
| RNF-E1 | Escalabilidad horizontal | Capacidad para escalar horizontalmente bajo mayor carga. | Documentación para configuración de clustering y balanceo de carga. |
| RNF-E2 | Crecimiento de datos | Manejo eficiente del crecimiento de volumen de datos. | Rendimiento estable con hasta 5 años de datos históricos para institución de 1000 estudiantes. |
| RNF-E3 | Múltiples tenants | Soporte eficiente para múltiples instituciones aisladas. | Rendimiento consistente con hasta 50 tenants activos en una instancia. |
| RNF-E4 | Cache distribuido | Implementación de caché para mejorar rendimiento bajo carga. | Soporte para Redis como sistema de caché distribuido. |
| RNF-E5 | Particionamiento de datos | Estrategia para particionar datos históricos. | Implementación de particionamiento para tablas con alto volumen (calificaciones, logs). |
| RNF-E6 | Escalabilidad de almacenamiento | Manejo eficiente de archivos y documentos generados. | Soporte para almacenamiento distribuido (S3, FTP) para documentos. |

### 7.5 Usabilidad

| ID | Requisito | Descripción | Criterio de aceptación |
|----|-----------|-------------|------------------------|
| RNF-U1 | Interfaz responsiva | Adaptación a diferentes tamaños de pantalla y dispositivos. | Diseño responsivo funcional en pantallas desde 320px hasta 2560px. |
| RNF-U2 | Consistencia visual | Mantener coherencia en elementos visuales y patrones de interacción. | Sistema de diseño documentado con componentes reutilizables. |
| RNF-U3 | Accesibilidad | Cumplimiento de estándares de accesibilidad. | Conformidad WCAG 2.1 nivel AA verificable. |
| RNF-U4 | Rendimiento percibido | Feedback visual durante operaciones prolongadas. | Indicadores de progreso para operaciones > 1s, skeleton loaders. |
| RNF-U5 | Documentación de usuario | Ayuda contextual y documentación accesible. | Sistema de ayuda integrado con búsqueda y guías contextuales. |
| RNF-U6 | Usabilidad móvil | Funcionalidad optimizada para dispositivos móviles. | Interfaces críticas funcionales en smartphones y tablets. |
| RNF-U7 | Multidioma | Soporte para múltiples idiomas en la interfaz. | Soporte completo para español, inglés básico, arquitectura i18n extensible. |
| RNF-U8 | Prevención de errores | Validación proactiva para prevenir errores de usuario. | Validación en tiempo real de formularios, confirmaciones para acciones destructivas. |

### 7.6 Mantenibilidad

| ID | Requisito | Descripción | Criterio de aceptación |
|----|-----------|-------------|------------------------|
| RNF-M1 | Modularidad | Código organizado en módulos con bajo acoplamiento. | Plugins aislados con interfaces bien definidas entre ellos. |
| RNF-M2 | Documentación técnica | Documentación clara del código y arquitectura. | Documentación phpDoc completa, diagramas actualizados, wiki técnica. |
| RNF-M3 | Pruebas automatizadas | Cobertura de pruebas para facilitar mantenimiento. | Cobertura de pruebas > 70% para componentes críticos. |
| RNF-M4 | Gestión de dependencias | Control claro de dependencias entre componentes. | Dependencias explícitas en manifest.json de cada plugin. |
| RNF-M5 | Versionado semántico | Sistema claro de versiones para cada componente. | Adherencia a SemVer 2.0 para core y plugins. |
| RNF-M6 | Logs de desarrollo | Registro detallado para depuración y resolución de problemas. | Sistema de logs multinivel con rotación y búsqueda. |
| RNF-M7 | Entorno de desarrollo | Configuración estandarizada para desarrollo. | Docker compose para entorno de desarrollo completo. |
| RNF-M8 | Backwards compatibility | Compatibilidad con versiones anteriores. | Documentación de breaking changes, migraciones automáticas. |

### 7.7 Portabilidad

| ID | Requisito | Descripción | Criterio de aceptación |
|----|-----------|-------------|------------------------|
| RNF-P1 | Independencia de plataforma | Funcionamiento en diversos sistemas operativos. | Compatibilidad verificada con Linux (Ubuntu, CentOS), Windows Server. |
| RNF-P2 | Configuración flexible | Adaptabilidad a diferentes entornos de hosting. | Configuración vía .env, soporte para variables de entorno. |
| RNF-P3 | Independencia de base de datos | Soporte para diferentes motores de base de datos. | Compatibilidad principal con MariaDB, soporte limitado para MySQL 8+. |
| RNF-P4 | Instalación simplificada | Proceso de instalación claro y reproducible. | Instalación completable en < 30 minutos por administrador con conocimientos básicos. |
| RNF-P5 | Requisitos mínimos | Funcionamiento en hardware modesto. | Operación básica en servidor con 2GB RAM, 2 CPU, 20GB almacenamiento. |
| RNF-P6 | Exportación de datos | Capacidad para exportar datos en formatos estándar. | Exportación a CSV, JSON para todos los datos principales. |
| RNF-P7 | Compatibilidad con contenedores | Soporte para despliegue containerizado. | Dockerfile y configuración Docker Compose funcionales. |
| RNF-P8 | Migración entre entornos | Facilidad para migrar entre distintos entornos. | Herramientas documentadas para migración desarrollo-producción. |

### 7.8 Compatibilidad

| ID | Requisito | Descripción | Criterio de aceptación |
|----|-----------|-------------|------------------------|
| RNF-C1 | Compatibilidad con navegadores | Funcionamiento en navegadores modernos. | Soporte para últimas 2 versiones de Chrome, Firefox, Safari, Edge. |
| RNF-C2 | APIs estandarizadas | Diseño de APIs según estándares establecidos. | Adherencia a REST o GraphQL con documentación OpenAPI/Swagger. |
| RNF-C3 | Formatos estándar | Uso de formatos ampliamente compatibles. | PDF/A para documentos oficiales, estándares abiertos para datos. |
| RNF-C4 | Interoperabilidad | Capacidad para integrarse con sistemas externos. | APIs documentadas, webhooks configurables, formatos de intercambio estándar. |
| RNF-C5 | Compatibilidad hacia atrás | Soporte para versiones anteriores de API o plugins. | Compatibilidad mínima de 2 versiones anteriores de APIs públicas. |
| RNF-C6 | Versionado de API | Gestión clara de versiones de API. | Versionado explícito en URLs de API (v1, v2) con deprecation notices. |
| RNF-C7 | Compatibilidad con impresión | Optimización para impresión de documentos. | Hojas de estilo específicas para impresión, pruebas con impresoras comunes. |
| RNF-C8 | Compatibilidad offline | Funcionamiento básico con conectividad limitada. | Modo offline para operaciones críticas con sincronización posterior. |

## 8. Normativa y Cumplimiento

### 8.1 Marco legal colombiano

El sistema Lazarus debe cumplir con la siguiente normativa colombiana relacionada con educación, gestión documental y tecnología:

#### Normativa educativa

| Norma | Descripción | Implicaciones para Lazarus |
|-------|-------------|----------------------------|
| **Ley 115 de 1994** | Ley General de Educación | Estructura del sistema educativo, autonomía escolar, gobierno escolar, evaluación. |
| **Decreto 1860 de 1994** | Reglamentación pedagógica y organizativa | PEI, evaluación y promoción, gobierno escolar, orientaciones curriculares. |
| **Decreto 1290 de 2009** | Evaluación del aprendizaje y promoción | Escalas de valoración, informes académicos, estructura de boletines. |
| **Decreto 1075 de 2015** | Decreto Único del Sector Educación | Compilación de normas educativas, requerimientos institucionales. |
| **Resolución 6404 de 2009** | Libro de registro de diplomas | Registro digital de diplomas y documentos de graduación. |
| **Decreto 1965 de 2013** | Convivencia escolar | Clasificación de situaciones disciplinarias, debido proceso, observador. |
| **Resolución 7550 de 1994** | Obligatoriedad de certificados | Requisitos para emisión y validez de certificados académicos. |

#### Gestión documental y protección de datos

| Norma | Descripción | Implicaciones para Lazarus |
|-------|-------------|----------------------------|
| **Ley 594 de 2000** | Ley General de Archivos | Gestión documental, conservación, acceso y seguridad de documentos. |
| **Ley 1581 de 2012** | Protección de Datos Personales | Tratamiento de datos de menores, autorización, finalidad, seguridad. |
| **Decreto 1377 de 2013** | Reglamentación de Protección de Datos | Políticas de tratamiento, transferencia, autorización. |
| **Ley 527 de 1999** | Comercio electrónico y firmas digitales | Validez de documentos electrónicos, requisitos para firmas. |
| **Decreto 2364 de 2012** | Firma electrónica | Implementación de firmas electrónicas en documentos académicos. |

#### Control y seguridad

| Norma | Descripción | Implicaciones para Lazarus |
|-------|-------------|----------------------------|
| **Ley 87 de 1993** | Control Interno | Procedimientos de control, auditoría, responsabilidad. |
| **ISO/IEC 27001** | Seguridad de la Información | Gestión de riesgos, confidencialidad, integridad, disponibilidad. |
| **Ley 1712 de 2014** | Transparencia y Acceso a Información | Publicación de información institucional, datos abiertos. |
| **CONPES 3854** | Seguridad Digital | Gestión de riesgos digitales, ciberseguridad. |

### 8.2 Protección de datos personales

El sistema debe implementar medidas específicas para el cumplimiento de la normativa de protección de datos personales, especialmente considerando el tratamiento de datos de menores de edad:

#### Requisitos de cumplimiento

| ID | Requisito | Descripción | Implementación |
|----|-----------|-------------|----------------|
| PD-1 | Política de tratamiento | Documentación clara sobre tratamiento de datos | Sección específica en el sistema con aceptación documentada |
| PD-2 | Autorización expresa | Obtención y registro de autorizaciones | Workflows de autorización con registro de IP, fecha y usuario |
| PD-3 | Protección reforzada | Medidas especiales para datos de menores | Cifrado adicional, restricciones de acceso, auditoría detallada |
| PD-4 | Finalidad específica | Documentación de propósitos de datos | Metadatos sobre finalidad de cada campo sensible |
| PD-5 | Derechos ARCO | Procedimientos para ejercer derechos | Interfaces para acceso, rectificación, cancelación, oposición |
| PD-6 | Datos sensibles | Tratamiento especial para información sensible | Clasificación de datos por sensibilidad con controles específicos |
| PD-7 | Transferencia segura | Controles para transferencia de datos | Cifrado en tránsito, logs de transferencias |
| PD-8 | Retención limitada | Políticas de retención y borrado | Configuración de períodos de retención con borrado automático |

#### Categorización de datos

El sistema categorizará los datos personales según su sensibilidad:

1. **Datos de identificación básica**
   - Nombres, apellidos, documento de identidad
   - Fecha de nacimiento, género
   - Información de contacto institucional

2. **Datos académicos**
   - Calificaciones y desempeño académico
   - Asistencia y participación
   - Reconocimientos y sanciones académicas

3. **Datos sensibles**
   - Información médica y psicológica
   - Situación familiar y socioeconómica
   - Registros disciplinarios detallados
   - Necesidades educativas especiales

4. **Datos biométricos**
   - Fotografías de identificación
   - Firmas digitalizadas
   - Cualquier otro dato biométrico

Cada categoría tendrá controles de acceso, cifrado y auditoría específicos según su nivel de sensibilidad.

### 8.3 Integraciones oficiales

Lazarus implementará compatibilidad con sistemas oficiales colombianos mediante plugins dedicados para importación y exportación de datos en los formatos requeridos, ya que estos sistemas no disponen de APIs públicas para integración directa.

#### SIMAT (Sistema Integrado de Matrícula)

El Sistema Integrado de Matrícula del Ministerio de Educación Nacional no ofrece API pública, por lo que la integración se basará en la generación e importación de archivos planos según las especificaciones oficiales.

| ID | Funcionalidad | Descripción |
|----|--------------|-------------|
| SI-1 | Exportación SIMAT | Generación de archivos planos con formato SIMAT (.txt, .csv) para carga manual en la plataforma oficial |
| SI-2 | Importación SIMAT | Capacidad para importar y procesar archivos descargados desde SIMAT con datos de estudiantes (ANEXOS 6A, 6B) |
| SI-3 | Validación de datos | Verificación previa de formatos, tipos de datos y reglas de negocio según especificaciones SIMAT vigentes |
| SI-4 | Reportes de inconsistencias | Identificación automática de discrepancias entre datos locales y datos SIMAT importados |
| SI-5 | Seguimiento de NUIP/ID | Identificación y mantenimiento de correspondencia entre IDs locales y códigos SIMAT para estudiantes |

#### ICFES-PRISMA

El sistema PRISMA del ICFES se maneja a través de plataforma web sin API pública para integración. Lazarus implementará herramientas para facilitar la interacción con este sistema.

| ID | Funcionalidad | Descripción |
|----|--------------|-------------|
| IC-1 | Generación archivos pre-registro | Preparación de archivos Excel/CSV con formato específico para carga masiva en PRISMA |
| IC-2 | Importación de resultados | Procesamiento de archivos PDF/Excel de resultados descargados desde plataforma ICFES |
| IC-3 | Análisis estadístico | Herramientas para procesar, visualizar y comparar resultados históricos con promedios nacionales |
| IC-4 | Asistente de pre-registro | Interfaz guiada para preparar la información requerida por el formulario web de PRISMA |
| IC-5 | Repositorio de resultados | Almacenamiento estructurado y búsqueda avanzada de resultados históricos por estudiante/cohorte |

#### SENA (Articulación Media Técnica)

La interacción con sistemas SENA se realiza principalmente a través de formularios web y documentación física. Lazarus proporcionará soporte para estos procesos.

| ID | Funcionalidad | Descripción |
|----|--------------|-------------|
| SE-1 | Registro de programas articulados | Gestión de información sobre programas de formación SENA articulados con la institución |
| SE-2 | Gestión de convenios | Seguimiento de fechas, documentación y requisitos para convenios SENA |
| SE-3 | Reportes para instructores | Generación de planillas y formatos requeridos por instructores SENA (.xls, .docx) |
| SE-4 | Seguimiento de certificaciones | Registro y control de certificaciones SENA obtenidas por estudiantes |
| SE-5 | Estadísticas de articulación | Reportes sobre participación, deserción y resultados de estudiantes en programas SENA |

#### MEN-Data / SNIES

Las plataformas del Ministerio de Educación para reportes estadísticos no cuentan con APIs públicas. La integración se enfocará en la preparación de datos para carga manual.

| ID | Funcionalidad | Descripción |
|----|--------------|-------------|
| MD-1 | Preparación DANE-C600 | Formularios y reportes para preparar la información solicitada en el Formulario C600 del DANE |
| MD-2 | Generación de anexos MEN | Archivos con estructura específica para anexos requeridos en reportes al Ministerio |
| MD-3 | Validación previa | Verificación automática de datos según reglas de validación oficiales antes de generar reportes |
| MD-4 | Histórico de reportes | Almacenamiento y consulta de reportes históricos enviados a entidades oficiales |
| MD-5 | Indicadores institucionales | Cálculo de indicadores clave de desempeño según metodologías oficiales (eficiencia interna, etc.) |

**Nota importante**: Debido a que estos sistemas gubernamentales no proporcionan APIs públicas documentadas para integración directa, Lazarus se enfocará en facilitar los procesos de importación/exportación manual y preparación de datos según los formatos requeridos por cada plataforma. Esto incluirá validaciones previas, transformación de datos, y plantillas predefinidas que cumplan con las especificaciones vigentes de cada entidad.

### 8.4 Estándares técnicos obligatorios

El sistema debe cumplir con los siguientes estándares técnicos para garantizar seguridad, accesibilidad y calidad:

#### Accesibilidad

- **WCAG 2.1 nivel AA**: Conformidad verificable con directrices de accesibilidad web
- Implementación de:
  - Texto alternativo para imágenes
  - Estructura semántica de contenido
  - Contrastes adecuados
  - Navegación por teclado
  - Responsive design para diversos dispositivos
  - Compatibilidad con lectores de pantalla

#### Seguridad de comunicaciones

- **TLS 1.2 o superior**: Obligatorio para todas las comunicaciones
- **HSTS**: Implementación de HTTP Strict Transport Security
- **CSP**: Políticas de seguridad de contenido para prevenir XSS
- **Cookies seguras**: Atributos HttpOnly, Secure, SameSite
- **Encabezados de seguridad**: X-Content-Type-Options, X-Frame-Options

#### Cifrado de datos

- **AES-256-GCM**: Para datos sensibles en reposo
- **Algoritmos de hash**: Argon2id para contraseñas de usuario
- **Claves asimétricas**: RSA-2048 o superior para firmas digitales
- **Cifrado de documentos**: PDF/A con firma electrónica avanzada

#### Autenticación

- **Multi-factor**: 2FA para roles críticos (SA, AC, AI)
- **Políticas de contraseñas**: Complejidad mínima, rotación, bloqueo
- **Single Sign-On**: Opcional para integración con sistemas institucionales
- **JWT**: Para API y comunicaciones entre componentes

#### Auditoría

- **Registros inmutables**: Logs con hash encadenado para eventos críticos
- **Sellado de tiempo**: Timestamping confiable para operaciones críticas
- **Trazabilidad**: Registro completo de cadena de modificaciones

#### Interoperabilidad

- **REST/GraphQL**: APIs documentadas según estándares OpenAPI 3.0
- **Formatos abiertos**: CSV, JSON, XML para intercambio de datos
- **Metadatos estándar**: Dublin Core para documentos institucionales

## 9. Diseño Técnico

### 9.1 Arquitectura del sistema

#### Visión general de la arquitectura

La arquitectura de Lazarus sigue un enfoque modular basado en plugins, construido sobre el framework Laravel 10 LTS. El sistema está diseñado como una aplicación multi-tenant que provee aislamiento de datos a nivel de base de datos para cada institución educativa.

```
+-----------------------------------------------------+
|                  ARQUITECTURA LAZARUS                |
+-----------------------------------------------------+
|                                                     |
|  +-------------------+      +-------------------+   |
|  |    APLICACIÓN     |      |    APLICACIÓN     |   |
|  | (TENANT 1)        |      | (TENANT 2)        |   |
|  +-------------------+      +-------------------+   |
|  | • Controllers     |      | • Controllers     |   |
|  | • Models          |      | • Models          |   |
|  | • Views           |      | • Views           |   |
|  | • Config          |      | • Config          |   |
|  +-------------------+      +-------------------+   |
|            |                        |               |
+------------|------------------------|--------------+
             |                        |                
+------------|------------------------|--------------+
|            v                        v               |
|  +-------------------+      +-------------------+   |
|  |   BASE DE DATOS   |      |   BASE DE DATOS   |   |
|  | (TENANT 1)        |      | (TENANT 2)        |   |
|  +-------------------+      +-------------------+   |
|                                                     |
+-----------------------------------------------------+
|                                                     |
|  +-------------------------------------------+      |
|  |                NÚCLEO (CORE)              |      |
|  +-------------------------------------------+      |
|  | • Multi-Tenancy                           |      |
|  | • Auth & RBAC                             |      |
|  | • Plugin System                           |      |
|  | • Institution Management                  |      |
|  +-------------------------------------------+      |
|                                                     |
|  +-------------------------------------------+      |
|  |             CAPA DE PLUGINS               |      |
|  +-------------------------------------------+      |
|  | +---------+ +---------+ +---------+       |      |
|  | |Academic | |Bulletins| |Licenses |  ...  |      |
|  | +---------+ +---------+ +---------+       |      |
|  +-------------------------------------------+      |
|                                                     |
|  +-------------------------------------------+      |
|  |          SERVICIOS COMPARTIDOS            |      |
|  +-------------------------------------------+      |
|  | • Notificaciones   • Caché                |      |
|  | • Colas            • Almacenamiento       |      |
|  +-------------------------------------------+      |
|                                                     |
+-----------------------------------------------------+
```

#### Componentes principales

1. **Núcleo (Core)**
   - Motor de multi-tenancy (stancl/tenancy)
   - Sistema base de autenticación y autorización
   - Infraestructura para plugins (nWidart/modules)
   - Gestión básica de usuarios e instituciones
   - Sistema de configuración y eventos

2. **Capa de Plugins**
   - Módulos independientes activables/desactivables
   - Interfaces estandarizadas para interoperabilidad
   - Aislamiento de dependencias
   - Extensión de modelos y controladores base

3. **Sistema de Tenancy**
   - Segregación a nivel de base de datos por institución
   - Middleware automático de identificación de tenant
   - Resolución dinámica de bases de datos
   - Migraciones específicas por tenant

4. **Capa de Presentación**
   - Interfaces Livewire/Inertia.js
   - Diseño responsivo con TailwindCSS
   - Componentes reutilizables
   - Plantillas por tenant

5. **Servicios Compartidos**
   - Sistema de notificaciones
   - Cola de trabajos (Laravel Horizon)
   - Caché distribuido (Redis)
   - Almacenamiento de archivos

6. **APIs y Integración**
   - API REST/GraphQL
   - Webhooks para eventos
   - Conectores para sistemas externos
   - SSO e impersonalización

#### Flujos principales

**Flujo de autenticación y autorización**

```
┌────────────┐     ┌───────────────┐     ┌───────────────┐     ┌────────────┐
│  Solicitud │────▶│  Identificar  │────▶│  Verificar    │────▶│  Verificar │
│  de acceso │     │  Tenant       │     │  Autenticación│     │  Permisos  │
└────────────┘     └───────────────┘     └───────────────┘     └────────────┘
                                                                      │
┌────────────┐     ┌───────────────┐     ┌───────────────┐           │
│  Respuesta │◀────│  Controlador/ │◀────│  Middleware   │◀──────────┘
│            │     │  Acción       │     │  RBAC         │
└────────────┘     └───────────────┘     └───────────────┘
```

**Flujo de instalación (InstallerServiceProvider)**

```
┌────────────┐     ┌───────────────┐     ┌───────────────┐     ┌────────────┐
│  Inicio de │────▶│  Verificar    │────▶│  Configurar   │────▶│  Crear     │
│  instalación│     │  Requisitos   │     │  Base Datos   │     │  Esquemas  │
└────────────┘     └───────────────┘     └───────────────┘     └────────────┘
                                                                      │
┌────────────┐     ┌───────────────┐     ┌───────────────┐           │
│  Completar │◀────│  Crear        │◀────│  Cargar Datos │◀──────────┘
│  Setup     │     │  Admin        │     │  Iniciales    │
└────────────┘     └───────────────┘     └───────────────┘
```

**Flujo de licenciamiento**

```
┌────────────┐     ┌───────────────┐     ┌───────────────┐     ┌────────────┐
│  Solicitud │────▶│  Verificar    │────▶│  Validar      │────▶│  Activar   │
│  activación│     │  Firma/Hash   │     │  Parámetros   │     │  Funciones │
└────────────┘     └───────────────┘     └───────────────┘     └────────────┘
                                                                      │
┌────────────┐     ┌───────────────┐                                  │
│  Operación │◀────│  Registro     │◀─────────────────────────────────┘
│  Sistema   │     │  Licencia     │
└────────────┘     └───────────────┘
```

#### Consideraciones arquitectónicas

- **Desacoplamiento**: Interfaces claras entre componentes para minimizar dependencias
- **Extensibilidad**: Puntos de extensión definidos para plugins
- **Rendimiento**: Caching estratégico y optimización de consultas
- **Mantenibilidad**: Modularidad y separación de responsabilidades
- **Seguridad**: Capas de protección en cada nivel de la aplicación
- **Escalabilidad**: Diseño preparado para crecimiento horizontal y vertical

### 9.2 Diseño de base de datos

#### Enfoque multi-tenant

Lazarus implementa una estrategia de multi-tenancy basada en el paquete stancl/tenancy con el enfoque "database-per-tenant". Cada institución educativa tiene su propia base de datos, lo que proporciona:

- Completo aislamiento de datos entre instituciones
- Flexibilidad para personalizaciones específicas
- Facilidad para respaldos y migraciones individuales
- Mejor rendimiento en consultas complejas
- Cumplimiento de requisitos regulatorios de separación de datos

**Base de datos central (lazarus_core)**

Contiene información global no específica de tenants:
- Registro de tenants (instituciones)
- Usuarios globales (administradores)
- Licencias
- Configuración del sistema
- Registro de plugins 
- Logs globales

**Bases de datos por tenant (lazarus_tenant_[id])**

Cada tenant tiene su propia base de datos con:
- Usuarios específicos del tenant
- Datos académicos
- Configuraciones personalizadas
- Documentos y registros
- Logs específicos

#### Modelo Entidad-Relación principal

El siguiente diagrama muestra las entidades principales del sistema (simplificado):

```
+---------------+       +----------------+       +---------------+
|     USERS     |       | INSTITUTIONS   |       |   BRANCHES    |
+---------------+       +----------------+       +---------------+
| id            |<----->| id             |<----->| id            |
| name          |       | name           |       | name          |
| email         |       | short_name     |       | code          |
| password      |       | code           |       | address       |
| status        |       | dane_code      |       | institution_id|
| ...           |       | tenant_id      |       | ...           |
+---------------+       +----------------+       +---------------+
      ^                        ^                        ^
      |                        |                        |
+---------------+       +----------------+       +---------------+
|     ROLES     |       | ACADEMIC_YEARS |       |    COURSES    |
+---------------+       +----------------+       +---------------+
| id            |       | id             |       | id            |
| name          |       | name           |       | name          |
| slug          |       | start_date     |       | grade         |
| ...           |       | end_date       |       | branch_id     |
+---------------+       | institution_id |       | academic_year_|
      ^                 +----------------+       +---------------+
      |                        ^                        ^
+---------------+               |                        |
| PERMISSIONS   |       +----------------+       +---------------+
+---------------+       |    PERIODS     |       |    GROUPS     |
| id            |       +----------------+       +---------------+
| name          |       | id             |       | id            |
| slug          |       | name           |       | name          |
| ...           |       | start_date     |       | course_id     |
+---------------+       | end_date       |       | capacity      |
                        | academic_year_ |       | director_id   |
                        +----------------+       +---------------+
                               ^                        ^
                               |                        |
                        +----------------+       +---------------+
                        |    SUBJECTS    |       |   STUDENTS    |
                        +----------------+       +---------------+
                        | id             |       | id            |
                        | name           |       | name          |
                        | code           |       | document      |
                        | area_id        |       | ...           |
                        | ...            |       | group_id      |
                        +----------------+       +---------------+
                               ^                        ^
                               |                        |
                        +----------------+       +---------------+
                        |     GRADES     |<------| GUARDIANS     |
                        +----------------+       +---------------+
                        | id             |       | id            |
                        | value          |       | name          |
                        | student_id     |       | document      |
                        | subject_id     |       | ...           |
                        | period_id      |       +---------------+
                        | ...            |
                        +----------------+
```

**Entidades Core**

- **users**: Usuarios del sistema con autenticación
- **roles**: Roles del sistema RBAC
- **permissions**: Permisos individuales
- **role_permissions**: Tabla pivote entre roles y permisos
- **institutions**: Instituciones educativas
- **branches**: Sedes de las instituciones
- **configurations**: Configuraciones por nivel (sistema/institución/sede)
- **plugins**: Registro de plugins disponibles e instalados
- **licenses**: Licencias del sistema
- **audit_logs**: Registro de auditoría

**Entidades Académicas**

- **academic_years**: Años académicos
- **periods**: Períodos dentro de un año académico
- **areas**: Áreas académicas
- **subjects**: Asignaturas dentro de áreas
- **courses**: Cursos (grados)
- **groups**: Grupos dentro de los cursos
- **students**: Estudiantes matriculados
- **teachers**: Docentes
- **enrollments**: Matrículas que vinculan estudiantes a cursos/grupos
- **assignments**: Asignaciones académicas (docente-asignatura-grupo)
- **guardians**: Acudientes
- **student_guardians**: Relación entre estudiantes y acudientes
- **grades**: Calificaciones
- **attendance**: Registro de asistencia
- **achievements**: Logros por asignatura
- **observations**: Observaciones del estudiante

#### Diccionario de datos (extracto)

**Tabla: users**

| Campo | Tipo | Descripción | Restricciones |
|-------|------|-------------|---------------|
| id | uuid | Identificador único | PK |
| name | varchar(100) | Nombre completo | NOT NULL |
| email | varchar(191) | Correo electrónico | UNIQUE, NOT NULL |
| username | varchar(50) | Nombre de usuario | UNIQUE, NOT NULL |
| password | varchar(191) | Contraseña hash | NOT NULL |
| document_type | varchar(20) | Tipo de documento | NOT NULL |
| document_number | varchar(30) | Número de documento | NOT NULL |
| phone | varchar(20) | Teléfono | NULL |
| address | varchar(191) | Dirección | NULL |
| status | enum | Estado (activo, inactivo, bloqueado) | NOT NULL, DEFAULT 'activo' |
| email_verified_at | timestamp | Verificación de email | NULL |
| profile_photo_path | varchar(2048) | Ruta de foto de perfil | NULL |
| created_at | timestamp | Fecha de creación | NOT NULL |
| updated_at | timestamp | Fecha de actualización | NOT NULL |

**Tabla: institutions**

| Campo | Tipo | Descripción | Restricciones |
|-------|------|-------------|---------------|
| id | uuid | Identificador único | PK |
| name | varchar(191) | Nombre de la institución | NOT NULL |
| short_name | varchar(50) | Nombre corto | NOT NULL |
| code | varchar(20) | Código institucional | UNIQUE, NOT NULL |
| dane_code | varchar(20) | Código DANE | UNIQUE, NOT NULL |
| nit | varchar(20) | NIT | UNIQUE, NOT NULL |
| address | varchar(191) | Dirección principal | NOT NULL |
| phone | varchar(20) | Teléfono principal | NOT NULL |
| email | varchar(191) | Email institucional | NOT NULL |
| website | varchar(191) | Sitio web | NULL |
| legal_representative | varchar(100) | Nombre representante legal | NOT NULL |
| logo_path | varchar(2048) | Ruta del logo | NULL |
| status | enum | Estado | NOT NULL, DEFAULT 'activo' |
| tenant_id | varchar(50) | ID de tenant | UNIQUE, NOT NULL |
| created_at | timestamp | Fecha de creación | NOT NULL |
| updated_at | timestamp | Fecha de actualización | NOT NULL |

#### Índices y optimizaciones

**Índices principales**

- Índices de búsqueda para documentos, nombres y códigos
- Índices compuestos para relaciones frecuentes
- Índices para columnas de ordenamiento común

**Estrategia de particionamiento**

- Particionamiento por rango para tablas históricas (calificaciones, asistencia)
- Particionamiento por lista para datos categorizados (por año académico)

**Optimizaciones**

- Campos JSON para estructuras flexibles (configuraciones, metadatos)
- Desnormalización estratégica para reportes frecuentes
- Tablas de resumen para estadísticas (materialized views)

#### Estrategia de respaldo

- Respaldos incrementales diarios por tenant
- Respaldos completos semanales
- Cifrado AES-256 de backups
- Retención configurable (por defecto 30 días)
- Scripts de verificación y prueba de restauración

### 9.3 Diseño de APIs

Lazarus implementa un conjunto completo de APIs para permitir la integración con sistemas externos y la comunicación entre componentes.

#### Arquitectura de API REST

La arquitectura de API REST de Lazarus sigue un enfoque modular donde cada plugin puede extender la API principal:

**Principios de diseño**:
- Cada plugin puede registrar sus propios endpoints y funcionalidades
- Sistema de middleware para autenticación, autorización y validación
- Versionado explícito para garantizar compatibilidad a largo plazo
- Documentación automática vía OpenAPI/Swagger
- Soporte para diversos formatos de respuesta (JSON, XML)
- Capacidades HATEOAS para navegabilidad

**Estructura básica de URLs**:
```
/api/v1/[plugin]/[recurso]
```

Por ejemplo:
- `/api/v1/academic/courses` - Listar cursos
- `/api/v1/certificates/generate` - Generar certificado
- `/api/v1/users/import` - Importar usuarios

#### Endpoints principales por plugin

*Core*
- `/api/v1/auth/*`: Autenticación y gestión de tokens
- `/api/v1/users/*`: CRUD de usuarios
- `/api/v1/institutions/*`: CRUD de instituciones
- `/api/v1/branches/*`: CRUD de sedes

*Académico*
- `/api/v1/academic/years/*`: Años académicos
- `/api/v1/academic/periods/*`: Períodos
- `/api/v1/academic/courses/*`: Cursos y grupos
- `/api/v1/academic/subjects/*`: Asignaturas
- `/api/v1/academic/enrollments/*`: Matrículas

*Boletines*
- `/api/v1/reports/bulletins/*`: Generación y consulta de boletines

*Certificaciones*
- `/api/v1/certificates/*`: Gestión de certificados

*LMS Connector*
- `/api/v1/lms/courses/*`: Sincronización de cursos LMS
- `/api/v1/lms/grades/*`: Sincronización de calificaciones
- `/api/v1/lms/users/*`: Sincronización de usuarios

*Resources Manager*
- `/api/v1/resources/*`: Gestión de recursos físicos
- `/api/v1/resources/maintenance/*`: Mantenimientos programados

*Meal Service*
- `/api/v1/meals/menus/*`: Gestión de menús
- `/api/v1/meals/attendance/*`: Asistencia al servicio de alimentación

*User Management*
- `/api/v1/users/batch/*`: Operaciones masivas de usuarios
- `/api/v1/users/profiles/*`: Gestión de perfiles extendidos

#### Extensibilidad por plugins

Cada plugin puede registrar sus propios endpoints de API mediante un sistema de registro centralizado:

```php
// Ejemplo de registro de endpoints desde un plugin
public function registerApiRoutes()
{
    ApiManager::registerEndpoint('v1', 'academic', [
        'path' => 'courses',
        'controller' => CoursesApiController::class,
        'methods' => ['GET', 'POST', 'PUT', 'DELETE'],
        'middleware' => ['api.auth', 'tenant.context']
    ]);
}
```

#### GraphQL API

Como alternativa a REST, se implementará una API GraphQL para consultas complejas y reducción de roundtrips:

```graphql
type Query {
  student(id: ID!): Student
  students(courseId: ID, filters: StudentFilters): [Student!]!
  grades(studentId: ID!, periodId: ID): [Grade!]!
}

type Student {
  id: ID!
  name: String!
  document: String!
  course: Course
  grades: [Grade!]
  guardians: [Guardian!]
}

type Grade {
  id: ID!
  value: Float!
  subject: Subject!
  period: Period!
  teacher: User
  createdAt: DateTime!
}
```

#### Webhooks

Mecanismo para notificar eventos a sistemas externos:

1. **WooCommerce Integration**
   - Endpoint: `/api/webhooks/woocommerce`
   - Eventos: nueva compra, actualización de licencia
   - Seguridad: Firma HMAC-SHA256

2. **LMS Integration**
   - Endpoint: `/api/webhooks/lms`
   - Eventos: nuevas calificaciones, cursos, enrolamientos
   - Soporte bidireccional

3. **Notificaciones Generales**
   - Endpoint: `/api/webhooks/events`
   - Eventos configurables: nuevas calificaciones, documentos generados, etc.
   - Filtros por tipo de evento y contexto

#### Seguridad de API

- Autenticación OAuth2/JWT
- Rate limiting por cliente y endpoint
- Validación estricta de inputs
- Sanitización de datos de salida
- Logs detallados de uso de API
- CORS configurado restrictivamente
- Tokens con alcance (scope) limitado

#### Documentación y SDKs

- Documentación automática generada con Swagger/OpenAPI
- Consola de pruebas interactiva
- SDKs generados para PHP y JavaScript
- Ejemplos de uso para integraciones comunes

### 9.4 Diseño de seguridad

#### Modelo de seguridad en capas

Lazarus implementa un enfoque de defensa en profundidad con múltiples capas de protección:

1. **Seguridad de infraestructura**
   - Recomendaciones de firewall
   - Configuración de servidor web (headers, TLS)
   - Directivas de seguridad de contenido (CSP)

2. **Seguridad de aplicación**
   - Autenticación robusta
   - Control de acceso basado en roles (RBAC)
   - Protección contra vulnerabilidades OWASP Top 10

3. **Seguridad de datos**
   - Cifrado de datos sensibles
   - Aislamiento multi-tenant
   - Respaldos cifrados

4. **Auditoría y monitoreo**
   - Logs inmutables
   - Alerta de eventos sospechosos
   - Trazabilidad de acciones

#### Autenticación avanzada

**Multi-Factor Authentication (MFA)**

- Opcional para todos los usuarios, configurable a nivel de institución
- Recomendada (pero no obligatoria) para roles críticos (SA, AC, AI)
- Implementación TOTP (Google Authenticator, Microsoft Authenticator, etc.)
- Opciones de recuperación vía correo electrónico o códigos de respaldo
- Registro de dispositivos confiables para reducir solicitudes repetidas

**JWT anidado para impersonación**

```
┌─────────────────────────────────────────┐
│ JWT Externo (Admin)                     │
│ ┌─────────────────────────────────────┐ │
│ │ JWT Interno (Usuario Impersonado)   │ │
│ │                                     │ │
│ └─────────────────────────────────────┘ │
└─────────────────────────────────────────┘
```

- Tokens con firma anidada para preservar identidad original
- Token exterior contiene la identidad del administrador autenticado
- Token interior contiene la identidad del usuario impersonado
- Todos los accesos y acciones registrados con ambas identidades
- Limitación temporal configurable (predeterminado: 60 minutos)
- Restricción de ciertas acciones sensibles durante impersonación
- Indicador visual claro en la interfaz de usuario

**Flujo de impersonación simplificado**:

1. Administrador ya autenticado en el sistema navega a la sección de usuarios
2. Selecciona un usuario y elige "Ingresar como" desde el menú de acciones
3. El sistema genera un JWT anidado sin requerir autenticación adicional
4. Se muestra banner persistente indicando el modo de impersonación
5. El sistema registra todas las acciones con ambas identidades
6. El usuario puede volver a su sesión original en cualquier momento a través del botón "Volver a mi usuario" en el banner de impersonación
7. La transición entre usuario impersonado y original es inmediata, sin necesidad de reautenticación
8. Todas las sesiones de impersonación son auditadas y reportadas

#### Protección contra ataques

**Rate Limiting**

- Por dirección IP, usuario y acción
- Escalada progresiva de tiempos de espera
- Whitelisting para APIs legítimas
- Notificación de intentos sospechosos

**Escaneo de Plugins**

- Verificación de hash SHA-256 para todos los archivos
- Análisis estático básico de código
- Detección de modificaciones no autorizadas
- Bloqueo de plugins comprometidos

**Prevención de inyección y XSS**

- Preparación de consultas SQL
- Escape de salida en capas de presentación
- Validación estricta de entrada
- Encabezados de seguridad (X-XSS-Protection, etc.)

#### Cifrado y protección de datos

**Estrategia de cifrado**

- Datos en reposo: AES-256-GCM
- Comunicaciones: TLS 1.2+
- Contraseñas: Argon2id
- Archivos sensibles: Cifrado a nivel de aplicación

**Gestión de claves**

- Rotación programada de claves
- Almacenamiento seguro de claves
- Separación de claves por tenant
- Protección de claves maestras

### 9.5 Diseño de escalabilidad

#### Arquitectura escalable

Lazarus está diseñado para escalar tanto vertical como horizontalmente:

**Escalabilidad vertical**
- Optimización de consultas SQL
- Índices estratégicos
- Caching eficiente
- Uso eficiente de memoria

**Escalabilidad horizontal**
- Stateless para servidores web
- Sesiones en Redis
- Balanceo de carga
- Sharding por tenant

#### Base de datos escalable

**MariaDB Galera**

```
+---------------+     +---------------+     +---------------+
|    MariaDB    |<--->|    MariaDB    |<--->|    MariaDB    |
|    Nodo 1     |     |    Nodo 2     |     |    Nodo 3     |
+---------------+     +---------------+     +---------------+
        ^                     ^                     ^
        |                     |                     |
        +---------------------+---------------------+
                              |
                     +----------------+
                     |    ProxySQL    |
                     +----------------+
                              ^
                              |
                     +----------------+
                     |   Aplicación   |
                     +----------------+
```

- Cluster replicado para alta disponibilidad
- ProxySQL para enrutamiento inteligente
- Read/write splitting configurable

**Estrategia de sharding**

- Sharding natural por tenant (database-per-tenant)
- Distribución de tenants entre clusters
- Metadata centralizada

#### Cache distribuido

**Redis tags por tenant**

- Prefijo de cache por tenant
- Invalidación selectiva
- Almacenamiento de sesiones
- Colas de trabajos

**Estrategia de caching**

- Cache de queries frecuentes
- Cache de configuración
- Cache de permisos y roles
- Cache de renderización parcial

#### Balanceo de carga

- Sticky sessions para mejor uso de cache
- Health checks para eliminación de nodos problemáticos
- Distribución geográfica para entornos multi-región
- Estrategias de degradación graciosa

### 9.6 Diagramas UML

#### Diagrama de Clases Core

```
+---------------------+        +----------------------+
|        User         |        |        Role          |
+---------------------+        +----------------------+
| - id: uuid          |        | - id: uuid           |
| - name: string      |<>------| - name: string       |
| - email: string     |        | - slug: string       |
| - password: string  |        | - description: string|
| - status: enum      |        | - level: enum        |
+---------------------+        +----------------------+
         |                              |
         |                              |
         v                              v
+---------------------+        +----------------------+
|     UserProfile     |        |      Permission      |
+---------------------+        +----------------------+
| - user_id: uuid     |        | - id: uuid           |
| - address: string   |        | - name: string       |
| - phone: string     |        | - slug: string       |
| - photo: string     |        | - description: string|
+---------------------+        +----------------------+
                                         ^
+---------------------+                  |
|     Institution     |                  |
+---------------------+        +----------------------+
| - id: uuid          |        |    RolePermission    |
| - name: string      |        +----------------------+
| - code: string      |        | - role_id: uuid      |
| - dane_code: string |        | - permission_id: uuid|
| - nit: string       |        | - conditions: json   |
| - tenant_id: string |        +----------------------+
+---------------------+
         |
         |
         v
+---------------------+        +----------------------+
|       Branch        |        |      TenantDomain    |
+---------------------+        +----------------------+
| - id: uuid          |<>------| - id: uuid           |
| - name: string      |        | - domain: string     |
| - code: string      |        | - branch_id: uuid    |
| - address: string   |        | - is_primary: boolean|
| - institution_id    |        | - status: enum       |
+---------------------+        +----------------------+
         |
         |
         v
+---------------------+        +----------------------+
|    Configuration    |        |        Plugin        |
+---------------------+        +----------------------+
| - id: uuid          |        | - id: uuid           |
| - key: string       |        | - name: string       |
| - value: json       |        | - code: string       |
| - level: enum       |        | - version: string    |
| - level_id: uuid    |        | - status: enum       |
+---------------------+        | - manifest: json     |
                               +----------------------+
                                         |
                                         |
                                         v
+---------------------+        +----------------------+
|      License        |        |    PluginInstance    |
+---------------------+        +----------------------+
| - id: uuid          |        | - id: uuid           |
| - key: string       |        | - plugin_id: uuid    |
| - type: enum        |<>------| - tenant_id: uuid    |
| - status: enum      |        | - status: enum       |
| - expires_at: date  |        | - settings: json     |
| - metadata: json    |        +----------------------+
+---------------------+
```

El diagrama de clases muestra las principales entidades del núcleo del sistema y sus relaciones:

1. **Gestión de usuarios y permisos**:
   - La clase `User` representa a los usuarios del sistema
   - Cada usuario puede tener múltiples roles (`Role`)
   - Los roles agrupan permisos (`Permission`) específicos
   - La tabla pivote `RolePermission` permite condiciones dinámicas para permisos

2. **Estructura organizacional**:
   - `Institution` representa a las instituciones educativas
   - Cada institución puede tener múltiples sedes (`Branch`)
   - Cada institución tiene un tenant_id único para multi-tenancy

3. **Dominios por sede (nuevo)**:
   - La clase `TenantDomain` permite asignar dominios/subdominios por sede
   - Cada sede puede tener múltiples dominios, con uno marcado como primario
   - Esta configuración es opcional y permite URLs personalizadas por sede

4. **Configuración y plugins**:
   - `Configuration` almacena ajustes a diferentes niveles (sistema/institución/sede)
   - `Plugin` representa los plugins disponibles en el sistema
   - `PluginInstance` es la instancia de un plugin activado para un tenant
   - `License` controla los permisos de uso y acceso a funcionalidades

#### Diagrama de Secuencia: Registro de Calificaciones

```
+--------+      +----------+      +----------+      +-----------+      +------------+      +-------------+
| Docente|      |Interfaz  |      |Controller|      |Grade      |      |Notification|      |Cache        |
|        |      |          |      |          |      |Service    |      |Service     |      |Service      |
+--------+      +----------+      +----------+      +-----------+      +------------+      +-------------+
    |                |                |                  |                  |                   |
    | 1. Accede form |                |                  |                  |                   |
    |--------------->|                |                  |                  |                   |
    |                |                |                  |                  |                   |
    |                | 2. Carga datos |                  |                  |                   |
    |                |--------------->|                  |                  |                   |
    |                |                |                  |                  |                   |
    |                |                | 3. Obtiene       |                  |                   |
    |                |                | estudiantes      |                  |                   |
    |                |                | y logros         |                  |                   |
    |                |                |----------------->|                  |                   |
    |                |                |                  |                  |                   |
    |                |                |                  | 4. Retorna datos |                   |
    |                |                |<-----------------|                  |                   |
    |                |                |                  |                  |                   |
    |                | 5. Muestra     |                  |                  |                   |
    |                | formulario     |                  |                  |                   |
    |                |<---------------|                  |                  |                   |
    |                |                |                  |                  |                   |
    | 6. Ingresa     |                |                  |                  |                   |
    | calificaciones |                |                  |                  |                   |
    |--------------->|                |                  |                  |                   |
    |                |                |                  |                  |                   |
    |                | 7. Envía datos |                  |                  |                   |
    |                |--------------->|                  |                  |                   |
    |                |                |                  |                  |                   |
    |                |                | 8. Valida datos  |                  |                   |
    |                |                |----------------+ |                  |                   |
    |                |                | (reglas de     | |                  |                   |
    |                |                | evaluación)    | |                  |                   |
    |                |                |<---------------+ |                  |                   |
    |                |                |                  |                  |                   |
    |                |                | 9. Guarda       |                  |                   |
    |                |                | calificaciones  |                  |                   |
    |                |                |----------------->|                  |                   |
    |                |                |                  |                  |                   |
    |                |                |                  | 10. Registra en  |                   |
    |                |                |                  | histórico        |                   |
    |                |                |                  |--------+         |                   |
    |                |                |                  |        |         |                   |
    |                |                |                  |<-------+         |                   |
    |                |                |                  |                  |                   |
    |                |                |                  | 11. Notifica     |                   |
    |                |                |                  | evento           |                   |
    |                |                |                  |----------------->|                   |
    |                |                |                  |                  |                   |
    |                |                |                  | 12. Invalida     |                   |
    |                |                |                  | caché            |                   |
    |                |                |                  |--------------------------------->|   |
    |                |                |                  |                  |                   |
    |                |                |                  | 13. Respuesta    |                   |
    |                |                |<-----------------|                  |                   |
    |                |                |                  |                  |                   |
    |                | 14. Confirma   |                  |                  |                   |
    |                | guardado       |                  |                  |                   |
    |                |<---------------|                  |                  |                   |
    |                |                |                  |                  |                   |
    | 15. Visualiza  |                |                  |                  |                   |
    | confirmación   |                |                  |                  |                   |
    |<---------------|                |                  |                  |                   |
    |                |                |                  |                  | 16. Envía         |
    |                |                |                  |                  | notificaciones    |
    |                |                |                  |                  | (async)           |
    |                |                |                  |                  |-----------+       |
    |                |                |                  |                  |           |       |
    |                |                |                  |                  |<----------+       |
    |                |                |                  |                  |                   |
+--------+      +----------+      +----------+      +-----------+      +------------+      +-------------+
```

Este diagrama de secuencia ilustra el flujo completo del proceso de registro de calificaciones:

1. El docente accede al formulario de registro de calificaciones
2. La interfaz solicita los datos necesarios al controlador
3-4. El controlador obtiene los estudiantes y logros del GradeService
5. Se muestra el formulario con la matriz de calificaciones
6-7. El docente ingresa las calificaciones y envía el formulario
8. El controlador valida los datos según reglas de evaluación configuradas
9. Se envían las calificaciones validadas al GradeService para su almacenamiento
10. Se registra en el histórico de calificaciones (para auditoría)
11. Se notifica el evento de nuevas calificaciones
12. Se invalida la caché relacionada (boletines, promedios)
13-15. Se confirma el guardado exitoso al docente
16. El sistema envía notificaciones asíncronas (a acudientes, coordinadores, etc.)

#### Diagrama de Componentes: Sistema Completo

```
+----------------------------------------------+
|                SISTEMA LAZARUS                |
+----------------------------------------------+
|                                              |
|  +------------------+   +------------------+ |
|  |      CORE        |<->|   RBAC-EXTENDED  | |
|  +------------------+   +------------------+ |
|  | • Multi-tenancy  |   | • Roles avanzados| |
|  | • Auth básica    |   | • Permisos cond. | |
|  | • Usuarios       |   | • Exportación    | |
|  +------------------+   +------------------+ |
|           ^                      ^           |
|           |                      |           |
|  +------------------+   +------------------+ |
|  |    ACADÉMICO     |<->|     BOLETINES    | |
|  +------------------+   +------------------+ |
|  | • Año académico  |   | • Plantillas     | |
|  | • Calificaciones |   | • Generación     | |
|  | • Asistencia     |   | • Publicación    | |
|  +------------------+   +------------------+ |
|           ^                      ^           |
|           |                      |           |
|  +------------------+   +------------------+ |
|  |    ANALYTICS     |<->| CERTIFICACIONES  | |
|  +------------------+   +------------------+ |
|  | • Dashboards     |   | • Documentos     | |
|  | • Indicadores    |   | • Firmas/sellos  | |
|  | • Alertas        |   | • Validación     | |
|  +------------------+   +------------------+ |
|           ^                      ^           |
|           |                      |           |
|  +------------------+   +------------------+ |
|  | LICENSE-MANAGER  |<->|     PAYMENTS     | |
|  +------------------+   +------------------+ |
|  | • Licencias      |   | • Pasarelas      | |
|  | • Activación     |   | • Facturación    | |
|  | • Portal cliente |   | • Conciliación   | |
|  +------------------+   +------------------+ |
|           ^                      ^           |
|           |                      |           |
|  +------------------+   +------------------+ |
|  |     MIGRATOR     |<->|    LMS-BRIDGE    | |
|  +------------------+   +------------------+ |
|  | • Importación    |   | • Conexión Moodle| |
|  | • Exportación    |   | • Sincronización | |
|  | • Verificación   |   | • Mapeo cursos   | |
|  +------------------+   +------------------+ |
|                                              |
+----------------------------------------------+
```

Este diagrama de componentes muestra la relación entre los diferentes plugins del sistema y sus principales responsabilidades, ilustrando las dependencias y conexiones entre ellos.

#### Diagrama de Despliegue

```
+-----------------------------------------------------+
|                 ENTORNO DE PRODUCCIÓN               |
+-----------------------------------------------------+
|                                                     |
|  +-------------------+      +-------------------+   |
|  |   BALANCEADOR     |      |  CERTIFICADO SSL  |   |
|  |    DE CARGA       |----->|    LET'S ENCRYPT  |   |
|  |    (NGINX)        |      |                   |   |
|  +-------------------+      +-------------------+   |
|            |                                        |
|            v                                        |
|  +-------------------+      +-------------------+   |
|  |   SERVIDOR WEB 1  |      |   SERVIDOR WEB 2  |   |
|  | (APP + NGINX/PHP) |      | (APP + NGINX/PHP) |   |
|  +-------------------+      +-------------------+   |
|            |                        |               |
|            |                        |               |
|            v                        v               |
|  +-------------------------------------------+      |
|  |             REDIS (CACHE/QUEUE)           |      |
|  +-------------------------------------------+      |
|                        |                            |
|                        v                            |
|  +-------------------------------------------+      |
|  |               MARIADB GALERA               |     |
|  | +---------------+ +---------------+        |     |
|  | |     NODO 1    | |    NODO 2     |        |     |
|  | +---------------+ +---------------+        |     |
|  +-------------------------------------------+      |
|                        |                            |
|                        v                            |
|  +-------------------------------------------+      |
|  |            ALMACENAMIENTO NFS              |     |
|  |            (ARCHIVOS COMPARTIDOS)          |     |
|  +-------------------------------------------+      |
|                                                     |
|  +-------------------------------------------+      |
|  |           BACKUP AUTOMATIZADO              |     |
|  |           (RESPALDOS DIARIOS)              |     |
|  +-------------------------------------------+      |
|                                                     |
+-----------------------------------------------------+
```

Este diagrama de despliegue ilustra la infraestructura recomendada para un entorno de producción, mostrando los diferentes componentes y sus relaciones para garantizar alta disponibilidad, rendimiento y seguridad.

#### Diagramas BPMN

**Proceso de Matrícula**

```
INICIO
  |
  v
[Verificar requisitos] --> [¿Completos?] --> NO --> [Solicitar documentos faltantes]
  |                                                     |
  | SÍ                                                  |
  v                                                     |
[Seleccionar curso/grupo] <-------------------------------
  |
  v
[Verificar cupo] --> [¿Hay cupo?] --> NO --> [Colocar en lista de espera]
  |                                             |
  | SÍ                                          |
  v                                             |
[Registrar información estudiante]              |
  |                                             |
  v                                             |
[Registrar información acudiente]               |
  |                                             |
  v                                             |
[Asignar conceptos de pago]                     |
  |                                             |
  v                                             |
[Procesar matrícula] --------------------------->
  |
  v
[Generar documentos (carné, contrato)]
  |
  v
[Notificar matrícula exitosa]
  |
  v
FIN
```

**Proceso de Generación de Boletines**

```
INICIO
  |
  v
[Verificar cierre de período] --> [¿Período cerrado?] --> NO --> [Esperar cierre de período]
  |                                                                 |
  | SÍ                                                              |
  v                                                                 |
[Consolidar calificaciones] <------------------------------------------
  |
  v
[Calcular promedios y estadísticas]
  |
  v
[Generar boletines preliminares]
  |
  v
[Revisión por coordinación] --> [¿Aprobados?] --> NO --> [Corregir inconsistencias]
  |                                                        |
  | SÍ                                                     |
  v                                                        |
[Generar boletines oficiales] <----------------------------
  |
  v
[Publicar para acudientes]
  |
  v
[Notificar disponibilidad]
  |
  v
[Registrar visualizaciones]
  |
  v
FIN
```

**Flujo de Migración entre Entornos**

```
INICIO
  |
  v
[Seleccionar tipo migración] --> [SaaS a On-Premise] --> [Verificar licencia destino]
  |                               |
  |                               v
  |                           [On-Premise a SaaS] --> [Verificar disponibilidad tenant]
  |                                                     |
  v                                                     |
[Respaldar datos origen] <---------------------------------
  |
  v
[Generar archivo .lzm]
  |
  v
[Verificar integridad]
  |
  v
[Transferir archivo] --> [¿Transferencia exitosa?] --> NO --> [Reintentar transferencia]
  |                                                             |
  | SÍ                                                          |
  v                                                             |
[Validar archivo en destino] <--------------------------------->
  |
  v
[Importar datos]
  |
  v
[Verificar consistencia] --> [¿Datos correctos?] --> NO --> [Restaurar respaldo]
  |                                                           |
  | SÍ                                                        |
  v                                                           |
[Activar entorno destino] <---------------------------------->
  |
  v
[Probar funcionalidades clave]
  |
  v
[Notificar migración completada]
  |
  v
FIN
```

## 10. Historias de Usuario

### 10.1 Historias de Core

| ID | Como... | Quiero... | Para... | Criterios de aceptación |
|----|---------|-----------|---------|-------------------------|
| HU-C01 | Super-administrador | Instalar el sistema | Comenzar a configurar la plataforma | - Wizard de instalación funcional<br>- Verificación automática de requisitos<br>- Creación exitosa de BD<br>- Usuario admin creado |
| HU-C02 | Administrador | Crear una nueva institución | Registrar un colegio en el sistema | - Formulario completo<br>- Validación de campos obligatorios<br>- Creación de tenant<br>- Notificación de éxito |
| HU-C03 | Administrador | Configurar roles y permisos | Establecer niveles de acceso adecuados | - Interfaz de asignación de permisos<br>- Vista previa de cambios<br>- Aplicación inmediata<br>- Historial de cambios |
| HU-C04 | Cualquier usuario | Cambiar mi contraseña | Mantener segura mi cuenta | - Verificación de contraseña actual<br>- Requisitos de seguridad validados<br>- Confirmación de nueva contraseña<br>- Notificación de cambio |
| HU-C05 | Administrador-institución | Crear una nueva sede | Expandir mi institución | - Campos para información completa<br>- Mapeo/geolocalización<br>- Configuración independiente<br>- Visualización en dashboard |
| HU-C06 | Administrador | Visualizar el dashboard | Tener una visión general del sistema | - Estadísticas relevantes<br>- Widgets configurables<br>- Datos en tiempo real<br>- Filtros por períodos |
| HU-C07 | Administrador-sede | Configurar parámetros institucionales | Personalizar el sistema | - Agrupación lógica de parámetros<br>- Validación en tiempo real<br>- Guardado automático<br>- Historial de cambios |
| HU-C08 | Administrador | Revisar logs de auditoría | Monitorear actividades críticas | - Filtros por usuario/acción/fecha<br>- Detalles de cada acción<br>- Exportación de registros<br>- Evidencia de integridad |
| HU-C09 | Administrador | Activar/desactivar plugins | Gestionar funcionalidades | - Lista de plugins disponibles<br>- Estado actual visible<br>- Verificación de dependencias<br>- Activación sin errores |
| HU-C10 | Admin-Cliente | Renovar una licencia | Mantener el servicio activo | - Estado actual visible<br>- Opciones de renovación claras<br>- Proceso de pago integrado<br>- Activación inmediata |
| HU-C11 | Super-Administrador | Ingresar como otro usuario | Brindar soporte técnico específico | - Lista filtrable de usuarios<br>- Impersonación directa sin revalidación<br>- Banner claro de impersonación<br>- Registro en logs de auditoría<br>- Opción para volver a mi usuario en cualquier momento |
| HU-C12 | Admin-Institución | Ingresar como docente o estudiante | Diagnosticar problemas específicos | - Selección de usuarios de su institución<br>- Impersonación inmediata<br>- Retorno a usuario original sin cerrar sesión<br>- Visualización clara de estado de impersonación |
| HU-C13 | Admin-Sede | Ingresar como usuario de mi sede | Asistir en tareas o capacitación | - Lista filtrable de usuarios de la sede<br>- Transición suave entre usuarios<br>- Límites contextuales claros<br>- Registro de acciones realizadas |
| HU-C14 | Cualquier usuario | Activar/desactivar MFA | Aumentar la seguridad de mi cuenta | - Proceso guiado de activación<br>- Generación de códigos de respaldo<br>- Opción para desactivar<br>- Recordatorios de seguridad |
| HU-C15 | Administrador | Ver historial de impersonaciones | Auditar el uso de esta funcionalidad | - Registro completo con timestamps<br>- Información de ambos usuarios<br>- Duración de cada sesión<br>- Acciones realizadas durante impersonación |

### 10.2 Historias por plugin

#### Plugin: Académico-Basic

| ID | Como... | Quiero... | Para... | Criterios de aceptación |
|----|---------|-----------|---------|-------------------------|
| HU-AB01 | Administrador | Configurar un nuevo año académico | Preparar el periodo escolar | - Fechas de inicio/fin<br>- Definición de períodos<br>- Estado (activo/inactivo/planeación)<br>- Bloqueo de años previos |
| HU-AB02 | Coordinador | Crear la estructura de cursos | Organizar los grupos académicos | - Creación de grados y grupos<br>- Asignación de directores de grupo<br>- Configuración de cupos<br>- Vista de resumen |
| HU-AB03 | Secretaría | Matricular estudiantes | Asignarlos a sus cursos | - Búsqueda rápida de estudiantes<br>- Verificación de cupos<br>- Proceso masivo disponible<br>- Estado de matrícula visible |
| HU-AB04 | Coordinador | Realizar asignación académica | Definir qué docentes dictan cada asignatura | - Interfaz tipo matriz<br>- Validación de carga horaria<br>- Verificación de conflictos<br>- Confirmación de asignación |
| HU-AB05 | Docente | Registrar calificaciones | Evaluar a mis estudiantes | - Lista de estudiantes por grupo<br>- Calificaciones por logro<br>- Cálculo automático de definitivas<br>- Guardar parcialmente |

#### Plugin: Boletines

| ID | Como... | Quiero... | Para... | Criterios de aceptación |
|----|---------|-----------|---------|-------------------------|
| HU-BO01 | Coordinador | Configurar la plantilla de boletín | Personalizar el formato institucional | - Editor visual WYSIWYG<br>- Variables dinámicas<br>- Vista previa en tiempo real<br>- Guardado de versiones |
| HU-BO02 | Coordinador | Generar boletines preliminares | Revisar antes de publicar | - Marca de agua visible<br>- Generación por curso<br>- Indicación de estado preliminar<br>- Sin posibilidad de descarga |
| HU-BO03 | Secretaría | Publicar boletines oficiales | Permitir su visualización a acudientes | - Confirmación de publicación<br>- Notificación a acudientes<br>- Registro de publicación<br>- Bloqueo tras publicación |
| HU-BO04 | Acudiente | Ver el boletín de mi hijo | Conocer su desempeño académico | - Acceso sólo a estudiantes asociados<br>- Formato oficial<br>- Opción para descargar PDF<br>- Historial de períodos |
| HU-BO05 | Directivo | Obtener estadísticas de desempeño | Analizar resultados académicos | - Gráficos por curso/área<br>- Comparativa entre períodos<br>- Identificación de casos críticos<br>- Exportación de datos |

#### Plugin: License-Manager

| ID | Como... | Quiero... | Para... | Criterios de aceptación |
|----|---------|-----------|---------|-------------------------|
| HU-LM01 | Super-admin | Crear una nueva licencia | Habilitar el sistema para un cliente | - Formulario con parámetros completos<br>- Selección de módulos incluidos<br>- Fechas de vigencia<br>- Generación exitosa de licencia |
| HU-LM02 | Admin-cliente | Activar mi licencia | Comenzar a usar el sistema | - Validación en tiempo real<br>- Confirmación de activación<br>- Visualización de detalles<br>- Habilitación inmediata |
| HU-LM03 | Super-admin | Ver el estado de todas las licencias | Monitorear uso y vencimientos | - Lista filtrable<br>- Indicadores de estado<br>- Alertas de próximos vencimientos<br>- Detalles completos por licencia |
| HU-LM04 | Admin-cliente | Transferir mi licencia | Migrar a otro servidor | - Proceso guiado<br>- Verificación de seguridad<br>- Confirmación en ambos extremos<br>- Registro de transferencia |
| HU-LM05 | Sistema | Verificar validez de licencia | Garantizar uso autorizado | - Verificación periódica<br>- Manejo de errores<br>- Alertas anticipadas<br>- Degradación controlada |

## 11. Plan de Pruebas

### 11.1 Estrategia de pruebas

La estrategia de pruebas para Lazarus contempla múltiples niveles y tipos de pruebas para garantizar la calidad del software:

#### Enfoque de pruebas

1. **Pruebas durante el desarrollo**
   - Pruebas unitarias para componentes críticos
   - TDD para funcionalidades complejas
   - Static analysis con PHPStan

2. **Pruebas por release**
   - Pruebas de integración entre plugins
   - Pruebas funcionales por historia de usuario
   - Pruebas de regresión automatizadas

3. **Pruebas continuas**
   - CI/CD con GitHub Actions
   - Pruebas de seguridad periódicas
   - Monitoreo de rendimiento

#### Entornos de prueba

- **Desarrollo**: Entorno local del desarrollador
- **Integración**: Servidor de pruebas automáticas
- **Staging**: Entorno similar a producción para pruebas finales
- **Producción**: Entorno real con monitoreo

#### Roles y responsabilidades

- **Desarrollador**: Pruebas unitarias, pruebas de integración básicas
- **QA** (externo/opcional): Pruebas funcionales detalladas, pruebas de aceptación
- **Cliente beta**: Pruebas de aceptación de usuario en entornos controlados

### 11.2 Tipos de pruebas

#### Pruebas unitarias

- **Frameworks**: PHPUnit
- **Cobertura objetivo**: >70% para código crítico
- **Enfoque**: Componentes aislados, servicios, modelos

*Ejemplo de caso de prueba unitaria:*

```php
public function test_calculo_promedio_por_periodo()
{
    // Arrange
    $calificaciones = [
        ['valor' => 4.5, 'porcentaje' => 20],
        ['valor' => 3.8, 'porcentaje' => 30],
        ['valor' => 4.2, 'porcentaje' => 50]
    ];
    
    // Act
    $promedio = $this->calculadorService->calcularPromedioPonderado($calificaciones);
    
    // Assert
    $this->assertEquals(4.13, $promedio, 'El promedio ponderado no es correcto');
}
```

#### Pruebas de integración

- **Enfoque**: Interacción entre componentes y plugins
- **Prioridad**: Interfaces críticas entre subsistemas
- **Cobertura**: Flujos de procesos completos

*Ejemplo de caso de prueba de integración:*

```php
public function test_generacion_boletin_incluye_datos_correctos()
{
    // Arrange
    $estudiante = Estudiante::factory()->create();
    $asignatura = Asignatura::factory()->create();
    Calificacion::factory()->create([
        'estudiante_id' => $estudiante->id,
        'asignatura_id' => $asignatura->id,
        'valor' => 4.2
    ]);
    
    // Act
    $servicioBoletin = app(BoletinService::class);
    $boletin = $servicioBoletin->generarBoletinEstudiante($estudiante->id);
    
    // Assert
    $this->assertStringContainsString($estudiante->nombre, $boletin->contenido);
    $this->assertStringContainsString($asignatura->nombre, $boletin->contenido);
    $this->assertStringContainsString('4.2', $boletin->contenido);
}
```

#### Pruebas funcionales

- **Framework**: Laravel Dusk
- **Enfoque**: Pruebas end-to-end de historias de usuario
- **Cobertura**: Todos los flujos críticos de usuario

*Ejemplo de caso de prueba funcional:*

```php
public function test_docente_puede_registrar_calificaciones()
{
    // Login como docente
    $this->browse(function (Browser $browser) {
        $browser->loginAs($this->docente)
                ->visit('/academico/calificaciones/grupo/1')
                ->assertSee('Registro de Calificaciones')
                ->type('calificaciones[1]', '4.5')
                ->type('calificaciones[2]', '3.8')
                ->press('Guardar')
                ->assertSee('Calificaciones guardadas correctamente')
                
                // Verificar persistencia
                ->refresh()
                ->assertInputValue('calificaciones[1]', '4.5');
    });
}
```

#### Pruebas de rendimiento

- **Herramientas**: JMeter, Laravel Telescope
- **Métricas clave**: Tiempo de respuesta, throughput, uso de recursos
- **Escenarios**:
  - Carga normal (100 usuarios concurrentes)
  - Carga pico (300 usuarios concurrentes)
  - Estrés (hasta punto de fallo)

#### Pruebas de seguridad

- **Análisis estático de código**: Revisión de vulnerabilidades
- **Pruebas de penetración**: Evaluación de defensas contra ataques comunes
- **Análisis de dependencias**: Verificación de componentes de terceros

### 11.3 Matriz de pruebas por plugin

La siguiente matriz define las pruebas específicas para cada plugin:

#### Core

| Prueba | Descripción | Tipo | Prioridad |
|--------|-------------|------|-----------|
| Autenticación | Verificar login, logout, recuperación de contraseña | Funcional | Alta |
| Multi-tenancy | Confirmar aislamiento de datos entre tenants | Integración | Alta |
| RBAC | Validar control de acceso según roles | Funcional | Alta |
| Instalación | Probar proceso completo de instalación | Funcional | Alta |
| Auditoría | Verificar registro de acciones críticas | Integración | Media |

#### Académico-Basic

| Prueba | Descripción | Tipo | Prioridad |
|--------|-------------|------|-----------|
| Matrícula | Proceso completo de matrícula | Funcional | Alta |
| Calificaciones | Registro y cálculo de calificaciones | Funcional | Alta |
| Promoción | Proceso de promoción de estudiantes | Integración | Alta |
| Reportes | Generación de reportes académicos | Funcional | Media |
| Asistencia | Registro y consulta de asistencia | Funcional | Media |

#### Boletines

| Prueba | Descripción | Tipo | Prioridad |
|--------|-------------|------|-----------|
| Generación | Creación de boletines individuales y masivos | Funcional | Alta |
| Plantillas | Personalización de plantillas | Funcional | Alta |
| Rendimiento | Tiempo de generación bajo carga | Rendimiento | Media |
| Publicación | Proceso de publicación y notificación | Integración | Alta |
| Seguridad | Control de acceso a boletines | Seguridad | Alta |

## 12. Estrategia de Migración

### 12.1 Proceso de migración

La migración de datos es un aspecto crítico para la adopción del sistema Lazarus, especialmente en instituciones que ya cuentan con sistemas previos. La estrategia contempla los siguientes procesos:

#### Migración desde sistemas existentes

1. **Análisis de la fuente de datos**
   - Mapeo de estructuras de datos origen
   - Identificación de datos críticos y no críticos
   - Evaluación de calidad de datos

2. **Transformación de datos**
   - Normalización según esquema Lazarus
   - Conversión de formatos
   - Enriquecimiento de datos incompletos

3. **Carga en Lazarus**
   - Carga por etapas priorizadas
   - Verificación de integridad
   - Resolución de conflictos

4. **Validación post-migración**
   - Verificación muestral
   - Reportes de reconciliación
   - Ajustes manuales si necesarios

#### Migración entre modelos de licenciamiento

El plugin Migrator facilita la transición entre modelos SaaS y On-Premise:

1. **SaaS → On-Premise**
   - Exportación de datos desde plataforma SaaS
   - Generación de archivo .lzm cifrado
   - Instalación de Lazarus On-Premise
   - Importación y verificación de datos

2. **On-Premise → SaaS**
   - Generación de archivo .lzm desde instancia local
   - Transferencia segura al proveedor
   - Creación de tenant en plataforma SaaS
   - Importación y configuración de acceso

### 12.2 Herramientas de migración

#### Archivo .lzm (Lazarus Migration)

Formato propietario para migración con las siguientes características:

- Contenedor ZIP con cifrado AES-256
- Estructura JSON para metadatos
- Archivos SQL para datos estructurados
- Directorio de assets para archivos binarios
- Manifiesto de integridad con hashes

```
archivo.lzm/
├── manifest.json       # Metadatos, versión y hashes
├── structure.sql       # Estructura de la base de datos
├── data/
│   ├── core_tables.sql # Datos de tablas principales
│   ├── academic.sql    # Datos académicos
│   └── ...
├── assets/
│   ├── logos/          # Imágenes de logos
│   ├── signatures/     # Firmas digitalizadas
│   └── documents/      # Documentos cargados
└── integrity.sha256    # Hash de verificación
```

#### CLI para migración

Comandos artisan para facilitar procesos de migración:

```
php artisan lazarus:export    # Exportar datos a .lzm
php artisan lazarus:import    # Importar desde .lzm
php artisan lazarus:validate  # Verificar archivo .lzm
php artisan lazarus:transfer  # Transferencia directa
```

### 12.3 Validación de datos migrados

Para garantizar la integridad de las migraciones, se implementan los siguientes mecanismos de validación:

#### Verificaciones automáticas

- **Conteos de registros**: Comparación de totales entre origen y destino
- **Checksums**: Verificación de sumas de verificación para datos críticos
- **Integridad referencial**: Validación de relaciones entre entidades
- **Completitud**: Verificación de campos obligatorios

#### Reportes de migración

- Resumen ejecutivo del proceso
- Detalles por entidad migrada
- Lista de excepciones y resoluciones
- Tiempos de ejecución
- Recomendaciones post-migración

#### Plan de rollback

- Respaldo pre-migración
- Puntos de verificación durante el proceso
- Procedimientos de restauración
- Ventanas de tiempo para validación

## 13. Plan de Capacitación

### 13.1 Usuarios administradores

La capacitación para administradores del sistema se estructura en los siguientes módulos:

#### Módulos de capacitación

1. **Fundamentos del sistema**
   - Arquitectura general
   - Modelo de plugins
   - Flujos principales
   - Terminología clave
   
2. **Instalación y configuración**
   - Requisitos técnicos
   - Proceso de instalación
   - Configuración inicial
   - Activación de licencias
   
3. **Administración de usuarios y permisos**
   - Modelo RBAC
   - Creación y gestión de roles
   - Asignación de permisos
   - Mejores prácticas de seguridad
   
4. **Gestión académica**
   - Configuración de estructura académica
   - Proceso de matrícula
   - Asignación académica
   - Reportes académicos
   
5. **Mantenimiento y solución de problemas**
   - Copias de seguridad
   - Actualización del sistema
   - Logs y auditoría
   - Troubleshooting básico

#### Materiales y recursos

- Manual administrativo detallado
- Guías paso a paso con capturas
- Videos tutoriales
- Entorno de capacitación (sandbox)

#### Evaluación de competencias

- Cuestionarios por módulo
- Ejercicios prácticos
- Certificación de administrador

### 13.2 Usuarios finales

La capacitación para usuarios finales se adapta a cada rol específico:

#### Capacitación por roles

1. **Docentes**
   - Acceso y navegación básica
   - Registro de calificaciones
   - Registro de asistencia
   - Observador del estudiante
   - Generación de reportes

2. **Secretaría académica**
   - Gestión de estudiantes
   - Proceso de matrícula
   - Generación de certificados
   - Publicación de boletines
   - Gestión documental

3. **Directivos**
   - Dashboard institucional
   - Reportes gerenciales
   - Indicadores de desempeño
   - Aprobación de procesos
   - Configuraciones institucionales

4. **Estudiantes y acudientes**
   - Acceso al portal
   - Consulta de calificaciones
   - Visualización de boletines
   - Solicitud de certificados
   - Comunicación con docentes

#### Metodología

- Sesiones cortas y enfocadas (máx. 2 horas)
- Enfoque práctico con ejercicios reales
- Material visual y simplificado
- Capacitación en cascada (train-the-trainer)

### 13.3 Recursos de autoservicio

Para complementar la capacitación formal, se proporcionarán los siguientes recursos de autoservicio:

#### Centro de ayuda integrado

- Base de conocimientos categorizada
- Búsqueda inteligente
- Artículos paso a paso
- Preguntas frecuentes
- Glosario de términos

#### Recursos multimedia

- Videotutoriales cortos por tarea
- Webinars grabados
- Infografías de procesos
- Checklists descargables

#### Comunidad y soporte

- Foro de usuarios
- Canal de anuncios
- Tickets de soporte
- Calendario de actualizaciones
- Sesiones Q&A programadas

## 14. Cronograma de Desarrollo

### 14.1 Resumen de sprints

El desarrollo de Lazarus se organizará en 64 sprints de 2 semanas cada uno, cubriendo un período total de 48 meses. Cada sprint tiene asignadas aproximadamente 22 horas de desarrollo (11 horas semanales).

| Etapa | Sprints | Período | Plugins | Horas aprox. |
|-------|---------|---------|---------|--------------|
| Fase 1 | S1-S8 | Meses 1-4 | core, installer, rbac-extended | 176 horas |
| Fase 2 | S9-S18 | Meses 5-9 | academico-basic, boletines | 220 horas |
| Fase 3 | S19-S28 | Meses 10-14 | license-manager, payments | 220 horas |
| Fase 4 | S29-S38 | Meses 15-19 | analytics | 220 horas |
| Fase 5 | S39-S48 | Meses 20-24 | certificaciones | 220 horas |
| Fase 6 | S49-S56 | Meses 25-28 | migrator v1 | 176 horas |
| Fase 7 | S57-S64 | Meses 29-32 | lms-bridge (beta) | 176 horas |
| Reserva | +32 sprints | Meses 33-48 | observador, refinamiento, extensiones, mantenimiento | 704 horas |

**Total estimado**: ~1,408 horas (principales plugins) + ~704 horas (observador, refinamiento, mantenimiento)

### 14.2 Cronograma detallado de 48 meses

| Sprint | Fecha | Objetivos | Entregables | Horas |
|--------|-------|-----------|-------------|-------|
| **S1** | May 2025 | Inicio proyecto, arquitectura base | Repositorio inicial, estructura | 22 |
| **S2** | Jun 2025 | Core: multi-tenancy, usuarios | BD central, gestión users | 22 |
| **S3** | Jun 2025 | Core: instituciones, RBAC básico | CRUD instituciones, roles base | 22 |
| **S4** | Jul 2025 | Core: dominios, sedes | CRUD sedes, config dominios | 22 |
| **S5** | Jul 2025 | Core: dashboard, notificaciones | UI base, sistema notificaciones | 22 |
| **S6** | Ago 2025 | Installer: wizard básico | Interfaz instalación | 22 |
| **S7** | Ago 2025 | Installer: verificaciones, CLI | Verificaciones, comandos | 22 |
| **S8** | Sep 2025 | RBAC-extended | UI permisos, arquetipos | 22 |
| **S9** | Sep 2025 | Académico: estructura base | Modelos académicos core | 22 |
| **S10** | Oct 2025 | Académico: años y períodos | CRUD años, períodos | 22 |
| **S11** | Oct 2025 | Académico: cursos y asignaturas | Gestión cursos, asignaturas | 22 |
| **S12** | Nov 2025 | Académico: matrícula | Workflow matrícula | 22 |
| **S13** | Nov 2025 | Académico: asignación académica | Gestión asignación docentes | 22 |
| **S14** | Dic 2025 | Académico: escala evaluación | Config escalas, logros | 22 |
| **S15** | Dic 2025 | Académico: calificaciones | Registro calificaciones | 22 |
| **S16** | Ene 2026 | Académico: reportes básicos | Listados, estadísticas básicas | 22 |
| **S17** | Ene 2026 | Boletines: estructura base | Motor generación boletines | 22 |
| **S18** | Feb 2026 | Boletines: plantillas, preliminar | Editor plantillas | 22 |
| **S19** | Feb 2026 | Boletines: oficiales, publicación | Publicación, permisos | 22 |
| **S20** | Mar 2026 | License-manager: estructura | Modelo licencias | 22 |
| **S21** | Mar 2026 | License-manager: JWT | Generación/validación JWT | 22 |
| **S22** | Abr 2026 | License-manager: archivo .lzl | Gestión licencias On-Premise | 22 |
| **S23** | Abr 2026 | License-manager: portal cliente | UI gestión licencias | 22 |
| **S24** | May 2026 | License-manager: auditoría | Sistema auditoría licencias | 22 |
| **S25** | May 2026 | Payments: estructura | Modelo pagos | 22 |
| **S26** | Jun 2026 | Payments: integración Stripe | Pasarela Stripe | 22 |
| **S27** | Jun 2026 | Payments: integración PSE | Pasarela PSE Colombia | 22 |
| **S28** | Jul 2026 | Payments: checkout, conciliación | UI checkout, reportes | 22 |
| **S29** | Jul 2026 | Analytics: estructura | Modelos analítica | 22 |
| **S30** | Ago 2026 | Analytics: recolección datos | Sistemas de captura | 22 |
| **S31** | Ago 2026 | Analytics: dashboard básico | Dashboard institucional | 22 |
| **S32** | Sep 2026 | Analytics: indicadores académicos | Reportes académicos | 22 |
| **S33** | Sep 2026 | Analytics: tendencias | Análisis tendencias | 22 |
| **S34** | Oct 2026 | Analytics: predicciones | Modelos predictivos simples | 22 |
| **S35** | Oct 2026 | Analytics: alertas riesgo | Sistema alertas tempranas | 22 |
| **S36** | Nov 2026 | Analytics: reportes avanzados | Exportación, programación | 22 |
| **S37** | Nov 2026 | Analytics: visualizaciones | Gráficos avanzados | 22 |
| **S38** | Dic 2026 | Analytics: integración decisiones | Recomendaciones automáticas | 22 |
| **S39** | Dic 2026 | Certificaciones: estructura | Modelo certificados | 22 |
| **S40** | Ene 2027 | Certificaciones: plantillas | Editor plantillas | 22 |
| **S41** | Ene 2027 | Certificaciones: diplomas | Generación diplomas | 22 |
| **S42** | Feb 2027 | Certificaciones: actas | Actas de grado | 22 |
| **S43** | Feb 2027 | Certificaciones: constancias | Sistema constancias | 22 |
| **S44** | Mar 2027 | Certificaciones: sellos/firmas | Gestión firmas digitales | 22 |
| **S45** | Mar 2027 | Certificaciones: validación | Verificación documentos | 22 |
| **S46** | Abr 2027 | Certificaciones: códigos QR | Sistema códigos QR | 22 |
| **S47** | Abr 2027 | Certificaciones: integración pagos | Conexión con payments | 22 |
| **S48** | May 2027 | Certificaciones: libro registro | Libro registro digital | 22 |
| **S49** | May 2027 | Migrator: estructura | Diseño estructura | 22 |
| **S50** | Jun 2027 | Migrator: formato .lzm | Definición formato | 22 |
| **S51** | Jun 2027 | Migrator: exportación | Export a .lzm | 22 |
| **S52** | Jul 2027 | Migrator: importación | Import desde .lzm | 22 |
| **S53** | Jul 2027 | Migrator: validación | Verificación integridad | 22 |
| **S54** | Ago 2027 | Migrator: transferencia online | API transferencia | 22 |
| **S55** | Ago 2027 | Migrator: CLI avanzado | Herramientas CLI | 22 |
| **S56** | Sep 2027 | Migrator: cifrado avanzado | Sistema cifrado AES-256 | 22 |
| **S57** | Sep 2027 | LMS-bridge: estructura | Arquitectura conectores | 22 |
| **S58** | Oct 2027 | LMS-bridge: conectores Moodle | API conectores Moodle | 22 |
| **S59** | Oct 2027 | LMS-bridge: usuarios | Sincronización usuarios | 22 |
| **S60** | Nov 2027 | LMS-bridge: enrolamientos | Sincronización enrolamientos | 22 |
| **S61** | Nov 2027 | LMS-bridge: calificaciones | Sincronía calificaciones | 22 |
| **S62** | Dic 2027 | LMS-bridge: batch → real-time | Sistema sincronía tiempo real | 22 |
| **S63** | Dic 2027 | LMS-bridge: resolución conflictos | Gestión conflictos sincronía | 22 |
| **S64** | Ene 2028 | LMS-bridge: monitoreo | Dashboard monitoreo | 22 |
| **S65-S96** | Ene 2028 - Abr 2029 | Observador, extensiones | Plugin Observador completo, mejoras | 704 |

### 14.3 Hitos principales

| Hito | Fecha | Descripción | Criterios de éxito |
|------|-------|-------------|-------------------|
| **MVP Core** | Sep 2025 | Sistema base funcional | Instalación exitosa, multi-tenancy, RBAC |
| **Release 0.1** | Sep 2025 | Core, installer, rbac-extended | Sistema base completo instalable |
| **Release 0.2** | Feb 2026 | Académico-basic, boletines | Registro académico funcional, boletines |
| **Release 0.3** | Jul 2026 | License-manager, payments | Comercialización completa |
| **Release 0.4** | Dic 2026 | Analytics | Reportes y alertas funcionales |
| **Release 0.5** | May 2027 | Certificaciones | Documentos oficiales disponibles |
| **Release 0.6** | Sep 2027 | Migrator v1 | Migración entre instancias operativa |
| **Release 0.7** | Ene 2028 | LMS-bridge (beta) | Integración con Moodle básica |
| **Release 0.8** | Oct 2028 | Observador | Sistema observador del estudiante |
| **Release 1.0** | Abr 2029 | Sistema completo | Todos los plugins estables y refinados |

## 15. Anexos Técnicos

### 15.1 Ejemplo de archivo .env

En el proceso de instalación "One-Click", el archivo `.env` se construye automáticamente mediante el diligenciamiento del formulario web correspondiente en la interfaz. Esto facilita la configuración inicial sin necesidad de edición manual de archivos. A continuación se muestra un ejemplo del archivo `.env` resultante con sus valores y comentarios explicativos:

```bash
# Configuración básica de la aplicación
# (Generado automáticamente por el wizard de instalación)
APP_NAME=Lazarus
APP_ENV=production
APP_KEY=base64:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=
APP_DEBUG=false
APP_URL=https://example.com

# Configuración de base de datos
# (Valores introducidos en el formulario de instalación)
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=lazarus_core
DB_USERNAME=lazarus_user
DB_PASSWORD=secure_password_here

# Configuración de tenancy
# (Configurado automáticamente)
TENANCY_DATABASE_PREFIX=lazarus_tenant_
TENANCY_DATABASE_NAME_KEY=database
TENANCY_DATABASE_AUTO_DELETE=true
TENANCY_DATABASE_AUTO_CREATE=true

# Configuración de correo
# (Valores introducidos en el formulario de instalación)
MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS=no-reply@example.com
MAIL_FROM_NAME="${APP_NAME}"

# Configuración de cache y sesión
# (Valores optimizados automáticamente según entorno)
CACHE_DRIVER=redis
SESSION_DRIVER=redis
QUEUE_CONNECTION=redis
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379
REDIS_PREFIX=lazarus_

# Configuración de seguridad
# (Valores predeterminados con opción de personalización)
SESSION_SECURE_COOKIE=true
SESSION_LIFETIME=120
SANCTUM_STATEFUL_DOMAINS=example.com
LAZARUS_2FA_ENABLED=true
LAZARUS_PASSWORD_EXPIRY_DAYS=90

# Configuración de almacenamiento
# (Configurable en instalación avanzada)
FILESYSTEM_DISK=local
# Alternativamente: s3, sftp

# Configuración específica de Lazarus
# (Valores generados durante la instalación)
LAZARUS_LICENSE_PUBLIC_KEY=xxxxx
LAZARUS_TENANT_ID=xxxxx
LAZARUS_AUDIT_RETENTION_DAYS=365
LAZARUS_BACKUP_ENABLED=true
LAZARUS_BACKUP_FREQUENCY=daily
```

Todas estas variables pueden ser modificadas posteriormente a través del panel de administración o editando manualmente el archivo `.env` en caso de instalaciones On-Premise.

### 15.2 Plantilla my.cnf optimizada

```ini
# Configuración optimizada de MariaDB para Lazarus
# Ajustar según recursos del servidor

[mysqld]
# Configuración básica
user = mysql
pid-file = /var/run/mysqld/mysqld.pid
socket = /var/run/mysqld/mysqld.sock
port = 3306
basedir = /usr
datadir = /var/lib/mysql
tmpdir = /tmp
lc-messages-dir = /usr/share/mysql

# Carácter y collation
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

# InnoDB
default_storage_engine = InnoDB
innodb_buffer_pool_size = 1G  # Ajustar al 70% de RAM disponible
innodb_buffer_pool_instances = 4
innodb_file_per_table = 1
innodb_flush_log_at_trx_commit = 2
innodb_log_buffer_size = 32M
innodb_log_file_size = 256M
innodb_write_io_threads = 8
innodb_read_io_threads = 8
innodb_flush_method = O_DIRECT

# Optimizaciones específicas multi-tenant
max_connections = 500
table_open_cache = 4000
table_definition_cache = 2000
open_files_limit = 10000

# Query Cache (útil para entornos lectura-intensivos)
query_cache_type = 1
query_cache_size = 128M
query_cache_limit = 2M

# Logs
slow_query_log = 1
slow_query_log_file = /var/log/mysql/mysql-slow.log
long_query_time = 2

# Seguridad
max_allowed_packet = 16M
sql_mode = STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION

# Optimización para multi-BD (múltiples tenants)
metadata_locks_hash_instances = 8
table_open_cache_instances = 8

[client]
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4
```

### 15.3 Estructura archivo .lzm

El formato `.lzm` (Lazarus Migration) es un archivo ZIP cifrado con AES-256 que contiene la siguiente estructura:

```
[archivo].lzm/
├── manifest.json     # Metadatos de la migración
├── structure/        # Estructura de base de datos
│   ├── tables.sql    # Definición de tablas
│   ├── views.sql     # Vistas
│   └── indexes.sql   # Índices y claves
├── data/             # Datos por módulo
│   ├── core/         # Datos del core
│   │   ├── users.csv
│   │   ├── roles.csv
│   │   └── ...
│   ├── academic/     # Datos académicos
│   │   ├── courses.csv
│   │   ├── subjects.csv
│   │   └── ...
│   └── ...
├── files/            # Archivos binarios
│   ├── logos/        # Logos institucionales
│   ├── signatures/   # Firmas digitalizadas
│   └── documents/    # Documentos subidos
├── config/           # Configuraciones
│   ├── settings.json # Configuraciones generales
│   ├── templates/    # Plantillas
│   └── ...
└── integrity.json    # Hashes de verificación
```

**Ejemplo de manifest.json:**

```json
{
  "version": "1.0",
  "created_at": "2025-10-15T14:30:00Z",
  "source": {
    "system": "Lazarus",
    "version": "0.4.2",
    "tenant_id": "inst_12345",
    "institution": "Colegio Ejemplo"
  },
  "contents": {
    "users": 152,
    "students": 1250,
    "courses": 25,
    "grades": 45000,
    "files": 87
  },
  "schema_version": "2025.10",
  "encryption": "AES-256-GCM",
  "compression": "deflate",
  "creator": "administrator@example.com"
}
```

### 15.4 Configuración webhook WooCommerce

Para la integración con WooCommerce, se utiliza un webhook con la siguiente configuración:

**En WooCommerce:**

1. Ir a WooCommerce > Configuración > Avanzado > Webhooks
2. Crear nuevo webhook:
   - Nombre: "Lazarus License Activation"
   - Estado: Activo
   - Tema: Pedido actualizado
   - Dirección de entrega: `https://[dominio-lazarus]/api/webhooks/woocommerce`
   - Secreto: `[clave-secreta-generada]`
   - Versión de la API: v3

**En Lazarus (config/lazarus-license.php):**

```php
return [
    'woocommerce' => [
        'enabled' => true,
        'webhook_secret' => env('LAZARUS_WOOCOMMERCE_SECRET', 'clave-secreta-aqui'),
        'product_mapping' => [
            // ID de producto en WooCommerce => configuración de licencia
            '123' => [
                'type' => 'saas',
                'duration_days' => 365,
                'modules' => ['core', 'academic', 'bulletins', 'analytics'],
                'limits' => [
                    'students' => 500,
                    'branches' => 2
                ]
            ],
            '124' => [
                'type' => 'on-premise',
                'duration_days' => 365,
                'modules' => ['core', 'academic', 'bulletins', 'analytics', 'certificates'],
                'limits' => [
                    'students' => 1000,
                    'branches' => 5
                ]
            ],
            // Más mapeos...
        ],
        'status_mapping' => [
            'completed' => 'active',
            'processing' => 'pending',
            'refunded' => 'cancelled',
            'failed' => 'cancelled',
            'cancelled' => 'cancelled'
        ],
        'notification_email' => true,
        'auto_provision' => true
    ]
];
```

**Estructura de payload esperado:**

```json
{
  "id": 123,
  "status": "completed",
  "date_created": "2025-10-15T14:30:00",
  "total": "500.00",
  "customer_id": 45,
  "billing": {
    "first_name": "Juan",
    "last_name": "Pérez",
    "company": "Colegio Ejemplo",
    "email": "admin@colegio-ejemplo.edu.co",
    "phone": "3101234567"
  },
  "line_items": [
    {
      "product_id": 123,
      "name": "Lazarus SaaS Premium Anual",
      "quantity": 1
    }
  ],
  "meta_data": [
    {
      "key": "institution_name",
      "value": "Colegio Ejemplo"
    },
    {
      "key": "institution_code",
      "value": "12345"
    }
  ]
}
```

### 15.5 Plantilla de roles JSON

Ejemplo de estructura JSON para exportación/importación de roles:

```json
{
  "role": {
    "name": "Coordinador Académico",
    "slug": "coordinador-academico",
    "description": "Rol para coordinadores académicos con permisos de supervisión",
    "parent_role": "directivo",
    "level": "sede",
    "scope_restriction": true
  },
  "permissions": [
    {
      "name": "academic.courses.view",
      "granted": true,
      "conditions": null
    },
    {
      "name": "academic.courses.update",
      "granted": true,
      "conditions": {
        "sede_id": "@current_sede_id"
      }
    },
    {
      "name": "academic.grades.approve",
      "granted": true,
      "conditions": {
        "sede_id": "@current_sede_id"
      }
    },
    {
      "name": "academic.enrollments.view",
      "granted": true,
      "conditions": {
        "sede_id": "@current_sede_id"
      }
    },
    {
      "name": "academic.enrollments.create",
      "granted": true,
      "conditions": {
        "sede_id": "@current_sede_id"
      }
    },
    {
      "name": "academic.enrollments.update",
      "granted": true,
      "conditions": {
        "sede_id": "@current_sede_id"
      }
    },
    {
      "name": "academic.teachers.assign",
      "granted": true,
      "conditions": {
        "sede_id": "@current_sede_id"
      }
    },
    {
      "name": "reports.academic.view",
      "granted": true,
      "conditions": {
        "sede_id": "@current_sede_id"
      }
    },
    {
      "name": "reports.bulletins.approve",
      "granted": true,
      "conditions": {
        "sede_id": "@current_sede_id"
      }
    },
    {
      "name": "observer.view",
      "granted": true,
      "conditions": {
        "sede_id": "@current_sede_id"
      }
    },
    {
      "name": "observer.create",
      "granted": true,
      "conditions": {
        "sede_id": "@current_sede_id"
      }
    }
  ],
  "metadata": {
    "created_at": "2025-01-15T10:30:00Z",
    "updated_at": "2025-02-20T14:15:00Z",
    "created_by": "user@example.com",
    "version": "1.2",
    "system_version": "0.3.1"
  }
}
```

### 15.6 Esquema manifest.json para plugins

Cada plugin debe incluir un archivo `manifest.json` en su raíz con la siguiente estructura:

```json
{
  "name": "Nombre del Plugin",
  "code": "academic-basic",
  "description": "Gestión académica básica para Lazarus",
  "version": "1.0.0",
  "author": {
    "name": "Alonso Arias",
    "email": "contact@example.com",
    "url": "https://example.com"
  },
  "minimum_core": "0.1.0",
  "compatible_core": "^0.1.0",
  "dependencies": [
    {
      "plugin": "core",
      "version": "^0.1.0"
    },
    {
      "plugin": "rbac-extended",
      "version": "^0.1.0",
      "optional": false
    }
  ],
  "conflicts": [
    {
      "plugin": "legacy-academic",
      "version": "*"
    }
  ],
  "scope": [
    "system",
    "institution",
    "branch"
  ],
  "settings": {
    "configurable": true,
    "has_migrations": true,
    "has_routes": true,
    "has_views": true,
    "has_translations": true
  },
  "permissions": [
    {
      "name": "academic.courses.view",
      "description": "Ver cursos"
    },
    {
      "name": "academic.courses.create",
      "description": "Crear cursos"
    },
    {
      "name": "academic.grades.register",
      "description": "Registrar calificaciones"
    }
  ],
  "paid": false,
  "price_tier": "basic"
}
```

---

**Nota final**: Este Documento de Especificación y Diseño (DED) proporciona una guía completa para el desarrollo del sistema Lazarus. La implementación debe seguir fielmente las especificaciones aquí detalladas, pero manteniendo la flexibilidad necesaria para adaptarse a situaciones imprevistas durante el desarrollo. El cronograma y alcance están diseñados considerando las restricciones de un solo desarrollador con dedicación parcial, priorizando la entrega incremental de valor.
