# ADBD_Prac1: Conceptos fundamentales de PostgreSQL
## 1. Creación de la base de datos
CREATE DATABASE biblioteca;
## 2. Crear dos usuarios: admin_biblio con permisos de administrador en la base de datos y usuario_biblio con permisos de lectura
### Usuario admin_biblio con sus permisos
CREATE ROLE admin_biblio WITH LOGIN PASSWORD 'adminpass';
GRANT ALL PRIVILEGES ON DATABASE biblioteca TO admin_biblio;
### Usuario usuario_biblio 
CREATE ROLE usuario_biblio WITH LOGIN PASSWORD 'usuariopass';
### Crear un rol llamado lectores con permisos únicamente de consulta sobre todas las tablas de la base de datos
CREATE ROLE lectores NOLOGIN;
### Asignar el usuario usuario_biblio a este rol
GRANT lectores TO usuario_biblio;
### Cambiar la contraseña del usuario usuario_biblio
ALTER ROLE usuario_biblio WITH PASSWORD 'nueva_contra';
### Consultar las tablas del sistema para listar los usuarios creados
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

INSERT INTO prestamos(id_prestamo, id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES( 1, 6, ‘2026-02-14’, ‘2026-05-17’, Carla)
INSERT INTO prestamos(id_prestamo, id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES( 2, 4, ‘2026-02-17’, ‘2026-04-01’, Pablo)
INSERT INTO prestamos(id_prestamo, id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES( 3, 5, ‘2026-03-23’, ‘2026-04-03’, Yurena)
INSERT INTO prestamos(id_prestamo, id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES( 4, 1, ‘2026-03-29’, Jesus)
INSERT INTO prestamos(id_prestamo, id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES( 5, 3, ‘2026-05-23’, ‘2026-06-25’, Irene)
INSERT INTO prestamos(id_prestamo, id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES( 6, 8, ‘2026-06-23’, Lara)
