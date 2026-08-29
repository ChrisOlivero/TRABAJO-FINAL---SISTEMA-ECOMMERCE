# 🛒 Sistema de E-commerce

Sistema web de comercio electrónico desarrollado en el marco del **Trabajo Final Integrador de la Tecnicatura Universitaria en Programación**.

El proyecto propone una solución para un emprendimiento de pequeña escala dedicado a la comercialización de **celulares nuevos, celulares usados y accesorios tecnológicos**, actualmente gestionado principalmente mediante redes sociales y WhatsApp.

El objetivo es centralizar y profesionalizar el proceso de comercialización, permitiendo gestionar el catálogo, stock, clientes y pedidos mediante una plataforma web propia.

---

## 📌 Contexto y problemática

El comercio planteado corresponde a un emprendimiento en etapa inicial, gestionado por una sola persona y que no necesariamente cuenta con un local comercial.

Actualmente, la comercialización se realiza principalmente mediante:

* WhatsApp.
* Redes sociales.
* Publicaciones y estados con fotografías de los productos.
* Coordinación directa con los clientes para concretar las ventas.
* Retiro de productos en el domicilio del vendedor o envío a domicilio.

Este modelo permite comenzar a comercializar sin una infraestructura importante, pero a medida que aumenta la cantidad de productos, consultas y ventas pueden aparecer dificultades relacionadas con la organización y el control de la información.

Entre las principales problemáticas identificadas se encuentran:

* Catálogo distribuido entre diferentes publicaciones y canales.
* Consultas manuales sobre disponibilidad y precios.
* Control manual del stock.
* Registro y seguimiento de pedidos mediante diferentes medios.
* Falta de un catálogo centralizado.
* Dificultad para consultar de manera organizada el estado de los pedidos.
* Dependencia de diferentes herramientas para administrar el proceso de venta.
* Mayor posibilidad de errores a medida que aumenta el volumen de operaciones.

---

## 🎯 Objetivo

Desarrollar una plataforma web propia que permita centralizar y organizar el proceso de comercialización, facilitando tanto la experiencia de compra del cliente como la gestión interna del emprendimiento.

El sistema estará orientado inicialmente a un comercio de pequeña escala y será desarrollado de manera modular, priorizando una solución mantenible y adaptable a las necesidades concretas del negocio.

---

## 👥 Actores

### Cliente

El cliente podrá:

* Registrarse e iniciar sesión.
* Consultar el catálogo.
* Buscar y filtrar productos.
* Consultar el detalle de los productos.
* Agregar productos al carrito.
* Modificar cantidades.
* Realizar el checkout.
* Seleccionar retiro o envío.
* Realizar el pago online.
* Generar pedidos.
* Consultar sus pedidos y su estado.

### Administrador

El administrador podrá:

* Iniciar sesión.
* Gestionar productos.
* Gestionar categorías.
* Gestionar stock.
* Consultar pedidos.
* Actualizar estados de pedidos.
* Cancelar pedidos.
* Realizar ajustes de stock cuando sea necesario.

---

## 🛍️ Catálogo

El sistema estará orientado inicialmente a la comercialización de:

* Celulares nuevos.
* Celulares usados.
* Accesorios tecnológicos.

Los productos podrán contar con información específica según corresponda, como:

* Marca.
* Modelo.
* Capacidad de almacenamiento.
* Memoria RAM.
* Color.
* Condición del producto.
* Precio.
* Stock.

La estructura definitiva del catálogo y sus posibles variantes será definida durante la etapa de diseño.

---

## 🛒 Proceso de compra

El flujo principal propuesto será:

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

El checkout contemplará:

* Carrito de compra.
* Datos del cliente.
* Selección entre retiro y envío a domicilio.
* Información necesaria para el envío.
* Pago online.
* Confirmación de la operación.
* Generación del pedido.

Inicialmente quedan fuera del alcance las integraciones con sistemas externos de facturación electrónica y logística, salvo que posteriormente se determine que resultan necesarias.

---

## 📦 Gestión de stock

El stock se gestionará inicialmente mediante una cantidad disponible asociada a cada producto o variante.

Agregar un producto al carrito **no reservará stock**.

Antes de finalizar una compra, el backend deberá verificar nuevamente la disponibilidad.

El flujo será:

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

No se contempla inicialmente un sistema completo de movimientos de inventario. Sin embargo, el administrador podrá realizar ajustes de stock cuando sea necesario, por ejemplo ante devoluciones, reposiciones o correcciones.

---

## 📋 Pedidos

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

## 💰 Precios

El precio aplicado durante una compra deberá conservarse dentro del pedido.

Por lo tanto, una modificación posterior del precio de un producto no deberá alterar el importe histórico de pedidos ya realizados.

Ejemplo:

```text
Precio al momento de la compra:
Celular X → $500.000

Pedido registrado:
Celular X → $500.000
```

Si posteriormente el precio cambia:

```text
Celular X → $550.000
```

el pedido anterior continuará mostrando el precio de **$500.000**.

---

## 🔐 Seguridad

El sistema contará con dos roles:

* `CLIENTE`
* `ADMIN`

Se propone implementar autenticación mediante **JWT** y autorización basada en roles.

La seguridad será validada mediante casos concretos, entre ellos:

* Un cliente no puede crear ni modificar productos.
* Un cliente no puede modificar categorías.
* Un cliente no puede modificar el stock.
* Un cliente no puede modificar estados administrativos.
* Un usuario no puede consultar pedidos pertenecientes a otro usuario modificando manualmente el identificador de la solicitud.
* Determinadas operaciones sobre pedidos estarán disponibles únicamente para administradores.
* Los datos recibidos por la API serán validados en backend.

---

## ⭐ Diferencial

El proyecto no busca competir directamente con plataformas comerciales de e-commerce existentes.

La propuesta consiste en desarrollar una solución propia orientada a las necesidades de un emprendimiento pequeño que actualmente gestiona sus ventas mediante redes sociales y mensajería.

El desarrollo de una solución propia permitirá:

* Centralizar catálogo, stock y pedidos.
* Adaptar el funcionamiento del sistema a las necesidades específicas del comercio.
* Tener control sobre la evolución del producto.
* Evitar depender de las limitaciones de una plataforma comercial determinada.
* Incorporar nuevas funcionalidades según las necesidades que surjan.
* Mantener una solución dimensionada al contexto del emprendimiento.

El diferencial se plantea principalmente desde la **adaptabilidad y control sobre la solución**, en lugar de intentar competir con plataformas generalistas.

---

## 🎯 MVP

### P0 — Funcionalidades obligatorias

#### Cliente

* Registro.
* Inicio de sesión.
* Catálogo.
* Categorías.
* Búsqueda y filtrado básico.
* Detalle de producto.
* Carrito.
* Datos de compra.
* Selección de retiro o envío.
* Pago online.
* Generación del pedido.
* Consulta de pedidos.
* Consulta del estado del pedido.

#### Administrador

* Inicio de sesión.
* Gestión de productos.
* Gestión de categorías.
* Gestión de stock.
* Consulta de pedidos.
* Actualización de estados.
* Cancelación de pedidos.

### P1 — Funcionalidades deseables

Las funcionalidades P1 serán evaluadas una vez completado el núcleo del sistema.

Entre las posibles extensiones se consideran:

* Filtros avanzados.
* Comparación de productos.
* Estadísticas administrativas.
* Mejoras en la gestión de inventario.
* Nuevas funcionalidades para la experiencia del cliente.

### Fuera de alcance inicial

* Facturación electrónica.
* Integraciones externas de logística.
* Aplicación móvil nativa.
* Marketplace.
* Gestión de múltiples sucursales.
* Sistema avanzado de movimientos de inventario.
* Funcionalidades orientadas a grandes volúmenes de usuarios.

---

## 🏗️ Arquitectura propuesta

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

Esta separación busca favorecer la mantenibilidad, la cohesión y la separación de responsabilidades.

---

## 💻 Tecnologías propuestas

### Backend

* Java
* Spring Boot
* Spring Web
* Spring Data JPA
* Hibernate
* Spring Security
* JWT
* Bean Validation
* DTOs
* Manejo global de excepciones

### Frontend

* TypeScript
* Vite
* HTML
* CSS

### Base de datos

* Base de datos relacional SQL.
* JPA / Hibernate.

La tecnología específica de base de datos será definida durante la etapa de diseño.

### Herramientas

* Git
* GitHub
* Postman o herramienta equivalente.
* Plataforma de despliegue online.

---

## 📐 Dimensión inicial

El sistema será planteado inicialmente para un emprendimiento de pequeña escala.

Como escenario de referencia se considera:

* Aproximadamente 15 productos o variantes.
* Aproximadamente 10 clientes.
* 1 administrador.
* Volumen reducido de pedidos diarios.

Estos valores representan el escenario inicial de prueba y permiten dimensionar la solución de acuerdo con el problema planteado.

---

## 🚀 Plan de desarrollo

### Etapa 1 — Backend y base de datos

* Configuración de Spring Boot.
* Configuración de la base de datos.
* Entidades.
* Repositories.
* API REST inicial.

### Etapa 2 — Autenticación y seguridad

* Registro.
* Login.
* JWT.
* Roles.
* Protección de endpoints.
* Validaciones.

### Etapa 3 — Catálogo

* Productos.
* Categorías.
* Búsqueda.
* Filtros.
* Detalle.
* Gestión administrativa.

### Etapa 4 — Carrito y pedidos

* Carrito.
* Checkout.
* Pago.
* Verificación de stock.
* Generación de pedidos.
* Historial.
* Estados.

### Etapa 5 — Administración

* Gestión de productos.
* Gestión de categorías.
* Stock.
* Pedidos.
* Cancelaciones.

### Etapa 6 — Integración, pruebas y despliegue

* Integración frontend/backend.
* Pruebas funcionales.
* Pruebas de seguridad.
* Corrección de errores.
* Despliegue online.
* Documentación.
* Video explicativo.

---

## ☁️ Despliegue

El proyecto contempla el despliegue online de al menos uno de sus componentes principales, de acuerdo con los requisitos establecidos para el Trabajo Final Integrador.

La plataforma y estrategia de despliegue serán definidas durante el desarrollo.

---

## 📚 Documentación

La documentación del proyecto se incorporará progresivamente dentro del repositorio.

Se prevé documentar:

* Relevamiento y problemática.
* Requisitos.
* Arquitectura.
* Modelo de datos.
* Módulos.
* Decisiones técnicas.
* API REST.
* Seguridad.
* Pruebas.
* Instalación y configuración.
* Despliegue.
* Informe final.

---

## 👥 Equipo

| Integrante                 | Rol        |
| -------------------------- | ---------- |
| Christian Emmanuel Olivero | Desarrollo |
| Marcos Rios                | Desarrollo |

### Tutor

**Oscar Londero**

---

## 📅 Hoja de ruta académica

| Etapa | Entrega                                |   Fecha límite |
| ----- | -------------------------------------- | -------------: |
| 1     | Propuesta de proyecto y repositorio    |     30/08/2026 |
| 2     | Diseño, base de datos y módulos        |     27/09/2026 |
| 3     | Desarrollo, documentación y despliegue |     14/11/2026 |
| 4     | Finalización del cursado               |     21/11/2026 |
| 5     | Defensa oral                           | Mesa de examen |

---

## 📌 Estado del proyecto

**Estado:** 🟡 Planificación y definición inicial.

El proyecto se encuentra en la etapa de definición del problema, alcance, arquitectura y tecnologías.

El README será actualizado progresivamente durante el desarrollo, incorporando las decisiones y componentes definitivos del sistema.
