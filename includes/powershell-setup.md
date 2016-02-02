<properties services="virtual-machines" title="Setting up PowerShell" authors="JoeDavies-MSFT" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm=""
   ms.workload="infrastructure"
   ms.date="05/12/2015"
   ms.author="josephd" />


## PowerShell のセットアップ

Azure PowerShell を使用する前に次の手順を実行します。

### PowerShell のバージョンを確認する

Windows PowerShell を使用するには、Windows PowerShell Version 3.0 か 4.0 が必要です。 Windows PowerShell のバージョンを確認するには、Windows PowerShell コマンド プロンプトで次のコマンドを入力します。

    $PSVersionTable

次のような結果が表示されます。

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2

**PSVersion** の値が 3.0 または 4.0 であることを確認します。 互換性のあるバージョンをインストールするを参照してください。 [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) または [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)します。

また、Azure PowerShell Version 0.8.0 以降も必要です。 インストールした Azure PowerShell のバージョンは、Azure PowerShell コマンド プロンプトで次のコマンドを使用して確認できます。

    Get-Module azure | format-table version

次のような結果が表示されます。

    Version
    -------
    0.8.16.1

手順と最新バージョンへのリンクでは、次を参照してください。 [インストールおよび Azure PowerShell の構成方法](powershell-install-configure.md)します。


### Azure アカウントとサブスクリプションを設定する

アクティブ化する Azure サブスクリプションがない場合、 [MSDN サブスクライバーの特典](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) サインアップ、または、 [無料評価版](http://azure.microsoft.com/pricing/free-trial/)します。

Azure PowerShell コマンド プロンプトを開き、次のコマンドで Azure にログオンします。

    Add-AzureAccount

Azure サブスクリプションが複数ある場合は、次のコマンドで、Azure サブスクリプションの一覧を表示できます。

    Get-AzureSubscription

次の種類の情報が表示されます。

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName : 
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Azure PowerShell コマンド プロンプトで次のコマンドを実行して、現在の Azure サブスクリプションを設定します。置換を含む引用符内のすべての < と > 適切な名前の文字。

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current 

Azure サブスクリプションとアカウントの詳細については、次を参照してください。 [方法: サブスクリプションへの接続](powershell-install-configure.md#Connect)します。





