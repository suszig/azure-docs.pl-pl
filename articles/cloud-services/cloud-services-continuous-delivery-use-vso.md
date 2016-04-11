<properties
    pageTitle="Visual Studio Team Services を使用した Azure での継続的な配信 | Microsoft Azure"
    description="Visual Studio Team Services チーム プロジェクトを自動的にビルドして Azure App Service の Web アプリ機能またはクラウド サービスにデプロイするための構成方法について説明します。"
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

# Visual Studio Team Services を使用した Azure への継続的な配信

Visual Studio Team Services チーム プロジェクトを自動的にビルドして Azure  Web アプリまたはクラウド サービスにデプロイするように構成できます。  (継続的なビルドを設定して、システムを使用して展開する方法の詳細について、 *内部設置型* Team Foundation Server を参照してください [Azure でクラウド サービスの継続的な配信](cloud-services-dotnet-continuous-delivery.md).)

このチュートリアルは、Visual Studio 2013 と Azure SDK がインストール済みであることを前提としています。 Visual Studio 2013 がない場合はダウンロードを選択して、 **無料で始める** リンクを [www.visualstudio.com](http://www.visualstudio.com)します。 Azure SDK インストール [ここ](http://go.microsoft.com/fwlink/?LinkId=239540)します。

> [AZURE.NOTE] Visual Studio チームのサービス アカウントをこのチュートリアルを完了する必要があります。
> 実行できます [Visual Studio チームのサービス アカウントを無料で開く](http://go.microsoft.com/fwlink/p/?LinkId=512979)します。

Visual Studio Team Services を使用してクラウド サービスを自動的にビルドして Azure にデプロイするようにセットアップするには、次の手順に従います。

## 手順 1: チーム プロジェクトを作成する

指示に従って、 [ここ](http://go.microsoft.com/fwlink/?LinkId=512980) 、チーム プロジェクトを作成し、Visual Studio にリンクします。 このチュートリアルでは、ソース管理ソリューションとして Team Foundation バージョン管理 (TFVC) を使用していることを前提としています。 Git バージョン管理を使用する場合は、「 [このチュートリアルの Git バージョン](http://go.microsoft.com/fwlink/p/?LinkId=397358)です。

## 手順 2: プロジェクトをソース管理にチェックインする

1. Visual Studio で、デプロイするソリューションを開くか、新しいソリューションを作成します。
このチュートリアルの手順に従って、Web アプリまたはクラウド サービス (Azure アプリケーション) をデプロイできます。
新しいソリューションを作成する場合は、新しい Azure クラウド サービス プロジェクトか、
新しい ASP.NET MVC プロジェクトを作成します。 プロジェクトが .NET Framework 4 または 4.5 をターゲットにしていることを確認し、クラウド サービス プロジェクトを作成している場合は、ASP.NET MVC の Web ロールおよび worker ロールのプロジェクトを追加して、Web ロールに対応するインターネット アプリケーションを選択します。 メッセージが表示されたら、選択 **インターネット アプリケーション**します。
Web アプリを作成する場合は、ASP.NET Web アプリケーション プロジェクトのテンプレートを選択し、次に MVC を選択します。 参照してください [Azure App Service で ASP.NET web アプリの作成](../web-sites-dotnet-get-started.md)します。

    > [AZURE.NOTE] Visual Studio チームのサービスでは、この時点で Visual Studio Web アプリケーションの CI デプロイメントのみサポートします。 Web サイト プロジェクトはサポート対象外です。

1. ソリューションは、コンテキスト メニューを開き **ソリューションをソース管理に追加**します。

    ![][5]

1. 選択を受け入れるか、既定値を変更し、 **OK** ] ボタンをクリックします。 ソース管理アイコンが表示されるプロセスが完了すると、 **ソリューション エクスプ ローラー**します。

    ![][6]

1. ソリューションのショートカット メニューを開き **チェックイン**します。

    ![][7]

1.  **保留中の変更** の領域 **チーム エクスプ ローラー**, 、チェックインのコメントを入力し、[、 **チェックイン** ] ボタンをクリックします。

    ![][8]

    チェックインをするときは、特定の変更を含むまたは除外するためのオプションに注意してください。 変更が除外されますが必要な場合は、選択、 **[すべて含む** リンクします。

    ![][9]

## 手順 3: プロジェクトを Azure に接続する

1. ソース コードが含まれる VSTS チーム プロジェクトが完成したので、チーム プロジェクトを Azure に接続することができます。   [Azure クラシック ポータル](http://manage.windowsazure.com), をクラウド サービスまたは web アプリを選択するかを選択して新しいものを作成、 **+** クリックして、左下にあるアイコン **クラウド サービス** または **Web アプリ** し **簡易作成**します。 選択、 **Visual Studio チームのサービスを使用した発行設定** リンクします。

    ![][10]

1. ウィザードで、] をクリックし、テキスト ボックスに、Visual Studio チームのサービス アカウントの名前を入力、 **今すぐ承認** リンクします。 サインインを求められることがあります。

    ![][11]

1.  **接続要求の** ポップアップ ダイアログで、選択、 **Accept** Azure VSTS のチーム プロジェクトを構成することを許可] ボタンをクリックします。

    ![][12]

1. 承認が成功すると、VSTS チーム プロジェクトのリストが含まれるドロップダウンが表示されます。 前の手順で作成したチーム プロジェクトの名前を選択し、ウィザードのチェック マーク ボタンをクリックします。

    ![][13]

1. プロジェクトがリンクされると、変更を VSTS チーム プロジェクトにチェックインするための手順が表示されます。  次のチェックイン時に、VSTS はプロジェクトをビルドして Azure にデプロイします。  このをクリックしてください、 **Visual Studio からチェックイン** リンクし、 **Visual Studio の起動** リンク (または同等の **Visual Studio** ポータル画面の下部にあるボタン)。

    ![][14]

## 手順 4: リビルドをトリガーし、プロジェクトを再デプロイする

1. Visual Studio で **チーム エクスプ ローラー**, 、選択、 **ソース管理エクスプ ローラー** リンクします。

    ![][15]

1. ソリューション ファイルの保存場所に移動し、ファイルを開きます。

    ![][16]

1.  **ソリューション エクスプ ローラー**, 、ファイルを開き、変更します。 たとえば、ファイルを変更 `_Layout.cshtml` MVC web ロールで Views\\Shared フォルダーの下です。

    ![][17]

1. サイトのロゴを編集して、 **Ctrl + S** ファイルを保存します。

    ![][18]

1.  **チーム エクスプ ローラー**, 、選択、 **保留中の変更** リンクします。

    ![][19]

1. コメントを入力し、[、 **チェックイン** ] ボタンをクリックします。

    ![][20]

1. 選択、 **ホーム** に戻るボタンを **チーム エクスプ ローラー** ホーム ページです。

    ![][21]

1. 選択、 **ビルド** 進行中のビルドを表示するリンク。

    ![][22]

    **チーム エクスプ ローラー** 、チェックインのビルドが開始されたことを示しています。

    ![][23]

1. 処理中のビルドの名前をダブルクリックして、ビルドの進行に合わせて詳細ログを表示します。

    ![][24]

1. ビルドの処理中、ウィザードを使用して TFS を Azure にリンクしたときに作成されたビルド定義を調べてください。  ビルド定義のショートカット メニューを開き **ビルド定義の編集**します。

    ![][25]

     **トリガー** ] タブで、ビルド定義は既定ではチェックインごとにビルドする設定が表示されます。

    ![][26]

     **プロセス** ] タブで、デプロイ環境がクラウド サービスまたは web アプリの名前に設定されている表示できます。 Web アプリを操作している場合は、ここで示されているものとは異なるプロパティが表示されます。

    ![][27]

1. 既定値と異なる値を使用する場合は、プロパティに対して希望の値を指定します。 Azure の発行のプロパティが、 **展開** セクションです。

    次の表に、使用可能なプロパティで、 **展開** セクション。

    |プロパティ|既定値|
    |---|---|
    |信頼されていない証明書を許可|false の場合、SSL 証明書はルート証明機関によって署名される必要があります。|
    |アップグレードの許可|新規作成の代わりに、既存のデプロイメントを更新するデプロイメントを許可します。 IP アドレスを保持します。|
    |削除しない|true の場合、既存の関連のないデプロイメントを上書きしません (アップグレードは許可)。|
    |デプロイメント設定へのパス|リポジトリのルート フォルダーを基準とした Web アプリの .pubxml ファイルへのパス。 クラウド サービスでは無視されます。|
    |Sharepoint デプロイメント環境|サービス名と同じ。|
    |Azure デプロイメント環境|Web アプリ名またはクラウド サービス名。|

1. 複数のサービス構成 (.cscfg ファイル) を使用している場合で目的のサービス構成を指定できます、 **Build、Advanced、MSBuild 引数** 設定します。 たとえば、ServiceConfiguration.Test.cscfg を使用するには、MSBuild 引数ライン オプション `/p:TargetProfile=Test` を設定します。

    ![][38]

    このころまでには、ビルドが正常に完了しています。

    ![][28]

1. Visual Studio の表示、ビルドの名前をダブルクリックした場合、 **ビルドの概要**, 、関連付けられた単体テスト プロジェクトのテスト結果を含みます。

    ![][29]

1.  [Azure クラシック ポータル](http://manage.windowsazure.com), に関連する展開を表示する、 **展開** ] タブのステージング環境を選択するとします。

    ![][30]

1.  目的のサイトの URL に移動します。 Web アプリでは、クリックして、 **参照** コマンド バーのボタンをクリックします。 クラウド サービスの URL を選択、 **概要** のセクションで、 **ダッシュ ボード** クラウド サービスのステージング環境が表示されるページです。 クラウド サービス向けの継続的な統合からのデプロイメントは、既定ではステージング環境に発行されます。 これを変更するには設定して、 **代替クラウド サービス環境** プロパティを **運用**します。 このスクリーンショットでは、クラウド サービスのダッシュボード ページでサイト URL が表示される場所を示しています。

    ![][31]

    新しいブラウザー タブが開いて、実行中のサイトが表示されます。

    ![][32]

    クラウド サービスの場合は、プロジェクトにその他の変更を加えると、さらにビルドが実行され、複数のデプロイメントが累積されます。 最新のデプロイは [アクティブ] とマークされます。

    ![][33]

## 手順 5: 以前のビルドを再デプロイする

この手順の内容は、クラウド サービスのみに適用され、省略可能です。 Azure クラシック ポータルでは、以前のデプロイを選択し、選択、 **を再展開** ] ボタンをクリックしてサイトを以前のチェックインに戻します。  これによって、TFS で新しいビルドが開始され、デプロイメント履歴に新しいエントリが作成されます。 

![][34]

## 手順 6. 運用デプロイメントを変更する

この手順の内容は、クラウド サービスのみに適用され、Web アプリには適用されません。 選択して、運用環境にステージング環境を昇格するには準備ができたら、 **スワップ** Azure クラシック ポータルでボタンをクリックします。 新たにデプロイされたステージング環境は運用に昇格され、以前の運用環境がある場合、運用環境はステージング環境になります。 運用環境とステージング環境でアクティブなデプロイメントは異なることはありますが、最近のビルドのデプロイメント履歴は環境にかかわらず同じです。

![][35]

## 手順 7: 単体テストの実行

この手順の内容は、Web アプリのみに適用され、クラウド サービスには適用されません。 デプロイメントにクオリティ ゲートを配置するには、単体テストを実行できます。失敗した場合は、デプロイメントを停止できます。

1.  Visual Studio で、単体テスト プロジェクトを追加します。

    ![][39]

1.  テストするプロジェクトにプロジェクト参照を追加します。

    ![][40]

1.  複数の単体テストを追加します。 開始するには、常に合格するダミーのテストを試してみてください。

        ```
        using System;
        using Microsoft.VisualStudio.TestTools.UnitTesting;

        namespace UnitTestProject1
        {
            [TestClass]
            public class UnitTest1
            {
                [TestMethod]
                [ExpectedException(typeof(NotImplementedException))]
                public void TestMethod1()
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1.  ビルド定義を編集して、選択、 **プロセス** タブをクリックし、展開、 **テスト** ノードです。

1.  設定、 **テスト失敗時にビルドを失敗させる** True に設定します。 これは、テストに合格しない限り、デプロイメントが行われないことを意味します。

    ![][41]

1.  新しいビルドがキューに追加されます。

    ![][42]

    ![][43]

1. ビルドの進行中は、進行状況を確認します。

    ![][44]

    ![][45]

1. ビルドが完了したら、テスト結果を確認します。

    ![][46]

    ![][47]

1.  失敗するテストを作成してみます。 最初のテストをコピーして新しいテストを追加し、その名前を変更して、NotImplementedException が予期される例外であるコード行をコメントアウトします。

        ```
        [TestMethod]
        //[ExpectedException(typeof(NotImplementedException))]
        public void TestMethod2()
        {
            throw new NotImplementedException();
        }
        ```

1. 変更をチェックインし、新しいビルドをキューに追加します。

    ![][48]

1. テスト結果を表示して失敗の詳細を確認します。

    ![][49]

    ![][50]

## 次のステップ
詳細については、Visual Studio チームのサービスでの単体テストは、次を参照してください。 [ビルドで単体テストを実行する](http://go.microsoft.com/fwlink/p/?LinkId=510474)です。 Git を使用している場合は、次を参照してください。 [Git でコードを共有](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) と [Azure App Service での GIT による継続的なデプロイ](../web-sites-publish-source-control.md)します。  Visual Studio チームのサービスの詳細については、次を参照してください。 [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861)します。

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG

