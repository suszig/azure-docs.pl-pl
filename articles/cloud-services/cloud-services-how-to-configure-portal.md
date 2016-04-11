<properties 
    pageTitle="クラウド サービスを構成する方法 |Microsoft Azure" 
    description="Azure のクラウド サービスの構成方法について説明します。 クラウド サービスの構成の更新方法と、ロール インスタンスへのリモート アクセスの構成方法を紹介します。 これらの例では、Azure ポータルを使用します。" 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/22/2015"
    ms.author="adegeo"/>




# Cloud Services の構成方法

> [AZURE.SELECTOR]
- [Azure クラシック ポータル](cloud-services-how-to-configure.md)
- [Azure ポータル](cloud-services-how-to-configure-portal.md)

クラウド サービスで最もよく使用される設定は Azure ポータルで構成できます。 また、構成ファイルを直接更新する場合は、サービス構成ファイルをダウンロードして内容を更新し、更新したファイルをアップロードして、クラウド サービスの構成を更新します。 どちらの方法でも、構成の更新はすべてのロール インスタンスに適用されます。

クラウド サービスで実行中の 1 つまたはすべてのロールへのリモート デスクトップ接続を有効にすることもできます。  リモート デスクトップを使用すると、アプリケーションの実行中にそのデスクトップにアクセスし、問題のトラブルシューティングや診断を行うことができます。  アプリケーション開発時にリモート デスクトップのサービス定義ファイル (.csdef) を構成しなかった場合でも、ロールへのリモート デスクトップ接続を有効にできます。  リモート デスクトップ接続を有効にするために、アプリケーションを再デプロイする必要はありません。

Azure で構成の更新中に 99.95% の可用性を保証できるのは、各ロールに少なくとも 2 つのロール インスタンスがある場合だけです。 この場合、1 台の仮想マシンでクライアントからの要求を処理し、もう 1 台で更新を行うことができます。 詳細については、次を参照してください。 [サービス レベル アグリーメント](http://azure.microsoft.com/support/legal/sla/)します。

## クラウド サービスの変更

1.  [Azure ポータル](http://portal.azure.com/), 、クラウド サービスに移動します。

2. をクリックして、 **設定** アイコンまたは **Essentials]/[すべて設定** 開くためのリンク、 **設定** ブレードです。

    ![[設定] ページ](./media/cloud-services-how-to-configure-portal/cloud-service.png)
    
    ここから表示することができます、 **プロパティ**, 、変更、 **構成**, 、管理、 **証明書**, 、および管理、 **ユーザー** このクラウド サービスへのアクセス権を持つユーザーです。

2. 下にある、 **監視** セクションの警告を構成するいずれかのタイルをクリックすることができます。 

    ![クラウド サービスの監視](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)
    
3. 下にある、 **ロールとインスタンス** セクションで、クラウド サービスのロール インスタンスを管理します] をクリックすることができます。

    ![クラウド サービス インスタンス](./media/cloud-services-how-to-configure-portal/cs-instance.png)
    
    ここから、クラウド サービスへのリモート接続、クラウド サービスの再起動または再イメージ化を行うことができます。
    
    ![クラウド サービス インスタンスのボタン](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

>[AZURE.NOTE]
>使用して、クラウド サービスで使用されるオペレーティング システムを変更することはできません、 **Azure ポータル**, 、この設定を変更することができます、 [Azure クラシック ポータル](http://manage.windowsazure.com/)します。 詳細については [ここ](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file)します。

## クラウド サービス構成ファイルの更新

1. まず、既存のクラウド サービス構成ファイル (.cscfg) をダウンロードします。

    1.  [Azure ポータル](http://portal.azure.com/), 、クラウド サービスに移動します。

    2. をクリックして、 **設定** アイコンまたは **Essentials]/[すべて設定** 開くためのリンク、 **設定** ブレードです。

        ![[設定] ページ](./media/cloud-services-how-to-configure-portal/cloud-service.png)
    
    3. をクリックして、 **構成** 項目。

        ![[構成] ブレード](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
    
    4. をクリックして、 **ダウンロード** ] ボタンをクリックします。

        ![ダウンロード](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)

2. サービス構成ファイルを更新した後、次のステップでファイルをアップロードして構成の更新内容を適用します。

    1. 開く上記の最初の 3 つの手順に従います、 **構成** 、クラウド サービスのブレードです。
    
    2. をクリックして、 **アップロード** ] ボタンをクリックします。

        ![アップロード](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
    
    3. .Cscfg ファイルを選択し、クリックして **OK**します。

## ロール インスタンスへのリモート アクセスの構成

使用してリモート アクセスを構成することはできません、 **Azure ポータル**, 、この設定を変更することができます、 [Azure クラシック ポータル](http://manage.windowsazure.com/)します。 これについては説明 [ここ](cloud-services-role-enable-remote-desktop.md)します。
            
## 次のステップ

* 学習方法 [クラウド サービス デプロイ](cloud-services-how-to-create-deploy-portal.md)します。
* 構成、 [カスタム ドメイン名](cloud-services-custom-domain-name-portal.md)します。
* [クラウド サービスを管理する](cloud-services-how-to-manage-portal.md)です。
* 構成 [ssl 証明書](cloud-services-configure-ssl-certificate-portal.md)します。
