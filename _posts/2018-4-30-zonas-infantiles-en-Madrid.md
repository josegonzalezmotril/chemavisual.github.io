---
layout: post
title: Zonas infantiles en Madrid
---
#### Visualización de las zonas infantiles en el mapa de Madrid

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
  
  
  Ahora sí lo vemos más claro. Visualizamos las zonas donde se concentran las zonas infantiles. Podemos ver que los barrios periféricos concentran mas zonas que el centro de Madrid, en el que vemos un hueco clarísimo. ¿Pero corresponde con la población infantil de Madrid?
  
  ### habitantes de 0 a 14 años	
## Barajas	8212
## moratalaz	10771
## entro	10888
## vicálvaro	12977
## retiro	14003
## chamberi	14387
## salamanca	15637
## moncloa - aravaca	16225
## tetuan	17590
## chamartin	18674
## arganzuela	18785
## villa de vallecas	19409
## usera	20364
## villaverde	22745
## San Blas - Canillejas	23537
## ciudad lineal	26148
## latina	27761
## hortaleza	30004
## puente de vallecas	30368
## carabanchel	35024
## fuencarral - el pardo	40398


