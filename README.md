```{r, eval=FALSE, include=TRUE}
"Protocolo:
 1. Daniel Felipe Villa Rengifo
 2. Lenguaje: R
 3. Tema: Correlación de Datos, Ilustrados en Graficas
 4. Fuentes:
    https://rpubs.com/camilamila/correlaciones"
```

# Matriz de Correlación:

Una matriz de correlación se utliza, para investigar la independencia entre multiples variables a la vez, el resultado de la tabla tiene el coeficiente de correlación, entre cada una de las vaiables que conforman el data frame.

En particular representar la matriz de correlación nos ayudara a entender, la diferencias entre esas variables, esto para mineria de datos ayudará muchisimo ya que así veremos que varibles vamos a utlizar o eliminar. porque se trata de hechos reduntantes, utlizando (algo, que veremos o no => "reducción de datos, para hacer analisis")

```{r}
# Vamos a utlizar varias librerias para hacer las matrices:
library(ggplot2)

#install.packages("corrplot")
library(corrplot)
```

```{r}
# Vamos a exportar la base de datos para despues importarla:
#mtcars <- datasets::mtcars

#Exportamos
#write.csv(mtcars, file = "mtcars.csv", row.names = F)

# Leemos la base de datos
#Daatos de una central de autos que toamaron mcuhos aspectos 11 en total

mtcars <- read.csv(file = "mtcars.csv", header = T, sep = ",", dec = ".")

#Ahora vamos a generar la matriz de correlacion solamnte para las variables numericas:
# Ya que para las categoricas no tiene sentido una matriz de correlación

# Para ello vamos a utilzar la función cor, que nos va a crear los coefientes de correlación para cada par columna, utilizando uno de mos metodos:
# spearman
# Pearson
#Kendall
# entre otros...


# En este caso utlizaremos el que más conozco y he utlizado (pearson)

mtcars.cor <- cor(mtcars, method = "pearson")

# Impirmimos la matriz de correlación reodeando a dos numeros:
write.csv(round(mtcars.cor, digits = 2), file = "MatrizCor.csv")

# Las matrices diagonales simepre seran igual dado que es la misma variable relacionada

# EL resto solamente puede tener correlación max 1 o min -1

"La corralación entre más positiva es o más negativa es más, mayor es la relación entre las variables y viseversa respectivamente (Directamente o Inversamente proporcional)"

# Como no es posible visualizar de manera sencilla los datos, los graficaremos para una mayor comprensión

# ENtre más grande la bola mayor es su correlación y viseversa
png("Corrplot1.png")

corrplot(mtcars.cor)

dev.off()

# Lo podriamos dejar así pero quedamos casi en las mismas que antes ya que hay mucho por mejorar:

# Ahora vamos a ir probando para ir mejorando la grafica:

# Lo que hicimos fue cambiar el metodo de bolasy mejorando las etiquetas de las columnas, para que se visualizen de mejor manera:

# No agrego titulos ya que se salen del margen de las imagenes
png(filename = "MatrizCorMos.png")

corrplot(mtcars.cor, method = "shade", shade.col = NA, tl.col = "black", tl.srt = 45)

dev.off()
```

```{r}

# Como podemos ver este grafica tampoco nos sirve:
# necesitamos una grafica que nos arrogen encima de cada cuadrado la correlación obtenida
# necesitamos que de forma grafica (una nueva palete de colores) mejore la estetica
# por ultimo necesitamos que los cuadrados o las figuras cambien su tamaño según la correlación


#Definamos una nueva paleta de colores:
# Vamos a especificar un array de colores
# Va mos a coger los mismo colores de aqui solo que no tan saturados y que no cansen a la vista:

colr <- colorRampPalette(c("#BB4444", "#EE9988", "#FFFFFF", "#77AADD", "#4477AA"))

# Volvemos a graficar ahora con nuestra nueva paleta:

# porque colr(200) para que quede escalado, ya que tenemos solo 5 colores y tenemos que llenar todo un espectro, asi dejamos listo lo que necesitamos

png(filename = "MatrizCorWithNum.png")

corrplot(mtcars.cor, method = "shade", tl.col = "black",
         col = colr(200), addCoef.col = "black",
         order = "AOE", tl.srt = 45)

dev.off()

# Si observamos el grafio podemos ver que la correlación negariva esta tachada, para ver lo que nos interesa

# COn el parametro type = "upper" extraemos los datos de la diagonal para arriba, además como toda la diagonal es de 1 la eliminaremos

# además con la función addshade, podemos ver como ahora tosos tienen lineas cruzadas y es que si la correlación es postiva la lineas iran par abajo, visevversa para las fechas de arriba

png(filename = "MatrizDiagWithNum.png")

corrplot(mtcars.cor, method = "shade", tl.col = "black",
         col = colr(200), addCoef.col = "black",
         order = "AOE", tl.srt = 45, type = "upper",
         diag = F, addshade = "all")

dev.off()


# Podemos cambiar el metodo para dejar los cuadrados:

png(filename = "MatrizDiagCircle.png")

corrplot(mtcars.cor, method = "circle", tl.col = "black",
         col = colr(200), addCoef.col = "black",
         order = "AOE", tl.srt = 45, type = "upper",
         diag = F, addshade = "all")

dev.off()

# entre todos los temas que habia yo me quedare con la elipse:

png(filename = "MatrizDiagEllipse.png")

corrplot(mtcars.cor, method = "ellipse", tl.col = "black",
         col = colr(200), addCoef.col = "black",
         order = "AOE", tl.srt = 45)

dev.off()

# Este me gusto más ya que maneja la elipse de un lado para saber si es psotiva o negativa y entre mas la correlación más delgada la "ellipse", viseversa para los negativos (más anchos)


```
```{r}
# Prodiamos hacer lo mismo con ggplot pero seria más complicado:
# Veamos solo el codigo que lleva:
library(reshape2)

mtcars.melted <- melt(mtcars.cor)

write.csv(mtcars.melted, file = "mtcars_melted.csv", row.names = F)

# Veamos que uno es data frame y el otro matriz
head(mtcars.cor)

head(mtcars.melted)

# Grafiquemos la matriz de correlación con ggplot:
png(filename = "MatrizCorGGplot.png")

mtcorgg <- ggplot(data = mtcars.melted,
                  aes(x = Var1, y = Var2, fill = value))+
  geom_tile()

# Como podemos observar hay mcuho trabajo por hacer (nada imposible, solo que como ud nos enseño,utlicen las herramientas a su alcance, no reinventen la rueda)

mtcorgg
dev.off()
```
Replit listo