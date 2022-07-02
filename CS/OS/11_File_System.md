# 11_File System



# 파일과 파일 시스템

## 1. 파일 시스템의 개요

### 파일 시스템의 개념

#### 개념

- 파일은 하드디스크, CD와 같은 제2저장장치에 보관되는데 이 과정에서 사용자가 직접 개입하게 되면 다른 사용자의 파일을 훼손하거나 저장장치를 어지럽히는 등 문제가 있을 수 있음
- 따라서 운영체제는 사용자가 파일에 '직접' 접근하는 것을 막음
- 그 대신 파일 관리자를 두고 저장장치의 전체 관리를 맡기는데 이를 파일 시스템이라고 함

#### 파일 시스템과 파일 관리자의 역할

- 파일 관리자는 사용자의 요청에 따라 파일을 저장하거나 파일의 내용을 읽어옴
- 파일 테이블을 사용하여 파일을 생성/수정/삭제
- 사용자가 파일을 사용하고자 할 때 읽기/쓰기/실행 등 다양한 접근 방법 제공
  - 이때 사용자는 파일에 접근하기 위해 파일관리자로부터 권한(키)을 획득해야 하는데, 이러한 권한을 파일 디스크립터(file descriptor)라고 함

### 파일 시스템의 기능

#### 파일 테이블

- 파일을 생성하고, 사용자가 파일을 편리하게 관리할 수 있도록 디렉터리 구조를 제공
- 또한 여러 종류의 파일을 구분하기 위해 파일 이름과 확장자를 만들어 관리
- 이름, 크기, 생성 날짜 등 다양한 정보를 파일 헤더에 저장하여 관리
- 윈도우: FAT, NTFS
- 유닉스: I-node
- 파일 시스템의 기능
  

| 기능           | 설명                                                         |
| -------------- | ------------------------------------------------------------ |
| 파일 구성      | 사용자의 요구에 따라 파일과 디렉터리 생성                    |
| 파일 관리      | 파일 생성, 수정, 삭제<br />조각 모음을 통해 사용자가 파일에 빨리 접근할 수 있도록 함 |
| 접근 권한 관리 | 다른 사용자로부터 파일을 보호하기 위해 접근 권한 관리        |
| 접근 방법 제공 | 파일에 접근하고자 하는 사용자에게 접근 방법 제공             |
| 무결성 보장    | 파일의 내용이 손상되지 않도록 함                             |
| 백업과 복구    | 사고로부터 파일을 보호                                       |
| 암호화         | 악의적 접근으로부터 파일을 보호                              |



### 블록과 파일 테이블

#### 블록

- 저장장치에서 사용하는 가장 작은 단위로, 한 블록에 주소 하나가 할당
- 메모리는 바이트 단위, 하드디스크는 섹터 단위
- 이 때 섹터 단위로 주소를 할당하면 너무 많은 주소가 필요하기 때문에 파일 관리자는 여러 섹터를 묶어 하나의 블록으로 만들고, 블록 하나에 주소 하나를 할당

#### 블록의 크기

-  블록의 크기가 작으면, 내부 단편화 현상이 줄어들어 저장장치를 효율적으로 사용 가능
- 하지만 파일이 여러 블록으로 나누어지기 때문에 파일 입출력 속도 저하



## 2. 파일 분류와 확장자

### 파일

- 논리적인 데이터의 집합
- 0과 1의 비트 패턴(bit pattern)으로 구성
- 운영체제 입장에서 실행 파일과 데이터 파일로 구분

### 실행 파일

- 운영체제가 메모리로 가져와 CPU를 이용하여 작업을 하는 파일
- 사용자의 요청으로 프로세스가 된 파일

### 데이터 파일

- 실행 파일이 작업하는 데 필요한 데이터를 모아놓은 파일

- 스스로 프로세스가 될 수 없고, 운영체제가 전송하거나 보관만 할 뿐 특별하게 다루지 않음

- 사진, 음악, 문서 파일 등

- 데이터 파일은 종류가 다양

  - 사진의 경우 BMP, JPG, GIF, PNG 등
  - 음악, 영상도 다양한 종류

- 다양한 종류를 구별하기 위해 헤더가 존재

  - 헤더에는 파일의 이름, 버전, 크기, 만든 날짜, 접근 권한 등의 정보 저장

- 확장자 존재

  | 파일       | 확장자                   | 비고                         |
  | ---------- | ------------------------ | ---------------------------- |
  | 실행       | exe, com                 | 유닉스에는 실행 파일 확장자X |
  | 소스코드   | c, cpp, pas, a, java     | -                            |
  | 라이브러리 | lib, a, dll              | -                            |
  | 배치       | bat, sh, csh             | -                            |
  | 문서       | txt, doc, hwp, pdf, ps   | -                            |
  | 동영상     | avi, asf, mkv, mov, rmv  | -                            |
  | 음악       | wav, mp3, ogg, flc, aac  | -                            |
  | 이미지     | bmp, gif, jpg, tiff, png | -                            |
  | 압축       | rar, zip, arc, al        | -                            |

  

## 3. 파일 이름과 연결 프로그램

### 파일 이름

- 대부분 `파일이름.확장자`로 구성
- 유의 사항
  - 초창기 운영체제의 영향으로 대부분 확장자는 3자 이하이지만 4~5자도 가능
  - 파일 이름에 마침표(.)를 여러번 사용 가능한데, 이때 마지막 마침표 다음의 글자를 확장자로 인식
  - 파일 이름은 현재 **경로 이름을 포함**하여 최대 255자
  - 운영체제에 따라 파일 이름에 사용할 수 있는 특수문자의 종류가 다르고 대소문자 구분 여부도 다름
  - 파일의 확장자를 변경한다고 해서 내용이 바뀌지는 않음

### 연결 프로그램

- 어떤 데이터 파일을 사용하는 응용 프로그램을 의미

- 윈도우에서 실행 파일을 더블클릭하면 프로세스가 생성되어 실행되고, 데이터 파일을 더블클릭하면 연결 프로그램이 실행됨

- 따라서 데이터 파일을 실행파일로 착각하기 쉬움

  

## 4. 파일 속성

### 파일 속성

| 속성          | 특징             |
| ------------- | ---------------- |
| name          | 파일 이름        |
| type          | 파일의 종류      |
| size          | 파일의 크기      |
| time          | 파일의 접근 시간 |
| location      | 파일의 위치      |
| accessibility | 파일의 접근 권한 |
| owner         | 파일의 소유자    |

- time: 파일의 접근 시간으로 만든 시간, 변경 시간, 최근 파일을 열어본 시간 등으로 세분화 됨
- location: 같은 파일이라고 하더라도 서로 다른 위치에 각각 존재하면 운영체제는 서로 다른 파일로 인식함
- owner: 파일의 소유자라는 개념은 윈도우에서는 거의 없는 개념이지만, 유닉스에서 명확하게 구분항 사용됨. 접근 권한도 유닉스에서 보다 세분화 되어있음

### 헤더

#### 파일 헤더

- 파일 테이블에서 관리하며 파일의 이름, 종류, 크기, 시간, 접근 권한, 파일이 있는 저장장치의 블록위치 등과 같은 일반적인 내용이 담겨있는 곳

#### 고유 헤더

- 응용 프로그램이 필요로 하는 정보가 담긴 곳
- 파일의 버전 번호, 크기, 특수 정보 등
- 파일을 복구할 때 유용



## 5. 파일 작업의 유형

- 파일 작업: 파일 삭제/이름 변경 등과 같이 파일을 변경하는 것. 파일 연산(file operation)이라고도 함
- 크게 파일 자체를 변경하는 작업과 파일 내용을 변경하는 작업으로 구분할 수 있음

### 파일 자체를 변경하는 작업

- 작업의 종류

  | 작업   | 설명        | 작업   | 설명           |
  | ------ | ----------- | ------ | -------------- |
  | open   | 파일 열기   | copy   | 파일을 복사    |
  | close  | 파일 닫기   | rename | 파일 이름 변경 |
  | create | 새파일 생성 | list   | 파일 나열      |
  | remove | 파일 이동   | search | 파일 찾기      |

  - remove: 파일이 저장된 디렉터리의 위치를 변경하는 곳



### 파일 내용을 변경하는 작업

- 작업의 종류

  | 작업     | 설명         | 작업     | 설명                    |
  | -------- | ------------ | -------- | ----------------------- |
  | open()   | 파일 열기    | write()  | 파일에 새로운 내용 작성 |
  | create() | 새 파일 생성 | update() | 파일 내용 일부 변경     |
  | close()  | 파일 닫기    | insert() | 파일 내용 추가          |
  | read()   | 파일 읽기    | delete() | 파일 내용 일부 삭제     |

  - 파일에 대한 권한은 파일 디스크립터를 통해서 획득

  

## 6. 파일 구조

- 순차 파일 구조, 인덱스 파일 구조, 직접 파일 구조

### 순차 파일 구조

- sequential file structure
- 파일 내용이 하나의 긴 줄로 늘어선 형태
- 카세트 테이프가 대표적
- 원하는 위치에 접근하기 위해서는 현재 위치에서 앞이나 뒤로 이동하여(forward/backward) 접근해야 함(빨리감기, 되감기)
- 순차 접근(sequential access)
- 장점
  - 데이터가 순서대로 기록되므로 공간의 낭비가 없음
  - 구조가 단순
  - 순서대로 데이터를 읽거나 저장할 때 처리속도가 매우 빠름
- 단점
  - 새로운 데이터 삽입이나 데이터 삭제시 시간이 오래걸림. 따라서 데이터의 변경이 잦은 경우에는 부적합
  - 특정 데이터로 이동하려면 앞에서부터 순서대로 움직여야 하므로 데이터 검색에 부적합



### 인덱스 파일 구조

- index file structure
- 순차 파일 구조에 인덱스 테이블을 추가하여 순차 접근 + 직접 접근 가능
- CD
- 인덱스 순차 접근(indexed sequential access)
- ISAM(Index Sequential Access Method) 파일: 인덱스 순차 접근 방식으로 구성된 파일
- 장점
  - 인덱스 테이블을 여러 개 만들면 다양한 접근이 가능. 따라서 데이터베이스와 같이 빠른 접근이 필요한 시스템에 사용
    - 성별/나이/이름/주소 등 다양한 인덱스 활용 가능

### 직접 파일 구조

- direct file structure
- 저장하려는 데이터의 특정 값에 어떤 관계를 정의하여 물리적인 주소로 바로 변환하는 파일 구조
- 특정 함수를 이용하여 직접 접근이 가능
- 이 때 사용하는 함수가 해시 함수(hash funciton)
- 장점
  - 데이터 접근이 매우 빠름
- 단점
  - 전체 데이터를 고르게 저장해 줄 해시함수를 찾아야 함
  - 해시함수를 잘 찾지 못할 경우, 또는 찾았다고 하더라도 저장 공간이 낭비되는 문제

# 디렉터리의 구조

## 1. 디렉터리의 개념

### 디렉터리의 개념

- 관련있는 파일을 하나로 모아놓은 곳
- 1개 이상의 자식 디렉터리(sub directory)를 가질 수 있고, 1개 이상의 파일을 가질 수 있음

### 디렉터리의 구성

- 여러 층으로 구성할 수 있음
- 루트 디렉터리(root directory): 최상위에 있는 디렉터리. `/`로 표현



## 2. 디렉터리 파일

- 디렉터리 또한 파일에 해당

- 일반 파일에는 데이터가 담겨있는 것처럼 디렉터리에는 파일의 정보가 담겨있고, 일반 파일과 마찬가지로 헤더를 가짐

- 헤더에는 디렉터리의 이름, 만든 시간, 접근 권한 등의 정보가 기록

- `.`와 `..`

  - `.`: 자기자신의 디렉터리
  - `..`: 상위 디렉터리

  

## 3. 경로

### 개념

- 파일이 전체 디렉터리 중 어디에 있는지를 나타내는 정보
- 서로 다른 디렉터리에는 같은 이름의 파일이 존재할 수 있으므로 디렉터리에 의한 구분이 필요

### 절대경로와 상대경로

- 절대경로(absolute path)
  - 루트 디렉터리를 기준으로 파일의 위치를 나타내는 방식(/가 루트 디렉터리를 의미)
    - ex) `/program/data/exm.c`
- 상대경로(relative path)
  - 현재 있는 위치를 기준으로 파일의 위치를 표시한 것
  - 절대경로의 예시에서 만약 사용자가 현재 `program` 디렉터리에 있다면
    `./data/exm.c` 또는 `data/exm.c`와 같이 나타낼 수 있음

### 디렉터리의 이동

- cd(change directory) 명령어 사용
- 현재위치보다 아래에 있는 데이터에 접근할 때는 상대경로 사용
- 다른 디렉터리로 이동할 때는 절대 경로 사용



## 4. 디렉터리 구조

### 초기

- 1단계 구조
- 파일이 많지 않아서 많은 디렉터리가 필요하지 않았음
- 루트 디렉터리에 새로운 디렉터리 생성 가능
- 디렉터리 안에 자식 디렉터리 생성은 불가능(최대 1단계)
  - 즉 디렉터리 안에는 파일만 존재
- 구조가 단순하나 파일이 많아지면 불편하다는 단점

### 이후

- 다단계 디렉터리 구조 등장
- 루트 디렉터리를 시작점으로 여러 단계의 디렉터리가 가지처럼 뻗은 형태(트리 디렉터리 구조)
- 확장에 제약이 없음
- 디렉터리에 파일과 디렉터리 모두 저장이 가능
- 엄밀하게 트리는 순환이 없는 그래프를 의미하지만, 오늘날의 디렉토리는 트리구조라고는 하지만 순환이 존재
  - 윈도우의 바로가기

## 5. 마운트

### 파티션

- 논리적인 디스크 분할
- 윈도우에서 C, D, E 드라이브는 파티션에 붙여진 이름
- 윈도우에서 각 파티션이 따로 보이는 이유는 파티션마다 파일 테이블이 따로 존재하기 때문
- 이렇게 파티션이 사용자에게 노출되면 파티션이 늘어남에따라 불편도 증가
- 유닉스의 경우 파일 테이블의 크기에 제한이 없고, 하나의 파일 테이블에 여러 개의 디스크 혹은 파티션을 통합하여 관리가 가능
  - 마운트는 이와 같이 여러 개의 파티션을 통합하는 명령어

### 마운트

- 여러 개의 파티션을 통합하는 명령어
- 마운트를 하고 나면 디렉터리를 이동할 때마다 사실은 다른 파티션으로 넘어가지만 사용자는 이를 알지 못함
- 마운트 연결 지점(mounting point): 어떤 파티션을 기존의 파티션에 연결하는 지점
- CD-ROM이나 USB와 같은 외부 저장장치도 마운트/마운트해제(un-mount) 가능

# 디스크 파일 할당

## 1. 연속할당과 불연속 할당

- 파일 시스템
  - 전체 디스크 공간을 같은 크기로 나누고 각 공간에 주소를 붙여 관리
  - 같은 크기로 나뉜 공간 하나를 '블록'이라고 하며, 보통 1~8KB 정도의 크기
  - 파일 시스템은 파일 테이블을 관리
  - 파일 테이블은 파티션당 하나씩 존재하고, 각 파티션의 맨 앞부분에 위치
- 연속할당과 불연속 할당
  - 하나의 파일이 사용하는 여러 개의 블록을 어떻게 연결하는지에 따라 구분
  - 연속할당
    - contiguous allocation
    - 파일을 구성하는 데이터를 디스크상에 연속적으로 배열하는 방식
    - 디스크에 남은 공간 중 파일의 크기와 맞는 연속된 공간이 없는 경우에는 연속할당이 불가능하기 때문에 실제로는 사용되지 않음
  - 불연속 할당
    - non-contiguous allocation
    - 비어있는 블록에 데이터를 분산하여 저장하고, 이에 관한 정보를 파일 시스템이 관리하는 방식
    - 연결 할당, 인덱스 할당

### 연결 할당

### 인덱스 할당



## 2. 디스크의 빈 공간 관리



# 유닉스 파일의 특징

## 1. 유닉스의 실행 파일





---

# Q.

- 파일 시스템 vs 파일 관리자 개념 명확히 하기
- 파일 search가 변경하는 작업?
- 휴지통도 하나의 디렉터리? remove가 파일 제거가 아니라 이동이라니까 이상해서
- 파일 자체를 변경하는 작업과 파일의 내용을 변경하는 작업에 겹치는 내용이 있는데 무슨 차이가 있는지 모르겠음
- 책(537p하단~538p)에 디스크립터 자세히좀.. 그냥 권한인줄 알았는데 왜 얘를 움직이면서 데이터를 찾아야됨?
- 인덱스 파일 구조의 단점은 따로 없을까? 인덱스 테이블이라는걸 만들어야 하니 메모리 상의 비효율?
- 하나의 파일이 여러 블록에 나눠지면 어케됨?
- 파티션 vs 디스크
- 마운트는 유닉스만 되는건가?