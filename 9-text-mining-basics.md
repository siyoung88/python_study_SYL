# 9강 - Text Mining Basics

## 0. Feedback and Schedule

~~Jan 26th, 27th~~   
Introduction week1 + assignment 1 due to Feb 2nd  
~~Feb 2nd,  3rd~~   
Introduction week2 + assignment 2 due to Feb 9th  
~~Feb 9th, 10th Machine Learning~~  
Machine Learning week1 + assignment 1 due to Feb 16th  
~~Feb 16th, 17th Machine Learning~~  
Machine Learning week 1 + assignment 2 due to Feb 24th  
~~Feb 23th~~, 24th Text Mining  
Text Mining week 2 + there's no assignment

## 1. Assignment Review

```python
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split


np.random.seed(0)
n = 15
x = np.linspace(0,10,n) + np.random.randn(n)/5
y = np.sin(x)+x/6 + np.random.randn(n)/10


X_train, X_test, y_train, y_test = train_test_split(x, y, random_state=0)

# You can use this function to help you visualize the dataset by
# plotting a scatterplot of the data points
# in the training and test sets.
def part1_scatter():
    import matplotlib.pyplot as plt
    %matplotlib notebook
    plt.figure()
    plt.scatter(X_train, y_train, label='training data')
    plt.scatter(X_test, y_test, label='test data')
    plt.legend(loc=4);
    
    
# NOTE: Uncomment the function below to visualize the data, but be sure 
# to **re-comment it before submitting this assignment to the autograder**.   
#part1_scatter()
```

scatter\(\) 부분은 그냥 plot에 관련된 것으로 신경 안쓰셔도 됩니다. x는 등간격으로 지정해 준다음 난수를 더했고 y의 경우에는 sin함수에 맞게 지정해 줬네요.

```python
def answer_one():
    from sklearn.linear_model import LinearRegression
    from sklearn.preprocessing import PolynomialFeatures

    result = np.zeros((4, 100))
    test = np.linspace(0, 10, 100)
    for i, degree in enumerate([1, 3, 6, 9]):
        poly = PolynomialFeatures(degree=degree)
        X_poly = poly.fit_transform(X_train.reshape(len(X_train), 1))
        linreg = LinearRegression().fit(X_poly, y_train)
        y = linreg.predict(poly.fit_transform(test.reshape(len(test), 1)))
        result[i, :] = y

    return result
```

sin 함수에 모형에 맞게 설정한 자료가 linear하게 regression 될리가 없습니다. 저희는 preprocessing 을 지정해준 1차 3차 6차 9차 방정식으로 발꿔줄 겁니다. prediction 값을 뽑을때도 processing 해줘야합니다.

```python
def answer_two():
    from sklearn.linear_model import LinearRegression
    from sklearn.preprocessing import PolynomialFeatures
    from sklearn.metrics import r2_score

    r2_test = np.zeros(10)
    r2_train = np.zeros(10)
    for degree in range(10):
        #train polynomial linear regression
        poly = PolynomialFeatures(degree=degree)
        X_train_poly = poly.fit_transform(X_train.reshape(len(X_train), 1))
        linreg = LinearRegression().fit(X_train_poly, y_train)

        #evaluate the polynomial linear regression
        r2_train[degree] = linreg.score(X_train_poly, y_train)

        X_test_poly = poly.fit_transform(X_test.reshape(len(X_test), 1))
        r2_test[degree] = linreg.score(X_test_poly, y_test)

    return (r2_train, r2_test)
answer_two()
```

r-squared test에 대한 내용은 후술하겠습니다.

```python
def plot_answer_three():
    import matplotlib.pyplot as plt
    r2_train, r2_test = answer_two()
    degrees = np.arange(0, 10)
    plt.figure()
    plt.plot(degrees, r2_train, degrees, r2_test)


plot_answer_three()
```

그냥 plotting 문제입니다.

```python
def answer_four():
    from sklearn.preprocessing import PolynomialFeatures
    from sklearn.linear_model import Lasso, LinearRegression
    from sklearn.metrics.regression import r2_score

    poly = PolynomialFeatures(12)
    X_poly = poly.fit_transform(X_train.reshape(len(X_train), 1))
    X_test_poly = poly.fit_transform(X_test.reshape(len(X_test), 1))

    linreg = LinearRegression().fit(X_poly, y_train)
    LinearRegression_R2_test_score = linreg.score(X_test_poly, y_test)

    linlasso = Lasso(alpha=0.001, max_iter=10000).fit(X_poly, y_train)
    Lasso_R2_test_score = linlasso.score(X_test_poly, y_test)

    return (LinearRegression_R2_test_score, Lasso_R2_test_score)
answer_four()
```

ridge와 lasso regularization을 테스트해보는 문항입니다. lasso의 경우에는 regularization이 들어갈 때에 iteration과 alpha도 지정할 수 있도록 되어있습니다.

```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split


mush_df = pd.read_csv('readonly/mushrooms.csv')
mush_df2 = pd.get_dummies(mush_df)

X_mush = mush_df2.iloc[:,2:]
y_mush = mush_df2.iloc[:,1]

# use the variables X_train2, y_train2 for Question 5
X_train2, X_test2, y_train2, y_test2 = train_test_split(X_mush, y_mush, random_state=0)

# For performance reasons in Questions 6 and 7, we will create a smaller version of the
# entire mushroom dataset for use in those questions.  For simplicity we'll just re-use
# the 25% test split created above as the representative subset.
#
# Use the variables X_subset, y_subset for Questions 6 and 7.
X_subset = X_test2
y_subset = y_test2
```

이번 테스트 셋 같은 경우에는 첫번째 애트리뷰트가 클래스이고. 문제에서 이들 중 25%만 쓴다고 합니다.

```python
def answer_five():
    from sklearn.tree import DecisionTreeClassifier

    clf = DecisionTreeClassifier(random_state=0).fit(X_train2, y_train2)
    features = []

    ind = clf.feature_importances_.argsort()[::-1][:5]

    return X_train2.columns[ind].tolist()
answer_five()
```

DecisionTreeClassifier\(\)를 통해서 DecisionTree를 사용해보면 됩니다. feature\_importances.argsor\(\)는 세트입니다.

```python
def answer_six():
    from sklearn.svm import SVC
    from sklearn.model_selection import validation_curve

    svc = SVC(kernel='rbf', C=1, random_state=0)
    train_scores, test_scores = validation_curve(svc,
                                                 X_subset,
                                                 y_subset,
                                                 param_name='gamma',
                                                 param_range=np.logspace(
                                                     -4, 1, 6),
                                                 scoring='accuracy',
                                                 cv=3)

    train_mscores = train_scores.mean(axis=1)
    test_mscores = test_scores.mean(axis=1)

    return (train_mscores, test_mscores)
answer_six()
```

SVC를 사용해볼 건데요. rbf는 커널의 이름이고 C는 어제 했던 margin의 정도입니다. 파라미터 같은 경우에는 0.0001 부터 10까지 쓰고 있는데요. 

```python
train_scores, test_scores = answer_six()
def plot_answer_seven():
    import matplotlib.pyplot as plt
    gamma = np.logspace(-4, 1, 6)
    plt.figure()
    plt.plot(gamma, train_scores, 'b--.', label='train_scores')
    plt.plot(gamma, test_scores, 'g-*', label='test_scores')
    plt.legend()


plot_answer_seven()
def answer_seven():

    param_range = np.logspace(-4, 1, 6)
    Underfitting, Overfitting, Good_Generalization = param_range[
        0], param_range[5], param_range[3]
    return (Underfitting, Overfitting, Good_Generalization)
answer_seven()
```

이게 plot된 모양을 보고 overfit인지 underfit인지 알아야합니다. b-- g-\*는 모르셔도 됩니다만 plot의 모형입니다. 

## 2. R-squared test

![](.gitbook/assets/image%20%2822%29.png)

![](.gitbook/assets/image%20%2816%29.png)

![](.gitbook/assets/image%20%2818%29.png)

![](.gitbook/assets/image%20%2819%29.png)

![](.gitbook/assets/image%20%2820%29.png)

![](.gitbook/assets/image%20%2821%29.png)

![](.gitbook/assets/image%20%2817%29.png)

source : [https://www.coursera.org/professional-certificates/ibm-data-science](https://www.coursera.org/professional-certificates/ibm-data-science)

