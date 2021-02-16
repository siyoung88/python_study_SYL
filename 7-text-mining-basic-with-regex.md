# 7강 - Experimental Design of Machine Learning Project

## 0. assignment review

```python
import numpy as np
import pandas as pd
from sklearn.datasets import load_breast_cancer

cancer = load_breast_cancer()

#print(cancer.DESCR) # Print the data set description
```

```python
def answer_one():
    columns = ['mean radius', 'mean texture', 'mean perimeter', 'mean area',
    'mean smoothness', 'mean compactness', 'mean concavity',
    'mean concave points', 'mean symmetry', 'mean fractal dimension',
    'radius error', 'texture error', 'perimeter error', 'area error',
    'smoothness error', 'compactness error', 'concavity error',
    'concave points error', 'symmetry error', 'fractal dimension error',
    'worst radius', 'worst texture', 'worst perimeter', 'worst area',
    'worst smoothness', 'worst compactness', 'worst concavity',
    'worst concave points', 'worst symmetry', 'worst fractal dimension',
    'target']
    index = range(0, 569, 1)
#   print(cancer['data'].shape)
    df = pd.DataFrame(data=cancer['data'], index=index, columns = columns[:30])
#   print(cancer['target'])
    df['target'] = cancer['target']
    return df

answer_one()
```

```python
def answer_two():
    cancerdf = answer_one()
    malignant_count = len(cancerdf[cancerdf['target'] == 0])
    benign_count = len(cancerdf[cancerdf['target'] == 1])
    
    index = ['malignant', 'benign']
    target = pd.Series(data=[malignant_count, benign_count], index=index)
    return target
answer_two()
```

```python
def answer_three():
    cancerdf = answer_one()
    X = cancerdf.iloc[:,:30]
    y = cancerdf.iloc[:,30:32].values.ravel()
#     y = cancerdf['target']
#    y = cancerdf.target
#     print(y)
    return X, y

answer_three()
```

```python
from sklearn.model_selection import train_test_split

def answer_four():
    X, y = answer_three()
    X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)
    return X_train, X_test, y_train, y_test
answer_four()
```

```python
from sklearn.neighbors import KNeighborsClassifier

def answer_five():
    X_train, X_test, y_train, y_test = answer_four()
    
    # Your code here
    knn = KNeighborsClassifier(n_neighbors = 1)
    knn.fit(X_train, y_train)
    return knn
answer_five()
```

```python
def answer_six():
    cancerdf = answer_one()
    knn = answer_five()
    means = (cancerdf.mean()[:-1].values.reshape(1, -1))
#   print(means.shape)
    # Your code here
    prediction = knn.predict(means)
    answer = np.array(prediction)
    return answer
answer_six()
```



