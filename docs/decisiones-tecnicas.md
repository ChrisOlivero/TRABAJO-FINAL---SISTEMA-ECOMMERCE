# Decisiones técnicas

## 1. Objetivo

Este documento registra las principales decisiones técnicas propuestas para el Trabajo Final Integrador. Su objetivo es explicar qué alternativa se plantea utilizar, por qué resulta adecuada para el alcance del proyecto y qué impacto tiene sobre el desarrollo y el mantenimiento.

Las decisiones corresponden al estado actual del diseño y podrán ajustarse durante la implementación si aparecen restricciones técnicas o si el tutor solicita modificaciones.

## 2. Arquitectura por capas

### Decisión

Adoptar una arquitectura backend separada en:

**Controller → Service → Repository → Database**

### Motivo

Esta separación permite distribuir responsabilidades y evitar que los controladores concentren reglas de negocio o acceso directo a la persistencia.

### Alternativas consideradas

- Concentrar lógica en los controladores: menor cantidad de clases inicialmente, pero mayor acoplamiento y dificultad para mantener y probar el sistema.
- Utilizar una arquitectura más compleja, como microservicios: aporta separación de servicios, pero no resulta necesaria para el tamaño y alcance planteado.

### Criterio adoptado

Se prioriza una arquitectura modular y mantenible sin introducir complejidad que no aporte valor al TFI.

---

## 3. Spring Boot para el backend

### Decisión

Utilizar Java con Spring Boot para desarrollar la API REST.

### Motivo

El proyecto requiere una API, persistencia, validaciones, seguridad y separación de responsabilidades. Spring Boot permite organizar estos componentes dentro de una estructura coherente con los contenidos trabajados durante la carrera.

### Impacto

El backend podrá evolucionar desde la implementación anterior hacia una aplicación web con servicios HTTP, manteniendo una estructura clara por responsabilidades.

---

## 4. React + TypeScript para el frontend

### Decisión

La arquitectura objetivo del frontend será React con TypeScript.

### Motivo

El sistema necesita varias vistas, navegación, formularios, catálogo, carrito, checkout y funcionalidades diferenciadas para cliente y administrador. React permite organizar la interfaz mediante componentes reutilizables y TypeScript aporta tipado estático.

### Evolución

La base actual contiene páginas y servicios desarrollados con TypeScript/Vite. La migración hacia React se considera parte de la evolución del proyecto y deberá realizarse de manera progresiva, reutilizando la lógica útil y corrigiendo incompatibilidades detectadas.

---

## 5. JPA / Hibernate para persistencia

### Decisión

Utilizar JPA con Hibernate como implementación de persistencia del backend.

### Motivo

El proyecto posee entidades relacionadas como usuarios, categorías, productos, pedidos y detalles de pedido. El mapeo objeto-relacional permite representar estas relaciones desde el modelo Java y reducir código repetitivo de acceso a datos.

### Consideración

El uso de JPA no elimina la necesidad de comprender SQL y el modelo relacional. Las relaciones, restricciones, índices y consultas deberán diseñarse de acuerdo con las necesidades reales del sistema.

---

## 6. DTOs para la API

### Decisión

Utilizar DTOs cuando sea necesario separar los datos expuestos por la API de las entidades de persistencia.

### Motivo

Evita exponer directamente el modelo interno de JPA y permite controlar qué información recibe o devuelve cada operación.

### Ejemplos de uso

- Registro e inicio de sesión.
- Datos de usuario.
- Productos para catálogo.
- Creación de pedidos.
- Respuestas de pedidos.
- Operaciones administrativas.

### Alternativa descartada como criterio general

Exponer todas las entidades JPA directamente desde los controladores simplificaría inicialmente el código, pero aumentaría el acoplamiento entre persistencia y API.

---

## 7. Autenticación mediante JWT

### Decisión

La autenticación se plantea mediante tokens JWT.

### Motivo

El sistema necesita identificar usuarios y proteger operaciones diferenciando clientes y administradores.

### Autorización

La autenticación y autorización se resolverán en backend. El frontend puede ocultar o mostrar funcionalidades según el rol, pero esa lógica visual no reemplaza la protección de los endpoints.

Los roles definidos para el diseño son:

- `CLIENTE`
- `ADMIN`

### Impacto

Los endpoints administrativos deberán verificar el rol correspondiente y las operaciones sobre pedidos deberán respetar la pertenencia del pedido al usuario autenticado.

---

## 8. Base de datos relacional

### Decisión

Utilizar una base de datos relacional SQL.

### Motivo

El dominio presenta relaciones claras entre usuarios, productos, categorías, pedidos y detalles de pedido. Las restricciones de integridad y las relaciones entre registros son importantes para mantener consistencia.

### Modelo propuesto

El diseño actual contempla cinco entidades principales:

- Usuario
- Categoría
- Producto
- Pedido
- DetallePedido

El modelo puede ampliarse si durante la implementación aparece una necesidad real que lo justifique.

---

## 9. Producto y variantes

### Decisión

En el MVP no se incorpora una entidad `Variante` independiente. El producto se considera actualmente una unidad comercializable con sus características propias.

### Motivo

La dimensión inicial del proyecto es reducida y el catálogo previsto permite representar teléfonos y accesorios sin introducir una jerarquía adicional.

### Evolución posible

Si posteriormente se requiere que un mismo producto tenga múltiples combinaciones independientes de almacenamiento, RAM, color o stock, podrá incorporarse una entidad de variantes.

---

## 10. Manejo del carrito

### Decisión

El carrito no reserva stock.

### Motivo

Reservar unidades mientras el usuario navega puede generar stock bloqueado por carritos abandonados y exige mecanismos adicionales de expiración.

### Regla

El stock se vuelve a validar en el checkout. La operación crítica se realiza en backend.

---

## 11. Manejo del stock

### Decisión

El stock solamente se descuenta cuando el pago es aprobado.

### Flujo propuesto

1. El cliente confirma la compra.
2. Backend obtiene y valida el stock disponible.
3. Se procesa el pago.
4. Si el pago resulta aprobado, se crea/confirma el pedido y se descuenta el stock.
5. Si el pago no resulta aprobado, no se realiza el descuento definitivo.

### Motivo

Evita que agregar productos al carrito o iniciar un checkout reduzca permanentemente el stock.

### Consideración

La implementación deberá cuidar la consistencia transaccional para evitar que dos operaciones puedan consumir incorrectamente las mismas unidades.

---

## 12. Precio histórico del pedido

### Decisión

`DetallePedido` almacenará el precio unitario utilizado al momento de la compra.

### Motivo

El precio actual de un producto puede cambiar después de una compra. El pedido debe conservar el valor con el que fue realizado.

### Consecuencia

El subtotal de cada detalle se calcula utilizando `precio_unitario × cantidad`, y el total del pedido se obtiene a partir de sus detalles.

---

## 13. Dirección de entrega

### Decisión

En el MVP no se crea una entidad independiente `Direccion`.

La dirección utilizada para un envío se almacena como parte de los datos históricos del pedido.

### Motivo

El alcance inicial no requiere administrar una agenda de múltiples direcciones por usuario. Guardar la información en el pedido permite conservar los datos correspondientes a esa operación concreta.

### Evolución posible

Si el sistema posteriormente necesita direcciones reutilizables, múltiples domicilios o administración de direcciones, podrá incorporarse una entidad específica.

---

## 14. Pago

### Decisión actual

El diseño contempla información básica de pago dentro del pedido:

- forma de pago;
- estado del pago;
- referencia de pago, cuando corresponda.

### Motivo

El proyecto requiere contemplar el flujo de pago, pero todavía no se ha definido un proveedor concreto. Por ese motivo no se fija en esta etapa una integración específica.

### Decisión pendiente

Seleccionar el proveedor y mecanismo concreto de pago antes de implementar la integración definitiva.

No se debe documentar como implementado un proveedor que todavía no haya sido seleccionado y probado.

---

## 15. Estados del pedido

### Decisión

El pedido tendrá estados administrados desde backend y gestionados por el administrador según el flujo definido.

Estados propuestos inicialmente:

`CREADO → CONFIRMADO → PREPARANDO → LISTO → ENTREGADO`

También se contempla `CANCELADO` cuando corresponda.

### Motivo

El estado permite representar la evolución de una compra desde su creación hasta su entrega y facilita la gestión administrativa.

---

## 16. Eliminación lógica

### Decisión

Las entidades principales podrán utilizar un indicador de eliminación lógica cuando sea apropiado.

### Motivo

Eliminar físicamente productos, categorías o usuarios puede afectar referencias históricas. Mantener los registros permite conservar información necesaria para pedidos anteriores.

### Consideración

La eliminación lógica requiere que las consultas de catálogo y administración filtren correctamente los registros que no deben aparecer como activos.

---

## 17. Validaciones y manejo de excepciones

### Decisión

Las validaciones críticas se realizarán en backend, acompañadas de un mecanismo centralizado para manejar errores de la API.

### Motivo

El frontend no debe ser la única barrera de validación. Un cliente puede enviar solicitudes directamente contra la API.

### Ejemplos

- Datos obligatorios.
- Formato de email.
- Contraseña.
- Cantidades válidas.
- Existencia del producto.
- Stock disponible.
- Permisos del usuario.
- Estado válido del pedido.

---

## 18. Evolución respecto del proyecto anterior

La base desarrollada previamente aporta las entidades principales y parte de la lógica de persistencia. Para el TFI no se propone copiarla sin modificaciones.

La evolución prevista consiste en:

- revisar el modelo existente;
- conservar conceptos reutilizables;
- adaptar roles al nuevo dominio;
- incorporar atributos específicos del catálogo de teléfonos y accesorios;
- incorporar precio histórico en detalles de pedido;
- incorporar información de entrega y pago;
- separar responsabilidades mediante Controller, Service y Repository;
- incorporar DTOs;
- incorporar autenticación y autorización;
- migrar el frontend hacia la arquitectura React definida;
- corregir inconsistencias entre frontend y backend.

De esta manera, el proyecto demuestra una evolución desde trabajos anteriores hacia un producto web más completo y mantenible.

---

## 19. Criterio general de complejidad

Las decisiones se toman buscando equilibrio entre calidad técnica y alcance académico.

No se incorporarán tecnologías o patrones únicamente para aumentar la cantidad de herramientas utilizadas. Cada componente deberá responder a una necesidad concreta del sistema o a un requisito del TFI.

La arquitectura podrá evolucionar durante la implementación, pero cualquier cambio significativo deberá quedar documentado y justificado.
