자바기반, json 형태로 데이터를 그림


gapminder.describe(include='all').T
<- 행이 많고 칼럼이 적을 때 보기 편할수도

```python
# 퍼센트(%)와 퍼센트포인트(%p)의 차이
# 퍼센트(%)는 상대적인 크기, 퍼센트포인트(%p)는 절대적인 크기를 나타냄

before = 1000 # 1000%
end = 1100 # 1100%

end - before

# 몇 퍼센트, 몇 퍼센트포인트 오른걸까?
percent_point = end - before
percent = (end - before) / before *100

print(f'퍼센트포인트 : {percent_point}')
print(f'퍼센트 : {percent}')

import math

# 로그스케일로 값 변화 : '배수'로 값을 표현 ( ex-  60 -> 70이 됐건 600->700이 됐건 같은 값을 가짐)
log_change = math.log(end) - math.log(before)
log_change

```
로그스케일을 사용하는 이유?

box plot + 분포 -> violin plot
