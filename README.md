# Project Overview
*Please see rl_datix_exercise.ipynb for relevant code.*

# Classification Model
Task: develop a model to predict whether a patient will be readmitted to hospital within 30 days
## Exploratory Analysis
The target feature is imbalanced
0: 67.5%
1: 32.5%

Exploratory analysis reveals several potentially predictive features
- num previous admissions

  <img width="363" height="273" alt="image" src="https://github.com/user-attachments/assets/07152c78-13df-4511-ba20-6c88bbba46c4" />

- diagnosis code

  <img width="385" height="273" alt="image" src="https://github.com/user-attachments/assets/0fe9d4d2-c443-4d85-b113-543cd8460f9d" />

- medication type
  
  <img width="390" height="273" alt="image" src="https://github.com/user-attachments/assets/72866a5d-0298-4742-89f8-fe59b58f4c53" />
  
There is a fair amount of overlap in common comments in the discharge notes between the two classes. Common phrases used for both classes include:
- "no complications"
- "monitoring for relapse advised"
- "recommend follow up / follow-up scan scheduled"
- "pneunomia"

## Modeling

### Approach
#### Features
We will dummy categorical variables and derive embeddings from discharge notes. We will also use PCA to reduce the dimensionality of the embedding feature space. No transformations will be used for numeric features.
#### Architecture
We will use a random forest as it tends to be a strong out of the box model. We will use max_depth=3 and n_estimators=10 as a simple baseline model. Given the class imbalance, we will use class weighting in training.
### Performance

  | Train | Test |
--- | --- | --- |
ROC AUC | 0.81 | 0.56 |
F1 Score | 0.65 | 0.5 |
Recall | 0.77 | 0.47 |

**Confusion Matrix**
  | Pred 0 | Pred 1 |
--- | --- | --- |
True 0 | 16 | 7 |
True 1| 9 | 8 |

Performance metrics indicate we are overfitting to the training set. We also have a tendency to underpredict the positive class (which is not entirely unexpected given the class imbalance). If we are using this model to inform scheduling of hospital staff, it may be better to overpredict rather than under predict (we'd rather over staff than under staff). In this case we may look to optimize recall. 

### Feature Importance
<img width="1286" height="786" alt="image" src="https://github.com/user-attachments/assets/e7a64069-4913-44aa-a5b8-a61be816856e" />

The embeddings derived from the discharge notes are by far the most important features. It is possible this may explain some of the overfitting we're seeing as these features are very rich.

## Future Work & Improvements

Next steps for this model would include
1. hyperparameter tuning: formal hyperparameter tuning may help to reduce overfitting and improve performance. Hyperparameters to tune include: max_depth, n_estimators, min_samples_leaf, embedding model, pca dimensions.
2. data expansion: if possible we would also look to increase (1) the # of records in our dataset and (2) the attributes describing patient condition. Additional records could help to both reduce overfitting and improve performance. Additional attributes might give the model more signal to learn from and improve performance. Additional attributes might include information about patient condition shortly after their discharge, possibly retrieved via a short questionnaire in an online patient portial.

# Named Entity Recognition
