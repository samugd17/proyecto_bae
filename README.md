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
Nose cumple la primera forma normal ya que tenemos atributos multievaluados en la entidad eventos. Por tanto realizamos los siguientes cambios:

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
   id VARCHAR(10) PRIMARY KEY NOT NULL,
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
  id_fecha VARCHAR(10) NOT NULL,
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
</div>


