## Web API コードの追加

次の手順では、ハード コーディングされた連絡先リストを返す単純な HTTP Get メソッドのコードを追加します。 

1. ソリューション エクスプ ローラーで右クリックし、 **モデル** フォルダー **追加 > クラス**します。 

    ![](./media/app-service-api-define-api-app/03-add-new-class-v3.png) 

2. 新しいファイルの名前 *Contact.cs*します。 

    ![](./media/app-service-api-define-api-app/0301-add-new-class-dialog-v3.png) 

3. クリックして **追加**します。

4. 1 回、 *Contact.cs* ファイルが作成されたら、ファイルの内容全体を次のコードに置き換えます。 

        namespace ContactsList.Models
        {
            public class Contact
            {
                public int Id { get; set; }
                public string Name { get; set; }
                public string EmailAddress { get; set; }
            }
        }

5. 右クリックし、 **コント ローラー** フォルダー、および選択 **追加 > コント ローラー**します。 

    ![](./media/app-service-api-define-api-app/05-new-controller-v3.png)

6.  **スキャフォールディングの追加** ダイアログ ボックスで、選択、 **Web API 2 コント ローラー - 空** ] をクリックし、 **追加**します。 

    ![](./media/app-service-api-define-api-app/06-new-controller-dialog-v3.png)

7. コント ローラーに名前 **ContactsController**, 、] をクリック **追加**します。 

    ![](./media/app-service-api-define-api-app/07-new-controller-name-v2.png)

8. ContactsController.cs ファイルが作成されたら、ファイルの内容全体を次のコードに置き換えます。 

        using ContactsList.Models;
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Threading.Tasks;
        using System.Web.Http;
        
        namespace ContactsList.Controllers
        {
            public class ContactsController : ApiController
            {
                [HttpGet]
                public IEnumerable<Contact> Get()
                {
                    return new Contact[]{
                        new Contact { Id = 1, EmailAddress = "barney@contoso.com", Name = "Barney Poland"},
                        new Contact { Id = 2, EmailAddress = "lacy@contoso.com", Name = "Lacy Barrera"},
                        new Contact { Id = 3, EmailAddress = "lora@microsoft.com", Name = "Lora Riggs"}
                    };
                }
            }
        }

## Swagger UI の有効化

既定では、API アプリ プロジェクトは automatic モードで有効になっている [Swagger](http://swagger.io/ "公式の Swagger 情報") メタデータの生成と使用する場合に、 **[API アプリ SDK** 、API テスト ページの Web API プロジェクトを変換するメニュー項目が既定で有効にもします。  

ただし、Azure API アプリの新しいプロジェクト テンプレートを使った場合は、API テスト ページが無効になります。 API アプリ プロジェクト テンプレートを使用して API アプリ プロジェクトを作成した場合は、次の手順を実行してテスト ページを有効にします。

**注:** と API アプリを展開する場合 *パブリック匿名* と、Swagger ui が有効なにはすべてのユーザーを検出して、Api を呼び出して、Swagger UI を使用することができます。 

1. 開いている、 *App_Start/SwaggerConfig.cs* ファイルを開き、 **EnableSwaggerUI**:

    ![](./media/app-service-api-define-api-app/12-enable-swagger-ui-with-box.png)

2. 次のコード行をコメント解除します。

            })
        .EnableSwaggerUi(c =>
            {

3. 完了すると、ファイルの内容は次のようになります。

    ![](./media/app-service-api-define-api-app/13-enable-swagger-ui-with-box.png)

## Web API のテスト

API のテスト ページを表示するには、次の手順に従います。

1. アプリをローカルで実行します (Ctrl + F5 キー)。

    ブラウザーが開き、HTTP 403 エラーが表示されます。これは、ベース URL がこのプロジェクトに有効な Web ページの URL でも API メソッドの URL でもないためです。
 
3.  ベース URL の末尾に `/swagger` を追加して Swagger ページに移動します。 

    ![](./media/app-service-api-define-api-app/swaggerhome.png)

2. をクリックして **連絡先 > 取得 > 試してみる**, 、API が機能しているおり、期待される結果を返すことを確認します。 

    ![](./media/app-service-api-define-api-app/swaggertry.png)

3. Visual Studio で [ **デバッグ > デバッグの停止]**します。

