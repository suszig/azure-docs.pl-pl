<properties
    pageTitle=".NET バックエンド モバイル サービスでのユーザーのサービス側承認 | Microsoft Azure"
    description=".NET バックエンド モバイル サービスで権限のあるユーザーのアクセスを制限する方法を学習します。"
    services="mobile-services"
    documentationCenter="windows"
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.topic="article"
    ms.devlang="dotnet"
    ms.date="12/09/2015"
    ms.author="krisragh"/>


# モバイル サービスでのユーザーのサービス側承認

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


> [AZURE.SELECTOR-LIST (Platform | Backend)]
- [(Any | .NET)](mobile-services-dotnet-backend-service-side-authorization.md)
- [(Any | Javascript)](mobile-services-javascript-backend-service-side-authorization.md)


このトピックでは、サーバー側ロジックを使用してユーザーを承認する方法を説明します。 このチュートリアルでは、テーブル コントローラーを変更して、ユーザー ID に基づいてクエリをフィルター処理し、ユーザーが自分のデータのみにアクセスできるようにします。 ユーザー ID によるユーザーのクエリ結果のフィルター処理は、承認の最も基本的な形式です。 またシナリオによっては、特定のユーザーのアクセスを許可するエンドポイントなど、より詳細なユーザーの承認情報を確認するために、ユーザーまたはロールのテーブルを作成することもできます。

このチュートリアルでは、Mobile Services クイック スタートに基づいており、[既存の Mobile Services アプリケーションに認証の追加] のチュートリアルに基づいて記述します。 最初に [既存の Mobile Services アプリケーションに認証の追加] を入力してください。

## <a name="register-scripts"></a>データ アクセス メソッドを変更します。

1. Visual Studio で、モバイル プロジェクトを開き、DataObjects　フォルダーを展開して、**TodoItem.cs** を開きます。 **TodoItem** クラスは、データ オブジェクトを定義します。フィルター処理に使用するためには、**UserId** プロパティを追加する必要があります。 次の新しい UserId プロパティを **TodoItem** クラスに追加します。

        public string UserId { get; set; }

    >[AZURE.NOTE] このデータ モデルを変更して、データベース内の既存のデータを維持するには、使用する必要があります [Code First Migrations](mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)します。

2. Visual Studio で、Controllers フォルダーを展開開く **TodoItemController.cs** し、以下の追加ステートメントを使用します。

        using Microsoft.WindowsAzure.Mobile.Service.Security;

3. **PostTodoItem** メソッドを見つけ、メソッドの先頭に次のコードを追加します。

     // Get the logged in user
     var currentUser = User as ServiceUser;
    
     // Set the user ID on the item
     item.UserId = currentUser.Id;

 このコードでは、TodoItem テーブルに挿入される前に、項目に、認証済みユーザーのユーザー ID を追加します。

3. **GetAllTodoItems** メソッドを見つけ、既存の **return** ステートメントを次のコード行と置き換えます。

     // Get the logged in user
     var currentUser = User as ServiceUser;
    
     return Query().Where(todo => todo.UserId == currentUser.Id);

 このクエリは、返される TodoItem オブジェクトにフィルター処理を実施して、それぞれのユーザーが自分で挿入した項目のみを受け取るためのものです。

4. モバイル サービス プロジェクトを Azure に対して再発行します。


## <a name="test-app"></a>アプリケーションをテストします。

1. クライアント側のアプリケーションを実行している場合は、前のチュートリアルでデータベースに項目を挿入していても、項目が返されることはない点に注意してください。 これは、ユーザーの ID 列を含まない前の項目が挿入され、現在は null 値であるために発生します。

2. その他のログイン アカウントがある場合は、アプリケーションを終了して削除し、もう一度実行することによって、ユーザーは自分のデータのみ表示できることを確認します。 ログインの資格情報ダイアログ ボックスが表示される場合、別のログインを入力し、前のログインで入力した項目が表示されないことを確認します。








[register server scripts]: #register-scripts 
[next steps]: #next-steps 
[3]: ./media/mobile-services-dotnet-backend-ios-authorize-users-in-scripts/mobile-quickstart-startup-ios.png 
[add authentication to existing mobile services app]: mobile-services-dotnet-backend-ios-get-started-users.md 

