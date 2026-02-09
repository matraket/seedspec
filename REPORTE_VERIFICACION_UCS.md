# Reporte de Verificación de UCs - 010_casos-uso_FINAL-HUMANO.md

**Fecha:** 09 de febrero de 2026  
**Total UCs Activos:** 75 (excluyendo UC-016 fusionado)  
**Documento Verificado:** `010_casos-uso_FINAL-HUMANO.md`

---

## Resumen Ejecutivo

| Métrica | Cantidad | Porcentaje |
|---------|----------|------------|
| **UCs CONFORMES** | 24 | 32.0% |
| **UCs NO CONFORMES** | 51 | 68.0% |
| **TOTAL** | 75 | 100% |

---

## Criterios de Verificación

Para que un UC sea considerado **CONFORME**, debe cumplir los siguientes 3 criterios:

1. **✅ Numeración Continua:** Los pasos del Flujo Normal deben estar numerados 1, 2, 3, ..., N sin reiniciar ni saltos.
2. **✅ Referencias US en Partes (si usa Partes):** Cada sección "Parte X" debe incluir `(US-XXX, US-YYY)` en su cabecera.
3. **✅ Mensaje de Confirmación:** El último paso del Flujo Normal debe ser un mensaje de confirmación del sistema.

---

## Tabla de Resultados Completa

| UC | Numeración Continua | Refs US (si usa Partes) | Mensaje Confirmación | Estado |
|----|---------------------|-------------------------|----------------------|--------|
| UC-001 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-002 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-003 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-004 | ✅ | ❌ | ❌ | **NO CONFORME** |
| UC-005 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-006 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-007 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-008 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-009 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-010 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-011 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-012 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-013 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-014 | ❌ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-015 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-017 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-018 | ✅ | ✅ | ✅ | **CONFORME** |
| UC-019 | ✅ | ❌ | ✅ | **NO CONFORME** |
| UC-020 | ✅ | ❌ | ❌ | **NO CONFORME** |
| UC-021 | ✅ | ✅ | ❌ | **NO CONFORME** |
| UC-022 | ✅ | ❌ | ❌ | **NO CONFORME** |
| UC-023 | ❌ | ✅ | ✅ | **NO CONFORME** |
| UC-024 | ✅ | ❌ | ❌ | **NO CONFORME** |
| UC-025 | ✅ | ❌ | ✅ | **NO CONFORME** |
| UC-026 | ✅ | ❌ | ❌ | **NO CONFORME** |
| UC-027 | ✅ | ❌ | ❌ | **NO CONFORME** |
| UC-028 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-029 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-030 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-031 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-032 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-033 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-034 | ✅ | ❌ | ❌ | **NO CONFORME** |
| UC-035 | ✅ | ❌ | ❌ | **NO CONFORME** |
| UC-036 | ✅ | ❌ | ✅ | **NO CONFORME** |
| UC-037 | ✅ | ❌ | ✅ | **NO CONFORME** |
| UC-038 | ✅ | ❌ | ❌ | **NO CONFORME** |
| UC-039 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-040 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-041 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-042 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-043 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-044 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-045 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-046 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-047 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-048 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-049 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-050 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-051 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-052 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-053 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-054 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-055 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-056 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-057 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-058 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-059 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-060 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-061 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-062 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-063 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-064 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-065 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-066 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-067 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-068 | ✅ | N/A (sin Partes) | ✅ | **CONFORME** |
| UC-069 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-070 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-071 | ✅ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-072 | ❌ | N/A (sin Partes) | ❌ | **NO CONFORME** |
| UC-073 | ✅ | ❌ | ❌ | **NO CONFORME** |
| UC-074 | ✅ | ✅ | ❌ | **NO CONFORME** |
| UC-075 | ✅ | ❌ | ❌ | **NO CONFORME** |
| UC-076 | ❌ | ❌ | ✅ | **NO CONFORME** |

---

## Lista de UCs NO CONFORMES (51 casos)

Los siguientes UCs **AÚN necesitan corrección**:

```
UC-002, UC-004, UC-005, UC-006, UC-007, UC-008, UC-010, UC-011, UC-012, UC-013, 
UC-014, UC-015, UC-019, UC-020, UC-021, UC-022, UC-023, UC-024, UC-025, UC-026, 
UC-027, UC-029, UC-031, UC-033, UC-034, UC-035, UC-036, UC-037, UC-038, UC-047, 
UC-053, UC-056, UC-057, UC-058, UC-059, UC-060, UC-061, UC-062, UC-063, UC-064, 
UC-065, UC-066, UC-067, UC-069, UC-070, UC-071, UC-072, UC-073, UC-074, UC-075, 
UC-076
```

---

## Detalle de Problemas por UC

### BC-Identidad

#### UC-002: Autenticación multi-tenant
- ❌ **Mensaje Confirmación:** Paso 10 no tiene mensaje de confirmación del sistema

#### UC-004: Gestión de roles y permisos
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2
- ❌ **Mensaje Confirmación:** Paso 14 no tiene mensaje de confirmación

#### UC-005: Traspaso de cargos directivos
- ❌ **Mensaje Confirmación:** Paso 9 no tiene mensaje de confirmación

---

### BC-Membresia

#### UC-006: Gestión de ficha de socio
- ❌ **Mensaje Confirmación:** Paso 9 no tiene mensaje de confirmación

#### UC-007: Gestión de estados del socio
- ❌ **Mensaje Confirmación:** Paso 9 no tiene mensaje de confirmación (flujo manual)

#### UC-008: Configuración de tipos de socio
- ❌ **Mensaje Confirmación:** Paso 9 no tiene mensaje de confirmación

#### UC-010: Gestión de ejercicios
- ❌ **Mensaje Confirmación:** Paso 19 no tiene mensaje de confirmación

#### UC-011: Alta simple de socio
- ❌ **Mensaje Confirmación:** Paso 13 no tiene mensaje de confirmación

#### UC-012: Alta compleja con workflow (cofradías)
- ❌ **Mensaje Confirmación:** Paso 23 no tiene mensaje de confirmación

#### UC-013: Baja de socio
- ❌ **Mensaje Confirmación:** Paso 24 no tiene mensaje de confirmación

#### UC-014: Gestión de lista de espera
- ❌ **Numeración:** Rompe en paso 7 - esperado 7, encontrado 1 (reinicia numeración)
- ❌ **Mensaje Confirmación:** Paso 17 no tiene mensaje de confirmación

#### UC-015: Generación y validación de carnets
- ❌ **Mensaje Confirmación:** Paso 34 no tiene mensaje de confirmación

---

### BC-Tesoreria

#### UC-019: Generación masiva de cargos periódicos
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2

#### UC-020: Gestión de cargos manuales
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2, Parte 3
- ❌ **Mensaje Confirmación:** Paso 20 no tiene mensaje de confirmación

#### UC-021: Registro de cobros
- ❌ **Mensaje Confirmación:** Paso 32 no tiene mensaje de confirmación

#### UC-022: Workflow de morosidad
- ❌ **Refs US:** Sin referencias US en Parte 5
- ❌ **Mensaje Confirmación:** Paso 22 no tiene mensaje de confirmación

#### UC-023: Generación de remesas SEPA
- ❌ **Numeración:** Rompe en paso 4 - esperado 4, encontrado 1 (reinicia numeración)

#### UC-024: Gestión de devoluciones SEPA
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2, Parte 3, Parte 4
- ❌ **Mensaje Confirmación:** Paso 19 no tiene mensaje de confirmación

#### UC-025: Pasarela de pago online
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2, Parte 3, Parte 4

#### UC-026: Registro contable
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2, Parte 3
- ❌ **Mensaje Confirmación:** Paso 18 no tiene mensaje de confirmación

#### UC-027: Caja por turnos (peñas)
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2, Parte 3
- ❌ **Mensaje Confirmación:** Paso 27 no tiene mensaje de confirmación

---

### BC-Eventos

#### UC-029: Calendario y Sincronización
- ❌ **Mensaje Confirmación:** Paso 12 no tiene mensaje de confirmación

#### UC-031: Control de Aforo y Listas de Espera
- ❌ **Mensaje Confirmación:** Paso 14 no tiene mensaje de confirmación

#### UC-033: Eventos Específicos: Comidas Populares (Peñas)
- ❌ **Mensaje Confirmación:** Paso 20 no tiene mensaje de confirmación

#### UC-034: Eventos Específicos: Procesiones (Cofradías)
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2, Parte 3
- ❌ **Mensaje Confirmación:** Paso 12 no tiene mensaje de confirmación

#### UC-035: Eventos Específicos: Cuadrillas de Costaleros
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2, Parte 3
- ❌ **Mensaje Confirmación:** Paso 4 no tiene mensaje de confirmación

#### UC-036: Eventos Específicos: Cultos (Cofradías)
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2, Parte 3

#### UC-037: Eventos Específicos: Competiciones (Clubes Deportivos)
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2, Parte 3, Parte 4

#### UC-038: Valoraciones y feedback de eventos
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2, Parte 3
- ❌ **Mensaje Confirmación:** Paso 16 no tiene mensaje de confirmación

---

### BC-Comunicacion

#### UC-047: Comunicaciones automáticas (Event Handlers)
- ❌ **Mensaje Confirmación:** Paso 6 no tiene mensaje de confirmación

---

### BC-Documentos

#### UC-053: Búsqueda y Filtrado de Documentos
- ❌ **Mensaje Confirmación:** Paso 10 no tiene mensaje de confirmación

---

### Transversal: Import/Export

#### UC-056: Importación Masiva de Socios
- ❌ **Mensaje Confirmación:** Paso 5 no tiene mensaje de confirmación

#### UC-057: Importación de Histórico de Pagos
- ❌ **Mensaje Confirmación:** Paso 3 no tiene mensaje de confirmación

#### UC-058: Exportación de Listados y Plantillas
- ❌ **Mensaje Confirmación:** Paso 5 no tiene mensaje de confirmación

#### UC-059: Exportación de informes económicos
- ❌ **Mensaje Confirmación:** Paso 13 no tiene mensaje de confirmación

#### UC-060: Exportación fiscal Modelo 182 (AEAT)
- ❌ **Mensaje Confirmación:** Paso 15 no tiene mensaje de confirmación

#### UC-061: Exportación de listados de eventos
- ❌ **Mensaje Confirmación:** Paso 15 no tiene mensaje de confirmación

#### UC-062: Exportación de histórico de comunicaciones
- ❌ **Mensaje Confirmación:** Paso 14 no tiene mensaje de confirmación

#### UC-063: Backup Completo Automático
- ❌ **Mensaje Confirmación:** Paso 10 no tiene mensaje de confirmación

---

### Transversal: Reporting

#### UC-064: Dashboard Principal y KPIs
- ❌ **Mensaje Confirmación:** Paso 3 no tiene mensaje de confirmación

#### UC-065: Gráficos de evolución y dashboards interactivos
- ❌ **Mensaje Confirmación:** Paso 7 no tiene mensaje de confirmación

#### UC-066: Informes para Asamblea General
- ❌ **Mensaje Confirmación:** Paso 10 no tiene mensaje de confirmación

#### UC-067: Certificados y memorias personalizables
- ❌ **Mensaje Confirmación:** Paso 9 no tiene mensaje de confirmación

---

### Transversal: Portal Socio

#### UC-069: Consulta de Datos Personales y Cuotas
- ❌ **Mensaje Confirmación:** Paso 10 no tiene mensaje de confirmación

#### UC-070: Inscripción a eventos desde portal
- ❌ **Mensaje Confirmación:** Paso 16 no tiene mensaje de confirmación

#### UC-071: Descarga de documentos personales
- ❌ **Mensaje Confirmación:** Paso 22 no tiene mensaje de confirmación

---

### Transversal: Cumplimiento

#### UC-072: Gestión RGPD y Consentimientos
- ❌ **Numeración:** Rompe en paso 2 - esperado 2, encontrado 1 (reinicia numeración)
- ❌ **Mensaje Confirmación:** Paso 6 no tiene mensaje de confirmación

#### UC-073: Ejercicio de Derechos ARCO
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2, Parte 3, Parte 4
- ❌ **Mensaje Confirmación:** Paso 12 no tiene mensaje de confirmación

#### UC-074: Cumplimiento Ley de Asociaciones
- ❌ **Mensaje Confirmación:** Paso 23 no tiene mensaje de confirmación

#### UC-075: Alertas Legales Automáticas
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2, Parte 3
- ❌ **Mensaje Confirmación:** Paso 18 no tiene mensaje de confirmación

#### UC-076: Alertas Fiscales y Tributarias
- ❌ **Numeración:** Rompe en paso 9 - esperado 9, encontrado 1 (reinicia numeración)
- ❌ **Refs US:** Sin referencias US en Parte 1, Parte 2, Parte 3

---

## Análisis de Patrones de Errores

### Por Tipo de Error

| Tipo de Error | Cantidad de UCs Afectados | % del Total |
|---------------|---------------------------|-------------|
| **Falta mensaje confirmación** | 46 | 61.3% |
| **Falta referencias US en Partes** | 22 | 29.3% |
| **Numeración discontinua** | 4 | 5.3% |

### UCs con Múltiples Problemas

| UC | Cantidad Problemas | Problemas |
|----|-------------------|-----------|
| UC-076 | 3 | Numeración + Refs US + Confirmación |
| UC-073 | 2 | Refs US + Confirmación |
| UC-075 | 2 | Refs US + Confirmación |
| UC-072 | 2 | Numeración + Confirmación |
| UC-038 | 2 | Refs US + Confirmación |
| UC-037 | 2 | Refs US + Confirmación (pero tiene mensaje) |
| UC-036 | 2 | Refs US + Confirmación (pero tiene mensaje) |
| UC-035 | 2 | Refs US + Confirmación |
| UC-034 | 2 | Refs US + Confirmación |
| UC-027 | 2 | Refs US + Confirmación |
| UC-026 | 2 | Refs US + Confirmación |
| UC-024 | 2 | Refs US + Confirmación |
| UC-022 | 2 | Refs US + Confirmación |
| UC-020 | 2 | Refs US + Confirmación |
| UC-014 | 2 | Numeración + Confirmación |
| UC-004 | 2 | Refs US + Confirmación |

### BCs con Mayor Tasa de Errores

| BC | UCs NO CONFORMES | UCs Totales | % Error |
|----|------------------|-------------|---------|
| **BC-Tesoreria** | 9/11 | 11 | 81.8% |
| **BC-Eventos** | 6/11 | 11 | 54.5% |
| **Transversal Import/Export** | 8/8 | 8 | 100% |
| **Transversal Reporting** | 4/4 | 4 | 100% |
| **Transversal Portal Socio** | 3/4 | 4 | 75.0% |
| **Transversal Cumplimiento** | 4/5 | 5 | 80.0% |
| **BC-Membresia** | 9/10 | 10 | 90.0% |
| **BC-Identidad** | 3/5 | 5 | 60.0% |
| **BC-Comunicacion** | 1/9 | 9 | 11.1% ✅ |
| **BC-Documentos** | 1/8 | 8 | 12.5% ✅ |

---

## Recomendaciones de Corrección

### Prioridad ALTA (Errores Críticos de Estructura)

1. **UC-014:** Corregir numeración discontinua (reinicia en paso 7)
2. **UC-023:** Corregir numeración discontinua (reinicia en paso 4)
3. **UC-072:** Corregir numeración discontinua (reinicia en paso 2)
4. **UC-076:** Corregir numeración discontinua (reinicia en paso 9) + añadir refs US + mensaje confirmación

### Prioridad MEDIA (Múltiples Problemas)

Corregir UCs con 2+ problemas (16 casos):
- UC-004, UC-014, UC-020, UC-022, UC-024, UC-026, UC-027, UC-034, UC-035, UC-038, UC-072, UC-073, UC-075, UC-076

### Prioridad BAJA (Solo Mensaje Confirmación)

Añadir mensaje de confirmación al último paso del Flujo Normal en 30 UCs:
- UC-002, UC-005, UC-006, UC-007, UC-008, UC-010, UC-011, UC-012, UC-013, UC-015, UC-021, UC-029, UC-031, UC-033, UC-047, UC-053, UC-056, UC-057, UC-058, UC-059, UC-060, UC-061, UC-062, UC-063, UC-064, UC-065, UC-066, UC-067, UC-069, UC-070, UC-071, UC-074

---

## Conclusiones

1. **68% de los UCs requieren corrección**, lo que indica que el documento está en fase de revisión activa.

2. **El problema más común es la falta de mensaje de confirmación** (46 UCs afectados), lo que sugiere una oportunidad de mejora sistemática mediante un patrón consistente.

3. **Los BCs transversales tienen mayor tasa de error** (Import/Export y Reporting al 100%), posiblemente porque fueron documentados más recientemente o con menos iteraciones de revisión.

4. **BC-Comunicacion y BC-Documentos** son los mejor documentados (11-12% de error), pueden servir como referencia de calidad.

5. **4 UCs tienen problemas de numeración discontinua**, lo cual es crítico porque afecta la legibilidad y trazabilidad del flujo.

---

**Fin del Reporte**
