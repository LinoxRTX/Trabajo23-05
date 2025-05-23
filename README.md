# Trabajo23-05
trabajo del viernes 23 de mayo

# Sistema de Reservas de Salas

Este repositorio contiene la documentación y diseño del sistema de reservas de salas implementado para el entorno académico. El proyecto abarca el modelado en tres niveles: **casos de uso**, **diagrama de clases**, y **componentes del sistema**.

## 📌 Descripción General

El sistema permite a los estudiantes y administradores realizar y gestionar reservas de salas, con validación de disponibilidad externa y envío de notificaciones mediante correo o SMS. 

Se integran distintos patrones de diseño (Bridge, Adapter, Singleton) y se estructura bajo una arquitectura modular.

---

## 📷 Diagramas del Sistema

### 1. Casos de Uso

Los actores principales son:

- **Estudiante**: puede reservar salas, cancelar, consultar disponibilidad, historial personal y de otros usuarios.
- **Administrador**: aprueba o rechaza reservas, genera reportes y gestiona notificaciones.
- **Sistema de Notificaciones**: se activa en cambios de estado.
- **Sistema Académico Externo**: permite consultar la disponibilidad de salas por medio de un API.

#### Casos:
- Reservar sala
- Cancelar reserva
- Ver estado de solicitudes
- Ver historial de reservas (propias y ajenas)
- Consultar disponibilidad externa
- Eliminar historial
- Aprobar / rechazar reserva (Administrador)
- Notificar por correo cambios relevantes

---

### 2. Diagrama de Clases

Clases principales y relaciones:

- `Usuario`: nombre, correo; métodos `reservarSala()` y `cancelarReserva()`
- `Administrador` (hereda de `Usuario`): puede `aprobarReserva()` y `rechazarReserva()`
- `SistemaReservas`: ejecuta `reservar()` y `cancelar()` solicitudes
- `Sala`: contiene `numero`, `disponible`; métodos de marcaje
- `Reserva`: fecha, estado
- `GestorNotificaciones`: aplica el patrón Bridge para delegar en:
  - `NotificadorCorreo`
  - `NotificadorSMS`
- El `GestorNotificaciones` es un Singleton
- Relación con `SistemaReservas` para notificación al gestionar cambios

---

### 3. Diagrama de Componentes

Organización modular del sistema:

#### 🔧 Servidor Web
- `Sistema de Gestión de Reservas` (Controlador principal)
- `Gestor de Notificaciones`
- `Navegador del Estudiante`:
  - Formulario de Reserva
  - Módulo de Historial

#### 📡 Servicios Externos
- `Sistema Académico Externo`
  - `API de Disponibilidad de Salas` (usado mediante patrón Adapter)
- `Módulo de Notificación Externa`
  - `Notificador SMS`
  - `Notificador Correo`

#### 🗄️ Persistencia
- `Base de Datos Central`: contiene usuarios y reservas

---

## 🛠️ Correcciones Implementadas

1. **Herencia definida entre Usuario y Administrador** en el diagrama de clases.
2. **Uso correcto del patrón Bridge**: `GestorNotificaciones` ya no depende directamente de tipos concretos (`Correo`, `SMS`).
3. **Separación de responsabilidades entre componentes**: navegación de estudiantes, lógica de negocio, persistencia.
4. **Aplicación del patrón Singleton** al `GestorNotificaciones`.
5. **El API externo se accede mediante Adapter**, tal como se indica visualmente.
6. **Unificación de historial en el módulo de vista del estudiante**, eliminando redundancias.
7. **Todos los actores están correctamente conectados a sus casos de uso relevantes**, y se identifican claramente las extensiones lógicas.

---

## 💡 Tecnologías sugeridas para implementación

- Lenguaje: Java, C#, Python
- Frameworks sugeridos: Spring Boot, ASP.NET Core, Flask
- Persistencia: SQLite o archivo JSON simulado
- Arquitectura: MVC + Singleton + Adapter + Bridge

---

