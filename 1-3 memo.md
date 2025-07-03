# 1주차
## 3일차
### 제어문(조건문과 반복문)

```python
input("어깨를 돌리셨습니까?") 
if 'Y':
    print("좋군요")
else: 
    print("돌려라")
```
-> 변수가 남아서 그대로 사용되는구나 

if 블록이랑 else 블록 두개만 있을 때는 한곳으로 반드시 분기가 되어야 합니다.
-> 조건문블록에서 실행이 모두 안되는 경우는 없다

```python
a = input()

is_in_arr = False # 스위치 역할 - False로 선언해놓고 시작하는 것이 권장됩니다.
# flag 변수 - 여러개의 조건문들이 하나의 상황에 의해서 함께 제어되어야 하는 경우
#

for i in arr:
    if a == i:
        is_in_arr = True
    else:
        is_in_arr = False

if is_in_arr: # if 절 뒤의 명제는 bool()을 통해 True / False로 치환되므로
    print('값이 있습니다.')
else:
    print('찾으시는 값이 없습니다.')
```
스위치를 이용하여 활성화를 시키는 방법을 사용할 수도 있다
-> 스위치 초기값은 False 가 더 안전하다

#### 삼항조건문 사용예시
```python
# 삼항조건문 사용예시
a = input("어깨를 돌리셨습니까?")

print("좋군요") if a == "Y" else print("한번 돌리시지 그래요?")
```

```python
a, b = map(int, input("주사위 번호 두 개를 입력하세요(예: a,b): ").split(","))
print("a가 이긴다" if (a > b) else "비긴다" if (a == b) else "b가 이긴다")
```

#### 논리연산자와 비트연산자 참고

```python
# 논리연산자 계산 우선순위에 의한 오류 예시
arr = [3, 2, 5]

# 괄호가 없으면 3, 2, 5 in arr 간의 확인이라 3에서 True 판단되고 조건문은 항상 True가 됨
if (3 or 2 or 5) in arr:
    print('참')
else:
    print('거짓')
```

비트연산자 : | &
-> 비트별로 연산을 한다



- not, and, or 순으로 우선 판단
- 단락평가를 한다
- 단락평가란? short-circuit evalution
- 첫번째 값으로 결과가 확인하면 두번째 값은 평가하지 않는 방법
- 참고:
    - ~ & | : 비트연산자 - 비트 단위로 논리 연산 (not, and, or 과 다른 방식으로 작동)

    - 비트 연산자는 비트(bit) 단위로 논리 연산을 할 때 사용하는 연산자입니다.

    - 또한, 비트 단위로 전체 비트를 왼쪽이나 오른쪽으로 이동시킬 때도 사용합니다.
```
&	대응되는 비트가 모두 1이면 1을 반환함. (비트 AND 연산)
|	대응되는 비트 중에서 하나라도 1이면 1을 반환함. (비트 OR 연산)
^	대응되는 비트가 서로 다르면 1을 반환함. (비트 XOR 연산)
~	비트를 1이면 0으로, 0이면 1로 반전시킴. (비트 NOT 연산)


연산자 우선순위
- https://dojang.io/mod/page/view.php?id=2461
```

```python
# 비트연산자에 의한 계산오류 예시
arr = [3, 2, 5]


if 3|2|5 in arr:
    print('참')
else:
    print('거짓')

# 위 연산의 경우 내부방식의 예시

# 4 2 1
# -----
# 0 1 1    = 2+1 = 3
# 0 1 0    =       2
# 1 0 1    = 4+1   5
# --------
# 1 1 1     4+2+1 7
```
비트연산자에서의 3|2|5 는 111로 7의 값을 갖게된다
! -> 의도대로 사용하려면 웬만하면 비트연산자 대신 or, and 등의 논리연산자를 사용하자



#### 문자열이 '숫자'로만 이루어져있는지 확인 - str.isdigit(), str.isdecimal(), str.isnumeric(), str.isalpha()
1. str.isdigit("판단하고자 하는 문자열")
2. "판단하고자 하는 문자열".isdigit()

```python
a = '1234' # 타입이 str이지만 온전히 숫자만 가지고 있다면 True를 반환
b = '1/2'
c = '가나다'
d = '가나다123'
e = '3²'
f = '-1'
g = '½'

print('isdigit()') # 문자열의 모든 문자가 숫자(0-9)이면 True를 출력
print(a.isdigit()) # True
print(b.isdigit()) # False
print(c.isdigit()) # False
print(d.isdigit()) # False
print(e.isdigit()) # True
print(f.isdigit()) # False '-1' 은 '-'가 음수이기 때문에
print(g.isdigit()) # False

print('isdecimal() - 특수문자는 숫자로 취급하지 않음')
print(a.isdecimal()) # True
print(b.isdecimal()) # False
print(c.isdecimal()) # False
print(d.isdecimal()) # False
print(e.isdecimal()) # False
print(f.isdecimal()) # False
print(g.isdecimal()) # False

print('isnumeric() -  문자열의 모든 문자가 숫자(분수, 아래 첨자, 위 첨자 등의 다른 숫자도 포함)')
print(a.isnumeric()) # True
print(b.isnumeric()) # False
print(c.isnumeric()) # False
print(d.isnumeric()) # False
print(e.isnumeric()) # True
print(f.isnumeric()) # False
print(g.isnumeric()) # True

print('isalpha() - 알파벳으로만 이루어진 문자열 판별')
print(a.isalpha()) # False
print(b.isalpha()) # False
print(c.isalpha()) # True
print(d.isalpha()) # False
print(e.isalpha()) # False
print(f.isalpha()) # False
print(g.isalpha()) # False
```

invalid syntax는 처음부터 실행이 안되는가
ㄴ 문법에러는 컴파일러 이전에 파이썬 자체에서의 검수?를 하기 때문에 컴파일 전에 종료됨
ㄴ 나머지는 컴파일에러임