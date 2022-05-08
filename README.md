#  Real-time and Safety Auxiliary Runtime for Linux

# 임베디드 시스템(Embedded system) -> 지능형 시스템(Intelligent system)

- 임베디드 시스템이란 미리 정해진 특정 기능을 수행하기 위해 컴퓨터의 하드웨어와 소프트웨어가 조합된 전자 제어 시스템을 통칭함
- 전통적인 임베디드 시스템은 기능 범위가 제한되고 전용 응용 프로그램이 있는 컴퓨터 기반 제품으로 셋톱박스, 산업용 자동화 장비, 의료기기, 스마트카드, 알람시계, 전동칫솔, 전자레인지 등이 이에 해당
- SW 지능화 및 기반 기술의 발전에 따라 고급 운영체제를 실행하고 자발적으로 인터넷에 연결하며 클라우드 기반 응용 프로그램을 실행하고 수집된 데이터를 분석하는 지능형(Intelligent) 시스템이 출현함
- 지능형 시스템은 고도의 지능성, 고성능 및 이종(heterogeneous) 컴퓨터 구조를 포함하는 적응성, 인간-기계 상호 작용과 기계 간 통신을 가능하게 하는 연결성을 갖추고 있으며, 기술적으로는 최소 32비트 사양 이상의 프로세서가 탑재됨 이와 같은 시스템이 늘어나고 있음
- 첨단 운전자 지원시스템(ADAS, Advanced Driver Assistance System) 자동차 인포테인먼트, 3D 프린터 등이 지능형 시스템에 해당함


# 엣지 컴퓨팅이란
 
 다양한 end-to-end system에서 발생하는 데이터를 중앙 집중식 데이터 센터와 같은 클라우드로 보내지 않고 데이터가 발생한 기기 또는 근거리에 있는 서버에서 실시간으로 처리하여 데이터 처리 시간을 큰 폭으로 단축하고 인터넷 대역폭 사용량또한 감소시키는 산업용 데이터 처리 체계이다. 이러한 엣지 컴퓨팅은 원격으로 자주 관리해야하고 보안이 필요하며 사용하는 사람들이 신뢰할 수 있어야 한다 => 이와 같은 것들을 위해 엣지 디바이스에는 다양한 소프트웨어가 탑제 되어야 한다

# workload란 

고객 대면 애플리케이션이나 백엔드 프로세스 같이 비즈니스 가치를 창출하는 리소스 및 코드 모음 또는
주어진 기간에 시스템에 의해 실행되어야 할 작업의 할당량을 의미

# 이와 같은 상황에서 문제 상황...

moden Multicore hw는 전문화/복잡/이기종
소프트웨어 요소가 다양함(FOSS, Real-time, Safety, 3rd party..) 같은 상황에서 엣지 디바이스에서 많은 양의 소프트웨어를 엔지니어링 하는 방법은

소프트웨어에서 활용하려는 동시 하드웨어 가속 기능

이것들을 엣지 컴퓨팅에 적용하기 위해 생각해야할 상황들

# The situation

# 1  Moore’s Law +  Moore’s Gap -> Multicore SoCs
이에 활용되는 software workloads가 다양해지고 있다.

# 2 Software Complexity -> Software Partitioning

하드웨어에 들어야가야하는 소프트웨어의 양이 늘고 있다 partitioning이 필요하다

software architects, we want a strong yet flexible 
architectural foundation that:

• 현재 및 미래의 요구 사항 수용
• 재사용 가능한 구성 요소를 엔지니어링할 수 있습니다.
• 다양한 제약 조건을 수용할 수 있습니다.
• 구성, 구성 및 정책 분리
• 구조화되고 이해하기 쉽습니다.
→ 소프트웨어 복잡성을 관리 가능한 엔터티로 나눕니다.
라이브러리, 커널 모듈, 프로그램, 패키지,
컨테이너, VM 및 여러 런타임.

# 3 Time To Market -> Open Source 4 Hardware enablement -> linux

a) 에지 장치에는 많은 양의 오픈 소스 미들웨어 및 기성품이 있습니다.
Linux에서만 사용할 수 있는 애플리케이션이 점점 늘어나고 있습니다.
b) 에지 장치용 보드 지원 패키지는 점점 더 많은 경우에만 사용할 수 있습니다.
리눅스.
c) Linux에서 코드를 이식하는 것은 점점 더 문제가 되고 있습니다.
→ 따라서 에지 장치에는 Linux 인스턴스가 점점 더 많이 포함될 것입니다.

# 참고
- https://spri.kr/posts/view/22163?code=inderstry_trend
- https://ettrends.etri.re.kr/ettrends/186/0905186020/35-6_78-87.pdf
- http://www.ittoday.info/Articles/Server_Virtualization_Technologies/Server_Virtualization_Technologies.htm#:~:text=Software%20partitioning%20is%20a%20software,instance%20of%20an%20operating%20system.
- https://www.openampproject.org/docs/blogs/HypervisorlessVirtioBlog_Feb2021.pdf
