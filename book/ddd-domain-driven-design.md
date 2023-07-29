# DDD (Domain Driven Design)

도메인 주도 설계 핵심

{% embed url="https://www.yes24.com/Product/Goods/48577718" %}

1. Domain
   1. 영역, 집합
   2. 한 비즈니스 내의 유사한 업무의 집합: ex) MPRS: 마케팅, 구매, 연구, 영업
   3. 애플리케이션을 비즈니스 domain 별로 나누어 설계 및 개발할 수 있다
2. DDD (Domain Driven Design)
   1. biz domain 별로 나누어 설계하는 방식
   2. 기존의 어플리케이션 설계가 biz domain에 대한 이해가 부족한 상태에서 설계 및 개발되었다는 반성에서 출발
   3. DDD에서는 기존의 현업에서 IT로의 일방향 소통구조를 탈피하여 현업과 IT의 쌍방향 커뮤니케이션을 중시
   4. 핵심 목표: [Low Coupling, High Cohesion](https://medium.com/clarityhub/low-coupling-high-cohesion-3610e35ac4a6)
   5. 방법:
      1. Strategic Design (개념 설계)
      2. Tactical Design (구체적 설계)
3. Strategic Design
   1. 비즈니스 domain의 context에 맞게 설계하는 방식
   2. context를 event storming으로 공유하고, 비즈니스 목적별로 서비스들을 그룹핑
   3. Bounded Context: 고유한 비즈니스 목적별로 domain의 사용자, 프로세스, 정책 등을 그룹핑한 것. bounded context는 domain안의 서비스를 경계 지은 context의 집합이라고 할 수 있음
   4. Domain Model: 비즈니스 domain의 서비스를 추상화한 설계도. domain을 sub domain으로 분해한것과 bounded context가 domain model.
   5. Micro Service: 1개의 Bounded Context는 최소한 1개 이상의 Micro service로 구성
   6. Context Map: bounded context 간의 관계를 도식화한 다이어그램
   7. Ubiquitous Language
      1. 현업, 개발자, 디자이너 등 참여자들이 동일한 의미로 이해하는 언어
      2. biz domain에 따라 동음이의어가 많기 때문에 정확한 커뮤니케이션을 위해 공통언어를 정의하고 사용
   8. Problem Space: biz domain 분할
      1. Core sub domain: 비즈니스 목적 달성을 위한 핵심 도메인으로 차별화를 위해 가장 많은 투자가 필요
      2. Supporting sub domain: 핵심 도메인을 지원하는 도메인
      3. Generic sub domain: 공통 기능(메일, SSO 등) 도메인으로서 3rd party 제품을 구매하는 것이 효율적
   9. Solution Space: context map 정의
      1. Shared Kernel: 복수의 Bounded Context가 공통으로 사용하는 Bounded Context간의 관계
      2. Upstream-Downstream: Publisher(=Upstream)와 Subscriber(=Downstream) 관계
      3. Open Host Service: 여러 종류의 Downstream Bounded Context를 고려하여 설계되는 Upstream Bounded Context
      4. Anti Corruption Layer: 다른 Bounded Context에서 받는 데이터를 본인에 맞게 구조, Type, 통신프로토콜 등을 변환해 주는 모듈계층을 가지는 Bounded Context. 보통 Downstream Bounded Context가 ACL을 가짐.
   10. Event Storming: biz domain 내에서 일어나는 것들을 찾아 bounded context를 식별하는 방법론
       1. Domain Event 정의: 비즈니스 도메인내에 발생하는 모든 이벤트를 과거형으로 기술
       2. Tell the story: 도출된 이벤트로 도메인의 업무 흐름을 이해하고 토론하여 보완
       3. 프로세스로 그룹핑: 이벤트들을 프로세스로 그룹핑
       4. Command 정의: 각 Domain Event를 발생시키는 명령을 현재형으로 정의하며 명령형(ex: 제품목록을 검색)으로 기술
       5. Trigger 정의: Command를 일으키는 Actor와 Event를 일으키는 External System와 Policy/Rule을 정의
       6. Aggregate 정의: Command 수행을 위해 CRUD해야 하는 데이터 객체 정의
          1. Aggregate 규칙: RPO
             1. Root only-Aggregate Root만 참조: Aggregate내부의 entity나 VO를 접근할 때 직접하지 말고 Aggregate root를 통해서 해라
             2. Primary key-참조는 Primary key 사용: 다른 Aggregate를 참조할때 객체자체를 레퍼런스하지 말고 객체의 primary key로 참조해라. Order는 Consumer 객체 자체를 참조하지 않고, consumerId값으로 Consumer aggregate를 참조함.
             3. One to One -한 개의 트랜잭션은 한 개의 Aggregate만 Writing
       7. Bounded Context 정의
       8. Context Map 작성
4. Tactical Design
   1. 개발을 위한 구체적인 설계도
   2. Model Driven Design: Strategic Design에서 설계한 각 Sub Domain별 Domain Model(Context Map)을 중심으로 설계
   3. [Layered Architecture](https://martinfowler.com/bliki/PresentationDomainDataLayering.html): 목적별 계층으로 나누어 설계
   4. Entity & Value Object: 식별성과 가변성으로 구별
   5. Aggregate & Factory
   6. Repository

Ref.

[https://happycloud-lee.tistory.com/94](https://happycloud-lee.tistory.com/94)
