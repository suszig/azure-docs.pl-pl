<properties
    pageTitle="ロジック アプリを作成する | Microsoft Azure"
    description="SaaS サービスを接続するロジック アプリの作成方法について説明します。"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="app-service\logic"
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/21/2015"
    ms.author="stepsic"/>

# SaaS サービスを接続する新しいロジック アプリを作成します。

| クイック リファレンス |
| --------------- |
| [Logic Apps の定義言語](https://msdn.microsoft.com/library/azure/dn948512.aspx?f=255&MSPPError=-2147217396) |
| [Logic Apps  コネクターのドキュメント](https://azure.microsoft.com/documentation/articles/app-service-logic-connectors-list/) |
| [Logic Apps フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) |

このトピックでは、どのように、わずか数分でを始めることができますを示しています [App Service Logic Apps](app-service-logic-what-are-logic-apps.md)します。 今回は、興味を持ったツイートを Dropbox フォルダーに保存できるようにするまでの流れを見ていきます。

このシナリオでは、以下のものが必要になります。

- Azure サブスクリプション
- Twitter アカウント
- Dropbox アカウント

<!--- TODO: Add try it now information here -->

## コネクタを取得する

最初に、使用する場合は 2 つのコネクタを作成する必要があります: [Dropbox コネクタ](app-service-logic-connector-dropbox.md) と [Twitter Connector](app-service-logic-connector-twitter.md)します。 Twitter API でリダイレクトが行われるため、無料のアプリを Twitter に登録する必要もあります。 コネクタを作成する手順は、以下のとおりです。

1. Azure ポータルにサインインします。

2. クリックして  [Marketplace ](https://portal.azure.com/#blade/HubsExtension/GalleryFeaturedMenuItemBlade/selectedMenuItemId/apiapps) Twitter の検索、ホーム画面 (または [ここをクリックして](https://portal.azure.com/#create/microsoft_com.TwitterConnector.0.2.2))。

3. 選択 **Twitter Connector** ] をクリック **作成**します。 すべての設定が表示されます。 名前のままにすることができます **Twitter Connector**します。  
4. 選択 **パッケージ設定**-ここで Twitter アプリケーションから情報を入力する必要があります。  次の手順で、無料のアプリケーションを設定できます。
    1. 移動して、 [アプリ登録ページを Twitter](http://apps.twitter.com)します。
    2. 新しいアプリを作成します。
    3. 名前と説明を入力します。  Web サイトの URL とコールバック URL として、任意の URL を入力できます (空白にはしないでください)。
    4. 登録されると、コピー、 **コンシューマー キー** に Twitter から、 **clientId** Azure では、フィールド、および **コンシューマー シークレット** に Twitter から、 **clientSecret します。**
    5. をクリックして **OK** 他の API 設定に戻るには Azure のウィンドウでします。

5. プランの名前を入力 **新しい app service のプランの作成**します。

    >[AZURE.NOTE]このセクションの手順では、新しい app service プランを作成することを前提としています。 既存の app service プランを使用している場合をクリックして **[既存の**, して既存のプランを選択して、このセクションの手順を省略し、最後です。 すべてのアプリをホストするプランが必要です。

6.  選択、 **価格レベル** 新しいプランのです。

    >[AZURE.NOTE]既定では、Logic Apps 向けお勧めとされるプランのみが表示されます。 をクリックして **をすべて表示** に利用可能なすべてのプランを参照してください。 ロジック アプリを Free レベルで実行する場合には、実行間隔は 1 時間ごと、使用できるアクションは 1 か月につき 1,000 回までとなります。

7. 作成、 **リソース グループ** フローのです。

    リソース グループは、アプリのコンテナーとしての役割を果たすものです。 同じアプリのリソースはすべて、同じリソース グループに入ります。

8. Azure サブスクリプションが複数ある場合には、使用するものを 1 つ選択します。

9. 選択、 **場所** ロジック アプリを実行します。

    ![[API アプリの作成] ビュー](./media/app-service-logic-create-a-logic-app/gallery.png)

10. クリックして **作成**します。 1 ～ 2 分でプロビジョニングが始まります。

11. ここで使用して、プロセスを繰り返して [Dropbox](https://portal.azure.com/#create/microsoft_com.DropboxConnector.0.2.2)します。

## ロジック アプリを起動する

次は、新しいロジック アプリを作成します。

1. をクリックして **+ 新規** 、画面の下部にある左にある展開 **Web + モバイル**, 、順にクリックして **ロジック アプリ**します。

    [ロジック アプリの作成] ビューが表示されるので、開始するための基本設定を入力していきます。

    ![[ロジック アプリの作成] ビュー](./media/app-service-logic-create-a-logic-app/createlogicapp.png)

2.  **名**, 、ロジック アプリのわかりやすい名前を入力します。

3. 選択、 **App service プラン** 、コネクタの作成時に使用しました。 これにより、場所、サブスクリプション、リソース グループが自動的に選択されます。

これにより、基本設定の処理はクリックしてしないでください **作成** はまだです。 次に、ワークフローにトリガーとアクションを追加します。

## トリガーを追加する

トリガーとは、ロジック アプリの実行の引き金となるものです。 ここでは、繰り返しトリガーを追加します。このトリガーは、事前に設定したスケジュールでワークフローを開始するためのものです。

1. 引き続き、 **ロジック アプリの作成** ビューで、[ **トリガーとアクション**します。

    フローを表示するデザイナーが全画面表示されるほか、作業のひな形となるテンプレートが表示されます。
    
2. このチュートリアルではみましょう **を最初から作成**します。  利用できそうなテンプレートがあれば、適宜使用してください。
    
    右側には、トリガーを設定できるサービスが一覧表示されています。

3. 上部のセクションで [ **繰り返し**します。

    繰り返しの設定を指定するためのボックスが表示されます。

    ![定期的なアイテム](./media/app-service-logic-create-a-logic-app/recurrence.png)

4.  定期的なアイテムを選択して **頻度** と **間隔** 緑色のチェック マークをクリックして (1 時間ごと) などです。

次に、フローにアクションを追加します。

## Twitter アクションを追加する

アクションとは、ワークフローの動作を指します。 アクションの数には上限はありません。また、アクションを調整して、あるアクションの情報がその次のアクションに送られるようにすることもできます。

1. 右側のペインで、クリックして **Twitter コネクタ**します。

2. をクリックし、読み込まれた後 **Authorize**, Twitter アカウントにサインインし、をクリックして、 **アプリの承認**します。

    これにより、コネクタが Twitter アカウントにアクセスできるようになります。 Twitter コネクタに用意されている処理のリストが表示されます。

    ![アクション](./media/app-service-logic-create-a-logic-app/actions.png)

    > [AZURE.NOTE]  **Authorize** ボタンでは、OAUTH セキュリティを使用して、Twitter などの SaaS サービスに接続します。 OAUTH についての詳細は [OAUTH セキュリティ](app-service-logic-oauth-security.md)します。

3. をクリックして **ツイートの検索**, 、次に **クエリを指定する**, 、何かのように入力 `#MicrosoftAzure` 緑色のチェック マークをクリックします。

    ![Twitter の検索](./media/app-service-logic-create-a-logic-app/twittersearch.png)

これで、Twitter コネクタがワークフローの一部となりました。

## Dropbox アクションを追加してアプリを作成する

最後に、ツイートを Dropbox のファイルにアップロードするアクションを追加します。

1. 右側のペインで、クリックして **Dropbox コネクタ**します。

2. プロビジョニングが完了したら **Authorize**, 、Dropbox アカウントにサインインし、 **許可**します。

    ![Dropbox コネクタの承認](./media/app-service-logic-create-a-logic-app/authorize.png)

    これで、コネクタが Dropbox アカウントにアクセスできるようになります。 Dropbox コネクタに用意されている処理のリストが表示されます。

4. クリックして **ファイルのアップロード**します。  

    Dropbox コネクタの設定が表示されるので、ここで Twitter の検索結果のデータを Dropbox に送るように設定していきます。

    ![Dropbox](./media/app-service-logic-create-a-logic-app/dropbox.png)

3.  **FilePath** フィールドに「 `/tweet.txt`

4.  **コンテンツ** フィールドで、をクリックして、 `...` ボタンをクリックし、をクリックして、 **テキストのツイート** オプション。

    これによって、値 `@first(body('twitterconnector')).TweetText` ] ボックスにします。 この値の各部の意味は、以下のとおりです。

    部分                               | 説明
    ------------------------------------------ | ------------
     `@`                                       | 実際の値ではなく、関数を入力しようとしていることを示します。
    `actions('twitterconnector').outputs.body` | Twitter コネクタのクエリの結果として返されたツイートを取得します。
    `first()`                                  | ツイートの検索アクションによって返されるのはリストであるものの、アップロードするのは 1 つのファイルのみとなります。
    `.TweetText`                               | ツイート メッセージのプロパティを選択します。

5. 緑色のチェック マークをクリックして、コネクタの設定を保存します。

5. これで、デザインが完了したら、 **コード ビュー** の左上デザイナー、およびこれには、ワークフローを定義する JSON コードを通知するだけでは、デザイナーで作成します。 このコードでは、[次のトピック] について説明しますが [ロジック アプリの機能を使用する]。

6. をクリックして、 **OK** をクリックして、画面の下部にある、 **作成** ] ボタンをクリックします。

    新しいロジック アプリが作成されます。

## 作成後にロジック アプリを管理する

ここまでの手順で、ロジック アプリが実行できるようになりました。 スケジュールされたワークフローが実行するたびに、特定のハッシュタグを使用してツイートをチェックします。 条件に合致するツイートが見つかった場合には、Dropbox に保存されます。 ここでは、最後にアプリを無効にしたり、動作内容を確認したりする方法を見ていきます。

1. クリックして **参照** し、画面の左側にある **Logic Apps**します。

2. 新たに作成したロジック アプリをクリックすると、現在の状態や全般的な情報を確認できます。

3. 新しいロジック アプリを編集するには、クリックして **トリガーとアクション**します。

5. アプリケーションを無効にする] をクリックして **を無効にする** コマンド バーにします。

クラウド上で実行できる簡単なロジック アプリを 5 分もしないうちに作成できました。 ロジック アプリの機能の使用に関する詳細については、[使用するロジック アプリの機能] を参照してください。 ロジック アプリ定義自体の詳細については、次を参照してください。 [ロジック アプリの定義を作成](app-service-logic-author-definitions.md)します。

<!-- Shared links -->
[Azure portal]: https://portal.azure.com
[Use logic app features]: app-service-logic-use-logic-app-features.md

