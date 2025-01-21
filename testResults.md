## Model Training and Stacking Results

This report summarizes the performance of several base models and two stacking models for predicting changes in NIRF ranking.

### Base Model Performance

The following table shows the performance of each base model after hyperparameter tuning using 5-fold cross-validation. The metrics reported are Out-of-Fold (OOF) Root Mean Squared Error (RMSE) and OOF R-squared (R<sup>2</sup>).

| Model             | Best Hyperparameters                                                                     | OOF RMSE    | OOF R<sup>2</sup> |
| :---------------- | :--------------------------------------------------------------------------------------- | :---------- | :----------- |
| Linear Regression | `{}`                                                                                     | 1317.69     | -7288.05     |
| Decision Tree     | `{'max_depth': None, 'min_samples_leaf': 10, 'min_samples_split': 2}`                   | 17.46       | -0.28        |
| Random Forest     | `{'max_depth': None, 'min_samples_split': 10, 'n_estimators': 100}`                     | 17.79       | -0.33        |
| SVM               | `{'C': 0.1, 'gamma': 'scale', 'kernel': 'linear'}`                                       | 15.64       | -0.03        |
| kNN               | `{'algorithm': 'auto', 'n_neighbors': 3, 'weights': 'distance'}`                         | 17.08       | -0.22        |
| Neural Network   | `{'hidden_layer_sizes': (50,50), 'activation': 'relu', 'alpha': 0.0001, 'max_iter': 1000}` |   74.70          |   -20.70           |

**Observations:**

*   **Linear Regression** performed exceptionally poorly, suggesting that a linear model is not suitable for this dataset and task.
*   **Decision Tree, Random Forest, SVM, and kNN** show more reasonable, but still relatively poor performance with negative R<sup>2</sup> values, indicating that these models, in their current configurations, are not capturing the underlying patterns in the data well.
*   **Neural Network** also performed poorly which suggests that either the chosen hyperparameters were not optimal, the model architecture might need adjustments, or a more complex neural network might be required to model the data effectively.

### Stacking Model Performance

Two stacking models were trained using the out-of-fold predictions of the base models as input features. The meta-models used were Linear Regression and Lasso Regression.

#### Stacking with Linear Regression (Meta-Model)

| Metric | Value  |
| :----- | :----- |
| RMSE   | 15.41  |
| R<sup>2</sup> | 0.003  |

**Best Hyperparameters:** `{'fit_intercept': True, 'positive': True}`

#### Stacking with Lasso Regression (Meta-Model)

| Metric | Value  |
| :----- | :----- |
| RMSE   | 7.48   |
| R<sup>2</sup> | 0.76   |

**Best Hyperparameters:** `{'alpha': 1, 'max_iter': 1000}`

**Observations:**

*   The stacking model with **Linear Regression** as the meta-model did not significantly improve upon the base models.
*   The stacking model with **Lasso Regression** as the meta-model achieved a substantial improvement, with a much lower RMSE and a positive R<sup>2</sup> of 0.76. This indicates that the Lasso meta-model was able to effectively combine the predictions of the base models.

### Conclusion

The **Lasso Regression stacking model** demonstrated the best performance among all models tested. This suggests that a non-linear combination of base model predictions, with appropriate regularization (as provided by Lasso), is a promising approach for this ranking prediction task.
