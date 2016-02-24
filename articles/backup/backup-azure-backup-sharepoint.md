<properties
    pageTitle="Azure への SharePoint ファームの DPM 保護 | Microsoft Azure"
    description="この記事では、Azure への SharePoint ファームの DPM 保護の概要について説明します"
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
    ms.date="12/10/2015"
    ms.author="giridham;jimpark"/>


# Azure への SharePoint ファームのバックアップ
System Center Data Protection Manager (DPM) を使用して SharePoint ファームを Azure にバックアップする方法は、他のデータ ソースのバックアップとよく似ています。 週、月、毎年のバックアップ ポイントでき、異なるさまざまなバックアップ ポイントの保有ポリシー オプション、azure は、毎日作成するバックアップのスケジュールの柔軟性を提供します。 DPM では、目標復旧時間 (RTO) 短縮のためにはローカル ディスク コピーを保存でき、コスト効率に優れた長期保有のためには Azure に保存できます。

## SharePoint のサポートされるバージョンと関連する保護シナリオ
DPM 用 Azure Backup は、次のシナリオをサポートします。

| ワークロード | バージョン | SharePoint のデプロイ | DPM のデプロイの種類 | DPM - System Center 2012 R2 | 保護と回復 |
| -------- | ------- | --------------------- | ------------------- | --------------------------- | ----------------------- |
| SharePoint | SharePoint 2013、SharePoint 2010、SharePoint 2007、SharePoint 3.0 | 物理サーバーまたはハイパー-V/VmWare 仮想マシンとしてデプロイされた SharePoint <br> -------------- <br> SQL AlwaysOn | 物理サーバーまたはオンプレミスの Hyper-V 仮想マシン | 更新プログラム ロールアップ 5 から、Azure へのバックアップをサポートします | SharePoint ファームの回復の保護: ディスクおよび回復ファームからのファーム、データベース、ファイル、リスト項目、および Azure からのデータベース |

## 開始する前に
SharePoint ファームを Azure にバックアップする前に、確認する必要がある点がいくつかあります。

### 前提条件
続行する前にいることを確認すべて、  [の前提条件](backup-azure-dpm-introduction.md#prerequisites) Microsoft Azure Backup を使用して保護するワークロードが満たされています。 前提条件は、バックアップ コンテナーの作成、コンテナー資格情報のダウンロード、Azure Backup エージェントのインストール、コンテナーへのサーバーの登録などのタスクに関するものです。

### DPM エージェント
DPM エージェントを、SharePoint サーバー、SQL サーバー、および SharePoint ファームの一部であるその他のサーバーにインストールする必要があります。 保護エージェントの設定に関する詳細については、次を参照してください。 [保護エージェントのセットアップ](https://technet.microsoft.com/library/hh758039.aspx)します。 唯一の例外は、1 台の Web フロント エンド (WFE) サーバーにだけエージェントをインストールすることです。 保護のエントリ ポイントとして使用するためにエージェントをインストールする必要がある WFE サーバーは 1 台だけです。

### SharePoint ファーム
DPM フォルダーが存在するボリュームには、ファーム内の 1,000 万項目ごとに 2 GB 以上の容量が必要です。 この容量はカタログ生成のために必要です。 DPM が特定の項目 (サイト コレクション、サイト、リスト、ドキュメント ライブラリ、フォルダー、個々のドキュメント、リスト項目) を回復できるよう、カタログ生成では各コンテンツ データベースに含まれる URL のリストが作成されます。 DPM 管理者コンソールの回復タスク領域の [回復可能な項目] ウィンドウで、URL の一覧を確認できます。

### SQL Server
DPM はローカル システムとして実行し、SQL Server データベースをバックアップするには、そのアカウントに SQL Server に対する sysadmin 権限が必要です。 NT authority \system に設定 *sysadmin* バックアップを作成する SQL Server にします。

SharePoint ファームで SQL Server データベースに SQL Server エイリアスが構成されている場合は、DPM によって保護されるフロント エンド Web サーバーに SQL Server クライアント コンポーネントをインストールします。

### SharePoint Server
パフォーマンスは SharePoint ファームのサイズなどのさまざまな要因に依存しますが、一般的なガイダンスとしては、1 つの DPM Server を使用して 25 TB の SharePoint ファームを保護できます。

### DPM の更新プログラム ロールアップ 5
Azure への SharePoint ファームの保護を開始するには、DPM の更新プログラム ロールアップ 5 以降をインストールする必要があります。 更新プログラム ロールアップ 5 を使用すると、Azure に対して (SQL AlwaysOn を使用して構成されている) SharePoint ファームを保護できます。
詳細については、次を参照してください [DPM 更新プログラムのロールアップ 5 をインストールする。]( http://blogs.technet.com/b/dpm/archive/2015/02/11/update-rollup-5-for-system-center-2012-r2-data-protection-manager-is-now-available.aspx)

### サポートされていないもの
1. SharePoint ファームの保護では、検索インデックスまたはアプリケーション サービス データベースは保護されません。 これらのデータベースの保護は別に構成する必要があります。
2. DPM では、スケールアウト ファイル サーバー (SOFS) 共有でホストされている SharePoint SQL データベースのバックアップは提供されません。

## SharePoint の保護の構成
使用して SharePoint VSS ライター サービス (WSS ライター サービス) を構成する必要があります **ConfigureSharePoint.exe** DPM を使用して SharePoint を保護することができます。

検索できる **ConfigureSharePoint.exe** フロント エンド Web サーバー上の [DPM のインストール パス] \bin フォルダーです。 このツールは、保護エージェントに SharePoint ファームに対する資格情報を提供します。 1 つの WFE サーバーでこのツールを実行します。 複数の WFE サーバーがある場合は、保護グループを構成するときに 1 つだけ選択します。

### SharePoint VSS ライター サービスを構成するには
1. WFE サーバーのコマンド プロンプトで、[DPM のインストール場所]\bin\ に移動します。
2. ConfigureSharePoint -EnableSharePointProtection を実行します。
3. ファーム管理者の資格情報を入力します。 このアカウントは、WFE サーバーのローカル管理者グループのメンバーである必要があります。 ファーム管理者がローカル管理者ではない場合は、WFE サーバーで次の権限を付与します。
  - DPM フォルダー (%Program Files%\Microsoft Data Protection Manager\DPM) に対するフル コントロールを WSS_Admin_WPG グループに付与します。
  - DPM レジストリ キー (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager) に対する読み取りアクセスを WSS_Admin_WPG グループに付与します。

>[AZURE.NOTE] SharePoint ファーム管理者の資格情報の変更が発生するたびに、ConfigureSharePoint.exe を再実行する必要があります。

## DPM を使用した SharePoint ファームのバックアップ
前述のように DPM と SharePoint ファームを構成した後は、DPM で SharePoint を保護できます。

### SharePoint ファームを保護するには
1.  **保護** ] タブをクリックして、DPM 管理者コンソールの **新規**します。
    ![新しい [保護] タブ](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)

2.  **保護グループの種類の選択** の画面、 **新しい保護グループの作成** ウィザード、[ **サーバー** ] をクリック **次**します。

    ![保護グループの種類の選択](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)

3.  **グループ メンバーの選択** 画面で、保護し、をクリックする SharePoint サーバーのチェック ボックスをオン **次へ]**

    ![グループ メンバーの選択](./media/backup-azure-backup-sharepoint/select-group-members2.png)

    >[AZURE.NOTE] DPM エージェントをインストール、使用は、ウィザードでサーバーを表示できます。 DPM ではその構造も示されます。 ConfigureSharePoint.exe を実行したので、DPM は SharePoint VSS ライターおよびその対応する SQL Database と通信し、SharePoint ファームの構造 (関連するコンテンツ データベースおよび対応する項目) を認識します。

4.  **データ保護方法の選択** 画面での名前を入力、 *保護グループ* し、適切な選択 *保護方法*します。 クリックして **次**します。

    ![データ保護方法の選択](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)

    >[AZURE.NOTE] 選択するディスクの保護は、時間が短い回復の目標復旧時間の達成に役立ちます。 Azure は、テープと比較してコスト効果の高い長期的な保護対象です。 詳細について参照してください [記事](https://azure.microsoft.com/documentation/articles/backup-azure-backup-cloud-as-tape/)

5.  **短期目標を指定** 画面で、適切な選択 *保有期間の範囲* し、バックアップを実行するときを特定します。

    ![短期的な目標の指定](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)

    >[AZURE.NOTE] 回復が最も 5 日間前のものをデータに必要な多くの場合、ディスク上の 5 日間の保有期間の範囲を選択し、バックアップは、この例の非運用時間帯が発生したことを確認します。

6. 保護グループに割り当てられる記憶域プールのディスク領域を確認し、クリックして **次**します。

7. すべての保護グループについて、DPM はレプリカを格納および管理するためのディスク領域を割り当てます。 この時点で、DPM は選択されたデータのコピーを作成する必要があります。 レプリカを作成し、をクリックする方法とタイミングを選択 **次**します。

    ![レプリカ作成方法の選択](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)

    >[AZURE.NOTE] ネットワーク トラフィックを受けないように、実稼働時間外の時刻を選択します。

8. DPM はレプリカに対して整合性チェックを実行することにより、データの整合性を保証します。 次の 2 つのオプションを使用できます。 整合性チェックを実行するスケジュールを定義することも、レプリカが不整合になったときは常に DPM に自動的にレプリカの整合性チェックを実行させることもできます。 使用するオプションを選択し、クリックして **次**します。

    ![整合性チェック](./media/backup-azure-backup-sharepoint/consistency-check.png)

9.  **オンライン保護データの指定** 画面で、SharePoint ファームを保護する] をクリックして選択 **次**します。

    ![DPM の SharePoint 保護 1](./media/backup-azure-backup-sharepoint/select-online-protection1.png)

10.  **オンライン バックアップ スケジュールの指定** 画面で、希望のスケジュールを選択し、をクリックして **次**します。

    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)

    >[AZURE.NOTE] DPM では、Azure の異なる時間に 2 つの日次バックアップが可能です。

11. バックアップのスケジュールによっては、選択した、 **[オンライン保持ポリシー** 画面で、毎日、毎週、月単位、および年単位のバックアップ ポイントの保有ポリシーを選択します。

    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)

    >[AZURE.NOTE] DPM では、異なるバックアップ ポイントに対して異なる保有ポリシーを選択できる祖父-父-子の保有期間スキームを使用します。

12. ディスクと同様に、Azure でも最初の参照ポイント レプリカを作成する必要があります。 クリックして Azure の初期バックアップ コピーを作成するため、設定オプションを選択して **次**します。

    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)

13. 選択した設定を確認、 **概要** ページを **グループの作成**します。 保護グループが作成されると、成功メッセージが表示されます。

    ![概要](./media/backup-azure-backup-sharepoint/summary.png)

## DPM を使用したディスクからの SharePoint アイテムの復元
次の例で、 *Recovering SharePoint item* が誤って削除し、回復する必要があります。
![DPM の SharePoint 保護 4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)

1. 開いている、 **DPM 管理者コンソール**します。 DPM によって保護されているすべての SharePoint ファームが、[保護] タブに表示されます。

    ![DPM の SharePoint 保護 3](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)

2. アイテムの回復を開始する選択、 **回復** ] タブをクリックします。

    ![DPM の SharePoint 保護 5](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)

3. SharePoint でを検索することができます、 *Recovering SharePoint item* 回復ポイントの範囲内でワイルドカード ベース検索を使用します。

    ![DPM の SharePoint 保護 6](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)

4. 検索結果から適切な回復ポイントを選択して右クリックして、選択 **回復**

5. さまざまな回復ポイント間を移動して、回復するデータベースまたはアイテムを選択できます。 選択 **日付 > の復旧時間** し、正しい選択 **データベース > SharePoint ファーム > 回復ポイント > 項目**します。

    ![DPM の SharePoint 保護 7](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)

6. アイテムを右クリックし、選択 **回復** を開くには、 **回復ウィザード**します。 クリックして **次**します。

    ![回復の選択の確認](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)

7. 実行し、をクリックする回復の種類を選択 **次**します。

    ![回復の種類](./media/backup-azure-backup-sharepoint/select-recovery-type.png)

    >[AZURE.NOTE] 上記の例では、元の SharePoint サイトを回復する項目を示します。

8. 選択、 **回復プロセス** を使用します。
    - 選択 **されて回復ファームを使用して回復** SharePoint ファームが変更されていないは復元中、回復ポイントと同じかどうかです。
    - 選択 **回復ファームを使用して回復** 回復ポイントを作成してから、SharePoint ファームが変更された場合。

    ![回復プロセス](./media/backup-azure-backup-sharepoint/recovery-process.png)

9. 一時的にデータベースを回復するためのステージング SQL インスタンスの場所を指定し、DPM サーバーおよび SharePoint サーバー上でアイテムを回復するためのステージング ファイル共有を指定します。

    ![ステージングの場所 1](./media/backup-azure-backup-sharepoint/staging-location1.png)

    DPM は、SharePoint アイテムをホストしているコンテンツ データベースを、一時的な SQL インスタンスにアタッチします。 DPM サーバーは、コンテンツ データベースからアイテムを回復し、DPM サーバー上のステージング ファイルの場所に格納します。 次に、DPM サーバーのステージングの場所に回復されたアイテムを、SharePoint ファーム上のステージング場所にエクスポートする必要があります。

    ![ステージングの場所 2](./media/backup-azure-backup-sharepoint/staging-location2.png)

10. 選択 **回復オプションの指定** SharePoint ファームに対するセキュリティ設定を適用すると、回復ポイントのセキュリティ設定を適用します。 クリックして **次**します。

    ![回復オプション](./media/backup-azure-backup-sharepoint/recovery-options.png)

    >[AZURE.NOTE] 使用されるネットワーク帯域幅を調整することができます。 これにより、業務時間中の運用サーバーへの影響を最小限にします。

11. 概要情報を確認し、クリックして **回復** ファイルの回復を開始します。

    ![回復の概要](./media/backup-azure-backup-sharepoint/recovery-summary.png)

12. 選択して、 **監視** ] タブで、 **DPM 管理者コンソール** を表示する、 *ステータス* の回復のです。

    ![回復の状態](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)

    >[AZURE.NOTE] ファイルが復元されます。 SharePoint サイトを更新して、復元されたファイルを確認すできます。

## DPM を使用した Azure からの SharePoint データベースの復元

1. SharePoint コンテンツ データベースを回復するには、さまざまな回復ポイントの間を移動して (前述したように)、回復する回復ポイントを選択します。

    ![DPM の SharePoint 保護 8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)

2. SharePoint の回復ポイントをダブルクリックして、使用可能な SharePoint カタログ情報を表示します。

    > [AZURE.NOTE] SharePoint ファームが Azure での長期的な保有期間の保護されているために、カタログ情報 (メタデータ) の DPM サーバーで利用可能はありません。 その結果、特定時点の SharePoint コンテンツ データベースを回復する必要があるときは常に、SharePoint ファームを再カタログ化する必要があります。

3. クリックして **を再カタログ化**します。

    ![DPM の SharePoint 保護 10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

     **クラウドの再カタログ化** ステータス ウィンドウが表示されます。

    ![DPM の SharePoint 保護 11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    カタログ化が完了すると、ステータスに変化 *成功*します。 クリックして **閉じる**します。

    ![DPM の SharePoint 保護 12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)

4. DPM に示すように SharePoint オブジェクトをクリックして **回復** ] タブをクリックして、コンテンツ データベースの構造を取得します。 右クリック > **回復**します。

    ![DPM の SharePoint 保護 13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)

5. この時点では、次の [回復した手順を](#restore-a-sharepoint-item-from-disk-using-dpm) ディスクから Sharepoint コンテンツ データベースを回復します。

## FAQ
Q: どのバージョンの DPM SQL 2014 および SQL 2012 (SP2 を) サポートします。<br>
A: DPM 2012 R2 更新プログラム ロールアップ 4 では、同じサポートしています

Q: SharePoint アイテム回復元の場所に SharePoint では、(ディスク上の保護) が SQL AlwaysOn を使用して構成されている場合ですか。<br>
A: はい、元の SharePoint サイトにアイテムを回復できます。

SharePoint が SQL AlwaysOn で構成されている場合に元の場所に SharePoint データベースを復元する q: でしょうか。<br>
A: SharePoint データベースは SQL AlwaysOn で構成されているので、可用性グループ (AG) を削除しない限り、変更することはできません。 結果として、DPM は元の場所にデータベースを復元できません。 別の SQL インスタンスに SQL Database を回復することはできます。

## 次のステップ
- SharePoint の DPM 保護に関する詳細については、確認できる [ビデオ シリーズ - SharePoint の DPM 保護](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)
- レビュー [System Center 2012 - Data Protection Manager のリリース ノート](https://technet.microsoft.com/library/jj860415.aspx)
- レビュー [System Center 2012 sp1 Data Protection Manager 用のリリース ノート](https://technet.microsoft.com/library/jj860394.aspx)

