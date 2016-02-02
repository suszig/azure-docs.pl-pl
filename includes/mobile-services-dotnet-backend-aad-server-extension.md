### (省略可能) Azure Active Directory 用に .NET Mobile Services を構成する

>[AZURE.NOTE] 以下の手順は、Azure Active Directory ログイン プロバイダーにのみ適用されるので、省略可能です。

1. インストール、 [WindowsAzure.MobileServices.Backend.Security NuGet パッケージ](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend.Security)します。

2. Visual Studio で、App_Start を展開し、WebApiConfig.cs ファイルを開きます。 次の追加 `を使用して` ステートメントを先頭にします。

        using Microsoft.WindowsAzure.Mobile.Service.Security.Providers;

3. 次のコードを追加、WebApiConfig.cs でも、 `登録` メソッドの直後に `オプション` がインスタンス化されます。

        options.LoginProviders.Remove(typeof(AzureActiveDirectoryLoginProvider));
        options.LoginProviders.Add(typeof(AzureActiveDirectoryExtendedLoginProvider));






