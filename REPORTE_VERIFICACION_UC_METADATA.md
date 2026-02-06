# REPORTE DE VERIFICACIÓN DE METADATA - CASOS DE USO

**Proyecto:** Associated - ERP para Colectividades Españolas  
**Documento:** 010_casos-uso_FINAL-HUMANO.md  
**Fecha de verificación:** 06 de Febrero de 2026  
**Versión:** 1.0

---

## 📊 RESUMEN EJECUTIVO

| Métrica | Valor | Porcentaje |
|---------|-------|------------|
| **Total UCs documentados** | 70 | - |
| **Total UCs esperados** | 71 | - |
| **UCs con metadata completa** | 45 | 64.3% |
| **UCs con metadata incompleta** | 25 | 35.7% |
| **UCs faltantes en secuencia** | 1 (UC-063) | - |

### Estructura Esperada por UC

Cada UC debe contener:

#### Campos de Metadata:
- ✅ **User Stories:** US-XXX, US-YYY
- ✅ **Bounded Context:** BC-[Nombre]
- ✅ **Application Service:** [Nombre]AppService
- ✅ **Aggregates Involucrados:** [Lista]
- ✅ **Prioridad:** Must Have | Should Have | Could Have

#### Secciones Obligatorias:
- ✅ **Descripción:** 2-3 párrafos explicativos
- ✅ **Actores:** Lista de actores que participan
- ✅ **Precondiciones:** Condiciones que deben cumplirse
- ✅ **Flujo Normal:** Pasos numerados del happy path
- ✅ **Domain Events Emitidos:** Eventos generados
- ✅ **Poscondiciones:** Estado resultante del sistema
- ✅ **Notas de Implementación:** Detalles técnicos

---

## ✅ UCs CON METADATA COMPLETA (45 casos)

Los siguientes UCs tienen todos los campos de metadata y secciones requeridas:

### BC-Identidad (5 UCs)
- ✅ UC-001: Provisión de nuevo tenant
- ✅ UC-002: Autenticación multi-tenant
- ✅ UC-003: Configuración de tenant
- ✅ UC-004: Gestión de roles y permisos
- ✅ UC-005: Traspaso de cargos directivos

### BC-Membresia (10 UCs)
- ✅ UC-006: Gestión de ficha de socio
- ✅ UC-007: Gestión de estados del socio
- ✅ UC-008: Configuración de tipos de socio
- ✅ UC-009: Gestión de antigüedad e historial
- ✅ UC-010: Gestión de ejercicios
- ✅ UC-011: Alta simple de socio
- ✅ UC-012: Alta compleja con workflow (cofradías)
- ✅ UC-013: Baja de socio
- ✅ UC-014: Gestión de lista de espera
- ✅ UC-015: Generación y validación de carnets

### BC-Tesoreria (11 UCs)
- ✅ UC-017: Configuración de planes de cuota
- ✅ UC-018: Gestión de suscripciones de cuota
- ✅ UC-019: Generación masiva de cargos periódicos
- ✅ UC-020: Gestión de cargos manuales
- ✅ UC-021: Registro de cobros
- ✅ UC-022: Workflow de morosidad
- ✅ UC-023: Generación de remesas SEPA
- ✅ UC-024: Gestión de devoluciones SEPA
- ✅ UC-025: Pasarela de pago online
- ✅ UC-026: Registro contable
- ✅ UC-027: Caja por turnos (peñas)

### BC-Eventos (7 UCs)
- ✅ UC-028: Registro y Configuración de Eventos
- ✅ UC-029: Calendario y Sincronización
- ✅ UC-030: Inscripciones Online
- ✅ UC-031: Control de Aforo y Listas de Espera
- ✅ UC-032: Check-in y Control de Asistencia
- ✅ UC-033: Eventos Específicos: Comidas Populares (Peñas)
- ✅ UC-038: Valoraciones y feedback de eventos

### BC-Comunicacion (6 UCs)
- ✅ UC-042: Gestión de plantillas de comunicación
- ✅ UC-043: Segmentación de destinatarios
- ✅ UC-044: Programación de envíos
- ✅ UC-045: Histórico y tracking de comunicaciones
- ✅ UC-046: Tablón de anuncios interno
- ✅ UC-047: Comunicaciones automáticas (Event Handlers)

### BC-Documentos (5 UCs)
- ✅ UC-048: Gestión de libro de actas digital
- ✅ UC-049: Registro de asistentes y cálculo de quórum
- ✅ UC-050: Archivo histórico y consulta de actas
- ✅ UC-051: Repositorio centralizado de documentos
- ✅ UC-052: Subida y previsualización de documentos

### Transversal: Import/Export (1 UC)
- ✅ UC-056: Importación Masiva de Socios

---

## ⚠️ UCs CON METADATA INCOMPLETA (25 casos)

### 🔴 CRÍTICO: UC Fusionado (1 caso)

#### UC-016: [FUSIONADO CON UC-015]
**Problema:** UC marcado como fusionado pero conserva entrada en la estructura  
**Impacto:** Confusión en conteo de UCs totales  
**Campos faltantes:**
- User Stories
- Bounded Context
- Application Service
- Aggregates
- Prioridad

**Secciones faltantes:**
- Descripción
- Actores
- Precondiciones
- Flujo Normal
- Domain Events Emitidos
- Poscondiciones
- Notas de Implementación

**Recomendación:** Eliminar entrada completa o convertir en nota explicativa sin numeración UC

---

### 🟠 ALTA PRIORIDAD: Metadata incompleta (8 casos)

#### UC-034: Eventos Específicos: Procesiones (Cofradías)
**Campos faltantes:**
- Application Service

**Secciones faltantes:**
- Poscondiciones
- Notas de Implementación

---

#### UC-035: Eventos Específicos: Cuadrillas de Costaleros
**Campos faltantes:**
- Application Service

---

#### UC-036: Eventos Específicos: Cultos (Cofradías)
**Campos faltantes:**
- Application Service

---

#### UC-037: Eventos Específicos: Competiciones (Clubes Deportivos)
**Campos faltantes:**
- Application Service

---

#### UC-039: Envío de Comunicaciones por Email
**Campos faltantes:**
- Application Service

---

#### UC-040: Envío de SMS para Urgencias
**Campos faltantes:**
- Application Service

**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones

---

#### UC-041: Notificaciones Push vía PWA
**Campos faltantes:**
- Application Service

**Secciones faltantes:**
- Actores
- Precondiciones
- Domain Events Emitidos

---

#### UC-053: Búsqueda y Filtrado de Documentos
**Campos faltantes:**
- User Stories

---

### 🟡 MEDIA PRIORIDAD: Import/Export y Reporting (9 casos)

#### UC-054: Control de Permisos y Límites de Almacenamiento
**Secciones faltantes:**
- Flujo Normal

---

#### UC-055: Control de Versiones y OCR Avanzado
**Secciones faltantes:**
- Flujo Normal

---

#### UC-057: Importación de Histórico de Pagos
**Campos faltantes:**
- User Stories

**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones

---

#### UC-058: Exportación de Listados y Plantillas
**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones

---

#### UC-059: Exportación de informes económicos
**Campos faltantes:**
- User Stories
- Aggregates

**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones
- Notas de Implementación

---

#### UC-060: Exportación fiscal Modelo 182 (AEAT)
**Campos faltantes:**
- User Stories
- Aggregates

**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones
- Notas de Implementación

---

#### UC-061: Exportación de listados de eventos
**Campos faltantes:**
- User Stories
- Aggregates

**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones
- Notas de Implementación

---

#### UC-062: Exportación de histórico de comunicaciones
**Campos faltantes:**
- User Stories
- Aggregates

**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones
- Notas de Implementación

---

#### UC-064: Dashboard Principal y KPIs
**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones

---

### 🟡 MEDIA PRIORIDAD: Reporting (continuación) (3 casos)

#### UC-065: Gráficos de evolución y dashboards interactivos
**Campos faltantes:**
- User Stories
- Aggregates

**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones
- Notas de Implementación

---

#### UC-066: Informes para Asamblea General
**Campos faltantes:**
- User Stories
- Aggregates

**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones
- Notas de Implementación

---

#### UC-067: Certificados y memorias personalizables
**Campos faltantes:**
- User Stories
- Aggregates

**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones
- Notas de Implementación

---

### 🟢 BAJA PRIORIDAD: Portal Socio (4 casos)

#### UC-068: Acceso al Portal y Autenticación
**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones

---

#### UC-069: Consulta de Datos Personales y Cuotas
**Secciones faltantes:**
- Precondiciones
- Domain Events Emitidos
- Poscondiciones
- Notas de Implementación

---

#### UC-070: Inscripción a eventos desde portal
**Campos faltantes:**
- User Stories
- Aggregates

**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones
- Notas de Implementación

---

#### UC-071: Descarga de documentos personales
**Campos faltantes:**
- User Stories
- Aggregates

**Secciones faltantes:**
- Domain Events Emitidos
- Poscondiciones
- Notas de Implementación

---

## 🚨 UC FALTANTE EN SECUENCIA

### UC-063: [NO EXISTE EN EL DOCUMENTO]

**Contexto:** La secuencia de UCs salta de UC-062 a UC-064  
**Impacto:** Inconsistencia en numeración secuencial  
**Ubicación esperada:** Entre sección "Transversal: Import/Export" y "Transversal: Reporting"  
**Bounded Context probable:** Transversal (Import/Export o Reporting)

**Recomendaciones:**
1. Verificar si fue eliminado intencionalmente o es un error de numeración
2. Si fue eliminado: renumerar UCs posteriores (UC-064 → UC-063, etc.)
3. Si falta: identificar qué User Story cubre y documentarlo
4. Actualizar matriz de trazabilidad en consecuencia

---

## 📈 ANÁLISIS POR BOUNDED CONTEXT

| BC | Total UCs | Completos | Incompletos | % Completitud |
|----|-----------|-----------|-------------|---------------|
| BC-Identidad | 5 | 5 | 0 | 100% ✅ |
| BC-Membresia | 11 | 10 | 1 | 90.9% ✅ |
| BC-Tesoreria | 11 | 11 | 0 | 100% ✅ |
| BC-Eventos | 11 | 7 | 4 | 63.6% ⚠️ |
| BC-Comunicacion | 9 | 6 | 3 | 66.7% ⚠️ |
| BC-Documentos | 8 | 5 | 3 | 62.5% ⚠️ |
| Transversal: Import/Export | 8 | 1 | 7 | 12.5% 🔴 |
| Transversal: Reporting | 4 | 0 | 4 | 0% 🔴 |
| Transversal: Portal Socio | 4 | 0 | 4 | 0% 🔴 |

### Observaciones:

1. **BCs Core (Identidad, Tesorería):** Excelente completitud (100%)
2. **BCs Core (Membresia, Eventos, Comunicacion, Documentos):** Buena completitud (62-91%)
3. **Funcionalidades Transversales:** Baja completitud (0-13%) 🔴
   - Estas secciones parecen estar en estado de **borrador** o **pendientes de detalle**

---

## 🎯 RECOMENDACIONES PRIORITARIAS

### 1. CRÍTICO - UC-016 (Fusionado)
**Acción:** Eliminar entrada completa del UC-016 o documentar correctamente  
**Justificación:** Genera confusión en conteo total de UCs (70 vs 71)

### 2. ALTA PRIORIDAD - UCs de Eventos Específicos (UC-034 a UC-037)
**Acción:** Completar campo "Application Service"  
**Campos faltantes:** 4 UCs sin Application Service  
**Impacto:** Falta claridad sobre responsabilidad de implementación  
**Sugerencia:**
- UC-034, UC-035, UC-036: `EventoEspecificoService` (BC-Eventos)
- UC-037: `CompeticionService` (BC-Eventos)

### 3. ALTA PRIORIDAD - UCs de Comunicación (UC-039, UC-040, UC-041)
**Acción:** Completar campo "Application Service" y secciones faltantes  
**Sugerencia:**
- UC-039: `EmailService`
- UC-040: `SmsService`
- UC-041: `PushNotificationService`

### 4. MEDIA PRIORIDAD - Sección Transversal: Import/Export
**Acción:** Completar 7 UCs incompletos (UC-057 a UC-062)  
**Patrón detectado:** Faltan "Domain Events Emitidos" y "Poscondiciones" en todos  
**Recomendación:** Template batch para completar estos UCs (estructura similar)

### 5. MEDIA PRIORIDAD - Sección Transversal: Reporting
**Acción:** Completar 4 UCs incompletos (UC-064 a UC-067)  
**Patrón detectado:** Faltan "User Stories", "Aggregates", "Domain Events" y "Notas de Implementación"  
**Recomendación:** Revisar si estos UCs son principalmente consultas (pocos eventos de dominio)

### 6. BAJA PRIORIDAD - Portal Socio
**Acción:** Completar 4 UCs (UC-068 a UC-071)  
**Justificación:** Portal Socio es capa de presentación, menor impacto en arquitectura core

### 7. INVESTIGAR - UC-063 Faltante
**Acción:** Verificar si es error de numeración o UC eliminado  
**Opciones:**
- Renumerar UCs posteriores
- Documentar UC-063 si corresponde a User Stories sin cubrir

---

## 📋 CHECKLIST DE ACCIÓN

### Fase 1: Correcciones Críticas (1-2 horas)
- [ ] Resolver UC-016 (fusionado)
- [ ] Investigar UC-063 (faltante)
- [ ] Completar Application Service en UC-034 a UC-041 (8 UCs)

### Fase 2: Completar Import/Export (2-3 horas)
- [ ] UC-057: Importación de Histórico de Pagos
- [ ] UC-058: Exportación de Listados y Plantillas
- [ ] UC-059: Exportación de informes económicos
- [ ] UC-060: Exportación fiscal Modelo 182
- [ ] UC-061: Exportación de listados de eventos
- [ ] UC-062: Exportación de histórico de comunicaciones

### Fase 3: Completar Reporting (2-3 horas)
- [ ] UC-064: Dashboard Principal y KPIs
- [ ] UC-065: Gráficos de evolución y dashboards interactivos
- [ ] UC-066: Informes para Asamblea General
- [ ] UC-067: Certificados y memorias personalizables

### Fase 4: Completar Portal Socio (1-2 horas)
- [ ] UC-068: Acceso al Portal y Autenticación
- [ ] UC-069: Consulta de Datos Personales y Cuotas
- [ ] UC-070: Inscripción a eventos desde portal
- [ ] UC-071: Descarga de documentos personales

### Fase 5: Completar Documentos (1 hora)
- [ ] UC-053: Búsqueda y Filtrado de Documentos
- [ ] UC-054: Control de Permisos y Límites de Almacenamiento
- [ ] UC-055: Control de Versiones y OCR Avanzado

---

## 🔍 PATRONES DETECTADOS

### Patrón 1: UCs Transversales Incompletos
**Observación:** Los UCs transversales (Import/Export, Reporting, Portal) tienen menor completitud  
**Hipótesis:** Documentación priorizada en BCs Core primero  
**Recomendación:** Aplicar templates consistentes a secciones transversales

### Patrón 2: Secciones más faltantes
1. **Domain Events Emitidos:** Falta en 18 UCs (72% de incompletos)
2. **Poscondiciones:** Falta en 18 UCs (72% de incompletos)
3. **Notas de Implementación:** Falta en 10 UCs (40% de incompletos)

**Acción:** Priorizar estas 3 secciones en revisión

### Patrón 3: Campos de metadata más faltantes
1. **Application Service:** Falta en 8 UCs
2. **User Stories:** Falta en 8 UCs
3. **Aggregates:** Falta en 8 UCs

**Acción:** Revisar trazabilidad US → UC en estos casos

---

## ✅ CONCLUSIÓN

El documento presenta una **buena base de trabajo** con:
- ✅ **64.3% de UCs completamente documentados**
- ✅ **BCs Core al 100%** (Identidad, Tesorería)
- ⚠️ **Secciones transversales requieren completar** (Import/Export, Reporting, Portal)

### Estimación de esfuerzo para completar:
- **Total horas:** 8-12 horas de trabajo
- **Prioridad:** Completar UC-034 a UC-041 primero (Application Services)
- **Impacto:** Alcanzar **100% de completitud** en metadata

### Estado final esperado:
- 71 UCs documentados (resolver UC-016 y UC-063)
- 100% de UCs con metadata completa
- Trazabilidad completa: 202 User Stories → 71 UCs → Application Services → BCs

---

**Fin del reporte**  
**Generado automáticamente por:** check_uc_metadata.py v1.0
