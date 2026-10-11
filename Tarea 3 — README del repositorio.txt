Markdown
# 📊 Sistema de Análisis Comercial y Gestión de Ventas (SQL Server)

[![SQL Server](https://img.shields.io/badge/Database-SQL%20Server-red?logo=microsoftsqlserver)](https://www.microsoft.com/sql-server)
[![Language](https://img.shields.io/badge/Language-T--SQL-blue)](https://docs.microsoft.com/sql/t-sql/language-reference)
[![Author](https://img.shields.io/badge/Author-Gabriel%20Wetzel-darkgreen)](https://github.com/wetzelgabriel18)

## 📌 Descripción del Proyecto

Este repositorio contiene el modelado relacional, los scripts de integración y las consultas analíticas orientadas al área comercial de una empresa. El objetivo principal es desnormalizar y procesar información transaccional de ventas para transformar datos crudos en **insights clave** sobre el comportamiento del cliente, rendimiento de productos y tendencias de facturación.

---

## 🛠️ Tecnologías y Entorno

* **Motor de Base de Datos:** Microsoft SQL Server (T-SQL)
* **IDE Recomendado:** SQL Server Management Studio (SSMS) / Azure Data Studio
* **Control de Versiones:** Git & GitHub
* **Visualización de datos:** Microsoft Power BI // Se agregó ya que el readme realizado por IA no lo incluía
---

## 📐 Modelo de Datos y Arquitectura

La solución se basa en una arquitectura relacional compuesta por **1 tabla de hechos (fact table)** y **4 tablas de dimensiones (dimension tables)**:

              +-------------------+
              |     Categorias    |
              +-------------------+
                        | (1:N)
                        v
+--------------+  (1:N)   +-------------------+
|   Ciudades   | -------> |     Productos     |
+--------------+          +-------------------+
|                            |
| (1:N)                      | (1:N)
v                            v
+--------------+  (1:N)   +-------------------+
|   Clientes   | -------> |      Ventas       |  <-- Tabla Transaccional
+--------------+          +-------------------+


### Detalle de las Tablas:
* **`Ventas` (Hechos):** Contiene las transacciones con campos como `fecha_venta`, `cantidad`, `precio_unitario` y las claves foráneas correspondientes.
* **`Clientes` (Dimensión):** Almacena la información de los compradores y su vinculación geográfica.
* **`Productos` (Dimensión):** Catálogo de artículos con sus precios de referencia y categoría.
* **`Categorias` (Dimensión):** Clasificación de los productos de acuerdo a su función. // se modifica la descripción de la tabla Categorías, ya que la descripción de la IA hablabla de jerarquías. 
* **`Ciudades` (Dimensión):** Registro de ubicaciones para análisis de cobertura geográfica.

---

## 💡 Consultas Destacadas

### Consolidación de Información Transaccional (`INNER JOIN`)
Consulta para integrar en una sola fila los datos descriptivos de clientes, productos, categorías y ciudades con los cálculos monetarios:

```sql
SELECT 
    v.fecha_venta,
    c.nombre AS nombre_cliente,
    p.nombre_producto,
    ca.descripcion AS descripcion_categoria,
    ci.nombre_ciudad,
    v.cantidad,
    v.precio_unitario, 
    (v.cantidad * v.precio_unitario) AS [total de venta]
FROM Ventas AS v
INNER JOIN Clientes AS c 
    ON c.id_cliente = v.id_cliente 
INNER JOIN Productos AS p 
    ON p.id_producto = v.id_producto
INNER JOIN Ciudades AS ci 
    ON ci.id_ciudad = c.id_ciudad
INNER JOIN Categorias AS ca 
    ON ca.id_categoria = p.id_categoria;

// eliminaría la sección de "Consultas Destacadas" ya que el archivo tiene varias consultas más con funciones diferentes, que permiten ver insights de ventas que resultarían más útiles para un equipo comercial. 

📈 Principales Hallazgos Comerciales
Incremento del Ticket Promedio: Se identificó que, manteniendo constante el volumen de pedidos entre períodos (10 pedidos por mes), la facturación se duplicó de $6,444.00 a $13,280.00, elevando significativamente el ticket promedio por compra.

Productos de Alto Valor Impulsores del Ingreso: El producto id_producto = 1 representa el mayor volumen de facturación ($7,200.00) con solo 6 unidades vendidas, demostrando mayor margen/valor que productos de alta rotación física.

Concentración de Clientes Clave (VIP): El cliente id_cliente = 12 lidera el gasto acumulado ($4,700.00 en 2 pedidos), perfilándose como cuenta prioritaria para estrategias de retención.

⚙️ Guía de Ejecución Paso a Paso
Clonar el repositorio:

Bash
git clone [https://github.com/wetzelgabriel18/Pre-entrega-5.git](https://github.com/wetzelgabriel18/Pre-entrega-5.git)
Cargar la Base de Datos:

Abre SQL Server Management Studio (SSMS).

Conéctate a tu instancia de SQL Server.

Abre y ejecuta los scripts de creación de tablas e inserción de datos en el siguiente orden:

Tablas maestras/dimensiones (Ciudades, Categorias, Clientes, Productos).

Tabla transaccional (Ventas).

Ejecutar las Consultas Analíticas:

Selecciona tu base de datos:

SQL
USE NombreDeTuBaseDeDatos;
GO
Ejecuta los scripts de reporte o análisis deseados presionando F5.

👤 Autor
Gabriel Wetzel

GitHub: @wetzelgabriel18