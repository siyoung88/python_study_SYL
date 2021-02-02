# 4강 - data manipulation with pandas and numpy

## 0. Course review

1. Introduction to Pandas
2. The Series Data Structure
3. Querying a Series
4. DataFrame Data Structure

5. DataFrame Indexing and Loading

csv 파일과 같이 comma로 구분되어있는 파일은 일단 불러오면 첫 줄에 column의 이름들이 표기되고 아래 자료들이 보입니다.  
pandas module을 통해서 가독성이 좋게 불러올 수 있습니다.   
head\(\) 는 상위 5개만 표기합니다. 

```python
import pandas as pd

# Pandas mades it easy to turn a CSV into a dataframe, we just call read_csv()
df = pd.read_csv('datasets/Admission_Predict.csv')

# And let's look at the first few rows
df.head()
```

| Index | Serial No. | GRE Score | TOEFL Score | University Rating | SOP | LOR | CGPA | Research | Chance to Admit |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 0 | 1 | 337 | 118 | 4 | 4.5 | 4.5 | 9.65 | 1 | 0.92 |
| 1 | 2 | 324 | 107 | 4 | 4.0 | 4.5 | 8.87 | 1 | 0.76 |
| 2 | 3 | 316 | 104 | 3 | 3.0 | 3.5 | 8.00 | 1 | 0.72 |
| 3 | 4 | 322 | 110 | 3 | 3.5 | 2.5 | 8.67 | 1 | 0.80 |
| 4 | 5 | 314 | 103 | 2 | 2.0 | 3.0 | 8.21 | 0 | 0.65 |

index\_col=0 옵션을 통해 인덱스를 지울 수도 있습니다.

```python
df = pd.read_csv('datasets/Admission_Predict.csv', index_col=0)
df.head()
```

| Serial No. | GRE Score | TOEFL Score | University Rating | SOP | LOR | CGPA | Research | Chance to Admit |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 337 | 118 | 4 | 4.5 | 4.5 | 9.65 | 1 | 0.92 |
| 2 | 324 | 107 | 4 | 4.0 | 4.5 | 8.87 | 1 | 0.76 |
| 3 | 316 | 104 | 3 | 3.0 | 3.5 | 8.00 | 1 | 0.72 |
| 4 | 322 | 110 | 3 | 3.5 | 2.5 | 8.67 | 1 | 0.80 |
| 5 | 314 | 103 | 2 | 2.0 | 3.0 | 8.21 | 0 | 0.65 |

Rename을 통해 칼럼들의 이름을 바꾸는 것도 가능합니다. 

```python
new_df=df.rename(columns={'GRE Score':'GRE Score', 'TOEFL Score':'TOEFL Score',
                   'University Rating':'University Rating', 
                   'SOP': 'Statement of Purpose','LOR': 'Letter of Recommendation',
                   'CGPA':'CGPA', 'Research':'Research',
                   'Chance of Admit':'Chance of Admit'})
new_df.head()
```

|  Serial No. | GRE Score | TOEFL Score | University Rating | Statement of Purpose | LOR | CGPA | Research | Chance of Admit |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 337 | 118 | 4 | 4.5 | 4.5 | 9.65 | 1 | 0.92 |
| 2 | 324 | 107 | 4 | 4.0 | 4.5 | 8.87 | 1 | 0.76 |
| 3 | 316 | 104 | 3 | 3.0 | 3.5 | 8.00 | 1 | 0.72 |
| 4 | 322 | 110 | 3 | 3.5 | 2.5 | 8.67 | 1 | 0.80 |
| 5 | 314 | 103 | 2 | 2.0 | 3.0 | 8.21 | 0 | 0.65 |

하지만, 안바뀐게 있지요? 이런 경우는 기존 csv파일의 칼럼 이름이 '깔끔하지' 않기 때문입니다. 주변에 인덴트 같은 것이 붙어 있을 수 있지요. 이것들을 `str.strip` 을 통해 벗겨줄 수 있습니다.

```python
new_df=new_df.rename(mapper=str.strip, axis='columns')
# Let's take a look at results
new_df.head()
```

|  Serial  No. | GRE Score | TOEFL Score | University Rating | Statement of Purpose | Letter of Recommendation | CGPA | Research | Chance of Admit |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 337 | 118 | 4 | 4.5 | 4.5 | 9.65 | 1 | 0.92 |
| 2 | 324 | 107 | 4 | 4.0 | 4.5 | 8.87 | 1 | 0.76 |
| 3 | 316 | 104 | 3 | 3.0 | 3.5 | 8.00 | 1 | 0.72 |
| 4 | 322 | 110 | 3 | 3.5 | 2.5 | 8.67 | 1 | 0.80 |
| 5 | 314 | 103 | 2 | 2.0 | 3.0 | 8.21 | 0 | 0.65 |

이렇게 하면 원하는 이름으로 '깔끔하게' 바꿀 수 있습니다.   
또한, `df.columns`을 통해 column들의 애트리뷰트 리스트를 불러올 수 있는데요. \(리스트로 형변환은 해줘야 합니다.\)

```python
# As an example, lets change all of the column names to lower case. First we need to get our list
cols = list(df.columns)
# Then a little list comprehenshion
cols = [x.lower().strip() for x in cols]
# Then we just overwrite what is already in the .columns attribute
df.columns=cols
# And take a look at our results
df.head()
```

|  Serial No. | gre score | toefl score | university rating | sop | lor | cgpa | research | chance of admit |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 337 | 118 | 4 | 4.5 | 4.5 | 9.65 | 1 | 0.92 |
| 2 | 324 | 107 | 4 | 4.0 | 4.5 | 8.87 | 1 | 0.76 |
| 3 | 316 | 104 | 3 | 3.0 | 3.5 | 8.00 | 1 | 0.72 |
| 4 | 322 | 110 | 3 | 3.5 | 2.5 | 8.67 | 1 | 0.80 |
| 5 | 314 | 103 | 2 | 2.0 | 3.0 | 8.21 | 0 | 0.65 |

인덱스 칼럼을 제외한 모든 칼럼의 애트리뷰트가 소문자로 변경되었습니다.

6. Querying a DataFrame

Boolean masking이 나옵니다. masking이란? 

![Picture 1. &#xC774;&#xBBF8;&#xC9C0; &#xBE44;&#xD2B8; &#xB9C8;&#xC2A4;&#xD0B9; \(&#xCD9C;&#xCC98;: wikipedia\)](.gitbook/assets/sprite_rendering_by_binary_image_mask.png)

 기존의 비트연산을 이용해서 원하는 데이터를 masking할 수 있다. \(비트 마스킹\)  
1010 1001 & 0000 0000 ? 지우기  
0000 0000 \| 1001 0010 ? 그리기

```python
    10010101   10100101
 OR 11110000   11110000
  = 11110101   11110101
  
    10010101   10100101
AND 00001111   00001111
  = 00000101   00000101
```

브로드캐스팅이 나옵니다. broadcasting 이란?  
어떠한 산술 연산\(arithmetic operation\)이 행렬 의 차원\(shape\)에 따라 처리되는 방법

```python
admit_mask=df['chance of admit'] > 0.7
admit_mask
```

```python
Serial No.
1       True
2       True
3       True
4       True
5      False
       ...  
396     True
397     True
398     True
399    False
400     True
Name: chance of admit, Length: 400, dtype: bool
```

그러면 이렇게 마스킹 한 데이터를 가지고 컨디션에 맞게 `df.where`를 통해서 뽑아볼 수 있습니다. 

```python
df.where(admit_mask).head()
```

|  Serial No. | gre score | toefl score | university rating | sop | lor | cgpa | research | chance of admit |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 337.0 | 118.0 | 4.0 | 4.5 | 4.5 | 9.65 | 1.0 | 0.92 |
| 2 | 324.0 | 107.0 | 4.0 | 4.0 | 4.5 | 8.87 | 1.0 | 0.76 |
| 3 | 316.0 | 104.0 | 3.0 | 3.0 | 3.5 | 8.00 | 1.0 | 0.72 |
| 4 | 322.0 | 110.0 | 3.0 | 3.5 | 2.5 | 8.67 | 1.0 | 0.80 |
| 5 | NaN | NaN | NaN | NaN | NaN | NaN | NaN | NaN |

하지만 Not a number가 너무 많죠. 이런 row는 `dropna()` 할 수 있습니다.

```python
df.where(admit_mask).dropna().head()
```

|  Serial No. | gre score | toefl score | university rating | sop | lor | cgpa | research | chance of admit |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 337.0 | 118.0 | 4.0 | 4.5 | 4.5 | 9.65 | 1.0 | 0.92 |
| 2 | 324.0 | 107.0 | 4.0 | 4.0 | 4.5 | 8.87 | 1.0 | 0.76 |
| 3 | 316.0 | 104.0 | 3.0 | 3.0 | 3.5 | 8.00 | 1.0 | 0.72 |
| 4 | 322.0 | 110.0 | 3.0 | 3.5 | 2.5 | 8.67 | 1.0 | 0.80 |
| 6 | 330.0 | 115.0 | 5.0 | 4.5 | 3.0 | 9.34 | 1.0 | 0.90 |

이 두가지를 한 번에 할 수 도 있는데요. 이게 4강의 핵심 내용입니다.  
지난번에 오버로딩에 관한 내용을 배웠죠? 오버로딩이란, 같은 이름의 메소드임에도 파라미터의 형식에따라 다르게 작동하는 메소드들을 가르키는데요. 이 기능을 이용해서 다음과 같은 문법을 통해 pandas는 아래와 같은 기능을 구현해 놓았습니다. 

```python
df[df['chance of admit'] > 0.7].head()
```

|  Serial No. | gre score | toefl score | university rating | sop | lor | cgpa | research | chance of admit |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 337 | 118 | 4 | 4.5 | 4.5 | 9.65 | 1 | 0.92 |
| 2 | 324 | 107 | 4 | 4.0 | 4.5 | 8.87 | 1 | 0.76 |
| 3 | 316 | 104 | 3 | 3.0 | 3.5 | 8.00 | 1 | 0.72 |
| 4 | 322 | 110 | 3 | 3.5 | 2.5 | 8.67 | 1 | 0.80 |
| 6 | 330 | 115 | 5 | 4.5 | 3.0 | 9.34 | 1 | 0.90 |

```python
# Or you can send it a list of columns as strings
df[["gre score","toefl score"]].head()
```

애트리뷰트를 넣으면 

|  Serial No. | gre score | toefl score |
| :--- | :--- | :--- |
| 1 | 337 | 118 |
| 2 | 324 | 107 |
| 3 | 316 | 104 |
| 4 | 322 | 110 |
| 5 | 314 | 103 |

해당 column만 뽑아낼 수 도 있고.   
boolean mask를 넣으면

```python
df[df["gre score"]>320].head()
```

| Serial No. | gre score | toefl score | university rating | sop | lor | cgpa | research | chance of admit |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 337 | 118 | 4 | 4.5 | 4.5 | 9.65 | 1 | 0.92 |
| 2 | 324 | 107 | 4 | 4.0 | 4.5 | 8.87 | 1 | 0.76 |
| 4 | 322 | 110 | 3 | 3.5 | 2.5 | 8.67 | 1 | 0.80 |
| 6 | 330 | 115 | 5 | 4.5 | 3.0 | 9.34 | 1 | 0.90 |
| 7 | 321 | 109 | 3 | 3.0 | 4.0 | 8.20 | 1 | 0.75 |

해당하는 row만 뽑아 낼 수도 있습니다. 난 그런데 이런 기능들이 달갑진 않더라..

7. Indexing Dataframes

8. Missing Values

9. Example: Manipulating DataFrame 

## 1. Assignment Guidance

## 3. Group by



## 4. Pearson Correlation Coefficient

[통계학](https://ko.wikipedia.org/wiki/%ED%86%B5%EA%B3%84%ED%95%99)에서 , 피어슨 상관 계수\(Pearson Correlation Coefficient ,PCC\)란 두 변수 X 와 Y 간의 선형 [상관 관계](https://ko.wikipedia.org/wiki/%EC%83%81%EA%B4%80_%EB%B6%84%EC%84%9D)를 계량화한 수치다 . 피어슨 상관 계수는 [코시-슈바르츠 부등식](https://ko.wikipedia.org/wiki/%EC%BD%94%EC%8B%9C-%EC%8A%88%EB%B0%94%EB%A5%B4%EC%B8%A0_%EB%B6%80%EB%93%B1%EC%8B%9D)에 의해 +1과 -1 사이의 값을 가지며, +1은 완벽한 양의 선형 상관 관계, 0은 선형 상관 관계 없음, -1은 완벽한 음의 선형 상관 관계를 의미한다. 일반적으로 상관관계는 피어슨 상관관계를 의미한다.



피어슨 상관 계수는 [등간척도](https://ko.wikipedia.org/w/index.php?title=%EB%93%B1%EA%B0%84%EC%B2%99%EB%8F%84&action=edit&redlink=1)나 [비례척도](https://ko.wikipedia.org/w/index.php?title=%EB%B9%84%EB%A1%80%EC%B2%99%EB%8F%84&action=edit&redlink=1)의 데이타에서 두 변수의 [공분산](https://ko.wikipedia.org/wiki/%EA%B3%B5%EB%B6%84%EC%82%B0) 을 [표준 편차](https://ko.wikipedia.org/wiki/%ED%91%9C%EC%A4%80_%ED%8E%B8%EC%B0%A8) 의 곱으로 나눈 [값](https://ko.wikipedia.org/wiki/%ED%91%9C%EC%A4%80_%ED%8E%B8%EC%B0%A8)이다.![{\displaystyle r\_{XY}={{{\sum \_{i}^{n}\left\(X\_{i}-{\overline {X}}\right\)\left\(Y\_{i}-{\overline {Y}}\right\)} \over {n-1}} \over {{\sqrt {{\sum \_{i}^{n}\left\(X\_{i}-{\overline {X}}\right\)^{2}} \over {n-1}}}{\sqrt {{\sum \_{i}^{n}\left\(Y\_{i}-{\overline {Y}}\right\)^{2}} \over {n-1}}}}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/135b1f3a3a7a31a050f8b7f9325e5b14db99e37a)

따라서![{\displaystyle r\_{XY}={{\sum \_{i}^{n}\left\(X\_{i}-{\overline {X}}\right\)\left\(Y\_{i}-{\overline {Y}}\right\)} \over {{\sqrt {\sum \_{i}^{n}\left\(X\_{i}-{\overline {X}}\right\)^{2}}}{\sqrt {\sum \_{i}^{n}\left\(Y\_{i}-{\overline {Y}}\right\)^{2}}}}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/ee1e03b44aabd2904cca430279faad515c617891)

