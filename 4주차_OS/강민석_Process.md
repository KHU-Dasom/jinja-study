# 프로세스(Process)

쉽게 말하면 **실행중인 프로그램**이다.

디스크에 있는 프로그램이 메모리에 로드되면 프로세스가 된다.

여기서 말하는 프로그램이란 `.exe`파일 같이 아직 실행되지 않은 명령어 집합체를 말한다.

## 1. 프로세스 문맥(Process Context)

- 여러 프로세스가 타이머 인터럽트에 의해 짧은 시간동안 CPU를 점유하며 서로 제어권을 넘겨주는 시분할 시스템 환경에 의해 나타남.
- 서로 제어권을 넘길때 어디까지 명령을 수행했는지, 상태에 대한 정보를 나타낼 필요가 있었음.

**프로세스가 어떤 상태에서 수행되고 있는지 정확히 규명하기 위해 필요한 정보들.**

프로세스 문맥은 **하드웨어 문맥, 프로세스의 주소공간, 커널상의 문맥**으로 나뉨.

- 하드웨어 문맥: PC(Program Counter)와 각종 레지스터에 저장하고 있는 값들.

> Program Counter는 다음에 실행될 명령어의 주소를 가지고 있는 레지스터. 인텔에서는 IP(Instruction Pointer)로도 불림.

- 프로세스 주소공간: **코드, 데이터, 스택, 힙**으로 구성된 프로세스만의 독자적 주소 공간.

![image](https://user-images.githubusercontent.com/30401054/205493183-1aed92d1-15f9-4e13-9c0f-8e6976699ab5.png)

코드는 프로그램의 코드가, 데이터는 전역변수 같은 데이터가, 스택에는 함수나 지역변수가 저장되어 있다.

- 커널상의 문맥: PCB(Process Control Block), Kernel Stack(커널상의 주소)를 말한다.

![image](https://user-images.githubusercontent.com/30401054/205495276-7999fb79-5257-4c06-a234-04747889146f.png)

PCB는 밑에서 설명, Kernel Stack이란 간단하게 프로세스 자체에서 처리할 수 없는것들은 운영체제 부탁하는데 이를 **시스템 콜**이라고 부른다. 시스템 콜이 발생하면 PC는 커널의 Code를 가리키고 커널의 함수를 호출한다. 이때 각 프로세스마다 호출하는 Code를 관리하기 위해서 Kernel Stack을 갖게 됨. 따라서 해당 프로세스의 커널 스택의 값이 뭔지 필요하게 됨.

## 2. 프로세스의 상태

PCB를 알기 전에 프로세스 상태에 대해 알고 있어야한다.

### Five-State Process Model

보통 우리는 이 모델을 가지고 설명을 많이한다.

시분할 시스템 환경에서 프로세스의 상태는 **New, Ready, Running, Waiting, Terminated**로 구분이 가능하다.

![image](https://user-images.githubusercontent.com/30401054/205495843-8cdbfe9f-67f2-4d11-83cb-5bd48e61cb29.png)

- New: 프로세스가 처음 생성된 단계, 메모리 할당 및 PCB와 스택 메모리 할당 및 초기화 등의 작업.
- Ready: 프로세스가 CPU에 할당되기를 기다리는 상태.
- Running: 프로세스가 할당되어 CPU를 잡고 명령을 수행중인 상태
- Waiting: 프로세스가 어떠한 이벤트가 발생하기를 기다리는 상태
- Terminated: 프로세스가 실행을 마치고 완전히 제거되진 않은 상태.

### State Transitions

> Dispatcher란 OS 프로그램의 일부로, 실행중인 프로그램을 중단하고 다른 프로그램을 실행시키도록 하는 프로그램이다.

- New -> Ready

OS가 Ready 단계로 넘어가길 허락(Admit)해야 넘어갈 수 있다.

이러한 이유로 메모리 자원 문제 및 CPU 문제가 있기 때문이다. 

- Ready -> Running

Dispatcher가 대기하고 있는 프로그램 중 적절한 프로그램을 골라 Running 상태로 옮김.

- Running -> terminated

OS는 프로그램이 종료하게 되면 exit상태를 거쳐 terminated로 옮김.

- Running -> Ready

프로그램의 할당 시간이 길어지면 timeout이 걸려(interrupt) Ready상태로 돌아간다.

- Running -> Blocked

프로그램 실행중 I/O Request, 시스템콜 등 작업을 요청하면 Blocked상태로 변경하여 대기한다.

- Blocked -> Ready

기다리던 이벤트가 발생하면 **Running**이 아닌 **Ready**상태로 간다.

중기 스케줄러로 인해 Seven-State Process Model이라는것도 생겨났으니 참고하면 된다.

## 3. PCB(Process Control Block)

![image](https://user-images.githubusercontent.com/30401054/205495631-e630a30a-f2c0-4daf-a0a6-6f9a084779c3.png)

커널 Data영역에 있는 프로세스에 관한 정보들

- OS가 관리하는 정보: (1) 및 스케줄링 및 우선순위에 대한 정보를 가지고 있다.

Pointer: 프로세스를 큐에서 관리하기에 이를 나타내기 위한 포인터
Process state: 위에서 말한 Ready, Running 등 프로세스 상태
Process number(PID): 프로세스를 구별하기 위한 숫자.

![image](https://user-images.githubusercontent.com/30401054/205497214-8a1ece70-d005-4a69-80fc-1ec03ccffa24.png)

PCB들은 메모리에 존재하고 상태에 따라 Ready Queue, Event Queue에서 관리 된다.

<details>
<summary> Ready Queue </summary>

Ready 상태에 있는 프로세스만 모아둔 큐를 말한다.

- **Scheduler**가 Ready큐에서 최우선순위 프로세스를 선택.
- 선택된 프로세스를 **Dispatch**해서 CPU가 실행한다.(Running)

</details>

<details>
<summary> Blocked(Waiting or Event) Queue - 대기큐 </summary>

Blocked상태에 있는 프로세스만 모아둔 큐

- 각각의 `Device driver`에 존재하는 큐
- 키보드, 디스크, 네트워크, 세마포어, 등에 대기큐가 존재한다.
- 예를 들어 I/O함수 등을 호출하면 Blocked 상태가 되며 PCB를 해당 장치의 대기큐로 이동시킴.
- 이벤트가 발생하면 Device driver내의 함수는 대기큐에서 해당 PCB를 찾고 대기큐에서 제거 후 Ready Queue로 넘김.(Ready)

</details>

- CPU 수행 관련 하드웨어 값: (2)에 해당. 해당 프로세스가 어떤 값을 가지고 있었는지에 대한 정보

ProgramCounter: 다음에 실행될 명령어의 위치를 가리키는 레지스터
Registers: 프로세스 실행 중 사용된 레즈스터 값들 (Accumulator, Index register...)

- 메모리 관련: (3)에 해당. Code, Data, Stack의 위치 정보. 프로세스가 물리적으로 어디에 있는지 나타내는 위치정보.
- 파일 관련: (4)에 해당. 어떤 파일을 열어놨는지 등에 대한 정보

**참고로 Blocked에 있는 프로세스를 찾을때 포트번호로 구별하여 PCB를 찾는다.**

## 4. Context Switch

![image](https://user-images.githubusercontent.com/30401054/205498368-14bf6f58-30f9-4b07-add4-1d7baa680bb5.png)

**멀티 프로세스 환경에서 프로세스가 실행되다가 인터럽트가 발생해서 CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정**

운영체제는 CPU를 내어주는 A프로세스의 상태를 A프로세스의 PCB에 저장하고, CPU를 새롭게 얻는 B프로세스의 상태를 B프로세스의 PCB에서 읽어온다. CPU입장에서는 Context는 PCB이고, 이가 바뀌는것이 **Context Switch**다. 이는 OS 스케줄러에서 수행한다.

인터럽트나 시스템콜이 발생한다해서 반드시 <u>Context Switch가 발생하는것은 아니다.</u>

![image](https://user-images.githubusercontent.com/30401054/205498888-c30c9d2f-a4c6-4a3d-9e6c-69c42d51aa10.png)

- Context Switch가 발생하지 않는 경우

프로세스A -> interrupt or system call -> kernelMode -> 프로세스A : Context Switch 없음.

- Context Switch가 발생하는 경우

프로세스A -> interrupt or I/O system call -> kernelMode -> **Context Switch** -> 프로세스B

> Context Switch시 Cache memory flush라는 프로세스가 바뀔때 Cache를 초기화 시켜 오버헤드가 큼. 단점으로 캐시 히트가 줄어들고 초반 프로세스 작업 속도가 느려질 수 있다.

> 또한 Context Switch을 하는동안 CPU는 아무일을 하지 않는 시간이 발생하는데 이를 오버헤드라고 부름.

## 5. Process Management









## Reference

- <https://velog.io/@zooneon/OS-Process-State#-dispatcher> 프로세스 상태모델
- <https://ws-pace.tistory.com/20> PCB 등
- <https://rebro.kr/172> Context Switch