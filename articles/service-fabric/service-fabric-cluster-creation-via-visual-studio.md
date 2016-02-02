<properties
   pageTitle="Visual Studio を使用した Service Fabric クラスターのセットアップ | Microsoft Azure"
   description="Visual Studio の Azure リソース グループ プロジェクトで作成した Azure リソース マネージャー (ARM) テンプレートを使用して Service Fabric クラスターをセットアップする方法について説明します"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/13/2015"
   ms.author="karolz@microsoft.com"/>


# Visual Studio による Service Fabric クラスターのセットアップ

設定する方法を説明する、 [* * サービス ファブリック * *](http://azure.microsoft.com/documentation/services/service-fabric/) を使用した **Visual Studio** と [* * Azure リソース マネージャー (ARM) * *](https://azure.microsoft.com/documentation/articles/resource-group-overview/) テンプレートです。 テンプレートを作成するために、Visual Studio **Azure リソース グループ プロジェクト**を使用します。 作成したテンプレートは、Visual Studio から Azure に直接デプロイできますが、スクリプトから使用したり、**継続的インテグレーション (CI)** 機能の一部として使用したりすることもできます。

## Azure リソース グループ プロジェクトで Service Fabric クラスター テンプレートを作成する

まず、Visual Studio を開き、Azure リソース グループ プロジェクトを作成します ([クラウド] フォルダーの下にあります)。

![[Azure リソース グループ] プロジェクトが選択されている ][1]

このプロジェクト用の新しい Visual Studio ソリューションを作成することも、既存のソリューションに追加することもできます。
>[AZURE.NOTE] [クラウド] ノードの下に [Azure リソース グループ] プロジェクトがない場合は、Azure SDK がインストールされていません。 Web Platform Installer を起動 ([ここからインストール](http://www.microsoft.com/web/downloads/platform.aspx) まだ行っていない場合)、"Azure SDK の .NET"を検索、および、Visual Studio のバージョンと互換性があるバージョンをインストールします。

[OK] ボタンをクリックすると、作成する ARM テンプレートの選択を求めるメッセージが表示されます。

![Service Fabric クラスター テンプレートが選択されている ][2]

[Service Fabric クラスター] テンプレートを選択し、もう一度 [OK] ボタンをクリックします。 これで、プロジェクトと ARM テンプレートが作成されます。

## デプロイメント用のテンプレートを準備する

テンプレートをデプロイしてクラスターを作成する前に、必要なテンプレート パラメーターの値を指定する必要があります。 これらのパラメーター値から読み込まれて `ServiceFabricCluster.param.dev.json` 下にあるファイル `テンプレート` リソース グループ プロジェクトのフォルダーです。 ファイルを開き、次の値を指定します。

| パラメーター名| 説明|
|-----------------------  |--------------------------|
| clusterLocation| Service Fabric クラスターを配置する **Azure リージョン**の名前。たとえば、"East US" などとします。|
| clusterName| テンプレートによって作成される Service Fabric クラスターの DNS 名。<br /><br /> 'MyBigCluster' に、このパラメーターを設定する場合など、 `clusterLocation` パラメーターは、米国東部に設定されて、クラスターの名前になります `myBigCluster.eastus.cloudapp.azure.com`します。|
| certificateThumbprint| クラスターをセキュリティで保護する証明書の拇印。|
| sourceVaultValue| クラスターをセキュリティで保護する証明書が格納されている Key Vault の*リソース ID*。|
| certificateUrlValue| クラスターのセキュリティ証明書の URL。|

Visual Studio Service Fabric ARM テンプレートは、証明書によって保護されている、安全なクラスターを作成します。 この証明書が最後の 3 つのテンプレート パラメーターで識別される (`certificateThumbprint`, 、`sourceVaultValue` と `certificateUrlValue`) に存在する必要があり、 **Azure Key Vault**します。 クラスターのセキュリティ証明書を作成する方法の詳細については、参照 [証明書を使用して Service Fabric クラスターを保護する方法](service-fabric-cluster-security.md) 記事です。

## 省略可能: パブリック アプリケーション ポートを追加する

テンプレートで、デプロイ前に変更した方がよい場合があるもう 1 つの部分は、クラスターの**パブリック アプリケーション ポート**です。 既定では、テンプレートは 2 つのパブリック TCP ポート (80 および 8081) を開きます。アプリケーションでより多くのポートが必要な場合は、テンプレート内の Azure ロード バランサー定義を変更する必要があります。 メイン テンプレート ファイルに定義が格納されます (`SecureFabricCluster.json`)。 そのファイルを開き、"loadBalancedAppPort" を検索します。 各ポートが、次の 3 つのアーティファクトに関連付けられていることがわかります。

1. ポートの TCP ポートの値を定義するテンプレート パラメーター。
    ```json
    "loadBalancedAppPort1": {
        "type": "int",
        "defaultValue": 80
    }
    ```

2. Azure ロード バランサーが特定の Service Fabric ノードの使用を試行する頻度と期間を定義する*プローブ*。この期間を過ぎると、別のノードにフェールオーバーします。 プローブは、ロード バランサー リソースの一部です。 最初の既定のアプリケーション ポートのプローブの定義を次に示します。
    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[parameters('loadBalancedAppPort1')]",
            "protocol": "tcp"
        }
    }
    ```

3. A *負荷分散規則* を結び付けると、ポート、プローブと、有効に負荷が一連の Service Fabric のクラスター ノード間で分散します。
    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[parameters('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[parameters('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "tcp"
        }
    }
    ```
クラスターに展開する予定のアプリケーションに複数のポートが必要がある場合は、プローブを追加し、負荷分散の規則の定義を作成することで追加する必要があります。 ARM テンプレートによって Azure ロード バランサーをしてください。 使用する方法の詳細については「 [Azure リソース マネージャーを使用して内部ロード バランサーの構成の開始](https://azure.microsoft.com/documentation/articles/load-balancer-internal-arm-powershell/) 記事です。

## Visual Studio を使用してテンプレートをデプロイする

内のすべての必要なパラメーター値を保存すると、`ServiceFabricCluster.param.dev.json` ファイル、テンプレートを展開し、Service Fabric クラスターを作成する準備ができました。 Visual Studio ソリューション エクスプローラーでリソース グループ プロジェクトを右クリックし、[配置] をクリックします。 必要であれば、Azure に対する認証を求める [リソース グループに配置する] ダイアログ ボックスが表示されます。

![[リソース グループに配置する] ダイアログ ボックス][3]

このダイアログ ボックスでは、クラスターの既存の Azure RM リソース グループを選択したり、新しいグループを作成したりすることができます。 通常は、Service Fabric クラスター用に別のリソース グループを使用することには意味があります。

[配置] ボタンをクリックすると、テンプレート パラメーター値の確認を求めるメッセージが表示されます。 [保存] ボタンをクリックします。 永続的な値を持たないパラメーターが 1 つあります。それは、クラスターの管理者アカウント パスワードです。パスワードの入力を求めるメッセージが表示されたときに、パスワード値を指定してください。
>[AZURE.NOTE] PowerShell が現在使用しているコンピューターからの Azure の管理に使用されなかった場合は、少し定型業務を行う必要があります。
>1.PowerShell のスクリプトを実行して有効にする [`Set-executionpolicy`] (https://technet.microsoft.com/library/hh849812.aspx) コマンドです。 開発用コンピューターでは、通常、"Unrestricted" ポリシーでかまいません。
>2.Azure PowerShell コマンドからの診断データ収集を許可し、実行するかどうかを決定 [`有効にする AzureRmDataCollection`] (https://msdn.microsoft.com/library/mt619303.aspx) または [`無効 AzureRmDataCollection`] (https://msdn.microsoft.com/library/mt619236.aspx) に応じて。 Azure PowerShell version 0.9.8 を使用している場合またはそれ以前、これらのコマンドの名前が付けられます `AzureDataCollection 有効にする` と `Discable AzureDataCollection`, 、それぞれします。 こうすることで、テンプレートのデプロイメント中に不要なメッセージが表示されないようにすることができます。

Visual Studio の出力ウィンドウで、デプロイメント プロセスの進行状況を監視することができます。 テンプレートのデプロイメントが完了したら、新しいクラスターを使用できます。

すべてのエラーがある場合は、 [Azure ポータル](https://portal.azure.com/) し通知を確認します。 リソース グループのデプロイメントが失敗すると、詳細な診断情報がここに表示されます。

## 次のステップ

- [Azure ポータルを使用して Service Fabric クラスターの設定についてください。](service-fabric-cluster-creation-via-portal.md)
- [ホットについて説明すると、Visual Studio を使用して Service Fabric アプリケーションの展開を管理するには](service-fabric-manage-applications-in-visual-studio.md)



[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png 
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png 
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png 

