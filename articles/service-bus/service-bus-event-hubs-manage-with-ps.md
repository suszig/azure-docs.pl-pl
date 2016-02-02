<properties
   pageTitle="PowerShell で Service Bus と Event Hubs リソースを管理する"
   description="PowerShell で Service Bus と Event Hubs リソースを作成し、管理する"
   services="service-bus"
   documentationCenter=".NET"
   authors="sethmanheim"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-bus"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/09/2015"
   ms.author="sethm"/>


# PowerShell で Service Bus と Event Hubs リソースを管理する

Microsoft Azure PowerShell は、Azure サービスのデプロイメントと管理を制御し自動化するために使用できるスクリプティング環境です。 この記事では、PowerShell を使用して、名前空間、キュー、Event Hubs などの Service Bus エンティティをローカルの Azure PowerShell コンソールを使用し、プロビジョニングして管理する方法を説明します。

## 前提条件

開始する前に、次の条件が必要です。

- Azure サブスクリプション。 Azure はサブスクリプション方式のプラットフォームです。 サブスクリプションの
、サブスクリプションの入手方法に関する情報を参照してください [購入に関するオプションを][], 、[メンバー プランの][], 、または [無料評価版の][]します。

- Azure PowerShell を搭載するコンピューター 手順については、次を参照してください。 [をインストールし、Azure PowerShell の [] を構成][]します。

- PowerShell スクリプト、NuGet パッケージ、.NET Framework の一般的理解。

## Service Bus 用の .NET アセンブリへの参照を含む

Service Bus の管理で利用できる PowerShell コマンドレットの数は限定されています。 既存のコマンドレットを通じて公開されていないエンティティをプロビジョニングするには、[Service Bus NuGet パッケージ] を参照することで、PowerShell 内から Service Bus の .NET クライアントを使用できます。

まず、このスクリプトが NuGet パッケージでインストールされる **Microsoft.ServiceBus.dll** アセンブリを見つけることができるかどうかを確認します。 柔軟性を持たせるために、スクリプトでは次のステップを実行します。

1. 呼び出されたパスを指定します。
2. という名前のフォルダーが見つかるまでパスを走査 `パッケージ`します。 このフォルダーは NuGet パッケージをインストールする際に作成されます。
3. 反復的に検索、 `パッケージ` という名前のアセンブリ用のフォルダー **Microsoft.ServiceBus.dll**します。
4. アセンブリを参照するので、タイプは後で利用できるようになります。

次は、こうした手順を PowerShell スクリプトで実装する方法を示しています。

```powershell

try
{
    # IMPORTANT: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## Service Bus 名前空間のプロビジョニング

.NET SDK の代わりに使用できる 2 つのコマンドレットは Service Bus 名前空間を使用する場合: [Get-azuresbnamespace] と [New-azuresbnamespace] です。

この例では、いくつかのローカル変数、スクリプトの作成します。 `$Namespace` と `$Location`します。

- `$Namespace` 名前を指定します、作業対象となる Service Bus 名前空間のです。
- `$Location` をデータ センターを識別する名前空間をプロビジョニングします。
- `$CurrentNamespace` では取得 (または作成) する参照名前空間を格納します。

実際のスクリプトで `$Namespace` と `$Location` パラメーターとして渡すことができます。

スクリプトのこの部分では、次を行います。

1. 指定される Service Bus の名前空間を取得しようとします。
2. 名前空間が見つかると、記述されているものを報告します。
3. 名前空間が見つからない場合、名前空間を作成し、新しく作成した名前空間を取得します。

    ``` powershell
    $Namespace = "MyServiceBusNS"
    $Location = "West US"

    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Output "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Output "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```
その他のサービス バス エンティティをプロビジョニングするには、インスタンスを作成、 `NamespaceManager` SDK からのオブジェクト。 [Get-azuresbauthorizationrule] コマンドレットを使用すると、接続文字列を指定するのにために使用する承認規則を取得します。 この例への参照を格納する、 `NamespaceManager` インスタンス、 `$NamespaceManager` 変数です。 スクリプトを使用して後で `$NamespaceManager` を他のエンティティをプロビジョニングします。

    ``` powershell
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    # Create the NamespaceManager object to create the Event Hub
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
    ```

## その他の Service Bus エンティティのプロビジョニング

キュー、トピック、Event Hubs などの他のエンティティをプロビジョニングするには、[.NET API for Service Bus] を使用することができます。 その他のエンティティを含むより詳細な例は、この記事の最後をご覧ください。

### Event Hub を作成する

スクリプトのこの部分ではさらに 4 つのローカル変数を作成します。 これらの変数がインスタンス化に使用されて、 `EventHubDescription` オブジェクトです。 スクリプトでは次を実行します。

1. 使用して、 `NamespaceManager` オブジェクト、Event Hub がにより識別されるかどうかにチェック `$Path` が存在します。
2. 存在しない場合は、作成、 `EventHubDescription` に渡すと、 `NamespaceManager` クラス `CreateEventHubIfNotExists` メソッドです。
3. 使いコンシューマー グループを作成、Event Hub が利用できることを決定した後 `ConsumerGroupDescription` と `NamespaceManager`します。

    ``` powershell
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"

    # Check if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Output "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription);
        Write-Output "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }

    # Create the consumer group if it doesn't exist
    Write-Output "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription);
    Write-Output "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

### キューを作成する

キューまたはトピックを作成するには、前のセクションと同様に、名前空間の確認を実行します。

    if ($NamespaceManager.QueueExists($Path))
    {
        Write-Output "The [$Path] queue already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] queue in the [$Namespace] namespace..."
        $QueueDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.QueueDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $QueueDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $QueueDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $QueueDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $QueueDescription.EnableBatchedOperations = $EnableBatchedOperations
        $QueueDescription.EnableDeadLetteringOnMessageExpiration = $EnableDeadLetteringOnMessageExpiration
        $QueueDescription.EnableExpress = $EnableExpress
        $QueueDescription.EnablePartitioning = $EnablePartitioning
        $QueueDescription.ForwardDeadLetteredMessagesTo = $ForwardDeadLetteredMessagesTo
        $QueueDescription.ForwardTo = $ForwardTo
        $QueueDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        if ($LockDuration -gt 0)
        {
            $QueueDescription.LockDuration = [System.TimeSpan]::FromSeconds($LockDuration)
        }
        $QueueDescription.MaxDeliveryCount = $MaxDeliveryCount
        $QueueDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $QueueDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        $QueueDescription.RequiresSession = $RequiresSession
        if ($EnablePartitioning)
        {
            $QueueDescription.SupportOrdering = $False
        }
        else
        {
            $QueueDescription.SupportOrdering = $SupportOrdering
        }
        $QueueDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateQueue($QueueDescription);
    }

### トピックを作成する

    if ($NamespaceManager.TopicExists($Path))
    {
        Write-Output "The [$Path] topic already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] topic in the [$Namespace] namespace..."
        $TopicDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.TopicDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $TopicDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $TopicDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $TopicDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $TopicDescription.EnableBatchedOperations = $EnableBatchedOperations
        $TopicDescription.EnableExpress = $EnableExpress
        $TopicDescription.EnableFilteringMessagesBeforePublishing = $EnableFilteringMessagesBeforePublishing
        $TopicDescription.EnablePartitioning = $EnablePartitioning
        $TopicDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        $TopicDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $TopicDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        if ($EnablePartitioning)
        {
            $TopicDescription.SupportOrdering = $False
        }
        else
        {
            $TopicDescription.SupportOrdering = $SupportOrdering
        }
        $TopicDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateTopic($TopicDescription);
    }

## 次のステップ

この記事では、PowerShell を使用した Service Bus エンティティのプロビジョニングにおける基本的なアウトラインを説明しました。 Service Bus のメッセージング エンティティを管理するために使用できる PowerShell コマンドレットの数に制限がありますが、Microsoft.ServiceBus.dll アセンブリを参照することで .NET クライアント ライブラリを使用して実行できるほぼすべての操作が、PowerShell スクリプトでも実行できます。

次のブログの投稿に詳しい例が掲載されています。

- [サービス バス キュー、トピック、および PowerShell スクリプトを使用してサブスクリプションを作成する方法](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Service Bus 名前空間と PowerShell スクリプトを使用して Event Hub を作成する方法](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

既製のスクリプトも次のページからダウンロードできます。

- [Service Bus PowerShell スクリプト](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)




[purchase options]: http://azure.microsoft.com/pricing/purchase-options/ 
[member offers]: http://azure.microsoft.com/pricing/member-offers/ 
[free trial]: http://azure.microsoft.com/pricing/free-trial/ 
[service bus nuget package]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/ 
[get-azuresbnamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx 
[new-azuresbnamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx 
[get-azuresbauthorizationrule]: https://msdn.microsoft.com/library/azure/dn495113.aspx 
[.net api for service bus]: https://msdn.microsoft.com/en-us/library/azure/mt419900.aspx 
[install and configure azure powershell]: ../install-configure-powershell.md 

