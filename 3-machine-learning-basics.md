# 3강 - basics of python project

## 0. Updating Github repository

1. 바탕화면으로 이동 cd desktop 
2. 클론된 라페지토리로 이동 cd python\__study\_SYL_
3. _add 하기 git add ._
4. _commit 하기 git commit -m "initial commit"_
5. _push 하기_ git push origin HEAD:master
6. _ID, password 입력 후 종료_ 

또는 해당 과정을 github web interface를 통해  
1. add file 클릭   
2. upload file 클릭  
3. file drop down 후 commit changes 클릭  
4. make pull request  
5. merge pull request  
과정을 통해서도 손쉽게 업로드 가능.

## 1. Commentary on the previous assignment

```python
import re
def logs():
    with open("assets/logdata.txt", "r") as file:
        logdata = file.read()
    y = logdata.splitlines()
    #print(y[0])
    name_list = list()
    dictionary_iter = dict()
    #for i in range(len(y)):
    for i in range(2):
        dictionary_iter['host'] = str(re.findall('.*[.][\d]* -',y[i]))
        dictionary_iter['host'] = dictionary_iter['host'][2:-4]
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
        name_list.append(dictionary_iter)
        name_list.append(i) 
    
    print(name_list)
    return name_list
    # YOUR CODE HERE
    raise NotImplementedError()
```

caution : `dictionary` is mutable  
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
    (\s\-\s)
    (?P<user_name>\w+|-)
    (\s\[)
    (?P<time>\d{1,2}\/\w+\/\d{2,4}:\d{1,2}:\d{1,2}:\d{1,2}\s-\d{1,4})
    (\]\s")
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
5. `VERBOSE`option deletes whitespace and enables commentary.
6. `finditer` enables iteration.
7. `groupdict` returns a new dictionary every time.

```python
In [20]: id(m.groupdict())
Out[20]: 3075475492L

In [21]: id(m.groupdict())
Out[21]: 3075473588L
```

## 1. Feature of Jupyter Notebook

magic command 

## 3. Relational Database 

