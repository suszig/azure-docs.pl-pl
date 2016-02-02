<properties
    pageTitle="ユニバーサル Windows アプリでのオフライン データの競合の処理 | Microsoft Azure"
    description="Azure Mobile Services を使用して、ユニバーサル Windows アプリケーションでのオフライン データの同期時に発生する競合を処理する方法について説明します。"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/12/2015"
    ms.author="glenga"/>



# モバイル サービスでのオフライン データの同期との競合の処理

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-offline-conflicts](../../includes/mobile-services-selector-offline-conflicts.md)]

## 概要

このトピックでは、Azure Mobile Services のオフライン機能を使用しているときに、データを同期し、競合を処理する方法について説明します。

このチュートリアルのビデオを見る場合は、下のクリップでこのチュートリアルと同じ手順が表示されます。

> [AZURE.VIDEO build-offline-apps-with-mobile-services]

このチュートリアルでは、オフラインの同期で発生する競合の処理をサポートするアプリケーション用に、ユニバーサル Windows C# ソリューションをダウンロードします。 モバイル サービスとアプリケーションを統合します。次に、Windows ストア 8.1 と Windows Phone 8.1 のクライアントを実行して同期の競合を生成して、これを解決します。

このチュートリアルでは、手順およびサンプル アプリケーションを前のチュートリアルに基づいて [オフライン データの開始を取得] です。 このチュートリアルを開始する前に完了してください [オフライン データの開始を取得] です。


## 前提条件

このチュートリアルでは、Windows 8.1 で実行されている Visual Studio 2013 が必要です。


## サンプル プロジェクトのダウンロード

![][0]

このチュートリアルは、[Todo オフラインのモバイル サービスのサンプル] がローカルのオフライン ストアと Azure のモバイル サービス データベースの間の同期の競合を処理する方法のチュートリアルです。

1. [Mobile Services サンプル GitHub リポジトリ] の zip ファイルをダウンロードして、作業ディレクトリに抽出します。

2. 場合は、まだインストールしていない SQLite for Windows 8.1 と Windows Phone 8.1 で説明したように、[オフライン データの開始を取得] チュートリアルでは、両方をランタイムをインストールします。

3. Visual Studio 2013 で、*mobile-services-samples\TodoOffline\WindowsUniversal\TodoOffline-Universal.sln* ソリューション ファイルを開きます。 **F5** キーを押してプロジェクトをリビルドして実行します。 NuGet パッケージが復元され、参照が正しく設定されていることを確認します。
    >[AZURE.NOTE] SQLite ランタイムへの以前の参照を削除してで説明したように更新された参照で置き換える必要があります、[の使用」のオフライン データのチュートリアルです。

4. アプリケーションの [**TodoItem の挿入**] にテキストを入力し、[**保存**] をクリックして、ローカルのストアに Todoitem を追加します。 次に、アプリケーションを閉じます。

アプリケーションはモバイル サービスにまだ接続されていないため、**[プッシュ]** および **[プル]** は例外をスローすることに注意してください。




## モバイル サービスに対するアプリケーションをテストします。

次に、Mobile Services に対してアプリケーションをテストします。

1. [Azure クラシック ポータルで] をクリックして、モバイル サービスのアプリケーション キーを検索 **キーの管理** のコマンド バーで、 **ダッシュ ボード** ] タブをクリックします。 **アプリケーション キー**をコピーします。

2. Visual Studio のソリューション エクスプローラーで、クライアント サンプル プロジェクトの App.xaml.cs ファイルを開きます。 **MobileServiceClient** の初期化を変更して、モバイル サービス URL およびアプリケーション キーを使用します。

         public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://your-mobile-service.azure-mobile.net/",
            "Your AppKey"
        );

3. Visual Studio で、**F5** キーを押してアプリケーションをビルドおよび再実行します。

    ![][0]


## 競合を作成するためのバックエンドでのデータの更新

現実のシナリオでは、1 つのアプリケーションがデータベースのレコードに更新をプッシュし、次に別のアプリケーションがそのレコードの使用していないバージョンを使用して同じレコードへプッシュしようとする場合に同期の競合が発生します。 説明した場合、[オフライン データ] を開始、オフラインの同期の機能をサポートするバージョンのシステム プロパティが必要です。 このバージョン情報は、データベースの更新ごとに検証されます。 アプリのインスタンスが以前のバージョンを使用してレコードを更新しようとすると、競合が発生し、としてキャッチされます、 `MobileServicePreconditionFailedException` アプリケーションでします。 アプリを捕捉しなかった場合、 `MobileServicePreconditionFailedException` 、 `MobileServicePushFailedException` は最終的にスローされます発生した同期エラーの数を示します。
>[AZURE.NOTE] オフライン データの同期で削除されたレコードの同期をサポートするために有効にしてください [論理的な削除](mobile-services-using-soft-delete.md)します。 ローカル ストアは、または呼び出しのレコードを手動で削除する必要がある場合は、 `imobileservicesynctable::purgeasync()` ローカル ストアを消去します。


次の手順では、同時に実行する Windows Phone 8.1 クライアントと Windows ストア 8.1 のクライアントに競合が発生し、サンプルを使用してこれを解決する方法を示します。

1. Visual Studio で、Windows Phone 8.1 プロジェクトを右クリックし、[**スタートアップ プロジェクトに設定**] をクリックします。 次に、**Ctrl+F5** キーを押して、デバッグせずに Windows Phone 8.1 クライアントを実行します。 エミュレーターで Windows Phone 8.1 クライアントが起動したら、[**プル**] をクリックして、ローカル ストアとデータベースの現在の状態を同期します。

    ![][3]

2. Visual Studio で Windows 8.1 ランタイム プロジェクトを右クリックしてから [**スタートアップ プロジェクトに設定**] をクリックして、もう一度スタートアップ プロジェクトに設定します。 次に、**F5** を押して実行します。 Windows ストア 8.1 クライアントが起動したら、[**プル**] をクリックして、ローカル ストアとデータベースの現在の状態を同期します。

    ![][4]

3. この時点では、両方のクライアントはデータベースと同期しています。 両クライアントのコードは増分同期も使用しているため、完了していない Todo 項目のみ同期します。 完了した Todo 項目は無視されます。 項目のいずれかを選択し、両クライアントで同じ項目のテキストを別の値に編集します。 [**プッシュ**] をクリックして両方の変更と、サーバーのデータベースを同期します。

    ![][5]

    ![][6]

4. 最後にプッシュを実行したクライアントには競合が発生しますが、ユーザーはデータベースにコミットする値をどちらにするか決定できます。 例外により、競合を解決するために使用する正しいバージョンの値が提供されます。

    ![][7]



## 同期の競合を処理するためのコードの確認

Mobile Services でオフライン機能を使用するためには、ローカル データベースとデータ転送オブジェクトの両方に [バージョン] 列を含める必要があります。 更新することによってこれを行う、 `TodoItem` クラスを次のメンバー。

        [Version]
        public string Version { get; set; }

`_ _Version` では、ローカルのデータベースの列が含まれている、  `OnNavigatedTo()` メソッドと、 `TodoItem` 、ローカル ストアを定義するクラスを使用します。

実装するクラスを作成するコードでオフラインの同期の競合を処理する `IMobileServiceSyncHandler`します。 呼び出しでは、この型のオブジェクトを渡す `MobileServiceClient.SyncContext.InitializeAsync()`します。 これにも発生、  `OnNavigatedTo()` 、サンプルのメソッドです。

     await App.MobileService.SyncContext.InitializeAsync(store, new SyncHandler(App.MobileService));

クラス `SyncHandler` で **SyncHandler.cs** 実装 `IMobileServiceSyncHandler`します。 メソッド `ExecuteTableOperationAsync` 各プッシュ操作がサーバーに送信されるときに呼び出されます。 型の例外 `MobileServicePreconditionFailedException` がスローされると、項目のローカルおよびリモート バージョン間で競合が発生することを意味します。

ローカル項目を優先して競合を解決するには、操作を再試行します。 競合が発生すると、ローカル項目のバージョンはサーバーのバージョンと一致するように更新されるため、操作をもう一度実行するとサーバーの変更はローカルの変更で上書きされます。

    await operation.ExecuteAsync();

サーバー項目を優先して競合を解決するから戻ります、 `ExecuteTableOperationAsync`します。 オブジェクトのローカル バージョンは破棄され、サーバーの値に置き換えられます。

プッシュ操作は停止 (ただし、キューに置かれた変更は保持) するメソッドを使用 `AbortPush()`:

    operation.AbortPush();

これによって現在のプッシュ操作は停止は、場合に、現在の操作を含めて、保留中のすべての変更が保存されます `AbortPush` から呼び出される `ExecuteTableOperationAsync`します。 次回 `PushAsync()` が呼び出されると、これらの変更はサーバーに送信します。

プッシュがキャンセルされると、 `PushAsync` をスロー、 `MobileServicePushFailedException`, 、し、例外プロパティ `PushResult.Status` 設定値を持つ `mobileservicepushstatus.cancelledbyoperation を含みます`します。






[0]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/mobile-services-handling-conflicts-app-run1.png 
[1]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/javascript-backend-database.png 
[2]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/dotnet-backend-database.png 
[3]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/wp81-view.png 
[4]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/win81-view.png 
[5]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/wp81-edit-text.png 
[6]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/win81-edit-text.png 
[7]: ./media/mobile-services-windows-store-dotnet-handling-conflicts-offline-data/conflict.png 
[handling conflicts code sample]: http://go.microsoft.com/fwlink/?LinkId=394787 
[get started with mobile services]: ../mobile-services-windows-store-get-started.md 
[get started with offline data]: mobile-services-windows-store-dotnet-get-started-offline-data.md 
[sqlite for windows 8.1]: http://go.microsoft.com/fwlink/?LinkId=394776 
[azure classic portal]: https://manage.windowsazure.com/ 
[handling database conflicts]: mobile-services-windows-store-dotnet-handle-database-conflicts.md#test-app 
[mobile services samples github repository]: http://go.microsoft.com/fwlink/?LinkId=512865 
[todo offline mobile services sample]: http://go.microsoft.com/fwlink/?LinkId=512866 

