# 🎓 Sistema de Reservas de Salas - Proyecto Académico

Este repositorio documenta el diseño de un **Sistema de Reservas de Salas** utilizado en entornos académicos. El sistema permite que estudiantes y administradores gestionen reservas de salas de estudio o reuniones, validando disponibilidad en tiempo real con sistemas externos y notificando los cambios por correo o SMS.

---

## 📘 Índice

- [🎯 Objetivo del Proyecto](#-objetivo-del-proyecto)
- [📐 Modelado del Sistema](#-modelado-del-sistema)
  - [Casos de Uso](#casos-de-uso)
  - [Diagrama de Clases](#diagrama-de-clases)
  - [Diagrama de Componentes](#diagrama-de-componentes)
- [🧱 Arquitectura y Diseño](#-arquitectura-y-diseño)
  - [Patrones de Diseño Aplicados](#patrones-de-diseño-aplicados)
  - [Errores Detectados y Correcciones](#errores-detectados-y-correcciones)
- [🔄 Flujo de Usuario](#-flujo-de-usuario)
- [🗂 Estructura del Repositorio](#-estructura-del-repositorio)
- [🚀 Instrucciones de Despliegue (Simulado)](#-instrucciones-de-despliegue-simulado)
- [📚 Glosario](#-glosario)
- [📬 Contacto](#-contacto)

---

## 🎯 Objetivo del Proyecto

Desarrollar un sistema modular y extensible para la gestión de reservas de salas, que permita:

- Realizar reservas desde un navegador web.
- Validar disponibilidad con fuentes externas.
- Notificar al usuario sobre cambios de estado.
- Ser fácilmente mantenible y ampliable gracias a la aplicación de principios SOLID y patrones de diseño.

---

## 📐 Modelado del Sistema

### Casos de Uso

Los actores principales interactúan con los siguientes casos:

- **Estudiante**:
  - Reservar sala.
  - Cancelar reserva.
  - Ver historial propio y de otros estudiantes.
  - Consultar disponibilidad de salas.
  - Eliminar historial de reservas.
  - Ver estado de solicitudes.
- **Administrador**:
  - Aprobar o rechazar solicitudes de reserva.
  - Generar reportes.
  - Disparar notificaciones al usuario.
- **Sistemas externos**:
  - Sistema Académico: API externa para verificar disponibilidad.
  - Sistema de Notificaciones: Envío de correo y SMS.

### Diagrama de Clases

Clases principales:

- `Usuario`: contiene datos comunes (nombre, correo).
- `Administrador`: hereda de `Usuario`, con métodos para aprobar o rechazar reservas.
- `SistemaReservas`: coordina reservas y cancelaciones.
- `Sala`: atributos como número y disponibilidad.
- `Reserva`: contiene fecha y estado.
- `GestorNotificaciones`: interfaz con métodos para enviar mensajes.
- `NotificadorCorreo` y `NotificadorSMS`: implementaciones del patrón Bridge.

### Diagrama de Componentes

- **Navegador del Estudiante**:
  - Formulario de reserva.
  - Módulo de historial.
- **Sistema de Gestión de Reservas**:
  - Controlador de reservas.
  - Lógica de negocio.
- **Gestor de Notificaciones**:
  - Correo y SMS a través de puente.
- **Módulo de API Externa**:
  - Adaptador para disponibilidad de salas.
- **Base de Datos Central**:
  - Registros de usuarios y reservas.

---

## 🧱 Arquitectura y Diseño

### Patrones de Diseño Aplicados

| Patrón     | Ubicación                             | Propósito |
|------------|----------------------------------------|-----------|
| **Bridge** | `GestorNotificaciones` ↔ Notificadores | Separar abstracción y su implementación (correo/SMS). |
| **Adapter**| API de Disponibilidad de Salas         | Integrar servicios externos sin modificar la lógica del sistema. |
| **Singleton** | `GestorNotificaciones`               | Garantizar una única instancia para control global. |

### Errores Detectados y Correcciones

| Diagrama | Error | Corrección Aplicada | Justificación Técnica |
|---------|-------|----------------------|------------------------|
| Casos de Uso | Actores con acciones duplicadas | Uso de herencia en clases (`Usuario`, `Administrador`) | Favorece la reutilización y claridad |
| Clases | Acoplamiento fuerte a notificadores concretos | Uso del patrón Bridge | Permite agregar nuevos canales sin modificar código existente |
| Clases | Ausencia de control de instancias | `GestorNotificaciones` como Singleton | Evita inconsistencias en notificaciones múltiples |
| Componentes | Falta de integración con API externa | Inclusión de un Adapter para `Sistema Académico Externo` | Encapsula la lógica de integración externa |

---

## 🔄 Flujo de Usuario

1. **Inicio**: el estudiante accede al navegador y abre el formulario de reserva.
2. **Consulta**: se consulta la disponibilidad en el sistema académico externo.
3. **Reserva**: si hay disponibilidad, se guarda en la BD.
4. **Validación**: un administrador aprueba o rechaza la solicitud.
5. **Notificación**: se informa al estudiante por correo o SMS.
6. **Historial**: el estudiante puede consultar y eliminar su historial.

---

## 🗂 Estructura del Repositorio

