# 🧠 Resumen Introducción a SQL

## 📊 Tipos de Datos y Atributos en SQL

### Atributos que podemos aplicar a los campos
- `NOT NULL` → No permite valores nulos. El campo es obligatorio.
- `NULL` → Permite valores nulos. Es el valor por defecto.
- `DEFAULT` → Asigna un valor por defecto si no se especifica uno.
- `AUTO_INCREMENT` → Aumenta automáticamente (solo numéricos). Ideal para IDs.
- `UNIQUE` → Garantiza que los valores no se repitan en la columna.
- `PRIMARY KEY` → Identifica de forma única a cada fila. Implica `NOT NULL` y `UNIQUE`.
- `FOREIGN KEY` → Define una relación con otra tabla.
- `CHECK` → Restringe los valores permitidos (ej: `CHECK (edad >= 18)`).
- `COMMENT` → Permite añadir un comentario sobre el campo.

### Numéricos:
- `INT`, `INTEGER` → Enteros estándar, hasta ±2 mil millones.
- `SMALLINT` → Enteros más pequeños, ideal para datos con pocos valores posibles.
- `BIGINT` → Enteros muy grandes, hasta ±9 cuatrillones. Útil para grandes volúmenes o IDs únicos.
- `DECIMAL(p,s)` / `NUMERIC(p,s)` → Precisión exacta. Perfecto para dinero y cálculos financieros.
- `FLOAT`, `REAL` → Precisión flotante simple. Menor precisión, mayor velocidad.
- `DOUBLE`, `DOUBLE PRECISION` → Mayor precisión que `FLOAT`, útil para ciencia o estadísticas.
- `BIT` o `BOOL` → Representa valores binarios. En MySQL puede ser usado como campo de bits o booleano (alias de `TINYINT(1)`).
- `TINYINT` → Entero muy pequeño (0 a 255 sin signo). Ideal para flags o rangos pequeños.
- `MEDIUMINT` → Entero mediano. Rango más amplio que `SMALLINT`, menor que `INT` (hasta ±8 millones).

### Texto:
- `CHAR(n)` → Longitud fija. Rellena con espacios si es más corto. De 1 a 255 caracteres.
- `VARCHAR(n)` → Longitud variable hasta `n`. Más flexible, muy usado. De 1 a 65535 caracteres dependiendo del charset.
- `TINYTEXT` → Longitud variable hasta 255 caracteres. Ideal para descripciones cortas.
- `TEXT` → Texto ilimitado. Hasta 65,535 caracteres. No ideal para índices.
- `MEDIUMTEXT` → Hasta 16,777,215 caracteres. Útil para textos largos como artículos o reportes.
- `LONGTEXT` (MySQL) → Para textos extensísimos (hasta 4GB). Ideal para logs, blobs de HTML o documentos.
- `BLOB` → Binary Large Object. Similar a `TEXT` pero para datos binarios (imágenes, archivos, etc.). No se puede indexar por contenido directamente.

### Fechas y Tiempos:
- `DATE` → Solo fecha (AAAA-MM-DD).
- `DATETIME` → Fecha y hora. Desde el 1 de enero del 1001 00:00:00 hasta 31 de diciembre de 9999 23:59:59.
- `TIME` → Solo hora (HH:MM:SS).
- `TIMESTAMP` → Fecha + hora. Muy usado para registros temporales.
- `INTERVAL` → Duración entre fechas u horas.

### Booleanos:
- `BOOLEAN` → `TRUE`, `FALSE`, `NULL`. En MySQL a veces representado como `TINYINT(1)`.

---

## 🕵️ CREATE: Crear DB/table
```sql
-- Para crear una DB
CREATE DATABASE NombreDB;

-- Para crear una tabla productos aplicando tipo de datos
CREATE TABLE Productos(
    idProducto INT(11) UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    Nombre VARCHAR(50) NOT NULL,
    Precio DOUBLE,
    Marca VARCHAR(30) NOT NULL,
    Categoria VARCHAR(30) NOT NULL,
    Stock INT(6) NOT NULL,
    Disponible BOOLEAN DEFAULT false
)
```

## 🕵️ SELECT: La Estrella de SQL

```sql
-- Seleccionar columnas específicas
SELECT columna1, columna2 FROM tabla;

-- Todas las columnas
SELECT * FROM tabla;

-- Con condiciones
SELECT * FROM empleados WHERE salario > 50000;

-- Ordenar resultados
SELECT * FROM productos ORDER BY precio DESC;

-- Limitar resultados
SELECT * FROM clientes LIMIT 10;

-- Eliminar duplicados
SELECT DISTINCT pais FROM clientes;

-- Funciones de agregación
SELECT AVG(precio), COUNT(*) FROM productos;
```

---

## 🤝 JOINs: Unión de Tablas

### 🔗 INNER JOIN
Devuelve solo los registros que coinciden en ambas tablas.
```sql
SELECT *
FROM empleados e
INNER JOIN departamentos d ON e.depto_id = d.id;
```

### 🧩 LEFT JOIN
Devuelve todos los registros de la izquierda y los que coinciden de la derecha (si hay).
```sql
SELECT *
FROM clientes c
LEFT JOIN pedidos p ON c.id = p.cliente_id;
```

### 🪞 RIGHT JOIN
Devuelve todos los registros de la derecha y los que coinciden de la izquierda.
```sql
SELECT *
FROM pedidos p
RIGHT JOIN clientes c ON p.cliente_id = c.id;
```

### 🧱 FULL OUTER JOIN
Devuelve todo, coincida o no. (No todos los motores lo soportan directamente).
```sql
SELECT *
FROM tablaA a
FULL OUTER JOIN tablaB b ON a.id = b.id;
```

---

## 🛠 Tips Extra
- Usa `AS` para alias: `SELECT nombre AS cliente_nombre`
- Filtros con `WHERE`, `BETWEEN`, `IN`, `LIKE`, `IS NULL`
- Agrupar con `GROUP BY` y filtrar grupos con `HAVING`

---

## 🚀 Consulta Final de Ejemplo
```sql
SELECT c.nombre, COUNT(p.id) AS cantidad_pedidos
FROM clientes c
LEFT JOIN pedidos p ON c.id = p.cliente_id
GROUP BY c.nombre
HAVING COUNT(p.id) > 5
ORDER BY cantidad_pedidos DESC;
```

---

