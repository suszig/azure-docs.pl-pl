<properties
    pageTitle="Azure モバイル アプリ (Windows 8.1) に対するオフライン同期の有効化 | Microsoft Azure"
    description="Azure モバイル アプリを使用して、Windows ストア アプリケーションのオフライン データをキャッシュして同期する方法を説明します。"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/22/2015"
    ms.author="wesmc"/>

# Windows アプリのオフライン同期を有効にする

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

## 概要

このチュートリアルでは、Azure モバイル アプリ バックエンドを使用して、Windows 8.1 ストアまたは Phone アプリケーションにオフライン サポートを追加する方法について説明します。 オフライン同期を使用すると、エンド ユーザーはネットワークにアクセスできなくても、データの表示、追加、変更など、モバイル アプリケーションとやり取りできます。 変更はローカル データベースに格納され、デバイスが再びオンラインになると、これらの変更がリモート バックエンドと同期されます。

このチュートリアルでは、チュートリアルでは、Windows 8.1 アプリケーション プロジェクトを更新します [Create a Windows app] Azure Mobile Apps のオフライン機能をサポートします。 ダウンロードしたクイック スタートのサーバー プロジェクトを使用しない場合は、データ アクセス拡張機能パッケージをプロジェクトに追加する必要があります。 サーバーの拡張機能パッケージの詳細については、次を参照してください。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)します。 

オフライン同期機能の詳細については、トピックを参照してください。 [Offline Data Sync in Azure Mobile Apps]します。

## 必要条件

このチュートリアルには、次のものが必要です。

* Windows 8.1 で実行されている Visual Studio 2013。
* 完了 [Windows アプリを作成する][create a windows app]です。
* [Azure Mobile Services SQLite Store][sqlite store nuget]
* [SQLite for Windows 8.1](http://www.sqlite.org/downloads)

## オフライン機能をサポートするようにクライアント アプリを更新する

Azure モバイル アプリのオフライン機能を使用すると、オフラインになっている状況でも、ローカル データベースとやり取りすることができます。 アプリケーションでこれらの機能を使用するには、`MobileServiceClient.SyncContext` をローカル ストアに初期化します。 その後、`IMobileServiceSyncTable` インターフェイスを使用してテーブルを参照します。 このチュートリアルでは、ローカル ストアに SQLite を使用します。

1. Windows 8.1 および Windows Phone 8.1 の SQLite ランタイムをインストールします。

    * **Windows 8.1 Runtime:** インストール [SQLite for Windows 8.1]します。
    * **Windows Phone 8.1:** インストール [SQLite for Windows Phone 8.1]します。

    >[AZURE.NOTE] 次の手順は Windows 10 UAP プロジェクトの場合も機能しますが、代わりにインストールする必要があります [SQLite for Windows 10]します。

2. Visual Studio で完成させたプロジェクトを開き、 [Create a Windows app] チュートリアルです。 インストール、 **Microsoft.Azure.Mobile.Client.SQLiteStore** Windows 8.1 ランタイムおよび Windows Phone 8.1 プロジェクトの NuGet パッケージ。 Windows Store 8.1 と Windows Phone 8.1 プロジェクトの両方に NuGet リファレンスを追加します。

    >[AZURE.NOTE] インストールは、インストールされているよりも、異なるバージョンの SQLite への参照を作成する場合は、コンパイル エラーが表示されます。 内の重複を削除することでこのエラーを解決する必要があります、 **参照** 、プロジェクト内のノードです。

3. ソリューション エクスプ ローラーで右クリックして **参照** Windows 8.1 ランタイムおよび Windows Phone 8.1 プラットフォーム プロジェクトおよびにある SQLite への参照があることを確認、 **拡張** セクションです。

    ![][1]
    </br>

    **Windows 8.1 ランタイム**

    ![][11]
    </br>

    **Windows Phone 8.1**

4. SQLite はプラットフォーム固有のアーキテクチャをなどを選択することが必要です、ネイティブ ライブラリ **x86**, 、**x64**, 、または **ARM**します。 **任意の CPU** はサポートされていません。 ソリューション エクスプ ローラーで、上部にある [ソリューション] をクリックし、プロセッサ アーキテクチャのドロップ ダウン ボックスをテストしたいサポート済みの設定の 1 つに変更します。

    ![][13]

5. ソリューション エクスプローラーで共有プロジェクトを使用し、MainPage.cs ファイルを開きます。 ファイルの先頭にある次の using ステートメントをコメント解除します。

        using Microsoft.WindowsAzure.MobileServices.SQLiteStore;  // offline sync
        using Microsoft.WindowsAzure.MobileServices.Sync;         // offline sync

6. MainPage.cs で、`todoTable` を `IMobileServiceTable` として初期化するコードの行をコメントにします。 `todoTable` を `IMobileServiceSyncTable` として初期化するコードの行をコメント解除します。

        //private IMobileServiceTable<TodoItem> todoTable = App.MobileService.GetTable<TodoItem>();
        private IMobileServiceSyncTable<TodoItem> todoTable = App.MobileService.GetSyncTable<TodoItem>(); // offline sync

7. MainPage.cs 内の `Offline sync` とマークされている部分で、`InitLocalStoreAsync` および `SyncAsync` メソッドをコメント解除します。 メソッド `InitLocalStoreAsync` は、SQLite ストアを使用してクライアントの同期コンテキストを初期化します。 Visual Studio で、すべてのコメント行を選択して使用することができます、 **Ctrl**+**K**+**U** のキーボード ショートカットをコメント解除します。

    `SyncAsync` では、プッシュ操作は `IMobileServicesSyncTable` の代わりに `MobileServiceClient.SyncContext` を実行することに注意してください。 これは、コンテキストはクライアントがすべてのテーブルに対して行った変更を追跡するためです。 これは、テーブル間にリレーションシップがある状況に対応することを目的としています。 この動作の詳細については、次を参照してください。 [Offline Data Sync in Azure Mobile Apps]します。

        private async Task InitLocalStoreAsync()
        {
            if (!App.MobileService.SyncContext.IsInitialized)
            {
                var store = new MobileServiceSQLiteStore("localstore.db");
                store.DefineTable<TodoItem>();
                await App.MobileService.SyncContext.InitializeAsync(store);
            }

            await SyncAsync();
        }

        private async Task SyncAsync()
        {
            await App.MobileService.SyncContext.PushAsync();
            await todoTable.PullAsync("todoItems", todoTable.CreateQuery());
        }

8. `OnNavigatedTo` イベント ハンドラーで、`InitLocalStoreAsync` の呼び出しをコメント解除します。

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            await InitLocalStoreAsync(); // offline sync
            await RefreshTodoItems();
        }

9. メソッド `InsertTodoItem`、`UpdateCheckedTodoItem`、および `ButtonRefresh_Click` にある 3 つの `SyncAsync` の呼び出しをコメント解除します。

        private async Task InsertTodoItem(TodoItem todoItem)
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);

            await SyncAsync(); // offline sync
        }

        private async Task UpdateCheckedTodoItem(TodoItem item)
        {
            await todoTable.UpdateAsync(item);
            items.Remove(item);
            ListItems.Focus(Windows.UI.Xaml.FocusState.Unfocused);

            await SyncAsync(); // offline sync
        }

        private async void ButtonRefresh_Click(object sender, RoutedEventArgs e)
        {
            ButtonRefresh.IsEnabled = false;

            await SyncAsync(); // offline sync
            await RefreshTodoItems();

            ButtonRefresh.IsEnabled = true;
        }

10. `SyncAsync` メソッドに例外ハンドラーを追加します。 オフラインの場合は、`MobileServicePushFailedException` が`PushResult.Status == CancelledByNetworkError` でスローされます。

        private async Task SyncAsync()
        {
            String errorString = null;

            try
            {
                await App.MobileService.SyncContext.PushAsync();
                await todoTable.PullAsync("todoItems", todoTable.CreateQuery()); // first param is query ID, used for incremental sync
            }

            catch (MobileServicePushFailedException ex)
            {
                errorString = "Push failed because of sync errors. You may be offine.\nMessage: " +
                  ex.Message + "\nPushResult.Status: " + ex.PushResult.Status.ToString();
            }
            catch (Exception ex)
            {
                errorString = "Pull failed: " + ex.Message +
                  "\n\nIf you are still in an offline scenario, " +
                  "you can try your Pull again when connected with your Mobile Serice.";
            }

            if (errorString != null)
            {
                MessageDialog d = new MessageDialog(errorString);
                await d.ShowAsync();
            }
        }

    この `PullAsync` の例では、リモート `todoTable` のすべてのレコードを取得しますが、クエリを渡すことによりレコードをフィルタリングすることも可能です。 `PullAsync` の最初のパラメーターは増分同期に使用されるクエリ ID ですが、この同期では、`UpdatedAt` タイムスタンプを使用して最後の同期から変更があったレコードのみを取得します。 クエリ ID は、クライアント アプリケーション内の各論理クエリに対して一意の、わかりやすい文字列にする必要があります。 増分同期を解除するには、`null` をクエリ ID として渡します。 これによってプル操作ごとにすべてのレコードを取得することになり、効率が悪くなる可能性があります。

    `MobileServicePushFailedException` はプッシュ操作とプル操作の両方で発生する場合があることに注意してください。 リレーションシップを持つすべてのテーブルに一貫性があることを確認するために、プル操作で内部的にプッシュが実行されることから、この例外はプルで発生する可能性があります。

11. Visual Studio でキーを押して、 **f5 キーを押して** キーを再構築し、クライアント アプリケーションを実行します。 アプリケーションは、前にオフライン同期の変更を実施したときと同様に動作しますが、これは挿入や更新の操作時にアプリケーションが同期の動作をするためです。 ただし、オフラインのシナリオで使用できるローカルのデータベースに入力を行います。  次のセクションのオフラインのシナリオを実施すれば、ローカルのデータベースに入力が行われます。  

## <a name="update-sync"></a>クライアント アプリの同期の動作を更新する

このセクションでは、クライアント アプリを変更して、Azure モバイル アプリのバックエンドへの接続を解除したオフラインのシナリオをシミュレートします。 データ項目を追加すると、例外ハンドラーによって、アプリがオフライン モードで実行されていることが `PushResult.Status == CancelledByNetworkError` で通知されます。 追加された項目はローカル ストアで保持されますが、再びオンラインになって Azure モバイル アプリのバックエンドにプッシュできるようになるまではモバイル アプリのバックエンドに同期されることはありません。

1. 共有プロジェクトで App.xaml.cs を編集します。 初期化をコメント アウト、 **MobileServiceClient** し、無効なモバイル アプリの URL を使用する次の行を追加します。

         public static MobileServiceClient MobileService = 
                new MobileServiceClient("https://your-service.azurewebsites.fail");

    アプリでも認証が使用されている場合、それが原因でサインインは失敗するので注意してください。 また、デバイス上で Wi-Fi および移動体通信ネットワークを無効にしてオフライン動作をデモンストレーションすることも、機内モードを使用することもできます。

2. キーを押して **f5 キーを押して** 、アプリケーションをビルドして実行します。 アプリを起動した際の更新時には同期が失敗することに注意してください。
3. 新しい todo 項目を入力し、クリックして **保存** ごとにします。 プッシュはそれぞれ `PushResult.Status=CancelledByNetworkError` で失敗します。 新しい todo 項目は、モバイル アプリ バックエンドにプッシュされるまでは、ローカル ストア内にのみ存在します。 
 
    `PushResult.Status=CancelledByNetworkError` の例外ダイアログは非表示にできます。そうすれば、クライアント アプリケーションはモバイル アプリ バックエンドに接続されているかのように動作し、作成、読み取り、更新、削除 (CRUD) 操作のすべてをシームレスにサポートします。 

4. アプリケーションを終了し、再起動して、作成した新しい項目がローカル ストアに保存されていることを確認します。

5. (省略可能)Visual Studio で開きます **サーバー エクスプ ローラー**します。 内のデータベースに移動 **Azure**]-> [**SQL データベース**します。 データベースを右クリックして **[SQL Server オブジェクト エクスプ ローラーで開く**します。 これで SQL データベースのテーブルとその内容を参照できます。 バックエンド データベース内のデータが変更されていないことを確認します。

6. (省略可能) Fiddler や Postman などの REST ツールを使用して、モバイルのバックエンドをクエリします。その際、`https://your-mobile-app-backend-name.azurewebsites.net/tables/TodoItem` の形式で、GET クエリを使用します。 

## <a name="update-online-app"></a>モバイル アプリ バックエンドに再接続するようにアプリケーションを更新する

ここでは、アプリケーションをモバイル アプリ バックエンドに再接続します。 これは、アプリケーションがオフライン状態から、モバイル アプリ バックエンドとのオンライン状態に移行したことをシミュレートします。 初めてアプリケーションを実行すると、`OnNavigatedTo` イベント ハンドラーが `InitLocalStoreAsync` を呼び出します。 これが次に `SyncAsync` を呼び出し、ローカル ストアとバックエンドのデータベースを同期します。 そのため、アプリはスタートアップ時に同期を試みることになります。

1. 共有プロジェクトで App.xaml.cs を開きます。 `MobileServiceClient` の初期化をコメント解除し、正しいモバイル アプリ の URL とゲートウェイの URL を使用します。

2. キーを押して、 **f5 キーを押して** キー、アプリケーションをリビルドして実行します。 アプリは、プッシュとプルの操作によって、`OnNavigatedTo` イベント ハンドラーが実行されるとすぐに、ローカルでの変更と Azure Mobile Apps バックエンドを同期します。

3. (省略可能) SQL Server Object Explorer または Fiddler などの REST ツールを使用して、更新データを表示します。 データが同期されるのは、Azure モバイル アプリのバックエンドのデータベースとローカル ストアの間であることに注意してください。

4. アプリケーションで、ローカル ストアで完了させる項目の横にあるチェック ボックスをクリックします。

  `UpdateCheckedTodoItem` 呼び出し `SyncAsync` を完全なモバイル アプリ バックエンドでは、各項目を同期します。 `SyncAsync` プッシュとプルの両方を呼び出します。 ただし、注意点を **に変更を加え、クライアントであるテーブルに対して、プルを実行するたびにクライアントの同期コンテキストでのプッシュは常に最初に実行される自動的に**します。 これは、ローカル ストアのすべてのテーブルとリレーションシップの一貫性を確実に保つためです。 そのため、ここでは `PushAsync` への呼び出しを削除できます。呼び出しはプルを実行すれば自動で行われるためです。 この動作は、認識をしていない場合、予期せぬプッシュを引き起こすことがあります。 この動作の詳細については、次を参照してください。 [Offline Data Sync in Azure Mobile Apps]します。


##概要

モバイル サービスのオフライン機能をサポートするために、`IMobileServiceSyncTable` インターフェイスを使用して、ローカル ストアにより `MobileServiceClient.SyncContext` を初期化しました。 この場合は、ローカル ストアは、SQLite データベースでした。

モバイル サービスに対する通常の CRUD 操作は、まるでアプリケーションが接続されているかのように機能しますが、すべての操作はローカル ストアに対して実施されます。

ローカル ストアをサーバーと同期する場合は、`IMobileServiceSyncTable.PullAsync` メソッドと `MobileServiceClient.SyncContext.PushAsync` メソッドを使用しました。

*  変更内容をサーバーにプッシュするために、`IMobileServiceSyncContext.PushAsync()` を呼び出しました。 このメソッドは、すべてのテーブルに対して変更をプッシュするため、同期テーブルではなく `IMobileServicesSyncContext` のメンバーです。

    何らかの方法で (CUD 操作により) ローカルで変更されたレコードだけが、サーバー宛てに送信されます。

* サーバー上のテーブルからアプリケーションにデータをプルするために、`IMobileServiceSyncTable.PullAsync` を呼び出しました。

    クライアントの同期コンテキストがテーブル上の変更を追跡する場合、プルは常に同期コンテキストのプッシュを発行します。 これは、ローカル ストアのすべてのテーブルとリレーションシップの一貫性を確実に保つためです。

    クライアントに格納されているデータをフィルターするためのクエリを指定できる `PullAsync()` のオーバーロードもあります。 クエリが渡されない場合、`PullAsync()` は、対応するテーブル (またはクエリ) 内のすべての行をプルします。 アプリケーションと同期する必要がある変更のみをフィルター処理するクエリを渡すことができます。

* 増分同期を有効にするには、クエリ ID を `PullAsync()` に渡します。 クエリ ID は、最後のプル操作の結果の最後に更新されたタイムスタンプを格納するのに使用されます。 クエリ ID は、アプリ内の各論理クエリに対して一意の、わかりやすい文字列にする必要があります。 クエリにパラメーターがある場合は、同じパラメーター値をクエリ ID の一部に含める必要があります。

    たとえば、userid でフィルター処理をする場合、一意のクエリ ID は次のようになります。

        await PullAsync("todoItems" + userid, syncTable.Where(u => u.UserId = userid));

    増分同期を無効にする場合は、`null` をクエリ ID として渡します。 この場合、`PullAsync` への呼び出しごとにすべてのレコードが再取得されるため、場合によっては非効率となります。

* アプリで定期的に `IMobileServiceSyncTable.PurgeAsync()` を呼び出して、ローカル ストアを消去する必要があります。


## その他のリソース

* [Azure モバイル アプリでのオフライン データ同期]

* [Cloud Cover: Offline Sync in Azure Mobile Services] \ (注: ビデオは、モバイル サービスには、Azure モバイル アプリで同様の方法でのオフライン同期機能)

* [Azure Friday: Azure Mobile Services のオフライン対応アプリ]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Offline Data Sync in Azure Mobile Apps]: ../app-service-mobile-offline-data-sync.md
[create a windows app]: ../app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921

[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
 
[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/


