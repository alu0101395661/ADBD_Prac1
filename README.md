# ADBD_Prac1: Conceptos fundamentales de PostgreSQL

## 1. Creación de la base de datos
CREATE DATABASE biblioteca;

## 2.1 Usuario admin_biblio con sus permisos
CREATE ROLE admin_biblio WITH LOGIN PASSWORD 'adminpass';
GRANT ALL PRIVILEGES ON DATABASE biblioteca TO admin_biblio;

## 2.2 Usuario usuario_biblio 
CREATE ROLE usuario_biblio WITH LOGIN PASSWORD 'usuariopass';

## 2.3 Crear un rol llamado lectores con permisos únicamente de consulta sobre todas las tablas de la base de datos
CREATE ROLE lectores NOLOGIN;

## 2.4 Asignar el usuario usuario_biblio a este rol
GRANT lectores TO usuario_biblio;

## 2.5 Cambiar la contraseña del usuario usuario_biblio
ALTER ROLE usuario_biblio WITH PASSWORD 'nueva_contra';

## 2.6 Consultar las tablas del sistema para listar los usuarios creados
SELECT rolname, rolsuper, rolcreaterole, rolcreatedb, rolcanlogin FROM pg_roles ORDER BY rolname;

## 3.1. Crear TABLA AUTORES autores(id_autor, nombre, nacionalidad)
CREATE TABLE autores (id_autor SERIAL PRIMARY KEY, nombre TEXT NOT NULL, nacionalidad TEXT);

## 3.2. Crear TABLA LIBROS libros(id_libro, titulo, anyo_publicacion, id_autor)
CREATE TABLE libros(id_libro SERIAL PRIMARY KEY, titulo TEXT NOT NULL, anyo_publicacion INT, id_autor INT NOT NULL, CONSTRAINT fk_autor FOREIGN KEY(id_autor) REFERENCES autores(id_autor));

## 3.3. Crear TABLA PRESTAMOS prestamos(id_prestamo, id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario)
CREATE TABLE prestamos(id_prestamo SERIAL PRIMARY KEY,id_libro INT NOT NULL, fecha_prestamo DATE NOT NULL, fecha_devolucion DATE, usuario_prestatario TEXT NOT NULL, CONSTRAINT fk_libro FOREIGN KEY (id_libro) REFERENCES libros(id_libro));

## 4. Insertar al menos 5 autores, 8 libros y 5 préstamos
INSERT INTO autores(id_autor, nombre, nacionalidad) VALUES (1, 'Miguel de Cervantes', 'España');

INSERT INTO autores(id_autor, nombre, nacionalidad) VALUES (2, 'Gabriel García Marquez', 'Colombia');

INSERT INTO autores(id_autor, nombre, nacionalidad) VALUES (3, 'Mary Shelley', 'Reino Unido');

INSERT INTO autores(id_autor, nombre, nacionalidad) VALUES (4, 'Isabel Allende', 'Chile');

INSERT INTO autores(id_autor, nombre, nacionalidad) VALUES (5, 'Terry Pratchet', 'Reino Unido');

INSERT INTO libros (id_libro, titulo, anyo_publicacion, id_autor) VALUES (1, ‘Cien años de soledad’, 1967, 2);

INSERT INTO libros (id_libro, titulo, anyo_publicacion, id_autor) VALUES (2, ‘Don Quijote de la Mancha’, 1605, 1);

INSERT INTO libros (id_libro, titulo, anyo_publicacion, id_autor) VALUES (3, ‘Frankenstein’, 1818, 3);

INSERT INTO libros (id_libro, titulo, anyo_publicacion, id_autor) VALUES (4, ‘La casa de los espíritus’, 1982, 4);

INSERT INTO libros (id_libro, titulo, anyo_publicacion, id_autor) VALUES (5, ‘Guardias, Guardias’, 1989, 5);

INSERT INTO libros (id_libro, titulo, anyo_publicacion, id_autor) VALUES (6, ‘Mort’, 1987, 5);

INSERT INTO libros (id_libro, titulo, anyo_publicacion, id_autor) VALUES (7, ‘El Segador’, 1991, 5);

INSERT INTO libros (id_libro, titulo, anyo_publicacion, id_autor) VALUES (8, ‘De Amor y de Sombra’, 1984, 4);

INSERT INTO prestamos(id_prestamo, id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES( 1, 6, ‘2026-02-14’, ‘2026-05-17’, Carla);

INSERT INTO prestamos(id_prestamo, id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES( 2, 4, ‘2026-02-17’, ‘2026-04-01’, Pablo);

INSERT INTO prestamos(id_prestamo, id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES( 3, 5, ‘2026-03-23’, ‘2026-04-03’, Yurena);

INSERT INTO prestamos(id_prestamo, id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES( 4, 1, ‘2026-03-29’, Jesus);

INSERT INTO prestamos(id_prestamo, id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES( 5, 3, ‘2026-05-23’, ‘2026-06-25’, Irene);

INSERT INTO prestamos(id_prestamo, id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES( 6, 8, ‘2026-06-23’, Lara);

## 5.1 Listar todos los libros con sus autores correspondientes
SELECT libros.titulo, libro.anyo_publicacion, autores.nombre AS autor FROM libros JOIN autores ON libros.id_autor = autores.id_autor;

##5.2 Autores con más de un libro registrado
SELECT autores.nombre FROM autores JOIN libros ON autores.id_autor = libros.id_autor GROUP BY autores.nombre HAVING COUNT(*) > 1;

##5.3 Mostrar los Préstamos que aún no tienen fecha de devolución
SELECT * FROM prestamos WHERE fecha_devolucion IS NULL;

##6.1 Calcular el número total de préstamos realizados
SELECT COUNT(*) AS total_prestamos FROM prestamos;

##6.2 Obtener el número de libros prestados por cada usuario
SELECT usuario_prestatario, COUNT(*) AS libros_prestados FROM prestamos GROUP BY usuario_prestatario;

##7.1 Actualizar la fecha de devolución de un préstamo pendiente.
UPDATE prestamos SET fecha_devolucion = CURRENT_DATE WHERE id_prestamo = 1 AND fecha_devolucion IS NULL;

##7.2 Eliminar un libro y comprobar el efecto en la tabla de préstamos (usar ON DELETE CASCADE o justificar el comportamiento)

##8.1 Crear una vista llamada vista_libros_prestados que muestre: título del libro, autor y nombre del prestatario

##8.2 Conceder permisos de consulta sobre esta vista únicamente a usuario_biblio. 

##9.1 Crear una función que reciba el nombre de un autor y devuelva todos los libros escritos por él. 

##9.2 Crear una consulta que devuelva los tres libros más prestados. 

##10.1 Exportar el contenido de la tabla libros a un archivo CSV. 

##10.2 Importar datos adicionales de autores desde un archivo CSV externo. 
