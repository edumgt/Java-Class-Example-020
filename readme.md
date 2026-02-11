# Java Education 007

이 저장소는 **자바 기반의 항만/선박 데이터 시뮬레이션 학습용 프로젝트**입니다.  
파일 기반 ISAM 인덱싱, 선박 이동 시뮬레이션, MongoDB 위치 업데이트, HWP 문서 생성 예제를 하나의 Maven 프로젝트 안에서 실습할 수 있습니다.

## 기술 스택

- **Language**: Java 17
- **Build Tool**: Maven (`pom.xml`)
- **Packaging**: JAR (Maven Shade Plugin 사용)
- **Logging**: Log4j2 (`log4j-api`, `log4j-core`)
- **Database / Driver**: MongoDB + `mongodb-driver-sync`
- **Document Libraries**:
  - Apache PDFBox
  - hwplib (`kr.dogfoot:hwplib`)
- **Data Modeling Artifacts**: Vuerd JSON (`1.vuerd.json`, `DB/test.vuerd.json`)

## 프로젝트 설명

### 1) ISAM 파일 저장/검색 예제
- `ISAMExample`: 항구 ID + 항구명을 고정 길이 레코드로 `isam_data.dat`에 저장하고, `isam_index.dat`로 오프셋 인덱스를 관리합니다.
- `ISAMExample2`: 위 구조를 확장해 위도/경도까지 포함한 레코드를 저장합니다.

### 2) 선박 이동 시뮬레이션
- `ShipSimulationISAM`: 하버사인(Haversine) 거리 계산을 기반으로 선박 위치를 주기적으로 계산/기록합니다.
- `ShipSimulationUnload`: 운항 중 항구 접근 시 하역 로직(비용 비교, 컨테이너 감소)을 포함한 시뮬레이션 예제입니다.
- 결과 데이터는 `ship_data.dat`, `ship_index.dat` 파일에 기록됩니다.

### 3) MongoDB 선박 위치 업데이트 예제
- `UpdateShipLocation`: MongoDB `testdb.ships` 컬렉션에서 특정 선박 좌표를 주기적으로 읽고 업데이트합니다.
- 로컬 MongoDB(예: `mongodb://localhost:27017`)를 전제로 합니다.

### 4) HWP 문서 생성 예제
- `CreateHwpExample`: hwplib를 사용해 간단한 HWP 파일을 생성하는 샘플입니다.

## 디렉터리 개요

- `src/main/java/com/sample`: ISAM/시뮬레이션/HWP 예제
- `src/main/java/com/ship`: MongoDB 기반 선박 위치 업데이트 예제
- `DB/`: ERD/Vuerd 관련 산출물
- `*.dat`: 샘플 실행 시 생성/사용되는 데이터 및 인덱스 파일

## 실행 전 참고

1. Java 17 및 Maven이 필요합니다.
2. MongoDB 예제를 실행하려면 로컬 MongoDB 인스턴스가 필요합니다.
3. `pom.xml`의 Shade Plugin `mainClass`는 현재 `com.sample.PdfBoxExample`으로 지정되어 있으나, 해당 클래스는 저장소에 보이지 않으므로 패키징 시 실행 진입점 설정을 점검해야 합니다.

## 간단 실행 예시

```bash
# 컴파일
mvn compile

# 특정 예제 실행 (클래스 직접 지정)
mvn exec:java -Dexec.mainClass="com.sample.ISAMExample"
```

MongoDB 빠른 실행 예시(프로젝트 내 메모):

```bash
docker run -d --name my-mongo -p 27017:27017 mongo
```
