# Credit Line Increase Model Card - Group 20

### Basic Information

* **Person or organization developing model**: Joon Kyu Hong, <joonhong96@gwu.edu>, 
                                               Bhavya Mounika Kundem, <bhavyamounika.kundem@gwu.edu>,
                                               Man Kuei Chen, <mankuei.chen@gwu.edu>
* **Model date**: August 23, 2022
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: [Modeling_Project_Deliverables_Group_20.ipynb](Modeling_Project_Deliverables_Group_20.ipynb)

### Intended Use
* **Primary intended uses**: This model is an *example* probability of default classifier, with an *example* use case for determining eligibility for a credit line increase.
* **Primary intended users**: Students in GWU DNSC 6301 bootcamp in 2022.
* **Out-of-scope use cases**: Any use beyond an educational example is out-of-scope.

### Training Data

* Data dictionary: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**ID**| ID | int | unique row indentifier |
| **LIMIT_BAL** | input | float | amount of previously awarded credit |
| **SEX** | demographic information | int | 1 = male; 2 = female
| **RACE** | demographic information | int | 1 = hispanic; 2 = black; 3 = white; 4 = asian |
| **EDUCATION** | demographic information | int | 1 = graduate school; 2 = university; 3 = high school; 4 = others |
| **MARRIAGE** | demographic information | int | 1 = married; 2 = single; 3 = others |
| **AGE** | demographic information | int | age in years |
| **PAY_0, PAY_2 - PAY_6** | inputs | int | history of past payment; PAY_0 = the repayment status in September, 2005; PAY_2 = the repayment status in August, 2005; ...; PAY_6 = the repayment status in April, 2005. The measurement scale for the repayment status is: -1 = pay duly; 1 = payment delay for one month; 2 = payment delay for two months; ...; 8 = payment delay for eight months; 9 = payment delay for nine months and above |
| **BILL_AMT1 - BILL_AMT6** | inputs | float | amount of bill statement; BILL_AMNT1 = amount of bill statement in September, 2005; BILL_AMT2 = amount of bill statement in August, 2005; ...; BILL_AMT6 = amount of bill statement in April, 2005 |
| **PAY_AMT1 - PAY_AMT6** | inputs | float | amount of previous payment; PAY_AMT1 = amount paid in September, 2005; PAY_AMT2 = amount paid in August, 2005; ...; PAY_AMT6 = amount paid in April, 2005 |
| **DELINQ_NEXT**| target | int | whether a customer's next payment is delinquent (late), 1 = late; 0 = on-time |

* **Source of training data**: GWU Blackboard, email <joonhong96@gwu.edu>,<bhavyamounika.kundem@gwu.edu>,<mankuei.chen@gwu.edu> for more information
* **How training data was divided into training and validation data**: 50% training, 25% validation, 25% test
* **Number of rows in training and validation data**:
  * Training rows: 15,000
  * Validation rows: 7,500

### Test Data
* **Source of test data**: GWU Blackboard, email <joonhong96@gwu.edu>,<bhavyamounika.kundem@gwu.edu>,<mankuei.chen@gwu.edu> for more information
* **Number of rows in test data**: 7,500
* **State any differences in columns between training and test data**: None

### Model details
* **Columns used as inputs in the final model**: 'LIMIT_BAL',
       'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6', 'BILL_AMT1',
       'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6',
       'PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6'
* **Column(s) used as target(s) in the final model**: 'DELINQ_NEXT'
* **Type of model**: Decision Tree 
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: Python version: 3.7.13
sklearn version: 1.0.2
* **Hyperparameters or other settings of your model**: 
```
DecisionTreeClassifier(ccp_alpha=0.0, class_weight=None, criterion='gini',
                       max_depth=6, max_features=None, max_leaf_nodes=None,
                       min_impurity_decrease=0.0, min_impurity_split=None,
                       min_samples_leaf=1, min_samples_split=2,
                       min_weight_fraction_leaf=0.0, presort='deprecated',
                       random_state=12345, splitter='best')`
```
### Quantitative Analysis

#### Correlation Heatmap
![Correlation Heatmap](download.png)

##### Pairwise Pearson correlation for the training data.

#### Iteration Plot
![plot](plot_1.png)

###### Here is the initial plot for tree depth vs. Training and Validation AUC

#### Table of Training and Validation AUC

![Training and Validation AUC](training_validation_auc.png)

##### Table of Training and Validation AUC
##### Based on the results above, we can find out the result that the difference between Training and Validation AUC becomes wider when tree depth is over 6.


#### Test Data 

##### We can get the 0.7438 AUC by using the test data.

#### Adverse Impact Ratios (AIR)

![AIR](air.png)

##### Validation AIR for race and sex protected groups.


#### Final Iteration Plot

![Iteration Plot](plot.png)

##### Iteration plot including Training AUC, Validation AUC, and AIR
##### 

### Ethical considerations

* **Describe potential negative impacts of using your model**: 
  * Math or software problems: Since the validation accuracy is approximately 70%, it indicates that there is a 30% probability of having a significant error while using the model.
  * Real-world risks: Someone trusts this model too much and reports the result from the decision tree without any specific consideration (ex: changing cutoff or checking any missing values). This hasty decision could cause damage or negative consequences to the company or individual. →   (This is called "automation complacency.")
* **Describe potential uncertainties relating to the impacts of using your model**:
  * Math or software problems: When it comes to operating the decision tree model, it is questionable if the privacy and personal information are perfectly protected.
  * Real-world risks: ( who, what, when or how) If someone uses this decision tree model to predict the credit eligibility for a particular race, then this can lead to some uncertainties because this model is just trained for a particular race. We can call this sample bias.
* **Describe any unexpected or results**:
When it comes to results about the variable importance, pay_0 shows an extremely remarkable output compared to other variables. In other words, if there is an error, unexpected change or missing values in pay_0, we can have unexpected results that we cannot predict and result could be extremely different from training.
