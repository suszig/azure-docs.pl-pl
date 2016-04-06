<properties
    pageTitle="バックアップ コンテナーで別の DPM サーバーからデータを復元する | Microsoft Azure"
    description="コンテナーに登録されている DPM サーバーから、Azure Backup コンテナーに保護しているデータを復元します。"
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

# バックアップ コンテナーで別の DPM サーバーからデータを復元する
コンテナーに登録されている DPM サーバーから、Azure Backup コンテナーに保護しているデータを復元できるようになりました。 この復元プロセスは DPM 管理コンソールに完全統合されており、その他の復元ワークフローと似ています。

データを回復する必要があります、バックアップ コンテナーに別の DPM サーバーから [System Center Data Protection Manager UR7](https://support.microsoft.com/en-us/kb/3065246) と [最新の Azure Backup エージェント](http://aka.ms/azurebackup_agent)します。

## 別の DPM サーバーからデータを復元する
別の DPM サーバーからデータを復元するには:

1.  **回復** ] タブをクリックして、DPM 管理コンソールの **"外部 DPM の追加"** (で画面の左下)。

    ![外部 DPM の追加](./media/backup-azure-alternate-dpm-server/add-external-dpm.png)

2. 新しいダウンロード **コンテナーの資格情報** に関連付けられている資格情報コンテナーから、 **DPM サーバー** バックアップ コンテナーに登録された DPM サーバーの一覧から、DPM サーバーを選択でき、データが復元されている場合、 **暗号化パスフレーズ** が回復中でデータが含まれる DPM サーバーに関連付けられています。

    ![外部 DPM の資格情報](./media/backup-azure-alternate-dpm-server/external-dpm-credentials.png)

    >[AZURE.NOTE] 同じ登録資格情報コンテナーに関連付けられている DPM サーバーのみでは、それぞれのデータを回復できます。

    外部 DPM サーバーが正常に追加されると、外部の DPM サーバーとローカルの DPM サーバーからのデータを表示できる、 **回復** ] タブをクリックします。

3. 外部 DPM サーバーで保護されている本稼働サーバーの一覧を閲覧し、該当するデータ ソースを選択します。

    ![外部 DPM サーバーの参照](./media/backup-azure-alternate-dpm-server/browse-external-dpm.png)

4. 選択 **月と年** から、 **回復ポイント** ボックスの一覧で、必要な選択 **回復日付** を選択し、回復ポイントの作成時のため、 **復旧時間**します。

    ファイルとフォルダーの一覧が下のページに表示されるので、閲覧し、任意の場所に復元できます。

    ![外部 DPM サーバーの回復ポイント](./media/backup-azure-alternate-dpm-server/external-dpm-recoverypoint.png)

5. 適切な項目を右クリックし、クリックして **回復**します。

    ![外部 DPM 回復](./media/backup-azure-alternate-dpm-server/recover.png)

6. レビュー、 **選択範囲を回復**します。 回復するバックアップ コピーの日時とバックアップ コピーの作成元を確認します。 選択範囲が正しくない場合は、クリックして **キャンセル** 適切な回復ポイントを選択する [回復] タブに戻る。 選択範囲が正しい場合は、クリックして **次**します。

    ![外部 DPM 回復の概要](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-summary.png)

7. 選択 **別の場所に回復**します。 **[参照]** 回復用の正しい場所をします。

    ![外部の DPM 回復の別場所](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-alternate-location.png)

8. 関連するオプションを選択して **コピーを作成**, 、**Skip**, 、または **上書き**します。
    - **コピーを作成** は、名前の競合がある場合に、ファイルのコピーを作成します。
    - **Skip** 、名前の競合がある場合に、ファイルの回復をスキップします。
    - **上書き** 、名前の競合が発生した場合に指定した場所に既存のコピーが上書きされます。

    適切なオプションを選択 **セキュリティの復元**します。 データを復元する復元先コンピューターのセキュリティ設定または回復ポイントが作成された時点で生成物に適用されたセキュリティを適用できます。

    識別するかどうか、 **通知** 、回復が正常に完了すると、送信されます。

    ![外部の DPM 回復の通知](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-notifications.png)

9.  **概要** 画面には、これまで選択したオプションが一覧表示します。 クリックすると **'Recover'**, 、データは、適切なオンプレミスの場所に回復します。

    ![外部 DPM 回復のオプション概要](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-options-summary.png)

    >[AZURE.NOTE] 回復ジョブを監視できる、 **監視** DPM サーバーのタブをクリックします。

    ![回復の監視](./media/backup-azure-alternate-dpm-server/monitoring-recovery.png)

10. クリックして **クリア外部 DPM** 上、 **回復** 外部 DPM サーバーのビューを削除する DPM サーバーのタブをクリックします。

    ![外部 DPM の消去](./media/backup-azure-alternate-dpm-server/clear-external-dpm.png)

## エラー メッセージのトラブルシューティング
|番号 |  エラー メッセージ | トラブルシューティングの手順 |
| :-------------: |:-------------| :-----|
|1.|        このサーバーは資格情報コンテナーが指定するコンテナーに登録されていません。|  **原因:** 選択されている資格情報コンテナーの資格情報ファイルが、回復を実行する DPM サーバーに関連付けられているバックアップ資格情報コンテナーに属していない場合に、このエラーが表示されます。 <br> **解決方法:** DPM サーバーが登録されているバックアップ資格情報コンテナーから資格情報コンテナーの資格情報ファイルをダウンロードします。|
|2.|        回復可能なデータがないか、選択したサーバーが DPM サーバーではありません。|   **原因:** バックアップ コンテナーに登録された DPM 2012 R2 UR7 でないその他の DPM サーバーがあるまたは DPM 2012 R2 UR7 で DPM サーバーでは、メタデータがアップロードされていないが、選択したサーバーは、DPM サーバーではなく (別名 Windows Server または Windows クライアント)。 <br> **解決方法:** 、バックアップ資格情報コンテナーに登録されている他の DPM サーバーがある場合は、SCDPM 2012 R2 UR7 および最新の Azure Backup エージェントがインストールされていることを確認します。 <br>DPM 2012 R2 UR7 でバックアップ資格情報コンテナーに登録されている他の DPM サーバーがある場合、回復プロセスを開始する UR7 のインストール後に、1 日まで待ちます。 以前にクラウドに保護されたすべてのバックアップのメタデータを夜間ジョブがアップロードします。 このデータを回復に利用できます。|
|3.|        このコンテナーには他の DPM サーバーが登録されていません。|   **原因:** は、回復の試行された資格情報コンテナーに登録されている他の DPM サーバー DPM 2012 R2 UR7 またはことはありません。<br>**解決方法:** 、バックアップ資格情報コンテナーに登録されている他の DPM サーバーがある場合は、SCDPM 2012 R2 UR7 および最新の Azure Backup エージェントがインストールされていることを確認します。<br>DPM 2012 R2 UR7 でバックアップ資格情報コンテナーに登録されている他の DPM サーバーがある場合、回復プロセスを開始する UR7 のインストール後に、1 日まで待ちます。 以前にクラウドに保護されたすべてのバックアップのメタデータを夜間ジョブがアップロードします。 このデータを回復に利用できます。|
|4.|        入力した暗号化パスフレーズは、次のサーバーに関連付けられているパスフレーズとは一致しません。 **<server name>**|  **原因:** が回復中で、DPM サーバーのデータからデータを暗号化するプロセスで使用する暗号化のパスフレーズが提供される暗号化のパスフレーズと一致しません。 エージェントはデータを復号できません。 そのため、回復は失敗します。<br>**解決方法:** が回復中でデータが含まれる DPM サーバーに関連付けられている正確な同じ暗号化パスフレーズを指定してください。|

## よく寄せられる質問:
1. **UR7 と最新の Azure Backup エージェントをインストールしましたが、別の DPM サーバーから外部 DPM サーバーを追加できません。なぜですか。**

    A) を (を使用して更新プログラムのロールアップ更新プログラム ロールアップ 7 以降) をクラウドに保護されているデータ ソースと既存の DPM サーバーには、少なくとも 1 日を開始する UR7 および最新の Azure Backup エージェントをインストールした後を待機する必要が *外部 DPM の追加サーバー*します。 これは DPM 保護グループのメタデータを Azure にアップロードするために必要です。 最初にこれは夜間ジョブで行われます。

2. **Azure Backup エージェントのバージョンの最小要件は何ですか。**

    A) この機能を有効にするには 2.0.8719.0 以降の Azure Backup エージェントが必要です。  コントロール パネルに移動して、azure Backup エージェントのバージョンを検証できる **>** すべてのコントロール パネルの項目 **>** プログラムと機能 **>** Microsoft Azure Recovery Services エージェント。 バージョンが 2.0.8719.0 未満の場合は、ダウンロード、 [最新の Azure Backup エージェント](https://go.microsoft.com/fwLink/?LinkID=288905) してインストールします。

    ![外部 DPM の消去](./media/backup-azure-alternate-dpm-server/external-dpm-azurebackupagentversion.png)

## 次のステップ:
•   [Azure Backup の FAQ](backup-azure-backup-faq.md)


