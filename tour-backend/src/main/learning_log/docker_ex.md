✅ 실패한 명령어들과 설명
### docker rm vibrant_franklin
역할: 이름이 vibrant_franklin인 컨테이너를 삭제합니다.

실패 이유: 아마 해당 이름의 컨테이너가 존재하지 않거나, 실행 중이었기 때문일 수 있어요.

삭제하려면 먼저 docker stop vibrant_franklin으로 중지한 뒤 삭제해야 해요.

### docker logs -f tour-backend
역할: tour-backend 컨테이너의 로그를 실시간 출력합니다.

실패 이유: 해당 컨테이너가 실행 중이 아니면 로그가 없거나 에러 발생.

이전에 봤던 "container is not running" 상태라면 로그가 아예 없었을 수 있어요.

### docker exec -it tour-backend sh
역할: tour-backend 컨테이너에 셸 접속 시도

실패 이유: 컨테이너가 꺼져 있었거나 (Exited) 아예 없었던 경우입니다.

### ping tourdb
역할: DNS 이름 tourdb가 같은 네트워크에서 인식되는지 확인

실패 이유: tourdb 컨테이너가 같은 네트워크에 연결되지 않았거나, 컨테이너가 꺼져 있었을 가능성이 큽니다.

### docker network ls
역할: 현재 Docker에 존재하는 네트워크 목록 조회

✅ 정상 작동했지만, 단순 조회 명령입니다.

### docker network inspect tour-network
역할: tour-network 내부에 어떤 컨테이너들이 연결되어 있는지, 설정을 보여줍니다.

✅ 정상 실행되었으나 "Containers": {}로 나왔던 게 문제였습니다.

### docker network connect tour-network tour-backend
docker network connect tour-network tourdb
역할: 해당 컨테이너를 tour-network에 강제로 연결

실패 이유: 컨테이너가 존재하지 않거나, 이미 연결된 경우, 또는 컨테이너가 정지 상태이면 실패할 수 있어요.

#### docker restart tour-backend
docker restart tourdb
역할: 해당 컨테이너를 재시작

실패 이유: 컨테이너가 존재하지 않거나, 내부적으로 에러로 바로 꺼지는 상태였을 수 있어요.

### docker logs tour-backend
역할: tour-backend 로그를 확인

실패 이유: 컨테이너가 아예 실행되지 않았거나, 에러 발생 직후라 로그가 짧았을 수도 있어요.





✅ 성공한 명령어들과 설명
### docker rm tour-backend
역할: 기존에 존재하던 tour-backend 컨테이너를 삭제

✅ 성공한 이유: 이전에 컨테이너가 꺼져 있었기 때문에 삭제 가능

### docker build -t tour-backend .
역할: 현재 디렉토리의 Dockerfile을 기반으로 이미지 생성

✅ 정상적으로 이미지가 잘 빌드됨

### docker run -d --name tour-backend --network tour-network -p 8080:8080 tour-backend
역할: 새 컨테이너를 tour-network에 연결하고, 8080 포트를 외부에 노출해서 실행

✅ 정상적인 실행 방식입니다. 이게 핵심 성공 포인트였어요!

### docker run tour-backend
역할: 빌드된 이미지로 컨테이너를 실행

⚠️ 주의: 이 명령은 포트도 안 열고, 네트워크도 안 붙이기 때문에 기능 제한 있음. 아마 실행은 되지만 통신 안 됐을 수도 있음

### docker run -d -p 8080:8080 --name tourapp --link tourdb:mariadb tour-backend
역할: tourdb 컨테이너와 --link로 연결해서 mariadb라는 이름으로 접근 가능하게 함

✅ 이 방식도 작동하긴 하지만, --link는 구식 방식이라 추천은 안 됨

--network를 사용하는 게 현재 표준입니다

### docker ps
역할: 현재 실행 중인 컨테이너 목록 확인

✅ 정상 작동

🧠 핵심 요약: 왜 어떤 명령은 실패하고, 어떤 건 성공했나?
성공한 이유	실패한 이유
올바른 --network 사용	컨테이너가 꺼져 있었음
새 컨테이너를 깨끗하게 생성	컨테이너가 네트워크에 연결 안 됨
포트와 네트워크 동시에 지정	DB 연결 등 내부 설정이 불완전
이미지 빌드부터 실행까지 순서대로 처리	이미 존재하는 이름/상태 충돌

🔧 추천 실행 순서 (매번 깔끔하게 하고 싶다면)
bash

docker rm -f tour-backend tourdb    # 이전 컨테이너 삭제
docker network create tour-network  # 네트워크 생성 (1회만)
docker build -t tour-backend .      # 이미지 빌드

# DB 먼저 실행
docker run -d --name tourdb --network tour-network -e MYSQL_ROOT_PASSWORD=pass -e MYSQL_DATABASE=yourdb mariadb

# 백엔드 실행
docker run -d --name tour-backend --network tour-network -p 8080:8080 tour-backend
필요하면 프론트도 같은 네트워크에 넣어서 실행하면 완벽하게 통신됩니다.
 

프론트
# mariadb -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 23
Server version: 11.8.2-MariaDB-ubu2404 mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> SHOW databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| tourdb             |
+--------------------+
5 rows in set (0.002 sec)

MariaDB [(none)]> USE tourdb;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [tourdb]> select * from users;
+---------+----------------------------+----------------+----------------------------+--------------------+-----------+--------------------------------------------------------------+---------------+------+-----------+
| user_id | create_date                | email          | modified_date              | name               | nickname  | password                                                     | phone         | role | username  |
+---------+----------------------------+----------------+----------------------------+--------------------+-----------+--------------------------------------------------------------+---------------+------+-----------+
|       1 | 2025-07-24 06:41:16.681797 | test@test.com  | 2025-07-24 06:41:16.681913 | 테스트사용자       | 테스트    | test123                                                      | 010-1234-5678 | NULL | test_user |
|       2 | 2025-07-24 06:41:16.741017 | login@test.com | 2025-07-24 06:41:16.741100 | 로그인사용자       | 로그인    | 123                                                          | 010-9876-5432 | NULL | 123       |
|       3 | 2025-07-24 06:56:39.015071 | a@a.a          | 2025-07-24 06:56:39.015123 | a                  | a         | $2a$10$6bIHkcpiLwcH3rFUG.4vi.OHTIvy/D0Kiup99UB2kLRSpqhIPovpC | 1             | USER | a         |
+---------+----------------------------+----------------+----------------------------+--------------------+-----------+--------------------------------------------------------------+---------------+------+-----------+
3 rows in set (0.001 sec)

MariaDB [tourdb]>


### docker-compose.yml로 자동화하는 방법