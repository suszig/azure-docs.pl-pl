<properties
    pageTitle="Azure App Service のデプロイに関するドキュメント"
    description="Azure App Service へのアプリのデプロイについて説明しているドキュメントを紹介します。"
    services="app-service"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/06/2015"
    ms.author="tdykstra"/>

# Azure App Service のデプロイに関するドキュメント

## 概要

この記事の一覧へのコンテンツを展開するために使用可能なメソッド [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), 、記事や操作方法に関する情報が含まれているブログへのリンクなどです。 追加の記事が発行されたら、このリストに加えられます。 

Web アプリをデプロイする最善の方法を設定し、 [継続的な配信ワークフロー](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) に統合されて、 [ソース管理システム](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)します。 自動化により、開発プロセスの効率が向上することに加え、バックアップと復元のプロセスの管理性と信頼性も向上します。

##### クラウドでホストされているソース管理システムからのデプロイ

* [Visual Studio Team Services を使用した継続的な配信](#vsts)
* [Git を使用したリポジトリ Web サイト](#git)
* [Mercurial を使用したリポジトリ Web サイト](#mercurial)
* [Dropbox からのデプロイの自動化](#dropbox)

##### オンプレミスのソース管理システムからのデプロイ

* [Team Foundation Server (TFS) による継続的な配信](#tfs)
* [内部設置型の Git または Mercurial リポジトリ](#onpremises)

##### コマンド ライン ツールを使用したデプロイの自動化

* [MSBuild を使用したデプロイの自動化](#msbuild)
* [FTP ツールとスクリプトを使用してファイルをコピー](#ftp)
* [Windows PowerShell を使用したデプロイの自動化](#powershell)
* [.NET 管理 API を使用したデプロイの自動化](#api)
* [Azure コマンド ライン インターフェイス (Azure CLI) からデプロイ](#cli)
* [Web デプロイ コマンド ラインからデプロイ](#webdeploy)
 
##### 統合開発環境 (IDE) からデプロイ

* [Visual Studio から直接デプロイ](#vs)
* [WebMatrix から直接デプロイ](#webmatrix)

もう 1 つの展開オプションでは、クラウド ベースのサービスを使用して [Octopus 展開](http://en.wikipedia.org/wiki/Octopus_Deploy)します。 詳細については、次を参照してください。 [デプロイ ASP.NET アプリケーションを Azure の Web サイト](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites)します。

##<a name="vsts"></a>Visual Studio Team Services を使用した継続的な配信

[Visual Studio Team Services](http://www.visualstudio.com/) (旧 Team Foundation Service) は Microsoft のクラウド ベースのソリューション ソース管理とチーム コラボレーションを対象。 開発者が 5 人以下のチームは、このサービスを無料で使用できます。 App Services での web アプリへの継続的な配信を行うことができ、リポジトリは、いずれかを使用して [Git または TFVC](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#gittfs)します。

詳細については、次のリソースを参照してください。

* [Visual Studio チームのサービスと TFVC を使用して Azure に継続的な配信](../cloud-services-continuous-delivery-use-vso.md)します。 TFVC を使用して Visual Studio Team Services から Web アプリへの継続的な配信を設定する方法を示すステップ バイ ステップ チュートリアルです。 分散型ソース管理オプションである Git とは対照的に、TFVC は集中型ソース管理オプションです。
* [Visual Studio Team Services および Git を使用して Azure に継続的な配信](../cloud-services-continuous-delivery-use-vso-git.md)します。 上のチュートリアルに似ていますが、TFVC の代わりに Git を使用します。

##<a name="git"></a>Git を使用したリポジトリ Web サイト

[Git](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#gittfs) は人気の高い分散型ソース管理システムです。 Azure を含む Git リポジトリを格納する一般的な web ベースのリポジトリ サイトから web アプリに展開を自動化するが簡単にする組み込みの機能を持つ [GitHub](http://www.github.com), 、[CodePlex](http://www.codeplex.com/), 、および [BitBucket](https://bitbucket.org/)します。 Git を使用したデプロイに伴う利点は、以前のデプロイが必要になったときに、比較的簡単に以前のデプロイにロールバックできることです。

詳細については、次のリソースを参照してください。

* [ソース管理から Git を使用した Web アプリへの発行](web-sites-publish-source-control.md)します。 Git を使用してローカル コンピューターから Web Apps に直接発行する方法 (Azure では、この発行方法をローカル Git と呼びます)。 また、GitHub、CodePlex、BitBucket からの Git リポジトリの継続的なデプロイを有効にする方法も示します。
* [Kudu を使用して GitHub を使用して Web アプリに展開する](http://azure.microsoft.com/documentation/videos/deploying-to-azure-from-github/)です。 Scott Hanselman と David Ebbo によるビデオ。GitHub から Web Apps へ直接 Web アプリをデプロイする方法を示します。
* [Web アプリに対する Azure ボタンのデプロイ](http://azure.microsoft.com/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/)します。 Git リポジトリからのデプロイをトリガーする方法に関するブログです。
* [Git、Mercurial、Dropbox に関する azure フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azuregit)します。

##<a name="mercurial"></a>Mercurial を使用したリポジトリ Web サイト

使用する場合 [Mercurial](http://mercurial.selenic.com/) ように、ソース管理システムとで、リポジトリに保存 [CodePlex](http://www.codeplex.com/) または [BitBucket](https://bitbucket.org/), 、コンテンツを自動的にデプロイする Azure App Service での組み込み機能を使用できます。

Mercurial を使用してデプロイを行う方法の詳細については、次のリソースを参照してください。

* [ソース管理から Git を使用した Web アプリへの発行](web-sites-publish-source-control.md)します。 このチュートリアルでは、Git リポジトリを発行する方法を示していますが、CodePlex または BitBucket でホストされている Mercurial リポジトリに関するプロセスは類似しています。
* [Git、Mercurial、Dropbox に関する azure フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azuregit)します。

##<a name="dropbox"></a>Dropbox からのデプロイの自動化

[Dropbox](https://www.dropbox.com/) 、ソース管理システムではありませんが、ソース コードを Dropbox に格納する場合、Dropbox アカウントからデプロイを自動化することができます。

* [Dropbox から Web アプリをデプロイ](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx)します。 使用する方法、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715) Dropbox デプロイを設定します。
* [Web アプリに対する Dropbox デプロイ](http://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Dropbox-Deployment-to-Windows-Azure-Web-Sites)します。 このビデオでは、Dropbox フォルダーを Web アプリに接続する方法について説明し、Web アプリを迅速に設定して動作させる方法や、簡単なドラッグ アンド ドロップのデプロイを使用して Web サイトを保持する方法を示します。
* [Git、Mercurial、Dropbox に関する azure フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azuregit)します。

##<a name="vs"></a>Visual Studio から直接デプロイ

Visual Studio から Web Apps をデプロイする方法については、次のリソースを参照してください。

* [Azure と ASP.NET を使ってみる](web-sites-dotnet-get-started.md)します。 Visual Studio と Web Deploy を使用して簡単な ASP.NET MVC Web プロジェクトを作成し、デプロイする方法。
* [Azure WebJobs のデプロイ Visual Studio を使用する方法](websites-dotnet-deploy-webjobs.md)します。 コンソール アプリケーション プロジェクトを Web ジョブとしてデプロイするための構成方法。  
* [Web アプリケーションへのメンバーシップ、OAuth、SQL データベースとのセキュリティで保護された ASP.NET MVC 5 アプリケーションの展開](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)します。 Visual Studio、Web Deploy、Entity Framework、Code First Migrations を使用して、ASP.NET MVC Web プロジェクトを SQL データベースと共に配置する方法。
* [Visual Studio と ASP.NET の web 配置の概要](http://msdn.microsoft.com/library/dd394698.aspx)します。 Visual Studio を使用して Web デプロイを実行するための基本的な概要です。 作成以来期間が経過していますが、現在も引き続き当てはまる情報が掲載されており、その中には Web アプリケーションと共にデータベースをデプロイするためのオプションの概要、実行する必要が生じる可能性のある付加的なデプロイ タスクの一覧、Visual Studio を独自に手動で構成する方法が含まれています。 このトピックには、Web Apps へのデプロイのみではなく、デプロイ全般に関する情報も掲載されています。
* [Visual Studio を使用して ASP.NET Web デプロイ](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction)します。 12 部構成のチュートリアル シリーズであり、このリスト内に掲載されている他のリソースより詳細にデプロイ タスクの範囲を網羅しています。 チュートリアルを作成した後で、不足している事項を説明するためにメモが追加されたため、一部の Azure デプロイ機能が追加されました。
* [Git リポジトリから Visual Studio 2012 での Azure に ASP.NET web サイトを直接デプロイ](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881)します。 Visual Studio 内で ASP.NET Web プロジェクトをデプロイし、Git プラグインを使用してコードを Git にコミットして、Azure を Git リポジトリに接続する方法について説明します。 Visual Studio 2013 以降、Git のサポートは組み込まれており、プラグインのインストールは必要ありません。

##<a name="webmatrix"></a>WebMatrix から直接デプロイ

WebMatrix から Web Apps をデプロイする方法については、次のリソースを参照してください。

* [ビルドおよび Node.js web サイトを WebMatrix を使用して Azure に配置](web-sites-nodejs-use-webmatrix.md)します。
* [作成し、WebMatrix を使用した Php-mysql web アプリのデプロイ](web-sites-php-mysql-use-webmatrix.md)します。
* [WebMatrix 3: Git の統合と Azure へのデプロイ](http://www.codeproject.com/Articles/577581/Webmatrixplus3-3aplusIntegratedplusGitplusandplusD)します。 WebMatrix を使用して、Git ソース管理リポジトリからデプロイを実行する方法です。

詳細については、次のリソースを参照してください。

* [Php-mysql web アプリを作成し、FTP を使用して展開](web-sites-php-mysql-deploy-use-ftp.md)します。

##<a name="tfs"></a>Team Foundation Server (TFS) による継続的な配信

Team Foundation Server は、ソース管理とチーム コラボレーションを対象にする、マイクロソフトのオンプレミスのソリューションです。 Web アプリへの継続的な配信を実行するように TFS を設定することができます。

詳細については、次のリソースを参照してください。

* [Azure でサービスのクラウドの継続的な配信](../cloud-services-dotnet-continuous-delivery.md)します。 このドキュメントは Azure クラウド サービスを対象にしていますが、そのコンテンツの一部は Web Apps にも当てはまります。

##<a name="gitmercurial"></a>内部設置型の Git または Mercurial リポジトリ

Git または Mercurial を使用する任意のリポジトリからデプロイを実行するために、Azure でそのリポジトリの URL を入力することができます。 ローカルの Git リポジトリから Web アプリへの直接のプッシュを実行することもできます。

詳細については、次のリソースを参照してください。

* [ソース管理から Git を使用した Web アプリへの発行](web-sites-publish-source-control.md)します。 Git を使用してローカル コンピューターから Web アプリに直接発行する方法 (Azure では、この発行方法をローカル Git と呼びます)。 また、GitHub、CodePlex、BitBucket からの Git リポジトリの継続的なデプロイを有効にする方法も示します。
* [任意の git/hg リポジトリから Web アプリへの発行](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html)します。 Web Apps で "外部リポジトリ" 機能を使用する方法を説明しているブログ。
* [Git、Mercurial、Dropbox に関する azure フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azuregit)します。
* [1 つの Git リポジトリから Azure への 2 つの web サイトをデプロイ](http://www.hanselman.com/blog/DeployingTWOWebsitesToWindowsAzureFromOneGitRepository.aspx)します。 Scott Hanselman 氏によるブログの投稿です。

##<a name="msbuild"></a>MSBuild を使用したデプロイの自動化

使用する場合、 [Visual Studio IDE](#vs) 開発では、使用することができます [MSBuild](http://msbuildbook.com/) を自動化して IDE 内のすべてことができます。 いずれかを使用するために MSBuild を構成する [Web Deploy](#webdeploy) または [FTP と FTPS](#ftp) ファイルをコピーします。 Web Deploy では、データベースのデプロイなど、デプロイに関連する多くのタスクを自動化することもできます。

MSBuild を使用したコマンドライン デプロイの詳細については、次のリソースを参照してください。

* [Visual Studio を使用して ASP.NET Web デプロイ: コマンドラインのデプロイ](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment)します。 Visual Studio を使用して Azure のデプロイを行うチュートリアル シリーズの第 10 部。 Visual Studio でプロファイルの発行を設定した後に、コマンド ラインを使用してデプロイを行う方法を示します。
* [『 Inside the Microsoft Build Engine: Using MSBuild and Team Foundation ビルド](http://msbuildbook.com/)します。 MSBuild を使用してデプロイを実行する方法に関する章が掲載されている書籍。

##<a name="ftp"></a>FTP ツールとスクリプトを使用してファイルをコピー

使用してコンテンツをアプリにデプロイできます [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) ファイルをコピーします。 これを簡単に web アプリの FTP の資格情報を作成し、スクリプトやなど、Internet Explorer を無料のユーティリティを備えたなどのブラウザーを含め、FTP を使用するアプリケーションで使用することができます [FileZilla](https://filezilla-project.org/)します。 Web Apps では、より安全な FTPS プロトコルもサポートしています。

FTP ユーティリティを使用して Web アプリのファイルを Azure にコピーすることは簡単ですが、データベースのデプロイや接続文字列の変更のような関連するデプロイ タスク自動的な処理や調整が FTP ユーティリティによって実行されることはありません。 また、多くの FTP ツールは、変更されていないファイルのコピーをスキップする目的でコピー元とコピー先のファイルを比較することもありません。 大規模なアプリで常にすべてのファイルをコピーする場合は、小規模な更新であっても常にすべてのファイルがコピーされるため、デプロイに長時間を要する可能性があります。

詳細については、次のリソースを参照してください。

* [FTP バッチ スクリプトを使用して](http://support.microsoft.com/kb/96269)します。

##<a name="powershell"></a>Windows PowerShell を使用したデプロイの自動化

MSBuild または FTP デプロイの機能を実行する [Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx)します。 この機能を実行する場合は、Azure の REST 管理 API を簡単に呼び出せる一連の Windows PowerShell コマンドレットを使用することもできます。

詳細については、次のリソースを参照してください。

* [GitHub リポジトリにリンクされる Web アプリのデプロイ](app-service-web-arm-from-github-provision.md)
* [Web アプリと SQL Database をプロビジョニングする](app-service-web-arm-with-sql-database-provision.md)
* [Azure でマイクロサービスを予測どおりにデプロイする](app-service-deploy-complex-application-predictably.md)
* [Azure での実際のクラウド アプリケーションのビルドでは、すべてを自動化](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything)します。 電子書籍の中に掲載されているサンプル アプリケーションが、Windows PowerShell スクリプトをどのように使用して Azure テスト環境を作成し、その環境をデプロイするかを説明する、電子書籍の章。 参照してください、 [リソース](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) その他の Azure PowerShell のドキュメントへのリンクについてのセクションです。
* [Windows PowerShell スクリプトを使用して、開発環境およびテスト環境に発行する](http://msdn.microsoft.com/library/dn642480.aspx)です。 Visual Studio で生成される Windows PowerShell デプロイ スクリプトの使用方法

##<a name="api"></a>.NET 管理 API を使用したデプロイの自動化

C# のコードを作成し、デプロイの目的で MSBuild や FTP の機能を実行することができます。 この作業を実行する場合は、Azure の管理 REST API にアクセスして、サイト管理機能を実行することができます。

詳細については、次のリソースを参照してください。

* [Azure 管理ライブラリと .NET を使用してすべてを自動化](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)します。 .NET 管理 API の紹介とその他のドキュメントへのリンク。

##<a name="cli"></a>Azure コマンド ライン インターフェイス (Azure CLI) からデプロイ

Windows、Mac、Linux マシンでコマンド ラインを使用し、FTP を使用してデプロイを実行することもできます。 この作業を実行する場合は、Azure CLI を使用して Azure REST 管理 API にアクセスすることもできます。

詳細については、次のリソースを参照してください。

* [Azure コマンド ライン ツール](/downloads/#cmd-line-tools)します。 コマンド ライン ツールに関する情報を掲載している Azure.com のポータル ページです。

##<a name="webdeploy"></a>Web デプロイ コマンド ラインからデプロイ

[Web Deploy](http://www.iis.net/downloads/microsoft/web-deploy) は Microsoft ソフトウェアだけでなくインテリジェントなファイルの同期は、IIS に配置するための機能もことができますを実行または他の多くの展開に関連するタスクを FTP を使用する場合は自動化できを調整します。 たとえば、Web Deploy を使用して Web アプリと共に新しいデータベースをデプロイ、またはデータベースの更新をデプロイすることができます。 また、Web Deploy は、変更されたファイルのみをインテリジェントにコピーするため、既存のサイトを更新するのに要する時間を最小化することもできます。 Microsoft WebMatrix、Visual Studio、Visual Studio Online、および Team Foundation Server は Web Deploy のビルトイン機能をサポートしていますが、コマンド ラインから Web Deploy を直接使用してデプロイを自動化することもできます。 Web Deploy の各コマンドは非常に強力ですが、学習曲線を急上昇させることもできます。

詳細については、次のリソースを参照してください。

* [単純な Web アプリ: デプロイ](http://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/)します。 Web デプロイの使用を簡単にするために作成したツールに関する David Ebbo のブログ
* [Web デプロイ ツール](http://technet.microsoft.com/library/dd568996)します。 Microsoft TechNet サイトの公式ドキュメント。 作成以来期間が経過していますが、現在も有効な出発点として使用できます。
* [Web Deploy の使用](http://www.iis.net/learn/publish/using-web-deploy)します。 Microsoft IIS.NET サイトの公式ドキュメント。 同じく作成以来期間が経過していますが、現在も有効な出発点として使用できます。
* [StackOverflow](http://www.stackoverflow.com)します。 コマンドラインから Web Deploy を使用する方法に関する最新の情報を掲載した、出発点として最適な場所です。
* [Visual Studio を使用して ASP.NET Web デプロイ: コマンドラインのデプロイ](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment)します。 MSBuild は Visual Studio によって使用されるビルド エンジンであり、Web アプリケーションを Web Apps にデプロイする目的でコマンド ラインから使用することもできます。 このチュートリアルは、Visual Studio のデプロイに注目したシリーズの一部です。

##<a name="nextsteps"></a>次のステップ

シナリオによっては、Web アプリをステージングと運用バージョンの間で簡単に切り替えられる環境が必要です。 詳細については、次を参照してください。 [Web アプリのステージングされたデプロイ](web-sites-staged-publishing.md)します。

バックアップと復元計画を策定することは、デプロイ ワークフローの重要な部分です。 Web アプリに関する情報をバックアップおよび復元の機能を参照してください [Web アプリのバックアップ](web-sites-backup.md)します。  

Azure のロールベースのアクセス制御を使用して Web アプリのデプロイへのアクセスを管理する方法については、次を参照してください。 [RBAC と Web アプリの公開](http://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing)します。

その他の展開に関するトピックについては、デプロイ」セクションを参照してください。 [Web Apps のドキュメント](/documentation/services/web-sites/)します。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)
 

