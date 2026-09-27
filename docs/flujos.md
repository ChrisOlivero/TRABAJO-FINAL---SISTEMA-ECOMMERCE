# Flujos funcionales del sistema

## 1. Objetivo

Este documento describe los principales flujos funcionales previstos para el sistema de e-commerce. Su objetivo es complementar la definición de módulos y modelo de datos, mostrando cómo interactúan los usuarios con el sistema y cuáles son las reglas de negocio que deben respetarse.

Los flujos se expresan a nivel funcional y servirán como referencia para el diseño de la API REST, la implementación del frontend y la posterior etapa de pruebas.

---

## 2. Actores

### Cliente

El cliente puede registrarse, iniciar sesión, consultar el catálogo, buscar y filtrar productos, visualizar detalles, gestionar su carrito, realizar el checkout y consultar sus pedidos.

### Administrador

El administrador puede acceder al panel administrativo, gestionar productos y categorías, modificar stock, consultar pedidos y actualizar sus estados.

---

# 3. Flujo de registro

### Objetivo

Permitir que una persona cree una cuenta para utilizar las funcionalidades que requieren autenticación.

### Flujo principal

1. El usuario accede a la pantalla de registro.
2. Completa sus datos personales y credenciales.
3. El frontend envía la información al backend.
4. El backend valida los datos recibidos.
5. Se verifica que el correo electrónico no esté registrado.
6. La contraseña se almacena utilizando un mecanismo seguro de hash.
7. Se crea el usuario con rol `CLIENTE`.
8. El sistema informa que el registro fue realizado correctamente.

### Validaciones principales

- Campos obligatorios.
- Formato válido del correo electrónico.
- Correo electrónico único.
- Reglas de contraseña definidas por el sistema.

---

# 4. Flujo de inicio de sesión

### Flujo principal

1. El usuario ingresa correo y contraseña.
2. El frontend envía las credenciales al backend.
3. El backend busca el usuario correspondiente.
4. Se valida la contraseña.
5. Si las credenciales son correctas, se genera un token JWT.
6. El cliente utiliza el token para acceder a recursos protegidos.
7. El sistema determina las operaciones permitidas según el rol del usuario.

### Autorización

Se contemplan inicialmente dos roles:

- `CLIENTE`
- `ADMIN`

Las operaciones administrativas deben estar protegidas en el backend y no solamente mediante restricciones visuales en el frontend.

---

# 5. Flujo de consulta del catálogo

### Flujo principal

1. El cliente accede al catálogo.
2. El frontend solicita los productos disponibles al backend.
3. El backend obtiene la información correspondiente desde la base de datos.
4. Se muestran los productos junto con su información relevante.
5. El cliente puede utilizar búsqueda y filtros.
6. El cliente puede seleccionar un producto para consultar su detalle.

### Información del producto

La propuesta contempla información como:

- Marca.
- Modelo.
- Almacenamiento.
- RAM.
- Color.
- Condición.
- Precio.
- Stock/disponibilidad.
- Imagen.

Los atributos específicos de teléfonos podrán ser opcionales para productos que no los necesiten, como accesorios.

---

# 6. Flujo de carrito

### Flujo principal

1. El cliente selecciona un producto.
2. Indica la cantidad deseada.
3. El producto se agrega al carrito.
4. El cliente puede modificar cantidades o eliminar productos.
5. El sistema calcula el total del carrito.
6. El cliente puede continuar hacia el checkout.

### Regla importante de stock

El agregado de un producto al carrito **no reserva stock**.

El stock se vuelve a verificar durante el checkout antes de confirmar la operación. Esta decisión evita mantener unidades bloqueadas durante un período indefinido mientras un usuario mantiene productos en su carrito.

---

# 7. Flujo de checkout

### Objetivo

Completar la información necesaria para generar un pedido y procesar el pago.

### Flujo principal

1. El cliente confirma que desea realizar la compra.
2. El sistema obtiene el contenido actual del carrito.
3. El backend vuelve a verificar el stock de cada producto.
4. Si alguna cantidad no está disponible, se informa el problema y no se genera el pedido confirmado.
5. El cliente completa o confirma sus datos.
6. Selecciona el tipo de entrega:
   - Retiro.
   - Envío.
7. Si corresponde, informa la dirección de entrega.
8. Selecciona la forma de pago disponible.
9. El sistema procesa o registra el resultado del pago según la integración definida para el proyecto.
10. Si el pago es aprobado, se genera/actualiza el pedido como operación confirmada.
11. Se descuenta el stock correspondiente.
12. Se conserva el precio utilizado al momento de la compra.
13. Se muestra la confirmación de la operación.

### Regla de consistencia

El descuento de stock debe producirse solamente cuando el pago haya sido aprobado, de acuerdo con la regla definida en la propuesta inicial.

---

# 8. Flujo de pago

El proyecto contempla un flujo de pago online, pero la propuesta inicial no define todavía un proveedor concreto de pagos.

Por este motivo, en esta etapa se define el comportamiento funcional sin acoplar el diseño a un proveedor específico.

### Resultado esperado

El procesamiento debe permitir distinguir al menos entre:

- Pago pendiente.
- Pago aprobado.
- Pago rechazado.

La implementación concreta del proveedor podrá definirse posteriormente como una decisión técnica, considerando el alcance y los tiempos del TFI.

### Pago aprobado

Cuando el pago es aprobado:

- El pedido continúa su proceso.
- Se descuenta el stock.
- Se conserva el precio histórico de cada detalle.

### Pago rechazado

Cuando el pago es rechazado:

- No debe descontarse el stock.
- La operación no debe quedar confirmada como compra exitosa.
- El cliente debe recibir información sobre el resultado.

---

# 9. Flujo de creación del pedido

### Datos principales

El pedido debe conservar información suficiente para representar la operación realizada, incluyendo:

- Usuario.
- Fecha.
- Estado.
- Total.
- Forma de pago.
- Estado del pago.
- Tipo de entrega.
- Dirección de entrega cuando corresponda.
- Detalles del pedido.

Cada detalle conserva la cantidad y el precio unitario utilizado en el momento de la compra.

### Precio histórico

El precio almacenado en el detalle del pedido representa el precio aplicado durante la compra. De esta forma, una modificación posterior del precio del producto no altera el importe histórico de pedidos ya realizados.

---

# 10. Flujo de stock

### Consulta

El stock disponible se muestra como parte de la información del producto.

### Verificación durante checkout

Antes de completar la compra, el backend verifica nuevamente las cantidades disponibles.

### Descuento

El stock se descuenta cuando el pago es aprobado.

### Cancelación

Cuando una operación cancelada implique la devolución de unidades, el stock deberá restaurarse de acuerdo con las reglas definidas para el estado del pedido.

### Administración

El administrador puede modificar el stock desde el módulo de administración.

No se contempla inicialmente un sistema completo de movimientos de inventario. Esta funcionalidad puede evaluarse como una evolución posterior si el alcance del proyecto lo permite.

---

# 11. Flujo de estados del pedido

Los estados iniciales previstos son:

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

La administración del pedido permitirá actualizar su estado según el avance de la preparación y entrega.

También se contempla la posibilidad de cancelación en los casos definidos por las reglas del sistema.

### Consideración

Las transiciones concretas y las condiciones para cada cambio deberán quedar implementadas y validadas en el backend para evitar modificaciones de estado inválidas.

---

# 12. Flujo de historial de pedidos del cliente

1. El cliente inicia sesión.
2. Accede a la sección de pedidos.
3. El frontend solicita sus pedidos al backend.
4. El backend obtiene únicamente los pedidos asociados al usuario autenticado.
5. Se muestran los pedidos y sus estados.
6. El cliente puede consultar el detalle de cada operación.

### Regla de seguridad

Un cliente no debe poder consultar pedidos pertenecientes a otro usuario, aunque conozca el identificador del pedido.

La validación debe realizarse en el backend.

---

# 13. Flujo administrativo de productos

### Alta

1. El administrador accede al panel.
2. Selecciona la opción para crear un producto.
3. Completa los datos.
4. El backend valida la información.
5. Se registra el producto.
6. El producto queda disponible según su configuración.

### Modificación

1. El administrador selecciona un producto.
2. Modifica los datos permitidos.
3. El backend valida la información.
4. Se actualiza el producto.

### Baja

La propuesta contempla la administración del catálogo. Para preservar información histórica de pedidos, se prioriza el uso de eliminación lógica cuando corresponda en lugar de eliminar físicamente registros que estén relacionados con operaciones anteriores.

---

# 14. Flujo administrativo de categorías

1. El administrador accede al módulo de categorías.
2. Puede crear una categoría.
3. Puede modificar sus datos.
4. Puede deshabilitar/eliminar lógicamente una categoría según las reglas implementadas.
5. El backend valida las operaciones antes de persistirlas.

---

# 15. Flujo administrativo de pedidos

1. El administrador inicia sesión.
2. Accede al panel de pedidos.
3. El sistema muestra los pedidos registrados.
4. El administrador consulta el detalle de un pedido.
5. Puede actualizar el estado permitido.
6. El backend valida la transición.
7. El nuevo estado queda persistido.
8. Si la operación implica cancelación y corresponde restaurar stock, se aplica la regla definida para esa situación.

---

# 16. Reglas transversales

Los flujos anteriores comparten las siguientes reglas:

- Las validaciones críticas deben ejecutarse en el backend.
- Las operaciones administrativas requieren autorización mediante rol.
- Los datos enviados por el cliente no deben considerarse confiables sin validación.
- El stock se verifica nuevamente antes de completar una compra.
- El stock no se reserva al agregar productos al carrito.
- El stock se descuenta cuando el pago es aprobado.
- Los pedidos conservan el precio aplicado durante la compra.
- Un usuario solamente puede consultar sus propios pedidos.
- Las operaciones de catálogo deben preservar la información histórica necesaria para los pedidos existentes.

---

# 17. Relación con los módulos

| Flujo | Módulos involucrados |
|---|---|
| Registro | Autenticación y usuarios |
| Inicio de sesión | Autenticación y seguridad |
| Catálogo | Catálogo |
| Carrito | Carrito |
| Checkout | Checkout y pago, stock, pedidos |
| Pago | Checkout y pago |
| Creación de pedido | Pedidos, stock |
| Historial | Pedidos, autenticación |
| Gestión de productos | Administración de catálogo |
| Gestión de categorías | Administración de catálogo |
| Gestión de stock | Stock / administración |
| Gestión de estados | Administración de pedidos |

---

# 18. Evolución prevista

Estos flujos representan el diseño funcional correspondiente a la etapa de diseño del TFI. Durante la implementación podrán precisarse aspectos técnicos que todavía no están definidos, especialmente la integración concreta del pago online y determinadas reglas de transición de estados.

Cualquier modificación relevante deberá mantenerse alineada con el alcance aprobado y documentarse como una decisión técnica del proyecto.
