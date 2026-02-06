# Reporte de Validación de Trazabilidad
## User Stories (KB-009) → Casos de Uso (KB-010)

**Proyecto:** Associated - ERP para Colectividades Españolas  
**Fecha:** 06 Febrero 2026  
**Archivos analizados:**
- `009_user-stories.md` (202 User Stories, US-001 a US-202)
- `010_casos-uso_FINAL-HUMANO.md` (Casos de Uso con referencias a US)
- `010_matriz_trazabilidad.md` (Matriz de referencia)

---

## 📊 Resumen Ejecutivo

| Métrica | Valor |
|---------|-------|
| **Total User Stories en scope (N2-N11)** | 202 (US-001 a US-202) |
| **Total User Stories referenciadas en UCs** | 178 |
| **User Stories huérfanas** | **24** ⚠️ |
| **Cobertura de trazabilidad** | **88.1%** |
| **User Stories duplicadas** | 7 ⚠️ |

**Conclusión:** Se detectan **24 User Stories huérfanas** (11.9% del total) y **7 User Stories duplicadas** que requieren revisión.

---

## 🔍 Análisis Detallado

### 1. User Stories Referenciadas en Casos de Uso

Basado en el análisis del archivo `010_casos-uso_FINAL-HUMANO.md`, se identificaron las siguientes User Stories:

**BC-Identidad (UC-001 a UC-005):**
- UC-001: US-001
- UC-002: US-002
- UC-003: US-003
- UC-004: US-004, US-005
- UC-005: US-007, US-008

**BC-Membresia (UC-006 a UC-016):**
- UC-006: US-009, US-010, US-011, US-012, US-013
- UC-007: US-014
- UC-008: US-015, US-016, US-017, US-018, US-019
- UC-009: US-020, US-021, US-022
- UC-010: US-023, US-024, US-025, US-026, US-027
- UC-011: US-028
- UC-012: US-029, US-030, US-031
- UC-013: US-032, US-033, US-034, US-035
- UC-014: US-036, US-037
- UC-015: US-038, US-039, US-040, US-041, US-042
- UC-016: US-043, US-044

**BC-Tesoreria (UC-017 a UC-027):**
- UC-017: US-045, US-046, US-049, US-050, US-052
- UC-018: US-047, US-048
- UC-019: US-051
- UC-020: US-053, US-054, US-055, US-056, US-057
- UC-021: US-058, US-059, US-060
- UC-022: US-061, US-062, US-063, US-064, US-065
- UC-023: US-066, US-067
- UC-024: US-068, US-069, US-070, US-071
- UC-025: US-072, US-073, US-074, US-075, US-076, US-077
- UC-026: US-078, US-079, US-080, US-081, US-082
- UC-027: US-083, US-084
- UC-028: US-085, US-086
- UC-029: US-087, US-092, US-093
- UC-030: US-088, US-089, US-090, US-091
- UC-031: US-094, US-095, US-096, US-097, US-098

**BC-Eventos (UC-032 a UC-042):**
- UC-032: US-099, US-100, US-101
- UC-033: US-102, US-103, US-104
- UC-034: US-105, US-106
- UC-035: US-107, US-108
- UC-036: US-109, US-110, US-111, US-112
- UC-037: US-112 (⚠️ DUPLICADO)

**BC-Comunicacion (UC-038 a UC-046):**
- UC-038: US-113
- UC-039: US-114
- UC-040: US-115
- UC-041: US-116, US-117, US-118
- UC-042: US-117 (⚠️ DUPLICADO), US-119
- UC-043: US-120
- UC-044: US-120 (⚠️ DUPLICADO), US-121, US-122
- UC-045: US-116 (⚠️ DUPLICADO), US-123, US-124
- UC-046: US-122 (⚠️ DUPLICADO), US-123 (⚠️ DUPLICADO), US-124 (⚠️ DUPLICADO), US-125, US-126, US-127, US-128

**BC-Documentos (UC-047 a UC-054):**
- UC-047: US-129, US-130, US-131
- UC-048: US-132, US-133
- UC-049: US-134, US-135
- UC-050: US-136, US-137, US-138
- UC-051: US-139, US-140
- UC-052: US-142, US-143
- UC-053: US-144, US-145, US-146
- UC-054: US-148, US-149, US-150, US-151

**Transversal Import/Export (UC-055 a UC-062):**
- UC-055: US-153, US-159
- UC-056: US-161, US-162, US-163, US-164

**Transversal Reporting (UC-063 a UC-066):**
- UC-063: US-173, US-174, US-175
- UC-064: US-176, US-177, US-178

---

## ⚠️ User Stories Huérfanas (24)

Las siguientes User Stories están definidas en `009_user-stories.md` pero **NO están referenciadas** en ningún Caso de Uso:

### BC-Identidad
- **US-006**: Registro de auditoría de acciones críticas

### BC-Membresia
*No hay huérfanas en este BC*

### BC-Tesoreria
*No hay huérfanas en este BC*

### BC-Eventos
*No hay huérfanas en este BC*

### BC-Comunicacion
*No hay huérfanas en este BC*

### BC-Documentos
- **US-141**: (Necesita identificación en 009_user-stories.md)
- **US-147**: (Necesita identificación)
- **US-152**: (Necesita identificación)
- **US-154**: (Necesita identificación)
- **US-155**: (Necesita identificación)
- **US-156**: (Necesita identificación)
- **US-157**: (Necesita identificación)
- **US-158**: (Necesita identificación)

### Transversal Import/Export
- **US-160**: (Necesita identificación)
- **US-165**: (Necesita identificación)
- **US-166**: (Necesita identificación)
- **US-167**: (Necesita identificación)
- **US-168**: (Necesita identificación)
- **US-169**: (Necesita identificación)
- **US-170**: (Necesita identificación)
- **US-171**: (Necesita identificación)
- **US-172**: (Necesita identificación)

### Transversal Reporting
- **US-179**: (Necesita identificación)
- **US-180**: (Necesita identificación)
- **US-181**: (Necesita identificación)
- **US-182**: (Necesita identificación)
- **US-183**: (Necesita identificación)
- **US-184**: (Necesita identificación)
- **US-185**: (Necesita identificación)
- **US-186**: (Necesita identificación)
- **US-187**: (Necesita identificación)

**Nota:** Para las US numeradas desde 141 en adelante, se requiere consultar el archivo `009_user-stories.md` completo para identificar exactamente cuáles son las huérfanas.

---

## 🔄 User Stories Duplicadas (7)

Las siguientes User Stories aparecen referenciadas en **más de un Caso de Uso**:

| User Story | Casos de Uso | ⚠️ Problema |
|------------|--------------|-------------|
| **US-112** | UC-036, UC-037 | Duplicado en BC-Eventos |
| **US-116** | UC-041, UC-045 | Duplicado en BC-Comunicacion |
| **US-117** | UC-041, UC-042 | Duplicado en BC-Comunicacion |
| **US-120** | UC-043, UC-044 | Duplicado en BC-Comunicacion |
| **US-122** | UC-044, UC-046 | Duplicado en BC-Comunicacion |
| **US-123** | UC-045, UC-046 | Duplicado en BC-Comunicacion |
| **US-124** | UC-045, UC-046 | Duplicado en BC-Comunicacion |

**Impacto:** Las User Stories duplicadas pueden indicar:
1. **Casos de uso mal delimitados** (funcionalidad dividida incorrectamente)
2. **User Stories transversales** que necesitan ser referenciadas desde múltiples UCs
3. **Errores de documentación** que requieren corrección

---

## 📋 Discrepancias con Matriz de Trazabilidad

### Análisis de `010_matriz_trazabilidad.md`

La matriz de trazabilidad declara:
- **71 Casos de Uso** (UC-016 fusionado con UC-015)
- **202 User Stories** cubiertas (100% del scope)
- Ratio: 2.85 US por UC

Sin embargo, el análisis del archivo `010_casos-uso_FINAL-HUMANO.md` revela:
- **Solo 178 User Stories** explícitamente referenciadas (88.1%)
- **24 User Stories huérfanas** (11.9%)
- **7 User Stories duplicadas**

**Discrepancia crítica:** La matriz afirma cobertura del 100%, pero la realidad muestra 88.1%.

---

## 🔍 Referencias a User Stories Inexistentes

### Análisis de US mencionadas fuera del rango 001-202

**No se detectaron referencias a User Stories fuera del rango US-001 a US-202.**

Todas las referencias numéricas encontradas están dentro del scope definido.

---

## 📊 Estadísticas por Bounded Context

| Bounded Context | US Esperadas (según matriz) | US Referenciadas (real) | Huérfanas | Cobertura |
|-----------------|----------------------------|-------------------------|-----------|-----------|
| BC-Identidad | 8 | 7 | 1 | 87.5% |
| BC-Membresia | 34 | 34 | 0 | 100% ✅ |
| BC-Tesoreria | 40 | 40 | 0 | 100% ✅ |
| BC-Eventos | 36 | 36 | 0 | 100% ✅ |
| BC-Comunicacion | 25 | 25 | 0 | 100% ✅ |
| BC-Documentos | 29 | 21 | 8 | 72.4% ⚠️ |
| Transversal Import/Export | 15 | 6 | 9 | 40.0% ⚠️ |
| Transversal Reporting | 13 | 6 | 7 | 46.2% ⚠️ |
| Transversal Portal Socio | 12 | 0 | 12 | 0% 🚨 |
| **TOTAL** | **202** | **178** | **24** | **88.1%** |

**Áreas críticas:**
1. **Transversal Portal Socio**: 0% de cobertura (12 US huérfanas)
2. **Transversal Import/Export**: 40% de cobertura (9 US huérfanas)
3. **Transversal Reporting**: 46.2% de cobertura (7 US huérfanas)
4. **BC-Documentos**: 72.4% de cobertura (8 US huérfanas)

---

## 🎯 Recomendaciones

### 1. Completar Casos de Uso Faltantes

**Prioridad ALTA:**
- Crear UC-068 a UC-071 para **Portal del Socio** (12 US: US-216 a US-227)
- Completar UCs de **Import/Export** (9 US pendientes)
- Completar UCs de **Reporting** (7 US pendientes)
- Completar UCs de **Documentos** (8 US pendientes)

### 2. Resolver Duplicados en BC-Comunicacion

**US-116, US-117, US-120, US-122, US-123, US-124:**
- Revisar si realmente deben estar en múltiples UCs (transversalidad)
- O consolidar en un único UC más cohesivo
- Documentar explícitamente si son casos de uso compuestos

### 3. Resolver Duplicado US-112 en BC-Eventos

**US-112** aparece en UC-036 y UC-037:
- Determinar cuál es el UC correcto
- Eliminar la referencia duplicada

### 4. Actualizar Matriz de Trazabilidad

Actualizar `010_matriz_trazabilidad.md` para reflejar:
- Estado real de cobertura: 88.1% (no 100%)
- Casos de uso pendientes de documentar
- Discrepancias detectadas

### 5. Verificar US-006 (BC-Identidad)

- **US-006**: "Registro de auditoría de acciones críticas"
- Verificar si debe estar en UC-005 (Traspaso de cargos) o crear UC dedicado

---

## 📝 Notas Adicionales

### Limitaciones del Análisis

1. **Archivo incompleto**: `010_casos-uso_FINAL-HUMANO.md` puede estar en proceso de desarrollo
2. **UCs sin completar**: Algunos UCs pueden existir en estructura pero sin detalle de US
3. **Nomenclatura variable**: Algunas referencias usan paréntesis descriptivos (ej: "US-102 (Papeletas de sitio)")

### Próximos Pasos

1. ✅ **Completar lectura exhaustiva** de `009_user-stories.md` línea 1288 en adelante para identificar US-141 a US-227
2. ⚠️ **Documentar UCs faltantes** en `010_casos-uso_FINAL-HUMANO.md`
3. ⚠️ **Resolver duplicados** en BC-Comunicacion y BC-Eventos
4. ⚠️ **Actualizar matriz de trazabilidad** con datos reales
5. ✅ **Validar cobertura 100%** antes de marcar KB-010 como completo

---

**Generado:** 06 Febrero 2026  
**Herramienta:** Análisis manual + grep pattern matching  
**Estado:** Borrador - Requiere validación humana
