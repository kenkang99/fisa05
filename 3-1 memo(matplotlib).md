matplot은 판다스 기반으로 실행된다<- 근데 df로 넣으면 안되고 numpy형태로 넣어야한다?
-> seaborn은 df 형태를 지원한다

matplot workflow
도화지 객체생성 -> 축 객체 생성 -> 좌표들을 채운다

seaborn은 matplotlib 기반


# category : 명목형 변수들은 순서대로 늘어놓기 위해 만들어진 판다스의 특별 자료형
# 우리눈에는 문자열처럼 보이지만 내부적으로는 Female :0, Male :1 로 넘버링해서 보관 - 메모리 덜 차지, 속도도 빠름
# 유니코드 정렬이 아니라 순서를 부여해야 하는 문자열 데이터에게 순위를 부여해서 관리
tips.info()


```python
import warnings

warnings.filterwarnings('ignore')
```
경고표시출력생략


```python
from ydata_profiling import ProfileReport

profile=ProfileReport(tips,title='tips analysis')
profile
```
시각화 report 분석 라이브러리(EDA 총집합) - ydata_profiling의 ProfileReport



