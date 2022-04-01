# 여러 결합의 조합

- 새로운 테이블

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
  INSERT INTO students (first_name, last_name, email, phone, birthdate)
      VALUES ("Aladdin", "Lampland", "aladdin@lampland.com", "555-3333", "2001-05-10");
  INSERT INTO students (first_name, last_name, email, phone, birthdate)
      VALUES ("Simba", "Kingston", "simba@kingston.com", "555-1111", "2001-12-24");
      
  CREATE TABLE student_projects (id INTEGER PRIMARY KEY,
      student_id INTEGER,
      title TEXT);
      
  INSERT INTO student_projects (student_id, title)
      VALUES (1, "Carrotapault");
  INSERT INTO student_projects (student_id, title)
      VALUES (2, "Mad Hattery");
  INSERT INTO student_projects (student_id, title)
      VALUES (3, "Carpet Physics");
  INSERT INTO student_projects (student_id, title)
      VALUES (4, "Hyena Habitats");
      
  CREATE TABLE project_pairs (id INTEGER PRIMARY KEY,
      project1_id INTEGER,
      project2_id INTEGER);
  
  INSERT INTO project_pairs (project1_id, project2_id)
      VALUES(1, 2);
  INSERT INTO project_pairs (project1_id, project2_id)
      VALUES(3, 4);
  ```

  `students`, `student_projects`, `project_pairs`

- 프로젝트를 진행하고있는 학생들이 서로 짝을 지어서 서로의 프로젝트를 리뷰하도록 하려고 한다. 이 때 어떤 학생들끼리 짝을 지을지는 `project_pairs` 테이블에 저장되어 있다.

- 원하는 결과는 한 행에 `project_pairs`의 두 `id`와 함께 각각의 `project`의 제목이 나오도록 하는 것이다.

- 이 경우에 `student_projects`를 `project_pairs`에 두번 결합하여 해결할 수 있다.

  ```sqlite
  SELECT a.title, b.title FROM project_pairs
      JOIN student_projects a
      ON project_pairs.project1_id = a.id
      JOIN student_projects b
      ON project_pairs.project2_id = b.id;
  ```

  이 경우 `JOIN`도 두번, `ON`도 두번 써야 함에 주의하자.