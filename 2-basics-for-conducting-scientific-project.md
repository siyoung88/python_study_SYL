# 2강 - basics for conducting project

## 0. 왜 python인가 ?

web of science의 논문 crawling 과제를 기억하시나요?  
업무 분배 -&gt; 0 부터 65000 까지의 index를 인원수에 맞게 분할했음.  
0-5000 SY  
5001-10000 YS  
10001-15000 SZ  
'''  
vs.  
0-65000 crawling  
업무 의뢰 -&gt; ㅇㅇ학부  
  
안전교육  
안전교육 영상 종료시 pop-up -&gt; javascript로 네트워크 pop-up 메세지 감지  
selenium으로 pop-up message click   
해당 프로그램을 ㅇㅇ학부 학생들끼리 거래    
  
"코딩을 잘하는 것과 업무를 효율적으로 하는 것은 별개의 문제"  
  
더 정확한 알고리즘을 통해 귀납적으로 최적해를 도출하는 ML 과정의 목표를 답습하는 것은 사회과학에서 지금 당장 받아들여지기 힘들겠지만. ML을 배우면서 "부수적으로" 얻게되는 프로그래밍을 통한 **올바른 실험 설정**과 **수학적 모델링 기법**, 그리고 **비정형데이터를 다루는 능력**이 사화과학 연구에 도움이 될것. 

## 1. assignment 수행 방법

1-1 Introduction to Data Science in Python 강의 에서 Week1 클릭.  
Lab: Your Personal Jupyter Notebook Workspace  
  
\# Jupyter Notebook vs. Pycharm -   
Jupyter Notebook 이 좀더 high level의 통계 실험에 유용. cell 별로 실험 결과를 볼 수 있어서 직관적.   
Pycharm. 상대적으로 lower level의 프로그래밍 지식이 있으면 유용. 큰 프로젝트를 구성하여 알고리즘 자체에 손대기 좋음.  
  
에서 각 week에 해당하는 assignment에 들어가서   
각 cell의 빈 algorithm을 채워넣고 실행하면 됨.   
  
\#실행 후 변경사항이 있을때에는 꼭 kernel을 초기화 해줄 것.   
  
해당 과정을 Coursera 자체 embed system에서 하는 것도 좋지만, 결국은 내가 직접 실험하기 위해서 Anaconda를 설치하는 것을 추천.   
  
1-2 [https://www.anaconda.com/products/individual](https://www.anaconda.com/products/individual) 에서 각 운영체제에 맞는 다운로드 파일 클릭.  
안내에 따라 설치하되 빨간색 표시된 anaconda python classpath설정 체크박스를 꼭 체크할 것.  
  
이후 아나콘다를 실행하고 Jupyter Notebook을 찾아서 실행하면 됨.  
과제에 필요한 파일은 매주 수업 후 업로드.   
  
1-3 여기까지 했으면, 1강 내용으로 돌아가 업로드를 테스트.  

## 2. Call by reference & Call by value 

Python 에서 Dictionary 와 List는 Call by reference로 취급합니다. 

```python
# It is important to realize that a slice of an array is a view into the same data. This is called passing by
# reference. So modifying the sub array will consequently modify the original array

# Here I'll change the element at position [0, 0], which is 2, to 50, then we can see that the value in the
# original array is changed to 50 as well
a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])
sub_array = a[:2, 1:3]
print("sub array index [0,0] value before change:", sub_array[0,0])
sub_array[0,0] = 50
print("sub array index [0,0] value after change:", sub_array[0,0])
print("original array index [0,1] value after change:", a[0,1])
```

sub array index \[0,0\] value before change: 2   
sub array index \[0,0\] value after change: 50   
original array index \[0,1\] value after change: 50

결국 프린트하면 알아볼 수 없다. \(python 에서는\)   
이런식으로 print가 미리 프로그래밍 되어있는 것이 이전 장의 overloading 이다.

## 3. regex and automata

[https://regex101.com/](https://regex101.com/)  
정규표현식

\*, + 반복

* 0개 이상 반복
* 하나 이상 반복

a\* , a, aa, aaa  
a+ a, aa, aaa  
\[a\]  
a,  
\[a\]\*  
, a , aa, aaa, aaaa  
\[ab\] A,b  
\[ab\]\* , a , b, ab, ababa, abababab, ,bbbbbbb  
\[a-z\] A,b,c,d,e,f,  
\[a-z\]+ A,b,c,d, apple, banana

  
\[A-Z\]\[a-z\]+ 고유명사 ex\) Apple, Siyoung, Haejun

  
. 모든 문자가 가능합니다.  
.\*  
, 1 ,2 ,a ,b ,c ,p , be ambitious, Most muscular.  
\[.\] ex\)  .  
\[.\]+ ex\)  ., .., ...   
\[.\]\* ex\) , ., ... , .....  
\[,\],.  
abc a\*  
ex\) abc a, abc aa,   
  
abc aaa \[ \(x\)  
abc aaa . \(x\)  
\[\[\] \(o\)  
\[.\] \(o\)  
\[010.5113.6365\]를 express 하려면,  
\[\[\]\d\d\d\[.\]\d\d\d\d\[.\]\d\d\d\d\[\]\] \[010.5113.6365\]

```python
[ab] .*[[][.]+ [\d]{1,3}
```

## 4. 절차형, 객체지향형, 함수형 프로그래밍

OOP

```python
class Country:
    """Super Class"""

    name = '국가명'
    population = '인구'
    capital = '수도'

    def show(self):
        print('국가 클래스의 메소드입니다.')


class Korea(Country):
    """Sub Class"""

    def __init__(self, name):
        self.name = name

    def show_name(self):
        print('국가 이름은 : ', self.name)
```

```python
>>> from inheritance import *
>>> a = Korea('대한민국')
>>> a.show()
국가 클래스의 메소드입니다.
>>> a.show_name()
국가 이름은 :  대한민국
>>> a.capital
'수도'
>>> a.name
'대한민국'
```

  
functional programming 

```python
people = ['Dr. Christopher Brooks', 'Dr. Kevyn Collins-Thompson', 'Dr. VG Vinod Vydiswaran', 'Dr. Daniel Romero']

def split_title_and_name(person):
    return person.split()[0] + ' ' + person.split()[-1]

#option 1
for person in people:
    print(split_title_and_name(person) == (lambda x: x.split()[0] + ' ' + x.split()[-1])(person))

#option 2
list(map(split_title_and_name, people)) == list(map(lambda person: person.split()[0] + ' ' + person.split()[-1], people))
```

해당 개념은 week1 과제에서 무시하셔도 좋습니다.

