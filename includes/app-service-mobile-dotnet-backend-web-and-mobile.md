このトピックでは、モバイル クライアントと Web クライアントの両方を使用するアプリを作成する方法を示します。 モバイル アプリと Web アプリを作成し、その両方で、同じ基礎データベースを使用します。

新しいモバイル アプリ バックエンドと簡単なの両方を作成する最初 *作業の一覧に* 新しいモバイル アプリ バックエンドにアプリケーション データを格納するアプリケーション。 モバイル アプリのバックエンドでは、サーバー側ビジネス ロジックでサポートされる .NET 言語を使用します。 クライアント アプリでは、Mobile App によってサポートされるクライアント プラットフォーム (iOS、Windows、Xamarin iOS、Xamarin Android など) を使用できます。

次に、モバイル アプリと同じデータベースを使用する Web アプリを作成します。 チュートリアルが終わるときには、同じデータで動作する Web クライアントとモバイル クライアントが作成されています。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 新しいモバイル アプリ バックエンドとクライアントを作成する

* このチュートリアルの手順に従って [Create a mobile app] モバイル アプリ バックエンドとクライアントの両方を作成します。 Mobile Apps によってサポートされるクライアント プラットフォーム (iOS、Windows、Xamarin iOS、Xamarin Android など) を使用できます。

* モバイル アプリ バックエンドを Azure にデプロイし、モバイル クライアント アプリケーションを、ホストされているバックエンドに接続したことを確認します。 モバイル アプリのコード プロジェクトは "エンティティ フレームワーク コード ファースト" を使用し、モバイル クライアント アプリから最初の REST 要求を受け取ると、データベースを初期化します。

## Visual Studio から、TodoList Web API を発行します。

このセクションでは、サンプルの Web アプリケーション ソリューションを使用して新しい Web アプリケーションを作成します。 サンプルで使用するデータベース スキーマ名と接続文字列を、モバイル アプリと同じものに変更します。

>[AZURE.NOTE] 次の手順を完了する前に、クライアントを接続することによって、モバイル アプリ バックエンドのデータベースを初期化したことを確認します。 完了していないと、Web アプリケーションを同じデータベースに接続することはできません。

1.  [Azure ポータル](https://portal.azure.com/), 、モバイル アプリと同じリソース グループとホスティング プランを使用して、新しい web アプリを作成します。

2. サンプル ソリューションをダウンロードする [MultiChannelToDo] し、Visual Studio で開きます。 このソリューションには、Web クライアント UI 用の Web API プロジェクトと Web アプリケーション プロジェクトの両方が含まれています。

3. Web API プロジェクトで、MultiChannelToDoContext.cs を編集します。 `OnModelCreating` で、スキーマ名を更新してモバイル アプリ名と同じにします。

        modelBuilder.HasDefaultSchema("your_mobile_app"); // your service name, replacing dashes with underscore

4. 次に、Azure ポータルから、モバイル アプリの接続文字列を取得します。

    - ポータルで、モバイル アプリを選択し、部分をクリックしてというラベルが付いた **ユーザー コード**します。

    - 開いたブレードで、次のように選択します。 **設定をすべて**, 、し **アプリケーション設定**します。

    -  **接続文字列** クリックして **接続文字列の表示**します。 設定の値をコピー **MS_TableConnectionString**します。 これは、モバイル アプリが SQL Database に接続するために使用する接続文字列です。

5. Visual Studio で、Web API プロジェクトを右クリックし、選択 **発行**します。 選択 **Azure Web Apps** として発行先に、上記で作成した web アプリを選択します。 をクリックして **次** が表示されるまで、 **設定** Web の発行ウィザードのセクションです。

6.  **データベース** セクションの値としてモバイル アプリの接続文字列を貼り付けます **MultiChannelToDoContext**します。 チェック ボックスのみを選択 **実行時にこの接続文字列を使用して**します。

7. Web API が Azure に正常に発行されると、確認ページが表示されます。 発行されたサービスの URL をコピーします。

## Visual Studio から TodoList Web クライアント UI を発行する

このセクションでは、AngularJS で実装されるサンプル Web クライアント アプリケーションを使用します。 その後、Visual Studio を使用して、プロジェクトを Azure の新しいホストされる App Service Web アプリに対して発行します。

1. Visual Studio でプロジェクトを開き **MultiChannelToDo.Web**します。 `js/service/ToDoService.js` ファイルを編集して、発行したばかりの Web API への URL を追加します。

        var apiPath = "https://your-web-api-site-name.azurewebsites.net";

2. プロジェクトを右クリックして **MultiChannelToDo.Web** 選択 **発行**します。

3.  **Web の発行** ウィザードで、 **Azure Web App** 発行対象とし、データベースを持たない新しい web アプリを作成します。

4. プロジェクトが正常に発行されると、ブラウザーに Web UI が表示されます。

## モバイル アプリと Web アプリのテスト

1. Web UI で、いくつかの ToDo 項目を追加または編集します。

    ![ブラウザーでの Web アプリケーションの表示](./media/app-service-mobile-dotnet-backend-web-and-mobile/web-app-in-browser.png)

2. 作成したモバイル アプリの実行、 [Create a mobile app] チュートリアルです。 Web アプリと同じ ToDo 項目が表示されます。

    ![Xamarin モバイル アプリの表示](./media/app-service-mobile-dotnet-backend-web-and-mobile/xamarin-ios-quickstart-device.png)

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)
* 古いポータルから新しいポータルへの変更ガイドについては、次を参照してください: [Azure ポータル内の移動に関するリファレンス。](http://go.microsoft.com/fwlink/?LinkId=529715)

<!-- Links -->

[MultiChannelToDo]: https://github.com/Azure/mobile-services-samples/tree/web-mobile/MultiChannelToDo
[Create a mobile app]: ../article/app-service-mobile/app-service-mobile-xamarin-ios-get-started.md

