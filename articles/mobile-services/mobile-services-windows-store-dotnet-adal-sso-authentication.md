<properties
    pageTitle="Active Directory 認証ライブラリのシングル サインオンによるアプリケーションの認証 (Windows ストア) | Microsoft Azure"
    description="Windows ストア アプリケーションで ADAL を使用してシングル サインオンのユーザーを認証する方法について説明します。"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/18/2015"
    ms.author="wesmc"/>

# Active Directory 認証ライブラリのシングル サインオンによるアプリケーションの認証

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-adal-sso](../../includes/mobile-services-selector-adal-sso.md)]

##概要

このチュートリアルではサポートするために、Active Directory 認証ライブラリを使用して、クイック スタート プロジェクトに認証を追加する [クライアント主導型ログイン操作](http://msdn.microsoft.com/library/azure/jj710106.aspx) と Azure Active Directory です。 サポートするために [サービス主導型ログイン操作](http://msdn.microsoft.com/library/azure/dn283952.aspx) を Azure Active Directory に始めて、 [Mobile Services アプリへの認証の追加](../mobile-services-dotnet-backend-windows-store-dotnet-get-started-users.md) チュートリアルです。

ユーザーを認証できるようにするには、Azure Active Directory (AAD) にアプリケーションを登録する必要があります。 この処理は 2 段階の手順で実行されます。 まず、モバイル サービスを登録し、モバイル サービスに対するアクセス許可を公開する必要があります。 次に、Windows ストア アプリを登録してこれらのアクセス許可へのアクセス権を付与する必要があります。


>[AZURE.NOTE] このチュートリアルの目的のモバイル サービスを使用してシングル サインオン Azure Active Directory 認証を使用して Windows ストア アプリを実行する方法を理解するのに役立つ、 [クライアント主導型ログイン操作](http://msdn.microsoft.com/library/azure/jj710106.aspx)します。 チュートリアルの完了モバイル サービスの初めての場合は、[モバイル サービスの開始を取得] です。


##前提条件

このチュートリアルには、次のものが必要です。

* Windows 8.1 で実行されている Visual Studio 2013。
* 完了、[モバイル サービスの開始を取得] チュートリアルです。
* Microsoft Azure Mobile Services SDK の NuGet パッケージ
* Active Directory 認証ライブラリの NuGet パッケージ

[AZURE.INCLUDE [mobile-services-dotnet-adal-register-service](../../includes/mobile-services-dotnet-adal-register-service.md)]

##Azure Active Directory にアプリケーションを登録する

アプリケーションを Azure Active Directory に登録するには、アプリケーションを Windows ストアに関連付け、アプリケーションのパッケージ セキュリティ ID (SID) を取得する必要があります。 パッケージ SID は、Azure Active Directory のネイティブ アプリケーション設定に登録されています。


###アプリケーションと新しいストア アプリ名との関連付け

1. Visual Studio で、クライアント アプリケーション プロジェクトを右クリックし、クリックして **ストア** と **アプリケーションをストアと関連付ける**

    ![][1]

2. デベロッパー センター アカウントにサインインします。

3. アプリとクリックに予約するアプリ名の入力 **予約**します。

    ![][2]

4. 新しいアプリ名を選択し、クリックして **次**します。

5. クリックして **関連付ける** にアプリをストア名に関連付けます。


###アプリケーションのパッケージ SID の取得

ここで、ネイティブ アプリケーション設定で構成されるパッケージ SID を取得する必要があります。

1. [Windows デベロッパー センター ダッシュ ボード] にログインし、をクリックして **編集** 、アプリケーションにします。

    ![][3]

2. クリックし、 **サービス**

    ![][4]

3. クリックして **Live サービス サイト**します。

    ![][5]

4. ページの上部にあるパッケージ SID をコピーします。

    ![][6]

###ネイティブ アプリケーション登録の作成

1. 移動 **Active Directory** [Azure クラシック ポータル] のディレクトリをクリックします。

    ![][7]

2. クリックして、 **アプリケーション** 上部にあるタブをクリック **追加** アプリです。

    ![][8]

3. クリックして **[組織が開発中のアプリケーションを追加**します。

4. アプリケーションの追加ウィザードで入力、 **名** ] をクリックしてアプリケーションを  **ネイティブ クライアント アプリケーション** 型です。 その後、クリックして続行します。

    ![][9]

5.  **リダイレクト URI** ボックス、アプリケーション パッケージ SID をコピーしておいた貼り付けしてネイティブ アプリケーション登録を完了する] をクリックします。

    ![][10]

6. クリックして、 **構成** ネイティブ クライアント アプリケーションとコピーのタブ、 **クライアント ID**します。 この情報は後で必要になります。

    ![][11]

7. ページを下へスクロールして、 **他のアプリケーションに対するアクセス許可** セクションし、以前に登録したモバイル サービス アプリケーションへのフル アクセスを付与します。 クリックして **保存**

    ![][12]

これで、モバイル サービスが、アプリケーションからシングル サインオン ログインを受信するように AAD で構成されます。



##認証を要求するようにモバイル サービスを構成する

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)]

##クライアント アプリケーションに認証コードを追加する

1. Visual Studio で、Windows ストア クライアント アプリケーション プロジェクトを開きます。

[AZURE.INCLUDE [mobile-services-dotnet-adal-install-nuget](../../includes/mobile-services-dotnet-adal-install-nuget.md)]

4. Visual Studio のソリューション エクスプローラー ウィンドウで MainPage.xaml.cs を開き、次の using ステートメントを追加します。

        using Windows.UI.Popups;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Newtonsoft.Json.Linq;


5. 次のコードを `AuthenticateAsync` メソッドを宣言する MainPage クラスに追加します。

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "<INSERT-AUTHORITY-HERE>";
            string resourceURI = "<INSERT-RESOURCE-URI-HERE>";
            string clientID = "<INSERT-CLIENT-ID-HERE>";
            while (user == null)
            {
                string message;
                try
                {
                  AuthenticationContext ac = new AuthenticationContext(authority);
                  AuthenticationResult ar = await ac.AcquireTokenAsync(resourceURI, clientID, (Uri) null);
                  JObject payload = new JObject();
                  payload["access_token"] = ar.AccessToken;
                  user = await App.MobileService.LoginAsync(MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                  message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                  message = "You must log in. Login Required";
                }
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

6. コードで、 `AuthenticateAsync` 代わりに、上記のメソッドを **挿入機関ここでは** 、形式をアプリケーションをプロビジョニングしたテナントの名前、https://login.windows.net/tenant-name.onmicrosoft.com でなければなりません。 この値は、[Azure クラシック ポータル] の Azure Active Directory の [ドメイン] タブからコピーできます。

7. コードで、 `AuthenticateAsync` 代わりに、上記のメソッドを **挿入リソース URI ここ** で、 **App ID URI** 、モバイル サービスのです。 [Azure Active Directory に登録する方法] を実行している場合のトピック、アプリケーション ID URI が https://todolist.azure-mobile.net/login/aad と同様にあります。

8. コードで、 `AuthenticateAsync` 代わりに、上記のメソッドを **INSERT CLIENT ID ここ** をネイティブ クライアント アプリケーションからコピーしたクライアント ID。

9. Visual Studio のソリューション エクスプローラー ウィンドウで、クライアント プロジェクトの Package.appxmanifest ファイルを開きます。 クリックして、 **機能** ] タブで有効にするし、 **エンタープライズ アプリケーション** と **プライベート ネットワーク (クライアントとサーバー)**します。 ファイルを保存します。

    ![][14]

10. MainPage.cs ファイルで、`OnNavigatedTo` イベント ハンドラーを更新して `AuthenticateAsync` メソッドを次のように呼び出します。

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            await AuthenticateAsync();
            await RefreshTodoItems();
        }


##認証を使用してクライアントをテストする

1. Visual Studio で、クライアント アプリケーションを実行します。
2. Azure Active Directory にログインするための画面が表示されます。
3. アプリケーションが認証を実行し、Todo 項目を返します。

    ![][15]




<!-- Images -->
[0]: ./media/mobile-services-windows-store-dotnet-adal-sso-authenticate/mobile-services-aad-app-manage-manifest.png
[1]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-vs-associate-app.png
[2]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-vs-reserve-store-appname.png
[3]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-store-app-edit.png
[4]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-store-app-services.png
[5]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-live-services-site.png
[6]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-store-app-package-sid.png
[7]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-select-aad.png
[8]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-aad-applications-tab.png
[9]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-native-selection.png
[10]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-native-sid-redirect-uri.png
[11]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-native-client-id.png
[12]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-native-add-permissions.png
[14]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-package-appxmanifest.png
[15]: ./media/mobile-services-windows-store-dotnet-adal-sso-authentication/mobile-services-app-run.png

<!-- URLs. -->
[How to Register with the Azure Active Directory]: mobile-services-how-to-register-active-directory-authentication.md
[Azure classic portal]: https://manage.windowsazure.com/
[Get started with Mobile Services]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[Windows Dev Center Dashboard]: http://go.microsoft.com/fwlink/p/?LinkID=266734
