## リソース マネージャー トークンの取得

Azure Active Directory では、Azure リソース マネージャーを使用してリソース上で実行するすべてのタスクを認証する必要があります。 その他の方法を参照してください、ここに示す例がパスワードの認証を使用して [Azure Resource Manager 要求][lnk-authenticate-arm]します。

1. 次のコードを追加、 **Main** アプリケーション id とパスワードを使用して Azure AD からトークンを取得する Program.cs 内のメソッドです。

    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.windows.net/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
    
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```

2. 作成、 **ResourceManagementClient** の末尾に次のコードを追加することで、トークンを使用するオブジェクト、 **Main** メソッド。

    ```
    var creds = new TokenCloudCredentials(subscriptionId, token.AccessToken);
    var client = new ResourceManagementClient(creds);
    ```

3. 使用するリソース グループを作成するか、その参照を取得します。

    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdateAsync(rgName,
        new ResourceGroup("East US")).Result;
    if (rgResponse.StatusCode != HttpStatusCode.Created
        && rgResponse.StatusCode != HttpStatusCode.OK)
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
