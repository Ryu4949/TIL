# SQL

### SQL 시작하기

- 먼저 vscode 열고 bash에서 `sqlite3 tutorial.sqlite3`
- 그 다음에 `.database` 치면 database가 생성되고 왼쪽에 tutorial.sqlite3 파일이 생길 것
- `Ctrl + Shift + P`누르고 `SQLite: Open Database` 선택하고, `tutorial.sqlite3` 선택
- 그러면 왼쪽 밑에 SQLite Explorer가 뜰 것
- 위에 파일이든 밑에 explorer든 우클릭 한다음에 `New Query`를 누르면 새로운 query를 날릴 수 있다



### 테이블 생성하기

- `CREATE TABLE groceries (id INTEGER PRIMARY KEY, name TEXT, quantity INTEGER);`
  - groceries라는 이름의 테이블을 만들고, 어떤 항목들을 넣을 것인지 정해준다.
  - id는 정수형태의 **고유 식별자**, name은 식품의 이름을 넣을 것이고 문자열, quantity는 얼마나 살지를 넣을 것이므로 정수형이다.

### 데이터 삽입하기

- `INSERT INTO groceries VALUES (1, "Bananas", 4);`

  - groceries 테이블에 id가 1, name이 "Bananas", quantity가 4인 데이터를 삽입한다.

- ```sqlite
  -- SQLite
  CREATE TABLE groceries (id INTEGER PRIMARY KEY, name TEXT, quantity INTEGER);
  
  INSERT INTO groceries VALUES (1, "Bananas", 4);
  
  INSERT INTO groceries VALUES (2, "Peanut Butter", 1);
  
  INSERT INTO groceries VALUES (3, "Dark Chocolate Bars", 2);
  ```



### 데이터 조회하기

- `SELECT name FROM groceries;`
  - groceries 테이블에서 name이라는 항목을 볼 때 이렇게 쓰면된다
  - 만약 table 전체를 조회하고 싶다면?
- `SELECT * FROM groceries;`



### 데이터 정렬/필터

- `ORDER BY`
  - 예를 들어 groceries 테이블을 quantity를 기준으로 오름차순 정렬하여 조회하고 싶다면?
  - `SELECT * FROM groceries ORDER BY quantity;`

- `WHERE`
  - 자료를 필터하기 위해 사용
  - `WHERE {조건}` 형태로 사용한다
  - groceries 테이블에서 사고자하는 양(quantity)가 2개 이하인 것만 조회해보자.
  - `SELECT * FROM groceries WHERE quantity <= 2;`



### 테이블 삭제하기

- 새로운 테이블을 만들기 위해 기존 테이블을 삭제해보자
  - `DROP TABLE groceries;`



### 데이터 집계하기

- 새로운 테이블과 함께 계속 진행하자

  ```sqlite
  -- SQLite
  CREATE TABLE groceries (id INTEGER PRIMARY KEY, name TEXT, quantity INTEGER, aisle INTEGER);
  
  INSERT INTO groceries VALUES (1, "Bananas", 4, 7);
  INSERT INTO groceries VALUES (2, "Peanut Butter", 1, 2);
  INSERT INTO groceries VALUES (3, "Dark Chocolate Bars", 2, 2);
  INSERT INTO groceries VALUES (4, "Ice Cream", 1, 12);
  INSERT INTO groceries VALUES (5, "Cherries", 6, 2);
  INSERT INTO groceries VALUES (6, "Chocolate Syrup", 1, 4);
  ```

- 먼저 전체 몇개의 식품을 구매했는지 사고 싶다면

  - `SELECT SUM(quantity) FROM groceries;`

- 가장 많이 산 식품이 어떤 것인지 알고 싶다면

  - `SELECT MAX(quantity) FROM groceries;`

- aisle 별로 몇개의 식품을 샀는지 알고싶다면
  - `SELECT SUM(quantity) FROM groceries GROUP BY aisle;`
  - `GROUP BY`를 쓰면 됨
  - 그런데 이렇게만 하면 몇번 통로에서 얼마만큼을 샀는지 알 수 없어서
  - `SELECT aisle, SUM(quantity) FROM groceries GROUP BY aisle;`이렇게 써주면 된다!
  
- 이 때 quantity의 합은 aisle을 기준으로 했는데 `SELECT` 다음에 `aisle`이 아니라 `name`을 쓰면 어떻게 될까?

- 같은 aisle에 해당하는 `name`이 여러개 있지만 하나만 표시가 되는데, 이 때 표시되는 name은 단순히 첫 번째 `name`을 가져옴. 그래서 이렇게는 사용하지 않는 게 좋음



---

# 심화 SQL 쿼리



## AND/OR 사용하기

- AUTO INCREMENT: 보통 테이블을 만들 때 고유번호를 생성하는데 사용. 사용하면 레코드의 값이 중복되지 않고 1씩 자동 증가

- 새로운 테이블을 사용하자

  ```sqlite
  -- SQLite
  CREATE TABLE exercise_logs (id INTEGER PRIMARY KEY AUTOINCREMENT, type TEXT, minutes INEGER,
  calories INTEGER, heart_rate INTEGER);
  
  INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
  INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 30, 105);
  INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);
  
  SELECT * FROM exercise_logs;
  ```

- 이렇게 모든 열에 데이터를 넣지 않고, 필요한 부분에만 넣을 때 테이블 명 다음에 열의 이름을 명시해준다. 여기서는 id가 빠졌는데 `AUTOINCREMENT`를 지정해줌으로써 자동으로 1씩 증가하도록 해줄 수 있다.

- 여기서 칼로리를 50보다 많이 소모한 운동을 찾고 싶다면, 그리고 칼로리 순으로 정렬
  - `SELECT * FROM exercise_logs WHERE calories > 50 ORDER BY calories;`
- 추가로, 칼로리를 50보다 많이 소모했고 시간은 30분보다 적게 들인 운동을 찾고 싶다면?
  - `SELECT * FROM exercise_logs WHERE calories > 50 AND minutes < 30 ORDER BY calories;`
- 비슷한 방식으로 `OR`연산자도 사용이 가능하다.
- 만약 `AND`와 `OR` 연산자를 같이 써준다면 `AND`가 더 우선시되지만, 항상 괄호를 사용해서 우선순위를 표시해주는 것이 바람직하다.



## IN

- 데이터를 조금 추가해보자

  ```sqlite
  -- SQLite
  CREATE TABLE exercise_logs (id INTEGER PRIMARY KEY AUTOINCREMENT, type TEXT, minutes INEGER,
  calories INTEGER, heart_rate INTEGER);
  
  INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
  INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 30, 105);
  INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);
  INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 30, 70, 90);
  INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 25, 72, 80);
  INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("rowing", 30, 70, 90);
  INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("hiking", 60, 80, 85);
  ```

- 여기서 biking만 뽑아내고 싶다면?

  - `SELECT * FROM exercise_logs WHERE type = "biking";`

- 여기다가 추가로 다른 야외활동도 뽑아내고 싶다면 물론 `OR`를 쓸 수 있다. 하지만 이는 종류가 많아지면 번거로워지고 이 때 사용할 수 있는게 `IN`이다.

  - `OR`를 쓴다면?
    - `SELECT * FROM exercise_logs WHERE type = "biking" OR type = "hiking" OR type = "tree climbing" OR type = "rowing";`
  - `IN`을 쓴다면?
    - `SELECT * FROM exercise_logs WHERE type IN ("biking", "hiking", "tree climbing", "rowing");`

- 이번에는 실내활동을 확인하고 싶다면?

  - 물론 `OR`를 쓸 수도 있고, `IN`으로 실내활동 목록을 작성할 수도 있겠지만

  - `SELECT * FROM exercise_logs WHERE type NOT IN ("biking", "hiking", "tree climbing", "rowing");`
  - 이렇게 `IN` 앞에 `NOT`을 추가해줌으로써 해결 가능하다.

- 이번에는 테이블을 하나 추가해보자. 의사가 추천하는 운동 목록과 그 이유이다.

  ```sqlite
  CREATE TABLE drs_favorites (id INTEGER PRIMARY KEY, type TEXT, reason TEXT);
  
  INSERT INTO drs_favorites(type, reason) VALUES ("biking", "Imporves endurance and flexibility.");
  
  INSERT INTO drs_favorites(type, reason) VALUES ("hiking", "Increases cardiovascular health.");
  ```

- 내가 한 운동 목록에서, 의사가 추천하는 운동과 일치하는 것만 뽑고 싶다면 어떻게 할 수 있을까

  - 물론 먼저 `drs_favorites`에서 `type`만 따로 불러온다음, 그 목록을 복사해서 `IN` 다음에 넣는 방법도 있을 것이다. 그러나 `exercise_logs` 테이블에 데이터 행이 추가되거나 삭제된다면 해당 쿼리는 무용지물이 된다.

    ```sqlite
    SELECT type FROM drs_favorites; /*여기서 목록을 복사*/
    SELECT * FROM exercise_logs WHERE type IN ("biking", "hiking");
    ```

  - 그래서

    ```sqlite
    SELECT * FROM exercise_logs WHERE type IN (SELECT type FROM drs_favorites);
    ```

    이렇게 써주면 위와 똑같은 쿼리가 된다! 이 때 괄호 안에 들어있는 쿼리를 **서브 쿼리**라고 한다. 이 서브쿼리는 `drs_favorites`의 변동을 실시간으로 반영하게 된다.

  - 매우 유용하지만 경우에 따라 점점 쿼리가 복잡해질 수도 있는데, 다음과 같은 예를 들어보자. 의사가 '심혈관에 좋다는 이유'로 추천한 운동을 추리려고 한다. 그렇다면

    ```sqlite
    SELECT * FROM exercise_logs WHERE type IN (SELECT type FROM drs_favorites WHERE reason = "Increases cardiovascular health.");
    ```

    이렇게 쓸 수 있다. 그런데 저렇게 긴 `reason`을 그때그때 복사하다보면 때때로 마침표를 누락하는 등 오타가 발생할 수 있고, 그렇게 되면 정확한 데이터를 출력하지 못하게 된다. 그래서 이럴 때 아주 유용하게 사용할 수 있는 것이 `LIKE`다.



## LIKE

- 우리는 위에서 심혈관에 좋다는 점이 이유인 운동을 찾고있다. 그래서 중요한 단어는 `cardiovascular`가 되고 여기에 와일드카드를 덧붙여 원하는 결과를 찾을 수 있다.

  ```sqlite
  SELECT * FROM exercise_logs WHERE type IN (SELECT type FROM drs_favorites WHERE reason LIKE "%cardiovascular%");
  ```

  이렇게 하면 `reason`에 `cardiovascular`라는 단어가 들어있는 모든 데이터를 출력해줄 것이다.