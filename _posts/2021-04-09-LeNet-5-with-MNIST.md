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

### 1. Load data


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


### 2. Convert to the dataset to float32


```python
print("Data type of X_train:", X_train.dtype)
print("Data type of X_test:", X_test.dtype)
```

    Data type of X_train: uint8
    Data type of X_test: uint8



```python
X_train_preprocessed = X_train.astype('float32')
X_test_preprocessed = X_test.astype('float32')
print("Data type of X_train_preprocessed:", X_train_preprocessed.dtype)
print("Data type of X_test_preprocessed:", X_test_preprocessed.dtype)
```

    Data type of X_train_preprocessed: float32
    Data type of X_test_preprocessed: float32


### 2. Normalize the data


```python
# Check range of values (peak to peak)
print("Peak-to-peak value of X_train:", np.ptp(X_train_preprocessed))
print("Peak-to-peak value of X_test:", np.ptp(X_test_preprocessed))
```

    Peak-to-peak value of X_train: 255.0
    Peak-to-peak value of X_test: 255.0



```python
# Normalize to range (0-1)
X_train_preprocessed = np.divide(X_train_preprocessed, 255)
X_test_preprocessed = np.divide(X_test_preprocessed, 255)
print("Peak-to-peak value of X_train:", np.ptp(X_train_preprocessed))
print("Peak-to-peak value of X_test:", np.ptp(X_test_preprocessed))
```

    Peak-to-peak value of X_train: 1.0
    Peak-to-peak value of X_test: 1.0


### 3. Reshape training and target variables


```python
# One-hot encode for Y_train and Y_test
Y_train_preprocessed = keras.utils.to_categorical(Y_train, NUMBER_OF_CLASSES)
Y_test_preprocessed = keras.utils.to_categorical(Y_test, NUMBER_OF_CLASSES)
print("Y_train_preprocessed's shape:", Y_train_preprocessed.shape)
print("Y_test_preprocessed's shape: ", Y_test_preprocessed.shape)
```

    Y_train_preprocessed's shape: (60000, 10)
    Y_test_preprocessed's shape:  (10000, 10)



```python
# Reshape X_train_preprocessed, X_test_preprocessed
X_train_preprocessed = X_train_preprocessed.reshape(X_train_preprocessed.shape[0],
                                                    X_train_preprocessed.shape[1],
                                                    X_train_preprocessed.shape[2],
                                                    1)  # 1 represents 1 channel
X_test_preprocessed = X_test_preprocessed.reshape(X_test_preprocessed.shape[0],
                                                  X_test_preprocessed.shape[1],
                                                  X_test_preprocessed.shape[2],
                                                  1)  # 1 represents 1 channel
```

## III - Build a Convolutional Model 

In this post, we will build a CNN to solve MNIST. Specifically we will use one of the earliest CNN architecture called [LeNet-5](https://en.wikipedia.org/wiki/LeNet) for our problem. Basically, LeNet-5 consists of seven layers, which are as follows:
1. `Conv2D`: 6 filters 5x5, stride = 1, padding = 2.
2. `AveragePooling2D`: 2x2, stride = 2, padding = 0.
3. `Conv2D`: 16 filters 5x5, stride = 1, padding = 0.
4. `AveragePooling2D`: 2x2, stride = 2, padding = 0.
5. `Dense`: 120 units, activation = "relu".
6. `Dense`: 84 units, activation = "relu".
7. `Dense`: 10 units, activation = "softmax".

![png](/../figs/2021-04-09-LeNet-5-with-MNIST/LeNet5.png)


```python
mnist_model = keras.Sequential([
    keras.layers.Input(shape=(28, 28, 1)),
    
    keras.layers.Conv2D(filters=6, kernel_size=(5, 5), strides=1, padding="valid"),
    keras.layers.BatchNormalization(),
    keras.layers.Activation("relu"),
    
    keras.layers.AveragePooling2D(pool_size=(2, 2), strides=(2, 2), padding="valid"),
    keras.layers.BatchNormalization(),
    
    keras.layers.Conv2D(filters=16, kernel_size=(5, 5), padding="valid"),
    keras.layers.BatchNormalization(),
    keras.layers.Activation("relu"),
    
    keras.layers.AveragePooling2D(pool_size=(2, 2), padding="valid"),
    keras.layers.BatchNormalization(),
    
    keras.layers.Flatten(),
    
    keras.layers.Dense(units=120, activation="relu"),
    keras.layers.BatchNormalization(),
    
    keras.layers.Dense(units=84, activation="relu"),
    keras.layers.BatchNormalization(),
    
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
    conv2d (Conv2D)              (None, 24, 24, 6)         156       
    _________________________________________________________________
    batch_normalization (BatchNo (None, 24, 24, 6)         24        
    _________________________________________________________________
    activation (Activation)      (None, 24, 24, 6)         0         
    _________________________________________________________________
    average_pooling2d (AveragePo (None, 12, 12, 6)         0         
    _________________________________________________________________
    batch_normalization_1 (Batch (None, 12, 12, 6)         24        
    _________________________________________________________________
    conv2d_1 (Conv2D)            (None, 8, 8, 16)          2416      
    _________________________________________________________________
    batch_normalization_2 (Batch (None, 8, 8, 16)          64        
    _________________________________________________________________
    activation_1 (Activation)    (None, 8, 8, 16)          0         
    _________________________________________________________________
    average_pooling2d_1 (Average (None, 4, 4, 16)          0         
    _________________________________________________________________
    batch_normalization_3 (Batch (None, 4, 4, 16)          64        
    _________________________________________________________________
    flatten (Flatten)            (None, 256)               0         
    _________________________________________________________________
    dense (Dense)                (None, 120)               30840     
    _________________________________________________________________
    batch_normalization_4 (Batch (None, 120)               480       
    _________________________________________________________________
    dense_1 (Dense)              (None, 84)                10164     
    _________________________________________________________________
    batch_normalization_5 (Batch (None, 84)                336       
    _________________________________________________________________
    dense_2 (Dense)              (None, 10)                850       
    =================================================================
    Total params: 45,418
    Trainable params: 44,922
    Non-trainable params: 496
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

    313/313 [==============================] - 1s 4ms/step - loss: 0.0273 - accuracy: 0.9927





    [0.0272968802601099, 0.9926999807357788]



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
      <td>0.019245</td>
      <td>0.993931</td>
      <td>0.046373</td>
      <td>0.988331</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.029051</td>
      <td>0.008527</td>
      <td>0.005838</td>
      <td>0.002323</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.003132</td>
      <td>0.953438</td>
      <td>0.037565</td>
      <td>0.980500</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.006448</td>
      <td>0.994307</td>
      <td>0.043261</td>
      <td>0.987771</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.010621</td>
      <td>0.996365</td>
      <td>0.046205</td>
      <td>0.989125</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.017502</td>
      <td>0.997828</td>
      <td>0.047923</td>
      <td>0.989750</td>
    </tr>
    <tr>
      <th>max</th>
      <td>0.158487</td>
      <td>0.999062</td>
      <td>0.063203</td>
      <td>0.990583</td>
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




    
![png](/../figs/2021-04-09-LeNet-5-with-MNIST/output_25_1.png)
    



    
![png](/../figs/2021-04-09-LeNet-5-with-MNIST/output_25_2.png)
    


## V - Conclusion

In this post, we built a LeNet-5 CNN to recognize digits with MNIST dataset. In comparison to [the previous post](https://newbiettn.github.io/2021/04/07/MNIST-with-keras/), this time, our model achieved a much better prediction accuracy on a seperate test set with 99.27% (vs. 96.66%). 


```python

```

