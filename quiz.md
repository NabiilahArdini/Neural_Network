# Neural Network Quiz

This quiz is part of Algoritma Academy assessment process. Congratulations on completing the Neural Network course! We will conduct an assessment quiz to test practical neural network techniques you have learned on the course. The quiz is expected to be taken in the classroom, please contact our team of instructors if you missed the chance to take it in class.

To complete this assignment, you need to build your classification model to classify the categories of fashion image using Neural Network algorithm in `Keras` framework by following these steps:

# 1. Data Preparation

Let us start our neural network experience by first preparing the dataset.  You will use the `fashionmnist` dataset. The data is stored as .csv files inside the `fashionmnist` folder from the course material and consists of train and test set of 10 different categories for 28 x 28 pixel sized fashion images. Use the following glossary for your target labels:

```
categories <- c("T-shirt", "Trouser", "Pullover", "Dress", 
    "Coat", "Sandal", "Shirt", "Sneaker", "Bag", "Boot")
```

## 1.1 Load the library and data

Please load the following package.

```
library(readr)
library(keras)
library(caret)
library(dplyr)
```

In this phase, please load and investigate our fashionmnist data and store it as `fashion_train` and `fashion_test`. Please use the `read_csv()` function from the `readr` package to speed up the data reading process.

```
fashion_train <- read_csv(...)
fashion_test <- read_csv(...)
```

Inspect the `fashion_train` data by using `head()` function.

```
# your code here
```

The `fashion_train` data consists of 60000 observations and 785 variables (1 target and 784 predictors). Each predictor represent pixels of the image.

## 1.2 Convert the data to matrix

The data contains the value of pixels stored in **data.frame**. However, we have to convert the data into the matrix before we create a model, hence, please convert the data to be matrix format using `data.matrix()` function and store the `fashion_train` matrix as `train_m` and `fashion_test` matrix as `test_m`.

```
train_m <- data.matrix(...)
test_m <- data.matrix(...)
```

## 1.3 Cross Validation

After that, we should separate the predictors and the target in our `train_m` and `test_m` data. 

```
# Predictor variables in `train_m`
train_x <-  ...

# Predictor variables in `test_m`
test_x <- ...

# Target variables in `train_m`
train_y <- ...

# Target variables in `test_m`
test_y <- ...

```

## 1.4 Prepare training and testing set (change to an array)

Next, we should convert the matrix of predictors into an array shape. Please use the `array_reshape(data, dim(data))` to convert the data.

```
train_x_array <- array_reshape(..., dim(...))
test_x_array <- array_reshape(..., dim(...))
```

## 1.5 Features scaling

The next preparation before the data is ready to be modeled is feature scaling. Now, please answer this question first.

___
1. After the data has been reshaped to an array, you must continue the preprocessed before training the network. If you inspect an arbitrary image in the training set, you will see that the pixel values fall in the range of 0 to 255. Then, what does the array divide by 255 do?
  - [ ] Convert the array between 0 to 255 into 0 to 1
  - [ ] Reshape the width and height into a single dimension since the data is a 3-d array (images, width, height)
  - [ ] Normalize the array between 0 to 255 into -1 to 1
___

The next step is to scale the value of the array (`train_x_array` and `test_x_array`) by dividing it to 255.

```
train_x.keras <- train_x_array/...
test_x.keras <- test_x_array/...
```

To prepare the data for the training model, we apply one-hot encoding to the target variable (`train_y`) using `to_categorical()` function from `Keras` and stored it as `train_y.keras` object.

```
train_y.keras <- ...
```

# 2 Build Neural Network Model

Before we applied the neural network to fashionmnist dataset, we should check the necessary knowledge about the neural network by answering the following questions:

___
2. The statement below is correct about Neural Networks, *EXCEPT*
  - [ ] Input layer is the first layer in Neural Network, the number of neurons depends on the predictor variable on the data
  - [ ] The initial weight  for each neuron is defined randomly
  - [ ] Activation functions are not doing any transformation to its previous layer
  - [ ] The neural network is called deep learning when it has more than one hidden layer
___

___
3. The neural network model is built to optimize (minimizing) the error, what kind of error in Regression case that we minimized?
  - [ ] Binary Crossentropy
  - [ ] Mean Absolute Error
  - [ ] Neuron weight
___

## 2.1 Build a model base using `keras_model_sequential()`

To organize the layers, we should create a base model, which is a Sequential model. Call a `keras_model_sequential()` function, and please pipe the base model with the model architecture.

## 2.2 Building Architecture (define layers, neurons, and activation function)

To define the architecture for each layer, we will build several models by tuning several parameters. Before building the architecture, we set the initializer to make sure the result will not change.

```
set.seed(100)
initializer <- initializer_random_normal(seed = 100)
```

First, create a model (stored it as `model_init`) by defining the following parameters:
- the first layer contains 32 nodes, relu activation function, 784 input shape
- the second layer contains 32 nodes, relu activation function
- the third layer contains 10 nodes, softmax activation function

```
model_init <- keras_model_sequential() %>% 
  layer_dense(units = ..., activation = "...", input_shape = c(...),
              kernel_initializer = initializer, bias_initializer = initializer) %>% 
  layer_dense(units = ..., activation = "...",
              kernel_initializer = initializer, bias_initializer = initializer) %>% 
  layer_dense(units = ..., activation = "...", 
              kernel_initializer = initializer, bias_initializer = initializer)
```

Second, create a model (stored it under `model_bigger`) by defining the following parameters:
- the first layer contains 512 nodes, relu activation function, 784 input shape
- the second layer contains 512 nodes, relu activation function
- the third layer contains 10 nodes, softmax activation function

```
model_bigger <- keras_model_sequential() %>% 
  layer_dense(units = ..., activation = "...", input_shape = c(...),
              kernel_initializer = initializer, bias_initializer = initializer) %>% 
  layer_dense(units = ..., activation = "...",
              kernel_initializer = initializer, bias_initializer = initializer) %>% 
  layer_dense(units = ..., activation = "...", 
              kernel_initializer = initializer, bias_initializer = initializer)
```

Please answer the following question.

___
4. In building the model architecture, we set several numbers of units. Below is the consideration using those number, *EXCEPT*
  - [ ] In the first layer, we use 784 input shape based on the number of our predictors
  - [ ] In the hidden and output layer, we use any even number
  - [ ] In the output layer, we use 10 that is the number of our categories
___

## 2.3 Building Architecture (define cost function and optimizer)

We still need to do several settings before training the `model_init` and `model_bigger`. We must compile the model by defining the loss function, optimizer type, and evaluation metrics. Please compile the model by setting these parameters:
- categorical crossentropy as the loss function
- adam as the optimizer with learning rate of 0.001
- use accuracy as the evaluation metric

```
model_init %>% 
  compile(loss = "...", 
          optimizer = ...(lr = ...), 
          metrics = "...")
```

```
model_bigger %>% 
  compile(loss = "...", 
          optimizer = ...(lr = ...), 
          metrics = "...")
```


## 2.4 Fitting model in the training set (define epoch and batch size)

In this step, we fit our model using `epochs = 10` and `batch_size = 100` for the `model_init` and `model_bigger`. Please save the model in `history_init` and `history_bigger` object.

```
history_init <- model_init %>%
  fit(train_x.keras, train_y.keras, epoch = ..., batch_size = ...)
```

```
history_bigger <- model_bigger %>% 
  fit(train_x.keras, train_y.keras, epoch = ..., batch_size = ...)
```

___
5. In the fitting model above, we set `epochs = 10`, which means ...
  - [ ] Number of iteration the model trained with all the data train
  - [ ] The model does the weighting for all batch 10 times
  - [ ] The model divides the batch 10 times
___

# 3 Predicting on the testing set

To evaluate the model performance toward unseen data, we will predict the testing (`test_x.keras`) data using the trained model. Please predict using `predict_classes()` function from `Keras` package and store it as `pred_init` and `pred_bigger`.

```
pred_init <- keras::predict_classes(object = ..., x= ...)

pred_bigger <- keras::predict_classes(object = ..., x= ...)
```

# 4 Evaluating the neural network model

As the label is still in dbl type, then please decode the label based on its categories.

```
decode <- function(data){
  sapply(as.character(data), switch,
       "0" = "T-Shirt",
       "1" = "Trouser",
       "2" = "Pullover",
       "3" = "Dress",
       "4" = "Coat",
       "5" = "Sandal",
       "6" = "Shirt",
       "7" = "Sneaker",
       "8" = "Bag",
       "9" = "Boot")
}
```

Decode the `pred_init` and `pred_bigger` before we evaluate the model performance using confusion matrix. You also need to decode the `test_y` vector to get the decoded actual/true label of the target variable.

```
reference <- decode(test_y)
pred_decode_in <- decode(...)
pred_decode_big <- decode(...)
```

## 4.1 Confusion Matrix (classification)

After decoding the target variable, then you can evaluate the model using several metrics, in this quiz, please check the accuracy in the confusion matrix below.

Note: do not forget to do the explicit coercion `as.factor`.

```
library(caret)
confusionMatrix(as.factor(...), as.factor(...))
confusionMatrix(as.factor(...), as.factor(...))
```

___  
  6. From the two confusion matrix, what can we infer?
  - [ ] The more the neuron, the model tends to overfit
  - [ ] The more the neuron, the model tends to underfit
  - [ ] The number of neuron in the hidden layer does not relate with underfit or overfit
___
  
# 4.2 Model Tuning

It turns out our boss wants to get the best model, so he asks you to compare one model to another model (store it as `model_tuning`). Now, let us try to build the `model_tuning` by tuning these while compiling the model :
- used the sgd as the optimizer with learning rate 0.001
- the rest is the same with `model_init`

```
model_tuning <- keras_model_sequential() %>% 
  layer_dense(units = 32, activation = "relu", input_shape = c(784)) %>% 
  layer_dense(units = 32, activation = "relu") %>% 
  layer_dense(units = 10, activation = "softmax")

model_tuning %>% 
  compile(loss = "categorical_crossentropy", 
          optimizer = ...(lr = 0.001), 
          metrics = "...")

history_tuning <- model_tuning %>%
  fit(train_x.keras, train_y.keras, epochs = 10, batch_size = 100)
```

After tuning the model, please do the predict `test_x.keras` using `model_tuning`.

```
pred_tuning <- keras::predict_classes(object = ..., x= ...)
```

Then, decode the `pred_tuning` and check the model performance using `confusionMatrix`.

```
pred_decode_tun <- decode(...)
confusionMatrix(as.factor(...), as.factor(...))
```

Please answer the following question.
___
7. The optimizer used to update the weight to minimize the loss function. What can you conclude from the model_init and model_tuning about the optimizer?
  - [ ] Optimizer Adam is more powerful than Sgd
  - [ ] Optimizer Sgd is more powerful than Adam
  - [ ] Both of the optimizers do not influence the model performance
___

___
8. From the two models above(`model_init`, and `model_tuning`), what is the best model for us to pick?
  - [ ] model_tuning, because the model has higher and more balanced accuracy between train and test
  - [ ] model_init, because the model has higher and more balanced accuracy between train and test
  - [ ] model_init, because the model has higher recall than model_tuning
___
