## <a name="obtain-an-azure-resource-manager-token"></a>Uzyskaj token usługi Azure Resource Manager
Usługa Azure Active Directory musi uwierzytelniać wszystkie zadania, które należy wykonać na zasobów przy użyciu usługi Azure Resource Manager. Tu przykładzie jest używane uwierzytelnianie hasła, aby inne podejścia, zobacz [żądań uwierzytelniania usługi Azure Resource Manager][lnk-authenticate-arm].

1. Dodaj następujący kod do **Main** metody w pliku Program.cs do pobrania tokenu z usługi Azure AD przy użyciu identyfikatora aplikacji i hasła.
   
    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.microsoftonline.com/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
   
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```
2. Utwórz **element ResourceManagementClient** obiekt, który używa tokenu, dodając następujący kod na końcu **Main** metody:
   
    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. Utwórz lub uzyskać odwołania do grupy zasobów, którego używasz:
   
    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx