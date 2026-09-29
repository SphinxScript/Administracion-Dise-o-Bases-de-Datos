# Administración y diseño de bases de datos - Práctica 1
## Ricardo David Rodríguez Pane
### 1. Creación de la base de datos
* Crear una base de datos llamada `
### 2. Creación de usuarios y permisos
* Crear dos usuarios:
  * `admin_biblio`: con permisos de administrador sobre la base de datos.
  * `usuario_biblio`: con permisos solo de lectura.
* Crear un rol llamado `lectores` con permisos únicamente de consulta sobre todas las tablas de la base de datos.
* Asignar el usuario `usuario_biblio` a este rol.
* Consultar las tablas del sistema para listar todos los usuarios creados (`pg_roles`).
* Cambiar la contraseña del usuario `usuario_biblio`.
* Configurar permisos de tal forma que el usuario `usuario_biblio` no pueda eliminar registros en ninguna tabla.
```sql
CREATE USER admin_biblio WITH PASSWORD 'admin';
ALTER DATABASE NER TO admin_biblio;
GRANT ALL ON SCHEMA public TO admin_biblio ;
CREATE USER usuario_biblio;
CREATE ROLE lectores NOLOGIN;
GRANT CONNECT ON DATABASE  lectores;
GRANT USAGE ON SCHEMA public TO lectores ;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO lectores ;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO lectores ;
GRANT lectores TO usuario_biblio;
SELECT * FROM pg_roles WHERE rolname IN ('admin_biblio', 'usuario_biblio', 'lectores');
    rolname     | rolsuper | rolinherit | rolcreaterole | rolcreatedb | rolcanlogin | rolreplication | rolconnlimit | rolpassword | rolvaliduntil | rolbypassrls | rolconfig |  oid  
----------------+----------+------------+---------------+-------------+-------------+----------------+--------------+-------------+---------------+--------------+-----------+-------
 lectores       | f        | t          | f             | f           | f           | f              |           -1 | ********    |               | f            |           | 16400
 admin_biblio   | f        | t          | f             | f           | t           | f              |           -1 | ********    |               | f            |           | 16398
 usuario_biblio | f        | t          | f             | f           | t           | f              |           -1 | ********    |               | f            |           | 16399
(3 rows)
ALTER USER usuario_biblio WITH PASSWORD 'usuario';

```
### 3. Creación de tablas
* Crear las siguientes tablas con sus respectivas claves primarias:
  * `autores` (`id_autor`, `nombre`, `nacionalidad`)
  * `libros` (`id_libro`, `titulo`, `año_publicacion`, `id_autor`)
  * `prestamos` (`id_prestamo`, `id_libro`, `fecha_prestamo`, `fecha_devolucion`, `usuario_prestatario`)
* Establecer las claves foráneas correspondientes.
```sql
CREATE TABLE autores (
id_autor serial PRIMARY KEY,
nombre VARCHAR(50) NOT NULL,
nacionalidad VARCHAR(50) NOT NULL 
);
CREATE TABLE
CREATE TABLE libros
(
id_libro SERIAL PRIMARY KEY,
titulo VARCHAR(100) NOT NULL,
anio_publicacion SMALLINT,
id_autor INT REFERENCES autores(id_autor)
);
CREATE TABLE
CREATE TABLE prestamos(
id_prestamo SERIAL PRIMARY KEY,
id_libro INT REFERENCES libros(id_libro),
fecha_prestamo DATE NOT NULL,
fecha_devolucion DATE,
usuario_prestatario VARCHAR(50) NOT NULL
);
CREATE TABLE

```
### 4. Inserción de datos
* Insertar al menos 5 autores, 8 libros y 5 préstamos de ejemplo.
```sql
INSERT INTO autores (nombre, nacionalidad)
VALUES ('Gabriel García Márquez', 'Colombiano');
INSERT 0 1
INSERT INTO autores (nombre, nacionalidad)
VALUES ('Carlos Ruiz Zafón', 'Español');
INSERT 0 1
INSERT INTO autores (nombre, nacionalidad)
VALUES ('Camilo José Cela', 'Español');
INSERT 0 1
INSERT INTO autores (nombre, nacionalidad)
VALUES ('Julia Navarro', 'Español');
INSERT INTO autores (nombre, nacionalidad)
VALUES ('William Shakespeare', 'Inglés');
INSERT 0 1
SELECT * FROM autores;
 id_autor |         nombre         | nacionalidad 
----------+------------------------+--------------
        5 | Gabriel García Márquez | Colombiano
        6 | Carlos Ruiz Zafón      | Español
        7 | Camilo José Cela       | Español
        8 | Julia Navarro          | Español
        9 | William Shakespeare    | Inglés
(5 rows)

INSERT INTO libros (titulo, anio_publicacion, id_autor)
VALUES ('Cien años de soledad', '1967', '5');
INSERT 0 1
INSERT INTO libros (titulo, anio_publicacion, id_autor)
VALUES ('El principe de la niebla', '1993', '6');
INSERT 0 1
INSERT INTO libros (titulo, anio_publicacion, id_autor)
VALUES ('La biblia de barro', '2005', '8');
INSERT 0 1
INSERT INTO libros (titulo, anio_publicacion, id_autor)
VALUES ('Dime quién soy', '2010', '8');
INSERT 0 1
INSERT INTO libros (titulo, anio_publicacion, id_autor)
VALUES ('Dispara, yo ya estoy muerto', '2006', '8');
INSERT 0 1
INSERT INTO libros (titulo, anio_publicacion, id_autor)
VALUES ('El niño que perdio la guerra', '2024', '8');
INSERT 0 1
INSERT INTO libros (titulo, anio_publicacion, id_autor)
VALUES ('Romeo y julieta', '1597', '9');
INSERT 0 1
INSERT INTO libros (titulo, anio_publicacion, id_autor)
VALUES ('Hamlet', '1601', '9');
INSERT 0 1
select * from libros;
 id_libro |            titulo            | anio_publicacion | id_autor 
----------+------------------------------+------------------+----------
        1 | Cien años de soledad         |             1967 |        5
        2 | El principe de la niebla     |             1993 |        6
        3 | La biblia de barro           |             2005 |        8
        4 | Dime quién soy               |             2010 |        8
        5 | Dispara, yo ya estoy muerto  |             2006 |        8
        6 | El niño que perdio la guerra |             2024 |        8
(6 rows)

INSERT INTO prestamos (id_libro, fecha_prestamo, usuario_prestatario)
VALUES ('1', '15-01-2026', 'Genghis Kan');
INSERT 0 1
INSERT INTO prestamos (id_libro, fecha_prestamo, usuario_prestatario)
VALUES ('2', '15-06-2026', 'Mao Zedong');
INSERT 0 1
INSERT INTO prestamos (id_libro, fecha_prestamo, usuario_prestatario)
VALUES ('3', '18-09-2026', 'Sara Rodríguez Martín');
INSERT 0 1
INSERT INTO prestamos (id_libro, fecha_prestamo, usuario_prestatario)
VALUES ('7', '16-10-2025', 'Juan González González');
INSERT 0 1
INSERT INTO prestamos (id_libro, fecha_prestamo, usuario_prestatario)
VALUES ('5', '11-8-2026', 'Albert Einstein');
INSERT 0 1
```

### 5. Consultas básicas
* Listar todos los libros con su autor correspondiente.
* Mostrar los préstamos que aún no tienen fecha de devolución.
* Obtener los autores que tienen más de un libro registrado.
```sql
SELECT
libros.titulo,
autores.nombre AS autor,
libros.anio_publicacion
FROM libros
JOIN autores ON libros.id_autor = autores.id_autor;
            titulo            |         autor          | anio_publicacion 
------------------------------+------------------------+------------------
 Cien años de soledad         | Gabriel García Márquez |             1967
 El principe de la niebla     | Carlos Ruiz Zafón      |             1993
 La biblia de barro           | Julia Navarro          |             2005
 Dime quién soy               | Julia Navarro          |             2010
 Dispara, yo ya estoy muerto  | Julia Navarro          |             2006
 El niño que perdio la guerra | Julia Navarro          |             2024
 Romeo y julieta              | William Shakespeare    |             1597
 Hamlet                       | William Shakespeare    |             1601
(8 rows)

SELECT 
prestamos.id_prestamo,
prestamos.id_libro,
prestamos.usuario_prestatario
FROM prestamos
WHERE  fecha_devolucion IS NULL;
 id_prestamo | id_libro |  usuario_prestatario   
-------------+----------+------------------------
           1 |        1 | Genghis Kan
           2 |        2 | Mao Zedong
           3 |        3 | Sara Rodríguez Martín
           4 |        7 | Juan González González
           5 |        5 | Albert Einstein
(5 rows)

SELECT
autores.nombre,
COUNT(libros.id_libro) AS cantidad_libros
FROM autores
JOIN libros ON autores.id_autor = libros.id_autor
GROUP BY autores.id_autor
HAVING COUNT(libros.id_libro) > 1;
       nombre        | cantidad_libros 
---------------------+-----------------
 Julia Navarro       |               4
 William Shakespeare |               2
(2 rows)
```

### 6. Consultas con agregación
* Calcular el número total de préstamos realizados.
* Obtener el número de libros prestados por cada usuario.
```sql
SELECT
COUNT(*) AS prestamos_totales_hechos
FROM prestamos;
 prestamos_totales_hechos 
--------------------------
                        5
(1 row)

SELECT
usuario_prestatario,
COUNT(usuario_prestatario) AS veces_prestado
FROM prestamos
GROUP BY usuario_prestatario;
  usuario_prestatario   | veces_prestado 
------------------------+----------------
 Sara Rodríguez Martín  |              1
 Genghis Kan            |              1
 Mao Zedong             |              1
 Juan González González |              1
 Albert Einstein        |              1
(5 rows)
```

### 7. Modificación de datos
* Actualizar la fecha de devolución de un préstamo pendiente.
* Eliminar un libro y comprobar el efecto en la tabla de préstamos (usar `ON DELETE CASCADE` o justificar el comportamiento).
```sql
UPDATE prestamos
SET fecha_devolucion = '30-01-2026'
WHERE id_prestamo = '1' ;
UPDATE 1
SELECT * FROM prestamos
;
 id_prestamo | id_libro | fecha_prestamo | fecha_devolucion |  usuario_prestatario   
-------------+----------+----------------+------------------+------------------------
           2 |        2 | 2026-06-15     |                  | Mao Zedong
           3 |        3 | 2026-09-18     |                  | Sara Rodríguez Martín
           4 |        7 | 2025-10-16     |                  | Juan González González
           5 |        5 | 2026-08-11     |                  | Albert Einstein
           1 |        1 | 2026-01-15     | 2026-01-30       | Genghis Kan
(5 rows)
DELETE FROM
libros
WHERE id_libro = '1';
ERROR:  update or delete on table "libros" violates foreign key constraint "prestamos_id_libro_fkey" on table "prestamos"
DETAIL:  Key (id_libro)=(1) is still referenced from table "prestamos".
```
#### No nos deja eliminar dicha entrada porque el libro que queremos eliminar sigue referenciado por una clave foránea
#### en la tabla prestamos. Habríamos podido hacerlo si al crear la tabla añadieramos la cláusula ON DELETE CASCADE, de
#### manera que al borrar el libro se nos borrarian las entradas de otras tablas que apuntan a dicho libro como clave
#### foránea (borraría también la entrada de préstamo en ese caso).

### 8. Creación de vistas
* Crear una vista llamada `vista_libros_prestados` que muestre:
  * Título del libro
  * Autor
  * Nombre del prestatario
* Conceder permisos de consulta sobre esta vista únicamente a `usuario_biblio`.
```sql
CREATE VIEW vista_libros_prestados AS
SELECT libros.titulo,
autores.nombre,
prestamos.usuario_prestatario
FROM libros
JOIN autores ON autores.id_autor = libros.id_autor
JOIN prestamos ON prestamos.id_libro = libros.id_libro
WHERE prestamos.fecha_devolucion IS NULL;
CREATE VIEW
GRANT SELECT ON vista_libros_prestados TO usuario_biblio;
GRANT
```

### 9. Funciones y consultas avanzadas
* Crear una función que reciba el nombre de un autor y devuelva todos los libros escritos por él.
* Crear una consulta que devuelva los tres libros más prestados.
```sql
CREATE FUNCTION libros_escritos_por_autor(p_nombres_autor TEXT)
RETURNS TABLE (
id_libro INT,
titulo VARCHAR,
anio_publicacion SMALLINT
) AS
$$
BEGIN
RETURN QUERY
SELECT
libros.id_libro,
libros.titulo,
libros.anio_publicacion
FROM libros
JOIN autores ON autores.id_autor = libros.id_autor
WHERE autores.nombre = p_nombres_autor;
END;
$$ LANGUAGE plpgsql;
CREATE FUNCTION

SELECT
libros.titulo,
COUNT(prestamos.id_prestamo) AS total_prestamos
FROM libros
JOIN prestamos ON prestamos.id_libro = libros.id_libro
GROUP BY libros.id_libro
ORDER BY total_prestamos DESC
LIMIT 3;
           titulo            | total_prestamos 
-----------------------------+-----------------
 Dispara, yo ya estoy muerto |               1
 El principe de la niebla    |               1
 Romeo y julieta             |               1
(3 rows)
```

### 10. Exportación e importación de datos
* Exportar el contenido de la tabla `libros` a un archivo CSV.
* Importar datos adicionales de autores desde un archivo CSV externo.
```sql
\COPY libros TO '/home/usuario/libros.csv' DELIMITER ',' CSV HEADER;
COPY 8

\COPY autores(nombre, nacionalidad) FROM '/home/usuario/importar.csv' DELIMITER ',' CSV HEADER;
COPY 4
SELECT * FROM autores;
 id_autor |         nombre         |  nacionalidad  
----------+------------------------+----------------
        5 | Gabriel García Márquez | Colombiano
        6 | Carlos Ruiz Zafón      | Español
        7 | Camilo José Cela       | Español
        8 | Julia Navarro          | Español
        9 | William Shakespeare    | Inglés
       14 | Geroge Orwell          | Inglés
       15 | Oscar Wilde            | Irlandés
       16 | Brandon Sanderson      | Estadounidense
       17 | John Gwynne            | Inglés
(9 rows)
```
