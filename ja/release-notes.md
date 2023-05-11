## Database > RDS for MariaDB > リリースノート

### May 16, 2023

#### Added Features and Updates

* Made improvements so that the user interface is consistent with NHN Cloud services
* Made modifications so that manual backup is not deleted even when DB instances are deleted
* Added parameter group feature
    * The database settings of DB instance can be freely changed
    * Applicable to multiple instances
    * Changes to settings in an existing DB instance are migrated to a parameter group with the same name as the DB instance
* Added DB security group feature
    * The access control of DB instance can be freely set
    * Applicable to multiple instances
    * Access control rules set on existing DB instances are migrated to the DB security group named as `{DB instance name}__{DB instance ID}` rule
* Provided a screen to view DB instances grouped by replication arrangements
* Displayed candidate master to web console
    * Available to secure storage by deleting the binary log of candidate master
    * Various logs of candidate master can be checked and downloaded
    * Rebuilding candidate master is available when an issue occur
        * The fixed IP does not change because DB instance of the candidate master remain unchanged
        * All data in the database is deleted, and recovered with the data of the master
* Rebuilding read replica is available
    * The fixed IP address does not change because the DB instance of the read replica remain unchanged
    * All data in the database is deleted, and recovered with the data of the master
* Recovery of master with a completed failover
    * High availability recovery of a new master and a master with a completed failover is available
    * Recovery can fail, and an unrecoverable master with a completed failover can be rebuilt
* Rebuilding master with a completed failover
    * The fixed IP does not change because DB instance of the master with a completed failover remain unchanged
    * All data in the database is deleted, and recovered with the data of the master

### 2023. 02. 14.

#### 機能改善

* サーバーダッシュボードのMariaDB指標のうち、ConnectionチャートにMax Connection値を追加で表記するように改善

#### バグ修正

* IAMコンソールでエラーが発生した時に適切なエラーページに移動しない問題を修正
* フェイルオーバーを利用してDBインスタンスタイプの変更またはストレージの拡張を行う場合にPing間隔がデフォルト値に設定される問題を修正

### 2023. 01. 10.

#### 機能改善

* 通知グループ名の重複を許可しないように修正

#### バグ修正

* 監視設定をポップアップウィンドウを閉じずに続けて修正する場合に、設定が正常に適用されない問題を修正
* サーバーダッシュボードページの更新設定が動作しない問題を修正
* DDLクエリ実行によるバックアップ失敗イベントが正常に残らない問題を修正

### 2022. 12. 13.

#### 機能改善

* DML負荷によるバックアップ失敗時、その内容をイベントメッセージに残すように改善

#### バグ修正

* DBスキーマを同期する時、断続的に削除できないスキーマが登録される現象を修正
* 削除したアカウントと同じ名前の別のホストを追加できない問題を修正
* フェイルオーバーした既存マスターを再起動するとユーザーアクセス制御を修正できない問題を修正

### 2022. 11. 15.

#### バグ修正

* `default_authentication_plugin`パラメータを`sha256_password`に設定すると、高可用性構成が解除される問題を修正

### 2022. 10. 11.

#### 機能改善

* インスタンス詳細画面で表示されるドメイン変更ツールチップの文言修正

#### バグ修正

* 廃止予定のイベントコードを削除
* 特定条件で断続的にリードレプリカを削除できない問題を修正

### 2022. 09. 14.

#### バグ修正

* Webブラウザの開発者コンソールにエラーメッセージが残る現象を修正
* **テーブルロック使用しない**状態の単一インスタンスを高可用性インスタンスに変更するとバックアップが失敗する現象を修正

### 2022. 08. 09.

#### 機能追加

* イベントリストをExcelにエクスポートする機能を追加

#### 機能改善

* 高可用性停止したインスタンスもDB Configurationを変更できるように修正
* バックアップ保管周期を最大30日から最大2年に修正
* DDL実行によるバックアップ失敗時、イベントメッセージに原因を残すように改善

#### バグ修正

* 断続的に内部エージェントとの通信問題によりバックアップに失敗する問題を修正

### 2022. 07. 12.

#### 機能追加

* サーバーダッシュボードでチャートをサーバーごとにグループ化して見ることができる機能を追加

#### 機能改善

* **レプリケーション中断**状態のリードレプリカのDB Configuration変更ができるように修正

#### バグ修正

* 断続的に**接続失敗**状態のDBインスタンスの状態が **正常**と表示される問題を修正
* リードレプリカ作成時、バックアップを実行していなくてもイベントログにバックアップ実行が残る問題を修正
* 断続的にボリューム拡張が失敗する問題を修正

### 2022. 06. 14.

#### 機能改善

* 複製ディレイにより再起動が失敗する場合はイベントを残すように改善
* 接続情報ドメインがcloud.toast.comからnhncloudservice.comに変更

#### バグ修正

* validate passwordプラグイン使用時、高可用性構成ができない問題を修正
* 高可用性インスタンスのタイプ変更が失敗したにもかかわらず、candidate masterのタイプが変更後のタイプに見える問題を修正

### 2022. 05. 10.

#### 機能改善

* エラーログの保存位置をデータボリュームに変更
* エラーログが100MBサイズで最大10個まで循環するように変更
* 強制再起動の実行時に、再び使用できるようになるまでWebコンソールを操作できないように修正
* フェイルオーバーの開始時点からWebコンソールで対象インスタンスを操作できないように修正
* processlistで、innodb statusを一緒に見られるようにユーザビリティを改善
* processlistで、数字でページを移動できるように改善
* processlistで、チャートを拡大してその区間だけを確認できるように改善
* processlistで、キーワードで検索できるように改善
* processlistで、照会した内容をCSV形式でダウンロードできるように改善

#### バグ修正

* 読み取りレプリカのバックアップで復元した時、誤った復元時間を選択できていた問題を修正
* Safariでモニタリンググラフが表示されない問題を修正
* マスターインスタンスのパラメータ変更後、読み取りレプリカのパラメータ変更に失敗する問題を修正

### 2022. 04. 12.

#### 機能改善

* 読み取りレプリカまたは一般インスタンスを高可用性インスタンスに変更するとき、使用可能な既存のバックアップがある場合、追加のバックアップなしでレプリケーションを構成するように改善

#### バグ修正

* インスタンス停止とインスタンスボリューム拡張を同時に行う場合、インスタンスボリューム拡張作業が終わらない現象を修正
* データボリュームの残り容量が1%未満の場合、再起動時にエラーが発生する現象を修正
* バックアップが成功してもバックアップ失敗イベントが残ることがある現象を修正

### 2022. 03. 15.

#### 機能追加

* **DB Configuration**に変数使用機能を導入

#### バグ修正

* 特定条件でモニタリングデータが収集されない現象を修正
* フェイルオーバーが発生したインスタンスの自動バックアップが削除されない現象を修正
* 作成に失敗した自動バックアップが期限切れになったときに削除されない現象を修正
* MariaDBに登録されたユーザーが多すぎる場合に、復元に失敗する現象を修正
* access ruleを修正してもバックアップ設定変更イベントが記録される現象を修正

### 2022. 01. 11.

### 機能追加

* MariaDBで実行中のプロセスリストおよびInnoDB状態を確認することができる機能の追加

### 機能改善

* WebコンソールでDBスキーマを作成する時、入力可能な名前の最小文字数をMariaDBと同じに修正

### 2021.12.14

#### 新商品発売

- TOAST Relational Database Service (RDS)は、Relational Databaseをクラウド環境で提供する商品です。
- 複雑な設定をしなくても、Relational Databaseを使うことができます。
- MariaDB 10.3.30バージョンを提供します。
