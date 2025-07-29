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

#### Docker 개요

- 커널이란?
  - 운영체제 중 항상 메모리에 올라가 있는 운영체제의 핵심 부분
  - 하드웨어와 응용 프로그램 사이에서 인터페이스를 제공하는 역할
  - 컴퓨터 자원들을 관리

- 컨테이너란? 
  - 다른 프로세스와 격리된 상태로 OS에서 SW를 실행하는  기술


  
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



docker run --name nginx_test -d -p 8001:80 public.ecr.aws/nginx/nginx:stable-perl

docker run --name nginx_test4 -d -p 8003:80 -v C:\ITStudy\03_docker\mount:/usr/share/nginx/html/ public.ecr.aws/nginx/nginx:stable-perl

/usr/share/nginx/html/
index.html

docker exec -it -e LC_ALL=C.UTF-8 nginx_test4 bash

도커는 컴퓨터 서버용이기 때문에 사람이 쓰기 위한 패키지는 안 깔려있다
-> 도커 서버 안에서 사용하려면 apt-get update 해서 패키지 설치할 기본 라이브러리부터 가져오기
-> apt-get install vim(apt-get install ~ 사용가능)

vim 파일하고서 수정 시 insert 누르고 글쓰기 가능, 끝나면 esc 누르고 :wq 작성하면 반영됨(write and quit)


#### docker hub 이미지들 확인 및 pull하기
- docker hub에서 찾을 수 있는 이미지들을 docker search 명령어를 통해서도 확인할 수 있다
ex- docker search jupyter

- 가져오는 방법은 docker pull 이미지풀네임
ex- docker pull jupyter/datascience-notebook

- 이미지는 실행 전 상태(설계도)이고, 컨테이너는 그 이미지를 기반으로 실행된 실체(실행 중 또는 중지된 프로그램)


docker history 이미지id 를 통해 내부적으로 어떻게 구성돼있는지 알 수 있다( 어디랑 마운트돼있는지, 포트는 어디인지 등등?)

docker run --name jupyter -d -p 8888:8888 -v C:\ITStudy\03_docker\mount:/home/jovyan jupyter/base-notebook

docker history 컨테이너id일부 로 하면, localhost:8888로 접속을 할 때 토큰정보를 확인할 수 있다

docker rmi jupyter/datascience-notebook
ㄴ 이미지를 삭제할 때는 docker rmi


$ docker exec docker-nginx sh -c 'cat /usr/share/nginx/html/index.html'   # 컨테이너 안에 들어가지 않고 컨테이너에게 명령을 내립니다.

```
$docker ps -a
$docker image ls
$docker pull nginx
$docker ps
$docker image ls
$docker history <image풀네임>
$docker run --name docker-nginx -p 80:80 public.ecr.aws/nginx/nginx:stable-perl
$docker ps -a
$docker exec <container_id> sh -c 'curl localhost'
$ docker exec docker-nginx sh -c 'cat /usr/share/nginx/html/index.html'   # 컨테이너 안에 들어가지 않고 컨테이너에게 명령을 내립니다.
$docker stop <container_id>
$docker start<container_id>
$docker ps -a
$docker container ls
$docker rm <container_id>
$docker container ls
$docker image ls
$docker image rm <imagee_id> or docker rmi <image명>
$docker image ls

docker run --name nginx_test -d -p 8001:80 public.ecr.aws/nginx/nginx:stable-perl

# 윈도우의 경로구분은 \, 리눅스의 경로구분은 /로 진행한다
docker run --name nginx_test4 -d -p 8003:80 -v C:\ITStudy\03_docker\mount:/usr/share/nginx/html/ public.ecr.aws/nginx/nginx:stable-perl

/usr/share/nginx/html/index.html

docker exec -it -e LC_ALL=C.UTF-8 nginx_test4 bash  
```


docker cp index.html 컨테이너이름:컨테이너경로/파일명 
-> 끝에 파일명까지 보내면 index.html에 대한 내용을 파일명으로 적은 이름으로 바뀌어서 컨테이너경로에 보내져진다

docker cp docker-nginx:/usr/share/nginx/html/ C:\ITStudy\03_docker\copy
ㄴ 폴더 vs 폴더


docker cp docker-nginx:/usr/share/nginx/html/ .
ㄴ 도커디렉토리를 현재 cmd 경로에 가져온다

#### 마운트란?




docker tag public.ecr.aws/nginx/nginx:stable-perl nginx
ㄴ 기존 image id image name을 nginx로 바꿔서 복사


# 전체 경로를 bindmount해서 이미지도 보임
$docker run --name mynginxserver -d -p 81:80 -v C:\ITStudy\03_docker\bindStorage:/usr/share/nginx/html nginx

# index.html만 bindmount해서 이미지는 안보임 
$docker run --name mynginxserver1 -d -p 82:80 -v C:\ITStudy\03_docker\bindStorage\index.html:/usr/share/nginx/html/index.html nginx

ㄴ 이미지 파일이 동봉돼있음

볼륨은 도커가 사용하기 위한 마운트


\\wsl.localhost\docker-desktop\mnt\docker-desktop-disk\data\docker\volumesㄴ 볼륨이 생성되는 디렉토리(되게 꼭꼭 숨어져있음)



# 볼륨 생성
docker volume create 볼륨명

# 볼륨 확인
docker volume ls

# 경로 확인
docker volume inspect 볼륨명 

# 볼륨 삭제
docker volume rm 볼륨명

# 바인드마운트
docker run -v 스토리지실제경로:컨테이너마운트경로

# 볼륨마운트
docker run -v 볼륨명:컨테이너마운트경로
docker run -it -p 83:80 -v 볼륨명:/usr/share/nginx/html nginx

# 제거하려는 볼륨이 마운트되어 있는 컨테이너가 있을 때는 해당 볼륨이 제거가 되지가 않습니다.
# 그럴 때는 해당 볼륨이 마운트되어 있는 모든 컨테이너를 먼저 삭제하고, 볼륨을 삭제해야 합니다.

# 볼륨 청소
docker volume prune
\\wsl.localhost\docker-desktop\mnt\docker-desktop-disk\data\docker\volumes

ㄴ 볼륨에 백업이 되고 가져다쓰면된다?

사용하고 있는 컨테이너를 먼저 삭제해야 볼륨도 삭제할 수 있다



##### 볼륨마운트와 바인드마운트의 차이
- 마운트 자체는 컨테이너가 종료되면 초기화되는 현상을 방지하기 위해 변경사항같은 것을 저장하기 위함?
- 바인드마운트는 로컬에서 마운트할 디렉토리 혹은 파일을 정의( 컨테이너랑 따로 구분되어있다? )
- 볼륨마운트는 localhost 서버에 임시 디렉토리를 생성해서 거기에 컨테이너의 디렉토리 혹은 파일을 마운트한다

| 항목           | **바인드 마운트**                | **볼륨 마운트**                     |
| ------------ | -------------------------- | ------------------------------ |
| 정의 위치        | **호스트의 특정 디렉토리**           | Docker가 관리하는 **익명 or 명명된 저장소** |
| 설정 방법        | `-v /호스트경로:/컨테이너경로`        | `-v 볼륨이름:/컨테이너경로`              |
| 데이터 보존       | 호스트 경로가 삭제되면 데이터도 사라짐      | 도커가 따로 관리하므로 안전하게 보존           |
| 퍼미션 문제       | **OS 보안 정책 따라 제한 있을 수 있음** | Docker가 내부적으로 권한 관리함           |
| 이식성 (이동 가능성) | 호스트 경로가 다르면 깨짐             | 어떤 시스템이든 Docker만 있으면 재사용 가능    |
| 백업/복원        | 수동 작업 필요                   | `docker volume` 명령어로 쉽게 가능     |
| 개발 편의성       | 소스코드 실시간 반영 등 **매우 편리**    | 실시간 변경 불가 (볼륨 내부는 독립된 공간)      |
| 성능 (리눅스 기준)  | 약간 느릴 수 있음                 | Docker 전용 드라이버로 더 빠름           |



### 나만의 이미지공유저장소만들기

이미지 공유 저장소 :  http://hub.docker.com/아이디/레포지토리명:버전명

#### 이미지만드는 방법
1. $docker commit  컨테이너이름  새로운이미지이름




docker run -p 8111:80 --rm atangi/mountcheck:1.1

80 : 컨테이너 안에 작업한 index.html을 그대로 포함함 
81 : 컨테이너 밖에 mount한 index.html / 나머지 파일들 -> mount한 파일은 commit시 적용되지 않습니다.


### dockerfile


<aside>
☘️ **Docker File  주요 구성 요소들 의미**

1. **FROM** : Base Image 지정(OS 및 버전 명시, Base Image에서 시작해서 커스텀 이미지를 추가)
2. **RUN** : shell command를 해당 docker image에 실행시킬 때 사용함
3. **WORKDIR** : Docker File에 있는 RUN, CMD, ENTRYPOINT, COPY, ADD 등의 지시를 수행할 곳
4. **EXPOSE** : 호스트와 연결할 포트 번호를 지정
5. **CMD** : application을 실행하기 위한 명령어
</aside>

ㄴ from 은 사용할 원본이미지(여기에다가 오버레이해주는거임)
ㄴ copy . /app - 현재 경로에서의 모든 파일을 app이라는 디렉토리에 넣어줘
WORKDIR – Dockerfile 전용 명령어 (작업 디렉토리 설정)

-t 는 태그(이름 붙일거야)

## streamlit을 도커 이미지로 작성해서 외부에서 배포한다면?
FROM 파이썬이미지(리눅스에 파이썬을 깔아놓은 상태)
COPY requirements.txt /app 
COPY 스트림릿 파일들  /app
ㄴ/app은 지저분하게 보이지 않게하기 위한 것?


WORKDIR /app
RUN pip install -r requirements.txt

CMD ["streamlit", "run", "app.py"]

EXPOSE 스트림릿이 배포되는 포트명 



##### 예시 

PS C:\ITStudy\03_docker\st-project> docker build -t kenkang9/st-app:1.0 .
PS C:\ITStudy\03_docker\st-project> docker images
REPOSITORY                   TAG           IMAGE ID       CREATED              SIZE
kenkang9/st-app              1.0           d18379c10a55   About a minute ago   2.7GB
PS C:\ITStudy\03_docker\st-project> docker history d183
IMAGE          CREATED          CREATED BY                                       SIZE      COMMENT
d18379c10a55   2 minutes ago    EXPOSE map[8501/tcp:{}]                          0B        buildkit.dockerfile.v0  
<missing>      2 minutes ago    CMD ["streamlit" "run" "app.py"]                 0B        buildkit.dockerfile.v0  
<missing>      2 minutes ago    RUN /bin/sh -c pip install -r requirements.t…   852MB     buildkit.dockerfile.v0   
<missing>      2 minutes ago    COPY . /app # buildkit                           25.9MB    buildkit.dockerfile.v0  
<missing>      20 minutes ago   COPY requirements.txt /app # buildkit            12.3kB    buildkit.dockerfile.v0  
<missing>      20 minutes ago   WORKDIR /app                                     8.19kB    buildkit.dockerfile.v0  
<missing>      7 weeks ago      CMD ["python3"]                                  0B        buildkit.dockerfile.v0  
<missing>      7 weeks ago      RUN /bin/sh -c set -eux;  for src in idle3 p…   16.4kB    buildkit.dockerfile.v0   
<missing>      7 weeks ago      RUN /bin/sh -c set -eux;   wget -O python.ta…   72.9MB    buildkit.dockerfile.v0   
<missing>      7 weeks ago      ENV PYTHON_SHA256=c30bb24b7f1e9a19b11b55a546…   0B        buildkit.dockerfile.v0   
<missing>      7 weeks ago      ENV PYTHON_VERSION=3.12.11                       0B        buildkit.dockerfile.v0  
<missing>      7 weeks ago      ENV GPG_KEY=7169605F62C751356D054A26A821E680…   0B        buildkit.dockerfile.v0   
<missing>      7 weeks ago      RUN /bin/sh -c set -eux;  apt-get update;  a…   19.7MB    buildkit.dockerfile.v0   
<missing>      7 weeks ago      ENV LANG=C.UTF-8                                 0B        buildkit.dockerfile.v0  
<missing>      7 weeks ago      ENV PATH=/usr/local/bin:/usr/local/sbin:/usr…   0B        buildkit.dockerfile.v0   
<missing>      18 months ago    RUN /bin/sh -c set -ex;  apt-get update;  ap…   619MB     buildkit.dockerfile.v0   
<missing>      18 months ago    RUN /bin/sh -c set -eux;  apt-get update;  a…   194MB     buildkit.dockerfile.v0   
<missing>      2 years ago      RUN /bin/sh -c set -eux;  apt-get update;  a…   52.2MB    buildkit.dockerfile.v0   
<missing>      2 years ago      # debian.sh --arch 'amd64' out/ 'bookworm' '…   133MB     debuerreotype 0.15   

- python 3.12에 대한 dockerfile의 각 줄이 아래부터 우선적으로 실행이 되고(from절) 그 다음에 우리가 새로 작성한 도커파일이 시작된다

PS C:\ITStudy\03_docker\st-project> docker run -p 10000:8501 -d kenkang9/st-app:1.0
429ce06d6b702c7f1ee045dc32d37ac83e8c5d0ef36cb9a97afe127a0f1b2a2a
PS C:\ITStudy\03_docker\st-project> docker push kenkang9/st-app:1.0    

이때, dockerhub 계정명이랑 틀리면 에러가 난다 -> tag로 이름만 바꾸면 됨
docker tag kenkang9/st-app:1.0 kenkang99/st-app:1.0






## ECR Repo(AWS)에서 가져온 이미지로 docker hub에 push

최초 딱 한번만 아래대로 적용해주세요

- aws ECR Repo는 os 정보를 가지고 있지 않은 이미지들이 종종 있습니다.

```sql
FROM public.ecr.aws/nginx/nginx:stable-perl  # 컨테이너명만 작성한 Dockerfile 작성
```

```sql
# 멀티 플랫폼 빌드를 위해 `buildx` 빌더를 생성하고 활성화하는 명령어
$ docker buildx create --use  
$ docker buildx build --platform linux/amd64,linux/arm64 -t atangi/mountcheck:0.0 --push .
```

render.com - 내 이미지를 배포할 수 있는 사이트