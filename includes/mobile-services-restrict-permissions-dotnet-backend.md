
既定では、モバイル サービスのリソースに対するすべての要求を実行するには、クライアントがアプリケーション キーを提出する必要があるという制限が加えられていますが、この方法ではリソースに対するアクセスが厳格に保護されるわけではありません。 リソースを保護するには、認証されたクライアントのみにアクセスを制限する必要があります。

1. Visual Studio で、Mobile Services プロジェクトを開き、Controllers フォルダーを展開して、**TodoItemController.cs** を開きます。 **TodoItemController** クラスは、TodoItem テーブルに対するデータ アクセスを実装します。 次の `using` ステートメントを追加します。

        using Microsoft.WindowsAzure.Mobile.Service.Security;

2. 次の _AuthorizeLevel_ 属性を **TodoItemController** クラスに適用します。

        [AuthorizeLevel(AuthorizationLevel.User)]

    これにより、_TodoItem_ テーブルに対するすべての操作には、認証されたユーザーが必要になります。 また *AuthorizeLevel* 属性をメソッド レベルで適用することもできます。

3. (省略可能)ローカルで認証をデバッグする場合は、展開、 `App_Start` フォルダーを開き、 **WebApiConfig.cs**, 、次のコードを追加し、 **登録** メソッドです。

        config.SetIsHosted(true);

    この作業により、ローカルのモバイル サービス プロジェクトに対して、*AuthorizeLevel* 設定に従うことも含め、まるで Azure でホストされているかのように動作することを指示します。 この設定がない場合は、*AuthorizeLevel* の設定にかかわりなく、localhost に対するすべての HTTP 要求は認証なしで許可されます。 セルフホスト モードを有効にした場合は、ローカル アプリケーション キーの値を設定することも必要です。

4. (省略可能)Web.config プロジェクト ファイルでの文字列値を設定、 `MS_ApplicationKey` アプリ設定です。

    これは、サービスをローカルで実行する場合に API ヘルプ ページをテストするために使用するパスワードです (ユーザー名はなし)。 この文字列値は、Azure のライブ サイトによって使用されないので、実際のアプリケーション キーを使用する必要はありません。任意の有効な文字列値で動作します。

4. プロジェクトを再発行します。





