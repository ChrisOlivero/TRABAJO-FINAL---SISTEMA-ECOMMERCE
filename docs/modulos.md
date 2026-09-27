# Módulos del sistema

## 1. Objetivo

Este documento define los módulos funcionales propuestos para el desarrollo del sistema de e-commerce del Trabajo Final Integrador.

La división se realiza por responsabilidades funcionales y no por entidades de base de datos. El objetivo es establecer un alcance claro para la segunda etapa del proyecto y facilitar posteriormente la organización del backend, frontend, APIs y pruebas.

El listado toma como base la propuesta inicial del proyecto y las funcionalidades previstas para los perfiles Cliente y Administrador.

---

## 2. Criterio de organización

Los módulos se agrupan según las principales capacidades que deberá ofrecer el sistema:

- funcionalidades disponibles para el cliente;
- funcionalidades administrativas;
- procesos transversales que afectan a distintos módulos.

La implementación priorizará primero las funcionalidades esenciales del MVP. Las funcionalidades secundarias podrán incorporarse posteriormente si el tiempo disponible y la evolución del proyecto lo permiten.

---

## 3. Módulos funcionales

### 3.1. Autenticación y usuarios

**Objetivo:** permitir el registro, inicio de sesión y gestión de acceso de los usuarios.

**Funcionalidades previstas:**

- Registro de nuevos clientes.
- Inicio de sesión.
- Generación y utilización de autenticación mediante JWT.
- Identificación del usuario autenticado.
- Gestión del rol del usuario.
- Protección de operaciones según permisos.
- Restricción del acceso de un cliente a los pedidos de otros usuarios.

**Roles previstos:**

- `CLIENTE`
- `ADMIN`

**Prioridad:** P0.

---

### 3.2. Catálogo de productos

**Objetivo:** permitir consultar los productos disponibles para la venta.

**Funcionalidades previstas:**

- Listado de productos.
- Consulta por categorías.
- Búsqueda de productos.
- Filtros según los atributos definidos para el catálogo.
- Visualización del detalle de un producto.
- Consulta de precio y disponibilidad.
- Diferenciación entre productos nuevos y usados.

Para los teléfonos se contemplan atributos como marca, modelo, almacenamiento, RAM, color y condición. Los accesorios podrán utilizar únicamente los atributos que correspondan a su tipo.

**Prioridad:** P0.

---

### 3.3. Carrito de compras

**Objetivo:** permitir al cliente seleccionar productos antes de iniciar el checkout.

**Funcionalidades previstas:**

- Agregar productos al carrito.
- Visualizar productos seleccionados.
- Modificar cantidades.
- Eliminar productos.
- Visualizar subtotal y total del carrito.
- Continuar hacia el proceso de checkout.

**Regla importante:** agregar un producto al carrito no implica reservar stock. La disponibilidad deberá verificarse nuevamente durante el checkout.

**Prioridad:** P0.

---

### 3.4. Checkout y pago

**Objetivo:** completar los datos necesarios para generar una compra y procesar su pago.

**Funcionalidades previstas:**

- Revisión del contenido del carrito.
- Ingreso o confirmación de datos del cliente.
- Selección entre retiro y envío.
- Registro de los datos de entrega cuando corresponda.
- Selección del medio de pago disponible.
- Validación final de stock.
- Procesamiento del pago mediante la integración que se defina durante la implementación.
- Confirmación del resultado del pago.
- Generación del pedido cuando corresponda.

**Regla de stock:** el stock no se descuenta al agregar productos al carrito. El descuento se realizará únicamente cuando el pago haya sido aprobado.

**Prioridad:** P0.

---

### 3.5. Pedidos

**Objetivo:** gestionar las compras realizadas por los clientes y su ciclo de vida.

**Funcionalidades previstas:**

- Creación del pedido luego de completar correctamente el checkout.
- Registro de los productos comprados.
- Conservación del precio unitario utilizado al momento de la compra.
- Cálculo y almacenamiento del total del pedido.
- Registro de fecha y forma de pago.
- Registro del tipo de entrega.
- Consulta del estado del pedido.
- Consulta del historial de pedidos por parte del cliente.
- Gestión del estado del pedido por parte del administrador.
- Cancelación según las reglas definidas para el proyecto.

**Estados previstos inicialmente:**

`CREADO → CONFIRMADO → PREPARANDO → LISTO → ENTREGADO`

El estado `CANCELADO` se contempla como estado alternativo para los casos en los que corresponda cancelar el pedido.

**Prioridad:** P0.

---

### 3.6. Gestión de stock

**Objetivo:** controlar la cantidad disponible de productos para evitar ventas superiores al stock existente.

**Funcionalidades previstas:**

- Consulta de stock disponible.
- Validación de stock durante el checkout.
- Descuento de stock luego de un pago aprobado.
- Reposición del stock cuando corresponda por cancelación.
- Modificación manual del stock por parte del administrador.

No se contempla inicialmente un sistema completo de movimientos de inventario o trazabilidad de cada modificación de stock.

**Prioridad:** P0.

---

### 3.7. Administración de productos

**Objetivo:** permitir al administrador mantener actualizado el catálogo.

**Funcionalidades previstas:**

- Alta de productos.
- Consulta de productos.
- Modificación de productos.
- Baja lógica o desactivación de productos.
- Actualización de precio.
- Actualización de stock.
- Actualización de disponibilidad.
- Asociación de productos con categorías.

**Prioridad:** P0.

---

### 3.8. Administración de categorías

**Objetivo:** permitir al administrador organizar el catálogo mediante categorías.

**Funcionalidades previstas:**

- Alta de categorías.
- Consulta de categorías.
- Modificación de categorías.
- Baja lógica o desactivación de categorías.
- Asociación de productos a una categoría.

**Prioridad:** P0.

---

### 3.9. Administración de pedidos

**Objetivo:** proporcionar al administrador herramientas para consultar y gestionar los pedidos realizados.

**Funcionalidades previstas:**

- Visualización de pedidos.
- Consulta del detalle de cada pedido.
- Consulta de los datos asociados al cliente.
- Actualización del estado del pedido.
- Gestión de cancelaciones cuando corresponda.
- Aplicación de las reglas de actualización de stock asociadas a la cancelación.

**Prioridad:** P0.

---

## 4. Funcionalidades transversales

Además de los módulos funcionales, el sistema contará con responsabilidades que atraviesan diferentes partes de la aplicación.

### 4.1. Seguridad y autorización

Incluye:

- autenticación mediante JWT;
- autorización basada en roles;
- protección de endpoints administrativos;
- validación de pertenencia de los pedidos al usuario autenticado;
- validación de datos recibidos desde el frontend.

### 4.2. Validaciones y manejo de errores

Incluye:

- validación de datos de entrada;
- validación de reglas de negocio;
- control de stock;
- control de estados de pedido;
- manejo centralizado de excepciones en el backend;
- respuestas HTTP coherentes para errores de la API.

### 4.3. Persistencia

La persistencia estará implementada mediante una base de datos relacional y JPA/Hibernate.

La organización del backend seguirá, como criterio general, la separación:

`Controller → Service → Repository → Database`

Los DTOs se utilizarán cuando aporten una separación clara entre los datos expuestos por la API y las entidades del dominio.

---

## 5. Priorización

| Prioridad | Módulos |
|---|---|
| **P0 - Esencial** | Autenticación y usuarios, catálogo, carrito, checkout y pago, pedidos, stock, administración de productos, administración de categorías, administración de pedidos |
| **Transversal P0** | Seguridad, autorización, validaciones, manejo de errores y persistencia |
| **P1 - Sujeto a tiempo y alcance** | Mejoras o funcionalidades complementarias que no sean necesarias para completar el flujo principal de compra |

La prioridad P1 no incorpora funcionalidades concretas adicionales en esta etapa para evitar ampliar el alcance sin aprobación o necesidad del proyecto.

---

## 6. Flujo funcional principal

El flujo principal esperado es:

`Registro / Login → Catálogo → Detalle de producto → Carrito → Checkout → Validación de stock → Pago → Generación del pedido → Preparación → Entrega`

El administrador interviene principalmente en:

`Login → Administración de catálogo / stock → Consulta de pedidos → Actualización del estado de pedidos`

---

## 7. Relación con el modelo de datos

Los módulos utilizarán principalmente las entidades definidas en el modelo de datos propuesto:

- **Autenticación y usuarios:** `Usuario`
- **Catálogo:** `Producto`, `Categoria`
- **Carrito:** gestión de selección de productos antes del pedido
- **Checkout y pago:** `Pedido`
- **Pedidos:** `Pedido`, `DetallePedido`, `Usuario`
- **Stock:** `Producto`
- **Administración de productos:** `Producto`, `Categoria`
- **Administración de categorías:** `Categoria`
- **Administración de pedidos:** `Pedido`, `DetallePedido`, `Producto`

El carrito no requiere necesariamente una entidad persistente en la base de datos dentro del alcance actual; su estrategia de implementación se definirá durante el desarrollo del frontend y backend.

---

## 8. Evolución respecto de la base anterior

La división modular conserva las funcionalidades centrales desarrolladas en trabajos anteriores, pero las organiza para el contexto del e-commerce del TFI.

La evolución prevista incluye:

- pasar de una aplicación centrada en la persistencia JPA a una arquitectura web basada en API REST;
- incorporar autenticación y autorización mediante JWT;
- separar responsabilidades mediante Controller, Service y Repository;
- formalizar el flujo de checkout y pago;
- aplicar reglas explícitas para el descuento y reposición de stock;
- conservar el precio histórico de cada producto dentro del pedido;
- incorporar operaciones administrativas diferenciadas de las operaciones del cliente;
- adaptar el catálogo al contexto de teléfonos nuevos/usados y accesorios.

---

## 9. Alcance de esta definición

Este documento representa la **propuesta de módulos para la etapa de diseño del TFI**. La implementación podrá ajustar detalles internos siempre que se mantengan los objetivos, reglas de negocio y alcance acordados con el equipo y el tutor.
