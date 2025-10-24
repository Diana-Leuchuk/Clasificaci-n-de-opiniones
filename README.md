# Clasificaci-n-de-opiniones

Este proyecto tiene como objetivo construir un modelo de clasificación de sentimientos capaz de identificar si una declaración es positiva o negativa a partir de texto.
Primero, se importaron las librerías necesarias para manipulación de datos, procesamiento de texto y modelado. Se cargaron dos conjuntos de datos:

dataset_train_95.csv: contiene declaraciones etiquetadas como positivas (1) o negativas (0).

dataset_test_5_sin_etiqueta.csv: contiene declaraciones sin etiqueta, sobre las cuales se realizarán predicciones.
Se separaron las características (texto de las declaraciones) de las etiquetas (sentimiento):

X = train['declaracion']
y = train['etiqueta']


Para evaluar el rendimiento del modelo, se dividió el conjunto de entrenamiento en entrenamiento y validación. 
Como los modelos de Machine Learning no entienden texto directamente, se transformaron las declaraciones en vectores numéricos mediante CountVectorizer, que convierte cada palabra en una característica binaria o de frecuencia:

vectorizador = CountVectorizer()
X_train_vec = vectorizador.fit_transform(X_train)
X_val_vec = vectorizador.transform(X_val)
X_test_vec = vectorizador.transform(test['declaracion'])


fit_transform aprende el vocabulario a partir del conjunto de entrenamiento y transforma el texto.

transform aplica ese vocabulario a los datos de validación y prueba.
Se procede al entrenamiento del modelo

Se entrenó un modelo de Logistic Regression para clasificar el sentimiento:

modelo = LogisticRegression(max_iter=1000)
modelo.fit(X_train_vec, y_train)


Se evaluó su desempeño usando el conjunto de validación:

accuracy = modelo.score(X_val_vec, y_val)
print(f"Precisión en el conjunto de validación: {accuracy:.2f}")


Este valor indica qué tan bien el modelo predice el sentimiento de declaraciones que no ha visto durante el entrenamiento.

Finalmente, se realizaron predicciones sobre el conjunto de prueba y se generó un archivo con los resultados:

predicciones = modelo.predict(X_test_vec)
test['etiqueta'] = predicciones
test.to_csv("dataset_test.csv", index=False)
print("Archivo dataset_test.csv generado con la columna 'etiqueta'.")


El archivo dataset_test.csv ahora contiene las declaraciones originales junto con la etiqueta predicha (1 = positiva, 0 = negativa).
