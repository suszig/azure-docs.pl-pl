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

このチュートリアルでは、Azure Mobile Apps のオフライン機能をサポートするために [作成、Windows アプリ] に関するチュートリアルから Windows 8.1 アプリケーション プロジェクトを更新します。 ダウンロードしたクイック スタートのサーバー プロジェクトを使用しない場合は、データ アクセス拡張機能パッケージをプロジェクトに追加する必要があります。 サーバーの拡張機能パッケージの詳細については、次を参照してください。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)します。

オフライン同期機能の詳細については、「[Azure Mobile Apps でのオフライン データ同期]」を参照してください。

## 必要条件

このチュートリアルには、次のものが必要です。

* Windows 8.1 で実行されている Visual Studio 2013。
* 完了 [Windows アプリを作成する ][create a windows app]します。
* [Azure Mobile Services SQLite Store ][sqlite store nuget]
* [SQLite for Windows 8.1](http://www.sqlite.org/downloads)

## オフライン機能をサポートするようにクライアント アプリを更新する

Azure モバイル アプリのオフライン機能を使用すると、オフラインになっている状況でも、ローカル データベースとやり取りすることができます。 アプリケーションでこれらの機能を使用する初期化、 `MobileServiceClient.SyncContext` をローカル ストアにします。 使用してテーブルを参照、 `IMobileServiceSyncTable` インターフェイスです。 このチュートリアルでは、ローカル ストアに SQLite を使用します。

1. Windows 8.1 および Windows Phone 8.1 の SQLite ランタイムをインストールします。

    * **Windows 8.1 Runtime:** [Windows 8.1 用 SQLite] をインストールします。
    * **Windows Phone 8.1:** [SQLite for Windows Phone 8.1] をインストールします。
    >[AZURE.NOTE] 次の手順は Windows 10 UAP プロジェクトでも機能しますが、代わりに、[SQLite for Windows 10] をインストールする必要があります。

2. Visual Studio では、[Windows アプリを作成する] このチュートリアルで完成させたプロジェクトを開きます。 Windows 8.1 ランタイムおよび Windows Phone 8.1 プロジェクト向けの **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet パッケージをインストールします。 Windows Store 8.1 と Windows Phone 8.1 プロジェクトの両方に NuGet リファレンスを追加します。
    >[AZURE.NOTE] インストールによって、すでにインストールしてある SQLite とは別のバージョンのリファレンスが追加で作成される場合、コンパイル エラーとなります。 このエラーを解決するには、プロジェクト内で **参照** ノードが重複しないようにします。

3. ソリューション エクスプローラーで Windows 8.1 ランタイムおよび Windows Phone 8.1 プラットフォーム プロジェクトの [**参照設定**] を右クリックし、[**拡張**] セクションにある SQLite への参照を追加します。

    ![][1]
    </br>

    **Windows 8.1 Runtime**

    ![][11]
    </br>

    **Windows Phone 8.1**

4. SQLite はネイティブ ライブラリであり、**x86**、**x64**、**ARM** などのプラットフォーム固有のアーキテクチャを選択する必要があります。 **[任意の CPU]** はサポートされません。 ソリューション エクスプ ローラーで、上部にある [ソリューション] をクリックし、プロセッサ アーキテクチャのドロップ ダウン ボックスをテストしたいサポート済みの設定の 1 つに変更します。

    ![][13]

5. ソリューション エクスプローラーで共有プロジェクトを使用し、MainPage.cs ファイルを開きます。 ファイルの先頭にある次の using ステートメントをコメント解除します。

        using Microsoft.WindowsAzure.MobileServices.SQLiteStore;  // offline sync
        using Microsoft.WindowsAzure.MobileServices.Sync;         // offline sync

6. MainPage.cs で、初期化するコードの行をコメント `todoTable` として、 `IMobileServiceTable`します。 初期化するコードの行をコメント解除 `todoTable` として、 `IMobileServiceSyncTable`:

        //private IMobileServiceTable<TodoItem> todoTable = App.MobileService.GetTable<TodoItem>();
        private IMobileServiceSyncTable<TodoItem> todoTable = App.MobileService.GetSyncTable<TodoItem>(); // offline sync

7. MainPage.cs で、マークされているリージョンに `オフライン同期`, 、メソッドのコメントを解除 `InitLocalStoreAsync` と `SyncAsync`します。 メソッド `InitLocalStoreAsync` SQLite ストアを使用してクライアントの同期コンテキストを初期化します。 Visual Studio では、すべてのコメント行を選択して、**Ctrl** + **K** + **U** のショートカット キーでコメント解除できます。

 `SyncAsync` に、プッシュ操作を実行、 `MobileServiceClient.SyncContext` の代わりに、 `IMobileServicesSyncTable`します。 これは、コンテキストはクライアントがすべてのテーブルに対して行った変更を追跡するためです。 これは、テーブル間にリレーションシップがある状況に対応することを目的としています。 この動作の詳細については、[Azure Mobile Apps のオフライン データ同期] を参照してください。

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

8. `OnNavigatedTo` イベント ハンドラーへの呼び出しをコメント解除 `InitLocalStoreAsync`:

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            await InitLocalStoreAsync(); // offline sync
            await RefreshTodoItems();
        }

9. 呼び出しをコメント解除します、3 `SyncAsync` メソッドで `InsertTodoItem`, 、`UpdateCheckedTodoItem`, 、および `ButtonRefresh_Click`:

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

10. 例外ハンドラーを追加、 `SyncAsync` メソッドです。 オフラインの状況では、 `MobileServicePushFailedException` がスローされ `PushResult.Status CancelledByNetworkError = =`します。

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

この `PullAsync` 例では、リモート システムのすべてのレコードを取得します `todoTable`, 、クエリを渡すことでレコードをフィルター処理することも、です。 最初のパラメーター `PullAsync` を使用する増分同期に使用されるクエリ id、 `UpdatedAt` タイムスタンプ、前回の同期以降に変更されたレコードのみを取得します。 クエリ ID は、クライアント アプリケーション内の各論理クエリに対して一意の、わかりやすい文字列にする必要があります。 オプトアウトの増分同期に渡す `null` クエリ ID として これによってプル操作ごとにすべてのレコードを取得することになり、効率が悪くなる可能性があります。

なお、 `MobileServicePushFailedException` プッシュ操作とプル操作の両方で発生することができます。 リレーションシップを持つすべてのテーブルに一貫性があることを確認するために、プル操作で内部的にプッシュが実行されることから、この例外はプルで発生する可能性があります。

11. Visual Studio で、**F5** キーを押してクライアント アプリをリビルドして実行します。 アプリケーションは、前にオフライン同期の変更を実施したときと同様に動作しますが、これは挿入や更新の操作時にアプリケーションが同期の動作をするためです。 ただし、オフラインのシナリオで使用できるローカルのデータベースに入力を行います。 次のセクションのオフラインのシナリオを実施すれば、ローカルのデータベースに入力が行われます。

## <a name="update-sync"></a>更新プログラムのクライアント アプリの同期の動作

このセクションでは、クライアント アプリを変更して、Azure モバイル アプリのバックエンドへの接続を解除したオフラインのシナリオをシミュレートします。 データ項目を追加すると、例外ハンドラーが通知されます、アプリがオフライン モードで動作している `PushResult.Status CancelledByNetworkError = =`します。 追加された項目はローカル ストアで保持されますが、再びオンラインになって Azure モバイル アプリのバックエンドにプッシュできるようになるまではモバイル アプリのバックエンドに同期されることはありません。

1. 共有プロジェクトで App.xaml.cs を編集します。 **MobileServiceClient** の初期化をコメント アウトし、無効なモバイル アプリ URL を使用する次の行を追加します。

         public static MobileServiceClient MobileService = 
                new MobileServiceClient("https://your-service.azurewebsites.fail");

    アプリでも認証が使用されている場合、それが原因でサインインは失敗するので注意してください。 また、デバイス上で Wi-Fi および移動体通信ネットワークを無効にしてオフライン動作をデモンストレーションすることも、機内モードを使用することもできます。

2. **F5** キーを押し、アプリケーションをビルドして実行します。 アプリを起動した際の更新時には同期が失敗することに注意してください。
3. 新しい todo 項目をいくつか入力し、それぞれについて [**保存**] をクリックします。 1 つのプッシュが失敗した、 `PushResult.Status=CancelledByNetworkError`します。 新しい todo 項目は、モバイル アプリ バックエンドにプッシュされるまでは、ローカル ストア内にのみ存在します。

    [例外] ダイアログを非表示にすることが `PushResult.Status=CancelledByNetworkError`, 、し、クライアント アプリケーションはすべてをサポートするモバイル アプリ バックエンドに接続されている作成と動作、読み取り、更新、削除 (CRUD) 操作をシームレスにします。

4. アプリケーションを終了し、再起動して、作成した新しい項目がローカル ストアに保存されていることを確認します。

5. (省略可能) Visual Studio で、**サーバー エクスプローラー**を開きます。 **[Azure]**、**[SQL Databases]** の順に選択して、データベースに移動します。 データベースを右クリックし、**[SQL Server オブジェクト エクスプローラーで開く]** を選択します。 これで SQL データベースのテーブルとその内容を参照できます。 バックエンド データベース内のデータが変更されていないことを確認します。

6. (省略可能)Fiddler や Postman のような REST ツールを使用して形式で取得クエリを使用して、モバイル バックエンドをクエリ `https://your-mobile-app-backend-name.azurewebsites.net/tables/TodoItem`します。

## <a name="update-online-app"></a>モバイル アプリ バックエンドに再接続するアプリケーションを更新します。

ここでは、アプリケーションをモバイル アプリ バックエンドに再接続します。 これは、アプリケーションがオフライン状態から、モバイル アプリ バックエンドとのオンライン状態に移行したことをシミュレートします。 最初に、アプリケーションを実行すると、 `OnNavigatedTo` イベント ハンドラーが呼び出す `InitLocalStoreAsync`します。 これは順番に呼び出します `SyncAsync` バックエンド データベースと、ローカル ストアを同期します。 そのため、アプリはスタートアップ時に同期を試みることになります。

1. 共有プロジェクトで App.xaml.cs を開きます。 先の初期化をコメント解除 `MobileServiceClient` 正しいモバイル アプリ URL、ゲートウェイの URL を使用します。

2. **F5** キーを押して、アプリケーションをリビルドして実行します。 アプリ プッシュとプル操作を使用して、Azure モバイル アプリ バックエンドと、ローカルの変更を同期するとすぐに、 `OnNavigatedTo` イベント ハンドラーを実行します。

3. (省略可能) SQL Server Object Explorer または Fiddler などの REST ツールを使用して、更新データを表示します。 データが同期されるのは、Azure モバイル アプリのバックエンドのデータベースとローカル ストアの間であることに注意してください。

4. アプリケーションで、ローカル ストアで完了させる項目の横にあるチェック ボックスをクリックします。

  `UpdateCheckedTodoItem` 呼び出し `SyncAsync` を完全なモバイル アプリ バックエンドでは、各項目を同期します。 `SyncAsync` はプッシュとプルの両方を呼び出します。 ただし、**クライアントが変更を行ったテーブルに対してプルを実行すると、クライアントの同期コンテキストへのプッシュが常に最初に自動的に実行される**ことに注意してください。 これは、ローカル ストアのすべてのテーブルとリレーションシップの一貫性を確実に保つためです。 それではここでおでしたが呼び出しを削除する `PushAsync` プルを実行するときに自動的に実行されるためです。 この動作は、認識をしていない場合、予期せぬプッシュを引き起こすことがあります。 この動作の詳細については、[Azure Mobile Apps のオフライン データ同期] を参照してください。


## 概要

使用してモバイル サービスのオフライン機能をサポートするために、 `IMobileServiceSyncTable` インターフェイスおよび初期化さ `MobileServiceClient.SyncContext` 、ローカル ストアを使用します。 この場合は、ローカル ストアは、SQLite データベースでした。

モバイル サービスに対する通常の CRUD 操作は、まるでアプリケーションが接続されているかのように機能しますが、すべての操作はローカル ストアに対して実施されます。

使用して、サーバーとローカル ストアを同期するときに、 `IMobileServiceSyncTable.PullAsync` と `MobileServiceClient.SyncContext.PushAsync` メソッドです。

*  サーバーに変更をプッシュするために呼び出しました `IMobileServiceSyncContext.PushAsync()`します。 このメソッドのメンバーである `IMobileServicesSyncContext` 、同期ではなくテーブルのすべてのテーブルに対して変更をプッシュするためです。

    何らかの方法で (CUD 操作により) ローカルで変更されたレコードだけが、サーバー宛てに送信されます。

* アプリケーション サーバーのテーブルからのデータをプルするため呼び出しました `IMobileServiceSyncTable.PullAsync`します。

    クライアントの同期コンテキストがテーブル上の変更を追跡する場合、プルは常に同期コンテキストのプッシュを発行します。 これは、ローカル ストアのすべてのテーブルとリレーションシップの一貫性を確実に保つためです。

    オーバー ロードも `PullAsync()` ようにクライアントに格納されているデータをフィルター処理するために指定するクエリ。 クエリが渡されない場合 `PullAsync()` は、対応するテーブル (またはクエリ) 内のすべての行をプルします。 アプリケーションと同期する必要がある変更のみをフィルター処理するクエリを渡すことができます。

* 増分同期を有効にするをクエリ ID を渡します `PullAsync()`します。 クエリ ID は、最後のプル操作の結果の最後に更新されたタイムスタンプを格納するのに使用されます。 クエリ ID は、アプリ内の各論理クエリに対して一意の、わかりやすい文字列にする必要があります。 クエリにパラメーターがある場合は、同じパラメーター値をクエリ ID の一部に含める必要があります。

    たとえば、userid でフィルター処理をする場合、一意のクエリ ID は次のようになります。

        await PullAsync("todoItems" + userid, syncTable.Where(u => u.UserId = userid));

    増分同期を無効にする場合は、渡す `null` クエリ ID として 呼び出すたびにすべてのレコードを取得する場合は、 `PullAsync`, 、これは非効率です。

* アプリを定期的に呼び出す必要があります `IMobileServiceSyncTable.PurgeAsync()` ローカル ストアを消去します。


## その他のリソース

* [Azure のモバイル アプリでのオフライン データの同期]

* [カバーをクラウド: Azure Mobile Services でのオフライン同期] \ (注: ビデオは、モバイル サービスには、Azure モバイル アプリで同様の方法でのオフライン同期機能)

* [Azure Friday: Azure Mobile Services でのオフライン対応のアプリケーション]





[update the app to support offline features]: #enable-offline-app 
[update the sync behavior of the app]: #update-sync 
[update the app to reconnect your mobile apps backend]: #update-online-app 
[next steps]: #next-steps 
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png 
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png 
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png 
[offline data sync in azure mobile apps]: ../app-service-mobile-offline-data-sync.md 
[create a windows app]: ../app-service-mobile-windows-store-dotnet-get-started.md 
[sqlite for windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919 
[sqlite for windows phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920 
[sqlite for windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921 
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/ 
[cloud cover: offline sync in azure mobile services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri 
[azure friday: offline-enabled apps in azure mobile services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/ 

