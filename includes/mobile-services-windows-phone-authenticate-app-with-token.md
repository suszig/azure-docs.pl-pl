
前の例では、標準のサインインを示しました。標準のサインインでは、アプリケーションが開始するたびに、クライアントは ID プロバイダーとモバイル サービスの両方にアクセスする必要があります。 この方法は非効率であるだけでなく、多くの顧客が同時にアプリケーションを開始すると、使用率に関連した問題が発生する場合があります。 よって、Mobile Services から返される承認トークンをキャッシュし、最初にその承認トークンの使用を試してから、プロバイダー ベースのサインインを使用するほうが効果的です。 

>[AZURE.NOTE]クライアントによって管理される、またはサービスによって管理される認証を使用しているかどうかに関係なく、モバイル サービスによって発行されたトークンをキャッシュすることができます。 このチュートリアルでは、サービスによって管理される認証を使用します。

1. MainPage.xaml.cs プロジェクト ファイルで次のコードを追加 **を使用して** ステートメント。

        using System.IO.IsolatedStorage;
        using System.Security.Cryptography;     

2. 置き換える、 **AuthenticateAsync** メソッドを次のコード。

        private async System.Threading.Tasks.Task AuthenticateAsync()
        {
            string message;
            // This sample uses the Facebook provider.
            var provider = "Facebook";

            // Provide some additional app-specific security for the encryption.
            byte [] entropy = { 1, 8, 3, 6, 5 };

            // Authorization credential.
            MobileServiceUser user = null;

            // Isolated storage for the app.
            IsolatedStorageSettings settings =
                IsolatedStorageSettings.ApplicationSettings;

            while (user == null)
            {
                // Try to get an existing encrypted credential from isolated storage.                    
                if (settings.Contains(provider))
                {
                    // Get the encrypted byte array, decrypt and deserialize the user.
                    var encryptedUser = settings[provider] as byte[];
                    var userBytes = ProtectedData.Unprotect(encryptedUser, entropy);
                    user = JsonConvert.DeserializeObject<MobileServiceUser>(
                        System.Text.Encoding.Unicode.GetString(userBytes, 0, userBytes.Length));
                }
                if (user != null)
                {
                    // Set the user from the stored credentials.
                    App.MobileService.CurrentUser = user;

                    try
                    {
                        // Try to return an item now to determine if the cached credential has expired.
                        await App.MobileService.GetTable<TodoItem>().Take(1).ToListAsync();
                    }
                    catch (MobileServiceInvalidOperationException ex)
                    {
                        if (ex.Response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
                        {
                            // Remove the credential with the expired token.
                            settings.Remove(provider);
                            user = null;
                            continue;
                        }
                    }
                }
                else
                {
                    try
                    {
                        // Login with the identity provider.
                        user = await App.MobileService
                            .LoginAsync(provider);

                        // Serialize the user into an array of bytes and encrypt with DPAPI.
                        var userBytes = System.Text.Encoding.Unicode
                            .GetBytes(JsonConvert.SerializeObject(user));
                        byte[] encryptedUser = ProtectedData.Protect(userBytes, entropy);

                        // Store the encrypted user credentials in local settings.
                        settings.Add(provider, encryptedUser);
                        settings.Save();
                    }
                    catch (MobileServiceInvalidOperationException ex)
                    {
                        message = "You must log in. Login Required";
                    }
                }
                message = string.Format("You are now logged in - {0}", user.UserId);
                MessageBox.Show(message);
            }
        }

    このバージョンの **AuthenticateAsync**, 、資格情報を使用するアプリケーションの試行がモバイル サービスにアクセスするでの暗号化のローカル ストレージに保存します。 保存されたトークンの有効期限が切れていないことを確認するための単純なクエリが送信されます。 401 が返されると、通常のプロバイダー ベースのサインインが試行されます。 保存された資格情報がないときも通常のサインインが実行されます。    

    >[AZURE.NOTE]このアプリは、ログイン時に期限切れのトークンのテストが、アプリケーションを使用するときに、次のトークンの有効期限が認証後に発生する可能性が。 トークンの期限切れに関連する認証エラーを処理するためのソリューションでは、投稿を参照してください。 [キャッシュと Azure Mobile Services での有効期限切れのトークンを処理するマネージ SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx)します。 
    
3. アプリケーションを 2 回再起動します。

    最初の再起動では、プロバイダーによるサインインが再度必要になります。 ただし、2 回目の再起動ではキャッシュされた資格情報が使用され、サインインは回避されます。 
