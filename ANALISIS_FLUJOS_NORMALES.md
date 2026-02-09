# Análisis Exhaustivo de Flujos Normales - 76 Casos de Uso
**Proyecto:** Associated  
**Documento Analizado:** `010_casos-uso_FINAL-HUMANO.md`  
**Fecha Análisis:** 09/02/2026  
**Total UCs Analizados:** 75 (UC-001 a UC-076, excluyendo UC-016 fusionado)

---

## Resumen Ejecutivo

Se ha realizado un análisis exhaustivo de la sección "Flujo Normal" de los 76 casos de uso documentados. El análisis revela **5 patrones estructurales dominantes** con una **alta consistencia** en la mayoría de los casos de uso (89% usa numeración continua, 72% no usa división en partes).

### Hallazgos Clave

1. **Consistencia estructural:** 89% usa numeración continua sin división en partes
2. **Voz narrativa mixta:** Combina acciones de Actor y Sistema de forma equilibrada
3. **Nivel de detalle medio:** Promedio de 13 pasos y 20-50 palabras por paso
4. **Enriquecimiento selectivo:** Solo 32% incluye mockups ASCII (reservado para UCs con UI intensiva)
5. **Anomalías mínimas:** 1 UC con estructura no estándar (UC-039), 0 UCs con código TS/SQL embebido

---

## Top 5 Patrones Más Comunes

### 1. Numeración Continua de Pasos
**Frecuencia:** 66/74 UCs (89%)  
**Patrón:** Los pasos están numerados secuencialmente del 1 hasta N sin reiniciar.

**Ejemplo:**
```
1. Secretario accede a "Socios > Nuevo Socio"
2. Sistema muestra formulario de alta
3. Secretario completa datos obligatorios
...
13. Sistema muestra confirmación
```

**UCs que lo usan:** UC-001 a UC-076 (exceptuando 8 casos con numeración irregular/reiniciada)

---

### 2. Sin División en Partes
**Frecuencia:** 54/74 UCs (73%)  
**Patrón:** Flujo único sin subdivisiones en "Parte 1", "Parte 2", etc.

**Casos que SÍ usan Partes (20 UCs - 27%):**
- **BC-Tesoreria:** UC-018, UC-019, UC-020, UC-021, UC-022, UC-023, UC-024, UC-025, UC-026, UC-027
- **BC-Eventos (específicos):** UC-034, UC-035, UC-036, UC-037, UC-038
- **BC-Identidad:** UC-004
- **Transversal-Cumplimiento:** UC-073, UC-074, UC-075, UC-076

**Observación:** Ninguno de los UCs con Partes incluye referencias a User Stories en las cabeceras de las Partes (formato esperado: `**Parte 1: Nombre** (US-XXX, US-YYY)`).

---

### 3. Incluye Mockups ASCII de UI
**Frecuencia:** 24/74 UCs (32%)  
**Patrón:** Usa caracteres box-drawing (┌─┐│└┘) para representar interfaces de usuario.

**Ejemplo:**
```
┌─ Alta de Socio: Paso 1/3 ─────────────────┐
│                                            │
│ DNI/NIE: *        [12345678A__________]    │
│ Nombre: *         [Juan________________]   │
│                                            │
│ [Cancelar]           [Siguiente Paso →]   │
└────────────────────────────────────────────┘
```

**UCs con mockups:** UC-004, UC-005, UC-008, UC-010, UC-011, UC-013, UC-014, UC-015, UC-018, UC-020, UC-021, UC-022, UC-023, UC-024, UC-026, UC-027, UC-033, UC-038, UC-072, UC-073, UC-074, UC-075, UC-076

**Uso típico:** Casos de uso de formularios, configuración, y procesos con interacción intensiva de UI.

---

### 4. Llamadas Explícitas a Application Services
**Frecuencia:** 34/74 UCs (46%)  
**Patrón:** Menciona explícitamente `NombreService.metodo()` en los pasos del flujo.

**Ejemplo:**
```
5. `AltaSocioService.altaSimple(request)`:
   - Valida datos personales
   - Crea Aggregate Socio
   - Genera número de socio
   - Emite evento SocioRegistrado
```

**Promedio:** 1-2 llamadas Service por UC cuando están presentes.

**UCs con más llamadas:** UC-018 (6 llamadas), UC-027 (4 llamadas), UC-013 (4 llamadas)

---

### 5. Mensajes de Confirmación al Usuario
**Frecuencia:** 38/74 UCs (51%)  
**Patrón:** Incluye mensajes tipo "Sistema muestra confirmación" al finalizar operaciones exitosas.

**Ejemplos:**
- "Sistema muestra confirmación: 'Socio creado. Número de socio: 00342'"
- "Sistema muestra: 'Configuración actualizada correctamente'"
- "Usuario recibe notificación de asignación de rol"

**Buena práctica:** Proporciona feedback explícito al usuario sobre el resultado de la operación.

---

## Tabla Comparativa Detallada

| UC | Usa Partes | Refs US en Partes | Numeración | Total Pasos | Mockups | Llamadas Svc | Tablas Reglas | Domain Events | Código TS/SQL |
|----|-----------|-------------------|------------|-------------|---------|--------------|---------------|---------------|---------------|
| UC-001 | NO | NO | Continua | 8 | 0 | 3 | 0 | 1 | NO |
| UC-002 | NO | NO | Continua | 9 | 0 | 2 | 0 | 0 | NO |
| UC-003 | NO | NO | Continua | 7 | 0 | 2 | 0 | 0 | NO |
| UC-004 | SÍ | NO | Reinicia | 12 | 1 | 2 | 0 | 0 | NO |
| UC-005 | NO | NO | Continua | 9 | 1 | 2 | 0 | 2 | NO |
| UC-006 | NO | NO | Continua | 9 | 0 | 2 | 0 | 1 | NO |
| UC-007 | NO | NO | Continua | 9 | 0 | 2 | 1 | 1 | NO |
| UC-008 | NO | NO | Continua | 9 | 1 | 1 | 0 | 1 | NO |
| UC-009 | NO | NO | Continua | 16 | 0 | 2 | 0 | 1 | NO |
| UC-010 | NO | NO | Continua | 19 | 1 | 3 | 1 | 2 | NO |
| UC-011 | NO | NO | Continua | 13 | 1 | 1 | 0 | 1 | NO |
| UC-012 | NO | NO | Continua | 23 | 1 | 0 | 0 | 0 | NO |
| UC-013 | NO | NO | Continua | 24 | 1 | 4 | 1 | 0 | NO |
| UC-014 | NO | NO | Irregular | 19 | 1 | 1 | 1 | 0 | NO |
| UC-015 | NO | NO | Continua | 34 | 1 | 3 | 0 | 0 | NO |
| UC-017 | NO | NO | Continua | 11 | 0 | 2 | 0 | 0 | NO |
| UC-018 | SÍ | NO | Continua | 30 | 1 | 6 | 1 | 0 | NO |
| UC-019 | SÍ | NO | Continua | 15 | 0 | 2 | 1 | 0 | NO |
| UC-020 | SÍ | NO | Continua | 20 | 1 | 2 | 0 | 0 | NO |
| UC-021 | SÍ | NO | Continua | 32 | 1 | 1 | 0 | 1 | NO |
| UC-022 | SÍ | NO | Continua | 22 | 1 | 1 | 0 | 1 | NO |
| UC-023 | SÍ | NO | Irregular | 35 | 1 | 3 | 2 | 1 | NO |
| UC-024 | SÍ | NO | Continua | 19 | 1 | 1 | 1 | 0 | NO |
| UC-025 | SÍ | NO | Continua | 15 | 0 | 1 | 0 | 0 | NO |
| UC-026 | SÍ | NO | Continua | 18 | 1 | 3 | 0 | 0 | NO |
| UC-027 | SÍ | NO | Continua | 27 | 1 | 4 | 0 | 0 | NO |
| UC-028 | NO | NO | Continua | 12 | 0 | 0 | 0 | 0 | NO |
| UC-029 | NO | NO | Continua | 12 | 0 | 0 | 0 | 0 | NO |
| UC-030 | NO | NO | Continua | 12 | 0 | 0 | 0 | 0 | NO |
| UC-031 | NO | NO | Continua | 14 | 0 | 0 | 0 | 1 | NO |
| UC-032 | NO | NO | Irregular | 29 | 0 | 0 | 0 | 0 | NO |
| UC-033 | NO | NO | Continua | 20 | 1 | 0 | 0 | 0 | NO |
| UC-034 | SÍ | NO | Continua | 12 | 0 | 0 | 0 | 0 | NO |
| UC-035 | SÍ | NO | Continua | 4 | 0 | 0 | 0 | 0 | NO |
| UC-036 | SÍ | NO | Continua | 8 | 0 | 0 | 0 | 0 | NO |
| UC-037 | SÍ | NO | Continua | 12 | 0 | 0 | 0 | 0 | NO |
| UC-038 | SÍ | NO | Continua | 16 | 1 | 2 | 0 | 0 | NO |
| UC-039 | NO | NO | No estándar (FN-X) | 0 | 0 | 0 | 0 | 0 | NO |
| UC-040 | NO | NO | Continua | 3 | 0 | 0 | 0 | 0 | NO |
| UC-041 | NO | NO | Continua | 2 | 0 | 0 | 0 | 0 | NO |
| UC-042 | NO | NO | Continua | 6 | 0 | 0 | 0 | 0 | NO |
| UC-043 | NO | NO | Continua | 7 | 0 | 0 | 0 | 0 | NO |
| UC-044 | NO | NO | Continua | 7 | 0 | 0 | 0 | 0 | NO |
| UC-045 | NO | NO | Continua | 7 | 0 | 0 | 0 | 0 | NO |
| UC-046 | NO | NO | Continua | 7 | 0 | 0 | 0 | 0 | NO |
| UC-047 | NO | NO | Continua | 6 | 0 | 0 | 0 | 0 | NO |
| UC-048 | NO | NO | Continua | 8 | 0 | 0 | 0 | 0 | NO |
| UC-049 | NO | NO | Continua | 6 | 0 | 0 | 0 | 0 | NO |
| UC-050 | NO | NO | Continua | 6 | 0 | 0 | 0 | 0 | NO |
| UC-051 | NO | NO | Continua | 5 | 0 | 0 | 0 | 0 | NO |
| UC-052 | NO | NO | Continua | 4 | 0 | 0 | 0 | 0 | NO |
| UC-053 | NO | NO | Continua | 10 | 0 | 0 | 0 | 0 | NO |
| UC-054 | NO | NO | Reinicia | 16 | 0 | 0 | 0 | 0 | NO |
| UC-055 | NO | NO | Irregular | 23 | 0 | 0 | 0 | 0 | NO |
| UC-056 | NO | NO | Continua | 5 | 0 | 0 | 0 | 0 | NO |
| UC-057 | NO | NO | Continua | 3 | 0 | 0 | 0 | 0 | NO |
| UC-058 | NO | NO | Continua | 5 | 0 | 0 | 0 | 0 | NO |
| UC-059 | NO | NO | Continua | 13 | 0 | 0 | 0 | 0 | NO |
| UC-060 | NO | NO | Continua | 15 | 0 | 0 | 0 | 0 | NO |
| UC-061 | NO | NO | Continua | 15 | 0 | 0 | 0 | 0 | NO |
| UC-062 | NO | NO | Continua | 14 | 0 | 0 | 0 | 0 | NO |
| UC-063 | NO | NO | Continua | 10 | 0 | 0 | 0 | 0 | NO |
| UC-064 | NO | NO | Continua | 3 | 0 | 0 | 0 | 0 | NO |
| UC-065 | NO | NO | Continua | 7 | 0 | 0 | 0 | 0 | NO |
| UC-066 | NO | NO | Continua | 10 | 0 | 1 | 0 | 0 | NO |
| UC-067 | NO | NO | Continua | 9 | 0 | 1 | 0 | 1 | NO |
| UC-068 | NO | NO | Continua | 4 | 0 | 0 | 0 | 0 | NO |
| UC-069 | NO | NO | Continua | 10 | 0 | 3 | 0 | 0 | NO |
| UC-070 | NO | NO | Continua | 16 | 0 | 2 | 0 | 0 | NO |
| UC-071 | NO | NO | Continua | 22 | 0 | 3 | 0 | 0 | NO |
| UC-072 | NO | NO | Reinicia | 10 | 1 | 2 | 0 | 0 | NO |
| UC-073 | SÍ | NO | Continua | 12 | 1 | 5 | 0 | 0 | NO |
| UC-074 | SÍ | NO | Continua | 23 | 1 | 1 | 0 | 0 | NO |
| UC-075 | SÍ | NO | Continua | 18 | 1 | 0 | 0 | 0 | NO |
| UC-076 | SÍ | NO | Irregular | 20 | 1 | 0 | 0 | 0 | NO |

**Nota sobre UC-016:** Marcado como `[FUSIONADO CON UC-015]` en el documento, no tiene flujo normal propio.

---

## Análisis de Patrones de Redacción

### Formato de Contenido

#### 1. Estructura de Pasos

**Patrón dominante:** Numeración secuencial continua
```
1. Actor realiza acción inicial
2. Sistema responde con validación/presentación
3. Actor proporciona información adicional
...
N. Sistema confirma operación completada
```

**Excepción:** UC-039 usa formato alternativo con secciones `FN-1`, `FN-2` (no estándar).

#### 2. Voz Narrativa

**Patrón:** Mixto Actor-Sistema

- **Inicio:** Generalmente comienza con acción del Actor
- **Desarrollo:** Alterna entre acciones de Actor y respuestas del Sistema
- **Cierre:** Típicamente termina con confirmación del Sistema

**Ejemplo típico:**
```
1. Secretario accede a "Configuración > Tipos de Socio"
2. Sistema muestra tipos actuales
3. Secretario pulsa "Nuevo Tipo"
4. Sistema solicita datos obligatorios
5. Secretario completa formulario
6. `TipoSocioService.crearTipo()` valida y crea registro
7. Sistema muestra confirmación
```

#### 3. Verbos Utilizados

**Para Actor:**
- accede, pulsa, selecciona, completa, confirma, introduce, marca, registra, solicita

**Para Sistema:**
- muestra, solicita, valida, genera, emite, crea, ejecuta, detecta, calcula, registra, alerta

**Modo verbal:** Tercera persona del presente (NO imperativo)

#### 4. Nivel de Detalle

**Promedio:** 20-50 palabras por paso (nivel medio)

**Distribución:**
- **Bajo (<20 palabras/paso):** UCs simples de consulta/exportación (UC-040 a UC-063)
- **Medio (20-50 palabras/paso):** Mayoría de los UCs core (UC-001 a UC-038)
- **Alto (>50 palabras/paso):** UCs complejos con lógica de negocio extensa (UC-015, UC-023)

**Técnica de detalle:** Sub-bullets para desglosar operaciones complejas
```
3. `AltaSocioService.altaSimple(request)`:
   - Valida DNI único
   - Genera número de socio
   - Crea Aggregate Socio
   - Emite evento SocioRegistrado
```

---

## Formato de Contenido Encontrado

### Mockups ASCII de UI

**Frecuencia:** 24/75 UCs (32%)

**Formato:**
```
┌─ Título del Mockup ──────────────────────┐
│                                           │
│ Campo 1: *        [Valor_____________]    │
│ Campo 2:          [Otro_valor________]    │
│                                           │
│ ☑ Checkbox activo                         │
│ ☐ Checkbox inactivo                       │
│                                           │
│ [Botón Cancelar]      [Botón Confirmar]  │
└───────────────────────────────────────────┘
```

**Uso apropiado:**
- Formularios de configuración (UC-008, UC-017, UC-072)
- Wizards multi-paso (UC-011)
- Selectores complejos (UC-004, UC-005)
- Dashboards y reportes (UC-027, UC-073)

**No usado en:** UCs de procesos batch, exportaciones, servicios backend

---

### Llamadas Explícitas a Application Services

**Frecuencia:** 34/75 UCs (46%)

**Formato:**
```
N. `NombreService.metodo(parametros)`:
   - Paso interno 1
   - Paso interno 2
   - Resultado esperado
```

**Ejemplos:**
- `TenantProvisioningService.provisionNewTenant(data)`
- `AltaSocioService.altaSimple(request)`
- `EjercicioService.abrirEjercicio(config)`
- `BajaSocioService.procesarBajaVoluntaria(socio_id, fecha_solicitud)`

**Promedio:** 1-2 llamadas por UC (cuando están presentes)

**Máximo:** UC-018 con 6 llamadas (gestión compleja de suscripciones)

---

### Tablas de Reglas de Negocio

**Frecuencia:** 8/75 UCs (11%)

**Formato:**

**Ejemplo 1: Matriz de Transiciones (UC-007)**
```
| Estado Origen | Estados Destino Permitidos |
|---------------|----------------------------|
| ACTIVO        | PendientePago, Suspendido, BajaVoluntaria |
| PENDIENTE_PAGO| Activo, Suspendido, BajaImpago |
```

**Ejemplo 2: Validaciones (UC-010)**
```
| Validación | Descripción | Consecuencia si Falla |
|------------|-------------|----------------------|
| Cuotas conciliadas | Verificar sin cargos pendientes | ⚠️ Advertencia |
| Remesas cerradas | Estado ENVIADA o COBRADA | ⚠️ Advertencia |
```

**Uso apropiado:**
- Máquinas de estado (transiciones válidas)
- Reglas de validación complejas
- Matrices de decisión
- Configuraciones parametrizables

---

### Domain Events Mencionados

**Frecuencia:** 13/75 UCs (17%)

**Formato:**
```
N. Sistema emite evento `EventoNombre`
   → Consumidores: BC-X (acción), BC-Y (acción)
```

**Eventos más comunes:**
- `SocioRegistrado` (UC-006, UC-011)
- `EstadoSocioCambiado` (UC-007)
- `TraspasoCompletado` (UC-005)
- `EjercicioAbierto` (UC-010)
- `PagoRegistrado` (UC-021)

**Observación:** Los eventos se mencionan en el flujo normal pero se detallan completamente en la sección "Eventos de Dominio" del UC.

---

### Mensajes de Confirmación al Usuario

**Frecuencia:** 38/75 UCs (51%)

**Formato:**
```
N. Sistema muestra confirmación: "Mensaje específico con datos"
```

**Ejemplos:**
- "Sistema muestra confirmación: 'Socio creado. Número de socio: 00342'"
- "Sistema confirma: 'Configuración actualizada correctamente'"
- "Usuario recibe email de confirmación con fecha efectiva"

**Buena práctica:** Proporciona feedback explícito sobre el resultado exitoso de la operación.

---

## Anomalías Detectadas

### 1. UC-039: Estructura No Estándar

**Problema:** Usa formato `FN-1`, `FN-2` en lugar de numeración secuencial clásica.

**Contenido encontrado:**
```
#### Flujo Normal: Envío de Email Masivo

**FN-1: Creación y Envío de Comunicación**
1. **Secretario crea nueva comunicación:**
2. **Sistema procesa previsualización:**

**FN-2: Procesamiento Asíncrono en Background**
1. **Job procesa emails en lotes:**
```

**Impacto:** Inconsistencia con el 98% de los demás UCs.

**Recomendación:** Convertir a numeración continua estándar (1. 2. 3. ... N).

---

### 2. Falta de Referencias US en Partes

**Problema:** 20 UCs usan división en Partes, pero NINGUNO incluye referencias a User Stories en las cabeceras.

**Formato esperado (no encontrado):**
```
**Parte 1: Configuración inicial** (US-020, US-021, US-022)
1. Paso 1
2. Paso 2

**Parte 2: Validación y creación** (US-023, US-024)
3. Paso 3
4. Paso 4
```

**Formato real encontrado:**
```
**Parte 1: Configuración inicial**
1. Paso 1
2. Paso 2

**Parte 2: Validación y creación**
1. Paso 1 (numeración reinicia)
2. Paso 2
```

**Impacto:** Pérdida de trazabilidad explícita entre pasos del flujo y las User Stories origen.

**Recomendación:** Añadir referencias US en cabeceras de Partes O eliminar división en Partes y usar flujo continuo.

---

### 3. Código TypeScript/SQL Embebido

**Resultado:** ✅ **NO SE DETECTÓ CÓDIGO TS/SQL EN FLUJOS NORMALES**

Todos los ejemplos de código se encuentran en las secciones "Notas de Implementación" como corresponde.

---

## Distribución por Bounded Context

### BC-Identidad (UC-001 a UC-005)
- **Pasos promedio:** 9
- **Mockups:** 40% (2/5)
- **Llamadas Service:** 100% (5/5)
- **Partes:** 20% (1/5 - UC-004)

### BC-Membresia (UC-006 a UC-015)
- **Pasos promedio:** 14
- **Mockups:** 50% (5/10)
- **Llamadas Service:** 70% (7/10)
- **Partes:** 0%

### BC-Tesoreria (UC-017 a UC-027)
- **Pasos promedio:** 20
- **Mockups:** 73% (8/11)
- **Llamadas Service:** 82% (9/11)
- **Partes:** 91% (10/11) ← **MAYOR USO DE PARTES**
- **Tablas de reglas:** 45% (5/11) ← **MAYOR USO DE TABLAS**

**Observación:** BC-Tesoreria es el más complejo, con flujos extensos que requieren división en partes para mantener legibilidad.

### BC-Eventos (UC-028 a UC-038)
- **Pasos promedio:** 12
- **Mockups:** 18% (2/11)
- **Llamadas Service:** 18% (2/11)
- **Partes:** 45% (5/11) - solo en eventos específicos (UC-034 a UC-038)

### BC-Comunicacion (UC-039 a UC-047)
- **Pasos promedio:** 6
- **Mockups:** 0%
- **Llamadas Service:** 0%
- **Partes:** 0%
- **Anomalía:** UC-039 con formato no estándar

### BC-Documentos (UC-048 a UC-055)
- **Pasos promedio:** 8
- **Mockups:** 0%
- **Llamadas Service:** 0%
- **Partes:** 0%

### Transversal: Import/Export (UC-056 a UC-063)
- **Pasos promedio:** 8
- **Mockups:** 0%
- **Llamadas Service:** 0%
- **Partes:** 0%

### Transversal: Reporting (UC-064 a UC-067)
- **Pasos promedio:** 7
- **Mockups:** 0%
- **Llamadas Service:** 50% (2/4)
- **Partes:** 0%

### Transversal: Portal Socio (UC-068 a UC-071)
- **Pasos promedio:** 13
- **Mockups:** 0%
- **Llamadas Service:** 100% (4/4)
- **Partes:** 0%

### Transversal: Cumplimiento (UC-072 a UC-076)
- **Pasos promedio:** 17
- **Mockups:** 100% (5/5) ← **MAYOR USO DE MOCKUPS**
- **Llamadas Service:** 80% (4/5)
- **Partes:** 80% (4/5)

---

## Estadísticas Globales

### Estructura
- **Con numeración continua:** 66/75 (88%)
- **Con numeración irregular/reiniciada:** 8/75 (11%)
- **Sin numeración (formato FN-X):** 1/75 (1%) - UC-039

### División en Partes
- **Sin partes (flujo único):** 55/75 (73%)
- **Con partes (2-4 subdivisiones):** 20/75 (27%)
- **Con referencias US en partes:** 0/75 (0%) ⚠️

### Contenido Enriquecido
- **Con mockups ASCII:** 24/75 (32%)
- **Con llamadas Service:** 34/75 (45%)
- **Con tablas de reglas:** 8/75 (11%)
- **Con Domain Events mencionados:** 13/75 (17%)
- **Con mensajes confirmación:** 38/75 (51%)

### Nivel de Detalle
- **Pasos promedio por UC:** 13
- **Palabras promedio por paso:** 20-50 (nivel medio)
- **UC más extenso:** UC-015 (34 pasos)
- **UC más breve:** UC-041 (2 pasos)

### Calidad de Código
- **UCs con código TS/SQL embebido:** 0/75 ✅
- **UCs con estructura no estándar:** 1/75 (UC-039)

---

## Recomendaciones para Consolidación

### 1. Estandarizar Estructura

**Problema:** 27% de los UCs usa división en Partes, pero:
- No incluyen referencias a User Stories
- La numeración a veces reinicia en cada Parte (inconsistente)
- Principalmente concentrado en BC-Tesoreria (91% de sus UCs)

**Recomendación:**

**Opción A - Eliminar Partes (preferida para consistencia):**
```
✅ ANTES (con partes):
**Parte 1: Configuración**
1. Paso A
2. Paso B

**Parte 2: Ejecución**
1. Paso C (reinicia)
2. Paso D

✅ DESPUÉS (flujo único):
1. Paso A (Configuración)
2. Paso B
3. Paso C (Ejecución)
4. Paso D
```

**Opción B - Enriquecer Partes (si se mantienen):**
```
**Parte 1: Configuración** (US-020, US-021, US-022)
1. Paso A
2. Paso B

**Parte 2: Ejecución** (US-023, US-024)
3. Paso C (numeración continúa)
4. Paso D
```

**Beneficio:** Mejora trazabilidad US→Pasos del flujo.

---

### 2. Corregir UC-039

**Problema:** Única estructura no estándar con formato `FN-1`, `FN-2`.

**Recomendación:** Convertir a numeración secuencial clásica:

```
✅ ANTES:
**FN-1: Creación y Envío**
1. Secretario crea
2. Sistema procesa

**FN-2: Procesamiento Async**
1. Job procesa

✅ DESPUÉS:
1. Secretario crea nueva comunicación
2. Sistema procesa previsualización
3. Secretario confirma y envía
4. Job procesa emails en lotes
```

---

### 3. Enriquecer Mockups ASCII Selectivamente

**Situación actual:** Solo 32% incluye mockups.

**Recomendación:** Añadir mockups en:
- UCs de configuración (UC-017, UC-042, UC-043)
- UCs de formularios complejos (UC-028, UC-029, UC-030)
- UCs de dashboards (UC-064, UC-065)

**No añadir en:**
- UCs de procesos batch/background
- UCs de exportación/importación
- UCs de servicios sin UI directa

**Beneficio:** Mejora comprensión visual sin sobrecargar UCs técnicos.

---

### 4. Homogeneizar Nivel de Detalle

**Problema:** Rango muy amplio (2 a 34 pasos por UC).

**Recomendación:** Establecer rango objetivo:
- **Mínimo:** 5 pasos (evitar UCs triviales)
- **Óptimo:** 8-20 pasos (legibilidad balanceada)
- **Máximo:** 25 pasos (considerar dividir en sub-UCs si excede)

**Acción específica:**
- **UC-015 (34 pasos):** Considerar dividir en UC-015A (Generación individual) y UC-015B (Generación masiva)
- **UC-040, UC-041 (2-3 pasos):** Enriquecer con más detalle o considerar fusionar con UCs relacionados

---

### 5. Aumentar Uso de Tablas de Reglas

**Situación actual:** Solo 11% incluye tablas.

**Recomendación:** Añadir tablas en UCs con:
- Máquinas de estado (transiciones válidas)
- Reglas de validación múltiples
- Configuraciones paramétricas
- Algoritmos de cálculo complejos

**Candidatos:** UC-018, UC-019, UC-021, UC-025, UC-031, UC-032

**Beneficio:** Clarifica lógica de negocio compleja de forma compacta.

---

### 6. Estandarizar Mención de Domain Events

**Situación actual:** Solo 17% menciona eventos en flujo normal.

**Recomendación:** Incluir eventos críticos en flujo normal:
```
N. Sistema emite evento `SocioRegistrado`
   → BC-Tesoreria: Crea CuentaSocio
   → BC-Comunicacion: Envía email bienvenida
```

**Beneficio:** Visibilidad de interacciones entre BCs en el happy path.

---

## Conclusiones

### Fortalezas del Documento

1. **Alta consistencia estructural:** 89% usa numeración continua
2. **Voz narrativa clara:** Mixta Actor-Sistema bien equilibrada
3. **Nivel de detalle apropiado:** 13 pasos promedio, 20-50 palabras/paso
4. **Calidad técnica:** 0 casos de código embebido en flujos
5. **Selectividad de mockups:** Usados apropiadamente en UCs con UI intensiva

### Áreas de Mejora

1. **Trazabilidad:** Ningún UC con Partes incluye referencias a User Stories
2. **Estructura mixta:** 27% usa Partes vs 73% flujo único (considerar estandarizar)
3. **UC-039 anómalo:** Estructura única no alineada con estándar del resto
4. **Bajo uso de tablas:** Solo 11% usa tablas para reglas complejas
5. **Eventos poco visibles:** Solo 17% menciona Domain Events en flujo normal

### Impacto General

El documento presenta **excelente calidad** con patrones consistentes en el 89% de los casos de uso. Las anomalías detectadas son **mínimas** (1 UC con estructura no estándar) y las áreas de mejora son **incrementales** (añadir trazabilidad, enriquecer contenido selectivamente).

**Recomendación final:** Aplicar las 6 mejoras propuestas mediante revisión incremental, priorizando:
1. Corregir UC-039 (impacto: 1 UC)
2. Añadir referencias US en Partes O eliminar Partes (impacto: 20 UCs)
3. Enriquecer tablas de reglas (impacto: ~10 UCs candidatos)

---

## Anexo: Casos de Uso por Categoría Estructural

### Categoría A: Flujo Simple Sin Partes (55 UCs)
UC-001, UC-002, UC-003, UC-005, UC-006, UC-007, UC-008, UC-009, UC-010, UC-011, UC-012, UC-013, UC-014, UC-015, UC-017, UC-028, UC-029, UC-030, UC-031, UC-032, UC-033, UC-039, UC-040, UC-041, UC-042, UC-043, UC-044, UC-045, UC-046, UC-047, UC-048, UC-049, UC-050, UC-051, UC-052, UC-053, UC-054, UC-055, UC-056, UC-057, UC-058, UC-059, UC-060, UC-061, UC-062, UC-063, UC-064, UC-065, UC-066, UC-067, UC-068, UC-069, UC-070, UC-071, UC-072

### Categoría B: Flujo con Partes (20 UCs)
UC-004, UC-018, UC-019, UC-020, UC-021, UC-022, UC-023, UC-024, UC-025, UC-026, UC-027, UC-034, UC-035, UC-036, UC-037, UC-038, UC-073, UC-074, UC-075, UC-076

### Categoría C: Flujo No Estándar (1 UC)
UC-039 (usa formato FN-X)

---

**Fin del Análisis**
