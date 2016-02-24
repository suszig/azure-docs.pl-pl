

既定では、モバイル サービスのリソースに対するすべての要求を実行するには、クライアントがアプリケーション キーを提出する必要があるという制限が加えられていますが、この方法ではリソースに対するアクセスが厳格に保護されるわけではありません。 リソースを保護するには、認証されたクライアントのみにアクセスを制限する必要があります。

1. Visual Studio で、モバイル サービス プロジェクトを開き、Controllers フォルダーを展開、および開く **TodoItemController.cs**します。  **TodoItemController** クラス TodoItem テーブルに対するデータ アクセスを実装します。 次の `using` ステートメントを追加します。

        using Microsoft.WindowsAzure.Mobile.Service.Security;

2. 次の適用 _AuthorizeLevel_ 属性を **TodoItemController** クラスです。 

        [AuthorizeLevel(AuthorizationLevel.User)]

    こうことを確認するに対するすべての操作、 _TodoItem_ テーブルには、認証されたユーザーが必要とします。 適用することも、 *AuthorizeLevel* メソッド レベルの属性です。

3. (省略可能)ローカルで認証をデバッグする場合は、展開、 `App_Start` フォルダーを開き、 **WebApiConfig.cs**, 、次のコードを追加し、 **登録** メソッドです。  

        config.SetIsHosted(true);

    こうことに従うことを含め、Azure でホストされているように実行するローカル モバイル サービス プロジェクト、 *AuthorizeLevel* 設定します。 関係なく認証なしこの設定がない localhost に対するすべての HTTP 要求が許可されている、 *AuthorizeLevel* 設定します。 セルフホスト モードを有効にした場合は、ローカル アプリケーション キーの値を設定することも必要です。

4. (省略可能) web.config プロジェクト ファイルで、`MS_ApplicationKey` アプリケーション設定に文字列値を設定します。 

    これは、サービスをローカルで実行する場合に API ヘルプ ページをテストするために使用するパスワードです (ユーザー名はなし)。  この文字列値は、Azure のライブ サイトによって使用されないので、実際のアプリケーション キーを使用する必要はありません。任意の有効な文字列値で動作します。
 
4. プロジェクトを再発行します。

