<properties
    pageTitle="Jupyter/IPython Notebook を作成する | Microsoft Azure"
    description="Azure でクラシック デプロイ モデルを使用して作成した Linux 仮想マシンに Jupyter/IPython Notebook をデプロイする方法について説明します。"
    services="virtual-machines"
    documentationCenter="python"
    authors="crwilcox"
    manager="wpickett"
    editor=""
    tags=“azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="python"
    ms.topic="article"
    ms.date="11/10/2015"
    ms.author="crwilcox"/>

# Azure での Jupyter Notebook

 [Jupyter プロジェクト](http://jupyter.org), 以前、 [IPython プロジェクト](http://ipython.org), 、するためのツールのコレクションを提供科学技術計算をリアルタイムなコンピューティング ドキュメントの作成時にコードが実行を組み合わせた強力な対話型シェルを使用します。 作成されるノートブック ファイルには、任意のテキスト、数式、入力コード、結果、グラフィックス、ビデオ、その他の種類のメディアなど、最新の Web ブラウザーで表示できるデータを含めることができます。 Python を初めて利用し、対話形式の環境で楽しく Python を学習する場合でも、本格的な並列計算や技術計算をする場合でも、Jupyter Notebook はお勧めの選択肢です。

![スクリーンショット](./media/virtual-machines-python-ipython-notebook/ipy-notebook-spectral.png)
SciPy および Matplotlib パッケージを使用して、録音物の構造を分析します。


## Jupyter を利用する 2 つの方法: Azure Notebook またはカスタム デプロイ
Azure を使用できるサービスを提供する [Jupyter の使用をすばやく開始
](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx)  Azure Notebook サービスを使用するは、Jupyter の web からアクセス可能なインターフェイスへのアクセス簡単に得ることができます。
拡張性の高いコンピューティング リソースに提供できます。これにより、Python とその多くの
ライブラリが持つ機能をすべて利用できます。  ユーザーがこれらをアクセスできる、インストールが、サービスによって処理されるため
リソース管理と、ユーザーが構成の必要はありません。

Notebook サービスが自分のシナリオに合っていない場合でも、この記事を読み進めてください。仮想マシン (VM) を使用して Jupyter Notebook を Microsoft Azure にデプロイする方法の説明があります。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## Azure での VM の作成と構成

最初の手順では、Azure で実行されている仮想マシン (VM) を作成します。
この VM はクラウドで動作する完全なオペレーティング システムであり、
Jupyter Notebook を実行します。 Azure では Linux と Windows のどちらの
バーチャル マシン、およびこれら 2 種類の仮想マシンの Jupyter の設定を取り上げます。

### Linux VM を作成し、Jupyter 用のポートを開く

記載されている手順に従います [ここ][portal-vm-linux] の仮想マシンを作成、 *Ubuntu* 配布します。 このチュートリアルでは、Ubuntu Server 14.04 LTS を使用します。 ユーザー名と仮定 *azureuser*します。

仮想マシンをデプロイした後、ネットワーク セキュリティ グループに関するセキュリティ規則を開く必要があります。  Azure ポータルからに移動する **ネットワーク セキュリティ グループ** 、VM に対応するセキュリティ グループ] タブを開きます。 次の設定で、セキュリティの受信の規則を追加する必要があります。
**TCP** 、プロトコルの **\ *** ソース (パブリック) ポートのおよび **9999** 送信先 (プライベート) ポートにします。

![スクリーンショット](./media/virtual-machines-python-ipython-notebook/azure-add-endpoint.png)

中に、ネットワーク セキュリティ グループをクリックします **ネットワーク インターフェイス** し、確認、 **パブリック IP アドレス** は次の手順で仮想マシンへの接続に必要とします。

## VM への必要なソフトウェアのインストール

VM では、Jupyter Notebook を実行するには最初にインストール Jupyter と
依存するソフトウェアをインストールする必要があります。 ssh と、VM の作成時に選択したユーザー名とパスワードを使用して、Linux VM に接続します。 このチュートリアルでは、PuTTY を使用して Windows から接続します。

### Ubuntu への Jupyter のインストール
Anaconda、よく使用されるデータ サイエンスの python ディストリビューションから表示されるリンクのいずれかを使用してをインストール [Continuum Analytics](https://www.continuum.io/downloads)します。  このドキュメントの執筆時点では、次に示すリンクで最新バージョンを入手できます。

#### Linux 用の Anaconda のインストール
<table>
  <th>Python 3.4</th>
  <th>Python 2.7</th>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64 ビット</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64 ビット</href>
    </td>
  </tr>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32 ビット</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32 ビット</href>
    </td>  
  </tr>
</table>


#### Ubuntu への Anaconda3 2.3.0 64 ビット版のインストール
例として、Anaconda を Ubuntu にインストールする方法を次に示します。

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install -f jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![スクリーンショット](./media/virtual-machines-python-ipython-notebook/anaconda-install.png)


### Jupyter の構成と SSL の使用
インストールした後、Jupyter 用の構成ファイルをセットアップする必要があります。 Jupyter の構成に問題が発生した場合に参考になりますを見て、 [Jupyter Notebook サーバーを実行しているについて](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html)します。

次は `cd` 証明書には、プロファイル ディレクトリを SSL を作成および編集
プロファイルの構成ファイルを編集します。

Linux で次のコマンドを使用します。

    cd ~/.jupyter

次のコマンドを使用し、SSL 証明書を作成します (Linux と Windows)。

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

ここでは自己署名 SSL 証明書を作成しているので、ノートブックに接続するときに、
ブラウザーでセキュリティに関する警告が表示されます。  長期間にわたって
製品を使用するには、組織に関連付けられている適切に署名された証明書を
使用する必要があります。  証明書の管理はこのデモの範囲を越えているので、
ここでは自己署名証明書を引き続き使用します。

証明書を使用するだけでなく、許可されてない使用からノートブックを保護するために
パスワードを指定する必要もあります。  Jupyter を使用してセキュリティ上の理由
暗号化されたパスワードを構成ファイルで使用するため、最初に
パスワードを暗号化する必要があります。  IPython には暗号化のためのユーティリティが用意されています。コマンド プロンプトで次のコマンドを実行します。

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

パスワードを入力して確認した後、パスワードが出力されます。 次の手順のために、これを書き留めてください。

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

次に、プロファイルの構成ファイル (
`jupyter_notebook_config.py` ディレクトリ内のファイル。  このファイルは存在しないことがあります。その場合は作成してください。  この
ファイルにはさまざまなフィールドが含まれ、既定ではいずれもコメント アウトされています。  このファイルは
よく使用する テキスト エディターで開くことができます。
次の内容に最低限です。 パスワードは、前の手順で取得した sha1 に必ず置き換えてください。

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed :wqtraffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### Jupyter Notebook を実行する

この時点で、Jupyter Notebook を開始する準備が整いました。 IPython Notebook を実行するには、
ノートブックを保存するディレクトリに移動して、
次のコマンドを使用して Jupyter Notebook サーバーです。

    /anaconda3/bin/jupyter-notebook

Jupyter Notebook アドレスにアクセスできる必要がありますようになりました
`https://[PUBLIC-IP-ADDRESS]:9999`

ノートブックに初めてアクセスするとき、ログイン ページでパスワードが要求されます。 論理積
「Jupyter Notebook ダッシュ ボード」を参照してください、ログインすると、
ノートブックのすべての操作のソース管理センターです。  このページから、
新しい notebook し既存のものを開きます。

![スクリーンショット](./media/virtual-machines-python-ipython-notebook/jupyter-tree-view.png)

### Jupyter Notebook の使用

クリックすると、 **新規** ] ボタンを次のオープニング ページが表示されます。

![スクリーンショット](./media/virtual-machines-python-ipython-notebook/jupyter-untitled-notebook.png)

マークされた領域、 `In []:` プロンプトがある入力領域と、ここでは
適切な Python コードを入力し、ヒットしたときに実行されます `Shift-Enter` または
実行アイコン (ツール バーにある右向き三角形) をクリックすると、ここに入力したコードが実行されます。

## 強力なパラダイム: リッチ メディアを含んだリアルタイムなコンピューティング ドキュメント

ノートブック自体は、Python とワード プロセッサを使用したことがあるユーザーにとっては違和感のないドキュメントです。
これは、Python とワード プロセッサの両方の特徴を持っているためです。
ノートブックでは、Python コードのブロックを実行できます。また、
ツール バーのドロップダウン メニューでセルの形式を [Code] から [Markdown] に変更して、メモなどのテキストを保存する
ツールバーです。

Jupyter ははるかに超えて、ワード プロセッサが許可されている、
IPython Notebook では、計算処理とリッチ メディア (テキスト、グラフィックス、ビデオなど、最新の Web ブラウザーで
表示できるほとんどのデータ) を混在させることができます。 テキスト、コード、ビデオなどを混在させることができます。

![スクリーンショット](./media/virtual-machines-python-ipython-notebook/jupyter-editing-experience.png)

Python に関連する多数の優れた科学技術計算用ライブラリを使用することで、1 つの環境で、
テクニカル コンピューティングでは、次のスクリーン ショットでは、単純な計算を実行して、簡単に
として 1 つの環境での複雑なネットワーク分析します。

最新の Web の機能とリアルタイムな計算処理を混在させるというパラダイムは、
多くの可能性を示すものです。このパラダイムは、クラウドに適した理想的なパラダイムです。Notebook の
用途には次のようなものがあります。

* 問題に対する調査作業を記録するためのコンピューティング スクラッチパッドとして使用。

* 「リアルタイムな」コンピューティング フォームまたはの同僚と作業結果を共有するには
  ハードコピーの形式 (HTML や PDF)。

* 配布や、計算処理を含んだライブ教材の資料を提示するには
  受講者が実際のコードをすぐに試したり、変更したり、
  対話形式で再実行しています。

* 方法で調査結果を示す「実行可能な用紙」を提供するには
  すぐに再現、検証および拡張できる他のユーザーがいます。

* コラボレーション コンピューティング用のプラットフォームとして: 複数のユーザーにログインできます、
  ライブのコンピューティング セッションを共有する同じ notebook サーバーです。


IPython のソース コードに移動すると [リポジトリ][], 、全体が表示されます
ダウンロードして、Azure Jupyter VM で試して、ノートブックの例を含むディレクトリ。  このサイトから `.ipynb` ファイルをダウンロードし、それらのファイルをユーザーのノートブックに関する Azure VM のダッシュボードにアップロードしてください (直接 VM にダウンロードしてもかまいません)。

## まとめ

Jupyter Notebook が対話的にアクセスするため、強力なインターフェイスを提供します。
強力なインターフェイスが用意されています。  このインターフェイスは幅広い
用途 (簡単な調査、Python の学習、データの分析と表示、シミュレーション、並列コンピューティングなど)
に対応しています。 作成される Notebook の
ドキュメントには実行された計算処理の完全な記録が含まれており、
Jupyter 他のユーザーと共有できます。  Jupyter Notebook として使用できます、
ローカル アプリケーションとして使用できますが、Azure でのクラウドのデプロイメントに最適です。

Jupyter の主要機能が Visual Studio を使用して内部で使用ではまた、
[Python Tools for Visual Studio][] (PTVS)。 PTVS は、Microsoft が提供する無料のオープン ソース プラグインです。
このプラグインによって、Visual Studio を Python の高度な開発環境として利用できます。
この開発環境には、IntelliSense を備えた高度なエディター、デバッグ、
プロファイル、並列コンピューティング統合の機能が含まれています。

## 次のステップ

詳細については、次を参照してください。、 [Python デベロッパー センター](/develop/python/)します。

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repository]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs


