<div align="justify">
# proyecto_bae

<div align="center">
<img src="https://img.kytary.com/eshop_es/velky_v2/na/637069143475800000/8a1f6ebd/64694457/roland-groovebox-mc-707.jpg" width="500px"/>
</div>

### Descripción del problema.

   A día de hoy los DJ's producen mezclas con música de artistas conocidos mundialmente, con mezclas que crean ellos mismos o mezclas pre-producidas por ellos mismos u otros DJ's. En el caso de las mezclas pre-producidas, los DJ's se encuentran en el dilema de poder encontrar esas mezclas y que además tengan una buena calidad.

  ### Objetivo.

  GrooveBox ofrece la posibilidad a los DJ's una plataforma en la que poder publicar sus mezclas para almacenarlas y ofrecerlas a otros DJ's para que puedan utilizar estas como recursos musicales de una manera cómoda, además de ofrecer una API REST para los distintos software de mezcla como RekordBox o Serato DJ para que estos puedan ofrecer consumo de mezclas sin necesidad descargar nada manualmente.

  
  ### Arquitectura y tecnologías a utilizar.
  
  GrooveBox se constará de cuatro servicios. Un servicio para la plataforma web desarrollada en Laravel junto al framework Livewire, que consume de un servicio de un servidor de archivos de Amazon Web Service S3 y un Servidor de Base de Datos MYSQL. También los desarrolladores tendrán opción a consumir un servicio API REST mediante tokens. Cada servicio sería desplegado con Docker exceptuando el de Amazon, que estaría desplegado directamente por Amazon.

  ## Solución a implementar

- Detecta y/o define al menos 8 tablas en la solución propuesta, __sin tener en cuenta las relaciones__.
- [Crea el Diagrama ER](../../ER/README.md)
- [Crea el Modelo Relacional](../../MR/README.md)
- [Realiza y justifica la Normalización de la BBDD](../../NORMALIZACION/README.md)
- [Programa la inclusión de elementos en la BBDD](../../PROGRAMACION/README.md)


  </div>


</div>
