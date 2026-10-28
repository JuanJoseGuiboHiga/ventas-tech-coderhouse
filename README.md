# Ventas Tech — Base de Datos, Pipeline ETL, Modelado Relacional y Medidas DAX

Repositorio del proyecto del curso de Data Analytics en Coderhouse. Contiene el ciclo analítico integral: creación y evolución del modelo relacional en SQL, pipeline de extracción, transformación y carga (ETL) en Power Query (lenguaje M), y el modelado dimensional con relaciones activas e inteligencia de tiempo mediante DAX en Power BI.

---

## 📌 Descripción

El repositorio incluye:
1. `ventas_tech_db.sql`: Script DDL/DML para la creación de tablas, definición de claves foráneas y carga de datos en PostgreSQL.
2. `m4_consultas_negocio.sql`: Script de consultas analíticas para responder a preguntas clave de negocio y métricas de desempeño.
3. `m5_consultas_join_union.sql`: Consultas relacionales avanzadas (`INNER JOIN`, `LEFT JOIN`, detección de registros huérfanos y consolidación `UNION ALL`).
4. `m6_pipeline_etl_powerquery.m`: Pipeline ETL documentado en lenguaje M para extracción, limpieza, imputación de calidad y modelado dimensional.
5. `GuiboHiga_JuanJose_Checkpoint2.pbix`: Archivo de Power BI Desktop con el modelo estrella/copo de nieve, tabla calendario `Dim_Fechas`, tabla `_Medidas` y validación temporal en Matriz.

---

## 🛠️ Tecnologías y Conceptos

- **Bases de Datos & SQL:** PostgreSQL 18 (DDL, DML, DQL).
- **ETL & Modelado:** Power BI Desktop, Power Query Engine, Lenguaje M.
- **Cálculo Analítico:** DAX (Data Analysis Expressions), Time Intelligence, Contextos de Fila y Filtro.
- **Herramientas de edición y versionado:** Notepad++, Git, GitHub.
- **Funciones y cláusulas SQL:** `INNER JOIN`, `LEFT JOIN`, `IS NULL`, `UNION ALL`, `GROUP BY`, `SUM()`, `COUNT()`, `AVG()`, `EXTRACT()`, `HAVING`, `CASE WHEN`, CTEs (`WITH`).
- **Operaciones en M:** `Table.Distinct`, `Table.ReplaceValue`, `Table.AddColumn`, `Table.NestedJoin`, `Table.TransformColumnTypes`.
- **Funciones DAX:** `SUM`, `CALCULATE`, `TOTALYTD`, `SAMEPERIODLASTYEAR`, `VAR` / `RETURN`, `DIVIDE`, `GENERATE`, `CALENDAR`.

---

## 🗂️ Estructura de los Scripts y Modelos

### 1. Modelo de Datos SQL (`ventas_tech_db.sql`)
* Creación de tablas de dimensiones y hechos con claves foráneas.
* Poblado de transacciones y registros de control sin movimientos.

### 2. Consultas de Negocio — Módulo 4 (`m4_consultas_negocio.sql`)
* Facturación total, ticket promedio, Top 5 de productos y clientes recurrentes.

### 3. Consultas con Join y Union — Módulo 5 (`m5_consultas_join_union.sql`)
* Integración dimensional, detección de clientes/productos sin movimientos y unificación multicanal.

### 4. Pipeline ETL en Power Query — Módulo 6 (`m6_pipeline_etl_powerquery.m`)
* Limpieza, tipado estricto, imputación de nulos por markup medio de catálogo y desduplicación de claves primarias.

### 5. Modelado y Medidas Core en DAX — Módulo 8 (`GuiboHiga_JuanJose_Checkpoint2.pbix`)
* **Modelo Relacional:** Esquema dimensional con relaciones activas `1:N` y dirección de filtro simple hacia `Fact_Ventas`.
* **Dim_Fechas:** Tabla calendario calculada para soporte de análisis temporal.
* **Tabla `_Medidas`:** Centralización de métricas core del negocio:
  * **Total Ventas (Base):**
    ```dax
    Total Ventas = SUM(Fact_Ventas[total_venta])
    ```
  * **Ventas Online (Filtro con CALCULATE):**
    ```dax
    Ventas Online = 
    CALCULATE(
        [Total Ventas],
        Fact_Ventas[canal] = "Online"
    )
    ```
  * **Ventas YTD (Acumulado temporal):**
    ```dax
    Ventas YTD = 
    TOTALYTD(
        [Total Ventas],
        Dim_Fechas[Date]
    )
    ```
  * **Ventas LY (Año anterior):**
    ```dax
    Ventas LY = 
    CALCULATE(
        [Total Ventas],
        SAMEPERIODLASTYEAR(Dim_Fechas[Date])
    )
    ```
  * **% Crecimiento Anual (Optimizado con Variables):**
    ```dax
    % Crecimiento Anual = 
    VAR VentasActual = [Total Ventas]
    VAR VentasAnterior = [Ventas LY]
    RETURN
        DIVIDE(
            VentasActual - VentasAnterior,
            VentasAnterior,
            0
        )
    ```

---

## 🗃️ Esquema Dimensional del Proyecto

### Tablas de Dimensiones
- **`Dim_Categorias`:** `id_categoria` (PK), `nombre_categoria`, `descripcion`
- **`Dim_Clientes`:** `id_cliente` (PK), `nombre_cliente`, `email`, `ciudad`, `pais`, `segmento`, `fecha_registro`
- **`Dim_Productos`:** `id_producto` (PK), `nombre_producto`, `categoria`, `subcategoria`, `precio`, `costo`, `stock`, `activo`
- **`Dim_Fechas`:** `Date` (PK), `Año`, `Mes`, `Mes Nombre`, `Trimestre`, `Día`

### Tabla de Hechos
- **`Fact_Ventas`:** `id_venta` (PK), `fecha_venta` (FK), `id_cliente` (FK), `id_producto` (FK), `nombre_producto`, `categoria_producto`, `cantidad`, `precio_unitario`, `descuento`, `total_venta`, `canal`

---

## ▶️ Cómo Ejecutarlo

### 1. Entorno SQL (PostgreSQL)
1. Crear la base de datos:
   ```sql
   CREATE DATABASE "Ventas_Tech_DB";
2. Ejecutar ventas_tech_db.sql para crear la estructura completa y cargar los datos actualizados.
3. Ejecutar los scripts analíticos según el módulo:
 - m4_consultas_negocio.sql para agregaciones y métricas mensuales.
 - m5_consultas_join_union.sql para análisis dimensional, detección de huérfanos y consolidación con UNION ALL.

### 2. Entorno Power BI / Power Query
1. Abrir GuiboHiga_JuanJose_Checkpoint2.pbix en Power BI Desktop.
2. En la vista Modelo, verificar el esquema de relaciones 1:N.
3. En la vista Informe, interactuar con la Matriz para validar los cálculos de Total Ventas, Ventas YTD, Ventas LY y % Crecimiento Anual.

👤 Autor
Juan José Guibo Higa
Curso: Data Analytics — Coderhouse
