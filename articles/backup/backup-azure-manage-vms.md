
<properties
    pageTitle="Azure 仮想マシンのバックアップを管理および監視する | Microsoft Azure"
    description="Azure 仮想マシンのバックアップを管理および監視する方法を説明する"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/29/2015"
    ms.author="aashishr"; "jimpark"; "trinadhk"/>

# Azure 仮想マシンのバックアップを管理および監視する

## 保護された仮想マシンを管理する

以下では、保護された仮想マシンを管理する方法を説明します。

1. 表示および管理するバーチャル マシンのバックアップ設定をクリックして、 **保護された項目** ] タブをクリックします。

2. 表示する保護された項目の名前をクリックして、 **バックアップの詳細** ] タブで、前回のバックアップに関する情報が表示されます。

    ![仮想マシンのバックアップ](./media/backup-azure-manage-vms/backup-vmdetails.png)

3. 表示および管理するバーチャル マシンのバックアップ ポリシー設定をクリックして、 **ポリシー** ] タブをクリックします。

    ![仮想マシンのポリシー](./media/backup-azure-manage-vms/manage-policy-settings.png)

     **バックアップ ポリシー** タブには既存のポリシーが表示されます。 必要に応じて変更できます。 新しいポリシーを作成する必要がある場合 **作成** 上、 **ポリシー** ページです。 ポリシーを削除する場合は、そのポリシーにどの仮想マシンも関連付けられていないことをご確認ください。

    ![仮想マシンのポリシー](./media/backup-azure-manage-vms/backup-vmpolicy.png)

4. 仮想マシンの詳細については、動作や状態を入手できます、 **ジョブ** ページです。 一覧内のジョブをクリックして詳細情報を表示するか、特定の仮想マシンのジョブをフィルターします。

    ![ジョブ](./media/backup-azure-manage-vms/backup-job.png)

## オンデマンドでの仮想マシンのバックアップ
仮想マシンに保護が構成されていれば、その仮想マシンについてオンデマンド バックアップを作成できます。 仮想マシンの最初のバックアップが保留中の場合、オンデマンド バックアップを実行すると、仮想マシンの完全なコピーが Azure のバックアップ コンテナーに作成されます。 最初のバックアップが完了している場合、オンデマンド バックアップを実行すると、前のバックアップから変更された部分のみが Azure のバックアップ コンテナーに送られます。

仮想マシンのオンデマンド バックアップを作成するには:

1. 移動し、 **保護された項目** ページをオンに **Azure の仮想マシン** として **型** (選択されていない) 場合] をクリック **選択** ] ボタンをクリックします。

    ![VM の種類](./media/backup-azure-manage-vms/vm-type.png)

2. オンデマンドのバックアップを行うし、をクリックする仮想マシンを選択 **今すぐバックアップ** ページの下部にあるボタンをクリックします。

    ![今すぐバックアップ](./media/backup-azure-manage-vms/backup-now.png)

    これにより、選択した仮想マシンにバックアップ ジョブが作成されます。 このジョブによって作成される回復ポイントの保有期間は、仮想マシンに関連付けられているポリシーで指定される期間と同じになります。

    ![バックアップ ジョブの作成](./media/backup-azure-manage-vms/creating-job.png)

    >[AZURE.NOTE] 仮想マシンに関連付けられたポリシーを表示するには内の仮想マシンをドリルダウン、 **保護された項目** ページし、バックアップ ポリシー] タブに移動します。

3. ジョブが作成されると、クリックすると **ジョブの表示** ジョブ] ページに対応するジョブを表示する、トースト バーのボタンをクリックします。

    ![バックアップ ジョブが作成されました](./media/backup-azure-manage-vms/created-job.png)

4. ジョブが正常に終了した後、仮想マシンの復元に使用できる回復ポイントが作成されます。 回復ポイントの列の値が 1 ずつ加算これはまた **保護された項目** ページです。

## 仮想マシンの保護を停止する
次の方法を使って、仮想マシンの以後のバックアップを停止することができます。

- 仮想マシンに関連付けられているバックアップ データを Azure Backup コンテナー内に保持する
- 仮想マシンに関連付けられているバックアップ データを削除する

仮想マシンに関連付けられているバックアップ データを保持する方法を選択した場合、バックアップ データを使用して仮想マシンを復元できます。 このような仮想マシンの詳しい料金について、次のようにクリックします。 [ここ](http://azure.microsoft.com/pricing/details/backup/)します。

仮想マシンに対する保護を停止するには:

1. 移動 **保護された項目** ページをオンに **Azure の仮想マシン** をクリックして、(選択されていない) 場合、フィルターの種類として **選択** ] ボタンをクリックします。

    ![VM の種類](./media/backup-azure-manage-vms/vm-type.png)

2. 仮想マシンを選択し、をクリックして **保護の停止** ページの下部にあります。

    ![保護の停止](./media/backup-azure-manage-vms/stop-protection.png)

3. 既定では、Azure Backup では、仮想マシンに関連付けられているバックアップ データは削除されません。

    ![保護の停止の確認](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    バックアップ データを削除するには、このチェック ボックスをオンにします。

    ![チェックボックス](./media/backup-azure-manage-vms/checkbox.png)

    バックアップを停止する理由を選択してください。 これは省略可能ですが、理由を示すことで、Azure Backup の開発チームがフィードバックを調査し、顧客シナリオに優先順位を付けるために役立ちます。

4. をクリックして **送信** を送信するボタン、 **保護を停止する** ジョブです。 をクリックして **ジョブの表示** 、対応するジョブを表示する **ジョブ** ページです。

    ![保護の停止](./media/backup-azure-manage-vms/stop-protect-success.png)

    オフにした場合 **関連付けられたバックアップ データを削除** オプション **保護の停止** ウィザード、ジョブの完了後、保護の状態に変わります **保護停止**します。 データは、明示的に削除されるまで Azure Backup に残ります。 仮想マシンを選択して、データをいつでも削除できます、 **保護された項目** ページとクリックして **削除**します。

    ![停止された保護](./media/backup-azure-manage-vms/protection-stopped-status.png)

    選択した場合、 **関連付けられたバックアップ データの削除** オプション、バーチャル マシンをできませんの一部、 **保護された項目** ページです。

## 仮想マシンの再保護
オフにした場合、 **関連付けられたバックアップ データの削除** オプション **保護の停止**, 、登録済みの仮想マシンのバックアップと同様の手順に従って、仮想マシンを再び保護することができます。 再保護された仮想マシンは、保護の停止前に保持されていたバックアップ データと、再保護された後に作成された回復ポイントを持つことになります。

再保護の後、バーチャル マシンの保護の状態に変更されます **Protected** より前の回復ポイントがある場合 **保護の停止**します。

  ![再保護された VM](./media/backup-azure-manage-vms/reprotected-status.png)

>[AZURE.NOTE] 仮想マシンを再保護する場合は、仮想マシンが保護されている最初のポリシーよりも別のポリシーを選択できます。

## 仮想マシンを登録解除する

バックアップ コンテナーから仮想マシンを削除する場合

1. をクリックして、 **登録解除** ページの下部にあるボタンをクリックします。

    ![保護を無効にする](./media/backup-azure-manage-vms/unregister-button.png)

    確認を求める画面の下部に、トースト通知が表示されます。 クリックして **はい** を続行します。

    ![保護を無効にする](./media/backup-azure-manage-vms/confirm-unregister.png)

## バックアップ データの削除
仮想マシンに関連付けられているバックアップ データを削除できるのは、次のタイミングです。

- "保護を停止" ジョブ中
- 仮想マシンで "保護を停止" ジョブが完了した後

バックアップのデータでは、仮想マシンを削除する、 *保護停止* 後が正常に完了の状態、 **バックアップの停止** ジョブ。

1. 移動、 **保護された項目** ページをオンに **Azure の仮想マシン** として *型* ] をクリックし、 **選択** ] ボタンをクリックします。

    ![VM の種類](./media/backup-azure-manage-vms/vm-type.png)

2. 仮想マシンを選択します。 仮想マシンで配置する **保護停止** 状態です。

    ![保護の停止](./media/backup-azure-manage-vms/protection-stopped-b.png)

3. クリックして、 **削除** ページの下部にあるボタンをクリックします。

    ![バックアップの削除](./media/backup-azure-manage-vms/delete-backup.png)

4.  **バックアップ データを削除** ウィザードでは (強く推奨)、バックアップ データを削除する理由を選択し、クリックして **送信**します。

    ![バックアップ データの削除](./media/backup-azure-manage-vms/delete-backup-data.png)

5. これで、選択した仮想マシンのバックアップ データを削除するジョブが作成されます。 クリックして **ジョブの表示** ジョブ] ページに対応するジョブを表示します。

    ![データ削除の成功](./media/backup-azure-manage-vms/delete-data-success.png)

    ジョブが完了すると、仮想マシンに対応するエントリがされなくなります **保護されている項目** ページです。

## ダッシュボード
 **ダッシュ ボード** ページが Azure の仮想マシン、ストレージ、過去 24 時間に関連付けられたジョブに関する情報を確認できます。 バックアップの状態と関連付けられているバックアップ エラーを表示できます。

![ダッシュボード](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

>[AZURE.NOTE] ダッシュ ボード内の値は 24 時間ごとに更新されます。

## 監査操作
Azure Backup では、顧客がトリガーしたバックアップ操作の "操作ログ" を確認できます。この機能を使えば、バックアップ コンテナーで行われた管理操作を正確に把握することができます。 操作ログは、バックアップ操作の事後分析や監査を行うのに役立ちます。

操作ログには、次の操作が記録されます。

- 登録
- 登録解除
- 保護の構成
- バックアップ ([今すぐバックアップ] で行うオンデマンド バックアップとスケジュールされたバックアップの両方)
- 復元
- 保護の停止
- バックアップ データの削除
- ポリシーの追加
- ポリシーの削除
- ポリシーの更新
- ジョブの取り消し

バックアップ コンテナーに対応する操作ログを表示するには、次の操作を行います。

1. 移動 **管理サービス** をクリックして Azure ポータル、 **操作ログ** ] タブをクリックします。

    ![操作ログ](./media/backup-azure-manage-vms/ops-logs.png)

2. このフィルターでは、次のように選択します。 **バックアップ** として *型* でバックアップ資格情報コンテナーの名前を指定し *サービス名* ] をクリック **送信**します。

    ![操作ログ フィルター](./media/backup-azure-manage-vms/ops-logs-filter.png)

3. 操作ログですべての操作を選択してクリックして  **詳細** 操作に対応する詳細を表示します。

    ![操作ログ取得の詳細](./media/backup-azure-manage-vms/ops-logs-details.png)

     **詳細ウィザード** 操作がトリガーされると、ジョブ Id、リソースをこの操作が開始されると、操作の開始時刻に関する情報が含まれています。

    ![操作の詳細](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## アラート通知
ポータルで、ジョブのカスタム アラート通知を取得できます。 この通知は、操作ログのイベントに PowerShell ベースのアラート ルールを定義することによって取得できます。

イベント ベースのアラートは、Azure リソース モードで動作します。 Azure リソース モードに切り替えるには、管理者特権のコマンド モードで次のコマンドレットを実行します。

```
PS C:\> Switch-AzureMode AzureResourceManager
```

バックアップが失敗した場合にアラートを発行するカスタム通知を定義する場合、コマンドのサンプルは次のようになります。

```
PS C:\> Add-AlertRule -Operator GreaterThanOrEqual -Threshold 1 -ResourceId '/subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault' -EventName Backup  -EventSource Administrative -Level Error -OperationName 'Microsoft.Backup/backupVault/Backup' -ResourceProvider Microsoft.Backup -Status Failed  -SubStatus Failed -RuleType Event -Location eastus -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -Name Backup-Failed -Description 'Backup failed for one of the VMs in vault trinadhkVault' -CustomEmails 'contoso@microsoft.com' -SendToServiceOwners
```

**ResourceId**: で取得できます操作ログ ポップアップから前のセクションで説明したようにします。 具体的には、操作の詳細を示したポップアップ ウィンドウに表示される ResourceUri が、このコマンドレットに指定する ResourceId となります。

**EventName**: IaaS vm は、バックアップ、サポートされている値は、- 登録、登録解除、ConfigureProtection、アラートのバックアップ、復元、StopProtection、DeleteBackupData、CreateProtectionPolicy、DeleteProtectionPolicy、UpdateProtectionPolicy

**レベル**: サポートされている値は、- 情報、エラーです。 操作が失敗した場合のアラートには Error を使用し、ジョブが成功した場合のアラートには Informational を使用します。

**OperationName**: これは"Microsoft.Backup/backupvault/ 形式になります<EventName>"を前述のとおりです。

**ステータス**: サポートされている値は開始、成功および失敗しました。 Status に Succeeded を指定する場合、Level には Informational を指定することをお勧めします。

**副状態**: バックアップ操作のステータスと同じ

**RuleType**: として使いやすく *イベント* バックアップのアラートは、イベントに基づいています。

**ResourceGroup**: 操作がトリガーされるリソースのリソース グループ。 これは、ResourceId の値から取得できます。 フィールドの間の値 */resourcegroups/* と */providers/* 値 ResourceId のリソース グループの値です。

**名前**: アラートのルールの名前。

**説明**: アラートのルールの説明。

**CustomEmails**: アラートの通知を送信するカスタムの電子メール アドレスを指定

**SendToServiceOwners**: このオプションは、すべての管理者とサブスクリプションの共同管理者にアラートの通知を送信します。

アラートの電子メール サンプルは、次のようになります。

ヘッダーのサンプル:

![アラート ヘッダー](./media/backup-azure-manage-vms/alert-header.png)

アラートの電子メールの本文のサンプル:

![アラートの本文](./media/backup-azure-manage-vms/alert-body.png)

### アラートに関する制限事項
イベント ベースのアラートには、次の制限事項が適用されます。

1. アラートは、バックアップ コンテナー内のすべての仮想マシン上でトリガーされます。 バックアップ コンテナー内の特定の仮想マシンのセットのアラートを取得するようにカスタマイズすることはできません。
2. アラートは、次のアラート期間にアラートの条件に一致するイベントがトリガーされなかった場合、自動解決されます。 使用 *WindowSize* 警告のトリガーを起動する期間を設定する追加 AlertRule コマンドレットのパラメーターです。

## 次のステップ

- [Azure VM の復元](backup-azure-restore-vms.md)


