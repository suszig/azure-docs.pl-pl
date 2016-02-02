<properties
    pageTitle="CLI から Azure へのログイン | Microsoft Azure"
    description="Mac、Linux、および Windows 用の Azure コマンドライン インターフェイス (Azure CLI) からの Azure サブスクリプションへの接続"
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/30/2015"
    ms.author="danlep"/>


# Azure コマンド ライン インターフェイス (Azure CLI) からの Azure サブスクリプションへの接続

Azure CLI は、Azure プラットフォームで使用できるオープン ソース、クロスプラットフォームのコマンド群です。 この記事では、すべての CLI コマンドを使用できるように、Azure CLI から Azure サブスクリプションに接続する方法について説明します。 CLI をインストールしていない場合は、次を参照してください。 [Azure CLI をインストール](xplat-cli-install.md)します。


[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-both-include.md)]


Azure CLI からサブスクリプションに接続する方法には、次の 2 つがあります。

* **職場または学校のアカウント ID か、Microsoft アカウント ID を使用して Azure にログインする** - この方法では、どちらかの種類のアカウント ID を使用して認証を行います。 最新の CLI では、多要素認証が有効になっているアカウントの対話型認証もサポートしています。 対話形式でログインした後は、リソース マネージャーを使用することも、従来の (サービス管理) コマンドを使用することもできます。

* **発行設定ファイルをダウンロードして使用する** - この方法では、管理タスクを実行できるようにするための証明書がローカル コンピューターにインストールされます。ただし、サブスクリプションと証明書が有効であることが必要です。 この方法では、従来の (サービス管理) コマンドしか使用できません。

認証とサブスクリプションの管理の詳細については、次を参照してください。 [アカウント ベースの認証と証明書ベースの認証 ][authandsub]します。

Azure アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、「 [Azure 無料評価版 ][free-trial]します。
>[AZURE.NOTE] 使用することができますバージョン 0.9.10 より前は、Azure CLI のバージョンを使用している場合、 `azure ログイン` 作業でのみコマンドまたは学校アカウントの id です。Microsoft アカウントの id は機能しません。 ただし、アカウントに、対話型ログインに任意の id を使用できます `azure ログイン` 以降では、Azure CLI バージョン 0.9.10 コマンドです。
>
CLI バージョン 0.9.9 以降は、多要素認証をサポートしています。



## 対話型のログイン方法の使用

使用して、 `azure ログイン` コマンド - いずれかで対話形式での認証に--引数なし。

- 多要素認証を必要とする職場または学校のアカウント ID
- Microsoft アカウント ID (リソース マネージャー デプロイメント モード機能にアクセスする場合)

> [AZURE.NOTE]  どちらの場合も、認証と承認は Azure Active Directory を使用して行われます。 Microsoft アカウントの ID を使用する場合、ログ プロセスは Azure Active Directory の既定のドメインにアクセスします (無料試用版にサインアップした場合は、Azure Active Directory によってアカウントの既定のドメインが作成されていることに気づかないことがあります)。

対話的ログオンすることは簡単: 型 `azure ログイン` 次のように、画面の指示に従います。

    azure login                                                                                                                                                                                         
    info:    Executing command login
    info:    To sign in, use a web browser to open the page http://aka.ms/devicelogin. Enter the code XXXXXXXXX to authenticate. If you're signing in as an Azure AD application, use the --username and --password parameters.

コードのコピーは前、に、提供し、http://aka.ms/devicelogin にブラウザーを開きます。 コードを入力すると、使用する ID のユーザー名とパスワードを入力するように求められます。 このプロセスが完了すると、コマンド シェルによってログイン プロセスが完了します。 次のような内容が表示されます。

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

## 職場または学校のアカウントでの非対話型ログインの使用

非対話型のログイン方法は、職場または学校のアカウント (*組織アカウント*とも呼ばれます) でのみ機能します。 このアカウントは組織によって管理され、組織の Azure Active Directory で定義されます。 実行できます [組織アカウントを作成する](#create-an-organizational-account) 場合は、1 つはないかを実行できます [著作物を作成または学校の ID を Microsoft アカウントの id から](./virtual-machines/resource-group-create-work-id-from-personal.md)します。 これは、指定する必要がいずれかのユーザー名またはユーザー名とパスワードを `azure ログイン` コマンドを次のようにします。

    azure login -u ahmet@contoso.onmicrosoft.com
    info:    Executing command login
    Password: *********
    |info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

パスワードを求められたら、入力します。

    If this is your first time logging in with these credentials, you are asked to verify that you wish to cache an authentication token. This prompt also occurs if you have previously used the `azure logout` command (described below). To bypass this prompt for automation scenarios, run `azure login` with the `-q` parameter.

* **ログアウト**するには、次のコマンドを使用します。

        azure logout -u <username>

    アカウントに関連付けられたサブスクリプションが Active Directory のみを使用して認証された場合は、ログアウトするとローカル プロファイルからサブスクリプション情報が削除されます。 ただし、サブスクリプション用に発行設定ファイルもインポートされている場合は、ログアウトするとローカル プロファイルから Active Directory 関連の情報のみが削除されます。

## 発行設定ファイルによる方法の使用

従来の (サービス管理) CLI コマンドを使用するだけで十分である場合は、発行設定ファイルを使用して接続できます。

* アカウントの**発行設定ファイルをダウンロード**するには、次のコマンドを使用します。

        azure account download


既定のブラウザーが開きにサインインするように求められます、 [Azure クラシック ポータル ][portal]します。 サインインした後、 `.publishsettings` ファイルのダウンロードです。 ファイルを保存した場所をメモしておきます。

    > [AZURE.NOTE] If your account is associated with multiple Azure Active Directory tenants, you may be prompted to select which Active Directory you wish to download a publish settings file for.
    >
    > Once selected using the download page, or by visiting the Azure classic portal, the selected Active Directory becomes the default used by the classic portal and download page. Once a default has been established, you will see the text '__click here to return to the selection page__' at the top of the download page. Use the provided link to return to the selection page.

* **発行設定ファイルをインポートするには**、次のコマンドを実行します。

        azure account import <path to your .publishsettings file>

    インポートした後、発行の設定を削除する必要があります、 `.publishsettings` ファイル、Azure CLI では、不要になったあり、セキュリティ上のリスクをサブスクリプションにアクセスするために使用できるようします。


## 複数のサブスクリプション

複数の Azure サブスクリプションがある場合は、Azure に接続すると、資格情報に関連付けられているすべてのサブスクリプションへのアクセスが許可されます。 1 つのサブスクリプションが既定として選択され、操作の実行時に Azure CLI によって使用されます。 サブスクリプションだけでなく、既定値は、どれを表示することができますを使用して、 `azure アカウント一覧` コマンドです。 このコマンドでは、次のような情報が返されます。

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

上のリストでは、**Current** 列に現在の既定のサブスクリプションとして Azure-sub-1 が表示されています。 既定のサブスクリプションを変更するには、使用、 `azure アカウントが設定` コマンドを使用し、既定にするサブスクリプションを指定します。 次に例を示します。

    azure account set Azure-sub-2

この結果、既定のサブスクリプションは Azure-sub-2 に変更されます。
> [AZURE.NOTE] 既定のサブスクリプションの変更は、直ちにグローバルで適用されます。新しい Azure CLI コマンドを実行するときには、同じコマンドライン インスタンスからでも別のインスタンスからでも、この新しい既定のサブスクリプションが使用されます。

Azure CLI で既定以外のサブスクリプションを使用する、現在の既定値を変更したくない場合は、使用、 `--サブスクリプション` コマンドのオプションを選択し、操作に使用するサブスクリプションの名前を指定します。

Azure サブスクリプションに接続すると、その Azure CLI コマンドの使用を開始することができます。

## CLI 設定の格納

CLI プロファイルとログが格納されている職場または学校のアカウントでログインするか、発行設定をインポートするかどうか、 `.azure` ディレクトリの場所、 `ユーザー` ディレクトリ。  `ユーザー` ディレクトリはオペレーティング システムによって保護されます。 ただし、これをお勧めの暗号化に追加の手順を実行すること、 `ユーザー` ディレクトリ。 そのためには、次の操作を行います。

* Windows の場合、ディレクトリ プロパティを変更するか、または BitLocker を使用します。
* Mac の場合、ディレクトリに対して FileVault を有効にします。
* Ubuntu の場合、Encrypted Home ディレクトリ機能を使用します。 その他の Linux ディストリビューションにも同様の機能が用意されています。

## その他のリソース

* [サービス管理 (クラシック) コマンド ][cliasm]

* [[Cliarm] リソース マネージャーのコマンドで Azure CLI の使用][cliarm]

* 詳細については、Azure CLI、ソース コードのダウンロード、問題の報告や、プロジェクトへの協力、 [GitHub リポジトリの Azure CLI](https://github.com/azure/azure-xplat-cli)します。

* Azure CLI または Azure を使用して問題が発生した場合にアクセスして、 [Azure フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/home)します。






[authandsub]: http://msdn.microsoft.com/library/windowsazure/hh531793.aspx#BKMK_AccountVCert 
[free-trial]: http://azure.microsoft.com/en-us/pricing/free-trial/ 
[portal]: https://manage.windowsazure.com 
[signuporg]: http://azure.microsoft.com/en-us/documentation/articles/sign-up-organization/ 
[cliasm]: virtual-machines/virtual-machines-command-line-tools.md 
[cliarm]: xplat-cli-azure-resource-manager.md 

