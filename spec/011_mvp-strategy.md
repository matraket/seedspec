# Estrategia de Implementacion del MVP -- Associated

| Campo        | Valor                              |
|--------------|------------------------------------|
| Version      | 1.0                                |
| Fecha        | 2026-03-21                         |
| Estado       | Verificado                         |
| Audiencia    | CTO, Director Tecnico, Tech Leads  |
| Proyecto     | Associated ERP                     |

---

## 1. Resumen Ejecutivo

Associated es un ERP ligero disenado para colectividades espanolas (asociaciones, clubs, federaciones) que implementa una arquitectura de Modular Monolith con aislamiento multi-tenant por base de datos. El MVP cubre los 3 Bounded Contexts criticos (Identity, Membership, Treasury) con 20 casos de uso (19 Must + 1 Should), distribuidos en 38 tareas a lo largo de 4 fases (Scaffold + 3 fases funcionales). La estrategia de implementacion sigue un modelo de capas de dependencia que garantiza que cada fase entrega valor incremental verificable, respetando el grafo de dependencias entre UCs y minimizando el riesgo de integracion.

---

## 2. Alcance del MVP

### 2.1 Bounded Contexts incluidos

| BC              | Justificacion de inclusion                                          |
|-----------------|---------------------------------------------------------------------|
| BC-Identity     | Fundacional: sin tenants ni autenticacion no existe el sistema      |
| BC-Membership   | Core de negocio: gestion de socios es la razon de ser de la app     |
| BC-Treasury     | Ciclo economico: cuotas, cobros y SEPA son criticos para operar     |

### 2.2 Bounded Contexts excluidos del MVP

| BC               | Razon de exclusion                                                 |
|------------------|--------------------------------------------------------------------|
| BC-Events        | Funcionalidad complementaria, no bloquea operativa basica          |
| BC-Communication | Puede gestionarse manualmente durante el MVP                       |
| BC-Documents     | Almacenamiento documental es secundario al ciclo core              |

### 2.3 Estadisticas del MVP

```
+----------------------------------+--------+
| Metrica                          | Valor  |
+----------------------------------+--------+
| Casos de uso totales             |     20 |
|   - Must Have                    |     19 |
|   - Should Have                  |      1 |
| Tareas totales                   |     38 |
|   - Backend                      |     20 |
|   - Frontend                     |     18 |
| User Stories cubiertas           |    ~56 |
| Requisitos funcionales trazados  |    ~40 |
| BCs activos (de 6)               |      3 |
| Transversales                    |      2 |
| Fases de implementacion          |      4 |
| Actores principales              |      6 |
+----------------------------------+--------+
```

---

## 3. Arquitectura de Alto Nivel

```
+------------------------------------------------------------------+
|                        CAPA DE PRESENTACION                      |
|                    React 19 + Mantine 8 + Vite 7                 |
+------------------------------------------------------------------+
          |                    |                    |
          v                    v                    v
+------------------+ +------------------+ +------------------+
|   BC-Identity    | |  BC-Membership   | |   BC-Treasury    |
|                  | |                  | |                  |
| - Tenant Prov.   | | - Tipos socio    | | - Planes cuota   |
| - Autenticacion  | | - Ejercicios     | | - Suscripciones  |
| - Roles/Permisos | | - Ficha socio    | | - Cargos         |
|                  | | - Alta/Baja      | | - Cobros         |
|                  | | - Estados        | | - SEPA           |
+--------+---------+ +--------+---------+ +--------+---------+
         |                    |                    |
         |          Domain Events (en proceso)     |
         |  <--------------------------------------->
         |                    |                    |
+------------------------------------------------------------------+
|                    CAPA DE INFRAESTRUCTURA                       |
|              NestJS 11 + Prisma 7 + PostgreSQL 18                |
+------------------------------------------------------------------+
         |                    |                    |
    +----+----+          +----+----+          +----+----+
    | Tenant  |          | Tenant  |          | Tenant  |
    |  DB A   |          |  DB B   |          |  DB N   |
    +---------+          +---------+          +---------+

                 Aislamiento multi-tenant (ADR-002)
```

### 3.1 Comunicacion entre Bounded Contexts

```
BC-Identity                BC-Membership              BC-Treasury
    |                           |                          |
    |--- TenantProvisioned ---->|                          |
    |--- UserCreated ---------->|                          |
    |                           |--- MemberRegistered ---->|
    |                           |--- MemberStatusChanged ->|
    |                           |                          |
    |                           |<-- ChargeGenerated ------|
    |                           |<-- PaymentRecorded ------|
```

Los BCs se comunican exclusivamente via Domain Events (ADR-008), manteniendo acoplamiento debil y autonomia de cada contexto.

---

## 4. Hoja de Ruta por Fases

### FASE 0: Scaffold (Prerequisito obligatorio)

**Objetivo estrategico:** Establecer la infraestructura base que habilita todo el desarrollo posterior.

**Entregables:**
- Docker Compose con PostgreSQL multi-tenant
- Pipeline CI/CD en GitHub Actions
- Estructura de carpetas DDD por Bounded Context
- Prisma setup con migraciones base
- NestJS bootstrap con modulos por BC
- Configuracion de calidad (ESLint, Prettier, Husky)

**Justificacion:** Sin infraestructura no hay desarrollo. Esta fase elimina toda friccion tecnica antes de implementar logica de negocio.

---

### FASE 1: Fundacion y Core (20 tareas: 14 back + 6 front)

**Objetivo estrategico:** Entregar un sistema funcional minimo donde un tenant puede operar el ciclo completo: alta de socios, configuracion de cuotas, generacion de cargos y registro de cobros.

**Backend (14 tareas):**

```
+------+--------+-------------------------------------------+-----------------+
| Task | UC     | Titulo                                    | BC              |
+------+--------+-------------------------------------------+-----------------+
|    1 | UC-001 | Provision de nuevo tenant                 | BC-Identity     |
|    2 | UC-002 | Autenticacion multi-tenant                | BC-Identity     |
|    3 | UC-003 | Configuracion de tenant                   | BC-Identity     |
|    4 | UC-008 | Configuracion de tipos de socio           | BC-Membership   |
|    5 | UC-010 | Gestion de ejercicios                     | BC-Membership   |
|    6 | UC-007 | Gestion de estados del socio              | BC-Membership   |
|    7 | UC-006 | Gestion de ficha de socio                 | BC-Membership   |
|    8 | UC-011 | Alta simple de socio                      | BC-Membership   |
|    9 | UC-013 | Baja de socio                             | BC-Membership   |
|   10 | UC-017 | Configuracion de planes de cuota          | BC-Treasury     |
|   11 | UC-018 | Gestion de suscripciones de cuota         | BC-Treasury     |
|   12 | UC-019 | Generacion masiva de cargos periodicos    | BC-Treasury     |
|   13 | UC-021 | Registro de cobros                        | BC-Treasury     |
+------+--------+-------------------------------------------+-----------------+
```

**Frontend (6 tareas):**

```
+------+--------+-------------------------------------------+
| Task | UC     | Titulo                                    |
+------+--------+-------------------------------------------+
|    0 | N/A    | Brand Setup (identidad visual, theme)     |
|    1 | UC-002 | Autenticacion multi-tenant (login page)   |
|    2 | UC-017 | Configuracion de planes de cuota          |
|    3 | UC-018 | Gestion de suscripciones de cuota         |
|    4 | UC-011 | Alta simple de socio (wizard multi-paso)  |
|    5 | UC-013 | Baja de socio                             |
+------+--------+-------------------------------------------+
```

**Justificacion del orden:**
- UC-001 y UC-002 son prerequisitos absolutos (sin tenant ni auth, nada funciona).
- UC-003 (configuracion de tenant) se incluye inmediatamente despues de provision, ya que branding y configuracion inicial son parte del setup fundacional del tenant.
- UC-008 antes de UC-006 porque los tipos de socio son prerequisito para crear fichas.
- UC-010 habilitado en paralelo (ejercicios son independientes de fichas).
- UC-017 antes de UC-018/UC-019 porque sin planes de cuota no hay suscripciones ni cargos.
- UC-011 requiere UC-008, UC-010 y UC-017 (convergencia de dependencias).
- El frontend prioriza login, cuotas y alta de socio como flujos criticos del dia a dia.

**Entregable:** Un tenant puede autenticarse, configurar tipos de socio, abrir ejercicio, dar de alta socios con cuotas, generar cargos y registrar cobros.

---

### FASE 2: Operativa Avanzada (13 tareas: 5 back + 8 front)

**Objetivo estrategico:** Completar el ciclo economico con SEPA, anadir importacion masiva para onboarding, y dotar al frontend de las vistas operativas que faltan.

**Backend (5 tareas):**

```
+------+--------+-------------------------------------------+-----------------+
| Task | UC     | Titulo                                    | BC              |
+------+--------+-------------------------------------------+-----------------+
|    1 | UC-004 | Gestion de roles y permisos               | BC-Identity     |
|    2 | UC-020 | Gestion de cargos manuales                | BC-Treasury     |
|    3 | UC-056 | Importacion masiva de socios              | Transversal     |
|    4 | UC-023 | Generacion de remesas SEPA                | BC-Treasury     |
|    5 | UC-024 | Gestion de devoluciones SEPA              | BC-Treasury     |
+------+--------+-------------------------------------------+-----------------+
```

**Frontend (8 tareas):**

```
+------+--------+-------------------------------------------+
| Task | UC     | Titulo                                    |
+------+--------+-------------------------------------------+
|    1 | UC-006 | Gestion de ficha de socio (listado)       |
|    2 | UC-008 | Configuracion de tipos de socio           |
|    3 | UC-019 | Vista de cargos periodicos                |
|    4 | UC-020 | Gestion de cargos manuales                |
|    5 | UC-021 | Registro de cobros                        |
|    6 | UC-056 | Importacion masiva de socios (wizard)     |
|    7 | UC-023 | Generacion de remesas SEPA                |
|    8 | UC-024 | Gestion de devoluciones SEPA              |
+------+--------+-------------------------------------------+
```

> **Nota:** Las fases representan una estrategia de entrega incremental orientada al TFM, no una priorización MoSCoW. El MVP completo comprende las 3 fases — un UC clasificado como Must puede estar en cualquier fase. El producto no sale al mercado hasta completar todas las fases. UC-004 es Must y aparece en Fase 2 porque en Fase 1 se opera con roles hardcoded; la granularidad real de permisos se implementa una vez el ciclo core está estable.

**Justificacion del orden:**
- UC-004 (roles) se pospone a Fase 2 porque en Fase 1 se trabaja con roles hardcoded/seeding. Ahora se necesita granularidad real.
- UC-020 (cargos manuales) es independiente del flujo automatico y se implementa en paralelo.
- UC-023/UC-024 (SEPA) son el ultimo eslabon del ciclo economico y requieren que cargos y cobros ya funcionen.
- UC-056 (importacion masiva) habilita el onboarding real de colectividades con datos legacy.
- El frontend completa TODAS las vistas operativas pendientes de Fase 1.

**Entregable:** Ciclo economico completo incluyendo SEPA, gestion granular de permisos, importacion masiva funcional, y todas las vistas operativas en la interfaz.

---

### FASE 3: Reporting y Cierre (6 tareas: 2 back + 4 front)

**Objetivo estrategico:** Dotar al MVP de capacidad analitica y completar las interfaces de administracion que cierran el producto.

**Backend (2 tareas):**

```
+------+--------+-------------------------------------------+-----------------+
| Task | UC     | Titulo                                    | BC              |
+------+--------+-------------------------------------------+-----------------+
|    1 | UC-064 | Dashboard principal y KPIs                | Transversal     |
|    2 | UC-065 | Graficos de evolucion                     | Transversal     |
+------+--------+-------------------------------------------+-----------------+
```

**Frontend (4 tareas):**

```
+------+--------+-------------------------------------------+
| Task | UC     | Titulo                                    |
+------+--------+-------------------------------------------+
|    1 | UC-001 | Provision de nuevo tenant (UI superadmin) |
|    2 | UC-010 | Gestion de ejercicios                     |
|    3 | UC-064 | Dashboard principal y KPIs                |
|    4 | UC-065 | Graficos de evolucion                     |
+------+--------+-------------------------------------------+
```

**Justificacion del orden:**
- UC-064/UC-065 son de solo lectura y requieren datos existentes para ser utiles. Ponerlos al final garantiza que hay datos reales que mostrar.
- La UI de UC-001 (superadmin) se pospone porque durante desarrollo el provisioning se hace via API/CLI.
- UC-010 (ejercicios) en frontend se pospone porque su backend ya funciona desde Fase 1 y la gestion manual es viable via API.

**Entregable:** MVP completo con dashboards, graficos de evolucion, panel de superadmin y todas las interfaces de configuracion.

---

## 5. Grafo de Dependencias entre UCs

```
UC-001 (Provision tenant)
  |
  |--- FUNDACIONAL ABSOLUTO ---
  |
  +---> UC-002 (Autenticacion) --- PREREQUISITO UNIVERSAL
  |
  +---> UC-004 (Roles y permisos) --- independiente de otros BCs
  |
  +---> UC-008 (Tipos de socio) --- CONFIGURACION BASE
           |
           +---> UC-006 (Ficha de socio)
           |       |
           |       +---> UC-007 (Estados del socio)
           |       |       |
           |       |       +---> UC-013 (Baja de socio)
           |       |
           |       +---> UC-020 (Cargos manuales)
           |
           +---> UC-010 (Ejercicios)
           |
           +---> UC-017 (Planes de cuota)
           |       |
           |       +---> UC-018 (Suscripciones)
           |       |       |
           |       |       +---> UC-019 (Generacion cargos)
           |       |               |
           |       |               +---> UC-021 (Registro cobros)
           |       |               |
           |       |               +---> UC-023 (Remesas SEPA)
           |       |                       |
           |       |                       +---> UC-024 (Devoluciones SEPA)
           |       |
           |       +---> UC-011 (Alta simple)
           |               |
           |               +--- requiere tambien: UC-010
           |
           +---> UC-056 (Importacion masiva)

UC-064 (Dashboard) <--- lectura de: UC-006, UC-011, UC-019, UC-021
  |
  +---> UC-065 (Graficos) <--- requiere tambien: UC-010
```

### Leyenda de criticidad

```
FUNDACIONAL:     UC-001, UC-002, UC-008, UC-017
                 Sin estos el sistema no puede operar.

INDEPENDIENTES:  UC-004, UC-020, UC-056, UC-064, UC-065
                 Solo dependen de fundacionales, no entre si.

CADENA LARGA:    UC-001 -> UC-008 -> UC-017 -> UC-018 -> UC-019 -> UC-023 -> UC-024
                 Ruta critica mas larga: 7 UCs encadenados (ciclo economico completo).
```

---

## 6. Flujos de Negocio

### Flujo 1: Setup Inicial (Provision)

```
Superadmin           Sistema              BD Central
    |                   |                     |
    |-- Crear tenant -->|                     |
    |                   |-- Crear schema ---->|
    |                   |-- Seed roles ------>|
    |                   |-- Crear admin ----->|
    |                   |                     |
    |                   |<- TenantProvisioned |
    |<-- Credenciales --|                     |
    |                   |                     |
    |-- Login --------->|                     |
    |                   |-- Validar creds --->|
    |                   |-- Generar JWT ----->|
    |<-- JWT + Refresh -|                     |
```

**Objetivo:** Tenant funcional con usuarios autenticados y roles basicos.
**UCs involucrados:** UC-001, UC-002, UC-004

---

### Flujo 2: Configuracion del Tenant

```
Presidente/          Sistema              BD Tenant
Secretario              |                     |
    |                   |                     |
    |-- Crear tipos --->|                     |
    |   de socio        |-- INSERT tipos ---->|
    |                   |<- MemberTypeCreated |
    |                   |                     |
    |-- Abrir ---------->|                    |
    |   ejercicio       |-- INSERT fiscal --->|
    |                   |<- FiscalYearOpened  |
    |                   |                     |
    |-- Configurar ---->|                     |
    |   planes cuota    |-- INSERT plans ---->|
    |                   |-- Vincular tipo --->|
    |                   |<- FeePlanCreated    |
```

**Objetivo:** Categorias de socios, ejercicio fiscal y planes de cuota definidos.
**UCs involucrados:** UC-008, UC-010, UC-017

---

### Flujo 3: Ciclo de Vida del Socio

```
Secretario           Sistema              BC-Membership       BC-Treasury
    |                   |                     |                    |
    |-- Alta socio ---->|                     |                    |
    |   (wizard)        |-- Crear Member ---->|                    |
    |                   |-- Crear Account ----+------------------->|
    |                   |-- Crear Subscrip ---+------------------->|
    |                   |<- MemberRegistered  |                    |
    |                   |                     |                    |
    |-- Editar ficha -->|                     |                    |
    |                   |-- UPDATE member --->|                    |
    |                   |<- MemberDataUpdated |                    |
    |                   |                     |                    |
    |-- Cambiar ------->|                     |                    |
    |   estado          |-- Transicion ------>|                    |
    |                   |<- StatusChanged     |                    |
    |                   |                     |                    |
    |-- Dar de baja --->|                     |                    |
    |                   |-- Verificar deuda --+------------------->|
    |                   |-- Desactivar ------>|                    |
    |                   |<- MemberDeactivated |                    |
```

**Objetivo:** Gestionar el ciclo completo desde el alta hasta la baja del socio.
**UCs involucrados:** UC-011, UC-006, UC-007, UC-013

---

### Flujo 4: Ciclo Economico

```
Tesorero             Sistema              BC-Treasury          Banco
    |                   |                     |                   |
    |-- Generar ------->|                     |                   |
    |   cargos          |-- Batch cargos ---->|                   |
    |   periodicos      |<- ChargeGenerated   |                   |
    |                   |                     |                   |
    |-- Cargo manual -->|                     |                   |
    |                   |-- INSERT charge --->|                   |
    |                   |<- ChargeGenerated   |                   |
    |                   |                     |                   |
    |-- Registrar ----->|                     |                   |
    |   cobro           |-- INSERT payment -->|                   |
    |                   |-- UPDATE charge --->|                   |
    |                   |<- PaymentRecorded   |                   |
    |                   |                     |                   |
    |-- Generar ------->|                     |                   |
    |   remesa SEPA     |-- Crear XML ------->|                   |
    |                   |-- Firmar + cifrar ->|                   |
    |                   |                     |-- Enviar XML ---->|
    |                   |<- RemittanceSent    |                   |
    |                   |                     |                   |
    |                   |                     |<- pain.002 -------|
    |                   |-- Parsear resp. --->|                   |
    |                   |<- PaymentReturned   |                   |
```

**Objetivo:** Gestionar el flujo economico completo desde suscripciones hasta SEPA.
**UCs involucrados:** UC-018, UC-019, UC-020, UC-021, UC-023, UC-024

---

### Flujo 5: Migracion (Onboarding)

```
Administrador        Sistema              BC-Membership       Bull Queue
    |                   |                     |                    |
    |-- Upload CSV ---->|                     |                    |
    |   (max 10MB)      |-- Validar formato ->|                    |
    |                   |                     |                    |
    |   [<=500 rows]    |-- Batch INSERT ---->|                    |
    |                   |   (100/TX)          |                    |
    |                   |<- ImportComplete    |                    |
    |                   |                     |                    |
    |   [>500 rows]     |-- Encolar job ------+------------------->|
    |                   |<- ImportQueued      |                    |
    |                   |                     |<- Process batch ---|
    |                   |                     |-- 100/TX --------->|
    |<-- Notificacion --|<- ImportComplete    |                    |
```

**Objetivo:** Permitir la carga de datos legacy de colectividades existentes.
**UCs involucrados:** UC-056

---

### Flujo 6: Reporting (Solo Lectura)

```
Presidente/          Sistema              Multiples BCs        Redis
Tesorero                |                     |                   |
    |                   |                     |                   |
    |-- Dashboard ----->|                     |                   |
    |                   |-- Check cache ------+------------------>|
    |                   |                     |                   |
    |   [cache miss]    |-- Query members --->|                   |
    |                   |-- Query accounts -->|                   |
    |                   |-- Query charges --->|                   |
    |                   |-- Agregar KPIs ---->|                   |
    |                   |-- SET cache --------+------------------>|
    |                   |                     |   (TTL 5min)      |
    |<-- KPIs + charts -|                     |                   |
    |                   |                     |                   |
    |   [cache hit]     |<--------------------+----- GET cache ---|
    |<-- KPIs + charts -|                     |                   |
```

**Objetivo:** Dashboards y analitica para la toma de decisiones.
**UCs involucrados:** UC-064, UC-065

---

## 7. Cadena de Trazabilidad

### 7.1 Trazabilidad funcional: RF -> BC -> US -> UC

```
Requisito Funcional (RF)          Bounded Context          User Story (US)          Caso de Uso (UC)
========================          ===============          ===============          ================

N2RF01 (Crear tenant)       --->  BC-Identity        --->  US-001             --->  UC-001
N2RF02 (Login multi-tenant) --->  BC-Identity        --->  US-002             --->  UC-002
N2RF03 (Config. tenant)     --->  BC-Identity        --->  US-003             --->  UC-003
N2RF04, N2RF05 (Roles)      --->  BC-Identity        --->  US-004, US-005     --->  UC-004

N3RF01..N3RF05 (Ficha)      --->  BC-Membership      --->  US-009..US-013     --->  UC-006
N3RF06 (Estados)            --->  BC-Membership      --->  US-014             --->  UC-007
N3RF07..N3RF11 (Tipos)      --->  BC-Membership      --->  US-015..US-019     --->  UC-008
N3RF15..N3RF19 (Ejercicios) --->  BC-Membership      --->  US-023..US-027     --->  UC-010
N3RF20 (Alta)               --->  BC-Membership      --->  US-028             --->  UC-011
N3RF24..N3RF27 (Baja)       --->  BC-Membership      --->  US-032..US-035     --->  UC-013

N4RF01 (Planes cuota)       --->  BC-Treasury        --->  US-043, US-044     --->  UC-017
N4RF01..N4RF07 (Suscrip.)   --->  BC-Treasury        --->  US-045..US-052     --->  UC-018
N4RF02, N4RF03 (Cargos)     --->  BC-Treasury        --->  US-047, US-048     --->  UC-019
N4RF08 (Cargo manual)       --->  BC-Treasury        --->  US-051             --->  UC-020
N4RF09..N4RF13 (Cobros)     --->  BC-Treasury        --->  US-053..US-057     --->  UC-021
N4RF17..N4RF21 (SEPA)       --->  BC-Treasury        --->  US-061..US-065     --->  UC-023
N4RF22, N4RF23 (Devol.)     --->  BC-Treasury        --->  US-066, US-067     --->  UC-024

N8RF01..N8RF04 (Importar)   --->  Transversal        --->  US-148..US-151     --->  UC-056
N9RF01..N9RF06 (Dashboard)  --->  Transversal        --->  US-161..US-166     --->  UC-064, UC-065
```

### 7.2 Trazabilidad no funcional: RNF -> RNFT -> ADR

```
Requisito No Funcional         Restriccion Tecnica           Decision Arquitectonica
=======================        ====================          ======================

RNF-006 (Datos sensibles) ---> RNFT-009 (AES-256-GCM)  --->  ADR-002 (BD aislada/tenant)
                                                             ADR-009 (Clean Architecture)

RNF-011 (Rate limiting)   ---> RNFT-011 (Throttle)     --->  ADR-006 (JWT + Passport)
                                                             ADR-007 (RBAC Guards)

RNF-015 (Rendimiento)     ---> RNFT-015 (<30s/1000)    --->  ADR-005 (Prisma ORM)
                               RNFT-017 (<2s dashboard)      ADR-004 (CQRS)
                               RNFT-018 (graficos)

RNF-019 (Cache)           ---> RNFT-019 (Redis TTL)    --->  ADR-004 (CQRS read models)

RNF-022 (Documentos)      ---> RNFT-022 (PDF gen)      --->  ADR-009 (Clean Architecture)

RNF-025 (Auditoria)       ---> RNFT-025 (INSERT-only)  --->  ADR-008 (Domain Events)
                               RNFT-037 (historial)

RNF-045 (UX)              ---> RNFT-045 (wizard)       --->  ADR-010 (REST API)
                               RNFT-050 (charts)

RNF-058 (Calidad)         ---> Quality gates CI        --->  ADR-011 (Testing strategy)
                                                             ADR-012 (CI pipeline)
```

### 7.3 Diagrama de trazabilidad cruzada

```
+--------+     +--------+     +--------+     +--------+
|   RF   |---->|   US   |---->|   UC   |---->|   BC   |
| (func) |     | (user  |     | (caso  |     | (bound.|
|        |     |  story)|     |  uso)  |     | context|
+--------+     +--------+     +--------+     +--------+
                                  |
                                  | implementa
                                  v
+--------+     +--------+     +--------+     +--------+
|  RNF   |---->|  RNFT  |---->|  ADR   |---->| Codigo |
| (no    |     | (restr.|     | (arch. |     | fuente |
|  func) |     |  tecn.)|     |  dec.) |     |        |
+--------+     +--------+     +--------+     +--------+
```

---

## 8. Estrategia de Capas

La implementacion sigue un modelo de 8 capas que respeta estrictamente el grafo de dependencias:

```
Capa 0 --- Fundacion ----------- UC-001, UC-002, UC-003
  |
  v
Capa 1 --- Configuracion ------- UC-004, UC-008
  |
  v
Capa 2 --- Operativa basica ---- UC-010, UC-006, UC-017
  |
  v
Capa 3 --- Operativa core ------ UC-011, UC-007, UC-018
  |
  v
Capa 4 --- Flujo economico ----- UC-019, UC-020, UC-021
  |
  v
Capa 5 --- SEPA ----------------- UC-023, UC-024
  |
  v
Capa 6 --- Procesos avanzados -- UC-013
  |
  v
Capa 7 --- Utilidades ---------- UC-056, UC-064, UC-065
```

### Principios de la estrategia

1. **Cada capa solo depende de capas anteriores.** Nunca se implementa un UC cuyas dependencias no esten completas en una capa previa.

2. **Valor incremental verificable.** Al finalizar cada capa, el sistema entrega funcionalidad utilizable. No hay capas "intermedias" sin valor propio.

3. **Ruta critica explicitada.** La cadena mas larga (UC-001 -> UC-008 -> UC-017 -> UC-018 -> UC-019 -> UC-023 -> UC-024) atraviesa 7 capas. Cualquier retraso en esta cadena impacta directamente la fecha de entrega.

4. **Paralelismo habilitado.** Dentro de cada capa, los UCs son independientes entre si y pueden implementarse en paralelo:
   - Capa 1: UC-004 y UC-008 en paralelo
   - Capa 2: UC-010, UC-006, UC-017 en paralelo
   - Capa 4: UC-019, UC-020, UC-021 en paralelo (UC-020 solo requiere socio existente)

5. **UC-013 (Baja) pospuesto a Capa 6.** Aunque parece simple, requiere verificacion de morosidad (UC-022 fuera de MVP) y workflow de estados completo. Se implementa tras el flujo economico para tener datos reales de deuda.

---

## 9. Matriz de Riesgo

### Fase 0: Scaffold

```
+------+-----------------------------------+----------+----------+---------------------------+
| ID   | Riesgo                            | Prob.    | Impacto  | Mitigacion                |
+------+-----------------------------------+----------+----------+---------------------------+
| R-01 | Complejidad multi-tenant en       | Media    | Critico  | POC aislado antes de      |
|      | Prisma con BD dinamicas           |          |          | integrar. Schema por      |
|      |                                   |          |          | tenant desde el inicio.   |
+------+-----------------------------------+----------+----------+---------------------------+
| R-02 | Pipeline CI demasiado lento       | Baja     | Medio    | Paralelizar jobs,         |
|      | en fases posteriores              |          |          | cache de dependencias.    |
+------+-----------------------------------+----------+----------+---------------------------+
```

### Fase 1: Fundacion y Core

```
+------+-----------------------------------+----------+----------+---------------------------+
| ID   | Riesgo                            | Prob.    | Impacto  | Mitigacion                |
+------+-----------------------------------+----------+----------+---------------------------+
| R-03 | Saga de TenantProvisioning falla  | Media    | Critico  | Implementar compensacion  |
|      | dejando tenant en estado parcial  |          |          | y estado PROVISIONING.    |
+------+-----------------------------------+----------+----------+---------------------------+
| R-04 | Rendimiento de generacion masiva  | Media    | Alto     | Batch processing con      |
|      | de cargos (UC-019) no cumple      |          |          | chunks de 100. Benchmark  |
|      | RNFT-015 (<30s/1000 socios)       |          |          | temprano con datos reales.|
+------+-----------------------------------+----------+----------+---------------------------+
| R-05 | Modelo de dominio de Treasury     | Media    | Alto     | DDD event storming previo.|
|      | demasiado complejo para Fase 1    |          |          | Simplificar sin perder    |
|      |                                   |          |          | extensibilidad.           |
+------+-----------------------------------+----------+----------+---------------------------+
| R-06 | Wizard de alta de socio (front)   | Baja     | Medio    | Disenar wizard como       |
|      | con demasiadas validaciones       |          |          | componente stateful con   |
|      | cross-step                        |          |          | React Hook Form + Zod.    |
+------+-----------------------------------+----------+----------+---------------------------+
```

### Fase 2: Operativa Avanzada

```
+------+-----------------------------------+----------+----------+---------------------------+
| ID   | Riesgo                            | Prob.    | Impacto  | Mitigacion                |
+------+-----------------------------------+----------+----------+---------------------------+
| R-07 | Generacion de XML SEPA con        | Alta     | Critico  | Validar contra esquemas   |
|      | formatos pain.008 incorrectos     |          |          | XSD oficiales. Tests con  |
|      |                                   |          |          | ficheros reales de bancos.|
+------+-----------------------------------+----------+----------+---------------------------+
| R-08 | Importacion masiva (>500 rows)    | Media    | Alto     | Bull Queue con reintentos.|
|      | timeout o perdida de datos        |          |          | Transacciones por batch   |
|      |                                   |          |          | de 100 registros.         |
+------+-----------------------------------+----------+----------+---------------------------+
| R-09 | Parser pain.002 para              | Media    | Alto     | Parsers especificos por   |
|      | devoluciones con variantes        |          |          | banco. Fallback manual    |
|      | entre bancos espanoles            |          |          | con UI de reconciliacion. |
+------+-----------------------------------+----------+----------+---------------------------+
| R-10 | Modelo RBAC demasiado granular    | Baja     | Medio    | Empezar con roles fijos   |
|      | para las necesidades reales       |          |          | (Presidente, Secretario,  |
|      |                                   |          |          | Tesorero, Socio). Permisos|
|      |                                   |          |          | custom en iteracion futura|
+------+-----------------------------------+----------+----------+---------------------------+
```

### Fase 3: Reporting y Cierre

```
+------+-----------------------------------+----------+----------+---------------------------+
| ID   | Riesgo                            | Prob.    | Impacto  | Mitigacion                |
+------+-----------------------------------+----------+----------+---------------------------+
| R-11 | Dashboard lento por queries       | Media    | Medio    | Redis cache con TTL 5min. |
|      | agregadas sobre multiples tablas   |          |          | Read models pre-calculados|
|      |                                   |          |          | via CQRS (ADR-004).      |
+------+-----------------------------------+----------+----------+---------------------------+
| R-12 | Graficos de evolucion requieren   | Baja     | Bajo     | Series temporales basicas |
|      | datos historicos que no existen    |          |          | desde INSERT-only audit   |
|      | en MVP temprano                   |          |          | trail. Seed de datos demo.|
+------+-----------------------------------+----------+----------+---------------------------+
```

---

## 10. Criterios de Exito del MVP

### 10.1 Criterios funcionales

| ID    | Criterio                                                        | Metrica                        |
|-------|-----------------------------------------------------------------|--------------------------------|
| CE-01 | Un superadmin puede provisionar un tenant completo              | Tenant operativo en <2 min     |
| CE-02 | Login multi-tenant funcional con JWT                            | Auth <500ms, rate limiting OK  |
| CE-03 | Alta de socio end-to-end con cuota asignada                     | Wizard completado sin errores  |
| CE-04 | Generacion masiva de cargos para 1000 socios                    | <30 segundos (RNFT-015)        |
| CE-05 | Registro de cobro con recibo generado                           | PDF generado correctamente     |
| CE-06 | Remesa SEPA generada en formato pain.008 valido                 | Validacion contra XSD oficial  |
| CE-07 | Importacion de 500 socios via CSV                               | Proceso completo sin perdida   |
| CE-08 | Dashboard con KPIs actualizados                                 | Carga <2 segundos (RNFT-017)   |

### 10.2 Criterios no funcionales

| ID    | Criterio                                                        | Metrica                        |
|-------|-----------------------------------------------------------------|--------------------------------|
| CE-09 | Aislamiento de datos entre tenants                              | 0 fugas cross-tenant en tests  |
| CE-10 | Datos sensibles cifrados (IBAN, DNI)                            | AES-256-GCM verificado         |
| CE-11 | Auditoria INSERT-only funcional                                 | Historial completo trazable    |
| CE-12 | Pipeline CI verde con quality gates                             | Lint + tests + build OK        |
| CE-13 | Cobertura de tests unitarios                                    | >80% en logica de dominio      |
| CE-14 | Tests E2E de flujos criticos                                    | Login, alta, cobro, SEPA       |

### 10.3 Criterios de negocio

| ID    | Criterio                                                        | Metrica                        |
|-------|-----------------------------------------------------------------|--------------------------------|
| CE-15 | Una colectividad puede operar su ciclo completo                 | Alta -> cuotas -> cobro -> SEPA|
| CE-16 | Migracion desde sistema legacy posible                          | CSV importado correctamente    |
| CE-17 | Roles diferenciados operativos                                  | Presidente, Secretario,        |
|       |                                                                 | Tesorero con permisos distintos|
| CE-18 | Sistema usable sin formacion tecnica                            | UX validada con usuario real   |

---

## Anexo A: Mapeo Fases - Capas - UCs

```
+----------+----------+----------------------------------------------------------+
| Fase     | Capas    | UCs                                                      |
+----------+----------+----------------------------------------------------------+
| Fase 0   | (infra)  | Scaffold: Docker, CI, Prisma, NestJS bootstrap           |
+----------+----------+----------------------------------------------------------+
| Fase 1   | 0, 1, 2, | UC-001, UC-002, UC-008, UC-010, UC-007, UC-006,         |
|          | 3, 4     | UC-011, UC-013, UC-017, UC-018, UC-019, UC-021           |
+----------+----------+----------------------------------------------------------+
| Fase 2   | 1, 4, 5, | UC-004, UC-020, UC-056, UC-023, UC-024                   |
|          | 6, 7     |                                                          |
+----------+----------+----------------------------------------------------------+
| Fase 3   | 7        | UC-064, UC-065                                           |
+----------+----------+----------------------------------------------------------+
```

---

## Anexo B: Actores del MVP

```
+----------------+--------------------------------------------------+------------------+
| Actor          | Responsabilidades en el MVP                      | UCs principales  |
+----------------+--------------------------------------------------+------------------+
| Superadmin     | Provision de tenants                             | UC-001           |
| Presidente     | Config. tipos, planes, roles, SEPA               | UC-008, UC-017,  |
|                |                                                  | UC-004, UC-023   |
| Secretario     | Alta/baja socios, ficha, importacion             | UC-011, UC-013,  |
|                |                                                  | UC-006, UC-056   |
| Tesorero       | Cargos, cobros, remesas, devoluciones            | UC-019, UC-020,  |
|                |                                                  | UC-021, UC-023,  |
|                |                                                  | UC-024           |
| Member (Socio) | Consulta ficha, estado, recibos                  | UC-006, UC-021   |
| Sistema        | Generacion automatica, transiciones              | UC-019, UC-007   |
+----------------+--------------------------------------------------+------------------+
```
