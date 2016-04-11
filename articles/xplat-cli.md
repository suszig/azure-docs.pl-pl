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

Azure CLI には `azure` コマンドを使用してアクセスします。 使用できるコマンドの一覧を表示するには、`azure` コマンドをパラメーターなしで使用します。 次のようなヘルプ情報が表示されます。

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

ここに表示されているトップ レベルのコマンドには、Azure の特定の領域で機能するコマンドが含まれています。 たとえば、`azure account` コマンドには、以前使用した `download` や `import` 設定など、Azure サブスクリプションに関連するコマンドが含まれています。 使用できるコマンドおよびオプションについては、[Mac、Linux、Windows 用の Azure CLI の使用] を参照してください。

ほとんどのコマンドは `azure <command> <operation> [parameters]` という形式で、サービスやアカウント構成などのオブジェクトに対する操作を行います。 その他のコマンドにはサブコマンドがあり、`azure <command> <subcommand> <operation> [parameters]` という形式で指定します。 次のコマンド例は、アカウント構成に対する操作です。

* インポート済みのサブスクリプションを表示する。

        azure account list

* 複数のサブスクリプションをインポート済みで、1 つのサブスクリプションを既定に設定する。

        azure account set <subscription>

`--help` または `-h` パラメーターを使用すると特定のコマンドに対するヘルプを表示できます。 または、`azure help [command] [options]` の形式で指定しても、同じ情報が返されます。 たとえば、次のコマンドでは、すべて同じ情報が返されます。

    azure account set --help

    azure account set -h

    azure help account set

コマンドに必要なパラメーターがわからない場合は、`--help`、`-h`、または `azure help [command]` を使用してヘルプを参照してください。

### 構成モードの設定

Azure CLI では、個別の管理操作を実行することができます _リソース_, 、データベース サーバー、データベース、または web サイトなどのユーザー管理のエンティティであります。 これは、既定の Azure CLI の動作モードと呼ばれます **Azure サービス管理**します。 ただし、複数のリソースで構成された複雑なソリューションがあるときは、ソリューション全体を 1 つの論理単位として管理できるようにすると便利です。

リソースの 1 つの論理単位としてのグループの管理をサポートするために、または _リソース グループ_, のプレビューが導入されています、 **リソース マネージャー** Azure リソースを管理する新しい方法として。

>[AZURE.NOTE] リソース マネージャーでは、現在はプレビュー段階にあり、Azure サービス管理と同じレベルの管理機能を提供できません。

新しいリソース マネージャーをサポートするために、Azure CLI では、`azure config mode` コマンドを使用して、これらの管理モードを切り替えることができます。

Azure CLI は、既定では Azure サービス管理モードに設定されています。 リソース マネージャー モードに切り替えるには、次のコマンドを使用します。

    azure config mode arm

Azure サービス管理モードに戻すには、次のコマンドを使用します。

    azure config mode asm

>[AZURE.NOTE] リソース マネージャー モードと Azure サービス管理モードは互いに排他的です。 つまり、どちらか一方のモードで作成されたリソースは、他方のモードは管理できません。

リソース マネージャーでは、Azure CLI の使用の詳細については、次を参照してください。 [リソース マネージャーで Azure CLI を使用して][cliarm]します。

### Azure サービス管理モードでのサービスの操作

Azure CLI を使用すると、Azure サービスを簡単に管理できます。 この例では、Azure CLI を使用して Azure Web サイトを管理する方法について説明します。

1. 新しい Azure Web サイトを作成するには、次のコマンドを使用します。 置換 **mywebsite** に一意の名前。

        azure site create mywebsite

    Web サイトが作成される場所を指定するように求められます。 地理的に近いリージョンを選択します。 Web サイトが http://mywebsite.azurewebsites.net で使用するこのコマンドが完了後 (置換 **mywebsite** 指定した名前にします)。

    > [AZURE.NOTE] プロジェクト ソース管理に Git を使用するかどうか、設定できる、 `--git` パラメーターがこの web サイトの Azure を Git リポジトリを作成します。 Git リポジトリがないディレクトリからこのコマンドを実行した場合、Git リポジトリの再初期化も行われます。 という名前の Git リモートも作成されます __azure__, 、展開をプッシュするを使用して、Azure の web サイトに使用できる、 `git push azure master` コマンドです。

    > [AZURE.NOTE] 'Site' が azure のコマンドではないエラーが発生した場合、Azure CLI は高い確率でリソース グループ モードです。 リソース モードに戻すには、`azure config mode asm` コマンドを使用してください。

2. サブスクリプションのすべての Web サイトを表示するには、次のコマンドを使用します。

        azure site list

    このリストには、前の手順で作成したサイトが含まれます。

2. Web サイトを停止するには次のコマンドを使用します。 置換 **mywebsite** 、サイト名。

        azure site stop mywebsite

    コマンドが完了したら、ブラウザーを更新するとサイトが停止したことを確認できます。

3. Web サイトを開始するには次のコマンドを使用します。 置換 **mywebsite** 、サイト名。

        azure site start mywebsite

    コマンドが完了したら、ブラウザーを更新するとサイトが開始したことを確認できます。

4. Web サイトを削除するには次のコマンドを使用します。 置換 **mywebsite** 、サイト名。

        azure site delete mywebsite

    コマンドが完了したら、`azure site list` コマンドを使用して、Web サイトが削除されていることを確認します。

<a id="script"></a>
## Mac、Linux、Windows 用の Azure CLI をコード化する方法

Azure CLI を使用して手動でコマンドを発行できますが、お使いのシステムで使用できるコマンド ライン インタープリターやその他のコマンド ライン ユーティリティの機能を利用して、複雑な自動化ワークフローを作成することもできます。 たとえば、次のコマンドはすべての Azure Websites を停止します。

    azure site list | grep 'Running' | awk '{system("azure site stop "$2)}'

この例では、`grep` コマンドに Web サイトのリストをパイプして、文字列 "Running" について各行を調べています。 一致した行は `awk` コマンドにパイプされます。`azure site stop` が呼び出され、渡された 2 番目の列 (実行中のサイト名) が、停止するサイト名に使用されます。

これは、コマンドの連結方法を示した例ですが、コマンド ライン インタープリターのスクリプト機能を使用して、さらに複雑なスクリプトを作成することもできます。 コマンドライン インタープリターの種類ごとに、スクリプト機能や構文が異なります。 Linux および OS X を含め、UNIX ベースのシステム用には Bash が最も広く使用されているコマンド ライン インタープリターでしょう。

Bash でのスクリプトについては、次を参照してください。、 [Advanced Bash-scripting Guide][advanced-bash]します。

OS X または Linux ベース システムのスクリプト作成に関する一般的な情報を参照してください。 [シェル スクリプト][script]します。

バッチ ファイルを使用して Windows ベースのシステムのスクリプトについては、次を参照してください。 [コマンド ライン リファレンス A ~ Z][batch]します。

### 結果について

スクリプトの作成時、コマンド出力をキャプチャしたり、出力を別のコマンドに渡したり、出力に特別な操作 (特定の値の確認など) を実行したりすることがあります。 Azure CLI は出力を STDOUT および STDERR に生成します。 各行にはプレフィックスとして、情報ステータス メッセージの場合は文字列 `info:` が、サービスに関して返されるデータの場合は `data:` がそれぞれ付加されますが、`--verbose` または `-v` パラメーターを使用すると、より詳細な情報を返すよう Azure CLI に指示できます。 これらのパラメーターで返される追加の情報には、文字列 `verbose:` がプレフィックスとして付加されます。

たとえば、次の出力は `azure site list` コマンドで返された情報です。

    info:    Executing command site list
    + Enumerating sites
    data:    Name           Status   Mode  Host names
    data:    -------------  -------  ----  -------------------------------
    data:    myawesomesite  Running  Free  myawesomesite.azurewebsites.net
    info:    site list command OK

`--verbose` または `-v` パラメーターを指定すると、次のような情報が返されます。

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

`verbose:` の情報は、JSON 形式のデータです。 使用することができます、 `--json` など、JSON をネイティブで認識するユーティリティを使用している場合は、JSON 形式で情報を返すパラメーター [jsawk](https://github.com/micha/jsawk) または [jq](http://stedolan.github.io/jq/)します。 次に例を示します。

    azure site list --json | jsawk -n 'out(this.Name)' | xargs -L 1 azure site delete -q

このコマンドは Web サイトのリストを JSON として取得してから jsawk を使用してサイト名を取得し、最後に xargs を使用して、各サイトの削除コマンドを実行します。このサイト名はパラメーターとして渡されています。

>[AZURE.NOTE]  `--json` パラメーターは、ステータスやデータの情報の生成をブロック (文字列が付けた `info:` と `data:`)。 たとえば、`--json` パラメーターを `azure site create` コマンドで使用した場合、返される出力はありません。このコマンドでは `info:` 以外のデータを返さないためです。

### エラーの操作

Azure CLI では、エラー情報を STDERR にログ、エラーに関する追加情報が記録されます、 **azure.err** から、ディレクトリにあるファイルが、スクリプトを実行します。 このファイルに情報が記録された場合、次のような文字列が STDOUT に書き込まれます。

    info:    Error information has been recorded to azure.err

### 終了ステータス

必須パラメーターが不足している場合、一部の Azure CLI コマンドは 0 以外の終了ステータスを返しません。 代わりに、ユーザーの入力が要求されます。 たとえば、Web サイトを新規作成する `azure site create` コマンドを使用したとき、名前または `--location` パラメーターがない場合は、これらの値を指定するように求められます。

終了ステータスに依存するスクリプトを作成する場合は、使用する Azure CLI コマンドがユーザー入力を求めないことを確認してください。

<a id="additional-resources"></a>

## その他のリソース

* [サービス管理での Azure CLI の使用][Using the Azure CLI]

* [リソース マネージャーでの Azure CLI の使用][cliarm]

* Azure CLI の詳細についてはソース コード、問題の報告をダウンロードし、プロジェクトへの協力を参照してください、 [GitHub リポジトリの Azure CLI](https://github.com/azure/azure-xplat-cli)します。

* Azure CLI または Azure を使用して問題が発生した場合にアクセスして、 [Azure フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/home)します。

* Azure の詳細については、次を参照してください。 [http://azure.microsoft.com/](http://azure.microsoft.com)します。




[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/?linkid=9828653&clcid=0x409
[authandsub]: http://msdn.microsoft.com/library/windowsazure/hh531793.aspx#BKMK_AccountVCert

[Azure Web Site]: ../media/freetrial.png
[select a preview feature]: ../media/antares-iaas-preview-02.png
[select subscription]: ../media/antares-iaas-preview-03.png
[free-trial]: http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E
[advanced-bash]: http://tldp.org/LDP/abs/html/
[script]: http://en.wikipedia.org/wiki/Shell_script
[batch]: http://technet.microsoft.com/library/bb490890.aspx
[cliarm]: xplat-cli-azure-resource-manager.md
[portal]: https://manage.windowsazure.com
[signuporg]: http://www.windowsazure.com/documentation/articles/sign-up-organization/
[Using the Azure CLI]: virtual-machines-command-line-tools.md

