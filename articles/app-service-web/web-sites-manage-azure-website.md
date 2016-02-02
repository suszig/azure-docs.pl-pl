<properties 
    pageTitle="Azure App Service での Web アプリの管理" 
    description="Azure App Service で Web アプリを管理するためのリソースへリンクしています。" 
    services="app-service\web" 
    documentationCenter="" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2015" 
    ms.author="erikre"/>


# Azure App Service での Web アプリの管理

このトピックには、web アプリの管理に関するリソースへのリンクが含まれています。 [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)します。 管理作業には、Web アプリのスムースな運用を維持するためのすべての作業が含まれます。

Web アプリのライフタイム全体にわたって、初期デプロイから、通常の運用、保守、更新に至るまで、さまざまな管理作業を実行することになります。

Azure ポータルでは、多くの Web アプリ管理作業を実行できます。

## Web アプリを運用環境にデプロイする前

### レベルの選択

Azure App Service には、 Free、Shared、Basic、Standard、および Premium の 5 つのレベルが用意されています。 機能と各階層の料金の詳細については、次を参照してください。 [料金の詳細](/pricing/details/app-service/)します。

- [App Service プランの](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) により複数の web アプリを同じレベルのグループ化できます。
- 常に実行できます [レベルの切り替え](web-sites-scale.md) web アプリを作成した後です。

### 構成

使用して、 [Azure ポータル](https://portal.azure.com/) さまざまな構成オプションを設定します。 詳細については、「 [Azure App Service で web アプリの構成](web-sites-configure.md)します。 以下にクイック チェックリストを示します。

- 必要に応じて、.NET、PHP、Java、または Python の**ランタイム バージョン**を選択します。
- Web アプリで WebSocket プロトコルを使用する場合、**WebSockets** を有効にします。 (これを使用するアプリが含まれます [ASP.NET SignalR](http://www.asp.net/signalr) または [socket.io](web-sites-nodejs-chat-app-socketio.md).)
- 継続的な Web ジョブを実行していますか? そうであれば、**[常時接続]** を有効にします。
- index.html など、**既定文書**を設定します。

これらの基本的な設定に加えて、以下の設定もあります。

- **Secure Socket Layer (SSL)** 暗号化。 カスタム ドメイン名で SSL を使用するには、SSL 証明書を取得し、それを使用するように Web アプリを設定する必要があります。 参照してください [Azure App Service で web アプリの HTTPS の有効化](web-sites-configure-ssl-certificate.md)します。
- **カスタム ドメイン名。**Web アプリは、自動的に azurewebsites.net 下のサブドメインを持ちます。 contoso.com などのカスタム ドメイン名を関連付けることができます。 参照してください [Azure App Service でのカスタム ドメイン名を構成する](web-sites-custom-domain-name.md)します。

言語固有の構成:

- **PHP**: [Azure App Service Web Apps で PHP を構成](web-sites-php-configure.md)します。
- **Python**: [Web アプリの Azure App Service での Python の構成](web-sites-python-configure.md)


## Web アプリの運用中

Web アプリの運用中、それが利用可能であり、ユーザー トラフィックに対して適切なスケーリングになっていることを確認する必要があります。 また、エラーのトラブルシューティングが必要になることがあるかもしれません。

### Monitoring

- Azure ポータルを使用することができます [パフォーマンス メトリックを追加](web-sites-monitor.md) CPU 使用率やクライアント要求の数などです。
- 詳細に分析するには、New Relic を使用してパフォーマンスを監視および管理してください。 参照してください [Azure App Service の .NET web アプリに対する New relic によるアプリケーション パフォーマンス管理](store-new-relic-web-sites-dotnet-application-performance-management.md)します。
- [Web アプリのスケーリング](web-sites-scale.md) トラフィックに応答します。 レベルに応じて、VM の数や VM インスタンスのサイズのスケーリングが可能です。  Standard および Premium レベルの場合には、自動スケーリングを設定することができます。その場合、Web アプリは、固定スケジュールに従って、または負荷に応じて、自動スケールされます。

### バックアップ

- 設定 [自動バックアップ](web-sites-backup.md) web アプリのです。 内のバックアップの詳細について [このビデオ](http://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/)します。
- ためのオプションについて [データベース復旧](../sql-database-business-continuity.md) Azure SQL データベースにします。

### トラブルシューティング

- 問題が生じた場合は、 [Visual Studio でのトラブルシューティングを行う](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), 、およびクラウドでのデバッグをライブ、診断ログを使用します。
- Visual Studio 以外にも、診断ログ収集のためのさまざまな方法があります。 参照してください [Azure App Service での web アプリの診断ログを有効にする](web-sites-enable-diagnostic-log.md)します。
- Node.js アプリケーションについては、次を参照してください。 [Azure App Service での Node.js web アプリをデバッグする方法について](web-sites-nodejs-debug.md)します。

### データの復元

- [復元](web-sites-restore.md) 以前にバックアップされている web アプリです。


## Web アプリの更新時

自動バックアップを有効にしない場合は、作成、 [手動バックアップ](web-sites-backup.md)します。

使用を検討して [のステージングされたデプロイメント](web-sites-staged-publishing.md)します。 このオプションを使用すると、運用デプロイと併用実行されるステージング デプロイへ更新を公開することができます。

Visual Studio Team Services を使用する場合は、ソース管理から継続的デプロイを設定することができます。

- [Team Foundation バージョン管理 (TFVC) を使用します。](../cloud-services-continuous-delivery-use-vso.md)
- [Git を使用してください。](../cloud-services-continuous-delivery-use-vso-git.md)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]





[before you deploy your site to production]: #before-you-deploy-your-site-to-production 
[while your website is running]: #while-your-website-is-running 
[when you update your website]: #when-you-update-your-website 

