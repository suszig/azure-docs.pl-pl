<properties
    pageTitle="PowerShell を使用した Windows Server/クライアント バックアップのデプロイと管理| Microsoft Azure"
    description="PowerShell を使用して Microsoft Azure Backup をデプロイおよび管理する手順の説明"
    services="backup"
    documentationCenter=""
    authors="aashishr"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/20/2015"
    ms.author="aashishr"; "jimpark"/>


# PowerShell を使用して Windows Server/Windows Client に Microsoft Azure Backup をデプロイおよび管理する手順
この記事では、PowerShell を使用して、Windows Server または Windows クライアント上に Microsoft Azure Backup をセットアップし、バックアップと回復を管理する方法を示します。

[AZURE.INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

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
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

使用して指定されたサブスクリプション内のすべてのバックアップ コンテナーの一覧を取得できる、 **Get AzureRMBackupVault** 内線番号です。


### Microsoft Azure Backup エージェントのインストール
Microsoft Azure Backup エージェントをインストールする前に、Windows Server に、インストーラーをダウンロードする必要があります。 インストーラーからの最新バージョンを取得できます、 [Microsoft ダウンロード センター](http://aka.ms/azurebackup_agent) またはバックアップ資格情報コンテナーのダッシュ ボード] ページからです。 インストーラーを簡単にアクセスできる場所に保存 * C:\Downloads\* します。

エージェントをインストールするには、管理者特権の PowerShell コンソールで、次のコマンドを実行します。

```
PS C:\> MARSAgentInstaller.exe /q
```

これにより、エージェントはすべて既定のオプションが指定されてインストールされます。 インストールは、バックグラウンドで数分かかります。 指定しない場合、 */nu* オプション、 **Windows Update** 、更新プログラムを確認するインストールの最後にウィンドウが開きます。 インストールすると、エージェントが、インストールされているプログラムの一覧に表示されます。

インストールされているプログラムの一覧を表示する [ **コントロール パネルの [** > **プログラム** > **プログラムと機能**です。

![インストールされているエージェント](./media/backup-client-automation/installed-agent-listing.png)

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
Azure Backup サービスで登録するには、いることを確認する必要があります、 [の前提条件](backup-configure-vault.md) が満たされています。 前提条件は、以下のとおりです。

- 有効な Azure サブスクリプションがあること
- バックアップ コンテナーがあること

コンテナーの資格情報をダウンロードするには、実行、 **Get AzureRMBackupVaultCredentials** などの任意の場所で、Azure PowerShell コンソールとストアに内線番号 * C:\Downloads\* します。

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

資格情報コンテナーにコンピューターを登録するときを使用して、 [Start-obregistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) コマンドレット。

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration -VaultCredentials $cred -Confirm:$false

CertThumbprint      : 7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName : test-vault
Region              : West US

Machine registration succeeded.
```

> [AZURE.IMPORTANT] 資格情報コンテナーの資格情報ファイルを指定するのに相対パスを使用しません。 このコマンドレットへの入力には、絶対パスを指定する必要があります。

### ネットワークの設定
プロキシ サーバーを介して Windows コンピューターをインターネットに接続した場合、プロキシ設定もエージェントに指定できます。 この例では、プロキシ サーバーがないため、プロキシ関連の情報はないことを明示的に示しています。

特定の曜日セットに対して、帯域幅の使用を、```work hour bandwidth```オプションと```non-work hour bandwidth```オプションで制御することもできます。

プロキシと帯域幅の詳細の設定を使用して、 [Set-obmachinesetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) コマンドレット。

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

### 暗号化の設定
データの機密性を保護するために、Microsoft Azure Backup に送信されるバックアップ データは暗号化されます。 暗号化パスフレーズは、復元時にデータの暗号化を解除するための "パスワード" になります。

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
Server properties updated successfully
```

> [AZURE.IMPORTANT] 設定することで、安全なパスフレーズ情報を保持します。 このパスフレーズがないと、Azure からデータを復元できません。

## ファイルとフォルダーのバックアップ
Windows サーバーおよびクライアントから Microsoft Azure Backup へのすべてのバックアップは、ポリシーによって管理されます。 ポリシーは 3 つの部分で構成されます。

1. A **バックアップ スケジュール** バックアップを実行して、サービスと同期する必要がある場合を指定します。
2. A **保持スケジュール** Azure で回復ポイントを保持する期間を指定します。
3. A **ファイル内包/除外仕様** をどのようなバックアップするかを指定します。

このドキュメントでは、バックアップを自動化するため、構成を何も行っていないことを前提としています。 まず、新しいバックアップ ポリシーを使用して、作成することで、 [New-obpolicy](https://technet.microsoft.com/library/hh770416.aspx) コマンドレットして使用することです。

```
PS C:\> $newpolicy = New-OBPolicy
```

この時点でポリシーは空で、どの項目を含むまたは除外するか、またバックアップをいつ実行するか、どこに格納するかを定義する他のコマンドレットが必要です。

### バックアップ スケジュールの構成
ポリシーの 3 つの部分の最初を使用して作成されるバックアップのスケジュール、 [New-obschedule](https://technet.microsoft.com/library/hh770401) コマンドレットです。 バックアップ スケジュールでは、バックアップをいつ実行するかを定義します。 スケジュールを作成するとき、次の 2 つの入力パラメーターを指定する必要があります。

- **曜日** バックアップを実行します。 バックアップ ジョブは、週に 1 回、毎日、またはこれらを組み合わせたさまざまな間隔で実行できます。
- **時間帯** バックアップが実行する必要があります。 1 日最大 3 回のバックアップを起動する時間を定義できます。

たとえば、毎週土曜日と日曜日の午後 4 時に実行されるようにバックアップ ポリシーを構成できます。

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

バックアップのスケジュールが、ポリシーに関連付けする必要があるし、を使用してこれを実現することができます、 [Set-obschedule](https://technet.microsoft.com/library/hh770407) コマンドレットです。

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### 保有ポリシーの構成
保有ポリシーでは、バックアップ ジョブから作成した回復ポイントをどのくらいの期間保持するかを定義します。 使って新しい保有ポリシーを作成するときに、 [New-obretentionpolicy](https://technet.microsoft.com/library/hh770425) コマンドレット、Azure Backup で保持されるバックアップの回復ポイントが必要な日数を指定することができます。 次の例では、7 日間の保有ポリシーを設定します。

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

保持ポリシーは、コマンドレットを使用してメインのポリシーと関連付ける必要があります [Set-obretentionpolicy](https://technet.microsoft.com/library/hh770405):

```
PS C:\> Set-OBRetentionPolicy -Policy $newpolicy -RetentionPolicy $retentionpolicy

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```
### バックアップするファイルを含むまたは除外する
```OBFileSpec``` オブジェクトにより、ファイルをバックアップに含めるか除外するかを定義します。 ここでは、コンピューター上の保護されたファイルとフォルダーを範囲から除外する一連のルールを示します。 ファイル内包または除外ルールは必要に応じていくつでも保持でき、ポリシーに関連付けることができます。 新しい OBFileSpec オブジェクトを作成して、次の操作を実行できます。

- 含めるファイルおよびフォルダーを指定
- 除外するファイルおよびフォルダーを指定
- フォルダー内のデータの再帰バックアップを指定。または、指定したフォルダー内の最上位レベルのファイルのみをバックアップするかどうかを指定。

後者は、New-OBFileSpec コマンドの NonRecursive フラグを使用して実行できます。

次の例では、ボリューム C: および D: をバックアップし、Windows フォルダーと任意の一時フォルダー内の OS バイナリを除外します。 ここで作成するためには 2 種類のファイルを使用して仕様、 [New-obfilespec](https://technet.microsoft.com/library/hh770408) コマンドレット内包用と除外のいずれかのいずれかです。 使用して、ポリシーに関連付けられているファイルの仕様が作成されたら、 [Add-obfilespec](https://technet.microsoft.com/library/hh770424) コマンドレットです。

```
PS C:\> $inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")

PS C:\> $exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude

PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $inclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid


PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $exclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### ポリシーの適用
これで、ポリシー オブジェクトが完了し、バックアップ スケジュール、保有ポリシー、およびファイルの包含/除外リストに関連付けられました。 次に、Microsoft Azure Backup で使用するためにこのポリシーをコミットできます。 適用する前に新しく作成されたポリシーを使用して、サーバーに関連付けられている既存のバックアップ ポリシーがないことを確認、 [Remove-obpolicy](https://technet.microsoft.com/library/hh770415) コマンドレットです。 ポリシーを削除すると確認のダイアログが表示されます。 確認をスキップするには、コマンドレットで ```-Confirm:$false``` フラグを使用します。

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

ポリシー オブジェクトのコミットを実行を使用して、 [Set-obpolicy](https://technet.microsoft.com/library/hh770421) コマンドレットです。 ここでも、確認のダイアログが表示されます。 確認をスキップするには、コマンドレットで ```-Confirm:$false``` フラグを使用します。

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want to save this backup policy ? [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

既存のバックアップ ポリシーの使用方法の詳細を表示する、 [Get-obpolicy](https://technet.microsoft.com/library/hh770406) コマンドレットです。 ドリル ダウンできるを使用して、 [Get-obschedule](https://technet.microsoft.com/library/hh770423) バックアップ スケジュール用のコマンドレットと [Get-obretentionpolicy](https://technet.microsoft.com/library/hh770427) 保有ポリシー用のコマンドレット

```
PS C:> Get-OBPolicy | Get-OBSchedule
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing

PS C:> Get-OBPolicy | Get-OBRetentionPolicy
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing

PS C:> Get-OBPolicy | Get-OBFileSpec
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### アドホック バックアップの実行
バックアップ ポリシーが設定されると、スケジュールに従ってバックアップが実行されます。 可能な使用は、アドホック バックアップをトリガーする、 [Start-obbackup](https://technet.microsoft.com/library/hh770426) コマンドレット。

```
PS C:> Get-OBPolicy | Start-OBBackup
Taking snapshot of volumes...
Preparing storage...
Estimating size of backup items...
Estimating size of backup items...
Transferring data...
Verifying backup...
Job completed.
The backup operation completed successfully.
```

## Microsoft Azure Backup からのデータの復元
このセクションでは、Microsoft Azure Backup からのデータの復元を自動化する方法を手順に従って説明します。 次の手順を実行します。

1. ソース ボリュームを選択する
2. バックアップの復元ポイントを選択する
3. 復元する項目を選択する
4. 復元プロセスをトリガーする

### ソース ボリュームの選択
Microsoft Azure Backup から項目を復元するには、まず、項目のソースを特定する必要があります。 Windows サーバーまたは Windows クライアントのコンテキストでコマンドを実行するため、コンピューターは既に特定されています。 ソースを特定する次の手順は、それを含むボリュームを特定することです。 ボリュームまたは実行してこのコンピューターからバックアップを取得するソースの一覧、 [Get-obrecoverablesource](https://technet.microsoft.com/library/hh770410) コマンドレットです。 このコマンドは、このサーバー/クライアントでバックアップされるすべてのソースの配列を返します。

```
PS C:> $source = Get-OBRecoverableSource
PS C:> $source
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### バックアップの復元ポイントの選択
実行することでバックアップ ポイントの一覧を取得できる、 [Get-obrecoverableitem](https://technet.microsoft.com/library/hh770399.aspx) コマンドレットと適切なパラメーターです。 この例では、ソース ボリュームの最新のバックアップ ポイントを選択 *d:* 使用して特定のファイルを修復するとします。

```
PS C:> $rps = Get-OBRecoverableItem -Source $source[1]
IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :

IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 17-Jun-15 6:31:31 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :
```
オブジェクト ```$rps``` は、バックアップ ポイントの配列です。 最初の要素は最新のポイントで、N 番目の要素は最も古いポイントです。 最新のポイントを選択するには、```$rps[0]``` を使用します。

### 復元する項目の選択
正確なファイルまたはリストア、再帰的に使用するフォルダーを識別するために、 [Get-obrecoverableitem](https://technet.microsoft.com/library/hh770399.aspx) コマンドレットです。 ここでは、フォルダー階層を ```Get-OBRecoverableItem``` のみを使用して参照できます。

この例では、ファイルを復元する場合に *finances.xls* オブジェクトを使用してを参照できます ```$filesFolders[1]```します。

```
PS C:> $filesFolders = Get-OBRecoverableItem $rps[0]
PS C:> $filesFolders
IsDir : True
ItemNameFriendly : D:\MyData\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\
LocalMountPoint : D:\
MountPointName : D:\
Name : MyData
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime : 15-Jun-15 8:49:29 AM

PS C:> $filesFolders = Get-OBRecoverableItem $filesFolders[0]
PS C:> $filesFolders
IsDir : False
ItemNameFriendly : D:\MyData\screenshot.oxps
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\screenshot.oxps
LocalMountPoint : D:\
MountPointName : D:\
Name : screenshot.oxps
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 228313
ItemLastModifiedTime : 21-Jun-14 6:45:09 AM

IsDir : False
ItemNameFriendly : D:\MyData\finances.xls
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\finances.xls
LocalMountPoint : D:\
MountPointName : D:\
Name : finances.xls
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 96256
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM
```

復元する項目を検索するには、```Get-OBRecoverableItem``` コマンドレットを使用します。 この例では検索する *finances.xls* このコマンドを実行してのファイルのハンドルを取得しました。

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### 復元プロセスのトリガー
復元プロセスをトリガーするには、まず、回復オプションを指定する必要があります。 使用してそのため、 [New-obrecoveryoption](https://technet.microsoft.com/library/hh770417.aspx) コマンドレットです。 この例と仮定するファイルを復元する *C:\temp*します。 だコピー先のフォルダーに既に存在するファイルをスキップする *C:\temp*します。 こうした回復オプションを作成するには、次のコマンドを使用します。

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

今すぐ復元をトリガーを使用して、 [Start-obrecovery](https://technet.microsoft.com/library/hh770402.aspx) コマンドを選択した ```$item``` の出力から、 ```Get-OBRecoverableItem``` コマンドレット。

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
The recovery operation completed successfully.
```


## Microsoft Azure Backup エージェントのアンインストール
Microsoft Azure Backup エージェントのアンインストールは、次のコマンドを使用して実行できます。

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

エージェント バイナリをコンピューターからアンインストールすると、考慮すべき別の結果が生じます。

- ファイル フィルターがコンピューターから削除され、変更の追跡が停止されます。
- すべてのポリシー情報がコンピューターから削除されますが、ポリシー情報は引き続きサービスに格納されます。
- すべてのバックアップ スケジュールが削除され、以降のバックアップは実施されません。

ただし、Azure に格納されたデータは、設定した保有ポリシーに基づいて維持されます。 古いポイントの期限は自動的に切れます。

## リモート管理
Microsoft Azure Backup エージェント、ポリシー、およびデータ ソースに関する管理はすべて、PowerShell を使ってリモートで実行できます。 リモートで管理されるコンピューターは、適切に準備されている必要があります。

既定では、WinRM サービスは手動で開始されるように設定されています。 スタートアップの種類に設定する必要があります *自動* 、サービスを開始する必要があります。 WinRM サービスが実行されていることを確認する、Status プロパティの値になります *を実行している*します。

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

リモート処理用に PowerShell を構成します。

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

コンピューターを、エージェントのインストールを始め、リモートで管理できるようになりました。 たとえば、次のスクリプトは、エージェントをリモート コンピューターにコピーし、インストールします。

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## 次のステップ
Azure Backup for Windows Server/Client の詳細については、以下を参照してください。

- [Azure Backup の概要](backup-configure-vault.md)
- [Windows Server のバックアップ](backup-azure-backup-windows-server.md)


