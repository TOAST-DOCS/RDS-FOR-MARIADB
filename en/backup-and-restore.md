## Database > RDS for MariaDB > Backup and Restoration

## Backup

You can prepare in advance to recover the database of DB instance in case of failure. You can perform backups through the web console whenever necessary, and you can configure to perform backups periodically. During backup, storage performance of the DB instance on which the backup is performed can be degraded. To avoid affecting service, it is better to perform back up at a time when the service is under low load. If you do not want the backup to degrade performance, you can use a
high-availability configuration or perform backups from read replica.

> [Note]
> High availability DB instances are backed up on the extra master without compromising the master's storage performance.

The following settings are applied to backup, and also to auto and manual backups.

**Use Table Lock**

* `FLUSH TABLES WITH READ LOCK` ets whether the syntax is enabled or disabled.
* Table lock enables the `FLUSH TABLES WITH READ LOCK` syntax periodically during backups to ensure consistency in backup data. If `FLUSH TABLES WITH READ LOCK` syntax fails to run, the backup will fail.
* You can disable table locking if the DML query load is high during a backup. If you do not use table lock, `FLUSH TABLES WITH READ LOCK` syntax will not run, so a high DML load does not cause the backup to fail. However, backups without table lock may not ensure consistency of backup data, and as a result, some operations, including restore and replication processes, are not supported for backups created without table lock and for DB instances with table locking disabled.

**Query Latency Dash Time (second)**

* When using table lock, set the wait time for `FLUSH TABLES WITH READ LOCK` syntax. `FLUSH TABLES WITH READ LOCK` syntax will wait for the query latency dash time. It can be set from 0 to 21,600 seconds. Longer settings reduce the likelihood of backup failures due to DML query load, but may result in longer overall backup times.

### Manual Backup

If you need to permanently store databases at a certain point in time, you can perform backups manually from the web console. Unlike automatic backups, manual backups are not deleted, unless you explicitly delete the backup, as they are when DB instance is deleted. 웹 콘솔에서 수동 백업을 수행하려면

![db-instance-backup-en](https://static.toastoven.net/prod_rds/24.03.12/mariadb/db-instance-backup-en.png)

❶ 백업하고자 하는 DB 인스턴스를 선택한 후 **백업** 버튼을 클릭하면 팝업 창이 나타납니다.

![db-instance-backup-popup-en](https://static.toastoven.net/prod_rds/24.03.12/mariadb/db-instance-backup-popup-en.png)

❷ 원한다면 백업을 수행할 DB 인스턴스를 변경합니다.
❸ 백업의 이름을 입력합니다. 아래와 같은 제약 사항이 있습니다.

* Backup name has to be unique for each region.
* Backup names are alphabetic, numeric, and - _ between 1 and 100 Only, and the first character has to be an alphabet.

혹은 백업 탭에서

![manual-backup-en](https://static.toastoven.net/prod_rds/24.03.12/mariadb/manual-backup-en.png)

❶ **백업 생성** 버튼을 클릭하면 팝업 창이 나타납니다.

![db-instance-backup-popup-en](https://static.toastoven.net/prod_rds/24.03.12/db-instance-backup-popup-en.png)

❷ 백업을 수행할 DB 인스턴스를 선택합니다.
❸ 백업의 이름을 입력한 후 확인 버튼을 클릭하면 백업 생성 요청을 할 수 있습니다.

### Auto Backup

In addition to manually performing backups, auto backups can occur when needed for restore operations or based on auto backup schedule settings. Auto backups have the same lifecycle as DB instances. When a DB instance is deleted, all archived auto backups are deleted. The following setting items are supported by auto backups.

**Allow Auto Backup**

* If you don't allow auto backups, all auto backups will be blocked from taking place, and some operations, including restore and replication processes, that might otherwise take place on demand, will not be supported. In addition, you won't be able to set the following auto backup-related items

**Auto Backup Retention Period**

* Sets the time period for storing auto backups on storage. It can be kept for up to 730 days, and if the auto backup archive period changes, the expired auto backup files will be deleted immediately.

**Auto Backup Replication Region**

* Set the auto backup file to be replicated to backup storage in another region. Auto Backup replication regions are features for disaster recovery that replicate and manage auto backup files from the original region equally to the destination region. Replication occurs in the background at regular intervals. When you set up an auto backup replication region, you are charged with inter-regional replication traffic, and the destination region is charged additionally for backup storage usage.

**Number of Auto Backup Retries**

* You can set the auto backup to retry if it fails due to DML query load or for other various reasons. You can retry maximum 10 times. Depending on the auto backup run time setting, you might not try again even if there are still more retries.

**Use Auto Backup Schedule**

* When using an auto backup schedule, backups are performed automatically at the auto backup performance time you set.

**Auto Backup Run Time**

* Allows you set the time that the backup automatically takes place. It consists of the backup start time, the backup window, and the backup retry expiration time. You can set the backup run time multiple times so that it does not overlap. Performs backup at any point in the backup window based on the start time of the backup. The backup window is not related to the total running time of the backup. Backup time is proportional to the size of the database and the service load. If the backup fails, retry the backup based on the number of backups retries if it does not exceed the backup retries times.

Auto backup name is given in the format of `{DB instance name} yyyy-MM-dd-HH-mm`.

> [Caution]
> Backups may not be performed in some situations, such as when a previous backup fails to terminate.

### Backup Storage and Pricing

All backup files are uploaded to the internal object storage and stored. For manual backups, they are stored permanently until you delete them separately, and object storage charges are incurred depending on the backup capacity. For automatic backups, it is stored for the set retention period and charges for the full size of the automatic backup file, which exceeds the storage size of the DB instance. If you do not have direct access to the internal object storage where the backup file is
stored, and when you need backup file, you can export the backup file to the object storage in NHN Cloud.

### Export Backup

#### 백업을 수행하면서 파일 내보내기

백업을 수행함과 동시에 백업 파일을 사용자 오브젝트 스토리지로 내보낼 수 있습니다.

![db-instance-list-export-obs-en](https://static.toastoven.net/prod_rds/24.03.12/mariadb/db-instance-list-export-obs-en.png)

![db-instance-list-export-obs-modal-en](https://static.toastoven.net/prod_rds/24.03.12/db-instance-list-export-obs-modal-en.png)

❶ 백업하고자 하는 DB 인스턴스를 선택한 후 드롭다운 메뉴에서 **오브젝트 스토리지로 백업 내보내기** 메뉴를 클릭하면 백업을 내보내기 위한 팝업 창이 나타납니다.
❷ 백업이 저장될 오브젝트 스토리지의 테넌트 ID를 입력합니다. 테넌트 ID는 API 엔드포인트 설정에서 확인할 수 있습니다.
❸ 백업이 저장될 오브젝트 스토리지의 NHN Cloud 계정 혹은 IAM 회원 ID를 입력합니다.
❹ 백업이 저장될 오브젝트 스토리지의 API 비밀번호를 입력합니다.
❺ 백업이 저장될 오브젝트 스토리지의 컨테이너를 입력합니다.
❻ 컨테이너에 저장될 백업의 경로를 입력합니다. 폴더 이름은 최대 255바이트이고, 전체 경로는 최대 1024바이트입니다. 특정 형태(. 또는 ..)는 사용할 수 없으며 특수문자(' " < > ;)와 공백은 입력할 수 없습니다.

#### 백업 파일 내보내기

내부 백업 스토리지에 저장된 백업 파일을 NHN Cloud의 사용자 오브젝트 스토리지로 내보낼 수 있습니다.

![db-instance-detail-backup-export-en](https://static.toastoven.net/prod_rds/24.03.12/mariadb/db-instance-detail-backup-export-en.png)

❶ 백업을 수행한 원본 DB 인스턴스의 상세 탭에서 내보내고자 하는 백업 파일을 선택한 후 **오브젝트 스토리지로 백업 내보내기** 버튼을 클릭하면 백업을 내보내기 위한 팝업 창이 나타납니다.

![backup-export-en](https://static.toastoven.net/prod_rds/24.03.12/mariadb/backup-export-en.png)

❷ 혹은 백업 탭에서 내보내고자 하는 백업 파일을 선택한 후 **오브젝트 스토리지로 백업 내보내기** 버튼을 클릭하면 백업을 내보내기 위한 팝업 창이 나타납니다.

> [Note]
> For manual backups, if the source DB instance that performed the backup was deleted, you cannot export the backup.

## Restoration

Backups allow you to restore data to any point in time. Restoration always creates new DB instance and cannot be restored to the existing DB instance. You can restore only to the same DB engine version as the source DB instance from which you performed the backup. Supports restoring snapshots to the point in time when the backup was created, and restoring point in time to a specific point in time. You can restore it as backup of external MariaDB as well as backup that you created in RDS for
MariaDB.

> [Caution]
> Restoration might fail if the storage size of the DB instance that you want to restore is smaller than the storage size of the source DB instance that you backed up, or if you use a different parameter group than the parameter group of the source DB instance.

### Snapshot Restoration

Restoring a backup to a point in time is called snapshot restoration. Restoration is done with only backup files, you do not need the source DB instance from which you performed the backup.

스냅샷 복원은 백업을 수행한 시점으로 복원합니다. 백업 파일만으로 복원을 진행하여, 백업을 수행한 원본 DB 인스턴스가 필요하지 않습니다. 웹 콘솔에서 스냅샷 복원을 하려면

![db-instance-snapshot-restoration-en](https://static.toastoven.net/prod_rds/24.03.12/mariadb/db-instance-snapshot-restoration-en.png)

❶ DB 인스턴스의 상세 탭에서 복원하려는 백업 파일을 선택한 후 **스냅샷 복원** 버튼을 클릭하면 DB 인스턴스 복원 화면으로 이동합니다.

혹은

![snapshot-restoration-en](https://static.toastoven.net/prod_rds/24.03.12/mariadb/snapshot-restoration-en.png)

❶ 백업 탭에서 복원하려는 백업 파일을 선택한 후 **스냅샷 복원** 버튼을 클릭하면 DB 인스턴스 복원 화면으로 이동합니다.

### Point-in-time Restoration

Restoring to a particular point in time is called point-in-time restoration. You can restore to a specific position in the binary log, as well as to restore to a specific time. Point-in-time restoration requires backup file and binary log from the time you performed the backup to the time you wanted the restore. Binary logs are stored in the storage of the source DB instance where the backup is performed. Shorter binary log retention period allows you to use more storage capacity, but it may be
difficult to restore to the desired point in time. For the cases listed below, you may not be able to restore to the desired point in time because there is no binary log required for point-in-time restoration.

* When you have deleted the binary log of the source DB instance for securing capacity
* When the binary log is automatically deleted by MariaDB based on the Binary log retention period
* When a binary log is deleted due to a failover of a high availability DB instance
* When binary logs are corrupted or deleted for various other reasons

웹 콘솔에서 시점 복원을 하려면

![point-in-time-restoration-list-en](https://static.toastoven.net/prod_rds/24.03.12/mariadb/point-in-time-restoration-list-en.png)

❶ 시점 복원을 하려는 DB 인스턴스를 선택 후 **시점 복원** 버튼을 클릭하면 시점 복원을 할 수 있는 페이지로 이동합니다.

#### Timestamp를 이용한 복원

Timestamp를 사용한 복원 시에는 선택한 시점과 가장 가까운 백업 파일을 기준으로 복원을 진행한 뒤, 원하는 시점까지의 바이너리 로그(binary log)를 적용합니다.

![point-in-time-restoration-01-en](https://static.toastoven.net/prod_rds/24.03.12/point-in-time-restoration-01-en.png)

❶ 복원 방법을 선택합니다.

![point-in-time-restoration-02-en](https://static.toastoven.net/prod_rds/24.03.12/point-in-time-restoration-02-en.png)

❷ 복원 시각을 선택합니다. 가장 최근 시점으로 복원하거나, 원하는 특정 시점을 직접 입력할 수 있습니다.

![point-in-time-restoration-03-en](https://static.toastoven.net/prod_rds/24.03.12/point-in-time-restoration-03-en.png)

❸ **복원될 마지막 쿼리 확인** 버튼을 클릭하면 마지막으로 복원될 쿼리를 확인할 수 있는 팝업 창이 표시됩니다.


#### 바이너리 로그(binary log)를 이용한 복원

바이너리 로그(binary log)를 활용한 복원 과정에서는 선택한 백업 파일로 먼저 복원을 진행한 후, 원하는 위치까지의 바이너리 로그(binary log)를 적용합니다.

![point-in-time-restoration-04-en](https://static.toastoven.net/prod_rds/24.03.12/point-in-time-restoration-04-en.png)

❹ 바이너리 로그(binary log)로 복원을 하기 위해서는 먼저 백업 파일을 선택해야 합니다.
❺ 바이너리 로그(binary log)파일을 선택합니다.
❻ 바이너리 로그(binary log)의 특정 위치를 입력합니다.

### Restoration with External MariaDB Backup

You can use an external MariaDB backup file to create a DB instance. When creating an external MariaDB backup file, refer to [Backup](backup-and-restore/#_1) and use the same version as the Percona XtraBackup used by RDS for MariaDB.

> [Caution]
> If the setting value of innodb\_data\_file\_path is not ibdata1:12M:autoextend, it is unable to restore to DB instance of RDS for MariaDB.

(1) Use the command below to perform a backup on the server where MariaDB is installed.

```
mariabackup --defaults-file={my.cnf 경로} --user {사용자} --password '{비밀번호}' --socket {MariaDB 소켓 파일 경로} --compress --compress-threads=1 --stream=xbstream {백업 파일이 생성될 디렉터리} 2>>{백업 로그 파일 경로} > {백업 파일 경로}
```

(2) Check that `completed OK!` is in the last line of the backup log file. If there is no `completed OK!`, the backup did not end successfully, so refer to the error message in the log file to proceed with the backup again.

(3) Upload the completed backup file to the object storage.

* The maximum file size that can be uploaded at a time is 5GB.
* If the backup file is larger than 5GB, you have to use a utility such as split to cut the backup file to less than 5GB and upload it in multi-part.
* For detailed information, refer to [Multipart Upload](/Storage/Object%20Storage/ko/api-guide/#_44).

(4) After accessing the web console of the project you want to restore, on the DB Instances tab, click the **Restore to Backup in Object Storage** button.

### Restoration by Using RDS for MariaDB Backup

You can use the backup file in RDS for MariaDB to restore the database in MariaDB directly. When restoring a RDS for MariaDB backup file, refer to the [Backup](backup-and-restore/#_1) and use the same version as Percona XtraBackup used by RDS for MariaDB.

(1) Export backup of RDS for MariaDB to object storage with reference to the [Export Backup](backup-and-restore/#_5).

(2) Download the backup of the object storage to the server on which you want to restore it.

(3) Stop the MariaDB service.

(4) Delete all files in the MariaDB data storage path.

``` 
rm -rf {MariaDB data storage path}/* 
```

(5) Unzip and restore the downloaded backup files.

```
cat {백업 파일 저장 경로} | xbstream -x -C {MariaDB 데이터 저장 경로}
mariabackup --decompress {MariaDB 데이터 저장 경로}
mariabackup --defaults-file={my.cnf 경로} --apply-log {MariaDB 데이터 저장 경로}
```

(6) Delete unnecessary files after unzipping files.

``` 
find {MariaDB data storage  path } -name "*.qp" -print0 | xargs -0 rm 
```

(7) Start MariaDB service. 