
1. MainPage.xaml.cs プロジェクト ファイルで次のコードを追加 **を使用して** ステートメント。

        using System.Linq;      
        using Windows.Security.Credentials;

2. 置き換える、 **AuthenticateAsync** メソッドを次のコード。

        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;

            // This sample uses the Facebook provider.
            var provider = "Facebook";

            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;

            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }

            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;

                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;

                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.

                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);

                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider,
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);

                    success = true;
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
            message = string.Format("You are now logged in - {0}", user.UserId);
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();

            return success;
        }

    このバージョンの **AuthenticateAsync**, に格納されている資格情報を使用しようとするアプリ、 **PasswordVault** サービスにアクセスします。 保存された資格情報がないときも通常のサインインが実行されます。

    >[AZURE.NOTE]キャッシュされたトークンは期限が切れて場合、およびアプリケーションを使用するとき、そのトークンの有効期限は認証も発生することができます。 トークンが期限切れを確認する方法については、次を参照してください。 [有効期限が切れた認証トークンをチェック](http://aka.ms/jww5vp)します。 トークンの期限切れに関連する認証エラーを処理するためのソリューションでは、投稿を参照してください。 [キャッシュと Azure Mobile Services での有効期限切れのトークンを処理するマネージ SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx)します。 

3. アプリケーションを 2 回再起動します。

    最初の再起動では、プロバイダーによるサインインが再度必要になります。 ただし、2 回目の再起動ではキャッシュされた資格情報が使用され、サインインは回避されます。 
