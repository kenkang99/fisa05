# 2주차
## 1일차

### 문자열 출력, 정규식

#### 문자열

##### 문자열을 보이는 그대로 나타내는 방법 print(r' ')

```python
print('C:\0ITStudy') # C:  ITStudy
print(r'C:\0ITStudy') # C:\0ITStudy
```

##### 자주 사용되는 문자열 메소드
split()
replace()
strip()
join()
format()

###### split()
형식 - split(self, /, sep=None, maxsplit=-1)

```python
# 기본값은 공백문자(' ', \t, \n, \r, \f )
str1 = '안녕/안녕/안녕하세요/반가워요/좋 은아 침입 니다'
str1.split() # ['안녕/안녕/안녕하세요/반가워요/좋', '은아', '침입', '니다']
```

! None이 ' ' , '\n' 등을 직접 의미하는 것은 아니고 split함수에서의 sep=None 이 공백문자를 의미하는 것임을 주의

https://regex101.com/ - python Flavor로 체크해서 quick reference 확인하면 익힐 수 있다
r"[a-zA-Z가-힣] - 영어, 한국어 포함
r"[^a-zA-Z가-힣] - 영어, 한국어 제외 모든 것
r"^[a-zA-Z가-힣] - 영어,한국어 중 하나로 시작하는 값 매치

#### 잡다지식
help(print) == print? 와 같은 기능(추가 ex - str.split?)