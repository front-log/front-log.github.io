---
layout: post
title: "11개의 데이터베이스 드라이버 및 Rust용 ORM(Rust용 ORM)으로 운영 준비 완료"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/10/11-database-drivers-and-orms-for-rust-that-are-ready-for-production.png"
tags: 
---


![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/10/11-database-drivers-and-orms-for-rust-that-are-ready-for-production.png?fit=730%2C487&ssl=1)

이 게시물에서 탐색할 수 있는 시간보다 더 많은 이유로 Rust는 개발자 커뮤니티에서 빠르게 인기를 얻고 있습니다. 이 제품의 장점 중 하나는 파일 기반 스토리지 플랫폼에서 메모리 기반 및 하이브리드 데이터베이스에 이르기까지 모든 주요 데이터베이스와 스토리지 형식을 지원한다는 것입니다.

이 가이드에서는 Rust에 사용할 수 있는 가장 인기 있고 안정적인 데이터베이스 드라이버와 ORM 11개를 비교합니다. 마지막으로, 다음 Rust 프로젝트에 적합한 데이터베이스를 선택할 수 있도록 모든 주요 정보를 편리한 표에 정리합니다.

## 데이터베이스 드라이버

데이터베이스 드라이버는 응용프로그램과 데이터베이스 간의 통신을 수행하는 코드 조각입니다. 응용프로그램과 데이터베이스 간의 데이터 교환을 위한 프로토콜과 형식을 구현합니다.

### 1. '스파이브'

MySQL은 매우 널리 사용되는 데이터베이스이며 SQL 데이터베이스를 사용하는 대부분의 응용 프로그램에서 가장 먼저 선택할 수 있습니다. mysql 상자는 MySQL 프로토콜의 순수한 Rust 구현을 제공한다. 텍스트 기반 프로토콜과 이진 프로토콜을 모두 지원합니다.

➡은 연결 풀뿐만 아니라 문의 캐슁도 지원합니다. TLS는 `native tls` 상자를 통해 지원됩니다. 이 상자에서는 행을 만드는 데 `params`를 사용합니다. 한 가지 단점은 MySQL이 비동기식 지원을 받지 못한다는 것이다.

### 2. ➡_➡c

짐작하셨겠지만 mysql_async는 mysql 드라이버의 비동기 버전입니다. mysql과 비슷한 API를 많이 갖고 있지만 개발자가 관리하는 제품이라 새로운 기능이 없을 수 있다.

mysql_async는 TLS 기반 암호화를 포함하여 MySQL 프로토콜을 완벽하게 지원합니다. `filt_inciptc` 상자를 사용하여 연결 풀을 유지할 수도 있습니다. 이 상자는 순전히 녹으로 씌어 있다.

### 3. tokio_postgres

비동기식 지원: 네, 그렇습니다

Postgres는 매우 인기있는 데이터베이스이다. Rust는 Tokio 비동기 런타임 위에 구축된 Postgres 프로토콜에 대한 비동기 구현이 있지만 다른 런타임에서도 작동할 수 있다.

쿼리의 실행은 완전히 파이프라인화되어 있습니다. 즉, 드라이버가 여러 쿼리를 보낼 수 있습니다. 즉, tokio-postgres는 여러 데이터 응답을 동시에 처리할 수 있습니다. 네이티브 tls를 이용한 TLS도 지원한다. 동일한 연결에서 여러 쿼리를 보낼 수 있으므로 드라이버에 연결 풀 지원이 없습니다.

tokio_postgres는 드라이버의 동기화 버전, OpenSSL 버전, native_tls 버전 등 다양한 구성에 대해 여러 개의 래퍼가 있다.

다음과 같은 다양한 추가 기능이 있는 다른 상자도 있습니다.

- 포스트그레스형
- `포스트그레스-파이어스`
- `포스트그레스(postgres-contract
- 포스트그레스

### 4. 레디스

Redis는 캐시, 작업자 대기열 및 마이크로 서비스를 만드는 데 사용할 수 있는 메모리 내 데이터베이스입니다. Rust는 Redis 데이터베이스를 매우 잘 지원합니다. redis 상자는 하이 레벨 API와 로우 레벨 API를 모두 제공합니다. 모든 쿼리는 파이프라인이므로 여러 쿼리를 동시에 보낼 수 있습니다.

Redis는 Redis 통신 규약을 완전히 구현했다. 펍/서브에 대한 지원은 아직 진행 중입니다. redis는 tokio를 비동기 런타임으로 사용합니다. 이 상자는 `r2d2` 상자를 사용하여 Lua 스크립트, 자동 다시 연결 및 연결 풀을 지원합니다.

### 5. rusqlite

비동기식 지원: 아니요

SQLite는 여러 플랫폼에서 데이터 저장소를 유지 관리하는 데 사용되는 SQL 지원을 포함하는 프로세스 중인 데이터베이스입니다. Android 애플리케이션은 종종 SQLite를 로컬 데이터베이스로 사용합니다.

Rust에는 SQLite C 드라이버 주위에 래퍼가 있습니다. rusqlite 상자는 bindgen을 사용하여 래퍼를 생성하므로 컴파일 시간이 길어질 수 있습니다. rusqlite는 SQLite 래퍼 주변의 안전한 래퍼입니다. 커밋, 인서트 등 후크도 지원한다.

rusqlite에는 비동기 지원이 포함되지 않습니다.

### 6. "lldb"

Google이 만든 LevDB는 키 값 스토어이며 키 값 스토어 전용입니다. SQL 데이터베이스가 아니라 NoSQL에 가깝습니다.

많은 데이터베이스가 LevelDB를 스토리지 엔진으로 사용합니다. Chrome은 IndexDB를 구현하기 위해 LevelDB를 사용한다.

`leveldb` 상자는 LevelDB에 대한 안전한 바인딩을 제공합니다. rusqlite와 마찬가지로 bindgen을 사용하기 때문에 컴파일 시간을 늘릴 수 있다. 또한 rusqlite와 마찬가지로 비동기식 지원은 없습니다.

### 7. mongodb

MongoDB는 NoSQL 데이터베이스이다. Rust는 비동기식 지원을 받는 공식 MongoDB 드라이버를 가지고 있다. 비동기 지원을 위해 tokio 런타임(tokio runtime)을 사용하여 순수 Rust로 작성되었습니다.

mongodb는 다양한 데이터베이스 운영뿐만 아니라 집계를 지원한다. bson 상자를 사용하여 bson 문서를 작성합니다. 이 문서에서는 트랜잭션이 지원되지 않습니다.

### 8. memcache

Memcached는 자유-오픈 소스 고성능 분산 메모리 객체 캐싱 시스템이다. memcache는 순수 Rust로 작성된 Memcached 클라이언트입니다. Memcached의 여러 인스턴스를 지원합니다. 자동 JSON 직렬화 및 압축을 비롯한 일부 기능은 아직 사용할 수 없습니다.

### 9. cdrs

비동기식 지원: 아니요

Cassandra는 확장 가능한 분산 SQL 데이터베이스입니다. crdrs는 카산드라와 실라DB의 데이터베이스 드라이버이다.

이 상자는 카산드라 프로토콜의 완전한 구현을 제공하며 클러스터를 지원합니다. 네이티브 tls를 이용한 TLS도 지원한다.

cdrs-inc는 tokio 비동기 런타임에 기반한 cdrs 상자의 비동기 버전이다.

## ORM

객체 관계 매퍼(ORM)는 Rust 구조로 안정적인 행을 매핑합니다. ORM은 여러 SQL 데이터베이스에 대한 지원을 제공합니다. 많은 데이터베이스 솔루션에는 여러 데이터베이스 드라이버, 연결 풀 지원 및 마이그레이션 툴이 포함되어 있습니다. 다음은 가장 인기 있는 두 가지입니다.

### 1. '스파이브'

비동기식 지원: 아니요

diesel은 Postgres, MySQL, SQLite의 ORM이다. diesel_cli라는 마이그레이션 유틸리티를 제공합니다. `type-safe`는 type-safe로, 마이그레이션 파일을 사용하여 테이블 유형을 생성합니다. 목표는 여러 데이터베이스에서 동일한 동작을 생성하는 것입니다.

에서는 비동기식 지원이 없고 쿼리별 그룹도 제대로 지원되지 않습니다. 안정적 러스트에서는 절차적 매크로에 대한 지원이 없어 오류가 좀 이상하다. 긴 중첩 유형을 포함할 수 있습니다.

데이터베이스의 드라이버를 변경할 수 있습니다. 연결 풀은 `r2d2`를 통해 지원됩니다.

### 2. 진기한

Quaint는 비동기 작업을 지원하는 ORM입니다. 런타임에 쿼리를 만들 수 있는 AST를 제공합니다.

Quaint는 연결 풀뿐만 아니라 MySQL, Postgres, SQLite를 포함한 여러 SQL 데이터베이스를 지원합니다. 데이터베이스 유형 안전 ORM이 아닙니다.

## 올바른 데이터베이스 드라이버를 선택하는 방법

Rust 커뮤니티는 ORM에 독립적인 데이터베이스 마이그레이션을 관리하기 위한 몇 가지 훌륭한 툴을 구축했습니다. 귀사에 가장 적합한 솔루션은 프로젝트의 목표와 요구사항에 따라 달라집니다.

정보에 입각한 선택을 돕기 위해 위에서 설명한 각 데이터베이스 드라이버 및 ORM과 관련된 주요 이점, 장단점 및 기능을 편리한 표에 정리했습니다.