# tf.estimator
It is a high-level API that is officially provided by TensorFlow. 
The advantage of using `tf.estimator` is that the code automatically build a graph and a tensorboard itself. 
A user only needed to specify features and labels and plug into the estimator functions. 
Just like PyTorch, the Estimator can differentiate `training` and `evaluation`. 
Also, estimators are built upon the `tf.keras.layers` which simplifies the customization.
Moreover, it is required to know the use of `estimator`  when you want to use TPU, because TensorFlow offers `TPUEstimator` which extends to `estimator`.


## Example code: [linear_regression.py](https://github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/examples/get_started/regression/linear_regression.py)

1. Create a dictionary, `feature_columns` that represents keys and values of features.

```py
feature_columns = [
	# "curb-weight" and "highway-mpg" are numeric columns.
	tf.feature_column.numeric_column(key="key1"),
	tf.feature_column.numeric_column(key="key2"),
]
```

`tf.feature_column.numeric_column` represents real valued or numerical features with designated key value.  `key1` and `key2` are categorical features of provided data.

2. Build the Estimator.

```py
model = tf.estimator.LinearRegressor(feature_columns=feature_columns)
```

We select linear regression as a deep learning model and previously built `feature_columns` to label.

Examples of models are listed below.

* [linear_regression.py](https://www.github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/examples/get_started/regression/linear_regression.py)
* [linear_regression_categorical.py](https://www.github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/examples/get_started/regression/linear_regression_categorical.py)
* [dnn_regression.py](https://www.github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/examples/get_started/regression/dnn_regression.py)
* [custom_regression.py](https://www.github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/examples/get_started/regression/custom_regression.py): to train a customized DNN regression model.

3. Train the model.

```py
# By default, the Estimators log output every 100 steps.
model.train(input_fn=input_train, steps=STEPS)
```

`input_train` is customized generator including batch and shuffle features containing train data. At every 100 steps, the Estimators compute the output.

4. Evaluate how the model performs on data it has not yet seen.

```py
eval_result = model.evaluate(input_fn=input_test)
```

`input_test` is another customized generator without shuffle feature containing validation data.

5. The evaluation returns a Python dictionary. The "average_loss" key holds the Mean Squared Error (MSE).

```py
average_loss = eval_result["average_loss"]
```

6. Run the model in prediction mode.

```py
input_dict = {
	"curb-weight": np.array([2000, 3000]),
	"highway-mpg": np.array([30, 40])
}
predict_input_fn = tf.estimator.inputs.numpy_input_fn(
	input_dict, shuffle=False)
predict_results = model.predict(input_fn=predict_input_fn)
```

Use the Estimator to predict the test data.


## ModeKeys and EstimatorSpec
Two are the fundamental classes defined at  [tensorflow/python/estimator/model_fn.py](https://www.github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/python/estimator/model_fn.py).
By calling `tf.estimator.ModeKeys.<ModeKeys>`, a user can switch different operations at corresponded modes.
All the `ModeKeys` have 3 kinds and each requires certain fields.

* **TRAIN** requires `loss` and `train_op`.
* **EVAL** requires `loss`.
* **PREDICT** requires `predictions`.

`tf.estimator` provides 3 kinds of regressors and classifiers.
* [tf.estimator.DNNRegressor](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor): for deep models that perform multi-class regression.
* [tf.estimator.DNNClassifier](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier): for deep models that perform multi-class classification.
* [tf.estimator.DNNLinearCombinedRegressor](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNLinearCombinedRegressor): for wide & deep models.
* [tf.estimator.DNNLinearCombinedClassifier](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNLinearCombinedClassifier): for wide & deep models.
* [tf.estimator.LinearRegressor](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor): for regressors based on linear models.
* [tf.estimator.LinearClassifier](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier): for classifiers based on linear models.

The use of `DNNClassifier`, for instance, is shown below.

```py
# Build a DNN with 2 hidden layers and 10 nodes in each hidden layer.
classifier = tf.estimator.DNNClassifier(
	feature_columns=my_feature_columns,
	# Two hidden layers of 10 nodes each.
	hidden_units=[10, 10],
	# The model must choose between 3 classes.
	n_classes=3)
```

Without calling `tf.variable_scope()` or building models, the graph, tensorboard, and model specification are all done automatically using `tf.estimator`.

Then, the specified operations and objects returned from a `model_fn` are passed through EstimatorSpec which is defined [here](https://github.com/tensorflow/tensorflow/blob/r1.11/tensorflow/python/estimator/model_fn.py#L67).
`EstimatorSpec` fully defines the model to be run by an Estimator.
Referred to the `ModeKeys`, each mode's required fields are passed through `EstimatorSpec`.

```py
tf.estimator.EstimatorSpec(
	mode=mode,
	predictions=predictions,
	loss=loss,
	train_op=train_op)
```

With `ModeKeys` and `EstimatorSpec`, models' train/eval/test functions are automatically inferred without creating each separated functions. 

## Next:
* More details of Estimator classes
* TPUEstimator
