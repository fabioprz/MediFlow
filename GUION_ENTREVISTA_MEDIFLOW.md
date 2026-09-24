# MediFlow · Guion de entrevista

**Autor:** Fabio Pérez Gutiérrez
**Curso:** Ingeniería de Software I · SIS3407
**Entrevistada:** Ricardo Antonio Vargas Cremades, en el papel de jefa de enfermería de urgencias
**Fecha de la entrevista:** 22/09/2026
**Duración:** 30 minutos

**Objetivo:** entender cómo se organiza hoy la fila de pacientes en urgencias y verificar los requisitos que estábamos suponiendo.

**Orden:** contexto → proceso actual → dolores → excepciones → verificación de supuestos. Así la persona habla de su trabajo antes de que se mencione el sistema.

---

## 1. Preguntas

### Tramo 1 · Contexto

- **C1.** ¿Cómo es un turno normal para usted, desde que llega hasta que se va?
- **C2.** ¿Qué papel tiene usted en la atención de los pacientes que llegan?

### Tramo 2 · Proceso actual

- **H1.** Piense en el último paciente que atendieron hoy. ¿Qué pasó desde que llegó hasta que lo vio el médico?
- **H2.** La última vez que un paciente preguntó cuánto le faltaba, ¿qué le respondieron y cómo lo calcularon?
- **H3.** Cuénteme de la última vez que llegó un paciente muy grave con la sala llena. ¿Qué hicieron con los que ya esperaban?

### Tramo 3 · Dolores

- **D1.** Cuénteme de un día reciente en que la sala de espera se le complicó. ¿Qué fue lo más difícil?
- **D2.** ¿Qué quejas escucha más seguido de los pacientes que esperan?

### Tramo 4 · Excepciones

- **E1.** Cuénteme de una vez en que la urgencia de un paciente se asignó mal. ¿Cómo se dieron cuenta y qué hicieron?
- **E2.** ¿Qué pasa con un paciente poco urgente que lleva horas esperando mientras siguen llegando casos graves?

### Tramo 5 · Verificación de supuestos

| Req. | Lo que suponíamos | Pregunta |
|---|---|---|
| RF1 | Recepción registra y pide nombre, fecha de nacimiento y motivo. | ¿Quién registra al paciente cuando llega y qué datos le pide? |
| RF2 | La urgencia la asigna y la cambia el personal de triage. | ¿Quién decide qué tan urgente es un paciente? |
| RF3 | Hay 5 niveles y el 1 es el más urgente. | ¿Cómo clasifican la urgencia? ¿Qué nivel se atiende primero? |
| RF3 | En un empate pasa quien llegó antes. | Si dos pacientes tienen la misma urgencia, ¿cómo deciden a quién atender primero? |
| RF5 | El paciente ve su turno en una pantalla. | ¿Cómo se entera hoy un paciente de que ya es su turno? |
| RNF1 | Urgencias opera las 24 horas. | ¿En qué horario atienden pacientes? |
| RNF5 | 5 segundos de retraso es aceptable. | Si un paciente cambia de urgencia, ¿en cuánto tiempo necesita enterarse el resto del personal? |

### Revisión de las preguntas

Revisamos todas las preguntas y reescribimos las que sugerían la respuesta o se contestaban con sí o no.

| Req. | Pregunta original | Problema | Cómo quedó |
|---|---|---|---|
| RF2 | ¿La urgencia la asigna enfermería? | Sugiere la respuesta. | ¿Quién decide qué tan urgente es un paciente? |
| RF3 | ¿Usan 5 niveles de urgencia? | Se contesta con sí o no. | ¿Cómo clasifican la urgencia? |
| RF3 | ¿Se atiende primero al que llegó antes? | Se contesta con sí o no. | ¿Cómo deciden a quién atender primero? |
| RF5 | ¿Lo quieren en pantalla, SMS o app? | Sugiere las respuestas. | ¿Cómo se entera hoy un paciente de que ya es su turno? |
| RNF5 | ¿5 segundos de retraso es aceptable? | Sugiere la respuesta y se contesta con sí o no. | ¿En cuánto tiempo necesita enterarse el resto del personal? |

---

## 2. Bitácora de la entrevista

### Notas por pregunta

| Preg. | Qué respondió | Qué aprendimos |
|---|---|---|
| C1 | Trabaja turnos de 12 horas. Recibe la entrega del turno anterior, revisa la sala y hace el triage de los que van llegando. | La información de la fila se pasa de voz entre turnos y se puede perder. |
| C2 | Hace el triage: toma signos vitales y decide qué tan urgente es cada paciente. Cuando llega alguien grave, reacomoda la fila. | Ella decide la urgencia y ordena la fila a mano. |
| H1 | Un señor con dolor de estómago. Recepción anotó sus datos en la computadora y en una hoja. Pasó a triage, le pusieron amarillo, esperó 50 minutos y lo llamaron en voz alta. | Los datos se capturan dos veces: computadora y papel. |
| H2 | Le dijo "como una hora". Lo calculó a ojo: contó los pacientes que iban antes y cuántos médicos estaban atendiendo. | El tiempo de espera depende de los pacientes adelante y del número de médicos. |
| H3 | Llegó un accidente de moto y pasó directo. Los demás esperaron más, nadie les avisó y dos se quejaron en recepción. | Cuando la fila cambia, los pacientes no se enteran. |
| D1 | Un lunes con 30 personas esperando, la hoja ya no tenía espacio y se perdió el orden. Tuvo que preguntar uno por uno quién había llegado primero. | Con mucha gente, el orden en papel se pierde. |
| D2 | "¿Cuánto me falta?" y "yo llegué antes que él". | Los pacientes no entienden por qué alguien que llegó después pasa primero. |
| E1 | Una señora con dolor de pecho quedó en verde. A los 20 minutos se puso pálida y la subieron a rojo. Avisó al médico de voz y corrigió la hoja. | El nivel se corrige seguido, y el médico también lo cambia. |
| E2 | Algunos esperan hasta 4 horas. Cada 2 horas revisa de nuevo a los que llevan mucho. Algunos se van sin avisar y no se dan cuenta. | Hay que recordar las revaloraciones y quitar de la fila a quien se va. |
| RF1 | Registra recepción. Pide nombre, fecha de nacimiento, motivo y un teléfono de contacto. | Falta el teléfono. |
| RF2 | Triage asigna el nivel. Si el médico ve algo distinto, él lo cambia. | Asignar es solo de triage. Modificar es de triage y del médico. |
| RF3 | Usan 5 colores: rojo, naranja, amarillo, verde y azul. Rojo es inmediato. | Nivel 1 = rojo y nivel 5 = azul. |
| RF3 | Con el mismo color, pasa el que llegó primero a recepción. | El empate se resuelve por hora de llegada a recepción. |
| RF5 | Los llaman por su nombre en voz alta. A veces no lo oyen o no están en la sala. | Una pantalla con número de turno resuelve esto y protege el nombre. |
| RNF1 | Urgencias atiende las 24 horas, todos los días. | El sistema no puede tener horario de apagado. |
| RNF5 | "Si es rojo, al momento. Lo demás, en menos de un minuto." | 5 segundos cumple. |

### Supuestos que resultaron falsos

| Req. | Lo que suponíamos | Lo que es en realidad | Qué cambiamos |
|---|---|---|---|
| RF1 | Recepción pide nombre, fecha de nacimiento y motivo. | También pide un teléfono de contacto. | Agregamos el teléfono a RF1. |
| RF2 | Solo triage asigna y cambia el nivel. | Triage lo asigna, pero el médico también lo cambia. | Separamos RF2 (asignar, solo triage) y RF6 (modificar, triage o médico). |

### Supuestos confirmados

| Req. | Supuesto | Detalle que agregamos |
|---|---|---|
| RF3 | Hay 5 niveles y el 1 es el más urgente. | Los colores: 1 rojo, 2 naranja, 3 amarillo, 4 verde, 5 azul. |
| RF3 | En un empate pasa quien llegó antes. | Cuenta la hora de llegada a recepción. |
| RF5 | Conviene una pantalla para avisar el turno. | Hoy los llaman en voz alta y muchos no oyen. |
| RNF1 | Operan las 24 horas. | Todos los días. |
| RNF5 | 5 segundos de retraso es aceptable. | El límite real es "al momento" para rojo. |

### Lo que no esperábamos

- **Revaloración cada 2 horas (E2).** No lo teníamos. Se volvió RF7: el sistema avisa cuando un paciente lleva 2 horas sin revisión.
- **Pacientes que se van sin avisar (E2).** Siguen ocupando un lugar en la fila. Se volvió RF8: registrar el retiro.
- **El médico llama al paciente (RF5).** Nadie quitaba al paciente de la fila al pasar a consulta. Se volvió RF9: llamar al siguiente paciente.
- **Doble captura (H1).** Los datos se escriben en la computadora y en papel. Refuerza RF1.
- **"Yo llegué antes que él" (D2).** Los pacientes no entienden la prioridad. Queda como pregunta abierta para diseño: cómo explicarlo en la pantalla de la sala.

---

## 3. Ficha de dominio para la dupla

> **Tu papel:** jefa o jefe de enfermería del área de urgencias.
>
> **El lugar:** una clínica privada de tamaño mediano en una ciudad de México. Urgencias recibe pacientes todo el día, desde gripas y cortadas hasta accidentes y dolores de pecho. Hay recepción, personal de enfermería y varios médicos por turno. En horas pico la sala de espera se llena.
>
> **Cómo trabajan:** usan una computadora en recepción, hojas de papel y mucha comunicación de voz entre el personal. No tienen un sistema que ordene la fila.
>
> **Qué hacer:** responde como alguien con años en este trabajo. Inventa detalles realistas cuando te pregunten algo concreto. No tienes que conocer MediFlow ni ayudar al entrevistador.
