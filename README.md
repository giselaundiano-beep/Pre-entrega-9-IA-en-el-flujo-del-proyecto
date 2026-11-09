# Pre-entrega-9-IA-en-el-flujo-del-proyecto
Proyecto RetailPro — Análisis y Consultas SQL Server
Este repositorio contiene scripts y ejercicios prácticos de T-SQL desarrollados sobre el modelo de datos de RetailPro (Tablas: Ventas, Clientes y Productos) utilizando SQL Server Management Studio (SSMS). El objetivo es consolidar habilidades de analítica comercial, manejo de subconsultas, uniones (JOINs) y funciones de agregación.
🛠️ Herramientas Utilizadas
•	SQL Server Management Studio (SSMS): Entorno de desarrollo para la ejecución de consultas T-SQL.
•	T-SQL (Transact-SQL): Lenguaje de consulta para estructurar agregaciones y lógicas relacionales.
•	Git y GitHub: Control de versiones y publicación del repositorio público.
🚀 Cómo Ejecutar los Scripts SQL
1.	Abrir SQL Server Management Studio (SSMS) y conectarse a tu instancia de servidor local o remoto.
2.	Seleccionar la base de datos de trabajo correspondiente a RetailPro mediante el comando:
SQL
USE RetailPro;
GO
3.	Copiar y pegar el bloque de código de la consulta que deseas ejecutar (por ejemplo, el Resumen Ejecutivo Mensual o el Ranking de Productos).
4.	Presionar F5 o hacer clic en el botón Execute para correr el script y visualizar los resultados en la pestaña Results.
•	Enlace al repositorio público: https://github.com/giselaundiano-beep/Pre-entrega-9-IA-en-el-flujo-del-proyecto/blob/main/README.md
•	📋 Control de Cambios en los Scripts (Tabla con las 3 Mejoras de Código)
Script / Versión Original (Lo que pasaste)	Versión Modificada / Sugerencia de la IA	Por qué es mejor la sugerencia de la IA (Justificación técnica)
1. Subconsulta de Monto Vendido (Ejercicio 1):


SELECT p.ID_PRODUCTO, p.NOMBRE_PRODUCTO, ISNULL( (SELECT SUM(v.MontoTotal), FROM dbo.VENTAS v WHERE v.ID_PRODUCTO = v.ID_PRODUCTO), 0 )	Versión Corregida:


SELECT p.ID_PRODUCTO, p.NOMBRE_PRODUCTO, ISNULL( (SELECT SUM(v.MontoTotal) FROM dbo.VENTAS v WHERE v.ID_PRODUCTO = p.ID_PRODUCTO), 0 )	Se eliminó la coma sobrante antes del FROM y se corrigió el error crítico de correlación en el WHERE (v.ID_PRODUCTO = v.ID_PRODUCTO por p.ID_PRODUCTO = v.ID_PRODUCTO), evitando que falle la ejecución en SQL Server. 
2. Ranking de Productos (Consulta 2):


SELECT TOP 5 id_producto, SUM (Cantidad) AS TOTAL_UNIDADES_VENDIDAS, SUM (Cantidad * Precio_unitario) AS TOTAL_FACTURADOFROM VentasGROUP BY (Id_Producto)ORDER BY (TOTAL_FACTURADO) DESC;	Versión Optimizada:


SELECT TOP 5 id_producto, SUM(Cantidad) AS TOTAL_UNIDADES_VENDIDAS, SUM(Cantidad * Precio_unitario) AS TOTAL_FACTURADO FROM Ventas GROUP BY Id_Producto ORDER BY TOTAL_FACTURADO DESC;	Se limpiaron los paréntesis redundantes en el GROUP BY y en el ORDER BY y se estandarizó el espaciado, mejorando notablemente la legibilidad y cumpliendo con las buenas prácticas de sintaxis en T-SQL.
3. Cálculo de Productos y Ventas (Ejercicio 3 - LEFT JOIN):


Versión original basada en subconsulta escalar fila por fila:


SELECT p.ID_PRODUCTO, (SELECT SUM(MontoTotal) FROM Ventas v WHERE v.ID_PRODUCTO = p.ID_PRODUCTO) FROM Productos p	Sugerencia Óptima (LEFT JOIN + GROUP BY):


SELECT p.ID_PRODUCTO, p.NOMBRE_PRODUCTO, ISNULL(SUM(v.MontoTotal), 0) AS TotalVendido FROM dbo.PRODUCTOS p LEFT JOIN dbo.VENTAS v ON p.ID_PRODUCTO = v.ID_PRODUCTO GROUP BY p.ID_PRODUCTO, p.NOMBRE_PRODUCTO;	La sugerencia de la IA reemplaza la subconsulta escalar (que procesa los datos fila por fila y es ineficiente en bases de datos grandes) por un LEFT JOIN con GROUP BY, optimizando drásticamente el rendimiento del motor de base de datos.




Tarea 4 — Reflexión Escribí un párrafo explicando en qué momento del proyecto la IA fue más útil y en qué momento tu criterio analítico fue más importante que su respuesta.
Durante el desarrollo de este proyecto con RetailPro, la asistencia de la IA fue especialmente útil en las fases iniciales de estructuración y depuración técnica, sobre todo al corregir rápidamente errores de sintaxis y correlación en las consultas o al estructurar con agilidad el formato base del README.md. Sin embargo, mi criterio analítico fue mucho más importante que su respuesta en el momento de interpretar el sentido comercial de los datos y al definir qué métricas realmente aportaban valor estratégico. Mientras la IA proveía la estructura lógica de los scripts, fui yo quien debió contextualizar los hallazgos para el equipo comercial, detectando los matices de la operación y decidiendo qué optimizaciones técnicas e interpretaciones de negocio debían priorizarse para que el análisis fuera verdaderamente útil y preciso.

