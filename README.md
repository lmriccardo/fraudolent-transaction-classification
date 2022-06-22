# Fraudolent Transaction Classification

This is the repository of the project for the Big Data Computing course at La Sapienza.

## Introduction

Financial fraud is a problem that has proved to be a menance and has a huge impact on the financial industry. Data mining is one of the techniques which has played an important role in credit card fraud detection in transactions which are online. Credit card fraud detection has proved to be a challenge mainly due to the 2 problems that it poses: both the profiles of fraudolent and normal behaviours change and data sets used are highly skewed. The performance of fraud detection is affected by the variables used and the technique used to detect fraud.

With this project I have experienced different Machine Learning techniques to predict whether a transaction has an high probability of being fraudolent or not. To this end, I used: *Decision Trees*, *Random Forest*, (simple) *Logistic Regression* and (maybe) *Neural Network* (from scratch) approach. 

---

## The Dataset

The data I used is available on Kaggle at this [link](https://www.kaggle.com/c/ieee-fraud-detection). The dataset is divided into train set and test set, both in turn divided into two files called `<train|test>_identity.csv` and the second `<train|test>_transaction.csv`. Here, there is a resume of categorical and numerical features of the Transaction table:

<ins>*Categorical Features*</ins> - Transaction

- `ProductCD`: product code, the product for each transaction
- `card1 - card6`: payment card information, such as card type, card category, issue bank, country, etc.
- `addr1,addr2`: billing region and billing country addresses
- `P_emaildomain`: purchaser email domain
- `R_emaildomain`: recipient email domain
- `M1-M9`: match, such as name card and addresses, etc.
- `isFraud`: 0 if it is okay, 1 otherwise

<ins>*Numerical Features*</ins> - Transaction

- `TransactionDT`: timedelta from a given reference datetime (not an actual timestamp)
- `TransactionAMT`: transaction payment amount in USD
- `C1-C14`: counting, such as how many addresses are found to be associated weith the payment card, etc.
- `D1-D15`: timedelta, such as deys between previous transaction, etc.
- `Vxxx`: Vesta engineered rich features, including ranking, counting, and other entity relations

While, in Identity table variables are identity information - network connection information (IP, ISP, Proxy, etc) and digital signature (UA/browser/os/version, etc) associated with transactions. `id01` to `id11`are numerical features for identity, which is collected by Vesta and security partners such as device rating, ip_domain rating, proxy rating, etc. All of these are not able to elaborate due to security. `DeviceType` is the type of the device used to pay (`nan`, `mobile`, `desktop`), while `DeviceInfo` describes the type of devices used like SAMSUNG, HUAWEILDN and LG, etc. 

In total the dataset has 590540 entires per 434 features.

---

## Machine Learning Pipeline

Like I said, I used three classical Machine Learning models to the end of the project: *Logistic Regression*, *Decision Tree* and *Random Forest*. To make predictions be more accurate I choose to apply each of the previous three models using a **K-Fold Cross Validation** approach with K=5. In this way I also fine-tuned model's parameter: `regParam` and `elasticNetParam` (for LR), `maxDepth` and `Impurity` (for DT), and, `maxDepth` and `numTrees` (for RF). Before the Cross Validator I decided to apply a simple initial pipeline consisting of: StringIndexer, OneHotEncoder, VectorAssembler and StandardScaler. Finally, this is the overall ML Pipeline

<img src="https://i.imgur.com/JZlw5iE.png" alt="Machine Learning Pipeline" />

According to the above image StandardScaler and OneHotEncoder are inside an "Optional" Box. That's because: OneHotEncoder is used only if the train set contains also categorical features, except for Decision Tree in which it is not applied at all; StandardScaler is applied only if it is required by the experiments.

---

## Experiments and Results

The following image describes the runned experiments

<img src="https://i.imgur.com/KsJRO2H.png" alt="Experiments" width=550 height=250/>

The following tables shows the results given by the previous experiments

|        *Accuracy*       |  **Numerical**  | **All Features** | **Categorical** |
|-------------------------|:---------------:|:----------------:|:---------------:|
| **Logistic Regression** | 0.9772 - 0.97735 ( - )|   0.9777 ()|      0.9733 () |
| **Decision Tree**       | 0.9773 - 0.9773  ( - )|              ()|      0.9734 () |
| **Random Forest**       |       - **0.9798** ()          |   ()                | ()                 |

|        *AUC ROC*       |  **Numerical**  | **All Features** | **Categorical** |
|-------------------------|:---------------:|:----------------:|:---------------:|
| **Logistic Regression** | 0.832 - 0.834 ( - )|   0.857 ()|      0.800 () |
| **Decision Tree**       | 0.428 - 0.428  ( - )|              ()|      0.707 () |
| **Random Forest**       |       - **0.845** ()          |   ()                | ()                 |

|        *F1-Score*       |  **Numerical**  | **All Features** | **Categorical** |
|-------------------------|:---------------:|:----------------:|:---------------:|
| **Logistic Regression** | 0.7137 - 0.7139 ( - )|   0.7227 ()|      0.5897 () |
| **Decision Tree**       | 0.7138 - 0.7138  ( - )|              ()|      0.5961 () |
| **Random Forest**       |       - **0.7438** ()          |   ()                | ()                 |

|        *Precision*       |  **Numerical**  | **All Features** | **Categorical** |
|-------------------------|:---------------:|:----------------:|:---------------:|
| **Logistic Regression** | 0.853 - 0.852 ( - )|   0.858 ()|      0.706 () |
| **Decision Tree**       | 0.878 - 0.878  ( - )|              ()|      0.732 () |
| **Random Forest**       |       - **0.937**()          |   ()                | ()                 |

|        *Recall*       |  **Numerical**  | **All Features** | **Categorical** |
|-------------------------|:---------------:|:----------------:|:---------------:|
| **Logistic Regression** | 0.613 - 0.614 ( - )|   0.624 ()|      0.505 () |
| **Decision Tree**       | 0.601 - 0.601  ( - )|              ()|      0.502 () |
| **Random Forest**       |       - **0.616** ()          |   ()                | ()                 |

`X - Y` means that `X` obtained with standardization (i.e., applying the StandardScaler) while `Y` not.
`X (Y)` means that `X` obtained without oversampling, while `Y` obtained with oversampling

---

## Further Informations

The entire project has been written using the **PySpark** framework on the **Community Edition Databricks** platform.