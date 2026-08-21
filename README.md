# TRABAJO-FINAL---SISTEMA-ECOMMERCE


# 🛒 Sistema de E-commerce

Sistema web de comercio electrónico desarrollado como proyecto para el **Trabajo Final Integrador de la Tecnicatura Universitaria en Programación**.

El proyecto tiene como objetivo desarrollar una plataforma web que permita gestionar un catálogo de productos y las operaciones asociadas al proceso de compra, contemplando diferentes perfiles de usuario y un módulo de administración.

La solución será diseñada de forma modular y adaptable, permitiendo su utilización en diferentes tipos de comercios y catálogos.

---

## 🎯 Objetivo

Desarrollar un sistema de e-commerce full stack que integre los conocimientos adquiridos durante la carrera, aplicando principios de programación orientada a objetos, desarrollo web, persistencia de datos, diseño de APIs, seguridad y buenas prácticas de desarrollo de software.

El sistema buscará proporcionar una solución mantenible, escalable y adaptable a diferentes contextos comerciales.

---

## 📌 Alcance inicial

El sistema contempla inicialmente dos perfiles principales:

### 👤 Cliente

* Registro de usuario.
* Inicio de sesión.
* Consulta del catálogo de productos.
* Consulta por categorías.
* Búsqueda y filtrado de productos.
* Visualización del detalle de un producto.
* Gestión del carrito de compras.
* Modificación de cantidades.
* Proceso de checkout.
* Generación de pedidos.
* Consulta del historial de pedidos.

### 🛠️ Administrador

* Inicio de sesión.
* Acceso al panel administrativo.
* Gestión de productos.
* Gestión de categorías.
* Gestión de stock.
* Consulta de pedidos.
* Actualización del estado de los pedidos.

> El alcance presentado corresponde a la propuesta inicial del proyecto. Los módulos y funcionalidades definitivos serán establecidos y validados durante el desarrollo junto con el docente tutor.

---

## 🏗️ Arquitectura propuesta

Se propone una arquitectura de aplicación web full stack, separando las responsabilidades entre frontend, backend y base de datos.

```text
┌──────────────────────────────┐
│           Frontend           │
│       React + TypeScript     │
└──────────────┬───────────────┘
               │
               │ HTTP / JSON
               ▼
┌──────────────────────────────┐
│          Backend             │
│         Spring Boot          │
├──────────────────────────────┤
│ Controllers                  │
│ Services                     │
│ Repositories                 │
│ DTOs                         │
│ Validation                   │
│ Security                     │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│       Base de datos SQL      │
└──────────────────────────────┘
```

La organización del backend seguirá, en términos generales, una separación de responsabilidades:

```text
Controller
    ↓
Service
    ↓
Repository
    ↓
Database
```

Esta estructura busca favorecer la mantenibilidad, la separación de responsabilidades y el bajo acoplamiento entre los diferentes componentes del sistema.

---

## 💻 Tecnologías propuestas

### Backend

* **Java**
* **Spring Boot**
* **Spring Web**
* **Spring Data JPA**
* **Hibernate**
* **Spring Security**
* **JWT**
* **Bean Validation**
* **DTOs**
* **Manejo global de excepciones**

### Frontend

* **React**
* **TypeScript**
* **HTML**
* **CSS**
* **Vite**
* **React Router**
* **TanStack Query**
* **Zustand**

### Base de datos

* Base de datos relacional SQL.
* JPA / Hibernate.
* Modelo entidad-relación.
* Mecanismo de migraciones o scripts para la gestión de la estructura de datos.

La tecnología específica de base de datos será definida durante la etapa de diseño, considerando los requisitos del sistema, facilidad de desarrollo, despliegue y mantenimiento.

### Herramientas

* **Git**
* **GitHub**
* **Postman** o herramienta equivalente para pruebas de API.
* Herramientas de desarrollo y gestión del proyecto.

---

## 🔐 Seguridad

El sistema contemplará mecanismos de autenticación y autorización.

La propuesta inicial incluye:

* Autenticación mediante JWT.
* Gestión de usuarios.
* Roles de usuario.
* Autorización de operaciones según el perfil.
* Validación de datos.
* Protección de endpoints.
* Manejo centralizado de errores.

El objetivo es que las operaciones administrativas estén restringidas a usuarios autorizados.

---

## 🗄️ Modelo de datos

El sistema utilizará una base de datos relacional.

Inicialmente se consideran como principales entidades del dominio:

```text
Usuario
Producto
Categoría
Carrito
Pedido
Detalle de Pedido
```

El modelo entidad-relación, sus cardinalidades, restricciones y estructura definitiva serán definidos durante la segunda etapa del proyecto.

---

## 📦 Módulos iniciales

Como propuesta inicial, el sistema se organizará en los siguientes módulos:

* **Autenticación y usuarios**
* **Catálogo**
* **Productos**
* **Categorías**
* **Carrito**
* **Pedidos**
* **Administración**
* **Stock**

El listado definitivo de módulos será revisado y validado durante la etapa correspondiente del Trabajo Final Integrador.

---

## 🌐 API REST

El backend expondrá una API REST para permitir la comunicación entre el frontend y los servicios del sistema.

La API será responsable de gestionar las operaciones relacionadas con:

* Usuarios.
* Autenticación.
* Productos.
* Categorías.
* Carrito.
* Pedidos.
* Administración.
* Stock.

La documentación detallada de los endpoints será incorporada durante el desarrollo.

---

## ☁️ Despliegue

El proyecto contempla el despliegue online de al menos uno de sus componentes principales, de acuerdo con los requisitos establecidos por la asignatura.

La estrategia y las plataformas de despliegue serán definidas durante las etapas de desarrollo e integración.

---

## 📁 Estructura propuesta del repositorio

El proyecto se desarrollará dentro de un único repositorio de GitHub.

La estructura inicial propuesta es:

```text
sistema-ecommerce/
│
├── backend/
│
├── frontend/
│
├── database/
│
├── docs/
│
├── .gitignore
└── README.md
```

La estructura podrá evolucionar durante el desarrollo de acuerdo con las necesidades del proyecto y las decisiones arquitectónicas adoptadas.

---

## 📚 Documentación

La documentación del proyecto se incorporará progresivamente dentro del repositorio.

Se prevé incluir:

* Propuesta del proyecto.
* Arquitectura.
* Modelo de datos.
* Módulos.
* Decisiones técnicas.
* Documentación de API.
* Instalación y configuración.
* Pruebas.
* Despliegue.
* Informe final.
* Material para presentación y defensa.

---

## 🚀 Instalación y ejecución

Las instrucciones de instalación, configuración y ejecución serán incorporadas y actualizadas a medida que se complete la implementación de los diferentes componentes.

La aplicación estará compuesta inicialmente por:

```text
Frontend
    ↓
API REST
    ↓
Backend
    ↓
Base de datos
```

---

## 👥 Equipo

**Integrantes:**
* Marcos Rios.
* Christian Emmannuel Olivero.

### Tutor

**Nombre del tutor:** Oscar Londero

---

## 📅 Hoja de ruta

| Etapa | Entrega                                |   Fecha límite |
| ----- | -------------------------------------- | -------------: |
| 1     | Propuesta de proyecto y repositorio    |     30/08/2026 |
| 2     | Diseño, base de datos y módulos        |     27/09/2026 |
| 3     | Desarrollo, documentación y despliegue |     14/11/2026 |
| 4     | Finalización del cursado               |     21/11/2026 |
| 5     | Defensa oral                           | Mesa de examen |

Las fechas corresponden al cronograma establecido en la consigna del Trabajo Final Integrador.

---

## 📌 Estado del proyecto

**Estado:** 🟡 En planificación y desarrollo inicial.

Actualmente el proyecto se encuentra en la etapa de definición de la propuesta, alcance, arquitectura y tecnologías.

El contenido de este README será actualizado progresivamente a medida que avance el desarrollo y se establezcan las decisiones definitivas del proyecto.

---

## 📄 Contexto académico

Proyecto desarrollado en el marco del:

**Trabajo Final Integrador**
**Tecnicatura Universitaria en Programación**

El proyecto se desarrolla bajo la supervisión de un docente tutor y siguiendo las etapas y requisitos establecidos por la asignatura.
