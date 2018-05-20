---
layout: notebook
title:  "Intro to Keras with MNIST!"
date: 2018-03-20
excerpt: The goal of this notebook is to provide some basic intuition on the diffrences between two types of deep neural networks and to encourage you to test different deep learning architectures.
tags: Keras DeepLearning
---




This experiment has been run on a NVIDIA GeForce GTX 970 but shouldn’t have any problem with using CPU only.

This experiment is written in Python with the help of the libraries tensorflow and keras, the model code exist on a different file to make this easier to run. See the (DeepLearningModels.py) file for full code.




```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from intro_keras import *
```

#### MNIST
The dataset contains 60,000 28x28 grayscale images of the 10 digits (integers in range 0-9) of handwritten digits.


```python
(X, Y), _ = get_dataset()
```


```python
# Plot the data
plt.imshow(X[0], cmap=plt.get_cmap('gray'))
plt.show()
```


![png](/image_folder/Intro-Keras/output_4_0.png)


### One hot encoding
Convert a categorical variables to a vector.  In this case the categorical  variables  is 10 digits integer value.
![png](/image_folder/Intro-Keras/One hot encoding.png)


```python
Y = np.array(pd.get_dummies(Y))
```


```python
print('train input shape: ', X.shape, '\ntrain targer shape: ', Y.shape)
```

    train input shape:  (60000, 28, 28)
    train targer shape:  (60000, 10)


## Models

### Small


```python
model_small=ModelSmall(X.shape, Y.shape)
model_small.create_model()
model_small.fit(X, Y)
```

### Medium


```python
model_medium = ModelMedium(X.shape, Y.shape)
model_medium.create_model()
model_medium.fit(X, Y)
```

### Large


```python
model_large = ModelLarge(X.shape, Y.shape)
model_large.create_model()
model_large.fit(X, Y)
```

### Regression


```python
# the data, shuffled and split between train and test sets
(X, Y), _ = mnist.load_data()
```


```python
model_regression = ModelRegression(X.shape, Y.shape)
model_regression.create_model()
model_regression.fit(X, Y)
```

### Compare


```python
models = [model_small, model_medium, model_large, model_regression]
model_names = ['Small', 'Medium', 'Large', 'Regression']
```


```python
for i, name in zip(models, model_names):
    plt.plot(np.arange(4), i.get_acc(),label=name)

plt.ylabel('Acc ')
plt.xlabel('Epochs ')
plt.title('Models')
plt.legend(loc='center left', bbox_to_anchor=(1, 0.5))
plt.tight_layout()
plt.show()
```
