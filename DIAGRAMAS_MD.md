# MediFlow · Diagramas de arquitectura (C4)

**Autor:** Fabio Pérez Gutiérrez
**Curso:** Ingeniería de Software I · SIS3407 · Unidad 3

Los tres primeros niveles del modelo C4, escritos como texto de Mermaid. GitHub los dibuja directamente. Para verlos en otro lado, pega el código en [mermaid.live](https://mermaid.live).

---

## Nivel 1 · Diagrama de contexto

El sistema completo, quién lo usa y con quién habla. No lleva tecnología.

```mermaid
flowchart TD
rec([Recepcionista])
tri([Enfermera de triage])
med([Médico])
adm([Administrador])
pac([Paciente])
sis[MediFlow<br/>fila de urgencias por prioridad]
rec -->|registra a los pacientes| sis
tri -->|asigna y revalora el nivel de urgencia| sis
med -->|cambia el nivel y llama al siguiente| sis
adm -->|supervisa ocupación y tiempos| sis
sis -->|muestra turno y tiempo de espera| pac
style sis fill:#ff5d00,color:#fff
```

MediFlow no se comunica con ningún sistema externo, porque ningún requisito lo pide. Los cinco actores son los de la especificación de requisitos (sección 2).

---

## Nivel 2 · Diagrama de contenedores

Las piezas que se despliegan y se ejecutan por separado.

```mermaid
flowchart TD
rec([Recepcionista])
tri([Enfermera de triage])
med([Médico])
adm([Administrador])
pac([Paciente])
subgraph sistema[MediFlow]
web[App web del personal<br/>React]
sala[Pantalla de la sala<br/>React]
api[API<br/>Node.js]
db[(Base de datos<br/>PostgreSQL<br/>un esquema por institución)]
end
rec -->|usa| web
tri -->|usa| web
med -->|usa| web
adm -->|usa| web
pac -->|mira| sala
web -->|HTTPS/JSON| api
sala -->|HTTPS/JSON| api
api -->|lee y escribe| db
style api fill:#ff5d00,color:#fff
```

- **La pantalla de la sala es un contenedor aparte** de la app del personal. Corre en otro equipo y nunca muestra nombres (RF5).
- **Una sola API y una sola base de datos para todas las instituciones** (SaaS). Cada institución tiene su propio esquema dentro de la base, para poder restaurar una sin afectar a las demás (RNF2).
- La API es la pieza que debe cumplir RNF1 (99.5 % de disponibilidad) y RNF5 (cambios visibles en 5 segundos).

---

## Nivel 3 · Diagrama de componentes

Lo que hay dentro de la API.

```mermaid
flowchart TD
web[App web del personal]
sala[Pantalla de la sala]
subgraph api[API de MediFlow]
auth[Acceso y roles<br/>usuarios y permisos]
registro[Registro<br/>pacientes y número de turno]
urgencia[Nivel de urgencia<br/>asignar, modificar y revalorar]
fila[Fila de espera<br/>orden y llamados]
tiempos[Tiempo de espera<br/>cálculo por turno]
supervision[Supervisión<br/>ocupación y promedios]
historial[Historial de cambios<br/>quién, cuándo y niveles]
notif[Notificador<br/>interfaz de aviso]
end
db[(Base de datos)]
web --> registro
web --> urgencia
web --> fila
web --> supervision
sala -->|consulta turnos| fila
registro -->|valida el rol| auth
urgencia -->|valida el rol| auth
fila -->|valida el rol| auth
registro -->|nuevo turno| fila
urgencia -->|nivel guardado| fila
urgencia -->|registra el cambio| historial
fila -->|pide recalcular| tiempos
supervision -->|pregunta cuántos esperan| fila
supervision -->|pregunta los tiempos| tiempos
fila -->|enviarActualizacion| notif
urgencia -->|enviarAviso| notif
notif --> sala
notif --> web
auth --> db
registro --> db
urgencia --> db
fila --> db
historial --> db
style urgencia fill:#ff5d00,color:#fff
```

| Componente | Requisitos que atiende |
|---|---|
| Acceso y roles | RF2, RF6, RF8, RF9, RNF3 |
| Registro | RF1 |
| Nivel de urgencia | RF2, RF6, RF7 |
| Fila de espera | RF3, RF8, RF9 |
| Tiempo de espera | RF4 |
| Supervisión | RF10 |
| Historial de cambios | RNF4 |
| Notificador | RF5, RF7, RNF5 |

Dos decisiones que se ven en el dibujo:

1. **Los componentes se preguntan entre sí en lugar de leer las tablas del otro.** Tiempo de espera y Supervisión no tocan la base de datos: le preguntan a Fila. Por eso solo cinco componentes se conectan a la base.
2. **El Notificador es una interfaz, no una tecnología.** Hoy avisa a la pantalla de la sala y a la lista de triage. Si después se agrega SMS o una app para el paciente, se agrega otra forma de enviar sin cambiar Fila ni Nivel de urgencia (patrón Adapter).

**Acceso y roles** es donde vive el candidato a polimorfismo: cada tipo de usuario (recepción, triage, médico, administrador) define qué acciones puede hacer. Registro, Nivel de urgencia y Fila solo preguntan "valida el rol".

---

## Supuestos

- La tecnología (React, Node.js, PostgreSQL) es un supuesto. Cámbiala por la que usa tu MediFlow.
- Los nombres `enviarActualizacion` y `enviarAviso` son operaciones propuestas del Notificador.
