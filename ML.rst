.. Roles

.. role:: red

######
Machine Learning
######

Classification
##############

Binary classification metrics
*****************************

+-------------+--------------------+----------------------+
|             | :math:`\hat{y} = 1`| :math:`\hat{y} = 0`  |
+-------------+--------------------+----------------------+
|:math:`y = 1`|   :red:`TP`        |   FN                 |
+-------------+--------------------+----------------------+
|:math:`y = 0`|   FP               |   TN                 |
+-------------+--------------------+----------------------+

* *TP* (True Positive): When we say it's positive and it is
* *FN* (False Negtive): When we say it's negative and it is not
* *FP* (False Positive): When we say it's positive and it is not
* *TN* (True Negative): When we say it's negative and it is 


* TPR, True Positive Rate, (sensitivity, recall, hit rate)
   - +-----+-----+
     | TP* | FN  |
     +-----+-----+
     |     |     |
     +-----+-----+
   - :math:`\frac{TP}{TP + FN}`
   - :math:`p(\hat{y}=1 | y=1)`
   - The probability to say it is there when it is

* TNR, True Negative Rate, (specificity, selectivity)
   - +-----+-----+
     |     |     |
     +-----+-----+
     | FP  | TN* |
     +-----+-----+
   - :math:`\frac{TN}{FP + TN}`
   - :math:`p(\hat{y}=0 | y=0)`
   - The probability to say it's not there when it's not

* FNR, False Negative Rate
   - +-----+-----+
     | TP  | FN* |
     +-----+-----+
     |     |     |
     +-----+-----+
   - :math:`\frac{FN}{TP + FN}`
   - :math:`p(\hat{y}=0 | y=1)`
   - The probability to say it's not there when it is

* FPR, False Positive Rate
   - +-----+-----+
     |     |     |
     +-----+-----+
     | FP* | TN  |
     +-----+-----+
   - :math:`\frac{FP}{FP + TN}`
   - :math:`p(\hat{y}=1 | y=0)`
   - The probability to say it is there when it's not

* PPV, Positive Predictive Value, (precision)
   - +-----+-----+
     | TP* |     |
     +-----+-----+
     | FP  |     |
     +-----+-----+
   - :math:`\frac{TP}{TP + FP}`
   - :math:`p(y=1 | \hat{y}=1)`
   - The probability it's there when you say it is

* NPV, Negative Predictive Value
   - +-----+-----+
     |     | FN  |
     +-----+-----+
     |     | TN* |
     +-----+-----+
   - :math:`\frac{TN}{FN + TP}`
   - :math:`p(y=0 | \hat{y}=0)`
   - The probability it's not there when you say it is not

* FDR, False Discovery Rate
   - +-----+-----+
     | TP  |     |
     +-----+-----+
     | FP* |     |
     +-----+-----+
   - :math:`\frac{FP}{TP + FP}`
   - :math:`p(y=0 | \hat{y}=1)`
   - The probability it's not there when you say it is

* FOR, False Omission Rate,
   - +-----+-----+
     |     | FN* |
     +-----+-----+
     |     | TN  |
     +-----+-----+
   - :math:`\frac{FN}{FN + TP}`
   - :math:`p(y=1 | \hat{y}=0)`
   - The probability it is there when you say it is not


Additionally, 

* Accuracy
   - :math:`\frac{TP + TN}{P + N} = \frac{TP + TN}{TP + TN+ FP+ FN}`
   - The probability you get it right

* F1 score
   - :math:`2\frac{PPV TPR}{PPV + TPR} = \frac{2TP}{2TP + FP+FN}`
   - The harmonic mean of precision (PPV) and sensitivity (TPR). 
   

ROC Curve
---------

.. image:: /images/ML/ROC.png
    :scale: 100

The ROC curve plots the False Positive Rate against the True Positive rate, as the threshold of the classifier changes. The False Positive Rate is :math:`p(\hat{y}=1|y=0)`, is the probability to say it's there when it's not, and the True Positive Rate is :math:`p(\hat{y}=1|y=1)` is the probability to say it's there then it is.

A classifier that always says it's not there (:math:`\hat{y}=0`), will have TPR=FPR=0, and will lie at the bottom left corner of the graph. A classifier that says it's allways there (:math:`\hat{y}=1`), will have TPR=FPR=1, and will lie at the top right corner of the graph. Ideally, we want a classifier that always says it's there when it is (TPR=1) and never says it's there when it's not (FPR=0). This represents the top left corner. This gives rise to the requirement that a classifier's ROC curve passes as close as possible from the top right corner.



XGBoost
#######

Data Preparation
****************

.. code-block:: python

    X_train, X_test, y_train, y_test = train_test_split(
        X, Y, test_size=test_size, random_state=seed
    )
    dtrain = xgb.DMatrix(X_train, label=y_train)
    dtest = xgb.DMatrix(X_test, label=y_test)


Train
*****

https://xgboost.readthedocs.io/en/latest/python/python_api.html#module-xgboost.training

.. code-block:: python

    xgboost.train(
        params,  # The model parameters
        dtrain,  # The training data
        num_boost_round=10, # How many boosting rounds to carry out
        evals=(), # What evaluation metrics to watch
        obj=None, feval=None, maximize=False,
        early_stopping_rounds=None, # After how many rounds of non-decreasing metrics to stop
        evals_result=None, # Dictionary to store the evaluation results
        verbose_eval=True, # How often to print out the evaluation results
        xgb_model=None, callbacks=None
    )

* :code:`evals` should be in the form :code:`evals=[(dtrain, 'train'), (dtest, 'test')]`
* :code:`early_stopping_rounds` takes into account decreases in the last metric given in :code:`evals`.


Cross validation
****************

.. code-block:: python

    xgboost.cv(
        params, # Parameters
        dtrain, # Training data
        num_boost_round=10, #How many boosting rounds to carry out
        nfold=3, # How many folds for cross validation
        stratified=False, folds=None,
        metrics=(), # Metrics to watch during cross validation
        obj=None, feval=None, maximize=False,
        early_stopping_rounds=None, # After how many rounds of non-decreasing metrics to stop
        fpreproc=None, as_pandas=True, verbose_eval=None, show_stdv=True, seed=0,
        callbacks=None, shuffle=True
    )




Parameters
**********

Below are some of the most common parameters that go in both the :code:`xgboost.train` and :code:`xgboost.cv` functions. A complete reference can be found in https://xgboost.readthedocs.io/en/latest/parameter.html

**Learning Task Parameters**

objective
   The objective to be minimised. Some examples are :code:`reg:squarederror`, :code:`reg:squaredlogerror`, :code:`reg:logistic`, :code:`binary:logistic`, :code:`multi:softmax`, :code:`multi:softprob`.

eval_metric
   metrics used for evaluation: :code:`rmse`, :code:`rmsle`, :code:`mae`, :code:`logloss`, :code:`mlogloss`, :code:`auc`

**Control model complexity**

max_depth [=6]:
   Maximum depth of a tree. 
gamma [=0]:
   Minimum loss reduction required to make a further partition on a leaf node of the tree. The larger the value the more conservative the algorithm is.
min_child_weight [default=1]
   Minimum sum of instance weight needed in a child. Larger values make the algorithm more conservative.

**Add randomness**

subsample [=1]
   subsample ratio of the training samples
colsample_bytree [=1]
   subsample ratio of columns when constructing each tree.

**Learning rate**

learning_rate (eta):
   Shrinkage of the new weights to make the boosting process more conservative. When its value decreases, increase the :code:`num_boost_round` to compensate.

**Regularisation**

alpha [=0]:
    L1 regularisation
lambda [=1]:
    L2 regularisation

**Class imbalance**

scale_pos_weight:
    Useful for imbalanced classes. Class imbalanced classification is benefitted from using the auc evaluation metric. (https://xgboost.readthedocs.io/en/latest/tutorials/param_tuning.html).


Feature importance 
*******************


.. code-block:: python

    model.get_score(importance_type='weight|gain|cover|')
    import xgboost as xgb
    xgb.plot_importance(model, importance_type='weight|gain|cover|')


Plot trees 
***********
.. code-block:: python

    import xgboost as xgb
    # Plot the 3rd tree from the model in axes ax
    xgb.plot_tree(model, ax=ax, num_trees=3)


Useful stuff
############


.. code-block:: python

   from sklearn.metrics import mean_squared_error, accuracy_score
   from sklearn.model_selection import train_test_split

