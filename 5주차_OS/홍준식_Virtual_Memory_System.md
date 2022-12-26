### 주 기억장치
현재 CPU가 처리하고 있는 내용을 저장
### 보조 기억장치
영구적인 데이터 저장소로 주 기억장치보다 느리지만 가격이 쌈

실행중인 모든 어플리케이션을 주 기억 장치에 올려야한다면 주 기억장치보다 큰 어플리케이션을 실행 시킬 수 없다.  
이것의 처리를 위하여 용량이 큰 프로그램을 여러 부분으로 나누어, 실행이 필요할 때마다 주기억 장치로 올려서 사용하는 오버레이 기법을 사용하였다.  
하지만 프로그래머의 능력에 의존한 오버레이로는 근본적인 메모리 부족을 해결하지 못하였다.  

# 가상 메모리란?
가상 메모리는 보조 기억장치의 공간을 주 기억장치의 공간처럼 사용할 수 있도록 해주는 기술이다.
한 프로세스가 실행될 때에 해당 프로세스 전체가 메모리에 올라가지 않도록 하는 기술로 실행에 필요한 부분만 메모리에 적재한다.  
- 물리적인 메모리 공간의 제약 사라짐  
- 동시에 더 많은 프로그램 실행 가능
- Swapping의 횟수가 줄어들어 IO  시간이 줄어듬

# MMU (Memory Management Unit)
![MMU](https://blog.kakaocdn.net/dn/bNVeBe/btqDahRmhMA/MJN2upo2fjvxHp7N6cc7fK/img.png)  
가상 주소는 프로그램이 구동될 때에 CPU에 의하여 생성되는 주소이다. (디버깅 시 보는 주소 공간)  
프로그램의 측면에서 모든 주소 공간이 구축된다.  

MMU는 가상 주소를 물리적 주소로 변환하고 메모리를 보호하는 기능을 한다.  
CPU가 메모리에 접근하기 이전에, MMU를 통하여 메모리 주소를 번역한다.(주소 바인딩)   

# Paging  vs Segmentation
![External Fragmentation](https://d3e8mc9t3dqxs7.cloudfront.net/wp-content/uploads/sites/11/2020/05/Fragmentation2.png)
### Internal Fragmentation (내부 단편화)  
잔여 메모리 공간에 비하여 작은 크기의 프로세스가 들어오는 문제  
### External Fragmentation (외부 단편화)  
잔여 메모리 공간이 연속적이지 않아 신규 프로세스를 할당할 수 있는 메모리 공간은 있지만 연속적인 공간이 없어 할당하지 못하는 문제  

![Paging](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter9/9_05_PageTable.jpg)
### 페이징
외부 단편화 문제를 해결  
프로세스를 물리적으로 일정한 크기로 나누어 메모리에 할당  
가상 메모리를 고정된 크기의 페이지로 나누어 관리한다.  
물리적 메모리 또한 페이지의 크기와 동일하게 나누고 이것을 프레임이라고 한다.  
물리적 공간은 연속적이지 않을 수 있고 페이지 테이블을 통하여 논리적 주소를 물리적으로 변환하여 사용한다.  
### 세그멘테이션
내부 단편화 문제를 해결  
프로세스를 논리적으로 나누어 메모리에 배치  

요즘에는 주로 페이징을 사용  

# Demand Paging
메모리에 데이터가 필요한 경우에 적제한다.  
메모리에 필요한 데이터 이미 있다면 가져오고, 없다면 page fault를 발생시켜 데이터를 읽어들인다.  
즉, 처음부터 필요한 데이터를 모두 읽어들이지 않는다.   
1. 만약 CPU가 메인 메모리에 없는 페이지를 읽어들이려고 하면 page fault가 발생한다.
2. OS는 필요한 페이지를 메모리로 가져오기 위하여 Interrupt를 발생시켜 프로세스를 blocking 상태로 둔다.
3. OS는 필요한 페이지를 물리적 메모리 공간에서 찾는다.
4. 논리적 메모리 공간에서 물리적 공간으로 페이지를 가져온다.
5. 페이지 테이블을 업데이트한다.
6. 시그널을 발생시키고 CPU는 다시 process를 ready 상태로 둔다.

# Page Replacement Algorithm
## FIFO
먼저 들어온 것을 먼저 내보낸다.  
![FIFO](https://media.geeksforgeeks.org/wp-content/uploads/20190412160604/fifo2.png)  
### Optimal Page Replacement
가장 오랫동안 사용되지 않을 것을 교체한다.  
완벽하지만 실제 구현 가능하지는 않다.   
![Optimal Page Replacement](https://media.geeksforgeeks.org/wp-content/uploads/20190412160500/optimal.png)  
### Least Recently Used
가장 최근에 사용되지 않은 것을 교체한다.  
![LRU](https://media.geeksforgeeks.org/wp-content/uploads/20190412161533/optimal2.png)  
