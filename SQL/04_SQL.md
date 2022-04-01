# 04_관계형 쿼리

- 이전까지의 내용은 하나의 테이블에서 어떻게 데이터를 뽑아내고 집계하는지에 관한 것
- 하지만 대부분의 경우에 테이블은 하나만 존재하지 않고 다수의 테이블이 존재하여 서로 관계를 맺고 있음
- 내용적으로도 관련이 있을 수 있고, 하나의 테이블에 있는 데이터가 다른 테이블에도 포함되어 있을 수 있음
  - 이런 경우에 다른 테이블에도 포함된 어떤 데이터가 변경되었을 경우 어떻게 해야할까?
  - 그렇기 때문에 특정 데이터 열은 하나의 테이블에만 저장하여 업데이트할 부분을 줄이고 중복된 데이터가 다른 테이블에 저장되는 일을 방지하는 것이 일반적이다.
  - 이렇게 여러 개의 테이블을 만들고 그들간의 데이터를 관계할 수 있는 방법이 필요한 것이다.

- 결국 SQL을 사용하여 여러 관계형 테이블에 나누어진 데이터를 다루고, 필요한 경우에 데이터를 한 곳에 모으는 방법을 이해해야 한다.
- 이 때 이용할 수 있는 것이 `JOIN`이다.



## JOIN

- 새로운 테이블과 함께 진행하자

  ```sqlite
  -- SQLite
  CREATE TABLE students (id INTEGER PRIMARY KEY,
      first_name TEXT,
      last_name TEXT,
      email TEXT,
      phone TEXT,
      birthdate TEXT);
  
  INSERT INTO students (first_name, last_name, email, phone, birthdate)
      VALUES ("Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24");
  INSERT INTO students (first_name, last_name, email, phone, birthdate)
      VALUES ("Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04");
      
  CREATE TABLE student_grades (id INTEGER PRIMARY KEY,
      student_id INTEGER,
      test TEXT,
      grade INTEGER);
  
  INSERT INTO student_grades (student_id, test, grade)
      VALUES (1, "Nutrition", 95);
  INSERT INTO student_grades (student_id, test, grade)
      VALUES (2, "Nutrition", 92);
  INSERT INTO student_grades (student_id, test, grade)
      VALUES (1, "Chemistry", 85);
  INSERT INTO student_grades (student_id, test, grade)
      VALUES (2, "Chemistry", 95);
  ```

  - 하나는 이름, 이메일, 전화번호, 생일이 담긴 `students`라는 테이블이고, 다른 하나는 고유식별자, 학생번호, 시험과목, 성적이 담긴 `student_grades`테이블이다.
  - 보고싶다면 `SELECT * FROM student_grades;`와 `SELECT * FROM students;`를 통해 확인할 수 있다.

- 보다시피 두개의 테이블은 서로 관련된 내용을 담고있다. 그래서 두 테이블을 결합해서 학생의 이름과 이메일을 점수 옆에 출력되도록 해볼 것이다.



### 교차 결합

- 가장 간단한 방법인 교차결합(cross join)인데, `FROM`뒤에 두 개의 테이블을 모두 명시해주면 된다.

  ```sqlite
  SELECT * FROM student_grades, students;
  ```

- 가장 간단한 방법이지만 이 방법은 하나의 테이블의 모든 행을 다른 테이블의 모든 행과 연동시키기 때문에 원하는 데이터를 추출하기에 적합하지 않다. 우리가 원하는 것은 학생 번호가 일치하는 경우에 연동시키는 것인데, 교차결합을 하면 student grades의 2행 X studnets의 4행이 연동되어 총 8행이 표시된다.



### 내부 결합

- 다음 방법은 교차결합과 비슷하지만 훨씬 유용한 내부결합(inner join)이다.

  ```sqlite
  /* implicit inner join */
  SELECT * FROM student_grades, students
      WHERE student_grades.student_id = students.id;
  ```

- 교차결합과 비슷하지만 `WHERE` 절을 통해서 특정 조건을 만족하는 행들을 보이게 할 수 있다.

- 이런 경우는 암시적 내부 결합(implicit inner join)이라고 부르며, 유용하지만 더 좋은 방법이 존재한다.



### 명시적 내부 결합

- 이번이 바로 `JOIN`을 사용하는 명시적 내부 결합(explicit inner join)이다.

  ```sqlite
  /* explicit inner join - JOIN */
  SELECT * FROM students
      JOIN student_grades
      ON students.id = student_grades.student_id;
  ```

- 하나의 테이블을 `FROM`으로 불러오고, 거기에 결합할 테이블을 `JOIN`을 통해 연동시킨다. 그리고 `WHERE` 대신 `ON`을 사용하여 검색할 열을 설정한다.

- 이렇게 `JOIN`을 해주면 students에서도 결합한 테이블의 열을 불러올 수 있다.

  ```sqlite
  /* explicit inner join - JOIN */
  SELECT first_name, last_name, email, test, grade FROM students
      JOIN student_grades
      ON students.id = student_grades.student_id;
  ```

- 게다가 `WHERE`과 `GROUP BY`등도 사용이 가능하다.

  ```sqlite
  SELECT first_name, last_name, email, test, grade FROM students
      JOIN student_grades
      ON students.id = student_grades.student_id
      WHERE grade > 90;
  ```

  이렇게 하면 성적이 90점보다 높은 경우만 불러올 수 있다.

- 한가지 주의할 점은 서로 다른 테이블에, 열이 담고있는 데이터의 의미는 다르지만 열의 이름이 같은 경우가 있을 수 있다. 그 경우에 어떤 테이블에서 해당 열을 가져와야 하는지 혼동이 올 수 있기 때문에, `SELECT`로 열을 선택할 때는 열의 이름에 테이블의 이름을 접두사로 붙이는 것이 바람직하다.

  ```sqlite
  SELECT students.first_name, students.last_name, students.email, student_grades.test, student_grades.grade FROM students
      JOIN student_grades
      ON students.id = student_grades.student_id
      WHERE grade > 90;
  ```



### 외부 결합

- 기존의 테이블에 더해서 새로운 테이블을 생성하자. id, student_id, title로 이루어진 `student_projects`라는 테이블이다.

  ```sqlite
  CREATE TABLE student_projects (id INTEGER PRIMARY KEY,
      student_id INTEGER,
      title TEXT);
      
  INSERT INTO student_projects (student_id, title)
      VALUES (1, "Carrotapault");
  ```

- 여기서 학생의 이름과 project 이름을 보려고 한다.

  ```sqlite
  SELECT students.first_name, students.last_name, student_projects.title 
  FROM  students
  JOIN student_projects
  ON students.id = student_projects.student_id;
  ```

- 이렇게하면 `Peter`와 그의 프로젝트인 `carrotapault`가 나오는 것을 볼 수 있다. 그러면 `Alice`는 어디에 갔을까? 내부결합을 하면 두 테이블에서 일치하는 기록(`ON`)으로만 행을 생성하기 때문에 `Alice`는 포함되지 않는다. `student_projects`에 `Alice`의 `student_id`와 일치하는 행이 없기 때문이다.

- 하지만 많은 경우에 우리는 어떤 학생에 해당하는 결과값이 없더라도 모든 학생이 포함되는 리스트를 필요로 한다. 이럴 때 유용한 방법이 외부 결합이다.

- 방식도 매우 간단한데, 내부결합의 `JOIN` 앞에다가 `LEFT OUTER`를 추가해주면 된다.

  ```sqlite
  /* outer join */ 
  SELECT students.first_name, students.last_name, student_projects.title
      FROM students
      LEFT OUTER JOIN student_projects
      ON students.id = student_projects.student_id;
  ```

  이렇게 하면 project가 없는 `Alice`의 경우에도 `NULL`을 가진 채로 리스트에 나타나게 된다.

- 작동 방식을 살펴보면 다음과 같다.

  - 먼저 `LEFT`가 왼쪽의 테이블에서 모든 행을 가져오라고 한다. 여기서 왼쪽 테이블은 `LEFT`의 왼쪽에 있는 `students`이다.
  - 그 다음에 `OUTER`는 오른쪽에 있는 테이블인 `student_projects`에 일치하는 항목이 없다고 해도 행을 유지해야 한다고 말해준다.

- `LEFT JOIN`외에도 `RIGHT JOIN`이 존재하지만 순서만 다를 뿐 `LEFT JOIN`으로 처리가 가능하다.

- 그 외에도 `FULL OUTER JOIN`이라는 것도 있지만 지금으로서는 불필요하고, 궁금하다면 검색해보도록 하자.





### 전체 코드

```sqlite
-- SQLite
CREATE TABLE students (id INTEGER PRIMARY KEY,
    first_name TEXT,
    last_name TEXT,
    email TEXT,
    phone TEXT,
    birthdate TEXT);

INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24");
INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04");
    
CREATE TABLE student_grades (id INTEGER PRIMARY KEY,
    student_id INTEGER,
    test TEXT,
    grade INTEGER);

INSERT INTO student_grades (student_id, test, grade)
    VALUES (1, "Nutrition", 95);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (2, "Nutrition", 92);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (1, "Chemistry", 85);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (2, "Chemistry", 95);

/* cross join */
SELECT * FROM student_grades, students;

/* inner join */
SELECT * FROM student_grades, students
    WHERE student_grades.student_id = students.id;

    /* explicit inner join - JOIN */
SELECT * FROM students
    JOIN student_grades
    ON students.id = student_grades.student_id;

    /* explicit inner join - JOIN */
SELECT first_name, last_name, email, test, grade FROM students
    JOIN student_grades
    ON students.id = student_grades.student_id;

SELECT first_name, last_name, email, test, grade FROM students
JOIN student_grades
ON students.id = student_grades.student_id
WHERE grade > 90;

SELECT students.first_name, students.last_name, students.email, student_grades.test, student_grades.grade FROM students
    JOIN student_grades
    ON students.id = student_grades.student_id
    WHERE grade > 90;
```



# 번외

### HAVING과 결합에서의 ORDER BY

- `WHERE`절에서는 집계함수를 사용할 수 없다. 그래서 집계 함수를 사용한 조건 비교를 할 때 `HAVING`을 사용한다. 
- 정렬을 할 때는 집계함수를 사용할 수 있다. 그래서 `ORDER BY SUM(~)`와 같이 사용이 가능하다.





