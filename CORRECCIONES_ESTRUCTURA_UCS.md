# Correcciones de Estructura - Casos de Uso
## Documento: 010_casos-uso_FINAL-HUMANO.md

**Fecha:** 06 Febrero 2026  
**Objetivo:** Homogeneizar estructura de todos los UCs con bloques obligatorios

---

## Resumen de Deficiencias Detectadas

| UC | Deficiencias Estructurales | Acción |
|----|---------------------------|--------|
| UC-034 | Falta **Notas de Implementación** | Generar contenido |
| UC-040 | Falta **Flujos de Excepción**, **Interacciones entre BCs** | Generar contenido |
| UC-041 | Falta **Actores**, **Precondiciones**, **Flujos Alternativos**, **Flujos de Excepción**, **Interacciones entre BCs**, **Postcondiciones**, **Notas de Implementación** | Generar contenido completo |
| UC-058 | Falta **Flujos de Excepción**, **Interacciones entre BCs**, **Postcondiciones**, **Notas de Implementación** | Generar contenido |
| UC-059 | Falta **Aggregates** en Metadatos, **Interacciones entre BCs** | Generar |
| UC-060 | Falta **Aggregates** en Metadatos (TIENE pero mal ubicado), **Interacciones entre BCs** | Reacomodar + generar |
| UC-061 | Falta **Aggregates** en Metadatos, **Interacciones entre BCs** | Generar |
| UC-062 | Falta **Aggregates** en Metadatos, **Interacciones entre BCs**, **Notas de Implementación** | Generar |
| UC-064 | Falta **Flujos de Excepción**, **Interacciones entre BCs** | Generar contenido |
| UC-067 | Falta **Interacciones entre BCs** | Generar contenido |
| UC-068 | Falta **Flujos Alternativos**, **Flujos de Excepción**, **Interacciones entre BCs** | Generar contenido |
| UC-069 | Falta **Flujos Alternativos**, **Flujos de Excepción** | Generar contenido |

---

## Correcciones Detalladas por UC

### UC-034: Eventos Específicos: Procesiones (Cofradías)

**Ubicación:** Línea ~11115

**Sección faltante:** Notas de Implementación

**Insertar después de:** Postcondiciones (buscar la línea que dice `#### Postcondiciones` del UC-034)

**Contenido a añadir:**

```markdown
#### Notas de Implementación

- **ACL a BC-Membresia:** Implementar adaptador `MembresiaACL` para consultas de antigüedad sin dependencia directa
- **Performance:** Precalcular antigüedad de hermanos en vista materializada para evitar cálculos en generación de papeletas (>500 hermanos)
- **Algoritmo de asignación:** Usar algoritmo estable para evitar cambios de posición si se regeneran papeletas (ordenación determinística)
- **QR Generation:** Librería `qrcode` para generar códigos QR embebidos en PDFs (formato PNG base64)
- **PDF Generation:** PDFKit para generar papeletas individuales con formato A6 (4 por página A4 para impresión)
- **Reservas de insignias:** Implementar cola de prioridad (Priority Queue) para gestionar conflictos en reservas simultáneas
- **Publicación programada:** Usar BullMQ scheduler para publicación automática en fecha/hora configurada
- **RNF-T-024:** Lazy loading de imágenes de insignias en lista de reservas (puede ser grande en cofradías con muchas insignias)
- **RNF-T-026:** Papeletas de sitio contienen datos personales → EventoDeAuditoria al generar/descargar
- **Validación negocio:** Validar que hermanos no tengan sanciones activas antes de asignar posición destacada
```

---

### UC-040: Envío de SMS para Urgencias

**Ubicación:** Línea ~14807

**Secciones faltantes:** Flujos de Excepción, Interacciones entre BCs

**Insertar después de:** Flujos Alternativos (buscar sección `#### Flujos Alternativos` del UC-040)

**Contenido a añadir:**

```markdown
#### Flujos de Excepción

**FE-1: Crédito SMS insuficiente**
- En paso de creación, si crédito < SMS necesarios:
  - Sistema bloquea envío y muestra: "Crédito insuficiente. Necesitas: X SMS. Disponible: Y"
  - Ofrece opciones: "Recargar crédito" o "Reducir destinatarios"
  - No crea comunicación hasta que haya crédito

**FE-2: Proveedor SMS no disponible (Twilio caído)**
- Si API de Twilio retorna error 5xx o timeout:
  - Sistema lanza `SmsProviderUnavailableException`
  - Respuesta HTTP 503 Service Unavailable
  - Sistema reintenta envío hasta 3 veces con backoff exponencial (5s, 15s, 45s)
  - Si falla definitivamente, marca comunicación como FALLIDA
  - Emite evento `ComunicacionFallida` → BC-Comunicacion notifica al emisor

**FE-3: Número de teléfono inválido**
- Si un destinatario tiene teléfono con formato inválido (no internacional, letras):
  - Sistema valida formato E.164 antes de enviar
  - Excluye número inválido del envío
  - Registra en logs con warning
  - Continúa con destinatarios válidos
  - Al finalizar, muestra resumen: "X enviados, Y excluidos por formato inválido"

**FE-4: Mensaje rechazado por filtro anti-spam**
- Si Twilio rechaza mensaje por contenido sospechoso (ej: palabras prohibidas):
  - Sistema lanza `SmsContentRejected Exception`
  - Respuesta HTTP 422 Unprocessable Entity
  - Muestra mensaje específico del proveedor
  - Sugiere reformular mensaje

#### Interacciones entre BCs

- **BC-Comunicacion → BC-Membresia:** Consulta teléfonos móviles de destinatarios por segmento (ACL)
- **BC-Comunicacion → Proveedor Externo (Twilio/Vonage):** Envío de SMS vía API REST
- **BC-Comunicacion → BC-Tesoreria:** Registro de coste SMS como gasto del ejercicio (si configurado)
- **BC-Comunicacion → BC-Auditoría:** Registro de EventoDeAuditoria por envío SMS (coste asociado)
```

---

### UC-041: Notificaciones Push vía PWA

**Ubicación:** Línea ~14987

**Secciones faltantes:** Actores, Precondiciones, Flujos Alternativos, Flujos de Excepción, Interacciones entre BCs, Postcondiciones, Notas de Implementación

**Insertar después de:** Descripción (línea ~14997)

**Contenido a añadir:**

```markdown
#### Actores

- **Presidente / Secretario** (envía notificaciones push a socios)
- **Socio** (activa/desactiva notificaciones en PWA)
- **Sistema** (gestiona suscripciones, envía notificaciones)

#### Precondiciones

- Socio tiene PWA instalada en dispositivo
- Socio activó notificaciones (granted permission en navegador)
- Suscripción push registrada y activa en sistema
- Service Worker registrado en PWA (sw.js activo)
```

**Insertar después de:** Eventos de Dominio (línea ~15067, buscar final de tabla de eventos)

**Contenido a añadir:**

```markdown
#### Flujos Alternativos

**FA-1: Socio desactiva notificaciones**
- Socio accede a "Configuración" en Portal > "Notificaciones"
- Pulsa "Desactivar notificaciones push"
- Sistema marca suscripción como `activa = false` en BD
- No elimina registro (permite reactivar fácilmente)
- Socio deja de recibir notificaciones pero puede reactivar

**FA-2: Envío con acción rápida (actionable notification)**
- Al crear notificación, emisor configura acción (ej: "Ver evento", "Confirmar asistencia")
- Payload incluye `actions: [{ action: 'view', title: 'Ver detalle', url: '/eventos/123' }]`
- Al hacer click en acción, PWA navega directamente a la URL especificada
- Útil para notificaciones de inscripciones, pagos pendientes, etc.

**FA-3: Notificación programada (scheduled push)**
- Emisor configura fecha/hora de envío futura
- Sistema encola notificación en BullMQ scheduler
- A la hora programada, ejecuta envío
- Útil para recordatorios de eventos (24h antes, 1h antes)

#### Flujos de Excepción

**FE-1: Suscripción expirada (error 410 Gone)**
- Durante envío, si web-push retorna 410:
  - Sistema marca suscripción como `activa = false`
  - Registra en logs: "Suscripción expirada para socioId X"
  - NO intenta reenviar a esa suscripción
  - Continúa con otros destinatarios
  - Socio deberá reactivar notificaciones manualmente

**FE-2: Ningún destinatario con push activo**
- Si el segmento no tiene socios con suscripciones activas:
  - Sistema muestra: "Ningún destinatario tiene notificaciones push activadas"
  - Sugiere: "Envía un email invitando a activar notificaciones"
  - No crea comunicación

**FE-3: Payload demasiado grande (>4KB)**
- Si el contenido de la notificación supera límite de web-push (4KB):
  - Sistema lanza `PayloadTooLargeException`
  - Respuesta HTTP 422 Unprocessable Entity
  - Sugiere acortar mensaje o usar enlace en lugar de texto largo

**FE-4: Permisos push denegados en navegador**
- Si socio intenta activar notificaciones pero ya denegó permisos:
  - Sistema muestra: "Permisos denegados. Actívalos en la configuración de tu navegador"
  - Muestra instrucciones específicas por navegador (Chrome, Firefox, Safari)

#### Interacciones entre BCs

- **BC-Comunicacion → BC-Membresia:** Consulta socios por segmento (ACL read-only)
- **BC-Comunicacion → Service Worker (PWA):** Envío de notificación vía Web Push Protocol
- **BC-Comunicacion:** Gestión interna de SuscripcionPush (create, update, delete)

**Nota:** Push notifications son internas a BC-Comunicacion, no requieren coordinación compleja con otros BCs.

#### Postcondiciones

**Éxito:**
- Notificaciones enviadas a todos los socios con suscripciones activas
- Suscripciones expiradas marcadas como inactivas automáticamente
- Evento `NotificacionPushEnviada` emitido con número de destinatarios alcanzados
- Socio recibe notificación en dispositivo (si está online) o al volver online (si offline)
- Registro de envío en tabla `comunicaciones` con estado ENVIADA

**Fallo:**
- Notificaciones no enviadas
- Error registrado en logs (proveedor caído, payload inválido, etc.)
- Comunicación marcada como FALLIDA
- Usuario emisor notificado del fallo
- Suscripciones problemáticas registradas para revisión

#### Notas de Implementación

- **Librería:** `web-push` (npm) para envío de notificaciones siguiendo Web Push Protocol
- **VAPID Keys:** Generar par de claves pública/privada para autenticación con navegadores (RNF-T-007)
- **Service Worker:** Implementar handler `push` event en `sw.js` para mostrar notificación
- **Persistencia:** Almacenar suscripciones con endpoint, keys.p256dh, keys.auth (cifrado end-to-end)
- **TTL:** Configurar Time-To-Live de notificaciones (ej: 24h). Si el dispositivo no está online en ese período, la notificación expira
- **Badge:** Incluir badge count para mostrar número de notificaciones pendientes en icono de app
- **Silent push:** NO implementar silent push (requiere permisos adicionales y puede violar políticas de navegadores)
- **RNF-T-045:** Notificaciones deben ser concisas (máx 120 caracteres en body) para buena UX móvil
- **RNF-T-026:** No incluir datos sensibles en notificación (solo avisos generales). Datos completos se consultan al abrir la app
- **Testing:** Usar herramientas como `web-push-testing-service` para probar en desarrollo sin certificados SSL
```

---

### UC-058: Exportación de Listados y Plantillas

**Ubicación:** Línea ~22706

**Secciones faltantes:** Flujos de Excepción, Interacciones entre BCs, Postcondiciones, Notas de Implementación

**Insertar después de:** Flujos Alternativos (buscar sección `#### Flujos Alternativos` del UC-058)

**Contenido a añadir:**

```markdown
#### Flujos de Excepción

**FE-1: Sin socios en el filtro especificado**
- Si query retorna 0 socios con los filtros aplicados:
  - Sistema muestra: "No se encontraron socios con los criterios seleccionados"
  - Sugiere ampliar filtros o revisar criterios
  - No genera archivo vacío

**FE-2: Campo solicitado no existe**
- Si la configuración incluye un campo personalizado que no existe en el tenant:
  - Sistema omite ese campo y continúa con los demás
  - Muestra warning: "Campo 'X' no encontrado, omitido de la exportación"

**FE-3: Error en generación de archivo**
- Si falla librería ExcelJS/CSV por memoria insuficiente (>100,000 registros):
  - Sistema lanza `ExportGenerationException`
  - Respuesta HTTP 500 Internal Server Error
  - Sugiere filtrar por estado o tipo de socio para reducir volumen

**FE-4: IBAN no disponible (campo protegido)**
- Si usuario sin permisos `tesoreria:read` solicita campo IBAN:
  - Sistema bloquea: "No tienes permisos para exportar IBANs"
  - Excluye campo IBAN de la exportación
  - Continúa con campos permitidos

#### Interacciones entre BCs

- **Transversal → BC-Membresia:** Consulta de socios con filtros (ACL read-only)
- **Transversal → BC-Tesoreria:** Consulta de IBANs si campo solicitado (requiere permisos)
- **Transversal → S3/MinIO:** Almacenamiento de archivo generado

**Nota:** Este UC actúa como orquestador de lectura cross-BC sin lógica de dominio.

#### Postcondiciones

**Éxito:**
- Archivo generado (XLSX, CSV o PDF) con datos solicitados
- Usuario notificado con enlace de descarga (presigned URL válido 24h)
- Registro de auditoría creado (RGPD Art. 30 - exportación de datos personales)
- Archivo almacenado en S3 con TTL 7 días (limpieza automática)

**Fallo:**
- Archivo no generado
- Usuario notificado de error (sin socios, permisos insuficientes, etc.)
- No se registra en auditoría hasta que exportación sea exitosa

#### Notas de Implementación

- **Librería Excel:** ExcelJS para generar .xlsx con formato profesional (bordes, colores alternos, filtros)
- **Librería CSV:** Implementación nativa con encoding UTF-8 BOM (compatible Excel Windows)
- **Librería PDF:** PDFKit para generar listados tabulares en PDF/A-1b
- **Plantillas predefinidas:**
  - `asamblea`: Nombre, Apellidos, Tipo, FechaAlta (sin datos contacto)
  - `banco`: NumeroSocio, Nombre, IBAN (solo tesorero)
  - `subvencion`: Datos completos + DNI (para justificantes oficiales)
- **Performance:** Para exportaciones >10,000 registros, procesar en chunks de 5,000 y generar progresivamente
- **RNF-T-026:** Campos sensibles (DNI, IBAN) cifrados en BD, descifrados solo durante exportación
- **RNF-T-009:** Usar presigned URLs de S3 con expiración 24h
- **RNF-T-058:** Testear exportación con datasets grandes (>50,000 registros) para validar performance
```

---

### UC-059: Exportación de informes económicos

**Ubicación:** Línea ~23013

**Secciones faltantes:** Aggregates en Metadatos, Interacciones entre BCs

**Modificar Metadatos** (línea ~23015, añadir línea después de Application Service):

```markdown
**Aggregates:**
  - BC-Tesoreria: **CuentaSocio** (saldos), **Movimiento** (transacciones), **RemesaSepa** (cobros domiciliados)
  - BC-Membresia: **Socio** (para estadísticas de morosidad)
```

**Insertar después de:** Eventos de Dominio (línea ~23099, buscar línea que dice `- \`EconomicReportGenerated\``)

**Contenido a añadir:**

```markdown
#### Interacciones entre BCs

- **Transversal → BC-Tesoreria:** Consulta agregada de Movimientos, Cargos, Remesas por ejercicio (ACL)
- **Transversal → BC-Membresia:** Consulta conteo de socios morosos (ACL)
- **Transversal → S3/MinIO:** Almacenamiento de PDF generado
- **Transversal → BC-Comunicacion:** Notificación con enlace de descarga (vía evento `EconomicReportGenerated`)

**Nota:** Operación de solo lectura con agregación de datos. No modifica estado de ningún BC.
```

---

### UC-060: Exportación fiscal Modelo 182 (AEAT)

**Ubicación:** Línea ~23868

**Secciones faltantes:** Aggregates en Metadatos (mal ubicado), Interacciones entre BCs

**Modificar Metadatos** (línea ~23870, añadir después de Application Service):

```markdown
**Aggregates:**
  - BC-Tesoreria: **Movimiento** (donaciones con importe), **EjercicioContable** (validar estado cerrado)
  - BC-Membresia: **Socio** (NIF/CIF de donantes, validación formato)
  - BC-Cumplimiento: **AlertaFiscal** (registro obligación tributaria presentada)
```

**Insertar después de:** Eventos de Dominio (línea ~23958, buscar línea que dice `- \`Modelo182Generated\``)

**Contenido a añadir:**

```markdown
#### Interacciones entre BCs

- **Transversal → BC-Tesoreria:** Consulta de Movimientos tipo DONATIVO del ejercicio cerrado
- **Transversal → BC-Membresia:** Consulta de NIFs de donantes (validación formato mod 23)
- **Transversal → BC-Cumplimiento:** Registro de presentación Modelo 182 (auditoría fiscal)
- **Transversal → S3/MinIO:** Almacenamiento de XML generado con retención indefinida
- **Transversal → AEAT (externo):** Usuario sube XML manualmente al portal AEAT (fuera del sistema)

**Nota:** Sistema genera XML pero NO envía telemáticamente a AEAT. Requiere presentación manual por asesor fiscal con certificado digital.
```

---

### UC-061: Exportación de listados de eventos

**Ubicación:** Línea ~24593

**Secciones faltantes:** Aggregates en Metadatos, Interacciones entre BCs

**Modificar Metadatos** (línea ~24595, añadir después de Application Service):

```markdown
**Aggregates:**
  - BC-Eventos: **Evento**, *Inscripcion* (lista de inscritos con estado)
  - BC-Membresia: **Socio** (datos personales de inscritos vía ACL)
```

**Insertar después de:** Eventos de Dominio (línea ~24689, buscar final de tabla de eventos)

**Contenido a añadir:**

```markdown
#### Interacciones entre BCs

- **Transversal → BC-Eventos:** Consulta de Inscripciones por eventoId con datos de comanda
- **Transversal → BC-Membresia:** Enriquecimiento con datos de Socio (nombre, email, teléfono) vía ACL
- **Transversal → S3/MinIO:** Almacenamiento de Excel/CSV generado con TTL 48h
- **Transversal → BC-Comunicacion:** (opcional) Envío de listado por email si solicitado

**Nota:** Lectura cross-BC sin modificación de estado. QR codes generados on-the-fly, no almacenados.
```

---

### UC-062: Exportación de histórico de comunicaciones

**Ubicación:** Línea ~25271

**Secciones faltantes:** Aggregates en Metadatos, Interacciones entre BCs, Notas de Implementación

**Modificar Metadatos** (línea ~25273, añadir después de Application Service):

```markdown
**Aggregates:**
  - BC-Comunicacion: **Comunicacion**, *Envio* (registros de envío individuales)
  - BC-Membresia: **Socio** (para filtrado por destinatario vía ACL)
```

**Insertar después de:** Eventos de Dominio (línea ~25365, buscar final de eventos)

**Contenido a añadir:**

```markdown
#### Interacciones entre BCs

- **Transversal → BC-Comunicacion:** Consulta paginada de tabla `Comunicacion` con joins a `ComunicacionEvento`
- **Transversal → BC-Membresia:** (opcional) Filtrado por socioId específico si petición RGPD Art. 15
- **Transversal → S3/MinIO:** Almacenamiento de Excel generado con retención 90 días (auditoría)

**Nota:** Export con datos sensibles (emails personales). Acceso restringido a rol `comunicacion:audit`.
```

**Insertar después de:** Postcondiciones (buscar sección `#### Postcondiciones` del UC-062)

**Contenido a añadir:**

```markdown
#### Notas de Implementación

- **Librería:** ExcelJS para generar Excel con 2 pestañas + formato condicional (colores por estado)
- **Performance:** Consulta paginada interna en chunks de 5,000 comunicaciones para evitar timeout
- **Métricas agregadas:** Calcular tasas (apertura, click, rebote) con queries SQL GROUP BY optimizadas
- **Checksum:** Calcular MD5 hash del archivo para verificación de integridad en auditorías
- **Retención:** Archivos almacenados 90 días en S3 con lifecycle policy (delete after 90d)
- **RGPD:** Si export incluye emails de socios, registrar EventoDeAuditoria (acceso a datos personales)
- **Anonimización:** Si `anonymize: true`, aplicar SHA-256 hash a emails antes de incluir en Excel
- **RNF-T-026:** Datos sensibles (emails, teléfonos) solo accesibles con permiso `comunicacion:audit`
- **RNF-T-016:** Export sincrónico hasta 10,000 comunicaciones. Si >10,000, usar procesamiento asíncrono (BullMQ)
```

---

### UC-064: Dashboard Principal y KPIs

**Ubicación:** Línea ~26009

**Secciones faltantes:** Flujos de Excepción, Interacciones entre BCs

**Insertar después de:** Flujos Alternativos (buscar sección `#### Flujos Alternativos` del UC-064)

**Contenido a añadir:**

```markdown
#### Flujos de Excepción

**FE-1: Error en consulta de BC específico**
- Si BC-Tesoreria no responde (timeout, BD caída):
  - Sistema muestra widget con mensaje: "Datos económicos temporalmente no disponibles"
  - Carga resto de widgets normalmente (fallos aislados no bloquean dashboard completo)
  - Registra error en logs para debugging

**FE-2: Tenant sin datos históricos (recién creado)**
- Si tenant tiene <7 días de antigüedad:
  - Sistema muestra mensaje: "Aún no hay datos suficientes para métricas"
  - Muestra widgets de configuración inicial (pendientes de completar)
  - Sugiere: "Configura tipos de socio, cuotas y crea tu primer evento"

**FE-3: Usuario sin permisos para ver ciertos KPIs**
- Si usuario con rol `vocal` (sin permisos financieros):
  - Sistema oculta widgets económicos (cuotas, recaudación)
  - Muestra solo widgets permitidos (eventos, comunicación)
  - NO muestra error, simplemente no renderiza widgets restringidos

**FE-4: Timeout en cálculo de métricas complejas**
- Si query de agregación tarda >5 segundos:
  - Sistema cancela query y muestra widget con loader infinito
  - Tras 10 segundos, muestra: "El cálculo está tardando más de lo habitual. Intenta reducir el período"
  - Opción: "Refrescar" o "Cambiar a período más corto"

#### Interacciones entre BCs

- **Dashboard → BC-Membresia:** Consulta conteos de socios por estado, tipo (ACL read-only)
- **Dashboard → BC-Tesoreria:** Consulta métricas económicas (recaudación, morosidad, saldos) (ACL)
- **Dashboard → BC-Eventos:** Consulta próximos eventos e inscripciones (ACL)
- **Dashboard → BC-Comunicacion:** Consulta métricas de comunicación (envíos, aperturas) (ACL)

**Nota:** Dashboard es capa de presentación pura (BFF). Toda lógica de negocio reside en BCs. Dashboard solo orquesta consultas y presenta datos.
```

---

### UC-067: Certificados y memorias personalizables

**Ubicación:** Línea ~28257

**Sección faltante:** Interacciones entre BCs

**Insertar después de:** Eventos de Dominio (línea que contiene `CertificadoEmitido`, buscar después de esa línea)

**Contenido a añadir:**

```markdown
#### Interacciones entre BCs

- **Transversal → BC-Membresia:** Consulta datos de Socio (nombre, número, estado) para certificado
- **Transversal → BC-Tesoreria:** Verificación de saldo de CuentaSocio (para certificado "al corriente")
- **Transversal → BC-Eventos:** Consulta de Inscripcion con asistencia confirmada (para certificado asistencia)
- **Transversal → BC-Identidad:** Consulta cargos directivos activos (para certificado Junta)
- **Transversal → BC-Documentos:** Almacenamiento de PDF generado como Documento oficial
- **Transversal → BC-Comunicacion:** (opcional) Envío de certificado por email si solicitado

**Nota:** Sistema de plantillas Handlebars permite generar certificados sin lógica hardcodeada. Cada tipo de certificado tiene su plantilla con placeholders específicos.
```

---

### UC-068: Acceso al Portal y Autenticación

**Ubicación:** Línea ~29047

**Secciones faltantes:** Flujos Alternativos, Flujos de Excepción, Interacciones entre BCs

**Insertar después de:** Flujo Normal (buscar línea que dice `async enviarMagicLink`, luego buscar el final del bloque TypeScript, línea ~29147)

**Contenido a añadir:**

```markdown
}
```

**Luego insertar:**

```markdown

#### Flujos Alternativos

**FA-1: Autenticación con magic link (sin contraseña)**
- Socio sin contraseña configurada accede a portal
- Introduce solo email y pulsa "Enviar enlace mágico"
- Sistema ejecuta `enviarMagicLink(email)`
- Socio recibe email con enlace único válido 15 minutos
- Al hacer click, sistema valida token y crea sesión automáticamente
- Útil para socios que no recuerdan contraseña o la han olvidado

**FA-2: Primer acceso (activación de cuenta)**
- Socio recién registrado recibe email de bienvenida con enlace de activación
- Enlace contiene token único de activación
- Al acceder, sistema solicita establecer contraseña
- Validación: mínimo 8 caracteres, mayúscula, número, carácter especial (RNF-T-001)
- Tras establecer contraseña, sesión se crea automáticamente

**FA-3: Recordar sesión (Remember Me)**
- En login, socio marca checkbox "Recordar sesión"
- Sistema genera Refresh Token con expiración 30 días (en lugar de 7 días)
- Cookie httpOnly con Refresh Token se guarda en navegador
- Próximas visitas no requieren re-login hasta que token expire

#### Flujos de Excepción

**FE-1: Email no registrado en sistema**
- Si usuario introduce email no existente:
  - Sistema NO revela que no existe (seguridad)
  - Muestra: "Si el email está registrado, recibirás un enlace"
  - No envía email (evitar enumeración de usuarios)

**FE-2: Magic link expirado**
- Si socio hace click en magic link tras 15 minutos:
  - Sistema muestra: "Este enlace ha expirado"
  - Opción: "Solicitar nuevo enlace"
  - Redirige a formulario de login con email pre-rellenado

**FE-3: Socio con estado inactivo (dado de baja)**
- Si socio con estado BAJA intenta acceder:
  - Sistema bloquea: "Tu cuenta está desactivada. Contacta con la secretaría"
  - No permite login ni envío de magic link
  - Registra intento de acceso en auditoría

**FE-4: Múltiples intentos fallidos (brute force)**
- Si socio falla login 5 veces en 10 minutos:
  - Sistema bloquea cuenta temporalmente 15 minutos
  - Muestra: "Demasiados intentos fallidos. Cuenta bloqueada 15 minutos"
  - Opción: "Restablecer contraseña" (envía email)
  - Tras 15 min, contador se resetea automáticamente

#### Interacciones entre BCs

- **BC-Identidad → BC-Membresia:** Verificación de estado de Socio (debe ser ACTIVO)
- **BC-Identidad → BC-Comunicacion:** Envío de email con magic link (vía evento o directa)
- **BC-Identidad:** Generación y validación de tokens JWT internamente

**Nota:** Portal de Socio usa autenticación simplificada (solo rol `socio`). No requiere selector de tenant como panel administrativo.
```

---

### UC-069: Consulta de Datos Personales y Cuotas

**Ubicación:** Línea ~29256

**Secciones faltantes:** Flujos Alternativos, Flujos de Excepción

**Insertar después de:** Interacciones entre BCs (línea ~29341, buscar final de sección `#### Interacciones entre BCs`)

**Contenido a añadir:**

```markdown

#### Flujos Alternativos

**FA-1: Descarga de recibo de pago**
- En vista de estado de cuotas, socio hace click en "Descargar recibo" de una cuota pagada
- Sistema genera PDF de recibo con:
  - Datos del socio
  - Detalle del cargo pagado (concepto, importe, fecha)
  - Método de pago utilizado (domiciliación, transferencia, efectivo)
  - Sello de "PAGADO" con fecha
- PDF se genera on-the-fly, no se almacena permanentemente

**FA-2: Solicitud de modificación de datos**
- Socio detecta error en sus datos (ej: dirección antigua)
- Pulsa "Solicitar cambio" junto al campo
- Sistema muestra formulario con valor actual y campo para nuevo valor
- Socio introduce cambio solicitado y justificación
- Sistema crea `SolicitudCambioDatos` y notifica a secretario
- Secretario aprueba/rechaza cambio desde panel admin

**FA-3: Descarga de carnet digital**
- Si socio tiene carnet activo para ejercicio actual:
  - Muestra botón "Descargar carnet digital"
  - Sistema genera imagen PNG del carnet con QR de validación
  - Formato optimizado para mobile (añadir a Apple Wallet / Google Pay)
- Si no tiene carnet activo:
  - Muestra mensaje: "Solicita tu carnet en secretaría"

#### Flujos de Excepción

**FE-1: Socio intenta acceder a datos de otro socio**
- Si socio manipula URL con `socioId` diferente:
  - Sistema valida que `JWT.socioId === requested_socioId`
  - Si no coincide, lanza `ForbiddenException`
  - Respuesta HTTP 403 Forbidden
  - Registra intento en auditoría (posible ataque)

**FE-2: Error al consultar estado de cuotas (BC-Tesoreria caído)**
- Si BC-Tesoreria no responde:
  - Sistema muestra: "Estado de cuotas temporalmente no disponible"
  - Muestra solo datos personales (disponibles en BC-Membresia)
  - Opción: "Reintentar" o "Contactar con tesorería"

**FE-3: Socio sin cuotas configuradas (recién dado de alta)**
- Si socio aún no tiene cargos generados:
  - Sistema muestra: "Aún no tienes cuotas asignadas"
  - Mensaje: "Se generarán automáticamente en los próximos días"

**FE-4: Descarga de recibo falla (PDF no disponible)**
- Si falla generación de PDF de recibo:
  - Sistema muestra: "Error al generar recibo. Intenta de nuevo"
  - Registra error en logs
  - Ofrece opción: "Solicitar recibo por email" (envío manual por tesorero)
```

---

## Resumen de Correcciones

| UC | Bloques Añadidos | Líneas Estimadas | Tipo |
|----|------------------|------------------|------|
| UC-034 | Notas Implementación | ~15 líneas | Contenido técnico |
| UC-040 | Flujos Excepción, Interacciones BC | ~45 líneas | Contenido completo |
| UC-041 | 7 secciones completas | ~150 líneas | Estructura completa |
| UC-058 | 4 secciones | ~50 líneas | Contenido completo |
| UC-059 | Aggregates, Interacciones BC | ~10 líneas | Metadata + ref |
| UC-060 | Aggregates, Interacciones BC | ~15 líneas | Metadata + ref |
| UC-061 | Aggregates, Interacciones BC | ~10 líneas | Metadata + ref |
| UC-062 | Aggregates, Interacciones BC, Notas | ~30 líneas | Contenido completo |
| UC-064 | Flujos Excepción, Interacciones BC | ~40 líneas | Contenido completo |
| UC-067 | Interacciones BC | ~10 líneas | Referencia cross-BC |
| UC-068 | Flujos Alt, Flujos Exc, Interacciones BC | ~80 líneas | Contenido completo |
| UC-069 | Flujos Alt, Flujos Exc | ~60 líneas | Contenido completo |

**Total:** ~515 líneas de contenido a añadir

---

## Proceso de Aplicación

### Opción A: Edición Manual
1. Abrir `010_casos-uso_FINAL-HUMANO.md`
2. Buscar cada UC por número (Ctrl+F)
3. Localizar punto de inserción indicado
4. Copiar contenido de este documento
5. Pegar en ubicación correcta
6. Verificar formato markdown

### Opción B: Aplicación Automatizada (Recomendada)
1. Usar herramienta Edit con las ubicaciones exactas
2. Aplicar correcciones secuencialmente por UC
3. Verificar tras cada edición

---

## Validación Post-Corrección

**Checklist por UC:**

- [ ] UC-034: Notas de Implementación presentes
- [ ] UC-040: Flujos de Excepción + Interacciones BC presentes
- [ ] UC-041: Estructura completa con 11 secciones
- [ ] UC-058: Flujos Exc + Interacciones + Post + Notas presentes
- [ ] UC-059: Aggregates en Metadatos + Interacciones BC presentes
- [ ] UC-060: Aggregates en Metadatos + Interacciones BC presentes
- [ ] UC-061: Aggregates en Metadatos + Interacciones BC presentes
- [ ] UC-062: Aggregates + Interacciones + Notas presentes
- [ ] UC-064: Flujos Excepción + Interacciones BC presentes
- [ ] UC-067: Interacciones BC presentes
- [ ] UC-068: Flujos Alt + Flujos Exc + Interacciones BC presentes
- [ ] UC-069: Flujos Alt + Flujos Exc presentes

**Script de verificación:**

```bash
# Verificar que cada UC tiene las secciones obligatorias
for uc in UC-034 UC-040 UC-041 UC-058 UC-059 UC-060 UC-061 UC-062 UC-064 UC-067 UC-068 UC-069; do
  echo "Verificando $uc..."
  # Verificar presencia de secciones según deficiencias identificadas
done
```

---

**Fecha de creación:** 06 Febrero 2026  
**Estado:** Listo para aplicar correcciones
