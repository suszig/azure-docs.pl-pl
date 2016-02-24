<properties 
    pageTitle="Azure App Service Web Apps への Java アプリケーションの追加" 
    description="このチュートリアルでは、Java を使用するように設定された Azure App Service Web Apps のインスタンスに、ページやアプリケーションを追加する方法について説明します。" 
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

# Azure App Service Web Apps への Java アプリケーションの追加

」の説明に従って、[Azure App Service] で、Java web アプリを初期設定した後 [Azure App Service で Java web アプリの作成](web-sites-java-get-started.md), 、アプリケーションをアップロードするには、WAR を配置することで、 **webapps** フォルダーです。

ナビゲーション パス、 **webapps** フォルダーは、Web アプリのインスタンスを設定する方法に応じて異なります。

- Azure Marketplace へのパスを使用して、web アプリを設定した場合、 **webapps** フォルダーは、フォーム **d:\home\site\wwwroot\bin\application\_server\webapps**, ここで、 **application\_server** アプリケーション サーバーの名前を指定、Web アプリのインスタンスで有効にします。 
- Azure の構成 UI へのパスを使用して、web アプリを設定した場合、 **webapps** フォルダーは、フォーム **d:\home\site\wwwroot\webapps**します。 

継続的な統合シナリオなどで、ソース管理を使用してアプリケーションや Web ページをアップロードできます。 Web アプリでソース管理を使用するための手順は [Azure App Service での GIT による継続的なデプロイ](web-sites-publish-source-control.md)します。 FTP も、アプリケーションまたは Web ページをアップロードするためのオプションです。

Tomcat web アプリケーションに関する注意: WAR ファイルをアップロードすると、 **webapps** フォルダー、Tomcat アプリケーション サーバーが検出する追加され自動的に読み込まれます。 ルート ディレクトリにファイル (WAR ファイル以外) をコピーした場合は、それらのファイルを使用する前に、アプリケーション サーバーの再起動が必要になります。 Azure で実行されている Tomcat Java web アプリの自動読み込み機能が追加される新しい WAR ファイルに基づいて、または新しいファイルまたはディレクトリに追加、 **webapps** フォルダーです。 

## 次のステップ

詳細については、次を参照してください。、 [Java デベロッパー センター](/develop/java/)します。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714

