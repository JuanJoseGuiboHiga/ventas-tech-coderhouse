# Ventas Tech — Base de Datos y Análisis SQL

Repositorio del proyecto del curso de Data Analytics en Coderhouse. Contiene la creación y evolución del modelo relacional junto con las consultas analíticas de negocio sobre la base de datos **Ventas_Tech_DB**.

---

## 📌 Descripción

El repositorio incluye:
1. `ventas_tech_db.sql`: Script DDL/DML para la creación de tablas, definición de claves foráneas y carga de datos iniciales/actualizados en PostgreSQL.
2. `m4_consultas_negocio.sql`: Script de consultas analíticas para responder a preguntas clave de negocio, segmentación y métricas de desempeño (agrupaciones y agregaciones).
3. `m5_consultas_join_union.sql`: Script de consultas avanzadas utilizando combinaciones relacionales (`INNER JOIN`, `LEFT JOIN`), detección de registros sin movimientos y consolidación multicanal (`UNION ALL` + `GROUP BY`).

---

## 🛠️ Tecnologías y Conceptos

- **Motor:** PostgreSQL 18
- **Lenguaje:** SQL (DDL, DML y DQL)
- **Funciones y cláusulas utilizadas:** `INNER JOIN`, `LEFT JOIN`, `IS NULL`, `UNION ALL`, `GROUP BY`, `SUM()`, `COUNT()`, `AVG()`, `EXTRACT()`, `HAVING`, `CASE WHEN`, CTEs (`WITH`), `ORDER BY`, `LIMIT`.

---

## 🗂️ Estructura de los Scripts

### 1. Modelo de Datos (`ventas_tech_db.sql`)
* **DROP TABLES:** Limpieza de tablas previas respetando el orden inverso de dependencias.
* **CREATE TABLES:** Definición de tablas de dimensiones y hechos: `categorias`, `subcategorias`, `segmentos`, `ciudades`, `clientes`, `productos`, `zonas`, `regiones`, `territorios` y `ventas`.
* **INSERT DATA:** Poblado de datos de prueba con transacciones e inserción de clientes y productos sin movimientos para análisis de cobertura.
* **VALIDACIÓN:** Verificación de integridad referencial.

### 2. Consultas de Negocio — Módulo 4 (`m4_consultas_negocio.sql`)
* **Consulta 1 — Resumen Ejecutivo Mensual:** Facturación total, cantidad de pedidos y ticket promedio por mes.
* **Consulta 2 — Ranking de Productos (Top 5):** Productos con mayores ingresos y su volumen físico vendido.
* **Consulta 3 — Clientes Recurrentes:** Clientes con más de un pedido y su total gastado.
* **Consulta 4 — Rendimiento Mensual vs. Promedio:** Clasificación de cada mes según si su facturación estuvo *por encima*, *por debajo* o *igual* al promedio mensual general.

### 3. Consultas con Join y Union — Módulo 5 (`m5_consultas_join_union.sql`)
* **Consulta 1 — Vista Base Dimensional (INNER JOIN):** Integración completa de la transacción con dimensiones de cliente, producto y localización geográfica.
* **Consulta 2 — Clientes sin Ventas (LEFT JOIN):** Identificación de clientes registrados que aún no han realizado compras (`WHERE v.id_venta IS NULL`).
* **Consulta 3 — Productos sin Rotación (LEFT JOIN):** Detección de ítems del catálogo que no registran ventas.
* **Consulta 4 — Consolidación Multicanal (UNION ALL + GROUP BY):** Unificación de ventas por zona/canal ('Online' en Sur y 'Presencial' en Centro) y agregación de totales por canal.

---

## 📊 Principales Hallazgos de Negocio

- **Concentración de ventas:** Los productos con `id_producto = 1` e `id_producto = 3` representan el 76% de las ventas del mes 3.
- **Clientes que gastaron más:** Los clientes con `id = 1` e `id = 5` son los clientes frecuentes que generaron el 73% de los ingresos brutos del mes 3.
- **Promedio mensual:** La facturación del mes 3 no incrementó ni disminuyó con respecto al promedio mensual, manteniéndose igual a la media.
- **Oportunidades de activación comercial:** La identificación de clientes y productos sin transacciones permite focalizar campañas de retargeting y rotación de stock estancado.

---

## 🗃️ Diagrama del Esquema

- **categorias** (`id_categoria` PK, `nombre_categoria`, `descripcion`)
- **subcategorias** (`id_subcategoria` PK, `id_categoria` FK, `nombre_subcategoria`)
- **segmentos** (`id_segmento` PK, `nombre`)
- **ciudades** (`id_ciudad` PK, `nombre`)
- **zonas** (`id_zona` PK, `nombre_zona`)
- **regiones** (`id_region` PK, `nombre_region`)
- **territorios** (`id_territorio` PK, `id_zona` FK, `id_region` FK, `nombre_territorio`)
- **clientes** (`id_cliente` PK, `nombre`, `email`, `id_segmento` FK, `id_ciudad` FK, `fecha_registro`)
- **productos** (`id_producto` PK, `nombre_producto`, `id_categoria` FK, `id_subcategoria` FK, `precio`, `stock`, `activo`)
- **ventas** (`id_venta` PK, `id_cliente` FK, `id_producto` FK, `id_territorio` FK, `cantidad`, `precio_unitario`, `fecha_venta`)

---

## ▶️ Cómo Ejecutarlo

1. Crear la base de datos en PostgreSQL:
   ```sql
   CREATE DATABASE "Ventas_Tech_DB";
2. Ejecutar ventas_tech_db.sql para crear la estructura completa y cargar los datos actualizados.
3. Ejecutar los scripts analíticos según el módulo:
 - m4_consultas_negocio.sql para agregaciones y métricas mensuales.
 - m5_consultas_join_union.sql para análisis dimensional, detección de huérfanos y consolidación con UNION ALL.

👤 Autor
Juan José Guibo Higa
Curso: Data Analytics — Coderhouse
