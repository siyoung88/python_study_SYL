# 3강 - Machine learning basics

## 3. Group by



## 4. Pearson Correlation Coefficient

[통계학](https://ko.wikipedia.org/wiki/%ED%86%B5%EA%B3%84%ED%95%99)에서 , 피어슨 상관 계수\(Pearson Correlation Coefficient ,PCC\)란 두 변수 X 와 Y 간의 선형 [상관 관계](https://ko.wikipedia.org/wiki/%EC%83%81%EA%B4%80_%EB%B6%84%EC%84%9D)를 계량화한 수치다 . 피어슨 상관 계수는 [코시-슈바르츠 부등식](https://ko.wikipedia.org/wiki/%EC%BD%94%EC%8B%9C-%EC%8A%88%EB%B0%94%EB%A5%B4%EC%B8%A0_%EB%B6%80%EB%93%B1%EC%8B%9D)에 의해 +1과 -1 사이의 값을 가지며, +1은 완벽한 양의 선형 상관 관계, 0은 선형 상관 관계 없음, -1은 완벽한 음의 선형 상관 관계를 의미한다. 일반적으로 상관관계는 피어슨 상관관계를 의미한다.



피어슨 상관 계수는 [등간척도](https://ko.wikipedia.org/w/index.php?title=%EB%93%B1%EA%B0%84%EC%B2%99%EB%8F%84&action=edit&redlink=1)나 [비례척도](https://ko.wikipedia.org/w/index.php?title=%EB%B9%84%EB%A1%80%EC%B2%99%EB%8F%84&action=edit&redlink=1)의 데이타에서 두 변수의 [공분산](https://ko.wikipedia.org/wiki/%EA%B3%B5%EB%B6%84%EC%82%B0) 을 [표준 편차](https://ko.wikipedia.org/wiki/%ED%91%9C%EC%A4%80_%ED%8E%B8%EC%B0%A8) 의 곱으로 나눈 [값](https://ko.wikipedia.org/wiki/%ED%91%9C%EC%A4%80_%ED%8E%B8%EC%B0%A8)이다.![{\displaystyle r\_{XY}={{{\sum \_{i}^{n}\left\(X\_{i}-{\overline {X}}\right\)\left\(Y\_{i}-{\overline {Y}}\right\)} \over {n-1}} \over {{\sqrt {{\sum \_{i}^{n}\left\(X\_{i}-{\overline {X}}\right\)^{2}} \over {n-1}}}{\sqrt {{\sum \_{i}^{n}\left\(Y\_{i}-{\overline {Y}}\right\)^{2}} \over {n-1}}}}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/135b1f3a3a7a31a050f8b7f9325e5b14db99e37a)

따라서![{\displaystyle r\_{XY}={{\sum \_{i}^{n}\left\(X\_{i}-{\overline {X}}\right\)\left\(Y\_{i}-{\overline {Y}}\right\)} \over {{\sqrt {\sum \_{i}^{n}\left\(X\_{i}-{\overline {X}}\right\)^{2}}}{\sqrt {\sum \_{i}^{n}\left\(Y\_{i}-{\overline {Y}}\right\)^{2}}}}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/ee1e03b44aabd2904cca430279faad515c617891)  


