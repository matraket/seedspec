# Estrategia de Implementacion del MVP -- Associated

| Campo     | Valor                                    |
|-----------|------------------------------------------|
| Version   | 1.0                                      |
| Fecha     | 2026-03-21                               |
| Estado    | Verificado                               |
| Audiencia | Direccion Tecnica / Product Owner        |

> Para el detalle tecnico de la implementacion, consultar `doc/mvp-strategy.md`.

---

## 1. Resumen Ejecutivo

El MVP de Associated cubre 20 de los 77 casos de uso especificados (26%), distribuidos en 3 fases de entrega mas una fase previa de infraestructura. La seleccion prioriza las tres areas funcionales que hacen operativo el sistema --identidad, gestion de socios y tesoreria--, dejando fuera deliberadamente eventos, comunicaciones y documentos. Esta estrategia permite entregar un producto funcional con el minimo alcance necesario para que una colectividad gestione su operativa diaria: dar de alta socios, cobrar cuotas y controlar su economia.

---

## 2. Criterios de Seleccion del Alcance

La seleccion del MVP responde a una pregunta concreta: **cual es el minimo necesario para que una colectividad pueda operar?**

**Criterio principal: operatividad core.** Sin gestion de identidad no hay acceso al sistema. Sin gestion de socios no hay a quien gestionar. Sin tesoreria no hay sostenibilidad economica. Estos tres pilares son indivisibles -- quitar cualquiera de ellos hace el sistema inutilizable.

**Concentracion en lo esencial.** Los 20 UCs seleccionados cubren entre el 60% y el 80% de cada area funcional core (Identity, Membership, Treasury), mientras que los tres areas excluidas (Events, Communication, Documents) quedan al 0%. No es un recorte arbitrario: es una decision de producto. Las colectividades pueden gestionar eventos con hojas de calculo, enviar comunicaciones por email directo y manejar documentos en carpetas compartidas. Lo que NO pueden hacer sin software es gestionar cobros recurrentes, generar remesas bancarias o controlar el estado de 500 socios.

**Composicion:** 19 casos de uso con prioridad Must (obligatorios segun especificacion) mas 1 Should (UC-065, graficos de evolucion). Esta unica excepcion se justifica porque complementa el dashboard principal y cierra la propuesta de valor analitica del MVP.

**Los 23 Must excluidos** se concentran en tres categorias: areas funcionales completas no-core (Events, Communication, Documents suman 15 UCs), funcionalidades transversales no bloqueantes (portal del socio, exportacion, cumplimiento normativo) y funcionalidades avanzadas dentro de areas core que pueden incorporarse despues (workflow de morosidad, registro contable, carnets).

---

## 3. Alcance del MVP

### Areas funcionales incluidas vs excluidas

| Area funcional   | Tipo       | UCs en MVP | UCs totales | Cobertura | Estado       |
|------------------|------------|------------|-------------|-----------|--------------|
| Identity         | Soporte    | 4          | 5           | 80%       | INCLUIDA     |
| Membership       | Core       | 6          | 10          | 60%       | INCLUIDA     |
| Treasury         | Core       | 7          | 11          | 64%       | INCLUIDA     |
| Transversal      | Transversal| 3          | 21          | 14%       | PARCIAL      |
| Events           | Core       | 0          | 11          | 0%        | EXCLUIDA     |
| Communication    | Soporte    | 0          | 9           | 0%        | EXCLUIDA     |
| Documents        | Soporte    | 0          | 8           | 0%        | EXCLUIDA     |

### Mapa del sistema

```
+-----------------------------------------------------------------------+
|                         ASSOCIATED - ERP                              |
|                                                                       |
|   DENTRO DEL MVP                    |   FUERA DEL MVP                 |
|   ................................  |   ............................  |
|   :                              :  |   :                          :  |
|   :  [Identity]    [Membership]  :  |   :  [Events]                :  |
|   :       \            |         :  |   :  [Communication]         :  |
|   :        \           |         :  |   :  [Documents]             :  |
|   :         +----+-----+         :  |   :                          :  |
|   :              |               :  |   :..........................:  |
|   :         [Treasury]           :  |                                 |
|   :              |               :  |                                 |
|   :     [Transversal parcial]    :  |                                 |
|   :..............................:  |                                 |
+-----------------------------------------------------------------------+
```

### Cifras clave

| Metrica                     | Valor                         |
|-----------------------------|-------------------------------|
| Casos de uso                | 20 de 77 (26%)                |
| Tareas de implementacion    | 38 (20 backend + 18 frontend) |
| User stories cubiertas      | ~56 de 202                    |
| Requisitos funcionales      | ~40 de 221                    |
| Areas core cubiertas        | 3 de 6                        |
| Fases de entrega            | 3 + 1 fase previa             |

---

## 4. Dependencias entre Casos de Uso

Los 20 casos de uso del MVP no son independientes. Forman un grafo dirigido de dependencias que condiciona el orden de implementacion. Ignorar este grafo implica construir funcionalidades que no pueden probarse ni desplegarse.

### Grafo completo de dependencias

```
UC-001 (Provision tenant) ---- FUNDACIONAL
  |
  +---> UC-002 (Autenticacion) ---- PREREQUISITO UNIVERSAL
  +---> UC-003 (Configuracion de tenant)
  +---> UC-004 (Roles y permisos)
  |
  +---> UC-008 (Tipos de socio) ---- CONFIGURACION BASE
          |
          +---> UC-006 (Ficha de socio)
          |       +---> UC-007 (Estados del socio)
          |       |       +---> UC-013 (Baja de socio)
          |       +---> UC-020 (Cargos manuales)
          |
          +---> UC-010 (Ejercicios)
          |
          +---> UC-017 (Planes de cuota)
          |       +---> UC-018 (Suscripciones)
          |       |       +---> UC-019 (Generacion cargos)
          |       |               +---> UC-021 (Registro cobros)
          |       |               +---> UC-023 (Remesas SEPA)
          |       |                       +---> UC-024 (Devoluciones SEPA)
          |       +---> UC-011 (Alta simple) <-- requiere tambien UC-010
          |
          +---> UC-056 (Importacion masiva)

UC-064 (Dashboard) <-- lectura de UC-006, UC-011, UC-019, UC-021
  +---> UC-065 (Graficos) <-- requiere tambien UC-010
```

### Puntos criticos

**4 UCs fundacionales** -- sin ellos el sistema no arranca:

| UC     | Rol                                          |
|--------|----------------------------------------------|
| UC-001 | Sin tenant no existe el sistema               |
| UC-002 | Sin autenticacion nadie accede                |
| UC-008 | Sin tipos de socio no hay socios              |
| UC-017 | Sin planes de cuota no hay economia           |

**Ruta critica** -- la cadena mas larga de dependencias (7 UCs):

```
UC-001 --> UC-008 --> UC-017 --> UC-018 --> UC-019 --> UC-023 --> UC-024
```

Cualquier retraso en un nodo de esta cadena impacta directamente la fecha de entrega del MVP completo.

### 4 flujos de negocio naturales

```
Setup:           UC-001 --> UC-002 --> UC-004
Configuracion:   UC-008 --> UC-010 --> UC-017
Ciclo del socio: UC-011 --> UC-006 --> UC-007 --> UC-013
Ciclo economico: UC-018 --> UC-019 --> UC-021 / UC-023 --> UC-024
```

---

## 5. Estrategia de Fases

### Fase 0: Scaffold (prerequisito)

**Objetivo:** Preparar la infraestructura base del proyecto -- entorno de desarrollo, estructura de modulos, pipelines de calidad y entorno de despliegue.

**Entregable:** Proyecto ejecutable con entorno completo, sin funcionalidad de negocio. Un equipo puede empezar a desarrollar al dia siguiente.

---

### Fase 1 (19 tareas: 13 backend + 6 frontend)

**Objetivo:** Construir los cimientos del sistema y la operativa diaria minima -- que un administrador pueda dar de alta socios y gestionar cuotas.

| UC     | Titulo                            | Area funcional |
|--------|-----------------------------------|----------------|
| UC-001 | Provision de nuevo tenant         | Identity       |
| UC-002 | Autenticacion multi-tenant        | Identity       |
| UC-003 | Configuracion de tenant           | Identity       |
| UC-008 | Configuracion de tipos de socio   | Membership     |
| UC-010 | Gestion de ejercicios             | Membership     |
| UC-007 | Gestion de estados del socio      | Membership     |
| UC-006 | Gestion de ficha de socio         | Membership     |
| UC-011 | Alta simple de socio              | Membership     |
| UC-013 | Baja de socio                     | Membership     |
| UC-017 | Configuracion de planes de cuota  | Treasury       |
| UC-018 | Gestion de suscripciones de cuota | Treasury       |
| UC-019 | Generacion masiva de cargos       | Treasury       |
| UC-021 | Registro de cobros                | Treasury       |

**Entregable:** Sistema operativo basico. Una colectividad puede: crear su espacio, autenticarse, dar de alta socios, definir cuotas, generar cargos y registrar cobros. Cubre el flujo completo desde la incorporacion del socio hasta el cobro.

**Justificacion del orden:** Se comienza por Identity (sin tenant ni login no hay sistema), se sigue con la configuracion de Membership (tipos de socio y ejercicios son prerequisitos de todo lo demas), y se cierra con Treasury (el flujo economico depende de que existan socios configurados). El frontend se limita a las 6 pantallas que el administrador necesita el dia a dia: login, cuotas, suscripciones, alta y baja de socios.

---

### Fase 2 (13 tareas: 5 backend + 8 frontend)

**Objetivo:** Completar las capacidades avanzadas de gestion economica y dar cobertura de interfaz a toda la operativa pendiente.

| UC     | Titulo                            | Area funcional |
|--------|-----------------------------------|----------------|
| UC-004 | Gestion de roles y permisos       | Identity       |
| UC-020 | Gestion de cargos manuales        | Treasury       |
| UC-056 | Importacion masiva de socios      | Transversal    |
| UC-023 | Generacion de remesas SEPA        | Treasury       |
| UC-024 | Gestion de devoluciones SEPA      | Treasury       |

**Entregable:** Sistema con capacidades avanzadas. Se incorporan: control de acceso por roles, cargos por conceptos no periodicos, importacion masiva para migracion desde otros sistemas, y el flujo completo de cobro bancario (remesas SEPA y gestion de devoluciones). El frontend completa TODAS las vistas operativas pendientes.

**Justificacion del orden:** SEPA depende de que existan cargos generados (Fase 1). La importacion masiva requiere que la estructura de socios y cuotas ya este operativa. Roles y permisos se posponen a Fase 2 porque durante la Fase 1 un unico perfil administrador es suficiente para validar la funcionalidad.

---

### Fase 3 (6 tareas: 2 backend + 4 frontend)

**Objetivo:** Cerrar el MVP con la capa de administracion avanzada y analitica de negocio.

| UC     | Titulo                            | Area funcional |
|--------|-----------------------------------|----------------|
| UC-064 | Dashboard principal y KPIs        | Transversal    |
| UC-065 | Graficos de evolucion             | Transversal    |
| UC-001 | Provision de tenant (interfaz)    | Identity       |
| UC-010 | Gestion de ejercicios (interfaz)  | Membership     |

**Entregable:** MVP completo. El administrador dispone de dashboards con metricas clave (socios activos, recaudacion, morosidad) y graficos de evolucion temporal. Las interfaces de superadministracion (provision de tenants) y configuracion avanzada (ejercicios) cierran el producto.

**Justificacion del orden:** El dashboard y los graficos son funcionalidades de lectura que agregan datos generados en las fases anteriores -- necesitan que el sistema ya tenga datos reales. La interfaz de provision de tenant se pospone porque durante las fases 1 y 2 la creacion de tenants puede hacerse por otros medios.

---

## 6. Estrategia de Capas

Las fases definen CUANDO se entrega. Las capas definen EN QUE ORDEN se construye dentro de cada fase. El modelo establece 8 capas gobernadas por tres principios:

1. **Dependencias estrictas:** cada capa solo depende de capas anteriores -- nunca de capas posteriores ni de la misma capa.
2. **Valor incremental:** al finalizar cada capa existe funcionalidad verificable y demostrable.
3. **Paralelismo habilitado:** dentro de cada capa, los casos de uso son independientes entre si y pueden desarrollarse en paralelo.

```
Capa 0 - Fundacion        [UC-001] [UC-002]
              |
Capa 1 - Configuracion    [UC-004] [UC-008]
              |
Capa 2 - Operativa basica [UC-010] [UC-006] [UC-017]
              |
Capa 3 - Operativa core   [UC-011] [UC-007] [UC-018]
              |
Capa 4 - Flujo economico  [UC-019] [UC-020] [UC-021]
              |
Capa 5 - SEPA             [UC-023] [UC-024]
              |
Capa 6 - Procesos avanz.  [UC-013]
              |
Capa 7 - Utilidades        [UC-056] [UC-064] [UC-065]
```

Este modelo garantiza que nunca se construye algo cuyas dependencias no estan resueltas, y que cada incremento es verificable de forma aislada.

---

## 7. Distribucion del Esfuerzo Backend / Frontend

La separacion entre servicios de negocio (backend) e interfaces de usuario (frontend) permite desarrollo paralelo por equipos o perfiles distintos. La distribucion del esfuerzo evoluciona de forma deliberada a lo largo de las fases:

| Fase   | Backend | Frontend | Proporcion       |
|--------|---------|----------|------------------|
| Fase 1 | 12      | 6        | 67% back / 33% front |
| Fase 2 | 5       | 8        | 38% back / 62% front |
| Fase 3 | 2       | 4        | 33% back / 67% front |

```
Esfuerzo por fase
                Backend                          Frontend
Fase 1  [============================================]  [======================]
         >>>>>>>>>>>>>>>>>> 67% <<<<<<<<<<<<<<<<<<       >>>>>>> 33% <<<<<<<<

Fase 2  [==================]                             [============================]
         >>>>>> 38% <<<<<<<<                              >>>>>>>>> 62% <<<<<<<<<<<<<<

Fase 3  [==========]                                     [====================]
         >>> 33% <<<                                      >>>>>> 67% <<<<<<<<<
```

**Fase 1: el backend lidera (67%).** Se construyen los cimientos: APIs, logica de negocio, validaciones y persistencia de las tres areas core. El frontend se limita a las 6 pantallas imprescindibles para la operativa diaria. El grueso del trabajo es "invisible" para el usuario pero es lo que hace posible todo lo demas.

**Fase 2: equilibrio (38/62).** El backend agrega las capacidades avanzadas (SEPA, importacion masiva, control de acceso) mientras el frontend completa todas las vistas operativas pendientes. Es la fase donde el producto se vuelve visualmente completo.

**Fase 3: el frontend lidera (67%).** El backend solo aniade dos servicios de lectura para dashboards. El trabajo se concentra en las interfaces de administracion y analitica que cierran la experiencia de usuario. El producto pasa de funcional a terminado.

Esta progresion es natural: primero se construyen los cimientos, luego se levantan las paredes, finalmente se pinta la fachada.

---

## 8. Vision Post-MVP

Fuera del MVP quedan 57 casos de uso que representan la evolucion natural del producto. Ninguno bloquea la operativa diaria de una colectividad:

- **Events:** gestion de eventos sociales, cenas, equipos. Las colectividades gestionan esto manualmente durante la adopcion inicial.
- **Communication:** plantillas, anuncios, notificaciones. Las herramientas de email existentes cubren esta necesidad temporalmente.
- **Documents:** actas, categorias, gestion documental. Carpetas compartidas sirven como solucion transitoria.
- **Portal del Socio:** acceso autonomo para los socios. No es critico hasta que la base de usuarios crezca.
- **Cumplimiento Normativo:** funcionalidades de auditoria y trazabilidad avanzada.

La incorporacion de estas areas seguira la misma logica de priorizacion: primero lo que genera mas valor operativo, despues lo que mejora la experiencia.
