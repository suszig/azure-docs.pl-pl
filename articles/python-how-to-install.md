<properties
    pageTitle="Python と SDK のインストール - Azure"
    description="Azure で使用するための Python と SDK のインストール方法について説明します。"
    services=""
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="08/31/2015"
    ms.author="huvalo"/>


# Python と SDK のインストール

Python は、Windows でのセットアップが容易であり、Mac および Linux ではプレインストールされています。 このガイドでは、インストール方法と Azure で使用するためのコンピューターの準備について説明します。

## Python Azure SDK の機能

Azure SDK for Python には、Azure 向けの Python アプリケーションを開発、デプロイ、管理するためのコンポーネントが用意されています。 Azure SDK for Python には次のコンポーネントが用意されています。

* **Azure 用 Python クライアント ライブラリ**。 これらのクラス ライブラリには、ストレージやサービス バスなどのAzure の機能にアクセスするためのインターフェイスや、ストレージ アカウントや仮想マシンなどの Azure リソースを管理するためのインターフェイスが用意されています。
* **Azure エミュレーター (Windows のみ)**。 コンピューティング エミュレーターとストレージ エミュレーターは、アプリケーションをローカルでテストできるようにするための Cloud Services およびデータ管理サービスのローカル エミュレーターです。 Azure エミュレーターは Windows 上でのみ動作します。

## 使用する Python とそのバージョン

利用可能な Python インタープリターにはいくつかの種類があります。以下に例を示します。

* CPython - 最も一般的に使用されている標準的な Python インタープリター
* IronPython - .Net/CLR 上で実行する Python インタープリター
* Jython - JVM 上で実行する Python インタープリター

このリリースでは、Python Azure SDK と Websites や Cloud Services などの Azure サービス向けの **CPython** のみをテストしサポートします。 Version 2.7 または 3.4 をお勧めします。

## Python を入手するには

CPython はいくつかの方法で入手できます。

* から直接 [www.python.org:operator[]][]
* 入手など [www.continuum.io:operator[]][], 、[www.enthought.com:operator[]][] または [www.activestate.com][]
* ソースからビルド

特定のニーズがない限り、以下に説明するように、最初の 2 つの入手方法をお勧めします。

## Windows、Linux、および MacOS (クライアント ライブラリのみ) でのインストール

既に Python をインストールしている場合は、pip を使用して、既存の Python 2.7 または Python 3.3 以降の環境ですべてのクライアント ライブラリのバンドルをインストールできます。 これからパッケージをダウンロード、 [Python パッケージ インデックス [][] (PyPI)。

メモを使用する必要があります、 `sudo` Linux と MacOS で ie コマンドです。 `sudo pip インストール azure`します。

    pip install azure

バージョン 1.0.0 以降では、ライブラリは複数のパッケージに分割されています。 このため、必要なパッケージまたはバンドルのみをインストールできます。

Azure Storage ランタイム クライアント ライブラリをインストールするには、次の手順に従います。

    pip install azure-storage

Azure Service Bus ランタイム クライアント ライブラリをインストールするには、次の手順に従います。

    pip install azure-servicebus

Azure リソース マネージャー (ARM) クライアント ライブラリをインストールするには、次の手順に従います。

    pip install azure-mgmt

Azure サービス管理 (ARM) クライアント ライブラリをインストールするには、次の手順に従います。

    pip install azure-servicemanagement-legacy

## Windows (Python、Azure エミュレーター、クライアント ライブラリ) でのインストール

Web プラットフォーム インストーラーを使用して、インストールを合理化できます。 以下の CPython [www.python.org][]します。

* [Microsoft Azure SDK for Python 2.7][]
* [Microsoft Azure SDK for Python 3.4][]

**注:** Windows Server では、WebPI インストーラーをダウンロードするため、IE ESC 設定の構成が必要になる場合があります。設定するには、[スタート]、[管理ツール]、[サーバー マネージャー]、[ローカル サーバー] の順にクリックしてから、[**IE セキュリティ強化の構成**] をクリックし、[オフ] に設定します。

### Python 2.7

WebPI インストーラーには、Python Azure アプリケーション開発に必要なものがすべて用意されています。

![how-to-install-python-webpi-27-1](./media/python-how-to-install/how-to-install-python-webpi-27-1.png)

インストールが完了したら、入力 `python` ことを確認プロンプトで問題なく実行されます。 インストールの方法によっては、"path" 変数を (正しいバージョンの) Python が検索されるように設定する必要があります。

![how-to-install-python-win-run-27](./media/python-how-to-install/how-to-install-python-win-run-27.png)

インストール後は、既定の場所から Python とクライアント ライブラリを使用することができます。

        C:\Python27\Lib\site-packages\azure

### Python 3.4

WebPI インストーラーには、Python Azure アプリケーション開発に必要なものがすべて用意されています。

![how-to-install-python-webpi-34-1](./media/python-how-to-install/how-to-install-python-webpi-34-1.png)

インストールの完了後、プロンプトで「python」と入力し、インストールが問題なく実行されたことを確認します。 インストールの方法によっては、"path" 変数を (正しいバージョンの) Python が検索されるように設定する必要があります。

![how-to-install-python-win-run-34](./media/python-how-to-install/how-to-install-python-win-run-34.png)

インストール後は、既定の場所から Python とクライアント ライブラリを使用することができます。

        C:\Python34\Lib\site-packages\azure

### Windows でのアンインストール

**Azure SDK for Python** WebPI 製品は、一般的な意味でのアプリケーションではありません。実際には、これは 32 ビット版 Python 2.7/3.4、Python 用 Azure クライアント ライブラリなどの各製品のコレクションです。これらの製品がまとめてバンドルされています。 このため、このコレクション自体には、従来のアンインストーラーがありません。インストールされるプログラムを個別に Windows のコントロール パネルから削除する必要があります。

**Azure SDK for Python** を再インストールする場合は、管理者として PowerShell コマンド プロンプトを開き、次のコマンドを実行するだけです。

    rm -force "HKLM:\SOFTWARE\Microsoft\Python Tools for Azure"

次に、WebPI を再実行します。

## その他のパッケージの入手

[Python パッケージ インデックス [][] (PyPI) が Python ライブラリの選択肢が豊富です。 ディストリビューションをインストールすると、Web 開発から技術計算にいたるさまざまなシナリオのうち、興味のある多くのシナリオを実施することができます。


## Python Tools for Visual Studio

[Python Tools for Visual Studio の][] (PTVS) は、無料の OSS プラグインで、Microsoft では VS を本格的な Python IDE に変わります。

![how-to-install-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

PTVS の使用はオプションですが、使用することをお勧めします。この PTVS によって、Python と Web のプロジェクト/ソリューション サポート、デバッグ、プロファイル、対話型ウィンドウ、テンプレートの編集、Intellisense を利用できるためです。

PTVS も簡単に配置するためのサポートと共に、Microsoft Azure へ配置する [クラウド サービスの][] と [web サイトの][]します。

PTVS は、既存の Visual Studio 2013 または 2015 インストールに対応しています。 ドキュメント、ダウンロード、およびディスカッションでは、[Python Tools for Visual Studio] を参照してください。

## Linux や MacOS 向けの Python Azure

Mac/Linux で開発する場合、次の方法がサポートされています。

1. Python クライアント ライブラリを使用して、Azure サービスを利用する

2. Linux VM でアプリケーションを実行する

3. Git を使って Azure Websites を開発して公開する

最初のシナリオでは、利用する Azure の PaaS 機能など、豊富な web アプリケーションを作成することができます [blob ストレージを][], 、[キュー ストレージを][], 、[テーブル ストレージを][] 、Azure REST API 用の Python ラッパーを介してなどです。 これらは、Windows、Mac、Linux とまったく同じように動作します。 また、ローカルの開発マシンや Linux 仮想マシンからこれらのクライアント ライブラリを使うこともできます。

VM のシナリオでは、選択した Linux VM (Ubuntu、CentOS、Suse) を起動し、必要なものを実行または管理するだけです。 たとえば、実行することができます [IPython][] repl/notebook Windows、Mac または Linux でコンピューターし、ブラウザーをポイントして、Linux または Windows 複数プロセッサの VM が Azure で IPython エンジンを実行します。 参照してください、 [Azure での IPython Notebook][] 」チュートリアルの詳細をします。

Linux VM をセットアップする方法については、次を参照してください、 [Linux の仮想マシンを実行しているを作成する][] チュートリアルです。

Git のデプロイを使って Python Web アプリケーションを開発できます。また、どのオペレーティング システムからでも Azure Web サイトを公開できます。 ご利用のリポジトリを Azure にプッシュする場合、自動的に仮想環境が作られ、pip によって必要なパッケージがインストールされます。

開発と Azure の web サイトの公開の詳細については、用のチュートリアルを参照してください。 [Django [] で web サイトの作成][], 、[Bottle [] で web サイトの作成][] と [Flask [] で web サイトの作成][]します。 WSGI 互換フレームワークの使用に関する一般的な情報を参照してください。 [Azure web サイトの [] での Python の構成][]します。


## その他のソフトウェアとリソース:

* [Continuum Analytics Python 配布][]
* [Enthought Python 配布][]
* [ActiveState Python 配布][]
* [SciPy - Python の科学ライブラリのスイート][]
* [NumPy - Python の数値ライブラリ][]
* [Django プロジェクトの完成度の高い web フレームワーク/CMS][]
* [IPython の高度な Repl/notebook Python の][]
* [Azure のでの IPython Notebook][]
* [Python Tools for Visual Studio の GitHub][]
* [Python デベロッパー センター](/develop/python/)


[continuum analytics python distribution]: http://continuum.io 
[enthought python distribution]: http://www.enthought.com 
[activestate python distribution]: http://www.activestate.com 
[www.python.org]: http://www.python.org 
[www.continuum.io]: http://continuum.io 
[www.enthought.com]: http://www.enthought.com 
[www.activestate.com]: http://www.activestate.com 
[scipy - a suite of scientific python libraries]: http://www.scipy.org 
[numpy - a numerics library for python]: http://www.numpy.org 
[django project - a mature web framework/cms]: http://www.djangoproject.com 
[ipython - an advanced repl/notebook for python]: http://ipython.org 
[ipython]: http://ipython.org 
[ipython notebook on azure]: virtual-machines-python-ipython-notebook.md 
[cloud services]: cloud-services-python-ptvs.md 
[websites]: web-sites-python-ptvs-django-mysql.md 
[python tools for visual studio]: http://aka.ms/ptvs 
[python tools for visual studio on github]: https://github.com/microsoft/ptvs 
[python package index]: http://pypi.python.org/pypi 
[microsoft azure sdk for python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281 
[microsoft azure sdk for python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990 
[setting up a linux vm via the azure portal]: create-and-configure-opensuse-vm-in-portal.md 
[how to use the azure command-line interface]: crossplat-cmd-tools.md 
[create a virtual machine running linux]: virtual-machines-linux-tutorial.md 
[creating websites with django]: web-sites-python-create-deploy-django-app.md 
[creating websites with bottle]: web-sites-python-create-deploy-bottle-app.md 
[creating websites with flask]: web-sites-python-create-deploy-flask-app.md 
[configuring python with azure websites]: web-sites-python-configure.md 
[table storage]: storage-python-how-to-use-table-storage.md 
[queue storage]: storage-python-how-to-use-queue-storage.md 
[blob storage]: storage-python-how-to-use-blob-storage.md 

