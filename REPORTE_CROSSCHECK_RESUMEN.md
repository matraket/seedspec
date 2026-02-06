# Cross-Check DDD: Resumen Ejecutivo

**Fecha:** 06 Febrero 2026  
**Documentos:** KB-005 (Modelo DDD) vs KB-010 (Casos de Uso)

---

## 🎯 Resultado Global

| Métrica | Valor | Estado |
|---------|-------|--------|
| **Cobertura del modelo DDD** | 70-75% | ⚠️ Requiere actualización |
| **Aggregates faltantes (críticos)** | 3 | ⚠️ |
| **Domain Events faltantes (críticos)** | 15+ | ⚠️ |
| **Inconsistencias de consumers** | 3 | ⚠️ |
| **Tiempo estimado de corrección** | 6-8 horas | ✅ Manejable |

---

## ❌ Top 10 Elementos Faltantes (Críticos)

| # | Elemento | Tipo | BC | Prioridad | Impacto |
|---|----------|------|----|-----------|---------| 
| 1 | `ListaEspera` | Aggregate | BC-Membresia | **ALTA** | UC-014 bloqueado |
| 2 | `ExpedienteDisciplinario` | Entity | BC-Membresia | **ALTA** | N3RF26 sin implementar |
| 3 | `EnlacePago` | Entity | BC-Tesoreria | **ALTA** | Pagos online sin soporte |
| 4 | `TenantProvisionado` | Event | BC-Identidad | **ALTA** | Provisión sin trazabilidad |
| 5 | `SuscripcionCreada` | Event | BC-Tesoreria | **ALTA** | Gestión cuotas incompleta |
| 6 | `SuscripcionCerrada` | Event | BC-Tesoreria | **ALTA** | Gestión cuotas incompleta |
| 7 | `RemesaSepaGenerada` | Event | BC-Tesoreria | **ALTA** | SEPA sin eventos |
| 8 | `AntiguedadAlcanzada` | Event | BC-Membresia | **MEDIA** | Derechos de voto |
| 9 | `PagoOnlineCompletado` | Event | BC-Tesoreria | **ALTA** | Pasarela sin confirmación |
| 10 | `ActaFirmada` | Event | BC-Documentos | **ALTA** | Cumplimiento legal |

---

## 📋 Checklist de Correcciones

### ✅ Fase 1: Aggregates Críticos (2 horas)

- [ ] Añadir `ListaEspera` a KB-005 sección 3.2.6
- [ ] Añadir `ExpedienteDisciplinario` como Entity de Socio en 3.2.1  
- [ ] Añadir `EnlacePago` como Entity de Cargo en 4.2.2

### ✅ Fase 2: Domain Events Críticos (3 horas)

#### BC-Identidad (sección 8.4)
- [ ] `TenantProvisionado`
- [ ] `TraspasoIniciado`
- [ ] `TraspasoCompletado`
- [ ] `CargoDirectivoAsignado`

#### BC-Membresia (sección 3.4)
- [ ] `AntiguedadAlcanzada`
- [ ] `TipoSocioCreado`
- [ ] `TipoSocioActualizado`
- [ ] `BajaDisciplinariaEjecutada`

#### BC-Tesoreria (sección 4.4)
- [ ] `SuscripcionCreada`
- [ ] `SuscripcionCerrada`
- [ ] `SuscripcionModificada`
- [ ] `RemesaSepaGenerada`
- [ ] `RemesaSepaEnviada`
- [ ] `PagoOnlineIniciado`
- [ ] `PagoOnlineCompletado`
- [ ] `CargoPagado`

#### BC-Documentos (nueva sección 7.3)
- [ ] Crear tabla de Domain Events
- [ ] `ActaCreada`
- [ ] `ActaCerrada`
- [ ] `ActaFirmada`
- [ ] `DocumentoSubido`

### ✅ Fase 3: Corregir Inconsistencias (1 hora)

- [ ] Actualizar payload de `SocioRegistrado` (añadir `cargoInscripcionId`)
- [ ] Añadir BC-Membresia como consumer de `SocioRegistrado` (generar carnet)
- [ ] Actualizar consumers de `EstadoSocioCambiado` (añadir BC-Comunicacion, BC-Eventos)
- [ ] Enriquecer payload de `EstadoSocioCambiado` (añadir `fechaCambio`, `usuarioId`)
- [ ] Actualizar payload de `PagoDevuelto` (añadir `socioId`, `fechaDevolucion`)

### ✅ Fase 4: Documentación Complementaria (1 hora)

- [ ] Crear sección "Extensiones Específicas" en KB-005
- [ ] Documentar `TurnoCaja` (BC-Tesoreria extensión)
- [ ] Documentar `EventoComida`, `Cuadrilla`, `Partido` como extensiones opcionales
- [ ] Añadir nota en `Ejercicio` sobre uso transversal (socios + contabilidad)

---

## 📊 Matriz de Aggregates

| Aggregate | KB-005 | UCs | Estado |
|-----------|--------|-----|--------|
| **Socio** | ✅ | ✅ | Consistente |
| **TipoSocio** | ✅ | ✅ | Consistente |
| **SolicitudAlta** | ✅ | ✅ | Consistente |
| **Carnet** | ✅ | ✅ | Consistente |
| **Ejercicio** | ✅ | ✅ | Consistente |
| **ListaEspera** | ❌ | ✅ | **Faltante** |
| **ExpedienteDisciplinario** | ❌ | ✅ | **Faltante** (como Entity) |
| **CuentaSocio** | ✅ | ✅ | Consistente |
| **PlanCuota** | ✅ | ✅ | Consistente |
| **RemesaSepa** | ✅ | ✅ | Consistente |
| **Movimiento** | ✅ | ✅ | Consistente |
| **TurnoCaja** | ❌ | ✅ | **Faltante** (extensión) |
| **Cargo** | ✅ (Entity) | ⚠️ | **Inconsistencia** (UC-024 lo trata como Aggregate) |
| **EnlacePago** | ❌ | ✅ | **Faltante** (como Entity de Cargo) |
| **Evento** | ✅ | ✅ | Consistente |
| **TipoEvento** | ✅ | ✅ | Consistente |
| **Inscripcion** | ✅ (Entity) | ✅ | Consistente |
| **EventoComida** | ❌ | ✅ | **Faltante** (extensión) |
| **Cuadrilla** | ❌ | ✅ | **Faltante** (extensión) |
| **Partido** | ❌ | ✅ | **Faltante** (extensión) |
| **Comunicacion** | ✅ | ✅ | Consistente |
| **Plantilla** | ✅ | ✅ | Consistente |
| **Anuncio** | ✅ | ✅ | Consistente |
| **Documento** | ✅ | ✅ | Consistente |
| **Categoria** | ✅ | ✅ | Consistente |
| **Acta** | ✅ | ✅ | Consistente |
| **Usuario** | ✅ | ✅ | Consistente |
| **Tenant** | ✅ | ✅ | Consistente |
| **MembresiaTenant** | ✅ | ✅ | Consistente |
| **Rol** | ✅ | ✅ | Consistente |

**Leyenda:**
- ✅ Definido y consistente
- ⚠️ Definido pero con inconsistencias
- ❌ No definido en KB-005

---

## 🔥 Inconsistencias Críticas de Domain Events

### 1. `SocioRegistrado`

| Aspecto | KB-005 | UC-011 | Estado |
|---------|--------|--------|--------|
| **Payload** | socioId, tipoSocio, fecha | socioId, tipoSocioId, **cargoInscripcionId**, fechaAlta | ⚠️ Payload incompleto |
| **Consumers** | BC-Tesoreria, BC-Comunicacion | BC-Tesoreria, BC-Comunicacion, **BC-Membresia** | ⚠️ Consumer faltante |

**Acción:** Añadir `cargoInscripcionId` al payload y BC-Membresia como consumer.

---

### 2. `EstadoSocioCambiado`

| Aspecto | KB-005 | UC-007 | Estado |
|---------|--------|--------|--------|
| **Payload** | socioId, estadoAnterior, estadoNuevo, motivo | + **fechaCambio**, **usuarioId** | ⚠️ Payload incompleto |
| **Consumers** | BC-Tesoreria | BC-Tesoreria, **BC-Comunicacion**, **BC-Eventos** | ⚠️ 2 consumers faltantes |

**Acción:** Enriquecer payload y añadir consumers.

---

### 3. `PagoDevuelto`

| Aspecto | KB-005 | UC-023 | Estado |
|---------|--------|--------|--------|
| **Payload** | pagoId, cargoId, motivo | + **socioId**, **fechaDevolucion** | ⚠️ Payload incompleto |
| **Consumers** | BC-Comunicacion, BC-Membresia | Consistente | ✅ |

**Acción:** Añadir campos de auditoría al payload.

---

## 📈 Métricas de Validación

### Por Bounded Context

| BC | Aggregates Válidos | Aggregates Faltantes | Events Válidos | Events Faltantes | Cobertura |
|----|-------------------|---------------------|----------------|-----------------|-----------|
| **BC-Identidad** | 4/4 | 0 | 6/13 | 7 | 46% |
| **BC-Membresia** | 5/8 | 3 | 11/19 | 8 | 58% |
| **BC-Tesoreria** | 4/7 | 3 | 9/31 | 22 | 29% |
| **BC-Eventos** | 2/6 | 4 | 8/12 | 4 | 67% |
| **BC-Comunicacion** | 3/3 | 0 | 0/6 | 6 | 0% (no doc) |
| **BC-Documentos** | 3/3 | 0 | 0/6 | 6 | 0% (no doc) |
| **TOTAL** | **21/31** | **10** | **34/87** | **53** | **39%** |

**Nota:** La cobertura de Domain Events es especialmente baja porque KB-005 solo documentó los eventos más críticos en la primera iteración.

---

## ⏱️ Roadmap de Correcciones

```
Semana 1: Aggregates críticos
├── Día 1: ListaEspera (2h)
├── Día 2: ExpedienteDisciplinario + EnlacePago (2h)
└── Día 3: Validación y pruebas (1h)

Semana 2: Domain Events críticos
├── Día 1-2: BC-Identidad + BC-Membresia (3h)
├── Día 3-4: BC-Tesoreria (3h)
└── Día 5: BC-Documentos + BC-Comunicacion (2h)

Semana 3: Inconsistencias y documentación
├── Día 1: Corregir consumers (1h)
├── Día 2: Enriquecer payloads (1h)
└── Día 3: Documentación extensiones (1h)

Total: ~16 horas distribuidas en 3 semanas
```

---

## ✅ Criterios de Aceptación

El cross-check se considerará **RESUELTO** cuando:

1. ✅ **100% de Aggregates críticos** documentados en KB-005
2. ✅ **100% de Domain Events críticos** documentados en KB-005
3. ✅ **0 inconsistencias** en payloads de eventos existentes
4. ✅ **0 inconsistencias** en consumers de eventos existentes
5. ✅ **Cobertura >= 85%** en matriz de Aggregates
6. ✅ **Cobertura >= 80%** en Domain Events (excluyendo eventos informativos de baja prioridad)

---

**Próximo paso:** Ejecutar Fase 1 del roadmap (Aggregates críticos)

---

**Documento completo:** Ver `REPORTE_CROSSCHECK_DDD.md`
