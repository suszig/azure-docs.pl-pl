<properties
 pageTitle="HPC Pack クラスター コンピューティング ノードの管理 | Microsoft Azure"
 description="Azure の HPC Pack クラスター コンピューティング ノードを追加、削除、起動、停止するための PowerShell スクリプト ツールについて説明します"
 services="virtual-machines"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="09/28/2015"
 ms.author="danlep"/>

# Azure の HPC Pack クラスターのコンピューティング ノードの数と可用性を管理する

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


Azure Vm の HPC Pack クラスターを作成した場合は追加、削除を簡単にする方法
開始 (プロビジョニング) または (プロビジョニング解除) コンピューティング ノードの数の Vm を停止します
を指定します。 これらのタスクには、実行される Azure PowerShell スクリプト
ヘッド ノード VM (HPC Pack 2012 R2 更新プログラム 1 以降) にインストールされます。 これらのスクリプトでは、数を制御できます。
コストを制御できるように、HPC Pack クラスター リソースの可用性です。

>[AZURE.NOTE] スクリプトはヘッド ノードの [%CCP\_HOME%bin フォルダーです。 必要があります。
各スクリプトを管理者として実行します。

## 前提条件

* **Azure Vm の HPC Pack クラスター** -以上を使用して、クラシック (サービス管理) の配置モデルで、HPC Pack クラスターを作成する HPC Pack 2012 R2 更新プログラム 1。 たとえば、Azure Marketplace の HPC Pack VM イメージや Azure PowerShell スクリプトを利用し、デプロイを自動化できます。 情報と前提条件は、次を参照してください。 [HPC Pack IaaS デプロイメント スクリプトで HPC クラスターを作成](virtual-machines-hpcpack-cluster-powershell-script.md)します。

* **Azure 発行設定ファイルまたは管理証明書** -ヘッド ノードで次のいずれかを実行する必要があります。

    * **発行設定ファイルのインポート、Azure**します。 これを行うには、ヘッド ノードで次の Azure PowerShell コマンドレットを実行します。

    ```
    Get-AzurePublishSettingsFile 
         
    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```
    
    * **ヘッド ノードで Azure 管理証明書を構成する**です。 .cer ファイルがある場合、それを CurrentUser\My 証明書ストアにインポートし、Azure 環境 (either AzureCloud または AzureChinaCloud) に対して次の Azure PowerShell コマンドレットを実行します。

    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## コンピューティング ノード VM の追加

使用コンピューティング ノードを追加、 **Add-hpciaasnode.ps1** スクリプトです。

### 構文
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### パラメーター

* **ServiceName** -新しいコンピューティング ノード Vm クラウド サービスの名前に追加されます。

* **ImageName** -Azure クラシック ポータルまたは Azure PowerShell コマンドレットを使用して入手できる Azure VM イメージの名前を **Get-azurevmimage**します。 このイメージは次の要件を満たしている必要があります。

    1. Windows オペレーティング システムをインストールする必要があります。

    2. コンピューティング ノード ロールに HPC Pack をインストールする必要があります。

    3. イメージは、公開 Azure VM イメージではなく、ユーザー カテゴリのプライベート イメージである必要があります。

* **数量**のコンピューティング ノードを追加する Vm の数。

* **InstanceSize** のコンピューティング ノード Vm のサイズ。

* **DomainUserName** -ドメイン ユーザー名、新しい Vm をドメインに参加させるために使用されます。

* **DomainUserPassword*** ドメインのユーザーのパスワード

* **NodeNameSeries** 0 名前付けパターンを計算ノード (省略可能)。 フォーマットでなければなりません & lt;*Root\_Name*& gt; & lt;*Start\_Number*& gt; % です。 たとえば、｢MyCN%10%｣は MyCN11 から始まる一連のコンピューティング ノード名を意味します。 指定されていない場合、HPC クラスターに構成されているノード命名規則が使用されます。

### 例

次の例では、クラウドで 20 サイズ大規模なコンピューティング ノード Vm を追加します。
VM イメージ hpccnimage1 に基づいてサービス hpcservice1 します。

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## コンピューティング ノード VM の削除

コンピューティング ノードを削除、 **Remove-hpciaasnode.ps1** スクリプトです。

### 構文

```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### パラメーター

 * **名前** -削除するクラスター ノードの名前。 ワイルドカードを利用できます。 パラメーター セット名は「Name」です。 両方を指定することはできません、 **名前** と **ノード** パラメーター。

* **ノード** * の HPC PowerShell コマンドレットで取得できるを削除するノードの HpcNode オブジェクト [Get-hpcnode](https://technet.microsoft.com/library/dn887927.aspx)します。 パラメーター セット名は「Node」です。 両方を指定することはできません、 **名前** と **ノード** パラメーター。

* **DeleteVHD** (省略可能) - 削除された Vm の関連付けられているディスクを削除するのに設定します。

* **強制的に** (省略可能) - それらを削除する前に強制的に HPC ノードをオフラインに設定します。

* **確認** (省略可能) - コマンドを実行する前に確認のダイアログを表示します。

* **WhatIf** の設定を実際には、コマンドを実行せず、コマンドを実行した場合の処理について説明します。

### 例

名前が始まるでオフラインのノードを強制的に次の例
*HPCNode-CN -* ノードとそれに関連付けられているディスクを削除するとします。

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## コンピューティング ノード VM の起動

開始ノードの計算、 **Start-hpciaasnode.ps1** スクリプトです。

### 構文

```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### パラメーター

* **名前** -開始するクラスター ノードの名前。 ワイルドカードを利用できます。 パラメーター セット名は「Name」です。 両方を指定することはできません、 **名前** と **ノード** パラメーター。

* **ノード**-HPC PowerShell コマンドレットで取得できる開始するノードの HpcNode オブジェクト [Get-hpcnode](https://technet.microsoft.com/library/dn887927.aspx)します。 パラメーター セット名は「Node」です。 両方を指定することはできません、 **名前** と **ノード** パラメーター。

### 例

次の例では、名前が始まるノードを起動 *HPCNode-CN -*です。

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## コンピューティング ノード VM の停止

コンピューティング ノードの停止、 **Stop-hpciaasnode.ps1** スクリプト

### 構文

```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### パラメーター


* **名前**-を停止するクラスター ノードの名前。 ワイルドカードを利用できます。 パラメーター セット名は「Name」です。 両方を指定することはできません、 **名前** と **ノード** パラメーター。

* **ノード** -HPC PowerShell コマンドレット Get-hpcnode で取得できるを停止するノードの HpcNode オブジェクトです。 パラメーター セット名は「Node」です。 両方を指定することはできません、 **名前** と **ノード** パラメーター。

* **強制的に** (省略可能) - それらを停止する前に強制的に HPC ノードをオフラインに設定します。

### 例

名前が始まるでオフラインのノードを強制的に次の例
*HPCNode-CN -* され、ノードが停止します。

Stop-HPCIaaSNode.ps1 –Name HPCNodeCN-* -Force

## 次のステップ

* 望む場合
自動的に拡大または縮小」の手順に従って、Azure コンピューティング リソース
ジョブと、クラスター上のタスクの現在のワークロードを参照してください [拡大および縮小を Azure でコンピューティング リソース、HPC Pack クラスター](virtual-machines-hpcpack-cluster-node-autogrowshrink.md)します。

