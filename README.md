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
   - [Programa la inclusión de elementos en la BBDD](#programa)
### Descripción del problema. <a name='problema'>

   A día de hoy los DJ's producen mezclas con música de artistas conocidos mundialmente, con mezclas que crean ellos mismos o mezclas pre-producidas por ellos mismos u otros DJ's. En el caso de las mezclas pre-producidas, los DJ's se encuentran en el dilema de poder encontrar esas mezclas y que además tengan una buena calidad.

  ### Objetivo. <a name='objetivo'>

  GrooveBox ofrece la posibilidad a los DJ's una plataforma en la que poder publicar sus mezclas para almacenarlas y ofrecerlas a otros DJ's para que puedan utilizar estas como recursos musicales de una manera cómoda, además de ofrecer una API REST para los distintos software de mezcla como RekordBox o Serato DJ para que estos puedan ofrecer consumo de mezclas sin necesidad descargar nada manualmente.

  
  ### Arquitectura y tecnologías a utilizar. <a name='arquitectura'>
  
  GrooveBox se constará de cuatro servicios. Un servicio para la plataforma web desarrollada en Laravel junto al framework Livewire, que consume de un servicio de un servidor de archivos de Amazon Web Service S3 y un Servidor de Base de Datos MYSQL. También los desarrolladores tendrán opción a consumir un servicio API REST mediante tokens. Cada servicio sería desplegado con Docker exceptuando el de Amazon, que estaría desplegado directamente por Amazon.

### Solución a implementar. <a name='solucion'>

#### Detecta y/o define al menos 8 tablas en la solución propuesta, sin tener en cuenta las relaciones. <a name='detecta'>
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
#### Diagrama Entidad-Relación. <a name='entidad'>
   
<div align='center'>
   
![Diagrama Entidad Relación](<https://github.com/samugd17/proyecto_bae/blob/main/img/modelo_entidad_relaci%C3%B3n-P%C3%A1gina-3.drawio.png>)

</div>
   
#### Modelo Relacionar. <a name='modelo'>
   
<div align='center'>
![Modelo Relacional](<https://github.com/samugd17/proyecto_bae/blob/main/img/modelo_entidad_relaci%C3%B3n-MR.drawio.png>)
   
#### Normalización. <a name='normalizacion'>
#### Programa la inclusión de elementos en la BBDD. <a name='programa'>


  </div>


</div>
