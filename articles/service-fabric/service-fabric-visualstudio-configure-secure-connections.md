<properties
   pageTitle="Service Fabric クラスターでサポートされているセキュリティ保護された接続を構成する |Microsoft Azure"
   description="Visual Studio を使用して、Azure Service Fabric クラスターでサポートされているセキュリティで保護された接続を構成する方法について説明します。"
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />

<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="10/08/2015"
   ms.author="cawaMS" />

# Visual Studio から Service Fabric クラスターにセキュリティ保護された接続を構成する

アクセス制御ポリシーが構成されている Service Fabric クラスターに Visual Studio を使用して安全にアクセスする方法について説明します。

## クラスターの接続の種類

Azure Service Fabric クラスターでサポートされている接続の 2 種類があります: **安全でない** 接続と **x509 証明書に基づく** 接続をセキュリティで保護します。 (Service Fabric クラスターがオンプレミスでホストされる場合 **Windows** と **Dst** 認証もサポートされています)。クラスターの作成中に、クラスターの接続の種類を構成する必要があります。 作成した接続の種類を後で変更することはできません。

Visual Studio Service Fabric Tools は、発行のためのクラスターへの接続向けにすべての認証の種類をサポートしています。 参照してください [Azure ポータルから Service Fabric クラスターを設定する](service-fabric-cluster-creation-via-portal.md) については、セキュリティで保護された Service Fabric クラスターを設定する方法です。

## 発行プロファイルにクラスター接続を構成する

Visual Studio からの Service Fabric プロジェクトを発行する場合、 **発行** ] ダイアログ ボックスでは、Azure Service Fabric クラスターをクリックして選択できます。、 **選択** ボタンをクリック **接続エンドポイント** セクションです。 Azure アカウントにサインインし、サブスクリプションで利用可能な既存のクラスターを選択できます。

![発行] ダイアログ ボックスにより、Service Fabric の接続を構成するユーザー][publishdialog]

 **Service Fabric クラスターの選択** ] ダイアログ ボックスが自動的にクラスターの接続を検証します。 検証に成功した場合、システムに正しい証明書がインストールされていてクラスターに安全に接続できるか、クラスターがセキュリティ保護されていないことを意味しています。 検証に失敗した場合は、ネットワークに問題が発生しているか、システムが正しく構成されていないためにセキュリティ保護されたクラスターに接続できない可能性があります。

![選択の Service Fabric クラスターのダイアログ ボックスでは、既存の選択やサービス ファブリック クラスター接続を新規作成して接続を構成するユーザー][selectsfcluster]

### セキュリティ保護されたクラスターに接続するには

1.  接続先のクラスターによって信頼されているクライアント証明書のいずれかにアクセスできることを確認します。 証明書は通常、Personal Information Exchange (.pfx) ファイルとして共有されています。 参照してください [Azure ポータルから Service Fabric クラスターを設定する](service-fabric-cluster-creation-via-portal.md) をクライアントへのアクセスの許可について、サーバーを構成するためです。

2.  信頼された証明書をインストールします。 そのためには、.pfx ファイルをダブルクリックするか、PowerShell スクリプト Import-PfxCertificate を使用して証明書をインポートします。 証明書をインストール **\LocalMachine\My** 場所です。 証明書のインポート時、すべての既定設定をそのまま使用して問題ありません。

3.  選択、 **発行]** を開くには、プロジェクトのショートカット メニューの **Azure アプリケーションの発行** ] ダイアログ ボックスとターゲット クラスターを選択します。 自動的に接続が解決され、発行プロファイルにセキュリティ保護された接続パラメーターが保存されます。

4.  [省略可能]: 発行プロファイルを編集して、セキュリティ保護されたクラスターの接続を指定できます。

    発行プロファイルの XML ファイルを手動で編集して証明書の情報を指定するため、証明書ストアの名前、ストアの場所、および証明書の拇印をメモしておいてください。 後で、証明書ストアの名前とストアの場所に対して値の指定が必要になります。 参照してください [方法: 証明書のサムプリントを取得](https://msdn.microsoft.com/library/ms734695(v=vs.110).aspx) の詳細。

     *ClusterConnectionParameters* パラメーターを使用すると、Service Fabric クラスターに接続するときに使用する PowerShell パラメーターを指定します。 Connect-ServiceFabricCluster コマンドレットが受け取るパラメーターをすべて使用できます。 参照してください [Connect-servicefabriccluster](https://msdn.microsoft.com/library/mt125938.aspx) の使用可能なパラメーターの一覧です。

    リモート クラスターに発行する場合は、そのクラスターに適したパラメーターを指定する必要があります。 次は、セキュリティ保護されていないクラスターに接続する場合の例です。

    `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`

    次は、x509 証明書ベースのセキュリティ保護されたクラスターに接続する場合の例です。

    ```
    <ClusterConnectionParameters
    ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000"
    X509Credential="true"
    ServerCertThumbprint="0123456789012345678901234567890123456789"
    FindType="FindByThumbprint"
    FindValue="9876543210987654321098765432109876543210"
    StoreLocation="CurrentUser"
    StoreName="My" />
    ```

5.  アップグレード パラメーターとアプリケーションのパラメーター ファイルの場所などの設定を編集してからアプリケーションを発行、 **Service Fabric アプリケーションの発行** Visual Studio でダイアログ。

## 次のステップ
Service Fabric クラスターにアクセスする方法の詳細については、次を参照してください。 [Service Fabric エクスプ ローラーを使用してクラスターの視覚化](service-fabric-visualizing-your-cluster.md)します。

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
