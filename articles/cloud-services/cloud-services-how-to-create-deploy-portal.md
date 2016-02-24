<properties
    pageTitle="クラウド サービスを作成してデプロイする方法 | Microsoft Azure"
    description="Azure の簡易作成の方法によりクラウド サービスを作成してデプロイする方法について説明します。 これらの例では、Azure ポータルを使用します。"
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
- [Azure クラシック ポータル](cloud-services-how-to-create-deploy.md)
- [Azure ポータル](cloud-services-how-to-create-deploy-portal.md)

Azure ポータルで作成し、クラウド サービスを展開するための 2 つの方法: *簡易作成* と *カスタム作成*します。

簡易作成方法を使用して、新しいクラウド サービスを作成し、使用する方法を説明 **アップロード** をアップロードして Azure にクラウド サービス パッケージを展開します。 この方法を使うと、Azure ポータルに、必要な事項をすべて完了するのに便利なリンクが操作の進行につれて表示されます。 クラウド サービスの作成時にデプロイする準備が整っている場合は、[カスタム作成] を使用して作成とデプロイを同時に実行できます。

> [AZURE.NOTE] Visual Studio Team Services (VSTS) からクラウド サービスを発行する場合、[簡易作成を使用し、Azure クイック スタート] または [ダッシュ ボードから VSTS の発行を設定し、します。 詳細については、次を参照してください。 [Visual Studio Team Services の使用による Azure への継続的な配信][TFSTutorialForCloudService], 、のヘルプを表示したり、 **クイック スタート** ページです。

## 概念
Azure のクラウド サービスとしてアプリケーションをデプロイするには、3 つのコンポーネントが必要です。

- **サービス定義**  
  クラウド サービス定義ファイル (.csdef) は、ロールの数も含めて、サービス モデルを定義します。

- **サービスの構成**  
  クラウド サービス構成ファイル (.cscfg) は、ロール インスタンスの数も含めて、クラウド サービスおよび個々のロールの構成設定を指定します。

- **サービス パッケージ**  
  サービス パッケージ (.cspkg) には、アプリケーション コードと構成、およびサービス定義ファイルが含まれています。

これらとパッケージを作成する方法の詳細を確認できます [ここ](cloud-services-model-and-package.md)します。

## アプリケーションの準備
クラウド サービスをデプロイする前に、アプリケーション コードとクラウド サービス構成ファイル (.cscfg) からクラウド サービス パッケージ (.cspkg) を作成する必要があります。 Azure SDK には、こういった必須のデプロイ ファイルを準備するためのツールが用意されています。 SDK をインストールすることができます、 [Azure のダウンロード](http://azure.microsoft.com/downloads/) ] ページで、アプリケーション コードの開発に使用する言語。

サービス パッケージをエクスポートする前に、以下の 3 つのクラウド サービス機能について特別な構成が必要です。

- データ暗号化のために Secure Sockets Layer (SSL) を使用するクラウド サービスを展開する場合 [アプリケーションを構成する](cloud-services-configure-ssl-certificate-portal.md#modify) SSL 用です。

- ロール インスタンスにリモート デスクトップ接続を構成する場合 [役割を構成する](cloud-services-role-enable-remote-desktop.md) リモート デスクトップ用です。 これは、クラシック ポータルでのみ実行できます。

- クラウド サービスの詳細監視を構成する場合は、クラウド サービスの Azure 診断を有効にします。 *最小監視* (既定の監視レベル) は、ロール インスタンス (仮想マシン) のホスト オペレーティング システムから収集したパフォーマンス カウンターを使用します。 *詳細監視* アプリケーション処理時に発生する問題を詳しく分析を有効にするロール インスタンス内のパフォーマンス データに基づいて追加のメトリックを収集します。 Azure 診断を有効にする方法については、次を参照してください。 [Azure での診断を有効にする](cloud-services-dotnet-diagnostics.md)です。

Web ロールまたはワーカー ロールのデプロイメントを伴うクラウド サービスを作成する必要があります [サービス パッケージを作成](cloud-services-model-and-package.md#servicepackagecspkg)します。

## 開始する前に

- Azure SDK をインストールしていない場合はクリックして **Azure SDK のインストール** を開くには、 [Azure のダウンロード ページ](http://azure.microsoft.com/downloads/), 、およびコードの開発に使用する言語の SDK をダウンロードします。 (これは後でも実行する機会があります)。

- 証明書を必要とするロール インスタンスがある場合は、証明書を作成します。 クラウド サービスには、秘密キーのある .pfx ファイルが必要です。 [Azure に証明書をアップロードする]() を作成してクラウド サービスを展開するとき。

- クラウド サービスをアフィニティ グループにデプロイすることを予定している場合は、アフィニティ グループを作成します。 アフィニティ グループを使用すると、自分のクラウド サービスおよびその他の Azure サービスをリージョンの同じ場所にデプロイすることができます。 アフィニティ グループを作成することができます、 **ネットワーク** 領域は、Azure の従来のポータルで、 **アフィニティ グループ** ページです。


## 手順 3: クラウド サービスを作成し、デプロイ パッケージをアップロードします。

1. [Azure ポータル][] にログインします。
2. クリックして **新規 > コンピューティング**, 、まで下にスクロールし、クリックして **クラウド サービス**します。

    ![クラウド サービスの発行](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)

3. 表示される情報] ページの下部にあるをクリックして **作成**します。 
4. 新しい **クラウド サービス** ブレードで、値を入力、 **DNS 名**します。
5. 新しい **リソース グループ** か、既存のものを選択します。
6. 選択、 **場所**します。
7. 選択 **パッケージ**, 、および、 **パッケージをアップロードします** ブレードで、必要なフィールドに入力します。  

     1 つのインスタンスを含む、ロールのいずれかの場合、以下のことを確認 **1 つまたは複数のロールに単一のインスタンスが含まれる場合でもデプロイ** が選択されています。

8. 確認して **展開を開始** が選択されています。
9. Click **OK**.

    ![クラウド サービスの発行](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## 証明書のアップロード

展開パッケージがあった場合 [証明書を使用するように構成](cloud-services-configure-ssl-certificate-portal.md#modify), 、ここで、証明書をアップロードできます。

1. 選択 **証明書**, 、および、 **証明書を追加** ブレードで、SSL 証明書の .pfx ファイルを選択するうえで、 **パスワード** 、証明書の
2. をクリックして **証明書のアタッチ**, 、順にクリック **OK** 上、 **証明書を追加** ブレードです。
3. をクリックして **作成** 上、 **クラウド サービス** ブレードです。 展開に達した場合、 **準備** になったら、次の手順に進むことができます。

    ![クラウド サービスの発行](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)


## デプロイが正常に完了したことを確認する

1. クラウド サービス インスタンスをクリックします。

    サービスがある状態が [ **を実行している**します。

2.  **Essentials**, をクリックして、 **サイトの URL** web ブラウザーで、クラウド サービスを開きます。

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)


[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## 次のステップ

* [クラウド サービスの一般的な構成](cloud-services-how-to-configure-portal.md)します。
* 構成、 [カスタム ドメイン名](cloud-services-custom-domain-name-portal.md)します。
* [クラウド サービスを管理する](cloud-services-how-to-manage-portal.md)です。
* 構成 [ssl 証明書](cloud-services-configure-ssl-certificate-portal.md)します。
