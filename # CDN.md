## NoSQL
### **```NoSQL 개념:```**
**NoSQL은 관계형 데이터베이스 관리 시스템과는 다른 데이터 모델을 사용하는 데이터베이스 유형을 나타낸다. NoSQL은 대규모의 분산된 데이터를 저장하고 처리하기 위해 설계되었으며, 유연한 스키마, 확장성 및 고성능을 특징으로 한다.**

### NoSQL과 RDBMS의 주요 차이점:

**```데이터 모델:```**

**RDBMS:** 테이블 기반의 구조로, 정의된 스키마 및 관계를 가진다.
**NoSQL:** 유연한 스키마를 가진다. 문서, 키-값 쌍, 그래프, 컬럼 기반 등 다양한 데이터 모델을 사용할 수 있다.

**```확장성:```**

**RDBMS:** 대부분 수직 확장을 통해 성능을 향상시킨다.
**NoSQL:** 수평 확장에 최적화되어 있어, 여러 서버나 노드에 데이터를 분산 저장하는 방식을 취한다.

**```트랜잭션:```**

**RDBMS:** ACID 트랜잭션(원자성, 일관성, 독립성, 지속성)을 지원한다.
**NoSQL:** 대부분의 NoSQL 데이터베이스는 완전한 ACID 트랜잭션을 지원하지 않으나, 일부는 유사한 기능을 제공한다.

**```쿼리 언어:```**

**RDBMS:** SQL을 사용하여 데이터를 쿼리한다.
**NoSQL:** 데이터베이스 유형에 따라 다양한 쿼리 언어 또는 API를 사용한다.

**```정규화와 중복:```**

**RDBMS:** 데이터 중복을 최소화하기 위해 정규화를 진행한다.
**NoSQL:** 유연한 스키마를 통해 데이터 중복이 일부 허용되며, 이는 빠른 읽기 연산을 위한 것이다.

## Partitioning
**큰 데이터 세트를 관리하기 쉬운 부분으로 나누는 과정이다. 주로 대규모 분산 데이터베이스나 스토리지 시스템에서 데이터를 분산시키기 위해 사용된다.**

### Partition Key (파티션 키)

**이는 데이터를 여러 파티션으로 분배하는 데 사용되는 키이다.**
**같은 파티션 키 값을 가진 데이터는 같은 파티션에 저장된다.**

### Sort Key (정렬 키)

**일단 데이터가 특정 파티션에 저장되면, 그 안에서 데이터를 정렬하는 데 사용되는 키이다.**
**이 키를 사용하여 데이터의 특정 순서나 범위를 빠르게 조회할 수 있다.**

### 적절한 예시:

**데이터베이스:** SNS 게시글
**Partition Key:** 사용자 ID
**Sort Key:** 게시글 작성일시
이 경우, 특정 사용자의 게시글을 효과적으로 찾을 수 있으며, 해당 사용자의 게시글 중에서도 작성일시 순으로 빠르게 데이터를 조회할 수 있다.

### 부적절한 예시:

**데이터베이스:** 온라인 상점의 주문 기록
**Partition Key:** 주문 금액
**Sort Key:** 주문 아이템명
여기서 주문 금액을 파티션 키로 사용하면, 비슷한 가격대의 주문들이 한 파티션에 몰릴 수 있어서 일부 파티션에 과도한 부하가 발생할 수 있다. 또한, 주문 아이템명으로 정렬하는 것은 주문 검색이나 분석에 큰 도움이 되지 않을 수 있다.

## GSI
**Amazon DynamoDB에서 제공하는 인덱싱 메커니즘이다. 인덱스를 사용하면 데이터에 빠르게 액세스할 수 있다.**

### GSI:

**키 속성:** 기본 키와 다른 속성을 사용할 수 있다.
**저장량:** GSI는 테이블의 전체 항목과는 독립적으로 저장된다.
**쓰기/읽기 용량:** GSI는 테이블과 별도의 용량 설정이 필요하다.
**생성:** GSI는 테이블을 생성한 후에도 생성할 수 있다.
**범위:** GSI는 전체 테이블에서 데이터를 조회한다.

### LSI:

**키 속성:** LSI는 테이블의 해시 키와 동일한 파티션 키를 사용하면서 다른 범위 키를 사용한다.
**저장량:** LSI는 테이블 항목과 함께 저장된다.
**쓰기/읽기 용량:** LSI는 메인 테이블과 동일한 용량 단위를 공유한다.
**생성:** LSI는 테이블 생성 시에만 생성할 수 있다.
**범위:** LSI는 동일한 파티션 키 값을 갖는 항목들 내에서 데이터를 조회한다.

### 주요 차이점:

**키 속성:** GSI는 전체 새로운 기본 키를 가질 수 있다. 반면, LSI는 동일한 파티션 키를 사용하되 범위 키만 다르다.
**생성 시기:** GSI는 언제든지 생성할 수 있지만, LSI는 테이블 생성 시에만 생성할 수 있다.
**용량 단위:** GSI는 별도의 용량 단위를 가지는 반면, LSI는 메인 테이블과 용량 단위를 공유한다.
**데이터 조회 범위:** GSI는 전체 테이블에서 데이터를 조회하는 반면, LSI는 동일한 파티션 키 값을 갖는 항목들 내에서 조회한다.

## Global Table
**Amazon DynamoDB의 기능으로, 여러 AWS 리전에 걸쳐 데이터를 자동으로 복제하는 확장 가능한 테이블이다. 이를 통해 전 세계 어디에서나 빠르게 데이터에 액세스할 수 있으며, 리전 장애 시에도 지속적인 응용 프로그램 가용성을 유지할 수 있다.**

### Global Table 설정 방법:

**DynamoDB 테이블 생성:** 원하는 AWS 리전에서 DynamoDB 테이블을 생성한다.
**스트림 활성화:** 테이블에 대한 DynamoDB Streams를 활성화한다. 이 스트림은 다른 리전의 테이블과 데이터를 동기화하는 데 사용된다.
**Global Table 생성:** DynamoDB 콘솔에서 "Global Tables" 섹션으로 이동하여, 생성한 테이블을 선택하고 "Add region"을 통해 다른 리전을 추가한다.
**리전 추가:** 필요한 만큼 다른 리전들을 추가하여 데이터를 여러 리전에 복제한다.

### 주의점:

모든 리전에서의 테이블은 동일한 스키마를 가져야 한다.
리전 간의 데이터 복제는 몇 분의 지연 시간을 가질 수 있다.

## TTL
**네트워크 패킷이나 데이터의 수명을 나타내는 개념이다. TTL은 패킷이 네트워크를 통해 전달될 때 목적지까지 도달하기 위해 허용된 최대 라우팅 횟수를 나타낸다.**

각각의 라우터는 패킷을 전달할 때마다 TTL 값을 1씩 감소시킨다. 만약 패킷의 TTL 값이 0이 되면, 해당 패킷은 폐기된다. 이렇게 함으로써 무한 루프나 라우팅 루프를 방지하고 네트워크에서 패킷이 끝없이 순환하는 것을 막을 수 있다.

일반적으로 TTL 값은 초기에 설정되며, 이 값은 시간이나 라우팅 횟수로 계산될 수 있다. 패킷이 목적지까지 전달되는 동안 TTL 값은 감소하며, 이를 통해 패킷의 수명을 관리하고 네트워크의 효율성을 높일 수 있다.