<properties
    pageTitle="DPM を使用した SQL Server ワークロード用 Azure Backup | Microsoft Azure"
    description="Azure Backup サービスを使用した SQL Server データベースのバックアップの概要"
    services="backup"
    documentationCenter=""
    authors="giridharreddy"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/26/2015"
    ms.author="aashishr", "giridham"; "jimpark"/>


# DPM を使用した SQL Server ワークロード用 Azure Backup

この記事では、Azure Backup を使用して SQL Server データベースのバックアップを構成する手順について説明します。

SQL Server データベースを Azure にバックアップするには、Azure アカウントが必要です。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト](https://azure.microsoft.com/pricing/free-trial/)をご覧ください。

SQL Server データベースの Azure へのバックアップと Azure からの回復の管理には、次の 3 つの手順が含まれます。

1. SQL Server データベースを保護するための Azure へのバックアップ ポリシーを作成します。
2. オンデマンドでのバックアップ コピーを Azure に作成します。
3. Azure からデータベースを回復します。

## 開始する前に
開始する前にいることを確認すべて、 [の前提条件](../backup-azure-dpm-introduction/#prerequisites) Microsoft Azure Backup を使用して保護するワークロードが満たされています。 前提条件は、バックアップ コンテナーの作成、コンテナー資格情報のダウンロード、Azure Backup エージェントのインストール、コンテナーへのサーバーの登録などのタスクに関するものです。

## SQL Server データベースを保護するための Azure へのバックアップ ポリシーの作成

1. DPM サーバーでは、新しいを作成して SQL Server データベースの場合は、新しいバックアップ ポリシーを構成 **保護グループ**します。 クリックして、 **保護** ワークスペース。

2. クリックして **新規** 新しい保護グループを作成します。

    ![Create Protection Group](./media/backup-azure-backup-sql/protection-group.png)

3. DPM は作成のガイダンス、スタート画面を表示、 **保護グループ**します。 クリックして **次**します。

4. 選択 **サーバー**します。

    ![Select Protection Group Type - 'Servers'](./media/backup-azure-backup-sql/pg-servers.png)

5. バックアップ対象のデータベースが存在する SQL Server コンピューターを展開します。 DPM に、そのサーバーからバックアップ可能なさまざまなデータ ソースが表示されます。 展開、 **SQL のすべての共有** をバックアップする (この場合選択したデータベース ReportServer$ MSDPM2012 および ReportServer$ MSDPM2012TempDB) を選択します。 クリックして **次**します。

    次のような画面が表示されます。

    ![Select SQL DB](./media/backup-azure-backup-sql/pg-databases.png)

6. 作成する保護グループの名前を指定します。 選択するかどうかを確認します"**オンライン保護**"オプション。

    ![Data Protection Method - short term disk & Online Azure](./media/backup-azure-backup-sql/pg-name.png)

7.  **短期的な目標値の指定** 画面で、バックアップ ポイント ディスクを作成するために必要な入力が含まれます。

    ここで、それが表示 **保有期間の範囲** に設定されている *5 日間*, 、**同期頻度** 1 回に設定されているすべて *15 分* バックアップが実行された頻度であります。 **完全バックアップの表す** に設定されている *午後 8時 00分*します。

    ![Short term goals](./media/backup-azure-backup-sql/pg-shortterm.png)

    >[AZURE.NOTE] 午後 8時 00分 (画面の入力) に従って、バックアップ ポイントは前日の午後 8時 00分のバックアップ ポイントから変更されているデータを転送することによって毎日が作成されます。 このプロセスと呼ばれる **完全バックアップの表す**します。 トランザクション ログは 15 分ごとに同期されますが、午後 9 時 00 分にデータベースを回復する必要がある場合は、最新の高速完全バックアップ ポイント (この例では午後 8 時) からログを再生することでポイントが作成されます。

8. クリックして **次へ]**

    DPM は、使用可能なストレージ領域全体と考えられるディスク領域使用率を表示します。

    ![Disk allocation](./media/backup-azure-backup-sql/pg-storage.png)

    DPM は、初期バックアップ コピーの作成用に、データ ソース (SQL Server データベース) ごとに 1 つのボリュームを作成します。 この方法では、論理ディスク マネージャー (LDM) は、DPM が保護できるデータ ソース (SQL Server データベース) を最大 300 個に制限します。 これを避けるために、DPM は複数のデータ ソースに単一のボリュームを使用する別の方法を実装しています。 使用してこれを有効に、 **DPM 記憶域プール データを併置**します。 この方法を使用すると、DPM は最大 2000 の SQL Database を保護できます。

    DPM を把握バックアップ ボリュームの増加を実稼働データの拡張場合、 **ボリュームを自動的に拡大** が選択されています。 オフにした場合 **ボリュームを自動的に拡大**, 、保護グループでデータ ソースのバックアップを作成するために使用するバックアップ記憶域が制限されます。

9. 管理者は、帯域幅の輻輳を避けるために手動 (オフ ネット) でこの初期バックアップを転送するか、ネットワーク経由で転送するかを選択できます。 最初の転送が行われるときに構成することもできます。 クリックして **次**します。

    ![Initial replication method](./media/backup-azure-backup-sql/pg-manual.png)

    初期バックアップ コピーでは、運用サーバー (SQL Server コンピューター) から DPM サーバーに、データ ソース (SQL Server データベース) 全体を転送する必要があります。 このデータは場合によっては非常に大きくなり、ネットワーク経由で転送すると帯域幅を超える可能性があります。 管理者がこの最初のバックアップを転送するかどうかの選択肢を指定したそのため、 **手動で** を回避または帯域幅が浪費する発生させる **自動的に** ネットワーク経由でします。 また、管理者を選択すると **ネットワーク** 、初期バックアップ コピーを作成するかどうかの選択肢が与えられます **今すぐ** または **後で**, 、指定した時刻。

    初期バックアップが完了した後は、残りのバックアップは初期バックアップのコピーに対する増分バックアップになります。これは一般に非常に小さく、ネットワーク経由で転送できます。

10. 整合性チェックを実行し、をクリックした場合に選択 **次**します。

    ![Consistency check](./media/backup-azure-backup-sql/pg-consistent.png)

    DPM は整合性チェックを実行して、バックアップ ポイントの整合性を確認します。 このチェックでは、運用サーバー (このシナリオでは SQL Server コンピューター) のバックアップ ファイルと DPM にバックアップされたこのファイルのデータのチェックサムを計算します。 競合がある場合は、DPM にバックアップされたファイルが破損していると見なされます。 DPM は、チェックサムの不一致に対応するブロックを送信することにより、バックアップされたデータを修正します。 整合性チェックは負荷の高い処理であるため、管理者はこのチェックをスケジュール設定するか、自動的に実行するかを選択できます。

11. データ ソースのオンライン保護を指定するをクリックして Azure に保護するデータベースを [ **次**します。

    ![Select datasources](./media/backup-azure-backup-sql/pg-sqldatabases.png)

12. 管理者は、組織のポリシーに合わせてバックアップ スケジュールや保有ポリシーを選択できます。

    ![Schedule and Retention](./media/backup-azure-backup-sql/pg-schedule.png)

    この例では、バックアップは 1 日に 1 回、午後 12 時 00 分と午後 8 時 (画面の下部) に作成されます。

    >[AZURE.NOTE] すばやく復旧するために、短期的なディスク上のいくつかの回復ポイントを持つことをお勧めします。 これは、"オペレーショナル リカバリ" と呼ばれます。 Azure は、より高い SLA を提供し、可用性を確保するのに適したオフサイトの場所として機能します。

    **ベスト プラクティス**: Azure のバックアップは DPM を使用してローカル ディスク バックアップの完了後にスケジュールされていることを確認します。 これにより、最新のディスク バックアップを Azure にコピーできるようになります。

13. 保有ポリシーのスケジュールを選択します。 保有ポリシーのしくみの詳細が掲載 [テープ インフラストラクチャ記事を交換できる Azure Backup を使用して](backup-azure-backup-cloud-as-tape.md)します。

    ![保有ポリシー](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    次の点に注意してください。

    - バックアップは 1 日に 1 回、午後 12 時 00 分と午後 8 時 (画面の下部) に作成され、180 日間保有されます。
    - バックアップの土曜日の午後 12時 00分 104 週は保持されます。
    - 午後 12時 00分に最後の土曜日のバックアップ 60 か月間保持されます。
    - バックアップの最後の土曜日の午後 12時 00分に年 3 月 10 年間は保持されます。

14. をクリックして **次** 初期バックアップ コピーを Azure に転送するための適切なオプションを選択します。 選択できる **ネットワーク経由で自動的に** または **オフライン バックアップ**します。

    - **ネットワーク経由で自動的に** から Azure へのバックアップに選択したスケジュールに従ってバックアップ データを転送します。
    - どのように **オフライン バックアップ** に動作が説明されている [Azure Backup でのオフライン バックアップ ワークフロー](backup-azure-backup-import-export.md)します。

    クリックして Azure の初期バックアップ コピーを送信する適切な転送メカニズムを選択 **次**します。

15. ポリシーの詳細を確認すると、 **概要** 画面で、をクリックして、 **グループの作成** ] ボタンをクリックして、ワークフローを完了します。 クリックして、 **閉じる** ボタンをクリックし、監視ワークスペースのジョブの進行状況を監視します。

    ![Creation of Protection Group In-Progress](./media/backup-azure-backup-sql/pg-summary.png)

## SQL Server データベースのオンデマンド バックアップ
前の手順でバックアップ ポリシーを作成しましたが、"回復ポイント" は最初のバックアップ実行時にのみ作成されます。 次の手順は、スケジューラが開始するのを待つ代わりに、回復ポイントの作成を手動でトリガーします。

1. 保護グループの状態が表示されるまで待機 **OK** 回復ポイントを作成する前にデータベースです。

    ![Protection Group Members](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)

2. 右クリックし、データベース **回復ポイントの作成**します。

    ![Create Online Recovery Point](./media/backup-azure-backup-sql/sqlbackup-createrp.png)

3. 選択 **オンライン保護** ドロップダウンをクリックして確定し **[ok]**します。 これにより、Azure での回復ポイントの作成が開始されます。

    ![Create recovery point](./media/backup-azure-backup-sql/sqlbackup-azure.png)

4. ジョブの進行状況を表示することができます、 **監視** ワークスペースが進行中を検索するジョブの 1 つの図のように、次の図です。

    ![Monitoring console](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## Azure からの SQL Server データベースの回復
保護されているエンティティ (SQL Server データベース) を Azure から回復するには、次の手順が必要です。

1. DPM サーバーの管理コンソールを開きます。 移動 **回復** するができる場所のサーバーを表示するワークスペースが DPM でバックアップします。 目的のデータベース (この場合は ReportServer$MSDPM2012) を参照します。 選択、 **からの回復** で終わる時間 **オンライン**します。

    ![Select Recovery point](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)

2. データベース名を右クリックし、クリックして **回復**します。

    ![Recover from Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)

3. DPM に回復ポイントの詳細が表示されます。 クリックして **次**します。 回復の種類を選択して **SQL Server の元のインスタンスに回復する**です。 これは、データベースを上書きします。 クリックして **次**します。

    ![Recover to Original Location](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    この例では、DPM はデータベースを別の SQL Server インスタンスまたはスタンドアロンのネットワーク フォルダーに回復できます。

4.  **指定の回復オプション** ] 画面で、復旧によって使用される帯域幅を調整するネットワークの使用帯域幅の調整のような回復オプションを選択することができます。 クリックして **次**します。

5.  **概要** ] 画面で、これまでに提供されるすべての回復の構成が表示されます。 クリックして **回復**します。

    回復の状態に、データベースが回復されていることが表示されます。 クリックして **閉じる** ウィザードを終了し、[進行状況を表示する、 **監視** ワークスペース。

    ![Initiate recovery process](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    回復が完了すると、復元されたデータベースのコピーはアプリケーション コンシステントになります。

### 次のステップ:

•   [Azure Backup の FAQ](backup-azure-backup-faq.md)


