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

Java web アプリを初期設定した後 [Azure App Service の][] 」の説明に従って [Azure App Service で Java web アプリの作成](web-sites-java-get-started.md), 、アプリケーションをアップロードするには、WAR を配置することで、 **webapps** フォルダーです。

**webapps** フォルダーへのナビゲーション パスは Web アプリのインスタンス設定方法によって異なります。

- Azure Marketplace へのパスを使用して、web アプリを設定した場合、 **webapps** フォルダーは、フォーム **d:\home\site\wwwroot\bin\application\_server\webapps**, ここで、 **application\_server** アプリケーション サーバーの名前を指定、Web アプリのインスタンスで有効にします。
- Azure の構成 UI を使用して Web アプリを設定した場合、**webapps** フォルダーへのパスは、**d:\home\site\wwwroot\webapps** という形式になります。

継続的な統合シナリオなどで、ソース管理を使用してアプリケーションや Web ページをアップロードできます。 Web アプリでソース管理を使用するための手順は [Azure App Service での GIT による継続的なデプロイ](web-sites-publish-source-control.md)します。 FTP も、アプリケーションまたは Web ページをアップロードするためのオプションです。

WAR ファイルを **webapps** フォルダーにアップロードすると、Tomcat アプリケーション サーバーで WAR ファイルの追加が検出され、WAR ファイルが自動的に読み込まれます。 ルート ディレクトリにファイル (WAR ファイル以外) をコピーした場合は、それらのファイルを使用する前に、アプリケーション サーバーの再起動が必要になります。 Azure で実行されている Tomcat Java Web アプリの自動読み込み機能は、**webapps** フォルダーに追加される新しい WAR ファイル、または新しいファイルやディレクトリに基づいて動作します。

## 次のステップ

詳細については、次を参照してください。、 [Java デベロッパー センター](/develop/java/)します。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]



[azure app service]: http://go.microsoft.com/fwlink/?LinkId=529714 

