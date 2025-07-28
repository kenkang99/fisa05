### pymysql 라이브러리

```python

  1. 모듈 import
  2. pymysql.connect() 메소드로 MySQL에 연결
     - 호스트명, 포트, 로그인, 암호, 접속할 DB 등을 파라미터로 지정
  3. MySQL 접속이 성공하면, Connection 객체로부터 cursor() 메서드를 호출하여 Cursor 객체를 가져옴
  4. Cursor 객체의 execute() 메서드를 사용하여 SQL 문장을 DB 서버에 전송
  5. SQL 쿼리의 경우 Cursor 객체의 fetchall(), fetchone(), fetchmany() 등의 메서드를 사용하여 서버로부터 가져온 데이타를 코드에서 활용
  6. 삽입, 갱신, 삭제 등의 DML(Data Manipulation Language) 문장을 실행하는 경우, INSERT/UPDATE/DELETE 후 Connection 객체의 commit() 메서드를 사용하여 데이타를 확정
  7. Connection 객체의 close() 메서드를 사용하여 DB 연결을 닫음

```

```python
# yaml 형태로 db 연결 정보 작성
%%writefile db.yaml
HOST: 'localhost'
USER: 'root'
PASSWD: 'Woorifisa5!'
```

```python
# yaml 형태의 db 연결 정보 불러오기
!pip install pyyaml

import yaml

DB_INFO = "db.yaml"
with open(DB_INFO,"r") as f:
    db_info = yaml.load(f, Loader=yaml.Loader)
```

```python

HOST = db_info["HOST"]
USER = db_info["USER"]
PASSWD = db_info["PASSWD"]
PORT = 3306

import pymysql
import pandas as pd
pd.options.display.float_format = '{:.2f}'.format

connection = pymysql.connect(
    host=HOST,     # MySQL Server Address
    port=PORT,          # MySQL Server Port
    user=USER,      # MySQL username
    passwd=PASSWD,    # password for MySQL username
    db='fisa',   # Database name
    charset='utf8mb4'
)

```

##### d
```python
# 1. 모듈을 불러옵니다.
import pymysql
from pymysql.connections import CLIENT
import pandas as pd
pd.options.display.float_format = '{:.2f}'.format


# 2. pymysql한테 3306 포트번호와 접속할 ID, PW를 준다
connection = pymysql.connect(
    host=HOST,     # MySQL Server Address
    port=PORT,          # MySQL Server Port
    user=USER,      # MySQL username
    passwd=PASSWD,    # password for MySQL username
    db='fisa',   # Database name
    charset='utf8mb4' , client_flag=CLIENT.MULTI_STATEMENTS # 여러 쿼리를 한번에 세미콜론으로 보낼 수 있도록 client_flag를 추가하는 방법
)

# 3. 대신 일하게 만들 커서를 만듭니다.
cursor = connection.cursor()
# dir(cursor)
# 4. 실행할 SQL문을 넘깁니다. - 옵션을 바꿔서 접속하면 여러 문장을 한번에 넘기는구나

# ALLEN 의 급여를 1000불 올리고, emp를 가지고 오는 방식으로 사용 
SQL1 = "SELECT * FROM emp"
SQL2 = "UPDATE emp SET sal=sal+1000 WHERE ename='ALLEN'; SELECT * FROM emp; "
# SELECT * FROM emp;
cursor.execute(SQL1) 
cursor.execute(SQL2) 

result = cursor.fetchall() # 전부 다 한꺼번에 가져옴 

# 5. DB에 현재 상태를 COMMIT 합니다.
connection.commit()

# 6. DB와 연결을 닫습니다.
connection.close()

result # 확인해보니 업데이트는 되는데 왜 result에 아무것도 없을까? -> 여러개의 실행문이 실행이 되긴 하지만 결과는 첫 번째 쿼리만의 결과를 저장해서 빈 리스트가 result에 저장됨
```


### docker

1. **docker run**: 컨테이너를 실행합니다.
예시: docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
    - `d`: 백그라운드에서 컨테이너를 실행합니다.
    - `it`: 인터랙티브 모드로 컨테이너를 실행하며 터미널 입력을 받을 수 있게 합니다.
    - `--name <name>`: 컨테이너에 이름을 지정합니다.
    - `p <host_port>:<container_port>`: 호스트의 포트(외부)를 컨테이너의 포트(내부)에 매핑합니다.
    - `v <host_dir>:<container_dir>`: 호스트 디렉토리를 컨테이너 디렉토리에 마운트합니다.
    - `-rm`: 컨테이너가 종료되면 자동으로 삭제합니다.
    
  # -한글자 (short option) -n -rm
  # --원래단어 (long option) --name --remove 

-a 는 all의 약어
-n 은 name의 약어
-rm 은 remove의 약어
-f 는 force의 약어

docker ps -a를 할 때 stop한 컨테이너가 있으면 그건 뜨지않음

2. 컨테이너 삭제
container id 혹은 container name로 삭제 가능
docker rm 'container id' <- 컨테이너가 현재 실행 중이면 삭제가 안됨 - container id는 앞에 몇 자만 적고 해도 삭제되지만 name은 풀네임으로 작성해줘야함(탭키 이용할 것)
ㄴ docker stop 'container id' 이후에 실행해야함 

2-1. 컨테이너 강제 삭제(실행 중임에도 삭제)
docker rm --force nginx_test

3. container 이름 명명 후 run
docker run --name nginx_test public.ecr.aws/nginx/nginx:stable-perl

4. 포트번호 로컬과 연결
 docker run --name nginx_test -d -p 8001:80 public.ecr.aws/nginx/nginx:stable-perl

결과 
CONTAINER ID   IMAGE                                    COMMAND                  CREATED         STATUS         PORTS                                     NAMES
2937493a19fe   public.ecr.aws/nginx/nginx:stable-perl   "/docker-entrypoint.…"   9 seconds ago   Up 9 seconds   0.0.0.0:8001->80/tcp, [::]:8001->80/tcp   nginx_test



충돌 일어났을 때 실행은 안됐지만 생성까지만 된 경우는 status에 created로 돼있음
 docker ps -a
CONTAINER ID   IMAGE                                    COMMAND                  CREATED          STATUS          PORTS                                     NAMES
762386f566c6   public.ecr.aws/nginx/nginx:stable-perl   "/docker-entrypoint.…"   6 seconds ago    Created                                                   nginx_test3
403f4fb22090   public.ecr.aws/nginx/nginx:stable-perl   "/docker-entrypoint.…"   50 seconds ago   Up 50 seconds   0.0.0.0:8002->80/tcp, [::]:8002->80/tcp   nginx_test2
c0338fc9c04a   public.ecr.aws/nginx/nginx:stable-perl   "/docker-entrypoint.…"   2 minutes ago    Up 2 minutes    80/tcp                                    nginx_test1
2937493a19fe   public.ecr.aws/nginx/nginx:stable-perl   "/docker-entrypoint.…"   3 minutes ago    Up 3 minutes    0.0.0.0:8001->80/tcp, [::]:8001->80/tcp   nginx_test


docker run --name nginx_test4 -d -p 8003:80 -v C:\ITStudy\03_docker\mount:/usr/share/nginx/html/ public.ecr.aws/nginx/nginx:stable-perl
ㄴ 로컬의 파일과 컨테이너와 연결하기

docker run --name nginx_test4 -d -p 8003:80 --rm -v C:\ITStudy\03_docker\mount:/usr/share/nginx/html/ public.ecr.aws/nginx/nginx:stable-perl
ㄴ 삭제하기


docker exec -it nginx_test4 bash

-it와 -dit는 보안을 위한 기능을 하는 것?

 docker exec -it -e LC_ALL=C.UTF-8 nginx_test4 bash
ㄴ nginx_test4를 bash에서 실행할 때 이제는 한국어가 작성됨 