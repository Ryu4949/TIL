# 10_입출력 시스템과 저장장치



# 입출력 시스템

- 필수장치: CPU, 메모리
- 주변장치: 입출력장치, 저장장치
  - 저속주변장치, 고속 주변장치

## 1. 입출력장치와 채널

	### 저속 주변장치

- 메모리와 주변장치 사이에 오가는 데이터의 양이 적어 데이터 전송률이 낮은 장치
- 키보드

### 고속 주변장치

- 데이터 전송률이 높은 장치
- 그래픽카드, 하드디스크 등

### 채널 공유와 채널 분리

- 채널
  - 여러 주변장치는 메인보드 내의 버스로 연결됨
  - 버스에는 다양한 종류의 장치가 연결되므로, 버스가 1개 뿐이라면 병목 현상이 발생하기 때문에 여러 버스를 묶어서 사용
  - 이 때 데이터가 지나다니는 하나의 통로를 채널이라고 함
  - 예를 들어 4채널 버스는 4개의 주변 장치가 동시에 데이터를 주고 받을 수 있음을 의미
- 채널 공유와 채널 분리
  - 채널이 여러개라고 하더라도 어떤 채널을 모든 주변장치가 공유한다면 데이터 전송 속도가 저하됨
  - 따라서 여러 채널을 효율적으로 사용하기 위해 전송속도가 비슷한 장치끼리 묶어 장치별로 채널을 할당하게 되는데, 이것을 채널 분리라고 함



## 2. 입출력 버스의 구조

### 초기의 구조

- 주변장치 수 적음, CPU와 메모리 속도 느림

- 모든 장치가 하나의 버스로 연결

- 폴링(polling): CPU가 작업 진행 중에 입출력 명령을 만나면 직접 입출력 장치에서 데이터를 가져오는 것

  - 작업 효율이 매우 떨어지는 방식

  

### 입출력 제어기를 사용한 구조

- 컴퓨터 기술의 발전으로 CPU와 메모리 성능이 급격히 향상되고 주변장치의 종류 다양화

- 이에 따라 CPU가 폴링 방식으로 주변장치를 관리하기 어려워져서 모든 입출력을 입출력 제어기(I/O controller)에 맡기는 구조로 변경

- 입출력 제어기

  - 2개의 채널 즉, 메인버스와 입출력 버스로 나뉨
  - 메인버스: CPU와 메모리가 사용
  - 입출력버스: 주변장치가 사용

  

### 입출력 버스의 분리

- 입출력 제어기로 작업효율이 향상되었으나, 저속 주변장치로 인해 고속 주변장치의 데이터 전송이 느린 문제(ex 그래픽카드)
- 이러한 문제를 해결하기 위해 입출력 버스를 고속 입출력 버스와 저속 입출력 버스로 분리
- 고속 주변장치는 고속 입출력버스에, 저속 주변장치는 저속 입출력버스에 연결하며 두 버스 사이의 데이터 전송은 채널 선택기(channel selector)가 관리
  - 채널 선택기는 두 버스의 데이터 전송 속도를 조절

## 3. 직접 메모리 접근

## 4. 인터럽트

## 5. 버퍼링



# 디스크 장치

## 1. 디스크 장치의 종류

## 2. 디스크 장치의 데이터 전송 시간

## 3. 디스크 장치 관리

## 4. 네트워크 저장장치



# 디스크 스케줄링

## 1. FCFS 디스크 스케줄링

## 2. SSTF 디스크 스케줄링

## 3. 블록 SSTF 디스크 스케줄링

## 4. SCAN 디스크 스케줄링

## 5. C-SCAN 디스크 스케줄링

## 6. LOOK 디스크 스케줄링

## 7. C-LOOK 디스크 스케줄링

## 8. SLTF 디스크 스케줄링





# RAID

## 1. RAID의 개요

## 2. RAID 0(스트라이핑)

## 3. RAID 1(미러링)

## 4. RAID 2

## 5. RAID 3

## 6. RAID 4

## 7. RAID 5

## 8. RAID 6

## 9. RAID 10

## 10. RAID 50과 RAID 60



# 하드웨어의 규격과 발전