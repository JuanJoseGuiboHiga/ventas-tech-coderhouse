# Ventas Tech — Base de Datos y Análisis SQL

Repositorio del proyecto del curso de Data Analytics en Coderhouse. Contiene la creación del modelo relacional y las consultas analíticas de negocio sobre la base de datos **Ventas_Tech_DB**.

---

## 📌 Descripción

El repositorio incluye:
1. `ventas_tech_db.sql`: Script DDL/DML para la creación de tablas, definición de claves foráneas y carga de datos iniciales en PostgreSQL.
2. `m4_consultas_negocio.sql`: Script de consultas analíticas para responder a preguntas clave de negocio, segmentación y métricas de desempeño.

---

## 🛠️ Tecnologías y Conceptos

- **Motor:** PostgreSQL 18
- **Lenguaje:** SQL (DDL, DML y DQL)
- **Funciones y cláusulas utilizadas:** `EXTRACT()`, `SUM()`, `COUNT()`, `AVG()`, `GROUP BY`, `HAVING`, `CASE WHEN`, CTEs (`WITH`), `ORDER BY`, `LIMIT`.

---

## 🗂️ Estructura de los Scripts

### 1. Modelo de Datos (`ventas_tech_db.sql`)
* **DROP TABLES:** Limpieza de tablas previas respetando el orden inverso de dependencias.
* **CREATE TABLES:** Definición de tablas `categorias`, `clientes`, `productos` y `ventas` con claves primarias y foráneas.
* **INSERT DATA:** Poblado de datos de prueba (categorías, clientes, productos y ventas).
* **VALIDACIÓN:** Verificación de integridad de los registros cargados.

### 2. Consultas de Negocio (`m4_consultas_negocio.sql`)
* **Consulta 1 — Resumen Ejecutivo Mensual:** Facturación total, cantidad de pedidos y ticket promedio por mes.
* **Consulta 2 — Ranking de Productos (Top 5):** Productos con mayores ingresos y su volumen físico vendido.
* **Consulta 3 — Clientes Recurrentes:** Clientes con más de un pedido y su total gastado.
* **Consulta 4 — Rendimiento Mensual vs. Promedio:** Clasificación de cada mes según si su facturación estuvo *por encima*, *por debajo* o *igual* al promedio mensual general.

---

## 📊 Principales Hallazgos de Negocio

- **Concentración de ventas:** El producto con `id_producto = 1` es el que generó más ingresos para la compañía en comparación con el resto del Top 5.
- **Clientes que gastaron más:** Los clientes con `id = 1` y `5` son los clientes frecuentes que generaron mayores ganancias.
- **Promedio mensual:** La facturación del mes 3 no estuvo ni por encima ni por debajo del promedio mensual, se mantuvo igual.

---

## 🗃️ Diagrama del Esquema

- **categorias** (`id_categoria` PK, `nombre_categoria`, `descripcion`)
- **clientes** (`id_cliente` PK, `nombre`, `email`, `ciudad`, `fecha_registro`)
- **productos** (`id_producto` PK, `nombre_producto`, `id_categoria` FK, `precio`, `stock`, `activo`)
- **ventas** (`id_venta` PK, `id_cliente` FK, `id_producto` FK, `cantidad`, `precio_unitario`, `fecha_venta`)

---

## ▶️ Cómo Ejecutarlo

1. Crear la base de datos en PostgreSQL:
   ```sql
   CREATE DATABASE "Ventas_Tech_DB";
2. Ejecutar ventas_tech_db.sql para crear la estructura y cargar los datos.
3. Ejecutar m4_consultas_negocio.sql para visualizar las métricas y los resultados analíticos.

👤 Autor
Juan José Guibo Higa
Curso: Data Analytics — Coderhouse
