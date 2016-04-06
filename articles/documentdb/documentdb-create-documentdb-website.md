<properties 
    pageTitle="Azure リソース マネージャーのテンプレートを使用した DocumentDB と Azure App Service Web Apps のデプロイ | Microsoft Azure" 
    description="Azure リソース マネージャーのテンプレートを使用して DocumentDB アカウント、Azure App Service Web Apps、サンプルの Web アプリケーションをデプロイする方法を説明します。" 
    services="documentdb, app-service\web" 
    authors="ryancrawcour" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/16/2015" 
    ms.author="ryancraw"/>

# Azure リソース マネージャーのテンプレートを使用した DocumentDB と Azure App Service Web Apps のデプロイ #

このチュートリアルは、Azure リソース マネージャー テンプレートを使用して、配置し、統合する方法を示します [Microsoft Azure DocumentDB](http://azure.microsoft.com/services/documentdb/), 、 [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) web アプリとサンプルの web アプリケーション。

このチュートリアルを完了すると、次の項目について説明できるようになります。  

-   Azure リソース マネージャーのテンプレートを使用して、DocumentDB アカウントと Azure App Service の Web アプリをデプロイおよび統合する方法
-   Azure リソース マネージャーのテンプレートを使用して、DocumentDB アカウント、App Service Web Apps の Web アプリ、Web デプロイ アプリケーションをデプロイおよび統合する方法

<a id="Prerequisites"></a>
## 前提条件 ##
> [AZURE.TIP] このチュートリアルを想定していない Azure リソース マネージャー テンプレート、JSON、または Azure PowerShell を使用した経験を参照するテンプレートやデプロイメント オプションを変更する場合、中には、これらの各分野の知識が必要になります。

このチュートリアルの手順を実行する前に、次のものを備えておく必要があります。

- Azure サブスクリプション。 Azure はサブスクリプション方式のプラットフォームです。  サブスクリプションの入手方法の詳細については、[購入オプション](http://azure.microsoft.com/pricing/purchase-options/)、[メンバー プラン](http://azure.microsoft.com/pricing/member-offers/)、[無料評価版](http://azure.microsoft.com/pricing/free-trial/)に関するページをご覧ください。
- Azure ストレージ アカウント。 手順については、次を参照してください。 [Azure ストレージ アカウントについて](../storage-whatis-account.md)します。
- Azure PowerShell 0.9.8 を実行できるワークステーション。 手順については、次を参照してください。 [をインストールし、Azure PowerShell を構成](../install-configure-powershell.md)します。 Azure PowerShell 1.0 プレビューについては、このチュートリアルはまだ更新されていません。 

##<a id="CreateDB"></a>手順 1: サンプル ファイルのダウンロードと抽出 ##
このチュートリアルで使用するサンプル ファイルをダウンロードすることから始めましょう。

1. ダウンロード、 [DocumentDB アカウント、Web アプリを作成およびデプロイのデモ アプリケーション サンプル](https://portalcontent.blob.core.windows.net/samples/CreateDocDBWebsiteTodo.zip) ローカル フォルダー (C:\DocumentDBTemplates など) にファイルを抽出します。  このサンプルでは、DocumentDB アカウント、App Service Web アプリ、Web アプリケーションをデプロイします。  また、Web アプリケーションをこの DocumentDB アカウントに接続するように自動的に構成します。

2. ダウンロード、 [DocumentDB アカウントと Web アプリのサンプルを作成](https://portalcontent.blob.core.windows.net/samples/CreateDocDBWebSite.zip) ローカル フォルダー (C:\DocumentDBTemplates など) にファイルを抽出します。  このサンプルでは、DocumentDB アカウントと App Service Web アプリをデプロイし、Web アプリの構成を変更して DocumentDB の接続情報を容易に表示できるようにしますが、Web アプリケーションは含んでいません。  

> [AZURE.TIP] お使いのコンピューターのセキュリティ設定によって指定することがあります必要を右クリックし、抽出したファイルのブロックを解除するをクリックすると **プロパティ**, をクリックすると、 **ブロックを解除する**です。

![[ブロックの解除] ボタンを強調表示した [プロパティ] ウィンドウのスクリーン ショット](./media/documentdb-create-documentdb-website/image1.png)

<a id="Build"></a>
##手順 2: Document アカウント、App Service の Web アプリ、デモ アプリケーション サンプルのデプロイ ##

では、最初のテンプレートをデプロイしましょう。

> [AZURE.TIP] このテンプレートは、web アプリ名と DocumentDB アカウント名をここで入力が a) 有効かつ b) 使用可能であるを確認していません。  PowerShell デプロイメント スクリプトの実行前に、指定する予定の名前の可用性を確認しておくことを強くお勧めします。

1. Microsoft Azure PowerShell を開きをダウンロードして抽出したフォルダーに移動して、 [は、DocumentDB アカウント、App Service web アプリを作成およびデプロイのデモ アプリケーション サンプル](https://portalcontent.blob.core.windows.net/samples/CreateDocDBWebsiteTodo.zip) (C:\DocumentDBTemplates\CreateDocDBWebsiteTodo など)。


2. CreateDocDBWebsiteTodo.ps1 という PowerShell スクリプトを実行します。  このスクリプトでは、次のような必須のパラメーターを使用します。
    - WebsiteName: App Service Web アプリの名前を指定します。これは、Web アプリへのアクセスに使用する URL を作成するときに使用されます (たとえば、"mydemodocdbwebapp" を指定した場合、Web アプリへのアクセスに使用する URL は mydemodocdbwebapp.azurewebsites.net になります)。

    - ResourceGroupName: デプロイする Azure リソース グループの名前を指定します。 指定したリソース グループが存在しない場合は作成されます。

    - docDBAccountName: 作成する DocumentDB アカウントの名前を指定します。

    - location: DocumentDB と Web アプリのリソースを作成する Azure の場所を指定します。  有効な値は East Asia、Southeast Asia、East US、West US、North Europe、West Europe です (指定した値は大文字と小文字が区別されます)。


3. このスクリプトを実行するコマンドの例を次に示します。

        PS C:\DocumentDBTemplates\CreateDocDBWebAppTodo> .\CreateDocDBWebsiteTodo.ps1 -WebSiteName "mydemodocdbwebapp" -ResourceGroupName "myDemoResourceGroup" -docDBAccountName "mydemodocdbaccount" -location "West US"

    > [AZURE.TIP] スクリプトの実行の一部として、Azure アカウントのユーザー名とパスワードの入力を求められますことに注意してください。  デプロイメントをすべて完了するには 10 ～ 15 分かかります。     

4. 結果の出力の例を次に示します。 

        VERBOSE: 1:06:00 PM - Created resource group 'myDemoResourceGroup' in location westus'
        VERBOSE: 1:06:01 PM - Template is valid.
        VERBOSE: 1:06:01 PM - Create template deployment 'Microsoft.DocumentDBWebSiteTodo'.
        VERBOSE: 1:06:08 PM - Resource Microsoft.DocumentDb/databaseAccounts 'mydemodocdbaccount' provisioning status is running
        VERBOSE: 1:06:10 PM - Resource Microsoft.Web/serverFarms 'mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:06:14 PM - Resource microsoft.insights/alertrules 'CPUHigh mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:06:16 PM - Resource microsoft.insights/autoscalesettings 'mydemodocdbwebapp-myDemoResourceGroup' provisioning status is succeeded
        VERBOSE: 1:06:16 PM - Resource microsoft.insights/alertrules 'LongHttpQueue mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:06:21 PM - Resource Microsoft.Web/Sites 'mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:06:23 PM - Resource microsoft.insights/alertrules 'ForbiddenRequests mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:06:25 PM - Resource microsoft.insights/alertrules 'ServerErrors mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:06:25 PM - Resource microsoft.insights/components 'mydemodocdbwebapp' provisioning status is succeeded
        VERBOSE: 1:16:22 PM - Resource Microsoft.DocumentDb/databaseAccounts 'mydemodocdbaccount' provisioning status is succeeded
        VERBOSE: 1:16:22 PM - Resource Microsoft.DocumentDb/databaseAccounts 'mydemodocdbaccount' provisioning status is succeeded
        VERBOSE: 1:16:24 PM - Resource Microsoft.Web/Sites/config 'mydemodocdbwebapp/web' provisioning status is succeeded
        VERBOSE: 1:16:27 PM - Resource Microsoft.Web/Sites/Extensions 'mydemodocdbwebapp/MSDeploy' provisioning status is running
        VERBOSE: 1:16:35 PM - Resource Microsoft.Web/Sites/Extensions 'mydemodocdbwebapp/MSDeploy' provisioning status is succeeded

        ResourceGroupName : myDemoResourceGroup
        Location          : westus
        Resources         : {mydemodocdbaccount, CPUHigh mydemodocdbwebapp, ForbiddenRequests mydemodocdbwebapp, LongHttpQueue mydemodocdbwebapp...}
        ResourcesTable    :
                    Name                                    Type                                   Location
                    ======================================  =====================================  =========
                    mydemodocdbaccount                      Microsoft.DocumentDb/databaseAccounts  westus
                    CPUHigh mydemodocdbwebapp              microsoft.insights/alertrules          eastus
                    ForbiddenRequests mydemodocdbwebapp    microsoft.insights/alertrules          eastus
                    LongHttpQueue mydemodocdbwebapp        microsoft.insights/alertrules          eastus
                    ServerErrors mydemodocdbwebapp         microsoft.insights/alertrules          eastus
                    mydemodocdbwebapp-myDemoResourceGroup  microsoft.insights/autoscalesettings   eastus
                    mydemodocdbwebapp                      microsoft.insights/components          centralus
                    mydemodocdbwebapp                      Microsoft.Web/serverFarms              westus
                    mydemodocdbwebapp                      Microsoft.Web/sites                    westus

        ProvisioningState : Succeeded


5. サンプル アプリケーションを見る前に、このテンプレートのデプロイメントによって得られた結果を把握しておきましょう。

    - App Service Web アプリが作成されました。

    - DocumentDB アカウントが作成されました。

    - Web デプロイ パッケージが App Service Web アプリにデプロイされました。

    - Web アプリの構成が変更され、DocumentDB エンドポイントとプライマリ マスター キーがアプリケーションの設定として表示されるようになりました。

    - 一連の既定の監視ルールが作成されました。

    
6. アプリケーションを使用するために移動するだけで web アプリの URL に (上記の例では、URL になります http://mydemodocdbwebapp.azurewebsites.net)。  次のような Web アプリケーションが表示されます。

    ![Todo アプリケーションのサンプル](./media/documentdb-create-documentdb-website/image2.png)

7. 前方に移動し、いくつかのタスクを作成しを開いてみましょう、 [Microsoft Azure ポータル](https://portal.azure.com)します。

8. リソース グループを参照するように選択し、デプロイメント時に作成したリソース グループを選択します (上のサンプルでは、myDemoResourceGroup)。

    ![myDemoResourceGroup を強調表示した Azure クラシック ポータルのスクリーン ショット](./media/documentdb-create-documentdb-website/image3.png)
9.  [概要] レンズのリソース マップに関連リソース (DocumentDB アカウント、App Service Web アプリ、監視) がすべて表示されることを確認します。

    ![[監視] レンズのスクリーン ショット](./media/documentdb-create-documentdb-website/image4.png)
10.  DocumentDB アカウントをクリックし、(アカウント ブレードの下部にある) クエリ エクスプローラーを起動します。

    ![Screenshot of the Resource Group and Account blades with the Query Explorer tile highlighted](./media/documentdb-create-documentdb-website/image8.png)

11. 既定のクエリ "SELECT * FROM c" を実行し、その結果を調べます。  このクエリでは、前の手順 7. で作成した Todo 項目の JSON 表現を取得しました。  クエリを自由に試してください。たとえば、SELECT * FROM c WHERE c.isComplete = true を実行してみると、完了済みとしてマークされているすべての Todo 項目が返されます。


    ![Screenshot of the Query Explorer and Results blades showing the query results](./media/documentdb-create-documentdb-website/image5.png)
12. DocumentDB ポータルの操作やサンプル Todo アプリケーションの変更を自由に試してみてください。  準備ができたら、別のテンプレートをデプロイしましょう。
    
<a id="Build"></a> 
## 手順 3: Document アカウントと Web アプリのサンプルのデプロイ ##

では、2 番目のテンプレートをデプロイしましょう。

> [AZURE.TIP] このテンプレートは、web アプリ名と DocumentDB アカウント名をここで入力が a) 有効かつ b) 使用可能であるを確認していません。  PowerShell デプロイメント スクリプトの実行前に、指定する予定の名前の可用性を確認しておくことを強くお勧めします。

1. Microsoft Azure PowerShell を開きをダウンロードして抽出したフォルダーに移動して、 [DocumentDB アカウントと web アプリのサンプルを作成する](https://portalcontent.blob.core.windows.net/samples/CreateDocDBWebSite.zip) (C:\DocumentDBTemplates\CreateDocDBWebsite など)。


2. CreateDocDBWebsite.ps1 という PowerShell スクリプトを実行します。  このスクリプトでは、デプロイした最初のテンプレートと同じパラメーターを使用します。次のようになります。
    - WebsiteName: App Service Web アプリの名前を指定します。これは、Web アプリへのアクセスに使用する URL を作成するときに使用されます (たとえば、"myotherdocumentdbwebapp" を指定した場合、Web アプリへのアクセスに使用する URL は myotherdocumentdbwebapp.azurewebsites.net になります)。

    - ResourceGroupName: デプロイする Azure リソース グループの名前を指定します。  指定したリソース グループが存在しない場合は作成されます。

    - docDBAccountName: 作成する DocumentDB アカウントの名前を指定します。

    -   location: DocumentDB と Web アプリのリソースを作成する Azure の場所を指定します。  有効な値は East Asia、Southeast Asia、East US、West US、North Europe、West Europe です (指定した値は大文字と小文字が区別されます)。

3. このスクリプトを実行するコマンドの例を次に示します。

        PS C:\DocumentDBTemplates\CreateDocDBWebSite> .\CreateDocDBWebSite.ps1 -WebSiteName "myotherdocumentdbwebapp" -ResourceGroupName "myOtherDemoResourceGroup" -docDBAccountName "myotherdocumentdbdemoaccount" -location "East US"

    > [AZURE.TIP] スクリプトの実行の一部として、Azure アカウントのユーザー名とパスワードの入力を求められますことに注意してください。  デプロイメントをすべて完了するには 10 ～ 15 分かかります。     

4. デプロイメントの出力は、最初のテンプレートの例とよく似ています。 
5. Azure ポータルを開く前に、このテンプレートのデプロイメントによって得られた結果を把握しておきましょう。

    - App Service Web アプリが作成されました。

    - DocumentDB アカウントが作成されました。

    -   Web アプリの構成が変更され、Azure DocumentDB エンドポイント、プライマリ マスター キー、セカンダリ マスター キーがアプリケーションの設定として表示されるようになりました。

    -   一連の既定の監視ルールが作成されました。

6. 開いてみましょう、 [Azure ポータル](https://portal.azure.com), 、リソース グループを参照し、(サンプルでは、myOtherDemoResourceGroup 上) の展開時に作成したリソース グループを選択します。
7. [概要] レンズで、デプロイしたばかりの Web アプリを選択します。

    ![myotherdocumentdbwebapp Web アプリを強調表示した [概要] レンズのスクリーン ショット](./media/documentdb-create-documentdb-website/image6.png)
8. Web アプリのブレードでクリックして **すべての設定**, 、し **アプリケーションの設定** し、どのアプリケーション設定が表示されて、DocumentDB エンドポイントとそれぞれの DocumentDB マスター_キーに注意してください。

    ![[Web アプリ] ブレード、[設定] ブレード、および [アプリケーションの設定] ブレードのスクリーン ショット](./media/documentdb-create-documentdb-website/image7.png)
9. 自由に、Azure ポータルの探索を続行または、DocumentDB のいずれかに従う [サンプル](http://go.microsoft.com/fwlink/?LinkID=402386) を独自の DocumentDB アプリケーションを作成します。

    
    
<a name="NextSteps"></a>
## 次のステップ

ご利用ありがとうございます。 Azure リソース マネージャーのテンプレートを使用して、DocumentDB、App Service Web アプリ、サンプルの Web アプリケーションをデプロイしました。

- DocumentDB の詳細については、次のようにクリックします。 [ここ](http://azure.com/docdb)します。
- Azure App Service Web apps の詳細についてをクリックして [ここ](http://go.microsoft.com/fwlink/?LinkId=325362)します。
- Azure リソース マネージャー テンプレートの詳細については、次のようにクリックします。 [ここ](https://msdn.microsoft.com/library/azure/dn790549.aspx)します。


## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)
* 古いポータルから新しいポータルへの変更ガイドについては、次を参照してください: [Azure クラシック ポータル内の移動に関するリファレンス。](http://go.microsoft.com/fwlink/?LinkId=529715)

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。
 


