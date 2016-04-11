<properties 
    pageTitle="リソース マネージャーの監査操作 | Microsoft Azure" 
    description="リソース マネージャーの監査ログを使用し、ユーザーの操作やエラーを確認します。 PowerShell、Azure、REST を表示します。" 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/02/2015" 
    ms.author="tomfitz"/>

# リソース マネージャーの監査操作

ソリューションのデプロイ中または使用中に問題を見つけたら、その原因を究明する必要があります。 リソース マネージャーでは、発生した問題とその原因を 2 通りの方法で発見できます。 
デプロイ コマンドを利用し、特定のデプロイと操作に関する情報を取得できます。 または、およびその他のアクションの展開に関する情報を取得する監査ログを使用することができます。 
ソリューションの有効期間中に取得されます。 このトピックでは、監査ログに重点を置いて説明します。 

監査ログには、リソースで実行されたすべての操作が含まれています。 そのため、組織のユーザーがリソースを変更した場合、そのアクション、時刻、ユーザーを特定できます。

監査ログを使用する際に留意するべき 2 つの重要な制限があります。

1. 監査ログは、90 日間のみ保持されます。
2. 15 日以内の範囲のみ照会することができます。

Azure PowerShell、Azure CLI、REST API、Azure ポータルを利用し、監査ログから情報を取得できます。

## PowerShell

[AZURE.INCLUDE [powershell-preview-inline-include](../includes/powershell-preview-inline-include.md)]

ログ エントリを取得するには、実行、 **Get AzureRmLog** コマンド (または **Get-azureresourcegrouplog** PowerShell version 1.0 より前)。 パラメーターを追加し、エントリの一覧を絞り込むことができます。 

次の例は、監査ログを利用し、ソリューションの使用期間中に行われたアクションを調査する方法を示すものです。 アクションが発生したタイミングとそれを要請したユーザーを確認できます。 開始日と終了日は、日付の形式で指定されます。

    PS C:\> Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00

または、日付関数を使用して、最後の 15 日など、日付の範囲を指定します。

    PS C:\> Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-15)

指定した開始時刻によっては、前のコマンドを実行したとき、そのリソース グループのアクションが長い一覧で返されることがあります。 検索基準を指定すると、探しものの結果を絞り込むことができます。 たとえば場合は、
次のコマンドを実行し、someone@example.com によって停止アクションが実行されたことを確認方法、web アプリが停止されました、調査しようとしています。

    PS C:\> Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-15) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action

    Authorization     :
                        Scope     : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
                        Action    : Microsoft.Web/sites/stop/action
                        Role      : Subscription Admin
                        Condition :
    Caller            : someone@example.com
    CorrelationId     : 84beae59-92aa-4662-a6fc-b6fecc0ff8da
    EventSource       : Administrative
    EventTimestamp    : 8/28/2015 4:08:18 PM
    OperationName     : Microsoft.Web/sites/stop/action
    ResourceGroupName : ExampleGroup
    ResourceId        : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
    Status            : Succeeded
    SubscriptionId    : xxxxx
    SubStatus         : OK

次の例では、指定した開始時刻後に失敗したアクションがわかります。 また、 **DetailedOutput** パラメーター エラー メッセージを表示します。

    PS C:\> Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-15) -Status Failed –DetailedOutput
    
このコマンドでは、多くのエントリとプロパティが返された場合は、取得することによって監査エネルギーを集中できます、 **プロパティ** プロパティです。

    PS C:\> (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties

    Content
    -------
    {}
    {[statusCode, Conflict], [statusMessage, {"Code":"Conflict","Message":"Website with given name mysite already exists...
    {[statusCode, Conflict], [serviceRequestId, ], [statusMessage, {"Code":"Conflict","Message":"Website with given name...

また、ステータス メッセージを見て、結果をさらに絞り込むことができます。

    PS C:\> (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json

    Code       : Conflict
    Message    : Website with given name mysite already exists.
    Target     :
    Details    : {@{Message=Website with given name mysite already exists.}, @{Code=Conflict}, @{ErrorEntity=}}
    Innererror :


## Azure CLI

実行するログ エントリを取得する、 **azure グループ ログ表示** コマンドです。

    azure group log show ExampleGroup

などに、JSON ユーティリティを使用して結果をフィルタ リングすることができます [jq](http://stedolan.github.io/jq/download/)します。 次の例では、Web 構成ファイルの更新を含む操作を探す方法を示しています。

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.localizedValue == \"Update web sites config\")"

追加することができます、 **– - 最後の展開** を最後のデプロイから返されるエントリを操作のみに制限するパラメーターです。

    azure group log show ExampleGroup --last-deployment

最後のデプロイからの操作の一覧が長すぎる場合は、失敗した操作の結果だけに絞り込むことができます。

    azure group log show tfCopyGroup --last-deployment --json | jq ".[] | select(.status.value == \"Failed\")"

                                   /Microsoft.Web/Sites/ExampleSite
    data:    SubscriptionId:       <guid>
    data:    EventTimestamp (UTC): Thu Aug 27 2015 13:03:27 GMT-0700 (Pacific Daylight Time)
    data:    OperationName:        Update website
    data:    OperationId:          cb772193-b52c-4134-9013-673afe6a5604
    data:    Status:               Failed
    data:    SubStatus:            Conflict (HTTP Status Code: 409)
    data:    Caller:               someone@example.com
    data:    CorrelationId:        a8c7a2b4-5678-4b1b-a50a-c17230427b1e
    data:    Description:
    data:    HttpRequest:          clientRequestId: <guid>
                                   clientIpAddress: 000.000.000.000
                                   method:          PUT

    data:    Level:                Error
    data:    ResourceGroup:        ExampleGroup
    data:    ResourceProvider:     Azure Web Sites
    data:    EventSource:          Administrative
    data:    Properties:           statusCode:       Conflict
                                   serviceRequestId:
                                   statusMessage:    {"Code":"Conflict","Message":"Website with given name
                                   ExampleSite already exists.","Target":null,"
                                   Details
                                   ":[{"Message":"Website with given name ExampleSite already exists."},
                                   {"Code":"Conflict"},
                                   {"ErrorEntity":{"Code":"Conflict","Message":"Website with given
                                   name ExampleSite already exists.","ExtendedCode
                                   ":"
                                   54001","MessageTemplate":"Website with given name {0} already exists.",
                                   "Parameters":["ExampleSite"],"
                                   InnerErrors":null}}],"Innererror":null}



## REST API

監査ログを操作するための REST 操作の一部である、 [インサイト REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx)します。 監査ログ イベントを取得するには、次を参照してください。 [サブスクリプションの管理イベントを一覧表示](https://msdn.microsoft.com/library/azure/dn931934.aspx)します。

## ポータル

ポータルでログに記録された操作を表示することもできます。 監査ログのブレードを選択するだけです。

![監査ログの選択](./media/resource-group-audit/select-audit.png) 

また、最新の操作の一覧を表示します。

![アクションの表示](./media/resource-group-audit/show-actions.png)

任意の操作を選択し、その詳細を表示できます。

## 次のステップ

- セキュリティ ポリシーを設定する方法については、次を参照してください。 [リソースへのアクセスを管理する](resource-group-rbac.md)です。
- サービス プリンシパルにアクセスを付与の詳細については、次を参照してください。 [Azure リソース マネージャーでサービス プリンシパルの認証](resource-group-authenticate-service-principal.md)します。
- については、すべてのユーザー リソースに対して操作を参照して [Azure リソース マネージャーによるリソースのロック](resource-group-lock-resources.md)します。

