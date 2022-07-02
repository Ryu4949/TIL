# 08_가상 메모리



# 가상 메모리의 개요

## 1. 가상 메모리 시스템

### 가상 메모리의 개념

- 물리 메모리의 크기와 상관없이 프로세스에 커다란 메모리 공간을 제공하는 기술. 이 기술로 프로세스는 운영체제가 어디에 있는지, 물리 메모리의 크기가 어느 정도인지 신경쓰지 않고 메모리를 사용할 수 있음
- 메모리의 크기는 컴퓨터마다 다른데, 운영체제가 만약 물리 메모리의 크기에만 의존한다면 크나큰 제약이 됨

### 가상 메모리의 크기와 주소

- 가상 메모리 시스템의 모든 프로세슨느 물리 메모리와 별개로, 자신이 메모리의 어느 위치에 있는지와 상관없이 0번지부터 시작하는 연속된 메모리 공간을 가짐
  - 논리 주소와의 차이는 논리 주소는 물리 메모리의 주소 공간에 비례한다는 점
- 가상메모리는 이론적으로 무한의 크기를 가짐. 그러나 실제로는 가상메모리의 최대 크기는 그 컴퓨터 시스템이 가진 물리 메모리의 최대 크기로 한정되고 이는 CPU의 비트에 따라 결정됨
  - 즉 32bit CPU라면 약 4GB가 메모리의 최대 크기
  - 실제로는 사용할 수 있는 최대 크기에 제약이 있음
- 스왑 영역
  - 실제의 제약에도 불구하고 무한의 크기가 있는 것처럼 구현하는 방법
  - 하드디스크에 존재하지만 메모리 관리자가 관리하는 영역으로, 메모리의 일부
  - 메모리 관리자는 물리 메모리의 부족한 부분을 스왑 영역으로 보충
  - 물리 메모리가 가득 찼을 때 일부 프로세스를 스왑 영역으로 보내고(스왑 아웃), 몇 개의 프로세스가 작업을 마침녀 스왑 영역에 있는 프로세스를 메모리로 가져옴
- 가상 메모리의 크기
  - 물리 메모리 + 스왑 영역
- 동적 주소 변환(Dynamic Address Translation, DAT)
  - 메모리 관리자가 물리 메모리와 스왑 영역을 합쳐서 프로세스가 사용하는 가상 주소를 실제 메모리의 물리 주소로 변환하는 것
  - DAT를 거치고 나면 프로세스가 아무 제약 없이 사용자의 데이터를 물리 메모리에 배치할 수 있음

### 가상 메모리의 메모리 분할 방식

- 가상 메모리 시스템에서는 운영체제(물리 주소 0번지)를 제외한 나머지 메모리 영역을 일정 크기로 나누어 일반 프로세스에 할당
- 방식은 크게 가변 분할 방식과 고정 분할 방식
  - 가변 분할 방식: 세그먼테이션
  - 고정 분할 방식: 페이징
- 세그먼테이션 기법은 외부 단편화의 문제, 페이징은 페이지 관리 상 어려움으로 인해 세그먼테이션-페이징 혼용 기법이 주로 사용됨

## 2. 매핑 테이블의 필요성과 역할

- 매핑 테이블: 가상 주소가 물리 메모리의 어느 위치에 있는지 알 수 있도록 정리한 표
- 페이징기법: 페이지 매핑 테이블(page mapping table) 또는 페이지 테이블
- 세그먼테이션 기법: 세그먼테이션 매핑 테이블(segmentation mapping table) 또는 세그먼테이션 테이블

# 페이징 기법

> 고정 분할 방식으로 메모리를 분할하여 관리하는 기법

## 1. 페이징 기법의 구현

	- 고정 분할 방식을 이용한 가상 메모리 관리 기법
	- 물리 주소 공간을 같은 크기로 나누어 사용
	- 페이지와 프레임은 각각 번호를 매겨 관리
	- 페이지와 프레임은 크기가 같기 때문에 페이지는 어떤 프레임에도 배치될 수 있음
	- 모든 페이지의 위치 정보는 페이지 테이블에 담겨있음
	- 페이지 테이블은 하나의 열(column)으로 구성되며, 페이지 순서로 구성되어 있음
	- 각 행의 숫자는 해당 페이지가 있는 프레임을 의미하고, 숫자가 아닌 `invalid`는 물리 메모리가 아닌 스왑 영역에 있음을 의미
	- PTE는 페이지 번호가 순서대로 저장되는 경우에는 프레임 번호로만 구성되고, 그렇지 않은 경우에는 페이지 번호+프레임 번호로 구성
 - 용어
   - 페이지(page): 가상 주소의 분할된 각 영역
   - 프레임(frame): 물리 메모리의 각 영역
   - 페이지 테이블(page table): 페이징 기법에서의 매핑테이블로, 가상 주소가 물리 메모리의 어느 위치에 있는지 알 수 있도록 정리한 표
   - 페이지 테이블 엔트리(Page Table Entry, PTE): 페이지 테이블의 각 행을 의미. 페이지 번호, 프레임 번호로 구성. 즉 페이지 테이블은 페이지 테이블 엔트리의 집합

## 2. 페이징 기법의 주소 변환

### 주소 변환 과정

- **read**: 프로세스가 30번지의 내용을 읽으려고 하는 상황
  - 30번지가 몇 페이지의 어느 위치에 있는지를 찾음(페이지3의 0번째 위치라고 가정)
  - 페당 페이지로 가서 그 페이지가 몇번째 프레임에 있는지를 알아냄(프레임 1이라고 가정)
  - 최\종적으로 프레임 1의 0번째 위치에 접근. 이 주소가 가상 주소 30번지의 물리 주소
- **write**: 프로세스가 18번지에 값을 저장하려고 하는 상황
  - 가상 주소 18번지가 어느 페이지/어느 위치에 있는지를 찾음(페이지 1, 8번째 위치)
  - 페이지1로 가서 해당 페이지가 있는 프레임을 알아 냄(프레임 3)
  - 프로세스가 저장하려는 값을 프레임 3의 8번째 위치에 저장

### 정형화된 주소 변환

- 가상 주소
  - `VA = <P, D>`
  - `VA`: 가상주소(virtual address)
  - `P`: 페이지(page)
  - `D`: 페이지의 처음 위치에서 해당 주소까지의 거리(distance), 오프셋(offset)이라고도 함
- 물리 주소
  - `PA = <F, D>`
  - `PA`: 물리 주소, 실제 주소
  - `F`: 프레임(frame)
  - `D`: distance
- 페이징 기법의 주소 변환 과정
  - `VA = <P, D> -> PA = <F, D>`
  - 주소를 변환할 때 P를 F로 변환하고, D는 변경하지 않는데 이는 페이지와 프레임의 크기를 같게 나누기 때문

### 16bit CPU의 주소 변환 예

- 위의 내용까지는 한 페이지를 10B로 나눈 경우를 가정한 것
- 컴퓨터는 보통 2진법을 사용하기 때문에 한페이지는 보통 2의 n승 형태이고, 페이지의 크기는 다양하게 나타남
- 이런 경우 가상 주소를 `<P, D>`로 변환하는 공식은 아래와 같음
  - P = (가상주소 / 한 페이지의 크기)의 몫
  - D = (가상 주소 / 한 페이지의 크기)의 나머지
- 16bit CPU의 컴퓨터
  - 한 페이지의 크기 2^10B로 가정
  - 한 프로세스가 사용할 수 있는 가상 메모리의 크기는 2^16B
  - 사용자가 사용할 수 있는 가상공간은 0번지 ~ 2^16-1 번지

## 3. 페이지 테이블 관리

## 4. 페이지 테이블 매핑 방식



# 세그먼테이션 기법

## 1. 세그먼테이션 기법의 구현

## 2. 세그먼테이션 기법의 주소 변환

# 세그먼테이션-페이징 혼용 기법

## 1. 메모리 접근 권한

## 2. 세그먼테이션-페이징 혼용 기법의 도입

## 3. 세그먼테이션-페이징 혼용 기법의 주소 변환



# 캐시 매핑 기법





---

# Q. 

- 한 페이지에 주소를 여러개 가진다는게 무슨 말..