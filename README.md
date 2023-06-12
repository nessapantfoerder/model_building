# Power Outage Analysis Model Building
UCSD DSC 80 Project 5

by Nessa Pantfoerder

---

## Introduction

This dataset outlines major power outages in the continental U.S. from January 2000 to July 2016 providing information on population, region, land area, consumption, outage duration and causes, as well as regional climate information.

Our exploratory data analysis on this dataset can be found ([here](https://nessapantfoerder.github.io/power_outage_analysis/)).

---

## Framing the Problem

**Prediction Problem**
- Predict the cause of a major power outage.

**Type**
- Multiclass Classification

**Response Variable**
- CAUSE.CATEGORY
- CAUSE.CATEGORY represents the categories of all the events causing the major power outages rather than just the descriptions of the categories like in CAUSE.CATEGORY.DETAIL

**Metric**
- F1 score
- Our dataset has an uneven distribution of classes making accuracy misleading. A model that predicts the majority class most of the time could achieve high accuracy, but it may perform poorly on the minority class. The F1 score considers both precision and recall, which are valuable for evaluating the performance on imbalanced classes.

---

## Baseline Model

**Description**

The model used is a Decision Tree classifier. The model aims to predict the cause of major power outages based on: 'CLIMATE.CATEGORY' and 'CLIMATE.REGION' from the dataset.

**Features**
- 'CLIMATE.CATEGORY' is a nominal categorical variable that represents the category of climate associated with the outage
- 'CLIMATE.REGION' is also a nominal categorical variable representing the region where the outage occurred

**Encodings**

To handle categorical features, the OneHotEncoder encoding scheme is used within the ColumnTransformer step of the pipeline. This encoding transforms each categorical feature into a binary vector, where each category is represented by a separate binary column.

**Performance**

The F1 score of 0.42 indicates that the baseline model has moderate performance in terms of precision and recall. The F1 score takes into account both the precision (ability to correctly identify positive cases) and recall (ability to capture all positive cases) of a model. 

The F1 score suggests that the model may have difficulties accurately predicting the cause of major power outages. With further feature engineering, model selection, hyperparameter tuning, or collection of data there is significant room for improvement.

---

## Final Model

**Features Added**
- 'OUTAGE.DURATION': This feature denotes the duration of each outage event. Longer outage durations may indicate more severe disruptions and potentially different causes. By including this feature, the model can capture the relationship between outage duration and the cause category, potentially improving its ability to differentiate between different outage causes based on their durations.
- 'CUSTOMERS.AFFECTED': This feature represents the number of customers affected by each outage event. The magnitude of customer impact can vary depending on the cause category. Certain causes, such as equipment failure, may affect a smaller number of customers compared to  causes like severe weather events. By including this feature, the model can capture the relationship between the number of affected customers and the cause category, which can provide additional predictive power.

**Modeling Algorithm**

The modeling algorithm chosen for the final model is the Decision Tree Classifier. Decision trees can handle both numerical and categorical features, making them suitable for the mixed data types present in the dataset. The Decision Tree Classifier was selected based on its interpretability, ability to handle non-linear relationships, and robustness to outliers.

**Hyperparameters**

The hyperparameters that performed the best for the model were 'model__max_depth': None, 'model__min_samples_leaf': 1, and 'model__min_samples_split': 5. To select the best hyperparameters, a grid search cross-validation approach was used. The hyperparameter grid consisted of different combinations of 'max_depth', 'min_samples_split', and 'min_samples_leaf' and the F1 score was used as the scoring metric. Grid search cross-validation searched the hyperparameter space and evaluated the model's performance on multiple cross-validation folds.

The final model's performance, as measured by the F1 score, is an improvement over the baseline model's performance. The inclusion of additional features, outage duration, and customers affected, provides the model with more information to make accurate predictions. By considering these features, the model can capture the relationships specific to the data generating process of power outages. The hyperparameter tuning further refines the model's performance by finding the optimal parameters of the decision tree classifier. The resulting F1 score of 0.69 in the final model indicates an improvement over the baseline model, demonstrating the effectiveness of the selected features and the tuned hyperparameters in enhancing predictive performance.

![Confusion Matrix](confustion-matrix.png)

---

## Fairness Analysis

**Group X**
- Outages in warm climates.

**Group Y**
- Outages in non-warm climates (e.g., normal, cold).

**Evalutation Metric** 
- Precision

**Null Hypothesis**
- Our model is fair. Its precision for warm climates and other climates are roughly the same, and any differences are due to random chance.

**Alternative Hypothesis**
- Our model is unfair. Its precision for warm climates is lower than its precision for all other climates.
- The differences in our groups cannot be explained by random chance alone.

**Test Statistic**
- Our test statistic will be:
    - *precision of model for Group X* − *precision of model for Group Y*

**Significance Level**
- Our significance level will be 0.05.

**P-Value**
- The p-value is 0.0.

**Conclusion**
- Under the null hypothesis, we rarely see differences in precision as large as 0.0229. 
- Therefore, we reject the null hypothesis that the two groups perform the same. 
- Thus the observed differences in our groups' performance are not due to random chance.

<iframe src="fairness-analysis.html" width=800 height=600 frameBorder=0></iframe>

---
