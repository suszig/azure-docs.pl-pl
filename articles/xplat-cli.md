<properties
    pageTitle="Mac、Linux、Windows 用の Azure CLI"
    description="Mac、Linux、Windows 用の Azure CLI をインストールおよび構成して Azure Services を管理する"
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    authors="squillace"
    services=""/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/10/2015"
    ms.author="rasquill"/>


# Azure CLI のインストールと構成

> [AZURE.SELECTOR]
- [PowerShell](powershell-install-configure.md)
- [Azure CLI](xplat-cli-install.md)


Azure CLI は、Azure Platform で使用できるオープン ソース、クロスプラットフォームのコマンド群です。 Azure CLI で使用できる機能の多くは、Azure ポータルで使用できるもの (たとえば、Web サイト、仮想マシン、モバイル サービス、SQL Database などの Azure Platform で提供されるサービスを管理する機能) と同じです。

Azure CLI は JavaScript で記述されており、Node.js が必要です。 Azure SDK for Node.js を使用して実装されており、Apache 2.0 ライセンスに基づいてリリースされています。 プロジェクトのリポジトリは [https://github.com/azure/azure-xplat-cli](https://github.com/azure/azure-xplat-cli)します。

このドキュメントでは、Mac、Linux、Windows 用の Azure CLI をインストールして構成する方法、およびこれを使用して Azure Platform で基本的なタスクを実行する方法について説明します。

<a id="install"></a>
## 方法: Azure CLI をインストールする

Azure CLI のインストールの手順の詳細は、参照、 [Azure CLI をインストール](xplat-cli-install.md) ページです。


<a id="configure"></a>
## 方法: Azure サブスクリプションに接続する

Azure CLI の一部のコマンドは Azure サブスクリプションがなくても機能しますが、多くのコマンドにはサブスクリプションが必要です。 Azure CLI がサブスクリプションで動作するを構成する方法については、次を参照してください。 [Azure CLI から Azure サブスクリプションへの接続](xplat-cli-connect.md)します。


<a id="use"></a>
## Azure CLI の使い方

使用して Azure CLI をアクセス、 `azure` コマンドです。 使用できるコマンドの一覧を表示する、 `azure` コマンドをパラメーターなし。 次のようなヘルプ情報が表示されます。

    info:             _    _____   _ ___ ___
    info:            /_\  |_  / | | | _ \ __|
    info:      _ ___/ _ \__/ /| |_| |   / _|___ _ _
    info:    (___  /_/ \_\/___|\___/|_|_\___| _____)
    info:       (_______ _ _)         _ ______ _)_ _
    info:              (______________ _ )   (___ _ _)
    info:
    info:    Microsoft Azure: Microsoft's Cloud Platform
    info:
    info:    Tool version 0.8.10
    help:
    help:    Display help for a given command
    help:      help [options] [command]
    help:
    help:    Opens the portal in a browser
    help:      portal [options]
    help:
    help:    Commands:
    help:      account        Commands to manage your account information and publish settings
    help:      config         Commands to manage your local settings
    help:      hdinsight      Commands to manage your HDInsight accounts
    help:      mobile         Commands to manage your Mobile Services
    help:      network        Commands to manage your Networks
    help:      sb             Commands to manage your Service Bus configuration
    help:      service        Commands to manage your Cloud Services
    help:      site           Commands to manage your Web Sites
    help:      sql            Commands to manage your SQL Server accounts
    help:      storage        Commands to manage your Storage objects
    help:      vm             Commands to manage your Virtual Machines
    help:
    help:    Options:
    help:      -h, --help     output usage information
    help:      -v, --version  output the application version

ここに表示されているトップ レベルのコマンドには、Azure の特定の領域で機能するコマンドが含まれています。 たとえば、 `azure アカウント` コマンドなど、Azure サブスクリプションに関連するコマンドに含まれる、 `ダウンロード` と `インポート` 以前使用した設定します。 使用できるコマンドおよびオプションについては、[Mac、Linux、Windows 用の Azure CLI の使用] を参照してください。

ほとんどのコマンドがパーセント形 `azure < コマンド >< 操作 > [パラメーター]` サービスやアカウント構成などのオブジェクトに対する操作を行います。その他のコマンドのサブコマンドし、形式に従う `azure < コマンド >< サブコマンド >< 操作 > [パラメーター]`します。次のコマンド例は、アカウント構成に対する操作です。

* インポート済みのサブスクリプションを表示する。

        azure account list

* 複数のサブスクリプションをインポート済みで、1 つのサブスクリプションを既定に設定する。

        azure account set <subscription>


`--ヘルプ` または `-h` パラメーターは、特定のコマンドに対するヘルプを表示を使用することができます。 または、 `azure help [コマンド] [オプション]` 形式は、同じ情報を返す場合にも使用できます。 たとえば、次のコマンドでは、すべて同じ情報が返されます。

    azure account set --help
    
    azure account set -h
    
    azure help account set

コマンドに必要なパラメーターは確かでない場合を使用してヘルプを参照してください `--ヘルプ`, 、`-h` または `azure help [コマンド]`します。

### 構成モードの設定

Azure CLI では、データベース サーバー、データベース、Web サイトなど、ユーザー管理のエンティティである個々の_リソース_に対して管理操作を実行できます。 これは Azure CLI の既定の動作モードであり、**Azure サービス管理**と呼ばれています。 ただし、複数のリソースで構成された複雑なソリューションがあるときは、ソリューション全体を 1 つの論理単位として管理できるようにすると便利です。

サポートとして 1 つの論理単位 _resource とリソースのグループを管理するためのプレビューが導入されています、 **リソース マネージャー** Azure リソースを管理する新しい方法として。
>[AZURE.NOTE] リソース マネージャーでは、現在はプレビュー段階にあり、Azure サービス管理と同じレベルの管理機能を提供できません。

新しいリソース マネージャーをサポートするために Azure CLI を使用すると、これらの管理モードを使用して切り替える、 `azure config モード` コマンドです。

Azure CLI は、既定では Azure サービス管理モードに設定されています。 リソース マネージャー モードに切り替えるには、次のコマンドを使用します。

    azure config mode arm

Azure サービス管理モードに戻すには、次のコマンドを使用します。

    azure config mode asm

>[AZURE.NOTE] リソース マネージャー モードと Azure サービス管理モードは互いに排他的です。 つまり、どちらか一方のモードで作成されたリソースは、他方のモードは管理できません。

リソース マネージャーでは、Azure CLI の使用の詳細については、次を参照してください。 [リソース マネージャー ][cliarm]します。

### Azure サービス管理モードでのサービスの操作

Azure CLI を使用すると、Azure サービスを簡単に管理できます。 この例では、Azure CLI を使用して Azure Web サイトを管理する方法について説明します。

1. 新しい Azure Web サイトを作成するには、次のコマンドを使用します。 **mywebsite** は、一意の名前に置き換えてください。

        azure site create mywebsite

    Web サイトが作成される場所を指定するように求められます。 地理的に近いリージョンを選択します。 Web サイトが http://mywebsite.azurewebsites.net で使用するこのコマンドが完了後 (置換 **mywebsite** 指定した名前にします)。
    > [AZURE.NOTE] プロジェクト ソース管理に Git を使用するかどうか、設定できる、 `- git` パラメーターがこの web サイトの Azure を Git リポジトリを作成します。 Git リポジトリがないディレクトリからこのコマンドを実行した場合、Git リポジトリの再初期化も行われます。 という名前の Git リモートも作成されます __azure__, 、展開をプッシュするを使用して、Azure の web サイトに使用できる、 `git push azure master` コマンドです。

    > [AZURE.NOTE] "site" が Azure のコマンドではないというエラーが発生した場合、高い確率で Azure CLI がリソース グループ モードになっています。 リソース モードに戻すを変更するには、使用、 `azure config モード asm` コマンドです。

2. サブスクリプションのすべての Web サイトを表示するには、次のコマンドを使用します。

        azure site list

    このリストには、前の手順で作成したサイトが含まれます。

2. Web サイトを停止するには次のコマンドを使用します。 **mywebsite** は、サイト名に置き換えてください。

        azure site stop mywebsite

    コマンドが完了したら、ブラウザーを更新するとサイトが停止したことを確認できます。

3. Web サイトを開始するには次のコマンドを使用します。 **mywebsite** は、サイト名に置き換えてください。

        azure site start mywebsite

    コマンドが完了したら、ブラウザーを更新するとサイトが開始したことを確認できます。

4. Web サイトを削除するには次のコマンドを使用します。 **mywebsite** は、サイト名に置き換えてください。

        azure site delete mywebsite

    コマンドが完了するを使用して、 `azure サイトの一覧` コマンドを web サイトが存在しないことを確認します。

<a id="script"></a>
## Mac、Linux、Windows 用の Azure CLI をコード化する方法

Azure CLI を使用して手動でコマンドを発行できますが、お使いのシステムで使用できるコマンド ライン インタープリターやその他のコマンド ライン ユーティリティの機能を利用して、複雑な自動化ワークフローを作成することもできます。 たとえば、次のコマンドはすべての Azure Websites を停止します。

    azure site list | grep 'Running' | awk '{system("azure site stop "$2)}'

この例の web サイトのリストをパイプ、 `grep` 各行を調べて、文字列"Running"コマンドです。 一致した行はパイプが、 `awk` コマンドを呼び出し `azure サイトの停止` を停止するサイト名 (実行中のサイト名) に渡される 2 番目の列を使用しています。

これは、コマンドの連結方法を示した例ですが、コマンド ライン インタープリターのスクリプト機能を使用して、さらに複雑なスクリプトを作成することもできます。 コマンドライン インタープリターの種類ごとに、スクリプト機能や構文が異なります。 Linux および OS X を含め、UNIX ベースのシステム用には Bash が最も広く使用されているコマンド ライン インタープリターでしょう。

Bash でのスクリプトについては、次を参照してください。、 [高度な Bash スクリプト ガイド ][advanced-bash]します。

OS X または Linux ベース システムのスクリプト作成に関する一般的な情報を参照してください。 [シェル スクリプトの ][script]します。

バッチ ファイルを使用して Windows ベースのシステムのスクリプトについては、次を参照してください。 [コマンド ライン リファレンス A ~ Z の ][batch]します。

### 結果について

スクリプトの作成時、コマンド出力をキャプチャしたり、出力を別のコマンドに渡したり、出力に特別な操作 (特定の値の確認など) を実行したりすることがあります。 Azure CLI は出力を STDOUT および STDERR に生成します。 各行は、文字列の先頭 `情報:` 情報ステータス メッセージの場合または `データ:` 、サービスに関して返されるデータのただし、あげて Azure CLI を使用して、詳細な情報を返す、 `-詳細` または `-v` パラメーター。 追加の情報がそれぞれ付加されますが、文字列が返されます `verbose:`します。

たとえばで、次の出力が返された、 `azure サイトの一覧` コマンド。

    info:    Executing command site list
    + Enumerating sites
    data:    Name           Status   Mode  Host names
    data:    -------------  -------  ----  -------------------------------
    data:    myawesomesite  Running  Free  myawesomesite.azurewebsites.net
    info:    site list command OK

場合、 `-詳細` または `-v` パラメーターを指定すると、次のような情報が返されます。

    info:    Executing command site list
    verbose: Subscription ####################################
    verbose: Enumerating sites
    verbose: [
    verbose:     {
    verbose:         ComputeMode: 'Shared',
    verbose:         HostNameSslStates: {
    verbose:             HostNameSslState: [
    verbose:                 {
    verbose:                     IpBasedSslResult: {
    verbose:                         $: { i:nil: 'true' }
    verbose:                     },
    verbose:                     SslState: 'Disabled',
    verbose:                     ToUpdateIpBasedSsl: {
    verbose:                         $: { i:nil: 'true' }
    verbose:                     },
    verbose:                     VirtualIP: {
    verbose:                         $: { i:nil: 'true' }
    verbose:                     },
    verbose:                     Thumbprint: {
    verbose:                         $: { i:nil: 'true' }
    verbose:                     },
    verbose:                     ToUpdate: {
    verbose:                         $: { i:nil: 'true' }
    verbose:                     },
    verbose:                     Name: 'myawesomesite.azurewebsites.net'
    verbose:                 },
    ...
    verbose:     }
    verbose: ]
    data:    Name           Status   Mode  Host names
    data:    -------------  -------  ----  -------------------------------
    data:    myawesomesite  Running  Free  myawesomesite.azurewebsites.net
    info:    site list command OK

注意してください、 `verbose:` 情報は、json 形式のデータが表示されます。 使用することができます、 `-json` など、JSON をネイティブで認識するユーティリティを使用している場合は、JSON 形式で情報を返すパラメーター [jsawk](https://github.com/micha/jsawk) または [jq](http://stedolan.github.io/jq/)します。 次に例を示します。

    azure site list --json | jsawk -n 'out(this.Name)' | xargs -L 1 azure site delete -q

このコマンドは Web サイトのリストを JSON として取得してから jsawk を使用してサイト名を取得し、最後に xargs を使用して、各サイトの削除コマンドを実行します。このサイト名はパラメーターとして渡されています。
>[AZURE.NOTE] `-Json` パラメーターは、ステータスやデータの情報の生成をブロック (文字列が付きます `情報:` と `データ:`)。 たとえば場合、 `-json` でパラメーターを使用、 `azure サイトを作成`, 、このコマンドはデータを返さない以外の場合、出力は返されません `情報:`します。

### エラーの操作

Azure CLI では、エラー情報を STDERR にログ出力しますが、エラーに関する追加情報が、スクリプトを実行したディレクトリにある **azure.err** ファイルに記録されます。 このファイルに情報が記録された場合、次のような文字列が STDOUT に書き込まれます。

    info:    Error information has been recorded to azure.err

### 終了ステータス

必須パラメーターが不足している場合、一部の Azure CLI コマンドは 0 以外の終了ステータスを返しません。 代わりに、ユーザーの入力が要求されます。 例についてを使用する場合、 `azure サイトを作成` サイト名がない場合は、新しい web サイトを作成するコマンドまたは `-場所` パラメーターを指定するこれらの値を指定するように求められます。

終了ステータスに依存するスクリプトを作成する場合は、使用する Azure CLI コマンドがユーザー入力を求めないことを確認してください。

<a id="additional-resources"></a>

## その他のリソース

* [[Azure cli の使用] サービス管理で Azure CLI の使用][using the azure cli]

* [リソース マネージャー ][cliarm]

* Azure CLI の詳細についてはソース コード、問題の報告をダウンロードし、プロジェクトへの協力を参照してください、 [GitHub リポジトリの Azure CLI](https://github.com/azure/azure-xplat-cli)します。

* Azure CLI または Azure を使用して問題が発生した場合にアクセスして、 [Azure フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/home)します。

* Azure の詳細については、次を参照してください。 [http://azure.microsoft.com/](http://azure.microsoft.com)します。





[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249 
[windows-installer]: http://go.microsoft.com/?linkid=9828653&clcid=0x409 
[authandsub]: http://msdn.microsoft.com/library/windowsazure/hh531793.aspx#BKMK_AccountVCert 
[azure web site]: ../media/freetrial.png 
[select a preview feature]: ../media/antares-iaas-preview-02.png 
[select subscription]: ../media/antares-iaas-preview-03.png 
[free-trial]: http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E 
[advanced-bash]: http://tldp.org/LDP/abs/html/ 
[script]: http://en.wikipedia.org/wiki/Shell_script 
[batch]: http://technet.microsoft.com/library/bb490890.aspx 
[cliarm]: xplat-cli-azure-resource-manager.md 
[portal]: https://manage.windowsazure.com 
[signuporg]: http://www.windowsazure.com/documentation/articles/sign-up-organization/ 
[using the azure cli]: virtual-machines-command-line-tools.md 

