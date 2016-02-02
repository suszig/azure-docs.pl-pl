<properties 
    pageTitle="Azure RemoteApp の FAQ | Microsoft Azure" 
    description="Azure RemoteApp についてよく寄せられる質問の回答を確認する。" 
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
    ms.date="10/23/2015" 
    ms.author="elizapo"/>


# Azure RemoteApp よく寄せられる質問

Azure RemoteApp について次のような質問が寄せられています。 他にもありますか? 参照してください、 [RemoteApp フォーラム](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) お知らせ調べるか、またはの下にコメントをドロップする必要があります。

## Azure RemoteApp とは

- **Azure RemoteApp とは。**RemoteApp とは、多くのさまざまなユーザー デバイスからアプリケーションへのセキュリティで保護されたリモート アクセスを提供する際に役立つ Azure サービスです。 詳細について [Azure RemoteApp](remoteapp-whatis.md)します。
- **展開オプションとは何ですか。**RemoteApp コレクションの 2 つの種類があります。 クラウドおよびハイブリッド型です。 ドメインへの参加の要不要など、さまざまな要素によって必要なコレクションが決まります。 すべての意志決定に関する話 [ここ](remoteapp-collections.md)します。

## Azure RemoteApp の使用に関するクイック ヒント

- * * するまでの時間切断されているか。 方法はアイドル状態でいられる時間ブートお願いですか? * * 4 時間です。 ユーザーのアイドル状態が 4 時間続くと、自動的に Azure RemoteApp からサインアウトされます。 その他の既定の設定をチェック_アウト [Azure サブスクリプションとサービスの制限、クォータ、および制約](azure-subscription-service-limits.md)します。
- **このサービスは無料で試すことができますか。**はい。 30 日間無料で使用できます。 試行期間が終了すると、(運用環境で使用可能な) 有料アカウントに切り替えるか、サービスの使用を終了することができます。 無料試用版を開始する [manage.windowsazure.com](http://manage.windowsazure.com) -RemoteApp の新しいインスタンスを作成します。 無料試用版では、2 つの RemoteApp インスタンスを作成でき、インスタンスあたり 10 ユーザーをサポートできます。   試行期間は、30 日であることに注意してください。
## Azure RemoteApp サブスクリプションの詳細

- **サービスの制限とは何ですか。**既定の設定について説明し、サービスで Azure RemoteApp の制限 [Azure サブスクリプションとサービスの制限、クォータ、および制約](azure-subscription-service-limits.md)します。 他に質問がある場合は、お知らせください。
- **必要なユーザー数は、どの程度でしょうか。**少なくとも 20 ユーザーが必要です。 明確にするために繰り返しますと、最低ユーザー数は 20 となっています。 つまり、20 ユーザー分の代金が課金されることになります。
- **RemoteApp の料金についてはどうですか。**チェック アウト [Azure RemoteApp の料金詳細 ](../../../pricing/details/remoteapp/)します。
- **コレクションの種類によってはコストがかかるものはありますか。** 
コレクションに対する要件によってはそうなる場合があります。 ハイブリッド コレクションの場合、Azure RemoteApp からオンプレミス ネットワークへの接続が必要です。 既存の VNET と Express のルートを使用する場合、追加コストはありません。 課金されます新しい Azure VNET と、ゲートウェイまたは Expressroute を使用する場合は、 [VPN ゲートウェイ](../../../pricing/details/vpn-gateway) または [Express Route](../../../pricing/details/expressroute/)します。 このコスト (詳細はリンクを参照) が、毎月の Azure の RemoteApp のコストに追加されます。

## サポートされているコレクションや使用すべきコレクションなど

- **カスタム基幹業務 (LOB) アプリケーションはサポートされていますか。**はい。 Azure RemoteApp でカスタム アプリケーションを使用するを作成、 [カスタム テンプレート イメージ](remoteapp-create-custom-image.md), 、RemoteApp コレクションにアップロードします。
- **自社のカスタム LOB アプリケーションは、Azure RemoteApp で動作しますか。**この質問の回答を見つけ出す最善の方法は、テストすることです。 レビュー、 [アプリケーション互換性要件](http://www.microsoft.com/download/details.aspx?id=18704) とチェック アウト、 [RD 互換性センター](http://www.rdcompatibility.com/compatibility/default.aspx)します。
- **組織にとって最適なデプロイ方法は、クラウドとハイブリッドのどちらですか。**ハイブリッド コレクションは、シングル サインオン (SSO) との完全な統合、およびセキュリティで保護されたオンプレミスのネットワーク接続を必要としている場合に、最も包括的なエクスペリエンスを提供します。 クラウド コレクションでは、複数の認証方法を使用することにより、アジャイルかつ簡単な方法を使用して、分離されたデプロイを構築できます。 詳細について、 [展開オプション](remoteapp-whatis.md)します。
- * * SQL または別のオンプレミスのデータベースがあるまたは Azure です。 展開の種類を使用すべきでしょうか。 * * は、SQL またはバックエンドのデータベースの場所によって異なります。 データベースがプライベート ネットワークにある場合は、ハイブリッド コレクションを使用します。 データベースがインターネットに表示され、クライアント接続で接続できる場合は、クラウド コレクションを使用できます。
- **ドライブの割り当て、USB とシリアル ポート、クリップボードの共有、およびプリンターのリダイレクトについてはどうでしょうか。**Azure RemoteApp は、これらのすべての機能についてサポートしています。 クリップボードの共有とプリンターのリダイレクトは、既定で有効になります。 リダイレクトの詳細については、 [ここ](remoteapp-redirection.md)します。


## テンプレート イメージ

- **クラウドまたは既存の仮想マシンを、RemoteApp コレクションのテンプレートとして使用できますか。**はい。 Azure VM に基づくイメージを作成することも、サブスクリプションに含まれるイメージの 1 つを使用することも、またはカスタム イメージを作成することもできます。 チェック アウト、 [RemoteApp のイメージ オプション](remoteapp-imageoptions.md)します。


## ネットワーク オプション

- * *、ハイブリッド コレクションには、VNET が必要です。 既存の VNET を使用できますか。 * * 既存の VNET が Azure VNET である場合を実行できます。 参照してください"手順 1: 仮想ネットワークをセットアップ"で、 [ハイブリッド コレクションの指示](remoteapp-create-hybrid-deployment.md) の詳細。
- **クラウドのコレクションで VNET を使用できますか。**はい、できます。 チェック アウト [クラウド コレクションの作成](remoteapp-create-cloud-deployment.md), 、詳細については、手順 1. 特にです。

## 認証オプション

- * * 認証については どのメソッドがサポートされているか。 * * クラウド コレクションは、Microsoft アカウントと Azure Active Directory のアカウントは、Office 365 アカウントもサポートしています。 ハイブリッド コレクションが同期されている Azure Active Directory アカウントのみをサポートしています (などのツールを使用して [Azure Active Directory 同期](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) 、Windows Server Active Directory の展開から具体的には、いずれかで同期パスワード同期オプションを使用して、または構成された Active Directory フェデレーション サービス (AD FS) フェデレーションと同期します。 構成することも [多要素認証 (MFA)](../../services/multi-factor-authentication/)します。

>[AZURE.NOTE]Azure Active Directory ユーザーは、サブスクリプションに関連付けられているテナントに属している必要があります (サブスクリプションは、ポータルの **[設定]** タブで表示および変更できます。 参照してください [RemoteApp で使用される Azure Active Directory テナントの変更](remoteapp-changetenant.md) の詳細)。

- **使用している Azure Active Directory アカウントにアクセス許可を付与できないのはなぜですか。**Azure Active Directory ユーザーは、サブスクリプションに関連付けられているディレクトリに属している必要があります。 このディレクトリは、ポータルの [設定] タブで表示および変更できます。 参照してください [RemoteApp で使用される Azure Active Directory テナントの変更](remoteapp-changetenant.md) の詳細。

## クライアント - Azure RemoteApp へのアクセスにはどのようなデバイスを使用できますか?

さまざまなクライアントをインストールするための手順を含め、適切なクライアント情報が表示 [Azure RemoteApp でアプリにアクセスする](remoteapp-clients.md)します。

- **クライアント アプリケーションがサポートするデバイスとオペレーティング システムはどれでしょう。**
コンピューターとタブレット:
    - Windows 10 (クライアント プレビュー)
    - Windows 8.1 および Windows 8
    - Windows 7 Service Pack 1
    - Mac OS X
    - Windows RT
    - Android タブレット
    - Ipad
電話:
    - iPhone
    - Android フォン
    - Windows Phone

    [ダウンロード](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) 今すぐ RemoteApp をします。
- **Azure RemoteApp はシン クライアントをサポートしますか。**はい、次の Windows Embedded シン クライアントがサポートされます。
    - Windows Embedded Standard 7
    - Windows Embedded 8 Standard
    - Windows Embedded 8.1 Industry Pro
    - Windows 10 IoT Enterprise

- **リモート デスクトップ セッション ホスト (RDSH) 用にサポートされる Windows Server のバージョンはどれですか。**Windows Server 2012 R2 です。

## サポートとフィードバック

- **RemoteApp のサポート プランについてはどうですか。**課金およびサブスクリプション管理が、無料でサポートされます。 テクニカル サポートは、 [Azure のサービス プラン](../../../support/plans/)します。 、無料のコミュニティ サポートを取得することも、 [Azure のディスカッション フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp)します。
- **フィードバックを発行する方法を教えてください。**参照してください、 [フィードバック フォーラム](http://feedback.azure.com/forums/247748-azure-remoteapp)します。
- **Azure RemoteApp についての問い合わせ先を教えてください。**加え、 [ディスカッション フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), 、質問を投稿する最適の場所は、毎週に参加することができます [Experts webinar を依頼](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), RemoteApp のあらゆることについて話をします。
- **RemoteApp に関するドキュメントはありますか。**ご問い合わせありがとうございます。 ポータルのヘルプ ドロアーのヘルプ コンテンツ (ポータルのいずれかのページの **?** をクリックするだけです) に加えて、RemoteApp のすべてについて詳細に説明する次の資料を参照できます。
    - **概要:**
        - [RemoteApp は何ですか?](remoteapp-whatis.md)
        - [RemoteApp テンプレート イメージは何ですか?](remoteapp-images.md)
        - [ライセンス作業をどのように?](remoteapp-licensing.md)
        - [RemoteApp と Office 連携するしくみですか?](remoteapp-o365.md)
        - [Redirection RemoteApp で](remoteapp-redirection.md)でしょうか。
    - **デプロイ:**
        - [カスタム テンプレート イメージの作成](remoteapp-create-custom-image.md)
        - [ハイブリッド コレクションの作成](remoteapp-create-hybrid-deployment.md)
        - [クラウド コレクションの作成](remoteapp-create-cloud-deployment.md)
        - [RemoteApp の Azure Active Directory を構成する](remoteapp-ad.md)
        - [Publish an app in RemoteApp](remoteapp-publish.md)
    - **管理:**
        - [ユーザーを追加する](remoteapp-user.md)
        - [を構成して、RemoteApp を使用のベスト プラクティス](remoteapp-bestpractices.md)

    ビデオ。RemoteApp についての多くのビデオがあります。概要 ([Azure RemoteApp の概要](http://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)) について説明する展開中に ([クラウド デプロイ](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) と [ハイブリッドデプロイメント](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be))します。確認してください。


### サポートのお願い

記事を評価したり、下にコメントを投稿したりするだけでなく、記事自体を変更できることを知っていましたか。 説明不足ですか。 間違いがありますか。 わかりにくいことが書いてありますか。 上へスクロールし、**[GitHub で編集]** をクリックして、変更を加えることができます。変更はこちらで確認し、承認されると、変更点や改善点がこのページに反映されます。





