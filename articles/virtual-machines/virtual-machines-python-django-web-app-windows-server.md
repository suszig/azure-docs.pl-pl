<properties
    pageTitle="Django を使用した Python Web アプリケーション | Microsoft Azure"
    description="このチュートリアルでは、従来のデプロイ モデルで Windows Server 2012 R2 Datacenter 仮想マシンを使用して Azure で Django ベースの Web サイトをホストする方法について説明します。"
    services="virtual-machines"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>


<tags 
    ms.service="virtual-machines" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="huvalo"/>


# Windows Server VM での Django Hello World Web アプリケーション

> [AZURE.SELECTOR]
- [Windows](virtual-machines-python-django-web-app-windows-server.md)
- [Mac/Linux](virtual-machines-python-django-web-app-linux.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。
 

このチュートリアルでは、Windows Server 仮想マシンを使用して Microsoft
Azure で Django ベースの Web サイトをホストする方法について説明します。 このチュートリアルは、Azure を使用した経験がない読者を対象に作成されています。 このチュートリアルを完了すると、クラウド内で動作する Django ベースのアプリケーションが完成します。

学習内容:

* Django をホストするように Azure の仮想マシンを設定します。 このチュートリアルでは Windows Server で Azure の仮想マシンを設定する方法について説明しますが、Azure でホストされている Linux VM の場合も基本的な手順は同じです。
* 新しい Django アプリケーションを Windows から作成します。

このチュートリアルを実行して、単純な Hello World Web
想定しています。 このアプリケーションは Azure の仮想マシンでホストされます。

完成したアプリケーションのスクリーンショットは次のようになります。

![Azure で Hello World ページを表示するブラウザー ウィンドウ][1]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## Django をホストする Azure の仮想マシンの作成と構成

1. 記載されている手順に従います [ここ](virtual-machines-windows-tutorial-classic-portal.md) 、Windows Server 2012 R2 Datacenter ディストリビューションの Azure の仮想マシンを作成します。

1. Azure で、ポート 80 トラフィックを Web から仮想マシン上のポート 80 に転送します。
 - Azure クラシック ポータルで新しく作成された仮想マシンに移動し、クリックして、 **エンドポイント** ] タブをクリックします。
 - クリックして、 **追加** 、画面の下部にあるボタンをクリックします。
    ![エンドポイントを追加します。](./media/virtual-machines-python-django-web-app-windows-server/django-helloworld-addendpoint.png)

 - 開き、 **TCP** プロトコルの **パブリック ポート 80** として **プライベート ポート 80**します。
![][port80]
1.  **ダッシュ ボード** ] タブ、[ **接続** を使用する **リモート デスクトップ** 新しく作成した Azure 仮想マシンにリモートでログインします。  

**重要な注意事項:** 以下のすべての手順は、仮想マシンに正しくログインして、想定し、ローカル コンピューターではなく、コマンドが発行します。

## <a id="setup"> </a>Python、Django、WFastCGI をインストールします。

**注:** IE ESC 設定を構成する必要があります、Internet Explorer を使用してダウンロードするのには ([スタート]、[管理ツールとサーバー マネージャー]、[ローカル サーバー] をクリックし、  **IE セキュリティ強化の構成**, 、設定をオフに)。

1. 最新の Python 2.7 または 3.4 からインストール [python.org][]します。
1. pip を使用して wfastcgi パッケージおよび django パッケージをインストールします。

    Python 2.7 の場合、以下のコマンドを使用します。

        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django

    Python 3.4 の場合、以下のコマンドを使用します。

        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## FastCGI を使用した IIS のインストール

1. FastCGI のサポートを利用して IIS をインストールします。  この操作には数分かかる可能性があります。

        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## 新しい Django アプリケーションの作成

1.   *C:\inetpub\wwwroot*, 、新しい Django プロジェクトを作成するには、次のコマンドを入力してください。

    Python 2.7 の場合、以下のコマンドを使用します。

        C:\Python27\Scripts\django-admin.exe startproject helloworld

    Python 3.4 の場合、以下のコマンドを使用します。

        C:\Python34\Scripts\django-admin.exe startproject helloworld

    ![New-AzureService コマンドの結果](./media/virtual-machines-python-django-web-app-windows-server/django-helloworld-cmd-new-azure-service.png)

1.   **Django 管理** コマンドには、Django ベースの web サイトの基本的な構造が生成されます。

  -   **\manage.py** ホスティングの開始や、Django ベースの web サイトを停止することができます
  -   **helloworld\helloworld\settings.py** 、アプリケーション向けの Django の設定が含まれています。
  -   **helloworld\helloworld\urls.py** 各 url とそのビューとの間のマッピング コードが含まれています。

1.  という名前の新しいファイルを作成する **views.py** で、 *C:\inetpub\wwwroot\helloworld\helloworld* ディレクトリ。 このファイルには、"Hello World" ページをレンダリングするビューが含まれます。 エディターを起動し、次のコードを入力します。

        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  urls.py ファイルの内容を次のコードに置き換えます。

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )

## IIS の構成

1. グローバルの applicationhost.config で handlers セクションのロックを解除します。  これにより、web.config で python ハンドラーを使用できるようになります。

        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers

1. WFastCGI を有効化します。  これにより、グローバルの applicationhost.config に、Python インタープリターの実行可能ファイルと wfastcgi.py スクリプトを参照するアプリケーションが追加されます。

    Python 2.7:

        c:\python27\scripts\wfastcgi-enable

    Python 3.4:

        c:\python34\scripts\wfastcgi-enable

1. Web.config ファイルを作成 *C:\inetpub\wwwroot\helloworld*します。  `scriptProcessor` 属性の値は、前の手順の出力と一致させる必要があります。  ページを参照してください [wfastcgi][] に wfastcgi 設定の詳細について pypi にします。

    Python 2.7:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

    Python 3.4:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

1. django のプロジェクト フォルダを参照するように、IIS の既定の Web サイトの場所を更新します。

        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"

1. 最後に、ブラウザーで Web ページを読み込みます。

![Azure で Hello World ページを表示するブラウザー ウィンドウ][1]


## Azure の仮想マシンのシャットダウン

このチュートリアルが終了したら、新しく作成した Azure の仮想マシンをシャットダウンまたは削除して、他のチュートリアル用にリソースを解放し、Azure に対する利用料金の発生を回避します。

[1]: ./media/virtual-machines-python-django-web-app-windows-server/django-helloworld-browser-azure.png

[port80]: ./media/virtual-machines-python-django-web-app-windows-server/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi

