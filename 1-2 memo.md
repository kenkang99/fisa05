# 1주차
## 2일차

### CS 배경지식

#### 메모리(RAM)이란?
- 주기억장치
    - 프로그램 실행 중에는 반드시 메모리에 있어야함
    - 메모리 어딘가에서 보조기억장치(하드웨어)에서 불러온 파일을 두고두고 꺼내쓰는 느낌

#### 변수란?
- 자료를 메모리에 저장해두고 불러오기 위해 이름을 지어주는 것
- 주소값을 사용하여 주소기준으로 식별을 함
    - 비트 형태로 저장되고 1바이트(8비트)마다 번호를 부여
        - 16 진수를 많이 사용하는 이유 ?






- 프로그래밍 언어 종류 2가지 - 저수준 언어, 고수준 언어
    - 저수준 언어 : 컴퓨터용 언어
    - 고수준 언어 : 인간용 언어
- 컴퓨터에 가까울 수록 저수준 언어, 인간과 가까울수록 고수준 언어라고 합니다
- 저수준언어에 가까울수록 저사양컴퓨터에서도 동작하기 쉽다. 따라서  IPTV, 복합기, 스캐너, 라디오, 프로젝터, 프린터와 같이 컴퓨터의 역할이 크게 요구되지 않거나 작은 하드웨어에서 사용됨

인터프리터는 하나씩 컴파일하게 된다
소스파일을 cpu가 이해할 수 있는 바이너리로 바꾸기 위해 컴파일을 함

컴파일 언어( C, C++, Java 등등)는 만들어진 소스파일을 프로그램실행을 통해 한번에 기계어로 바꿔주기 때문에 실행속도가 빠르지만 중간에 오류가 있다면 실행이 안된다는 단점이 있음
ㄴ 빠르고 단단한 프로그램이 필요할 때 사용(ex- 게임, 큰 프로그램 등)
인터프리터 언어( 파이썬, 자바스크립트, R 등등)는 Line By Line , 하나하나 번역해서 실행해서 속도가 느린 편
ㄴ 가변성이 많을 때 고치기 쉬움( ex- 웹사이트)
인터프리터도 사실상 REPL(Read, eval, print, loop)를 통해 컴파일을 하는 것이다

- 왜 1바이트는 8비트가 되었을까?
    - bit: 컴퓨터가 정보를 처리하는 최소 단위
        - 한 개의 비트는 0 또는 1을 표현
        - 8비트는 2^8개 만큼의 수를 표현(256개의 숫자를 표현)
    - MSB(Most Signature Bit/최상위부호비트)
        - MSB를 제외한 7개의 비트로 표현을 하면 2^7개 만큼 표현이 가능하고
        - 2^7 => 128개의 숫자를 표현할 수 있습니다.
        - 즉, 자신들이 사용하는 문자 체계에서 모든 숫자를 다 만드는데 7개의 비트면 충분했고, 부호까지 포함해도 8개의 비트면 모두 다 표현이 가능
        - 그래서 8비트는 1바이트가 되었습니다.

- 영어권 국가들은 전혀 문제가 되지 않습니다.
    - 한국, 중국, 일본, 이스라엘, 아랍, …… 태국…..
    - 그러나 아시아권 국가들은 1바이트로는 문자를 전부 표현할 수가 없습니다
    - 그래서 아주 오래된 언어들(키보드안의 문자들로 다 표현할 수 있는 언어)을 제외하면 거의 대부분의 언어들은 문자를 표현하는 최소 크기를 2바이트("UTF-8 유니코드"에서는, 한글이나 한자가 ***3바이트***입니다.)를 사용합니다.
- 한글은 어떻게 숫자로 표현하죠?
    - 아스키 테이블을 사용할 수 없습니다
    - 한글을 표현할 수 있는 새로운 문자 인코딩 셋을 만들었습니다
    - 문자 => 숫자(인코딩), 숫자 => 문자(디코딩)
    - 한글을 표현할 수 있는 인코딩 셋
!        - **UTF-8**, CP949, EUC-KR
        - 프로그래밍을 할 때는 주고 기본 UTF-8을 기본 인코딩 셋으로 사용합니다.
        - 한글 윈도우즈 같은 경우 기본 EUC-KR을 사용하는 경우가 많아요


CR LF에 대한 설명? <- 운영체제별로 줄바꿈인식형식이 정해져있는데 보통 리눅스는 LF형식으로 읽으면 정상실행이 되는거고 window는 CR인데 요즘 CR/LF로 되는 것이다
중간중간에 1분정도 생각정리or필기할 시간 주면 좋겠다
메모리주소 최초 할당은 랜덤으로? 아니면 변수값마다 고유 id가 있는가 <- 자주 쓰는 값들은 이미 파이썬을 실행할 때 주소가 할당돼있는 값들이 있음
ㄴ 변수명이 아닌 변수값별로 고유한 id값이 있다?
ㄴ 이미 할당돼있지않은 값에 대해서는 최초로 주소할당된 것을 사용하다가 1시간동안 그 값은 그 주소만을 사용(변수를 삭제하든 말든), 그러고서 다시 나중에 확인해보면 id가 바뀜
오버헤드란?



## 텍스트 데이터와 바이너리 데이터

데이터 포맷은 텍스트 데이터와 바이너리 데이터로 구분합니다.

- **텍스트 데이터**
    
    <aside>
    💡 문자 데이터로 사용할 수 있는 영역. 일반적으로 텍스트 에디터로 편집할 수 있는 데이터 포맷. 
    일반적인 자연언어(한국어, 영어, 한자 등)와 숫자, 공백, 특수문자 등으로 구성된다. 
    특수하게 줄바꿈과 탭등의 제어 문자도 포함되어 있다. 
    이외에는 모두 에디터에서 시각적으로 확인할 수 있는 데이터 형식이다.
    
    </aside>
    
    [장점] 텍스트 에디터가 있으면 편집할 수 있다. 설명도 포함할 수 있고 가독성이 높다.
    [단점] 바이너리 데이터에 비해 크기가 크다.
    
    - 텍스트 데이터가 파일에 저장되는 방식
    
    ![- 출처 : https://codedragon.tistory.com/5103](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/88b79179-00a6-4691-bece-836fc119c2b2/Untitled.png)
    
    - 출처 : https://codedragon.tistory.com/5103
    
    - 프로그래밍 언어의 소스코드도 텍스트 데이터. XML/JSON/YAML/CSV…웹에서 주로 사용되는 데이터 포맷은 텍스트 데이터를 기반으로 합니다.
    - 최근의 텍스트 데이터를 기반으로 하는 곳은 모두 utf-8로 인코딩합니다. HTML에서 utf-8사용을 권장하기 때문입니다.
    - 소스코드 파일은 텍스트 데이터 → 컴파일링 하고 나면 바이너리 데이터로 바뀔 뿐

텍스트 데이터는 문자가 적혀있는 데이터. 어떤 문자 코드(문자 인코딩)으로 저장돼 있냐에 따라 다른 의미를 갖게 됩니다. 같은 문장의 텍스트 파일이라도 문자 인코딩이 다르면 다른 문자를 표현합니다.

그 예로, euc_kr(줄바꿈 CRLF)와 utf-8(줄바꿈 LF)로 나타낼 때 같은 문장의 텍스트 데이터라도 파일을 열어봤을 때 문자가 깨져있을 수 있습니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fc8e399e-5d96-44cd-8e90-3e9e791bf528/Untitled.png)

- **바이너리 데이터**
    
    ![- 출처 : https://codedragon.tistory.com/5103](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/952a7e55-15cc-43dc-98a1-d202b7520fbf/Untitled.png)
    
    - 출처 : https://codedragon.tistory.com/5103
    

[장점] 텍스트 데이터에 비해 크기가 작다.
[단점] 텍스트 에디터로 편집할 수 없다. 어떤 바이트에 어떤 데이터가 있다고 정의해야 한다.

- 바이너리 데이터가 파일에 저장되는 방식

웹에서 사용되는 데이터는 대부분 **바이너리 데이터**입니다. 그 이유는 이미지와 동영상 같은 파일 때문입니다. 이미지와 동영상 파일은 용량이 크므로 최대한 서버의 부하를 줄이기 위해 크기를 압축하기 때문입니다. 이미지와 동영상의 경우에는 이미지에 따라 JPEG/GIF/PNG 등의 형식을 이용합니다.

코랩에서는 !를 앞에 붙이면 셸에서 실행되게끔 해준다

- 네이밍 컨벤션* (Naming Convention)
    
    통일성을 갖기 위해서는 사람들이 공유하는 코딩 스타일 가이드를 가지고 있는 것이 좋습니다.
    
    - **snake_case**
        - 모든 공백을 “_“로 바꾸고 모든 단어는 소문자입니다.
        - 파이썬에서는 함수, 변수 등을 명명할 때 사용합니다.
        - ex) this_is_snake_case
    - **PascalCase**
        - 모든 단어가 대문자로 시작합니다.
        - UpperCamelCase, CapWords라고도 불립니다.
        - 파이썬에서는 클래스를 명명할 때 사용합니다.
        - ex) ThisIsPascalCase
    - **camelCase**
        - 처음은 소문자로 시작하고 이후 모든 단어의 첫 글자는 대문자로 합니다.
        - 파이썬에서는 거의 사용하지 않습니다. (java 등에서 사용)
        - ex) thisCamelCase
    - **kebab-case**
        - 케밥이 꼬챙이에 꽂힌 모습에서 유래한 방법
        - 주로 HTML에서 많이 사용
        - 소문자로 시작하며, 띄어쓰기는 -로 대체합니다
        
        - 근데 회사마다 또 달라요

부동소수점은 약간의 오차가 생긴다(정확한 계산이 안나오게됨)
논리연산 좀 헷갈리네
리스트는 왜 같아도 id값이 다른가
튜플 vs 리스트 - 가변자료형이냐 아니냐의 차이

dir(list())에 __ __ 로 붙어있는 애들은 클래스 내부에서만 사용할 것들이다

문자열도 튜플과 같은 성격을 가지고있다

tu+tu # 불가변인데 왜 추가가 될까? <- 튜플 tu의 메모리주소 안에는 변하지 않는 값이 들어있고 원본에 연산한 새 결과를 반환한 것이다.

가변 불변은 자기 메모리 주소 안에 원본을 바꿀 수 있는가 아닌가의 차이다

li.insert(10000000000,1) 해도 맨 마지막에 요소 추가됨

def sample(a, b, /, c, d=0, *, e, f=1):
    ...
# a,b : 위치전용
# c,d : 둘 다 OK
# e,f : 키워드전용

| 호출 예                          | 설명           |
| ----------------------------- | ------------ |
| `sample(1, 2, 3, 4, e=5)`     | ✅            |
| `sample(a=1, b=2, 3, 4, e=5)` | ❌ a,b는 위치전용  |
| `sample(1, 2, c=3, d=4, e=5)` | ✅ c,d 키워드 OK |

아래 가변연산자들은 원래의 가변형자료를 가진 변수의 값을 바꾸므로 가변형연산자이다
#  'append',  'clear', 'copy',  'extend', 'insert', 'pop', 'remove', 'reverse',  'sort'

callable = “원소 1개 받아 비교 가능한 값 1개 반환” 
iterable = "원소 2개 이상 받아야지만 반환가능한 경우

문자열 순서 뒤집어서 확인하기 혹은 초기화하기는 인덱싱으로 해야함

대괄호는 안에 있는 것이 생ㅇ략이 돼도 된다는 뜻임

Help on method_descriptor:

find(...) unbound builtins.str method
    S.find(sub[, start[, end]]) -> int
    
    Return the lowest index in S where substring sub is found,
    such that sub is contained within S[start:end].  Optional
    arguments start and end are interpreted as in slice notation.
    
    Return -1 on failure.

'사과나무'.find('과') # 1

'사과나무'.find('과',2) # -1

'사과나무'.find('과',0,1) # -1

딕셔너리에서 value는 그대로, 키는 바꿀 수 없음 -> 삭제하고 새로 정의해야함

not subscriptable  <- 하나씩 가져올 수가 없다
ㄴ 이런 형태 error에서는 list로 형변환해주면 됨

dict에서의 조회는 value가 아닌 key로 하게된다

del은 변수 요소별로, 변수 자체 등의 유연한 삭제가 가능하지만 pop은 단일 삭제 및 삭제요소 반환이라는 차이가 있다



파이선 셉션에서 셸(Shell)에서 실행하는 방법 - ! 이용하기(ex- !pip install pandas)

#### 매직커맨드(%,%%)

```python
# %, %% (매직 커맨드)
# IPython/Jupyter에서 제공하는 특수 명령어
# '%'는 한 줄에 대해, '%%'는 셀 전체에 대해 적용됩니다.
# 코드 실행 환경을 제어하거나, 편리 기능을 제공합니다.


```
%pwd, %time 등이 존재한다 <- 사실상 %time만 유용하게 사용할듯?

```python
# 단일 매직커맨드 사용예시

%time print('hello')

''' 출력결과
hello
CPU times: user 371 µs, sys: 0 ns, total: 371 µs # cpu가 돌아간 시간
Wall time: 319 µs # 코드가 돌아간 시간(나노세컨드)
'''
```

```python
# 다중 매직커맨드 사용예시
%%time
print('hello')
print('hello')
# %time과 출력에 대한 정보결과는 동일하다
```

#### 이스케이핑 문자
\로 감싼 문자는 텍스트 데이터로 간주됨

1. 원래의 기능을 하는 기호 혹은 무언가를 텍스트자체로 보여주게끔 변환해주는 기능

2. 원래 문자를 특수기능을 하게끔 변환해주는 기능( ex - \t, \n 등)

```python
# 1. 원래의 기능을 하는 기호 혹은 무언가를 텍스트자체로 보여주게끔 하는 기능
print('That\'s a dog.') 
print("\"안녕하세요\"")

'''출력결과
That's a dog.
"안녕하세요"
'''
```
<-print("That's a dog.") 대신 사용가능하지만 보통 안 써서 코드 해석할 수 있기만 하면 됨

```python
# 2. 원래 문자를 특수기능을 하게끔 변환해주는 기능

print("Hello World!\nThat's a dog\tThat's a dog")

'''출력결과
That's a dog.
"안녕하세요"
'''
```




#### 깨알지식모음

##### 여러 줄의 코드를 한 줄에 작성하여 실행하기

```python
print("Hello World!"); print("That's a dog"); print("That's a dog") # 여러줄의 코드를 부득이하게 한줄에 넣을 때 ;으로 각 명령어의 마침표

'''출력결과
Hello World!
That's a dog
That's a dog
'''
```


##### 보이는 그대로와 유사하게 출력하기

```python

print('''Hello World!
That's a dog
That's a dog''')

'''출력결과
Hello World!
That's a dog
That's a dog
'''
```

##### 내장함수를 변수명으로 사용하게되는 경우 대처 <- del

```python
# 내장함수를 변수명으로 사용하여 초기화해버리면 함수로 사용할 수는 없게됨

print(list())
list=1
print(list)
print(list())

'''출력결과
[]
1
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
/tmp/ipython-input-4-3518667268.py in <cell line: 0>()
      3 list=1
      4 print(list)
----> 5 print(list())

TypeError: 'int' object is not callable
'''
```


