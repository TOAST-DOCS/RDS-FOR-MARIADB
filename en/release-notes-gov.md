## Database > RDS for MariaDB > Release Notes

### 2024. 05. 16.

#### 기능 추가 및 개선

* Slow Query 분석 기능 추가
  * Slow Query 분석, Process List, InnoDB Status 모니터링 기능을 포함한 분석 탭 제공
  * DB 인스턴스 수정 화면에서 Slow Query 분석 기능을 사용하지 않도록 설정할 수 있는 기능 제공
* 파라미터 그룹 변경 사항 적용 시 실제로 변경되는 파라미터 항목을 확인할 수 있도록 개선
* 고가용성 상태가 비정상인 경우 경고 문구를 노출하고 이벤트를 발생시키도록 개선
* 읽기 복제본 생성 시 스토리지 종류를 선택할 수 있도록 개선
* MariaDB 10.6.16 버전 추가
* API v3.0 추가 및 변경
  * DB 인스턴스 복제하기 API 요청에 `storage.storageType` 필드 추가
  * DB 인스턴스 상세 보기 API 응답에 `notificationGroupIds` 필드 추가
  * API v3.0 호출 시 프로젝트 통합 앱키를 사용할 수 있도록 개선

### March 14, 2024

#### Added Features

* Added the feature to promote candidate masters
* Added the feature to force promote candidate masters
* Added the feature to wait for replication delay on restart with failover
* Added the feature to turn off DB schema & user direct control settings

### February 15, 2024

#### Added Features

* Added DB schema & user-directed control settings
* Improved to better identify connected notification groups
    * Exposed connected notification group information on the DB instance view details screen

### January 11, 2024

#### New Releases

- Relational Database Service (RDS) provides Relational Database in the cloud environment.
- No complicated configuration is required to enable relational database.
