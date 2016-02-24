
既定では、モバイル アプリ バックエンドの API は匿名で呼び出すことができます。 次に、認証されたクライアントのみにアクセスを制限する必要があります。  

+ **(ポータル) を使用して Node.js バックエンド** :  
    
    モバイル アプリの **設定**, 、] をクリックして **簡単テーブル** と、テーブルを選択します。 をクリックして **アクセス許可を変更**, [ **認証済みアクセスのみ** アクセス権をすべて次] をクリックし、 **保存**します。 

+ **.NET バックエンド (c#)**:  

    サーバー プロジェクトに移動 **コント ローラー** > **TodoItemController.cs**します。 追加、 `[Authorize]` 属性を **TodoItemController** クラスに、次のようにします。 アクセスを特定のメソッドのみに制限するには、この属性を、クラスではなく、そのメソッドのみに適用するだけです。 サーバー プロジェクトを発行します。


        [Authorize]
        public class TodoItemController : TableController<TodoItem>

+ **(Node.js コード) を使用して Node.js バックエンド** :  
    
    テーブルへのアクセスに対して認証を要求するには、Node.js サーバー スクリプトに次の行を追加します。


        table.access = 'authenticated';

    For more details, refer to [How to: Require authentication for access to tables](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). To learn how to download the quickstart code project from your site, see [How to: Download the Node.js backend quickstart code project using Git](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).


