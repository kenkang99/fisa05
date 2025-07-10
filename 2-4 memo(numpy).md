# 2주차
## 4일차

### numpy, pandas

#### numpy

## 1. 넘파이 NumPy 란?
##### Numpy: Numeric + Python의 약자, 수학 및 과학 연산 라이브러리
<aside>
💡 Numerical Python의 줄임말로 Python에서 벡터, 행렬 등 수치 연산을 수행하는 선형대수(Linear algebra) 라이브러리입니다.  속에 아주 많은 선형대수 연산 관련 함수, 변수 들을 가지고 있습니다.

+ 선형대수 관련 수치 연산을 지원하고 **내부적으로는 C로 구현**되어 있어 연산이 빠른 속도로 수행됩니다.
</aside>

- 배열이나 행렬 계산에 필요한 함수 제공
- 수열 데이터를 다룰 때 용이, 이후에 Pandas에서 DataFrame 형태로 사용함
- 다차원 배열(Array)을 다룰 때 주로 사용함 (인공 신경망, 비정형 데이터 처리, 자연어 처리 등)
- 코어 부분이 C로 구현되어 동일한 연산을 하더라도 Python에 비해 속도가 빠름
- 라이브러리에 구현되어있는 함수들을 활용해 짧고 간결한 코드 작성 가능

<aside>
💡 *차원* 

- (수학) 공간 내에 있는 점 등의 위치를 나타내기 위해 필요한 축의 개수
- (데이터) 값이 측정된 기준
ex.  국가별 남성의 평균 수명 데이터는 실수 형태의 평균 나이를 값으로 가지며, 
       성별과 국가라는 두 개의 차원을 가진다.

</aside>


#### 잡다지식

import numpy as np # np는 numpy를 의미하는 게 약속처럼 되어있습니다.

#from numpy import * -> 이 경우 파이썬의 기본 내장함수와 겹치는 이름들을 numpy가 덮어쓰기 때문에 권장하지는 않습니다

<- array([1,2,3,4,5]) 이런 식으로 써야하는데 이건 파이선 기본 내장함수과 이름이 겹친다

type(test)  # ndarray 가 소문자인 이유 -개발자가 직접 만든 게 아니라 패키지에서 가져온 '만들어져 있는' 자료형이므로 ( list, dict 등등 모두 이와 같은 이유다)

넘파이에서의 append는 메모리공간을 이어서 하나 추가하는게 아니라 아예 새로 값이 return되는거라 하나의 추가하는 개념이 아님

방 하나씩 차지해서 값을 갖고 있다가 사용해주는 형식이기 때문에 방안의 자료형과 같은 자료형으로 연산시 다른 자료형과 연산도 가능하다
ex- 
np.array([1])+1 # 2
[23]+5 # TypeError: can only concatenate list (not "int") to list

stride 개념으로 원소를 자르기 때문에 형태가 안 맞으면 ndarray를 생성하지 못한다

test3 = np.append(test2, [None,1,1]) 
test3 # dtype=object는 각 요소별로 자료형이 섞여있는 것이다( 방마다 다른 자료형이 들어있는 것이다. ) <- list일 때도 이러므로 ndarray만의 장점이 없다 이렇게되면
ex-a= array([1, 2, 3, 4, 11, 12, 13, 14, None, 1, 1], dtype=object) <- a+5 가 안 먹힘
-> 그래서 None 대신 np.nan을 쓰게됨 ( nan : not a number)

pytorch는 float16를 기본으로 돌아간다

a.astype('uint') # unsigned int : 양수만을 담는 자료형

a.reshape((-1,3,1)) # 면 행 렬, -1로 채운 곳에는 면에서 요소의 부족한 개수만큼 알아서 맞춰준다는 뜻 -> 여기서는 그럼 2로 되겠네요

numpy에는 최빈값구하기는 없음 -> pandas를 사용 혹은 st.mode 이용하기

list든 ndarray든 슬라이싱은 index out of range가 발생하지 않는다

numpy에서는 c언어 기반이라 다음과 같이 비교연산자가 적용된다 ~(not) &(and) |(or) 

브로드캐스팅 - shape이 달라도 서로 산술 연산이 가능케 하는 메커니즘, 대표 라이브러리로는 numpy,pandas 등이 있음