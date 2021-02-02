# 3강 - basics of python project

## 0. Schedule

~~Jan 26th, 27th~~   
Introduction week1 + assignment 1 due to Feb 2nd  
~~Feb 2nd~~,  3rd   
Introduction week2 + assignment 2 due to Feb 9th  
Feb 9th, 10th Introduction to Python  
Introduction week4 + assignment 4 due to Feb 16th  
Feb 16th, 17th Machine Learning  
Machine Learning week 1 + assignment 1 due to Feb 23th  
Feb 23th, 24th Text Mining  
Text Mining week 1 + assignment 1 there's no due date

3 python Introductory courses + 2 appliances \(each from machine learning and text mining\)   
패스트 캠퍼스 강의 16강

과제: 매주 화요일까지 해당 week의 contents 수강 및 assignment push   
수업 내용: 화요일, 강의에 대한 개념 보조설명  
수요일, 과제 진행을 위한 상세설명 및 과제 review

## 0. Updating Github repository

1. 바탕화면으로 이동 `cd desktop` 
2. 클론된 라페지토리로 이동 `cd python_study_SYL`
3. `git status` 를 통해서 nothing to commit, working tree clean 메세지 확인
4. working tree 가 clean하지 않다면, `git pull`
5. branch해서 다른 branch들도 확인하기, Figure 1을 참조한.  4-1 `git branch` 를 통해 어떤 branch들이 있는지 확인 가능 4-2 `git branch master`를 통해 master branch를 생성 4-3 `git branch` 를 통해 master branch가 생겼는지 확인 4-4 `git checkout master` 를 통해 master branch로 이동  4-5 local git repository folder를 오픈해서 자료가 없는지 확인 4-6 `git branch --set-upstream-to=origin/master master` 를 통해 origin \(remote\)저장소와 local의 master 브랜치를 연결 4-7 `git status` 를 통해 새로운 브랜치인 것을 확인 
6. 진행한 과제의 assignment1 폴더를 local git repository folder 다운로드 또는 이동/복사
7. add 하기 __`git add .`
8. commit 하기 __`git commit -m "week1 commit"`
9. push 하기 __`git push origin HEAD:master`
10. ID, password 입력 후 종료

![Figure 1. version management diagram with Git](.gitbook/assets/git_image.png)

또는 해당 과정을 Github web interface를 통해  
1. add file 클릭   
2. upload file 클릭  
3. file drop-down 후 commit changes 클릭  
4. `make pull request`  
5. `merge pull request`  
과정을 통해서도 손쉽게 업로드 가능.

## 1. Commentary on the previous assignment

```python
import re
def logs():
    with open("assets/logdata.txt", "r") as file:
        logdata = file.read()
    y = logdata.splitlines() #불러온 데이터의 줄 구분
    #print(y[0])
    name_list = list() #리스트 선언
    dictionary_iter = dict() #딕셔너리 선언
    for i in range(len(y)): #y의 줄 수 만큼 반복
    #for i in range(2): #몇번 반복할지
        dictionary_iter['host'] = str(re.findall('.*[.][\d]* -',y[i])) #찾아서
        dictionary_iter['host'] = dictionary_iter['host'][2:-4] #잘라내
        #print(dictionary_iter['host'])
        dictionary_iter['user_name'] = str(re.findall('- [\w]* [[]',y[i]))
        if len(dictionary_iter['user_name']) == 2 :
            dictionary_iter['user_name'] = '-'
        else:
            dictionary_iter['user_name'] = dictionary_iter['user_name'][4:-4]
        #print(dictionary_iter['user_name'])
        dictionary_iter['time'] = str(re.findall('[[].*[]]',y[i]))
        dictionary_iter['time'] = dictionary_iter['time'][3:-3]
        #print(dictionary_iter['time'])
        dictionary_iter['request'] = str(re.findall('["].*["]',y[i]))
        dictionary_iter['request'] = dictionary_iter['request'][3:-3]
        #print(dictionary_iter['request'])
        name_list.append(dictionary_iter.copy())
        name_list.append(i) 
    
    print(name_list)
    return name_list
    # YOUR CODE HERE
    raise NotImplementedError()
```

caution : `dictionary` is mutable == call by reference  
we should use `.copy()` to return new dictionaries

```python
import re
def logs():
    with open("assets/logdata.txt", "r") as file:
        logdata = file.read()
    
    # YOUR CODE HERE
    logs = []
    pattern = """
    (?P<host>\d{1,3}.\d{1,3}.\d{1,3}.\d{1,3})
    (\s\-\s) #필요없는 곳
    (?P<user_name>\w+|-)
    (\s\[) #필요없는 곳
    (?P<time>\d{1,2}\/\w+\/\d{2,4}:\d{1,2}:\d{1,2}:\d{1,2}\s-\d{1,4})
    (\]\s") #필요없는 
    (?P<request>\w+\s[\/\w\-\+\S]+\s\w+\/\d{1}.\d{1})
    """
    
    for item in re.finditer(pattern,logdata,re.VERBOSE):
        logs.append(item.groupdict())
    
    return logs
    # raise NotImplementedError()
```

1. backslash changes meta character \(right after it\) into character and character into meta character.
2. ?P is an extension to basic regex.
3. \(\) is for grouping.
4. &lt;&gt; indicates keys from dictionaries.
5. `VERBOSE`option deletes whitespace and enables commentary. 5-1 But then, how do we express 'whitespace'? 5-2 it can be expressed as \s
6. `finditer` enables iteration.
7. `groupdict` returns a new dictionary every time.

```python
In [20]: id(m.groupdict())
Out[20]: 3075475492L

In [21]: id(m.groupdict())
Out[21]: 3075473588L
```

## 1. Feature of Jupyter Notebook

magic command 란 무엇일까요? Jupyter는 본래 IPython을 모태로 하고 있습니다. IPython은 Python + Shell의 사용성을 지향한 소프트웨어로서, Shell의 명령어를 Python에서 직접 활용할 수 있도록 합니다.   
따라서, 이러한 명령을 기존의 Python 언어를 사용하는 타 IDE인 Pycharm과 같은 프로그램에서는 사용할 수 없습니다.  
예를 들면, stata 소프트웨어 자체도 C언어로 짜여졌습니다. 다만, 그 안에서 사용하는 자체적인 규칙의 방향성이 다르고 우리는 이를 새로운 언어처럼 생각하고 있는 것이지요.   
반면에, R의 경우에는 그 자체로 Python과 C에 상응하는 '프로그래밍 언어'입니다.

```python
%%timeit -n 100
```

## 2. Bitwise and Logical operators

1. Bitwise Operators a = 60, b = 13 이라 하면, 이진법으로 나타냈을 때 a = 0011 1100, b = 0000 1101 이다. 1 2 4 8 16 32 64 128  32+16+8+4 = 60

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC5F0;&#xC0B0;&#xC790;</th>
      <th style="text-align:left">&#xC124;&#xBA85;</th>
      <th style="text-align:left">&#xC608;&#xC2DC;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&amp;</td>
      <td style="text-align:left">AND &#xC5F0;&#xC0B0;&#xC73C;&#xB85C;, &#xB458; &#xB2E4; &#xCC38;(1)&#xC77C;&#xB54C;&#xB9CC;
        1&#xC774;&#xB2E4;.</td>
      <td style="text-align:left">
        <p><code>(a &amp; b) = 12</code> -&gt;</p>
        <p>0000 1100</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">|</td>
      <td style="text-align:left">OR &#xC5F0;&#xC0B0;&#xC73C;&#xB85C;, &#xB458; &#xC911; &#xD558;&#xB098;&#xB9CC;
        &#xCC38;(1)&#xC774;&#xC5B4;&#xB3C4; 1&#xC774;&#xB2E4;.</td>
      <td style="text-align:left">
        <p><code>(a | b) = 61</code> -&gt;</p>
        <p>0011 1101</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">^</td>
      <td style="text-align:left">XOR &#xC5F0;&#xC0B0;&#xC73C;&#xB85C;, &#xB458; &#xC911; &#xD558;&#xB098;&#xB9CC;
        &#xCC38;&#xC77C; &#xB54C; 1&#xC774;&#xB2E4;.</td>
      <td style="text-align:left">
        <p><code>(a ^ b) = 49</code> -&gt;</p>
        <p>0011 0001</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">~</td>
      <td style="text-align:left">&#xBCF4;&#xC218;&#xB97C; &#xCDE8;&#xD55C;&#xB2E4;.</td>
      <td style="text-align:left">
        <p><code>(~a) = -61</code> -&gt;</p>
        <p>1100 0011</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;&lt;</td>
      <td style="text-align:left">&#xC9C0;&#xC815;&#xD558;&#xB294; &#xBE44;&#xD2B8; &#xC218;&#xB9CC;&#xD07C;
        &#xC67C;&#xCABD;&#xC73C;&#xB85C; &#xBE44;&#xD2B8; &#xC2DC;&#xD504;&#xD2B8;</td>
      <td
      style="text-align:left">
        <p><code>a &lt;&lt; 2 = 240</code> -&gt;</p>
        <p>1111 0000</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&gt;&gt;</td>
      <td style="text-align:left">&#xC9C0;&#xC815;&#xD558;&#xB294; &#xBE44;&#xD2B8; &#xC218;&#xB9CC;&#xD07C;
        &#xC624;&#xB978;&#xCABD;&#xC73C;&#xB85C; &#xBE44;&#xD2B8; &#xC2DC;&#xD504;&#xD2B8;</td>
      <td
      style="text-align:left">
        <p><code>a &gt;&gt; 2 = 15</code> -&gt;</p>
        <p>0000 1111</p>
        </td>
    </tr>
  </tbody>
</table>

2. Logical Operators   
기존 언어들에서는 and는 &&이고 or는 \|\|이다. python에서는 and와 or를 써서 비트연산자와 시각적인 구분을 둔다.  
논리연산자에서는 a와 b를 True, False의 boolean으로 봤을 때 

| 연산 | 설 | 예시 |
| :--- | :--- | :--- |
| and | 둘다 참일때 참\(True\) | \(a and b\) = False |
| or | 둘 중 하나만 참이어도 참\(True\) | \(a or b\) = True |
| not | 논리 반전 | not\(a and b\) = True |

## 3. Relational Database

![Figure 2. &#xAD00;&#xACC4;&#xD615; &#xB370;&#xC774;&#xD130;&#xBCA0;&#xC774;&#xC2A4; \(TCPschool.com &#xCD9C;&#xCC98;\)](.gitbook/assets/img_mysql_table.png)

행, 튜플, 레코드의 접근 방법. 열, 필드, 속성의 접근 방법. 수정 방법. 조건에 따른 검색 방법  


