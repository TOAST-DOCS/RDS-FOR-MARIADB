## Database > RDS for MariaDB > 開発者ガイド

## マイグレーション

mysqldumpを利用してNHN Cloud RDSの外部にデータでエクスポートしたり、外部からデータをインポートすることができます。mysqldumpユーティリティはMariaDBをインストールした時、デフォルトで提供されます。

### mysqldumpを利用してエクスポート

* NHN Cloud RDSのインスタンスを準備します。
* エクスポートするデータを保存する外部インスタンス、またはローカルクライアントがインストールされたコンピュータの容量が十分にあるか確認します。
* NHN Cloudの外部にデータをエクスポートする必要がある時、Floating IPを作成してデータをエクスポートするRDSインスタンスに接続します。

下記のmysqldumpコマンドを使用して外部にデータをエクスポートします。

#### ファイルでエクスポートする場合

```
mysqldump -h{rds_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} --single-transaction --routines --events --triggers --databases {database_name1, database_name2, ...} > {local_path_and_file_name}
```

#### NHN Cloud RDS外部のmysql dbにエクスポートする場合

```
mysqldump -h{rds_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} --single-transaction --routines --events --triggers --databases {database_name1, database_name2, ...} | mysql -h{external_db_host} -u{external_db_id} -p{external_db_password} --port={external_db_port}
```

### mysqldumpを利用してインポート

* データをインポートするNHN Cloud RDS外部のデータベースを準備します。
* インポートするNHN Cloud RDSインスタンスの容量が十分にあるか確認します。
* Floating IPを作成してNHN Cloud RDSインスタンスに接続します。
* 下記のmysqldumpコマンドを使用して外部からデータをインポートします。

```
mysqldump -h{external_db_host} -u{external_db_id} -p{external_db_password} --port={external_db_port} --single-transaction --routines --events --triggers --databases {database_name1, database_name2, ...} | mysql -h{rds_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} 
```

### コピーを利用してエクスポート

* コピーを利用してNHN Cloud RDSのデータを外部データベースにエクスポートできます。
* 外部データベースのバージョンはNHN Cloud RDSのバージョンと同じか、それより新しいバージョンでなければいけません。
* データをエクスポートするNHN Cloud RDS MasterまたはRead Only Slaveインスタンスを準備します。
* Floating IPを作成して、データをエクスポートするNHN Cloud RDSインスタンスに接続します。
* 下記のコマンドを使用してNHN Cloud RDSインスタンスからデータをファイルでエクスポートします。

#### Master RDSインスタンスからエクスポートする場合

```
mysqldump -h{rds_master_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} --single-transaction --master-data=2 --routines --events --triggers --databases {database_name1, database_name2, ...} > {local_path_and_file_name}
```

#### Read Only Slave RDSインスタンスからエクスポートする場合

```
mysqldump -h{rds_read_only_slave_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} --single-transaction --dump-slave=2 --routines --events --triggers --databases {database_name1, database_name2, ...} > {local_path_and_file_name}
```

* バックアップされたファイルを開き、コメントに書かれたMASTER_LOG_FILEおよびMASTER_LOG_POSを別途記録します。
* NHN Cloud RDSインスタンスからデータをバックアップする外部ローカルクライアント、またはデータベースがインストールされたコンピュータの容量が十分にあるか確認します。
* 外部データベースのmy.cnf (Winodwsの場合my.ini)ファイルに下記のようなオプションを追加します。
* server-idの場合、NHN Cloud RDSインスタンスのDB Configuration項目のserver-idと異なる値を入力します。

```
...
[mysqld]
...
server-id={server_id}
replicate-ignore-db=rds_maintenance
...
```

* 外部データベースを再起動します。
* バックアップされたファイルを下記のコマンドを使用して外部データベースに入力します。

```
mysql -h{external_db_host} -u{exteranl_db_id} -p{external_db_password} --port={exteranl_db_port} < {local_path_and_file_name}
```

* NHN Cloud RDSインスタンスでコピーに使用するアカウントを作成します。
* 新規コピーを設定する前に、もしかするとあるかもしれない既存コピー情報を初期化するには、下記のクエリーを実行します。この時、RESET SLAVEを実行すると既存コピー情報が初期化されます。

```
STOP SLAVE;

RESET SLAVE;
```

* コピーに使用するアカウント情報と、別途記録しておいたMASTER_LOG_FILEとMASTER_LOG_POSを利用して、外部データベースに下記のようにクエリーを実行します。

```
CHANGE MASTER TO master_host = '{rds_master_instance_floating_ip}', master_user='{user_id_for_replication}', master_password='{password_forreplication_user}', master_port ={rds_master_instance_port}, master_log_file ='{MASTER_LOG_FILE}', master_log_pos = {MASTER_LOG_POS};

START SLAVE;
```

* 外部DBとNHN Cloud RDSインスタンスの原本データが同じ場合、外部DBにSTOP SLAVEコマンドを利用して複製を終了します。

### 複製を利用してインポートする

* 複製を利用して外部DBをNHN Cloud RDSにインポートします。
* NHN Cloud RDSバージョンは外部DBバージョンと同じか、それより新しいバージョンである必要があります。
* データをエクスポートする外部MariaDBインスタンスに接続します。
* 下記のコマンドで外部MariaDBインスタンスからデータをバックアップします。
* 外部MariaDBインスタンス(マスター)からインポートする場合

```
mysqldump -h{master_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} --single-transaction --master-data=2 --routines --events --triggers --databases {database_name1, database_name2, ...} > {local_path_and_file_name}
```

* 外部MariaDBインスタンス(スレーブ)からインポートする場合

```
mysqldump -h{slave_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} --single-transaction --dump-slave=2 --routines --events --triggers --databases {database_name1, database_name2, ...} > {local_path_and_file_name}
```

* バックアップされたファイルを開き、コメントのMASTER_LOG_FILEおよびMASTER_LOG_POSを別途記録します。
* NHN Cloud RDSインスタンスからデータをバックアップするクライアントやコンピュータの容量が十分にあるかを確認します。
* 外部DBのmy.cnf(Winodwsの場合my.ini)ファイルに下記のオプションを追加します。
* server-idの場合、NHN Cloud RDSインスタンスのDB Configuration項目のserver-idと同じ値を入力します。

```
...
[mysqld]
...
server-id={server_id}
replicate-ignore-db=rds_maintenance
...
```

* 外部DBを再起動します。
* 外部ネットワークからインポート(import)すると、時間がかかる場合があるため、
* 内部NHN Cloud Imageを作成してバックアップファイルをコピーした後、NHN Cloudにインポートすることを推奨します。
* バックアップされたファイルを、下記のコマンドでNHN Cloud RDSに入力します。
* 複製構成はDNSをサポートしないため、IPに変換して実行します。

```
mysql -h{rds_master_insance_floating_ip} -u{db_id} -p{db_password} --port={db_port} < {local_path_and_file_name}
```

* 外部MariaDBインスタンスで複製に使用するアカウントを作成します。

```
MariaDB> CREATE USER 'user_id_for_replication'@'{external_db_host}' IDENTIFIED BY '<password_forreplication_user>';
MariaDB> GRANT REPLICATION CLIENT, REPLICATION SLAVE ON *.* TO 'user_id_for_replication'@'{external_db_host}';
```

* 複製に使用するアカウント情報と、別途記録しておいたMASTER_LOG_FILE、MASTER_LOG_POSを利用してNHN Cloud RDSに次のクエリーを実行します。

```
MariaDB> call mysql.tcrds_repl_changemaster ('rds_master_instance_floating_ip',rds_master_instance_port,'user_id_for_replication','password_forreplication_user','MASTER_LOG_FILE',MASTER_LOG_POS );
```

* 複製を開始するには、下記のプロシージャを実行します。

```
MariaDB> call mysql.tcrds_repl_slave_start;
```

* 外部DBとNHN Cloud RDSインスタンスの原本データが同じ場合、下記コマンドを利用して複製を終了します。

```
MariaDB> call mysql.tcrds_repl_init();
```

## オブジェクトストレージを利用したバックアップおよび復元

* RDS for MariaDBのバックアップファイルをオブジェクトストレージへエクスポートしたり、オブジェクトストレージのバックアップファイルを利用してDBインスタンスを復元できます。

> [注意]オブジェクトストレージのバックアップファイルと復元しようとしているMariaDBのバージョンは同じである必要があります。

### オブジェクトストレージにバックアップをエクスポート

* NHN CloudのオブジェクトストレージにRDS for MariaDBのバックアップをエクスポートできます。
* Webコンソールの**Instance**タブでDBインスタンスを選択した後、**追加機能**メニューから**オブジェクトストレージへバックアップをエクスポート**ボタンを押すと、手動バックアップを行います。バックアップされたファイルをすぐユーザーが指定したオブジェクトストレージにアップロードできます。
* また、DBインスタンス詳細画面の**バックアップ& Acess制御**タブで既存バックアップファイルを選択し、**オブジェクトストレージにバックアップをエクスポート**ボタンを押すと、ユーザーが指定したオブジェクトストレージにアップロードできます。
* バックアップファイルは、ユーザーが指定したオブジェクトストレージのコンテナにマルチパートで構成されたオブジェクトにアップロードされます。

### オブジェクトストレージのバックアップファイルを利用して手動で復元

* オブジェクトストレージのバックアップファイルを利用して直接MariaDBを復元できます。
* 復元するMariaDBおよびXtraBackupがインストールされていると仮定します。
* オブジェクトストレージのバックアップを復元したいサーバーにダウンロードします。
* MariaDBサービスを停止します。
* MariaDBデータ保存パスのすべてのファイルを削除します。

```
rm -rf {MariaDBデータ保存パス}/*
```  

* ダウンロードしたバックアップファイルを解凍し、復元します。

```
cat {バックアップファイル保存パス} | xbstream -x -C {MariaDBデータ保存パス}
mariabackup --decompress {MariaDBデータ保存パス}
mariabackup --defaults-file={my.cnfパス} --apply-log {MariaDBデータ保存パス}
```

* 解凍後、不要なファイルを削除します。

```
find {MariaDBデータ保存パス} -name "*.qp" -print0 | xargs -0 rm
```

* MariaDBサービスを開始します。

### オブジェクトストレージのRDS for MariaDBバックアップファイルを利用してDBインスタンス作成

* オブジェクトストレージのRDS for MariaDBバックアップファイルを利用して同じリージョン、他のプロジェクトのRDS for MariaDBに復元できます。
* [オブジェクトストレージにバックアップをエクスポート](./developer-guide/#_5 )を参考にしてバックアップファイルをオブジェクトストレージにエクスポートします。
* 復元するプロジェクトのWebコンソールに接続した後、 Instanceタブでオブジェクトストレージにあるバックアップから復元ボタンをクリックします。
* バックアップファイルが保存されたオブジェクトストレージの情報およびDBインスタンスの情報を入力した後、**作成**ボタンをクリックします。

### オブジェクトストレージの外部MariaDBバックアップファイルを利用してDBインスタンス作成

* 一般MariaDBバックアップファイルを利用してRDS for MariaDBのDBインスタンスに復元できます。

> [注意] innodb_data_file_pathの設定値がibdata1:12M:autoextendではない場合、RDS for MariaDBのDBインスタンスに復元できません。

* MariaDBがインストールされたサーバーで以下のコマンドを利用してバックアップを行います。

```
mariabackup --defaults-file={my.cnfパス} --user {ユーザー} --password '{パスワード}' --socket {MariaDBソケットファイルパス} --compress --compress-threads=1 --stream=xbstream {バックアップファイルが作成されるディレクトリ} 2>>{バックアップログファイルパス} > {バックアップファイルパス}
```

* バックアップログファイルの最後の行に`completed OK!`があるかを確認します。
    * completed OK!がない場合、バックアップが正常に終了していないので、ログファイルにあるエラーメッセージを参考にしてバックアップを再度行います。
* 完了したバックアップファイルをオブジェクトストレージにアップロードします。
    * 一度にアップロードできる最大ファイルサイズは5GBです。
    * バックアップファイルのサイズが5GBより大きい場合、splitなどのユーティリティを利用してバックアップファイルを5GB以下に分割してマルチパートでアップロードする必要があります。
    * 詳細はhttps://docs.nhncloud.com/ja/Storage/Object%20Storage/ja/api-guide/#_43を参照してください。
* 復元するプロジェクトのWebコンソールに接続した後、Instanceタブでオブジェクトストレージにあるバックアップから復元ボタンをクリックします。
* バックアップファイルが保存されたオブジェクトストレージの情報と、DBインスタンスの情報を入力した後、**作成**ボタンをクリックします。

## Procedure

* RDS for MariaDBは、ユーザーの利便性を提供するために、ユーザーアカウントで制限されているいくつかの機能を実行するプロシージャを独自に提供しています。

### tcrds_active_process

* ProcesslistでSleep状態ではなく、ACTIVE状態のクエリを照会します。
* 実行時間が古い順に出力され、クエリー内容(SQL)は100桁まで出力されます。

```
MariaDB> CALL mysql.tcrds_active_process();
```

### tcrds_process_kill

* 特定のプロセスを強制終了します。
* 終了するプロセスIDはinformation_schema.processlistで確認することができ、tcrds_active_processとtcrds_current_lockプロシージャを利用して、プロセスの情報を確認できます。

```
MariaDB> CALL mysql.tcrds_process_kill(processlist_id );
```

### tcrds_current_lock

* 現在ロックを待っているプロセスと、ロックを専有しているプロセス情報を確認します。
* (w)カラム情報がロックを取得するために待機するプロセス情報
* (B)カラム情報がロックを専有しているプロセス情報
* ロックを専有しているプロセスを強制終了するには、(B)PROCESSカラムを確認した後、call tcrds_process_kill(process_id)を実行します。

```
MariaDB> CALL mysql.tcrds_current_lock();
```

### tcrds_repl_changemaster

* 複製を利用して外部MariaDB DBをNHN Cloud RDSにインポートする時に使用します。
* NHN Cloud RDSの複製は、コンソールの **複製作成**で行うことができます。

```
MariaDB> CALL mysql. tcrds_repl_changemaster (master_instance_ip, master_instance_port, user_id_for_replication, password_for_replication_user, MASTER_LOG_FILE, MASTER_LOG_POS);
```

* パラメータ説明
    * master_instance_ip：複製対象(Master)サーバーのIP
    * master_instance_port：複製対象(Master)サーバーのMariaDB Port
    * user_id_for_replication：複製対象(Master)サーバーのMariaDBに接続する複製用のアカウント
    * password_for_replication_user：複製用アカウントのパスワード
    * MASTER_LOG_FILE：複製対象(Master)のbinary logファイル名
    * MASTER_LOG_POS：複製対象(Master)のbinary logポジション

```
ex) call mysql.tcrds_repl_changemaster('10.162.1.1',10000,'db_repl','password','mysql-bin.000001',4);
```

> [注意] 複製用アカウントが複製対象(Master) MariaDBに作成されている必要があります。

### tcrds_repl_init

* MariaDB 複製情報を初期化します。

```
MariaDB> CALL mysql.tcrds_repl_init();
```

### tcrds_repl_slave_stop

* MariaDBの複製を停止します。

```
MariaDB> CALL mysql.tcrds_repl_slave_stop();
```

### tcrds_repl_slave_start

* MariaDBの複製を開始します。

```
MariaDB> CALL mysql.tcrds_repl_slave_start();
```

### tcrds_repl_skip_repl_error

* SQL_SLAVE_SKIP_COUNTER=1を実行します。次のようなDuplicate keyエラー発生した時は、tcrds_repl_skip_repl_errorプロシージャを実行すると複製エラーを解決できます。
* MariaDB error code 1062: 'Duplicate entry ? for key ?'

```
MariaDB> CALL mysql. tcrds_repl_skip_repl_error();
```

