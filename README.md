<div align="justify">
   
# proyecto_bae_dj

   <div align="center">
   <img src="https://img.kytary.com/eshop_es/velky_v2/na/637069143475800000/8a1f6ebd/64694457/roland-groovebox-mc-707.jpg" width="500px"/>
   </div>

   
# Índice:
1. [Descripción del problema](#problema)
2. [Objetivo](#objetivo)
3. [Arquitectura y tecnologías a utilizar.](#arquitectura)
4. [Solución a implementar](#solucion)
   - [Detecta y/o define al menos 8 tablas en la solución propuesta, sin tener en cuenta las relaciones.](#detecta)
   - [Diagrama Entidad-Relación](#entidad)
   - [Modelo Relacional.](#modelo)
   - [Normalización](#normalizacion)
      1. [Comprobar si se cumple la 1ª Forma Normal.](#1)
      2. [Comprobar si se cumple la 2ª Forma Normal.](#2)
      3. [Comprobar si se cumple la 3ª Forma Normal.](#3)
      4. [Genera el diagrama E/R resultante.](#4)
   - [Programa la inclusión de elementos en la BBDD](#programa)

<a name='problema'>

### Descripción del problema. </a>

   A día de hoy los DJ's producen mezclas con música de artistas conocidos mundialmente, con mezclas que crean ellos mismos o mezclas pre-producidas por ellos mismos u otros DJ's. En el caso de las mezclas pre-producidas, los DJ's se encuentran en el dilema de poder encontrar esas mezclas y que además tengan una buena calidad.
<a name='objetivo'>

### Objetivo. </a>

  GrooveBox ofrece la posibilidad a los DJ's una plataforma en la que poder publicar sus mezclas para almacenarlas y ofrecerlas a otros DJ's para que puedan utilizar estas como recursos musicales de una manera cómoda, además de ofrecer una API REST para los distintos software de mezcla como RekordBox o Serato DJ para que estos puedan ofrecer consumo de mezclas sin necesidad descargar nada manualmente.

<a name='arquitectura'>
  
### Arquitectura y tecnologías a utilizar. </a>
  
  GrooveBox se constará de cuatro servicios. Un servicio para la plataforma web desarrollada en Laravel junto al framework Livewire, que consume de un servicio de un servidor de archivos de Amazon Web Service S3 y un Servidor de Base de Datos MYSQL. También los desarrolladores tendrán opción a consumir un servicio API REST mediante tokens. Cada servicio sería desplegado con Docker exceptuando el de Amazon, que estaría desplegado directamente por Amazon.

<a name='solucion'>

## Solución a implementar. </a>
<a name='detecta'>

### Detecta y/o define al menos 8 tablas en la solución propuesta, sin tener en cuenta las relaciones. </a>
- Evento
- País
- Equipo
- Persona
   - DJ
   - Productor
- Contenido
   - Mezcla
   - Canción
- Categoría

<a name='entidad'>

### Diagrama Entidad-Relación. </a>
   
   <div align='center'>

   ![Diagrama Entidad-Relación](</img/modelo_entidad_relaci%C3%B3n.drawio.png>)

   </div>

<a name='modelo'>

### Modelo Relacional. </a>
   
   <div align='center'>

   ![Modelo Relacional](</img/modelo_relacional.drawio.png>)

   </div>

<a name='normalizacion'>

### Normalización. </a>
   
<a name='1'>

#### 1. Comprobar si se cumple la 1ª Forma Normal. </a>
Nose cumple la primera forma normal ya que tenemos atributos multievaluados en la entidad eventos. Por tanto realizamos los siguientes cambios: El atriubuto multievaluado "fecha" lo convertimos en una nueva tabla donde le damos una "id" como primary key y luego le aportamos el los atributos de dia, mes y año.

<div align="center">

![Normalización](</img/Normalización.drawio.png>)

</div>

<a name='3'>

#### 2. Comprobar si se cumple la 2ª Forma Normal. </a>
La segunda forma normal se cumple ya que una relación está en segunda forma normal si y sólo si está en primera forma normal y todos los atributos que NO forman parte de la clave principal tienen dependencia funcional completa de ella, lo cual se cumple en nuestro diagrama.
   
<a name='5'> 

#### 3. Comprobar si se cumple la 3ª Forma Normal. </a>
En cuanto a la tercera forma normal, también se cumple ya que no existe transitividad entre los atributos de nuestras entidades.

<a name='8'>  

#### 4. Genera el diagrama E/R resultante. </a>
Una vez realizado los cambios y comprobadas que todas las formas normales se cumplen, actualizamos los diagramas ER y MR.

__DIAGRAMA ER:__
<div align="center">
<img src="img\modelo_entidad_relación-ER Corregido.drawio.png">
</div>

<br>

__DIAGRAMA MR:__
<div align="center">
<img src="img\modelo_entidad_relación-MR Corregido.drawio.png">
</div>

<a name='programa'>

### Programa la inclusión de elementos en la BBDD. </a>

1. Creamos nuestra base de datos en MySQL.

```sql
create database groovebox;

use groovebox;
```

2. Creamos la tabla necesaria para cada entidad.

```sql
create table pais (
   capital VARCHAR(20) PRIMARY KEY NOT NULL,
   nombre VARCHAR(20) DEFAULT "nombre",
   UNIQUE (capital)
);

create table fecha (
   id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
   dia INT DEFAULT NULL,
   mes INT DEFAULT NULL,
   año INT DEFAULT NULL
);

create table evento (
   nombre VARCHAR(20) PRIMARY KEY NOT NULL,
   aforo INT DEFAULT 0
);

create table fecha_pais_eventos (
  nombre_evento VARCHAR(20) NOT NULL,
  capital_pais VARCHAR(20) NOT NULL,
  id_fecha INT NOT NULL,
  FOREIGN KEY (nombre_evento) REFERENCES evento(nombre),
  FOREIGN KEY (capital_pais) REFERENCES pais(capital),
  FOREIGN KEY (id_fecha) REFERENCES fecha(id),
  PRIMARY KEY (nombre_evento, capital_pais, id_fecha)
);

create table persona (
   id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
   nombre VARCHAR(20) DEFAULT "nombre",
   apellido VARCHAR(20) DEFAULT "apellido",
   genero ENUM ("H", "M") DEFAULT NULL,
   edad INT DEFAULT 0
);

create table dj (
   id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
   id_persona INT,
   FOREIGN KEY (id_persona) REFERENCES persona(id)
);

create table productor (
   id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
   id_persona INT,
   FOREIGN KEY (id_persona) REFERENCES persona(id)
);

create table evento_dj (
   id_evento VARCHAR(20),
   id_dj INT,
   FOREIGN KEY (id_evento) REFERENCES evento(nombre),
   FOREIGN KEY (id_dj) REFERENCES dj(id),
   PRIMARY KEY (id_evento, id_dj)
);

create table contenido (
   id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
   nombre VARCHAR(20) DEFAULT "nombre",
   duracion INT DEFAULT 0,
   genero VARCHAR(20) DEFAULT "sin especificar"
);

create table cancion (
   id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
   id_contenido INT,
   id_productor INT,
   FOREIGN KEY (id_contenido) REFERENCES contenido(id),
   FOREIGN KEY (id_productor) REFERENCES productor(id) 
);

create table categoria(
   nombre VARCHAR(20) PRIMARY KEY NOT NULL
);

create table mezcla (
   id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
   id_contenido INT,
   id_dj INT,
   nombre_categoria VARCHAR(20),
   puntaje INT DEFAULT NULL,
   FOREIGN KEY (id_contenido) REFERENCES contenido(id),
   FOREIGN KEY (id_dj) REFERENCES productor(id),
   FOREIGN KEY (nombre_categoria) REFERENCES categoria(nombre)  
);
```

3. Creamos varios procedimientos para la inserción de datos nuevos y aleatorios en las diferentes tablas.

__TABLA PERSONA:__
```sql
DELIMITER //
DROP PROCEDURE IF EXISTS insert_persona//
CREATE PROCEDURE insert_persona(IN num_inserts INT)
BEGIN
   DECLARE i INT DEFAULT 0;
   DECLARE nombres VARCHAR(20);
   DECLARE apellidos VARCHAR(20);
   DECLARE genero ENUM('H', 'M');
   DECLARE edad INT;

   WHILE i < num_inserts DO
      SET nombres = CONCAT('nombre', FLOOR(RAND() * num_inserts));
      SET apellidos = CONCAT('apellido', FLOOR(RAND() * num_inserts));
      SET genero = IF(FLOOR(RAND() * 2) = 0, 'H', 'M');
      SET edad = FLOOR(RAND() * 70);

   INSERT INTO persona (nombre, apellido, genero, edad) VALUES (nombres, apellidos, genero, edad);

   SET i = i + 1;
   END WHILE;
END //

DELIMITER ;
CALL insert_persona(50);

- - - Output - - -
mysql> select * from persona;
+-----+----------+------------+--------+------+
| id  | nombre   | apellido   | genero | edad |
+-----+----------+------------+--------+------+
|   1 | nombre99 | apellido87 | H      |   23 |
|   2 | nombre50 | apellido52 | H      |    1 |
|   3 | nombre71 | apellido52 | H      |   58 |
|   4 | nombre76 | apellido28 | H      |   54 |
|   5 | nombre52 | apellido28 | M      |   25 |
|.............................................|
|  97 | nombre3  | apellido84 | H      |   64 |
|  98 | nombre31 | apellido82 | H      |   34 |
|  99 | nombre89 | apellido96 | H      |   64 |
| 100 | nombre10 | apellido77 | M      |   31 |
+-----+----------+------------+--------+------+
100 rows in set (0,00 sec)
```
Cómo podemos ver en los resultados de las inserciones aleatorias tenemos personas con edades por debajo de los 14 años, que será el mínimo que utilizaremos en la app. Esto, lo arreglaremos con el siguiente trigger:

```sql
DELIMITER //
DROP TRIGGER IF EXISTS check_edad_minima //
CREATE TRIGGER check_edad_minima
BEFORE INSERT ON persona
FOR EACH ROW
BEGIN
   IF NEW.edad < 14 THEN
      SET NEW.edad = 14;
   END IF;
END //
```
Realizamos otro insert de 50 personas y podemos ver varios con edades de 14 años, lo que nos permite comprobar que el trigger ha actuado.

```sql
+-----+----------+------------+--------+------+
| id  | nombre   | apellido   | genero | edad |
+-----+----------+------------+--------+------+
|.............................................|
| 130 | nombre12 | apellido9  | H      |   43 |
| 131 | nombre18 | apellido48 | M      |   66 |
| 132 | nombre22 | apellido18 | M      |   40 |
| 133 | nombre11 | apellido23 | M      |   65 |
| 134 | nombre32 | apellido21 | H      |   63 |
| 135 | nombre40 | apellido16 | H      |   67 |
| 136 | nombre12 | apellido18 | H      |   21 |
| 137 | nombre15 | apellido31 | H      |   14 |
| 138 | nombre5  | apellido7  | H      |   32 |
| 139 | nombre8  | apellido22 | M      |   30 |
| 140 | nombre45 | apellido12 | M      |   65 |
| 141 | nombre3  | apellido27 | M      |   14 |
| 142 | nombre37 | apellido26 | H      |   14 |
| 143 | nombre43 | apellido40 | H      |   46 |
| 144 | nombre4  | apellido23 | H      |   54 |
| 145 | nombre38 | apellido24 | H      |   29 |
| 146 | nombre28 | apellido30 | H      |   63 |
| 147 | nombre23 | apellido32 | M      |   14 |
| 148 | nombre19 | apellido23 | H      |   33 |
| 149 | nombre43 | apellido44 | M      |   39 |
| 150 | nombre14 | apellido39 | H      |   52 |
+-----+----------+------------+--------+------+
150 rows in set (0,00 sec)
```
__TABLA CONTENIDO:__
```sql
DELIMITER //
DROP PROCEDURE IF EXISTS insert_contenido//
CREATE PROCEDURE insert_contenido(IN num_inserts INT)
BEGIN
  DECLARE i INT DEFAULT 0;
  DECLARE nombres VARCHAR(20);
  DECLARE duracion_minutos INT;
  DECLARE genero VARCHAR(20);
  
  WHILE i < num_inserts DO
    SET nombres = CONCAT('nombre', FLOOR(RAND() * num_inserts));
    SET duracion_minutos = FLOOR(RAND() * 180) + 2;
    SET genero = CASE FLOOR(RAND() * 3)
        WHEN 0 THEN 'House'
        WHEN 1 THEN 'Reggaeton'
        WHEN 2 THEN 'Hardstyle'
        ELSE 'Sin especificar'
      END;
    
    INSERT INTO contenido (nombre, duracion, genero)
    VALUES (nombres, duracion_minutos, genero);
    
    SET i = i + 1;
  END WHILE;
END //

DELIMITER ;
CALL insert_contenido(100);

- - - Output - - -
mysql> select* from contenido;
+-----+----------+----------+-----------+
| id  | nombre   | duracion | genero    |
+-----+----------+----------+-----------+
|   1 | nombre67 |       28 | Hardstyle |
|   2 | nombre7  |       52 | House     |
|   3 | nombre94 |       51 | Reggaeton |
|   4 | nombre80 |       79 | Hardstyle |
|   5 | nombre41 |      157 | House     |
|   6 | nombre72 |       84 | House     |
|   7 | nombre10 |       41 | Hardstyle |
|   8 | nombre32 |       47 | House     |
|   9 | nombre61 |       49 | Reggaeton |
|  10 | nombre49 |       18 | Hardstyle |
|.......................................|
|  90 | nombre18 |      118 | Hardstyle |
|  91 | nombre58 |      142 | House     |
|  92 | nombre52 |       23 | House     |
|  93 | nombre69 |       84 | House     |
|  94 | nombre64 |      108 | House     |
|  95 | nombre37 |      142 | Hardstyle |
|  96 | nombre61 |      124 | Reggaeton |
|  97 | nombre84 |       92 | Hardstyle |
|  98 | nombre37 |      169 | Reggaeton |
|  99 | nombre89 |      155 | Reggaeton |
| 100 | nombre37 |       24 | Reggaeton |
+-----+----------+----------+-----------+
100 rows in set (0,00 sec)
```
__TABLA DJ:__
```sql
DELIMITER //
DROP PROCEDURE IF EXISTS insert_dj//
CREATE PROCEDURE insert_dj(IN num_inserts INT)
BEGIN
   DECLARE i INT DEFAULT 0;
   DECLARE first_available_id INT;
   
   WHILE i < num_inserts DO
      SELECT MIN(id) INTO first_available_id FROM persona WHERE id NOT IN (SELECT id_persona FROM productor) AND id NOT IN (SELECT id_persona FROM dj);
      INSERT INTO dj (id_persona) VALUES (first_available_id);

      SET i = i + 1;
   END WHILE;
END //

DELIMITER ;
CALL insert_dj(10);

- - - Output - - -
mysql> select * from dj;
+----+------------+
| id | id_persona |
+----+------------+
|  1 |          1 |
|  2 |          2 |
|  3 |          3 |
|  4 |          4 |
|  5 |          5 |
|  6 |          6 |
|  7 |          7 |
|  8 |          8 |
|  9 |          9 |
| 10 |         10 |
+----+------------+
10 rows in set (0,00 sec)
```
__TABLA PRODUCTOR:__
```sql
DELIMITER //
DROP PROCEDURE IF EXISTS insert_productor//
CREATE PROCEDURE insert_productor(IN num_inserts INT)
BEGIN
   DECLARE i INT DEFAULT 0;
   DECLARE first_available_id INT;
   
   WHILE i < num_inserts DO
      SELECT MIN(id) INTO first_available_id FROM persona WHERE id NOT IN (SELECT id_persona FROM productor) AND id NOT IN (SELECT id_persona FROM dj);
      INSERT INTO productor (id_persona) VALUES (first_available_id);

      SET i = i + 1;
   END WHILE;
END //

DELIMITER ;
CALL insert_productor(10);

- - - Ouput - - -
mysql> select * from productor;
+----+------------+
| id | id_persona |
+----+------------+
|  1 |         11 |
|  2 |         12 |
|  3 |         13 |
|  4 |         14 |
|  5 |         15 |
|  6 |         16 |
|  7 |         17 |
|  8 |         18 |
|  9 |         19 |
| 10 |         20 |
+----+------------+
10 rows in set (0,00 sec)
```

__TABLA CANCIÓN:__
```sql
DELIMITER //
DROP PROCEDURE IF EXISTS insert_cancion//
CREATE PROCEDURE insert_cancion(IN num_inserts INT)
BEGIN

      DECLARE first_available_id_contenido INT;
      DECLARE first_available_id_productor INT;
      DECLARE i INT DEFAULT 0;

   WHILE i < num_inserts DO
      SELECT MIN(id) INTO first_available_id_contenido FROM contenido WHERE id NOT IN (SELECT id_contenido FROM cancion);
      SELECT MIN(id) INTO first_available_id_productor FROM productor WHERE id NOT IN (SELECT id_productor FROM cancion);
      INSERT INTO cancion (id_contenido, id_productor) VALUES (first_available_id_contenido, first_available_id_productor);

      SET i = i + 1;
   END WHILE;
END //

DELIMITER ;
CALL insert_cancion(10);

- - - Output - - -
mysql> select * from cancion;
+----+--------------+--------------+
| id | id_contenido | id_productor |
+----+--------------+--------------+
|  2 |            1 |            1 |
|  3 |            2 |            2 |
|  4 |            3 |            3 |
|  5 |            4 |            4 |
|  6 |            5 |            5 |
|  7 |            6 |            6 |
|  8 |            7 |            7 |
|  9 |            8 |            8 |
| 10 |            9 |            9 |
| 11 |           10 |           10 |
+----+--------------+--------------+
10 rows in set (0,00 sec)
```
__TABLA CATEGORÍA:__
Para la tabla categoría solo insertaremos 3 datos ya que su único atributo es su clave primaria y ya están predefinidas como "Buena", "Media" y "Mala".
```sql
INSERT INTO categoria VALUES("Buena");
INSERT INTO categoria VALUES("Media");
INSERT INTO categoria VALUES("Mala");
```

__TABLA MEZCLA:__
```sql
DELIMITER //
DROP PROCEDURE IF EXISTS insert_mezcla//
CREATE PROCEDURE insert_mezcla(IN num_inserts INT)
BEGIN
   DECLARE i INT DEFAULT 0;
   DECLARE id_contenido INT;
   DECLARE id_dj INT;
   DECLARE nombre_categoria VARCHAR(20);
   DECLARE puntaje INT;

   WHILE i < num_inserts DO
      SELECT id INTO id_contenido FROM contenido ORDER BY RAND() LIMIT 1;
      SELECT id_persona INTO id_dj FROM dj ORDER BY RAND() LIMIT 1;
      SELECT nombre INTO nombre_categoria FROM categoria ORDER BY RAND() LIMIT 1;

      SET puntaje = CASE nombre_categoria
         WHEN 'Buena' THEN FLOOR(RAND() * 2) + 4
         WHEN 'Media' THEN 3
         WHEN 'Mala' THEN FLOOR(RAND() * 2) + 1
         ELSE "Sin especificar"
      END;

      INSERT INTO mezcla (id_contenido, id_dj, nombre_categoria, puntaje) VALUES (id_contenido, id_dj, nombre_categoria, puntaje);

      SET i = i + 1;
   END WHILE;
END //

DELIMITER ;
CALL insert_mezcla(10);

mysql> select * from mezcla;
+----+--------------+-------+------------------+---------+
| id | id_contenido | id_dj | nombre_categoria | puntaje |
+----+--------------+-------+------------------+---------+
| 21 |           31 |     2 | Mala             |       2 |
| 22 |           26 |     9 | Buena            |       5 |
| 23 |           95 |     4 | Buena            |       4 |
| 24 |           98 |     4 | Mala             |       1 |
| 25 |           36 |     1 | Buena            |       5 |
| 26 |           87 |     5 | Mala             |       2 |
| 27 |           69 |    10 | Media            |       3 |
| 28 |           42 |     1 | Mala             |       2 |
| 29 |           76 |     9 | Buena            |       5 |
| 30 |           41 |     2 | Buena            |       5 |
+----+--------------+-------+------------------+---------+
10 rows in set (0,00 sec)

```

__TABLA EVENTO:__

```sql
DELIMITER //
DROP PROCEDURE IF EXISTS insert_evento//
CREATE PROCEDURE insert_evento(IN num_inserts INT)
BEGIN
   DECLARE i INT DEFAULT 0;
   DECLARE nombre VARCHAR(20);
   DECLARE aforo INT;

   WHILE i < num_inserts DO
      SET nombre = CONCAT("nombre", i);
      SET aforo = FLOOR(RAND() * 20000);

      INSERT INTO evento (nombre, aforo) VALUES (nombre, aforo);
      SET i = i + 1;
   END WHILE;
END//

DELIMITER ;
CALL insert_evento(10);

- - - Output - - -
mysql> select * from evento;
+---------+-------+
| nombre  | aforo |
+---------+-------+
| nombre0 | 19160 |
| nombre1 | 14553 |
| nombre2 | 15286 |
| nombre3 | 12771 |
| nombre4 | 17997 |
| nombre5 | 11672 |
| nombre6 |  4371 |
| nombre7 |  6837 |
| nombre8 |  1076 |
| nombre9 |  4867 |
+---------+-------+
10 rows in set (0,00 sec)
```
__TABLA EVENTO_DJ:__

```sql
DELIMITER //
DROP PROCEDURE IF EXISTS insert_evento_dj//
CREATE PROCEDURE insert_evento_dj(IN num_inserts INT)
BEGIN
   DECLARE i INT DEFAULT 0;
   DECLARE evento_nombre VARCHAR(9);
   DECLARE dj_id INT;

   WHILE i < num_inserts DO
      SELECT nombre INTO evento_nombre FROM evento
      WHERE nombre NOT IN (SELECT id_evento FROM evento_dj) ORDER BY RAND() LIMIT 1;

      SELECT id INTO dj_id FROM dj
      WHERE id NOT IN (SELECT id_dj FROM evento_dj) ORDER BY RAND() LIMIT 1;

      INSERT INTO evento_dj (id_evento, id_dj) VALUES (evento_nombre, dj_id);

      SET i = i + 1;
   END WHILE;
END //

DELIMITER ;
CALL insert_evento_dj(10);

- - - Output - - -
mysql> select * from evento_dj;
+-----------+-------+
| id_evento | id_dj |
+-----------+-------+
| nombre7   |     1 |
| nombre3   |     2 |
| nombre4   |     3 |
| nombre0   |     4 |
| nombre2   |     5 |
| nombre9   |     6 |
| nombre8   |     7 |
| nombre1   |     8 |
| nombre5   |     9 |
| nombre6   |    10 |
+-----------+-------+
10 rows in set (0,00 sec)

</div>


