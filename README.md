# yooosh

## Main Project

<details>
<summary>Live-Casting (라이브캐스팅) </summary>

        "크리에이터가 팬들과 실시간으로 소통하며 자신의 상품을 판매하고, 특별 이벤트를 통해 팬덤을 강화할 수 있는 MSA 기반의 인터랙티브 방송 플랫폼"

        ## 페르소나 (Persona)
        이 프로젝트의 주요 사용자는 다음과 같이 정의할 수 있습니다.

        1. 크리에이터 '코코' (Creator Coco) 👩‍🎨
        목표: 자신의 채널에서 라이브 방송을 통해 팬들과 소통하고 싶어 합니다. 방송 중에 직접 디자인한 굿즈를 판매(라이브 커머스)하거나, 깜짝 이벤트를 열어 팬들에게 특별한 경험을 선물하고 싶어 합니다.

        주요 활동:

        라이브 방송 시작 및 종료

        방송 중 실시간 채팅으로 팬들과 소통

        VOD(다시보기) 영상에 달린 '좋아요' 수 확인

        자신을 '구독'한 팬 목록 확인

        라이브 중 "선착순 100명 한정판 굿즈 판매" 이벤트 시작

        2. 열혈팬 '레니' (Passionate Fan Lenny) 👨‍💻
        목표: 좋아하는 크리에이터 '코코'의 라이브 방송을 놓치지 않고 시청하는 것이 중요합니다. 방송에 적극적으로 참여(채팅, 좋아요)하고, 크리에이터가 진행하는 특별 이벤트에 참여하여 한정판 굿즈를 구매하고 싶어 합니다.

        주요 활동:

        고화질의 라이브 스트리밍 시청

        실시간 채팅 참여 및 '좋아요' 클릭

        크리에이터 채널 '구독'

        선착순 이벤트 시작 시, 빠르게 구매 버튼 클릭

        3. 플랫폼 엔지니어 '제이' (Platform Engineer Jay) 🛠️
        목표: 플랫폼이 안정적으로 운영되도록 시스템을 구축하고 관리합니다. 수많은 마이크로서비스들의 상태를 한눈에 파악하고, 특정 서비스에 장애가 발생해도 전체 시스템에 영향이 가지 않도록 견고하게 설계해야 합니다.

        주요 활동:

        새로운 기능(마이크로서비스)을 개발하고 K8s에 배포

        Prometheus와 Grafana 대시보드로 시스템 전반의 헬스 체크

        ELK 스택으로 모든 서비스의 로그를 중앙에서 검색 및 분석

        Istio를 통해 서비스 간 트래픽 제어 및 장애 전파 방지

        ## 요구사항 (Requirements)
        요청하신 기술 스택이 각 요구사항에 어떻게 사용되는지 함께 명시했습니다.

        기능 요구사항 (Functional Requirements)
        [필수] 동영상 '좋아요' 기능

        사용자는 라이브 방송 또는 VOD 영상에 '좋아요'를 누를 수 있다.

        '좋아요' 수는 실시간으로 집계되어 표시된다.

        사용 기술:

        React: '좋아요' 버튼 UI

        API Gateway: '좋아요' 요청 라우팅

        Spring Boot (Interaction Service): '좋아요' 요청 처리 로직

        Kafka: '좋아요' 클릭 이벤트를 비동기적으로 발행(Publish)

        Redis: 영상별 '좋아요' 카운트를 빠르게 집계 (Caching & Counter)

        NoSQL (e.g., MongoDB): '좋아요' 이벤트 로그 저장

        [필수] 크리에이터 '구독' 기능

        사용자는 특정 크리에이터 채널을 '구독'할 수 있다.

        구독 발생 시 크리에이터에게 알림이 간다.

        사용 기술:

        RDB (e.g., MySQL): 사용자-크리에이터 간의 구독 관계를 트랜잭션으로 안전하게 저장

        Kafka: '구독' 이벤트를 발행하여 알림 서비스 등 다른 서비스에 전파

        Spring Boot (User Service, Notification Service): 구독 로직 및 알림 처리

        [필수] 실시간 채팅 기능

        라이브 방송 중 모든 참여자는 실시간으로 메시지를 주고받을 수 있다.

        사용 기술:

        React: 채팅 UI

        WebSocket & Spring Boot (Chat Service): 클라이언트와 실시간 양방향 통신

        Kafka: 대규모 채팅 메시지를 안정적으로 중계하는 메시지 큐 역할

        NoSQL (e.g., Cassandra): 대용량 채팅 로그를 쓰기 성능 저하 없이 저장

        [필수] 선착순 이벤트 처리 기능

        크리에이터가 라이브 중 "선착순 N명" 이벤트를 시작할 수 있다.

        수만 명의 동시 요청이 발생해도 정확하게 선착순 N명만 처리해야 한다.

        사용 기술:

        Redis: Single-Threaded 특성과 **Atomic-Operation (INCR 명령어)**을 활용하여 동시성 문제를 해결하고 공정한 선착순 처리 구현

        Kafka: 이벤트 참여 요청을 큐에 저장하여 순차적으로 처리하고, 당첨자에게 알림을 보내는 등 후속 처리를 위해 이벤트 발행

        Spring Boot (Event Service): 이벤트 로직 처리

        기타 기능

        라이브 스트리밍 및 VOD 제공

        사용 기술:

        Cloud (Object Storage): 원본 영상 및 VOD 파일 저장

        CDN: 전 세계 사용자에게 지연 없이 영상을 전송

        비기능 요구사항 (Non-Functional Requirements)
        아키텍처: 모든 기능은 독립적으로 개발/배포/확장 가능한 **마이크로서비스 아키텍처(MSA)**로 구성한다. (Spring Boot)

        인프라 및 배포: 모든 서비스는 컨테이너화(Docker)하여 클라우드(AWS, GCP 등) 환경의 쿠버네티스(K8s) 클러스터 위에서 동작한다.

        안정성 및 장애 격리:

        특정 서비스의 장애가 다른 서비스로 전파되는 것을 막는다. (Istio의 Circuit Breaker, Hystrix는 현재 유지보수 모드이므로 Istio나 Resilience4j로 대체 학습 가능)

        모든 외부 요청은 API Gateway를 통해 들어오며, 인증/인가 및 라우팅을 처리한다.

        관측 가능성 (Observability):

        모든 마이크로서비스의 로그는 중앙에서 수집 및 분석/시각화한다. (ELK Stack: Elasticsearch, Logstash, Kibana)

        K8s 클러스터와 각 서비스의 CPU, Memory, Latency 등 시스템 메트릭을 실시간으로 수집하고 모니터링한다. (Prometheus, Grafana)

        서비스 간의 호출 관계와 흐름을 추적한다. (Istio)

        데이터 관리:

        서비스의 특성에 맞게 관계형 DB와 NoSQL DB를 혼용하여 사용한다(Polyglot Persistence). (RDB, NoSQL, 분산 DB)

</details>



## Side Project
1. Sentifl -> 졸업 작품을 다시 리펙토링
2. SSAFYNEWS -> 관통 프로젝트 다시 리펙토링 
<details>
<summary>SSAFYNEWS</summary>


![SSAFYNEWS.png](attachment:24052c16-8484-47db-bf1f-6b1ea23b6339:SSAFYNEWS.png)

# 개요

### 기간

2025.05.22 - 2025.05.27

### 인원

2명

### 사용한 주요 기술

- Vue.js
- Django
- PostgreSQL
- Docker
- Kafka
- Elasticsearch
- Ollama

### Github 저장소

[GitHub - Yoo-SeungHyeon/SSAFY-1st-half-pjt: SSAFY 1학기 최종 관통 프로젝트](https://github.com/Yoo-SeungHyeon/SSAFY-1st-half-pjt)

# 배경

삼성 SW AI 아카데미에서 1학기에 배운 기술들을 사용해보고자 진행한 프로젝트

# 목표

많은 뉴스를 보기 쉽게 가공하여 사용자에 맞는 뉴스를 추천해주는 서비스

# 설계

## SRS (Software Requirements Specification)

[SRS - SSAFYNEWS](https://www.notion.so/SRS-SSAFYNEWS-20a6f535e08a801eae5ac58bb48cee66?pvs=21) 

### 기술 선정 이유

- Kafka : Docker를 활용한 Zookeeper없는 브로커를 경험해보기 위해 사용하였습니다.
- Elasticsearch : 검색에 자동완성, 유사 검색어 기능을 제공하기 위해 도입하였습니다.
- PostgreSQL : pgvector를 통해 유사 기사를 코사인 유사도로 제공하기 위해 도입하였습니다.
- Ollama : Open AI API를 제공 받았으나, 추후 배포와 비용 절감을 위해 Ollama를 사용했습니다.

# 담당 역할 및 기여

### 역할

Full Stack

### 기여

분석을 제외한 데이터 수집, 전처리, WEB, 검색기능 등을 구현하였습니다.

또한 팀장으로서 문서작업 보고서 작성, 발표를 맡았습니다.

### 상세 내용

Vue.js를 컴포넌트 단위로 설계하고, 페이지를 구분한 후, 해당 페이지에 사용할 컴포넌트와 Pinia를 세팅하였습니다. 

그리고 API 연결이 끝난 후, 디자인을 수정할 때 바이브 코딩을 적용하였습니다.

DRF를 통해 RESTful API를 설계하였습니다.

Ollama를 통해 로컬에서 동작하는 챗봇을 만들었습니다.

Docker Compose를 통해 Ollama 까지 Docker로 구성하였습니다.

Elasticsearch로 자동완성과 유사 검색어 기능을 구현하였습니다.

# 구현 과정에 발생한 문제 및 해결

### 발생한 문제 1 (localhost URL로 인한 라우팅 문제)

![image.png](attachment:c208b78b-ee2c-4977-9bcd-701f68473ce1:image.png)

docker compose로 front와 back 컨테이너를 동시에 올렸을 때, browser에서 back 컨테이너를 찾지 못하는 문제가 발생하였습니다. 그 원인은 browser 입장에서 back은 [localhost:8000](http://localhost:8000) 경로에 존재하지만, front 입장에서는 같은 네트워크 이므로 컨테이너 명으로 접근하여야 했습니다. 그래서 back:8000 경로로 접근해야 통신이 가능했습니다.

해당 문제는 네트워크를 host로 바꾸는 방법도 있으나 근본적인 해결책이 되지는 못했습니다.

이를 Nginx를 통해 해결하고자 하였고 그 결과 절대 경로가 아닌 상대 경로로 해결할 수 있음을 알았습니다.

기존의 .env에는 ‘VITE_API_BASE_URL=jhttp://localhost:8000’ 또는 ‘VITE_API_BASE_URL=http://back:8000’ 이렇게 절대 경로로 구성하였으나 이를 ‘VITE_API_BASE_URL=/api’ 이런 방식으로 구성하면 앞에 자동으로 소스 경로를 붙여서 경로 요청을 한다는 사실을 알았습니다. 이를 통해 nginx에서 /api 요청은 back으로 요청이 가도록 라우팅을 처리햇고 그 결과 문제없이 동작한다는 사실을 알았습니다.

![image.png](attachment:88cc4ee4-9c0c-41d2-8b7a-5930967c8bfa:image.png)

![image.png](attachment:26d20e1c-6a72-4bf1-a58c-258d4240dcb4:image.png)

### 발생한 문제 2 (Build 문제)

Front를 배포할 때 최적화와 용량 축소를 위해 Build를 통해 dist 폴더를 배포하는 방식을 채택했습니다. 하지만 해당 방식을 build 과정에 환경 변수가 하드코딩 된다는 사실을 깨달았습니다.

build를 진행하면 dist 폴더에 다음과 같은 파일이 존재합니다.

![image.png](attachment:1ee5c594-434e-44bc-8045-fb4e745d1d0e:image.png)

경로 부분을 자세히 확인하면 기존의 환경변수 처리가 모두 하드코딩 되어있음을 확인할 수 있습니다.

![image.png](attachment:cad9899b-54df-4234-9597-15a48eabaa80:image.png)

이 때문에 container를 만들 때 args 옵션이나 environments 옵션이 적용되지 않았고, 해당 옵션들을 활용하려면 node.js를 통해 배포하는 등 사전 build를 진행하지 않는 방식으로 해야 한다는 사실을 알게 되었습니다.

# 데모 및 시연 영상

### 발표 자료

[SSAFY_1학기_관통프로젝트.pdf](attachment:ecaf58b4-0f56-4940-ae6f-2f93bb444c90:SSAFY_1학기_관통프로젝트.pdf)

### 참고 스크린샷

메인 페이지

![image.png](attachment:34aabd4c-5ad5-4cc6-840e-c5e4e7caa80d:image.png)

카테고리 필터링

![image.png](attachment:bc082b39-a27f-4e5e-8012-1d4cfd6fc77e:image.png)

추천순 정렬

![image.png](attachment:14699883-85f3-4f93-a14c-5c9eda6982be:image.png)

상세 페이지 및 유사 기사 추천

![image.png](attachment:f002cdad-6163-443c-8b93-36be4bd20456:image.png)

상세 페이지 좋아요 및 댓글

![image.png](attachment:74fbfac5-0352-41a5-a1ce-9d5b354245ec:image.png)

로그인 / 회원가입

![image.png](attachment:13b0d7b2-2ab7-4521-8876-680aa502ad4d:image.png)

![image.png](attachment:d514bf51-2698-4301-8645-db86dbe7dc3b:image.png)

대시보드

![image.png](attachment:3612ac9f-5f04-464a-ac58-f423a2af6282:image.png)

![image.png](attachment:6fc15007-306c-4e55-a5f2-e6dcd55c9af7:image.png)

![image.png](attachment:876662ba-ee14-438c-82e9-3b7a46cf86de:image.png)

검색창

![image.png](attachment:4ebb50bc-0c21-46fe-a184-6edd60379817:image.png)

![image.png](attachment:586b16e6-7caf-49f8-b646-63f36e121193:image.png)

![image.png](attachment:a9026935-f5c4-48a7-bb1f-7f3566a7aeb1:image.png)

AI 챗봇 기능

![image.png](attachment:e1304a96-9758-40d2-9f50-b101b0e78f59:image.png)

Swagger API 구조

![image.png](attachment:65a52f66-b741-417a-80b4-0b4b3cbc7658:image.png)

![image.png](attachment:f7b74769-9f93-4da8-aa9d-0fd8f8799454:image.png)

RSS 수집

![image.png](attachment:53a8d654-bdc7-4a10-9c72-4ef7dd247605:image.png)

Consumer 전처리 및 적재

![image.png](attachment:b49e36a4-8567-4260-8623-8872f0de9a16:image.png)

Ollama를 활용한 Local AI

![image.png](attachment:4b5ef65d-b254-4579-908b-3835d50fcede:image.png)

Elasticsearch Indexing

![image.png](attachment:44d05712-d421-4b0c-93eb-288877a442aa:image.png)

### 시연 영상

![시연영상.gif](attachment:f536a494-8b22-4d6b-ad33-861e46669f72:시연영상.gif)

# 회고

### 깨달은 점

### 아쉬운 점 및 개선 방안

1. 한글 검색어 자동완성 기능

한글의 경우 한 글자가 완성되기 전까지는 어떤 글자인지 몰라 자동완성이 제대로 동작하지 않았습니다.

이를 위해 Event 발생마다, 또는 자음 모음 단위로 자동완성 기능을 구현하였으나 제대로 동작하지 않아 아쉬움이 남습니다.

1. 배포 최적화

front는 build하면 하드코딩 되는 문제 때문에 최적화를 진행하지 못했고, back의 경우에도 모든 라이브러리를 전부 docker 이미지에 build하게 되면서 컨테이너의 무게가 심하게 무거워 자원의 낭비가 너무 심했습니다.

![image.png](attachment:89666817-6365-4c4b-b456-5dd0741ddb57:image.png)

이를 copy 최소화, 의존성(requirements.txt) 관리 등으로 해결했으면 좋았을 거 같습니다.

1. build 최적화

원인을 모르겠으나, 최초 빌드시 build 시간이 약 1시간 40분이 소요되었습니다.

![image.png](attachment:d5c1a5a7-a7d9-45c3-ab32-011d8ef63d07:image.png)

직관적으로는 spark와 airflow, hadoop등 무거운 이미지가 많아서 그렇다고 파악하였으나, 해당 문제를 좀 더 고민하여 해결했으면 좋았을 거 같습니다.

</details>

3. 균열 탐지 AI 모듈
4. SSABAB -> 실사용자 프로젝트


## Toy Proejct (Web)
1. Todo
2. Auth
3. Blog
4. Board
5. Shop
6. Video (Redis, CDN)
7. Chat (Web Socket, Redis)


## Toy Project (AI)
1. ML
2. DL