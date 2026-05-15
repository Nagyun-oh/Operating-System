# Operating System Projects

운영체제 수업에서 진행한 Linux Kernel 기반 과제들을 정리한 저장소입니다.

본 프로젝트는 Linux 환경에서 C언어를 사용하여 시스템 콜 추가, 커널 모듈 작성, 시스템 콜 후킹, 프로세스 추적, CPU 스케줄링, 가상 메모리 분석, 페이지 교체 알고리즘, `/proc` 파일 시스템, FAT 기반 파일 시스템 구현을 실습하는 것을 목표로 합니다.

단순히 운영체제 개념을 이론적으로 학습하는 데 그치지 않고, Linux Kernel 내부 구조를 직접 수정하고 User Space와 Kernel Space 사이의 동작 흐름을 코드로 구현하며 운영체제의 핵심 원리를 이해하고자 했습니다.

## Overview

이 저장소는 총 4개의 Assignment로 구성되어 있습니다.

- **Assignment 1**: System Call 추가 및 System Call Table Hooking
- **Assignment 2**: Process 추적, Process/Thread 성능 비교, CPU Scheduling
- **Assignment 3**: Virtual Memory Area 추적 및 Page Replacement Algorithm
- **Assignment 4**: `/proc` 파일 시스템 구현 및 FAT 기반 User-level File System 구현

## Tech Stack

| Category | Tech |
|---|---|
| Language | C |
| OS | Linux / Ubuntu |
| Kernel | Linux Kernel 5.4.282 |
| Build | Makefile |
| Debugging | dmesg, GDB |
| Virtualization | QEMU / VirtualBox |
| Kernel Programming | Kernel Module, System Call, System Call Table |
| OS Concepts | Process, Thread, Scheduling, Virtual Memory, Page Replacement, File System |

## Project Structure

```text
Operating-System-Projects/
├── Assignment1/
│   └── README.MD
├── Assignment2/
│   └── README.MD
├── Assignment3/
│   └── README.MD
├── Assignment4/
│   └── README.MD
└── README.MD
```

## Assignments

| Assignment | Topic | Main Features |
|---|---|---|
| Assignment 1 | System Call & Hooking | `os_ftrace` 시스템 콜 추가, 커널 모듈 작성, System Call Table Hooking |
| Assignment 2 | Process & Scheduling | `task_struct` 기반 프로세스 정보 추적, `fork()` / `pthread` 성능 비교, CPU 스케줄링 알고리즘 구현 |
| Assignment 3 | Memory Management | 프로세스의 VMA 정보 출력, Page Replacement Algorithm 시뮬레이터 구현 |
| Assignment 4 | File System | `/proc` 파일 시스템 기반 프로세스 정보 출력, FAT 기반 User-level File System 구현 |

## Assignment 1. System Call & Hooking

Assignment 1에서는 Linux Kernel에 새로운 시스템 콜인 `os_ftrace`를 추가하고, 커널 모듈을 통해 시스템 콜 테이블을 후킹하는 과정을 구현했습니다.

### Main Features

- Linux Kernel Source 수정
- 새로운 System Call 등록
- `os_ftrace` 시스템 콜 구현
- Kernel Module 작성
- System Call Table Hooking
- 기존 System Call Pointer 백업 및 복원
- `openat`, `read`, `write`, `lseek`, `close` 등 I/O 관련 시스템 콜 추적

### What I Learned

이 과제를 통해 User Space에서 호출한 시스템 콜이 Kernel Space에서 어떻게 처리되는지 이해할 수 있었습니다.  
또한 시스템 콜 테이블을 직접 수정하는 과정에서 커널 내부 주소 접근, Write Protection 해제, 원본 함수 포인터 백업 및 복원 과정의 중요성을 배웠습니다.

특히 잘못된 함수 주소 참조나 부적절한 후킹 방식이 Kernel Panic으로 이어질 수 있다는 점을 경험하며, 커널 프로그래밍에서는 작은 실수도 시스템 전체에 치명적인 영향을 줄 수 있음을 체감했습니다.

## Assignment 2. Process & Scheduling

Assignment 2에서는 프로세스 정보를 추적하는 커널 모듈을 작성하고, Process와 Thread의 성능을 비교했습니다. 또한 CPU Scheduling Algorithm을 직접 구현하여 각 알고리즘의 성능을 분석했습니다.

### Main Features

- `task_struct` 기반 프로세스 정보 탐색
- 프로세스 상태, Context Switch 횟수, 부모/형제/자식 프로세스 정보 출력
- `fork()` 기반 다중 프로세스 연산 프로그램 구현
- `pthread` 기반 다중 스레드 연산 프로그램 구현
- Process와 Thread의 수행 시간 비교
- CPU Scheduling Algorithm 구현

### Scheduling Algorithms

- FCFS, First Come First Served
- SJF, Shortest Job First
- SRTF, Shortest Remaining Time First
- RR, Round Robin

### Evaluation Metrics

각 스케줄링 알고리즘에 대해 다음 지표를 계산했습니다.

- Average Waiting Time
- Average Turnaround Time
- Average Response Time
- CPU Utilization
- Gantt Chart

### What I Learned

이 과제를 통해 Process와 Thread의 생성 및 실행 비용 차이를 직접 비교할 수 있었습니다.  
또한 CPU Scheduling Algorithm을 구현하면서 동일한 입력 데이터라도 스케줄링 방식에 따라 대기 시간, 반환 시간, 응답 시간이 크게 달라질 수 있음을 확인했습니다.

## Assignment 3. Memory Management

Assignment 3에서는 특정 PID를 입력받아 해당 프로세스의 가상 메모리 영역 정보를 출력하는 커널 모듈을 구현했습니다. 또한 Page Replacement Algorithm 시뮬레이터를 작성하여 페이지 교체 정책에 따른 Page Fault 발생률을 비교했습니다.

### Main Features

- PID 기반 프로세스 탐색
- 프로세스 이름 및 PID 출력
- Code 영역 주소 출력
- Data 영역 주소 출력
- Heap 영역 주소 출력
- Memory Mapping File Path 출력
- Page Replacement Algorithm 시뮬레이터 구현

### Page Replacement Algorithms

- OPT, Optimal
- FIFO, First In First Out
- LRU, Least Recently Used
- Clock, Second-Chance Algorithm

### What I Learned

이 과제를 통해 프로세스가 실행될 때 가상 메모리 공간이 코드, 데이터, 힙, 매핑 영역 등으로 나뉘어 관리된다는 점을 코드 레벨에서 확인할 수 있었습니다.

또한 Page Replacement Algorithm을 직접 구현하면서 Page Frame 수, Reference String, 교체 정책에 따라 Page Fault Rate가 달라지는 과정을 이해할 수 있었습니다.

## Assignment 4. File System

Assignment 4에서는 Linux의 `/proc` 파일 시스템을 활용하여 커널 내부 프로세스 정보를 User Space에서 확인할 수 있도록 구현했습니다. 또한 FAT 구조를 기반으로 하는 User-level File System을 직접 설계하고 구현했습니다.

### Assignment 4-1. Proc File System

`/proc/proc_학번/processInfo` 형태의 가상 파일을 생성하고, 해당 파일을 읽을 때 Kernel 내부의 `task_struct` 정보를 탐색하여 프로세스 정보를 출력하도록 구현했습니다.

#### Main Features

- `/proc` 가상 파일 생성
- 특정 PID 입력 처리
- PID, PPID, UID, GID 출력
- `utime`, `stime`, `state` 출력
- 프로세스 이름 출력
- `task_struct` 기반 프로세스 정보 탐색

### Assignment 4-2. FAT-based File System

FAT, File Allocation Table 구조를 기반으로 사용자 수준의 파일 시스템을 구현했습니다.

#### Main Features

- 파일 생성
- 파일 삭제
- 파일 읽기
- 파일 쓰기
- 파일 목록 출력
- FAT Table 기반 블록 연결
- File Entry 관리
- Data Block 관리
- 프로그램 종료 시 파일 시스템 상태 저장
- 프로그램 재실행 시 기존 파일 시스템 상태 복원

### What I Learned

이 과제를 통해 `/proc` 파일 시스템이 실제 디스크 파일이 아니라 커널 내부 정보를 User Space에 제공하기 위한 가상 파일 시스템이라는 점을 이해할 수 있었습니다.

또한 FAT 기반 파일 시스템을 직접 구현하면서 파일 데이터가 블록 단위로 저장되고, 여러 블록이 FAT Table을 통해 연결되는 방식을 학습했습니다.  
파일 생성, 삭제, 읽기, 쓰기 기능을 구현하면서 파일 시스템에서 블록 관리와 메타데이터 관리가 얼마나 중요한지 알 수 있었습니다.

## Key Learning Points

이 프로젝트를 통해 다음과 같은 운영체제 핵심 개념을 학습했습니다.

- Linux Kernel Source 수정
- System Call 추가
- Kernel Module 작성
- System Call Table Hooking
- User Space와 Kernel Space의 동작 흐름
- `task_struct` 기반 프로세스 정보 탐색
- Process와 Thread의 차이
- CPU Scheduling Algorithm
- Virtual Memory Area 구조
- Page Replacement Algorithm
- `/proc` Virtual File System
- FAT File System 구조
- Kernel Panic 원인 분석 및 디버깅

## Troubleshooting

### 1. Kernel Panic During System Call Hooking

시스템 콜 후킹 과정에서 원본 시스템 콜 주소를 잘못 참조하거나, 복원 과정을 정확히 처리하지 않으면 Kernel Panic이 발생할 수 있었습니다.

이를 해결하기 위해 다음을 신중하게 처리했습니다.

- 원본 System Call Pointer 백업
- Hooking 대상 주소 검증
- Write Protection 해제 및 복구
- Module 제거 시 원래 함수 포인터 복원

### 2. Process and Thread Performance Difference

처음에는 Process와 Thread의 차이를 개념적으로만 알고 있었지만, `fork()`와 `pthread`를 각각 사용하여 동일한 연산을 수행해 보면서 생성 비용과 자원 공유 방식의 차이를 더 명확히 이해할 수 있었습니다.

### 3. Page Replacement Result Analysis

Page Replacement Algorithm은 단순히 구현하는 것보다, 동일한 Reference String에 대해 각 알고리즘의 Page Fault Rate를 비교하는 과정이 중요했습니다.  
이를 통해 알고리즘별 특성과 성능 차이를 수치적으로 분석할 수 있었습니다.

## How to Build

각 Assignment 폴더로 이동한 뒤 Makefile을 사용하여 빌드할 수 있습니다.

```bash
cd Assignment1
make
```

커널 모듈의 경우 다음과 같이 로드 및 제거할 수 있습니다.

```bash
sudo insmod module_name.ko
sudo rmmod module_name
```

커널 로그는 다음 명령어로 확인할 수 있습니다.

```bash
dmesg
```

## Notes

이 프로젝트는 운영체제 수업 및 Linux Kernel 학습을 위한 교육용 프로젝트입니다.

특히 System Call Table Hooking은 실제 운영 환경에서 사용하기에 안전하지 않으며, 본 저장소에서는 Linux Kernel 내부 구조를 이해하기 위한 실습 목적으로만 사용했습니다.

## Repository Purpose

이 저장소는 운영체제 수업에서 수행한 과제와 구현 과정을 정리하기 위한 목적입니다.  
각 Assignment 폴더에는 구현 내용, 실행 결과, 문제 해결 과정, 고찰을 포함한 개별 README가 정리되어 있습니다.

## Author

- Oh Nagyun
- Kwangwoon University
- Computer Engineering
