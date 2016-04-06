<properties
    pageTitle="Microsoft Azure Backup - PowerShell を使用した DPM バックアップのデプロイと管理| Microsoft Azure"
    description="PowerShell を使用して、Data Protection Manager (DPM) 用に Microsoft Azure Backup をデプロイおよび管理する手順の説明"
    services="backup"
    documentationCenter=""
    authors="AnuragMehrotra"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/26/2015"
    ms.author="jimpark"; "aashishr"; "sammehta"; "anuragm"/>


# PowerShell を使用して Data Protection Manager (DPM) サーバーに Microsoft Azure Backup をデプロイおよび管理する手順
この記事では、PowerShell を使用して、DPM サーバー上に Microsoft Azure Backup をセットアップし、バックアップと回復を管理する方法を示します。

## PowerShell 環境のセットアップ
PowerShell を使用して Data Protection Manager から Azure へのバックアップを管理する前に、PowerShell に正しい環境があることを確認してください。 PowerShell セッションの開始時に、必ず次のコマンドレットを実行して適切なモジュールをインポートし、DPM コマンドレットを適切に参照できるようにしてください。

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome to the DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## セットアップと登録
開始するには

1. [最新の PowerShell のダウンロード](https://github.com/Azure/azure-powershell/releases) (必要な最小バージョン: 1.0.0)
2. 切り替えることによって、Azure Backup コマンドレットを有効にする *AzureResourceManager* モードを使用して、 **Switch-azuremode** コマンドレット。

```
PS C:\> Switch-AzureMode AzureResourceManager
```

PowerShell を使用して次のセットアップおよび登録タスクを自動化できます。

- バックアップ コンテナーの作成
- Microsoft Azure Backup エージェントのインストール
- Microsoft Azure Backup サービスへの登録
- ネットワークの設定
- 暗号化の設定

### バックアップ コンテナーを作成していること

> [AZURE.WARNING] お客様は、最初に Azure Backup を使用して、サブスクリプションで使用される Azure Backup プロバイダーを登録する必要があります。 これは、Register-AzureProvider -ProviderNamespace "Microsoft.Backup" コマンドを実行して行うことができます。

使用して新しいバックアップ資格情報コンテナーを作成することができます、 **新規 AzureRMBackupVault** 内線番号です。 バックアップ コンテナーは ARM リソースであるため、リソース グループ内に配置する必要があります。 管理者特権の Azure PowerShell コンソールで、次のコマンドを実行します。

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

使用して指定されたサブスクリプション内のすべてのバックアップ コンテナーの一覧を取得できる、 **Get AzureRMBackupVault** 内線番号です。


### DPM サーバーへの Microsoft Azure Backup エージェントのインストール
Microsoft Azure Backup エージェントをインストールする前に、Windows Server に、インストーラーをダウンロードする必要があります。 インストーラーからの最新バージョンを取得できます、 [Microsoft ダウンロード センター](http://aka.ms/azurebackup_agent) またはバックアップ資格情報コンテナーのダッシュ ボード] ページからです。 インストーラーを簡単にアクセスできる場所に保存 * C:\Downloads\* します。

エージェントをインストールするには、管理者特権の PowerShell コンソールで、次のコマンドを実行 **DPM サーバーで**:

```
PS C:\> MARSAgentInstaller.exe /q
```

これにより、エージェントはすべて既定のオプションが指定されてインストールされます。 インストールは、バックグラウンドで数分かかります。 指定しない場合、 */nu* オプション、 **Windows Update** 、更新プログラムを確認するインストールの最後にウィンドウが開きます。

エージェントが、インストールされているプログラムの一覧に表示されます。 インストールされているプログラムの一覧を表示する [ **コントロール パネルの [** > **プログラム** > **プログラムと機能**です。

![インストールされているエージェント](./media/backup-dpm-automation/installed-agent-listing.png)

#### インストール オプション
コマンドラインで利用可能なすべてのオプションを表示するには、次のコマンドを使用します。

```
PS C:\> MARSAgentInstaller.exe /?
```

利用可能なオプションは、次のとおりです。

| オプション | 詳細 | 既定値 |
| ---- | ----- | ----- |
| /q | サイレント インストール | - |
| /p:"location" | Azure Backup エージェントのインストール フォルダーへのパス。 | C:\Program files \microsoft Azure Recovery Services エージェント |
| /s:"location" | Azure Backup エージェントのキャッシュ フォルダーへのパス。 | C:\Program files \microsoft Azure Recovery Services agent \scratch |
| /m | Microsoft Update をオプトイン | - |
| /nu | インストールが完了した後に更新プログラムを確認しません。 | - |
| /d | Microsoft Azure Recovery Services エージェントをアンインストールします。 | - |
| /ph | プロキシ ホストのアドレス | - |
| /po | プロキシ ホストのポート番号 | - |
| /pu | プロキシ ホストのユーザー名 | - |
| /pw | プロキシ パスワード | - |

### Microsoft Azure Backup サービスへの登録
Azure Backup サービスで登録するには、いることを確認する必要があります、 [の前提条件](backup-azure-dpm-introduction.md) が満たされています。 前提条件は、以下のとおりです。

- 有効な Azure サブスクリプションがあること
- バックアップ コンテナーがあること

コンテナーの資格情報をダウンロードするには、実行、 **Get AzureBackupVaultCredentials** などの任意の場所で、Azure PowerShell コンソールとストアに内線番号 * C:\Downloads\* します。

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

資格情報コンテナーにコンピューターを登録するときを使用して、 [Start-dpmcloudregistration](https://technet.microsoft.com/library/jj612787) コマンドレット。

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

これにより、"TestingServer" という名前の DPM サーバーは、Microsoft Azure Backup コンテナーに、指定されたコンテナーの資格情報を使用して登録されます。

> [AZURE.IMPORTANT] 資格情報コンテナーの資格情報ファイルを指定するのに相対パスを使用しません。 このコマンドレットへの入力には、絶対パスを指定する必要があります。

### 初期構成の設定
DPM サーバーが Azure Backup 資格情報コンテナーに登録されると、既定のサブスクリプション設定で起動されます。 これらのサブスクリプションの設定には、ネットワーク、暗号化、およびステージング領域が含まれます。 まず、既存 (既定値) の設定を使用してハンドルを取得する必要があります。 サブスクリプションの設定の変更を開始する、 [Get-dpmcloudsubscriptionsetting](https://technet.microsoft.com/library/jj612793) コマンドレット。

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

このローカル PowerShell オブジェクトをすべての変更が加えられた ```$setting```  、完全なオブジェクトがそれらを保存するには、DPM および Azure Backup にコミットを使用して、 [Set-dpmcloudsubscriptionsetting](https://technet.microsoft.com/library/jj612791) コマンドレットです。 変更が保持されていることを確認するには、```–Commit``` フラグを使用します。 コミットしない限り、Azure Backup により設定が適用または使用されません。

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### ネットワーク
インターネット上の DPM コンピューターの Azure Backup サービスの接続にプロキシ サーバーを使用している場合、続行するには、プロキシ サーバーの設定を指定する必要があります。 使用してこれは、 ```-ProxyServer```, 、```-ProxyPort```, 、```-ProxyUsername``` と ```ProxyPassword``` を持つパラメーター、 [Set-dpmcloudsubscriptionsetting](https://technet.microsoft.com/library/jj612791) コマンドレットです。 この例では、プロキシ サーバーがないため、プロキシ関連の情報はないことを明示的に示しています。

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

特定の曜日セットに対して、帯域幅の使用を、```-WorkHourBandwidth```オプションと```-NonWorkHourBandwidth```オプションで制御することもできます。 この例ではスロットルは設定しません。

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### ステージング領域の構成
DPM サーバーで実行されている Microsoft Azure Backup エージェントには、クラウドから復元されるデータ用の一時的なストレージ (ローカル ステージング領域) が必要です。 ステージング領域を使用して、構成、 [Set-dpmcloudsubscriptionsetting](https://technet.microsoft.com/library/jj612791) コマンドレットおよび ```-StagingAreaPath``` パラメーター。

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

上記の例では、ステージング領域に設定されます *C:\StagingArea* PowerShell オブジェクト ```$setting```します。 指定したフォルダーが既に存在することを確認します。存在しない場合は、サブスクリプション設定の最終コミットが失敗します。


### 暗号化の設定
データの機密性を保護するために、Microsoft Azure Backup に送信されるバックアップ データは暗号化されます。 暗号化パスフレーズは、復元時にデータの暗号化を解除するための "パスワード" になります。 設定したら、この情報をセキュリティで保護された安全な場所に保管することが重要です。

次の例では、最初のコマンドは、文字列を変換。 ```passphrase123456789``` セキュリティで保護された文字列を名前付きセキュリティで保護された文字列を変数に割り当てます ```$Passphrase```します。 2 番目のコマンド、セキュリティで保護された文字列を設定する ```$Passphrase``` バックアップの暗号化のパスワードとして。

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [AZURE.IMPORTANT] 設定することで、安全なパスフレーズ情報を保持します。 このパスフレーズがないと、Azure からデータを復元できません。

この時点で、必要な変更はすべて ```$setting``` オブジェクトに行われています。 変更を忘れずにコミットします。

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## Microsoft Azure Backup へのデータの保護
このセクションでは、運用サーバーを DPM に追加し、ローカルの DPM 記憶域、Azure Backup の順にデータを保護します。 この例では、ファイルとフォルダーをバックアップする方法を示します。 このロジックは、DPM がサポートされたすべてのデータ ソースのバックアップに簡単に拡張して適用できます。 すべての DPM バックアップは、4 つの部分で保護グループ (PG) によって制御されます。

1. **グループのメンバーが** 保護可能なすべてのオブジェクトの一覧を示します (とも呼ばれます *Datasources* DPM では) 同じ保護グループで保護します。 たとえば、運用 VM とテスト SQL Server データベースがそれぞれ異なるバックアップ要件を持つため、別々の PG で保護したいという場合があります。 運用サーバー上のデータソースをバックアップする前に、DPM エージェントがサーバーにインストールされており、DPM により管理されていることを確認する必要があります。 手順に従います [DPM エージェントをインストールする](https://technet.microsoft.com/library/bb870935.aspx) と適切な DPM サーバーにリンクします。
2. **データの保護方法** 、目的のバックアップ場所 - テープ、ディスク、およびクラウドを指定します。 この例では、ローカル ディスクとクラウドへのデータを保護します。
3. A **バックアップ スケジュール** バックアップを実行する必要がある場合と、DPM サーバーと実稼働サーバーの間でデータを同期する頻度を指定します。
4. A **保持スケジュール** Azure で回復ポイントを保持する期間を指定します。

### 保護グループの作成
まず、新しい保護グループを使用して、作成、 [New-dpmprotectiongroup](https://technet.microsoft.com/library/hh881722) コマンドレットです。

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

上記のコマンドレットがという名前の保護グループを作成する *ProtectGroup01*します。 既存の保護グループを後で変更して、バックアップを Azure クラウドに追加することもできます。 ただし、保護グループを変更する新規または既存の必要がありますハンドルを取得する、 *変更可能な* オブジェクトを使用して、 [かを問わず](https://technet.microsoft.com/library/hh881713) コマンドレットです。

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### 保護グループへのグループ メンバーの追加
各 DPM エージェントは、インストール先のサーバー上でデータソースの一覧を認識しています。 保護グループにデータソースを追加するには、DPM エージェントは最初にデータソースの一覧を DPM サーバーに送信する必要があります。 1 つ以上のデータソースが選択され、保護グループに追加されます。 このために必要な PowerShell の手順は、次のとおりです。

1. DPM エージェントから DPM が管理するすべてのサーバーの一覧を取得します。
2. 特定のサーバーの選択
3. サーバー上のすべてのデータソースの一覧を取得します。
4. 1 つ以上のデータソースの選択と保護グループへの追加

DPM エージェントがインストールされているし、DPM サーバーによって管理されるサーバーの一覧を取得すると、 [Get DPMProductionServer](https://technet.microsoft.com/library/hh881600) コマンドレットです。 この例でフィルター処理や名前を持つ PS をのみ設定されます *productionserver01* バックアップします。

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

データ ソースの一覧を取得 ```$server``` を使用して、 [Get-dpmdatasource](https://technet.microsoft.com/library/hh881605) コマンドレットです。 この例ではフィルター処理して、ボリュームの * D:\* バックアップ用に構成することができます。 このデータ ソースが保護グループに追加しを使用して、 [Add-dpmchilddatasource](https://technet.microsoft.com/library/hh881732) コマンドレットです。 使用してください、 *忘れず* 保護グループ オブジェクト ```$MPG``` を追加します。

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

この手順を、選択したすべてのデータソースが保護グループに追加されるまで必要なだけ繰り返します。 1 つのデータソースで開始して、保護グループを作成するためのワークフローを完了し、後で保護グループにさらにデータソースを追加することもできます。

### データ保護方法の選択
次の手順が、メソッドを使用して保護を指定するには、データ ソースを保護グループに追加すると、 [Set-dpmprotectiontype](https://technet.microsoft.com/library/hh881725) コマンドレットです。 この例では、保護グループをローカル ディスクとクラウド バックアップ用にセットアップします。 使用してクラウドを保護するデータ ソースを指定する必要があります、 [Add-dpmchilddatasource](https://technet.microsoft.com/library/hh881732.aspx) コマンドレットのオンライン フラグとします。

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### 保有期間の範囲設定
使用して、バックアップ ポイントの保有期間を設定、 [Set-dpmpolicyobjective](https://technet.microsoft.com/library/hh881762) コマンドレットです。 バックアップ スケジュールが定義される前に保有期間を設定するのは不自然な操作に思えるかもしれませんが、```Set-DPMPolicyObjective``` コマンドレットでは、後で変更できる既定のバックアップ スケジュールが自動的に設定されます。 バックアップ スケジュールを設定した後で、保有ポリシーを設定することも常に可能です。

次の例では、コマンドレットはディスク バックアップ用の保有期間パラメーターを設定します。 ここではバックアップの保有期間は 10 日間で、運用サーバーと DPM サーバーの間でデータが 6 時間ごとに同期されます。 ```SynchronizationFrequencyMinutes``` ではバックアップ ポイントの作成頻度は定義されませんが、データの DPM サーバーへのコピー頻度は定義されます。これにより、バックアップの肥大化を回避できます。

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

Azure へのバックアップ (DPM は、オンライン バックアップを参照) の保有期間の範囲を構成できます [祖父-父-子スキーム (GFS) を使用して保有期間を長く使用](backup-azure-backup-cloud-as-tape.md)します。 つまり、日次、週次、月次、年次の保有期間ポリシーを組み合わせた保有ポリシーを定義できます。 この例では、複雑な保有期間スキームを表す配列を作成し、保有期間の範囲を使用し、構成、 [Set-dpmpolicyobjective](https://technet.microsoft.com/library/hh881762) コマンドレットです。

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### バックアップ スケジュールの設定
```Set-DPMPolicyObjective``` コマンドレットを使用して保護目標を指定する場合、DPM は既定のバックアップ スケジュールを自動的に設定します。 既定のスケジュールを変更するには、使用、 [Get-dpmpolicyschedule](https://technet.microsoft.com/library/hh881749) コマンドレットに続いて、 [Set-dpmpolicyschedule](https://technet.microsoft.com/library/hh881723) コマンドレットです。

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

上記の例で ```$onlineSch``` は、GFS スキームの保護グループ用の既存のオンライン保護スケジュールを含む 4 つの要素を持つ配列です。

1. ```$onlineSch[0]``` は日次スケジュールを含みます
2. ```$onlineSch[1]``` は週次スケジュールを含みます
3. ```$onlineSch[2]``` は月次スケジュールを含みます
4. ```$onlineSch[3]``` は年次スケジュールを含みます

したがって、週次のスケジュールを変更する必要がある場合は、```$onlineSch[1]``` を参照します。

### 初回バックアップ
データソースを初めてバックアップする場合、DPM レプリカ ボリューム上に保護対象データソースのコピーを作成する初期レプリカを、DPM により作成する必要があります。 このアクティビティは特定の期間、スケジュール設定することができますできますトリガーすることも、手動でを使用して、 [Set-dpmreplicacreationmethod](https://technet.microsoft.com/library/hh881715) コマンドレットとパラメーター ```-NOW```します。

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### DPM レプリカと回復ポイントのボリューム サイズの変更
DPM レプリカ ボリュームとボリュームを使用してシャドウ コピーのサイズを変更することもできます。 [セット DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) コマンドレットで、下の例。
Get-datasourcediskallocation データソース $DS
Set-datasourcediskallocation データソース $DS-protectiongroup $MPG-手動 ReplicaArea (2 gb) ShadowCopyArea (2 gb)

### 保護グループに対する変更のコミット
最後に、DPM が新しい保護グループの構成ごとにバックアップを実行する前に、変更をコミットする必要があります。 これを使用して、 [Set-dpmprotectiongroup](https://technet.microsoft.com/library/hh881758) コマンドレットです。

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## バックアップ ポイントの表示
使用することができます、 [Get DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) コマンドレットをデータ ソースのすべての回復ポイントの一覧を取得します。 この例で紹介します。
- すべての pg を DPM サーバーは配列に格納されます。 ```$PG```
- 対応するデータソースを取得します ```$PG[0]```
- データソースのすべての回復ポイントを取得します。

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## Azure で保護されているデータの復元
データの復元は、```RecoverableItem``` オブジェクトと ```RecoveryOption``` オブジェクトの組み合わせです。 前のセクションで、データソースのバックアップ ポイントの一覧を取得しました。

次の例では、バックアップ ポイントと回復対象を組み合わせて、Hyper-V 仮想マシンを Microsoft Azure Backup から復元する方法を示します。 次のトピックがあります。

- 回復オプションを使用して、作成、  [New-dpmrecoveryoption](https://technet.microsoft.com/library/hh881592) コマンドレットです。
- ```Get-DPMRecoveryPoint``` コマンドレットを使用したバックアップ ポイントの配列の取得。
- 復元元のバックアップ ポイントの選択。

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

使用されているコマンドは、任意のデータソースの種類に合わせて簡単に拡張できます。

## 次のステップ

- DPM の Azure Backup の詳細については「 [DPM Backup の概要](backup-azure-dpm-introduction.md)


