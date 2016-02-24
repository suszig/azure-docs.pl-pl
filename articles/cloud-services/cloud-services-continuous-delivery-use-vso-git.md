<properties
    pageTitle="Visual Studio Team Services と Git を使用した Azure での継続的な配信 | Microsoft Azure" 
    description="Visual Studio Team Services チーム プロジェクトを Git を使用して自動的にビルドして Azure App Service の Web アプリ機能またはクラウド サービスにデプロイするための構成方法について説明します。"
    services="cloud-services"
    documentationCenter=".net"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/02/2015"
    ms.author="tarcher"/>

# Visual Studio Team Services と Git を使用した Azure への継続的な配信

Visual Studio Team Services チーム プロジェクトを使用してソース コードの Git リポジトリをホストし、コミットをリポジトリにプッシュするたびに自動的にビルドして Azure の Web アプリまたはクラウド サービスにデプロイすることができます。

Visual Studio 2013 および Azure SDK をインストールする必要があります。 Visual Studio 2013 がない場合はダウンロードを選択して、 **無料で始める** リンクを [www.visualstudio.com](http://www.visualstudio.com)します。 Azure SDK インストール [ここ](http://go.microsoft.com/fwlink/?LinkId=239540)します。


> [AZURE.NOTE] Visual Studio チームのサービス アカウントをこのチュートリアルを完了する必要があります。
> 実行できます [Visual Studio チームのサービス アカウントを無料で開く](http://go.microsoft.com/fwlink/p/?LinkId=512979)します。

Visual Studio Team Services を使用してクラウド サービスを自動的にビルドして Azure にデプロイするようにセットアップするには、次の手順に従います。

## 手順 1. Git リポジトリを作成する

1. Visual Studio チームのサービス アカウントがない場合は、いずれかを取得  [ここ](http://go.microsoft.com/fwlink/?LinkId=397665)します。 チーム プロジェクトを作成するときに、ソース管理システムとして Git を選択します。 指示に従い、Visual Studio をチーム プロジェクトに接続します。

2.  **チーム エクスプ ローラー**, 、選択、 **このリポジトリをクローン** リンクします。

    ![][3]

3. ローカル コピーの場所を指定し、選択、 **複製** ] ボタンをクリックします。

## 手順 2. プロジェクトを作成してリポジトリにコミットする

1.  **チーム エクスプ ローラー**, で、 **ソリューション** セクションで、選択、 **新規** リンク ローカル リポジトリで新しいプロジェクトを作成します。

    ![][4]

2. このチュートリアルの手順に従って、Web アプリまたはクラウド サービス (Azure アプリケーション) をデプロイできます。 新しい Azure クラウド サービス プロジェクトを作成します。
新しい ASP.NET MVC プロジェクトを作成します。 プロジェクトが .NET Framework 4 以降を対象にしていることを確認します。 クラウド サービス プロジェクトを作成する場合は、ASP.NET MVC Web ロールと worker ロールを追加します。
Web アプリを作成する場合は、選択、 **ASP.NET Web アプリケーション** プロジェクト テンプレートをクリックして **MVC**します。 参照してください [Azure App Service で ASP.NET web アプリの作成](../web-sites-dotnet-get-started.md) の詳細。

3. ソリューションのショートカット メニューを開き **コミット**します。

    ![][7]

4. Visual Studio Team Services で初めて Git を使用する場合は、Git で自分自身が識別されるように情報を提供する必要があります。  **保留中の変更** の領域 **チーム エクスプ ローラー**, 電子メール アドレスやユーザー名を入力します。 コミットのコメントを入力し、[、 **コミット** ] ボタンをクリックします。

    ![][8]

5. チェックインをするときは、特定の変更を含むまたは除外するためのオプションに注意してください。 必要な変更が除外されている場合は、選択 **[すべて含む**します。

6. これで、リポジトリのローカル コピーに変更をコミットできました。 次に、サーバーを使用して変更を同期を選択して、 **同期** リンクします。

## 手順 3: プロジェクトを Azure に接続する

1. ソース コードを含む Git リポジトリが Visual Studio Team Services に保持され、Git リポジトリを Azure に接続する準備が整いました。   [Azure クラシック ポータル](http://manage.windowsazure.com), をクラウド サービスまたは web アプリを選択するかを選択して新しいものを作成、+ 左クリックして、下にあるアイコン **クラウド サービス** または **Web アプリ** し **簡易作成**します。

    ![][9]

3. クラウド サービスを選択して、 **Visual Studio チームのサービスを使用した発行設定** リンクします。 Web アプリを選択、 **ソース管理からデプロイを設定** リンクします。

    ![][10]

2. ウィザードで、テキスト ボックスに、Visual Studio チームのサービス アカウントの名前を入力し、選択、 **今すぐ承認** リンクします。 サインインを求められることがあります。

    ![][11]

3.  **接続要求の** ポップアップ ダイアログで、選択 **Accept** Azure Visual Studio Team Services でチーム プロジェクトを構成することを許可します。

    ![][12]

4. 承認が成功すると、Visual Studio Team Services チーム プロジェクトが含まれるドロップダウン リストが表示されます。  前のステップで作成したチーム プロジェクトの名前を選択し、ウィザードのチェック マーク ボタンを選択します。

    ![][13]

    次にコミットをリポジトリにプッシュするときに、Visual Studio Team Services はプロジェクトをビルドして Azure にデプロイします。

## 手順 4: リビルドをトリガーし、プロジェクトを再デプロイする

1. Visual Studio で、ファイルを開き、変更します。 たとえば、ファイルを変更 `_Layout.cshtml` MVC web ロールで Views\\Shared フォルダーの下です。

    ![][17]

2. サイトのフッター テキストを編集し、そのファイルを保存します。

    ![][18]

3.  **ソリューション エクスプ ローラー**, 、ショートカット メニューを開き、ソリューション ノード、プロジェクト ノード、または変更するファイルを選択し、 **コミット**します。

4. コメントを入力して、 **コミット**します。

    ![][20]

5. 選択、 **同期** リンクします。

    ![][38]

6. 選択、 **プッシュ** してコミットを Visual Studio Team Services 内のリポジトリにプッシュへのリンク。 (使用することも、 **同期** コミットをリポジトリにコピーする] ボタンをクリックします。 その違いは **同期** ではリポジトリから最新の変更が取得されます)。

    ![][39]

7. 選択、 **ホーム** に戻るボタンを **チーム エクスプ ローラー** ホーム ページです。

    ![][21]

8. 選択 **ビルド** 進行中のビルドを表示します。

    ![][22]

    **チーム エクスプ ローラー** 、チェックインのビルドが開始されたことを示しています。

    ![][23]

9. ビルドの処理に応じて詳細なログを表示するには、処理中のビルドの名前をダブルクリックします。

10. ビルドの処理中に、Azure にリンクするためのウィザードを使用したときに作成されたビルド定義を調べます。  ビルド定義のショートカット メニューを開き **ビルド定義の編集**します。

    ![][25]

11.  **トリガー** ] タブで、既定でチェックインのたびに、上に構築するビルド定義が設定されている表示されます。 クラウド サービスの場合は、Visual Studio Team Services により master 分岐がビルドされてステージング環境に自動的にデプロイされます。 その場合も、ライブ サイトにデプロイするための手動による手順を実行する必要があります。 ステージング環境がない Web アプリの場合は、master 分岐が直接ライブ サイトにデプロイされます。

    ![][26]

1.  **プロセス** ] タブで、デプロイ環境がクラウド サービスまたは web アプリの名前に設定されている表示できます。

    ![][27]

1. 既定値と異なる値を使用する場合は、プロパティに対して希望の値を指定します。 Azure の発行のプロパティが、 **展開** セクションも必要になる MSBuild のパラメーターを設定します。 たとえば、クラウド サービス プロジェクトで「クラウド」以外のサービス構成を指定する、MSbuild パラメーターに設定 `/p:TargetProfile=[YourProfile]` 、 *[YourProfile]* サービス構成のような名前のサービス構成ファイルに一致します*。YourProfile*.cscfg です。

    次の表に、使用可能なプロパティで、 **展開** セクション。

    |プロパティ|既定値|
    |---|---|
    |信頼されていない証明書を許可|false の場合、SSL 証明書はルート証明機関によって署名される必要があります。|
    |アップグレードの許可|新規作成の代わりに、既存のデプロイメントを更新するデプロイメントを許可します。 IP アドレスを保持します。|
    |削除しない|true の場合、既存の関連のないデプロイメントを上書きしません (アップグレードは許可)。|
    |デプロイメント設定へのパス|リポジトリのルート フォルダーを基準とした Web アプリの .pubxml ファイルへのパス。 クラウド サービスでは無視されます。|
    |Sharepoint デプロイメント環境|サービス名と同じ。|
    |Azure デプロイメント環境|Web アプリ名またはクラウド サービス名。|

1. このころまでには、ビルドが正常に完了しています。

    ![][28]

1. Visual Studio の表示、ビルドの名前をダブルクリックした場合、 **ビルドの概要**, 、関連付けられた単体テスト プロジェクトのテスト結果を含みます。

    ![][29]

1.  [Azure クラシック ポータル](http://manage.windowsazure.com), に関連する展開を表示する、 **展開** ] タブのステージング環境を選択するとします。

    ![][30]

1.  目的のサイトの URL に移動します。 Web アプリの選択、 **参照** ポータルでボタンをクリックします。 クラウド サービスの URL を選択、 **概要** のセクションで、 **ダッシュ ボード** ステージング環境が表示されるページです。

    クラウド サービス向けの継続的な統合からのデプロイメントは、既定ではステージング環境に発行されます。 これを変更するには設定して、 **代替クラウド サービス環境** プロパティを **運用**します。 ここでは、クラウド サービスのダッシュボード ページにサイト URL が表示されます。

    ![][31]

    新しいブラウザー タブが開いて、実行中のサイトが表示されます。

    ![][32]

1.  プロジェクトにその他の変更を加えると、さらにビルドが実行され、複数のデプロイが累積されます。 最新のデプロイは [アクティブ] とマークされます。

    ![][33]

## 手順 5: 以前のビルドを再デプロイする

この手順は省略可能です。 Azure クラシック ポータルで以前のデプロイを選択して、 **を再展開** してサイトを以前のチェックインに戻します。 これによって、TFS で新しいビルドが開始され、デプロイメント履歴に新しいエントリが作成されます。 

![][34]

## 手順 6. 運用デプロイメントを変更する

選択して、運用環境にステージング環境を昇格するには準備ができたら、 **スワップ** Azure クラシック ポータルで。 新たにデプロイされたステージング環境は運用に昇格され、以前の運用環境がある場合、運用環境はステージング環境になります。 運用環境とステージング環境でアクティブなデプロイメントは異なることはありますが、最近のビルドのデプロイメント履歴は環境にかかわらず同じです。

![][35]

## 手順 7. working 分岐からデプロイする

Git を使用する場合は、通常、working 分岐で変更を行い、開発が完了状態に到達したときに master 分岐に統合します。 プロジェクトの開発フェーズの間に、この working 分岐をビルドして Azure にデプロイします。

1.  **チーム エクスプ ローラー**, 、選択、 **ホーム** ボタンをクリックしを選択し、 **分岐** ] ボタンをクリックします。

    ![][40]

2. 選択、 **新しい分岐** リンクします。

    ![][41]

3. "Working"など分岐の名前を入力して、 **分岐の作成**します。 これにより、新しいローカル分岐が作成されます。

    ![][42]

4. 分岐を発行します。 分岐名を選択して **発行されていない分岐**, 、して **発行**します。

    ![][44]

6. 既定では、master 分岐が変更された場合にのみ、継続的なビルドがトリガーされます。 Working 分岐に対して継続的なビルドを設定するには、選択、 **ビルド** ページに **チーム エクスプ ローラー**, 、して **ビルド定義の編集**します。

7. 開いている、 **ソース設定** ] タブをクリックします。  **継続的インテグレーションとビルドのための分岐を監視**, 、選択 **ここをクリックして、新しい行を追加する**です。

    ![][47]

8. refs/heads/working など作成した分岐を指定します。

    ![][48]

9. コードに変更を加え、変更したファイルのショートカット メニューを開きを選択し、 **コミット**します。

    ![][43]

10. 選択、 **同期されていないコミット** リンク、および選択、 **同期** ボタンまたは **プッシュ** を Visual Studio Team Services の working 分岐のコピーに変更内容をコピーします。

    ![][45]

11. 移動し、 **ビルド** 表示して、working 分岐に対してトリガーされたビルドを見つけます。

## 次のステップ

Visual Studio チームのサービスでの Git の使用に関するその他のヒントについては、次を参照してください。 [開発し、Visual Studio を使用して Git でコードを共有](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) し、Azure に発行する Visual Studio チーム サービスによって管理されていない Git リポジトリを使用する方法の詳細については、次を参照してください。 [Azure App Service での GIT による継続的なデプロイ](../web-sites-publish-source-control.md)します。 Visual Studio Team Services の詳細については、次を参照してください。 [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861)します。

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG

