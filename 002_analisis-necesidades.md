# Análisis de Necesidades: ERP Ligero para Colectividades Españolas

**Proyecto:** Associated  
**Versión:** 1.0
**Fecha:** Febrero 2026

---

## Resumen Ejecutivo

**Las asociaciones culturales, cofradías, clubes deportivos y peñas festeras españolas comparten un problema crítico: el 75% sigue gestionando socios, cuentas y comunicaciones con hojas de Excel o papel.** Este documento consolida las necesidades identificadas en conversaciones con juntas directivas, tesoreros voluntarios y responsables de entidades reales de Aragón y otras comunidades, estableciendo los requisitos funcionales para un ERP ligero que resuelva sus pain points específicos.

El tejido asociativo español -más de **300.000 asociaciones activas**- sostiene buena parte de la vida cultural, festiva y deportiva del país, especialmente en el ámbito rural. Sin embargo, la gestión de estas entidades recae mayoritariamente en voluntarios sin formación administrativa que enfrentan obligaciones legales, fiscales y de protección de datos cada vez más exigentes. La brecha entre las obligaciones y las herramientas disponibles genera frustración, errores y abandono de responsabilidades.

---

## 1. El Problema de Fondo: Voluntarios Gestionando con Herramientas del Siglo Pasado

La realidad que describen los tesoreros entrevistados es unánime: heredan "un Excel con macros que nadie sabe cómo funciona" y una caja de zapatos con recibos. **"Cuando llegué a tesorero de la peña, me encontré con siete versiones diferentes del listado de socios"**, relata un responsable de una peña turolense de 440 socios. La situación se agrava con cada relevo de junta directiva, donde la información se pierde o fragmenta.

Los problemas documentados incluyen:
- Pérdida de trazabilidad histórica
- Errores en cálculos manuales de cuotas
- Devoluciones bancarias no controladas
- Incumplimiento de plazos fiscales por desconocimiento
- Comunicaciones duplicadas o contradictorias

**El coste oculto más grave es el freno al relevo generacional**: nadie quiere asumir la tesorería porque "es un lío tremendo" y "el anterior tesorero se fue sin explicar nada".

Las soluciones de mercado como Berrly o Playoff (**34-70€/mes**) resuelven parte del problema, pero sus precios resultan prohibitivos para asociaciones pequeñas con presupuestos ajustados. El software específico para cofradías (HermandApp, Hermandia, GranCofrade) ofrece funcionalidades muy especializadas pero fragmentadas. **Ninguna solución actual integra las necesidades de los cuatro tipos de colectividades en una única plataforma económica.**

---

## 2. Necesidades de Arquitectura y Acceso al Sistema

### 2.1 Necesidad de Aislamiento Multi-Entidad (Multi-Tenant)

**Contexto del problema:**
Muchas personas pertenecen simultáneamente a varias colectividades (cofradía + peña + asociación cultural). Actualmente, cada entidad gestiona sus datos de forma completamente aislada, lo que genera duplicidad de información y dificulta la visión consolidada para el socio.

**Necesidades identificadas:**
- Cada colectividad debe tener un espacio completamente aislado con sus propios datos, configuraciones y usuarios
- Un mismo usuario (persona física) puede pertenecer a múltiples entidades con roles diferentes en cada una
- Los datos de una entidad nunca deben ser accesibles desde otra
- Cada entidad debe poder personalizar su configuración (tipos de socio, cuotas, campos personalizados) sin afectar a otras

**Problema que resuelve:**
- Permite que un usuario acceda a todas sus colectividades desde un único punto de entrada
- Garantiza la privacidad y seguridad de los datos de cada entidad
- Facilita el modelo de negocio SaaS con planes diferenciados por entidad

### 2.2 Necesidad de Control de Accesos y Roles

**Contexto del problema:**
En las colectividades, los cargos rotan frecuentemente (elecciones cada 2-4 años típicamente). Cuando cambia la junta directiva, el traspaso de accesos es caótico: contraseñas compartidas, emails personales con información de la entidad, y falta de trazabilidad sobre quién hizo qué.

**Necesidades identificadas:**
- Sistema de roles que refleje la estructura real de las colectividades:
  - **Presidente/Hermano Mayor**: Acceso total, aprobación de operaciones críticas
  - **Secretario**: Gestión de socios, actas, documentación oficial
  - **Tesorero**: Gestión económica, cuotas, cobros, informes fiscales
  - **Vocales**: Acceso limitado según área (eventos, comunicación, etc.)
  - **Socio**: Solo lectura de su propia información via portal
- Auditoría de acciones críticas (quién, cuándo, qué modificó)
- Proceso de traspaso de cargos que preserve el histórico y revoque accesos anteriores
- Posibilidad de roles personalizados según necesidades de cada entidad

**Contexto normativo:**
La Ley Orgánica 1/2002 establece que la Junta Directiva debe estar compuesta solo por asociados mayores de edad. El sistema debe poder validar esta restricción.

**Problema que resuelve:**
- Continuidad operativa en cambios de junta
- Seguridad y control de accesos
- Trazabilidad para rendición de cuentas en Asamblea

---

## 3. Necesidades de Gestión de Socios/Miembros

### 3.1 Registro Centralizado de Socios (Ficha Completa)

**Contexto del problema:**
La secretaría de las entidades mantiene listas de socios en múltiples formatos: Excel, libretas, fichas en papel, grupos de WhatsApp. Cuando alguien pregunta "¿este señor es socio?", a menudo hay que consultar varias fuentes para confirmarlo.

**Necesidades identificadas:**

**Datos básicos (comunes a todas las colectividades):**
- Nombre completo, DNI/NIE, fecha de nacimiento
- Dirección postal, email, teléfono(s)
- IBAN para domiciliación de cuotas
- Fotografía para carnet
- Fecha de alta, estado actual, tipo de socio

**Datos específicos por tipo de colectividad:**

| Colectividad | Campos específicos |
|--------------|-------------------|
| **Cofradías** | Fecha de bautismo, parroquia, padrinos/avalistas, fecha de jura de reglas, imposición de medalla |
| **Clubes deportivos** | Categoría deportiva, número de licencia federativa, certificado médico (fecha caducidad), certificado delitos sexuales (para técnicos) |
| **Peñas festeras** | Talla de camiseta, preferencias alimentarias (para comidas), disponibilidad para turnos de voluntariado |
| **Asociaciones culturales** | Áreas de interés, habilidades que puede aportar, disponibilidad horaria |

**Estados del socio:**
- Activo (al corriente de pago)
- Pendiente de pago (morosidad leve)
- Suspendido (morosidad grave, sin derechos)
- Baja voluntaria
- Baja por impago (tras procedimiento estatutario)
- Baja disciplinaria (tras expediente)
- Aspirante / Lista de espera
- Fallecido (mantener para histórico)

**Problema que resuelve:**
- Elimina listas dispersas y versiones contradictorias
- Centraliza la información para consulta inmediata
- Facilita el cumplimiento de RGPD (datos en un solo lugar)

### 3.2 Tipos de Miembros y Reglas Asociadas

**Contexto del problema:**
Cada colectividad maneja categorías de socios con derechos y obligaciones específicas. El cálculo manual de cuotas diferentes por perfil genera errores frecuentes.

**Necesidades identificadas por tipo de colectividad:**

**Cofradías y hermandades:**
- Hermanos numerarios: Pleno derecho, voz y voto, cuota completa
- Hermanos honorarios: Reconocimiento sin cuota, sin derecho a voto
- Aspirantes/neófitos: Periodo de formación (1 año mínimo típicamente), cuota reducida
- Menores de edad: Inscritos por tutores, derechos limitados hasta los 18 años
- **Regla crítica de antigüedad:** Solo con 2+ años pueden votar, y 5+ años para ser Hermano Mayor

**Peñas festeras:**
- Adultos: Cuota completa (95-175€/año según datos de peñas turolenses)
- Juveniles/menores de 35 años: Descuentos del 30-40%
- Infantiles menores de 14 años: Cuotas reducidas (80-100€)
- Socios de honor: Sin cuota, reconocimiento especial
- **Caso especial:** La peña El Agüelo de Teruel funciona con cuota simbólica de 0€ para sus 1.400 socios

**Clubes deportivos:**
- Socio del club: Paga cuota, participa en Asamblea, no practica deporte
- Deportista federado: Requiere licencia, seguro, vinculación a modalidad
- Socio-deportista: Ambas condiciones
- Familiar: Cuota reducida para familiares directos de socios

**Asociaciones culturales:**
- Ordinarios: Cuota estándar (30-60€/año típicamente)
- Fundadores: Reconocimiento especial, posibles beneficios
- Honorarios: Sin cuota, por méritos
- Protectores: Aportación económica superior

**Reglas configurables necesarias:**
- Edad mínima/máxima por categoría
- Cuota asociada (fija o calculada)
- Derechos: voto en asamblea, elegibilidad para cargos
- Periodo de carencia antes de adquirir ciertos derechos
- Transiciones automáticas (ej: de juvenil a adulto al cumplir edad)

### 3.3 Histórico y Trazabilidad del Socio (Vida del Socio)

**Contexto del problema:**
Cuando cambia la junta directiva, se pierde el contexto histórico de los socios. Preguntas como "¿cuántos años lleva de socio?", "¿pagó el año pasado?" o "¿participó en la última asamblea?" requieren bucear en archivos dispersos.

**Necesidades identificadas:**
- **Timeline completo del socio** con todos los eventos relevantes:
  - Fecha de alta inicial y sucesivas altas/bajas
  - Cambios de categoría de socio
  - Historial de pagos (cuotas, eventos, otros conceptos)
  - Participación en eventos y actividades
  - Cargos ocupados en junta directiva
  - Sanciones o expedientes (si los hubiera)
  - Comunicaciones relevantes recibidas

- **Cálculo automático de antigüedad:**
  - Crítico para cofradías: determina derechos de voto y elegibilidad para cargos
  - Debe considerar periodos de baja (¿se acumula o se reinicia?)

- **Estadísticas derivadas:**
  - Tasa de retención por año
  - Altas/bajas por periodo
  - Socios activos vs históricos

**Problema que resuelve:**
- Trazabilidad completa sin depender de la memoria de personas
- Continuidad en cambios de junta
- Base para estadísticas y toma de decisiones

### 3.4 Gestión por Ejercicios / Temporadas

**Contexto del problema:**
La mayoría de colectividades trabajan por ejercicios anuales (año natural o temporada). Actualmente, esto se traduce en "duplicar el Excel de socios cada año" y perder la conexión entre ejercicios.

**Necesidades identificadas:**
- **Concepto de ejercicio** como entidad que agrupa:
  - Estado de cada socio en ese ejercicio (activo, baja, etc.)
  - Cuotas y pagos asociados al ejercicio
  - Eventos y actividades del ejercicio
  - Documentación generada (actas, memorias, etc.)

- **Operaciones de ejercicio:**
  - Apertura de nuevo ejercicio con arrastre automático de socios activos
  - Cierre de ejercicio con generación de memoria/informe
  - Comparativas entre ejercicios (evolución de socios, recaudación, etc.)

- **Flexibilidad de fechas:**
  - Año natural (enero-diciembre): típico de asociaciones
  - Temporada deportiva (septiembre-agosto): clubes deportivos
  - Ejercicio cofrade (puede variar según tradición local)

**Problema que resuelve:**
- Elimina el "duplicar Excel cada año"
- Permite comparativas históricas
- Facilita informes anuales para Asamblea

### 3.5 Procesos de Alta, Baja y Modificación

**Contexto del problema:**
El alta de un nuevo miembro puede ser un simple formulario o un proceso complejo con múltiples pasos. Actualmente se gestiona con papel, emails y seguimiento mental.

**Necesidades identificadas:**

**Proceso de alta (configurable según tipo de entidad):**

Para **cofradías** (proceso complejo documentado):
1. Solicitud escrita al Hermano Mayor
2. Aval de dos hermanos con 2+ años de antigüedad
3. Entrega de documentación (partida de bautismo, etc.)
4. Pago de cuota de inscripción
5. Periodo de formación con asistencia a sesión informativa
6. Jura de reglas ante Junta de Oficiales
7. Imposición de medalla y cordón con cruz

Para **peñas y asociaciones** (proceso simple):
1. Solicitud (formulario online o presencial)
2. Pago de cuota
3. Alta efectiva

**El sistema debe gestionar:**
- Flujo de estados configurable (solicitud → pendiente documentación → pendiente pago → activo)
- Documentos pendientes de entregar con alertas
- Fechas límite y recordatorios
- Lista de espera con prioridades (para entidades con límite de socios)

**Proceso de baja:**
- **Voluntaria:** Solicitud por escrito, efectiva según estatutos (fin de ejercicio típicamente)
- **Por impago:** Tras 2 años sin pagar cuotas con notificación previa (según mayoría de estatutos)
- **Disciplinaria:** Expediente con audiencia al interesado, resolución de Junta, posible recurso a Asamblea

**Contexto jurídico:** La jurisprudencia exige proporcionalidad. Una sentencia de Cantabria anuló una expulsión por impago de recargos por considerarla desproporcionada.

### 3.6 Listas de Espera

**Contexto del problema:**
Algunas entidades populares tienen más demanda que capacidad. La peña La Botera de Teruel mantiene lista de espera activa para preservar su carácter familiar con límite de 440 socios.

**Necesidades identificadas:**
- Registro de solicitudes con fecha y datos del aspirante
- Prioridades configurables (hijos de socios, recomendados, orden cronológico)
- Notificación automática cuando hay vacante
- Plazo para aceptar la plaza antes de pasar al siguiente
- Histórico de solicitudes rechazadas/caducadas

### 3.7 Carnets y Documentación de Identidad

**Contexto del problema:**
El carnet de socio sirve para identificación en eventos, acceso a descuentos, y en cofradías para acreditar la participación en procesiones.

**Necesidades identificadas:**

**Carnet digital:**
- Código QR único por socio y ejercicio
- Validación instantánea mediante escaneo
- Accesible desde el portal del socio (PWA)
- Modelo de referencia: App Interpeñas de Teruel

**Carnet físico (opcional):**
- Generación de PDF imprimible
- Diseño personalizable por entidad (logo, colores)
- Datos mínimos: nombre, foto, número de socio, ejercicio vigente, QR

**Documentación específica de cofradías:**
- **Papeleta de sitio:** Acredita posición en el cortejo procesional, vinculada a cuota y antigüedad
- Debe poder generarse masivamente antes de Semana Santa

---

## 4. Necesidades de Tesorería y Finanzas

### 4.1 Definición y Generación de Cuotas

**Contexto del problema:**
Cada entidad tiene su estructura de cuotas, con diferentes importes por tipo de socio, periodicidad variable, y cuotas extraordinarias puntuales. El tesorero calcula manualmente quién debe qué.

**Necesidades identificadas:**

**Tipos de cuota:**
- Cuota ordinaria: Periodicidad configurable (anual, semestral, trimestral, mensual)
- Cuota de inscripción: Pago único al darse de alta
- Cuota extraordinaria: Derramas puntuales (obras, eventos especiales)
- Cuota de evento: Vinculada a actividad específica

**Generación de cargos:**
- Generación masiva por tipo de socio y ejercicio
- Prorrateo automático para altas a mitad de ejercicio (configurable)
- Aplicación de descuentos por categoría (juvenil, familiar, etc.)
- Exenciones configurables (socios de honor, situaciones especiales)

**Problema que resuelve:**
- Saber en todo momento quién debe qué
- Eliminar cálculos manuales y errores asociados

### 4.2 Registro de Cobros y Métodos de Pago

**Contexto del problema:**
Los socios pagan por múltiples vías: efectivo en mano, transferencia, Bizum, domiciliación. El tesorero apunta en una libreta o Excel quién ha pagado, con frecuentes errores y conflictos ("yo ya pagué").

**Necesidades identificadas:**

**Métodos de pago a registrar:**
- Efectivo (requiere recibo como justificante)
- Transferencia bancaria (con referencia para identificar)
- Domiciliación SEPA (automatizado)
- Bizum / pago móvil
- Tarjeta (si hay pasarela online)

**Estados del pago:**
- Pendiente: Cargo generado, no cobrado
- Pagado: Cobro confirmado
- Parcial: Pago incompleto
- Devuelto: Recibo domiciliado rechazado por el banco
- Anulado: Cargo cancelado (error, exención posterior)

**Información del cobro:**
- Fecha de pago
- Método utilizado
- Referencia/identificador
- Usuario que registró el cobro
- Justificante adjunto (si aplica)

### 4.3 Justificantes y Adjuntos por Pago

**Contexto del problema:**
Para auditorías internas, rendición de cuentas en Asamblea, o disputas con socios, es necesario poder demostrar que un pago se realizó. Actualmente los justificantes están dispersos en emails, capturas de WhatsApp, o simplemente no existen.

**Necesidades identificadas:**
- Adjuntar comprobante a cada movimiento (captura de transferencia, recibo firmado, extracto bancario)
- Formatos soportados: PDF, imagen (JPG, PNG)
- Visualización rápida sin necesidad de descargar
- Generación automática de recibo para pagos en efectivo

**Problema que resuelve:**
- Trazabilidad completa para auditorías
- Resolución de disputas con socios ("yo pagué" → "aquí está el justificante")
- Respaldo para rendición de cuentas en Asamblea

### 4.4 Gestión de Morosidad

**Contexto del problema:**
La morosidad en cuotas es alta por falta de seguimiento sistemático. Muchos tesoreros "dejan pasar" impagos por no tener un proceso claro o por evitar conflictos personales.

**Necesidades identificadas:**

**Workflow de morosidad (configurable según estatutos):**
1. Detección automática del impago (cuota no cobrada en fecha)
2. Primera comunicación amistosa (email automático, configurable)
3. Recordatorio tras X días (segundo aviso)
4. Reclamación formal tras Y días (carta/burofax si persiste)
5. Suspensión de derechos según estatutos
6. Expediente sancionador con audiencia al interesado
7. Baja por impago tras certificación de descubierto

**Plazos típicos según estatutos:**
- Baja por impago tras 2 años sin pagar cuotas
- Notificación previa obligatoria
- Periodo para regularizar antes de la baja definitiva

**Documentación generada:**
- Certificado de descubierto (firmado por tesorero, VºBº de presidente)
- Notificaciones enviadas con acuse de recibo
- Resolución de baja con fecha y motivo

**Rehabilitación:**
- Posibilidad de volver a darse de alta abonando deudas pendientes
- Penalización adicional si lo establecen los estatutos (recargo, nueva cuota de inscripción)

### 4.5 Cobro Automatizado: Remesas SEPA

**Contexto del problema:**
La funcionalidad más demandada por tesoreros. Actualmente, generar una remesa bancaria requiere conocimientos técnicos o pagar a una gestoría.

**Necesidades identificadas:**

**Requisitos técnicos SEPA:**
- Generación de ficheros XML en formato ISO 20022 (pain.008.001.08)
- Identificador de acreedor del Banco de España (obligatorio)
- Gestión de mandatos SEPA firmados por cada socio
- Caducidad de mandatos: si no hay adeudos en 36 meses, caduca

**Tipos de secuencia:**
- FRST: Primer cobro a un socio (o primer cobro tras >36 meses)
- RCUR: Cobros recurrentes
- OOFF: Cobro único
- FNAL: Último cobro de una serie

**Presentación:**
- Plazo mínimo: 3 días hábiles de antelación (SEPA CORE)
- Exportación de fichero para subir al banco online

**Gestión de devoluciones:**
- El deudor puede devolver en 8 semanas (adeudo autorizado)
- Hasta 13 meses si alega operación no autorizada
- Cada devolución genera gastos bancarios (3-5€ típicamente)
- Registro de motivo de devolución
- Reintento con fecha distinta

### 4.6 Pasarela de Pago Online

**Contexto del problema:**
Para cuotas extraordinarias, inscripciones a eventos, o socios que no quieren domiciliar, es útil ofrecer pago puntual con tarjeta.

**Necesidades identificadas:**
- Generación de enlaces de pago únicos por concepto
- QR para pago desde móvil
- Conciliación automática con cargos pendientes
- Comisión asumida por entidad o repercutida al socio (configurable)

### 4.7 Contabilidad para Entidades Sin Ánimo de Lucro

**Contexto del problema:**
Las asociaciones tienen obligaciones contables específicas. El Plan de Contabilidad para ENL (RD 1491/2011) es obligatorio para las de utilidad pública, pero incluso las pequeñas necesitan contabilidad ordenada.

**Necesidades identificadas:**

**Para asociaciones pequeñas (no utilidad pública):**
- Libro de ingresos y gastos bien estructurado
- Categorización de movimientos (cuotas, subvenciones, gastos operativos, eventos)
- Informe para presentar en Asamblea General

**Para asociaciones de utilidad pública (cumplimiento RD 1491/2011):**
- Plan de cuentas específico ENL
- Fondo social (en lugar de capital social)
- Cuenta 720: Cuotas de asociados
- Cuenta 740: Donaciones
- Tratamiento de subvenciones (capital vs corrientes)
- Memoria con apartado 12 obligatorio

**Informes necesarios:**
- Balance de situación
- Cuenta de resultados (Excedente del ejercicio)
- Memoria económica para Asamblea

### 4.8 Obligaciones Fiscales y Alertas

**Contexto del problema:**
Muchos tesoreros desconocen las obligaciones fiscales de las asociaciones. Hacienda ha intensificado los requerimientos a entidades que incumplen.

**Necesidades identificadas:**

**Modelos fiscales aplicables:**

| Modelo | Obligación | Plazo | Cuándo aplica |
|--------|-----------|-------|---------------|
| **182** | Declaración de donativos | 1-31 enero | Asociaciones de utilidad pública, donaciones >150€ |
| **190** | Resumen retenciones IRPF | 1-31 enero | Si contratan personal o profesionales |
| **111** | Retenciones trimestrales | 20 abril/julio/octubre/enero | Ídem anterior |
| **200** | Impuesto Sociedades | 25 julio | Obligatorio salvo exención |
| **347** | Operaciones con terceros | Febrero | Operaciones >3.005,06€ con mismo proveedor/cliente |

**Exención de declarar Impuesto de Sociedades** (tres requisitos simultáneos):
- Ingresos totales ≤ 75.000€
- Rentas no exentas sometidas a retención ≤ 2.000€
- Todas las rentas no exentas con retención aplicada

**Alertas necesarias:**
- Aviso cuando se aproximen umbrales de exención
- Recordatorio de plazos de presentación
- Generación de datos para Modelo 182 (listado de donantes)

### 4.9 Caja por Turnos y Arqueos (Específico Peñas)

**Contexto del problema:**
Durante las fiestas, las peñas operan un bar con turnos de voluntarios. El control del efectivo es crítico para evitar descuadres y suspicacias.

**Necesidades identificadas:**
- Apertura de caja con fondo inicial
- Registro de ventas por turno (manual o con TPV básico)
- Cierre de turno con arqueo (efectivo contado vs registrado)
- Cierre de evento con balance total
- Asignación de responsable por turno
- Informe de descuadres para investigar

---

## 5. Necesidades de Gestión de Eventos

### 5.1 Planificación y Calendario de Actividades

**Contexto del problema:**
Las colectividades organizan múltiples actividades a lo largo del año. Actualmente se gestionan en calendarios personales, grupos de WhatsApp, o la memoria del organizador.

**Necesidades identificadas:**

**Tipos de eventos por colectividad:**

| Colectividad | Tipos de eventos |
|--------------|------------------|
| **Cofradías** | Cultos (novenas, triduos, besamanos), procesiones, juntas, ensayos de costaleros |
| **Peñas** | Fiestas patronales, comidas populares, excursiones, asambleas |
| **Clubes** | Entrenamientos, partidos, torneos, asambleas, eventos sociales |
| **Asociaciones** | Talleres, conferencias, excursiones, asambleas |

**Información del evento:**
- Nombre, descripción, tipo
- Fecha/hora inicio y fin
- Ubicación (con posibilidad de mapa)
- Aforo máximo (si aplica)
- Precio/cuota de inscripción (si aplica)
- Documentación asociada (programa, normas)
- Responsable/organizador

**Calendario:**
- Vista mensual, semanal, lista
- Filtrado por tipo de evento
- Exportación a calendarios externos (iCal)
- Sincronización con calendario del socio

### 5.2 Inscripciones y Control de Aforo

**Contexto del problema:**
Las inscripciones a eventos llegan por múltiples canales (WhatsApp, email, presencial, llamada) y se apuntan donde se puede. No hay control real de plazas ni lista de espera.

**Necesidades identificadas:**
- Inscripción online desde portal del socio
- Control de plazas disponibles en tiempo real
- Lista de espera automática cuando se completa aforo
- Notificación a lista de espera si hay bajas
- Cierre de inscripciones en fecha límite
- Inscripción con pago asociado (si el evento tiene coste)

**Datos de inscripción:**
- Socio inscrito (o datos si es externo)
- Fecha de inscripción
- Estado (confirmado, pendiente de pago, cancelado)
- Opciones elegidas (menú, transporte, etc.)
- Información adicional (alergias, necesidades especiales)

### 5.3 Check-in y Control de Asistencia

**Contexto del problema:**
Saber quién asistió realmente a un evento es útil para estadísticas, justificación de subvenciones, y control de pagos.

**Necesidades identificadas:**
- Check-in mediante escaneo de QR del carnet
- Check-in manual por lista
- Registro de hora de entrada
- Informe de asistencia vs inscritos
- Exportación para justificación de actividades (subvenciones)

### 5.4 Eventos Específicos: Comidas Populares (Peñas)

**Contexto del problema:**
Las comidas populares durante fiestas requieren gestión específica: plazas limitadas, menús con opciones, alergias, pago anticipado.

**Necesidades identificadas:**
- Gestión de menús con opciones (vegetariano, infantil, etc.)
- Registro obligatorio de alergias/intolerancias
- Cobro anticipado (típico 35-50€)
- Lista de comensales para el catering
- Asignación de mesas (opcional)
- Control de asistencia el día del evento

### 5.5 Eventos Específicos: Procesiones y Cultos (Cofradías)

**Contexto del problema:**
La organización de la salida procesional es la actividad más compleja de una cofradía, con asignaciones de posiciones por antigüedad, gestión de insignias, y control de asistencia.

**Necesidades identificadas:**

**Papeletas de sitio:**
- Generación por antigüedad estricta
- Reserva de insignias y varas
- Publicación de listas (típicamente Viernes de Dolores)
- Impresión o envío digital

**Cuadrillas de costaleros/portadores:**
- Igualás digitales (medición de altura)
- Registro de costaleros con posiciones históricas
- Notificaciones de ensayos
- Control de asistencia a ensayos
- Gestión de relevos durante la procesión

**Cultos:**
- Calendario de novenas, triduos, quinarios, etc.
- Control de aforo para eventos masivos (besamanos)
- Inscripción previa si es necesario

### 5.6 Eventos Específicos: Competiciones (Clubes Deportivos)

**Contexto del problema:**
Los clubes deportivos gestionan partidos, convocatorias, y resultados. El control de sanciones acumuladas es crítico para evitar alineaciones indebidas.

**Necesidades identificadas:**
- Calendario de competición (liga, copa, amistosos)
- Convocatoria de jugadores (con límites según categoría)
- Control de sanciones acumuladas (tarjetas, expulsiones)
- Alerta de jugadores no convocables por sanción
- Registro de resultados y estadísticas (goles, asistencias)
- Acta de partido (si se gestiona internamente)

---

## 6. Necesidades de Comunicación

### 6.1 Canales Actuales y Sus Limitaciones

**Contexto del problema:**
El 100% de las entidades consultadas usa WhatsApp como canal principal, pero todas reconocen sus limitaciones.

**Problemas documentados:**
- Máximo 1.024 miembros por grupo
- Números de teléfono visibles (problema de privacidad/RGPD)
- Listas de difusión solo llegan a quienes te tienen guardado
- Sobrecarga: "los grupos se saturan con mensajes que opacan información importante"
- Votaciones imposibles de gestionar: "son un infierno" sin conteo automático

**Email:**
- Mejor para comunicaciones formales
- Problemas de direcciones erróneas y rebotes
- No todos lo consultan regularmente

**Redes sociales:**
- No alcanzan a todos los socios
- Confusión entre comunicación interna y externa

### 6.2 Comunicación Masiva Segmentada

**Necesidades identificadas:**

**Canales a soportar:**
- Email (canal principal para comunicaciones formales)
- SMS (para urgencias, coste adicional)
- Notificaciones push (via PWA del portal del socio)
- Tablón de anuncios interno (visible en portal)

**Segmentación de destinatarios:**
- Por tipo de socio
- Por estado de pago (al corriente, morosos)
- Por antigüedad
- Por participación en eventos
- Por etiquetas personalizadas
- Combinaciones (ej: "socios activos mayores de 2 años de antigüedad")

**Plantillas:**
- Plantillas predefinidas para comunicaciones frecuentes
- Personalización con datos del socio (nombre, estado, etc.)
- Guardado de plantillas personalizadas

**Histórico:**
- Registro de todas las comunicaciones enviadas
- Visualización de qué recibió cada socio
- Estadísticas de apertura (para email)

### 6.3 Notificaciones Automáticas

**Necesidades identificadas:**
- Confirmación de alta
- Recordatorio de pago próximo
- Aviso de recibo domiciliado
- Notificación de impago
- Convocatoria de Asamblea
- Confirmación de inscripción a evento
- Recordatorio de evento próximo

### 6.4 Actas y Documentación Oficial

**Contexto del problema:**
El libro de actas es obligatorio (LO 1/2002) y debe reflejar las reuniones de Asamblea General y Junta Directiva. Actualmente se hace en Word con numeración manual.

**Necesidades identificadas:**
- Numeración correlativa automática
- Plantillas por tipo de reunión:
  - Asamblea General Ordinaria
  - Asamblea General Extraordinaria
  - Junta Directiva
- Campos obligatorios: fecha, lugar, asistentes, orden del día, acuerdos
- Registro de asistentes con firma (física o digital)
- Control de quórum
- Firma del secretario con VºBº del presidente
- Archivo histórico consultable
- Exportación a PDF para archivo físico

---

## 7. Necesidades de Gestión Documental

### 7.1 Repositorio Centralizado de Documentos

**Contexto del problema:**
La documentación de las colectividades está dispersa: estatutos en el email del expresidente, facturas en una carpeta del ordenador del tesorero, actas en papel en un cajón. Cuando hay traspaso de junta, se pierde información crítica.

**Necesidades identificadas:**

**Estructura de almacenamiento:**
- Carpetas/categorías predefinidas:
  - Estatutos y reglamentos
  - Actas de reuniones
  - Documentación fiscal (modelos, certificados)
  - Facturas y justificantes
  - Subvenciones (solicitudes, justificaciones)
  - Contratos y convenios
  - Seguros y pólizas
  - Patrimonio (inventarios, valoraciones)
  - Comunicaciones oficiales
- Posibilidad de crear subcarpetas

**Metadatos del documento:**
- Nombre descriptivo
- Categoría
- Fecha del documento
- Fecha de subida
- Usuario que subió
- Etiquetas personalizadas
- Descripción/notas

**Funcionalidades:**
- Subida de documentos (PDF, Word, Excel, imágenes)
- Previsualización sin descarga
- Búsqueda por nombre, categoría, fecha, etiquetas
- Permisos por carpeta (quién puede ver/editar)
- Límite de almacenamiento según plan contratado

**Problema que resuelve:**
- Centralización: toda la documentación en un único lugar accesible
- Continuidad: la información permanece aunque cambien las personas
- Seguridad: control de quién accede a qué

### 7.2 Documentos Avanzados

**Necesidades identificadas para evolución:**
- Control de versiones (histórico de cambios)
- OCR para facturas (extracción automática de datos)
- Búsqueda en contenido del documento (full-text search)
- Etiquetado automático sugerido
- Vinculación con otros módulos (factura → pago, acta → asamblea)

---

## 8. Necesidades de Importación y Exportación

### 8.1 Importación desde Excel/CSV

**Contexto del problema:**
El mayor freno a la adopción de un nuevo sistema es la migración de datos existentes. Si el tesorero tiene que volver a teclear 400 socios, no usará el sistema.

**Necesidades identificadas:**
- Importación de socios desde Excel/CSV
- Mapeo flexible de columnas (el Excel de cada entidad es diferente)
- Validación previa con informe de errores
- Gestión de duplicados (detectar, ignorar, sobrescribir)
- Importación de histórico de pagos (si existe)

**Campos mínimos para importación:**
- Nombre, apellidos
- DNI/NIE (identificador único)
- Email y/o teléfono
- Tipo de socio
- Estado (activo/baja)
- Fecha de alta

### 8.2 Exportación de Datos

**Necesidades identificadas:**

**Listados exportables:**
- Listado de socios (completo o filtrado)
- Informe de cuotas y pagos por ejercicio
- Libro de ingresos y gastos
- Datos para Modelo 182 (donantes con NIF y cantidades)
- Listado de asistentes a evento
- Histórico de comunicaciones

**Formatos:**
- Excel (.xlsx) para trabajo posterior
- CSV para integración con otros sistemas
- PDF para presentación formal (informes)

**Casos de uso:**
- Preparar documentación para Asamblea General
- Entregar listado a banco para remesa SEPA
- Justificar actividades para subvención
- Backup periódico de datos

---

## 9. Necesidades de Visibilidad y Reporting

### 9.1 Dashboard / Panel de Control

**Contexto del problema:**
Para saber el estado actual de la entidad, el tesorero tiene que "cocinar" datos en Excel: contar socios activos, sumar cuotas pendientes, calcular morosidad. Esto lleva tiempo y se hace poco, por lo que la junta opera "a ciegas".

**Necesidades identificadas:**

**Métricas clave (KPIs):**
- Socios activos en el ejercicio actual
- Comparativa con ejercicio anterior (+/- %)
- Altas y bajas del periodo
- Cuotas emitidas vs cobradas (% recaudación)
- Importe pendiente de cobro
- Número de morosos y % sobre total
- Próximos eventos e inscripciones

**Visualizaciones:**
- Gráfico de evolución de socios (últimos ejercicios)
- Gráfico de recaudación mensual
- Distribución por tipo de socio

**Acceso:**
- Dashboard adaptado al rol (tesorero ve finanzas, secretario ve socios)
- Actualización en tiempo real

**Problema que resuelve:**
- Visibilidad inmediata del estado de la entidad
- Detección temprana de problemas (caída de socios, aumento de morosidad)
- Datos listos para informar en reuniones de junta

### 9.2 Informes Predefinidos

**Necesidades identificadas:**
- Informe de socios para Asamblea General
- Informe económico para Asamblea General
- Certificado de estar al corriente de pago
- Certificado de composición de Junta Directiva (para Registro)
- Memoria de actividades (para subvenciones)

---

## 10. Necesidades de Autoservicio del Socio

### 10.1 Portal del Socio (PWA)

**Contexto del problema:**
Los socios consultan constantemente a la junta: "¿estoy al corriente?", "¿cuándo es la próxima actividad?", "¿dónde está el acta de la última asamblea?". Esto genera carga de trabajo y dependencia de la disponibilidad de los directivos.

**Necesidades identificadas:**

**Funcionalidades del portal:**
- Ver sus datos personales (con opción de solicitar modificación)
- Consultar estado de cuotas (pagado/pendiente)
- Descargar recibos/justificantes de pago
- Ver y descargar su carnet digital
- Consultar calendario de eventos
- Inscribirse a eventos (si hay inscripción abierta)
- Recibir y consultar avisos/comunicaciones
- Acceder a documentos públicos (estatutos, actas aprobadas)
- Actualizar preferencias de comunicación
- Gestionar consentimientos RGPD

**Características técnicas:**
- Web responsive (móvil-first)
- PWA instalable (sin necesidad de app nativa)
- Acceso con email + contraseña (o enlace mágico)
- Notificaciones push (previo consentimiento)

**Problema que resuelve:**
- Reduce consultas repetitivas a la junta
- Empodera al socio con acceso a su información
- Moderniza la experiencia de pertenencia

---

## 11. Necesidades de Cumplimiento Normativo

### 11.1 RGPD y Protección de Datos

**Contexto del problema:**
Las asociaciones son responsables del tratamiento de datos personales de sus socios. Muchas desconocen sus obligaciones o las cumplen de forma deficiente.

**Necesidades identificadas:**

**Registro de Actividades de Tratamiento (RAT):**
- Obligatorio para cualquier responsable
- Campos: finalidades, categorías de datos, destinatarios, plazos de conservación
- Plantilla precargada para colectividades

**Gestión de consentimientos:**
- Registro con fecha y texto de cada consentimiento
- Tipos típicos:
  - Comunicaciones de la entidad
  - Comunicaciones comerciales de terceros
  - Uso de imagen en redes sociales/web
  - Cesión a federación (clubes deportivos)
- Posibilidad de revocar con registro de fecha

**Derechos de los interesados:**
- Acceso: exportar todos los datos del socio
- Rectificación: solicitud de modificación
- Supresión: "derecho al olvido" (con limitaciones legales)
- Portabilidad: exportar en formato estructurado
- Plazo de respuesta: 1 mes

**Base jurídica:**
- Ejecución de relación asociativa para gestión de socios
- Consentimiento específico para otros tratamientos

**Particularidad cofradías:**
Las cofradías pueden tratar datos de convicciones religiosas bajo la excepción del artículo 9.2.d del RGPD (entidades con finalidad religiosa), pero solo referidos a miembros actuales/antiguos y sin comunicación externa sin consentimiento.

### 11.2 Ley Orgánica 1/2002 del Derecho de Asociación

**Necesidades de cumplimiento:**

**Libros obligatorios que el sistema debe facilitar:**
- Libro de socios (relación actualizada de asociados)
- Libro de actas (reuniones de Asamblea y Junta)
- Contabilidad (imagen fiel de situación financiera)
- Inventario de bienes

**Órganos obligatorios:**
- Asamblea General: debe reunirse al menos una vez al año
- Junta Directiva: solo asociados mayores de edad

**Comunicaciones al Registro:**
- Plazo máximo 1 mes para alteraciones sustanciales:
  - Cambio de junta directiva
  - Modificación de estatutos
  - Cambio de domicilio

### 11.3 Alertas de Cumplimiento

**Necesidades identificadas:**
- Recordatorio de Asamblea General anual
- Alerta de renovación de junta (según mandato estatutario)
- Aviso de plazo para comunicar cambios al Registro
- Recordatorio de plazos fiscales (modelos, declaraciones)
- Alerta de caducidad de documentos (seguros, certificados)

---

## 12. Necesidades Específicas por Tipo de Colectividad

### 12.1 Cofradías: Patrimonio, Procesiones y Costaleros

| Necesidad | Descripción | Prioridad |
|-----------|-------------|-----------|
| Inventario de patrimonio | Pasos, imágenes, orfebrería, textiles con fotografías y valoración | Alta |
| Gestión de túnicas/enseres | Préstamo con fianza, devolución en plazo, control de estado | Alta |
| Cuadrillas de costaleros | Igualás, posiciones, ensayos, turnos de carga | Alta |
| Papeletas de sitio | Reparto por antigüedad, reserva de insignias, bocinas | Alta |
| Calendario de cultos | Novenas, triduos, besamanos con control de aforo | Media |
| Seguros de procesión | RC, suspensión por lluvia, accidentes de costaleros | Media |

### 12.2 Peñas Festeras: Fiestas, Comidas y Bar

| Necesidad | Descripción | Prioridad |
|-----------|-------------|-----------|
| Programación de fiestas | Calendario de actos, coordinación con Interpeñas | Alta |
| Comidas populares | Reservas, menús, alergias, cobro anticipado | Alta |
| Turnos de voluntarios | Gestión de disponibilidad para barra y servicios | Alta |
| Stock y ventas bar | Control de bebidas, caja durante fiestas, TPV básico | Media |
| Carnet digital | QR para identificación, acceso a descuentos | Media |
| Gestión de carrozas | Presupuesto, materiales, equipos de montaje | Baja |

### 12.3 Clubes Deportivos: Licencias y Competiciones

| Necesidad | Descripción | Prioridad |
|-----------|-------------|-----------|
| Licencias federativas | Tramitación, renovación, documentación requerida | Alta |
| Convocatorias y actas | Control de sanciones, alineaciones, resultados | Alta |
| Gestión de equipos | Categorías, plantillas, histórico deportivo | Alta |
| Reserva de instalaciones | Integración con sistemas municipales si es posible | Media |
| Desplazamientos | Coordinación de transporte, dietas, permisos menores | Media |
| Equipaciones | Inventario, tallas, entrega a deportistas | Baja |

### 12.4 Asociaciones Culturales: Subvenciones y Actividades

| Necesidad | Descripción | Prioridad |
|-----------|-------------|-----------|
| Justificación subvenciones | Memorias, facturas categorizadas, plazos | Alta |
| Gestión de actividades | Inscripciones, materiales, evaluación | Alta |
| Comunicación institucional | Actas, certificados, documentación oficial | Media |
| Control de asistencia | QR o lista para actividades formativas | Media |

---

## 13. Contexto Aragonés: Especificidades a Contemplar

### 13.1 Registro de Asociaciones de Aragón

El Decreto 260/2012 estructura el registro en tres unidades: Zaragoza, Huesca y Teruel. El trámite permite inscripción telemática con DNIe o Cl@ve.

**Documentación para inscripción:**
- Acta fundacional (mínimo 3 personas)
- Estatutos según modelo oficial
- Autorización de uso del domicilio social
- Tasa 23

### 13.2 Fiestas del Ángel de Teruel (La Vaquilla)

Las 21 peñas turolenses con más de 11.000 peñistas generan un impacto económico de 41,7 millones de euros. Estructura con Federación Interpeñas y presidencia rotativa anual.

### 13.3 Semana Santa del Bajo Aragón

La Ruta del Tambor y Bombo (declarada Fiesta de Interés Turístico Internacional) agrupa 9 pueblos con estructuras organizativas basadas en cofradías tradicionales.

### 13.4 Subvenciones Disponibles en Aragón

**Gobierno de Aragón:** 140.000€/año para actividades culturales de asociaciones (1.000-12.000€/beneficiario).

**Diputación de Teruel:** 170.000€ en acción social, 92.000€ para clubes deportivos (máximo 7.000€/beneficiario).

---

## 14. Resumen de Necesidades

### Necesidades Core (Transversales a todas las colectividades)

1. **Multi-tenant con roles**: Espacio aislado por entidad, usuarios con permisos configurables
2. **Registro de socios**: Ficha completa, tipos de socio, estados, campos personalizables
3. **Histórico del socio**: Timeline de eventos, cálculo de antigüedad, trazabilidad
4. **Gestión por ejercicios**: Concepto de temporada/año, cierre y apertura, comparativas
5. **Cuotas y cobros**: Definición, generación, registro de pagos, justificantes
6. **Morosidad**: Workflow automatizado, notificaciones, documentación
7. **Remesas SEPA**: Generación de ficheros, mandatos, devoluciones
8. **Repositorio documental**: Almacenamiento centralizado, categorías, búsqueda
9. **Importación/Exportación**: Migración desde Excel, exportación de informes
10. **Comunicación segmentada**: Email, tablón, segmentos configurables
11. **Dashboard**: Métricas clave, visualizaciones, informes predefinidos
12. **Portal del socio**: Autoservicio, consulta, carnet digital
13. **Cumplimiento RGPD**: Consentimientos, derechos, RAT
14. **Alertas de cumplimiento**: Plazos fiscales, registrales, documentales

### Necesidades por Módulo Específico

15. **Eventos**: Calendario, inscripciones, aforo, check-in
16. **Actas**: Libro de actas, plantillas, numeración, firmas
17. **Caja por turnos**: Arqueos, cierres, control de efectivo (peñas)
18. **Inventario**: Bienes, préstamos, mantenimiento (cofradías, peñas)
19. **Competiciones**: Convocatorias, sanciones, resultados (clubes)
20. **Subvenciones**: Justificación, memorias, categorización (asociaciones)

---
