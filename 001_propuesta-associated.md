# Propuesta de TFM para el Máster en Desarrollo Asistido por IA

**El equilibrio entre arquitectura ejemplar y problema real define el éxito de un TFM.** Tras investigar tendencias de software 2025-2026, nichos desatendidos, y dominios donde DDD aporta valor genuino, hemos seleccionado la siguiente propuesta que combinan relevancia local (Aragón/Teruel), demostración técnica apropiada, y problema validados para público generalista.

Esta propuesta permite demostrar Clean Architecture, DDD y TDD sin sobre-ingeniería, priorizando un monolito modular bien estructurado sobre arquitecturas empresariales innecesarias para un MVP. Estas decisiones se tomarán en su momento oportuno.

---

## Associated: ERP ligero para asociaciones culturales y peñas

Las asociaciones culturales, peñas, cofradías y clubes deportivos pequeños en España gestionan socios, eventos y finanzas con Excel o papel. Las **300.000 asociaciones activas** en España carecen de herramientas adaptadas a sus necesidades específicas: carnets digitales, cobro de cuotas por SEPA, gestión de eventos con asistencia, y contabilidad conforme a la normativa de entidades sin ánimo de lucro.

**Problema que resuelve:** Tesoreros voluntarios dedican horas a tareas administrativas. La morosidad en cuotas es alta por falta de automatización. La transparencia contable es difícil de mantener sin herramientas adecuadas.

**Público objetivo:** Peñas festeras, asociaciones culturales, cofradías (especialmente relevante en Aragón con su rica tradición de fiestas patronales como la Semana Santa de Alcañiz).

**Por qué permite demostrar lo aprendido:**
- **DDD natural:** Bounded contexts claros → Membresía (socios, carnets, altas/bajas), Tesorería (cuotas, pagos, remesas SEPA), Eventos (actividades, inscripciones, asistencia), Comunicación
- **Agregados ricos:** `Socio` como aggregate root con historial de pagos, estado de membresía, participación en eventos
- **Domain Events:** `SocioRegistrado`, `CuotaCobrada`, `PagoRechazado`, `EventoCreado`
- **Reglas de negocio:** Diferentes tipos de socio con cuotas distintas, períodos de gracia, penalizaciones por morosidad

**Nivel de complejidad:** Medio - abordable como MVP con funcionalidad core

**Potencial de originalidad:** Alto - las soluciones existentes (Berrly, Playoff) son genéricas o caras. Un enfoque específico para peñas aragonesas con integración WhatsApp y generación de remesas SEPA sería diferenciador.

**Encaje con el alumno:** Experiencia en ERP personalizado en IPGSoft aplica directamente. Conocimiento del contexto local de Alcañiz y sus fiestas patronales aporta dominio rico.

La clave del éxito será **empezar con 2-3 bounded contexts core**, documentar decisiones arquitectónicas en ADRs, y tener usuarios reales (aunque sean pocos) usando el MVP antes de la entrega. Un proyecto desplegado con 5 usuarios reales impresiona más que arquitectura perfecta sin validación.