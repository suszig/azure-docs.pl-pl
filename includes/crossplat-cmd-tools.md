# Mac および Linux 用 Azure コマンド ライン ツールの使用方法

このガイドでは、Mac および Linux 用 Azure コマンド ライン ツールを使用して Azure のサービスを作成および管理する方法について説明します。 ここでは、**ツールのインストール**、**発行の設定のインポート**、**Azure Websites の作成および管理**、**Azure Virtual Machines の作成および管理**の各シナリオについて説明します。 包括的なリファレンス ドキュメントについては、次を参照してください。 [Mac および Linux のドキュメント ][reference-docs]します。

## 目次

* [Mac および Linux 用 Azure コマンド ライン ツールします。](#Overview)
* [Mac および Linux 用 Azure コマンド ライン ツールをインストールする方法](#Download)
* [Azure アカウントを作成する方法](#CreateAccount)
* [ダウンロードする方法と、発行設定のインポート](#Account)
* [作成して、Azure の Web サイトを管理する方法](#WebSites)
* [作成して、Azure の仮想マシンを管理する方法](#VMs)


<h2>
            <a id="Overview"></a>
            Mac および Linux 用 Azure コマンド ライン ツールについて
          </h2>

Mac および Linux 用 Azure コマンド ライン ツールは、Azure サービスをデプロイおよび管理するためのコマンド ライン ツールのセットです。

次のようなタスクがサポートされます。

* 発行の設定のインポート。
* Azure Websites の作成および管理。
* Azure Virtual Machines の作成および管理。

サポートされているコマンドの一覧については、入力 `azure - ヘルプ` 、ツールをインストールした後コマンドラインでまたは参照してください、 [[リファレンス ドキュメント] のドキュメントを参照][reference-docs]します。

<h2>
            <a id="Download">Mac と Linux 用 Azure コマンド ライン ツールのインストール方法</a>
          </h2>

次の説明に従って、お使いのオペレーティング システムに対応するコマンド ライン ツールをインストールしてください。

* **Mac**: ダウンロード、 [Azure SDK インストーラー ][mac-installer]します。 ダウンロードした .pkg ファイルを開き、指示に従ってインストール手順を実行します。

* **Linux**: 最新バージョンのインストール [Node.js ][nodejs-org] (を参照してください [Node.js パッケージ マネージャーを使用して ][install-node-linux])、次のコマンドを実行します。

        npm install azure-cli -g

    **メモ**: 昇格した特権で次のコマンドを実行することが必要になる場合があります。

        sudo npm install azure-cli -g

* **Windows**: ここでは使用可能な Winows インストーラー (.msi ファイル) を実行します。 [Azure コマンド ライン ツール ][windows-installer]します。


インストールをテストするには、入力 `azure` コマンド プロンプト。 インストールが成功した場合、使用可能なすべての一覧が表示されます `azure` コマンドです。

<h2>
            <a id="CreateAccount"></a>
            Azure アカウントの作成方法
          </h2>

Mac および Linux 用 Azure コマンド ライン ツールを使用するには、Azure アカウントが必要です。

Web ブラウザーを開きを参照 [http://www.windowsazure.com ][windowsazuredotcom] ] をクリック **無料評価版** 右上隅にします。

![Azure Web サイト][azure web site]

指示に従ってアカウントの作成手順を実行します。

<h2>
            <a id="Account"></a>
            発行の設定をダウンロードおよびインポートする方法
          </h2>

最初に、発行の設定をダウンロードしてインポートする必要があります。 これにより、Azure サービスを作成および管理するためのツールを使用できるようになります。 ダウンロードする、発行の設定を使用して、 `アカウント ダウンロード` コマンド。

    azure account download

既定のブラウザーが開き、管理ポータルにサインインするよう促されます。 サインインした後、 `.publishsettings` ファイルがダウンロードされます。 ファイルを保存した場所をメモしておきます。

次に、インポート、 `.publishsettings` ファイルは、次のコマンドを実行して置き換える `{.publishsettings ファイルへのパス}` へのパスで、 `.publishsettings` ファイル。

    azure account import {path to .publishsettings file}

保存された情報のすべてを削除することができます、 <code>インポート</code> コマンドを使用する、 <code>クリア アカウント</code> コマンド。

    azure account clear

オプションの一覧を表示する `アカウント` コマンドを使用して、 `-ヘルプ` オプション。

    azure account -help

インポートした後、発行の設定を削除する必要があります、 `.publishsettings` セキュリティ強化のためのファイルです。
> [AZURE.NOTE] 発行の設定をインポートする場合は、内に、Azure サブスクリプションにアクセスするための資格情報が格納されて、 `ユーザー` フォルダーです。  `ユーザー` フォルダーは、オペレーティング システムによって保護されています。 ただし、これは推奨の暗号化に追加の手順を実行すること、 `ユーザー` フォルダーです。 そのためには、次の操作を行います。    
> 
> -Windows では、フォルダーのプロパティを変更または BitLocker を使用します。
> Mac で FileVault を有効に、フォルダーのです。
> -Ubuntu の場合、Encrypted Home ディレクトリ機能を使用します。 その他の Linux ディストリビューションにも同等の機能が用意されています。

これで、Azure Websites および Azure Virtual Machines を作成および管理する準備が整いました。

<h2>
            <a id="WebSites"></a>
            Azure の Web サイトを作成および管理する方法
          </h2>

### Web サイトの作成

Azure の web サイトを作成するには、まずという空のディレクトリを作成 `MySite` し、そのディレクトリを参照します。

次に、次のコマンドを実行します。

    azure site create MySite --git

このコマンドの出力には、新しく作成された Web サイトの既定 URL が含まれています。  `- Git` オプションでは、git を使用して両方のローカル アプリケーション ディレクトリと web サイトのデータ センターに git リポジトリを作成して、web サイトに発行することができます。 ローカル フォルダーが既に git リポジトリである場合、このコマンドを実行すると新しいリモートが既存のリポジトリに追加され、Web サイトのデータ センター内のリポジトリが指定されます。

実行できる、 `azure サイトを作成` コマンドには、次のオプションのいずれか。

* `--location [location name]`します。 このオプションを使用すると、Web サイトを作成するデータ センターの場所を指定できます (たとえば、"米国西部")。 このオプションを省略した場合、場所を選択するよう促されます。
* `- ホスト名 [カスタム ホスト名]`します。 このオプションを使用すると、Web サイトのカスタム ホスト名を指定できます。

次に、Web サイト ディレクトリにコンテンツを追加します。 通常の git フローを使用して (`git 追加`, 、`git コミット`) コンテンツをコミットします。 次の git コマンドを使用して、Web サイトのコンテンツを Azure にプッシュします。

    git push azure master

### GitHub からの発行の設定

GitHub リポジトリからの継続的な発行を設定するには、使用、 `- GitHub` サイトを作成するときのオプションします。

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

GitHub リポジトリのローカル クローンがある場合や、GitHub リポジトリへの単一のリモート参照を含むリポジトリがある場合、このコマンドを実行すると GitHub リポジトリ内のコードがサイトに自動的に発行されます。 それ以降、GitHub リポジトリにプッシュされたすべての変更がサイトに自動的に発行されます。

GitHub からの発行を設定すると、master 分岐が既定の分岐として使用されます。 別の分岐を指定するには、ローカル リポジトリから次のコマンドを実行します。

    azure site repository <branch name>

### アプリケーションの設定の構成

アプリケーション設定は、実行時にアプリケーションで使用できるキーと値のペアです。 Azure の Web サイトに設定すると、アプリケーションの設定の値により、サイトの Web.config ファイルで定義されたのと同じキーで設定が上書きされます。 Node.js アプリケーションと PHP アプリケーションの場合、アプリケーション設定を環境変数として使用できます。 次の例は、キーと値のペアを設定する方法を示しています。

    azure site config add <key>=<value> 

すべてのキー/値ペアの一覧を参照するには、次のコマンドを使用します。

    azure site config list 

または、キーがわかっていて、値を参照する場合は、次のコマンドを使用できます。

    azure site config get <key> 

既存のキーの値を変更する場合は、まず既存のキーをクリアして再度追加します。 クリア コマンドは次のとおりです。

    azure site config clear <key> 

### サイトの一覧表示と表示

Web サイトの一覧を表示するには、次のコマンドを使用します。

    azure site list

サイトに関する詳細な情報を取得する、 `サイトの表示` コマンドです。 次の例の詳細を表示 `MySite`:

    azure site show MySite

### サイトの停止、開始、または再起動

停止、開始、またはサイトを再起動すると、 `サイト stop`, 、`開始をサイト`, 、または `サイトの再起動` コマンド。

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

### サイトの削除

最後でサイトを削除することができます、 `サイトの削除` コマンド。

    azure site delete MySite

実行したフォルダー内のいずれかのコマンドの上で実行する場合は、 `サイト作成`, 、サイト名を指定する必要はありません `MySite` 最後のパラメーターとして。

完全な一覧を表示する `サイト` コマンドを使用して、 `-ヘルプ` オプション。

    azure site -help 

<h2>
            <a id="VMs"></a>
            Azure の仮想マシンを作成および管理する方法
          </h2>

Azure の仮想マシンを作成するには、自分で用意した仮想マシン イメージ (.vhd ファイル) やイメージ ギャラリーから入手した仮想マシン イメージを使用します。 使用可能なイメージを表示する、 `vm イメージ リスト` コマンド。

    azure vm image list

プロビジョニングおよび使用可能なイメージの 1 つから仮想マシンの起動、 `仮想マシンの作成` コマンドです。 次の例は、Linux 仮想マシンを作成する方法を示しています (と呼ばれる `myVM`) (CentOS 6.2) のイメージ ギャラリー内のイメージからです。 ルート ユーザー名と仮想マシンのパスワードが `myusername` と `mypassw0rd です` それぞれします。 (ことに注意してください、 `-場所` パラメーター、仮想マシンを作成するデータ センターを指定します。 省略した場合、 `-場所` パラメーター、場所を選択するように求められます)。

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

渡すことを検討することが、 `-ssh` フラグ (Linux) または `--rdp` フラグ (Windows) を `仮想マシンの作成` を新しく作成された仮想マシンへのリモート接続を有効にします。

.Vhd ファイルからイメージを作成するにはカスタム イメージから仮想マシンをプロビジョニングする場合、 `vm イメージを作成` コマンドを使用して、 `仮想マシンの作成` 仮想マシンをプロビジョニングするコマンドです。 次の例は、Linux イメージを作成する方法を示しています (と呼ばれる `myImage`)、ローカル .vhd ファイルからです。 (、 `-場所` パラメーター、画像が格納されているデータを指定します)。

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

イメージをローカル .vhd から作成する代わりに、Azure BLOB ストレージに格納されている .vhd からイメージを作成できます。 これを行うことができます、 `-blob-url` パラメーター。

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

イメージを作成した後を使用して、イメージから仮想マシンをプロビジョニングできます `仮想マシンの作成`します。 次のコマンドと呼ばれるバーチャル マシンを作成する `myVM` 上記で作成したイメージから (`myImage`)。

    azure vm create myVM myImage myusername --location "West US"

仮想マシンをプロビジョニングした後は、たとえば、エンドポイントを作成して、仮想マシンにリモート アクセスできるようにすることができます。 次の例では、 `vm エンドポイントを作成する` で外部ポート 22 とローカル ポート 22 を開くコマンド `myVM`:

    azure vm endpoint create myVM 22 22

(IP アドレス、DNS 名、およびエンドポイント情報を含む) の仮想マシンに関する詳細情報を取得する、 `vm ショー` コマンド。

    azure vm show myVM

仮想マシンをシャットダウン、起動、または再起動するには、次のいずれかのコマンドを使用します。

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

最後に、使用して VM を削除して、 `vm の削除` コマンド。

    azure vm delete myVM

作成して、仮想マシンを管理するためのコマンドの完全な一覧を表示、 `-h` オプション。

    azure vm -h


[azure web site]: ./media/crossplat-cmd-tools/freetrial.png 
[nodejs-org]: http://nodejs.org/ 
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager 
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249 
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464 
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246 
[windowsazuredotcom]: http://www.windowsazure.com 

