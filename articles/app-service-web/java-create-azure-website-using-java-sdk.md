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

## 概要

このチュートリアルは、作成する Azure SDK for Java アプリケーションをでの Web アプリを作成する方法を示します [Azure App Service の][], にアプリケーションを展開します。 次の 2 つの部分から構成されます。

- パート 1 では、Web アプリを作成する Java アプリケーションをビルドする方法を説明します。
- パート 2 では、簡単な JSP の "Hello World" アプリケーションを作成する方法を説明し、FTP クライアントを使用して App Service にコードをデプロイします。


## 前提条件

### ソフトウェアのインストール

この記事の AzureWebDemo アプリケーション コードは、使用してをインストールする Azure Java SDK 0.7.0 を使用して記述されました、 [Web Platform Installer][] (WebPI)。 さらに、確認の最新バージョンを使用して、 [Azure Toolkit for Eclipse の][]します。 SDK をインストールしたら、**Maven リポジトリ**で**インデックスの更新**を実行して、Eclipse プロジェクトの依存関係を更新します。次に、**[依存関係]** ウィンドウで、各パッケージの最新バージョンを再度追加します。 **[ヘルプ] > [インストールの詳細]** の順にクリックして、Eclipse にインストールされているソフトウェアのバージョンを確認できます。少なくとも次のバージョンがインストールされている必要があります。

- Microsoft Azure Libraries for Java 0.7.0.20150309 のパッケージ
- Eclipse IDE for Java EE Developers 4.4.2.20150219


### Azure でクラウド リソースを作成して構成する

この手順を開始する前に、アクティブな Azure サブスクリプションを持ち、Azure で既定の Active Directory (AD) を設定する必要があります。


### Azure で Active Directory (AD) を作成する

Azure サブスクリプションでまだ Active Directory (AD) がない場合にログイン、 [Azure クラシック ポータルの [][] Microsoft アカウントを使用します。 複数のサブスクリプションを持っている場合は、**[サブスクリプション]** をクリックし、このプロジェクトで使用するサブスクリプションの既定のディレクトリを選択します。 **[適用]** をクリックしてそのサブスクリプション ビューに切り替えます。

1. 左側のメニューから **[Active Directory]** を選択します。 **[新規] > [ディレクトリ] > [カスタム作成]** の順にクリックします。

2. **[ディレクトリの追加]** で、**[新しいディレクトリの作成]** を選択します。

3. **[名前]** にディレクトリ名を入力します。

4. **[ドメイン]** にドメイン名を入力します。既定ではディレクトリに含まれている基本的なドメイン名です。書式を `< domain_name >. onmicrosoft.com`します。ディレクトリ名や自分が所有する別のドメイン名に基づいて名前を付けることができます。組織が既に使用している別のドメイン名を後から追加することもできます。

5. **[国またはリージョン]** でロケールを選択します。

AD の詳細については、次を参照してください。 [は Azure AD ディレクトリの []][]でしょうか。


### Azure の管理証明書を作成する

Azure SDK for Java は、管理証明書を使用して Azure サブスクリプションを認証します。 これらは、クライアント アプリケーションの認証に使用する X.509 v3 証明書で、サービス管理 API を使用してサブスクリプション所有者に代わって動作し、サブスクリプションのリソースを管理します。

この手順のコードは、自己署名証明書を使用して Azure を認証します。 この手順では、証明書を作成し、それをアップロードする必要があります、 [Azure クラシック ポータルの [][] 事前します。 これには、次の手順が含まれます。

- クライアント証明書を表す PFX ファイルを生成し、ローカルに保存します。
- PFX ファイルから管理証明書 (CER ファイル) を生成します。
- CER ファイルを Azure サブスクリプションにアップロードします。
- PFX ファイルを JKS に変換します。Java はこの形式を使用して、証明書を使用して認証するためです。
- アプリケーションの認証コードを記述します。これはローカルの JKS ファイルを参照します。

この手順が完了すると、CER 証明書は Azure サブスクリプションに格納され、JKS 証明書はローカル ドライブに格納されます。 管理証明書の詳細については、次を参照してください。 [を作成し、[] を Azure の管理証明書をアップロード][]します。


#### 証明書を作成する

自身の自己署名証明書を作成するには、オペレーティング システムでコマンド コンソールを開き、次のコマンドを実行します。

> **注:**  このコマンドを実行するコンピューターは JDK がインストールされている必要があります。 また、keytool へのパスは、JDK をインストールする場所によって異なります。 詳細については、次を参照してください。 [キーと証明書管理ツール (keytool) []][] Java オンライン ドキュメントにします。

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

- `< java インストール ディレクトリ >` Java がインストールされているディレクトリへのパスです。
- `< キーストア id >` キーストア エントリ識別子を指定します (たとえば、 `AzureRemoteAccess`)。
- `< 証明書ストア ディレクトリ >` 証明書を格納するディレクトリへのパス (たとえば `c:/証明書`)。
- `< 証明書ファイル名 >` 証明書ファイルの名前を指定します (たとえば `AzureWebDemoCert`)。
- `< パスワード >` パスワードは、証明書を保護する; には、少なくとも 6 文字の長さをする必要があります。パスワードを入力しなくても続行できますが、推奨しません。
- `< dname >` エイリアスに関連付けられる X.500 の識別名は、自己署名証明書の発行者とサブジェクト フィールドとして使用されます。

詳細については、次を参照してください。 [を作成し、[] を Azure の管理証明書をアップロード][]します。


#### 証明書をアップロードする

自己署名証明書を Azure にアップロードするには、旧ポータルの **[設定]** ページに進み、**[管理証明書]** タブをクリックします。 ページの下部にある **[アップロード]** をクリックし、作成した CER ファイルの場所に移動します。


#### PFX ファイルを JKS に変換する

(管理者として実行)、Windows コマンド プロンプトで、証明書が含まれていて、次のコマンドを実行ディレクトリに移動、 `< java インストール ディレクトリ >` お使いのコンピューターで Java をインストールしたディレクトリには。

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

1. 新しい Maven プロジェクトを作成します。 **[ファイル] > [新規作成] > [Maven プロジェクト]** の順にクリックします。 **[New Maven Project]** で、**[Create a simple project]** と **[Use default workspace location]** を選択します。

2. **[New Maven Project]** の 2 ページ目で、次にように指定します。

    - グループ ID: `com. < ユーザー名 >. azure.webdemo`
    - Artifact ID: AzureWebDemo
    - Version: 0.0.1-SNAPSHOT
    - Packaging: jar
    - Name: AzureWebDemo

    **[完了]** をクリックします。

3. プロジェクト エクスプローラーで、新しいプロジェクトの pom.xml ファイルを開きます。 **[依存関係]** タブを選択します。 これは新しいプロジェクトなので、まだパッケージが表示されていません。

4. Maven リポジトリ ビューを開きます。 **[Window] > [ビューを表示] > [その他] > [Maven] > [Maven リポジトリ]** の順にクリックし、**[OK]** をクリックします。 **[Maven リポジトリ]** ビューが IDE の下部に表示されます。

5. **[グローバル リポジトリ]** を開き、**[中央]** リポジトリを右クリックして **[インデックスの再構築]** を選択します。

    ![][1]

    この手順は、接続の速度に応じて数分かかる場合があります。 インデックスを再構築するときに、**[中央]** の Maven リポジトリに Microsoft Azure パッケージが表示されます。

6. **[依存関係]** で、**[追加]** をクリックします。  **Enter Group ID...** 入力 `azure 管理`します。 基本管理と App Service Web Apps 管理用のパッケージを選択します。

        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites

   > **注:** 後、新しいバージョンの依存関係を更新する場合
   > リリースでは、この一覧内の依存関係の各を再度追加する必要があります。
   > クリックした後 **追加** し、各依存関係を選択し、それが表示されます
   > 新しいバージョン番号、 **の依存関係** ] ボックスの一覧です。

**[OK]** をクリックします。 次に、Azure パッケージが **[依存関係]** の一覧に表示されます。


### Azure SDK を呼び出して Web アプリを作成する Java コードを記述する

次に、Azure SDK for Java で API を呼び出して App Service Web アプリを作成するコードを記述します。

1. Java クラスを作成してメイン エントリ ポイントのコードを格納します。 プロジェクト エクスプローラーで、プロジェクト ノードを右クリックして **[新規] > [クラス]** の順に選択します。

2. **New Java Class**, 、クラスに名前を `WebCreator` を確認し、 **public static void main** チェック ボックスをオンします。 選択内容が次のように表示されます。

    ![][2]

3. **[完了]** をクリックします。 プロジェクト エクスプローラーに WebCreator.java ファイルが表示されます。


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

AzureWebDemo アプリケーションの目的を App Service Web アプリを作成するためこのアプリケーションのメイン クラスに名前を `WebAppCreator`します。 このクラスは、Azure サービス管理 API を呼び出して Web アプリを作成するメイン エントリ ポイントのコードを提供します。

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

- `< サブスクリプション id >` リソースを作成する Azure サブスクリプション id を指定します。
- `< 証明書ストア パス >` はパスと、ローカルの証明書ストア ディレクトリ内の JKS ファイルへのファイル名。たとえば、 `C:/Certificates/CertificateName.jks` Linux 用および `C:\Certificates\CertificateName.jks` Windows 用です。
- `< 証明書パスワード >` 、JKS 証明書を作成したときに指定したパスワードを指定します。
- `webAppName` を選択する任意の名前を指定できます。 ここでは、名前 `WebDemoWebApp`します。 完全なドメイン名は、 `webAppName` で、 `domainName` 追加され、したがって、この場合、完全なドメインは、 `webdemowebapp.azurewebsites.net`します。
- `domainName` 上記のように指定する必要があります。
- `webSpaceName` で定義された値のいずれかを指定する必要があります、 [WebSpaceNames:operator[]][] クラスです。
- `appServicePlanName` 上記のように指定する必要があります。

> **注:** たびにこのアプリケーションを実行すると、値を変更する必要があります。 
> `webAppName` と `appServicePlanName` (または、azure web アプリを削除 
> ポータル アプリケーションを再度実行する前に)。 それ以外の場合、実行されます。 
> Azure で同じリソースが既に存在するために失敗します。


#### Web 作成メソッドを定義する

次に、Web アプリを作成するメソッドを定義します。 このメソッドは、 `createWebApp`, 、web アプリと web スペースのパラメーターを指定します。 作成し、構成で定義されている App Service Web アプリ管理クライアントがそれも、 [WebSiteManagementClient:operator[]][] オブジェクトです。 この管理クライアントは、Web アプリ作成の鍵となります。 アプリケーションがサービス管理 API を呼び出すことで Web アプリ (作成、更新、削除などの操作を実行する) を管理できる、RESTful Web サービスを提供します。

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

最後に、呼び出す `createWebApp` から `メイン`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {
    
            // Create web app
            createWebApp();
    
        }  // end of main()
    
    }  // end of WebAppCreator class

#### アプリケーションを実行して Web アプリの作成を確認する

アプリケーションの実行を確認するには、**[実行] > [実行]** の順にクリックします。 アプリケーションが実行を完了すると、Eclipse コンソールに次の出力が表示されます。

    ----------
    Web app created - HTTP response 200
    
    ----------
    
    Name of web app created: WebDemoWebApp
    
    ----------

Azure クラシック ポータルにログインし、**[Web Apps]** をクリックします。 数分以内に、新しい Web アプリが Web アプリの一覧に表示されます。


## Web アプリへのアプリケーションのデプロイ

AzureWebDemo を実行して新しい Web アプリを作成したら、旧ポータルにログインし、**[Web Apps]** をクリックして、**[Web アプリ]** の一覧から **[WebDemoWebApp]** を選択します。 Web アプリのダッシュ ボード] ページで、クリックして **参照** (または、URL をクリックして `webdemowebapp.azurewebsites.net`)、移動します。 コンテンツがまだ Web アプリに発行されていないため、空のプレースホルダー ページが表示されます。

次に、"Hello World" アプリケーションを作成し、これを Web アプリにデプロイします。


### JSP Hello World アプリケーションを作成する

#### アプリケーションを作成する

アプリケーションを Web にデプロイする方法を説明するために、以下の手順では、簡単な "Hello World" Java アプリケーションを作成し、これをアプリケーションを作成した App Service Web アプリにアップロードする方法を示しています。

1. **[File] > [New] > [Dynamic Web Project]** の順にクリックします。 名前を付けます `JSPHello`します。 このダイアログ ボックスの他の設定を変更する必要はありません。 **[完了]** をクリックします。

    ![][3]

2. Project Explorer で、**[JSPHello]** プロジェクトを展開し、**[WebContent]** を右クリックして **[New] > [JSP File]** の順にクリックします。 新しいファイルの名前を [New JSP File] ダイアログ ボックスで、 `index.jsp`します。 **[次へ]** をクリックします。

3. **[Select JSP Template]** ダイアログで、**[New JSP File (html)]** を選択し、**[Finish]** をクリックします。

4. Index.jsp で、次のコードを追加、 `< head >` と `< body >` タグのセクション。

     <head>
       ...
       java.util.Date date = new java.util.Date();
     </head>
    
     <body>
       Hello, the time is <%= date %> 
     </body>



#### Hello World アプリケーションを localhost で実行する

このアプリケーションを実行する前に、いくつかのプロパティを構成する必要があります。

1. **[JSPHello]** プロジェクトを右クリックし、**[Properties]** を選択します。

2. **[Properties]** ダイアログ ボックスで、**[Java Build Path]** を選択し、**[Order and Export]** タブを選択します。次に、**[JRE System Library]** チェック ボックスをオンにし、**[Up]** をクリックして一覧の先頭に移動します。

    ![][4]

3. さらに、**[Properties]** ダイアログ ボックスで、**[Targeted Runtimes]** を選択し、**[New]** をクリックします。

4. **[New Server Runtime Environment]** ダイアログ ボックスで、**Apache Tomcat v7.0** などのサーバーを選択し、**[Next]** をクリックします。  **Tomcat サーバー** ダイアログ ボックスで、セット **名** に `Apache Tomcat v7.0`, 、設定と **Tomcat Installation Directory** を使用する Tomcat サーバーのバージョンがインストールされているディレクトリにします。

    ![][5]

    **[完了]** をクリックします。

5. 次に、**[Properties]** ダイアログ ボックスの **[Targeted Runtimes]** ページに戻ります。 **[Apache Tomcat v7.0]** を選択し、**[OK]** をクリックします。

    ![][6]

6. Eclipse の **[Run]** メニューで、**[Run]** をクリックします。 **[Run]** ダイアログ ボックスで、**[Run on Server]** を選択します。 **[Run on Server]** ダイアログ ボックスで、**[Tomcat v7.0 Server]** を選択します。

    ![][7]

    **[完了]** をクリックします。

7. アプリケーションを実行すると表示されます、 **JSPHello** ページが Eclipse の localhost ウィンドウに表示されます (`JSPHello/`)、次のメッセージを表示します。

    `Hello World、時刻は Tue 年 3 月 24日 23時 21分: 10 GMT 2015`


#### アプリケーションを WAR としてエクスポートする

Web プロジェクト ファイルを Web アーカイブ (WAR) ファイルとしてエクスポートすると、これを Web アプリにデプロイできます。 次の Web プロジェクト ファイルは、WebContent フォルダーに存在します。

    META-INF
    WEB-INF
    index.jsp

1. WebContent フォルダーを右クリックし、**[Export]** を選択します。

2. **[Export Select]** ダイアログ ボックスで、**[Web] > [WAR]** ファイルの順にクリックし、**[次へ]** をクリックします。

3. **[WAR Export]** ダイアログ ボックスで、現在のプロジェクトの src ディレクトリを選択し、末尾に WAR ファイルの名前を含めます。 次に例を示します。

    `< プロジェクト パス >/JSPHello/src/JSPHello.war`

WAR ファイルを展開する方法の詳細については、次を参照してください。 [Java アプリケーションを Azure App Service Web Apps を追加](web-sites-java-add-app.md)します。


### FTP を使用して Hello World アプリケーションをデプロイする

サードパーティの FTP クライアントを選択してアプリケーションを発行します。 この手順では、2 つのオプション (Azure に組み込まれている Kudu コンソールと、グラフィカル UI を備えた使いやすい一般的なツール FileZilla) について説明します。

> **注:** Azure Toolkit for Eclipse は、ストレージ アカウントとクラウド サービスへのデプロイをサポートしていますが、Web アプリへのデプロイは現在サポートしていません。 ストレージ アカウントを展開し、クラウド サービス」の説明に従って、Azure デプロイ プロジェクトを使用して [Eclipse で Azure の Hello World アプリケーションを作成する](http://msdn.microsoft.com/library/azure/hh690944.aspx), が web アプリには。 FTP や GitHub などの他の方法を使用して、Web アプリにファイルを転送します。
>
> **注:** Windows コマンド プロンプトから FTP (Windows に付属するコマンド ラインの FTP.EXE ユーティリティ) を使用することはお勧めしません。 FTP.EXE などのアクティブ FTP を使用する FTP クライアントは、多くの場合、ファイアウォールでの動作に失敗します。 アクティブ FTP は LAN ベースの内部アドレスを指定するため、FTP サーバーは接続に失敗する可能性があります。

FTP を使用した App Service Web アプリへのデプロイの詳細については、次のトピックをご覧ください。

- [FTP ユーティリティを使用して展開します。](web-sites-deploy.md)


#### [デプロイ資格情報を設定する] を選択して構成できます

**AzureWebDemo** アプリケーションを実行して Web アプリを作成したことを確認します。 ファイルはこの場所に転送されます。

1. 旧ポータルにログインし、**[Web Apps]** をクリックします。 **WebDemoWebApp** が Web アプリの一覧に表示され、実行していることを確認します。 **[WebDemoWebApp]** をクリックし、**[ダッシュボード]** ページを開きます。

2. **[ダッシュボード]** ページの **[概要]** で、**[Set up your deployment credentials]** をクリックします (既にデプロイ資格情報を設定している場合は、**[Reset your deployment credentials]** が読み込まれます)。

    デプロイ資格情報は、Microsoft アカウントと関連付けられています。 Git と FTP を使用してデプロイする際のユーザー名パスワードを指定する必要があります。 これらの資格情報を使用すると、Microsoft アカウントに関連付けられているすべての Azure サブスクリプション内の任意の Web アプリにデプロイできます。 Git と FTP のデプロイ資格情報をダイアログ ボックスに入力し、後で使用するためにパスワードとユーザー名を記録します。


#### FTP の接続情報を取得する

FTP を使用して、新しく作成された Web アプリにアプリケーション ファイルをデプロイするには、接続情報を取得する必要があります。 接続情報を取得するには、2 つの方法があります。 1 つは、Web アプリの**ダッシュボード** ページにアクセスする方法、もう 1 つは、Web アプリの発行プロファイルをダウンロードする方法です。 発行プロファイルは、FTP ホスト名やログオン資格情報などの情報を Azure App Service の Web アプリに提供する XML ファイルです。 このユーザー名とパスワードを使用すると、この Web アプリだけでなく、Azure アカウントに関連付けられているすべてのサブスクリプション内の任意の Web アプリにデプロイできます。

Web アプリのブレードから FTP の接続情報を取得する、 [Azure ポータルの][]:

1. **Essentials** で、**FTP ホスト名**を検索してコピーします。 これはのような URI `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`します。

2. **Essentials** で、**FTP/デプロイ ユーザー名**を検索してコピーします。 これには、フォームが *webappname\deployment username*。 たとえば `WebDemoWebApp\deployer77`します。

発行プロファイルから FTP 接続情報を取得するには

1. Web アプリのブレードで、**[発行プロファイルを取得]** をクリックします。 .publishsettings ファイルがローカル ドライブにダウンロードされます。

2. XML エディターまたはテキスト エディターで .publishsettings ファイルを開き、検索、 `< publishProfile >` 要素を含む `publishMethod ="FTP"`します。この要素は次のようになります。

        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>

3. Web アプリの `publishProfile` 次のように FileZilla Site Manager 設定にマップを設定します。

- `publishUrl` と同じ **FTP ホスト名**, に設定された値 **ホスト**します。
- `publishMethod ="FTP"` を設定することを意味 **プロトコル** に **FTP - File Transfer Protocol**, 、および **暗号化** に **Use plain FTP**します。
- `userName` と `userPWD` の実際のユーザー名とパスワードの値がデプロイ資格情報をリセットするときに指定したキーします。 `userName` と同じ **展開/FTP ユーザー**します。 これらは、FileZilla の**ユーザー**と**パスワード**にマップされます。
- `ftpPassiveMode ="True"` FTP サイトがパッシブ FTP を使用することを意味の転送です。 選択 **パッシブ** 上、 **設定の転送** ] タブをクリックします。


#### Java アプリケーションをホストする Web アプリを構成する

アプリケーションを発行する前に、Web アプリが Java アプリケーションをホストできるように、いくつかの構成設定を変更する必要があります。

1. 旧ポータルで、Web アプリの **[ダッシュボード]** ページに移動し、**[構成]** をクリックします。 **[構成]** ページで、次の設定を指定します。

2. **[Java バージョン]** の既定値を **[オフ]** にし、アプリケーションに対応する Java バージョンを選択します (例: 1.7.0_51)。 次に、**[Web コンテナー]** が Tomcat サーバーのバージョンに設定されていることを確認します。

3. **[既定のドキュメント]** で、index.jsp を追加し、これを一覧の先頭に移動します (Web アプリの既定のファイルは hostingstart.html です)。

4. **[保存]** をクリックします。


#### Kudu を使用してアプリケーションを発行する

アプリケーションを発行する 1 つの方法は、Azure に組み込まれている Kudu デバッグ コンソールを使用することです。 Kudu は安定性があり、App Service Web Apps や Tomcat サーバーと一貫性があることが知られています。 ブラウザーから次の形式の URL を参照して、Web アプリのコンソールにアクセスします。

`https://&lt;webappname&gt;.scm.azurewebsites.net/DebugConsole`

1. この手順では、Kudu コンソールは次の URL にあります。この場所にアクセスしてください。

    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`

2. 上部のメニューから、**[デバッグ コンソール] > [CMD]** の順に選択します。

3. コンソールのコマンドラインに移動 `/サイト/wwwroot` (をクリックしてまたは `サイト`, 、し `wwwroot` ページの上部にあるディレクトリ ビューで)。

    `cd/サイト/wwwroot`

4. **Java バージョン**を指定したら、Tomcat サーバーで webapps ディレクトリを作成する必要があります。 コンソールのコマンド ラインで、webapps ディレクトリに移動します。

    `mkdir webapps`

    `cd webapps`

5. JSPHello.war をドラッグして `< プロジェクト パス >/JSPHello/src/` し、下の Kudu ディレクトリ ビューにドロップ `/site/wwwroot/webapps`します。[Drag here to upload and zip] 領域にはドラッグしないでください。Tomcat がファイルを解凍します。

  ![][8]

最初に、JSPHello.war が単独でディレクトリ領域に表示されます。

  ![][9]

短時間 (おそらく 5 分未満) で、Tomcat サーバーはアンパックされた JSPHello ディレクトリに WAR ファイルを解凍します。 ルート ディレクトリをクリックして、Index.jsp が解凍され、コピーされていることを確認します。 確認したら、webapps ディレクトリに戻り、アンパックされた JSPHello ディレクトリが作成されていることを確認します。 これらの項目が表示されない場合は、少し待ってから繰り返します。

  ![][10]


#### FileZilla を使用してアプリケーションを発行する (省略可能)

アプリケーションの発行に使用できるもう 1 つのツールは、FileZilla です。これは、サード パーティの一般的な FTP クライアントで、使いやすいグラフィカル UI を備えています。 ダウンロードしてインストールから FileZilla を [http://filezilla-project.org/](http://filezilla-project.org/) 場合は、既にがあるないことです。 クライアントの使用に関する詳細については、次を参照してください。、 [FileZilla ドキュメント](https://wiki.filezilla-project.org/Documentation) とこのブログ エントリ [FTP クライアント - パート 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx)します。

1. FileZilla で、**[ファイル] > [サイト マネージャー]** の順にクリックします。
2. **[サイト マネージャー]** ダイアログ ボックスで、**[新しいサイト]** をクリックします。 新しい空の FTP サイトが表示され、**[Select Entry]** に名前を指定するように求められます。 この手順で名前を付けます `AzureWebDemo FTP`します。

    **[一般]** タブで、次の設定を指定できます。
    - **ホスト:** Enter、 **FTP ホスト名** ダッシュ ボードからコピーします。
    - **ポート:** (空白のままに、これは、パッシブ転送と、サーバーが使用するポートを決定します)。
    - **プロトコル:** FTP ファイル転送プロトコル
    - **暗号化:** Use plain FTP
    - **ログオンの種類:** -標準
    - **ユーザー:** 展開の入力/FTP ユーザー ダッシュ ボードからコピーしました。 これは、*Web アプリ名\デプロイ ユーザー名*という形式の完全な FTP ユーザー名です。
    - **パスワード:** デプロイ資格情報を設定したときに指定したパスワードを入力します。

    **[転送設定]** タブで **[パッシブ]** を選択します。

3. **[接続]** をクリックします。 かどうかは成功すると、FileZilla のコンソールが表示されます、 `ステータス: 接続されている` メッセージと問題、 `リスト` ディレクトリの内容を一覧表示するコマンドです。

4. **ローカル** サイト パネルで、JSPHello.war ファイルが格納されているソース ディレクトリを選択します。パスは次のようになります。

    `< プロジェクト パス >/JSPHello/src/`

5. **リモート** サイト パネルで、転送先フォルダーを選択します。 WAR ファイルを展開するが、 `webapps` web アプリのルート ディレクトリです。 移動 `/サイト/wwwroot`, を右クリックして `wwwroot`, を選択して **ディレクトリを作成**します。 ディレクトリに名前を `webapps` し、そのディレクトリに入力します。

6. 転送する JSPHello.war `/site/wwwroot/webapps`します。 **ローカル** ファイル リストで JSPHello.war を選択し、これを右クリックして **[アップロード]** を選択します。 表示することを確認する必要があります `/site/wwwroot/webapps`します。

7. JSPHello.war を webapps ディレクトリにコピーすると、Tomcat サーバーは WAR ファイル内のファイルを自動的にアンパック (解凍) します。 Tomcat サーバーはほぼ瞬時にアンパックを開始しますが、ファイルが FTP クライアントに表示されるまで長い時間 (場合によっては数時間) がかかる可能性があります。


#### Hello World アプリケーションを Web アプリで実行する

1. WAR ファイルをアップロードして、Tomcat サーバーがアンパックされた作成したことを確認した後 `JSPHello` ディレクトリ参照 `http://webdemowebapp.azurewebsites.net/JSPHello` アプリケーションを実行します。

   > **注:** ] をクリックすると **参照** 従来のポータルからする可能性があります
   > "この Java ベースの web アプリケーションはと答えると、既定の web を取得します。
   > 正常に作成されました。" Web ページを更新する必要があります。
   > 注文を表示する既定の web ページではなく、アプリケーションを出力します。

2. アプリケーションを実行すると、Web ページに次の出力が表示されます。

    `Hello World、時刻は Tue 年 3 月 24日 23時 21分: 10 GMT 2015`


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
[azure app service]: http://go.microsoft.com/fwlink/?LinkId=529714 
[web platform installer]: http://go.microsoft.com/fwlink/?LinkID=252838 
[azure toolkit for eclipse]: https://msdn.microsoft.com/library/azure/hh690946.aspx 
[azure classic portal]: https://manage.windowsazure.com 
[what is an azure ad directory]: http://technet.microsoft.com/library/jj573650.aspx 
[create and upload a management certificate for azure]: http://msdn.microsoft.com/library/azure/gg551722.aspx 
[key and certificate management tool (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html 
[websitemanagementclient]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/WebSiteManagementClient.html 
[webspacenames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html 
[azure portal]: https://portal.azure.com 

