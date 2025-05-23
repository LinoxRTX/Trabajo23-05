# 🎓 Sistema de Reservas de Salas - Documentación Detallada

Este documento proporciona una descripción exhaustiva del diseño y la implementación de un sistema de reservas de salas en entornos académicos. Se abordan los diagramas de casos de uso, clases y componentes, identificando errores comunes y presentando las correcciones aplicadas, fundamentadas en buenas prácticas de modelado UML.

---

## 📌 Índice

1. [Introducción](#introducción)
2. [Objetivos del Sistema](#objetivos-del-sistema)
3. [Diagramas UML](#diagramas-uml)
   - [Casos de Uso](#casos-de-uso)
   - [Diagrama de Clases](#diagrama-de-clases)
   - [Diagrama de Componentes](#diagrama-de-componentes)
4. [Errores Detectados y Correcciones](#errores-detectados-y-correcciones)
5. [Patrones de Diseño Aplicados](#patrones-de-diseño-aplicados)
6. [Flujo de Usuario](#flujo-de-usuario)
7. [Estructura del Repositorio](#estructura-del-repositorio)
8. [Glosario](#glosario)
9. [Contacto](#contacto)

---

## Introducción

El sistema de reservas de salas está diseñado para facilitar la gestión de espacios en instituciones académicas. Permite a estudiantes y administradores interactuar con el sistema para realizar y gestionar reservas, consultar disponibilidad y recibir notificaciones sobre el estado de las mismas.

---

## Objetivos del Sistema

- **Facilitar la reserva de salas** por parte de los estudiantes.
- **Permitir a los administradores** aprobar o rechazar solicitudes de reserva.
- **Integrar sistemas externos** para verificar la disponibilidad de salas en tiempo real.
- **Notificar a los usuarios** sobre el estado de sus reservas mediante correo electrónico o SMS.
- **Mantener un historial** de reservas accesible para los usuarios.

---

## Diagramas UML

### Casos de Uso

Los actores principales del sistema son:

- **Estudiante**:
  - Reservar sala.
  - Cancelar reserva.
  - Consultar disponibilidad de salas.
  - Ver estado de solicitudes.
  - Ver y eliminar historial de reservas.
- **Administrador**:
  - Aprobar o rechazar reservas.
  - Generar reportes.
  - Notificar a los usuarios sobre cambios en las reservas.
- **Sistema Externo**:
  - Proporcionar información sobre la disponibilidad de salas.
- **Sistema de Notificaciones**:
  - Enviar notificaciones por correo electrónico o SMS.

### Diagrama de Clases

Las clases principales del sistema incluyen:

- `Usuario`: clase base con atributos como nombre y correo electrónico.
- `Estudiante`: hereda de `Usuario`; puede realizar y cancelar reservas.
- `Administrador`: hereda de `Usuario`; puede aprobar o rechazar reservas.
- `Sala`: representa una sala con atributos como número y disponibilidad.
- `Reserva`: contiene información sobre la fecha y el estado de la reserva.
- `SistemaReservas`: gestiona la lógica de reserva y cancelación.
- `GestorNotificaciones`: envía notificaciones utilizando diferentes canales.
- `NotificadorCorreo` y `NotificadorSMS`: implementan el envío de notificaciones por correo y SMS, respectivamente.

### Diagrama de Componentes

El sistema se divide en los siguientes componentes:

- **Interfaz de Usuario**:
  - Formulario de reserva.
  - Módulo de historial.
- **Servidor Web**:
  - Controlador de reservas.
  - Lógica de negocio.
- **Gestor de Notificaciones**:
  - Envío de notificaciones por correo y SMS.
- **Sistema Externo**:
  - API para verificar la disponibilidad de salas.
- **Base de Datos**:
  - Almacena información sobre usuarios y reservas.

---

## Errores Detectados y Correcciones

### Casos de Uso

- **Error**: Inclusión de opciones de menú como casos de uso.
  - **Corrección**: Se redefinieron los casos de uso para representar acciones significativas desde la perspectiva del usuario, evitando detalles de implementación de la interfaz.
  - **Justificación**: Los casos de uso deben centrarse en las necesidades del usuario, no en las opciones de la interfaz. :contentReference[oaicite:0]{index=0}

- **Error**: Uso excesivo de relaciones `include` y `extend`.
  - **Corrección**: Se limitaron las relaciones a un máximo de dos niveles y se reorganizaron para reflejar adecuadamente la reutilización de comportamientos comunes.
  - **Justificación**: Un uso excesivo de estas relaciones puede complicar el diagrama y dificultar su comprensión. :contentReference[oaicite:1]{index=1}

### Diagrama de Clases

- **Error**: Falta de visibilidad en atributos y métodos.
  - **Corrección**: Se especificaron las visibilidades (`public`, `private`, `protected`) para todos los miembros de las clases.
  - **Justificación**: La visibilidad es esencial para entender el encapsulamiento y las restricciones de acceso en el diseño orientado a objetos. :contentReference[oaicite:2]{index=2}

- **Error**: Acoplamiento fuerte entre `GestorNotificaciones` y los canales de notificación.
  - **Corrección**: Se aplicó el patrón Bridge para desacoplar la abstracción (`GestorNotificaciones`) de sus implementaciones (`NotificadorCorreo`, `NotificadorSMS`).
  - **Justificación**: Este patrón permite cambiar o añadir nuevos canales de notificación sin modificar el gestor principal.

### Diagrama de Componentes

- **Error**: Falta de claridad en la interacción con sistemas externos.
  - **Corrección**: Se introdujo un adaptador (`AdapterSistemaAcademico`) para interactuar con el sistema externo de disponibilidad de salas.
  - **Justificación**: El patrón Adapter permite que el sistema se comunique con interfaces externas sin depender de sus implementaciones específicas.

- **Error**: Ausencia de patrón de diseño para la arquitectura del sistema.
  - **Corrección**: Se adoptó el patrón MVC (Modelo-Vista-Controlador) para estructurar el sistema.
  - **Justificación**: MVC separa las responsabilidades, facilitando el mantenimiento y la escalabilidad del sistema.

---

## Patrones de Diseño Aplicados

- **Bridge**: Utilizado en el `GestorNotificaciones` para permitir la implementación de múltiples canales de notificación sin modificar la abstracción principal.

- **Adapter**: Implementado en `AdapterSistemaAcademico` para permitir la integración con el sistema externo de disponibilidad de salas.

- **Singleton**: Aplicado en `GestorNotificaciones` para asegurar que solo exista una instancia de este gestor en todo el sistema.

- **MVC (Modelo-Vista-Controlador)**: Adoptado para estructurar la aplicación, separando la lógica de negocio, la interfaz de usuario y el control de flujo.

---

## Flujo de Usuario

1. **Inicio de Sesión**: El estudiante accede al sistema e inicia sesión.
2. **Consulta de Disponibilidad**: El sistema verifica la disponibilidad de salas a través del sistema externo.
3. **Solicitud de Reserva**: El estudiante solicita la reserva de una sala disponible.
4. **Aprobación de Reserva**: El administrador revisa y aprueba o rechaza la solicitud.
5. **Notificación**: El sistema notifica al estudiante sobre el estado de su reserva mediante correo electrónico o SMS.
6. **Gestión de Historial**: El estudiante puede consultar y eliminar su historial de reservas.

---

## Estructura del Repositorio

