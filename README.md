# Associated — Metodología de expansión documental

## 1. Propósito del repositorio

Este repositorio reúne la base documental del TFM **Associated**, un ERP ligero orientado a asociaciones culturales, cofradías, clubes deportivos y peñas festeras españolas. Su objetivo no es solo describir una idea de producto, sino mostrar de forma trazable cómo se pasó desde una **propuesta inicial de alto nivel** y un **análisis de necesidades** hacia un conjunto de especificaciones funcionales, de dominio, arquitectónicas, tecnológicas y operativas que conforman un corpus documental completo para el alcance definido del proyecto.

En términos metodológicos, el repositorio documenta un proceso de expansión progresivo: problema de negocio → necesidades → requisitos → modelo de dominio → decisiones de arquitectura → stack → concreción técnica → historias de usuario → casos de uso → estrategia de implementación del MVP.

---

## 2. Punto de partida documental

La construcción de la especificación parte de dos documentos base:

- [001_propuesta-associated.md](001_propuesta-associated.md): formula el problema, el público objetivo, el valor del proyecto para el TFM y una primera intuición de bounded contexts y eventos de dominio.
- [002_analisis-necesidades.md](002_analisis-necesidades.md): amplía la propuesta mediante un análisis exhaustivo de necesidades reales de colectividades españolas, organizado en secciones temáticas y orientado a detectar pain points, obligaciones y capacidades necesarias.

A partir de esos dos insumos, el repositorio desarrolla una cadena de artefactos en la que cada documento declara explícitamente sus **inputs** y su **estado**, siguiendo una lógica incremental de derivación.

---

## 3. Metodología seguida: expansión paso a paso

### Paso 0. Delimitación del problema y tesis del proyecto

La propuesta inicial fija el marco conceptual del TFM:

- dominio: gestión de colectividades españolas;
- problema: alta carga administrativa sobre perfiles voluntarios;
- oportunidad: ausencia de soluciones asequibles y adaptadas;
- enfoque académico: demostrar DDD, Clean Architecture y decisiones arquitectónicas justificadas, evitando sobreingeniería.

Este documento funciona como **marco de intención**: todavía no define especificaciones, pero sí orienta el tipo de solución y anticipa conceptos de dominio relevantes.

### Paso 1. Levantamiento y estructuración de necesidades

El análisis de necesidades transforma la idea inicial en un mapa más concreto de problemas y expectativas. Su función metodológica es doble:

1. **capturar lenguaje del dominio** desde casos reales o plausibles del contexto español;
2. **organizar las necesidades por áreas funcionales** para permitir su posterior conversión a requisitos.

De este modo, el documento 002 actúa como base primaria para derivar requisitos funcionales y también como soporte para RNFs, porque no solo identifica funcionalidades, sino restricciones operativas, legales y de usabilidad.

### Paso 2. Formalización en requisitos funcionales (RF)

El documento [003_requisitos-funcionales.md](003_requisitos-funcionales.md) deriva de KB-002 y convierte las necesidades en **221 requisitos funcionales** identificados con nomenclatura `NxRFyy`.

Metodológicamente, este paso implica:

- segmentar el dominio en secciones N2 a N13;
- expresar cada capacidad como requisito verificable;
- mantener la semántica del problema original, pero con mayor precisión operativa.

Aquí aparece la primera gran estructura de trazabilidad: las necesidades dejan de ser solo narrativas y pasan a ser **un inventario formal de comportamiento esperado**.

### Paso 3. Derivación de requisitos no funcionales base (RNF)

El documento [004_rnf-base.md](004_rnf-base.md) toma como inputs el análisis de necesidades y los requisitos funcionales para identificar **66 requisitos no funcionales agnósticos de tecnología**.

La lógica seguida es separar:

- **qué debe hacer** el sistema → RF;
- **cómo debe comportarse** el sistema en términos de seguridad, rendimiento, disponibilidad, RGPD, usabilidad y mantenibilidad → RNF.

Este nivel sigue siendo intencionalmente tecnológico-agnóstico. Todavía no se seleccionan herramientas concretas; se fijan restricciones y metas de calidad.

### Paso 4. Modelado del dominio y bounded contexts

El documento [005_modelo-dominio.md](005_modelo-dominio.md) integra la propuesta, el análisis de necesidades, los RF y los RNF base para construir el modelo de dominio.

La metodología de expansión en este paso consiste en:

- identificar subdominios;
- delimitar bounded contexts;
- definir aggregates, entities y relaciones entre contextos;
- explicitar domain events e interacciones;
- alinear cada bloque del dominio con las secciones funcionales del catálogo RF.

El resultado es una arquitectura conceptual del negocio organizada en seis bounded contexts principales:

- `BC-Identidad`
- `BC-Membresia`
- `BC-Tesoreria`
- `BC-Eventos`
- `BC-Comunicacion`
- `BC-Documentos`

Además, los requisitos N8 a N11 se tratan como capacidades **transversales** implementadas sobre los contextos existentes, no como bounded contexts dedicados.

### Paso 5. Toma de decisiones de arquitectura mediante ADRs

El documento [006_adrs.md](006_adrs.md) toma como base los RNF y el modelo de dominio para registrar **12 Architectural Decision Records**.

Este paso convierte necesidades y restricciones en decisiones explícitas, justificadas y trazables, por ejemplo:

- monolito modular;
- multi-tenant por base de datos separada;
- módulos por bounded context;
- domain events in-process;
- persistencia relacional con PostgreSQL;
- JWT + refresh tokens;
- RBAC granular;
- outbox pattern;
- Clean Architecture por módulo;
- API REST + OpenAPI.

La metodología aquí no consiste en “elegir tecnología porque sí”, sino en **documentar por qué una decisión arquitectónica responde a RNFs y al modelo del dominio**.

### Paso 6. Selección del stack tecnológico

El documento [007_stack.md](007_stack.md) deriva de los RNF base y de los ADRs. Su función es materializar las decisiones arquitectónicas en un stack concreto.

La secuencia aplicada es deliberada:

1. primero se establecen restricciones y decisiones;
2. después se eligen tecnologías coherentes con esas decisiones.

Así, el stack no nace como punto de partida, sino como consecuencia de artefactos previos. El resultado documentado incluye, entre otros, TypeScript, NestJS, React, PostgreSQL, Prisma, MinIO/S3, Vitest, Playwright, GitHub Actions y Sentry.

### Paso 7. Concreción de RNF en requisitos técnicos (RNFT)

El documento [008_rnf-tecnicos.md](008_rnf-tecnicos.md) conecta los RNF base con el stack seleccionado y traduce los objetivos de calidad en mecanismos técnicos verificables.

Este paso introduce una segunda capa de refinamiento no funcional:

- RNF-agnóstico → intención de calidad;
- RNFT → implementación técnica, configuración, métricas y verificación.

Por eso el documento incluye una matriz RNF → RNFT → tecnología, consolidando la trazabilidad entre calidad deseada y medios técnicos concretos.

### Paso 8. Derivación de User Stories y criterios de aceptación

El documento [009_user-stories.md](009_user-stories.md) toma como inputs los requisitos funcionales, el modelo de dominio y los RNF técnicos para generar **202 User Stories** con criterios de aceptación.

La metodología observada en este documento consiste en:

- reorganizar el alcance funcional por bounded context y por áreas transversales;
- expresar necesidades desde la perspectiva de actores y valor de negocio;
- incorporar criterios de aceptación en formato verificable;
- priorizar con MoSCoW.

Este paso hace de puente entre especificación analítica y planificación de producto, porque acerca el catálogo RF al lenguaje de entrega incremental.

### Paso 9. Agrupación cohesiva en casos de uso y application services

El documento [010_casos-uso.md](010_casos-uso.md) deriva del modelo de dominio y de las User Stories. Su lógica metodológica es **agrupar historias en casos de uso cohesivos** usando criterios explícitos:

1. cohesión funcional;
2. transaccionalidad;
3. mismo application service;
4. mismo aggregate.

Cada caso de uso incorpora, según el material relevado:

- metadatos;
- User Stories agrupadas;
- bounded context involucrado;
- application service principal;
- aggregates participantes;
- prioridad y complejidad;
- flujos normales, alternativos y de excepción;
- domain events;
- interacciones entre bounded contexts.

En esta etapa el repositorio ya no describe solo “qué necesita el sistema”, sino **cómo se ejecuta operativamente cada capacidad relevante**.

### Paso 10. Incorporación de la estrategia del MVP

El repositorio ya incorpora [011_mvp-strategy.md](011_mvp-strategy.md) como artefacto de cierre del ciclo documental principal. Este documento sintetiza KB-003 a KB-010 para seleccionar el subconjunto de casos de uso priorizados, delimitar los bounded contexts incluidos en el MVP y ordenar la implementación en fases con dependencias explícitas.

---

## 4. Reglas de trazabilidad y nomenclatura

El repositorio sigue una convención explícita de identificadores:

| Tipo de artefacto | Formato | Significado |
|---|---|---|
| Requisito funcional | `NxRFyy` | Sección de necesidades + número secuencial |
| Requisito no funcional | `RNF-XXX` | Requisito de calidad agnóstico |
| Requisito no funcional técnico | `RNFT-XXX` | Concreción tecnológica de un RNF |
| Bounded Context | `BC-Nombre` | Contexto delimitado del dominio |
| ADR | `ADR-XXX` | Decisión arquitectónica registrada |
| User Story | `US-XXX` | Historia de usuario secuencial |
| Caso de uso | `UC-XXX` | Caso de uso secuencial |
| Documento base | `KB-XXX` | Entrada del índice de knowledge base |

### Reglas metodológicas observadas

1. Cada documento declara sus **inputs** en la cabecera.
2. Cada documento indica su **estado** y versión.
3. La generación sigue un **orden estricto**, evitando producir artefactos avanzados sin sus predecesores.
4. Las secciones de trazabilidad conectan artefactos entre sí, no solo dentro del mismo documento.
5. La nomenclatura se utiliza como soporte de auditoría documental y no solo como convención estética.

### Estructura general de documentación

De acuerdo con [AGENTS.md](AGENTS.md), los documentos siguen una plantilla común basada en:

- título;
- proyecto;
- versión;
- fecha;
- inputs;
- estado;
- contenido principal;
- sección de trazabilidad;
- changelog.

Esto refuerza el carácter acumulativo y verificable del repositorio.

---

## 5. Orden de generación de artefactos

La secuencia metodológica consolidada en el repositorio es la siguiente:

1. **KB-001** Propuesta TFM
2. **KB-002** Análisis de necesidades
3. **KB-003** Requisitos funcionales
4. **KB-004** RNF base
5. **KB-005** Modelo de dominio y bounded contexts
6. **KB-006** ADRs
7. **KB-007** Stack tecnológico
8. **KB-008** RNF técnicos
9. **KB-009** User Stories y criterios de aceptación
10. **KB-010** Casos de uso / Application Services
11. **KB-011** Estrategia de implementación del MVP

### Relación inputs → outputs

| Paso | Inputs principales | Output |
|---|---|---|
| Necesidades → RF | KB-002 | KB-003 |
| Necesidades + RF → RNF | KB-002, KB-003 | KB-004 |
| Propuesta + necesidades + RF + RNF | KB-001, KB-002, KB-003, KB-004 | KB-005 |
| RNF + dominio | KB-004, KB-005 | KB-006 |
| RNF + ADR | KB-004, KB-006 | KB-007 |
| RNF + stack | KB-004, KB-007 | KB-008 |
| RF + dominio + RNFT | KB-003, KB-005, KB-008 | KB-009 |
| Dominio + US | KB-005, KB-009 | KB-010 |
| Todo el corpus analítico | KB-003 a KB-010 | KB-011 |

---

## 6. Decisiones de alcance documental

### Alcance incluido

El scope documental principal comprende **N2 a N11**, equivalentes a **202 RFs**. Según el índice maestro, estas secciones representan el núcleo funcional y transversal del sistema:

- N2: Arquitectura y acceso
- N3: Socios y miembros
- N4: Tesorería y finanzas
- N5: Eventos
- N6: Comunicación
- N7: Gestión documental
- N8: Importación y exportación
- N9: Reporting
- N10: Portal del socio
- N11: Cumplimiento normativo

### Alcance excluido

Se excluyen del scope documental principal las secciones **N12** y **N13**, que suman **19 RFs**:

- N12: funcionalidades específicas por tipo de colectividad;
- N13: especificidades del contexto aragonés.

### Justificación metodológica de la exclusión

La exclusión no implica irrelevancia, sino priorización del alcance del TFM. En [AGENTS.md](AGENTS.md) se justifican cuatro motivos:

1. complejidad de análisis desproporcionada para el objetivo académico;
2. extensibilidad del modelo garantizada por la arquitectura modular y por bounded contexts;
3. necesidad de mantener un scope demostrable y controlable dentro del TFM;
4. delimitación explícita del corpus principal sobre N2-N11 como base suficiente y autocontenida para el TFM.

---

## 7. Relación entre RF, RNF, BC, ADR, RNFT, US y UC

La metodología del repositorio puede resumirse como una cadena de transformación y refinamiento:

```text
Problema y propuesta
    ↓
Necesidades de alto nivel
    ↓
RF (qué hace el sistema)
    ↓
RNF (cómo debe comportarse)
    ↓
BC / Modelo de dominio (cómo se divide el negocio)
    ↓
ADR (decisiones estructurales)
    ↓
Stack (tecnologías coherentes con ADR y RNF)
    ↓
RNFT (concreción técnica verificable)
    ↓
US (valor de negocio en lenguaje de producto)
    ↓
UC (ejecución operativa y application services)
    ↓
MVP (selección priorizada de implementación)
```

### Papel de cada artefacto

- **RF**: formalizan capacidades funcionales a partir del análisis de necesidades.
- **RNF**: fijan restricciones y atributos de calidad del sistema.
- **BC**: estructuran el dominio y delimitan responsabilidades.
- **ADR**: registran las decisiones de arquitectura derivadas del dominio y los RNF.
- **RNFT**: traducen los RNF a mecanismos técnicos medibles sobre el stack.
- **US**: expresan valor de negocio y prioridad de entrega.
- **UC**: agrupan historias en operaciones ejecutables, normalmente asociadas a application services.

### Relaciones de trazabilidad más relevantes

- **KB-002 → KB-003**: necesidades a requisitos funcionales.
- **KB-002 + KB-003 → KB-004**: necesidades y capacidades a restricciones de calidad.
- **KB-001 + KB-002 + KB-003 + KB-004 → KB-005**: problema, necesidades, requisitos y calidad a modelo de dominio.
- **KB-004 + KB-005 → KB-006**: calidad y dominio a decisiones arquitectónicas.
- **KB-004 + KB-006 → KB-007**: decisiones y restricciones a selección tecnológica.
- **KB-004 + KB-007 → KB-008**: requisitos de calidad a implementación técnica.
- **KB-003 + KB-005 + KB-008 → KB-009**: requisitos, dominio y restricciones técnicas a historias de usuario.
- **KB-005 + KB-009 → KB-010**: dominio e historias a casos de uso y servicios de aplicación.

---

## 8. Estado actual de la documentación

De acuerdo con el estado actual del repositorio, la base documental principal se encuentra completada y cerrada para el alcance definido:

- documentos generados: **9 de 9**;
- último bloque incorporado: **estrategia de implementación del MVP**;
- corpus documental disponible: **KB-003 a KB-011**;
- cobertura documental estructurada: **KB-003 a KB-011 revisados y consistentes entre sí**.

### Situación actual resumida

| Documento | Estado observado |
|---|---|
| KB-001 Propuesta | Base disponible |
| KB-002 Análisis de necesidades | Base disponible |
| KB-003 RF | Revisado |
| KB-004 RNF base | Revisado |
| KB-005 Modelo de dominio | Revisado |
| KB-006 ADRs | Revisado |
| KB-007 Stack | Revisado |
| KB-008 RNF técnicos | Revisado |
| KB-009 User Stories | Revisado |
| KB-010 Casos de uso | Revisado |
| KB-011 Estrategia MVP | Revisado |

---

## 9. Hallazgos relevantes al revisar el repositorio

Durante la reconstrucción metodológica se observa un patrón claro y consistente: **cada artefacto nace como refinamiento controlado del anterior**, no como documento aislado.

El conjunto KB-003 a KB-010 aparece articulado como un recorrido documental completo: requisitos funcionales y no funcionales, modelo de dominio, decisiones arquitectónicas, stack, concreción técnica, historias de usuario y casos de uso mantienen una secuencia metodológica continua y explícita.

La incorporación de [011_mvp-strategy.md](011_mvp-strategy.md) cierra ese recorrido con una priorización de implementación apoyada en los artefactos previos, sin abrir nuevas fases documentales dentro del alcance principal.

---

## 10. Cierre del corpus documental

Tras la incorporación de [011_mvp-strategy.md](011_mvp-strategy.md), el repositorio presenta un corpus documental metodológicamente cerrado para el alcance definido del TFM:

- KB-003 a KB-010 cubren el espectro analítico, funcional, arquitectónico, técnico y operativo del sistema;
- KB-011 sintetiza ese corpus en una estrategia de implementación del MVP;
- la trazabilidad entre necesidades, requisitos, bounded contexts, decisiones, historias y casos de uso queda expresada de forma explícita en los documentos del repositorio.

En otras palabras, el repositorio ya completó la **expansión analítica, de diseño y de priorización del MVP** dentro del alcance documental establecido.
