---
title: "CNN LeNet-5 on MNIST"
author: "Ngoc Tran Trung"
date: "9 April 2021"
comments: yes
layout: post
---

In the previous post, we used `tf.keras` to build and train a 2-layer neural network to predict digits 0-9 with MNIST dataset. This time, we will step up our game a bit when we build a Convolutional Neural Network (CNN) for our learning.

## I - Load Packages and Define Constants


```python
import tensorflow as tf
import numpy as np
from tensorflow import keras
import pandas as pd

np.random.seed(1)
```


```python
NUMBER_OF_CLASSES = 10
EPOCHS = 30
BATCH_SIZE = 2**6
VALIDATION_SPLIT = .2
```

## II - Load and Process the Dataset


```python
# Load MNIST dataset
(X_train, Y_train), (X_test, Y_test) = keras.datasets.mnist.load_data()
print("X_train's shape:", X_train.shape)
print("Y_train's shape:", Y_train.shape)
print("X_test's shape:", X_test.shape)
print("Y_test's shape:", Y_test.shape)
```

    X_train's shape: (60000, 28, 28)
    Y_train's shape: (60000,)
    X_test's shape: (10000, 28, 28)
    Y_test's shape: (10000,)


This time, we don't need to flatten the images since `keras` allows us to explicit define the shape of the input, which is 28x28. We only need to one-hot encoding for the target variables, which are `Y_train` and `Y_test`.


```python
# One-hot encode for Y_train and Y_test
Y_train_preprocessed = keras.utils.to_categorical(Y_train, NUMBER_OF_CLASSES)
Y_test_preprocessed = keras.utils.to_categorical(Y_test, NUMBER_OF_CLASSES)
print("Y_train_preprocessed's shape:", Y_train_preprocessed.shape)
print("Y_test_preprocessed's shape: ", Y_test_preprocessed.shape)
```

    Y_train_preprocessed's shape: (60000, 10)
    Y_test_preprocessed's shape:  (10000, 10)


## III - Build a Convolutional Model 

In this post, we will build a CNN to solve MNIST. Specifically we will use one of the earliest CNN architecture called [LeNet-5](https://en.wikipedia.org/wiki/LeNet) for our problem. 

It is important to note that the input size of the images of LeNet-5 is 32x32 while ours is 28x28, we should add some padding to make our size 32x32. Although we can use `keras` function `.ZeroPadding2D()` to do that, I still would like to decouple this preprocessing step from model building. We also need to reshape it to define that our images have 1 channel.

![png](/../figs/2021-04-09-LeNet-5-with-MNIST/LeNet5.png)


```python
# Padding and reshape X_train
X_train_preprocessed = np.pad(X_train, ((0, 0), (2, 2), (2, 2)))  # padding 2 on top and bottom for 2D
X_train_preprocessed = X_train_preprocessed.reshape(X_train_preprocessed.shape[0], 
                                       X_train_preprocessed.shape[1], 
                                       X_train_preprocessed.shape[2], 
                                       1)  # 1 for 1 channel
print("X_train's shape:", X_train_preprocessed.shape)

# Doing similar for X_test
X_test_preprocessed = np.pad(X_test, ((0, 0), (2, 2), (2, 2)))
X_test_preprocessed = X_test_preprocessed.reshape(X_test_preprocessed.shape[0],
                                     X_test_preprocessed.shape[1], 
                                     X_test_preprocessed.shape[2], 1)
print("X_test's shape:", X_test_preprocessed.shape)
```

    X_train's shape: (60000, 32, 32, 1)
    X_test's shape: (10000, 32, 32, 1)


After padding, we have our new images are of size 32x32. Now we proceed to build our CNN model using LeNet-5 architecture. Basically, LeNet-5 basically consits of seven layers, which are as follows:
1. `Conv2D`: 6 filters 5x5, stride = 1, padding = 2.
2. `AveragePooling2D`: 2x2, stride = 2, padding = 0.
3. `Conv2D`: 16 filters 5x5, stride = 1, padding = 0.
4. `AveragePooling2D`: 2x2, stride = 2, padding = 0.
5. `Dense`: 120 units, activation = "relu".
6. `Dense`: 84 units, activation = "relu".
7. `Dense`: 10 units, activation = "softmax".


```python
mnist_model = keras.Sequential([
    #keras.layers.Input(shape=(64, 64)),
    keras.layers.Conv2D(filters=6, kernel_size=(5, 5), strides=1, padding="valid", input_shape=(32, 32, 1)),
    keras.layers.AveragePooling2D(pool_size=(2, 2), strides=(2, 2), padding="valid"),
    keras.layers.Conv2D(filters=16, kernel_size=(5, 5), padding="valid"),
    keras.layers.AveragePooling2D(pool_size=(2, 2), padding="valid"),
    keras.layers.Flatten(),
    keras.layers.Dense(units=120, activation="relu"),
    keras.layers.Dense(units=84, activation="relu"),
    keras.layers.Dense(units=10, activation="softmax")
])
```


```python
# Model summary
mnist_model.summary()
```

    Model: "sequential"
    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    conv2d (Conv2D)              (None, 28, 28, 6)         156       
    _________________________________________________________________
    average_pooling2d (AveragePo (None, 14, 14, 6)         0         
    _________________________________________________________________
    conv2d_1 (Conv2D)            (None, 10, 10, 16)        2416      
    _________________________________________________________________
    average_pooling2d_1 (Average (None, 5, 5, 16)          0         
    _________________________________________________________________
    flatten (Flatten)            (None, 400)               0         
    _________________________________________________________________
    dense (Dense)                (None, 120)               48120     
    _________________________________________________________________
    dense_1 (Dense)              (None, 84)                10164     
    _________________________________________________________________
    dense_2 (Dense)              (None, 10)                850       
    =================================================================
    Total params: 61,706
    Trainable params: 61,706
    Non-trainable params: 0
    _________________________________________________________________



```python
# Compile our model
mnist_model.compile(optimizer="adam", 
                    loss="categorical_crossentropy",
                    metrics=['accuracy'])
```


```python
# Fit the model
history = mnist_model.fit(X_train_preprocessed, 
                          Y_train_preprocessed, 
                          epochs=EPOCHS,
                          verbose=0,
                          batch_size=BATCH_SIZE,
                          validation_split=VALIDATION_SPLIT)
```


```python
# Evaluate the new model using a test set
mnist_model.evaluate(X_test_preprocessed, Y_test_preprocessed)
```

    313/313 [==============================] - 1s 4ms/step - loss: 0.1565 - accuracy: 0.9713





    [0.15645572543144226, 0.9713000059127808]



## IV - Model History


```python
df_loss_acc = pd.DataFrame(history.history)
df_loss_acc.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>loss</th>
      <th>accuracy</th>
      <th>val_loss</th>
      <th>val_accuracy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>30.000000</td>
      <td>30.000000</td>
      <td>30.000000</td>
      <td>30.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.079604</td>
      <td>0.979224</td>
      <td>0.150545</td>
      <td>0.967842</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.117880</td>
      <td>0.018772</td>
      <td>0.030265</td>
      <td>0.006766</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.027080</td>
      <td>0.893604</td>
      <td>0.107625</td>
      <td>0.944417</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.037492</td>
      <td>0.977495</td>
      <td>0.128079</td>
      <td>0.966542</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.047665</td>
      <td>0.985073</td>
      <td>0.136388</td>
      <td>0.969583</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.072220</td>
      <td>0.989229</td>
      <td>0.171658</td>
      <td>0.971417</td>
    </tr>
    <tr>
      <th>max</th>
      <td>0.681337</td>
      <td>0.991625</td>
      <td>0.215185</td>
      <td>0.974917</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Make two corresponding data frames from history df.
df_loss = df_loss_acc[['loss', 'val_loss']]
df_loss = df_loss.rename(columns={'loss': 'Training loss', 'val_loss': 'Dev loss'})
df_acc = df_loss_acc[['accuracy', 'val_accuracy']]
df_acc = df_acc.rename(columns={'accuracy': 'Dev accuracy', 'val_accuracy': 'Dev accuracy'})

# Plot two data frames
df_loss.plot(title='Training vs Dev Loss', figsize=(12, 6))
df_acc.plot(title='Training vs Dev Accuracy', figsize=(12, 6))
```




    <AxesSubplot:title={'center':'Training vs Dev Accuracy'}>




    
![png](/../figs/2021-04-09-LeNet-5-with-MNIST/output_20_1.png)
    



    
![png](/../figs/2021-04-09-LeNet-5-with-MNIST/output_20_2.png)
    


As we can see, the average accuracy on the training set and the dev of our model are 97.92% and 96.78% whereas its accuracy on the test set is 97.13%. Thus, there could be a high variance in our model that adding regularization may benefit our model.

## V - Conclusion

In this post, we built a LeNet-5 CNN to recognize digits with MNIST dataset. In comparison to [the previous post](https://newbiettn.github.io/2021/04/07/MNIST-with-keras/), this time, our model achieved a slightly better prediction accuracy on the seperate test set with 97.13% (vs. 96.66%). 

