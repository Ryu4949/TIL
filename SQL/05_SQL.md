# 자가 결합

- 새로운 `students` 테이블로 진행하도록 하자.

  ```sqlite
  CREATE TABLE students (id INTEGER PRIMARY KEY AUTOINCREMENT,
      first_name TEXT,
      last_name TEXT,
      email TEXT,
      phone TEXT,
      birthdate TEXT,
      buddy_id INTEGER);
  
  INSERT INTO students 
      VALUES (1, "Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24", 2);
  INSERT INTO students 
      VALUES (2, "Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04", 1);
  INSERT INTO students 
      VALUES (3, "Aladdin", "Lampland", "aladdin@lampland.com", "555-3333", "2001-05-10", 4);
  INSERT INTO students 
      VALUES (4, "Simba", "Kingston", "simba@kingston.com", "555-1111", "2001-12-24", 3);
  ```

  이름, 이메일, 전화번호, 생일에다가 `buddy_id`라는 항목이 있다. 단짝친구로 지정된 친구의 id를 의미한다.

- 이러한 경우가 기존의 것과 다른 점은 기존에는 한 테이블의 데이터가 다른 테이블과 관계를 맺고 있었다면, 이번에는 하나의 테이블 내에서 하나의 열이 다른 열과 관계를 맺고 있다는 점이다.

- 이제 학생의 이름 옆에 해당 학생의 buddy를 나타내보자.

- 먼저 `JOIN`을 생각할 수 있겠다.

  ```sqlite
  SELECT first_name, last_name, email 
  	FROM students
  	JOIN students;
  ```

- 하지만 이렇게 하면 error가 발생한다. 같은 테이블을 결합하는 것이기 때문에, 열의 이름도 모두 같고, 따라서 선택한 열들을 어디에서 가져와야 하는지 혼동이 오는 것이다. 

- 그렇다면 이전에 했던 것 처럼 table 이름을 접두사로 붙이는 것으로 해결이 가능할까? 그것도 아니다. 왜냐하면 두 테이블의 이름마저 같기 때문이다. 

- 그래서 먼저 한가지 방법은, `FROM`의 `students`와 `JOIN`의 `students` 중 하나에게 '에일리어스'를 주는 방법이 있다. 별명이라고 생각하면 된다. `JOIN`의 `students`에 `buddies`라는 별명을 지어주자

  ```sqlite
  SELECT first_name, last_name, email
  	FROM students
  	JOIN students buddies;
  ```

- 이제 선택한 열에 테이블 이름을 접두사로 구분할 수 있다. 접두사를 붙여주고 `ON`을 이용하여 어떤 경우에 이메일을 가져올지 정해주자

  ```sqlite
  SELECT students.first_name, students.last_name, buddies.email
  	FROM students
  	JOIN students buddies
  	ON students.buddy_id = buddies.id;
  ```

  

### 전체 코드

```sqlite
-- SQLite
CREATE TABLE students (id INTEGER PRIMARY KEY AUTOINCREMENT,
    first_name TEXT,
    last_name TEXT,
    email TEXT,
    phone TEXT,
    birthdate TEXT,
    buddy_id INTEGER);

INSERT INTO students 
    VALUES (1, "Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24", 2);
INSERT INTO students 
    VALUES (2, "Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04", 1);
INSERT INTO students 
    VALUES (3, "Aladdin", "Lampland", "aladdin@lampland.com", "555-3333", "2001-05-10", 4);
INSERT INTO students 
    VALUES (4, "Simba", "Kingston", "simba@kingston.com", "555-1111", "2001-12-24", 3);


/* self join */
SELECT students.first_name, students.last_name, buddies.email as buddy_email
    FROM students
    JOIN students buddies
    ON students.buddy_id = buddies.id;
```

