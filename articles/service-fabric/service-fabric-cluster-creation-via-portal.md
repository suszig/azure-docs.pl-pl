<properties
   pageTitle="Azure ポータルからの Service Fabric クラスターのセットアップ | Microsoft Azure"
   description="Azure ポータルからの Service Fabric クラスターのセットアップ。"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="11/19/2015"
   ms.author="chackdan"/>


# Azure ポータルからの Service Fabric クラスターのセットアップ

このページでは、Service Fabric クラスターのセットアップについて説明します。 サブスクリプションに、このクラスターを構成する IaaS VM をデプロイできるだけのコア数が割り当てられていることが前提となります。


## クラスターの作成

1. Azure ポータルにログオン [http://aka.ms/servicefabricportal](http://aka.ms/servicefabricportal)します。

2. **[新規]** をクリックして、新しいリソース テンプレートを追加します。 検索が呼び出されます下にあるすべての市場でテンプレートの **サービス ファブリック クラスター**
(に移動できる **すべて** 最上位のカテゴリのマーケットプ レースをクリックして > と、「すべて」の下の「ファブリック」を検索し、enter キーを押して自動フィルターが機能しない場合があります、そうすること **enter キーを押して**)  
    ![SearchforServiceFabricClusterTemplate][searchforservicefabricclustertemplate]

3. 一覧から "Service Fabric Cluster" を選択します
4. Service Fabric Cluster ブレードに移動し、**[作成]** をクリックしてクラスターの詳細を入力します。
5. 作成、 **新しいリソース グループ (RG)** - ようにクラスター名と同じでそれらを後で検索することをお勧めします。 配置への変更や、クラスターを削除しようとしているときに特に便利です。

    注 - 既存のリソース グループを使用することもできますが、新しいリソース グループを作成することをお勧めします。 そうすれば、必要のないクラスターの削除がとても簡単です。

     ![CreateRG][createrg]

6. クラスターをデプロイする**サブスクリプション**を選択します (特に、複数のサブスクリプションがある場合)。

7. ドロップダウンから**場所**を選択します (既定の米国西部以外の場所に作成する場合)。

8. **ノードの種類**を構成します。 ノードの種類は、Cloud Services の「ロール」と同等のものと見なすことができます。 VM のサイズ、VM の数、プロパティが定義されています。 1 つのクラスターで複数のノードの種類を使用できます。 唯一の制約は、少なくとも 1 つのノードの種類 (プライマリ、つまりポータルで定義する最初のノード) を 5 VM 以上にすることです。
    1. 必要がある VM サイズ/Pricing 層を選択します。 (既定値は D4 標準的な場合だけでも事足りますクラスターを使用してこのアプリケーションのテスト用、D2 を選択する、またはいずれかの小さい VM のサイズを)
    2. VM の数を選択します。後でノードの種類の VM 数をスケールアップまたはスケールダウンできます。ただし、プライマリつまり最初のノードの種類は 5 VM 以上にする必要があります。
    3. (1 ~ 12 文字でアルファベットと数字のみを含む)、NodeType に名前を選択します。
    4. VM リモート デスクトップ ユーザーの名前とパスワードを選択します。
    5. **複数のノード種類がある場合の考慮事項**。 単一ノード種類のクラスターをデプロイする場合は、次の手順を省略します。

        - 例を使って概念を説明します。 小さい (A2、D2 などのような VM サイズ) を持つ Vm をポートの「フロント エンド」のサービスを開くには、インターネットとは、「バックエンド」サービス コンピューティング (D4 D6 のような VM サイズの大きい Vm 上で集中型の「フロント エンド」のサービスと、「バックエンド」サービスを含むアプリケーションを展開して、配置する場合、D12 など) がインターネットに接続します。
        - 両方のサービスを 1 つのノード種類に配置することもできますが、2 ノード種類のクラスターに配置することをお勧めします。各ノード種類は、プロパティが異なっていてもよく (インターネット接続など)、VM のサイズと数は独立して変更できます。
        - 最初に、5 VM 以上にするノードの種類を定義します。 他のノードの種類は、最少 1 個の VM でかまいません。

    ![CreateNodeType][createnodetype]

9. **アプリケーション ポート** - クラスターにアプリケーションをすぐにデプロイする場合は、そのノード種類 (または作成したノード種類) でアプリケーションに対して開くポートを追加します。 追加できますポート ノードの種類に後で、ロード バランサーに関連付けられた (プローブを追加し、負荷 blanancer ルール、プローブを追加する必要があります) このノード型を変更することで。 今すぐ行うと、ポータルの Automation が必要なプローブとルールを LB に追加するので少し楽です。
    1. アプリケーション パッケージの一部であるサービス マニフェストでアプリケーション ポートを検索できます。 各アプリケーション、サービス マニフェスト開き、アプリケーションが外部と通信する必要があるすべての"input"エンドポイントをメモします。
    2. すべてのポート、"アプリケーションの入力エンドポイント] フィールドにコンマを追加します。 TCP クライアントの接続エンドポイントの 19000、既定で指定する必要はありません。 たとえば、アプリケーションではポート "83" を開く必要があります。 この設定は、アプリケーション パッケージ (でしたである 1 つ以上の servicemanifest.xml) に、servicemanifest.xml で表示されます。

  ほとんどのサンプル アプリケーションはポート 80 と 8081 を使用するので、このクラスターにサンプルをデプロイする場合はそれらを追加します。

  ![ポート][ports]

10. **省略可能: の配置プロパティ**-その他の構成を追加する必要はありません、"NodeTypeName"の既定の配置プロパティがシステムで追加します。 アプリケーションで必要な場合はさらに追加できます。


## セキュリティの構成

現時点では、Service Fabric は X509 証明書によるクラスターの保護のみをサポートします。 このプロセスを開始する前に、KeyVault に証明書をアップロードする必要があります。 参照してください [サービス ファブリック クラスター セキュリティ](service-fabric-cluster-security.md) する方法の詳細についてです。

クラスターのセキュリティ保護はオプションですが、強くお勧めします。 クラスターをセキュリティで保護しない場合は、セキュリティ モードを「なし」に切り替える必要があります。

セキュリティの考慮事項およびに文書化する方法の詳細について [サービス ファブリック クラスター セキュリティ](service-fabric-cluster-security.md)

![SecurityConfigs][securityconfigs]



## オプション: 診断の構成

既定では、問題のトラブルシューティングのためクラスターで診断が有効になります。 診断を無効にする場合:

1. [診断構成] ブレードに移動します。

2. [状態] を [オフ] に変更します。

## オプション: ファブリックの設定

これは、service fabric クラスターの既定の設定を変更できるようにする高度なオプションです。 お勧めする **変わりません** 、既定の設定ことを確認してから、アプリケーションまたはクラスターを含める必要があります。

## クラスター作成の完了

**[概要]** をクリックして、提供した構成を確認するか、またはクラスターのデプロイに使用する ARM テンプレートをダウンロードできます。 必須の設定を指定すると、[作成] ボタンが有効になり、クラスター作成プロセスを開始できます。


[通知] で進行状況を確認できます (画面右側のステータス バーの近くにある「ベル」アイコンをクリック)。 クラスターの作成中に [スタート画面にピン留めする] をクリックした場合、[Service Fabric クラスターのデプロイ] がスタート画面にピン留めされます。

![通知][notifications]

## クラスターの状態の表示

デプロイメントが完了した後、ポータルでクラスターを検査できます。

1. 移動して **参照** - リソースをクリックして **サービス ファブリック クラスター**します。

2. クラスターを探してクリックします。

  ![BrowseCluster][browsecluster]

3. これにより、クラスターのパブリック IP アドレスなどのクラスターに関する詳細が、ダッシュボードに表示されます。 **[クラスター パブリック IP アドレス]** の上にポインターを移動するとクリップボードが開き、クリックしてコピーできます。

  ![ClusterDashboard][clusterdashboard]

  クラスター ダッシュ ボード] ブレードで、ノードのモニターの一部では、正常な vs は異常である Vm の数を示します。 正常性状態に複数の詳細を確認するにはセクションで [Service Fabric の正常性モデルの概要](service-fabric-health-introduction.md) のドキュメント


## クラスターへの接続とアプリケーションのデプロイ

クラスターをセットアップした後は、接続してアプリケーションのデプロイを開始できます。

1. Service Fabric SDK がインストールされているコンピューターでは、Windows PowerShell を起動します。

2. 作成したクラスターがセキュリティで保護されているかどうかに応じて、次の PS コマンド セットのいずれかを実行します。

- オプション 1: **セキュリティ保護されていないクラスターへの接続**。

```powershell
Connect-serviceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 -KeepAliveIntervalInSec 10 
```

次のコマンドを実行してアプリケーションをデプロイします。パスは実際のコンピューターのものに置き換えます。

 - オプション 2: **セキュリティで保護されたクラスターへの接続**

次のコマンドを実行し、"Connect-serviceFabricCluster" PS コマンドの実行に使用するコンピューターに証明書を設定します。

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
            -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
            -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

次の PS を実行してセキュアなクラスターに接続します。 証明書の詳細はポータルで指定したものと同じです。

```powershell
Connect-serviceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
            -KeepAliveIntervalInSec 10 `
            -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
            -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
            -StoreLocation CurrentUser -StoreName My
```
たとえば、上の PS コマンドの結果は次のようになります。

```powershell
Connect-serviceFabricCluster -ConnectionEndpoint sfcluster4doc.westus.cloudapp.azure.com:19000 `
            -KeepAliveIntervalInSec 10 `
            -X509Credential -ServerCertThumbprint C179E609BBF0B227844342535142306F3913D6ED `
            -FindType FindByThumbprint -FindValue C179E609BBF0B227844342535142306F3913D6ED `
            -StoreLocation CurrentUser -StoreName My
```


次のコマンドを実行してアプリケーションをデプロイします。パスは実際のコンピューターのものに置き換えます。 次の例は、単語数サンプル アプリケーションをデプロイします。

前の手順で接続したクラスターにパッケージをコピーします。


```powershell
$applicationPath = "C:\VS2015\WordCount\WordCount\pkg\Debug"
```

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $applicationPath -ApplicationPackagePathInImageStore "WordCount" -ImageStoreConnectionString fabric:ImageStore
```
アプリケーションの種類を Service Fabric に登録します。

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "WordCount"
```

登録したアプリケーションの種類で新しいインスタンスを作成します。

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/WordCount -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0.0
```

選択したブラウザーが開き、アプリケーションがリッスンしているエンドポイントに接続します。 このサンプル アプリケーションの場合、URL は次のようになります。

http://sfcluster4doc.westus.cloudapp.azure.com:31000




## 次のステップ

- [Visual Studio で Service Fabric アプリケーションの管理](service-fabric-manage-application-in-visual-studio.md)します。
- [Service Fabric クラスターのセキュリティ](service-fabric-cluster-security.md)
- [Service Fabric の正常性モデルの概要](service-fabric-health-introduction.md)



[searchforservicefabricclustertemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png 
[createrg]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png 
[createnodetype]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png 
[ports]: ./media/service-fabric-cluster-creation-via-portal/ports.png 
[sfconfigurations]: ./media/service-fabric-cluster-creation-via-portal/SFConfigurations.png 
[securityconfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png 
[notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png 
[browsecluster]: ./media/service-fabric-cluster-creation-via-portal/browse.png 
[clusterdashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png 
[secureconnection]: ./media/service-fabric-cluster-creation-via-portal/SecureConnection.png 

