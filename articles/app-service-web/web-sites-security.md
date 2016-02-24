<properties
    pageTitle="Azure App Service での Web アプリのセキュリティ保護"
    description="Azure の Web アプリをセキュリティで保護する方法について説明します。"
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="09/16/2015"
    ms.author="cephalin"/>


#Azure App Service での Web アプリのセキュリティ保護

Web アプリの開発における課題の 1 つが、セキュリティで保護された安全なサービスを顧客に提供する方法です。 この記事では、機能について説明 [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) web アプリをセキュリティで保護することができます。

> [AZURE.NOTE] Web ベース アプリケーション用のセキュリティに関する考慮事項の詳細についてでは、このドキュメントの範囲外です。 さらなるガイダンスを web アプリケーションをセキュリティで保護するための出発点として、次を参照してください。、 [Open Web Application Security Project (OWASP)]( https://www.owasp.org/index.php/Main_Page), 、特に、 [上位 10 個のプロジェクト。](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), 、OWASP メンバーによって判別されたは、リスクが上位 10 個重要な web アプリケーションの現在のセキュリティが記載されています。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

##<a name="https"></a> 保護された通信

使用する場合、***. azurewebsites.net** web アプリに対して作成されるドメイン名すべての SSL 証明書が提供されるのですぐに HTTPS を使用できます ***. azurewebsites.net** ドメイン名です。 サイトで使用する場合、 [カスタム ドメイン名](web-sites-custom-domain-name.md), 、SSL 証明書をアップロードする [HTTPS を有効にする](web-sites-configure-ssl-certificate.md) 、カスタム ドメインに対してです。

##<a name="develop"></a> 保護された開発

### プロファイルの発行と発行の設定

アプリケーションを開発する場合は、管理タスクの実行またはなどのユーティリティを使用してタスクの自動化 **Visual Studio**, 、**Web Matrix**, 、**Azure PowerShell** または **Azure コマンド ライン インターフェイス (Azure CLI)**, を使用するか、 *発行の設定* ファイルまたは *発行プロファイル*します。 どちらも Azure に対する認証に使用されるため、許可されていないアクセスを防ぐためにセキュリティで保護される必要があります。

* A **発行設定** ファイルが含まれます

    * Azure サブスクリプション ID

    * サブスクリプションの管理タスクを実行することを許可する管理証明書 *アカウント名やパスワードを提供することがなく*です。

* A **発行プロファイル** ファイルが含まれます

    * Web アプリへ発行するための情報

発行設定または発行プロファイルを使用するユーティリティを使用する場合は、ユーティリティに設定または発行プロファイルを含むファイルをインポートし、 **削除** ファイルです。 場合など、プロジェクトに携わる他のユーザーと共有する、ファイルを保持する必要がありますに保管して、安全な場所など、 **暗号化された** ディレクトリにアクセス許可が制限されます。

さらに、インポートされた資格情報が保護されていることを確認する必要があります。 たとえば、 **Azure PowerShell** と **Azure コマンド ライン インターフェイス (Azure CLI)** インポートされた情報を両方とも格納、 **ホーム ディレクトリ** (*~* Linux または OS X システムでと */ユーザー/yourusername* Windows システムでします)。さらにセキュリティをすることもできます **暗号化** 、オペレーティング システムの暗号化ツールを使用してこれらの場所。

### 構成設定と接続文字列
接続文字列、認証資格情報、およびその他の機密情報は構成ファイルに保存するのが一般的な方法です。 残念ながら、これらのファイルは Web サイト上で公開される場合やパブリック リポジトリにチェックインされる場合があり、結果としてこの情報が危険にさらされる可能性があります。

Azure App Service では、Web Apps ランタイム環境の一部として構成情報を格納できます。 **アプリ設定** と **接続文字列**します。 介して実行時にアプリケーションに、値が公開されます *環境変数* 大半のプログラミング言語です。 .NET アプリケーションの場合、これらの値は実行時に .NET 構成に挿入されます。

**アプリケーション設定** と **接続文字列** は構成可能なを使用して、 [Azure ポータル](http://portal.azure.com) または PowerShell や Azure CLI などのユーティリティです。

アプリケーション設定と接続文字列の詳細については、次を参照してください。 [web アプリの構成](web-sites-configure.md)します。

### FTPS

Azure にはセキュリティで保護された FTP アクセスをファイル システムを使用して web アプリの **FTPS**します。 これによって、Web アプリ上のアプリケーション コードや診断ログに安全にアクセスできます。 Web アプリの FTPS リンクは、次の手順で見つけることができます。

1. 開いている、 [Azure ポータル](http://portal.azure.com)します。
2. 選択 **[すべて参照**します。
3.  **参照** ブレードで、 **Web Apps**します。
4.  **Web Apps** ブレードで、目的の web アプリを選択します。
5. Web アプリのブレードで、次のように選択します。 **設定をすべて**です。
6.  **設定** ブレードで、 **プロパティ**します。
7. FTP と FTPS リンクがあり、 **設定** ブレードです。 

FTPS の詳細については、次を参照してください。 [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol)します。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 次のステップ

Azure のプラットフォームでは、レポートに関する情報のセキュリティについて、 **セキュリティ インシデントまたは迷惑行為**, 、またはを実行する Microsoft に通知する **侵入テスト** 、サイトの「セキュリティ」を参照してください、 [Microsoft Azure トラスト センター](http://azure.microsoft.com/support/trust-center/security/)します。

詳細については **web.config** または **applicationhost.config** web アプリでファイルを参照してください [Azure App Service web apps でロック解除される構成オプション](http://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/)します。

可能性のある攻撃の検出に役立つ、web アプリのログ情報の詳細については、次を参照してください。 [診断ログの記録を有効にする](web-sites-enable-diagnostic-log.md)です。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

