## 집계 쿼리와 HAVING

- 지난번에 이어서..

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
  
  SELECT * FROM exercise_logs WHERE type = "biking";
  
  SELECT * FROM exercise_logs WHERE type = "biking" OR type = "hiking" OR type = "tree climbing" OR type = "rowing";
  
  SELECT * FROM exercise_logs WHERE type IN ("biking", "hiking", "tree climbing", "rowing");
  
  
  CREATE TABLE drs_favorites (id INTEGER PRIMARY KEY, type TEXT, reason TEXT);
  
  INSERT INTO drs_favorites(type, reason) VALUES ("biking", "Imporves endurance and flexibility.");
  
  INSERT INTO drs_favorites(type, reason) VALUES ("hiking", "Increases cardiovascular health.");
  
  SELECT type FROM drs_favorites;
  SELECT * FROM exercise_logs WHERE type IN ("biking", "hiking");
  
  SELECT * FROM exercise_logs WHERE type IN (SELECT type FROM drs_favorites);
  
  SELECT * FROM exercise_logs WHERE type IN (SELECT type FROM drs_favorites WHERE reason = "Increases cardiovascular health.");
  
  SELECT * FROM exercise_logs WHERE type IN (SELECT type FROM drs_favorites WHERE reason LIKE "%cardiovascular%");
  ```

- 이번에는 운동별로 얼마나 많은 칼로리를 소모했는지 알고 싶은데, 이때는 집계 쿼리를 이용할 수 있다.

  ```sqlite
  SELECT type, SUM(calories) FROM exercise_logs GROUP BY type;
  ```

  이렇게 하면 `SUM(calories)`라는 열이 따로 표시되어 보이게 된다. 만약 `SUM(calories)`라는 이름 대신에 다른 이름이 보이도록 하고 싶다면 `SUM(calories)`뒤에 `AS ~`를 써서 새로운 이름을 붙일 수 있다.

  ```sqlite
  SELECT type, SUM(calories) AS totlal_calories FROM exercise_logs GROUP BY type;
  ```

- 다음으로는 총 150칼로리 이상을 소모한 운동을 보고 싶다. 만약 다음과 같이 작성한다면

  ```sqlite
  SELECT type, SUM(calories) AS total_calories FROM exercise_logs WHERE calories >= 150
  GROUP BY type;
  ```

  결과값으로 `dancing`만이 나타날 것이다. 왜냐하면 `biking`은 전체로 보면 150칼로리 이상을 소모했지만, 한번의 `biking`으로 150 이상을 소모하지는 않았기 때문이다.

- 이 때 `HAVING`이라는 구문을 사용할 수 있다.

  ```sqlite
  SELECT type, SUM(calories) AS total_calories FROM exercise_logs GROUP BY type HAVING total_calories >= 150;
  ```

  `HAVING`절을 쓰는 위치에 주의하도록 하자. `HAVING`은 각 열의 개별 값이 아니라 그룹으로 묶인 값에 대해 조건을 적용한다.

  이러한 연산자에는 `SUM`외에도 `AVG, MIN, MAX` 등이 있다.

- 만약 `calories`의 평균에 대해 이러한 작업을 수행하고자 한다면...

  ```sqlite
  SELECT type, AVG(calories) AS avg_calories FROM exercise_logs GROUP BY type HAVING avg_calories;
  ```

- 두번이상 한 운동을 보고 싶다면

  ```sqlite
  SELECT type, COUNT(type) AS cnt FROM exercise_logs GROUP BY type HAVING cnt >= 2;
  /*또는*/
  SELECT type FROM exercise_logs GROUP BY type HAVING COUNT(*) >= 2;
  ```

  이 때 아래 쿼리의 경우에는 횟수는 출력되지 않고 `type`만 출력된다.







### 전체 코드

- 일부 위와 다를 수 있음

```sqlite
CREATE TABLE exercise_logs
    (id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    minutes INTEGER, 
    calories INTEGER,
    heart_rate INTEGER);

INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 115, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 45, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 165, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 25, 72, 80);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("rowing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("hiking", 60, 80, 85);

SELECT * FROM exercise_logs;

SELECT type, SUM(calories) AS total_calories FROM exercise_logs GROUP BY type;

SELECT type, SUM(calories) AS total_calories FROM exercise_logs
    GROUP BY type
    HAVING total_calories > 150
    ;

SELECT type, AVG(calories) AS avg_calories FROM exercise_logs
    GROUP BY type
    HAVING avg_calories > 70
    ; 
    
SELECT type FROM exercise_logs GROUP BY type HAVING COUNT(*) >= 2;
```

