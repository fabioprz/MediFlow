# Visión del producto

> Plantilla del curso · Ingeniería de Software I · SIS3407
> Este documento es el primer entregable del semestre y la base de todo lo que viene después.
> Se entrega completo en la semana 4 y se presenta ante el grupo.

---

Autor: Fabio Pérez Gutiérrez
Fecha de la última versión: 18 de agosto de 2026  
Repositorio:  

---

## 1. Descripción del sistema

Nombre del sistema: MediFlow

Descripción: MediFlow es un sistema para hospitales y clínicas que organiza la atención de los pacientes según el nivel de urgencia indicado por el personal médico. Permite conocer quién debe ser atendido primero, estimar tiempos de espera y mantener un mejor control de los pacientes que esperan atención.

---

## 2. Problema y usuarios

El problema: En hospitales y clínicas, la cantidad de pacientes puede provocar largas esperas, desorganización y dificultad para saber quién debe ser atendido primero.

Cómo se resuelve hoy sin el sistema: El personal registra y organiza a los pacientes mediante sistemas separados, hojas de registro o comunicación directa entre trabajadores. Además, los tiempos de espera no siempre son claros para el paciente.

Usuarios del sistema:

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
|---|---|---|
| Paciente | Conocer su turno y tiempo aproximado de espera | No saber cuánto falta para ser atendido |
| Personal médico | Ver pacientes ordenados según su urgencia | Que un paciente urgente no sea atendido a tiempo |
| Administrador | Supervisar pacientes, tiempos y saturación | Una mala organización de los recursos disponibles |

Un conflicto entre usuarios: Los pacientes quieren ser atendidos lo antes posible, pero el personal médico necesita dar prioridad a los casos más urgentes, aunque hayan llegado después.

---

## 3. Alcance

### Dentro del alcance

- Registrar pacientes que solicitan atención.
- Permitir al personal autorizado asignar y modificar el nivel de urgencia.
- Ordenar automáticamente a los pacientes según su prioridad.
- Calcular y mostrar tiempos aproximados de espera.

### Explícitamente fuera del alcance

- Diagnosticar enfermedades o decidir automáticamente la urgencia médica.
- Administrar medicamentos o tratamientos.
- Gestionar pagos, seguros médicos o facturación.

Por qué queda fuera: El diagnóstico y la decisión del nivel de urgencia requieren conocimiento y responsabilidad médica. MediFlow busca organizar la atención, no sustituir las decisiones del personal de salud. Los pagos y seguros tampoco forman parte del problema principal que busca resolver el sistema.

---

## 4. Tipo de sistema y restricciones

Tipo de sistema: De información.

Por qué es de ese tipo: Porque registra, organiza y muestra información de los pacientes para ayudar al personal a gestionar el orden de atención.

Atributos de calidad que impone:

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
| Disponibilidad | Debe estar accesible durante la atención | El personal pierde acceso a la organización de pacientes |
| Seguridad | Maneja información privada de los pacientes | Podrían exponerse datos personales |
| Precisión | Las prioridades y tiempos deben mostrarse correctamente | Un paciente podría aparecer en una posición incorrecta |

Reglas de negocio que ya identifiqué:

1. El nivel de urgencia solo puede ser asignado o modificado por personal autorizado.
2. Un paciente con mayor nivel de urgencia tiene prioridad aunque haya llegado después.
3. Si cambia el nivel de urgencia de un paciente, el sistema debe reorganizar la fila y recalcular los tiempos de espera.

---

## 5. Ciclo de vida elegido

Modelo elegido: Incremental.

Por qué le conviene a este proyecto: Permite desarrollar MediFlow por partes y probar cada función antes de agregar la siguiente. Por ejemplo, primero se puede desarrollar el registro de pacientes, después la asignación de urgencia y posteriormente el cálculo de tiempos de espera. Esto facilita realizar cambios conforme se pruebe el sistema.

### Alternativas descartadas

Alternativa 1: Cascada.

*Por qué la descarté:* Requiere definir la mayoría de los requisitos desde el inicio y dificulta realizar cambios durante el desarrollo.

Alternativa 2: Modelo en V.

*Por qué la descarté:* Aunque permite realizar pruebas detalladas, resulta más rígido y complejo de lo necesario para el alcance actual de MediFlow.

---

## Antes de entregar

Reviso que el documento cumpla lo siguiente:

- [x] La descripción del apartado 1 se entiende sin ser del área
- [x] Hay al menos dos tipos de usuario con necesidades distintas
- [x] Identifiqué un conflicto real entre usuarios
- [x] El alcance dice qué queda fuera, no solo qué queda dentro
- [x] Las exclusiones son específicas, no genéricas
- [x] Identifiqué el tipo de sistema y al menos dos atributos de calidad
- [x] Anoté al menos tres reglas de negocio no obvias
- [x] Justifiqué el ciclo de vida contra dos alternativas descartadas
- [ ] El documento está en mi repositorio y se puede leer desde el navegador
- [x] Borré todas las instrucciones en cursiva de la plantilla
