# Listado de User Stories Huérfanas
## Para Asignación a Casos de Uso

**Total:** 42 User Stories sin UC asignado  
**Fecha:** 06 Febrero 2026

---

## BC-Identidad (1 huérfana)

### US-006: Registro de auditoría de acciones críticas
- **RF:** N2RF06
- **Prioridad:** Must ⚠️
- **Acción:** Añadir a UC-005 (Traspaso de cargos) o crear UC-006 dedicado
- **Descripción:** Registro de todas las acciones críticas con detalle (usuario, timestamp, operación, valores anterior/nuevo, IP origen)

---

## BC-Documentos (2 huérfanas)

### US-141: Búsqueda de documentos
- **RF:** N7RF07
- **Prioridad:** Should
- **Acción:** Añadir a UC-053 (Búsqueda y filtrado de documentos)
- **Descripción:** Motor de búsqueda por nombre, contenido (si OCR), categoría, fecha

### US-147: Vinculación de documentos con otros módulos
- **RF:** N7RF12
- **Prioridad:** Should
- **Acción:** Añadir a UC-054 (Control de permisos y límites)
- **Descripción:** Vincular documentos con socios, eventos, actas, pagos para contexto

---

## Transversal N8 - Import/Export (7 huérfanas)

### US-152: Importación de histórico de pagos
- **RF:** N8RF02
- **Prioridad:** Should
- **Acción:** Añadir a UC-057 (Importación de histórico de pagos)
- **Descripción:** Importar pagos históricos desde Excel/CSV para migración

### US-154: Exportación de informe de cuotas y pagos
- **RF:** N8RF04
- **Prioridad:** Should
- **Acción:** Añadir a UC-059 (Exportación de informes económicos)
- **Descripción:** Exportar informe detallado de cuotas y pagos por socio/periodo

### US-155: Exportación de libro de ingresos y gastos
- **RF:** N8RF05
- **Prioridad:** Should
- **Acción:** Añadir a UC-059 (Exportación de informes económicos)
- **Descripción:** Exportar libro de ingresos y gastos en formato oficial ENL

### US-156: Exportación de datos para Modelo 182
- **RF:** N8RF06
- **Prioridad:** Could
- **Acción:** Completar UC-060 (Exportación fiscal Modelo 182)
- **Descripción:** Generar XML para declaración Modelo 182 AEAT (donaciones)

### US-157: Exportación de listado de asistentes a evento
- **RF:** N8RF07
- **Prioridad:** Should
- **Acción:** Completar UC-061 (Exportación de listados de eventos)
- **Descripción:** Exportar listado de asistentes para justificación de subvenciones

### US-158: Exportación de histórico de comunicaciones
- **RF:** N8RF08
- **Prioridad:** Could
- **Acción:** Completar UC-062 (Exportación de histórico comunicaciones)
- **Descripción:** Exportar histórico de emails/SMS enviados para auditoría

### US-160: Backup periódico de datos
- **RF:** N8RF13
- **Prioridad:** Must ⚠️
- **Acción:** Completar UC-063 (Backup completo automático)
- **Descripción:** Backup automático diario con pg_dump, retención 30 días, subida a S3

---

## Transversal N9 - Reporting (8 huérfanas)

### US-165: Gráfico de evolución de socios
- **RF:** N9RF03
- **Prioridad:** Should
- **Acción:** Añadir a UC-064 (Dashboard principal y KPIs)
- **Descripción:** Gráfico de evolución temporal de altas, bajas y total de socios

### US-166: Gráfico de recaudación mensual
- **RF:** N9RF04
- **Prioridad:** Should
- **Acción:** Añadir a UC-064 (Dashboard principal y KPIs)
- **Descripción:** Gráfico de recaudación mensual vs presupuestado

### US-167: Dashboard adaptado por rol
- **RF:** N9RF05
- **Prioridad:** Should
- **Acción:** Añadir a UC-064 (Dashboard principal y KPIs)
- **Descripción:** Dashboard personalizado según rol (Presidente, Tesorero, Secretario, etc.)

### US-168: Informe de socios para Asamblea
- **RF:** N9RF06
- **Prioridad:** Must ⚠️
- **Acción:** Añadir a UC-066 (Informes para Asamblea General)
- **Descripción:** Informe de estado de socios: altas, bajas, total, morosos, para Asamblea

### US-169: Informe económico para Asamblea
- **RF:** N9RF07
- **Prioridad:** Must ⚠️
- **Acción:** Añadir a UC-066 (Informes para Asamblea General)
- **Descripción:** Informe económico anual: ingresos, gastos, saldo, para Asamblea

### US-170: Certificado de estar al corriente de pago
- **RF:** N9RF08
- **Prioridad:** Must ⚠️
- **Acción:** Añadir a UC-067 (Certificados y memorias personalizables)
- **Descripción:** Generar certificado oficial de estar al corriente de pago para socio

### US-171: Certificado de composición de Junta
- **RF:** N9RF09
- **Prioridad:** Should
- **Acción:** Añadir a UC-067 (Certificados y memorias personalizables)
- **Descripción:** Certificado oficial de composición de Junta Directiva vigente

### US-172: Memoria de actividades para subvenciones
- **RF:** N9RF10
- **Prioridad:** Should
- **Acción:** Añadir a UC-067 (Certificados y memorias personalizables)
- **Descripción:** Memoria de actividades del ejercicio para justificar subvenciones

---

## Transversal N10 - Portal del Socio (9 huérfanas)

### US-179: Acceso al carnet digital
- **RF:** N10RF05
- **Prioridad:** Must ⚠️
- **Acción:** Añadir a UC-069 o crear UC-070 (Carnet digital)
- **Descripción:** Socio accede a su carnet digital con QR desde el portal

### US-180: Consulta de calendario de eventos
- **RF:** N10RF06
- **Prioridad:** Should
- **Acción:** Añadir a UC-069 (Consulta de datos personales y cuotas)
- **Descripción:** Socio consulta calendario de eventos desde portal

### US-181: Inscripción a eventos desde portal
- **RF:** N10RF07
- **Prioridad:** Should
- **Acción:** Crear UC-070 (Inscripción a eventos desde portal)
- **Descripción:** Socio se inscribe a eventos directamente desde el portal

### US-182: Consulta de avisos y comunicaciones
- **RF:** N10RF08
- **Prioridad:** Should
- **Acción:** Añadir a UC-069 (Consulta de datos personales y cuotas)
- **Descripción:** Socio consulta avisos y comunicaciones recibidas

### US-183: Acceso a documentos públicos
- **RF:** N10RF09
- **Prioridad:** Should
- **Acción:** Crear UC-071 (Descarga de documentos personales)
- **Descripción:** Socio accede a documentos públicos (actas, normas, etc.)

### US-184: Actualización de preferencias de comunicación
- **RF:** N10RF10
- **Prioridad:** Should
- **Acción:** Añadir a UC-069 (Consulta de datos personales y cuotas)
- **Descripción:** Socio actualiza preferencias de comunicación (email, SMS, push)

### US-185: Gestión de consentimientos RGPD
- **RF:** N10RF11
- **Prioridad:** Must ⚠️
- **Acción:** Crear UC-072 o añadir a UC-069
- **Descripción:** Socio gestiona sus consentimientos RGPD desde el portal

### US-186: PWA instalable
- **RF:** N10RF12
- **Prioridad:** Should
- **Acción:** Añadir a UC-068 (Acceso al portal y autenticación)
- **Descripción:** Portal es instalable como PWA en dispositivos móviles

### US-187: Consentimiento para notificaciones push
- **RF:** N10RF13
- **Prioridad:** Should
- **Acción:** Añadir a UC-068 (Acceso al portal y autenticación)
- **Descripción:** Socio consiente recibir notificaciones push desde el portal

---

## Transversal N11 - Cumplimiento Normativo (15 huérfanas) 🚨 CRÍTICO

### US-188: Registro de Actividades de Tratamiento (RAT)
- **RF:** N11RF01
- **Prioridad:** Must ⚠️
- **Acción:** Crear UC-072 (Gestión RGPD - RAT)
- **Descripción:** Registro de Actividades de Tratamiento según Art. 30 RGPD

### US-189: Gestión de consentimientos
- **RF:** N11RF02
- **Prioridad:** Must ⚠️
- **Acción:** Crear UC-072 (Gestión RGPD - Consentimientos)
- **Descripción:** Gestión de consentimientos explícitos para tratamiento de datos

### US-190: Ejercicio del derecho de acceso
- **RF:** N11RF03
- **Prioridad:** Must ⚠️
- **Acción:** Crear UC-073 (Ejercicio de derechos ARCO)
- **Descripción:** Socio ejerce derecho de acceso a sus datos personales (Art. 15 RGPD)

### US-191: Ejercicio del derecho de rectificación
- **RF:** N11RF04
- **Prioridad:** Must ⚠️
- **Acción:** Crear UC-073 (Ejercicio de derechos ARCO)
- **Descripción:** Socio ejerce derecho de rectificación de datos incorrectos (Art. 16 RGPD)

### US-192: Ejercicio del derecho de supresión
- **RF:** N11RF05
- **Prioridad:** Must ⚠️
- **Acción:** Crear UC-073 (Ejercicio de derechos ARCO)
- **Descripción:** Socio ejerce derecho de supresión "derecho al olvido" (Art. 17 RGPD)

### US-193: Ejercicio del derecho de portabilidad
- **RF:** N11RF06
- **Prioridad:** Must ⚠️
- **Acción:** Crear UC-073 (Ejercicio de derechos ARCO)
- **Descripción:** Socio ejerce derecho de portabilidad de datos (Art. 20 RGPD)

### US-194: Base jurídica para datos religiosos
- **RF:** N11RF07
- **Prioridad:** Must ⚠️
- **Acción:** Crear UC-072 (Gestión RGPD - Datos sensibles)
- **Descripción:** Base jurídica para tratamiento de datos religiosos en cofradías (Art. 9 RGPD)

### US-195: Libro de socios digital
- **RF:** N11RF08
- **Prioridad:** Must ⚠️
- **Acción:** Crear UC-074 (Cumplimiento Ley de Asociaciones)
- **Descripción:** Libro de socios digital según Art. 21 LO 1/2002

### US-196: Inventario de bienes
- **RF:** N11RF09
- **Prioridad:** Should
- **Acción:** Crear UC-074 (Cumplimiento Ley de Asociaciones)
- **Descripción:** Inventario de bienes de la asociación según Art. 21 LO 1/2002

### US-197: Validación de Asamblea General anual
- **RF:** N11RF10
- **Prioridad:** Should
- **Acción:** Crear UC-075 (Alertas de cumplimiento legal)
- **Descripción:** Validación de convocatoria de Asamblea General Ordinaria anual (Art. 12 LO 1/2002)

### US-198: Validación de Junta Directiva
- **RF:** N11RF11
- **Prioridad:** Should
- **Acción:** Crear UC-075 (Alertas de cumplimiento legal)
- **Descripción:** Validación de composición y renovación de Junta Directiva según estatutos

### US-199: Alerta de comunicación al Registro
- **RF:** N11RF12
- **Prioridad:** Should
- **Acción:** Crear UC-075 (Alertas de cumplimiento legal)
- **Descripción:** Alerta de obligación de comunicar cambios al Registro de Asociaciones

### US-200: Alerta de renovación de Junta
- **RF:** N11RF13
- **Prioridad:** Should
- **Acción:** Crear UC-075 (Alertas de cumplimiento legal)
- **Descripción:** Alerta de próxima renovación de Junta Directiva según estatutos

### US-201: Alerta de plazos fiscales
- **RF:** N11RF14
- **Prioridad:** Should
- **Acción:** Crear UC-076 (Alertas fiscales y contables)
- **Descripción:** Alertas de plazos fiscales (Modelo 182, 347, etc.)

### US-202: Alerta de caducidad de documentos
- **RF:** N11RF15
- **Prioridad:** Should
- **Acción:** Crear UC-076 (Alertas fiscales y contables)
- **Descripción:** Alertas de caducidad de documentos (certificados médicos, licencias, seguros)

---

## Resumen de Casos de Uso a Crear/Completar

### UCs Existentes a Completar (13)
- UC-005 o UC-006: +US-006 (BC-Identidad)
- UC-053: +US-141 (BC-Documentos)
- UC-054: +US-147 (BC-Documentos)
- UC-057: +US-152 (Import/Export)
- UC-059: +US-154, US-155 (Import/Export)
- UC-060: +US-156 (Import/Export)
- UC-061: +US-157 (Import/Export)
- UC-062: +US-158 (Import/Export)
- UC-063: +US-160 (Import/Export)
- UC-064: +US-165, US-166, US-167 (Reporting)
- UC-066: +US-168, US-169 (Reporting)
- UC-067: +US-170, US-171, US-172 (Reporting)
- UC-068 a UC-071: +US-179 a US-187 (Portal Socio)

### UCs Nuevos a Crear (6)
- **UC-072:** Gestión RGPD (US-188, US-189, US-194) → 3 Must
- **UC-073:** Ejercicio derechos ARCO (US-190, US-191, US-192, US-193) → 4 Must
- **UC-074:** Cumplimiento Ley Asociaciones (US-195, US-196, US-197, US-198) → 1 Must, 3 Should
- **UC-075:** Alertas cumplimiento legal (US-199, US-200) → 2 Should
- **UC-076:** Alertas fiscales y contables (US-201, US-202) → 2 Should

---

## Priorización de Trabajo

### CRÍTICO (8 Must)
1. US-188: RAT (RGPD)
2. US-189: Consentimientos (RGPD)
3. US-190: Derecho acceso (RGPD)
4. US-191: Derecho rectificación (RGPD)
5. US-192: Derecho supresión (RGPD)
6. US-193: Derecho portabilidad (RGPD)
7. US-194: Base jurídica datos religiosos (RGPD)
8. US-195: Libro de socios digital (LO 1/2002)

### ALTA (7 Must restantes)
9. US-006: Registro de auditoría (BC-Identidad)
10. US-160: Backup periódico (Import/Export)
11. US-168: Informe socios Asamblea (Reporting)
12. US-169: Informe económico Asamblea (Reporting)
13. US-170: Certificado al corriente (Reporting)
14. US-179: Carnet digital (Portal Socio)
15. US-185: Consentimientos RGPD portal (Portal Socio)

### MEDIA (27 Should/Could restantes)
16-42. Resto de User Stories Should y Could

---

**FIN DEL LISTADO**

**Próxima acción:** Asignar responsables para documentar los UCs faltantes según priorización.
