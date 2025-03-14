### Implementación de Redes Convolucionales y Recurrentes con Python

#### Objetivo:
Comprender el funcionamiento de las redes convolucionales y recurrentes mediante la implementación de un modelo de clasificación de imágenes utilizando Python y TensorFlow.

#### Dataset:
Para este ejercicio, utilizaremos el dataset **CIFAR-10**, que consiste en 60,000 imágenes de 32x32 píxeles distribuidas en 10 categorías.

#### Paso 1: Preparación del Dataset

1. **Recolección de datos**: Utilizaremos el dataset CIFAR-10.
3. **Etiquetado**: Cada imagen ya tiene una etiqueta clara.
4. **Balanceo de datos**: CIFAR-10 tiene una distribución equitativa entre las categorías.

El dataset CIFAR-10 está disponible en varias fuentes. Aquí tienes algunas opciones para acceder a él:

1. **UCI Machine Learning Repository**:
   - URL: [https://archive.ics.uci.edu/ml/datasets/cifar-10](https://archive.ics.uci.edu/ml/datasets/cifar-10)
   - Este enlace te lleva a la página del dataset en el repositorio de aprendizaje automático de UCI, donde puedes encontrar información detallada sobre el conjunto de datos[1].

2. **Hugging Face Datasets**:
   - URL: [https://huggingface.co/datasets/uoft-cs/cifar10](https://huggingface.co/datasets/uoft-cs/cifar10)
   - Aquí puedes descargar el dataset directamente o utilizarlo con la biblioteca Hugging Face[2].

3. **TensorFlow Datasets**:
   - URL: [https://www.tensorflow.org/datasets/catalog/cifar10](https://www.tensorflow.org/datasets/catalog/cifar10)
   - TensorFlow ofrece una forma sencilla de cargar el dataset CIFAR-10 directamente en tus proyectos de aprendizaje automático[4].

4. **Sitio web del Departamento de Ciencias de la Computación de la Universidad de Toronto**:
   - URL: [https://www.cs.toronto.edu/~kriz/cifar.html](https://www.cs.toronto.edu/~kriz/cifar.html)
   - Puedes descargar el dataset en formato binario o Python desde este sitio web[5].

Para cargar el dataset CIFAR-10 en Python utilizando TensorFlow, puedes usar el siguiente código:

```python
import tensorflow as tf
from tensorflow.keras.datasets import cifar10

# Cargar el dataset CIFAR-10
(x_train, y_train), (x_test, y_test) = cifar10.load_data()
```

#### Paso 2: División del Dataset

Dividiremos el dataset en conjuntos de entrenamiento, validación y pruebas.

```python
from sklearn.model_selection import train_test_split

# Dividir el conjunto de entrenamiento en entrenamiento y validación
x_train, x_val, y_train, y_val = train_test_split(x_train, y_train, test_size=0.15, random_state=42)

# Conjunto de pruebas ya está separado como x_test y y_test
```

#### Paso 3: Preprocesamiento de Imágenes

Normalizaremos los valores de píxeles entre 0 y 1, y aplicaremos técnicas de augmentación.

```python
from tensorflow.keras.preprocessing.image import ImageDataGenerator

# Normalización y augmentación
datagen = ImageDataGenerator(
    rescale=1./255,
    rotation_range=20,
    width_shift_range=0.1,
    height_shift_range=0.1,
    horizontal_flip=True
)

# Aplicar augmentación al conjunto de entrenamiento
train_generator = datagen.flow(x_train, y_train, batch_size=32)
val_generator = datagen.flow(x_val, y_val, batch_size=32)
test_generator = datagen.flow(x_test, y_test, batch_size=32)
```

#### Paso 4: Uso de Transfer Learning

Utilizaremos un modelo preentrenado **InceptionV3**.

```python
from tensorflow.keras.applications import InceptionV3

# Cargar modelo preentrenado InceptionV3
base_model = InceptionV3(weights='imagenet', include_top=False, input_shape=(32, 32, 3))

# Congelar las capas base
base_model.trainable = False

# Agregar capas finales para clasificación
from tensorflow.keras.models import Model
from tensorflow.keras.layers import Dense, GlobalAveragePooling2D

x = base_model.output
x = GlobalAveragePooling2D()(x)
x = Dense(1024, activation='relu')(x)
predictions = Dense(10, activation='softmax')(x)

# Crear el modelo final
model = Model(inputs=base_model.input, outputs=predictions)
```

#### Paso 5: Entrenamiento del Modelo

Configuraremos hiperparámetros y utilizaremos técnicas como Early Stopping y Learning Rate Scheduler.

```python
from tensorflow.keras.optimizers import Adam
from tensorflow.keras.callbacks import EarlyStopping, ReduceLROnPlateau

# Configuración de hiperparámetros
optimizer = Adam(lr=0.001)
model.compile(optimizer=optimizer, loss='sparse_categorical_crossentropy', metrics=['accuracy'])

# Callbacks para entrenamiento
early_stop = EarlyStopping(monitor='val_loss', patience=5, min_delta=0.001)
reduce_lr = ReduceLROnPlateau(monitor='val_loss', factor=0.5, patience=3, min_lr=0.00001)

# Entrenar el modelo
history = model.fit(train_generator, epochs=20, validation_data=val_generator, callbacks=[early_stop, reduce_lr])
```

#### Paso 6: Evaluación del Modelo

Evaluar el modelo utilizando precisión, sensibilidad y puntaje F1.

```python
from sklearn.metrics import accuracy_score, f1_score, classification_report

# Evaluar el modelo en el conjunto de pruebas
test_loss, test_acc = model.evaluate(test_generator)
y_pred = model.predict(test_generator)
y_pred_class = y_pred.argmax(axis=1)

# Calcular métricas
accuracy = accuracy_score(y_test, y_pred_class)
f1 = f1_score(y_test, y_pred_class, average='macro')
print(f'Precisión: {accuracy:.4f}, F1 Score: {f1:.4f}')
print(classification_report(y_test, y_pred_class))
```

#### Paso 7: Implementación en Producción

Guardar el modelo en formato `.h5` y desplegarlo usando Flask.

```python
# Guardar el modelo
model.save('modelo_cifar10.h5')

# Desplegar el modelo con Flask
from flask import Flask, request, jsonify
from tensorflow.keras.models import load_model

app = Flask(__name__)

# Cargar el modelo guardado
loaded_model = load_model('modelo_cifar10.h5')

@app.route('/predict', methods=['POST'])
def predict():
    # Procesar la imagen de entrada
    img = request.files['image']
    img_array = tf.keras.preprocessing.image.load_img(img, target_size=(32, 32))
    img_array = tf.keras.preprocessing.image.img_to_array(img_array)
    img_array = tf.expand_dims(img_array, 0) / 255.0
    
    # Realizar la predicción
    predictions = loaded_model.predict(img_array)
    predicted_class = predictions.argmax(axis=1)[0]
    
    return jsonify({'class': predicted_class})

if __name__ == '__main__':
    app.run(debug=True)
```

### Análisis de Resultados  

- **Análisis de Resultados**: El modelo debe alcanzar una precisión del 80% o superior en el conjunto de pruebas. El uso de transfer learning y técnicas de augmentación ayuda a mejorar la generalización del modelo.

# Mejoras a implementar  
- **Mejoras**:
  - **Fine-tuning**: Descongelar algunas capas del modelo base para ajustarlas a nuestro conjunto de datos.
  - **Experimentar con diferentes modelos**: Comparar el rendimiento de InceptionV3 con otros modelos preentrenados como VGG16 o ResNet50.
  - **Optimización de Hiperparámetros**: Utilizar técnicas como Grid Search o Bayesian Optimization para encontrar los mejores hiperparámetros.

>[(NOTE)]
> Hata este punto obtendrian el un 80% de la actividad, si implementan el reto es sobre 100%


### RETO: Implementación con AI-PC de Intel

Para implementar este modelo en un entorno de AI-PC de Intel, puedes seguir los siguientes pasos:

1. **Instalar TensorFlow y OpenVINO**: Asegúrate de tener TensorFlow y OpenVINO instalados en tu sistema.
2. **Optimizar el Modelo**: Utiliza OpenVINO para optimizar el modelo y mejorar su rendimiento en hardware Intel.
3. **Desplegar el Modelo**: Despliega el modelo optimizado en un entorno de producción utilizando Flask o FastAPI.

Puedes encontrar más detalles sobre la implementación con AI-PC en el repositorio de GitHub de Intel: [https://github.com/intel/AI-PC_Notebooks](https://github.com/intel/AI-PC_Notebooks).

### Gráficos de Precisión y Pérdida

Durante el entrenamiento, es útil visualizar la precisión y la pérdida tanto en el conjunto de entrenamiento como en el de validación. Esto te permite evaluar el rendimiento del modelo y ajustar los hiperparámetros según sea necesario.

```python
import matplotlib.pyplot as plt

# Plotear precisión y pérdida
plt.figure(figsize=(10, 5))

plt.subplot(1, 2, 1)
plt.plot(history.history['accuracy'], label='Entrenamiento')
plt.plot(history.history['val_accuracy'], label='Validación')
plt.title('Precisión')
plt.legend()

plt.subplot(1, 2, 2)
plt.plot(history.history['loss'], label='Entrenamiento')
plt.plot(history.history['val_loss'], label='Validación')
plt.title('Pérdida')
plt.legend()

plt.tight_layout()
plt.show()
```

