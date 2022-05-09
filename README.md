#  Real-time and Safety Auxiliary Runtime for Linux

#1 임베디드 시스템(Embedded system) -> 지능형 시스템(Intelligent system)


|                |Embedded system                |Intelligent system           |
|----------------|-------------------------------|-----------------------------|
|Definition      |임베디드 시스템은 각종 전자 제품이나 정보 기기 등에 설치되어 있는 마이크로프로세서에 미리 정해진 특정한 기능을 수행하는 SW를 내장시킨 시스템            |고급 운영체제를 실행하고 자발적으로 인터넷에 연결하며 클라우드 기반 응용 프로그램을 실행하고 수집된 데이터를 분석하는 시스템            |
|characteristic  |정해진 특정기능만 수행            |지능성, 적응성, 연결성을 갖추고            |
|Example         | 의료기기, 스마트카드, 알람시계, 전동칫솔|ADAS, 3D 프린터 등            |  

## 엣지 컴퓨팅이란
 
 다양한 end-to-end system에서 발생하는 데이터를 중앙 집중식 데이터 센터와 같은 클라우드로 보내지 않고 데이터가 발생한 기기 또는 근거리에 있는 서버에서 실시간으로 처리하여 데이터 처리 시간을 큰 폭으로 단축하고 인터넷 대역폭 사용량또한 감소시키는 산업용 데이터 처리 체계이다. 이러한 엣지 컴퓨팅은 원격으로 자주 관리해야하고 보안이 필요하며 사용하는 사람들이 신뢰할 수 있어야 한다 => 이와 같은 것들을 위해 엣지 디바이스에는 다양한 소프트웨어가 탑제 되어야 한다

## workload란 

고객 대면 애플리케이션이나 백엔드 프로세스 같이 비즈니스 가치를 창출하는 리소스 및 코드 모음 또는
주어진 기간에 시스템에 의해 실행되어야 할 작업의 할당량을 의미

## 이와 같은 상황에서 문제 상황...

moden Multicore hw는 전문화/복잡/이기종
소프트웨어 요소가 다양함(FOSS, Real-time, Safety, 3rd party..) 같은 상황에서 엣지 디바이스에서 많은 양의 소프트웨어를 엔지니어링 하는 방법은

소프트웨어에서 활용하려는 동시 하드웨어 가속 기능

이것들을 엣지 컴퓨팅에 적용하기 위해 생각해야할 상황들

## The situation

### 1  Moore’s Law +  Moore’s Gap -> Multicore SoCs
이에 활용되는 software workloads가 다양해지고 있다.

### 2 Software Complexity -> Software Partitioning

하드웨어에 들어야가야하는 소프트웨어의 양이 늘고 있다 유연하고 구조적 기반을 위해 partitioning이 필요하다

• 현재 및 미래의 요구 사항 수용
• 재사용 가능한 구성 요소를 엔지니어링할 수 있습니다.
• 다양한 제약 조건을 수용할 수 있습니다.
• 구성, 구성 및 정책 분리
• 구조화되고 이해하기 쉽습니다.
→ 소프트웨어 복잡성을 관리 가능한 엔터티로 나눕니다.
라이브러리, 커널 모듈, 프로그램, 패키지,
컨테이너, VM 및 여러 런타임.

### 3 Time To Market -> Open Source 4 Hardware enablement -> linux

a) 에지 장치에는 많은 양의 오픈 소스 미들웨어 및 기성품이 있습니다.
Linux에서만 사용할 수 있는 애플리케이션이 점점 늘어나고 있습니다.
b) 에지 장치용 보드 지원 패키지는 점점 더 많은 경우에만 사용할 수 있습니다.
리눅스.
c) Linux에서 코드를 이식하는 것은 점점 더 문제가 되고 있습니다.


→ 따라서 에지 장치에는 Linux 인스턴스가 점점 더 많이 포함될 것입니다.
- 만약 그렇다면 에지 디바이스에서 rt/s wl은 어디서 동작 될 것인가

# 2 Where will the Real-time and Safety workload Run?

- Hypervisor란

## 1. Software-based partitioning  

Software Techniques를 이용하여 WorkLoad를 리눅스에서 바로 실행시킨다.
CPU Reservation을 이용해 User-level-process에서 사용할 수 있다.
Reactivity을 얻을 수 있으나 Process간의 간섭으로 인해 Real-Time을 보장하기 어려움. 

## 2. Whiteboard partitioning

CPU Upload를 이용해 Core를 오프라인으로 바꾼다.
Offline course에서 사용되는 별도의 hypervisor에서 실행되는 RTOS를 사용
WorkLoad가 충돌하는 경우를 대비하여 Linux와 별도의 Hypervisor를 이용했지만 
Linux가 해당코어의 workload를 방해할 수 있는 가능성이 존재하므로 안전하지 않음.

## 3. Virtualization-based partitioning -> beside linux in a virtual machine

같은 하드웨어에서 hypervisor를 통해 workload를 분할해서 실행함.

## 4. Physical partitioning -> beside Linux on a compute island -> hypervisor-less

workload가 같은 soc내에 First Computer에서는 Linux를 실행하고
Secondary Computer에서 workload를 실행


# 3. How to Share Resources Betweem Runtime?

Same Soc에서 서로 다른 Runtime을 가진 workload가 실행되고 있을 때

1. printf(), console and debug Access
2. Auxiliary Runtime에서 Linux File Read/Write
3. Linux와 Auxiliary Runtime간의 Intra-SOC messaging

을 위해 Runtime을 통합시켜야함 이때 가장 이상적인 방식이 TCP/IP를 이용하는 것인데
TCP/IP는 WAN protocal로 매우 무거워서 적용시키기 어려움

# 4. OpenAMP

TCP/IP 동작을 빠르게해 Auxiliary Runtime과 Linux의 효율적인 통합을 위해사용

Core간의 통신을 기반으로 RPMsg라는 메세징 프로토콜을 사용함. 이때 두가지 하드웨어 구성요소가 


이때 virtio를 활용하는데
- virtio란
hypervisor의 추상화 계층

1.2.3.4.5.

# 5. Hypervisor-Less-Virtio
를 사용하는 이유

# 6. Conclusion

# 참고
- https://spri.kr/posts/view/22163?code=inderstry_trend
- https://ettrends.etri.re.kr/ettrends/186/0905186020/35-6_78-87.pdf
- http://www.ittoday.info/Articles/Server_Virtualization_Technologies/Server_Virtualization_Technologies.htm#:~:text=Software%20partitioning%20is%20a%20software,instance%20of%20an%20operating%20system.
- https://www.openampproject.org/docs/blogs/HypervisorlessVirtioBlog_Feb2021.pdf
