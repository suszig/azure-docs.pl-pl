<properties
    pageTitle="既存のアプリケーションへの Mobile Services の追加 (WP8) | Microsoft Azure"
    description="Azure Mobile Services Windows Phone 8 アプリからのデータを使用する方法について説明します。"
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/11/2015"
    ms.author="glenga"/>


# 既存のアプリケーションへの Mobile Services の追加

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started-data](../../includes/mobile-services-selector-get-started-data.md)]

##概要

このトピックでは、Azure Mobile Services を使用して Windows Phone 8 アプリケーションのデータを活用する方法について説明します。 このチュートリアルでがメモリにデータを格納するアプリケーションをダウンロードして、新しいモバイル サービスの作成、アプリケーション、およびへのログインと、モバイル サービスを統合、 [Azure classic portal] データに、アプリケーションの実行中に加えられた変更を表示します。

次のビデオで、Nick Harris によるこのチュートリアルのデモをご覧いただけます。
>[AZURE.VIDEO mobile-get-started-with-data-windows-phone]

##前提条件

+ Visual Studio 2012 Express for Windows Phone 8、および [Windows Phone 8 SDK] Windows 8 で実行されています。 このチュートリアルを完了して、Windows Phone 8.1 アプリケーションを作成するには、Visual Studio 2013 Update 2 以降が必要です。

+ Azure アカウント。 アカウントがない場合は、無料の試用アカウントを数分で作成することができます。 詳細については、「[Azure の無料評価版サイト](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A756A2826&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Farticles%2Fdocumentation%2Fmobile-services-windows-phone-get-started-data%2F)」をご覧ください。

##<a name="download-app"></a>GetStartedWithData プロジェクトをダウンロードする

このチュートリアルを基づいて、 [GetStartedWithData アプリケーション][Developer Code Samples site], 、Windows Phone Silverlight 8 アプリケーション プロジェクトします。

1. GetStartedWithData サンプル アプリケーション プロジェクトをダウンロード、 [Developer Code Samples site]します。

    >[AZURE.NOTE]Windows Phone Silverlght 8.1 アプリケーションを作成するには、Windows Phone 8.1 をダウンロードした Windows Phone Silverlight 8 アプリケーション プロジェクトの対象 OS に変更します。 Windows Phone ストア アプリを作成するには、ダウンロード、 [Windows Phone Store アプリケーション バージョン](http://go.microsoft.com/fwlink/p/?LinkId=397372) の GetStartedWithData サンプル アプリケーション プロジェクト。

2. Visual Studio で、ダウンロードしたプロジェクトを開き、MainPage.xaml.cs ファイルを確認します。

    追加された **TodoItem** オブジェクトがメモリ内に格納されている **ObservableCollection & lt;TodoItem & gt;**します。

3. キーを押して、 **f5 キーを押して** キーをプロジェクトをリビルドし、アプリケーションを開始します。

4. アプリケーションで、テキスト ボックスにテキストを入力し、クリックして、 **保存** ] ボタンをクリックします。

    ![][0]

    保存されたテキストが下のリストに表示されます。

##<a name="create-service"></a>Azure クラシック ポータルで新しいモバイル サービスを作成します。

[AZURE.INCLUDE [mobile-services-create-new-service-data](../../includes/mobile-services-create-new-service-data.md)]

##<a name="add-table"></a>新しいテーブルをモバイル サービスに追加する

[AZURE.INCLUDE [mobile-services-create-new-service-data-2](../../includes/mobile-services-create-new-service-data-2.md)]

##<a name="update-app"></a>モバイル サービスをデータ アクセスに使用するためにアプリケーションを更新する

モバイル サービスの準備が整ったら、ローカル コレクションの代わりに Mobile Services に項目を格納するようにアプリケーションを更新します。

1.  **ソリューション エクスプ ローラー** Visual Studio で、[プロジェクト名を右クリックして [ **NuGet パッケージの管理**します。

2. 左ペインで選択、 **オンライン** カテゴリで、検索 `WindowsAzure.MobileServices`, 、] をクリックして **インストール** 上、 **Azure Mobile Services** をパッケージ化し、使用許諾契約書に同意します。

    ![][7]

    これにより、Mobile Services クライアント ライブラリがプロジェクトに追加されます。

3.  [Azure classic portal], 、] をクリックして **Mobile Services**, 、作成したモバイル サービスをクリックします。

4. クリックして、 **ダッシュ ボード** タブし、メモ、 **サイトの URL**, 、順にクリックして **キーの管理** のメモに記録、 **アプリケーション キー**します。

    ![][8]

    これらの値は、アプリケーション コードからモバイル サービスにアクセスするときに必要になります。

5. Visual Studio で App.xaml.cs ファイルを開き、次の `using` ステートメントを追加またはコメント解除します。

        using Microsoft.WindowsAzure.MobileServices;

6. この同じファイルで定義する次のコードをコメント解除、 **MobileService** 変数、および URL とアプリケーション キーでモバイル サービスを指定、 **MobileServiceClient** コンス トラクターに順にします。

        //public static MobileServiceClient MobileService = new MobileServiceClient(
        //    "AppUrl",
        //    "AppKey"
        //);

    新しいインスタンスを作成 **MobileServiceClient** 、モバイル サービスにアクセスするために使用されます。

6. MainPage.cs ファイルで、次の `using` ステートメントを追加またはコメント解除します。

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json;

7. この DataModel フォルダーに置き換える、 **TodoItem** クラス定義を次のコード。

        public class TodoItem
        {
            public string Id { get; set; }

            [JsonProperty(PropertyName = "text")]
            public string Text { get; set; }

            [JsonProperty(PropertyName = "complete")]
            public bool Complete { get; set; }
        }

7. 既存を定義する行をコメント **項目** コレクション、次の行をコメント解除します。

        private MobileServiceCollection<TodoItem, TodoItem> items;
        private IMobileServiceTable<TodoItem> todoTable =
            App.MobileService.GetTable<TodoItem>();

    このコードでは、モバイル サービス対応のバインディング コレクション (**項目**) と SQL データベース テーブルのプロキシ クラス **TodoItem** (**todoTable**)。

7.  **InsertTodoItem** メソッドを設定するコード行を削除して、 **TodoItem**.**Id** プロパティには、追加、 **async** 修飾子をメソッドには、次のコード行をコメント解除します。

        await todoTable.InsertAsync(todoItem);

    このコードでは、新しい項目をテーブルに挿入します。

8.  **RefreshTodoItems** メソッドを追加、 **async** 修飾子をメソッドは、次のコード行をコメント解除します。

        items = await todoTable.ToCollectionAsync();

    これにより、todoTable 内で、モバイル サービスから返されたすべての TodoItem オブジェクトが格納される項目のコレクションへのバインディングが設定されます。

9.  **UpdateCheckedTodoItem** メソッドを追加、 **async** 修飾子をメソッドには、次のコード行をコメント解除します。

         await todoTable.UpdateAsync(item);

    これにより、項目の更新がモバイル サービスに送信されます。

バックエンド ストレージの Mobile Services を使用するようにアプリケーションを更新した後は、Mobile Services に対してアプリケーションをテストします。

##<a name="test-app"></a>新しいモバイル サービスに対するアプリケーションのテスト

1. Visual Studio で、F5 キーを押してアプリケーションを実行します。

2. 前に、テキスト ボックスに、テキストを入力し、クリックと **保存**します。

    これにより、新しい項目が挿入としてモバイル サービスに送信されます。

3.  [Azure classic portal], をクリックして **Mobile Services**, 、モバイル サービスをクリックします。

4. クリックして、 **データ** ] タブの [ **参照**します。

    ![][9]

    注意して、 **TodoItem** テーブルには、モバイル サービスによって生成された id 値を持つデータには今すぐが含まれていて、アプリケーションの TodoItem クラスに対応テーブルに列が自動的に追加されています。

これでチュートリアルは終了します。

## <a name="next-steps"> </a>次のステップ

このチュートリアルでは、Windows Phone 8 アプリで Mobile Services のデータを操作できるようにするための基本について説明しました。 次に、以下のその他のトピックのいずれかを読むことを検討してください。

* [アプリへの認証の追加](mobile-services-windows-phone-get-started-users.md)
  <br/>アプリケーションのユーザーを認証する方法について説明します。

* [アプリケーションにプッシュ通知を追加する](mobile-services-javascript-backend-windows-phone-get-started-push.md)
  <br/>Mobile Services を使用してアプリケーションにごく基本的なプッシュ通知を送信する方法について説明します。

* [Mobile Services C# の場合操作方法に関する概念リファレンス](mobile-services-windows-dotnet-how-to-use-client-library.md)
  <br/>.NET で Mobile Services を使用する方法について説明します。

<!-- Anchors. -->
[Download the Windows Phone 8 app project]: #download-app
[Create the mobile service]: #create-service
[Add a data table for storage]: #add-table
[Update the app to use Mobile Services]: #update-app
[Test the app against Mobile Services]: #test-app
[Next Steps]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-windows-phone-get-started-data/mobile-quickstart-startup-wp8.png
[7]: ./media/mobile-services-windows-phone-get-started-data/mobile-add-nuget-package-wp.png
[8]: ./media/mobile-services-windows-phone-get-started-data/mobile-dashboard-tab.png
[9]: ./media/mobile-services-windows-phone-get-started-data/mobile-todoitem-data-browse.png

<!-- URLs. -->

[Azure classic portal]: https://manage.windowsazure.com/
[Windows Phone 8 SDK]: http://go.microsoft.com/fwlink/p/?LinkID=268374
[Mobile Services SDK]: http://go.microsoft.com/fwlink/p/?LinkID=268375
[Developer Code Samples site]:  http://go.microsoft.com/fwlink/p/?LinkId=271146



