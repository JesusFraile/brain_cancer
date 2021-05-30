# Usando Aprendizaje Transferido para lograr un 99% de precisión en detección de tumores cerebrales

En el siguiente informe se expondrá como se ha usado transferencia de aprendizaje para la detección  de tumores cerebrales a partir de imágenes. 

El dataset que se ha utilizado puede encontrarse en https://www.kaggle.com/preetviradiya/brian-tumor-dataset

## Exploración del conjunto de datos
Vamos a hacer un pequeño análisis preexploratorio de los datos para tomar decisiones sobre el modelo a utilizar. 

![](https://raw.githubusercontent.com/JesusFraile/brain_cancer/main/images/count_data.png)

Tenemos en total 2087 datos etiquetados como sanos y 2513 como con cáncer. Esto supone una relación de 1:0,84 por tanto, podemos considerar a priori que estamos ante un conjunto de datos balanceado. 

A continuación, a modo de ejemplo se representará una serie de 14 imágenes aleatorias del dataset.

![](https://raw.githubusercontent.com/JesusFraile/brain_cancer/main/images/plot_images.png)

## Elección del modelo y entrenamiento
Se ha dividido el dataset de tal forma que entrenaremos al modelo con el 80% de los datos, un 10% los usaremos como datos de validación para decidir cuando detener el entrenamiento para evitar el overfitting o underfitting, y el 10% restante se usará para evaluar el rendimiento del modelo. 

Como estamos ante un caso en los que debe primar la precisión sobre el tiempo de computación se ha decidido utilizar el modelo pre-entrenado de NasNetLarge eliminando la última capa del mismo. Este modelo recibe como entrada una imagen RGB de 331x331 y cuenta con un total de 206885319 parámetros. Posteriormente se le ha añadido una capa de 250 neuronas con un Dropout de 0.3 para evitar el overfitting y como salida una capa con una neurona con función de activación softmax.

El método de entrenamiento a seguir para realizar una transferencia de aprendizaje es el siguiente:
-1. Se congelan las capas del modelo, es decir, se bloquean los pesos originales del modelo. De esta forma tendremos un modelo que ya sabrá diferenciar todo tipo de patrones y en las primeras etapas del entrenamiento no variarán. 
-2. Se entrena tan solo las capas que se le han añadido. De esta forma el modelo se especializará en realizar la tarea que queremos. 
-3. Finalmente, si se considera necesario se descongelan todas las capas y se reentrena el modelo. 

En nuestro caso el punto 3 no ha sido necesario realizarlo, ya que no aportaba grandes beneficios al modelo. En total se han entrenado 84916818 parámetros, lo que supone una reducción de un 60% del modelo NasNet. 

En el entrenamiento se ha usado un batch size de 40 imágenes y el optimizador Adam, el cual permite que el ratio de aprendizaje vaya disminuyendo según avanzan las épocas. Cada época ha llevado un tiempo de unos 80 segundos utilizando una Nvidia GTX 1080Ti
![](https://raw.githubusercontent.com/JesusFraile/brain_cancer/main/images/train.PNG)

![](https://raw.githubusercontent.com/JesusFraile/brain_cancer/main/images/train_loss.PNG)

## Evaluación del modelo
Se ha evaluado el modelo sobre el dataset de test. Para estudiar a simple vista el modelo se ha obtenido la matriz de confusión del mismo (0 = sano, 1 = tumor). 
![](https://raw.githubusercontent.com/JesusFraile/brain_cancer/main/images/confusion_matrix.png)

Vemos que más o menos obtiene el mismo número de falsos positivos que de falsos negativos. 

Finalmente se obtiene las métricas más usadas para problemas de clasificación como este.
![](https://raw.githubusercontent.com/JesusFraile/brain_cancer/main/images/metrics.PNG)

##Otras consideraciones
Como se ha comentado se ha decidido no entrenar con todas las capas del modelo descongeladas. Además se ha probado diferentes técnicas de oversampling de imágenes como rotaciones, simetrías, rudio Gaussiano, ... con el objetivo de mejorar el modelo. En este caso se ha optado por no incluirlas, puesto que la relación coste/beneficio era insignificante. 

En el caso que se quisiera hacer un estudio más profundo del modelo sería conveniente estudiar el por qué se clasifican mal ciertas imágenes (imágenes con baja resolución, tumores muy pequeños y difícilmente perceptibles debido al cambio de tamaño, ...)
