<properties 
    pageTitle="HTML/JavaScript アプリへの認証の追加 | Microsoft Azure" 
    description="Mobile Services を使用して、Google、Facebook、Twitter、Microsoft アカウントなどのさまざまな ID プロバイダーを通じて HTML アプリのユーザーを認証する方法について説明します。" 
    services="mobile-services" 
    documentationCenter="" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="javascript" 
    ms.topic="article" 
    ms.date="11/30/2015" 
    ms.author="glenga"/>


# Mobile Services アプリへの認証の追加

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

このトピックでは、PhoneGap または Cordova アプリを含む HTML アプリから Azure Mobile Services のユーザーを認証する方法について説明します。 このチュートリアルでは、モバイル サービスでサポートされている ID プロバイダーを使用して、クイック スタート プロジェクトに認証を追加します。 モバイル サービスによって正常に認証および承認されると、ユーザー ID 値が表示されます。

このチュートリアルは、Mobile Services のクイック スタートに基づいています。 先にチュートリアル「Mobile Services の使用」を完了している必要があります。

## <a name="register"></a>アプリケーションを認証に登録し、モバイル サービスを構成します。

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

## <a name="permissions"></a>認証されたユーザーへのアクセス許可を制限します。

[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)]


3. app ディレクトリで、**server** サブフォルダーから次のいずれかのコマンド ファイルを実行します。

    + **launch-windows** (Windows コンピューター)
    + **launch-mac.command** (Mac OS X コンピューター)
    + **launch-linux.sh** (Linux コンピューター)
    >[AZURE.NOTE]Windows コンピューターで次のように入力します。 `R` PowerShell が要求すると、スクリプトを実行することを確認します。 Web ブラウザーでは、インターネットからダウンロードしたスクリプトであるため、実行しないよう警告されることがあります。 その場合は、ブラウザーがスクリプトの読み込みを開始するよう要求する必要があります。

    これにより、新しいアプリケーションをホストする Web サーバーがローカル コンピューター上で起動します。

2. URL を開く <a href="http://localhost:8000/" target="_blank">http://localhost:8000/</a> で web ブラウザーが、アプリケーションを開始します。

    データの読み込みに失敗します。 この問題は、認証されないユーザーとしてアプリケーションがモバイル サービスにアクセスしようとしても、_TodoItem_ テーブルでは認証が要求されるために発生します。

3. (省略可能) Web ブラウザーのスクリプト デバッガーを開き、ページを再読み込みします。 アクセス拒否エラーが発生することを確認します。

次に、モバイル サービスのリソースを要求する前に認証を許可するようにアプリケーションを更新します。

## <a name="add-authentication"></a>アプリに認証を追加します。

>[AZURE.NOTE]ログインはポップアップで行われるため、**login** メソッドはボタンのクリック イベントから呼び出す必要があります。 そうしないと、多くのブラウザーではログイン ウィンドウが表示されません。

1. プロジェクト ファイル index.html を開き、H1 要素を探して、その下に次のコード スニペットを追加します。

        <div id="logged-in">
            You are logged in as <span id="login-name"></span>.
            <button id="log-out">Log out</button>
        </div>
        <div id="logged-out">
            You are not logged in.
            <button>Log in</button>
        </div>

    これで、ページからモバイル サービスにログインできるようになります。

2. page.js ファイルの末尾で refreshTodoItems 関数を呼び出しているコード行を探し、次のコードに置き換えます。

     function refreshAuthDisplay() {
         var isLoggedIn = client.currentUser !== null;
         $("#logged-in").toggle(isLoggedIn);
         $("#logged-out").toggle(!isLoggedIn);
    
         if (isLoggedIn) {
             $("#login-name").text(client.currentUser.userId);
             refreshTodoItems();
         }
     }
    
     function logIn() {
         client.login("facebook").then(refreshAuthDisplay, function(error){
             alert(error);
         });
     }
    
     function logOut() {
         client.logout();
         refreshAuthDisplay();
         $('#summary').html('<strong>You must login to access data.</strong>');
     }
    
     // On page init, fetch the data and set up event handlers
     $(function () {
         refreshAuthDisplay();
         $('#summary').html('<strong>You must login to access data.</strong>');          
         $("#logged-out button").click(logIn);
         $("#logged-in button").click(logOut);
     });

 これで、認証プロセスを処理する関数のセットが作成されます。 ユーザーは、Facebook ログインを使用して認証されます。 Facebook 以外の ID プロバイダーを使用している場合は、上の **login** メソッドに渡す値を、*microsoftaccount*、*facebook*、*twitter*、*google*、*aad* のいずれかに変更します。
 >[AZURE.IMPORTANT]PhoneGap アプリでは、次のプラグインもプロジェクトに追加する必要があります。
 ><ul><li><code>phonegap プラグインの追加 https://git-wip-us.apache.org/repos/asf/cordova-plugin-device.git</code></li>
 ><li><code>phonegap プラグインの追加 https://git-wip-us.apache.org/repos/asf/cordova-plugin-inappbrowser.git</code></li></ul>

9. アプリケーションが実行されているブラウザーに戻り、ページを更新します。

       When you are successfully logged-in, the app should run without errors, and you should be able to query Mobile Services and make updates to data.

    >[AZURE.NOTE]Internet Explorer を使用するときにログインした後にエラーが発生した可能性があります: <code>ウィンドウの呼び出し元に到達できません。別の Internet Explorer ゾーンにある可能性があります</code>します。これは、ポップアップが localhost (イントラネット) とは異なるセキュリティ ゾーン (インターネット) で実行されているためです。このことがアプリケーションに影響するのは、localhost を使用する開発時だけです。回避策として、**[インターネット オプション]** の **[セキュリティ]** タブを開き、**[ローカル イントラネット]**、**[サイト]** の順にクリックして、**[イントラネットのネットワークを自動的に検出する]** をオフにします。テストが終了したら、この設定を必ず元に戻します。

## <a name="next-steps"> </a>次のステップ

次は、チュートリアルでは、[ユーザーの認証のスクリプト] で、ユーザー ID 値になりますが、認証されたユーザーに基づいてモバイル サービスによって提供され、モバイル サービスから返されたデータをフィルター処理に使用します。 [Mobile Services Html/javascript How-to Conceptual reference リファレンス] で、Html/javascript でモバイル サービスを使用する方法の詳細についてください。






[register your app for authentication and configure mobile services]: #register 
[restrict table permissions to authenticated users]: #permissions 
[add authentication to the app]: #add-authentication 
[next steps]: #next-steps 
[4]: ./media/mobile-services-html-get-started-users/mobile-services-selection.png 
[5]: ./media/mobile-services-html-get-started-users/mobile-service-uri.png 
[13]: ./media/mobile-services-html-get-started-users/mobile-identity-tab.png 
[14]: ./media/mobile-services-html-get-started-users/mobile-portal-data-tables.png 
[15]: ./media/mobile-services-html-get-started-users/mobile-portal-change-table-perms.png 
[get started with mobile services]: mobile-services-html-get-started.md 
[authorize users with scripts]: mobile-services-javascript-backend-service-side-authorization.md 
[mobile services html/javascript how-to conceptual reference]: mobile-services-html-how-to-use-client-library.md 

