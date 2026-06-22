# Executive Summary
A machine learning classifier was made to detect fraud accounts to limit company losses & provide accounts to legitimate applicants. The best performing model showcased an 11% False-Positive Rate with emphasis on recommendations to improve data and model quality.

# Problem Statement
With fraud cases present in the banking industry. A detection algorithm is needed to identify fraudulent accounts to avoid company losses and provide accounts to only legitimate applicants. 

# Data Acquisition
The dataset used comes from the Bank Account Fraud suite of datasets. This is published at the NeurIPS 2022. The base variant was chosen for best representation of the original datasets used for fraud detection. The dataset is already in a structured format with no empty values. Full details can be found at the [Kaggle Page](https://www.kaggle.com/datasets/sgpjesus/bank-account-fraud-dataset-neurips-2022).

# Data Preparation
- **Month-based Train-test Split:** As recommended by the paper, the first 6 months served as the training set and the remaining 2 months as the test set.
- **Imputation using Medians:** The training set itself has no empty values, however, numeric columns have their missing values coded as "-1." As this encoding impacts model estimates, these values are replaced by the column-wise median ignoring the "-1" values. The median is chosen due to the skewed distributions of all columns using this encoding.
- **Removal of Redundant Columns:** Checking the column-wise variabilities, device_fraud_count only has a single value present for the entire training set. With this in mind, I dropped this column to reduce the number of features to be considered by the machine learning models.
- **Fraud & Non-Fraud Balancing:** There is a severe imbalance of fraud and non-fraud cases. To address this, I utilized randomized undersampling to match the number of fraud and non-fraud cases. I understand the loss of information with this method, with its effects seen later on, but this method was chosen over other methods (randomized oversampling or SMOTE) due to computational constraints. 
- **Scaling of Continuous Columns:** Lastly, with the balanced training set, numeric columns are scaled using StandardScaler() for better model performance. 

# Model Building
- **Utilization of Various Algorithms:** With many classifiers available in the scikit-learn package, I chose to build base models with default parameters using logistic regression, stochastic gradient descent (SGD), k-nearest neighbors (KNN), decision trees, AdaBoost, random forests, gradient boosted trees, and support vector machines (SVM). For this project, I will refer to the untuned logistic regression classifier as the reference classifier due to its simplicity and interpretability over other models.
- **Evaluation of Fit of Base Models:** 5-fold cross-validation was done to determine the in-sample false positive rate (FPR) with AdaBoost showing the lowest median FPR. <br/> <img width="630" height="470" alt="untuned models cv" src="https://github.com/user-attachments/assets/c860c4be-5df9-46f1-8a73-55cede44b8f0" /> 
- **Evaluation of Predictive Ability of Base Models:** Test FPRs were also observed for the models with gradient boosting showing the best performance. It can also be noticed that overfitting occured for the SVM model due to the massive difference in percentage points between the cross-validation performance and the test performance. <img width="678" height="455" alt="untuned models test fpr" src="https://github.com/user-attachments/assets/6412480e-babb-4ef0-a079-b41248ea0178" />

# Hyperparameter Tuning
- **Selection of Hyperparameters:** Using RandomizedSearchCV, various parameters were explored where the parameters containing the lowest 5-fold cross-validation FPR was selected. RandomizedSearchCV was used for its computational efficiency, especially with multiple algorithms used in the project which can drastically increase running time if GridSearchCV was used.
- **Addition of Voting Classifier:** With the best performing tuned models, a voting ensemble classifier was also built using the predictions from each tuned model.
- **Evaluation of Fit of Tuned Models:** AdaBoost still remains the best performing model when it comes to the 5-fold cross-validation. <br/> <img width="654" height="470" alt="tuned models cv" src="https://github.com/user-attachments/assets/78e2449a-bae7-454e-ac57-b3720da3e4fa" />
- **Evaluation of Predictive Ability of Tuned Models:** Gradient boosting still remains the best performing model when it comes to test performance. <br/> <img width="629" height="470" alt="tuned models and voting test fpr" src="https://github.com/user-attachments/assets/daf1c489-d0d5-433a-a058-206f03703349" />
- **Comparisons between Tuned & Base Model Performance:** A surprising observation is that the base gradient boosted model performed better than the tuned model, albeit being close in FPR.<br/> <img width="633" height="470" alt="tuning changes" src="https://github.com/user-attachments/assets/d88fca4b-bab5-4338-b66f-17f1ac3f97c9" />

# Final Model
- With the Gradient Boosting Decision Trees having best performance rates, This is the best learning algorithm for this project. When it comes to parameters, I chose the tuned parameters despite the higher FPR due to the small differences that can be seen between the tuned and test metrics.
- Even 5% FPR was not achieved by any of the classifiers, a model was built that reduced the FPR of the reference classifier (base logistic regression) by approximately 40%.

# Recommendations
- **Explore other Time Splits:** The project uses the split done in the paper done by the dataset authors. Thus, other time splits (different time splits, rolling window, expanding window) could be explored.
- **Feature Engineering:** With the only modification done the columns being scaling and dummy encoding, creation of other features e.g. PCA could be done.
- **Use other balancing methods:** Since randomized undersampling was used to balance the positive and negative cases, which led to a great amount of information lost, other balancing methods could be considered.
- **Use other machine learning algoirthms:** All classifiers are in the scikit-learn package and familiar with me. More complex models from other packages (e.g. neural networks) can be utilized.
- **Perform more exhaustive tuning:** With the tuning algorithm being RandomizedSearchCV and the hyperparameters being the most common parameters used in the machine learning algorithms, GridSearchCV and addition of more hyperparameters of consideration could be utilized.
- **Model deployment:** To form predictions with new data outside of the test set, the best performing model can be deployed to an API and create predictions for potential account openers.

# Citation
```bibtex
@article{jesusTurningTablesBiased2022,
  title={Turning the {{Tables}}: {{Biased}}, {{Imbalanced}}, {{Dynamic Tabular Datasets}} for {{ML Evaluation}}},
  author={Jesus, S{\'e}rgio and Pombal, Jos{\'e} and Alves, Duarte and Cruz, Andr{\'e} and Saleiro, Pedro and Ribeiro, Rita P. and Gama, Jo{\~a}o and Bizarro, Pedro},
  journal={Advances in Neural Information Processing Systems},
  year={2022}
}
```
