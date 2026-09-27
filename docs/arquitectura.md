# Arquitectura del sistema

## 1. Objetivo

Este documento define la arquitectura propuesta para el sistema de e-commerce del Trabajo Final Integrador. La arquitectura busca separar responsabilidades, facilitar el mantenimiento y permitir la evolución del sistema durante las siguientes etapas del proyecto.

La solución se plantea como una aplicación web full stack compuesta por un frontend, un backend expuesto mediante una API REST y una base de datos relacional.

## 2. Vista general

```text
┌──────────────────────────────┐
│          Frontend            │
│ React + TypeScript           │
│ React Router                 │
│ TanStack Query               │
│ Zustand                      │
└──────────────┬───────────────┘
               │ HTTP / JSON
               ▼
┌──────────────────────────────┐
│          Backend             │
│ Spring Boot                  │
│                              │
│ Controller                   │
│      ↓                       │
│ Service                      │
│      ↓                       │
│ Repository                   │
│      ↓                       │
│ JPA / Hibernate              │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│      Base de datos SQL       │
└──────────────────────────────┘
```

La comunicación entre frontend y backend se realizará mediante HTTP utilizando JSON como formato de intercambio.

## 3. Frontend

El frontend será desarrollado con React y TypeScript.

Sus responsabilidades principales serán:

- Presentar la interfaz de usuario.
- Gestionar navegación mediante React Router.
- Consumir la API REST del backend.
- Gestionar el estado de la interfaz y del carrito cuando corresponda.
- Gestionar las consultas y mutaciones contra la API mediante TanStack Query.
- Mantener información de estado global mediante Zustand cuando resulte necesario.
- Validar aspectos de interacción propios de la interfaz.
- Mostrar mensajes de error y confirmación al usuario.

El frontend no será responsable de aplicar reglas de negocio críticas. Las validaciones relacionadas con stock, autorización, pedidos, precios y operaciones administrativas deberán ser verificadas nuevamente por el backend.

## 4. Backend

El backend será desarrollado con Java y Spring Boot y expondrá una API REST.

Se propone una separación en capas:

```text
Controller
    ↓
Service
    ↓
Repository
    ↓
Database
```

### 4.1 Controller

Los controladores reciben las solicitudes HTTP, validan la estructura básica de entrada y delegan la operación correspondiente a la capa de servicio.

Responsabilidades:

- Definir endpoints REST.
- Recibir parámetros, path variables y cuerpos JSON.
- Utilizar DTOs para entrada y salida cuando corresponda.
- Aplicar validaciones de entrada.
- Devolver respuestas HTTP adecuadas.
- No concentrar reglas de negocio complejas.

### 4.2 Service

La capa de servicios concentra la lógica de negocio de la aplicación.

Responsabilidades principales:

- Coordinar operaciones entre entidades y repositorios.
- Aplicar reglas de negocio.
- Verificar disponibilidad de stock durante el checkout.
- Gestionar la creación de pedidos.
- Calcular y preservar la información necesaria del pedido.
- Aplicar reglas relacionadas con los estados del pedido.
- Coordinar las operaciones administrativas.
- Controlar las operaciones que requieren autorización.

Esta capa será especialmente importante para evitar que las reglas de negocio queden distribuidas entre controladores y entidades de persistencia.

### 4.3 Repository

Los repositorios serán responsables del acceso a los datos persistidos mediante JPA/Hibernate.

Responsabilidades:

- Consultar entidades.
- Persistir nuevas entidades.
- Actualizar registros.
- Eliminar o aplicar eliminación lógica cuando corresponda.
- Ejecutar consultas específicas necesarias para los módulos.

Los repositorios no deberían contener reglas de negocio propias del dominio.

### 4.4 Persistencia

JPA/Hibernate será utilizado como mecanismo de persistencia entre las entidades Java y la base de datos relacional.

Las entidades previstas para el modelo inicial son:

- Usuario.
- Categoria.
- Producto.
- Pedido.
- DetallePedido.

El modelo completo se encuentra documentado en `modelo-datos.md`.

## 5. API REST

El backend expondrá recursos mediante endpoints REST.

A nivel conceptual se prevén recursos relacionados con:

- Autenticación y usuarios.
- Categorías.
- Productos.
- Pedidos.
- Administración.

Los endpoints concretos y sus contratos deberán definirse durante la implementación de la API.

Se evitará acoplar el frontend directamente con las entidades JPA. Cuando sea necesario, se utilizarán DTOs para controlar la información expuesta por la API.

## 6. Seguridad

La autenticación se realizará mediante JWT según lo definido en la propuesta del proyecto.

Se contemplan dos roles principales:

- `CLIENTE`
- `ADMIN`

El backend será responsable de verificar el token y las autorizaciones asociadas al usuario.

La seguridad se aplicará especialmente sobre:

- Operaciones administrativas.
- Gestión de productos y categorías.
- Gestión de stock.
- Gestión de pedidos.
- Acceso al historial de pedidos de cada cliente.

El frontend puede ocultar o mostrar opciones según el rol, pero esa condición no reemplaza la autorización del backend.

## 7. DTOs

Los DTOs se utilizarán cuando aporten una separación clara entre el modelo de persistencia y el contrato de la API.

Sus principales objetivos serán:

- Evitar exponer directamente las entidades JPA cuando no sea conveniente.
- Controlar los datos recibidos por la API.
- Definir respuestas específicas para cada operación.
- Reducir el acoplamiento entre persistencia y presentación.

No se propone crear DTOs de manera indiscriminada para cada objeto si no aportan valor al diseño.

## 8. Validaciones y manejo de excepciones

Las validaciones se dividirán entre frontend y backend, manteniendo al backend como fuente final de validación de las reglas de negocio.

Se deberán contemplar, entre otros casos:

- Datos obligatorios.
- Formato de correo electrónico.
- Credenciales inválidas.
- Producto inexistente.
- Stock insuficiente.
- Pedido inexistente.
- Acceso no autorizado.
- Operaciones administrativas realizadas por usuarios sin permisos.

El backend deberá centralizar el tratamiento de excepciones para devolver respuestas HTTP coherentes y comprensibles para el frontend.

## 9. Transacciones

Las operaciones que modifiquen conjuntamente pedido, detalle y stock deberán tratarse como una unidad de trabajo transaccional.

El objetivo es evitar estados inconsistentes, por ejemplo, crear un pedido sin actualizar correctamente el stock o modificar el stock cuando la operación de compra no corresponde.

La implementación concreta de las transacciones se definirá durante el desarrollo del backend.

## 10. Regla de stock

La arquitectura respeta la regla definida en la propuesta:

1. Agregar un producto al carrito no reserva stock.
2. Durante el checkout el backend vuelve a verificar la disponibilidad.
3. El stock se descuenta solamente cuando el pago es aprobado.
4. Ante una cancelación aplicable, el stock podrá restaurarse según las reglas definidas para el pedido.

Esta lógica deberá permanecer en el backend y no depender exclusivamente del frontend.

## 11. Flujo de una solicitud

Un flujo típico será:

```text
Usuario
  │
  ▼
Frontend React
  │
  │ HTTP + JSON
  ▼
Controller
  │
  ▼
Service
  │
  ├── Validaciones
  ├── Reglas de negocio
  ├── Autorización
  └── Transacciones
  │
  ▼
Repository
  │
  ▼
JPA / Hibernate
  │
  ▼
Base de datos
```

La respuesta recorrerá el camino inverso hasta llegar nuevamente al frontend.

## 12. Evolución respecto de la base anterior

El proyecto parte de una base desarrollada previamente, pero la arquitectura final no se considera una copia directa de esa implementación.

La evolución prevista consiste en:

- Pasar de una aplicación basada en JPA y lógica de dominio previa a una arquitectura web basada en Spring Boot y API REST.
- Mantener las entidades centrales cuando continúen siendo útiles.
- Adaptar el modelo a las necesidades específicas del nuevo e-commerce.
- Incorporar DTOs donde mejoren la separación de responsabilidades.
- Incorporar autenticación y autorización mediante JWT.
- Separar claramente responsabilidades entre controller, service y repository.
- Integrar el frontend React con el backend mediante HTTP/JSON.
- Mantener las reglas de stock y pedidos en el backend.

La implementación existente será revisada y refactorizada antes de reutilizarse cuando sea necesario.

## 13. Organización propuesta del código

### Backend

```text
backend/
└── src/
    └── main/
        └── java/
            └── .../
                ├── controller/
                ├── service/
                ├── repository/
                ├── dto/
                ├── entity/
                ├── enums/
                ├── exception/
                ├── security/
                └── config/
```

Los nombres concretos de paquetes podrán ajustarse a la estructura final del proyecto.

### Frontend

```text
frontend/
└── src/
    ├── components/
    ├── pages/
    ├── services/
    ├── hooks/
    ├── stores/
    ├── types/
    ├── routes/
    └── assets/
```

La organización definitiva podrá evolucionar durante la implementación, manteniendo la separación entre presentación, acceso a datos, estado y componentes reutilizables.

## 14. Criterios arquitectónicos

Las decisiones arquitectónicas se orientan por los siguientes criterios:

- Separación de responsabilidades.
- Bajo acoplamiento.
- Alta cohesión.
- Mantenibilidad.
- Claridad del código.
- Reutilización razonable.
- Seguridad centralizada en backend.
- Validación de reglas críticas en servidor.
- Evolución progresiva sin incorporar complejidad innecesaria.

## 15. Alcance de esta definición

Esta arquitectura corresponde a la etapa de diseño del TFI. Algunos detalles de implementación, como la estructura exacta de endpoints, configuración final de seguridad, proveedor de pago, motor concreto de base de datos y estrategia definitiva de despliegue, deberán definirse durante las siguientes etapas.

No se consideran decisiones cerradas aquellas que todavía no hayan sido acordadas por el equipo o aprobadas por el tutor.
