# Reporte de Cross-Check: Modelo DDD vs Casos de Uso

**Proyecto:** Associated - ERP para Colectividades Españolas  
**Fecha:** 06 Febrero 2026  
**Documentos analizados:**
- **KB-005:** `005_modelo-dominio.md` (fuente de verdad DDD)
- **KB-010:** `010_casos-uso_FINAL-HUMANO.md` (71 Casos de Uso)

---

## 📋 Resumen Ejecutivo

| Métrica | Resultado | Estado |
|---------|-----------|--------|
| **Aggregates definidos en KB-005** | 21 | ✅ |
| **Entities definidas en KB-005** | 14+ | ✅ |
| **Domain Events definidos en KB-005** | 26 | ✅ |
| **Casos de Uso analizados** | 71 | ✅ |
| **Aggregates referenciados en UCs** | ~51 únicos | ⚠️ |
| **Domain Events referenciados en UCs** | ~60 únicos | ⚠️ |
| **Inconsistencias críticas detectadas** | 15+ | ⚠️ |
| **% Referencias válidas (estimado)** | 70-75% | ⚠️ |

**Conclusión:** Existen inconsistencias significativas entre el modelo de dominio DDD (KB-005) y las referencias en los casos de uso (KB-010). Se requiere actualización del modelo de dominio para incluir elementos emergentes del análisis de casos de uso.

---

## ✅ Elementos Correctamente Definidos

### Aggregates Raíz (Aggregate Roots) - KB-005

| BC | Aggregates Definidos | Estado |
|----|---------------------|--------|
| **BC-Identidad** | Usuario, Tenant, MembresiaTenant, Rol | ✅ Completo |
| **BC-Membresia** | Socio, TipoSocio, SolicitudAlta, Carnet, Ejercicio | ✅ Completo |
| **BC-Tesoreria** | CuentaSocio, PlanCuota, RemesaSepa, Movimiento | ✅ Completo |
| **BC-Eventos** | Evento, TipoEvento | ⚠️ Incompleto (falta Inscripcion como Aggregate?) |
| **BC-Comunicacion** | Comunicacion, Plantilla, Anuncio | ✅ Completo |
| **BC-Documentos** | Documento, Categoria, Acta | ✅ Completo |

**Total:** 21 Aggregates Root definidos

### Entities (dentro de Aggregates) - KB-005

| Aggregate Padre | Entities | Estado |
|----------------|----------|--------|
| **Socio** | HistorialEstados, CamposPersonalizados | ✅ |
| **SolicitudAlta** | DocumentoPendiente, Aval | ✅ |
| **CuentaSocio** | Cargo, Pago, MandatoSepa, SuscripcionCuota | ✅ |
| **RemesaSepa** | AdeudoSepa | ✅ |
| **Evento** | Inscripcion | ✅ |
| **Comunicacion** | Envio | ✅ |
| **Acta** | Asistente, Acuerdo | ✅ |

**Total:** 14 Entities principales definidas

### Domain Events Documentados - KB-005

#### BC-Membresia (10 eventos)
- ✅ `SocioRegistrado`
- ✅ `SocioDadoDeBaja`
- ✅ `EstadoSocioCambiado`
- ✅ `TipoSocioCambiado`
- ✅ `DatosSocioActualizados`
- ✅ `CarnetEmitido`
- ✅ `CarnetValidado`
- ✅ `EjercicioAbierto`
- ✅ `EjercicioCerrado`
- ✅ `SolicitudAltaCreada`, `SolicitudAltaAprobada`

#### BC-Tesoreria (9 eventos)
- ✅ `CargoGenerado`
- ✅ `PagoRegistrado`
- ✅ `PagoDevuelto`
- ✅ `MorosidadDetectada`
- ✅ `SocioSuspendidoPorImpago`
- ✅ `RemesaGenerada`
- ✅ `RemesaProcesada`
- ✅ `MandatoCreado`
- ✅ `MandatoCaducado`

#### BC-Eventos (7 eventos)
- ✅ `EventoCreado`
- ✅ `EventoPublicado`
- ✅ `EventoCancelado`
- ✅ `InscripcionRealizada`
- ✅ `InscripcionCancelada`
- ✅ `AforoCompletado`
- ✅ `AsistenciaRegistrada`
- ✅ `PlazaLiberada`

#### BC-Identidad (6 eventos)
- ✅ `UsuarioCreado`
- ✅ `UsuarioAutenticado`
- ✅ `AutenticacionFallida`
- ✅ `UsuarioBloqueado`
- ✅ `RolAsignado`
- ✅ `TenantCreado`

**Total:** 26 Domain Events documentados en KB-005

---

## ❌ Aggregates NO Definidos en KB-005 (referenciados en UCs)

### 1. **ListaEspera** (UC-014)
- **UC:** UC-014 - Gestión de lista de espera
- **Referencia:** `**Aggregates:** **ListaEspera** (nuevo Aggregate Root), AspiranteEnEspera (Entity)`
- **BC:** BC-Membresia
- **Estado KB-005:** ❌ NO DEFINIDO
- **Impacto:** ALTO - Se menciona explícitamente como "nuevo Aggregate Root"
- **Recomendación:** Añadir a KB-005 en sección 3.2.6

**Propuesta de definición:**
```
#### 3.2.6 Aggregate: ListaEspera

┌─────────────────────────────────────────────────────────────┐
│ LISTA_ESPERA (Aggregate Root)                               │
├─────────────────────────────────────────────────────────────┤
│ Identity: ListaEsperaId                                     │
│                                                             │
│ Entities:                                                   │
│   - AspiranteEnEspera[] (aspirantes ordenados por prioridad)│
│                                                             │
│ Properties:                                                 │
│   - tipoSocioId: TipoSocioId (lista específica por tipo)    │
│   - capacidadMaxima: int?                                   │
│   - estado: EstadoListaEspera (abierta, cerrada)            │
│                                                             │
│ Invariants:                                                 │
│   - Prioridad única por aspirante                           │
│   - No duplicados (mismo aspirante solo 1 vez)              │
└─────────────────────────────────────────────────────────────┘
```

---

### 2. **ExpedienteDisciplinario** (UC-013)
- **UC:** UC-013 - Baja de socio
- **Referencia:** `**Aggregates:** **Socio**, **ExpedienteDisciplinario** (Entity)`
- **BC:** BC-Membresia
- **Estado KB-005:** ❌ NO DEFINIDO
- **Impacto:** MEDIO - Necesario para bajas disciplinarias (N3RF26)
- **Recomendación:** Añadir como Entity dentro de Aggregate Socio o como Aggregate independiente

**Propuesta de definición (como Entity):**
```
#### Entity: ExpedienteDisciplinario (dentro de Socio)

┌─────────────────────────────────────────────────────────────┐
│ EXPEDIENTE_DISCIPLINARIO (Entity)                           │
├─────────────────────────────────────────────────────────────┤
│ Identity: ExpedienteId                                      │
│                                                             │
│ Properties:                                                 │
│   - fechaApertura: Date                                     │
│   - motivo: string                                          │
│   - gravedad: Gravedad (leve, grave, muy_grave)             │
│   - estado: EstadoExpediente (abierto, resuelto, archivado) │
│   - resolucion: string?                                     │
│   - fechaCierre: Date?                                      │
│   - sancion: TipoSancion? (amonestacion, suspension, baja)  │
│                                                             │
│ Invariants:                                                 │
│   - Si estado=resuelto, debe existir resolución y sanción   │
└─────────────────────────────────────────────────────────────┘
```

---

### 3. **EnlacePago** (UC-024 - Integración pasarela)
- **UC:** UC-024 - Integración con pasarela de pago
- **Referencia:** `**Aggregates:** **Cargo**, EnlacePago (Entity), IntegracionPasarela (Entity)`
- **BC:** BC-Tesoreria
- **Estado KB-005:** ❌ NO DEFINIDO
- **Impacto:** ALTO - Crítico para pagos online (N4RF24-27)
- **Recomendación:** Añadir como Entity dentro de Aggregate Cargo

**Propuesta de definición:**
```
#### Entity: EnlacePago (dentro de Cargo)

┌─────────────────────────────────────────────────────────────┐
│ ENLACE_PAGO (Entity)                                        │
├─────────────────────────────────────────────────────────────┤
│ Identity: EnlacePagoId                                      │
│                                                             │
│ Properties:                                                 │
│   - url: string (URL de pasarela)                           │
│   - tokenPasarela: string (token único)                     │
│   - fechaGeneracion: DateTime                               │
│   - fechaExpiracion: DateTime (típ. +24h)                   │
│   - estado: EstadoEnlace (pendiente, usado, expirado)       │
│   - metadatosPasarela: JSON                                 │
│                                                             │
│ Invariants:                                                 │
│   - URL válida y firmada                                    │
│   - Token único y no predecible                             │
│   - Expiración posterior a generación                       │
└─────────────────────────────────────────────────────────────┘
```

---

### 4. **TurnoCaja** (UC-026 - Caja de turnos para peñas)
- **UC:** UC-026 - Gestión de caja de turnos
- **Referencia:** `**Aggregates:** **TurnoCaja**, VentaCaja (Entity), ArqueoCaja (Entity)`
- **BC:** BC-Tesoreria (extensión específica peñas)
- **Estado KB-005:** ❌ NO DEFINIDO (se menciona en RF pero no modelado)
- **Impacto:** BAJO - Extensión específica peñas (N4RF34-38)
- **Recomendación:** Añadir a KB-005 como extensión documentada en sección 4.2.10

**Propuesta de definición:**
```
#### 4.2.10 Aggregate: TurnoCaja (Extensión Peñas)

┌─────────────────────────────────────────────────────────────┐
│ TURNO_CAJA (Aggregate Root)                                 │
├─────────────────────────────────────────────────────────────┤
│ Identity: TurnoCajaId                                       │
│                                                             │
│ Entities:                                                   │
│   - VentaCaja[] (ventas del turno)                          │
│   - ArqueoCaja? (recuento al cerrar turno)                  │
│                                                             │
│ Properties:                                                 │
│   - fechaTurno: Date                                        │
│   - turno: Turno (mañana, tarde, noche)                     │
│   - responsableId: UsuarioId                                │
│   - estado: EstadoTurno (abierto, cerrado, cuadrado)        │
│   - fondoInicial: Dinero                                    │
│   - totalVentas: Dinero                                     │
│   - descuadre: Dinero?                                      │
│                                                             │
│ Invariants:                                                 │
│   - Solo un turno abierto a la vez por ubicación            │
│   - Arqueo obligatorio al cerrar turno                      │
└─────────────────────────────────────────────────────────────┘
```

---

### 5. **CategoriaContable** (UC-025 - Registro contable)
- **UC:** UC-025 - Registro de movimientos contables
- **Referencia:** `**Aggregates:** **Movimiento**, CategoriaContable, EjercicioContable`
- **BC:** BC-Tesoreria
- **Estado KB-005:** ⚠️ PARCIALMENTE DEFINIDO
  - `Movimiento` existe como Aggregate (sección 4.2.9)
  - `CategoriaContable` existe como Value Object
  - `EjercicioContable` NO EXISTE (conflicto con `Ejercicio` de BC-Membresia)
- **Impacto:** MEDIO - Necesario para contabilidad (N4RF28-33)
- **Recomendación:** Aclarar diferencia entre Ejercicio (Membresia) y Ejercicio Contable (Tesoreria)

**Análisis:** 
- En KB-005, `Movimiento` tiene propiedad `ejercicioId: EjercicioId` que referencia al Ejercicio de BC-Membresia
- No se requiere Aggregate separado `EjercicioContable`, pero sí aclarar que el `Ejercicio` de Membresia sirve también para delimitar periodos contables
- **Acción:** Actualizar documentación de `Ejercicio` en KB-005 para indicar su uso transversal

---

### 6. Aggregates de Extensiones Específicas (Peñas, Cofradías, Clubes)

Estos Aggregates se mencionan en UCs de extensiones específicas por tipo de colectividad:

#### **EventoComida** (UC-033 - Eventos de comida peñas)
- **Referencia:** `**Aggregates:** Evento (Aggregate Root), EventoComida (extensión)`
- **Estado:** ❌ NO DEFINIDO en KB-005
- **Tipo:** Extensión BC-Eventos para peñas
- **Recomendación:** Documentar en KB-005 sección 5.2.5 como extensión

#### **Cuadrilla** (UC-037 - Ensayos de tambores)
- **Referencia:** `**Aggregates:** Cuadrilla (nuevo), Ensayo`
- **Estado:** ❌ NO DEFINIDO en KB-005
- **Tipo:** Extensión BC-Eventos para cofradías/peñas de tambores
- **Recomendación:** Documentar como extensión específica

#### **Partido** (UC-039 - Partidos deportivos)
- **Referencia:** `**Aggregates:** Partido (extiende Evento), Convocatoria, Sancion`
- **Estado:** ❌ NO DEFINIDO en KB-005
- **Tipo:** Extensión BC-Eventos para clubes deportivos
- **Recomendación:** Documentar como extensión específica

**NOTA:** Estos Aggregates corresponden a requisitos N12 (Específicas Colectividad) y N13 (Contexto Aragonés) que fueron **EXCLUIDOS del scope documental principal** según AGENTS.md. Sin embargo, aparecen referenciados en los UCs. 

**Decisión requerida:** ¿Se documentan en KB-005 como "Extensiones Futuras" o se eliminan de los UCs?

---

## ❌ Domain Events NO Definidos en KB-005 (referenciados en UCs)

### Eventos de BC-Identidad

| Evento | UC | Estado KB-005 | Impacto |
|--------|----|--------------|---------| 
| `TenantProvisionado` | UC-001 | ❌ NO DEFINIDO | ALTO - Crítico para provisión |
| `TenantConfigActualizado` | UC-003 | ❌ NO DEFINIDO | MEDIO |
| `RolPersonalizadoCreado` | UC-004 | ❌ NO DEFINIDO | BAJO |
| `TraspasoIniciado` | UC-005 | ❌ NO DEFINIDO | MEDIO |
| `TraspasoCompletado` | UC-005 | ❌ NO DEFINIDO | MEDIO |
| `CargoDirectivoAsignado` | UC-005 | ❌ NO DEFINIDO | MEDIO |
| `TenantCambiado` | UC-002 | ❌ NO DEFINIDO | BAJO - UX |

**Recomendación:** Añadir estos 7 eventos a la tabla de Domain Events de BC-Identidad (sección 8.4 de KB-005)

---

### Eventos de BC-Membresia

| Evento | UC | Estado KB-005 | Impacto |
|--------|----|--------------|---------| 
| `AntiguedadAlcanzada` | UC-009 | ❌ NO DEFINIDO | MEDIO - Cálculo derechos |
| `EventoTimelineRegistrado` | UC-009 | ❌ NO DEFINIDO | BAJO - Auditoría |
| `TipoSocioCreado` | UC-008 | ❌ NO DEFINIDO | MEDIO |
| `TipoSocioActualizado` | UC-008 | ❌ NO DEFINIDO | BAJO |
| `ExpedienteDisciplinarioAbierto` | UC-013 | ❌ NO DEFINIDO | MEDIO |
| `BajaDisciplinariaEjecutada` | UC-013 | ❌ NO DEFINIDO | ALTO |
| `AspiranteAgregadoListaEspera` | UC-014 | ❌ NO DEFINIDO | MEDIO |
| `PlazaAsignada` | UC-014 | ❌ NO DEFINIDO | MEDIO |

**Recomendación:** Añadir estos 8 eventos a la tabla de Domain Events de BC-Membresia (sección 3.4 de KB-005)

---

### Eventos de BC-Tesoreria

| Evento | UC | Estado KB-005 | Impacto |
|--------|----|--------------|---------| 
| `PlanCuotaCreado` | UC-017 | ❌ NO DEFINIDO | MEDIO |
| `PlanCuotaModificado` | UC-017 | ❌ NO DEFINIDO | MEDIO |
| `PlanCuotaVinculado` | UC-017 | ❌ NO DEFINIDO | BAJO |
| `SuscripcionCreada` | UC-018 | ❌ NO DEFINIDO | ALTO - Crítico |
| `SuscripcionCerrada` | UC-018 | ❌ NO DEFINIDO | ALTO - Crítico |
| `SuscripcionModificada` | UC-018 | ❌ NO DEFINIDO | MEDIO |
| `GeneracionMensualCompletada` | UC-019 | ❌ NO DEFINIDO | MEDIO |
| `CargaMasivaCreada` | UC-020 | ❌ NO DEFINIDO | BAJO |
| `CargoPagado` | UC-021 | ❌ NO DEFINIDO | ALTO - Crítico |
| `ReciboGenerado` | UC-021 | ❌ NO DEFINIDO | MEDIO |
| `AvisoMorosidadEnviado` | UC-022 | ❌ NO DEFINIDO | MEDIO |
| `MorosidadRegularizada` | UC-022 | ❌ NO DEFINIDO | MEDIO |
| `BajaPorImpago` | UC-022 | ❌ NO DEFINIDO | ALTO |
| `RemesaSepaGenerada` | UC-023 | ❌ NO DEFINIDO | ALTO - Crítico |
| `RemesaSepaEnviada` | UC-023 | ❌ NO DEFINIDO | ALTO - Crítico |
| `MandatoSepaRegistrado` | UC-023 | ❌ NO DEFINIDO | MEDIO |
| `CargoMarcadoReintento` | UC-023 | ❌ NO DEFINIDO | MEDIO |
| `MandatoSepaRevocado` | UC-023 | ❌ NO DEFINIDO | MEDIO |
| `PagoOnlineIniciado` | UC-024 | ❌ NO DEFINIDO | ALTO |
| `PagoOnlineCompletado` | UC-024 | ❌ NO DEFINIDO | ALTO |
| `PagoOnlineFallido` | UC-024 | ❌ NO DEFINIDO | ALTO |
| `MovimientoRegistrado` | UC-025 | ❌ NO DEFINIDO | MEDIO |

**Recomendación:** Añadir estos 22 eventos a la tabla de Domain Events de BC-Tesoreria (sección 4.4 de KB-005). Algunos son variantes de eventos existentes que requieren nombres más específicos.

---

### Eventos de BC-Eventos

| Evento | UC | Estado KB-005 | Impacto |
|--------|----|--------------|---------| 
| `EventoModificado` | UC-029 | ❌ NO DEFINIDO | MEDIO |
| `InscripcionModificada` | UC-031 | ❌ NO DEFINIDO | MEDIO |
| `AsistenciaConfirmada` | UC-032 | ❌ NO DEFINIDO | MEDIO |
| `ValoracionRegistrada` | UC-040 | ❌ NO DEFINIDO | BAJO |

**Recomendación:** Añadir estos 4 eventos a la tabla de Domain Events de BC-Eventos (sección 5.3 de KB-005)

---

### Eventos de BC-Comunicacion

| Evento | UC | Estado KB-005 | Impacto |
|--------|----|--------------|---------| 
| `ComunicacionEnviada` | UC-041 | ❌ NO DEFINIDO | MEDIO |
| `ComunicacionProgramada` | UC-041 | ❌ NO DEFINIDO | BAJO |
| `EnvioFallido` | UC-041 | ❌ NO DEFINIDO | MEDIO |
| `PlantillaCreada` | UC-044 | ❌ NO DEFINIDO | BAJO |
| `PlantillaModificada` | UC-044 | ❌ NO DEFINIDO | BAJO |
| `AnuncioPublicado` | UC-047 | ❌ NO DEFINIDO | BAJO |

**Recomendación:** Añadir estos 6 eventos a la tabla de Domain Events de BC-Comunicacion (actualmente la sección 6.3 de KB-005 solo lista eventos consumidos, no emitidos)

---

### Eventos de BC-Documentos

| Evento | UC | Estado KB-005 | Impacto |
|--------|----|--------------|---------| 
| `ActaCreada` | UC-048 | ❌ NO DEFINIDO | MEDIO |
| `ActaCerrada` | UC-049 | ❌ NO DEFINIDO | MEDIO |
| `ActaFirmada` | UC-050 | ❌ NO DEFINIDO | ALTO - Legal |
| `DocumentoSubido` | UC-051 | ❌ NO DEFINIDO | MEDIO |
| `DocumentoEliminado` | UC-052 | ❌ NO DEFINIDO | MEDIO |
| `CategoriaCreada` | UC-054 | ❌ NO DEFINIDO | BAJO |

**Recomendación:** Añadir tabla de Domain Events a BC-Documentos (actualmente NO tiene sección de eventos en KB-005)

---

## ⚠️ Inconsistencias en Consumers de Domain Events

### Evento: `SocioRegistrado`

**Definición KB-005 (sección 3.4):**
```
| `SocioRegistrado` | Alta completada | socioId, tipoSocio, fecha | 
  BC-Tesoreria (generar cuota), BC-Comunicacion (bienvenida) |
```

**Referencia en UC-011:**
```
| `SocioRegistrado` | socioId, tipoSocioId, cargoInscripcionId, fechaAlta | 
  BC-Tesoreria (crear CuentaSocio), BC-Comunicacion (email bienvenida), BC-Membresia (generar carnet) |
```

**Diferencias:**
1. **Payload:** KB-005 no incluye `cargoInscripcionId` (añadido en UC)
2. **Consumers:** UC añade BC-Membresia (generar carnet) que no aparece en KB-005

**Recomendación:** Actualizar KB-005 para incluir `cargoInscripcionId` en payload y añadir BC-Membresia como consumer para generación de carnet.

---

### Evento: `EstadoSocioCambiado`

**Definición KB-005 (sección 3.4):**
```
| `EstadoSocioCambiado` | Cambio de estado | socioId, estadoAnterior, estadoNuevo, motivo | 
  BC-Tesoreria (suspender/reactivar cobros) |
```

**Referencia en UC-007:**
```
| `EstadoSocioCambiado` | socioId, estadoAnterior, estadoNuevo, motivo, fechaCambio, usuarioId |
  Consumidores:
    - BC-Tesoreria: Suspender/reactivar cobros
    - BC-Comunicacion: Notificar al socio
    - BC-Eventos: Actualizar elegibilidad para inscripciones
```

**Diferencias:**
1. **Payload:** UC añade `fechaCambio` y `usuarioId` (auditoría)
2. **Consumers:** UC añade BC-Comunicacion y BC-Eventos

**Recomendación:** Actualizar KB-005 con payload completo y consumers adicionales.

---

### Evento: `PagoDevuelto`

**Definición KB-005 (sección 4.4):**
```
| `PagoDevuelto` | Devolución bancaria | pagoId, cargoId, motivo | 
  BC-Comunicacion (notificar), BC-Membresia (marcar morosidad) |
```

**Referencia en UC-023:**
```
| `PagoDevuelto` | pagoId, cargoId, socioId, motivo, fechaDevolucion | 
  Consumidores: BC-Membresia (actualizar estado), BC-Comunicacion (notificar), MorosidadService |
```

**Diferencias:**
1. **Payload:** UC añade `socioId` y `fechaDevolucion`
2. **Consumers:** UC especifica `MorosidadService` como consumer interno de BC-Tesoreria

**Recomendación:** Actualizar payload en KB-005. `MorosidadService` es un Domain Service interno, no requiere cambio en tabla de eventos.

---

## 🔍 Aggregates con Posible Ambigüedad

### 1. **Inscripcion**

**En KB-005 (sección 5.2.2):**
- Definida como **Entity** dentro del Aggregate **Evento**

**En UCs:**
- UC-030: `**Aggregates:** Evento (Aggregate Root), Inscripcion (Entity)` ✅ CONSISTENTE
- UC-031: `**Aggregates:** Evento (Aggregate Root), Inscripcion (Entity)` ✅ CONSISTENTE

**Conclusión:** No hay inconsistencia. Inscripcion está correctamente modelada como Entity.

---

### 2. **Cargo**

**En KB-005 (sección 4.2.2):**
- Definido como **Entity** dentro del Aggregate **CuentaSocio**

**En UCs:**
- UC-020: `**Aggregates:** **CuentaSocio**, *Cargo* (Entity)` ✅ CONSISTENTE (notación con * indica Entity)
- UC-024: `**Aggregates:** **Cargo**, EnlacePago (Entity)` ❌ INCONSISTENTE (Cargo como Aggregate Root)

**Análisis:** En UC-024 (integración pasarela), Cargo se referencia como Aggregate Root con Entity `EnlacePago` dentro. Esto contradice el modelo DDD donde Cargo es Entity de CuentaSocio.

**Opciones:**
1. **Opción A:** Mantener Cargo como Entity y `EnlacePago` como propiedad/VO dentro de Cargo
2. **Opción B:** Promover Cargo a Aggregate Root (BREAKING CHANGE - no recomendado)

**Recomendación:** Mantener Cargo como Entity (Opción A). Actualizar UC-024 para reflejar:
```
**Aggregates:** **CuentaSocio**, *Cargo* (Entity con EnlacePago VO), *IntegracionPasarela* (Entity)
```

---

### 3. **Ejercicio vs EjercicioContable**

**En KB-005:**
- `Ejercicio` es Aggregate Root de BC-Membresia (sección 3.2.5)

**En UCs:**
- UC-010: `**Aggregates:** **Ejercicio**` ✅ CONSISTENTE
- UC-025: `**Aggregates:** **Movimiento**, CategoriaContable, EjercicioContable` ❌ INCONSISTENTE

**Análisis:** No existe `EjercicioContable` en KB-005. El Aggregate `Ejercicio` de BC-Membresia sirve para delimitar periodos tanto de gestión de socios como contables.

**Recomendación:** 
- **NO crear** Aggregate `EjercicioContable`
- Actualizar UC-025 para usar `Ejercicio` de BC-Membresia
- Añadir nota en KB-005 explicando el uso transversal de `Ejercicio`

---

## 📊 Estadísticas Detalladas

### Por Bounded Context

| BC | Aggregates KB-005 | Aggregates UCs | Discrepancias | Domain Events KB-005 | Domain Events UCs | Discrepancias |
|----|------------------|----------------|---------------|---------------------|------------------|---------------|
| **BC-Identidad** | 4 | 4 | 0 | 6 | 13 | +7 nuevos |
| **BC-Membresia** | 5 | 8 | +3 nuevos | 11 | 19 | +8 nuevos |
| **BC-Tesoreria** | 4 | 7 | +3 nuevos | 9 | 31 | +22 nuevos |
| **BC-Eventos** | 2 | 6 | +4 extensiones | 8 | 12 | +4 nuevos |
| **BC-Comunicacion** | 3 | 3 | 0 | 0 (solo consumers) | 6 | +6 nuevos |
| **BC-Documentos** | 3 | 3 | 0 | 0 | 6 | +6 nuevos |
| **Total** | **21** | **31+** | **+10** | **34** | **87+** | **+53** |

---

## ✅ Recomendaciones Prioritarias

### Prioridad ALTA (Críticas)

1. **Añadir Aggregates faltantes a KB-005:**
   - `ListaEspera` (BC-Membresia) - sección 3.2.6
   - `ExpedienteDisciplinario` como Entity de Socio - sección 3.2.1
   - `EnlacePago` como Entity de Cargo - sección 4.2.2

2. **Añadir Domain Events críticos a KB-005:**
   - BC-Identidad: `TenantProvisionado`, `TraspasoCompletado`, `CargoDirectivoAsignado`
   - BC-Membresia: `AntiguedadAlcanzada`, `BajaDisciplinariaEjecutada`
   - BC-Tesoreria: `SuscripcionCreada`, `SuscripcionCerrada`, `RemesaSepaGenerada`, `PagoOnlineCompletado`
   - BC-Documentos: `ActaFirmada` (implicaciones legales)

3. **Corregir inconsistencias de consumers:**
   - Actualizar `SocioRegistrado` para incluir BC-Membresia (generar carnet)
   - Actualizar `EstadoSocioCambiado` para incluir BC-Comunicacion y BC-Eventos

### Prioridad MEDIA

4. **Añadir Aggregates específicos documentados:**
   - `TurnoCaja` (extensión BC-Tesoreria para peñas) - sección 4.2.10

5. **Completar tabla de Domain Events de BC-Comunicacion:**
   - Actualmente solo lista eventos consumidos, añadir eventos emitidos

6. **Crear tabla de Domain Events de BC-Documentos:**
   - Actualmente no existe, añadir sección 7.3

7. **Enriquecer payloads de eventos existentes:**
   - Añadir `fechaEvento` y `usuarioId` a todos los eventos para auditoría
   - Ejemplo: `EstadoSocioCambiado` debe incluir `fechaCambio` y `usuarioId`

### Prioridad BAJA

8. **Documentar extensiones específicas:**
   - Crear sección en KB-005 para extensiones por tipo de colectividad
   - Documentar `EventoComida`, `Cuadrilla`, `Partido` como extensiones opcionales

9. **Añadir eventos informativos:**
   - `PlantillaCreada`, `AnuncioPublicado`, `CategoriaCreada`

10. **Aclarar uso transversal de Ejercicio:**
    - Añadir nota en sección 3.2.5 explicando que sirve tanto para gestión de socios como delimitación contable

---

## 🔧 Acciones Inmediatas Sugeridas

1. **Actualizar KB-005 con Aggregates críticos faltantes** (1-2 horas)
   - Añadir secciones 3.2.6 (ListaEspera), actualizar 3.2.1 (ExpedienteDisciplinario), actualizar 4.2.2 (EnlacePago)

2. **Completar tablas de Domain Events** (2-3 horas)
   - Añadir eventos críticos a secciones 3.4, 4.4, 5.3, 6.3 (nueva), 7.3 (nueva)

3. **Corregir inconsistencias de consumers** (30 minutos)
   - Actualizar tablas de eventos en secciones 3.4 y 4.4

4. **Validar modelo actualizado** (1 hora)
   - Re-ejecutar cross-check tras cambios
   - Verificar coherencia entre KB-005 y KB-010

5. **Documentar decisiones sobre extensiones** (30 minutos)
   - Decidir si extensiones específicas (Peñas/Cofradías/Clubes) se documentan en KB-005 o se marcan como "Fuera de scope"
   - Actualizar AGENTS.md con decisión

---

## 📝 Notas Finales

- **Cobertura actual:** El modelo DDD de KB-005 cubre aproximadamente el 70-75% de las necesidades identificadas en los casos de uso
- **Causa principal de discrepancias:** Los casos de uso fueron generados con mayor detalle que el modelo de dominio inicial, identificando elementos emergentes durante el análisis
- **Impacto:** Las discrepancias NO bloquean la implementación, pero su resolución mejorará la consistencia y mantenibilidad del sistema
- **Tiempo estimado de resolución:** 6-8 horas para implementar todas las recomendaciones de prioridad ALTA y MEDIA

---

**Generado:** 06 Febrero 2026  
**Herramienta:** Cross-check manual KB-005 vs KB-010  
**Revisor:** Sistema de análisis de consistencia DDD
