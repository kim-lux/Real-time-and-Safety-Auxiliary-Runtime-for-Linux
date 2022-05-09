#  Real-time and Safety Auxiliary Runtime for Linux

# 1. 임베디드 시스템(Embedded system) -> 지능형 시스템(Intelligent system)


|                |Embedded system                |Intelligent system           |
|----------------|-------------------------------|-----------------------------|
|Definition      |마이크로프로세서에 미리 정해진 특정한 기능을 수행하는 SW를 내장시킨 시스템|고급 운영체제를 실행하고 자발적으로 인터넷에 연결하며 클라우드 기반 응용 프로그램을 실행하고 수집된 데이터를 분석하는 시스템            |
|characteristic  |정해진 특정기능만 수행            |지능성, 적응성, 연결성을 갖추고 기능을 확장시킬 수 있음|
|Example         | 의료기기, 스마트카드, 알람시계, 전동칫솔|ADAS, 3D 프린터 등            |  

## EDGE COMPUTING이란
 
- 다양한 end-to-end system에서 발생하는 데이터를 클라우드로 보내지 않고 데이터가 발생한 기기 또는 근거리에 있는 서버에서 실시간으로 처리하는 것.
- 데이터 처리 시간을 큰 폭으로 단축하고 인터넷 대역폭 사용량또한 감소시키는 데이터 처리 체계.  
- 원격으로 자주 관리해야하고 보안이 필요하며 사용하는 사람들이 신뢰할 수 있어야 함. 
- 이와 같은 것들을 실현시키기 위해 EDGE Device에는 다양한 소프트웨어가 탑제 되어야 햠.

##### Workload란 
- 1. 리소스 및 코드 모음 
- 2. 주어진 기간에 시스템에 의해 실행되어야 할 작업의 할당량

## 2. Current Trend of SW

### 1  Moore’s Law +  Moore’s Gap -> Multicore SoCs

- 트랜지스터의 크기가 점점 줄어 들며 하나의 Multicore Chip에 다양한 Processing core, caches, RAM, Flash, I/O가 들어 갈 수 있게 되었음  
- 이에 따라 지능형 시스템의 소프트웨어 요소(Content)도 같이 다양해지는 상황(FOSS, Real-time, Safety, 3rd party..)에서 많은 양의 소프트웨어를 엔지니어링해야 하므로 활용되는 software workloads가 다양해지고 있다.

### 2 Software Complexity -> Software Partitioning

HW에 들어야가야하는 소프트웨어의 양이 늘고 있기 때문에 유연한 프로그래밍을 위해 partitioning이 필요함.

##### Partition이 필요한 이유
- 여러개의 OS를 하나의 시스템에서 사용하기 위해

### 3 Hardware enablement -> linux

a) EDGE DEVICE에는 많은 양의 오픈 소스가 있는데 Linux에서만 사용할 수 있는 애플리케이션이 점점 늘어나고 있음.
b) EDGE DEVICE BSP(보드 지원 패키지) 또한 Linux에서 사용되는 경우가 늘어나고 있음.
c) Linux에서 다른 OS 코드를 적용시키는 것은 문제가 일어날 확률이 많음.

→ 따라서 EDGE DEVICE에는 Linux를 더욱 많이 사용할 것임.

# 3 Where will the Real-time and Safety workload Run?

- Real-time and Safety Workload는 어디서 실행될 것인가? -- 사진 다이어그램 첨부

## 1. Software-based partitioning  

Software Techniques를 이용하여 WorkLoad를 리눅스에서 바로 실행시킨다.
CPU Reservation을 이용해 User-level-process에서 사용할 수 있다.
Reactivity을 얻을 수 있으나 Process간의 간섭으로 인해 Real-Time을 보장하기 어려움. 

## 2. Whiteboard partitioning

##### Hypervisor란(사진으로 설명)

CPU Upload를 이용해 Core를 오프라인으로 바꾼다.
Offline course에서 사용되는 별도의 hypervisor에서 실행되는 RTOS를 사용
WorkLoad가 충돌하는 경우를 대비하여 Linux와 별도의 Hypervisor를 이용했지만 
Linux가 해당코어의 workload를 방해할 수 있는 가능성이 존재하므로 안전하지 않음.

## 3. Virtualization-based partitioning -> beside linux in a virtual machine

같은 하드웨어에서 hypervisor를 통해 workload를 분할해서 실행함.

## 4. Physical partitioning -> beside Linux on a compute island -> hypervisor-less

workload가 같은 soc내에 First Computer에서는 Linux를 실행하고 Secondary Computer에서 workload를 실행

# 4. How to Share Resources Betweem Runtime?

같은 Soc에서 서로 다른 Runtime을 가진 workload가 실행되고 있을 때

1. printf(), console and debug Access
2. Auxiliary Runtime에서 Linux File Read/Write
3. Linux와 Auxiliary Runtime간의 Intra-SOC messaging

을 위해 Runtime을 연결시켜야함 이때 가장 이상적인 방식이 TCP/IP를 이용하는 것인데
TCP/IP는 WAN protocal로 heavyweight하여 적용시키기 어려움

# 4. OpenAMP(Open Asymmetric Multi-Processing)

Windriver는 OpenAMP proxy 애플리케이션에 resource sharing solution을 구축하는 데 효율적인 software IP components를 가지고 있음

- OpenAMP는 Auxiliary Runtime과 Linux의 효율적인 연결을 위해사용.
- OpenAMP는 Master Processor에서 실행되고 Remote context에서 printf(), scanf(), open(), close(), read() 및 write() 호출을 처리하는 proxy application이 포함되어 있음.
- Core간의 통신을 기반으로 RPMsg라는 메세징 프로토콜을 사용함. (사진 첨부)이때 두가지 하드웨어 구성요소가 RPMsg defines the communication channels and the endpoints which provide logical connections on top of a RPMsg channel

이때 virtio를 활용하는데

##### virtio란
- 이 구조는 일부 장치에 대해서는 매번 Trap해서 emulation하지 말고 hypervisor와 Guest가 바로 통신 할 수 있는 채널을 만들어 불필요한 오버헤드를 줄이자는 것. 이를 위해 Guest에는 특정 장치가 Host와 통신 하기 위한 frontend 드라이버가 있어야 하고 마찬가지로 hypervisor에도 Guest와 특정 장치가 통신 할 수 있는 backend 드라이버가 필요하다.

# 5. Hypervisor-Less-Virtio
를 사용하는 이유

# 6. Conclusion
# 7. 부록 
###### Full virtualization vs paravirtualization

# 참고
- https://spri.kr/posts/view/22163?code=inderstry_trend
- https://ettrends.etri.re.kr/ettrends/186/0905186020/35-6_78-87.pdf
-http://www.ittoday.info/Articles/Server_Virtualization_Technologies/Server_Virtualization_Technologies.htm#:~:text=Software%20partitioning%20is%20a%20software,instance%20of%20an%20operating%20system.
- https://www.openampproject.org/docs/blogs/HypervisorlessVirtioBlog_Feb2021.pdf
- https://developer.ibm.com/articles/l-virtio/
- https://www.adjust.com/glossary/emulated-devices/
- https://selfish-developer.com/entry/virtio
