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

### Solución a implementar. </a>
<a name='detecta'>

#### Detecta y/o define al menos 8 tablas en la solución propuesta, sin tener en cuenta las relaciones. </a>
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
- Equipo
- Tipo 
   - Iluminación
   - Audio
   - Efectos

<a name='entidad'>

#### Diagrama Entidad-Relación. </a>
   
   <div align='center'>

   ![Diagrama Entidad-Relación](</img/modelo_entidad_relaci%C3%B3n.drawio.png>)

   </div>

<a name='modelo'>

#### Modelo Relacional. </a>
   
   <div align='center'>

   ![Modelo Relacional](</img/modelo_relacional.drawio.png>)

   </div>

<a name='normalizacion'>

#### Normalización. </a>
   
<a name='1'>

##### 1. Comprobar si se cumple la 1ª Forma Normal. </a>
No se cumple la primera forma normal ya que tenemos atributos multievaluados en la entidad eventos. Por tanto realizamos los siguientes cambios:

<div align="center">

![Normalización](</img/Normalización.drawio.png>)

</div>

<a name='3'>

##### 3. Comprobar si se cumple la 2ª Forma Normal. </a>
La segunda forma normal se cumple ya que una relación está en segunda forma normal si y sólo si está en primera forma normal y todos los atributos que NO forman parte de la clave principal tienen dependencia funcional completa de ella, lo cual se cumple en nuestro diagrama.
   
<a name='5'> 

##### 5. Comprobar si se cumple la 3ª Forma Normal. </a>
En cuanto a la tercera forma normal, también se cumple ya que no existe transitividad entre los atributos de nuestras entidades.

<a name='8'>  

##### 8. Genera el diagrama E/R resultante. </a>
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

#### Programa la inclusión de elementos en la BBDD. </a>

</div>
