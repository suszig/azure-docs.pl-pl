<properties 
    pageTitle="Azure へのカスタム Java Web アプリのアップロード" 
    description="このチュートリアルでは、カスタム Java Web アプリを Azure App Service Web Apps にアップロードする方法を示します。" 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/19/2015" 
    ms.author="robmcm"/>

# Azure へのカスタム Java Web アプリのアップロード

このトピックへのカスタム Java web アプリをアップロードする方法について説明 [Azure App Service] Web アプリです。 Java Web サイトまたは Web アプリに適用される情報や、特定のアプリケーションを有効にする例も取り上げています。

Azure は」の説明に従って、Azure ポータルの構成 UI や Azure Marketplace を使用して Java web アプリを作成するための手段を [Azure App Service で Java web アプリの作成](web-sites-java-get-started.md)します。 このチュートリアルは、Azure ポータルの構成 UI や Azure Marketplace を使用しないシナリオを対象にしています。  

## 構成のガイドライン

ここでは、Azure のカスタム Java Web アプリに想定される設定について説明します。

- Java プロセスで使用される HTTP ポートは動的に割り当てられます。  このプロセスでは、環境変数 `HTTP_PLATFORM_PORT` のポートを使用する必要があります。
- 1 つの HTTP リスナー以外のすべてのリッスン ポートを無効にする必要があります。  Tomcat では、それはシャットダウン、HTTPS、AJP などのポートです。
- コンテナーは IPv4 トラフィック専用に構成する必要があります。
-  **スタートアップ** コマンドは、アプリケーションが、構成で設定する必要があります。
- 使用してディレクトリの書き込みアクセス許可を必要とするアプリケーションは Azure web アプリのコンテンツ ディレクトリに配置する必要があります **D:\home**します。  環境変数 `HOME` は、D:\home を参照します。  

web.config ファイルで必要に応じて環境変数を設定できます。

## web.config の httpPlatform の構成

次のとおり、 **httpPlatform** web.config 内で書式設定します。
                                 
**引数** (Default ="") です。 実行可能ファイルまたはスクリプトで指定した引数、 **processPath** 設定します。

例 (と **processPath** 含まれている)。

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"
    arguments="start"
    
    processPath="%JAVA_HOME\bin\java.exe"
    arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP\_PLATFORM\_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"


**processPath** -HTTP 要求のリッスン プロセスを起動するスクリプトまたは実行可能ファイルへのパス。

次に例を示します。


    processPath="%JAVA_HOME%\bin\java.exe"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat"

    processPath="%HOME%\site\wwwroot\bin\tomcat\bin\catalina.bat"
                                                                                       
**rapidFailsPerMinute** (Default = 10)。プロセスが指定されている回数 **processPath** 1 分あたりのクラッシュを許可します。 この制限を超えた場合 **HttpPlatformHandler** は分単位の残りのプロセスを起動しなくなります。
                                    
**requestTimeout** (Default ="00: 02:00"です)。期間 **HttpPlatformHandler** でリッスン プロセスからの応答を待つ `%HTTP_PLATFORM_PORT%`します。

**startupRetryCount** (Default = 10)。回数 **HttpPlatformHandler** で指定されたプロセスを起動しようとして、 **processPath**します。 参照してください **startupTimeLimit** 詳細です。

**startupTimeLimit** (Default = 10 秒です)。期間 **HttpPlatformHandler** ポートでリッスンしているプロセスを開始する実行可能ファイル/スクリプトが待機します。  この制限時間を超えた場合、 **HttpPlatformHandler** プロセスを強制終了され、それをもう一度起動しようとしています。 **startupRetryCount** 回です。
                                                                                      
**stdoutLogEnabled** (Default ="true"です)。True の場合、 **stdout** と **stderr** で指定されたプロセスの **processPath** 設定で指定されたファイルにリダイレクトされます **stdoutLogFile** (を参照してください **stdoutLogFile** セクション)。
                                    
**stdoutLogFile** (Default="d:\home\LogFiles\httpPlatformStdout.log"です)。絶対ファイル パス **stdout** と **stderr** で指定されたプロセスから **processPath** ログに記録されます。
                                    
> [AZURE.NOTE] `%HTTP_PLATFORM_PORT%` の一部として指定する必要がある特殊なプレース ホルダーは、 **引数** またはの一部として、 **httpPlatform** **environmentVariables** ] ボックスの一覧です。 これでは、内部で生成されたポートで置き換えられる **HttpPlatformHandler** によってプロセスが指定できるように **processPath** ポートをリッスンできます。

## デプロイ

Java ベースの Web アプリは、インターネット インフォメーション サービス (IIS) ベースの Web アプリケーションの場合とほぼ同じ手段を使用して、簡単にデプロイできます。  FTP、Git、Kudu はすべて、Web アプリ用の統合 SCM 機能と同様に、デプロイ メカニズムとしてサポートされています。 WebDeploy はプロトコルとして機能しますが、Java は Visual Studio で開発されていないため、WebDeploy は Java Web アプリのデプロイには向いていません。

## アプリケーションの構成例

次のアプリケーションの web.config ファイルとアプリケーション構成は、App Service Web Apps で Java アプリケーションを有効にする方法を示す例として挙げています。

### Tomcat
App Service Web Apps には 2 種類の Tomcat インスタンスが用意されていますが、独自のインスタンスをアップロードすることもできます。 次に示しているのは、個別の Java 仮想マシン (JVM) に Tomcat をインストールする例です。

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
          <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%HOME%\site\wwwroot\bin\tomcat\bin\startup.bat" 
            arguments="">
          <environmentVariables>
            <environmentVariable name="CATALINA_OPTS" value="-Dport.http=%HTTP_PLATFORM_PORT%" />
            <environmentVariable name="CATALINA_HOME" value="%HOME%\site\wwwroot\bin\tomcat" />
            <environmentVariable name="JRE_HOME" value="%HOME%\site\wwwroot\bin\java" /> <!-- optional, if not specified, this will default to %programfiles%\Java -->
            <environmentVariable name="JAVA_OPTS" value="-Djava.net.preferIPv4Stack=true" />
          </environmentVariables>
        </httpPlatform>
      </system.webServer>
    </configuration>

Tomcat 側で、行う必要のあるいくつかの構成変更があります。 server.xml では次のように設定する必要があります。

-   シャットダウン ポート = -1
-   HTTP コネクタ ポート = ${port.http}
-   HTTP コネクタ アドレス = "127.0.0.1"
-   HTTPS と AJP のコネクタをコメントアウト
-   IPv4 の設定は catalina.properties ファイルを追加できます。 内でも設定できます。     `java.net.preferIPv4Stack=true`
    
Direct3d の呼び出しは App Service Web Apps ではサポートされていません。 それらの呼び出しを無効にするには、アプリケーションによるこのような呼び出しに備えて、次の Java オプションを追加します。`-Dsun.java2d.d3d=false`

### Jetty

Tomcat の場合と同様、Jetty の独自のインスタンスをアップロードできます。 Jetty のフル インストールを実行する場合、構成は次のようになります。

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
          <add name="httppPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%JAVA_HOME%\bin\java.exe" 
             arguments="-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP_PLATFORM_PORT% -Djetty.base=&quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115&quot; -jar &quot;%HOME%\site\wwwroot\bin\jetty-distribution-9.1.0.v20131115\start.jar&quot;"
            startupTimeLimit="20"
          startupRetryCount="10"
          stdoutLogEnabled="true">
        </httpPlatform>
      </system.webServer>
    </configuration>

Jetty の構成を start.ini で変更して、`java.net.preferIPv4Stack=true` を設定する必要があります。

### Springboot
実行されている Springboot アプリケーションを取得するには、JAR または WAR ファイルをアップロードし、次の web.config ファイルを追加する必要があります。 web.config ファイルは、wwwroot フォルダーに配置されます。 web.config で、JAR ファイルをポイントするように引数を調整します。次の例では、JAR ファイルも wwwroot フォルダーに配置されています。  

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
          <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
            arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\my-web-project.jar&quot;">
        </httpPlatform>
      </system.webServer>
    </configuration>


### Hudson

Microsoft のテストでは、Hudson 3.1.2 war と既定の Tomcat 7.0.50 インスタンスを使用しましたが、セットアップに UI を使用しませんでした。  専用のインスタンスにインストールすることを勧め Hudson はソフトウェア ビルド ツールであるため、ここで、 **AlwaysOn** 、web アプリでフラグを設定することができます。

1. つまり、web アプリのルート ディレクトリ、 **d:\home\site\wwwroot**, 、作成、 **webapps** ディレクトリ (まだない場合) に Hudson.war を配置 **d:\home\site\wwwroot\webapps**します。
2. Apache maven 3.0.5 (Hudson と互換性あり) をダウンロードし、[配置 **d:\home\site\wwwroot**します。
3. Web.config を作成 **d:\home\site\wwwroot** しを次の内容を貼り付けます。
    
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>
          <system.webServer>
            <handlers>
              <add name="httppPlatformHandler" path="*" verb="*" 
        modules="httpPlatformHandler" resourceType="Unspecified" />
            </handlers>
            <httpPlatform processPath="%AZURE_TOMCAT7_HOME%\bin\startup.bat"
        startupTimeLimit="20"
        startupRetryCount="10">
        <environmentVariables>
          <environmentVariable name="HUDSON_HOME" 
        value="%HOME%\site\wwwroot\hudson_home" />
          <environmentVariable name="JAVA_OPTS" 
        value="-Djava.net.preferIPv4Stack=true -Duser.home=%HOME%/site/wwwroot/user_home -Dhudson.DNSMultiCast.disabled=true" />
        </environmentVariables>            
            </httpPlatform>
          </system.webServer>
        </configuration>

    この時点で、Web アプリを再び開始して、変更を行うことができます。  Hudson を開始 http://yourwebapp/hudson に接続します。

4. Hudson の自動構成の後に、次の画面が表示されます。

    ![Hudson](./media/web-sites-java-custom-upload/hudson1.png)
    
5. Hudson の構成ページにアクセスします。] をクリック **Manage Hudson**, 、] をクリックし、 **システムの構成**します。
6. 次に示しているように JDK を構成します。

    ![Hudson の構成](./media/web-sites-java-custom-upload/hudson2.png)

7. 次に示すように Maven を構成します。

    ![Maven の構成](./media/web-sites-java-custom-upload/maven.png)

8. 設定を保存します。 これで、Hudson は構成されて使用できる状態になりました。

Hudson の詳細については、次を参照してください。 [http://hudson-ci.org](http://hudson-ci.org)します。

### Liferay

Liferay は、App Service Web Apps でサポートされます。 Liferay には大量のメモリが必要になることがあるため、十分なメモリを用意できる中規模または大規模な専用ワーカーで、Web アプリを実行する必要があります。 Liferay も起動するまで数分かかります。 そのため、これをお勧め web アプリを設定すること **Always On**します。  

Tomcat にバンドルされている Liferay 6.1.2 Community Edition GA3 を使用して、次のファイルを Liferay のダウンロード後に編集します。

**Server.xml**

- シャットダウン ポートを -1 に変更。
- HTTP コネクタを次のように変更。 
        `<Connector port="${port.http}" protocol="HTTP/1.1" connectionTimeout="600000" address="127.0.0.1" URIEncoding="UTF-8" />`
- AJP コネクタをコメントアウト。

 **Liferay\tomcat-7.0.40\webapps\ROOT\WEB-INF\classes** フォルダー、という名前のファイルを作成する **portal-ext.properties**します。 このファイルには、次に示している 1 行を追加する必要があります。

    liferay.home=%HOME%/site/wwwroot/liferay

Tomcat-7.0.40 フォルダーと同じディレクトリ レベルでという名前のファイル作成 **web.config** 以下の内容。

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <system.webServer>
        <handlers>
    <add name="httpPlatformHandler" path="*" verb="*"
         modules="httpPlatformHandler" resourceType="Unspecified" />
        </handlers>
        <httpPlatform processPath="%HOME%\site\wwwroot\tomcat-7.0.40\bin\catalina.bat" 
                      arguments="run" 
                      startupTimeLimit="10" 
                      requestTimeout="00:10:00" 
                      stdoutLogEnabled="true">
          <environmentVariables>
      <environmentVariable name="CATALINA_OPTS" value="-Dport.http=%HTTP_PLATFORM_PORT%" />
      <environmentVariable name="CATALINA_HOME" value="%HOME%\site\wwwroot\tomcat-7.0.40" />
            <environmentVariable name="JRE_HOME" value="D:\Program Files\Java\jdk1.7.0_51" /> 
            <environmentVariable name="JAVA_OPTS" value="-Djava.net.preferIPv4Stack=true" />
          </environmentVariables>
        </httpPlatform>
      </system.webServer>
    </configuration>

下にある、 **httpPlatform** ブロック、 **requestTimeout** に設定されている"00: 10:00"です。  この時間を短くすることはできますが、Liferay のブートストラップ中に何らかのタイムアウト エラーが表示される可能性があります。  この値が変更された場合、 **connectionTimeout** 、tomcat の server.xml も変更されています。  

注意点としては、前に示している web.config で JRE_HOME 環境変数を、64-bit JDK を参照するように指定しています。 既定では 32-bit ですが、Liferay では高位のメモリが必要になる場合があるため、64-bit JDK を使用することをお勧めします。

Liferay を実行する web アプリを再起動して、これらの変更を加えた後は、次に、http://yourwebapp を開きます。 Liferay ポータルは Web アプリのルートから利用できます。 

## 次のステップ

Liferay の詳細については、次を参照してください。 [http://www.liferay.com](http://www.liferay.com)します。

Java の詳細については、次を参照してください。、 [Java デベロッパー センター](/develop/java/)します。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 
 
<!-- External Links -->
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714


