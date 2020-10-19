# Decision-Tree-Analysis

_Datasets and questions are credited to BT2101 (Business Analytics for Decision Making, Prof. Setiono, Ruby)_

This notebook focuses on the decision tree mechanism and its variations through real-world data processing and comparisons.

## Discussion Question 1:  

(contents of the problem are attached in the pdf file)

### (a) How heterogeneous are the samples in the dataset? Compute using the Gini index.

Gini index is defined to be $Gini(q) = 1 - \sum_{h}P_h^2$  

From the dataset, $$P_{above} = \frac{6}{10} = \frac{3}{5}$$ $$P_{below} = 1 - P_{above} = \frac{2}{5}$$  

So the Gini index is calculated as $$1 - (P_{above}^2 + P_{below}^2) = 1 - (\frac{3^2}{5^2} + \frac{2^2}{5^2}) = \frac{12}{25} = 0.48$$ 

### (b) Using the values of the variable Year Born, what is the maximum reduction in the diversity that can be achieved? Measure the reduction in terms of Gini index.  

After examining the data samples, we consider spliting where _Year-Born  greater than or equal to 75_ or _Year-Born less than 75_:
(other splits are also possible but considered less rational)  

Impurity after split:  
  
$$I_G(q_1,q_2) = \frac{4}{10} \cdot Gini(q_1) + \frac{6}{10} \cdot Gini(q_2) = \frac{2}{5} \times 0 + \frac{3}{5} \times \frac{4}{9} = 0.267$$  
  
So the achieved reduction measured by Gini index would be $Gini(q) - I_G(q_1,q_2) = 0.213$  

### (c) If we consider splitting the data according to the values of the variable Job Type, how many possible splits are there?
  
As _Job-Type_ is a categorical variable, with $N=5$ variations in total, the number of possible splits are $2^{N-1} - 1 = 2^4-1=15$.  

### (d) If we consider splitting the data according to the values of the variable Education Level, how many possible splits are there? List all these possible splits.
  
As _Education_ is a categorical variable, taking values in {$1,2,3,4$}, the possible splits are listed as follows:  
  
$$1 \quad versus \quad 2,3,4$$
$$2 \quad versus \quad 1,3,4$$
$$3 \quad versus \quad 1,2,4$$
$$4 \quad versus \quad 1,2,3$$
$$1,2 \quad versus \quad 3,4$$
$$1,3 \quad versus \quad 2,4$$
$$1,4 \quad versus \quad 2,3$$
  
In total, there are 7 possible splits.  

### (e) Suppose the information from Employee No. 11 is now available, but with its value for the variable Year Born missing. Suggest 2 possible ways to handle the missing value so that this new data sample can be included for building the classification tree.
  
Approach 1: Use the **Identification** method  

As the _Year-born_ attribute is continuous, to encode and identify the missing data, we assign a value of -1 to the new data sample.  
  
Approach 2: Use the **Substitution** method  

We can substitute the missing value with the mean of the other observations, which is calculated as:  <br>

$$Mean=\frac{\sum_{i=1}^{10}Yearborn[i]}{10} \approx 77$$  

Thus the value $77$ is assigned to the new data sample as the year born.


## Discussion Question 2:

(contents of the problem are attached in the pdf file)  

### (a) What is the entropy of the training samples with respect to the classification?

Entropy index is defined to be $Entropy(q) = - \sum_{h}P_h \cdot log_2{P_h}$  

From the dataset, $$P_{yes} = \frac{6}{12} = \frac{1}{2}$$
$$P_{no} = 1 - P_{yes} = \frac{1}{2}$$
The Entropy index is calculated as $$-\sum_{h}P_h \cdot log_2{P_h} = -(\frac{1}{2}log_2{\frac{1}{2}} + \frac{1}{2}log_2{\frac{1}{2}}) = -(log_2{\frac{1}{2}}) = 1$$

So the Entropy index of the training samples is $1$. 


### (b) Suppose INCOME is to be the first attribute used for splitting the root node of the decision tree. What is the best cut-off point for splitting that will maximize the information gain?
  
We shortlisted two possible cases of splits which are not obviously comparable in performance to determine which yields more information gain:  

- Case 1: Split at income = 81 (thousand dollars) 

    Node $q_1$ is the group of samples with income less than or equal to 81 (thousand dollars). Two samples (out of 8) are in Good-Credit. Its entropy index is computed as: 

    $$Entropy(q_1) = - \sum_{h}P_h \cdot log_2{P_h} = -(\frac{2}{8}log_2{\frac{2}{8}} + \frac{6}{8}log_2{\frac{6}{8}}) = 0.81$$  

    Node $q_2$ is the group of samples with income greater than 81 (thousand dollars).  We note that all the samples in $q_2$ are not in Good-Credit. Therefore its entropy is 0.  
      
    So the Impurity after split would be:
    $$I_E(q_1,q_2) = \frac{8}{12} \cdot Entropy(q_1) + \frac{4}{12} \cdot Entropy(q_2) = 0 + \frac{2}{3} \times 0.81 = 0.54$$  
    
    $\therefore$ Information Gain = 1 - 0.54 = 0.46  
    
- Case 2: Split at income = 72 (thousand dollars) 
    
    Node $q_1$ is defined to be the group of samples with income less than 72 (thousand dollars). All sample points are **not in Good-Credit**, thus the entropy value is 0.  
    
    Node $q_2$  has its sample points with income greater than or equal to 72 (thousand dollars). Only one sample (out of 7) is not in Good-credit. Thus its entropy index is calculated by: 
    $$Entropy(q_1) = - \sum_{h}P_h \cdot log_2{P_h} = -(\frac{1}{7} \times log_2{\frac{1}{7}} + \frac{6}{7} \times log_2{\frac{6}{7}}) = 0.59$$  
    So the Impurity after split would be:  <br>
    $$I_E(q_1,q_2) = \frac{5}{12} \cdot Entropy(q_1) + \frac{7}{12} \cdot Entropy(q_2) = 0 + \frac{7}{12} \times 0.59 = 0.34$$
   
    $\therefore$ Information Gain = 1 - 0.34 = 0.66  
  
  
We can see that 0.46 (case 1) < 0.66 (case 2)
  
  
**To conclude, since splitting at income = 72 gives more information gain, we decided to fix the final split point at income greater than or equal to 77 versus less than 77, which is the middle point between 72 and 81, to realize a more flexible and generalized model.**  

### (c) Build a complete decision tree that classifies all the training data samples correctly.<br>

We constructed a decision tree with the help of C5.0 package in R. The decision rule was:  

  1. if the _Income_ is greater than 81 (thousand dollars), predict _Yes_;
  2. if the _Income_ is less than or equal to 81 (thousand dollars):  
    
      - if the _Age_ is less than or equal to 60, predict _No_;
      - if the _Age_ is greater than 60, predict _Yes_;   


The full decision tree is depicted as follows: (attached in pdf file)

Obviously seen from the plot that the model classifies all the training samples correctly.


## Discussion Question 3

(contents of the problem are attached in the pdf file)  

### (a) What is the decision that minimizes the expected opportunity loss (regret)?

Regret (opportunity cost) matrix:(in ten thousand dollars)

(The table is attached in pdf file),align='c'). 

If _Major Renovation_ is exercised, expected opportunity loss = (1/3) * 0 + (2/3) * 80 = 53.333;  
Else if _Minor Renovation_ is exercised, expected opportunity loss = (1/3) * 150 + (2/3) * 20 = 63.333;  
Else _Nothing_ is done, then the expected opportunity loss = (1/3) * 250 + (2/3) * 0 = 83.333;  

$\therefore$ Choose _Major Renovation_ to minimise expected opportunity loss.  

### (b) Determine the Expected Value with Original Information.

The expected values grid with original information is depicted as:

(The table is attached in pdf file). 

So the expected values (in ten thousand of dollars) for each course of action will be:

``` {r Expected-Original, echo = FALSE}
df1 <- data.frame("Expected-Profits"=c(30,20,0),row.names = c("Major renovation","Minor renovation","Do nothing"))
kable(df1,align='c')
```

### (c) Determine the Expected Value of Perfect Information.

(The table is attached in pdf file). 

(All the following metrics are in ten thousand of dollars)  
Expected Value With Perfect Information (EVWPI) = (1/3) * 250 + (2/3) * 0 = 83.333  

Expected Profit With Original Information (EVWOI) to maximize EV = 30  

Thus the Expected Value of Perfect Information (EVPI) = EVWPI - EVWOI = 83.333 - 30 = 53.333  


### (d) Determine the Expected Value of Sample Information and the best course of action.

We have the **prior probability**: $$P(FM) = \frac{1}{3} \quad P(UM) = \frac{2}{3}$$
$$P(PS|FM) = 0.80 \quad P(NS|FM) = 0.20$$ $$P(PS|UM) = 0.40 \quad P(NS|UM) = 0.60$$  
**Joint probability**:  
$$P(PS \cap FM) = P(FM) \cdot P(PS|FM) = \frac{4}{15} \approx 0.267$$
$$P(NS \cap FM) = P(FM) \cdot P(NS|FM) = \frac{1}{15} \approx 0.067$$
$$P(PS \cap UM) = P(UM) \cdot P(PS|UM) = \frac{4}{15} \approx 0.267$$
$$P(NS \cap UM) = P(UM) \cdot P(NS|UM) = 0.400$$
  
**Marginal probabilities**:  
$$P(PS) = P(PS \cap FM) + P(PS \cap UM) = \frac{8}{15} \approx 0.534$$
$$P(NS) = P(NS \cap FM) + P(NS \cap UM) = \frac{7}{15} \approx 0.467$$

  
**Posterior (revised) probabilities**:  
$$P(FM|PS)=P(FM \cap PS)/P(PS) = \frac{\frac{4}{15}}{\frac{8}{15}} = 0.5$$
$$P(UM|PS)=P(UM \cap PS)/P(PS) = \frac{\frac{4}{15}}{\frac{8}{15}} = 0.5$$
$$P(FM|NS)=P(FM \cap NS)/P(NS) = \frac{\frac{1}{15}}{\frac{7}{15}} = \frac{1}{7} \approx 0.143$$
$$P(UM|NS)=P(UM \cap NS)/P(NS) = \frac{0.4}{\frac{7}{15}} = \frac{6}{7} \approx 0.857$$

**If we decide to conduct the market research:**     
(cost is 5 in ten thousand dollars)  
  
- If the research outcome is _positive_:  
  
  - Major renovation:  
    E1 = (0.5)(250) + (0.5)(-80) = 85  
      
  - Minor renovation:  
    E2 = (0.5)(100) + (0.5)(-20) = 40  
      
  - Do nothing:  
    E3 = (0.5)(0) + (0.5)(0) = 0  
      
  It is better to adopt **major renovation**.  
  
- If the research outcome is _negative_:  
  
  - Major renovation:  
    E4 = (1/7)(250) + (6/7)(-80) = -32.86  
      
  - Minor renovation:  
    E5 = (1/7)(100) + (6/7)(-20) = -2.857  
      
  - Do nothing:  
    E6 = (1/7)(0) + (6/7)(0) = 0  
      
  It is better to **do nothing**.  
  
  
Therefore, the expected return would be:  
$$E_{research} = P(PS) \cdot E1 + P(NS) \cdot E6 = \frac{8}{15} \times 85 + \frac{7}{15} \times 0 = 45.33$$
which is **40.33** if deducted by the cost.

Compared to: (no research conducted thus no information acquired)
$$E_{major} = 30 \quad E_{minor} = 20 \quad E_{nothing} = 0$$
  
Apparently the best course of action is to carry out the market research.


