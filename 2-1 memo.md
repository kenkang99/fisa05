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

###### replace()
 형식 - str.replace(self, old, new, count=-1, /)

```python
str1 = '안녕 나는 짱구 아빠야'

str1.replace("짱구", '훈이') # 안녕 나는 짱구 아빠야

str1 = '안녕 나는 짱구 짱아 짱짱맨 짱 아빠야' 

str1.replace("짱", '훈', 4)  # 안녕 나는 훈구 훈아 훈훈맨 짱 아빠야

```
! str1은 기본적으로 불변 자료형이기 때문에 getter의 방식대로 적용돼 str1이 실질적으로 변형되지는 않는다

! str 특성상 불변자료형으로 getter이긴 하지만 return에 str을 return해서 다음과 같이 연속으로 작성해도 적용이 잘 된다

```python
 string.replace("(", "").replace(")", "").replace("\n", " ")
```

###### strip()
형식 - str.strip(chars=None, /)

```python
st2 = '          \t hello   \n        '

st2.strip() # 'hello'

```
strip()은 공백 뿐만 아니라 공백문자까지 삭제해준다


```python
actors = '1010로미오&줄리엣1010'
actors1 = '로미오1010&줄리엣'

actors.strip('10') # '로미오&줄리엣'
actors1.replace('10','') # '로미오&줄리엣'
```
! 가운데 공백혹은 노이즈들은 따로 삭제가 불가능해서 replace를 통해서 제거하는 방법도 채용할 수 있다

###### join()
형식 - str.join(self, iterable, /)

```python
a = ['짱구', '짱아', '장구엄마', '짱구아빠']

'*'.join(a) #'짱구*짱아*장구엄마*짱구아빠'
```
join()에 들어갈 argument는 iterable로 복수형이어야한다.

```python
dict1 = {'a': 1234, 'b':356}

# join을 사용해서 ' '을 구분자로 묶어주세요
# 'a b' '1234 356'

print(' '.join(dict1.keys())) # a b
print(' '.join(str(dict1.values()))) # d i c t _ v a l u e s ( [ 1 2 3 4 ,   3 5 6 ] )  

```
dict1.values의 경우 객체형태로 존재하기 때문에 단순 str()으로 각각의 요소에 str를 적용할 수 없다 <- str는 argument 전체에 대해 문자열형태로 만드므로

그래서 map과 같은 yield를 사용한 generator와 같은 느낌으로 순차적으로 탐색을 하는 방법으로 각자 적용시켜야함

```python

# map은 객체의 메모리표현형태로 출력됨
[map(str,dict1.values())] # [<map at 0x7b60fd1a87c0>]

# *를 통해 요소별 확인
[*map(str,dict1.values())] # ['1234','356'] 
```

###### count()
형식 - S.count(sub[, start[, end]]) -> int
<- 카운트할 문자열 필수로 필요한 부분이 sub인자이고, [, start] 부분이 검색할 인덱스 시작번호로 부가적인 요소, [, end] 부분은 검색할 인덱스 끝번호로 부가적인 요소
**count(self, sub: str, start: SupportsIndex | None = ..., end: SupportsIndex | None = ..., /) -> int: ...** 형식임


```python
'happy'.count('p',0,2) # 0
'happy'.count('p',0,3) # 1
```

###### find()
형식 - S.find(sub[, start[, end]]) -> int
<- 존재하면 시작점의 인덱스 반환, 존재하지 않으면 -1을 반환

```python
str1 = 'IT AI AI Engineering Engineer deer'

str1.find('AI',5,7) # -1
str1.find('AI',5,8) # 6
```

##### 부가적인 문자열 메소드
###### casefold()
유니코드 기준으로 적용돼 lower()보다 강력하게 소문자로 변환시킴 

```python
string2='ⓑ ɛ ʒ ß HELLO Uou'  # 국제화(i18n) - 개발 자체가 각 지역에서 쉽게 사용할 수 있도록 설계하는 것을 의미합니다.

string2.casefold() # ⓑ ɛ ʒ ss hello uou
```


#### 정규식
+ "특정 조건 또는 패턴"을 치환하는 과정을 쉽게 처리할 수 있는 방법

+ re 패키지를 사용한다

match() : 문자열의 첫 시작부터 정규식과 매치되는지 조사한다.
search() : 문자열 전체를 검색하여 정규식과 매치되는지 조사한다.
findall() : 정규식과 매치되는 모든 문자열(substring)을 리스트로 돌려준다.
finditer() : 정규식과 매치되는 모든 문자열(substring)을 반복 가능한 객체로 돌려준다. 

형식 - 
p = re.compile('패턴')
p.method(데이터)

##### match()
```python
str1 = 'IT AI AI Engineering Engineer deer'

p = re.compile("AI")
p.match(str1) # str1의 첫 시작이 p와 일치하는지 탐색하는 메서드

# 아무것도 출력되지 않습니다.

p = re.compile("IT")
p.match(str1) # <re.Match object; span=(0, 2), match='IT'>

```
! re.Match 객체는 iterable한 것은 아니고 해당되는 매서드를 통해 각각의 요소들을 활용할 수는 있음 ex - p.match(str1).group() # 'IT'

##### search()
```python
str1 = 'IT AI AI Engineering Engineer deer'

p = re.compile("AI")
p.search(str1) # str1의 어느 위치이건 패턴과 일치하는 첫 부분의 인덱스 범위를 찾아줍니다.

# <re.Match object; span=(3, 5), match='AI'>

```

##### findall()
```python
str1 = 'IT AI AI Engineering Engineer deer'

p = re.compile("AI")
p.findall(str1) # 정규식과 일치하는 모든 문자열(substring)을 리스트로 돌려줍니다.

```

##### finditer()
search()의 다수 버전 느낌

```python
[*p.finditer(str1)] # 정규식과 일치하는 반복가능한 객체로 돌려줍니다.

# 출력결과
'''
[<re.Match object; span=(3, 5), match='AI'>,
 <re.Match object; span=(6, 8), match='AI'>]'''

```
##### [문자클래스] - 정규표현식 문법

1. [abc] - a, b, c중 한 개의 문자와 매치
  - a : 매치여부 OK
  - apple : ok
  - double : no

2. \d   : 숫자와 매치, [0-9]와 동일

3. \D : 숫자가 아닌 것과 매치 [^0-9]와 동일

4. \s : whitespace 문자와 매치

5. \S : whitespace 문자가 아닌것과 매치,

6. \w : 문자 + 숫자와 매치, [a-zA-Z0-9_]와 동일

7. \W : 문자+숫자가 아닌 문자와 매치. [^a-zA-Z0-9_]와 동일
  


8. a.b : a와 b 사이에 줄바꿈 문자를 제외한 모든 문자 허용

9. a[.]b : a와 b 사이에 dot 문자만 허용

10. ca*t  : a 문자가 0번 이상 반복 허용

11. ca+t :  a 문자가 1번 이상 반복 허용

12. ca?t : a 문자가 없거나, 1번만 허용

13. ca{3}t : a 문자가 3번 반복되면 매치

14. ca{2, 3}t : a 문자가 2~3번 반복되면 매치

```python

str1 = 'abIT aIT IT it AI AI Engineering Engineer deer 한글 010-1234-5678 😊❤ aa👌 '

import re

p = re.compile("[a-zA-Z]+") # 1글자 이상의 알파벳으로 된 문자열

# +를 안 붙이면 문자별로 그룹핑이 됨

p.findall(str1)

# 출력결과
'''
['abIT',
 'aIT',
 'IT',
 'it',
 'AI',
 'AI',
 'Engineering',
 'Engineer',
 'deer',
 'aa']
'''

```

```python
p = re.compile("\w+") # 1글자 이상의 알파벳으로 된 문자열+숫자 그룹

p.findall(str1)

'''출력결과
['abIT',
 'aIT',
 'IT',
 'it',
 'AI',
 'AI',
 'Engineering',
 'Engineer',
 'deer',
 '한글',
 '010',
 '1234',
 '5678',
 'aa']
'''
```

```python
p = re.compile("[a-zA-Z]*") # 0글자 이상의 알파벳으로 된 문자열 , 문자열별로 알파벳 혹은 공백이 모두 그룹핑이 됨
# *은 ab*를 예시로 했을 때, a혹은 ab혹은 abbbb.. 등이 포함됨
 
p.findall(str1)
'''
['abIT',
 '',
 'aIT',
 '',
 'IT',
 '',
 'it',
 '',
 'AI',
 '',
 'AI',
 '',
 'Engineering',
 '',
 'Engineer',
 '',
 'deer',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 'aa',
 '',
 '',
 '']
 '''
```

```python
p = re.compile("[a-zA-Z]?") # 공백 또는 1글자의 알파벳으로 된 문자열
# ?는 *의 단일 문자적용 느낌임

p.findall(str1)
'''출력결과
['a',
 'b',
 'I',
 'T',
 '',
 'a',
 'I',
 'T',
 '',
 'I',
 'T',
 '',
 'i',
 't',
 '',
 'A',
 'I',
 '',
 'A',
 'I',
 '',
 'E',
 'n',
 'g',
 'i',
 'n',
 'e',
 'e',
 'r',
 'i',
 'n',
 'g',
 '',
 'E',
 'n',
 'g',
 'i',
 'n',
 'e',
 'e',
 'r',
 '',
 'd',
 'e',
 'e',
 'r',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 '',
 'a',
 'a',
 '',
 '',
 '']
'''
```

```python
p = re.compile("[^a-zA-Z]+") # 알파벳으로 되어있지 않은 모든 것을 출력

# ^는 문자열에서의 ! 역할 느낌임
p.findall(str1)

# [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' 한글 010-1234-5678 😊❤ ', '👌 ']
```


https://regex101.com/ - python Flavor로 체크해서 quick reference 확인하면 익힐 수 있다
r"[a-zA-Z가-힣] - 영어, 한국어 포함
r"[^a-zA-Z가-힣] - 영어, 한국어 제외 모든 것
r"^[a-zA-Z가-힣] - 영어,한국어 중 하나로 시작하는 값 매치

#### 잡다지식
help(print) == print? 와 같은 기능(추가 ex - str.split?)