<properties 
    pageTitle="Xamarin Android での Mobile Apps の認証の使用" 
    description="Mobile Apps を使用して、AAD、Google、Facebook、Twitter、Microsoft などのさまざまな ID プロバイダーを通じて Xamarin Android アプリのユーザーを認証する方法について説明します。" 
    services="app-service\mobile" 
    documentationCenter="xamarin" 
    authors="mattchenderson" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-mobile" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-xamarin-android" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="12/07/2015" 
    ms.author="mahender"/>


# Xamarin.Android アプリに認証を追加する

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

このトピックでは、クライアント アプリケーションから Mobile App のユーザーを認証する方法について説明します。 このチュートリアルでは、Azure Mobile Apps でサポートされている ID プロバイダーを使用して、クイック スタート プロジェクトに認証を追加します。 Mobile App によって正常に認証され、承認されると、ユーザー ID 値が表示されます。

このチュートリアルは、モバイル アプリのクイック スタートに基づいています。 チュートリアル [Xamarin.Android アプリの作成] を完了することも、まず必要があります。 ダウンロードしたクイック スタートのサーバー プロジェクトを使用しない場合は、認証拡張機能パッケージをプロジェクトに追加する必要があります。 サーバーの拡張機能パッケージの詳細については、次を参照してください。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)します。

## <a name="register"></a>アプリケーションを認証に登録し、App Services を構成します。

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="permissions"></a>認証されたユーザーへのアクセス許可を制限します。

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Visual Studio または Xamarin Studio で、デバイスまたはエミュレーターを使用してクライアント プロジェクトを実行します。 アプリケーションの開始後に、状態コード 401 (許可されていません) のハンドルされない例外が発生することを確認します。 これは、認証されないユーザーとして Mobile App バックエンドにアプリがアクセスしようとするために生じます。 *TodoItem* テーブルは今すぐ認証が必要です。

次に、認証されたユーザーで Mobile App のバックエンドからリソースを要求するように、クライアント アプリを更新します。

## <a name="add-authentication"></a>アプリに認証を追加します。

ユーザーが **[サインイン]** ボタンをタップし認証されてからデータを表示する必要がある場合は、アプリを更新します。

1. **TodoActivity** クラスに次のコードを追加します。

     // Define a authenticated user.
     private MobileServiceUser user;
     private async Task<bool> Authenticate()
     {
             var success = false;
             try
             {
                 // Sign in with Facebook login using a server-managed flow.
                 user = await client.LoginAsync(this,
                     MobileServiceAuthenticationProvider.Facebook);
                 CreateAndShowDialog(string.Format("you are now logged in - {0}",
                     user.UserId), "Logged in!");
    
                 success = true;
             }
             catch (Exception ex)
             {
                 CreateAndShowDialog(ex, "Authentication failed");
             }
             return success;
     }
    
     [Java.Interop.Export()]
     public async void LoginUser(View view)
     {
         // Load data only after authentication succeeds.
         if (await Authenticate())
         {
             //Hide the button after authentication succeeds. 
             FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;
    
             // Load the data.
             OnRefreshItemsSelected();
         }
     }

 これにより、ユーザーを認証する新しいメソッドと、新しい **[サインイン]** ボタンのメソッド ハンドラーが作成されます。 上記のサンプル コードでは、ユーザーは Facebook ログインを使用して認証されます。 認証されると、ダイアログを使用してユーザー ID が表示されます。
 > [AZURE.NOTE] Facebook 以外の ID プロバイダーを使用している場合は、上記の例の **LoginAsync** に渡される値を _MicrosoftAccount_、_Twitter_、_Google_、_WindowsAzureActiveDirectory_ のいずれかに変更します。

3. **OnCreate** メソッド内の次のコード行を削除またはコメント アウトします。

        OnRefreshItemsSelected ();

4. Activity_To_Do.axml ファイル内の既存の *AddItem* ボタンの前に次の *LoginUser* ボタン定義を追加します。

        <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />

5. 次の要素を Strings.xml リソース ファイルに追加します。

        <string name="login_button_text">Sign in</string> 

6. Visual Studio または Xamarin Studio で、デバイスまたはエミュレーターでクライアント プロジェクトを実行し、選択した ID プロバイダーを使用してサインインします。

    ログインに成功すると、アプリケーションにログイン ID と todo 項目の一覧が表示され、データを更新することができます。




[create a xamarin.android app]: app-service-mobile-xamarin-android-get-started.md 

