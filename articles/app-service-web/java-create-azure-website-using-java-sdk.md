<properties 
    pageTitle="Azure SDK for Java を使用した Azure App Service での Web アプリの作成" 
    description="Azure SDK for Java を使用してプログラムで Azure App Service の Web アプリを作成する方法について説明します。" 
    tags="azure-classic-portal"
    services="app-service\web" 
    documentationCenter="Java" 
    authors="donntrenton" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/12/2015" 
    ms.author="v-donntr"/>


# Azure SDK for Java を使用した Azure App Service での Web アプリの作成

<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## 概要

このチュートリアルは、作成する Azure SDK for Java アプリケーションをでの Web アプリを作成する方法を示します [Azure App Service][], にアプリケーションを展開します。 次の 2 つの部分から構成されます。

- パート 1 では、Web アプリを作成する Java アプリケーションをビルドする方法を説明します。
- パート 2 では、簡単な JSP の "Hello World" アプリケーションを作成する方法を説明し、FTP クライアントを使用して App Service にコードをデプロイします。


## 前提条件

### ソフトウェアのインストール

この記事の AzureWebDemo アプリケーション コードは、使用してをインストールする Azure Java SDK 0.7.0 を使用して記述されました、 [Web Platform Installer][] (WebPI)。 さらに、確認の最新バージョンを使用して、 [Azure Toolkit for Eclipse][]します。 SDK をインストールした後は、Eclipse プロジェクトの依存関係を更新を実行して、 **インデックスの更新** で **Maven リポジトリ**, 、内の各パッケージの最新バージョンを再度追加、 **の依存関係** ウィンドウです。 クリックすると、Eclipse にインストールされているソフトウェアのバージョンを確認できます **ヘルプ > インストールの詳細**; が必要には、少なくとも次のバージョン。

- Microsoft Azure Libraries for Java 0.7.0.20150309 のパッケージ
- Eclipse IDE for Java EE Developers 4.4.2.20150219


### Azure でクラウド リソースを作成して構成する

この手順を開始する前に、アクティブな Azure サブスクリプションを持ち、Azure で既定の Active Directory (AD) を設定する必要があります。


### Azure で Active Directory (AD) を作成する

Azure サブスクリプションでまだ Active Directory (AD) がない場合にログイン、 [Azure クラシック ポータル][] Microsoft アカウントを使用します。 複数のサブスクリプションの場合はクリックして **サブスクリプション** し、このプロジェクトで使用するサブスクリプションの既定のディレクトリを選択します。 クリックして **適用** そのサブスクリプション ビューに切り替えます。

1. 選択 **Active Directory** 左にあるメニューからです。 **新規のクリック > ディレクトリ > カスタム作成**します。

2.  **ディレクトリの追加**, [ **新しいディレクトリの作成**します。

3.  **名**, 、ディレクトリ名を入力します。

4.  **ドメイン**, 、ドメイン名を入力します。 これは、既定でディレクトリに含まれている基本的なドメイン名で、`<domain_name>.onmicrosoft.com` という形式になっていますです。 ディレクトリ名や自分が所有する別のドメイン名に基づいて名前を付けることができます。 組織が既に使用している別のドメイン名を後から追加することもできます。

5.  **国または地域**, 、ロケールを選択します。

AD の詳細については、次を参照してください。 [Azure AD ディレクトリは][]でしょうか。


### Azure の管理証明書を作成する

Azure SDK for Java は、管理証明書を使用して Azure サブスクリプションを認証します。 これらは、クライアント アプリケーションの認証に使用する X.509 v3 証明書で、サービス管理 API を使用してサブスクリプション所有者に代わって動作し、サブスクリプションのリソースを管理します。

この手順のコードは、自己署名証明書を使用して Azure を認証します。 この手順では、証明書を作成し、それをアップロードする必要があります、 [Azure クラシック ポータル][] 事前します。 これには、次の手順が含まれます。

- クライアント証明書を表す PFX ファイルを生成し、ローカルに保存します。
- PFX ファイルから管理証明書 (CER ファイル) を生成します。
- CER ファイルを Azure サブスクリプションにアップロードします。
- PFX ファイルを JKS に変換します。Java はこの形式を使用して、証明書を使用して認証するためです。
- アプリケーションの認証コードを記述します。これはローカルの JKS ファイルを参照します。

この手順が完了すると、CER 証明書は Azure サブスクリプションに格納され、JKS 証明書はローカル ドライブに格納されます。 管理証明書の詳細については、次を参照してください。 [の作成と Azure の管理証明書をアップロード][]します。


#### 証明書を作成する

自身の自己署名証明書を作成するには、オペレーティング システムでコマンド コンソールを開き、次のコマンドを実行します。

> **注:**  このコマンドを実行するコンピューターは JDK がインストールされている必要があります。 また、keytool へのパスは、JDK をインストールする場所によって異なります。 詳細については、次を参照してください。 [キーと証明書管理ツール (keytool)][] Java オンライン ドキュメントにします。

.pfx ファイルを作成するには:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

.cer ファイルを作成するには:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

各値の説明:

- `<java-install-dir>` は、Java がインストールされているディレクトリへのパスです。
- `<keystore-id>` は、キーストア エントリの識別子です (`AzureRemoteAccess` など)。
- `<cert-store-dir>` は、証明書を格納するディレクトリへのパスです (`C:/Certificates` など)。
- `<cert-file-name>` は、証明書ファイルの名前です (`AzureWebDemoCert` など)。
- `<password>` は、証明書を保護する場合のパスワードです。6 文字以上の長さにする必要があります。 パスワードを入力しなくても続行できますが、推奨しません。
- `<dname>` は、エイリアスに関連付けられる X.500 識別名であり、自己署名証明書の発行者フィールドとサブジェクト フィールドとして使用されます。

詳細については、次を参照してください。 [の作成と Azure の管理証明書をアップロード][]します。


#### 証明書をアップロードする

自己署名証明書を Azure にアップロードするには、 **設定** クラシック ポータルでのページでをクリックし、 **管理証明書** ] タブをクリックします。 をクリックして **アップロード** 、ページの下部で、作成した CER ファイルの場所に移動します。


#### PFX ファイルを JKS に変換する

Windows コマンド プロンプト (管理者として実行) で、証明書を含むディレクトリに移動し、次のコマンドを実行します。`<java-install-dir>` は、Java がインストールされているコンピューターのディレクトリです。

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. メッセージが表示されたら、変換先のキーストア パスワードを入力します。これは、JKS ファイルのパスワードになります。

2. メッセージが表示されたら、ソースのキーストア パスワードを入力します。これは、PFX ファイルに指定したパスワードです。

2 つのパスワードを同じにする必要はありません。 パスワードを入力しなくても続行できますが、推奨しません。


## Web アプリ作成アプリケーションの構築

### Eclipse ワークスペースと Maven プロジェクトを作成する

このセクションでは、AzureWebDemo という名前の Web アプリ作成アプリケーションのワークスペースと Maven プロジェクトを作成します。

1. 新しい Maven プロジェクトを作成します。 クリックして **ファイル > 新規 > Maven プロジェクト**します。  **新しい Maven プロジェクト**, [ **単純なプロジェクトを作成する** と **既定のワークスペースの場所を使用して**します。

2. 2 ページ目に **新しい Maven プロジェクト**, 、以下を指定します。

    - Group ID: `com.<username>.azure.webdemo`
    - Artifact ID: AzureWebDemo
    - Version: 0.0.1-SNAPSHOT
    - Packaging: jar
    - Name: AzureWebDemo

    クリックして **完了**します。

3. プロジェクト エクスプローラーで、新しいプロジェクトの pom.xml ファイルを開きます。 選択、 **の依存関係** ] タブをクリックします。 これは新しいプロジェクトなので、まだパッケージが表示されていません。

4. Maven リポジトリ ビューを開きます。 **[ウィンドウ] をクリックして > ビューを表示 > その他の > Maven > Maven リポジトリ** ] をクリック **OK**します。  **Maven リポジトリ** ビューが IDE の下部に表示されます。

5. 開いている **グローバル リポジトリ**, を右クリックし、 **中央** リポジトリ、および選択 **インデックスの再構築**します。

    ![][1]
    
    この手順は、接続の速度に応じて数分かかる場合があります。 インデックスを再構築時は、Microsoft Azure パッケージが表示されます、 **中央** Maven リポジトリです。

6.  **の依存関係**, 、クリックして **追加**します。  **Enter Group ID...** 入力 `azure-management`します。 基本管理と App Service Web Apps 管理用のパッケージを選択します。

        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites

    > **注:** 後、新しいバージョンの依存関係を更新する場合
    > リリースでは、この一覧内の依存関係の各を再度追加する必要があります。
    > クリックした後 **追加** し、各依存関係を選択し、それが表示されます
    > 新しいバージョン番号、 **の依存関係** ] ボックスの一覧です。

Click **OK**. Azure パッケージには、表示、 **の依存関係** ] ボックスの一覧です。


### Azure SDK を呼び出して Web アプリを作成する Java コードを記述する

次に、Azure SDK for Java で API を呼び出して App Service Web アプリを作成するコードを記述します。

1. Java クラスを作成してメイン エントリ ポイントのコードを格納します。 プロジェクト エクスプ ローラーでプロジェクト ノードを右クリックし、 **新規 > クラス**します。

2.  **New Java Class**, 、クラスに名前を `WebCreator` を確認し、 **public static void main** チェック ボックスをオンします。 選択内容が次のように表示されます。

    ![][2]

3. クリックして **完了**します。 プロジェクト エクスプローラーに WebCreator.java ファイルが表示されます。


### App Service Web アプリを作成する Azure API を呼び出す


#### 必要なインポートを追加する

WebCreator.java で、次のインポートを追加します。これらのインポートは、Azure API を使用する場合に管理ライブラリ内のクラスへのアクセスを提供します。

    // General imports
    import java.net.URI;
    import java.util.ArrayList;
    
    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;
    
    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;
    
    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;
    
    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### メイン エントリ ポイントのクラスを定義する

AzureWebDemo アプリケーションの目的は App Service Web アプリを作成することなので、このアプリケーションのメイン クラスに `WebAppCreator` という名前を付けます。 このクラスは、Azure サービス管理 API を呼び出して Web アプリを作成するメイン エントリ ポイントのコードを提供します。

Web アプリと Web スペースに次のパラメーター定義を追加します。 自身の Azure サブスクリプション ID と証明書情報を提供する必要があります。

    public class WebAppCreator {
    
        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";
    
        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

各値の説明:

- `<subscription-id>` は、リソースを作成する Azure サブスクリプション ID です。
- `<certificate-store-path>` は、ローカルの証明書ストア ディレクトリ内の JKS ファイルへのパスとファイル名です。 たとえば、Linux の場合は `C:/Certificates/CertificateName.jks`、Windows の場合は `C:\Certificates\CertificateName.jks` です。
- `<certificate-password>` は、JKS 証明書を作成したときに指定したパスワードです。
- `webAppName` には任意の名前を選択できます。この手順では、`WebDemoWebApp` という名前を使用します。 完全なドメイン名は、`webAppName` に `domainName` が追加されるため、この場合の完全なドメインは `webdemowebapp.azurewebsites.net` となります。
- `domainName` は、上記のように指定する必要があります。
- `webSpaceName` 定義された値のいずれかを指定する必要があります、 [WebSpaceNames][] クラスです。
- `appServicePlanName` は、上記のように指定する必要があります。

> **注:** たびにこのアプリケーションを実行すると、値を変更する必要があります。 
>  `webAppName` と `appServicePlanName` (または、azure web アプリを削除 
> ポータル アプリケーションを再度実行する前に)。 それ以外の場合、実行されます。 
> Azure で同じリソースが既に存在するために失敗します。


#### Web 作成メソッドを定義する

次に、Web アプリを作成するメソッドを定義します。 このメソッド `createWebApp` は、Web アプリと Web スペースのパラメーターを指定します。 作成し、構成で定義されている App Service Web アプリ管理クライアントがそれも、 [WebSiteManagementClient][] オブジェクトです。 この管理クライアントは、Web アプリ作成の鍵となります。 アプリケーションがサービス管理 API を呼び出すことで Web アプリ (作成、更新、削除などの操作を実行する) を管理できる、RESTful Web サービスを提供します。

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

このコードは、成功または失敗を示す HTTP 状態の応答を出力します。成功した場合は、作成された Web アプリの名前を出力します。


#### main() メソッドを定義する

createWebApp() を呼び出して Web アプリを作成する、main() メソッド コードを提供します。

最後に、`createWebApp` を `main` から呼び出します。

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### アプリケーションを実行して Web アプリの作成を確認する

をクリックしてアプリケーションを実行することを確認するには、 **実行 > 実行**します。 アプリケーションが実行を完了すると、Eclipse コンソールに次の出力が表示されます。

    ----------
    Web app created - HTTP response 200
    
    ----------
    
    Name of web app created: WebDemoWebApp
    
    ----------

Azure 旧ポータルにログインし、クリックして **Web Apps**します。 数分以内に、新しい Web アプリが Web アプリの一覧に表示されます。


## Web アプリへのアプリケーションのデプロイ

AzureWebDemo を実行して、旧ポータルにログインして新しい web アプリの作成] をクリックした後 **Web Apps**, を選択して **WebDemoWebApp** で、 **Web Apps** ] ボックスの一覧です。 Web アプリのダッシュ ボード] ページで、クリックして **参照** (または、URL をクリックして `webdemowebapp.azurewebsites.net`)、移動します。 コンテンツがまだ Web アプリに発行されていないため、空のプレースホルダー ページが表示されます。

次に、"Hello World" アプリケーションを作成し、これを Web アプリにデプロイします。


### JSP Hello World アプリケーションを作成する

#### アプリケーションを作成する

アプリケーションを Web にデプロイする方法を説明するために、以下の手順では、簡単な "Hello World" Java アプリケーションを作成し、これをアプリケーションを作成した App Service Web アプリにアップロードする方法を示しています。

1. クリックして **ファイル > 新規 > 動的 Web プロジェクト**します。 これに `JSPHello` という名前を付けます。 このダイアログ ボックスの他の設定を変更する必要はありません。 クリックして **完了**します。

    ![][3]

2. プロジェクト エクスプ ローラーで、 **JSPHello** プロジェクトを右クリックして **WebContent**, 、] をクリックし、 **新規 > JSP ファイル**します。 [New JSP File] ダイアログ ボックスで、新しいファイルに `index.jsp` という名前を付けます。 クリックして **次**します。

3.  **Select JSP Template** ] ダイアログ ボックスで選択 **New JSP File (html)** ] をクリック **完了**します。

4. Index.jsp で、`<head>` タグと `<body>` タグのセクションに次のコードを追加します。

        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
    
        <body>
          Hello, the time is <%= date %> 
        </body>


#### Hello World アプリケーションを localhost で実行する

このアプリケーションを実行する前に、いくつかのプロパティを構成する必要があります。

1. 右クリックし、 **JSPHello** プロジェクトし、選択 **プロパティ**します。

2.  **プロパティ** ] ダイアログ: 選択 **Java Build Path**, を選択、 **を注文し、エクスポート** ] タブで、チェック **JRE システム ライブラリ**, 、] をクリックし、 **を** 一覧の先頭に移動します。

    ![][4]

3. また、 **プロパティ** ダイアログ: 選択 **Targeted Runtimes** ] をクリック **新規**します。

4.  **New Server Runtime Environment** ダイアログ ボックスなどのサーバーを選択 **Apache Tomcat v7.0** ] をクリック **次**します。  **Tomcat サーバー** ダイアログ ボックスで、セット **名** に `Apache Tomcat v7.0`, 、設定と **Tomcat Installation Directory** を使用する Tomcat サーバーのバージョンがインストールされているディレクトリにします。

    ![][5]

    クリックして **完了**します。

5. 戻って、 **Targeted Runtimes** のページ、 **プロパティ** ダイアログ。 選択 **Apache Tomcat v7.0**, 、] をクリックし、 **OK**します。

    ![][6]

6. Eclipse **実行** ] メニューのをクリックして **実行**します。  **実行** ダイアログで、 **サーバー上で実行**します。  **サーバー上で実行** ダイアログで、 **Tomcat v7.0 Server**:

    ![][7]

    クリックして **完了**します。

7. アプリケーションを実行すると表示されます、 **JSPHello** ページが Eclipse の localhost ウィンドウに表示されます (`http://localhost:8080/JSPHello/`)、次のメッセージを表示します。

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### アプリケーションを WAR としてエクスポートする

Web プロジェクト ファイルを Web アーカイブ (WAR) ファイルとしてエクスポートすると、これを Web アプリにデプロイできます。 次の Web プロジェクト ファイルは、WebContent フォルダーに存在します。

    META-INF
    WEB-INF
    index.jsp

1. WebContent フォルダーを右クリックして **エクスポート**します。

2.  **Export Select** ダイアログ ボックスで、をクリックして **Web > WAR** ファイルをクリック **次**します。

3.  **WAR Export** ダイアログ ボックスで、現在のプロジェクトの src ディレクトリを選択し、末尾に WAR ファイルの名前を含めます。 次に例を示します。

    `<project-path>/JSPHello/src/JSPHello.war`

WAR ファイルを展開する方法の詳細については、次を参照してください。 [Java アプリケーションを Azure App Service Web Apps を追加](web-sites-java-add-app.md)します。


### FTP を使用して Hello World アプリケーションをデプロイする

サードパーティの FTP クライアントを選択してアプリケーションを発行します。 この手順では、2 つのオプション (Azure に組み込まれている Kudu コンソールと、グラフィカル UI を備えた使いやすい一般的なツール FileZilla) について説明します。

> **注:** Azure Toolkit for Eclipse は、ストレージ アカウントへの展開をサポートしているし、クラウド サービスしますが、現在の web アプリへの展開をサポートしていません。 ストレージ アカウントを展開し、クラウド サービス」の説明に従って、Azure デプロイ プロジェクトを使用して [Eclipse で Azure の Hello World アプリケーションを作成する](http://msdn.microsoft.com/library/azure/hh690944.aspx), が web アプリには。 FTP や GitHub などの他の方法を使用して、Web アプリにファイルを転送します。

> **注:** お勧めしません (コマンドラインの FTP.EXE ユーティリティ Windows に付属している) Windows のコマンド プロンプトから FTP を使用します。 FTP.EXE などのアクティブ FTP を使用する FTP クライアントは、多くの場合、ファイアウォールでの動作に失敗します。 アクティブ FTP は LAN ベースの内部アドレスを指定するため、FTP サーバーは接続に失敗する可能性があります。

FTP を使用した App Service Web アプリへのデプロイの詳細については、次のトピックをご覧ください。

- [FTP ユーティリティを使用したデプロイ](web-sites-deploy.md)


#### [デプロイ資格情報を設定する] を選択して構成できます

実行するかどうかを確認、 **AzureWebDemo** アプリケーションを web アプリを作成します。 ファイルはこの場所に転送されます。

1. 旧ポータルにログインし、クリックして **Web Apps**します。 確認 **WebDemoWebApp** web アプリの一覧に表示され、実行されていることを確認します。 クリックして **WebDemoWebApp** を開くには、 **ダッシュ ボード** ページです。

2.  **ダッシュ ボード** ] ページで、[ **[概要**, 、] をクリックして **デプロイ資格情報を設定** (既にデプロイ資格情報がある場合は、これを読み取り **展開資格情報のリセット**)。

    デプロイ資格情報は、Microsoft アカウントと関連付けられています。 Git と FTP を使用してデプロイする際のユーザー名パスワードを指定する必要があります。 これらの資格情報を使用すると、Microsoft アカウントに関連付けられているすべての Azure サブスクリプション内の任意の Web アプリにデプロイできます。 Git と FTP のデプロイ資格情報をダイアログ ボックスに入力し、後で使用するためにパスワードとユーザー名を記録します。


#### FTP の接続情報を取得する

FTP を使用して、新しく作成された Web アプリにアプリケーション ファイルをデプロイするには、接続情報を取得する必要があります。 接続情報を取得するには、2 つの方法があります。 Web アプリのアクセス方法の 1 つは **ダッシュ ボード** ページは、他の方法は、web をダウンロードするアプリの発行プロファイル。 発行プロファイルは、FTP ホスト名やログオン資格情報などの情報を Azure App Service の Web アプリに提供する XML ファイルです。 このユーザー名とパスワードを使用すると、この Web アプリだけでなく、Azure アカウントに関連付けられているすべてのサブスクリプション内の任意の Web アプリにデプロイできます。

Web アプリのブレードから FTP の接続情報を取得する、 [Azure ポータル][]:

1.  **Essentials**, を検索し、コピー、 **FTP ホスト名**します。 これは、`ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net` のような URI です。

2.  **Essentials**, を検索し、コピー、 **FTP/デプロイ ユーザー名**します。 これには、フォームが *webappname\deployment username*。 たとえば `WebDemoWebApp\deployer77`です。

発行プロファイルから FTP 接続情報を取得するには

1. Web アプリのブレードで、[ **発行プロファイルを取得**します。 .publishsettings ファイルがローカル ドライブにダウンロードされます。

2. XML エディターまたはテキスト エディターで .publishsettings ファイルを開き、`publishMethod="FTP"` を含む `<publishProfile>` 要素を検索します。 この要素は次のようになります。

        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>

3. Web アプリの `publishProfile` 設定は、次のように FileZilla Site Manager 設定にマップされることに注意してください。

- `publishUrl` 同じ **FTP ホスト名**, に設定された値 **ホスト**します。
- `publishMethod="FTP"` 設定することを意味 **プロトコル** に **FTP - File Transfer Protocol**, 、および **暗号化** に **Use plain FTP**します。
- `userName` と `userPWD` は、デプロイ資格情報をリセットするときに指定された、実際のユーザー名とパスワードの値のキーです。 `userName` 同じ **展開/FTP ユーザー**します。 それらの割り当て先 **ユーザー** と **パスワード** filezilla です。
- `ftpPassiveMode="True"` FTP サイトがパッシブ FTP 転送を使用しています。選択 **パッシブ** 上、 **設定の転送** ] タブをクリックします。


#### Java アプリケーションをホストする Web アプリを構成する

アプリケーションを発行する前に、Web アプリが Java アプリケーションをホストできるように、いくつかの構成設定を変更する必要があります。

1. 従来のポータルで web アプリに移動 **ダッシュ ボード** ページを **構成**します。  **構成** ] ページで、次の設定を指定します。

2.  **Java バージョン** 既定値は **オフ**; Java バージョンを選択して、アプリケーションがターゲット例: 1.7.0_51 です。 これを行うことも確認 **Web コンテナー** が Tomcat サーバーのバージョンに設定します。

3.  **既定のドキュメント**, 、index.jsp を追加し、一覧の一番上に移動します。 (Web アプリの既定のファイルは hostingstart.html です)。

4. クリックして **保存**します。


#### Kudu を使用してアプリケーションを発行する

アプリケーションを発行する 1 つの方法は、Azure に組み込まれている Kudu デバッグ コンソールを使用することです。 Kudu は安定性があり、App Service Web Apps や Tomcat サーバーと一貫性があることが知られています。 ブラウザーから次の形式の URL を参照して、Web アプリのコンソールにアクセスします。

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. この手順では、Kudu コンソールは次の URL にあります。この場所にアクセスしてください。

    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`

2. 上部のメニューから選択 **デバッグ コンソール > CMD**します。

3. コンソールのコマンド ラインで、`/site/wwwroot` に移動します (または、ページの上部にあるディレクトリ ビューで、`site`、`wwwroot` の順にクリックします)。

    `cd /site/wwwroot`

4. 指定した後 **Java バージョン**, 、Tomcat サーバーで webapps ディレクトリを作成する必要があります。 コンソールのコマンド ラインで、webapps ディレクトリに移動します。

    `mkdir webapps`

    `cd webapps`

5. JSPHello.war を `<project-path>/JSPHello/src/` からドラッグし、`/site/wwwroot/webapps` の下の Kudu ディレクトリ ビューにドロップします。 [Drag here to upload and zip] 領域にはドラッグしないでください。Tomcat がファイルを解凍します。

  ![][8]

最初に、JSPHello.war が単独でディレクトリ領域に表示されます。

  ![][9]

短時間 (おそらく 5 分未満) で、Tomcat サーバーはアンパックされた JSPHello ディレクトリに WAR ファイルを解凍します。 ルート ディレクトリをクリックして、Index.jsp が解凍され、コピーされていることを確認します。 確認したら、webapps ディレクトリに戻り、アンパックされた JSPHello ディレクトリが作成されていることを確認します。 これらの項目が表示されない場合は、少し待ってから繰り返します。

  ![][10]


#### FileZilla を使用してアプリケーションを発行する (省略可能)

アプリケーションの発行に使用できるもう 1 つのツールは、FileZilla です。これは、サード パーティの一般的な FTP クライアントで、使いやすいグラフィカル UI を備えています。 ダウンロードしてインストールから FileZilla を [http://filezilla-project.org/](http://filezilla-project.org/) 場合は、既にがあるないことです。 クライアントの使用に関する詳細については、次を参照してください。、 [FileZilla ドキュメント](https://wiki.filezilla-project.org/Documentation) とこのブログ エントリ [FTP クライアント - パート 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx)します。

1. FileZilla で、次のようにクリックします。 **ファイル > サイト マネージャー**します。
2.  **サイト マネージャー** ダイアログ ボックスで、をクリックして **新しいサイト**します。 新しい空の FTP サイトが表示され **[Select Entry** 名を指定するよう求められます。 この手順では、`AzureWebDemo-FTP` という名前を付けます。

     **全般** ] タブで、次の設定を指定します。
    - **ホスト:** Enter、 **FTP ホスト名** ダッシュ ボードからコピーします。
    - **ポート:** (空白のままに、これは、パッシブ転送と、サーバーが使用するポートを決定します)。
    - **プロトコル:** FTP ファイル転送プロトコル
    - **暗号化:** Use plain FTP
    - **ログオンの種類:** -標準
    - **ユーザー:** 展開の入力/FTP ユーザー ダッシュ ボードからコピーしました。 これは、完全な FTP ユーザー名であり、その形式 *webappname\username*します。
    - **パスワード:** デプロイ資格情報を設定したときに指定したパスワードを入力します。

     **設定の転送** ] タブで [ **パッシブ**します。

3. クリックして **接続**します。 成功すると、FileZilla のコンソールに `Status: Connected` というメッセージが表示され、ディレクトリの内容を一覧表示する `LIST` コマンドが実行されます。

4.  **ローカル** サイト パネルで、JSPHello.war ファイルが存在は、パスは次のようになりますされるソース ディレクトリを選択します。

    `<project-path>/JSPHello/src/`

5.  **リモート** サイト パネルで目的のフォルダーを選択します。 WAR ファイルを Web アプリのルートの下の `webapps` ディレクトリにデプロイします。 移動 `/site/wwwroot`, を右クリックして `wwwroot`, を選択して **ディレクトリを作成**します。 ディレクトリに `webapps` という名前を付け、そのディレクトリに入力します。

6. JSPHello.war を `/site/wwwroot/webapps` に転送します。 JSPHello.war を選択、 **ローカル** 、ファイル リストを右クリックして選択 **アップロード**します。 ファイルが `/site/wwwroot/webapps` に表示されるはずです。

7. JSPHello.war を webapps ディレクトリにコピーすると、Tomcat サーバーは WAR ファイル内のファイルを自動的にアンパック (解凍) します。 Tomcat サーバーはほぼ瞬時にアンパックを開始しますが、ファイルが FTP クライアントに表示されるまで長い時間 (場合によっては数時間) がかかる可能性があります。


#### Hello World アプリケーションを Web アプリで実行する

1. WAR ファイルがアップロードされ、Tomcat サーバーがアンパックされた `JSPHello` ディレクトリを作成したことを確認したら、ブラウザーで `http://webdemowebapp.azurewebsites.net/JSPHello` にアクセスしてアプリケーションを実行します。

    > **注:** ] をクリックすると **参照** 従来のポータルからする可能性があります
    "この Java ベースの web アプリケーションはと答えると、既定の web を取得します。
    正常に作成されました。" Web ページを更新する必要があります。
    注文を表示する既定の web ページではなく、アプリケーションを出力します。

2. アプリケーションを実行すると、Web ページに次の出力が表示されます。

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### Azure リソースをクリーンアップする

この手順では、App Service Web アプリを作成します。 Web アプリが存在する限りリソースに課金されます。 テストや開発のために Web アプリを引き続き使用する予定がない場合は、停止や削除を検討する必要があります。 Web アプリを停止する場合は、少額の料金が発生しますが、いつでも再開できます。 Web アプリを削除する場合は、アップロードされているすべてのデータが消去されます。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

  [1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
  [2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
  [3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
  [4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
  [5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
  [6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
  [7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
  [8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
  [9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
  [10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png
 

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web Platform Installer]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure classic portal]: https://manage.windowsazure.com
[What is an Azure AD directory]: http://technet.microsoft.com/library/jj573650.aspx
[Create and Upload a Management Certificate for Azure]: http://msdn.microsoft.com/library/azure/gg551722.aspx
[Key and Certificate Management Tool (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure Portal]: https://portal.azure.com

