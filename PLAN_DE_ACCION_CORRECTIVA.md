# 🚀 PLAN DE ACCIÓN CORRECTIVA
## Documento: 010_casos-uso_FINAL-HUMANO.md

**Fecha:** 06 Febrero 2026  
**Objetivo:** Llevar el documento de KB-010 de calidad 75% → 100%  
**Estimación Total:** 2-3 semanas (28-36 horas)

---

## 📊 RESUMEN DE PROBLEMAS DETECTADOS

| Categoría | Problemas | Impacto | Prioridad |
|-----------|-----------|---------|-----------|
| Estructural | UC-063 faltante, error numeración US | 🔴 Bloqueante | CRÍTICO |
| Metadata | 25 UCs incompletos (35.7%) | 🟠 Alto | ALTA |
| Trazabilidad | 42 US huérfanas (20.8%), 14 Must sin cubrir | 🔴 Bloqueante | CRÍTICO |
| DDD | 10 Aggregates + 53 Events no documentados | 🟠 Alto | ALTA |
| Calidad | Duplicados, tablas, consistencia | 🟡 Medio | MEDIA |

---

## FASE 1: CORRECCIONES CRÍTICAS 🔴

**Objetivo:** Resolver bloqueantes para alcanzar calidad mínima de entrega  
**Duración:** 3-4 días  
**Esfuerzo:** 10-12 horas  
**Prioridad:** CRÍTICA

---

### Tarea 1.1: Generar UC-063 Completo

**Problema:** UC-063 falta en secuencia, salta de UC-062 a UC-064

**Acciones:**
1. Usar plantilla completa de `REPORTE_UC063_UC016.md`
2. Insertar UC-063 después de UC-062 (línea ~25400)
3. Asignar **US-160** como User Story correcta (NO US-201, US-202)

**Contenido UC-063:**
- **Título:** Backup Completo Automático
- **Bounded Context:** Transversal (Infraestructura)
- **Application Service:** `BackupService.generarBackupCompleto()`
- **User Story:** US-160
- **Prioridad:** Should Have

**Verificación:**
- [x] Sección `### UC-063:` creada con estructura completa
- [x] Metadata completa (US, BC, Service, Aggregates, Priority)
- [x] Flujos Normal, Alternativos y Excepciones documentados
- [x] Domain Events definidos (BackupCompletado, BackupFallido)
- [x] Notas de Implementación con tecnologías (pg_dump, S3, BullMQ)

**Estimación:** 1-2 horas

---

### Tarea 1.2: Corregir Numeración User Stories (N8)

**Problema:** Tabla Import/Export referencia US-188→202 (incorrectas)  
**Realidad:** Las correctas son US-148→160

**Acciones:**
1. Localizar tabla Import/Export (línea ~21776-21783)
2. Corregir referencias de User Stories para cada UC:

| UC | User Stories INCORRECTAS | User Stories CORRECTAS |
|----|--------------------------|------------------------|
| UC-056 | US-188, US-189, US-190 | US-148, US-149, US-150, US-151 |
| UC-057 | US-191, US-192 | US-152 |
| UC-058 | US-193, US-194, US-195 | US-153, US-154, US-159 |
| UC-059 | US-196, US-197 | US-155, US-156 |
| UC-060 | US-198 | *(verificar en 009_user-stories.md)* |
| UC-061 | US-199 | US-157 |
| UC-062 | US-200 | US-158 |
| UC-063 | US-201, US-202 | **US-160** |

3. Actualizar línea resumen (línea ~21785):
   ```diff
   - **Total Transversal Import/Export:** 8 UCs cubriendo 15 User Stories (US-188 a US-202)
   + **Total Transversal Import/Export:** 8 UCs cubriendo 13 User Stories (US-148 a US-160)
   ```

**Verificación:**
- [x] Todas las referencias US-188→202 reemplazadas por US-148→160
- [x] Mapeo UC→US correcto según `009_user-stories.md`
- [x] Total de User Stories actualizado (15 → 13)

**Estimación:** 30 minutos

---

### Tarea 1.3: Completar Application Services Faltantes

**Problema:** 8 UCs sin campo "Application Service" definido

**UCs afectados:**
- UC-034: Eventos Específicos: Procesiones (Cofradías)
- UC-035: Eventos Específicos: Cuadrillas de Costaleros
- UC-036: Eventos Específicos: Cultos (Cofradías)
- UC-037: Eventos Específicos: Competiciones (Clubes Deportivos)
- UC-039: Envío de Comunicaciones por Email
- UC-040: Envío de SMS para Urgencias
- UC-041: Notificaciones Push vía PWA

**Acciones:**
1. Para cada UC, añadir campo:
   ```markdown
   **Application Service:** [NombreBC]Service.[metodo]
   ```

**Propuestas:**
- UC-034: `EventoService.registrarProcesion()`
- UC-035: `EventoService.gestionarCuadrilla()`
- UC-036: `EventoService.registrarCulto()`
- UC-037: `EventoService.registrarCompeticion()`
- UC-039: `ComunicacionService.enviarEmail()`
- UC-040: `ComunicacionService.enviarSMS()`
- UC-041: `ComunicacionService.enviarPushNotification()`

**Verificación:**
- [x] 7 UCs tienen campo "Application Service" definido
- [x] Nombres de Application Services siguen convención `[BC]Service.[metodo]`

**Estimación:** 2 horas

---

### Tarea 1.4: Actualizar KB-005 con Aggregates Críticos

**Problema:** 10 Aggregates usados en UCs pero NO definidos en KB-005

**Aggregates faltantes (prioridad ALTA):**
1. **ListaEspera** (BC-Membresia) → UC-014 bloqueado
2. **ExpedienteDisciplinario** (BC-Membresia) → Requisito legal N3RF26
3. **EnlacePago** (BC-Tesoreria) → Pagos online N4RF24-27

**Aggregates faltantes (prioridad MEDIA):**
4. TurnoCaja (BC-Tesoreria - extensión peñas)
5. CategoriaContable (BC-Tesoreria)
6. EjercicioContable (BC-Tesoreria)
7. EventoComida (BC-Eventos - extensión peñas)
8. Cuadrilla (BC-Eventos - extensión cofradías)
9. Partido (BC-Eventos - extensión clubes)
10. AlertaLegal (BC-Cumplimiento)

**Acciones:**
1. Abrir `005_modelo-dominio.md`
2. Para cada Aggregate, añadir definición:
   ```markdown
   #### Aggregate: [Nombre]
   
   **Descripción:** [2-3 líneas]
   
   **Invariantes:**
   - [Regla de negocio 1]
   - [Regla de negocio 2]
   
   **Entidades hijas:** [Si aplica]
   
   **Value Objects:** [Si aplica]
   ```

3. Actualizar Context Map si hay nuevas relaciones entre BCs

**Verificación:**
- [x] 10 Aggregates documentados en KB-005
- [x] Cada Aggregate tiene descripción e invariantes
- [x] Context Map actualizado si aplica

**Estimación:** 2-3 horas

---

### Tarea 1.5: Crear UCs de N11 Cumplimiento Normativo

**Problema:** 15 User Stories de N11 sin cobertura (0%), 8 con prioridad Must

**Acciones:**

Crear **5 nuevos Casos de Uso:**

#### UC-072: Gestión RGPD y Consentimientos
- **User Stories:** US-188, US-189, US-194
- **Prioridad:** Must Have
- **Bounded Context:** BC-Cumplimiento (nuevo) o Transversal
- **Funcionalidad:** Registro de consentimientos RGPD, gestión de bases legales

#### UC-073: Ejercicio de Derechos ARCO
- **User Stories:** US-190, US-191, US-192, US-193
- **Prioridad:** Must Have
- **Bounded Context:** BC-Cumplimiento
- **Funcionalidad:** Acceso, Rectificación, Cancelación, Oposición de datos personales

#### UC-074: Cumplimiento Ley de Asociaciones
- **User Stories:** US-195, US-196, US-197, US-198
- **Prioridad:** Must Have (US-195)
- **Bounded Context:** BC-Cumplimiento
- **Funcionalidad:** Registro de asociados, libro de actas, comunicación al Registro

#### UC-075: Alertas Legales Automáticas
- **User Stories:** US-199, US-200
- **Prioridad:** Should Have
- **Bounded Context:** BC-Cumplimiento
- **Funcionalidad:** Alertas de renovación de seguros, caducidad de licencias

#### UC-076: Alertas Fiscales y Tributarias
- **User Stories:** US-201, US-202
- **Prioridad:** Should Have
- **Bounded Context:** BC-Cumplimiento
- **Funcionalidad:** Alertas de plazos fiscales, caducidad de documentos

**Estructura por UC:**
- Metadata completa (US, BC, Service, Aggregates, Priority)
- Descripción (2-3 párrafos)
- Actores
- Precondiciones
- Flujo Normal (pasos numerados)
- Flujos Alternativos (si aplica)
- Flujos de Excepción
- Domain Events
- Postcondiciones
- Notas de Implementación

**Verificación:**
- [x] 5 nuevos UCs creados (UC-072 a UC-076)
- [x] 15 User Stories de N11 cubiertas (100% cobertura)
- [x] Estructura completa y homogénea con UCs existentes
- [x] BC-Cumplimiento definido en KB-005 si se crea como BC nuevo

**Estimación:** 3-4 horas

---

### 📊 Checklist Fase 1

- [x] Tarea 1.1: UC-063 generado (1-2h)
- [x] Tarea 1.2: Numeración US corregida (30min)
- [x] Tarea 1.3: Application Services añadidos (2h)
- [x] Tarea 1.4: KB-005 actualizado con Aggregates (2-3h)
- [x] Tarea 1.5: UCs N11 creados (3-4h)

**Total Fase 1:** 10-12 horas

**Resultado esperado:** 0 bloqueantes críticos, calidad mínima alcanzada

---

## FASE 2: COMPLETITUD METADATA 🟠

**Objetivo:** Alcanzar 100% de metadata completa en todos los UCs  
**Duración:** 1 semana  
**Esfuerzo:** 8-10 horas  
**Prioridad:** ALTA

---

### Tarea 2.1: Completar Import/Export (UC-056 a UC-062)

**Problema:** 6 UCs con patrón común de metadata incompleta

**UCs afectados:** UC-057, UC-058, UC-059, UC-060, UC-061, UC-062

**Campos faltantes comunes:**
- Domain Events Emitidos
- Poscondiciones

**Acciones:**
1. Para cada UC, añadir sección "Domain Events" si falta
2. Para cada UC, añadir sección "Poscondiciones" si falta
3. Revisar que User Stories sean correctas (ver Tarea 1.2)

**Ejemplo Domain Event para exportaciones:**
```markdown
#### Eventos de Dominio

- **ExportacionSolicitada** → Sistema de Tareas (procesar en background)
- **ExportacionCompletada** → BC-Comunicacion (notificar al usuario)
- **ExportacionFallida** → Sistema de Alertas
```

**Ejemplo Poscondiciones:**
```markdown
#### Postcondiciones

**Éxito:**
- Archivo exportado generado en S3
- Usuario notificado con enlace de descarga
- Registro de auditoría de la exportación

**Fallo:**
- Error registrado en logs
- Usuario notificado del fallo
```

**Verificación:**
- [ ] 6 UCs tienen sección "Domain Events"
- [ ] 6 UCs tienen sección "Poscondiciones"
- [ ] Formato homogéneo con resto de UCs

**Estimación:** 2 horas

---

### Tarea 2.2: Completar Reporting (UC-064 a UC-067)

**Problema:** 4 UCs con metadata incompleta

**UCs afectados:** UC-064, UC-065, UC-066, UC-067

**Campos faltantes comunes:**
- User Stories (algunos)
- Aggregates Involucrados
- Domain Events Emitidos
- Notas de Implementación

**Acciones:**
1. Verificar User Stories de N9 en `009_user-stories.md` (US-161 a US-172)
2. Asignar User Stories correctas a cada UC
3. Definir Aggregates involucrados (probablemente Transversal: lectura de múltiples BCs)
4. Documentar Domain Events si aplica
5. Añadir Notas de Implementación (tecnologías de reporting: Chart.js, PDF generation)

**Verificación:**
- [ ] 4 UCs tienen User Stories asignadas
- [ ] 4 UCs tienen Aggregates documentados
- [ ] 4 UCs tienen Domain Events (si aplica)
- [ ] 4 UCs tienen Notas de Implementación

**Estimación:** 2 horas

---

### Tarea 2.3: Completar Portal Socio (UC-068 a UC-071)

**Problema:** 4 UCs con metadata incompleta

**UCs afectados:** UC-068, UC-069, UC-070, UC-071

**Campos faltantes comunes:**
- Similar a Reporting (capa de presentación)

**Acciones:**
1. Verificar User Stories de N10 en `009_user-stories.md` (US-173 a US-187)
2. Asignar User Stories correctas
3. Definir Application Services (probablemente `PortalSocioService`)
4. Documentar interacciones con BCs backend

**Verificación:**
- [ ] 4 UCs tienen metadata completa
- [ ] Interacciones con BCs documentadas

**Estimación:** 2 horas

---

### Tarea 2.4: Completar BC-Documentos (UC-053 a UC-055)

**Problema:** 3 UCs con secciones incompletas

**UCs afectados:** UC-053, UC-054, UC-055

**Campos faltantes:**
- US-141, US-147 sin asignar (según reporte de trazabilidad)
- Flujo Normal faltante en algunos

**Acciones:**
1. Asignar User Stories faltantes
2. Completar Flujo Normal donde falte
3. Homogeneizar con resto de UCs del BC-Documentos

**Verificación:**
- [ ] 3 UCs tienen estructura completa
- [ ] User Stories faltantes asignadas

**Estimación:** 1 hora

---

### Tarea 2.5: Añadir Domain Events a 18 UCs Faltantes

**Problema:** 18 UCs sin sección "Domain Events Emitidos"

**Estrategia:**
1. Revisar cada UC y determinar si emite Domain Events
2. Si NO emite eventos (ej: operaciones de solo lectura), añadir nota:
   ```markdown
   #### Eventos de Dominio
   
   No emite Domain Events (operación de solo lectura).
   ```
3. Si SÍ emite eventos, documentarlos según formato estándar

**Verificación:**
- [ ] 18 UCs tienen sección "Domain Events"
- [ ] Eventos documentados o explícitamente marcados como N/A

**Estimación:** 2 horas

---

### Tarea 2.6: Añadir Poscondiciones a 18 UCs Faltantes

**Problema:** 18 UCs sin sección "Poscondiciones"

**Acciones:**
1. Para cada UC, documentar estados post-ejecución:
   - Escenario de éxito
   - Escenario de fallo
2. Usar formato estándar:
   ```markdown
   #### Postcondiciones
   
   **Éxito:**
   - [Cambios en el sistema]
   - [Notificaciones enviadas]
   - [Estado del aggregate]
   
   **Fallo:**
   - [Estado sin cambios]
   - [Usuario notificado del error]
   ```

**Verificación:**
- [ ] 18 UCs tienen sección "Poscondiciones"
- [ ] Formato homogéneo

**Estimación:** 1 hora

---

### 📊 Checklist Fase 2

- [ ] Tarea 2.1: Import/Export completado (2h)
- [ ] Tarea 2.2: Reporting completado (2h)
- [ ] Tarea 2.3: Portal Socio completado (2h)
- [ ] Tarea 2.4: BC-Documentos completado (1h)
- [ ] Tarea 2.5: Domain Events añadidos (2h)
- [ ] Tarea 2.6: Poscondiciones añadidas (1h)

**Total Fase 2:** 8-10 horas

**Resultado esperado:** 71 UCs con estructura 100% completa y homogénea

---

## FASE 3: DOMAIN EVENTS 🟡

**Objetivo:** Documentar arquitectura event-driven completa  
**Duración:** 3-5 días  
**Esfuerzo:** 6-8 horas  
**Prioridad:** MEDIA

---

### Tarea 3.1: Crear Tabla de Domain Events en KB-005 (BC-Comunicacion)

**Problema:** BC-Comunicacion no tiene tabla de Domain Events (0% documentado)

**Acciones:**
1. Abrir `005_modelo-dominio.md`
2. Localizar sección BC-Comunicacion
3. Añadir tabla de Domain Events:
   ```markdown
   ### Domain Events
   
   | Evento | Payload | Consumers |
   |--------|---------|-----------|
   | ComunicacionEnviada | { comunicacionId, destinatarios[], tipoCanal } | BC-Auditoría |
   | ComunicacionFallida | { comunicacionId, motivo } | Sistema de Alertas |
   | ... | ... | ... |
   ```

**Verificación:**
- [ ] Tabla de Domain Events creada en KB-005
- [ ] Eventos documentados con payload y consumers

**Estimación:** 1 hora

---

### Tarea 3.2: Crear Tabla de Domain Events en KB-005 (BC-Documentos)

**Problema:** BC-Documentos no tiene tabla de Domain Events

**Acciones:** Similar a Tarea 3.1

**Estimación:** 1 hora

---

### Tarea 3.3: Documentar 53 Domain Events Emergentes

**Problema:** 53 eventos usados en UCs pero NO documentados en KB-005

**Distribución:**
- BC-Tesoreria: 22 eventos
- BC-Membresia: 8 eventos
- BC-Eventos: 12 eventos
- BC-Comunicacion: 6 eventos
- BC-Documentos: 5 eventos

**Acciones:**
1. Extraer todos los eventos mencionados en "#### Eventos de Dominio" de los UCs
2. Para cada evento, añadir entrada en tabla de KB-005:
   - Nombre del evento
   - Payload (campos principales)
   - Consumers (qué BCs lo consumen)
3. Revisar consistencia: mismo evento debe tener mismo payload en todos los UCs

**Verificación:**
- [ ] 53 eventos documentados en KB-005
- [ ] Payload consistente entre UCs y KB-005
- [ ] Consumers documentados

**Estimación:** 3-4 horas

---

### Tarea 3.4: Validar Consumers de Domain Events

**Problema:** 3 eventos con inconsistencias de consumers detectadas

**Eventos afectados:**
1. **SocioRegistrado:** Algunos UCs dicen "→ BC-Tesoreria", otros no mencionan BC-Comunicacion
2. **EstadoSocioCambiado:** No se documenta notificación a BC-Comunicacion ni impacto en BC-Eventos
3. **Payloads sin campos de auditoría:** Faltan `fechaEvento`, `usuarioId` en algunos eventos

**Acciones:**
1. Revisar cada evento y establecer lista canónica de consumers
2. Actualizar todos los UCs que usan ese evento con la lista correcta
3. Añadir campos de auditoría (`fechaEvento`, `usuarioId`, `tenantId`) a todos los eventos

**Verificación:**
- [ ] 3 eventos con consumers consistentes en todos los UCs
- [ ] Todos los eventos tienen campos de auditoría

**Estimación:** 1-2 horas

---

### 📊 Checklist Fase 3

- [ ] Tarea 3.1: Tabla Events BC-Comunicacion (1h)
- [ ] Tarea 3.2: Tabla Events BC-Documentos (1h)
- [ ] Tarea 3.3: 53 eventos documentados (3-4h)
- [ ] Tarea 3.4: Consumers validados (1-2h)

**Total Fase 3:** 6-8 horas

**Resultado esperado:** Catálogo completo de Domain Events, arquitectura event-driven clara

---

## FASE 4: CALIDAD Y CONSISTENCIA 🟢

**Objetivo:** Pulir detalles y asegurar coherencia global  
**Duración:** 2-3 días  
**Esfuerzo:** 4-6 horas  
**Prioridad:** BAJA

---

### Tarea 4.1: Resolver 7 User Stories Duplicadas

**Problema:** 7 US referenciadas en múltiples UCs

**User Stories duplicadas:**
- US-112: UC-036, UC-037 (BC-Eventos)
- US-116: UC-041, UC-045 (BC-Comunicacion)
- US-117: UC-041, UC-042 (BC-Comunicacion)
- US-120: UC-043, UC-044 (BC-Comunicacion)
- US-122: UC-044, UC-046 (BC-Comunicacion)
- US-123: UC-045, UC-046 (BC-Comunicacion)
- US-124: UC-045, UC-046 (BC-Comunicacion)

**Acciones:**
1. Revisar cada par de UCs que comparten US
2. Determinar si:
   - Opción A: US es transversal (válido que aparezca en múltiples UCs) → Añadir nota explicativa
   - Opción B: US está mal asignada → Reasignar a un solo UC
3. Documentar decisión en sección de Notas de Trazabilidad

**Verificación:**
- [ ] 7 US duplicadas revisadas
- [ ] Decisión documentada (mantener o reasignar)
- [ ] Nota explicativa si se mantiene duplicación

**Estimación:** 1 hora

---

### Tarea 4.2: Actualizar Tablas Resumen con Conteos Correctos

**Problema:** Inconsistencias en tablas de resumen ejecutivo

**Ubicaciones a corregir:**
1. Línea ~55-67 (Resumen Ejecutivo)
2. Línea ~30099-30110 (Resumen Final)

**Correcciones:**
```diff
- | BC-Membresia          | 10 | 34 | Core         |
+ | BC-Membresia          | 11 | 34 | Core         |

- | Import/Export (N8)    | 8  | 13 | Cross-cutting|
+ | Import/Export (N8)    | 8  | 13 | Cross-cutting| (si se añade UC-063)

- | TOTAL                 | 71 | 202|              |
+ | TOTAL                 | 76 | 202|              | (si se añaden UC-072 a UC-076)
```

**Verificación:**
- [ ] Tablas actualizadas en ambas ubicaciones
- [ ] Conteos coinciden con UCs reales

**Estimación:** 15 minutos

---

### Tarea 4.3: Validar Referencias RNF-T-XXX contra KB-008

**Problema:** Verificar que todas las referencias a RNF-T-XXX sean válidas

**Acciones:**
1. Extraer todas las menciones de "RNF-T-" en `010_casos-uso_FINAL-HUMANO.md`
2. Verificar que cada RNF-T-XXX exista en `008_rnf-tecnicos.md`
3. Identificar referencias rotas o typos (ej: RNF-T-999)

**Verificación:**
- [ ] Todas las referencias RNF-T-XXX son válidas
- [ ] No hay typos en números de RNF

**Estimación:** 1-2 horas

---

### Tarea 4.4: Análisis de Consistencia de Reglas de Negocio

**Problema:** Verificar que no haya contradicciones en reglas de negocio entre UCs relacionados

**Áreas a revisar:**
1. **Estados del socio:** Verificar que transiciones de estado sean consistentes (UC-007, UC-011, UC-013, UC-022)
2. **Flujos de pago:** Verificar que proceso de pago sea consistente (UC-020, UC-021, UC-022, UC-023, UC-024, UC-025)
3. **Gestión de eventos:** Verificar que lógica de inscripciones sea consistente (UC-030, UC-031, UC-032)
4. **Permisos RBAC:** Verificar que actores autorizados sean consistentes entre UCs del mismo BC

**Acciones:**
1. Crear matriz de reglas de negocio por área
2. Identificar contradicciones
3. Resolver inconsistencias con decisión de diseño documentada

**Verificación:**
- [ ] Reglas de negocio consistentes en UCs relacionados
- [ ] Contradicciones resueltas y documentadas

**Estimación:** 2-3 horas

---

### Tarea 4.5: Revisión Ortográfica y Formato

**Problema:** Pulir ortografía, gramática y formato markdown

**Acciones:**
1. Ejecutar corrector ortográfico en español
2. Verificar formato markdown:
   - Títulos consistentes (## vs ### vs ####)
   - Listas con formato uniforme
   - Bloques de código con lenguaje especificado
3. Verificar enlaces internos (anclas) funcionan correctamente

**Verificación:**
- [ ] Sin errores ortográficos críticos
- [ ] Formato markdown consistente
- [ ] Enlaces internos verificados

**Estimación:** 1 hora

---

### 📊 Checklist Fase 4

- [ ] Tarea 4.1: US duplicadas resueltas (1h)
- [ ] Tarea 4.2: Tablas actualizadas (15min)
- [ ] Tarea 4.3: Referencias RNF-T validadas (1-2h)
- [ ] Tarea 4.4: Reglas de negocio consistentes (2-3h)
- [ ] Tarea 4.5: Ortografía y formato (1h)

**Total Fase 4:** 4-6 horas

**Resultado esperado:** Documento pulido, consistente y listo para defensa de TFM

---

## 📈 RESUMEN DE ESTIMACIONES

| Fase | Duración | Horas | Tareas | Estado |
|------|----------|-------|--------|--------|
| **Fase 1: Críticos** | 3-4 días | 10-12h | 5 | 🔴 Urgente |
| **Fase 2: Metadata** | 1 semana | 8-10h | 6 | 🟠 Alta |
| **Fase 3: Events** | 3-5 días | 6-8h | 4 | 🟡 Media |
| **Fase 4: Calidad** | 2-3 días | 4-6h | 5 | 🟢 Baja |
| **TOTAL** | **2-3 semanas** | **28-36h** | **20** | |

---

## 🎯 CRITERIOS DE ACEPTACIÓN

### Fase 1 Completada:
- ✅ UC-063 documentado completo
- ✅ 0 errores de numeración de User Stories
- ✅ 100% de UCs tienen Application Service definido
- ✅ KB-005 actualizado con 10 Aggregates críticos
- ✅ N11 Cumplimiento con 100% cobertura (15 US, 5 UCs)

### Fase 2 Completada:
- ✅ 71+ UCs con estructura 100% completa
- ✅ 0 UCs sin Domain Events o Poscondiciones
- ✅ Metadata homogénea en todos los UCs

### Fase 3 Completada:
- ✅ 87 Domain Events documentados en KB-005
- ✅ 100% eventos con payload y consumers definidos
- ✅ 0 inconsistencias en consumers de eventos

### Fase 4 Completada:
- ✅ 0 User Stories duplicadas sin justificación
- ✅ Tablas resumen con conteos correctos
- ✅ 100% referencias RNF-T válidas
- ✅ 0 contradicciones en reglas de negocio
- ✅ Documento pulido y presentable

---

## 📅 CRONOGRAMA SUGERIDO

### Semana 1:
- **Lunes-Martes:** Fase 1 (Críticos) → 10-12h
- **Miércoles-Viernes:** Fase 2 (Metadata) → 8-10h

### Semana 2:
- **Lunes-Miércoles:** Fase 3 (Domain Events) → 6-8h
- **Jueves-Viernes:** Fase 4 (Calidad) → 4-6h

### Semana 3 (buffer):
- **Lunes-Martes:** Revisión final y ajustes
- **Miércoles:** Validación completa
- **Jueves-Viernes:** Preparación presentación TFM

---

## ✅ SIGUIENTE PASO INMEDIATO

**ACCIÓN RECOMENDADA:**

Comenzar con **Tarea 1.1: Generar UC-063 Completo**

**Razón:** Es el bloqueante más crítico y tiene plantilla completa disponible en `REPORTE_UC063_UC016.md`

**Comando para empezar:**
```bash
# Abrir editor en línea ~25400 (después de UC-062)
# Copiar plantilla UC-063 de REPORTE_UC063_UC016.md
# Guardar y verificar con:
grep -n "^### UC-063" 010_casos-uso_FINAL-HUMANO.md
```

---

**Fecha de generación:** 06 Febrero 2026  
**Última actualización:** 06 Febrero 2026
