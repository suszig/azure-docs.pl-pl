<properties
    pageTitle="Azure App Service を使用したアジャイル ソフトウェア開発"
    description="アジャイル ソフトウェア開発をサポートする方法で Azure App Service を使用して拡張性の高い複雑なアプリケーションを開発する方法について説明します。"
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/16/2015"
    ms.author="cephalin"/>



# Azure App Service を使用したアジャイル ソフトウェア開発

このチュートリアルでは、拡張性の高い複雑なアプリケーションを作成する方法を学習します [Azure App Service](/services/app-service/) をサポートする方法で [アジャイル ソフトウェア開発](https://en.wikipedia.org/wiki/Agile_software_development)します。 既にわかっていると想定してハウツー [予測可能な Azure で複雑なアプリケーションを展開](app-service-deploy-complex-application-predictably.md)します。

技術的な処理の限界によって、アジャイル手法を用いた実装の成功が妨げられることはよくあることです。 などの機能を azure App Service [継続的な発行](web-sites-publish-source-control.md), 、[ステージング環境](web-sites-staged-publishing.md) (スロット) と [監視](web-sites-monitor.md), 、オーケストレーションと展開の管理とうまく組み合わせると、 [Azure リソース マネージャー](resource-group-overview.md), 、アジャイル ソフトウェア開発を採用する開発者向けの優れたソリューションの一部を使用します。

次の表に、アジャイル開発に関連するいくつかの要件と、Azure サービスが各要件にどのように対応できるかを示します。

| 要件| Azure でできること|
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -コミットするたびにビルド<br>- を自動的に作成し、高速化| 継続的デプロイを行うように構成すると、Azure App Service は、開発ブランチに基づくライブ実行ビルドとして機能できます。コードはブランチにプッシュされるたびに自動的にビルドされ、Azure でライブで実行されます。|
| - ビルドにセルフ テストを実行させる| Azure リソース マネージャー テンプレートを使用して、負荷テスト、Web テストなどをデプロイできます。|
| - 運用環境のクローン内でテストを実行する| Azure リソース マネージャーのテンプレートは、(アプリケーション設定、接続文字列テンプレート、スケーリングなどを含む) Azure 運用環境のクローンを作成するために使用できます。迅速かつ予想どおりをテストします。|
| - 最新のビルドの結果を容易に表示する| リポジトリから Azure への継続的デプロイは、新しいコードを、変更をコミットしたすぐ後でライブ アプリケーションでテストできることを意味します。|
| -メイン ブランチに毎日コミット<br>-展開の自動化| 実稼働アプリケーションとリポジトリのメイン ブランチとの継続的インテグレーションによって、すべてのコミット/マージを運用環境のメイン ブランチに自動的にデプロイします。|

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## 学習内容

いただくへの新しい変更を発行するために一般的な開発テスト段階運用ワークフローで、 [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) サンプル アプリケーションの 2 つで構成される [web アプリの](/services/app-service/web/), 、フロント エンド (FE) とその他は Web API バックエンド (BE) および [SQL データベース](/services/sql-database/)します。 次に示すデプロイ アーキテクチャで作業します。

![](./media/app-service-agile-software-development/what-1-architecture.png)

図の説明は次のとおりです。

-   デプロイ アーキテクチャは 3 つの個別の環境に分割されます (または [リソース グループ](resource-group-overview.md) azure)、それぞれに独自 [App Service プラン](azure-web-sites-web-hosting-plans-in-depth-overview.md), 、[スケーリング](web-sites-scale.md) 設定、および SQL データベース。
-   各環境は別々に管理できます。 異なるサブスクリプションで存在することもできます。
-   ステージングと実稼働は、同じ App Service アプリの 2 つのスロットとして実装されます。 マスター ブランチは、ステージング スロットとの継続的インテグレーションを行うようにセットアップされます。
-   (実稼働データ) ステージング スロットでマスター ブランチへのコミットを検証すると、検証されたステージング アプリは運用スロットにスワップ [ダウンタイムなしで](web-sites-staged-publishing.md)します。

運用環境とステージング環境が、テンプレートによって定義されている [* < repository_root > */ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json)します。

開発およびテスト環境は、テンプレートによって定義 [* < repository_root > */ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json)します。

一般的なブランチ戦略を使用して、コードを開発ブランチからテスト ブランチに移動した後、マスター ブランチに移動させることもできます (いわば品質の上方移動)。

![](./media/app-service-agile-software-development/what-2-branches.png)

## 前提条件

-   Azure アカウント
-   A [GitHub](https://github.com/) account
-   Git Shell (と共にインストールされる [GitHub for Windows](https://windows.github.com/)) -これにより、同じセッションで Git と PowerShell の両方のコマンドを実行します。
-   最新 [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/0.9.4-June2015/azure-powershell.0.9.4.msi) ビット
-   以下の事柄の基礎知識:
    -   [Azure リソース マネージャー](resource-group-overview.md) テンプレートのデプロイ (も参照してください [予測可能な Azure で複雑なアプリケーション展開](app-service-deploy-complex-application-predictably.md))
    -   [Git](http://git-scm.com/documentation)
    -   [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] このチュートリアルを完了するには、Azure アカウントが必要です。
> + することができます [Azure アカウントを無料で開く](/pricing/free-trial/?WT.mc_id=A261C142F) -クレジット有料の Azure サービスを使用することができ、アカウントは維持し、使用する無料の Web Apps などの Azure サービスこれらの使用後にもします。
> + することができます [Visual Studio のサブスクライバーの特典を有効に](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -Visual Studio のサブスクリプション クレジットにより、毎月提供を Azure の有料サービスを使用することができます。
>
> 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 運用環境をセットアップする

>[AZURE.NOTE] このチュートリアルで使用するスクリプトは、GitHub リポジトリからの継続的パブリッシングを自動的に構成します。 これを行うには、GitHub 資格情報が既に Azure に保存されている必要があります。保存されていない場合、スクリプト化されたデプロイは、Web アプリに対するソース管理設定を構成しようとした時点で失敗します。 
>
>Azure で、GitHub の資格情報を格納する web アプリを作成、 [Azure ポータル](https://portal.azure.com) と [GitHub のデプロイを構成する](web-sites-publish-source-control.md#Step7)します。 この操作を行うのは 1 回だけです。 

一般的な DevOps シナリオでは、Azure でライブ実行されているアプリケーションがあり、継続的パブリッシングを通してそれを変更します。 このシナリオには、開発、テスト、および運用環境にデプロイするために使用するテンプレートがあります。 このセクションで、それを設定します。

1.  自分専用のフォークを作成、 [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) リポジトリです。 フォークの作成方法の詳細については、次を参照してください。 [、リポジトリをフォーク](https://help.github.com/articles/fork-a-repo/)します。 フォークが作成されたら、ブラウザーでそれを確認できます。

    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Git Shell セッションを開きます。 Git Shell をまだがあるない場合は、インストール [GitHub for Windows](https://windows.github.com/) ようになりました。

3.  次のコマンドを実行して、フォークのローカル クローンを作成します。

        git clone https://github.com/<your_fork>/ToDoApp.git 

4.  ローカル クローンを作成したらに移動 *< repository_root >*\ARMTemplates、および deploy.ps1 スクリプトを次のように実行します。

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git

4.  メッセージが表示されたら、データベースにアクセスするためのユーザー名とパスワードを入力します。

    Azure のさまざまなリソースのプロビジョニングの進行状況が表示されます。 デプロイが完了すると、スクリプトによってアプリケーションがブラウザー内に起動し、わかりやすいビープ音が鳴ります。

    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
    >[AZURE.TIP] 見て *< repository_root >*\ARMTemplates\Deploy.ps1 に一意の Id を使用してリソースを生成する方法を参照してください。 同じ方法で、同じデプロイのクローンを、リソース名の競合を心配せずに作成できます。

6.  Git Shell セッションに戻り、次を実行します。

        .\swap –Name ToDoApp<unique_string>master

    ![](./media/app-service-agile-software-development/production-4-swap.png)

7.  スクリプトが終了する場合、フロント エンドのアドレスを参照に戻る (http://ToDoApp*< unique_string >*master.azurewebsites.net/) を運用環境で実行されているアプリケーションを参照してください。

5.  ログイン、 [Azure ポータル](https://portal.azure.com) を見て何が作成されたとします。

    一方には、同じリソース グループ内の 2 つの web アプリを表示することができます、 `Api` 名のサフィックスです。 リソース グループ ビューを表示している場合は、SQL Database とサーバー、App Service プラン、および Web アプリのステージング スロットも表示されます。 さまざまなリソースを参照し、それらを比較 *< repository_root >*\ARMTemplates\ProdAndStage.json をテンプレート内の構成方法を確認します。

    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

これで、運用環境がセットアップされました。 次に、アプリケーションの新しい更新を開始します。

## 開発ブランチとテスト ブランチを作成する

Azure の運用環境で実行されている複雑なアプリケーションがあり、アジャイル手法に従ってアプリケーションを更新します。 このセクションでは、更新を行うために必要な開発ブランチとテスト ブランチを作成します。

1.  最初に、テスト環境を作成します。 Git Shell セッションで次のコマンドを実行して、**NewUpdate** という名前の新しいブランチ用の環境を作成します。

        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate

1.  メッセージが表示されたら、データベースにアクセスするためのユーザー名とパスワードを入力します。

    デプロイが完了すると、スクリプトによってアプリケーションがブラウザー内に起動し、わかりやすいビープ音が鳴ります。 これで、新しいブランチと、専用のテスト環境が用意されました。 このテスト環境について、いくつかの事柄を再確認しておきます。

    -   任意の Azure サブスクリプションで作成できます。 これは、運用環境をテスト環境から切り離して管理できることを意味します。
    -   テスト環境は Azure でライブ実行されます。
    -   テスト環境は運用環境と同じですが、ステージング スロットとスケーリング設定は異なります。 ProdandStage.json と Dev.json の違いはこれだけであるため、どちらの環境であるかをこれによって判断できます。
    -   テスト環境は、価格レベルが異なる独自の App Service プラン (**無料**など) で管理できます。
    -   このテスト環境の削除は、リソース グループの削除と同じように容易に実行できます。 これを行う方法を学びます [後](#delete)します。

2.  次のコマンドを実行して、開発ブランチの作成に進みます。

        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev

3.  メッセージが表示されたら、データベースにアクセスするためのユーザー名とパスワードを入力します。

    この開発環境について、いくつかの事柄を再確認しておきます。

    -   開発環境はテスト環境と同じテンプレートを使用してデプロイされるため、その構成はテスト環境と同じです。
    -   開発環境は開発者自身の Azure サブスクリプションで作成でき、テスト環境とは別に管理されます。
    -   開発環境は、Azure でライブ実行されます。
    -   開発環境の削除は、リソース グループの削除と同じように容易に実行できます。 これを行う方法を学びます [後](#delete)します。

>[AZURE.NOTE] 複数の開発者が新しい更新のために作業する場合、各開発者は、次の手順を実行することで、ブランチと専用の開発環境を簡単に作成できます。
>
>1.GitHub でリポジトリの自分専用のフォークを作成します (表示 [、リポジトリをフォーク](https://help.github.com/articles/fork-a-repo/))します。
>2.ローカル コンピューターにフォークのクローンを作成します。
>3.自分専用の開発ブランチと環境を作成するためのコマンドを実行します。

操作が完了すると、GitHub フォークには 3 つのブランチが存在します。

![](./media/app-service-agile-software-development/test-1-github-view.png)

さらに、3 つの別々のリソース グループに 6 つの Web アプリケーション (2 つで 1 セットが 3 セット) が存在します。

![](./media/app-service-agile-software-development/test-2-all-webapps.png)
>[AZURE.NOTE] ProdandStage.json では、運用環境を**標準**価格レベルで使用することを指定しています。これは実稼働アプリケーションのスケーラビリティに適しています。

## すべてのコミットをビルドしてテストする

ProdAndStage.json テンプレート ファイルと Dev.json テンプレート ファイルには既にソース管理パラメーターが指定されており、既定で Web アプリの継続的パブリッシングが設定されています。 したがって、GitHub ブランチに対するすべてのコミットは、そのブランチから Azure への自動デプロイをトリガーします。 ここで、このセットアップがどのようには機能するかを見ていきましょう。

1.  ローカル リポジトリの開発ブランチにいることを確認します。 これを行うには、Git Shell で次のコマンドを実行します。

        git checkout Dev

2.  使用するコードを変更することで、アプリの UI レイヤーに簡単な変更を加える [ブートス トラップ](http://getbootstrap.com/components/) 一覧が表示されます。 開いている *< repository_root >*\src\MultiChannelToDo.Web\index.cshtml と強調表示されているように変更します。

    ![](./media/app-service-agile-software-development/commit-1-changes.png)
    >[AZURE.NOTE] 上の図が読み取れない場合: 
    >
    >-行 18 で、次のように変更します。 `チェックリスト` に `リスト グループ`します。
    >-行 19 で、次のように変更します。 `クラス ="チェック リスト項目"` に `クラス ="リスト グループ項目"`します。

3.  変更を保存します。 Git Shell に戻り、次のコマンドを実行します。

        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev

    これらの Git コマンドは、TFS などの別のソース管理システムでの "コードのチェックイン" に似ています。 実行すると `git プッシュ`, 、新しいコミットによって、開発環境で変更を反映するようにアプリケーションが再構築し、Azure に自動コード プッシュがトリガーされます。

4.  開発環境に対するこのコード プッシュが発生したことを確認するには、開発環境の Wb アプリ ブレードに移動し、**[デプロイ]** 部分を調べます。 最新のコミット メッセージを確認できます。

    ![](./media/app-service-agile-software-development/commit-2-deployed.png)

5.  そこから、**[参照]** をクリックして、Azure で実行されているライブ アプリケーションの新しい変更を確認します。

    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)

    これは、アプリケーションに対する非常に小さな変更です。 ただし、複雑な Web アプリケーションに対する新しい変更によって、意図的でない、望ましくない副作用が発生することがあります。 ライブ ビルドでのすべてのコミットを簡単にテストできることで、問題を顧客が目にする前に捕まえることができます。

ここまでの説明で、**NewUpdate** プロジェクトの開発者として、自分用の開発環境の作成、すべてのコミットのビルド、およびすべてのコミットのテストを容易に実行できることを十分に理解できたはずです。

## テスト環境にコードをマージする

コードを開発ブランチから NewUpdate ブランチにプッシュする準備ができたら、次の標準的な Git プロセスを実行します。

1.  NewUpdate に対するすべての新しいコミット (他の開発者によって作成されたコミットなど) を、GitHub の開発ブランチにマージします。 GitHub での新しいコミットによってコード プッシュがトリガーされ、開発環境でビルドされます。 その後、開発ブランチのコードが NewUpdate ブランチからの最新のビットで引き続き動作することを確認できます。

2.  開発ブランチからのすべての新しいコミットを GitHub の NewUpdate ブランチにマージします。 この操作によって、コード プッシュとテスト環境でのビルドがトリガーされます。

ここでも、これらの Git ブランチには継続的デプロイが既に設定されているため、インテグレーション ビルドの実行と同じように、それ以外の操作を行う必要はないことに注意してください。 実行する必要があるのは、Git を使用した標準的なソース管理プラクティスだけであり、すべてのビルド プロセスは Azure が代わりに実行します。

次に、コードを **NewUpdate** ブランチにプッシュします。 Git Shell で、次のコマンドを実行します。

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

これで終了です。

テスト環境の Web アプリ ブレードに移動し、(NewUpdate ブランチにマージされた) 新しいコミットがテスト環境にプッシュされたことを確認します。 次に、**[参照]** をクリックし、スタイルの変更が Azure でライブ実行されていることを確認します。

## 運用環境に更新をデプロイする

ステージング/運用環境にコードをプッシュする操作は、テスト環境にコードをプッシュするときに実行した操作と違いはありません。 それは非常に単純です。

Git Shell で、次のコマンドを実行します。

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

新しいコードは、ProdandStage.json 内のステージング/運用環境のセットアップ方法に基づいて、**ステージング** スロットにプッシュされ、そこで実行されます。 したがって、ステージング スロットの URL に移動すると、新しいコードがそこで実行されていることがわかります。 これを行うには、実行、 `Show-azurewebsite` Git シェルのコマンドレットにします。

    Show-AzureWebsite -Name ToDoApp<unique_string>master -Slot Staging

ステージング スロットで更新を確認した後、残っている唯一の操作は、それを運用環境にスワップすることです。 Git Shellで、次のコマンドを実行します。

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

ご利用ありがとうございます。 新しい更新が実稼働している Web アプリケーションに正常にパブリッシュされました。 これは、開発環境とテスト環境の作成、およびすべてのコミットのビルドとテストを容易に実行した結果です。 これらは、アジャイル ソフトウェア開発の重要な構成要素です。

<a name="delete"></a>
## 開発環境とテスト環境を削除する

開発環境とテスト環境は意図的に自己完結型リソース グループとして構築したため、それらは非常に簡単に削除できます。 このチュートリアルで作成した、GitHub のブランチと Azure アーチファクトの両方を削除するには、Git Shell で次のコマンドを実行するだけですみます。

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Switch-AzureMode AzureResourceManager
    Remove-AzureResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## 概要

アジャイル ソフトウェア開発は、アプリケーション プラットフォームとして Azure を採用することを希望する多くの企業になくてはならないものです。 このチュートリアルでは、運用環境の完全なまたは完全に近いレプリカの作成と削除を容易に実行する方法について説明しました。これは複雑なアプリケーションでも同じです。 すべてのコミットを Azure でビルドして構築できる開発プロセス作成機能を活用する方法についても説明しました。 このチュートリアルが、Azure App Service と Azure リソース マネージャーを最大限に利用して、アジャイル手法に対応する DevOps ソリューションを作成する方法の十分な説明になっていれば幸いです。 次に、DevOps 手法の詳細をなどを実行することによってこのシナリオでビルドできる [実稼働環境でテスト](app-service-web-test-in-production-get-start.md)します。 一般的なテスト運用シナリオでは、次を参照してください。 [(ベータ テスト段階) Azure App Service で Flighting 展開](app-service-web-test-in-production-controlled-test-flight.md)します。

## その他のリソース

-   [Azure で予測可能な複雑なアプリケーションを展開します。](app-service-deploy-complex-application-predictably.md)
-   [アジャイル開発の実践: 最新の開発サイクルのヒントし、テクニック](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
-   [リソース マネージャー テンプレートを使用して Azure の Web アプリの高度な展開戦略](http://channel9.msdn.com/Events/Build/2015/2-620)
-   [Azure リソース マネージャー テンプレートの作成](resource-group-authoring-templates.md)
-   [JSONLint の JSON 検証コントロール](http://jsonlint.com/)
-   [ARMClient – サイトへの GitHub のパブリッシュの設定](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
-   [Git のブランチ機能-基本的なブランチとマージ](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [David Ebbo のブログ](http://blog.davidebbo.com/)
-   [Azure PowerShell](powershell-install-configure.md)
-   [Azure クロスプラット フォーム コマンド ライン ツール](xplat-cli-install.md)
-   [作成または Azure AD でユーザーの編集](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
-   [プロジェクト Kudu Wiki](https://github.com/projectkudu/kudu/wiki)





