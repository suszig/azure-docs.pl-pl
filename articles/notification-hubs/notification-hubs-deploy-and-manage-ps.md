<properties 
    pageTitle="PowerShell を使用した Notification Hubs のデプロイと管理" 
    description="PowerShell を使用して Notification Hubs の作成と管理を自動化する方法" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="wesmc7777" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/10/2015" 
    ms.author="wesmc"/>


# PowerShell を使用した Notification Hubs のデプロイと管理

## 概要

この記事では PowerShell を使用して Azure Notification Hubs を作成および管理する方法を説明します。 このトピックでは、次の一般的なオートメーション タスクが表示されます。

+ 通知ハブの作成
+ 資格情報の設定

通知ハブ用には、新しい service bus 名前空間を作成する必要がある場合は、次を参照してください。 [PowerShell で Service Bus の管理](../service-bus/service-bus-powershell-how-to-provision.md)します。

Notification Hubs は Azure PowerShell に含まれているコマンドレットを使用して直接管理することはできません。 PowerShell からの最良のアプローチは、Microsoft.ServiceBus.dll アセンブリを参照することです。 アセンブリが付属、 [Microsoft Azure 通知ハブの NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)します。


## 前提条件

この記事を読み始める前に、次の項目を用意する必要があります。

- Azure サブスクリプション。 Azure はサブスクリプション方式のプラットフォームです。 サブスクリプションの
サブスクリプションを取得する方法については、[購入のオプション] を参照してください。
[メンバーの提供物]、[無料評価版] またはです。

- Azure PowerShell を搭載するコンピューター 手順については、次を参照してください。 [をインストールし、Azure PowerShell を構成する]。

- PowerShell スクリプト、NuGet パッケージ、.NET Framework の一般的理解。


## Service Bus 用の .NET アセンブリへの参照を含む

Azure Notification Hubs の管理はまだ Azure PowerShell の PowerShell コマンドレットに含まれていません。 用意された .NET クライアントを使用する通知ハブをプロビジョニング、 [Microsoft Azure 通知ハブの NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)します。

まず、スクリプトが Visual Studio プロジェクトの NuGet パッケージとしてインストールされる **Microsoft.ServiceBus.dll** アセンブリを検出できることを確認します。 柔軟性を持たせるために、スクリプトでは次のステップを実行します。

1. 呼び出されたパスを決定します。
2. という名前のフォルダーが見つかるまでパスを走査 `パッケージ`します。 このフォルダーは Visual Studio プロジェクトで NuGet パッケージをインストールする際に作成されます。
3. 反復的に検索、 `パッケージ` という名前のアセンブリ用のフォルダー **Microsoft.Azure.NotificationHubs.dll**します。
4. アセンブリを参照するので、タイプは後で利用できるようになります。

次は、こうした手順を PowerShell スクリプトで実装する方法を示しています。

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## NamespaceManager クラスの作成

通知ハブをプロビジョニングするには、インスタンスを作成、 [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) SDK からのクラスです。

Azure PowerShell に含まれている [Get-azuresbauthorizationrule] コマンドレットを使用すると、接続文字列を指定するのにために使用する承認規則を取得します。 参照を格納しています、 `NamespaceManager` インスタンス、 `$NamespaceManager` 変数です。 使用して `$NamespaceManager` 通知ハブをプロビジョニングします。

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## 新しい通知ハブのプロビジョニング

新しい通知ハブをプロビジョニングするには [.NET API を通知ハブ] を使用します。

スクリプトのこの部分では 4 つのローカル変数を設定します。

1. `$Namespace` : 通知ハブを作成する名前空間の名前を設定します。
2. `$Path` : このパスは新しい通知ハブの名前に設定します。 たとえば、「Myhub」のように設定します。
3. `$WnsPackageSid` : から Windows アプリケーションのパッケージ SID に設定、 [Windows デベロッパー センター](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409)します。
4. `$WnsSecretkey`: 秘密キーにアプリケーションの設定を Windows から、 [Windows デベロッパー センター](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409)します。

これらの変数は、名前空間に接続し、Windows アプリの Windows 通知サービス (WNS) 資格情報を使用して WNS 通知を処理するように構成された新しい通知ハブを作成するために使用します。 パッケージを取得する方法についての SID と秘密キーは、「、 [Notification Hubs の概要](notification-hubs-windows-store-dotnet-get-started.md) チュートリアルです。

+ スクリプト スニペットを使用して、 `NamespaceManager` オブジェクトがないか確認して、通知ハブがで識別されるかどうかに `$Path` が存在します。

+ スクリプトにより作成されますが、存在しない場合、 `NotificationHubDescription` を WNS に資格情報およびに渡さ、 `NamespaceManager` クラス `CreateNotificationHub` メソッドです。

``` powershell

$Namespace = "<Enter your namespace>
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## その他のリソース

- [PowerShell で Service Bus を管理します。](../service-bus/service-bus-powershell-how-to-provision.md)
- [サービス バス キュー、トピック、および PowerShell スクリプトを使用してサブスクリプションを作成する方法](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Service Bus 名前空間と PowerShell スクリプトを使用して Event Hub を作成する方法](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

既製のスクリプトも次のページからダウンロードできます。
- [Service Bus PowerShell スクリプト](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)



[purchase options]: http://azure.microsoft.com/pricing/purchase-options/ 
[member offers]: http://azure.microsoft.com/pricing/member-offers/ 
[free trial]: http://azure.microsoft.com/pricing/free-trial/ 
[install and configure azure powershell]: ../install-configure-powershell.md 
[.net api for notification hubs]: https://msdn.microsoft.com/library/azure/mt414893.aspx 
[get-azuresbnamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx 
[new-azuresbnamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx 
[get-azuresbauthorizationrule]: https://msdn.microsoft.com/library/azure/dn495113.aspx 

