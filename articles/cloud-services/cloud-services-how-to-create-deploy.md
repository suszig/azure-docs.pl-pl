<properties
    pageTitle="クラウド サービスを作成してデプロイする方法 | Microsoft Azure"
    description="Azure の簡易作成の方法によりクラウド サービスを作成してデプロイする方法について説明します。"
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
    ms.date="12/07/2015"
    ms.author="adegeo"/>





# クラウド サービスを作成してデプロイする方法

> [AZURE.SELECTOR]
- [Azure classic portal](cloud-services-how-to-create-deploy.md)
- [Azure portal](cloud-services-how-to-create-deploy-portal.md)


Azure クラシック ポータルには、クラウド サービスを作成してデプロイする方法として、**[簡易作成]** と **[カスタム作成]** の 2 つの方法が用意されています。

このトピックでは、簡易作成の方法を使って新しいクラウド サービスを作成し、その後、**[アップロード]** を使用して Azure にクラウド サービス パッケージをアップロードしてデプロイする方法について説明します。 この方法を使うと、Azure クラシック ポータルに、必要な事項をすべて完了するのに便利なリンクが操作の進行につれて表示されます。 クラウド サービスの作成時に展開する準備が整っている場合は、**[カスタム作成]** を使用して作成と展開を同時に実行できます。
> [AZURE.NOTE] Visual Studio Team Services (VSTS) からクラウド サービスを発行する予定の場合は、[簡易作成] を使用した後、**[クイック スタート]** またはダッシュボードから VSTS 発行を設定する必要があります。 詳細については、次を参照してください。 [サービスを使用して Visual Studio チーム ][tfstutorialforcloudservice], 、のヘルプを表示したり、 **クイック スタート** ページです。

## 概念

Azure のクラウド サービスとしてアプリケーションをデプロイするには、3 つのコンポーネントが必要です。

- **サービス定義**  
  クラウド サービス定義ファイル (.csdef) は、ロールの数も含めて、サービス モデルを定義します。

- **サービス構成**  
  クラウド サービス構成ファイル (.cscfg) は、ロール インスタンスの数も含めて、クラウド サービスおよび個々のロールの構成設定を指定します。

- **サービス パッケージ**  
  サービス パッケージ (.cspkg) には、アプリケーション コードと構成、およびサービス定義ファイルが含まれています。

これらとパッケージを作成する方法の詳細を確認できます [ここ](cloud-services-model-and-package.md)します。

## アプリケーションの準備

クラウド サービスをデプロイする前に、アプリケーション コードとクラウド サービス構成ファイル (.cscfg) からクラウド サービス パッケージ (.cspkg) を作成する必要があります。 Azure SDK には、こういった必須のデプロイ ファイルを準備するためのツールが用意されています。 SDK をインストールすることができます、 [Azure のダウンロード](http://azure.microsoft.com/downloads/) ] ページで、アプリケーション コードの開発に使用する言語。

サービス パッケージをエクスポートする前に、以下の 3 つのクラウド サービス機能について特別な構成が必要です。

- データ暗号化のために Secure Sockets Layer (SSL) を使用するクラウド サービスを展開する場合 [アプリケーションを構成する](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) SSL 用です。

- ロール インスタンスにリモート デスクトップ接続を構成する場合 [役割を構成する](cloud-services-role-enable-remote-desktop.md) リモート デスクトップ用です。

- クラウド サービスの詳細監視を構成する場合は、クラウド サービスの Azure 診断を有効にします。 *最小監視* (既定の監視レベル) では、ロール インスタンス (仮想マシン) のホスト オペレーティング システムから収集したパフォーマンス カウンターが使用されます。 詳細監視は、ロール インスタンス内のパフォーマンス データに基づいて追加のメトリックを収集して、アプリケーション処理時に発生する問題を詳しく分析できます。 Azure 診断を有効にする方法については、次を参照してください。 [Azure で診断を有効にすると](cloud-services-dotnet-diagnostics.md)します。

Web ロールまたはワーカー ロールのデプロイメントを伴うクラウド サービスを作成する必要があります [サービス パッケージを作成](cloud-services-model-and-package.md#servicepackagecspkg)します。

## 開始する前に

- Azure SDK をインストールしていない場合はクリックして **Azure SDK のインストール** を開くには、 [Azure のダウンロード ページ](http://azure.microsoft.com/downloads/), 、およびコードの開発に使用する言語の SDK をダウンロードします。 (これは後でも実行する機会があります)。

- 証明書を必要とするロール インスタンスがある場合は、証明書を作成します。 クラウド サービスには、秘密キーのある .pfx ファイルが必要です。 実行できます [証明書を Azure にアップロード](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) を作成してクラウド サービスを展開するとき。

- クラウド サービスをアフィニティ グループにデプロイすることを予定している場合は、アフィニティ グループを作成します。 アフィニティ グループを使用すると、自分のクラウド サービスおよびその他の Azure サービスをリージョンの同じ場所にデプロイすることができます。 アフィニティ グループは、Azure クラシック ポータルの **[Networks]** 領域にある **[アフィニティ グループ]** ページで作成できます。


## 方法: 簡易作成によるクラウド サービスの作成

1. [Azure クラシック ポータル](http://manage.windowsazure.com/), 、クリックして **新規**>**コンピューティング**>**クラウド サービス**>**簡易作成**します。

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)

2. **[URL]** ボックスに、運用デプロイのクラウド サービスにアクセスするパブリック URL で使用するサブドメイン名を入力します。 運用デプロイの URL 形式: http://*myURL*. cloudapp.net します。

3. **[リージョン/アフィニティ グループ]** で、クラウド サービスをデプロイするリージョンまたはアフィニティ グループを選択します。 リージョン内の他の Azure サービスと同じ場所にクラウド サービスをデプロイする場合は、アフィニティ グループを選択します。

4. **[クラウド サービスを作成する]** をクリックします。

    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)

    処理の状態はウィンドウの下部にあるメッセージ領域で監視できます。

    **[Cloud Services]** 領域が開き、新しいクラウド サービスが表示されます。 状態が [作成済み] に変わると、クラウド サービスの作成が正常に完了しています。

    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)


## 方法: クラウド サービスの証明書のアップロード

1. [Azure クラシック ポータル](http://manage.windowsazure.com/), 、クリックして **クラウド サービス**, をクラウド サービスの名前をクリックして、クリックして **証明書**します。

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)

2. **[証明書をアップロードします]** または **[アップロード]** をクリックします。

3. **[ファイル]** の **[参照]** を使用して、証明書ファイル (.pfx) を選択します。

4. **[パスワード]** ボックスに、証明書の秘密キーを入力します。

5. **[OK]** (チェックマーク) をクリックします。

    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)

    アップロードの進捗状況は下図のメッセージ領域で監視できます。 アップロードが完了すると、証明書がテーブルに追加されます。 メッセージ領域で [OK] をクリックし、メッセージを閉じます。

    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## 方法: クラウド サービスのデプロイ

1. [Azure クラシック ポータル](http://manage.windowsazure.com/), 、クリックして **クラウド サービス**, をクラウド サービスの名前をクリックして、クリックして **ダッシュ ボード**します。

2. **[新しい運用環境のデプロイをアップロードします]** または **[アップロード]** をクリックします。

3. **[デプロイ ラベル]** に、新しいデプロイの名前を入力します (たとえば、MyCloudServicev4)。

3. **[パッケージ]** で、**[参照]** を使って、使用するサービス パッケージ ファイル (.cspkg) を選択します。

4. **[構成]** で、**[参照]** を使って、使用するサービス構成ファイル (.cscfg) を選択します。

5. クラウド サービスにインスタンスが 1 つしかないロールがある場合は、**[1 つ以上のロールに単一のインスタンスが含まれている場合でもデプロイする]** チェック ボックスをオンにして、デプロイを有効にして続行します。

    Azure でクラウド サービスの保守中およびサービス更新中に 99.95% のアクセスを保証できるのは、あらゆるロールに少なくとも 2 つのインスタンスがある場合だけです。 必要な場合、クラウド サービスをデプロイした後に、**[スケール]** ページで、ロール インスタンスを追加できます。 詳細については、次を参照してください。 [サービス レベル アグリーメント](http://azure.microsoft.com/support/legal/sla/)します。

6. **[OK]** (チェックマーク) をクリックして、クラウド サービスのデプロイを開始します。

    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)

    デプロイの状態は、メッセージ領域で監視できます。 メッセージを非表示にするには、[OK] をクリックします。

    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## デプロイが正常に完了したことを確認する

1. **[ダッシュボード]** をクリックします。

    サービスのステータスが、**実行中**になっていることを確認します。

2. **[概要]** で、サイト URL をクリックして、Web ブラウザーでクラウド サービスを開きます。

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


## 次のステップ

* [、クラウド サービスの一般的な構成](cloud-services-how-to-configure.md)します。
* 構成、 [カスタム ドメイン名](cloud-services-custom-domain-name.md)します。
* [クラウド サービスを管理する](cloud-services-how-to-manage.md)します。
* 構成 [ssl 証明書](cloud-services-configure-ssl-certificate.md)します。



[tfstutorialforcloudservice]: http://go.microsoft.com/fwlink/?LinkID=251796 

