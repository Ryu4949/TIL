# CASE

```sqlite
CREATE TABLE exercise_logs
    (id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    minutes INTEGER, 
    calories INTEGER,
    heart_rate INTEGER);

INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 30, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 165, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 25, 72, 80);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("rowing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("hiking", 60, 80, 85);

SELECT * FROM exercise_logs;

SELECT COUNT(*) FROM exercise_logs WHERE heart_rate > 220 - 30;
```



- 지난번과 같은 운동 데이터에 쿼리 한줄이 추가되었다. 사람의 최대 심박수는 220 - 나이라고 해서 심박수가 최대 수치를 넘어가는지를 확인하기 위한 쿼리다. 
- 해당 쿼리에서 볼 수 있듯, 쿼리 내에서는 사칙연산이 가능하고 괄호를 통한 연산 순서의 조정 또한 가능하다.

- 이번에는 심박수가 '최댓값의 50~90%'라는 목표치 이내에 들어가는지 확인해보자.

  ```sqlite
  /* 50-90% of max*/
  SELECT COUNT(*) FROM exercise_logs WHERE
      heart_rate >= ROUND(0.50 * (220-30)) 
      AND  heart_rate <= ROUND(0.90 * (220-30));
  ```

  이렇게 하면 4라는 결과값을 얻을 수 있다. 여기서 `ROUND`는 반올림을 해주는 기능을 한다. 계산식 다음에 숫자를 지정하면 특정 소수점 자리, 정수 자리에서 반올림이 가능하다. 자세한건 검색

- 이 결과의 의미는 내가 했던 모든 운동에 기록된 심박수 중에 4개만이 목표치에 들어있다는 것을 의미한다. 그렇다면 나머지 값들은 어느 구간에 존재할까? 물론 90~100% 구간이나 50% 미만의 구간에 존재한다는 것은 알 수 있다. 그것을 확인해보자.

- 이 때 `GROUP BY`를 써야할 것 같지만 그룹으로 묶을 열이 없다. 그래서 이런 경우에 `CASE`를 사용할 수 있다.

  ```sqlite
  /* CASE */
  SELECT type, heart_rate,
      CASE 
          WHEN heart_rate > 220-30 THEN "above max"
          WHEN heart_rate > ROUND(0.90 * (220-30)) THEN "above target"
          WHEN heart_rate > ROUND(0.50 * (220-30)) THEN "within target"
          ELSE "below target"
      END as "hr_zone"
  FROM exercise_logs;
  ```

  - 먼저 `CASE`는 파이썬의 `if`와 비슷하다고 생각하면 된다. `CASE`내에는 `WHEN`을 활용하여 특정 조건에 어떤 카테고리에 해당할 것인지를 표시해준다. `ELSE`는 위에 작성한 조건 외의 전부를 의미한다.
  - 조건의 명시가 끝나면 `END`로 마무리하며 어떤 이름으로 해당 열을 표시할 것인지 정한다. 이 경우 각 운동의 심박수가 어떤 범위에 속하는지를 `hr_zone`이라는 이름의 열에 나타내 준다.

- 이제 각 범위에 몇개가 속해있는지를 확인해보자

  ```sqlite
  SELECT COUNT(*),
      CASE 
          WHEN heart_rate > 220-30 THEN "above max"
          WHEN heart_rate > ROUND(0.90 * (220-30)) THEN "above target"
          WHEN heart_rate > ROUND(0.50 * (220-30)) THEN "within target"
          ELSE "below target"
      END as "hr_zone"
  FROM exercise_logs
  GROUP BY hr_zone;
  ```

  `hr_zone`을 기준으로 묶을 것이고 해당 범위에 속하는 개수를 표시할 것이므로 `SELECT` 다음에는 `type`과 `heart_rate`를 지우고 `COUNT(*)`로 대체해준다. `COUNT(*)`옆에 콤마 안찍어주면 오류남







### 전체 코드

```sqlite
CREATE TABLE exercise_logs
    (id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    minutes INTEGER, 
    calories INTEGER,
    heart_rate INTEGER);

INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 30, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 165, 120);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("tree climbing", 25, 72, 80);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("rowing", 30, 70, 90);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("hiking", 60, 80, 85);

SELECT * FROM exercise_logs;

SELECT COUNT(*) FROM exercise_logs WHERE heart_rate > 220 - 30;

/* 50-90% of max*/
SELECT COUNT(*) FROM exercise_logs WHERE
    heart_rate >= ROUND(0.50 * (220-30)) 
    AND  heart_rate <= ROUND(0.90 * (220-30));
    
/* CASE */
SELECT type, heart_rate,
    CASE 
        WHEN heart_rate > 220-30 THEN "above max"
        WHEN heart_rate > ROUND(0.90 * (220-30)) THEN "above target"
        WHEN heart_rate > ROUND(0.50 * (220-30)) THEN "within target"
        ELSE "below target"
    END as "hr_zone"
FROM exercise_logs;

SELECT COUNT(*),
    CASE 
        WHEN heart_rate > 220-30 THEN "above max"
        WHEN heart_rate > ROUND(0.90 * (220-30)) THEN "above target"
        WHEN heart_rate > ROUND(0.50 * (220-30)) THEN "within target"
        ELSE "below target"
    END as "hr_zone"
FROM exercise_logs
GROUP BY hr_zone;
```

