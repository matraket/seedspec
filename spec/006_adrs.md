# Architectural Decision Records (ADRs)

**Proyecto:** Associated - ERP Ligero para Colectividades Españolas  
**Versión:** 1.1  
**Fecha:** Abril 2026  
**Inputs:** KB-004 (RNF Base), KB-005 (Modelo de Dominio)  
**Estado:** Verificado  
**Total ADRs:** 14

---

## Índice

- [Architectural Decision Records (ADRs)](#architectural-decision-records-adrs)
  - [Índice](#índice)
  - [ADR-001: Arquitectura General - Monolito Modular](#adr-001-arquitectura-general---monolito-modular)
    - [Estado](#estado)
    - [Contexto](#contexto)
    - [Decisión](#decisión)
    - [Consecuencias](#consecuencias)
    - [Trazabilidad](#trazabilidad)
  - [ADR-002: Estrategia Multi-Tenant por Base de Datos](#adr-002-estrategia-multi-tenant-por-base-de-datos)
    - [Estado](#estado-1)
    - [Contexto](#contexto-1)
    - [Decisión](#decisión-1)
    - [Consecuencias](#consecuencias-1)
    - [Trazabilidad](#trazabilidad-1)
  - [ADR-003: Estructura de Módulos por Bounded Context](#adr-003-estructura-de-módulos-por-bounded-context)
    - [Estado](#estado-2)
    - [Contexto](#contexto-2)
    - [Decisión](#decisión-2)
    - [Consecuencias](#consecuencias-2)
    - [Trazabilidad](#trazabilidad-2)
  - [ADR-004: Comunicación entre Bounded Contexts](#adr-004-comunicación-entre-bounded-contexts)
    - [Estado](#estado-3)
    - [Contexto](#contexto-3)
    - [Decisión](#decisión-3)
    - [Consecuencias](#consecuencias-3)
    - [Trazabilidad](#trazabilidad-3)
  - [ADR-005: Persistencia - Base de Datos Relacional](#adr-005-persistencia---base-de-datos-relacional)
    - [Estado](#estado-4)
    - [Contexto](#contexto-4)
    - [Decisión](#decisión-4)
    - [Consecuencias](#consecuencias-4)
    - [Trazabilidad](#trazabilidad-4)
  - [ADR-006: Estrategia de Autenticación](#adr-006-estrategia-de-autenticación)
    - [Estado](#estado-5)
    - [Contexto](#contexto-5)
    - [Decisión](#decisión-5)
    - [Consecuencias](#consecuencias-5)
    - [Trazabilidad](#trazabilidad-5)
  - [ADR-007: Autorización RBAC con Permisos Granulares](#adr-007-autorización-rbac-con-permisos-granulares)
    - [Estado](#estado-6)
    - [Contexto](#contexto-6)
    - [Decisión](#decisión-6)
    - [Consecuencias](#consecuencias-6)
    - [Trazabilidad](#trazabilidad-6)
  - [ADR-008: Estrategia de Eventos: Domain Events y Integration Events](#adr-008-estrategia-de-eventos-domain-events-y-integration-events)
    - [Estado](#estado-7)
    - [Contexto](#contexto-7)
    - [Decisión](#decisión-7)
    - [Consecuencias](#consecuencias-7)
    - [Trazabilidad](#trazabilidad-7)
  - [ADR-009: Arquitectura de Capas por Módulo](#adr-009-arquitectura-de-capas-por-módulo)
    - [Estado](#estado-8)
    - [Contexto](#contexto-8)
    - [Decisión](#decisión-8)
    - [Consecuencias](#consecuencias-8)
    - [Trazabilidad](#trazabilidad-8)
  - [ADR-010: API REST como Interfaz Principal](#adr-010-api-rest-como-interfaz-principal)
    - [Estado](#estado-9)
    - [Contexto](#contexto-9)
    - [Decisión](#decisión-9)
    - [Consecuencias](#consecuencias-9)
    - [Trazabilidad](#trazabilidad-9)
  - [ADR-011: Almacenamiento de Ficheros](#adr-011-almacenamiento-de-ficheros)
    - [Estado](#estado-10)
    - [Contexto](#contexto-10)
    - [Decisión](#decisión-10)
    - [Consecuencias](#consecuencias-10)
    - [Trazabilidad](#trazabilidad-10)
  - [ADR-012: Estrategia de Testing](#adr-012-estrategia-de-testing)
    - [Estado](#estado-11)
    - [Contexto](#contexto-11)
    - [Decisión](#decisión-11)
    - [Consecuencias](#consecuencias-11)
    - [Trazabilidad](#trazabilidad-11)
  - [ADR-013: Transición SUSPENDED → NONPAYMENT\_LEAVE](#adr-013-transición-suspended--nonpayment_leave)
    - [Estado](#estado-12)
    - [Contexto](#contexto-12)
    - [Decisión](#decisión-12)
    - [Consecuencias](#consecuencias-12)
    - [Trazabilidad](#trazabilidad-12)
  - [ADR-014: Blacklist de Access Tokens en Redis](#adr-014-blacklist-de-access-tokens-en-redis)
    - [Estado](#estado-13)
    - [Contexto](#contexto-13)
    - [Decisión](#decisión-13)
    - [Consecuencias](#consecuencias-13)
    - [Trazabilidad](#trazabilidad-13)
  - [Trazabilidad General](#trazabilidad-general)
    - [Matriz ADR → RNF](#matriz-adr--rnf)
    - [Matriz ADR → BC](#matriz-adr--bc)
  - [Changelog](#changelog)

---

## ADR-001: Arquitectura General - Monolito Modular

### Estado

**Aceptado**

### Contexto

El proyecto Associated es un TFM con restricciones de tiempo y recursos. Según KB-001, el objetivo es demostrar Clean Architecture y DDD sin sobre-ingeniería, priorizando un monolito modular bien estructurado sobre arquitecturas empresariales innecesarias para un MVP.

El equipo de desarrollo es de 1 persona. El público objetivo son asociaciones pequeñas/medianas (50-500 socios) con volúmenes de datos moderados.

**Alternativas consideradas:**

1. **Microservicios**: Alta complejidad operacional, overhead de comunicación, requiere equipo DevOps
2. **Monolito tradicional**: Simple pero difícil de evolucionar, acoplamiento alto
3. **Monolito modular**: Estructura clara, baja complejidad operacional, evolución a microservicios posible

### Decisión

**Adoptamos arquitectura de Monolito Modular** donde cada Bounded Context (KB-005) se implementa como un módulo independiente dentro de una única aplicación desplegable.

```
┌─────────────────────────────────────────────────────────────┐
│                      ASSOCIATED APP                         │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐        │
│  │ Identity │ │Membership│ │ Treasury │ │  Events  │        │
│  │  Module  │ │  Module  │ │  Module  │ │  Module  │        │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘        │
│  ┌──────────┐ ┌──────────┐                                  │
│  │Communic. │ │Documents │                                  │
│  │  Module  │ │  Module  │                                  │
│  └──────────┘ └──────────┘                                  │
├─────────────────────────────────────────────────────────────┤
│                    Shared Kernel                            │
├─────────────────────────────────────────────────────────────┤
│                   Infrastructure                            │
└─────────────────────────────────────────────────────────────┘
```

### Consecuencias

**Positivas:**

- Despliegue simple (un único artefacto)
- Debugging y tracing simplificado
- Transacciones locales entre módulos si es necesario
- Menor latencia en comunicación entre BCs
- Evolución gradual a microservicios posible (módulos ya aislados)
- Adecuado para equipo pequeño y MVP

**Negativas:**

- Escalado solo vertical (toda la app) o replicación completa
- Riesgo de acoplamiento si no se mantienen límites estrictos
- Fallo en un módulo puede afectar a toda la aplicación
- Un único stack tecnológico para todos los módulos

**Mitigaciones:**

- Reglas de arquitectura automatizadas (ArchUnit o similar)
- Comunicación entre módulos solo vía interfaces públicas
- Revisión periódica de dependencias entre módulos

### Trazabilidad

| Referencia | Descripción                                                      |
| ---------- | ---------------------------------------------------------------- |
| KB-001     | "Priorizando un monolito modular bien estructurado"              |
| RNF-020    | Escalabilidad horizontal (aplicación stateless permite réplicas) |
| RNF-057    | Documentación actualizada de arquitectura                        |

---

## ADR-002: Estrategia Multi-Tenant por Base de Datos

### Estado

**Aceptado**

### Contexto

Associated es una aplicación SaaS donde múltiples colectividades (tenants) comparten la infraestructura. Según RNF-004, se requiere aislamiento completo de datos entre tenants aplicando el principio de mínimo privilegio.

**Alternativas consideradas:**

| Estrategia             | Aislamiento | Complejidad    | Coste     | Escalabilidad |
| ---------------------- | ----------- | -------------- | --------- | ------------- |
| Columna discriminadora | Bajo        | Baja           | Bajo      | Alta          |
| Schema por tenant      | Medio       | Media          | Medio     | Media         |
| **BD por tenant**      | **Alto**    | **Media-Alta** | **Medio** | **Media**     |
| Instancia por tenant   | Muy alto    | Alta           | Alto      | Baja          |

### Decisión

**Adoptamos base de datos separada por tenant** con usuario de conexión específico por tenant, cumpliendo el principio de mínimo privilegio a nivel de conexión.

```
┌─────────────────────────────────────────────────────────────┐
│                    ARQUITECTURA DATOS                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   ┌─────────────┐                                           │
│   │   DB-Main   │  ← Usuarios, Tenants, Membresías          │
│   │ (Identity)  │    Acceso: user_main (solo esta BD)       │
│   └─────────────┘                                           │
│                                                             │
│   ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│   │ DB-Tenant-1 │  │ DB-Tenant-2 │  │ DB-Tenant-N │         │
│   │ user_t1     │  │ user_t2     │  │ user_tN     │         │
│   └─────────────┘  └─────────────┘  └─────────────┘         │
│                                                             │
│   Cada user_tX solo tiene permisos sobre su BD              │
│   No existe usuario con acceso a múltiples DBs de tenant    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Ubicación de datos por BC (según KB-005 §10.2):**

| BC               | Base de Datos | Justificación                               |
| ---------------- | ------------- | ------------------------------------------- |
| BC-Identity      | DB-Main       | Cross-tenant: usuarios, tenants, membresías |
| BC-Membership    | DB-TenantX    | Datos aislados por entidad                  |
| BC-Treasury      | DB-TenantX    | Datos aislados por entidad                  |
| BC-Events        | DB-TenantX    | Datos aislados por entidad                  |
| BC-Communication | DB-TenantX    | Datos aislados por entidad                  |
| BC-Documents     | DB-TenantX    | Datos aislados + storage ficheros           |

### Consecuencias

**Positivas:**

- Aislamiento completo: imposible acceder a datos de otro tenant por error de código
- Cumplimiento RGPD simplificado: backup/restore/delete por tenant
- Rendimiento predecible: sin "vecinos ruidosos"
- Migraciones independientes por tenant si necesario
- Auditoría clara: cada conexión identificable

**Negativas:**

- Más conexiones a gestionar (pool por tenant)
- Provisioning más complejo (crear BD + usuario + permisos)
- Consultas cross-tenant requieren lógica especial (solo en DB-Main)
- Mayor consumo de recursos (conexiones, memoria)

**Mitigaciones:**

- Connection pooling con límites por tenant
- Scripts automatizados de provisioning de tenants
- Monitorización de conexiones activas
- DB-Main como única fuente de verdad cross-tenant

### Trazabilidad

| Referencia               | Descripción                                                   |
| ------------------------ | ------------------------------------------------------------- |
| RNF-004                  | "Base de datos independiente, usuario de conexión específico" |
| RNF-038                  | Backups independientes por tenant                             |
| RNF-029                  | Derecho de supresión RGPD (borrado de BD completa)            |
| N2RF01                   | Aislamiento multi-tenant                                      |
| KB-005 §10               | Consideraciones multi-tenant                                  |
| KB-012 (Modelo de Datos) | ENT-001..006 (DB-Main), ENT-007..041 (DB-Tenant)              |

---

## ADR-003: Estructura de Módulos por Bounded Context

### Estado

**Aceptado**

### Contexto

Con 6 Bounded Contexts identificados (KB-005), necesitamos una estructura de código que mantenga el aislamiento lógico, facilite la navegación y permita evolución independiente de cada módulo.

### Decisión

**Adoptamos estructura de carpetas por Bounded Context** con cada módulo autocontenido siguiendo la misma estructura interna.

```
src/
├── Shared/                          # Shared Kernel
│   ├── Domain/
│   │   ├── ValueObjects/            # MemberId, TenantId, Money...
│   │   ├── Events/                  # DomainEvent base
│   │   └── Interfaces/              # AggregateRoot, Repository...
│   └── Infrastructure/
│       └── Persistence/             # Base classes
│
├── identity/                        # BC-Identity (Generic)
│   ├── Application/
│   ├── Domain/
│   └── Infrastructure/
│
├── membership/                      # BC-Membership (Core)
│   ├── Application/
│   ├── Domain/
│   └── Infrastructure/
│
├── treasury/                        # BC-Treasury (Core)
│   ├── Application/
│   ├── Domain/
│   └── Infrastructure/
│
├── events/                          # BC-Events (Core)
│   ├── Application/
│   ├── Domain/
│   └── Infrastructure/
│
├── communication/                   # BC-Communication (Supporting)
│   ├── Application/
│   ├── Domain/
│   └── Infrastructure/
│
└── documents/                       # BC-Documents (Supporting)
    ├── Application/
    ├── Domain/
    └── Infrastructure/
```

**Reglas de dependencia:**

1. Módulos solo dependen de `Shared`
2. Módulos NO dependen directamente de otros módulos
3. Comunicación entre módulos vía Domain Events o interfaces en `Shared`
4. Cada módulo tiene su propio conjunto de tests

### Consecuencias

**Positivas:**

- Navegación intuitiva del código
- Límites de BC claramente visibles
- Facilita asignación de responsabilidades
- Preparado para extracción a microservicio
- Tests aislados por módulo

**Negativas:**

- Posible duplicación de código entre módulos
- Requiere disciplina para no crear dependencias directas
- Shared Kernel puede crecer demasiado si no se controla

### Trazabilidad

| Referencia  | Descripción                         |
| ----------- | ----------------------------------- |
| KB-005 §2   | 6 Bounded Contexts identificados    |
| KB-005 §9.3 | Shared Kernel definido              |
| RNF-057     | Estructura del proyecto documentada |

---

## ADR-004: Comunicación entre Bounded Contexts

### Estado

**Aceptado**

### Contexto

Según el Context Map (KB-005 §9), los BCs necesitan comunicarse para mantener consistencia eventual. Por ejemplo, cuando se registra un socio (BC-Membership), BC-Treasury debe generar su cuota inicial y BC-Communication debe enviar email de bienvenida.

La comunicación entre BCs tiene dos naturalezas distintas que requieren mecanismos diferentes:

- **Comunicación intra-BC**: lógica de negocio que ocurre dentro del mismo Bounded Context (auditoría, efectos secundarios locales). No necesita entrega garantizada ni consumers.
- **Comunicación cross-BC**: coordinación entre Bounded Contexts distintos que requiere consistencia eventual y entrega garantizada.

**Alternativas consideradas para comunicación cross-BC:**

1. **Llamadas síncronas directas**: Simple pero acopla módulos
2. **Domain Events in-process con dispatcher síncrono**: Desacoplado pero sin garantía de entrega — rechazado para producción
3. **Outbox Pattern + OutboxProcessor**: Garantía de entrega at-least-once, desacoplamiento real
4. **Message Broker externo**: Máximo desacoplamiento, complejidad operacional — diferido post-MVP

### Decisión

**Adoptamos CQRS con @nestjs/cqrs** para Commands y Queries dentro de cada BC.

Para la comunicación entre BCs, adoptamos **dos tipos de eventos con mecanismos distintos**:

**1. Domain Events (intra-BC)** — registros de auditoría write-only en tenant DB. Sin despacho, sin consumers. Toda la lógica intra-BC se ejecuta directamente en el command handler que produce el evento. No hay dispatcher in-process.

**2. Integration Events (cross-BC)** — Outbox Pattern en main DB. Un único `OutboxProcessor` lee de main DB y despacha a `@EventsHandler` en los BCs destino.

```
┌─────────────────────────────────────────────────────────────────┐
│                  FLUJO DE INTEGRATION EVENTS                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  BC-Membership                                                  │
│  ┌──────────────────┐     ┌────────────────────────────────┐    │
│  │  CommandHandler  │────►│ IntegrationEventPublisher      │    │
│  │ (RegisterMember) │     │ (shared/, escribe main DB)     │    │
│  └──────────────────┘     └────────────┬───────────────────┘    │
│                                        │                        │
│                            main DB outbox_event (pending)       │
│                                        │                        │
│                           ┌────────────▼───────────────────┐    │
│                           │      OutboxProcessor           │    │
│                           │  (polling 5s, batch 50)        │    │
│                           └────────────┬───────────────────┘    │
│                                        │ EventBus.publish()     │
│                          ┌─────────────┴──────────────┐         │
│                          │                            │         │
│                ┌─────────▼──────────┐   ┌────────────▼───────┐  │
│                │  @EventsHandler    │   │  @EventsHandler    │  │
│                │  BC-Treasury       │   │  BC-Communication  │  │
│                └────────────────────┘   └────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Integration Events clave identificados (KB-005):**

| Evento                  | Productor     | Consumidores                    | Tipo              |
| ----------------------- | ------------- | ------------------------------- | ----------------- |
| `MemberRegistered`      | BC-Membership | BC-Treasury, BC-Communication   | Integration Event |
| `MemberDeactivated`     | BC-Membership | BC-Treasury, BC-Communication   | Integration Event |
| `PaymentRecorded`       | BC-Treasury   | BC-Membership                   | Integration Event |
| `PaymentReturned`       | BC-Treasury   | BC-Membership, BC-Communication | Integration Event |
| `DelinquencyDetected`   | BC-Treasury   | BC-Communication                | Integration Event |
| `RegistrationCompleted` | BC-Events     | BC-Treasury, BC-Communication   | Integration Event |
| `EventPublished`        | BC-Events     | BC-Communication                | Integration Event |

**Nota sobre eventos auto-consumidos (Treasury):** Los eventos `SepaMandateRegistered` y `SubscriptionCreated` que anteriormente se auto-consumían dentro de BC-Treasury ya no se despachan como eventos. La lógica intra-BC que antes ejecutaban sus handlers ahora se ejecuta directamente en los command handlers correspondientes.

### Consecuencias

**Positivas:**

- Desacoplamiento real entre módulos (producers no conocen consumers)
- Garantía de entrega at-least-once para Integration Events via Outbox
- Domain Events como log de auditoría inmutable en tenant DB
- Extensibilidad: añadir handlers sin modificar productor
- Testabilidad: handlers testeables de forma aislada
- Un único `IntegrationEventPublisher` compartido en `shared/` — todos los BCs inyectan el mismo

**Negativas:**

- Consistencia eventual (no transaccional entre BCs)
- Latencia: Integration Events no son instantáneos (procesador batch, polling 5s)
- Debugging más complejo (flujo indirecto via OutboxProcessor)

**Evolución futura:**

- Message broker externo (Redis Streams, Kafka) si se extraen microservicios — sin cambios en producers/consumers gracias al puerto `IntegrationEventPublisher`

### Trazabilidad

| Referencia              | Descripción                                       |
| ----------------------- | ------------------------------------------------- |
| KB-005 §9               | Context Map con relaciones Pub/Sub                |
| KB-005 §3.4, §4.4, §5.3 | Eventos por BC                                    |
| ADR-008                 | Dual-outbox: main DB (Integration) + tenant DB (Domain) |
| RNF-067                 | Entrega garantizada de Integration Events         |

---

## ADR-005: Persistencia - Base de Datos Relacional

### Estado

**Aceptado**

### Contexto

El dominio de Associated tiene:

- Entidades con relaciones complejas (Member → Charges → Payments)
- Necesidad de consultas ad-hoc (reporting, filtrados)
- Requisitos ACID para operaciones financieras (BC-Treasury)
- Volumen de datos moderado (cientos/miles de registros por tenant)

**Alternativas consideradas:**

1. **PostgreSQL**: Robusto, JSON support, extensiones, open source
2. **MySQL/MariaDB**: Popular, buen rendimiento, fácil hosting
3. **SQLite**: Ultra-simple, single-file, limitaciones concurrencia
4. **MongoDB**: Flexible schema, no ideal para relaciones complejas

### Decisión

**Adoptamos PostgreSQL** como sistema de base de datos relacional.

**Justificación:**

- Excelente soporte para JSON (campos semi-estructurados como CamposPersonalizados)
- Extensiones útiles (pg_trgm para búsqueda fuzzy, uuid-ossp)
- Robusto en operaciones ACID
- Buen ecosistema de herramientas (pgAdmin, migraciones)
- Disponible en todos los cloud providers
- Licencia permisiva (PostgreSQL License)

### Consecuencias

**Positivas:**

- Consistencia transaccional garantizada
- SQL estándar para consultas complejas
- Herramientas maduras de backup/restore
- Migraciones versionadas con herramientas estándar
- JSON columns para flexibilidad donde se necesite

**Negativas:**

- Requiere servidor dedicado (vs SQLite embedded)
- Schema migrations necesarias para cambios
- Posible overhead para datos muy simples

### Trazabilidad

| Referencia               | Descripción                                                |
| ------------------------ | ---------------------------------------------------------- |
| RNF-004                  | Multi-tenant por BD (PostgreSQL soporta múltiples DBs)     |
| RNF-038                  | Backups con pg_dump nativo                                 |
| RNF-066                  | Migraciones con herramientas estándar                      |
| N4RF17-23                | Operaciones SEPA requieren ACID                            |
| KB-012 (Modelo de Datos) | Todas las entidades ENT-001..040 definen el esquema Prisma |

---

## ADR-006: Estrategia de Autenticación

### Estado

**Aceptado**

### Contexto

Los usuarios pueden pertenecer a múltiples tenants (N2RF02). La autenticación debe ser segura, soportar múltiples métodos (RNF-001), y permitir acceso unificado.

**Alternativas consideradas:**

1. **JWT stateless**: Escalable, pero difícil invalidación
2. **Sessions + cookies**: Tradicional, fácil invalidación, stateful
3. **OAuth2/OIDC con IdP externo**: Delegado, complejo para MVP
4. **JWT + refresh tokens + blacklist**: Balance entre escalabilidad e invalidación

### Decisión

**Adoptamos JWT con refresh tokens** almacenando sesiones activas en base de datos para permitir invalidación.

```
┌─────────────────────────────────────────────────────────────┐
│                    FLUJO AUTENTICACIÓN                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. Login (email + password)                                │
│     └─► Validar credenciales en DB-Main                     │
│     └─► Generar access_token (JWT, 15min) + refresh_token   │
│     └─► Guardar sesión en DB (para invalidación)            │
│                                                             │
│  2. Request autenticado                                     │
│     └─► Validar JWT signature + expiration                  │
│     └─► Extraer tenantId del token (o header)               │
│     └─► Verificar blacklist Redis (no revocado — ADR-014)   │
│                                                             │
│  3. Refresh token                                           │
│     └─► Validar refresh_token en DB                         │
│     └─► Generar nuevo access_token                          │
│                                                             │
│  4. Logout                                                  │
│     └─► Invalidar sesión en DB + blacklist Redis (ADR-014)  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Claims del JWT:**

```json
{
  "jti": "uuid-v4",
  "sub": "usuario-uuid",
  "email": "user@example.com",
  "tenants": ["tenant-1-uuid", "tenant-2-uuid"],
  "current_tenant": "tenant-1-uuid",
  "roles": ["TESORERO"],
  "exp": 1234567890,
  "iat": 1234567890
}
```

### Consecuencias

**Positivas:**

- Stateless para la mayoría de requests (JWT válido = acceso)
- Invalidación posible vía blacklist/check de sesión (ver ADR-014: a partir de esa decisión, la verificación de blacklist es obligatoria en cada request autenticado)
- Soporte multi-tenant nativo en claims
- Escalable (no requiere sesión en servidor para cada request)

**Negativas:**

- Complejidad adicional vs sessions tradicionales
- Tokens pueden ser usados hasta expiración si no se verifica blacklist (ver ADR-014: la blacklist es ahora obligatoria, eliminando este gap)
- Refresh token requiere almacenamiento seguro en cliente

**Métodos de autenticación soportados:**

1. Email + contraseña (RNF-001)
2. Magic link por email (RNF-001)
3. (Futuro) OAuth2 con Google/Microsoft

### Trazabilidad

| Referencia | Descripción                             |
| ---------- | --------------------------------------- |
| RNF-001    | Autenticación segura, múltiples métodos |
| RNF-002    | Gestión de sesiones, expiración         |
| RNF-005    | Tokens transmitidos sobre HTTPS         |
| N2RF02     | Acceso unificado multi-entidad          |
| ADR-014    | Blacklist de access tokens — complementa invalidación post-logout |

---

## ADR-007: Autorización RBAC con Permisos Granulares

### Estado

**Aceptado**

### Contexto

Cada tenant tiene roles diferenciados (Presidente, Secretario, Tesorero, Vocal, Socio) con permisos específicos (RNF-003). Los permisos deben ser verificables en cada operación.

### Decisión

**Adoptamos RBAC (Role-Based Access Control)** con permisos granulares definidos por operación.

```
┌─────────────────────────────────────────────────────────────┐
│                    MODELO RBAC                              │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  User ──(1:N)──► TenantMembership ──(N:1)──► Role           │
│                           │                      │          │
│                           │                      ▼          │
│                      ┌────┴────┐           ┌─────────┐      │
│                      │ Tenant  │           │Permisos │      │
│                      │ Context │           │  [ ]    │      │
│                      └─────────┘           └─────────┘      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Roles predefinidos (KB-005 §8.3):**

| Rol          | Scope               | Permisos Típicos                      |
| ------------ | ------------------- | ------------------------------------- |
| PRESIDENT    | Global tenant       | Todo + aprobaciones críticas          |
| SECRETARY    | Members, Documents  | CRUD members, actas, comunicaciones   |
| TREASURER    | Economía            | Cuotas, cobros, remesas, contabilidad |
| BOARD_MEMBER | Configurable        | Según área asignada                   |
| MEMBER       | Solo lectura propia | Ver sus datos, eventos, pagos         |

**Nomenclatura de permisos:**

```
{module}:{resource}:{action}

Ejemplos:
- membership:members:create
- membership:members:read
- membership:members:update
- membership:members:delete
- treasury:remittances:generate
- treasury:payments:register
- events:events:publish
```

### Consecuencias

**Positivas:**

- Modelo claro y auditable
- Roles personalizables por tenant (además de los sistema)
- Verificación declarativa en handlers/controllers
- Extensible: añadir permisos sin cambiar estructura

**Negativas:**

- Número de permisos puede crecer significativamente
- Cache de permisos necesaria para rendimiento
- Complejidad en UI para gestionar roles personalizados

### Trazabilidad

| Referencia | Descripción                                |
| ---------- | ------------------------------------------ |
| RNF-003    | RBAC con granularidad a nivel de operación |
| RNF-013    | Principio de mínimo privilegio             |
| N2RF04-05  | Roles predefinidos y personalizables       |
| KB-005 §8  | BC-Identity con agregado Rol               |

---

## ADR-008: Estrategia de Eventos: Domain Events y Integration Events

### Estado

**Aceptado**

### Contexto

El sistema maneja dos tipos de eventos con propósitos y garantías distintas:

- **Comunicación intra-BC**: necesidades de auditoría y trazabilidad local dentro del mismo Bounded Context.
- **Comunicación cross-BC**: coordinación entre Bounded Contexts distintos que requiere entrega garantizada.

La arquitectura anterior usaba una única tabla `outbox_events` multi-tenant sin distinguir entre estos dos casos, lo que generaba acoplamiento innecesario y ausencia de garantías claras.

### Decisión

**Adoptamos un diseño dual-outbox** con dos tablas `outbox_event` con propósitos claramente diferenciados: una en main DB para Integration Events (cross-BC, procesada por OutboxProcessor) y otra en cada tenant DB para Domain Events (intra-BC, log de auditoría write-only).

```
┌─────────────────────────────────────────────────────────────────┐
│                     DISEÑO DUAL-OUTBOX                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  TENANT DB (por tenant)              MAIN DB (global)           │
│  ┌──────────────────────────┐        ┌──────────────────────┐   │
│  │  outbox_event            │        │  outbox_event        │   │
│  │  (Domain Events)         │        │  (Integration Events)│   │
│  │  Write-only audit log    │        │  + tenant_id         │   │
│  │  Sin status, sin retry   │        │  + status/retry      │   │
│  │  NO OutboxProcessor      │        │  Procesada por       │   │
│  └──────────────────────────┘        │  OutboxProcessor     │   │
│                                      └──────────┬───────────┘   │
│                                                 │               │
│                                    ┌────────────▼─────────────┐ │
│                                    │     OutboxProcessor      │ │
│                                    │  polling 5s, batch 50    │ │
│                                    │  mutex de concurrencia   │ │
│                                    └────────────┬─────────────┘ │
│                                                 │               │
│                              ┌──────────────────┴────────────┐  │
│                              │ @EventsHandler en BCs destino │  │
│                              └───────────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### Domain Events (intra-BC) — tenant DB

Registros de auditoría write-only. Se escriben en la misma transacción que la operación de dominio. No existe OutboxProcessor que los lea ni consumers. Toda la lógica intra-BC se ejecuta directamente en el command handler.

**Schema en tenant DB:**

```sql
CREATE TABLE outbox_event (
    id             UUID PRIMARY KEY,
    bounded_context VARCHAR(50)  NOT NULL,
    event_type      VARCHAR(100) NOT NULL,
    aggregate_id    UUID         NOT NULL,
    aggregate_type  VARCHAR(100) NOT NULL,
    payload         JSONB        NOT NULL,
    actor_id        UUID         NULL,
    occurred_at     TIMESTAMP    NOT NULL DEFAULT now()
);

-- Índices de consulta de auditoría
CREATE INDEX ON outbox_event (aggregate_id, occurred_at);
CREATE INDEX ON outbox_event (bounded_context);
CREATE INDEX ON outbox_event (occurred_at);
```

#### Integration Events (cross-BC) — main DB

Eventos que cruzan Bounded Contexts. Se persisten en main DB mediante `IntegrationEventPublisher` (dual-write best-effort: commit tenant DB primero, luego write main DB). Procesados por el `OutboxProcessor` único.

**Schema en main DB:**

```sql
CREATE TABLE outbox_event (
    id              UUID PRIMARY KEY,
    tenant_id       UUID         NULL,     -- NULL para eventos de BC-Identity
    bounded_context VARCHAR(50)  NOT NULL,
    event_type      VARCHAR(100) NOT NULL,
    aggregate_id    UUID         NOT NULL,
    aggregate_type  VARCHAR(100) NOT NULL,
    payload         JSONB        NOT NULL,
    actor_id        UUID         NULL,
    status          VARCHAR(20)  NOT NULL DEFAULT 'pending',
                                          -- pending | processing | processed | failed
    retry_count     INT          NOT NULL DEFAULT 0,
    max_retries     INT          NOT NULL DEFAULT 3,
    created_at      TIMESTAMP    NOT NULL DEFAULT now(),
    processed_at    TIMESTAMP    NULL
);

-- Índice principal de polling
CREATE INDEX ON outbox_event (status, created_at);
CREATE INDEX ON outbox_event (tenant_id);
CREATE INDEX ON outbox_event (bounded_context, status);
CREATE INDEX ON outbox_event (aggregate_id);
```

#### Publisher: IntegrationEventPublisher

Un único publisher compartido en `shared/`. Todos los BCs inyectan el mismo:

- **Puerto**: `shared/application/ports/integration-event.publisher.ts`
- **Implementación**: `shared/infrastructure/persistence/prisma-integration-event.publisher.ts`
- Escribe en main DB via `PrismaMainService`

#### OutboxProcessor

- **Polling**: cada 5s (configurable post-MVP)
- **Batch**: 50 eventos por tick
- **Concurrencia**: mutex/flag `isProcessing` — si el tick anterior no terminó, el siguiente no arranca
- **Stale recovery**: al arrancar, resetea eventos `processing` con más de 5 minutos a `pending`
- **Aislamiento de errores**: `try/catch` por evento individual — el fallo de un consumer no bloquea el tick
- **Flujo**: `SELECT pending → UPDATE processing → EventBus.publish() → UPDATE processed/failed`

#### Dual-write (MVP)

Best-effort: la operación de dominio en tenant DB hace commit primero; luego `IntegrationEventPublisher` escribe en main DB. El riesgo de inconsistencia es mínimo (mismo servidor PostgreSQL). Migración futura a CDC si escala.

#### Idempotencia

- **Default**: idempotencia natural por lógica de negocio (upserts, checks de existencia en el domain handler).
- **Fallback**: para acciones no-idempotentes (emails, PDFs, APIs externas), el consumer guarda `source_event_id` en su propia tabla para detectar duplicados.
- No existe tabla genérica `processed_event` — cada consumer resuelve con su lógica de dominio.

### Consecuencias

**Positivas:**

- Garantía de entrega at-least-once para Integration Events (via Outbox + OutboxProcessor)
- Domain Events como log de auditoría inmutable, bajo acoplamiento
- Un único OutboxProcessor — no hay loop multi-tenant
- Separación clara de propósitos: main DB para delivery, tenant DB para auditoría
- Publisher compartido: los BCs no conocen detalles de infraestructura

**Negativas:**

- Latencia: Integration Events no son instantáneos (polling 5s)
- Dual-write introduce ventana de inconsistencia pequeña (mitigada por same-server PostgreSQL)
- Complejidad añadida: dos schemas, OutboxProcessor, gestión de retry

### Trazabilidad

| Referencia  | Descripción                                          |
| ----------- | ---------------------------------------------------- |
| KB-005 §9.2 | Relaciones Pub/Sub entre BCs                         |
| ADR-004     | Decisión de CQRS + Integration Events cross-BC       |
| RNF-067     | Entrega garantizada de Integration Events            |
| RNF-007     | Auditoría (Domain Events como log inmutable)         |

---

## ADR-009: Arquitectura de Capas por Módulo

### Estado

**Aceptado**

### Contexto

Cada módulo/BC necesita una estructura interna que separe responsabilidades, facilite testing y mantenga el dominio aislado de detalles de infraestructura.

### Decisión

**Adoptamos Clean Architecture / Hexagonal** simplificada con 3 capas por módulo.

```
┌───────────────────────────────────────────────────────────┐
│                    CAPAS POR MÓDULO                       │
├───────────────────────────────────────────────────────────┤
│                                                           │
│  ┌────────────────────────────────────────────────────┐   │
│  │                 INFRASTRUCTURE                     │   │
│  │  ┌───────────┐  ┌───────────┐  ┌───────────┐       │   │
│  │  │Repository │  │  External │  │Controllers│       │   │
│  │  │   Impl    │  │  Services │  │           │       │   │
│  │  └───────────┘  └───────────┘  └───────────┘       │   │
│  └──────────────────────────┬─────────────────────────┘   │
│                             │ depende de                  │
│  ┌──────────────────────────▼─────────────────────────┐   │
│  │                  APPLICATION                       │   │
│  │  ┌───────────┐  ┌───────────┐  ┌───────────┐       │   │
│  │  │ Commands  │  │  Queries  │  │  DTOs     │       │   │
│  │  │ Handlers  │  │  Handlers │  │           │       │   │
│  │  └───────────┘  └───────────┘  └───────────┘       │   │
│  └──────────────────────────┬─────────────────────────┘   │
│                             │ depende de                  │
│  ┌──────────────────────────▼─────────────────────────┐   │
│  │                    DOMAIN                          │   │
│  │  ┌───────────┐  ┌───────────┐  ┌───────────┐       │   │
│  │  │Aggregates │  │  Domain   │  │  Value    │       │   │
│  │  │ Entities  │  │  Events   │  │  Objects  │       │   │
│  │  └───────────┘  └───────────┘  └───────────┘       │   │
│  │  ┌───────────┐  ┌───────────┐                      │   │
│  │  │Repository │  │  Domain   │  ← Interfaces/Ports  │   │
│  │  │Interfaces │  │ Services  │                      │   │
│  │  └───────────┘  └───────────┘                      │   │
│  └────────────────────────────────────────────────────┘   │
│                                                           │
└───────────────────────────────────────────────────────────┘
```

**Regla de dependencia (Dependency Rule):**

- Las dependencias apuntan hacia el centro: **Infrastructure → Application → Domain**
- **Domain** no conoce nada de Application ni Infrastructure
- **Application** no conoce nada de Infrastructure
- **Infrastructure** implementa las interfaces (ports) definidas en Domain
- La inversión de dependencias permite que el dominio defina contratos que la infraestructura cumple

### Consecuencias

**Positivas:**

- Dominio testeable sin infraestructura
- Facilita cambio de implementaciones (BD, servicios externos)
- Separación clara de responsabilidades
- Código de negocio aislado

**Negativas:**

- Más archivos/carpetas que arquitectura tradicional
- Indirección adicional (interfaces, mappers)
- Curva de aprendizaje inicial

### Trazabilidad

| Referencia  | Descripción                                            |
| ----------- | ------------------------------------------------------ |
| KB-001      | "Demostrar Clean Architecture y DDD"                   |
| RNF-058-060 | Testing por capas (unit → domain, integration → infra) |

---

## ADR-010: API REST como Interfaz Principal

### Estado

**Aceptado**

### Contexto

La aplicación necesita exponer funcionalidad a clientes web (SPA) y móvil (PWA/nativa). Se requiere una API consistente, documentada y segura.

### Decisión

**Adoptamos API REST** con OpenAPI spec como interfaz principal.

**Convenciones:**

| spec/013_inventario-de-endpoints.md | Inventario de endpoints EP-NNN que materializan las convenciones REST |

- Recursos en plural: `/api/v1/members`, `/api/v1/events`
- Versionado en URL: `/api/v1/...`
- Tenant en header: `X-Tenant-Id: {uuid}`
- Auth en header: `Authorization: Bearer {jwt}`
- Respuestas JSON con envelope consistente
- Códigos HTTP estándar

**Estructura de respuesta:**

```json
// Success
{
  "data": { ... },
  "meta": { "page": 1, "total": 100 }
}

// Error
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "El email no es válido",
    "details": [...]
  }
}
```

### Consecuencias

**Positivas:**

- Estándar ampliamente conocido
- Documentación auto-generada (OpenAPI/Swagger)
- Facilita testing con herramientas estándar
- Compatible con cualquier cliente HTTP

**Negativas:**

- Overfetching/underfetching (vs GraphQL)
- Múltiples requests para vistas complejas
- No ideal para real-time (complementar con WebSockets)

### Trazabilidad

| Referencia  | Descripción                   |
| ----------- | ----------------------------- |
| RNF-015-016 | Tiempos de respuesta medibles |
| RNF-057     | Documentación de API          |
| N10RF01-15  | Portal socio consume API      |

---

## ADR-011: Almacenamiento de Ficheros

### Estado

**Aceptado**

### Contexto

BC-Documents requiere almacenar ficheros (estatutos, actas, justificantes). Los ficheros deben estar aislados por tenant y ser accesibles de forma segura.

**Alternativas consideradas:**

1. **Base de datos (BLOB)**: Simple pero no escalable
2. **Sistema de ficheros local**: Simple, no distribuido
3. **Object Storage (S3-compatible)**: Escalable, estándar

### Decisión

**Adoptamos Object Storage S3-compatible** con estructura por tenant.

```
┌──────────────────────────────────────────────────────┐
│                 ESTRUCTURA STORAGE                   │
├──────────────────────────────────────────────────────┤
│                                                      │
│  bucket: associated-documents                        │
│  │                                                   │
│  ├── tenant-uuid-1/                                  │
│  │   ├── documentos/                                 │
│  │   │   ├── estatutos-2024.pdf                      │
│  │   │   └── ...                                     │
│  │   ├── actas/                                      │
│  │   └── justificantes/                              │
│  │                                                   │
│  ├── tenant-uuid-2/                                  │
│  │   └── ...                                         │
│  │                                                   │
│  └── tenant-uuid-N/                                  │
│                                                      │
└──────────────────────────────────────────────────────┘
```

**Acceso:**

- URLs pre-firmadas (presigned URLs) con expiración corta
- Nunca exponer bucket público
- Verificar permisos en aplicación antes de generar URL

### Consecuencias

**Positivas:**

- Escalable: sin límite práctico de almacenamiento
- Aislamiento: prefijos por tenant
- Portabilidad: S3-compatible funciona con múltiples providers
- Backup: versionado y lifecycle policies nativas

**Negativas:**

- Dependencia de servicio externo
- Costes adicionales por storage/transfer
- Complejidad vs sistema de ficheros local

### Trazabilidad

| Referencia | Descripción                           |
| ---------- | ------------------------------------- |
| RNF-009    | Ficheros seguros, validación de tipos |
| RNF-022    | Subida de ficheros con límites        |
| N7RF01-12  | Gestión documental                    |
| KB-005 §7  | BC-Documents                          |

---

## ADR-012: Estrategia de Testing

### Estado

**Aceptado**

### Contexto

RNF-058/059/060 definen una pirámide de testing con umbrales específicos. Necesitamos una estrategia que garantice calidad sin sacrificar velocidad de desarrollo.

### Decisión

**Adoptamos pirámide de testing** con distribución 70/20/10 y CI quality gates.

```
┌─────────────────────────────────────────────────────────────┐
│                 PIRÁMIDE DE TESTING                         │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│                      ┌───────┐                              │
│                      │  E2E  │  10% - <15min                │
│                      │ Tests │  Flujos críticos             │
│                    ┌─┴───────┴─┐                            │
│                    │Integration│  20% - <10min              │
│                    │   Tests   │  Repos, APIs               │
│                  ┌─┴───────────┴─┐                          │
│                  │  Unit Tests   │  70% - <2min             │
│                  │   (Domain)    │  Aggregates, VOs, Events │
│                  └───────────────┘                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**CI Quality Gates (RNF-058):**

| Métrica         | Global | Diff (PRs) |
| --------------- | ------ | ---------- |
| Line coverage   | ≥80%   | ≥85%       |
| Branch coverage | ≥70%   | ≥75%       |

**Exclusiones de cobertura:**

- DTOs/ViewModels
- Configuración
- Código generado
- Migraciones

**Tipos de test por capa:**

| Capa           | Tipo Test   | Qué testea                       | Mocks                    |
| -------------- | ----------- | -------------------------------- | ------------------------ |
| Domain         | Unit        | Aggregates, VOs, Domain Services | Ninguno                  |
| Application    | Unit        | Command/Query Handlers           | Repositories             |
| Infrastructure | Integration | Repository implementations       | DB real (testcontainers) |
| API            | Integration | Controllers, validaciones        | In-memory o DB           |
| E2E            | E2E         | Flujos completos                 | Ninguno                  |

### Consecuencias

**Positivas:**

- Feedback rápido (unit tests en <2min)
- Confianza en cambios (cobertura obligatoria)
- Regresiones detectadas temprano
- Documentación ejecutable

**Negativas:**

- Inversión inicial en setup de tests
- Mantenimiento de tests
- Posible falsa sensación de seguridad (cobertura ≠ calidad)

### Trazabilidad

| Referencia | Descripción                  |
| ---------- | ---------------------------- |
| RNF-058    | Unit tests, CI quality gates |
| RNF-059    | Integration tests            |
| RNF-060    | E2E tests                    |
| KB-001     | "Demostrar TDD"              |

---

## ADR-013: Transición SUSPENDED → NONPAYMENT_LEAVE

### Estado

**Aceptado**

### Contexto

UC-007 define la tabla de transiciones de estado de socio. Las transiciones documentadas desde SUSPENDED son:
- SUSPENDED → ACTIVE (rehabilitación por pago)
- SUSPENDED → DISCIPLINARY_LEAVE (baja disciplinaria)

Sin embargo, el StatusTransitionValidator del backend permite además SUSPENDED → NONPAYMENT_LEAVE, que no estaba en la spec original. Esta transición se descubrió durante el flujo SDD leave-flow-completion (marzo 2026).

El escenario de negocio es real: un socio suspendido por impago que acumula suficiente deuda debería poder transicionar directamente a baja por impago, sin necesidad de pasar primero por otro estado intermedio.

### Decisión

**Se acepta SUSPENDED → NONPAYMENT_LEAVE como transición válida.** Justificación:
- Semánticamente correcta: la suspensión por impago es el paso previo natural a la baja por impago
- Ya implementada y verificada en el backend (StatusTransitionValidator)
- El frontend (LeaveActions + NonpaymentLeavePage) ya la consume vía useAvailableTransitions
- Eliminarla obligaría a un workaround artificial (ej: reactivar → suspender → dar de baja)

Actualizar la tabla de transiciones de UC-007 para incluirla.

### Consecuencias

**Positivas:**

- Flujo de baja por impago directo desde suspensión, sin pasos intermedios artificiales
- Alineación entre spec, backend y frontend

**Negativas:**

- Amplía la superficie de transiciones de estado (una más que mantener)

### Trazabilidad

| Referencia               | Descripción                                                                           |
| ------------------------ | ------------------------------------------------------------------------------------- |
| UC-007                   | Tabla de transiciones de estado del socio                                             |
| UC-013                   | Baja de socio                                                                         |
| US-033                   | Baja por impago                                                                       |
| StatusTransitionValidator | `api/src/membership/domain/services/status-transition-validator.ts`                  |

---

## ADR-014: Blacklist de Access Tokens en Redis

### Estado

**Aceptado**

### Contexto

ADR-006 establece autenticación JWT stateless con access token de 15 minutos de TTL y refresh tokens almacenados en base de datos. El flujo de logout invalida la sesión en DB (refresh token), pero el access token sigue siendo válido hasta su expiración natural — un gap de seguridad de hasta 15 minutos donde un token robado o comprometido continúa otorgando acceso.

Redis 7.x ya forma parte del stack tecnológico (spec 007 §4.3) para caching de sesiones server-side (RNFT-002) y Bull Queue (RNFT-018), por lo que no introduce nueva infraestructura.

Se necesita invalidación inmediata del access token tras logout sin comprometer la naturaleza stateless del JWT para la mayoría de requests.

**Alternativas consideradas:**

1. **No hacer nada** — aceptar el gap de 15 minutos. Insuficiente para requisitos de seguridad (RNF-001, RNF-002)
2. **Reducir TTL del access token** (ej. 2 min) — mitiga pero no elimina el gap, incrementa significativamente los refresh requests y la carga sobre DB-Main
3. **Blacklist en Redis** — lookup O(1) por request, TTL nativo auto-limpia entradas expiradas, Redis ya está en el stack
4. **Blacklist en PostgreSQL** — funcional pero latencia mayor (~2-5ms vs <1ms), sin TTL nativo (requiere job de limpieza manual), tabla crece sin control si falla la limpieza

### Decisión

**Adoptamos blacklist de access tokens en Redis** usando el claim `jti` (JWT ID) como clave de invalidación.

**Estructura en Redis:**

```
Key:   blacklist:{jti}
Value: "1"
TTL:   token.exp - now()   (auto-expiración alineada con el token)
```

**Flujo de logout actualizado:**

```
┌─────────────────────────────────────────────────────────────┐
│                  FLUJO LOGOUT (actualizado)                 │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. Cliente envía POST /auth/logout                         │
│     (JWT en header, refreshToken en body)                   │
│     └─► Extraer JTI y EXP del token                         │
│     └─► Redis SET blacklist:{jti} "1" EX (exp - now)        │
│     └─► Invalidar refresh_token en DB-Main                  │
│     └─► Responder 204 No Content                            │
│                                                             │
│  2. Request autenticado (cadena de guards)                  │
│     └─► ThrottlerGuard (rate limiting)                      │
│     └─► JwtAuthGuard (validar firma + expiración)           │
│     └─► BlacklistCheck (Redis GET blacklist:{jti})          │
│         ├─► Key existe → 401 Unauthorized                   │
│         └─► Key no existe → continuar                       │
│     └─► PermissionsGuard (RBAC)                             │
│     └─► Handler                                             │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Excepción:** El endpoint de logout (`POST /auth/logout` — EP-005) está exento del BlacklistCheck. Justificación: (1) el usuario está creando una entrada en blacklist, no siendo verificado contra ella; (2) FE-4 de UC-002 requiere que el logout funcione en modo best-effort si Redis no está disponible; (3) JwtAuthGuard ya valida la firma del token.

**Comportamiento con Redis caído: fail-closed.** Si el BlacklistCheck no puede conectar con Redis para verificar la blacklist, el request se rechaza con 503 Service Unavailable. Justificación: seguridad > disponibilidad para flujos de autenticación. Un atacante no debe poder explotar una caída de Redis para usar tokens invalidados.

**Comportamiento con Redis caído durante logout: best-effort.** Si Redis no está disponible al momento del logout, el refresh token se revoca igualmente en DB-Main y se responde 204. El access token expirará naturalmente en ≤15 minutos. Se registra un log de warning. Ver UC-002 FE-4.

**Implementación del BlacklistCheck:**

- Puede integrarse dentro del `JwtAuthGuard` existente (tras validar firma) o como guard independiente en la cadena
- La decisión de guard único vs separado se deja al implementador; lo relevante es que el check ocurra DESPUÉS de validar firma y ANTES de verificar permisos

**Requisito sobre el claim `jti`:**

- Todo access token generado DEBE incluir el claim `jti` (UUID v4) — actualizar la generación de tokens de ADR-006 para garantizarlo

### Consecuencias

**Positivas:**

- Invalidación inmediata de access tokens tras logout (gap = 0)
- Auto-limpieza por TTL nativo de Redis: sin tablas que crezcan, sin jobs de mantenimiento
- Reutiliza infraestructura Redis existente (caching de sesiones, Bull Queue)
- Impacto de rendimiento mínimo: Redis GET < 1ms por request
- La mayoría de requests (tokens válidos, no en blacklist) siguen siendo stateless en la práctica — el check es un simple cache miss

**Negativas:**

- Introduce componente stateful en el flujo JWT, acotado a una operación Redis GET por request
- Dependencia de Redis para autenticación: si Redis cae, los requests autenticados fallan (fail-closed)
- Requiere que el claim `jti` esté presente en todos los access tokens (cambio en generación de tokens)

**Mitigaciones:**

- Redis en modo Sentinel o Cluster para alta disponibilidad
- Health check de Redis en el endpoint `/health` para monitorización proactiva
- Circuit breaker opcional entre el guard y Redis para degradación controlada (si se relaja fail-closed en el futuro)

### Trazabilidad

| Referencia | Descripción                                                         |
| ---------- | ------------------------------------------------------------------- |
| RNF-001    | Autenticación segura — invalidación inmediata cierra gap de 15 min  |
| RNF-002    | Gestión de sesiones — logout efectivo con blacklist                  |
| ADR-006    | Estrategia de autenticación JWT — este ADR complementa el flujo     |
| RNF-068    | Invalidación Inmediata de Access Tokens Post-Logout                 |
| UC-002     | Autenticación multi-tenant — flujo de logout (FA-4, FE-4, FE-5)    |

---

## Trazabilidad General

### Matriz ADR → RNF

| ADR     | RNFs Relacionados         |
| ------- | ------------------------- |
| ADR-001 | RNF-020, RNF-057          |
| ADR-002 | RNF-004, RNF-029, RNF-038 |
| ADR-003 | RNF-057                   |
| ADR-004 | RNF-067                   |
| ADR-005 | RNF-004, RNF-038, RNF-066 |
| ADR-006 | RNF-001, RNF-002, RNF-005 |
| ADR-007 | RNF-003, RNF-013          |
| ADR-008 | RNF-007, RNF-067          |
| ADR-009 | RNF-058-060               |
| ADR-010 | RNF-015-016, RNF-057      |
| ADR-011 | RNF-009, RNF-022          |
| ADR-012 | RNF-058, RNF-059, RNF-060 |
| ADR-013 | —                         |
| ADR-014 | RNF-001, RNF-002, RNF-068 |

### Matriz ADR → BC

| ADR     | BCs Afectados                            |
| ------- | ---------------------------------------- |
| ADR-001 | Todos                                    |
| ADR-002 | BC-Identity (DB-Main), resto (DB-Tenant) |
| ADR-003 | Todos                                    |
| ADR-004 | Todos (productores y consumidores)       |
| ADR-005 | Todos                                    |
| ADR-006 | BC-Identity                              |
| ADR-007 | BC-Identity, todos (verificación)        |
| ADR-008 | Todos (emisión y recepción)              |
| ADR-009 | Todos                                    |
| ADR-010 | Todos (exposición API)                   |
| ADR-011 | BC-Documents                             |
| ADR-012 | Todos                                    |
| ADR-013 | BC-Membership                            |
| ADR-014 | BC-Identity                              |

---

## Changelog

- v1.1 (Abr 2026):
  - ADR-013: Transición SUSPENDED → NONPAYMENT_LEAVE (baja automática por morosidad)
  - ADR-014: Blacklist de access tokens en Redis para invalidación inmediata tras logout
- v1.0 (Feb 2026): Versión inicial
  - 12 ADRs cubriendo arquitectura, persistencia, comunicación, seguridad
  - Decisiones alineadas con RNF-004 (multi-tenant) como eje central
  - Trazabilidad completa a KB-004 y KB-005
