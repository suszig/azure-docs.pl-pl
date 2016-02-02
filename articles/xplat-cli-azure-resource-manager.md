<properties
    pageTitle="リソース マネージャーでの Azure CLI | Microsoft Azure"
    description="Mac、Linux、Windows 用の Azure CLI を利用し、リソース グループとして複数のリソースをデプロイします。"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services="azure-resource-manager"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2015"
    ms.author="danlep"/>


# Azure リソース マネージャーでの、Mac、Linux、および Windows 用 Azure CLI の使用

> [AZURE.SELECTOR]
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)




この記事では、Azure リソース マネージャー モードで、Mac、Linux、Windows 用 Azure コマンドライン インターフェイス (CLI) を使用して、Azure リソースを作成し、管理する方法について説明します。
>[AZURE.NOTE] 作成し、コマンドラインでの Azure リソースの管理には、Azure アカウントが必要 ([無料評価版は、ここ](http://azure.microsoft.com/pricing/free-trial/))します。 必要がありますも [Azure CLI をインストール](xplat-cli-install.md), 、および [自分のアカウントに関連付けられている Azure のリソースを使用するログオン](xplat-cli-connect.md)します。 これらの操作を完了した場合は、使用する準備が整いました。

## Azure リソース

Azure リソース マネージャーを使用して、作成のグループを管理 _リソース_ (ユーザーによって管理されるバーチャル マシン、データベース サーバー、データベース、または web サイトなどのエンティティ) 1 つの論理単位 _resource とします。

Azure リソース マネージャーの長所を 1 つは、JSON *テンプレート*にリソースのデプロイ可能なグループの構造とリレーションシップを記述するという_宣言型_の方法で Azure のリソースを作成できることです。 テンプレートはパラメーターを特定します。そのパラメーターに値をコマンドの実行時にインラインで入力するか、別の JSON azuredeploy-parameters.json ファイルに保存できます。 これにより、同じテンプレートに異なるパラメーターを設定して、新しいリソースを簡単に作成できます。 たとえば、Web サイトを作成するテンプレートには、サイト名や Web サイトを配置するリージョンなどの共通的な設定を用意できます。

テンプレートを使用してグループを変更または作成すると、_デプロイ_が作成されてグループに適用されます。 Azure リソース マネージャーの詳細については、次を参照してください。、 [Azure リソース マネージャーの概要](../resource-group-overview.md)します。

デプロイを作成すると、従来の (サービス管理) デプロイ モデルと同様に、個々のリソースをコマンド ラインで強制的に管理できます。 たとえば、Azure リソース マネージャーの CLI コマンドを使用して開始、停止、またはなどのリソースを削除する [Azure リソース マネージャーの仮想マシン](../virtual-machines/virtual-machines-deploy-rmtemplates-azure-cli.md)します。

## 認証

Azure CLI を使用して Azure リソース マネージャーを操作するには、仕事か学校のアカウント (組織アカウント) か Microsoft アカウント (CLI バージョン 0.9.10 より) を使用して Microsoft Azure で認証する必要があります。 .publishsettings ファイルによってインストールされた証明書の場合、このモードでは認証できません。

Microsoft Azure への認証の詳細については、次を参照してください。 [Azure CLI から Azure サブスクリプションへの接続](xplat-cli-connect.md)します。
>[AZURE.NOTE] Azure Active Directory によって管理される職場か学校のアカウントを使用しするとき、Azure のロールベースの Access Control (RBAC) を使用して、Azure リソースへのアクセスと使用状況を管理することもできます。 詳細については、「 [の管理とリソースへのアクセスの監査](resource-group-rbac.md)します。

## Azure リソース マネージャー モードの設定

Azure リソース マネージャー モードは既定で無効になっているため、次のコマンドを使って Azure CLI リソース マネージャー コマンドを有効にしてください。

    azure config mode arm

>[AZURE.NOTE] Azure リソース マネージャー モードと Azure サービス管理モードは互いに排他的です。 つまり、どちらか一方のモードで作成されたリソースは、他方のモードは管理できません。

## 場所を見つける

ほとんどの Azure リソース マネージャー コマンドには、リソースを作成し、検索するための有効な場所が必要です。 次のコマンドを使用して、異なる Azure リソースに利用可能なすべての場所を検索できます。

    azure location list

これには Azure リソースと、「米国西部」や「米国東部」など、その Azure リソースが利用できる Azure リージョンが一覧表示されます。

## リソース グループの作成

リソース グループは、ネットワーク、ストレージ、その他のリソースの論理グループです。 Azure リソース マネージャー モードのほとんどすべてのコマンドにはリソース グループが必要です。 次のコマンドを使用して、たとえば _testRG_ という名前のリソース グループを作成することができます。

    azure group create -n "testRG" -l "West US"

その後、このグループへのリソースの追加を開始し、これを使用して新しい仮想マシンなどのリソースを構成できます。


## リソース グループ テンプレートの利用

### リソース グループ テンプレートの検索と構成

テンプレートを操作するときにするか [独自に作成](resource-group-authoring-templates.md), 、テンプレートは、のいずれかを使用、 [テンプレート ギャラリー](https://azure.microsoft.com/documentation/templates/), 、これでも使用可能な [GitHub](https://github.com/Azure/azure-quickstart-templates)します。

新しいテンプレートの作成はこの記事の範囲を超えて、ので最初に使用してみましょう、 _101 シンプル-vm-from image_ から使用できるテンプレート [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-linux-vm)します。 既定では、これにより新しい仮想ネットワークで Ubuntu 4.04.2-LTS 仮想マシンが 1 つ作成されます。リージョンは米国西部でサブネットが 1 つです。 次のパラメーターをいくつか指定すれば、このテンプレートを使用できます。

* 一意のストレージ アカウント名
* VM の管理者ユーザー名
* パスワード
* VM のドメイン名

>[AZURE.TIP] 以下の手順では、Azure CLI で VM テンプレートを使用する方法を 1 つだけ紹介します。 その他の例については、次を参照してください。 [展開 Azure リソース マネージャー テンプレートと Azure CLI を使用して仮想マシンの管理と](../virtual-machines/virtual-machines-deploy-rmtemplates-azure-cli.md)します。

1. ダウンロード ファイル azuredeploy.json およびから azuredeploy.parameters.json [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-linux-vm) 、ローカル コンピューター上の作業フォルダーにします。

2. テキスト エディターで azuredeploy.parameters.json ファイルを開き、お使いの環境に適したパラメーター値を入力します (**ubuntuOSVersion** 値は変更しないでください)。

        {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "newStorageAccountName": {
              "value": "MyStorageAccount"
            },
            "adminUsername": {
              "value": "MyUserName"
            },
            "adminPassword": {
              "value": "MyPassword"
            },
            "dnsNameForPublicIP": {
              "value": "MyDomainName"
            },
            "ubuntuOSVersion": {
              "value": "14.04.2-LTS"
            }
          }
        }

    ```
3. azuredeploy.parameters.json ファイルを保存した後、次のコマンドを使用して、テンプレートに基づく新しいリソース グループを作成します。  `-E` オプションは、前の手順で変更した azuredeploy.parameters.json ファイルを指定します。 *testRG* を使用するグループ名と置き換え、*testDeploy* を自分で選んだデプロイ名と置き換えます。 場所は、テンプレート パラメーター ファイルに指定されている場所と同じ場所にする必要があります。

        azure group create "testRG" "West US" -f azuredeploy.json -d "testDeploy" -e azuredeploy.parameters.json

    このコマンドでは、デプロイがアップロードされると OK と表示されますが、その時点ではまだグループのリソースにデプロイが適用されていません。

4. デプロイの状態を確認するには、次のコマンドを使用します。

        azure group deployment show "testRG" "testDeploy"

    **ProvisioningState** に、デプロイの状態が表示されます。

    デプロイが成功した場合は、次のような出力が表示されます。

        azure-cli@0.8.0:/# azure group deployment show testRG testDeploy
        info:    Executing command group deployment show
        + Getting deployments
        + Getting deployments
        data:    DeploymentName     : testDeploy
        data:    ResourceGroupName  : testRG
        data:    ProvisioningState  : Running
        data:    Timestamp          : 2015-10-26T16:15:29.5562024Z
        data:    Mode               : Incremental
        data:    Name                   Type          Value
        data:    ---------------------  ------------  ---------------------
        data:    newStorageAccountName  String        MyStorageAccount
        data:    adminUsername          String        MyUserName
        data:    adminPassword          SecureString  undefined
        data:    dnsNameForPublicIP     String        MyDomainName
        data:    ubuntuOSVersion        String        14.04.2-LTS
        info:    group deployment show command OK

    >[AZURE.NOTE] 構成が適切でないことがわかった場合、または実行時間の長いデプロイを停止する必要がある場合は、次のコマンドを使用します。
    >
    > `azure group 展開 stop"testRG""testDeploy"`
    >
    > デプロイ名を指定しない場合、テンプレート ファイル名に基づいてデプロイ名が自動的に作成されます。 出力の一部として返される、 `azure グループを作成` コマンドです。

    これで、指定したドメイン名を利用し、VM に SSH 接続できます。VM に接続をする必要がある場合、フォームの完全修飾ドメイン名を使用する `< domainName >. < 地域 >. cloudapp.azure.com`, など `MyDomainName.westus.cloudapp.azure.com`します。

5. グループを表示するには、次のコマンドを使用します。

        azure group show "testRG"

    このコマンドにより、グループ内のリソースに関する情報が返されます。 複数のグループがあればを使用して、 `azure グループの一覧` グループ名の一覧を取得し、使用するコマンド `azure グループの表示` 特定のグループの詳細を表示します。

直接テンプレートを使用することもできます。 [GitHub](https://github.com/Azure/azure-quickstart-templates), 、コンピューターにダウンロードする代わりにします。 その場合、コマンドに **--template-url** オプションを指定し、テンプレートの azuredeploy.json ファイルに URL を渡します。 URL を取得するには、GitHub で _raw_ モードで azuredeploy.json を開き、ブラウザーのアドレス バーに表示される URL をコピーします。 次のようにコマンドを使用し、この URL を使用してデプロイを直接作成できます。

    azure group deployment create "testDeploy" -g "testResourceGroup" --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json

必要なテンプレート パラメーターを入力するように求められます。
> [AZURE.NOTE] _raw_ モードで JSON テンプレートを開くことが重要です。 ブラウザーのアドレス バーに表示される URL は、通常モードで表示される URL とは異なります。 GitHub でファイルを表示するときに_未加工_モードでファイルを開くには、右上にある **[未加工]** をクリックします。

## リソースの操作

テンプレート言語を使用すると、グループ全体の構成の変更を宣言できますが、場合によっては、特定のリソースのみの操作が必要になることがあります。 これを使用して行える、 `azure リソース` コマンドです。
> [AZURE.NOTE] 使用する場合、 `azure リソース` コマンド以外、 `リスト` コマンドを使用して、操作はリソースの API バージョンを指定する必要があります、 `-o` パラメーター。 使用する API バージョンが不明な場合は、テンプレート ファイルでリソースの **apiVersion** フィールドを確認してください。

1. グループ内のすべてのリソースの一覧を表示するには、次のコマンドを使用します。

        azure resource list "testRG"

2. グループ内の個別のリソースを表示するには、次のようなコマンドを使用します。

        azure resource show "testRG" "MyUbuntuVM" Microsoft.Compute/virtualMachines -o "2015-06-15"

    **Microsoft.Compute/virtualMachines** パラメーターに注意してください。 このパラメーターは、情報を要求するリソースの種類を示します。 前の手順でダウンロードしたテンプレート ファイルを確認すると、テンプレートに記述されている仮想マシンのリソースで、種類の定義にこの同じ値が使用されていることがわかります。

    このコマンドを実行すると、仮想マシンに関する情報が表示されます。

3. リソースの詳細を表示しているときを使用して、多くの場合、 `-json` パラメーター。 一部の値が入れ子構造や集合になるので、出力が読みやすくなります。 次の例のコマンドを実行すると、**show** コマンドの結果を JSON ドキュメントとして表示した場合の例が示されます。

        azure resource show "testRG" "MyUbuntuVM" Microsoft.Compute/virtualMachines -o "2015-06-15" --json

    >[AZURE.NOTE] > 文字を使用して出力をファイルにパイプすると、JSON データをファイルに保存できます。 次に例を示します。
    >
    > `azure リソースの表示"testRG""MyUbuntuVM"Microsoft.Compute/virtualMachines-o「2015年-06-15」-json > myfile.json`

4. 既存のリソースを削除するには、次のようなコマンドを使用します。

        azure resource delete "testRG" "MyUbuntuVM" Microsoft.Compute/virtualMachines -o "2015-06-15"


## ログの記録

グループに対して実行される操作の記録された情報を表示するには、使用、 `azure グループ ログ表示` コマンドです。 既定では、グループに対して実行された直前の操作が表示されます。 省略可能なを使用してすべての操作を表示する `--すべて` パラメーター。 最後の展開を使用して `--last-deployment`します。 特定の展開を使用して `--展開` し、展開名を指定します。 次の例では、*MyGroup* グループで実行されたすべての操作のログが表示されます。

    azure group log show MyGroup --all

## 次のステップ

* Azure リソース マネージャーで Azure PowerShell を使用する方法の詳細については、次を参照してください。 [Azure リソース マネージャーで Azure PowerShell を使用して](../powershell-azure-resource-manager.md)します。
* Azure ポータルから Azure リソース マネージャーを使用する方法については、次を参照してください。 [[psrm] Azure リソースを管理するリソース グループを使用した][psrm]します。


[signuporg]: http://www.windowsazure.com/documentation/articles/sign-up-organization/ 
[adtenant]: http://technet.microsoft.com/library/jj573650#createAzureTenant 
[psrm]: http://go.microsoft.com/fwlink/?LinkId=394760 

