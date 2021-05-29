# Usando Aprendizaje Transferido para lograr un 99% de precisión en detección de tumores cerebrales

En el siguiente informe se expondrá como se ha usado transferencia de aprendizaje para la detección  de tumores cerebrales a partir de imágenes. 

El dataset que se ha utilizado puede encontrarse en https://www.kaggle.com/preetviradiya/brian-tumor-dataset

## Exploración del conjunto de datos
Vamos a hacer un pequeño análisis preexploratorio de los datos para tomar decisiones sobre el modelo a utilizar. 

![](https://raw.githubusercontent.com/JesusFraile/brain_cancer/main/images/count_data.png)

Tenemos en total 2087 datos etiquetados como sanos y 2513 como con cancer. Esto supone una relación de 1:0,84 por tanto, podemos considerar a priori que estamos ante un conjunto de datos balanceado. 

Representando a modo de ejemplo una serie de X imágenes vemos que 
