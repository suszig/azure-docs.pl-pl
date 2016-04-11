次の手順を作成する新しいカスタム [ApiController](http://go.microsoft.com/fwlink/p/?LinkId=512673) アプリケーションへのプッシュ通知を送信します。 これと同じコードを実装すること、 [TableController](http://msdn.microsoft.com/library/azure/dn643359.aspx) またはバックエンド サービスで他の場所。 

1. Visual Studio ソリューション エクスプ ローラーで、モバイル サービス プロジェクトの Controllers フォルダーを右クリックし、展開 **追加**, 、クリックして **新しいスキャフォールディング項目**します。

    [スキャフォールディングの追加] ダイアログ ボックスが表示されます。

2. 展開 **Azure Mobile Services** ] をクリック **Azure Mobile Services カスタム コント ローラー**, 、] をクリックし、 **追加**, 指定した後、 **コント ローラー名** の `NotifyAllUsersController`, 、] をクリックし、 **追加** もう一度です。

    ![Web API の [スキャフォールディングの追加] ダイアログ](./media/mobile-services-dotnet-backend-update-server-push-vs2013/add-custom-api-controller.png)

    という名前の新しい空のコント ローラー クラス作成 **NotifyAllUsersController**します。 

3. 新しい NotifyAllUsersController.cs プロジェクト ファイルで次のコードを追加 **を使用して** ステートメント。

        using Newtonsoft.Json.Linq;
        using System.Threading.Tasks;

4. コントローラーで POST メソッドを実装する次のコードを追加します。

        public async Task<bool> Post(JObject data)
        {
            try
            {
                // Define the XML paylod for a WNS native toast notification 
                // that contains the value supplied in the POST request.
                string wnsToast = 
                    string.Format("<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<toast><visual><binding template=\"ToastText01\">" + 
                    "<text id=\"1\">{0}</text></binding></visual></toast>", 
                    data.GetValue("toast").Value<string>());

                // Define the WNS native toast with the payload string.
                WindowsPushMessage message = new WindowsPushMessage();
                message.XmlPayload = wnsToast;

                // Send the toast notification.
                await Services.Push.SendAsync(message);
                return true;
            }
            catch (Exception e)
            {
                Services.Log.Error(e.ToString());
            }
            return false;
        }

    >[AZURE.NOTE]この POST メソッドは、安全ではないアプリケーション キーを持つすべてのクライアントから呼び出すことができます。 エンドポイントをセキュリティで保護するには、認証を求めるメソッドまたはクラスに属性 `[AuthorizeLevel(AuthorizationLevel.User)]` を適用します。 

