<properties 
   pageTitle="Azure Automation のデータの管理 | Microsoft Azure"
   description="この記事には、Azure Automation 環境の管理に関する複数のトピックが含まれています。  現在は、データの保持、Azure Automation のバックアップ、Azure Automation での障害復旧が含まれています。"
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="11/02/2015"
   ms.author="bwren;sngun" />

# Azure Automation のデータの管理

この記事には、Azure Automation 環境の管理に関する複数のトピックが含まれています。

## データの保持

Azure Automation でリソースを削除する場合、完全に削除される前に、監査目的で 90 日間保持されます。  この期間にリソースを表示または使用することはできません。  このポリシーは、削除済みのオートメーション アカウントに属するリソースにも適用されます。

Azure Automation は、90 日を超えるジョブを自動的に削除した後、完全に消去します。

次の表は、さまざまなリソースの保持ポリシーをまとめたものです。

|データ|ポリシー|
|:---|:---|
|アカウント|ユーザーによってアカウントが削除された日から 90 日後に完全に消去されます。|
|資産|ユーザーによって資産が削除された日から 90 日後に、または資産を保持するアカウントがユーザーによって削除された日から 90 日後に完全に消去されます。|
|モジュール|ユーザーによってモジュールが削除された日から 90 日後に、またはモジュールを保持するアカウントがユーザーによって削除された日から 90 日後に完全に消去されます。|
|Runbooks|ユーザーによってリソースが削除された日から 90 日後に、またはリソースを保持するアカウントがユーザーによって削除された日から 90 日後に完全に消去されます。|
|ジョブ|最終更新日から 90 日後に削除された後、完全に消去されます。 これは、ジョブが完了、停止、または中断された後に実行されます。|
|ノード構成/MOF ファイル| 古いノード構成は、新しいノード構成が生成された後、90 日間で完全に削除されます。|
|DSC ノード| ノードが Azure ポータルを使用するオートメーション アカウントから登録した後 90 日間は完全に削除または [登録解除 AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) Windows PowerShell コマンドレット。 また、ノードは、ユーザーによってノードを保持するアカウントが削除された後、90 日間で完全に削除されます。 |
|ノード レポート| そのノードの新しいレポートが生成された後、90 日間で完全に削除削除されます。|

保持ポリシーはすべてのユーザーに適用され、現在カスタマイズすることはできません。

## Azure Automation のバックアップ

Microsoft Azure でオートメーション アカウントを削除すると、Runbook、モジュール、構成、設定、ジョブ、資産など、アカウント内のすべてのオブジェクトが削除されます。 アカウントを削除した後にオブジェクトを修復することはできません。  削除する前に、以下の情報を使用してオートメーション アカウントの内容をバックアップできます。 

### Runbook

ご使用の runbook を Azure 管理ポータルを使用してスクリプト ファイルにエクスポートすること、または [Get-azureautomationrunbookdefinition](https://msdn.microsoft.com/library/dn690269.aspx) Windows PowerShell コマンドレット。  説明したように、これらのスクリプト ファイルが別のオートメーション アカウントにインポートする [を作成するか、Runbook をインポートする](https://msdn.microsoft.com/library/dn643637.aspx)です。


### 統合モジュール

Azure Automation から統合モジュールをエクスポートすることはできません。  オートメーション アカウントの外部で統合モジュールを使用できることを確認する必要があります。

### 資産

エクスポートすることはできません [資産](https://msdn.microsoft.com/library/dn939988.aspx) Azure Automation からです。  Microsoft Azure 管理ポータルを使用して、変数、資格情報、証明書、接続、およびスケジュールの詳細を記録する必要があります。  その後、別のオートメーションにインポートする Runbook で使用されているすべての資産を手動で作成する必要があります。

使用する [Azure コマンドレット](https://msdn.microsoft.com/library/dn690262.aspx) 後で参照できるしに保存したり、暗号化されていない資産の詳細を取得または別のオートメーション アカウントで同等の資産を作成します。

暗号化されている変数または資格情報のパスワード フィールドの値を、コマンドレットを使用して取得することはできません。  これらの値がわからないかどうかは、それらを取得するには、runbook を使用して、 [Get-automationvariable](https://msdn.microsoft.com/library/dn940012.aspx) と [Get-automationpscredential](https://msdn.microsoft.com/library/dn940015.aspx) アクティビティ。

Azure Automation から証明書をエクスポートすることはできません。  すべての証明書が Azure の外部で使用できることを確認する必要があります。

### DSC の構成

Azure 管理ポータルを使用してスクリプト ファイルを構成をエクスポートすることができますか 
[エクスポート AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) Windows PowerShell コマンドレット。 これらの構成は、別のオートメーション アカウントでインポートして使用できます。


##Azure Automation での geo レプリケーション

Azure Automation アカウントでの標準である geo レプリケーションでは、アカウント データを別の地理的リージョンにバックアップして冗長性を確保します。 アカウントのセットアップ時にはプライマリ リージョンを選択できます。アカウントにはセカンダリ リージョンが自動的に割り当てられます。 プライマリ リージョンからコピーされた、セカンダリ データは、データの損失が発生した場合にも継続的に更新されます。  

次の表に、使用可能なプライマリ リージョンとセカンダリ リージョンのペアを示します。

|プライマリ            |セカンダリ
| ---------------   |----------------
|米国中南部   |米国中北部
|米国東部 2          |米国中央部
|西ヨーロッパ        |北ヨーロッパ
|東南アジア    |東アジア
|東日本         |西日本

万一、プライマリ リージョンのデータが失われた場合、Microsoft はその復旧を試みます。 プライマリ リージョンを復元できない場合は、geo フェールオーバーが実行されます。影響を受けた顧客には、サブスクリプションを通じてこのことが通知されます。


