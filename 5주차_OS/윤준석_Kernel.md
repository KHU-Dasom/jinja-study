# **Kernel**

## **🖥️ 커널이란?**

![kernel](https://static.javatpoint.com/blog/images/what-is-kernel.png)

- 하드웨어와 가장 가까이 있는 일종의 프로그램으로 항상 메모리에 상주하며 하드웨어와 응용 프로그램 사이에서 인터페이스를 제공하는 역할을 함.
- 즉, 컴퓨터 자원들을 관리하는 역할을 함.
- 다만, 커널은 항상 컴퓨터 자원들을 바라만 보고 있기 때문에 사용자(user)와의 상호작용은 지원하지 않음 -> 사용자와의 직접적인 상호작용을 위한 shell이라는 프로그램이 존재함.

![linux_kernel](https://live.staticflickr.com/7314/8730835900_b6d6d429a8_b.jpg)

![windowsnt_kernel](https://csdl-images.ieeecomputer.org/mags/co/1998/10/figures/rx0401.gif)
### **Shell**

![shell](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbuMPbF%2FbtqZlQMBSNx%2F1p80FhTQppyLGtUuKzOb60%2Fimg.png)

- 사용자와 커널 사이에 위치하여 사용자가 커널에게 명령을 내리는 역할을 함.
- 정확하게는 응용 프로그램의 명령어와 커널이 대화할 수 있도록 해줌. -> 이를 명령어 해석기라고 부름.
- 커널마다 각각의 쉘을 가지고 있으며 종류도 다양함. Linux 커널의 경우 대표적으로 `sh`, `csh`, `tcsh`, `ksh`, `bash`, `zsh` 등이 있음.

![linux_shell](https://user-images.githubusercontent.com/39883609/209477864-f3eff255-133e-4ca1-94dc-49bcca1d766e.png)

#### **Bourne Shell (`/bin/sh`)**

- 가장 기본적인 쉘, 유닉스 최초의 쉘.
- 최초의 쉘이라 기능이 매우 단순함.
- 일반 유저의 쉘 프롬프트는 `$`로 표시되며, root 유저의 쉘 프롬프트는 `#`로 표시됨.

![bourne_shell](https://user-images.githubusercontent.com/39883609/209478833-427e6836-f5d4-4d81-9d27-1f332e8a6ac7.png)

#### **C Shell (`/bin/csh`)**

- C언어로 작성된 쉘로 Bourne Shell의 단점을 보완하기 위해 개발됨.
- C언어 구문과 유사한 shell 문법을 가짐.

#### **TC Shell (`/bin/tcsh`)**
- C shell에 명령행 완성과 명령행 편집 기능을 추가한 쉘.

#### **Korn Shell (`/bin/ksh`)**
- Bourne Shell의 단점을 보완하기 위해 개발된 쉘로 `csh`의 기능들을 포함하여 개발됨.
- Unix 계열에서 많이 사용됨.

#### **Bourne Again Shell (`/bin/bash`)**

- Bourne Shell의 단점을 보완하기 위해 개발된 쉘로 현재 리눅스에서 표준으로 채택된 쉘임.
- `csh`와 `ksh`의 기능들을 통합/보완하여 개발됨.
- `sh`에 비해 확장된 기능을 제공함.
    - `history`, alias, 자동 완성(tab), 연산, job control, etc.

## **커널의 자원 관리**

![kernel_resource](https://www.researchgate.net/profile/Ktamil-Selvan-3/publication/265598483/figure/fig1/AS:295796806307841@1447534862985/General-Structure-of-Kernel.png)

- 커널의 가장 큰 목표는 시스템의 물리적 자원(하드웨어)과 추상화 자원을 관리하는 것임.
- 추상화 자원이란?
  - 물리적으로 하나뿐인 자원을 여러 사용자(프로세스)들이 번갈아 사용할 수 있도록 여러개처럼 보이게 하는 것.
  - 커널이 물리적 자원을 관리함에 따라 각 사용자(프로세스)들이 하나의 하드웨어를 독점하는 것처럼 느낄 수 있도록 함.
- 커널을 다음과 같은 작업을 수행함.
  - Task(Process) Management: 물리적 자원인 CPU를 추상적 자원인 Task로 제공
  - Memory Management: 물리적 자원인 메모리를 추상적 자원인 Page 또는 Segment로 제공
  - File System: 물리적 자원인 디스크를 추상적 자원인 File로 제공
  - Network Managment : 물리적 자원인 네트워크 장치를 추상적 자원인 Socket으로 제공
  - Device Driver Management : 각종 외부 장치에 대한 접근
  - Interrupt Handling : 인터럽트 핸들러
  - I/O Communication : 입출력 통신 관리

![linux_kernel_manager](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPhCua%2FbtquOHk32N1%2FmQ1dh768s9r2civkBINAJK%2Fimg.png)

## **커널의 종류**

### **Monolithic Kernel**

- 커널의 기능들(VFS, IPC, File System 등)을 하나의 커널에서 직접 처리하는 방식.
- 각각의 서비스들은 커널 내부의 여러 계층에서 관리되며 사용자가 시스템 자원을 시스템 콜을 통해 사용할 수 있게 해줌.

![linux_kernel](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F212D324158CF7FF60D)

- 모든 기능들이 하나의 커널에서 직접 처리되므로 커널 내부에서 서비스들이 시스템 자원을 공유하므로 효율적이며 속도가 빠른 장점이 있음.
- 반면에 같은 맥락에서 커널의 크기가 크고, 하나의 오류만 발생해도 전 체 시스템에 영향을 줄 수 있으며, 커널의 기능이 늘어날수록 커널의 크기가 커지고 복잡해지는 단점이 있음.
- 초기의 모놀리식 커널은 단일 모듈이었기 때문에 내부 서비스의 추가 및 수정이 어려웠으나, 현재는 모듈화되어 있어서 서비스의 추가 및 수정이 용이함.
- 대표적인 모놀리식 커널을 사용하는 OS에는 Unix, Linux, MS-DOS, 윈도우 9X, Mac OS 8.6 이하 등이 있음.

### **Micro Kernel**

![micro_kernel](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F2134605058CF7FF714)

- 기존의 모놀리식 커널에서 핵심 서비스(Process Management, Memory Management, Network Management)만을 남겨두고 나머지를 제외하여 가볍게 만든 커널.

![micro_kernel_vs_monolithic_kernel](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4Z38l%2Fbtrn15jC4mH%2FRimeFAKVaaQ67HYIjiXxOk%2Fimg.png)

- 기존의 모놀리식 커널의 시스템 서비스들(VFS, IPC, Device Driver 등)을 마이크로 커널에서는 개별적인 서버의 형태로 존재함.
- 마이크로 커널은 핵심 서비스만을 가지고 있고 서버를 추가하는 방식이기에 커널 변경 없이 간단히 기능을 추가 및 수정할 수 있음.
- 또한 프로세스가 각각의 서버 영역에서 수행되기 때문에 하나의 서비스가 다운되어도 다른 서비스에 영향을 끼치지 않음.
- 하지만 커널 내의 서비스들이 각각의 서버 영역에 존재하므로 서비스끼리 리소스 공유를 위해서는 컨텍스트 스위칭이 필수적으로 발생하게 되어 성능 저하가 발생할 수 있음.
- 대표적인 마이크로 커널을 사용하는 OS에는 Mach, 심비안 등이 있음.

### **Hybrid Kernel**

![windows_hybrid_kernel](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F240B9C3E58CF7FF831)

- 모놀리식 커널과 마이크로 커널의 장점을 결합한 커널.
- 기존의 모놀리식 커널과 다를 것이 없다는 의견도 있음.

## **Linux Kernel**

![linux_source_level](https://blog.kakaocdn.net/dn/Er5wO/btquSq2CaXo/bc1MKz7IIIjqU7Sn8GExC0/img.png)

![linux_source](https://user-images.githubusercontent.com/39883609/209545887-d9cc7cb8-c6cc-4961-bbdf-d452698163d4.png)

### **kernel**

![linux_kernel](https://user-images.githubusercontent.com/39883609/209545921-e873a718-6ae5-4984-be33-72f96471c311.png)

- 태스크 관리자가 구현된 디렉터리.
- 태스크의 생성 및 소멸, 프로그램 실행, 스케쥴링, 시그널 처리등의 기능 구현.

### **arch**

![linux_arch](https://user-images.githubusercontent.com/39883609/209546425-a7841633-88a7-4770-8a4c-d0eb062e8db5.png)

- 하드웨어 종속적인 부분들이 구현된 디렉터리.

- `arch/x86`: x86 아키텍처에서 사용되는 하드웨어 종속적인 부분들이 구현된 디렉터리. 여러 종류의 CPU 아키텍ㅊ처를 지원하기 위해 아키텍처별로 디렉터리를 구분함. `x86`, `arm`, `mips`, `arc` 등이 있음.

![linux_arch_x86](https://user-images.githubusercontent.com/39883609/209546583-e35aa88a-586f-4324-9ec2-60f789143969.png)

- `arch/x86/boot`: 시스템 부팅 시 사용하는 부트로더가 구현된 디렉터리.

![linux_arch_x86_boot](https://user-images.githubusercontent.com/39883609/209547018-208ded56-d5fc-45e5-9d85-9666ca0067bb.png)

- `arch/x86/kernel`: 태스크 관리자 중 컨텍스트 스위칭 및 쓰레드 관리 기능이 구현된 디렉터리.

![linux_arch_x86_kernel](https://user-images.githubusercontent.com/39883609/209547125-39726a26-801e-4e5f-834c-9e32e71599cd.png)

- `arch/x86/mm`: 메모리 관리자 중 페이징 및 하드웨어 종속 부분이 구현된 디렉터리.

![linux_arch_x86_mm](https://user-images.githubusercontent.com/39883609/209547165-1a7ac331-347d-459a-a92c-7127faa90541.png)

- `arch/x86/lib`: 커널이 사용하는 라이브러리 함수가 구현된 디렉터리.

![linux_arch_x86_lib](https://user-images.githubusercontent.com/39883609/209547194-9ca8e4b0-ccf3-4a3d-baf1-3b73a7ecb1e8.png)

- `arch/x86/math-emu`: FPU(Floting Point Unit) 관련 기능이 구현된 디렉터리.

![linux_arch_x86_fpu](https://user-images.githubusercontent.com/39883609/209547241-42f84f6d-9d4b-48ca-aa69-e82c454a5b8e.png)

### **fs**

- 다양한 파일시스템과 시스템 호출이 구현된 디렉터리.
- `open()`, `read()`, `write()`와 같은 시스템 콜이 구현되어 있음.

![linux_fs](https://user-images.githubusercontent.com/39883609/209547391-9a5e069d-476f-42f1-b339-ac94b015ad3d.png)

### **mm**

- 메모리 관리자가 구현된 디렉터리.
- 물리적 메모리, 가상 메모리, 동적 메모리 관리 기능이 구현되어 있음.

![linux_mm](https://user-images.githubusercontent.com/39883609/209547655-1faf173a-ae42-4fe3-9070-8225d7816f1e.png)

### **driver**

- 디바이스 드라이버가 구현된 디렉터리.
- 여러가지 디바이스 드라이버가 구현되어 있음.
  - `block device driver`: 파일 시스템이 사용하는 디바이스 드라이버.
  - `char device driver`: 터미널이나 키보드 등의 장치파일을 위한 디바이스 드라이버.
  - `network device driver`: tcp, ip 등 네트워크를 위한 디바이스 드라이버.

![linux_driver](https://user-images.githubusercontent.com/39883609/209547943-f5f6a0a0-91b3-418d-b7a9-1e8cad420244.png)

### **net**

- 통신 프로토콜이 구현된 디렉터리.
- TCP/IP 뿐만 아니라 UNIX domain socket, 802.11, IPX, RPC, 이더넷, 블루투스 등 다양한 통신 프로토콜이 구현되어 있음.
- 사용자 인터페이스를 제공하는 소켓이 해당 디렉터리에 구현되어 있음.

![linux_network](https://user-images.githubusercontent.com/39883609/209548181-32ac6084-4fc5-4f6f-9da5-cbe2b83427d3.png)

### **ipc**

- IPC(Inter Process Communication) 기능이 구현된 디렉터리.
- 파이프는 `fs`에 시그널은 `kernel`에 소켓은 `net`에 구현되어 있음.

![linux_ipc](https://user-images.githubusercontent.com/39883609/209548305-7dde0d7b-5f64-4ec7-978b-06b74acee7c5.png)

### **init**

- 커널 초기화 부분, 커널의 메인 시작 함수가 구현된 디렉터리.
- `start_kernel()` 함수를 통해 커널을 전역적으로 초기화함.

![linux_init](https://user-images.githubusercontent.com/39883609/209548542-5756ba6a-7c12-460d-9b3f-f2716238bb02.png)

![linux_init_start_kernel](https://user-images.githubusercontent.com/39883609/209548512-e91b5eb9-4d23-469f-975f-9cfc3312e2da.png)

### **include**

- 헤더 파일들을 모아 놓은 디렉터리.

![linux_include](https://user-images.githubusercontent.com/39883609/209548613-bcd45c2e-fc41-47eb-8656-7a28cc9b54f3.png)

---

![linux_source](https://user-images.githubusercontent.com/39883609/209545887-d9cc7cb8-c6cc-4961-bbdf-d452698163d4.png)
