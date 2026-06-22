# Executive Summary

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
- 

# Hyperparameter Tuning

# Final Model

# Recommendations

# Citation
```bibtex
@article{jesusTurningTablesBiased2022,
  title={Turning the {{Tables}}: {{Biased}}, {{Imbalanced}}, {{Dynamic Tabular Datasets}} for {{ML Evaluation}}},
  author={Jesus, S{\'e}rgio and Pombal, Jos{\'e} and Alves, Duarte and Cruz, Andr{\'e} and Saleiro, Pedro and Ribeiro, Rita P. and Gama, Jo{\~a}o and Bizarro, Pedro},
  journal={Advances in Neural Information Processing Systems},
  year={2022}
}
```
