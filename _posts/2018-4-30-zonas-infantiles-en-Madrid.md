---
layout: post
title: Zonas infantiles en Madrid
---
## Visualización de las zonas infantiles en el mapa de Madrid

![columpios](https://github.com/josegonzalezmotril/josegonzalezmotril.github.io/blob/master/images/infantil.jpg?raw=true)

Hoy voy a continuar con mapas de la ciudad de Madrid, donde vamos a visualizar datos sacados del Ayuntamiento. En este caso las zonas infantiles de las que dispone la capital.

Para este mapa he usado R para visualizar los datos. Parece complicado, pero con unas pocas sentencias se pueden hacer visualizaciones muy chulas. vamos a por la primera:

```r
#instalamos los paquetes de mapas de google para R
install.packages(c("ggmap", "maps"))
library(ggmap)
library(readxl)

#cargamos nuestra base de datos
areas1 <- read_excel("areas_infantiles_ci5.xls")
names(areas1)

#generamos el mapa
mapa.madrid = get_map(location = "madrid", maptype = "terrain", zoom = 12)

#y lo visualizamos como mapa de puntos, donde alpha es la transparencia
ggmap(mapa.madrid,extent = "device")+
  geom_point(data = areas1, aes(x=longitud , y=latitud),colour="red", alpha= 0.15,size=2)
 ```


lo que nos produce este mapa:


![puntosinfantiles](https://github.com/josegonzalezmotril/josegonzalezmotril.github.io/blob/master/images/zonas%20infantiles.jpeg?raw=true)



En él vemos situadas todas las zonas infantiles de la ciudad. Es una visualización poco atractiva ya que, al ser tantas, no se diferencian entre sí. Pero podemos hacer algo más. Vamos a crear un mapa de calor también en R:


```r
mapa.madrid2 = get_map(location = "madrid", maptype = "terrain", zoom = 12)
ggmap(mapa.madrid2,extent="device")+
  geom_density2d(data=areas1,aes(x=longitud,y=latitud),size=.3)+
  stat_density2d(data = areas1,aes(x=longitud,y=latitud, fill=..level..,alpha=..level..),
                 size=0.01,bins=16,geom="polygon")+
  scale_fill_gradient(low="green", high="red")+
  scale_alpha(range=c(0.1,0.25),guide=F)
  ```
  
  
  con este resultado:
  
  
  ![mapacalor](https://github.com/josegonzalezmotril/josegonzalezmotril.github.io/blob/master/images/mapa%20de%20calor%20zonas%20infantiles.jpeg?raw=true)
  
  
  Ahora sí lo vemos más claro. Visualizamos las zonas donde se concentran las zonas infantiles. Podemos ver que los barrios periféricos concentran más zonas que el centro de Madrid, en el que vemos un hueco clarísimo. ¿Pero corresponde con la población infantil de Madrid?
  
  
  En este gráfico, creado en QLIK, vemos la población infantil de 0 a años en la ciudad dividida por distritos:
  
  
   ![poblacion](https://github.com/josegonzalezmotril/josegonzalezmotril.github.io/blob/master/images/poblacioninfaltil.png?raw=true)
   
   Efectivamente vemos que a zona centro de Madrid es la que menos población infantil tiene y por ello menos zonas infantiles hay. Pero en zonas como Villa de Vallecas a pesar de tener una población infantil alta, no hay una suficiente concentración de zonas de juego.
   
   En el primer post de mi blog comentaba la falta de zonas caninas en el centro. Hoy vemos que tampoco hay demasiadas zonas de juego. Obviamente es una zona donde se concentran oficinas y sedes de servicios públicos. Pero creo que si se pusiesen más zonas de esparcimiento podría convertirse en una zona muy grata para pasear con los niños y/o el perro durante el fin de semana y festivos, cuando esa zona queda más muerta.
  
  ¡Espero que os haya gustado!


