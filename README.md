# Ventas Tech — Base de Datos

Script SQL para las **Pre-entregas** del curso de Data Analytics en Coderhouse.

## 📌 Descripción

Este repositorio contiene el archivo `ventas_tech_db.sql`, que carga la base de datos **Ventas_Tech_DB** en PostgreSQL. El objetivo es modelar un esquema relacional de ventas y poder consultarlo con consultas de validación.

## 🛠️ Tecnologías

- **Motor:** PostgreSQL 18
- **Lenguaje:** SQL (DDL y DML)

## 🗂️ Estructura del script

El script está organizado en cuatro secciones:

1. **DROP TABLES** — Elimina las tablas si existen (orden inverso a las dependencias).
2. **CREATE TABLES** — Crea las tablas `categorias`, `clientes`, `productos` y `ventas` con sus claves primarias y foráneas.
3. **INSERT DATA** — Carga datos de prueba (4 categorías, 5 clientes, 6 productos, 10 ventas).
4. **VALIDACIÓN** — Consultas `SELECT` para verificar los registros cargados.

## 🗃️ Diagrama del esquema

- **categorias** (id_categoria PK, nombre_categoria, descripcion)
- **clientes** (id_cliente PK, nombre, email, ciudad, fecha_registro)
- **productos** (id_producto PK, nombre_producto, id_categoria FK, precio, stock, activo)
- **ventas** (id_venta PK, id_cliente FK, id_producto FK, cantidad, precio_unitario, fecha_venta)

## ▶️ Cómo ejecutarlo

1. Creá la base de datos en PostgreSQL:
   ```sql
   CREATE DATABASE "Ventas_Tech_DB";

2. Ejecutá el script ventas_tech_db.sql dentro de esa base (por ejemplo, con pgAdmin o por consola con psql).

👤 Autor
Juan José Guibo Higa
Curso: Data Analytics — Coderhouse
