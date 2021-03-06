---
layout: post
title: "비활성화된 SQL Wide-Column 저장소가 없음"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/12/nosql-wide-column-stores.png
tags: undefined
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/nosql-wide-column-stores.png?fit=730%2C487&ssl=1)

많은 사람들은 NoSQL이 고대 기술이라고 믿는다. 그러나 데이터베이스의 세계에서는 NoSQL이 70년대 초반부터 존재해왔음에도 불구하고 아기처럼 여겨지고 있다. 그게 어떻게 가능하죠?

NoSQL은 구글과 아마존이 많은 연구와 자원을 투입했던 2000년대 후반까지는 그다지 인기가 없었습니다. 그 이후, 그것의 인기와 유용성은 기하급수적으로 증가하여 거의 모든 큰 웹사이트와 회사가 어떤 식으로든 NoSQL을 활용할 수 있게 되었다.

또 다른 일반적인 오해는 NoSQL이 의미론적 상대인 SQL보다 더 좋을 수도 더 나쁠 수도 있다는 것이다. 반대로, 이러한 두 가지 데이터베이스 유형은 서로 다른 유형의 데이터에 적합하기 때문에 결코 서로 대체하거나 서로 다른 결과를 나타내지 않을 것이다.

SQL 데이터베이스는 상세하게 설명하지 않아도 미리 정의된 스키마가 있는 반면, NoSQL 데이터베이스는 동적이어서 비정형 데이터에 적합합니다. 필수사항은 아니지만 어떤 SQL 데이터베이스도 스키마를 사용할 수 없습니다.

이러한 점을 염두에 두고, 오늘은 덜 복잡한 NoSQL 데이터베이스 관리 시스템 중 하나를 살펴보겠습니다. 열 제품군이라고도 하는 넓은 열 저장소입니다. 이 NoSQL 모델은 행을 저장하지 않고 열에 저장합니다. 따라서 쿼리에 완벽하고 대규모 데이터 집합에는 최적이 아닙니다.

그런 다음, 다음 설명을 통해 기둥이 넓은 매장을 올바르게 사용하는 데 중요한 역할을 합니다.

- 서로 다른 NoSQL 데이터베이스 관리 시스템
- 기둥이 넓은 매장은 무엇입니까?
- 열 패밀리 데이터베이스 개체
- 칼럼 관계형 모델: 장점과 단점
- OLTP 응용 프로그램의 쿼리
- OLAP 응용 프로그램의 쿼리
- 주요 이점 및 접근 방식 조정 방법

## 서로 다른 NoSQL 데이터베이스 관리 시스템

먼저 4가지 주요 NoSQL 데이터베이스 관리 시스템을 살펴보겠습니다. 이는 칼럼 패밀리가 왜 그렇게 인기가 있는지 더 잘 알 수 있도록 도와줄 것입니다.

#### 1. 키밸류 스토어

가장 간단한 유형은 키 값 저장소입니다. Redis가 하나의 예입니다. 모든 항목에는 속성 이름/키 및 값이 지정됩니다.

#### 2. 문서 데이터베이스

MongoDB와 같은 문서 데이터베이스는 키를 문서라고 하는 복잡한 데이터 스키마와 연결합니다. 중첩된 문서와 키 배열/값 쌍은 각 문서 내부에 포함될 수 있습니다.

#### 3. 그래프 데이터베이스

Neo4j와 같은 그래프 데이터베이스는 소셜 연결과 같은 네트워크 정보를 정렬합니다. 노드(또는 꼭지점, 즉 사물, 장소, 사람, 범주 등)의 컬렉션은 각 반사 데이터(속성)가 서로 다른 노드 간의 관계를 설정하는 레이블(에지)을 부여받는다.

#### 4. '넓은 기둥가게'

넓은 열은 행이 아닌 열 주위에 구조 데이터를 저장합니다. HBase와 Apache Cassandra는 두 가지 예입니다. 일반적으로 열 패밀리가 지원됩니다. 관계형 데이터베이스 테이블과 유사한 방식으로 여러 열이 동시에 사용됩니다.

## 기둥이 넓은 매장은 무엇입니까?

넓은 열 저장소는 일반적인 테이블, 열 및 행을 사용하지만 관계형 데이터베이스(RDB)와 달리 동일한 테이블 내의 행마다 열 형식과 이름이 다를 수 있습니다. 그리고 각 열은 디스크에 별도로 저장됩니다.

열 데이터베이스는 각 열을 별도의 파일에 저장합니다. 한 파일에는 키 열만 저장되고 다른 파일에는 이름, ZIP 등의 이름만 저장됩니다. 행의 각 열은 자동 색인화(각각은 거의 인덱스로 기능함)에 의해 관리되며, 이는 스캔/쿼리된 열 오프셋이 각 파일의 해당 행에 있는 다른 열 오프셋에 해당함을 의미합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/columnar-database-concept-visual.png?resize=730%2C140&ssl=1)

기존 행 지향 스토리지는 단일 행의 여러 열을 쿼리할 때 최상의 성능을 제공합니다. 물론 관계형 데이터베이스는 매우 구체적인 정보를 담고 있는 열을 중심으로 구성되며 각 항목에 대한 특수성을 뒷받침합니다. 예를 들어, 고객 테이블을 살펴보겠습니다. 열 값에는 고객 이름, 주소 및 연락처 정보가 포함됩니다. 모든 고객의 형식은 동일합니다.

Columnar 패밀리가 다릅니다. 자동 수직 분할 기능을 제공합니다. 스토리지는 열 기반이며 덜 제한적인 속성으로 구성됩니다. 또한 RDB 테이블은 행 기반 스토리지로 제한되며 앞으로 나아가기 전에 모든 속성을 설명하는 행으로 구성된 튜플 스토리지를 처리합니다(예: 튜플 1 속성 1, 튜플 1 속성 2, 튜플 2 속성 2 등). 그 반대인 Columnar storage는 우리가 Column family라는 용어를 사용하는 이유이다.

> 참고: 일부 주상절리 시스템은 기본적으로 600만 행의 수평 분할 옵션을 제공합니다. 검색을 실행할 시간이 되면 실제 쿼리 중에 파티션을 분할할 필요가 없습니다. 가장 일반적으로 사용되는 열을 기준으로 기본적으로 수평 파티션을 정렬하도록 시스템을 설정합니다. 이렇게 하면 찾고 있는 값을 포함하는 범위 수가 최소화됩니다.

제공되는 한 가지 유용한 옵션(InfiniDB의 한 가지 예)은 최신 쿼리를 기반으로 수평 파티션을 자동으로 만드는 것입니다. 따라서 더 이상 중요하지 않은 훨씬 오래된 쿼리의 영향을 받지 않습니다.

## 열 패밀리 데이터베이스 개체

패밀리(데이터베이스 개체)에는 관련 정보의 열이 포함됩니다. 개체는 키가 값에 연결된 키-값 쌍으로 구성된 튜플이며 값은 열 집합입니다. 패밀리는 하나의 속성 또는 관련 속성 집합일 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/column-family-db-object-visual.png?resize=730%2C492&ssl=1)

첫 번째 열 모형을 엔티티/속성/값 테이블이라고 할 수 있습니다. 엔티티(열) 내부에는 값/속성 테이블이 있습니다. 고객 데이터의 경우 첫 번째 열 옵션에 대해 다음이 있을 수 있습니다.

RDB와 비교했을 때, 속성/값 테이블은 더 고유한 속성을 입력할 때 빛을 발합니다.

수퍼 열은 동일한 정보를 저장하지만 형식이 다릅니다.

수퍼 열 패밀리와 수퍼 열은 데이터를 더 빨리 얻을 수 있도록 처음 두 모형에 대한 행 ID만 추가합니다. 도면요소만큼 수퍼 열 모형을 사용합니다. 개별 NoSQL 테이블에 저장하거나 수퍼 열 패밀리로 컴파일할 수 있습니다.

### 두 개의 주요 주상절리 계열 유형

#### 1. 칼럼 관계 모델

Columnar 유형 스토리지는 NoSQL의 일부로 간주되더라도 Columnar 관계 모델을 통합할 수 있다.

#### 2. 키밸류 스토어

키-값 스토어 및/또는 빅 테이블.

## 칼럼 관계형 모델: 장점과 단점

### 이점

칼럼 관계 모델은 속성별 방식으로 저장될 때 특성의 압축을 개선할 수 있다. 각 파일의 모든 데이터는 동일한 데이터 파일입니다.

동일한 속성을 공유하는 항목이 수십 개 있다고 가정해 보겠습니다. 이 속성을 통해 모든 튜플을 선택한 다음 ID 범위를 사용하여 추가로 필터링할 수 있습니다(예: ID가 230에서 910인 튜플만 해당). 이러한 압축은 스토리지 요구량을 줄이고 더 쉽게 쿼리할 수 있습니다.

예를 들어, x보다 큰 값을 가진 튜플 컬렉션을 찾고 있다고 가정해 보십시오. 모든 튜플을 검색하여 x보다 큰 값을 가진 튜플을 수집하는 대신, 해당 값을 대상으로 지정하고 적격하지 않은 튜플을 건너뛰면 됩니다. 따라서 더 적은 디스크 블록/바이트가 선택됩니다. 일반적으로 하나의 속성만 쿼리하는 경우 쿼리 속도가 빠릅니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/12/query-advantage-columnar-relational.png?resize=690%2C449&ssl=1)

각 속성은 블록에 별도로 저장되므로 디스크 블록 검색당 검색할 수 있는 튜플 및 특성의 비율이 훨씬 높습니다. 의사 결정 과정이 더 빠릅니다. 주상절리 관계 모델의 또 다른 관련 이점은 더 빠른 결합이다.

또한 데이터베이스에 추가할 새 특성을 가져올 때마다 새 열을 추가하는 것이 훨씬 쉽습니다. 컬럼바 데이터베이스는 거대한 테이블을 재구성할 필요 없이 새 열에 대한 다른 파일을 만들 뿐입니다.

### 단점들

단점으로는 업데이트가 비효율적일 수 있습니다. 예를 들어 여러 속성에 대한 특정 튜플을 업데이트한다고 가정해 보겠습니다. RDB 모델은 이 작업을 더 빨리 수행할 수 있습니다. Columnar family 그룹 특성이 touple 행과 반대로 작동한다는 사실은 RDB가 필요로 하는 것보다 더 많은 블록을 사용하여 여러 속성을 업데이트해야 한다는 것이다.

조인 또는 쿼리로 여러 속성을 터치하면 열 스토리지의 성능이 저하될 수 있습니다(그러나 다른 요인도 마찬가지임). 또한 각 레코드 파일에서 레코드를 삭제해야 하므로 주상절리에서 행을 삭제할 때 속도가 느려집니다.

전반적으로, Columnar 패밀리는 OLAP(Online Analytical Processing)에서는 잘 작동하지만 OLTP(Online Transaction Processing)에서는 잘 작동하지 않는다. 아래에서는 OLTP 대 OLAP 시나리오에 대해 좀 더 자세히 살펴보겠습니다.

## OLTP 응용 프로그램의 쿼리

일반적으로 이 경우 데이터베이스의 매우 작은 부분(예: 하나 또는 몇 개의 계정 튜플)에서 단일 업데이트가 수행됩니다. 그럼에도 불구하고, 그들은 RDB에게 속도 면에서 유리한 여러 가지 속성을 다루어야 할 것이다.

존 스미스는 고객 서비스에 전화를 걸며, 당신은 그의 고객 ID나 전화번호를 통해 그의 정보를 정확히 알아낼 수 있다. 전화 번호가 고유하지 않을 수도 있지만 선택할 계정이 좁혀집니다. 이것은 분석적인 시나리오라기보다는 트랜잭션 시나리오입니다.

그렇다면 OLTP 시스템에 Columnar 데이터베이스가 더 나은가요? 틀렸습니다. 컬럼바 데이터베이스에서 OLTP 유형(단일 행 작업) 트랜잭션을 시도하지 마십시오. 행 지향 시스템을 통해 이 프로세스를 수행하면 테이블의 끝(마지막 페이지)에 새 항목(행)이 추가됩니다.

이와는 대조적으로, 주상절리 시스템은 각각의 파일에 새로운 값을 추가/추가해야 한다. 데이터베이스에 있는 행 수가 많을수록 성능 저하가 더 심각해집니다(이렇게 하지 마십시오. 일괄 삽입은 많은 데이터를 신속하게 삽입할 수 있는 해결 방법입니다).

## OLAP 응용 프로그램의 쿼리

일반적으로 테이블에 있는 모든 계정 값(합계)의 평균과 같은 메타데이터 통찰력을 찾는 쿼리를 수행할 경우, 컬럼나 데이터베이스는 RDB 모델보다 훨씬 빠르게 특정 열에 액세스하고 집계 및 요약을 수행할 수 있습니다.

아마도 당신은 당신의 남성 고객의 평균 연령을 알고 싶을 것이다. 이 경우 일반적으로 순차적 스캔이 발생하며, 이는 성능 저하 요인입니다. 각각 100개의 열이 있는 항목이 1억 행에 있다고 가정해 보겠습니다. 성별에 대한 복합 인덱스를 만들거나 모든 항목을 읽어서 대상 데이터를 필터링해야 합니다(기가바이트 또는 테라바이트 단위).

수 톤의 데이터를 포함하는 수많은 행/열의 튜플을 읽는 대신, 칼럼 시스템을 사용하면 쿼리와 실제로 관련이 있는 두 개 또는 세 개의 열만 스캔하여 조사해야 하는 튜플을 좁힐 수 있습니다.

## 주요 이점 및 접근 방식 조정 방법

Columnar 데이터베이스는 수직 파티셔닝(쿼리의 관련 없는 열을 필터링하여 분석 쿼리에 적합), 수평 파티셔닝(관련되지 않은 익스텐트를 제거하여 효율성 향상), 압축 개선 및 열 자동 인덱싱과 관련하여 향상된 자동화 기능을 제공합니다.

InfiniDB와 유사한 시스템에서는 대부분의 명령에 표준 MySQL 구문을 사용할 수 있습니다. 예를 들어 테이블 만들기, 선택, 삽입 등이 있습니다. 데카르트 제품 부족 및 트리거 지원 등의 몇 가지 예외가 있습니다.

마지막으로 표준 SQL/MySQL에 대한 지식을 프런트 엔드와 통합합니다.