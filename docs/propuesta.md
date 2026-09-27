# Propuesta del proyecto

## Trabajo Final Integrador — Tecnicatura Universitaria en Programación

### 1. Descripción del proyecto

El proyecto consiste en el desarrollo de un sistema web de comercio electrónico orientado a un emprendimiento de pequeña escala dedicado a la comercialización de celulares nuevos, celulares usados y accesorios tecnológicos.

Actualmente, el proceso de comercialización se plantea principalmente mediante redes sociales y WhatsApp. La propuesta busca centralizar el catálogo, el stock, los clientes y los pedidos mediante una plataforma web propia.

El sistema será desarrollado como un producto de software aplicable a un contexto real o simulado, priorizando una solución mantenible, modular y adaptable al contexto del emprendimiento.

---

## 2. Problemática

El modelo actual de comercialización puede generar dificultades a medida que aumenta la cantidad de productos, consultas y ventas.

Entre las principales problemáticas identificadas se encuentran:

- Catálogo distribuido entre diferentes publicaciones y canales.
- Consultas manuales sobre disponibilidad y precios.
- Control manual del stock.
- Registro y seguimiento de pedidos mediante diferentes medios.
- Falta de un catálogo centralizado.
- Dificultad para consultar de manera organizada el estado de los pedidos.
- Dependencia de diferentes herramientas para administrar el proceso de venta.
- Mayor posibilidad de errores a medida que aumenta el volumen de operaciones.

---

## 3. Objetivo general

Desarrollar una plataforma web propia que permita centralizar y organizar el proceso de comercialización, facilitando tanto la experiencia de compra del cliente como la gestión interna del emprendimiento.

El sistema estará orientado inicialmente a un comercio de pequeña escala y será desarrollado de manera modular, priorizando una solución mantenible y adaptable a las necesidades concretas del negocio.

---

## 4. Actores

### Cliente

El cliente podrá:

- Registrarse e iniciar sesión.
- Consultar el catálogo.
- Buscar y filtrar productos.
- Consultar el detalle de los productos.
- Agregar productos al carrito.
- Modificar cantidades.
- Realizar el checkout.
- Seleccionar retiro o envío.
- Realizar el pago online.
- Generar pedidos.
- Consultar sus pedidos y su estado.

### Administrador

El administrador podrá:

- Iniciar sesión.
- Gestionar productos.
- Gestionar categorías.
- Gestionar stock.
- Consultar pedidos.
- Actualizar estados de pedidos.
- Cancelar pedidos.
- Realizar ajustes de stock cuando sea necesario.

---

## 5. Catálogo

El sistema estará orientado inicialmente a:

- Celulares nuevos.
- Celulares usados.
- Accesorios tecnológicos.

Los productos podrán contar con información específica según corresponda:

- Marca.
- Modelo.
- Capacidad de almacenamiento.
- Memoria RAM.
- Color.
- Condición del producto.
- Precio.
- Stock.

La estructura definitiva del catálogo y sus posibles variantes será definida durante la etapa de diseño.

---

## 6. Proceso de compra

El flujo principal propuesto es:

```text
Catálogo
   ↓
Detalle del producto
   ↓
Carrito
   ↓
Datos del cliente
   ↓
Retiro o envío
   ↓
Pago online
   ↓
Confirmación
   ↓
Generación del pedido
```

El checkout contempla:

- Carrito de compra.
- Datos del cliente.
- Selección entre retiro y envío a domicilio.
- Información necesaria para el envío.
- Pago online.
- Confirmación de la operación.
- Generación del pedido.

Inicialmente quedan fuera del alcance las integraciones con sistemas externos de facturación electrónica y logística, salvo que posteriormente se determine que resultan necesarias.

---

## 7. Gestión de stock

Agregar un producto al carrito no reservará stock.

Antes de finalizar una compra, el backend deberá verificar nuevamente la disponibilidad.

El flujo definido es:

```text
Agregar al carrito
        ↓
Checkout
        ↓
Verificación de stock
        ↓
Procesamiento del pago
        ↓
¿Pago aprobado?
   ├── No → No se modifica el stock
   │
   └── Sí
        ↓
   Descuento de stock
        ↓
   Confirmación del pedido
```

La verificación del stock deberá realizarse en el backend para evitar inconsistencias ante operaciones simultáneas.

No se contempla inicialmente un sistema completo de movimientos de inventario. El administrador podrá realizar ajustes de stock cuando sea necesario.

---

## 8. Pedidos

Los pedidos tendrán inicialmente los siguientes estados:

```text
CREADO
   ↓
CONFIRMADO
   ↓
PREPARANDO
   ↓
LISTO
   ↓
ENTREGADO
```

El administrador será responsable de gestionar la evolución del estado del pedido.

También se permitirá la cancelación de pedidos por parte del administrador.

Una cancelación podrá realizarse según las reglas definidas para cada estado y, cuando corresponda, deberá restituir el stock descontado.

---

## 9. Precios

El precio aplicado durante una compra deberá conservarse dentro del pedido.

Por lo tanto, una modificación posterior del precio de un producto no deberá alterar el importe histórico de pedidos ya realizados.

Esto se resolverá almacenando el precio correspondiente en el detalle del pedido.

---

## 10. Seguridad

El sistema contará con dos roles:

- `CLIENTE`
- `ADMIN`

Se propone implementar autenticación mediante JWT y autorización basada en roles.

Entre las reglas de seguridad previstas:

- Un cliente no puede crear ni modificar productos.
- Un cliente no puede modificar categorías.
- Un cliente no puede modificar el stock.
- Un cliente no puede modificar estados administrativos.
- Un usuario no puede consultar pedidos pertenecientes a otro usuario.
- Determinadas operaciones sobre pedidos estarán disponibles únicamente para administradores.
- Los datos recibidos por la API serán validados en backend.

---

## 11. MVP

### P0 — Funcionalidades obligatorias

#### Cliente

- Registro.
- Inicio de sesión.
- Catálogo.
- Categorías.
- Búsqueda y filtrado básico.
- Detalle de producto.
- Carrito.
- Datos de compra.
- Selección de retiro o envío.
- Pago online.
- Generación del pedido.
- Consulta de pedidos.
- Consulta del estado del pedido.

#### Administrador

- Inicio de sesión.
- Gestión de productos.
- Gestión de categorías.
- Gestión de stock.
- Consulta de pedidos.
- Actualización de estados.
- Cancelación de pedidos.

### P1 — Funcionalidades deseables

Las funcionalidades P1 serán evaluadas una vez completado el núcleo del sistema.

Entre las posibles extensiones:

- Filtros avanzados.
- Comparación de productos.
- Estadísticas administrativas.
- Mejoras en la gestión de inventario.
- Nuevas funcionalidades para la experiencia del cliente.

### Fuera de alcance inicial

- Facturación electrónica.
- Integraciones externas de logística.
- Aplicación móvil nativa.
- Marketplace.
- Gestión de múltiples sucursales.
- Sistema avanzado de movimientos de inventario.
- Funcionalidades orientadas a grandes volúmenes de usuarios.

---

## 12. Arquitectura propuesta

El sistema seguirá una arquitectura full stack separando frontend, backend y base de datos.

```text
┌─────────────────────────────┐
│          Frontend           │
│       React + TypeScript    │
└──────────────┬──────────────┘
               │
               │ HTTP / JSON
               ▼
┌─────────────────────────────┐
│          Backend            │
│         Spring Boot         │
├─────────────────────────────┤
│ Controllers                 │
│ Services                    │
│ Repositories                │
│ DTOs                        │
│ Validation                  │
│ Security                    │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│       Base de datos SQL     │
└─────────────────────────────┘
```

La organización lógica del backend seguirá:

```text
Controller
    ↓
Service
    ↓
Repository
    ↓
Database
```

---

## 13. Tecnologías propuestas

### Backend

- Java.
- Spring Boot.
- Spring Web.
- Spring Data JPA.
- Hibernate.
- Spring Security.
- DTOs.
- Manejo global de excepciones.

### Frontend

- TypeScript.
- HTML.
- CSS.

### Base de datos

- Base de datos relacional SQL.
- JPA / Hibernate.

### Herramientas

- Git.
- GitHub.
- Postman.

---

## 14. Dimensión inicial

El sistema será planteado inicialmente para un emprendimiento de pequeña escala.

Como escenario de referencia:

- Aproximadamente 15 productos o variantes.
- Aproximadamente 10 clientes.
- 1 administrador.
- Volumen reducido de pedidos diarios.

Estos valores representan el escenario inicial de prueba y permiten dimensionar la solución de acuerdo con el problema planteado.

---

## 15. Plan de desarrollo

### Etapa 1 — Backend y base de datos

- Configuración de Spring Boot.
- Configuración de la base de datos.
- Entidades.
- Repositories.
- API REST inicial.

### Etapa 2 — Autenticación y seguridad

- Registro.
- Login.
- JWT.
- Roles.
- Protección de endpoints.
- Validaciones.

### Etapa 3 — Catálogo

- Productos.
- Categorías.
- Búsqueda.
- Filtros.
- Detalle.
- Gestión administrativa.

### Etapa 4 — Carrito y pedidos

- Carrito.
- Checkout.
- Pago.
- Verificación de stock.
- Generación de pedidos.
- Historial.
- Estados.

### Etapa 5 — Administración

- Gestión de productos.
- Gestión de categorías.
- Stock.
- Pedidos.
- Cancelaciones.

### Etapa 6 — Integración, pruebas y despliegue

- Integración frontend/backend.
- Pruebas funcionales.
- Pruebas de seguridad.
- Corrección de errores.
- Despliegue online.
- Documentación.
- Video explicativo.

---

## 16. Despliegue

El proyecto contempla el despliegue online de al menos uno de sus componentes principales, de acuerdo con los requisitos establecidos para el Trabajo Final Integrador.

La plataforma y estrategia de despliegue serán definidas durante el desarrollo.

---

## 17. Equipo

| Integrante | Rol |
|---|---|
| Christian Emmanuel Olivero | Desarrollo |
| Marcos Rios | Desarrollo |

### Tutor

**Oscar Londero**

---

## 18. Hoja de ruta académica

| Etapa | Entrega | Fecha límite |
|---|---|---:|
| 1 | Propuesta de proyecto y repositorio | 30/08/2026 |
| 2 | Diseño, base de datos y módulos | 27/09/2026 |
| 3 | Desarrollo, documentación y despliegue | 14/11/2026 |
| 4 | Finalización del cursado | 21/11/2026 |
| 5 | Defensa oral | Mesa de examen |

---

## 19. Criterios de éxito

El proyecto deberá permitir:

- Centralizar el catálogo.
- Gestionar productos y categorías.
- Gestionar stock.
- Registrar clientes.
- Gestionar pedidos.
- Permitir el flujo de compra definido.
- Aplicar autenticación y autorización.
- Mantener la información histórica de los pedidos.
- Contar con una arquitectura mantenible.
- Disponer de documentación técnica.
- Contar con al menos un componente desplegado online para la entrega final.

---

## 20. Estado y evolución del proyecto

La propuesta constituye la base inicial del proyecto. Durante las siguientes etapas se desarrollarán y documentarán la arquitectura, el modelo de datos, los módulos y las decisiones técnicas.

Las decisiones podrán evolucionar durante el desarrollo cuando sea necesario, manteniendo el alcance alineado con los requisitos del Trabajo Final Integrador y con las definiciones aprobadas por el tutor.
