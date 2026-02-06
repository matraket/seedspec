# REPORTE DE INVESTIGACIÓN: UC-063 y UC-016

**Fecha:** 06 Febrero 2026  
**Documento analizado:** 010_casos-uso_FINAL-HUMANO.md

---

## 🎯 RESUMEN EJECUTIVO

### UC-063: ❌ FALTA DOCUMENTAR
- **Estado:** Mencionado en tablas pero sin documentación completa
- **User Story correcta:** US-160 (Backup periódico de datos)
- **Acción:** Generar UC-063 completo

### UC-016: ✅ FUSIÓN VÁLIDA
- **Estado:** Correctamente fusionado con UC-015
- **Justificación:** 80% código compartido, misma transaccionalidad
- **Acción:** Mantener como está (no requiere corrección)

---

## ANÁLISIS UC-063: BACKUP AUTOMÁTICO COMPLETO

### ❌ Estado: FALTA COMPLETAR DOCUMENTACIÓN DETALLADA

**Ubicación en documento:**
- ✅ Mencionado en tabla Import/Export
- ✅ Mencionado en highlights técnicos
- ✅ Incluido en índice de navegación
- ✅ Incluido en lista de UCs de alta complejidad
- ❌ **NO existe sección `### UC-063:` con documentación completa**

**Comparación:**
- UC-056, UC-057, UC-058, UC-059, UC-060, UC-061, UC-062 → **Documentados completos**
- UC-063 → **Solo mencionado en tabla, sin desarrollo**

---

### 🚨 PROBLEMA: User Stories INCORRECTAS en Tabla

**Tabla del documento indica:** US-201, US-202  
**User Stories reales de backup:** US-160 (única)

**ERROR DETECTADO:**

La tabla de Import/Export tiene un **error de referencia de User Stories**:

| Dato en tabla UC | User Stories indicadas | User Stories reales (009_user-stories.md) |
|------------------|------------------------|-------------------------------------------|
| UC-056 a UC-063  | US-188 a US-202        | **US-148 a US-160**                       |

**Verificación:**
- En `009_user-stories.md`, la sección "7. Transversal: Importación y Exportación (N8)" contiene **US-148 a US-160** (13 User Stories)
- Las **US-188 a US-202** corresponden a la sección "10. Transversal: Cumplimiento Normativo (N11)"

---

### 📄 Contenido de US-160 (RF Origen: N8RF13)

```gherkin
> Como administrador,
> quiero poder descargar un backup completo de los datos,
> para tener copia de seguridad propia de la entidad.

Criterios de Aceptación:

Escenario 1: Descarga de backup manual
  GIVEN soy administrador autenticado
  WHEN solicito un backup completo
  THEN se genera archivo comprimido con:
    - Socios (CSV)
    - Movimientos económicos (CSV)
    - Documentos (ZIP)
    - Comunicaciones (CSV)
    - Configuración del tenant (JSON)

Escenario 2: Backup programado
  GIVEN he configurado backup semanal
  WHEN llega el día programado
  THEN recibo notificación con enlace de descarga
  AND enlace válido por 7 días

Escenario 3: Información del backup
  GIVEN he descargado un backup
  WHEN descomprimo el archivo
  THEN incluye README con:
    - Fecha y hora de generación
    - Versión del sistema
    - Número de registros por tabla
```

**US-201 y US-202 reales (N11: Cumplimiento Normativo):**
- US-201: Alerta de plazos fiscales
- US-202: Alerta de caducidad de documentos
- ❌ **No tienen relación con backup**

---

### 🔍 Análisis de la Ausencia

**¿Por qué falta UC-063?**

Revisando el Changelog del documento:
```
UC-059 a UC-063: Documentados brevemente (exportaciones económicas, 
fiscal, eventos, comunicaciones, backup)
```

**Interpretación:** El autor documentó los UCs UC-059 a UC-062 de forma breve, pero **UC-063 se quedó pendiente** de documentación completa, quedando solo la referencia en la tabla.

**Evidencia de la Falta:**

```bash
grep -n "^### UC-063" 010_casos-uso_FINAL-HUMANO.md
# RESULTADO: Sin resultados
```

**Últimas secciones UC documentadas:**
- UC-062: Exportación de histórico de comunicaciones (línea ~24894)
- [SALTO]
- UC-064: Dashboard Principal y KPIs (línea ~25417)

**Conclusión:** UC-063 **no fue documentado** tras UC-062.

---

## ✅ PROPUESTA UC-063 COMPLETO

### UC-063: Backup Completo Automático

**Metadatos:**
- **User Stories:** US-160
- **Bounded Context:** Transversal (Infraestructura)
- **Application Service:** `BackupService.generarBackupCompleto()`
- **Aggregates:** N/A (operación de infraestructura)
- **Prioridad:** Should Have (según US-160)

---

#### Descripción

Sistema de backup automático programable que genera copias completas de la base de datos del tenant y documentos asociados, con retención configurable y almacenamiento seguro en S3.

El backup incluye exportación de todas las tablas críticas en formato portable (CSV/JSON), documentos almacenados en S3, y metadatos del sistema. Permite tanto ejecución manual por el administrador como programación de backups periódicos.

Implementa política de retención configurable (30 días por defecto) para gestión automática del espacio de almacenamiento.

---

#### Actores

- **Administrador del tenant** (descarga manual)
- **Sistema** (ejecución programada)

---

#### Precondiciones

- Tenant activo y no suspendido
- Credenciales S3 configuradas correctamente
- Espacio disponible en bucket (>= tamaño estimado del backup)
- Conexión a base de datos disponible

---

#### Flujo Normal

1. Administrador configura frecuencia de backup (semanal/mensual) desde panel de configuración
2. Sistema ejecuta tarea programada en horario nocturno (03:00 AM hora del tenant)
3. Sistema ejecuta `pg_dump` de la base de datos del tenant con formato custom y compresión
4. Sistema exporta datos a archivos portables:
   - Socios → CSV con encoding UTF-8
   - Movimientos económicos → CSV con formato contable
   - Documentos → Descarga desde S3 y crea ZIP
   - Comunicaciones → CSV con historial de envíos
   - Configuración del tenant → JSON
5. Sistema comprime todo en archivo `.tar.gz`
6. Sistema sube archivo a S3 con path: `backups/{tenant_id}/{fecha_ISO}.tar.gz`
7. Sistema genera archivo `README.txt` con metadatos del backup:
   - Fecha y hora de generación
   - Versión del sistema
   - Número de registros por tabla
   - Hash SHA256 del archivo para verificación de integridad
8. Sistema cifra el backup con AES-256-GCM usando clave del tenant
9. Sistema envía notificación por email al administrador con:
   - Confirmación de backup exitoso
   - Tamaño del archivo
   - Enlace de descarga firmado (válido 7 días)
10. Sistema aplica política de retención: elimina backups con antigüedad > 30 días

---

#### Flujos Alternativos

**FA-1: Descarga manual on-demand por administrador**
- Administrador accede a "Configuración → Backups"
- Administrador hace clic en "Generar Backup Ahora"
- Sistema ejecuta pasos 3-9 de forma síncrona
- Administrador recibe enlace de descarga inmediato

**FA-2: Backup completo antes de cierre de ejercicio**
- Sistema detecta que el ejercicio está por cerrarse (N3RF18)
- Sistema genera backup automático con etiqueta `ejercicio_{año}`
- Sistema conserva este backup durante 10 años (requisito fiscal)
- Sistema notifica al administrador y al tesorero

**FA-3: Backup previo a operaciones críticas**
- Antes de importación masiva (UC-056, UC-057)
- Antes de cambio de plan de cuotas (UC-017)
- Antes de cierre de ejercicio (UC-010)
- Sistema genera backup con etiqueta descriptiva

---

#### Flujos de Excepción

**FE-1: Fallo en pg_dump**
- Sistema detecta error en proceso de dump
- Sistema registra error en logs con detalles técnicos
- Sistema envía notificación inmediata al administrador
- Sistema programa reintento automático en 1 hora
- Si 3 reintentos fallan, sistema escala alerta a soporte técnico

**FE-2: Fallo en subida a S3**
- Sistema no puede conectar con S3 o upload falla
- Sistema almacena backup temporalmente en disco local
- Sistema reintenta upload cada 30 minutos (máximo 24 horas)
- Sistema notifica al administrador del almacenamiento temporal
- Si persiste 24h, sistema alerta para revisión de credenciales S3

**FE-3: Bucket sin espacio disponible**
- Sistema detecta que bucket está al límite de capacidad
- Sistema notifica inmediatamente al administrador
- Sistema sugiere eliminar backups antiguos o ampliar plan
- Sistema no elimina backups automáticamente sin confirmación

**FE-4: Backup corrupto detectado**
- Sistema ejecuta verificación de integridad (hash SHA256)
- Si hash no coincide, sistema marca backup como corrupto
- Sistema genera nuevo backup automáticamente
- Sistema notifica del problema y reemplaza enlace de descarga

---

#### Eventos de Dominio

- **BackupCompletado** → Sistema de Monitoreo, Auditoría
  ```typescript
  {
    tenantId: string;
    backupId: string;
    fecha: Date;
    tamaño: number; // bytes
    hash: string; // SHA256
    s3Path: string;
    tipoBackup: 'manual' | 'programado' | 'previo_operacion';
  }
  ```

- **BackupFallido** → Sistema de Alertas, Soporte Técnico
  ```typescript
  {
    tenantId: string;
    fecha: Date;
    motivo: string;
    fase: 'dump' | 'compresion' | 's3_upload';
    errorDetalle: string;
    intentosRealizados: number;
  }
  ```

---

#### Interacciones entre BCs

- **N/A** (operación de infraestructura sin dependencias de BCs de negocio)

---

#### Postcondiciones

**Éxito:**
- Archivo de backup generado y almacenado en S3
- Backup cifrado y con hash de verificación
- Administrador notificado con enlace de descarga
- Registro de auditoría del backup creado
- Política de retención aplicada (backups antiguos eliminados)

**Fallo:**
- Error registrado en logs del sistema
- Administrador notificado del fallo
- Reintento programado (si aplica)
- Estado del sistema sin cambios (idempotencia)

---

#### Notas de Implementación

**Tecnologías:**
- **pg_dump:** `--format=custom --compress=9 --no-owner --no-acl`
- **Cifrado:** AES-256-GCM con clave derivada del `tenant_id` + secret del sistema
- **Signed URLs:** AWS SDK `getSignedUrl()` con expiración 7 días
- **Scheduler:** BullMQ con cron expression configurable por tenant
- **Retención:** Job diario que verifica antigüedad y elimina backups > 30 días

**Consideraciones de Performance:**
- Ejecutar en horario nocturno para minimizar impacto
- Backups grandes (>1GB) se realizan en streaming para evitar cargar toda la BD en memoria
- Compresión nivel 9 para minimizar tamaño (trade-off: más CPU, menos storage)

**Consideraciones de Seguridad:**
- Backups SIEMPRE cifrados antes de subir a S3
- Signed URLs con expiración corta (7 días)
- Logs de backup NO incluyen datos sensibles (solo metadatos)
- Política S3 Bucket con acceso restringido por IAM Role

**Estimación de Tamaño:**
- Tenant pequeño (~100 socios): ~10-50 MB
- Tenant mediano (~500 socios): ~50-200 MB
- Tenant grande (~2000 socios): ~200 MB - 1 GB
- Incluye compresión ~70% del tamaño original

**RNF Relacionados:**
- RNF-T-033: Backup automático programable
- RNF-T-009: Cifrado de datos en reposo (S3)
- RNF-T-044: Disponibilidad 99.5% (recuperación desde backup)

**Casos de Uso Relacionados:**
- UC-010: Cierre de ejercicio (backup previo automático)
- UC-056: Importación masiva (backup previo recomendado)
- UC-057: Importación de histórico de pagos (backup previo recomendado)

---

## ANÁLISIS UC-016: [FUSIONADO CON UC-015]

### ✅ Estado: FUSIÓN VÁLIDA Y CORRECTAMENTE DOCUMENTADA

**Ubicación en documento:**
- ✅ Entrada en tabla BC-Membresia: `[FUSIONADO CON UC-015]`
- ✅ Nota explicativa completa (línea ~3798-3809)
- ✅ Referencia en changelog

---

### 📝 Contenido de la Nota de Fusión

```markdown
### UC-016: [FUSIONADO CON UC-015]

**Nota Explicativa:**
El caso de uso UC-016 fue originalmente planificado para gestionar la 
renovación anual de carnets, pero durante el análisis de diseño se 
identificó que esta funcionalidad está altamente acoplada con la 
generación inicial de carnets.

Para evitar fragmentación excesiva y mantener la cohesión del 
aggregate **Carnet**, UC-016 fue fusionado con **UC-015: Generación 
y validación de carnets**.

UC-015 ahora incluye:
- Generación de carnets digitales por primera vez
- Renovación automática al inicio de cada ejercicio
- Validación por escaneo QR
- Generación de carnets físicos y papeletas de sitio

**Ver:** UC-015 para documentación completa.
```

---

### 📊 User Stories Asociadas

**UC-015:**
- **US-043:** Generación de carnets digitales (QR, wallet Apple/Google)
- **US-044:** Renovación anual de carnets ← **Antes UC-016**
- **US-045:** Validación de carnet por QR
- **US-046:** Carnets físicos y papeletas (cofradías)

**Análisis:** US-044 (renovación anual) es la funcionalidad que originalmente iba a ser UC-016 independiente, pero se integró en UC-015.

---

### ✅ Validación Técnica de la Fusión

**Desde perspectiva de negocio:**
- ✅ **Cohesión funcional:** Generación inicial y renovación operan sobre el mismo aggregate (`Carnet`)
- ✅ **Flujo unificado:** La renovación es esencialmente una regeneración con nueva fecha de validez
- ✅ **Evita duplicación:** Ambos procesos comparten:
  - Generación de QR
  - Diseño de plantilla
  - Emisión de notificación
  - Almacenamiento en S3

**Desde perspectiva técnica:**
```
UC-016 fusionado con UC-015: La generación masiva de carnets comparte 
el 80% del código con generación individual. Se implementa como flag 
`batch: boolean` en `CarnetService.generarCarnets()`.
```

**Implementación sugerida:**
```typescript
// Application Service: CarnetService
async generarCarnets(
  socioIds: string[], 
  batch: boolean = false,
  ejercicio?: string
): Promise<Result<Carnet[]>> {
  // Lógica unificada para generación/renovación
  // Flag batch para procesamiento masivo
}
```

---

### ✅ Criterios de Validación de la Fusión

1. ✅ **Cohesión funcional:** Ambos UCs operan sobre el mismo Aggregate
2. ✅ **Evitar fragmentación:** 80% de código compartido
3. ✅ **Simplicidad:** Un solo Application Service en lugar de dos
4. ✅ **Misma transaccionalidad:** Ambos son operaciones atómicas
5. ✅ **Mismo actor:** Secretario ejecuta ambos

**Conclusión:** La fusión es **técnica y funcionalmente correcta**.

---

## 🎯 CONCLUSIONES Y RECOMENDACIONES

### Para UC-063: ✅ REQUIERE ACCIÓN CORRECTIVA

**Acciones necesarias:**

1. **Generar UC-063 completo** según propuesta en este documento
2. **Corregir tabla de Import/Export:**
   - Cambiar: `US-201, US-202` → `US-160`
   - Cambiar: `15 User Stories (US-188 a US-202)` → `13 User Stories (US-148 a US-160)`
3. **Insertar UC-063 en posición correcta** (después de UC-062, antes de UC-064)

**Estimación:** 1-2 horas

---

### Para UC-016: ✅ MANTENER FUSIÓN

**Análisis:**
✅ La fusión con UC-015 es **válida y bien justificada**

**Evidencia:**
1. ✅ Nota explicativa completa en el documento
2. ✅ Referencia clara a UC-015 para la documentación
3. ✅ Justificación técnica sólida (80% código compartido)
4. ✅ Cohesión del aggregate `Carnet` preservada
5. ✅ US-044 (renovación carnets) correctamente incluida en UC-015

**Recomendación:**
🟢 **MANTENER LA FUSIÓN** - No requiere acción correctiva

---

## 📋 RESPUESTA A LA PREGUNTA FINAL

### ¿Son 70 o 71 UCs el número correcto?

**Respuesta: 71 UCs es el número correcto.**

**Desglose:**
- **70 UCs con documentación completa individual** (UC-001 a UC-015, UC-017 a UC-071)
- **1 UC fusionado explícitamente** (UC-016 → UC-015)
- **Total de IDs UC asignados:** 71 (UC-001 a UC-071)

**Situación real:**
- ✅ UC-001 a UC-015: Documentados
- ⚠️ UC-016: Fusionado (documentado como nota, no como UC completo)
- ✅ UC-017 a UC-062: Documentados
- ❌ **UC-063: FALTA DOCUMENTAR** ← Único UC pendiente
- ✅ UC-064 a UC-071: Documentados

**Conteo de UCs operativos:**
- **70 UCs completos** (excluyendo UC-016 y UC-063)
- **1 UC fusionado** (UC-016)
- **1 UC faltante** (UC-063)

**Total de funcionalidades:** 70 UCs activos (71 IDs, pero UC-016 fusionado)

---

**Fecha de generación:** 06 Febrero 2026
