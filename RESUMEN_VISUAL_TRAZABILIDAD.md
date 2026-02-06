# Resumen Visual de Trazabilidad
## User Stories → Casos de Uso

**Fecha:** 06 Febrero 2026  
**Proyecto:** Associated v1.9

---

## 🎯 Métricas Globales

```
┌─────────────────────────────────────────────────────────────┐
│                   COBERTURA GLOBAL: 79.2%                   │
│                                                             │
│  ████████████████████████████████████████░░░░░░░░░          │
│  160 US cubiertas                        42 huérfanas      │
│                                                             │
│  Total User Stories: 202 (scope N2-N11)                   │
│  User Stories referenciadas: 160                           │
│  User Stories huérfanas: 42                                │
│  User Stories duplicadas: 7                                │
└─────────────────────────────────────────────────────────────┘
```

---

## 📊 Cobertura por Bounded Context

```
BC-Membresia        ████████████████████████████████████████ 100% ✅  (34/34)
BC-Tesoreria        ████████████████████████████████████████ 100% ✅  (40/40)
BC-Eventos          ████████████████████████████████████████ 100% ✅  (30/30)
BC-Comunicacion     ████████████████████████████████████████ 100% ✅  (23/23)
BC-Identidad        ███████████████████████████████████░░░░░  87% ⚠️   (7/8)
BC-Documentos       ████████████████████████████████░░░░░░░░  83% ⚠️  (10/12)
Import/Export (N8)  ██████████████████░░░░░░░░░░░░░░░░░░░░░░  46% 🚨   (6/13)
Portal Socio (N10)  ████████████████░░░░░░░░░░░░░░░░░░░░░░░░  40% 🚨   (6/15)
Reporting (N9)      █████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░  33% 🚨   (4/12)
Cumplimiento (N11)  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░   0% 🚨🚨 (0/17)
```

---

## 🚨 Áreas Críticas - Requieren Atención Inmediata

### 1. N11 - Cumplimiento Normativo (0% cobertura)
```
Estado: 🚨🚨 CRÍTICO
US Huérfanas: 17 (incluyendo 8 Must)
Impacto: RGPD, Ley de Asociaciones, alertas legales
```

**User Stories huérfanas Must:**
- US-188: RAT (Registro Actividades Tratamiento)
- US-189: Gestión consentimientos RGPD
- US-190: Derecho de acceso
- US-191: Derecho de rectificación
- US-192: Derecho de supresión
- US-193: Derecho de portabilidad
- US-194: Base jurídica datos religiosos
- US-195: Libro de socios digital

**Acción requerida:**
- Crear 4-5 Casos de Uso nuevos (UC-072 a UC-076)
- Documentar flujos normales, alternativos y excepciones
- Vincular Domain Events y Application Services

---

### 2. N9 - Reporting (33.3% cobertura)
```
Estado: 🚨 CRÍTICO
US Huérfanas: 8 (incluyendo 3 Must)
Impacto: Informes Asamblea, certificados oficiales
```

**User Stories huérfanas Must:**
- US-168: Informe de socios para Asamblea
- US-169: Informe económico para Asamblea
- US-170: Certificado estar al corriente de pago

**Acción requerida:**
- Completar UC-064 a UC-067 con las US faltantes
- Priorizar las 3 Must para Asamblea General

---

### 3. N10 - Portal del Socio (40% cobertura)
```
Estado: 🚨 CRÍTICO
US Huérfanas: 9 (incluyendo 2 Must)
Impacto: Acceso self-service socios, UX
```

**User Stories huérfanas Must:**
- US-179: Acceso al carnet digital
- US-185: Gestión de consentimientos RGPD

**Acción requerida:**
- Verificar si UC-068 a UC-071 existen en versión preliminar
- Completar documentación con las 9 US faltantes

---

### 4. N8 - Import/Export (46.2% cobertura)
```
Estado: 🚨 ALTO
US Huérfanas: 7 (incluyendo 1 Must)
Impacto: Migraciones, backups, exportación fiscal
```

**User Story huérfana Must:**
- US-160: Backup periódico de datos

**Acción requerida:**
- Completar UC-056 a UC-063 con las US faltantes

---

## ⚠️ Áreas Mejorables

### 5. BC-Identidad (87.5% cobertura)
```
US Huérfana Must: US-006 (Registro de auditoría)
Acción: Añadir a UC-005 o crear UC-006 dedicado
```

### 6. BC-Documentos (83.3% cobertura)
```
US Huérfanas Should: US-141, US-147
Acción: Completar UC-053 y UC-054
```

---

## 🔄 User Stories Duplicadas (Requieren Revisión)

```
US-112 → BC-Eventos (2 UCs)
US-116 → BC-Comunicacion (2 UCs)
US-117 → BC-Comunicacion (2 UCs)
US-120 → BC-Comunicacion (2 UCs)
US-122 → BC-Comunicacion (2 UCs)
US-123 → BC-Comunicacion (2 UCs)
US-124 → BC-Comunicacion (2 UCs)
```

**Análisis:** Concentración de duplicados en BC-Comunicacion sugiere necesidad de revisar agrupación de UCs.

---

## 📝 Resumen de Discrepancias

### Matriz de Trazabilidad vs Realidad

| Métrica | Matriz (declarado) | Realidad | Diferencia |
|---------|-------------------|----------|------------|
| Cobertura | 100% | 79.2% | **-20.8%** |
| US Cubiertas | 202 | 160 | **-42 US** |
| US Huérfanas | 0 | 42 | **+42 US** |

### Errores en la Matriz

1. **Referencias a US inexistentes:**
   - US-203 a US-227: NO EXISTEN (el documento termina en US-202)

2. **Mapping incorrecto:**
   - US-188 a US-202 asignadas a N8 (Import/Export)
   - **Realidad:** Pertenecen a N11 (Cumplimiento Normativo)

---

## 🎯 Plan de Acción Recomendado

### Fase 1: CRÍTICO (1-2 semanas)
- [ ] **Prioridad 1:** Documentar N11 Cumplimiento (UC-072 a UC-076)
  - Cubrir las 8 US Must de RGPD
  - Documentar flujos de ejercicio de derechos
  - Incluir alertas de cumplimiento legal

- [ ] **Prioridad 2:** Completar N9 Reporting (UC-064 a UC-067)
  - Añadir US-168, US-169, US-170 (Must para Asamblea)
  - Completar dashboards y gráficos

- [ ] **Prioridad 3:** Completar N10 Portal Socio (UC-068 a UC-071)
  - Añadir US-179, US-185 (Must)
  - Documentar acceso self-service completo

### Fase 2: ALTA (1 semana)
- [ ] Completar N8 Import/Export (US-160 Must + 6 Should)
- [ ] Añadir US-006 en BC-Identidad (auditoría Must)
- [ ] Resolver duplicados en BC-Comunicacion

### Fase 3: MEDIA (3-5 días)
- [ ] Completar BC-Documentos (2 Should)
- [ ] Corregir Matriz de Trazabilidad
- [ ] Validar cobertura 100% final

---

## 📂 Archivos Generados

1. **REPORTE_TRAZABILIDAD_FINAL.md**
   - Análisis exhaustivo completo
   - Listado detallado de huérfanas
   - Recomendaciones por prioridad

2. **trazabilidad_detallada.csv**
   - Tabla completa US-001 a US-202
   - Estado de cada US (Cubierta/Huérfana)
   - Bounded Context asignado

3. **RESUMEN_VISUAL_TRAZABILIDAD.md** (este archivo)
   - Vista rápida del estado
   - Gráficos de cobertura
   - Plan de acción

---

## ✅ Próximos Pasos

1. **Revisar este reporte** con el equipo de documentación
2. **Priorizar Fase 1** (UCs críticos de N11, N9, N10)
3. **Asignar responsables** para cada Caso de Uso pendiente
4. **Establecer deadline** para completar cobertura 100%
5. **Actualizar AGENTS.md** con el nuevo estado

---

**Generado:** 06 Febrero 2026  
**Herramienta:** Análisis automatizado + validación manual  
**Validación:** ✅ 202/202 User Stories verificadas  
**Estado:** **Reporte Final Listo para Revisión**

---

## 🔗 Referencias

- **KB-009:** `009_user-stories.md` (202 User Stories)
- **KB-010:** `010_casos-uso_FINAL-HUMANO.md` (Casos de Uso documentados)
- **Matriz:** `010_matriz_trazabilidad.md` (Requiere actualización)
- **AGENTS.md:** Instrucciones del proyecto (v2.0)

---

**FIN DEL RESUMEN VISUAL**
