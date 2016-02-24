### (省略可能) Azure Active Directory 用に .NET Mobile Services を構成する

>[AZURE.NOTE] Azure Active Directory ログイン プロバイダーにのみ適用されるので、次の手順は省略できます。

1. インストール、 [WindowsAzure.MobileServices.Backend.Security NuGet パッケージ](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend.Security)します。

2. Visual Studio で、App_Start を展開し、WebApiConfig.cs ファイルを開きます。 次の `using` ステートメントを先頭に追加します。

        using Microsoft.WindowsAzure.Mobile.Service.Security.Providers;

3. また、WebApiConfig.cs で、`options` がインスタンス化された直後に、次のコードを `Register` メソッドに追加します。

        options.LoginProviders.Remove(typeof(AzureActiveDirectoryLoginProvider));
        options.LoginProviders.Add(typeof(AzureActiveDirectoryExtendedLoginProvider));

