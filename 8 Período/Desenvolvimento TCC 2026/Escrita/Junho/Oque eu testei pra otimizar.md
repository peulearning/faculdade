## 1. Experimentando Balanceamento de Classes + Label Smoothing

Logo após minha célula **11. de Gerador de Dados de Imagem** eu adicionei um verificador para averiguar o desbalanceamento das classes "Pressure & Diabetic".


```

from sklearn.utils.class_weight import compute_class_weight

import numpy as np

  

# Classes do generator

classes = train_generator.classes

  

# Calcular pesos

weights = compute_class_weight(

    class_weight='balanced',

    classes=np.unique(classes),

    y=classes

)

  

class_weights = dict(enumerate(weights))

  

print("Pesos das classes:")

print(class_weights)
```

essa foi minha saída :

```
Pesos das classes: 
{0: np.float64(0.8604651162790697),
 1: np.float64(1.1935483870967742)}
```

em outras palavras oque podemos concluir é que há **desbalanceamento, entretanto não e tão severo assim. Então possivelmente não há necessidade de adicionar esse parâmetro ao treinamento pois pode acarretar em criar ruídos.. **  


tentei aplicar o  Label Smoothing na hora de compilar tanto no treinamento quanto no fine-tunning.

```
model.compile(
    optimizer=Adam(learning_rate=1e-3),

    loss=tf.keras.losses.CategoricalCrossentropy(
        label_smoothing=0.1
    ),

    metrics=['accuracy']
)
```

a minha conclusão ao combinar as duas técnicas ao qual usei o modelo caiu significativamente pois tenho apenas 2 classes, feridas visualmente semelhantes e um dataset pequeno. e isso já gera uma dificuldade para separar as classes, e utilizando desse parâmetro o modelo ainda menos confiante.