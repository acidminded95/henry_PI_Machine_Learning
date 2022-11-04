![HenryLogo](https://d31uz8lwfmyn8g.cloudfront.net/Assets/logo-henry-white-lg.png)
# henry_pi_machine_learning

Este repositorio contiene mi segundo proyecto individual para la carrera de Date Science de Henry.

El desarrollo del proyecto se encuentra contenido en el cuaderno 'test.ipynb', tal cuaderno se desarrolló en inglés para futuras referencias y exposiciones del proyecto. Así mismo, este readme será traducido en un futuro.

## ¿Qué se esperaba del proyecto?

El proyecto consistía en generar un modelo de Machine Learning capaz de clasificar propiedades del mercado inmobiliario de Colombia en dos categorias (caro o barato) basándonos en el precio promedio de las mismas. Para esto nos fueron disponibilizados dos datasets conteniendo información sobre anuncios de propiedades en venta: uno para entrenar nuestros modelos (con información sobre el precio de cada propiedad) y que usaríamos para generar las predicciones de nuestros modelos (sin la información sobre el precio de la propiedad) y enviarlas a Henry para ser calificadas.

La información completa de la consignia, así como los datasets disponibilizados, pueden encontrarse en el repositorio de Henry: https://github.com/soyHenry/Datathon (a 4 de noviembre de 2022).

Dentro de las columnas que compartían ambos datasets encontramos:

• Información acerca de la fecha de publicación del anuncio y fecha de dada de baja de la publicación. 

• La ubicación de la propiedad tanto en términos de latitud y longitud, como del departamento y ciudad o municipio en que se encuentra ubicada.

• Información relativa a la propiedad misma, como la cantidad de habitaciones, de baños, el área construida, el área del terreno y el tipo de propiedad (casa, apartamento, local comercial, etc.)

• Detalles del anuncio de venta de la propiedad: el título y la descripción del inmueble.

## Mi acercamiento para resolver el problema:

Mi enfoque para dar una solución al problema se concentró en tratar de limpiar los datos de la mejor manera posible para que, al momento de entrenar los modelos de ML, estos tuviesen la mejor posibilidad de dar un resultado satisfactorio.

Para esto, agrupé las columnas del dataset de entrenamiento según la lista planteada arriba, deshechando algunas columnas que rápidamente pude ver que no aportarían nada al modelo (como las que tenían un sólo valor en toda la columna o las que tenían un valor distinto para cada fila). 

Una vez agrupadas las columnas, fui analizando grupo por grupo determinando las mejores soluciones para imputar valores faltantes y las columnas que contenían mejores datos para usar en el modelo.

Tras esto, determinadé 3 subsets de datos de entrenamiento posibles, difirendo entre sí principalmente en la cantidad de registros que tenían. Puesto que algunas columnas contaban con muchos registros duplicados, al ser estos deshechados, el tamaño de los datasets podía reducirse considerablemente.

Además de estos tres subests de datos de entrenamiento se diseñaron dos líneas de preprocesamiento distintas:

- La primera, que destaca porque recupera valores númericos desde columnas categóricas con muchos valores posibles, usando OneHotEncoder (en particular, de las columnas de Departamento(L2) y Ciudad/Municipio(L3)) y posteriormente reduce la dimensionalidad del dataset de 339 columnas a 30 usando PCA. Este enfoque fue abordado debido a que la ubicación de un inmueble suele tener gran peso sobre su precio, así que al tener mayor información al respecto se esperaban mejores resultados.

- La segunda, más austera usando principalmente columnas que ya eran de tipo numérico y codificando únicamente una columna de datos categóricos (la que contenía el tipo d ela propiedad). En esta línea de preprocesamiento únicamente se tomaron en cuenta las coordenadas de las propiedades como referencia a su ubicación. Este enfoque dio mejores resultados que el primero.

Al final del cuaderno (test.ipynb) se intenta un tercer enfoque que trata de obtener información relevante de la descripción de las propiedades y convertirlo en un valor numérico. Sin embargo, este enfoque nod io los resultados esperados.

Una vez hechos los datasets de entrenamiento y las líneas d epreprocesamiento, decidí probarlos primero usando CrossValidation con diferentes tipos de modelos de clasificación (KNeighbors, DecisionTree, RandomForest, LinearSVC y SVC) para después hacer pruebas más exhaustivas (con GridSearchCV) en los modelos más prometedores.

## Resultados

El mejor modelo generado (un RandomForestClassifier) durante la semana de trabajo obtuvo una puntuación de accuracy de 0.9027 y de recall de 0.7643. Considero que no fue per se un mal resultado pero que dado el peso que se le dio a la métrica del 'recall', el proceso tenía seguramente puntos en que mejorar.

En un intento de última hora se logró mejorar drásticamente el resultado de recall del modelo removiendo los outliers de la columna 'precio' antes de obtener el promedio con el que se obtendria  la columna target. Recall: 0.9168 Accuracy: 0.7710