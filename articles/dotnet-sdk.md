<properties 
    pageTitle="Azure .NET SDK とは" 
    description="Azure .NET SDK に含まれるものを紹介します。" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="mollybos" 
    services=""/>

<tags 
    ms.service="multiple" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/11/2015" 
    ms.author="tdykstra"/>

# Azure SDK for .NET とは

## 概要

Azure SDK for .NET は、Visual Studio のツール、コマンドライン ツール、ランタイム バイナリ、およびクライアント ライブラリを 1 つにまとめたもので、Azure で実行するアプリケーションの開発、テスト、デプロイに役立ちます。 この記事では、SDK のインストールにより、どのようなことができるかについて説明します。 SDK をダウンロードすることができます、 [Azure のダウンロード ページ](/downloads/)します。 

Azure SDK for .NET にも含まれています [Azure サービスを使用するためのクライアント ライブラリ](http://go.microsoft.com/fwlink/?LinkId=510472)します。 これらのライブラリは NuGet で個々にインストールできます。

##<a id="included"></a>Azure SDK for .NET のインストールに含まれるもの

Azure SDK for .NET には次の製品が含まれており、インストールされます。

- [Visual Studio Express for Web](#vwd)
- [Microsoft ASP.NET and Web Tools for Visual Studio](#wte)
- [Microsoft Azure Tools for Microsoft Visual Studio](#tools)
- [Microsoft Azure Authoring Tools](#auth)
- [Microsoft Azure エミュレーター](#emulator)
- [Microsoft Azure ストレージ エミュレーター](#stgemulator)
- [Microsoft Azure Storage Tools](#stgtools)
- [Microsoft Azure Libraries for .NET](#libraries)
- [HDInsight Tools for Visual Studio](#hdinsight) と [Microsoft Hive ODBC ドライバー](#hdinsight)
- [Microsoft Azure Mobile App SDK V1.0](#mobile)
- [Microsoft Azure PowerShell](#ps)

###<a id="vwd"></a>Visual Studio Express for Web

SDK がインストールされている場合は、コンピューターには、Visual Studio がない、 [Visual Studio Express for Web](http://www.visualstudio.com/products/visual-studio-express-vs.aspx)します。 
 
###<a id="wte"></a>Microsoft ASP.NET and Web Tools for Visual Studio

この製品をインストールすると、Azure の Web サイトで次のことができるようになります。

* [Web プロジェクトを Azure Websites に発行](web-sites-dotnet-get-started.md)します。
* [Azure web ジョブにコンソール アプリケーション プロジェクトを発行](websites-dotnet-deploy-webjobs.md)します。
* [Azure の web サイトおよび SQL データベース リソースを作成して、新しい web プロジェクトを作成するときに web プロジェクトの発行または](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)です。
* [PowerShell デプロイメント スクリプトを作成する新規の web サイトの作成中に](http://msdn.microsoft.com/library/dn642480.aspx)します。
* [管理し、サーバー エクスプ ローラーで Azure web サイトのトラブルシューティングを行う](web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement)します。
* [Web サイトおよび web ジョブに対してリモートでデバッグ モードで実行](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)します。 

>[AZURE.NOTE] これらの機能を使用する .NET 用 Azure SDK をインストールする必要はありません。これらは、Visual Studio の更新版にも含まれます。 

###<a id="tools"></a>Microsoft Azure Tools for Microsoft Visual Studio

この製品を利用すると、Azure のリソース (主に Cloud Services および仮想マシン) で次のことができるようになります。

* [作成、開くには、およびクラウド サービス プロジェクトを発行](cloud-services-dotnet-get-started.md)します。
* [クラウドのデプロイメント パッケージをサービス プロジェクトを作成](http://msdn.microsoft.com/library/ff683672.aspx)します。
* [新しい web プロジェクトを作成する Azure 仮想マシンを作成](virtual-machines-dotnet-create-visual-studio-powershell.md)します。
* [新しい仮想マシンを作成する PowerShell スクリプトを作成](http://msdn.microsoft.com/library/dn642480.aspx)します。
* [Visual Studio プロジェクトのプロパティ ウィンドウでクラウド サービス プロジェクトの設定の管理を表示および](http://msdn.microsoft.com/library/ee405486.aspx)です。
* 表示および管理 [クラウド サービス](http://msdn.microsoft.com/library/ff683675.aspx), 、[仮想マシン](http://msdn.microsoft.com/library/jj131259.aspx), 、および [Service Bus](http://msdn.microsoft.com/library/jj149828.aspx) サーバー エクスプ ローラーでします。 
* [クラウド サービスおよび仮想マシンに対してリモート デバッグ モードで実行](http://msdn.microsoft.com/library/ff683670.aspx)します。
* [Azure リソース グループ デプロイ プロジェクトを使用したリソースのプロビジョニングの自動化](https://msdn.microsoft.com/library/dn872471.aspx)

###<a id="auth"></a>Microsoft Azure Authoring Tools

この製品には次のツールが含まれています。

*  [CSPack コマンド ライン ツール](http://msdn.microsoft.com/library/gg432988.aspx) 展開パッケージを作成するためです。
*  [CSEncrypt コマンド ライン ツール](http://msdn.microsoft.com/library/hh404001.aspx) リモート デスクトップ接続を介してクラウド サービス ロール インスタンスにアクセスするためのパスワードを暗号化するためです。
* クラウド サービス プロジェクトがランタイム環境と通信し、診断する場合に必要な各種のランタイム バイナリ。 これらのバイナリは、NuGet パッケージでは利用できません。

###<a id="emulator"></a>Microsoft Azure エミュレーター

 [Azure エミュレーター](http://msdn.microsoft.com/library/dn339018.aspx) ようにをテストできるクラウド サービス プロジェクトのローカル コンピューターに Azure にデプロイする前に、クラウド サービス環境をシミュレートします。

###<a id="stgemulator"></a>Microsoft Azure ストレージ エミュレーター

 [Azure ストレージ エミュレーター](http://msdn.microsoft.com/library/hh403989.aspx) 、SQL Server インスタンスとローカル ファイル システムを使用してローカルでテストできるように、Azure ストレージ (キュー、テーブル、blob) をシミュレートします。 

###<a id="stgtools"></a>Microsoft Azure Storage Tools

これにより、インストール [AzCopy](http://aka.ms/AzCopy), 、コマンド ライン ツールが、Azure ストレージ アカウントに出入りするデータの転送を行うこともできます。

###<a id="libraries"></a>Microsoft Azure Libraries for .NET

この製品には次のツールが含まれています。

* Azure のストレージ、サービス バス、キャッシュ用の NuGet パッケージ。このパッケージは、ユーザーのコンピューターに格納されるため、Visual Studio による新規のクラウド サービス プロジェクトの作成がオフラインでできるようになります。
* Visual Studio プラグインはできる [インロール キャッシュ](http://msdn.microsoft.com/library/dn386103.aspx) プロジェクトの Visual Studio でローカルに実行します。 

###<a id="hdinsight"></a>HDInsight Tools for Visual Studio および Microsoft Hive ODBC ドライバー

サーバー エクスプローラーの HDInsight ツールを使用すると、Hive データベースと HDInsight クラスターにリンクされたストレージ アカウントを移動したり、テーブルを作成したり、Hive クエリを送信できます。 詳細については、次を参照してください。 [Visual Studio の HDInsight Hadoop Tools を使い始める](hdinsight-hadoop-visual-studio-tools-get-started.md)します。

###<a id="mobile">Microsoft Azure Mobile App SDK

操作するためのツール [Azure App Service Mobile Apps](app-service-mobile-value-prop-preview.md)します。

###<a id="ps"></a>Microsoft Azure PowerShell

Azure PowerShell を使用する [Azure 環境の作成とデプロイを自動化](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything)します。

##<a id="notincluded"></a>Azure SDK for .NET のインストールに含まれないもの

SDK をインストールしたときに、Azure 向けアプリケーションの開発に必要なものが含まれない場合もあります。 主なものは次のとおりです。

* [クライアント ライブラリ](http://go.microsoft.com/fwlink/?LinkId=510472)します。 

    Azure SDK には、Azure のサービスを使用するために必要なクライアント ライブラリが含まれています。ただし、SDK をインストールしたときに、そのすべてがインストールされるわけではありません。 アプリケーションでは、SDK によってインストールされない、クライアント ライブラリを必要とする場合をから入手できます [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472)します。 クライアント ライブラリが SDK によってインストールされる場合でも、NuGet.org で現在のバージョンに更新することをお勧めいたします。

    **クライアント ライブラリのローカル コピー。** Azure SDK for .NET は、いくつかの Azure クライアント ライブラリ (ストレージ、サービス バス、キャッシュなど) 用の NuGet パッケージをユーザーのコンピューターにコピーします。 これらのクライアント ライブラリは、新規のクラウド サービス プロジェクトに自動的に含められるため、インターネットに接続していない場合でも、Visual Studio はローカルの NuGet パッケージでプロジェクトを作成できます。 一般に、クライアント ライブラリは SDK よりも頻繁に更新されているため、NuGet.org で入手できるクライアント ライブラリの方が SDK によってインストールされるものよりも新しい場合が多いことに注意してください。 

    **クライアント ライブラリが含まれるプロジェクト テンプレート。** のみ [Azure クラウド サービス](cloud-services-dotnet-get-started.md) と [Azure Mobile Service](mobile-services-dotnet-backend-windows-store-dotnet-leaderboard.md) プロジェクト テンプレートは、いくつかのクライアント ライブラリを自動的に含まれています。 その他のライブラリまたはその他のテンプレートは、インストール、 [クライアント ライブラリの NuGet パッケージ](http://go.microsoft.com/fwlink/?LinkId=510472) 必要があります。

* [Azure のモバイル サービス プロジェクト テンプレート](mobile-services-dotnet-backend-windows-store-dotnet-leaderboard.md)します。

    モバイル サービスのテンプレートは、Visual Studio 2013 Update 2 以降でのみ利用できます。 Azure SDK for .NET をインストールしても、Visual Studio 2012 以前および Visual Studio 2013 Update 1 以前のバージョンでは利用できません。

##<a id="faq"></a>よく寄せられる質問

- [Visual Studio に Azure の機能の多くが含まれているのにもかかわらず、 Azure SDK for .NET をインストールする必要がありますか。](#azinvs)
- [クライアント ライブラリを入手したいのですが、 Azure SDK for .NET をインストールしないと入手できませんか。](#clientlib)
- [以前のバージョンの Azure SDK for .NET はどこにありますか。](#olderversions)
- [Azure SDK for .NET のバージョンのライフサイクル ポリシーを教えてください。](#lifecycle)
- [Azure SDK for .NET との互換性があるゲスト OS を教えてください。](#guestos)
- [Azure SDK for .NET はどのようにアンインストールできますか。](#uninstall)

###<a id="azinvs"></a>Visual Studio に Azure の機能の多くが含まれているのにもかかわらず、 Azure SDK for .NET をインストールする必要がありますか。

最新のツールを使用して Azure 向けの開発をする場合は、SDK をインストールすることをお勧めします。 できれば SDK をインストールしたくない場合、次の条件を満たしていれば、インストールする必要はありません。

* 最新版をインストールした [Visual Studio Update](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5)します。
* 開発の対象が Azure Websites または Mobile Services 向けに限られており、Cloud Services または Virtual Machines 向けの開発ではない。
* アプリケーションがストレージを使用しない、または使用するとしてもストレージ エミュレーターも AzCopy ツールも必要ではない。

###<a id="clientlib"></a>クライアント ライブラリを入手したいのですが、 Azure SDK for .NET をインストールしないと入手できませんか。

SDK でクライアント ライブラリがインストールされるのは、インターネットに接続していなくてもクラウド サービス プロジェクトを作成できるようにする場合だけです。 現在のクライアント ライブラリは NuGet パッケージにで使用できる [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472)します。 詳細については、次を参照してください。 [.NET 用 Azure SDK をインストールするときに含まれないもの](#notincluded) 前述のです。

###<a id="olderversions"></a>以前のバージョンの Azure SDK for .NET はどこにありますか。

以前のバージョンを参照してください、 [Azure SDK for .NET](/downloads/archive-net-downloads/) ダウンロード ページです。 

###<a id="lifecycle"></a>Azure SDK for .NET のバージョンのライフサイクル ポリシーを教えてください。

参照してください [Microsoft Azure Cloud Services のサポート ライフ サイクル ポリシー](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq)します。

###<a id="guestos"></a>Azure SDK for .NET と互換性があるゲスト OS バージョンを教えてください。

参照してください [Azure ゲスト OS リリースと SDK の互換性対応表](http://msdn.microsoft.com/library/ee924680.aspx)します。

###<a id="uninstall"></a>Azure SDK for .NET はどのようにアンインストールできますか。

この記事の下に示すように各項目 [Azure sdk for .NET に含まれるもの](#included) Windows コントロール パネルの [インストールされているプログラムの一覧の個別のプログラムは、 **プログラムと機能**です。  これらを、グループとしてアンインストールする方法はなく、各プログラムは個別にアンインストールする必要があります。

Azure SDK for .NET が既にインストールされている場合に新しいバージョンをインストールしたときは、通常は古いバージョンをアンインストールする必要はありません。 SDK のインストールでは、多くの場合、新しいものを追加し古いものをそのままにするのではなく、既存のプログラムを更新します。 

ただし、以前のバージョンの不要な残留物を削除する場合は、同じプログラムの新しいバージョンが存在する場合のみ、以前のバージョン番号と明記されているプログラムのみをアンインストールします。 たとえば、Microsoft Azure Tools for Microsoft Visual Studio 2013 をバージョン 2.5 から 2.6 に更新した場合、2.5 と 2.6 の両方がある場合に、バージョン 2.5 をアンインストールします。 Microsoft Azure Authoring Tools のバージョン 2.5 のみが表示されている場合は、アンインストールするのは安全ではない場合があります。

示されているプログラムの場合は、そのバージョン番号 **プログラムと機能** 誤解を招くことができます。  たとえば、SDK バージョン 2.6 は、Visual Studio 2013 用の SDK をインストールした場合は、Microsoft Azure Mobile App SDK V1.0 を含み、Visual Studio 2015 用をインストールした場合は、Microsoft Azure Mobile App SDK V2.0 を含みます。この場合のバージョンは、SDK のバージョンではなくプログラムの Visual Studio のバージョンを示しています。

この記事には、Azure SDK の以前のバージョンに含まれていたすべてのプログラムは記載していません。以前の SDK バージョンには、以降のバージョンでは削除されたプログラムを含んでいる場合もあり、以前の SDK バージョンには他にもアンインストールできるプログラムがある場合があります。 たとえば、バージョン 2.5 をインストール"Azure Resource Manager Tools for Visual Studio"がないのでではこの記事の一覧に個別のプログラムとして表示されなく **プログラムと機能**です。  この記事では、Azure SDK for .NET バージョン 2.6 に含まれているプログラムのみを一覧表示しています。  

> **注:** はもインストールされているとは別に他のコンテキストで一部のプログラムは、SDK に含まれると、SDK をすべてを必要としない場合でも、必要な場合があります。 これは、以前のバージョンの SDK によってインストールされた、以降のバージョンの SDK では削除されたプログラムにも該当します。 プログラムをアンインストールする場合には、コンピューターで引き続き必要なものは削除しないように注意する必要があります。



##<a id="versions"></a>バージョン

現在インストールされているバージョンを参照するか、以前のバージョンをダウンロードするを参照してください、 [Azure SDK for .NET のバージョン履歴](/downloads/archive-net-downloads/) ページです。 

##<a id="resources"></a>リソース

現在の Azure SDK for .NET またはクライアント ライブラリをダウンロードするを参照してください、 [Azure のダウンロード ページ](/downloads/)します。

Azure SDK for .NET のソース コード、クライアント ライブラリなどを参照してください。 [GitHub.com/Azure](https://github.com/azure/)します。

Azure クライアント ライブラリのリファレンス ドキュメントを参照してください。 [Azure .NET のリファレンス](/documentation/api/)します。 


