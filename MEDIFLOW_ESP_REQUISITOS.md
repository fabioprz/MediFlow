# MediFlow · Especificación de requisitos

**Autor:** Fabio Pérez Gutiérrez
**Curso:** Ingeniería de Software I · SIS3407
**Versión:** 1.0
**Prototipo navegable:**https://www.figma.com/make/nrkTsWjtU8PXDGl2ZvVjhG/MediFlow?fullscreen=1&t=1XEQY6VCC2IMFIVr-1&code-node-id=0-6


---

## 1. Propósito y alcance

### 1.1 Propósito

MediFlow ordena la fila de pacientes de urgencias según el nivel de urgencia que asigna el personal de salud, calcula cuánto esperará cada paciente y se lo muestra. MediFlow organiza la atención; no diagnostica ni decide la urgencia.

**Tipo de sistema:** de información y SaaS. Registra, ordena y muestra datos de pacientes, así que los atributos de calidad que impone son disponibilidad, seguridad de los datos, rendimiento al actualizar la fila y facilidad de uso para un personal con prisa, así mismo es un SaaS ya que es un servicio ofrecido a hospitales y/o clínicas como software para que cada institución lo use de manera particular y como lo requiera.

### 1.2 Dentro del alcance

- Registrar a los pacientes que llegan a urgencias.
- Asignar y modificar el nivel de urgencia, solo por personal autorizado.
- Ordenar la fila automáticamente por nivel de urgencia.
- Calcular y mostrar el tiempo de espera.
- Avisar las revaloraciones y registrar a los pacientes que se retiran.

### 1.3 Fuera del alcance

- Diagnosticar enfermedades o decidir la urgencia de forma automática: requiere conocimiento y responsabilidad médica.
- Administrar medicamentos o tratamientos.
- Gestionar pagos, seguros o facturación: no forman parte del problema de la fila.

---

## 2. Usuarios y su contexto

En la Visión del producto teníamos tres usuarios: paciente, personal médico y administrador. La entrevista mostró que "personal médico" en realidad son tres roles con tareas distintas: recepción, enfermería de triage y médico.

| Usuario | Qué hace hoy (entrevista) | Qué necesita del sistema | Qué le preocupa |
|---|---|---|---|
| **Paciente** | Espera sin saber cuánto le falta; lo llaman en voz alta y a veces no oye (H3, RF5). | Ver su turno y su tiempo de espera. | Que alguien que llegó después pase antes sin explicación (D2). |
| **Recepcionista** | Captura los datos dos veces: en la computadora y en una hoja (H1). | Registrar una sola vez y dar un número de turno. | Perder el orden cuando la sala se llena (D1). |
| **Enfermera de triage** | Decide la urgencia, reacomoda la fila a mano y revisa cada 2 horas a los que siguen esperando (C2, E2). | Asignar el nivel rápido y ver la fila ordenada. | Que un paciente empeore sin que nadie lo note (E1). |
| **Médico** | Cambia el nivel si ve algo distinto y llama al paciente de voz (RF2, RF5). | Ver quién sigue y llamarlo. | Atender tarde a un paciente grave. |
| **Administrador** | No se entrevistó. | Ver cuántos pacientes esperan y cuánto tardan. | Una mala organización de los recursos. |

---

## 3. Nomenclatura y definiciones

| Elemento | Significado |
|---|---|
| **RF#** | Requisito funcional: algo que el sistema debe hacer. |
| **RNF#** | Requisito no funcional: una condición de calidad que el sistema debe cumplir, con métrica. |
| **CU-##** | Caso de uso. |
| **P#** | Pantalla del prototipo en Figma. |
| **Origen** | De dónde salió el requisito y si está **Confirmado** (validado en la entrevista) o es **Supuesto** (nadie lo ha confirmado). |
| **Prioridad** | Alta: sin esto el sistema no sirve. Media: importante, puede ir en un segundo incremento. Baja: puede esperar. |

| Término | Definición |
|---|---|
| **Triage** | Revisión inicial en la que enfermería decide qué tan urgente es un paciente. |
| **Nivel de urgencia** | 1 rojo (inmediato), 2 naranja, 3 amarillo, 4 verde, 5 azul (menos urgente). |
| **Hora de llegada** | Hora en que el paciente se registró en recepción. |
| **Tiempo de espera** | Tiempo que falta para que llamen al paciente, no para que termine su consulta. |
| **Revaloración** | Nueva revisión del nivel de un paciente que sigue esperando. |

---

## 4. Requisitos funcionales

### RF1 · Registrar paciente

| Campo | Valor |
|---|---|
| **ID** | RF1 |
| **Descripción** | El sistema debe permitir a recepción registrar a un paciente con nombre, fecha de nacimiento, motivo de consulta y teléfono de contacto, y asignarle un número de turno único. |
| **Actor** | Recepcionista |
| **Origen** | Visión del producto (alcance) · corregido en la entrevista (RF1, H1): se agregó el teléfono. **Confirmado** |
| **Prioridad** | Alta |
| **Criterio de aceptación** | Al registrar a dos pacientes, sus números de turno no se repiten. Si falta un dato obligatorio, el sistema no guarda el registro. |
| **Relaciones** | Caso de uso: CU-01. Lo necesitan: RF2, RF8. |

### RF2 · Asignar nivel de urgencia

| Campo | Valor |
|---|---|
| **ID** | RF2 |
| **Descripción** | El sistema debe permitir solo a enfermería de triage asignar el nivel de urgencia inicial de un paciente, del 1 al 5. |
| **Actor** | Enfermera de triage |
| **Origen** | Visión del producto (regla de negocio 1) · corregido en la entrevista (RF2): asignar es solo de triage. **Confirmado** |
| **Prioridad** | Alta |
| **Criterio de aceptación** | Con una cuenta de triage, el nivel se guarda. Con una cuenta de recepción, el sistema rechaza la acción y muestra un mensaje. |
| **Relaciones** | Caso de uso: CU-02. Depende de: RF1. Dispara: RF3, RF4. |

### RF3 · Ordenar la fila por urgencia

| Campo | Valor |
|---|---|
| **ID** | RF3 |
| **Descripción** | El sistema debe ordenar la fila del nivel 1 (rojo, más urgente) al 5 (azul, menos urgente); con el mismo nivel, pasa primero quien llegó antes a recepción. |
| **Actor** | Sistema (automático) |
| **Origen** | Visión del producto (regla de negocio 2) · confirmado en la entrevista (RF3). **Confirmado** |
| **Prioridad** | Alta |
| **Criterio de aceptación** | Se registran 4 pacientes con niveles y horas conocidos, y el orden en pantalla coincide con el esperado. |
| **Relaciones** | Casos de uso: CU-02, CU-03, CU-05. Depende de: RF2. |

### RF4 · Recalcular tiempo de espera

| Campo | Valor |
|---|---|
| **ID** | RF4 |
| **Descripción** | El sistema debe recalcular el tiempo de espera de todos los pacientes cada vez que un paciente entra, sale o cambia de nivel, sumando 20 minutos por cada paciente adelante y dividiendo entre el número de médicos en turno. |
| **Actor** | Sistema (automático) |
| **Origen** | Visión del producto (regla de negocio 3) · método confirmado en la entrevista (H2). El promedio de 20 minutos es **Supuesto**. |
| **Prioridad** | Alta |
| **Criterio de aceptación** | Con 3 pacientes adelante y 1 médico, el sistema muestra 60 minutos. Con 2 médicos, muestra 30 minutos. |
| **Relaciones** | Casos de uso: CU-02, CU-03, CU-05, CU-07. Depende de: RF3. |

### RF5 · Mostrar turno en la sala

| Campo | Valor |
|---|---|
| **ID** | RF5 |
| **Descripción** | El sistema debe mostrar en la pantalla de la sala el número de turno y el tiempo de espera de cada paciente, sin su nombre. |
| **Actor** | Paciente |
| **Origen** | Visión del producto · confirmado en la entrevista (RF5, H3). **Confirmado** |
| **Prioridad** | Media |
| **Criterio de aceptación** | Al registrar a un paciente, su número aparece en la pantalla de la sala y su nombre no aparece. |
| **Relaciones** | Caso de uso: CU-06. Depende de: RF4. |

### RF6 · Modificar nivel de urgencia

| Campo | Valor |
|---|---|
| **ID** | RF6 |
| **Descripción** | El sistema debe permitir a enfermería de triage o al médico cambiar el nivel de urgencia de un paciente que ya está en la fila. |
| **Actor** | Enfermera de triage, Médico |
| **Origen** | Entrevista (RF2, E1): el médico también cambia el nivel. **Confirmado** |
| **Prioridad** | Alta |
| **Criterio de aceptación** | Un médico cambia a un paciente de verde a rojo y el cambio se guarda. Con una cuenta de recepción, el sistema rechaza la acción. |
| **Relaciones** | Casos de uso: CU-03, CU-04. Depende de: RF2. Dispara: RF3, RF4. |

### RF7 · Avisar revaloración

| Campo | Valor |
|---|---|
| **ID** | RF7 |
| **Descripción** | El sistema debe avisar a enfermería de triage cuando un paciente lleve 2 horas en la fila sin que se revise su nivel. |
| **Actor** | Enfermera de triage |
| **Origen** | Entrevista (E2): hallazgo inesperado. **Confirmado** |
| **Prioridad** | Media |
| **Criterio de aceptación** | Un paciente pasa 2 horas sin revisión y aparece un aviso en la lista de triage. |
| **Relaciones** | Caso de uso: CU-04. Depende de: RF2. |

### RF8 · Registrar retiro de paciente

| Campo | Valor |
|---|---|
| **ID** | RF8 |
| **Descripción** | El sistema debe permitir a recepción o a triage marcar que un paciente se retiró y quitarlo de la fila. |
| **Actor** | Recepcionista, Enfermera de triage |
| **Origen** | Entrevista (E2): hallazgo inesperado. **Confirmado** |
| **Prioridad** | Media |
| **Criterio de aceptación** | Se marca a un paciente como retirado y deja de aparecer en la fila y en la pantalla de la sala. |
| **Relaciones** | Caso de uso: CU-07. Depende de: RF1. Dispara: RF4. |

### RF9 · Llamar al siguiente paciente

| Campo | Valor |
|---|---|
| **ID** | RF9 |
| **Descripción** | El sistema debe permitir al médico llamar al primer paciente de la fila, mostrar su número en la pantalla de la sala y quitarlo de la espera. |
| **Actor** | Médico |
| **Origen** | Entrevista (RF5): hoy los llaman en voz alta y nadie los quita de la lista. **Confirmado** |
| **Prioridad** | Alta |
| **Criterio de aceptación** | El médico presiona "Llamar", el número del primer paciente aparece en la pantalla de la sala y el paciente sale de la fila. |
| **Relaciones** | Caso de uso: CU-05. Depende de: RF3. Dispara: RF4. |

### RF10 · Supervisar ocupación de la sala

| Campo | Valor |
|---|---|
| **ID** | RF10 |
| **Descripción** | El sistema debe mostrar al administrador cuántos pacientes esperan en cada nivel de urgencia y el tiempo de espera promedio del día. |
| **Actor** | Administrador |
| **Origen** | Visión del producto (usuario administrador). No se entrevistó a un administrador. **Supuesto** |
| **Prioridad** | Baja |
| **Criterio de aceptación** | Con 2 pacientes en rojo y 3 en verde, el panel muestra esos números y el promedio coincide con el cálculo a mano. |
| **Relaciones** | Caso de uso: CU-08. Depende de: RF3, RF4. |

---

## 5. Requisitos no funcionales

### 5.1 Disponibilidad

#### RNF1 · Operación continua

| Campo | Valor |
|---|---|
| **ID** | RNF1 |
| **Descripción** | El sistema debe estar disponible las 24 horas, todos los días. |
| **Métrica** | 99.5 % del tiempo cada mes (máximo 3.6 horas sin servicio al mes, incluido el mantenimiento). |
| **Por qué ese límite** | Urgencias atiende las 24 horas (entrevista, RNF1). 3.6 horas al mes se pueden cubrir con la hoja de papel que ya usan como respaldo; más tiempo que eso vuelve a traer el desorden de D1. |
| **Origen** | Entrevista (RNF1). El horario es **Confirmado**; el 99.5 % es **Supuesto**. |
| **Criterio de aceptación** | Un monitor mide el sistema durante 30 días y registra 3.6 horas o menos sin servicio. |

#### RNF2 · Recuperación sin pérdida

| Campo | Valor |
|---|---|
| **ID** | RNF2 |
| **Descripción** | Si el sistema se cae, debe volver a funcionar sin perder pacientes registrados. |
| **Métrica** | 15 minutos o menos para volver a funcionar, 0 pacientes perdidos. |
| **Por qué ese límite** | En 15 minutos pueden llegar varios pacientes; más tiempo obliga a reconstruir la fila a mano, como en D1. |
| **Origen** | Entrevista (D1) · **Supuesto** |
| **Criterio de aceptación** | Se apaga el servidor con 10 pacientes en la fila; al volver, en 15 minutos o menos, están los 10 en el mismo orden. |

### 5.2 Seguridad

#### RNF3 · Acceso con cuenta

| Campo | Valor |
|---|---|
| **ID** | RNF3 |
| **Descripción** | Todo el personal debe entrar con usuario y contraseña, y la sesión debe cerrarse sola si no se usa. |
| **Métrica** | Cierre de sesión tras 15 minutos sin uso. |
| **Por qué ese límite** | Las computadoras de recepción y triage se comparten entre turnos. 15 minutos es menos que el tiempo que una computadora queda sola en una sala llena. |
| **Origen** | Visión del producto (atributo seguridad) · **Supuesto** |
| **Criterio de aceptación** | Sin usuario y contraseña no se puede entrar. Una sesión sin uso se cierra a los 15 minutos. |

#### RNF4 · Registro de cambios de urgencia

| Campo | Valor |
|---|---|
| **ID** | RNF4 |
| **Descripción** | Cada asignación o cambio de nivel debe guardar quién lo hizo, cuándo, y el nivel anterior y el nuevo. |
| **Métrica** | 100 % de los cambios de nivel registrados. |
| **Por qué ese límite** | El nivel de urgencia es una decisión médica con responsabilidad (Visión, alcance). En E1 el cambio solo quedó tachado en papel. |
| **Origen** | Entrevista (E1) · **Supuesto** |
| **Criterio de aceptación** | Se hacen 5 cambios de nivel y el historial muestra los 5 con usuario, fecha, hora y niveles. |

### 5.3 Rendimiento

#### RNF5 · Actualización de la fila

| Campo | Valor |
|---|---|
| **ID** | RNF5 |
| **Descripción** | Un cambio en la fila debe verse en todas las pantallas casi al instante. |
| **Métrica** | 5 segundos o menos. |
| **Por qué ese límite** | Para un paciente en rojo la entrevistada pidió "al momento". 5 segundos es menos de lo que tarda el médico en levantarse para llamarlo. |
| **Origen** | Entrevista (RNF5) · **Confirmado** |
| **Criterio de aceptación** | Se cambia el nivel de un paciente y un cronómetro mide 5 segundos o menos hasta que se ve en dos pantallas distintas. |

### 5.4 Usabilidad

#### RNF6 · Asignación rápida

| Campo | Valor |
|---|---|
| **ID** | RNF6 |
| **Descripción** | La enfermera de triage debe poder asignar un nivel sin pasos de más. |
| **Métrica** | 3 clics o menos desde la lista de pendientes hasta que el nivel queda guardado. |
| **Por qué ese límite** | En horas pico la enfermera hace triage con la sala llena (C1, D1). Cada paso de más retrasa al siguiente paciente. |
| **Origen** | Entrevista (C1, D1) · **Supuesto** |
| **Criterio de aceptación** | En el prototipo, se cuentan los clics de P2 a P6: deben ser 3 o menos. |

---

## 6. Casos de uso

### 6.1 Diagrama

![Diagrama de casos de uso](diagramas/casos-de-uso.png)

Archivo editable: [`diagramas/casos-de-uso.drawio`](diagramas/casos-de-uso.drawio)

| Caso de uso | Actor | Requisitos que realiza |
|---|---|---|
| CU-01 Registrar paciente | Recepcionista | RF1 |
| CU-02 Asignar nivel de urgencia | Enfermera de triage | RF2, RF3, RF4 |
| CU-03 Modificar nivel de urgencia | Enfermera de triage, Médico | RF6, RF3, RF4 |
| CU-04 Revalorar paciente en espera | Enfermera de triage | RF7, RF6 |
| CU-05 Llamar al siguiente paciente | Médico | RF9, RF3, RF4 |
| CU-06 Consultar turno de espera | Paciente | RF5 |
| CU-07 Registrar retiro de paciente | Recepcionista, Enfermera de triage | RF8, RF4 |
| CU-08 Supervisar ocupación de la sala | Administrador | RF10 |

### 6.2 Caso de uso escrito a detalle

El caso de uso más importante, **CU-02 Asignar nivel de urgencia**, está escrito completo en [`diagramas/caso-de-uso-detallado.md`](diagramas/caso-de-uso-detallado.md).

---

## 7. Tabla de trazabilidad

| Req. | Origen | Estado | Caso de uso | Pantalla del prototipo |
|---|---|---|---|---|
| RF1 | Visión · Entrevista (RF1, H1) | Confirmado | CU-01 | — |
| RF2 | Visión · Entrevista (RF2) | Confirmado | CU-02 | P3 |
| RF3 | Visión · Entrevista (RF3) | Confirmado | CU-02, CU-03, CU-05 | P6 |
| RF4 | Visión · Entrevista (H2) | Confirmado (promedio de 20 min supuesto) | CU-02, CU-03, CU-05, CU-07 | P6, P7 |
| RF5 | Visión · Entrevista (RF5) | Confirmado | CU-06 | P7 |
| RF6 | Entrevista (RF2, E1) | Confirmado | CU-03, CU-04 | — |
| RF7 | Entrevista (E2) | Confirmado | CU-04 | — |
| RF8 | Entrevista (E2) | Confirmado | CU-07 | P5 |
| RF9 | Entrevista (RF5) | Confirmado | CU-05 | — |
| RF10 | Visión | Supuesto | CU-08 | — |
| RNF1 | Entrevista (RNF1) | Supuesto (métrica) | Todos | — |
| RNF2 | Entrevista (D1) | Supuesto | Todos | — |
| RNF3 | Visión | Supuesto | Todos | P1 |
| RNF4 | Entrevista (E1) | Supuesto | CU-02, CU-03 | — |
| RNF5 | Entrevista (RNF5) | Confirmado | CU-02, CU-03, CU-05 | P6, P7 |
| RNF6 | Entrevista (C1, D1) | Supuesto | CU-02 | P2 → P3 → P6 |

---

## 8. Revisión de la dupla

**Revisor:** Ricardo Antonio Vargas Cremades
**Fecha:** 24/09/2026

La dupla buscó requisitos que se pudieran entender de dos maneras. Cada ambigüedad se resolvió con la entrevista.

| Req. | Frase ambigua | Interpretación A | Interpretación B | Cómo quedó |
|---|---|---|---|---|
| RF3 | "Más urgente" | El 5 es lo más urgente. | El 1 es lo más urgente. | Nivel 1 = rojo, el más urgente. |
| RF3 | "Se registró primero" | Hora de llegada a recepción. | Hora en que le asignaron urgencia. | Hora de llegada a recepción. |
| RF4 | "Tiempo de espera" | Hasta que llamen al paciente. | Hasta que lo vea el médico. | Hasta que lo llamen (sección 3). |
| RF2 | "Personal de triage" | Solo médicos. | Médicos y enfermería. | Asigna enfermería; modifica enfermería o médico (RF6). |
| RNF1 | "99.5 % del tiempo" | Incluye mantenimiento programado. | No lo incluye. | Sí lo incluye. |

---

## 9. Registro de cambios

| Versión | Fecha | Cambio | Motivo |
|---|---|---|---|
| 0.1 | 18/08/2026 | Visión del producto: sistema, usuarios, alcance y atributos de calidad. | Unidad 1. |
| 0.2 | 22/09/2026 | Primer borrador: RF1 a RF5 y RNF1 a RNF3, con supuestos marcados. | Inicio de la Unidad 2. |
| 0.3 | [24/09/2026] | Se aclararon "más urgente", "tiempo de espera", "hora de llegada" y "99.5 %". | Revisión de la dupla. |
| 1.0 | [24/09/2026] | RF1 agrega teléfono. RF2 se divide en RF2 (asignar) y RF6 (modificar). Se agregan RF7, RF8, RF9 y RF10. Los RNF se agrupan por atributo, con métrica y justificación; se agregan RNF2, RNF4 y RNF6. Se agregan casos de uso y trazabilidad. | Entrevista con la dupla. |
