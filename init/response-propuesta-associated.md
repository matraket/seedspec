## Descripción breve del proyecto

Para este Trabajo de Fin de Máster se ha pensado en un **software SaaS** orientado a la **gestión integral de colectividades** (asociaciones, cofradías, clubes/equipos deportivos, peñas, etc.). El objetivo es **centralizar y ordenar** la información y los procesos que normalmente se llevan con Excel, papel y mensajería (altas/bajas, cuotas, documentos, comunicación, etc.), reduciendo el trabajo manual, los errores y la dependencia de una sola persona (p. ej., tesorería o secretaría). El resultado debe servir como base para definir un **MVP útil y vendible**, y un roadmap de mejoras posteriores.

---

## Propuestas de features para el SaaS (ideas para consolidar y convertir en tareas)

### 1) Gestión de entidades (multi-asociación) y roles
**Qué es:** Un sistema multi-tenant donde cada entidad tiene su propio espacio y usuarios con roles (admin, tesorería, secretaría, lectura, etc.).  
**Qué resuelve:** Control de accesos, seguridad y continuidad cuando cambian juntas directivas.

### 2) Registro de socios (ficha completa)
**Qué es:** Alta/edición/baja de socios con datos de contacto, tipo de socio, observaciones y estado.  
**Qué resuelve:** Evita listas dispersas en Excel y facilita que la secretaría tenga todo centralizado.

### 3) Histórico de socios (vida del socio)
**Qué es:** Historial de membresía por periodos (alta/baja), cambios relevantes y estados.  
**Qué resuelve:** Permite recuperar datos, trazabilidad y estadísticas (retención, altas/bajas por año).

### 4) Ejercicios / socios por año (temporadas)
**Qué es:** Gestión por ejercicios (2025, 2026...) para asociar cuotas, estados y actividad a cada año.  
**Qué resuelve:** Sustituye el “duplicar Excel cada año” y permite comparativas entre años.

### 5) Tipos de socio y reglas asociadas
**Qué es:** Configurar categorías (adulto/infantil/familiar/honorífico...) con condiciones y precios distintos.  
**Qué resuelve:** Evita cálculos manuales y errores cuando hay cuotas diferentes por perfil.

### 6) Gestión de cuotas (generación y seguimiento)
**Qué es:** Definir cuotas por ejercicio y crear “cargos” a socios (anual/mensual/trimestral/extraordinaria).  
**Qué resuelve:** Ordena el ciclo de cobros y permite saber en todo momento quién debe qué.

### 7) Registro de cobros (manual) y morosidad
**Qué es:** Registrar pagos por efectivo/transferencia/otros, con estado (pagado/pendiente/parcial).  
**Qué resuelve:** Reduce conflictos (“yo pagué”), y facilita perseguir impagos sin caos.

### 8) Justificantes y adjuntos por pago
**Qué es:** Adjuntar comprobantes (captura transferencia, recibo, factura) a un movimiento.  
**Qué resuelve:** Aporta trazabilidad y respaldo para auditorías internas o rendición de cuentas.

### 9) Repositorio de documentos por entidad (con cuota por plan)
**Qué es:** Espacio de almacenamiento con carpetas/categorías (actas, facturas, estatutos, subvenciones…).  
**Qué resuelve:** Centraliza documentación, evita que se pierda en Drive personales o WhatsApp y facilita traspasos.

### 10) Importación y exportación (Excel/CSV)
**Qué es:** Importar socios y estados desde Excel/CSV y exportar listados e informes básicos.  
**Qué resuelve:** Reduce la fricción de adopción (migración rápida) y permite respaldos/entregables para asamblea.

### 11) Comunicación segmentada (mínima viable)
**Qué es:** Enviar avisos/email por segmentos (activos, morosos, por tipo de socio, etc.) o tablón interno.  
**Qué resuelve:** Evita “mensajes a todo el mundo” y mejora la coordinación sin depender de grupos de WhatsApp.

### 12) Panel de control (dashboard básico)
**Qué es:** Métricas simples: socios activos por ejercicio, cuotas cobradas vs pendientes, altas/bajas recientes.  
**Qué resuelve:** Da visibilidad inmediata a junta/tesorería sin tener que “cocinar” reportes en Excel.

### 13) Consentimientos y privacidad (mínimo RGPD)
**Qué es:** Campos para registrar consentimientos (comunicaciones, imágenes) con fecha y texto.  
**Qué resuelve:** Reduce riesgos y mejora el orden legal sin burocracia extra.

### 14) Portal del socio (web responsive / PWA)
**Qué es:** Acceso para socios desde web móvil para ver cuotas, estado, documentos y avisos (sin app nativa).  
**Qué resuelve:** Aporta valor al socio final y reduce consultas repetidas a la junta (¿debo algo?, ¿dónde está X?).

### 15) Carnet digital (opcional, rápido)
**Qué es:** Carnet PDF/QR asociado al socio, válido por ejercicio, descargable desde el portal.  
**Qué resuelve:** Facilita control de acceso a eventos y moderniza la experiencia sin gran complejidad.

---

## Ideas post-MVP (para roadmap)

### A) Pagos online e integración bancaria
**Qué es:** Enlaces/QR de pago, conciliación, domiciliaciones SEPA y gestión de devoluciones.  
**Qué resuelve:** Automatiza cobros y reduce el trabajo manual del tesorero.

### B) Eventos e inscripciones
**Qué es:** Crear eventos, inscripción con plazas, lista de espera, check-in y cobro asociado.  
**Qué resuelve:** Ordena actividades y evita inscripciones por 4 canales distintos.

### C) Caja por turnos y cierres de evento
**Qué es:** Registro de caja por evento/turno con arqueo y cierre.  
**Qué resuelve:** Trazabilidad del efectivo y control económico real por actividad.

### D) Documentos “pro” (búsqueda, versionado, OCR)
**Qué es:** Etiquetas, búsqueda avanzada, control de versiones, OCR para facturas.  
**Qué resuelve:** Mejora la recuperación de información y la justificación de subvenciones.

### E) Actas, convocatorias y votaciones
**Qué es:** Plantillas, flujos de aprobación, registro de asistentes/quórum y votaciones.  
**Qué resuelve:** Profesionaliza el gobierno interno y deja evidencias claras.

### F) Inventario / patrimonio
**Qué es:** Inventario de bienes, préstamos, mantenimiento, responsables.  
**Qué resuelve:** Evita pérdidas y descontrol de material (muy útil en cofradías/peñas/deporte).

---

### Nota para convertirlo en tareas
Una forma rápida de pasar a backlog es marcar cada propuesta como:
- **MVP (must-have)**: socios + ejercicios + cuotas + cobros + documentos + import/export + roles + dashboard básico
- **Post-MVP (should/could)**: SEPA/pagos online, eventos, app nativa, OCR, votaciones, inventario