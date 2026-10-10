# Ventas Tech — Base de Datos, Pipeline ETL y Análisis de Datos

Repositorio del proyecto del curso de Data Analytics en Coderhouse. Contiene la creación y evolución del modelo relacional en SQL, junto con el pipeline de extracción, transformación y carga (ETL) desarrollado en Power Query (lenguaje M) para la integración analítica en Power BI sobre el dataset corporativo.

---

## 📌 Descripción

El repositorio incluye:
1. `ventas_tech_db.sql`: Script DDL/DML para la creación de tablas, definición de claves foráneas y carga de datos iniciales/actualizados en PostgreSQL.
2. `m4_consultas_negocio.sql`: Script de consultas analíticas para responder a preguntas clave de negocio, segmentación y métricas de desempeño (agrupaciones y agregaciones).
3. `m5_consultas_join_union.sql`: Script de consultas avanzadas utilizando combinaciones relacionales (`INNER JOIN`, `LEFT JOIN`), detección de registros sin movimientos y consolidación multicanal (`UNION ALL` + `GROUP BY`).
4. `m6_pipeline_etl_powerquery.m`: Pipeline ETL documentado en lenguaje M para la extracción, limpieza, imputación de calidad y modelado dimensional (tablas de dimensiones y hechos) desde archivos fuente.

---

## 🛠️ Tecnologías y Conceptos

- **Bases de Datos & SQL:** PostgreSQL 18 (DDL, DML, DQL).
- **ETL & Modelado:** Power BI Desktop, Power Query Engine, Lenguaje M.
- **Herramientas de edición y versionado:** Notepad++, Git, GitHub.
- **Funciones y cláusulas SQL:** `INNER JOIN`, `LEFT JOIN`, `IS NULL`, `UNION ALL`, `GROUP BY`, `SUM()`, `COUNT()`, `AVG()`, `EXTRACT()`, `HAVING`, `CASE WHEN`, CTEs (`WITH`), `ORDER BY`, `LIMIT`.
- **Operaciones y funciones en Lenguaje M:** `Excel.Workbook`, `Table.PromoteHeaders`, `Table.TransformColumnTypes`, `Table.Distinct` (garantía de unicidad en PK), `Table.SelectRows` (eliminación de registros vacíos), `Table.ReplaceValue` (imputaciones dimensionales), `Table.AddColumn` (reglas de negocio y cálculo de markup contextual), `Table.NestedJoin` (`JoinKind.LeftOuter`), `Table.ExpandTableColumn`.

---

## 🗂️ Estructura de los Scripts y Pipelines

### 1. Modelo de Datos SQL (`ventas_tech_db.sql`)
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

### 4. Pipeline ETL y Calidad de Datos en Power Query — Módulo 6 (`m6_pipeline_etl_powerquery.m`)
* **Dim_Clientes:** Extracción de hoja origen, control de unicidad en Primary Key (`id_cliente`) para garantizar relaciones 1 a varios con hechos, descarte de registros en blanco, imputación de nulos en atributos categóricos (`ciudad`, `email`) para evitar blancos en segmentadores, y tipado de `fecha_registro` a formato `date`.
* **Dim_Productos:** Control de clave primaria (`id_producto`), imputación de categoría faltante ("Sin Categoría") e imputación contextual de precios nulos mediante el factor de recargo (markup medio de catálogo ~1.93 sobre el costo), asegurando tipos numéricos para medidas DAX de ingresos y margen.
* **Dim_Categorias:** Extracción, tipado entero de la clave y atributos descriptivos de texto, con control de filas vacías para evitar categorías nulas.
* **Fact_Ventas:** Tipado estricto de claves foráneas (`id_cliente`, `id_producto`), métricas cuantitativas y fecha transaccional; enriquecimiento mediante `Left Outer Join` con `Dim_Productos` para desnormalización controlada de atributos y estructuración dimensional de hechos.

---

## 📊 Principales Hallazgos y Decisiones de Modelado

- **Concentración de ventas:** Los productos con `id_producto = 1` e `id_producto = 3` representan el 76% de las ventas del mes 3.
- **Clientes que gastaron más:** Los clientes con `id = 1` e `id = 5` son los clientes frecuentes que generaron el 73% de los ingresos brutos del mes 3.
- **Promedio mensual:** La facturación del mes 3 se mantuvo alineada con la media mensual general.
- **Oportunidades de activación comercial:** La identificación de clientes y productos sin transacciones permitió focalizar campañas de retargeting y rotación de stock.
- **Integridad y robustez en ETL:** Las claves primarias fueron desduplicadas en Power Query para evitar relaciones de muchos a muchos ambiguas en el modelo de Power BI. Se preservó el catálogo aplicando reglas de negocio (imputación por markup) en lugar de descartar registros transaccionales valiosos.

---

## 🗃️ Esquema Dimensional del Proyecto

### Tablas de Dimensiones
- **Dim_Categorias:** `id_categoria` (PK), `nombre_categoria`, `descripcion`
- **Dim_Clientes:** `id_cliente` (PK), `nombre_cliente`, `email`, `ciudad`, `pais`, `segmento`, `fecha_registro`
- **Dim_Productos:** `id_producto` (PK), `nombre_producto`, `categoria`, `subcategoria`, `precio`, `costo`, `stock`, `activo`

### Tabla de Hechos
- **Fact_Ventas:** `id_venta` (PK), `fecha_venta`, `id_cliente` (FK), `id_producto` (FK), `nombre_producto`, `categoria_producto`, `cantidad`, `precio_unitario`, `descuento`, `total_venta`, `canal`

---

## ▶️ Cómo Ejecutarlo

### 1. Entorno SQL (PostgreSQL)
1. Crear la base de datos en PostgreSQL:
   ```sql
   CREATE DATABASE "Ventas_Tech_DB";
2. Ejecutar ventas_tech_db.sql para crear la estructura completa y cargar los datos actualizados.
3. Ejecutar los scripts analíticos según el módulo:
 - m4_consultas_negocio.sql para agregaciones y métricas mensuales.
 - m5_consultas_join_union.sql para análisis dimensional, detección de huérfanos y consolidación con UNION ALL.

### 2. Entorno Power BI / Power Query
1. Abrir Power BI Desktop, abrir archivo Pipeline_ETL_GuiboHiga_JuanJose.pbix y acceder al Editor de Power Query (Transformar datos).
2. Para cada consulta (Dim_Clientes, Dim_Productos, Dim_Categorias, Fact_Ventas), abrir el Editor Avanzado.

👤 Autor
Juan José Guibo Higa
Curso: Data Analytics — Coderhouse
