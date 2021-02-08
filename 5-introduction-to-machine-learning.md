# 5강 - Advanced Python review

## 0. Schedule

~~Jan 26th, 27th~~   
Introduction week1 + assignment 1 due to Feb 2nd  
~~Feb 2nd,  3rd~~   
Introduction week2 + assignment 2 due to Feb 9th  
~~Feb 9th~~, 10th Machine Learning  
Machine Learning week1 + assignment 1 due to Feb 16th  
Feb 16th, 17th Text Mining  
Machine Learning week 1 + assignment 2 due to Feb 24th  
Feb 23th, 24th Text Mining  
Text Mining week 2 + there's no assignment

## 1. Commentary on the previous assignment

```python
def proportion_of_education():
    # your code goes here
    # YOUR CODE HERE
    # raise NotImplementedError()
    import pandas as pd
    df = pd.read_csv('assets/NISPUF17.csv', index_col=0)
    # selecting the needed column from the dataframe
    df_mother = df['EDUC1']
    total = df_mother.count()
    less_than_high_school = df_mother[df_mother == 1].count()
    high_school = df_mother[df_mother == 2].count()
    more_than_high_school_not_college = df_mother[df_mother == 3].count()
    college = df_mother[df_mother == 4].count()
    dictionary = {"less than high school": less_than_high_school/total,
                  "high school": high_school/total,
                  "more than high school but not college": more_than_high_school_not_college/total,
                  "college": college/total}
    
    return dictionary

proportion_of_education()
```



```python
def average_influenza_doses():
    # YOUR CODE HERE
    # raise NotImplementedError()
    # selecting the needed column from the dataframe
    import pandas as pd
    df = pd.read_csv('assets/NISPUF17.csv', index_col=0) #load index as it is 
    # grouping the dataframe by whether breastfed or not (categorical) 
    df_cpf_01 = df.groupby('CBF_01')
    # get the representative value (average) of numerical attribute (P_NUMFLU) from the grouped category
    df_cpf_01_mean = df_cpf_01['P_NUMFLU'].mean() 
    
    return (df_cpf_01_mean.iloc[0], df_cpf_01_mean.iloc[1]) #first row and second row (0,1)

average_influenza_doses()
```



```python
def chickenpox_by_sex():
    # YOUR CODE HERE
    # raise NotImplementedError()
    import pandas as pd
    df = pd.read_csv('assets/NISPUF17.csv', index_col=0) #first column as index
    #df_sex = df.groupby('SEX')
    # where P_NUMVRC is the total number of varicella doses
    #masking with overloaded bit operator
    #had cpox -> 1
    #not had cpox -> 2
    #male -> 1
    #female -> 2
    df_male_had_cpox = df['HAD_CPOX'][(df['HAD_CPOX'] == 1) & (df['SEX'] == 1) & (df['P_NUMVRC'] >= 1)].count()
    df_male_not_had_cpox = df['HAD_CPOX'][(df['HAD_CPOX'] == 2) & (df['SEX'] == 1) & (df['P_NUMVRC'] >= 1)].count()
    df_female_had_cpox = df['HAD_CPOX'][(df['HAD_CPOX'] == 1) & (df['SEX'] == 2) & (df['P_NUMVRC'] >= 1)].count()
    df_female_not_had_cpox = df['HAD_CPOX'][(df['HAD_CPOX'] == 2) & (df['SEX'] == 2) & (df['P_NUMVRC'] >= 1)].count()
    return {'male': df_male_had_cpox/df_male_not_had_cpox, 'female': df_female_had_cpox/df_female_not_had_cpox}
    #returning dictornay
chickenpox_by_sex()
```



```python
def corr_chickenpox():
    import scipy.stats as stats
    import numpy as np
    import pandas as pd
    
    # this is just an example dataframe
    # df=pd.DataFrame({"had_chickenpox_column":np.random.randint(1,3,size=(100)),
    #              "num_chickenpox_vaccine_column":np.random.randint(0,6,size=(100))})

    # here is some stub code to actually run the correlation
    # corr, pval=stats.pearsonr(df["had_chickenpox_column"],df["num_chickenpox_vaccine_column"])
    
    # YOUR CODE HERE
    # raise NotImplementedError()
    
    import pandas as pd
    df = pd.read_csv('assets/NISPUF17.csv', index_col=0)
    # removing outliers like 77 from (HAD_CPOX) column, and remvong NA values from (P_NUMVRC) column
    # or just df['P_NUMVRC'].dropna()
    df = df[(df['HAD_CPOX'] < 3)& (df['P_NUMVRC'] >= 0)] 
    
    corr, pval=stats.pearsonr(df['HAD_CPOX'], df['P_NUMVRC'])
    
    # just return the correlation
    return corr
    
corr_chickenpox()
```

## 2. Iterable

 기존에 `finditer()`와 같은 함수를 사용했었고. 또한, 앞으로 `zip()`  과 같은 함수가 나올 뿐만 아니라 기본적으로 `for` 문에 있어서도. `for x in range(y):` 와 같은 구문을 우리는 종종 사용하는데요. 

이때, 해당 함수의 parameter로 `*iterables`라고 표기되어있는 것을 종종 볼 수있습니다. 이 `*iterables`는 무엇을 의미하는 것일까요? 

`iterable`이란, member를 하나씩 차례로 반환하는 `object`를 말합니다. `iterable`의 예로는 sequence type인 `list`, `str`, `tuple`이 있습니다. 

```python
for x in range(5):
    print x
```

Python에서 당연하게 사용해왔던 위와 같은 `for` 문도 사실 `range()`에서 `iterable`한 `list`를 만들고 `for loop`에서 해당 `list`를 `iterator`로 자동 변환해주기 때문인데요. 이때 등장하는 `iterator`는 또 다른 개념입니다. 

위의 코드를 `while`문과 비교해보면 아래와 같은 차이가 있습니다. 

```python
def print_each(iterable):
    iterator = iter(iterable)
    while True:
        try:
            item = next(iterator)
        except StopIteration:
            break  # Iterator exhausted: stop the loop
        else:
            print(item)
```

`iterator`는 `next()`와 함께 순환하는 `object`인데요. 이 과정을 `for loop` 에서는 

```python
def print_each(iterable):
    for item in iterable:
        print(item)
```

처럼 자동으로 이 과정을 수행합니다. `iter()`라는 `iterable`을 받아 `iterator`로 리턴하고 `next()` 메소를 통해 `StopIteration`이라는 `exception`이 `raise`될 때까지 도는 것이 바로 `for` 문입니다. 이를 iterator protocol이라고 합니다. 

위 와 같이, `next()` 메소드를 기반으로 데이터를 순차적으로 호출 가능한 `object`를 `iterator`라고 하며, 마지막 `StopIteration exception`에 도달했을 때에 멈춥니다.  고로, `iterable` 이라고 해서 `iterator` 인 것은 아닙니다. `iterable`을 `iterator`로 변환하기 위해서는 `iter()` 라는 built-in function을 사용해야 하는 것이죠. 

## 3. Axis

데이터프레임을 다룰 때에 

## 4. Random \(Training set, Test set\)

사회과학 분야에서는, 다양한 변수들의 내재성과 무작위 선택의 유효성을 증명하기 위해 많은 과정을 필요로 합니다. 

## 5. Classifier, Regression, Prediction

기본적으로 머신러닝에서는 통계학과 컴퓨터공학, 마지막으로 프로젝트에 사용할 사회과학적 개념과 데이터를 많이 활용합니다. 하지만, 기본적으로 공학적인 시각을 가진 문제 해결 방법으라고 할 수 있는데요. 사회과학에서의 문제해결방법과 어떻게 다른지. 

머신러닝의 대표적인 활용 방향인 Classifier와 Regression, Prediction을 예시로 설명하겠습니다. 

## 

