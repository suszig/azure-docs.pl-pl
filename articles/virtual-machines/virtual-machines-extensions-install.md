<properties
 pageTitle="仮想マシン拡張機能の管理 | Microsoft Azure"
 description="クラシック デプロイ モデルで、Azure 仮想マシンの拡張機能を追加、検索、更新、および削除する方法について説明します。"
 services="virtual-machines"
 documentationCenter=""
 authors="squillace"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>
<tags
 ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="infrastructure-services"
 ms.date="12/08/2015"
 ms.author="rasquill"/>
#仮想マシン拡張機能の管理

Azure の Windows 仮想マシンまたは Linux 仮想マシンのいずれかで VM 拡張機能を検索、追加、変更、削除する方法について説明します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャー モード


##VM 拡張機能の使用

動作、またはその他のプログラムが Azure Vm で動作するかできる機能を azure VM 拡張機能が実装する (たとえば、 **WebDeployForVSDevTest** 拡張機能は、Azure VM で Web 配置のソリューションを Visual Studio を使用) またはその他のいくつかの動作をサポートする VM とのやり取りするための機能を提供する (たとえば、使用できます Powershell、Azure CLI から VM アクセス拡張機能、および REST クライアントをリセットするか、または Azure VM でのリモート アクセス値を変更する)。

>[AZURE.IMPORTANT] サポートされる機能で拡張機能の一覧については、次を参照してください。 [Azure VM 拡張機能と機能](virtual-machines-extensions-features.md)です。 各 VM 拡張機能は特定の機能をサポートするため、拡張機能を使用して実行可能なことと不可能なことはその拡張機能によって異なります。 したがって、VM を変更する前に、使用する VM 拡張機能についてのドキュメントを必ず読んでください。 VM 拡張機能の中には削除がサポートされていないものがあり、また別の拡張機能には VM の動作を根本から変更するように設定できるプロパティがあります。

最も一般的なタスクは次のとおりです。

1.  使用可能な拡張機能の検索

2.  読み込み済みの拡張機能の更新

3.  拡張機能の追加

4.  拡張機能の削除

##使用可能な拡張機能の検索

Azure VM 拡張機能は、(サポートされる機能で拡張機能の一覧については、次を参照してください [Azure VM 拡張機能と機能](virtual-machines-extensions-features.md)。)。以下を使用して、拡張機能とその詳細情報を見つけることができます。

-   PowerShell
-   Azure クロスプラットフォーム インターフェイス (Azure CLI)
-   サービス管理 REST API

いずれか [Azure PowerShell](https://msdn.microsoft.com/library/azure/dn495240.aspx) コマンドレットまたは [サービス管理 REST Api](https://msdn.microsoft.com/library/ee460799.aspx) 利用可能な拡張機能に関する情報を検索します。

###Azure PowerShell

一部の拡張機能には、PowerShell からの構成を行いやすくする固有の PowerShell コマンドレットがあります。また、以下のコマンドレットはすべての VM 拡張機能で動作します。

次のコマンドレットを使用すると、使用可能な拡張機能に関する情報を取得できます。

-   使用する web ロールまたはワーカー ロールのインスタンスは、 [Get-azureserviceavailableextension](https://msdn.microsoft.com/library/azure/dn722498.aspx)
    コマンドレットを実行します。
-   使用する仮想マシンのインスタンスの [Get-azurevmavailableextension](https://msdn.microsoft.com/library/azure/dn722480.aspx) コマンドレットです。

     たとえば、次のコード例は一覧表示する方法を示しています、。
    情報、 **IaaSDiagnostics** PowerShell を使用して拡張します。

        PS C:\PowerShell> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
        VERBOSE: 5:09:01 PM - Begin Operation: Get-AzureVMAvailableExtension
        VERBOSE: 5:09:06 PM - Completed Operation: Get-AzureVMAvailableExtension

        Publisher                   : Microsoft.Azure.Diagnostics
        ExtensionName               : IaaSDiagnostics
        Version                     : 1.2
        Label                       : Microsoft Monitoring Agent Diagnostics
        Description                 : Microsoft Monitoring Agent Extension
        PublicConfigurationSchema   :
        PrivateConfigurationSchema  :
        IsInternalExtension         : False
        SampleConfig                :
        ReplicationCompleted        : True
        Eula                        :
        PrivacyUri                  :
        HomepageUri                 :
        IsJsonExtension             : True
        DisallowMajorVersionUpgrade : False
        SupportedOS                 :
        PublishedDate               :
        CompanyName                 :


###Azure コマンド ライン インターフェイス (Azure CLI)

一部の拡張機能 (Docker VM 拡張機能など) には、構成を行いやすくする固有の Azure CLI コマンドがあります。また、以下のコマンドレットはすべての VM 拡張機能で動作します。

使用することができます、 **azure vm 拡張機能の一覧** 利用可能な拡張機能に関する情報を取得して使用するコマンド、 **–-json** を 1 つまたは複数の拡張機能に関する利用可能なすべての情報を表示します。 拡張機能名を指定しない場合、このコマンドはすべての利用可能な拡張機能についての説明を JSON で返します。

たとえば、次のコード例の情報を一覧表示する方法を示しています。、 **IaaSDiagnostics** Azure CLI を使用して、拡張子 **azure vm 拡張機能リスト** コマンドと使用、 **–-json** を完全な情報を取得するオプションです。


    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



###サービス管理 REST API

以下の REST API を使用すると、利用可能な拡張機能についての情報を取得できます。

-   使用する web ロールまたはワーカー ロールのインスタンスは、 [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) 操作します。 使用可能な拡張のバージョンを一覧表示、使用することができます [List Extension Versions](https://msdn.microsoft.com/library/dn495437.aspx)します。

-   使用する仮想マシンのインスタンスの [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) 操作します。 使用可能な拡張のバージョンを一覧表示、使用することができます [List Resource Extension Versions](https://msdn.microsoft.com/library/dn495440.aspx)します。

##拡張機能の追加、更新、無効化

拡張機能は、インスタンスの作成時に追加するか、実行中のインスタンスに追加することができます。 拡張機能は更新、無効化、または削除できます。 これらのアクションは、Azure PowerShell またはサービス管理 REST API 操作を使用して行うことができます。 一部の拡張機能のインストールと設定にはパラメーターが必須です。 拡張機能ではパブリック パラメーターとプライベート パラメーターがサポートされます。


###Azure PowerShell

Azure PowerShell コマンドレットを使用するのが、最も簡単に拡張機能を追加および更新する方法です。 拡張機能コマンドレットを使用する場合、拡張機能の構成のほとんどが自動で実行されます。 場合によっては、プログラムを使用して拡張機能を追加する必要があります。 これを行う場合、拡張機能の構成を指定する必要があります。

次のコマンドレットを使用すると、拡張機能でパブリック パラメーターとプライベート パラメーターを構成する必要があるかどうかを知ることができます。

-   使用する web ロールまたはワーカー ロールのインスタンスは、 **Get-azureserviceavailableextension** コマンドレットです。

-   使用する仮想マシンのインスタンスの **Get-azurevmavailableextension** コマンドレットです。

###サービス管理 REST API

残りの部分を使用して、使用可能な拡張機能の一覧を取得する場合
Api、拡張機能を構成する方法に関する情報が表示されます。 返される情報では、パラメーター情報がパブリック スキーマとプライベート スキーマによって表される場合があります。 パブリック パラメーターの値は、インスタンスに関するクエリで返されます。 プライベート パラメーターの値は返されません。

次の REST API を使用すると、拡張機能でパブリック パラメーターとプライベート パラメーターを構成する必要があるかどうかを知ることができます。

-   Web ロールまたはワーカー ロールのインスタンス、 **PublicConfigurationSchema** と **PrivateConfigurationSchema** 要素にはからの応答内の情報が含まれて、 [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) 操作します。

-   仮想マシンのインスタンス、 **PublicConfigurationSchema** と **PrivateConfigurationSchema** 要素にはからの応答内の情報が含まれて、 [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) 操作します。

>[AZURE.NOTE]拡張機能では、JSON で定義されている構成も使用できます。 これらの種類の拡張機能の使用する場合のみ、 **SampleConfig** 要素を使用します。

