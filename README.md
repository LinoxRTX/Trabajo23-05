# Sistema de Reservas de Salas

Este repositorio documenta el diseño de un sistema de reservas de salas en entornos académicos. Se presentan tres vistas clave del sistema:

1. Casos de uso del sistema.
2. Diagrama de clases.
3. Diagrama de componentes.

Incluye el uso de patrones de diseño como **Singleton**, **Bridge**, y **Adapter**, y una estructura orientada a objetos con separación clara de responsabilidades.

---

## 🧩 Diagramas del Sistema

### 1. Casos de Uso

El sistema permite a los siguientes actores interactuar con el sistema:

- **Estudiante**:
  - Reservar sala.
  - Cancelar reserva.
  - Ver estado de solicitudes.
  - Ver historial personal y de otros usuarios.
  - Eliminar historial de reservas.
  - Consultar disponibilidad externa.
- **Administrador**:
  - Aprobar o rechazar reservas.
  - Generar reportes.
  - Notificar cambios por correo.
- **Sistema Externo**:
  - Consulta de disponibilidad a través de un API.
- **Sistema de Notificaciones**:
  - Enviar notificaciones por correo o SMS según el evento.

---

### 2. Diagrama de Clases

#### Clases Principales:

- `Usuario` (nombre, correo): puede `reservarSala()` y `cancelarReserva()`.
- `Administrador` (hereda de `Usuario`): puede `aprobarReserva()` y `rechazarReserva()`.
- `SistemaReservas`: lógica para `reservar()` y `cancelar()`.
- `Sala`: contiene `numero`, `disponible`; tiene métodos `marcarDisponible()` y `marcarOcupado()`.
- `Reserva`: con atributos `fecha` y `estado`.
- `GestorNotificaciones`: tiene `enviarCorreo()` y `enviarSMS()` usando notificadores implementados con el patrón **Bridge**:
  - `NotificadorCorreo`
  - `NotificadorSMS`

---

### 3. Diagrama de Componentes

#### Componentes del Servidor Web:

- `Controlador de Reservas`
- `Sistema de Gestión de Reservas`
- `Gestor de Notificaciones`
- `Navegador del Estudiante`:
  - Formulario de Reserva
  - Módulo de Historial

#### Servicios Externos:

- `Módulo de Notificación Externa`:
  - `Notificador SMS`
  - `Notificador Correo`
- `Sistema Académico Externo`:
  - `API de Disponibilidad de Salas` (acceso mediante patrón **Adapter**)

#### Base de Datos:

- `Base de Datos Central`: usuarios y reservas.

---

## ❌ Errores Detectados en los Diagramas

| Diagrama | Error | Corrección Aplicada | Justificación Técnica |
|---------|-------|----------------------|------------------------|
| Casos de Uso | “Estudiante” y “Administrador” duplican funcionalidades de “Usuario” | Se estableció que ambos heredan de `Usuario` en el diagrama de clases | Promueve reutilización de código y diseño orientado a objetos |
| Casos de Uso | "Notificar cambio por correo" está aislado | Se integró como resultado de `AprobarReserva` y `CancelarReserva` | Es una acción secundaria, dependiente del resultado principal |
| Clases | `GestorNotificaciones` accede directamente a clases concretas | Se aplicó patrón **Bridge** entre gestor y notificadores | Desacopla la lógica de envío de notificaciones del gestor principal |
| Clases | No se especificaba que `GestorNotificaciones` es Singleton | Se implementó como clase Singleton | Evita múltiples instancias no necesarias, mejora consistencia de estado |
| Componentes | No se define cómo se conecta el sistema externo | Se aplica patrón **Adapter** para acceder al API de disponibilidad | Permite la conexión con servicios externos sin alterar la lógica interna |
| Componentes | Estructura del “Servidor Web” es ambigua | Se organiza en vista-controlador-servicio (MVC) | Aclara responsabilidades de cada módulo y favorece mantenibilidad |

---

## ✅ Correcciones Implementadas

1. Se definió correctamente la **herencia entre Usuario y Administrador**.
2. El `GestorNotificaciones` ahora usa **Bridge** para conectarse a distintos canales.
3. Se declaró `GestorNotificaciones` como **Singleton**.
4. La consulta a disponibilidad externa se hace mediante un **Adapter**, tal como se muestra.
5. El **modelo MVC** se respeta en el diseño de componentes: formularios → controlador → lógica → notificaciones/BD.
6. Se organizó mejor el diagrama de clases para reflejar relaciones claras y responsabilidades bien separadas.

---

## 💡 Arquitectura del Proyecto

- **Lenguaje sugerido**: Java / C# / Python
- **Patrones usados**:
  - Bridge: separación de gestor y canales de notificación
  - Adapter: para conectar con APIs externas
  - Singleton: para instancias globales de gestores
- **Base de datos**: simulada o real (según entorno)
- **Organización**: MVC + Módulos externos desacoplados


