# Propuestas de TFM para el Máster en Desarrollo Asistido por IA

**El equilibrio entre arquitectura ejemplar y problema real define el éxito de un TFM.** Tras investigar tendencias de software 2025-2026, nichos desatendidos, y dominios donde DDD aporta valor genuino, presento 7 propuestas que combinan relevancia local (Aragón/Teruel), demostración técnica apropiada, y problemas validados para público generalista.

Cada propuesta permite demostrar Clean Architecture, DDD y TDD sin sobre-ingeniería, priorizando un monolito modular bien estructurado sobre arquitecturas empresariales innecesarias para un MVP.

---

## 1. GestorPeñas: ERP ligero para asociaciones culturales y peñas

Las asociaciones culturales, peñas, cofradías y clubes deportivos pequeños en España gestionan socios, eventos y finanzas con Excel o papel. Los **900.000+ asociaciones** registradas en España carecen de herramientas adaptadas a sus necesidades específicas: carnets digitales, cobro de cuotas por SEPA, gestión de eventos con asistencia, y contabilidad conforme a la normativa de entidades sin ánimo de lucro.

**Problema que resuelve:** Tesoreros voluntarios dedican horas a tareas administrativas. La morosidad en cuotas es alta por falta de automatización. La transparencia contable es difícil de mantener sin herramientas adecuadas.

**Público objetivo:** Peñas festeras, asociaciones culturales, cofradías (especialmente relevante en Aragón con su rica tradición de fiestas patronales como la Semana Santa de Alcañiz).

**Por qué permite demostrar lo aprendido:**
- **DDD natural:** Bounded contexts claros → Membresía (socios, carnets, altas/bajas), Tesorería (cuotas, pagos, remesas SEPA), Eventos (actividades, inscripciones, asistencia), Comunicación
- **Agregados ricos:** `Socio` como aggregate root con historial de pagos, estado de membresía, participación en eventos
- **Domain Events:** `SocioRegistrado`, `CuotaCobrada`, `PagoRechazado`, `EventoCreado`
- **Reglas de negocio:** Diferentes tipos de socio con cuotas distintas, períodos de gracia, penalizaciones por morosidad

**Nivel de complejidad:** Medio — abordable como MVP con funcionalidad core

**Potencial de originalidad:** Alto — las soluciones existentes (Berrly, Playoff) son genéricas o caras. Un enfoque específico para peñas aragonesas con integración WhatsApp y generación de remesas SEPA sería diferenciador.

**Encaje con el alumno:** Experiencia en ERP personalizado en IPGSoft aplica directamente. Conocimiento del contexto local de Alcañiz y sus fiestas patronales aporta dominio rico.

---

## 2. AutonomoClaro: Gestión fiscal simplificada para autónomos españoles

España tiene **3,3 millones de autónomos** que luchan con obligaciones fiscales complejas: Modelo 303, 130, categorización de gastos deducibles, y cálculo de IRPF. La mayoría usa herramientas básicas y paga a gestorías por tareas automatizables. El **75% de autónomos** considera la carga administrativa su mayor frustración.

**Problema que resuelve:** Autónomos pierden tiempo y dinero en gestión fiscal. Las herramientas existentes (Holded, Quipu) están diseñadas para empresas más grandes o no automatizan suficientemente la categorización de gastos.

**Público objetivo:** Autónomos españoles, especialmente freelancers, profesionales independientes y pequeños comerciantes.

**Por qué permite demostrar lo aprendido:**
- **DDD excelente:** Bounded contexts → Facturación (facturas emitidas, numeración, clientes), Gastos (tickets, categorización, deducibilidad), Impuestos (cálculo trimestral, modelos, presentación), Tesorería (cobros, pagos, flujo de caja)
- **Reglas de negocio complejas:** Cálculo de IVA según tipo de actividad, gastos deducibles según uso profesional, retenciones IRPF
- **TDD crítico:** La precisión en cálculos fiscales es fundamental — test suite exhaustivo demuestra madurez técnica
- **Event Sourcing opcional:** Registro de auditoría natural para movimientos fiscales

**Nivel de complejidad:** Medio-Alto — el dominio fiscal español tiene complejidad real que justifica DDD

**Potencial de originalidad:** Medio-Alto — enfoque en categorización automática con IA de tickets fotográficos y pre-rellenado de modelos trimestrales sería diferenciador.

**Encaje con el alumno:** Experiencia con soluciones ERP y conocimiento de flujos contables. Posible interés personal si trabaja como autónomo o conoce autónomos locales.

---

## 3. TrazaOrigen: Trazabilidad simplificada para bodegas y almazaras pequeñas

Las pequeñas bodegas y almazaras familiares de Aragón deben cumplir estrictos requisitos de trazabilidad (SILICIE, libros de bodega, informes a Consejos Reguladores) pero solo el **53% de almazaras** ha aprovechado programas de digitalización. Usan Excel o papel, arriesgando sanciones y perdiendo certificaciones.

**Problema que resuelve:** Productores pequeños no pueden permitirse ERPs vitivinícolas empresariales (€5.000-15.000/año) pero necesitan cumplir normativa. La trazabilidad desde materia prima hasta botella/garrafón es obligatoria.

**Público objetivo:** Bodegas familiares (1-5 empleados), almazaras cooperativas pequeñas, productores con DO (Aceite del Bajo Aragón, Cariñena, Calatayud).

**Por qué permite demostrar lo aprendido:**
- **DDD con bounded contexts productivos:** Recepción (partidas de uva/aceituna, proveedores, calidades), Producción (depósitos, procesos, trasiegas), Almacén (lotes, envasado, etiquetado), Ventas (salidas, trazabilidad inversa), Normativo (libros SILICIE, informes DO)
- **Agregados con invariantes fuertes:** `Lote` que garantiza trazabilidad completa, `Depósito` con capacidad y contenido consistentes
- **Domain Events trazables:** `PartidaRecibida`, `TrasiegaRealizada`, `LoteEnvasado`, `VentaRegistrada`
- **Value Objects ricos:** `Calidad`, `Variedad`, `OrigenParcela`

**Nivel de complejidad:** Alto — dominio técnico rico con normativa real

**Potencial de originalidad:** Muy Alto — nicho específico con clara necesidad no cubierta por soluciones asequibles

**Encaje con el alumno:** Relevancia geográfica directa (Bajo Aragón es zona de producción). Posibilidad de validar con productores locales reales. Demuestra capacidad de entender dominios complejos.

---

## 4. CuidaAbuelo: Companion digital para mayores y sus familias

El **71% de adultos mayores de 50 años** desea soporte tecnológico adaptado a sus necesidades. España envejece rápidamente y 53+ millones de europeos son cuidadores no remunerados. Las apps existentes son complejas; los mayores necesitan interfaces extremadamente simplificadas mientras las familias necesitan tranquilidad.

**Problema que resuelve:** Mayores aislados por la complejidad tecnológica. Familias preocupadas sin forma de monitorizar bienestar a distancia. Recordatorios de medicación olvidados con consecuencias graves.

**Público objetivo:** Adultos 65+ que viven solos o con autonomía limitada, y sus familias cuidadoras (hijos adultos, especialmente los que viven lejos).

**Por qué permite demostrar lo aprendido:**
- **DDD centrado en bienestar:** Bounded contexts → Usuario Mayor (perfil simplificado, preferencias), Medicación (recordatorios, confirmaciones, historial), Comunicación (videollamadas simplificadas, mensajes de voz), Familia (dashboard, alertas, notas compartidas), Actividad (patrones de uso, anomalías)
- **Domain Events significativos:** `MedicacionTomada`, `MedicacionOmitida`, `LlamadaRealizada`, `PatronAnomaloDetectado`
- **Reglas de negocio humanas:** Escalado de alertas si medicación no confirmada, detección de cambios en patrones de actividad
- **Clean Architecture ejemplar:** Lógica de detección de anomalías aislada de interfaz, permite cambiar de ML a reglas simples

**Nivel de complejidad:** Medio — el reto es la simplicidad de UX, no la complejidad técnica

**Potencial de originalidad:** Alto — combinar simplificación extrema con dashboard familiar inteligente es diferenciador

**Encaje con el alumno:** Flutter permite crear interfaces nativas excelentes. Experiencia con n8n puede automatizar alertas y notificaciones. Impacto social significativo.

---

## 5. EquipoFácil: Coordinación ultraligera para grupos amateur

Equipos deportivos amateur, bandas de música, grupos de senderismo y clubs de lectura usan WhatsApp caóticamente para coordinar. Las apps existentes (TeamSnap, Spond) tienen demasiadas funciones y planes de pago elevados para grupos casuales que solo necesitan saber "¿quién viene el sábado?".

**Problema que resuelve:** Coordinadores de grupos casuales pierden tiempo persiguiendo confirmaciones. Los mensajes importantes se pierden en chats grupales. No hay histórico de asistencia ni rotación justa de responsabilidades.

**Público objetivo:** Entrenadores de fútbol base, coordinadores de grupos de senderismo, directores de bandas amateur, organizadores de quedadas periódicas (partidas de rol, clubs de lectura).

**Por qué permite demostrar lo aprendido:**
- **DDD con scope contenido:** Bounded contexts → Grupo (miembros, roles, configuración), Eventos (actividades, RSVPs, asistencia), Comunicación (anuncios, recordatorios), Rotaciones (responsabilidades turnadas)
- **Agregados enfocados:** `Evento` como aggregate root con lista de confirmaciones y estado
- **Value Objects prácticos:** `Confirmacion` (sí/no/quizás con comentario), `Rol` (jugador, suplente, entrenador)
- **Domain Events simples:** `EventoCreado`, `ConfirmacionRecibida`, `RecordatorioEnviado`

**Nivel de complejidad:** Bajo-Medio — ideal para MVP pulido

**Potencial de originalidad:** Medio — la diferenciación está en la simplicidad extrema y gratuidad para funciones básicas

**Encaje con el alumno:** Probablemente participe en grupos así (hackathons KAIZEN, equipos). Puede pilotar con grupos reales. Flutter ideal para app móvil nativa.

---

## 6. ApadrinaProducto: Plataforma de venta directa con apadrinamiento

Iniciativas como "Apadrina un Olivo" de Oliete (Teruel) demuestran que consumidores valoran conexión directa con productores locales. Sin embargo, crear estas experiencias requiere desarrollo custom costoso. Pequeños productores de aceite, vino, miel o jamón no pueden replicar este modelo.

**Problema que resuelve:** Productores locales dependen de intermediarios que reducen márgenes. Consumidores quieren productos con historia y trazabilidad pero no encuentran canales directos. El modelo de suscripción/apadrinamiento fideliza pero es complejo de gestionar.

**Público objetivo:** Pequeños productores agroalimentarios (aceite, vino, miel, embutidos, queso), consumidores urbanos interesados en productos de origen.

**Por qué permite demostrar lo aprendido:**
- **DDD con múltiples bounded contexts:** Productores (perfil, historia, productos), Catálogo (productos, temporadas, disponibilidad), Suscripciones (apadrinamientos, entregas periódicas, regalos), Pedidos (compras puntuales, logística), Comunidad (actualizaciones del productor, fotos de temporada)
- **Agregados con ciclo de vida interesante:** `Apadrinamiento` con renovaciones, pausas, regalos
- **Domain Events narrativos:** `ApadrinamientoCreado`, `ActualizacionTemporadaPublicada`, `EntregaProgramada`, `RegaloEnviado`
- **Reglas de negocio del mundo real:** Disponibilidad según cosecha, precios de temporada, logística de productos perecederos

**Nivel de complejidad:** Medio-Alto — marketplace con suscripciones añade complejidad

**Potencial de originalidad:** Muy Alto — plataforma white-label para que cualquier productor pueda tener su propio "Apadrina un X"

**Encaje con el alumno:** Conexión directa con Teruel y sus productos (aceite Bajo Aragón, jamón, melocotón de Calanda). Validación con productores locales reales posible.

---

## 7. MultiServi: TPV unificado para negocios rurales multiservicio

En pueblos pequeños, un único establecimiento combina bar, tienda, punto de pan, recogida de farmacia y paquetería. Estos **negocios multiservicio** son el último recurso comercial de muchos municipios pero gestionan actividades heterogéneas con herramientas fragmentadas o ninguna.

**Problema que resuelve:** Propietarios de multiservicio necesitan gestionar inventarios mixtos (bebidas, alimentación, productos varios), actividades con diferente IVA, y servicios de punto de recogida. Las soluciones TPV estándar no contemplan esta heterogeneidad.

**Público objetivo:** Negocios multiservicio rurales, emprendedores que toman el relevo de comercios en pueblos pequeños.

**Por qué permite demostrar lo aprendido:**
- **DDD pragmático:** Bounded contexts → Ventas (TPV, tickets, caja), Inventario (productos heterogéneos, proveedores diversos), Servicios (puntos de recogida, encargos), Contabilidad (actividades con diferente IVA)
- **Complejidad justificada:** El modelo de dominio debe manejar productos con diferentes tratamientos fiscales, servicios sin stock, comisiones de intermediación
- **Value Objects específicos:** `CategoriaFiscal`, `TipoServicio`, `OrigenProducto`
- **Testing crítico:** Cálculos de IVA mixto, cierre de caja, conciliación

**Nivel de complejidad:** Medio — scope controlable con funcionalidad core

**Potencial de originalidad:** Alto — nicho muy específico sin competencia directa

**Encaje con el alumno:** Conocimiento directo del contexto rural aragonés. Puede validar con negocios reales en la comarca de Alcañiz. Experiencia ERP directamente aplicable.

---

## Comparativa de propuestas

| Propuesta | Complejidad | Originalidad | Demostración DDD | Validación local | Atractivo |
|-----------|-------------|--------------|------------------|------------------|-----------|
| **GestorPeñas** | Media | Alta | Excelente | Muy Alta | Alto |
| **AutonomoClaro** | Media-Alta | Media-Alta | Excelente | Media | Alto |
| **TrazaOrigen** | Alta | Muy Alta | Excelente | Muy Alta | Medio-Alto |
| **CuidaAbuelo** | Media | Alta | Buena | Media | Muy Alto |
| **EquipoFácil** | Baja-Media | Media | Buena | Alta | Alto |
| **ApadrinaProducto** | Media-Alta | Muy Alta | Excelente | Muy Alta | Muy Alto |
| **MultiServi** | Media | Alta | Buena | Muy Alta | Medio |

---

## Recomendación final

Para maximizar el equilibrio entre demostración técnica, originalidad y viabilidad de MVP, recomiendo priorizar:

1. **GestorPeñas** — Mejor ratio complejidad/demostración. Bounded contexts claros, reglas de negocio reales, validación inmediata con peñas locales. Puede pilotar con la propia peña del alumno o conocidos.

2. **ApadrinaProducto** — Mayor potencial de impacto y diferenciación. Conecta con la identidad aragonesa, tiene modelo de negocio claro, y permite demostrar arquitectura sofisticada justificadamente.

3. **TrazaOrigen** — Si se busca profundidad técnica y dominio complejo, este proyecto demuestra capacidad de modelar regulación real con DDD. Ideal para demostrar madurez técnica.

La clave del éxito será **empezar con 2-3 bounded contexts core**, documentar decisiones arquitectónicas en ADRs, y tener usuarios reales (aunque sean pocos) usando el MVP antes de la entrega. Un proyecto desplegado con 5 usuarios reales impresiona más que arquitectura perfecta sin validación.