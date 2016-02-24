<properties 
    pageTitle="Azure RemoteApp とは | Microsoft Azure" 
    description="Azure RemoteApp を使用してアプリとリソースを任意のデバイスと共有する方法について説明します。" 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="11/04/2015" 
    ms.author="elizapo"/>

# Azure RemoteApp とは

Azure RemoteApp は、Azure をサポートするリモート デスクトップ サービスによってオンプレミスの Microsoft の RemoteApp プログラムの機能を提供します。 Azure RemoteApp は、多くのさまざまなユーザーのデバイスのアプリケーションにセキュリティで保護されたリモート アクセスを提供します。 Azure RemoteApp では基本的に、非永続的なターミナル サーバー セッションがクラウドでホストされ、それらを使用したり、他のユーザーと共有したりできます。

Azure RemoteApp では、ほぼすべてのデバイスでアプリとリソースを共有できます。 アプリはクラウドでホストされます。つまり、管理者は、ユーザーの需要を満たすためにハードウェアとスケーリングに対処する必要はありません。 管理者が実行する必要があるのは共有するアプリのアップロードだけであり、後はユーザーにそれらのアプリを使用してもらうだけです。 [ユーザーが自分のデバイスを保持する取得](remoteapp-clients.md), 、Azure ポータルですべてを管理している間、します。 さらに、会社の資格情報を使用することを選択して、アプリとデータのセキュリティを確保することもできます。

Azure RemoteApp の詳細については続きを読むか、おが既に説明に納得してしまう場合 [今すぐお試し](http://azure.microsoft.com/services/remoteapp/)します。

Azure の RemoteApp について質問はありますか。 チェック アウト、 [FAQ](remoteapp-faq.md)します。

Azure RemoteApp の一部である、 [Microsoft Virtual Desktop Infrastructure](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx)します。

**新しい!**Azure RemoteApp について詳細を知りたいですか。 または、Azure RemoteApp を規模を拡大して検証する準備ができていますか。 週単位に参加 [専門家にたずねるウェビナー](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website)します。

## Azure RemoteApp コレクション
2 種類の [Azure RemoteApp コレクション](remoteapp-collections.md):


- A **クラウド コレクション** でホストされ、クラウド内のプログラムのデータを格納します。 ユーザーは、Microsoft アカウントまたは Azure Active Directory と同期また連携した企業資格情報でログインしてアプリにアクセスできます。

    クラウド コレクションを選択するのは、会社のプライベート ネットワークで、共有するアプリケーションに任意のリソースへの接続が必要ない場合 (たとえば、VPN デバイスを使用する場合) です。 アプリケーションがインターネット、OneDrive、または Azure 上のリソースを必要とする場合は、クラウド コレクションが適しています。 また、作成するのも、最も簡単です。

- A **ハイブリッド コレクション** でホストされ、Azure クラウドでユーザー データにアクセスやも、ローカル ネットワーク上に格納されているリソースにデータを格納します。 ユーザーは、Azure Active Directory と同期また連携した企業資格情報でログインしてアプリにアクセスできます。

    ハイブリッド コレクションを選択するのは、会社のプライベート ネットワーク上のリソースへの接続が必要な場合です。 たとえば、アプリケーションに、次のいずれかへのアクセスが必要な場合です。

    - イントラネット上に配置されたファイル サーバー
    - Quicken
    - ファイアウォールの内側にあるデータベース

    これは一般に、クラウドに移動することができない多数のリソースをプライベート ネットワーク上に持つ大企業にとって役立ちます。

さまざまなコレクションはネットワークなどのさまざまなオプション、ので解明 [コレクション](remoteapp-collections.md) 自分に最適です。 


### コレクションの更新
ハイブリッド コレクションとクラウドのコレクション間の主な違いの 1 つは、ソフトウェア更新の処理方法です。 プレインストールの Office 365 ProPlus または Office 2013 のイメージを使用するクラウド コレクションでは、更新プログラムについて心配する必要はありません。 サービスはそれ自体が継続的に保守され、更新プログラムはアプリとオペレーティング システムの両方に適用されます。

カスタム テンプレート イメージを使用するクラウド コレクションとハイブリッド コレクションでは、イメージとアプリを保守する必要があります。 ドメインに参加しているイメージでは、Windows Update、グループ ポリシー、または System Center などのツールを使用して更新プログラムを制御できます。

カスタム テンプレート イメージの更新後に、Azure クラウドに新しいイメージをアップロードし、新しいイメージを使用するコレクションを更新します。 (Azure RemoteApp からこれを行う **クイック スタート** ページまたはダッシュ ボードです)。

参照してください [コレクションの更新](remoteapp-update.md) の詳細。

## サポートされている RemoteApp クライアント
Azure RemoteApp は、Windows および Windows RT の RemoteApp クライアント アプリケーションと Mac、iOS および Android 用の Microsoft のリモート デスクトップ アプリでサポートされています。 ユーザーは、モバイル デバイスまたはコンピューティング デバイスでこれらのアプリを使用して、新しい Azure RemoteApp プログラムにアクセスできます。

参照してください [Azure RemoteApp でアプリにアクセスする](remoteapp-clients.md) の詳細については、クライアントにします。

## 次のステップ
始めましょう。 実際に使ってみてください。 以下の記事が Azure RemoteApp を開始するのに役立ちます。

- [Azure RemoteApp にはどのような種類のコレクションが必要ですか。](remoteapp-collections.md)
- [Azure RemoteApp イメージの作成](remoteapp-imageoptions.md)
- [Azure RemoteApp のクラウド コレクションの作成方法](remoteapp-create-cloud-deployment.md)
- [Azure RemoteApp のハイブリッド コレクションの作成方法](remoteapp-create-hybrid-deployment.md)
- [RemoteApp のライセンスはどのような仕組みになっていますか。](remoteapp-licensing.md)
- [Best practices for using Azure RemoteApp (Azure RemoteApp を使用するためのベスト プラクティス)](remoteapp-bestpractices.md)
- [Azure RemoteApp よく寄せられる質問](remoteapp-faq.md)
 

### サポートのお願い 
記事を評価したり、下にコメントを投稿したりするだけでなく、記事自体を変更できることを知っていましたか。 説明不足ですか。 間違いがありますか。 わかりにくいことが書いてありますか。 上にスクロールしてクリックして **GitHub の編集** の変更を加えるについては、私たちに来るし、次に、筆に一度表示されます、変更および強化ここです。
