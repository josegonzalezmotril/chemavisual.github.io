---
layout: post
title: Zonas infantiles en Madrid
---
#### Visualización de las zonas infantiles en el mapa de Madrid

![columpios](https://github.com/josegonzalezmotril/josegonzalezmotril.github.io/blob/master/images/infantil.jpg?raw=true)

Hoy voy a continuar con mapas de la ciudad de Madrid, donde vamos a visualizar datos sacados del Ayuntamiento. En este caso las zonas infantiles de las que dispone la capital.

Para este mapa he usado R para visualizar los datos. Parece complicado, pero con unas pocas sentencias se pueden hacer visualizaciones muy chulas. vamos a por la primera:

'''r
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
  '''
