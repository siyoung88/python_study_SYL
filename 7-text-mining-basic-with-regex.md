# 7강 - Text mining basic with Regex



## 3. regex and automata

[https://regex101.com/](https://regex101.com/)  
정규표현식

\*, + 반복  
meta character : \[, \], ., ^, \*, + 

* 0개 이상 반복
* 하나 이상 반복

`a*`  example\) a, aa, aaa  
`a+` example\) a, aa, aaa  
`[a]` example\) a  
`[a]*` example\) a , aa, aaa, aaaa  
`[ab]` example\) a,b  
`[ab]*` example\) a , b, ab, ababa, abababab, ,bbbbbbb  
`[a-z]` example\) a,b,c,d,e,f,  
`[a-z]+` example\) a,b,c,d, apple, banana  
`[A-Z][a-z]+` 고유명사 ex\) Apple, Siyoung, Haejun  
`.` : newlines 제외 모든 문자가 가능합니다.  
`.*`example\) 1 ,2 ,a ,b ,c ,p , be ambitious, Most muscular.  
`[.]` example\)  .  
`[.]+` example\)  ., .., ...   
`[.]*` example\) , ., ... , .....  
`[,]` example\) ,   
`abc a*`example\) abc a, abc aa,   
  
`abc aaa [` 와 같은 표현 방식은 '\['가 meta character이므로 유효하지 않습니다.\(x\)  
`abc aaa .` 와 같은 표현 방식은 '.'가 meta character이므로 유효하지 않습니다.\(x\)  
`[[]` \(o\)  
`[.]` \(o\)  
\[010.5113.6365\]를 express 하려면,  
`[[]\d\d\d[.]\d\d\d\d[.]\d\d\d\d[]]` \[010.5113.6365\]

```python
[ab] .*[[][.]+ [\d]{1,3}
```

