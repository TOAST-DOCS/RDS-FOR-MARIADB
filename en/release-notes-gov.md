## Database > RDS for MariaDB > Release Notes

### 2024. 11. 14.

#### 기능 추가 및 개선

* 스토리지 자동 확장 기능 추가
* 스토리지 크기 확장 시 DB 인스턴스를 재시작하지 않도록 개선
* DB 인스턴스 수정 기능에 포함되어 있던 스토리지 크기 확장 기능을 드롭다운 메뉴로 분리
* 고가용성 일시 중지 상태에서 예비 마스터를 재구축 시 일시 중지 상태가 유지되도록 변경
* 자동 백업 설정 중 백업 재시도 만료 시각 설정 항목을 제거하고 백업 윈도우 시간 범위 내에서 백업을 재시도하도록 개선
* MariaDB 10.11.7, MariaDB 10.11.8 버전 추가

### 2024. 10. 29.

#### 기타

* 한국(평촌) 리전 지원 종료

### 2024. 09. 12.

#### 기능 추가 및 개선

* 증분 백업 기능 추가
* DB 인스턴스 삭제 시 자동 백업 삭제 여부를 선택할 수 있도록 개선

### July 11, 2024

#### Added Features

* Add the procedure that controls foreign_key_checks

#### Bug Fixes

* Fixed an issue where snapshot restoration with a backup of a deleted DB instance was not possible

### June 12, 2024

#### Added Features

* Added the feature to upgrade DB instance OS

### May 16, 2024

#### Added Features

* Added Slow Query analytics
  * Provided the Analytics tab with Slow Query analysis, Process List, and InnoDB Status monitoring features
  * Provided the feature to disable Slow Query Analytics on the Edit DB Instance screen
* Improved to see which parameter items actually change when applying parameter group changes
* Improved to expose warning text and raise an event when high availability status is abnormal
* Improved to select a storage type when creating read replicas
* Added MySQL 8.0.36 version
* Added and modified API v3.0
  * Added the `storage.storageType` field to DB instance replicate API request
  * Added the `notificationGroupIds` field to DB instance detail API response
  * Improved the ability to use project integration appkeys when calling API v3.0

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
