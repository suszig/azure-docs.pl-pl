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

このトピックでは、PhoneGap または Cordova アプリを含む HTML アプリから Azure Mobile Services のユーザーを認証する方法について説明します。  このチュートリアルでは、モバイル サービスでサポートされている ID プロバイダーを使用して、クイック スタート プロジェクトに認証を追加します。 モバイル サービスによって正常に認証および承認されると、ユーザー ID 値が表示されます。  

このチュートリアルは、モバイル サービスのクイック スタートに基づいています。 また、最初のチュートリアルを完了する必要があります [Get started with Mobile Services]します。 

##<a name="register"></a>アプリケーションを認証に登録し、Mobile Services を構成する

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)] 

##<a name="permissions"></a>アクセス許可を、認証されたユーザーだけに制限する

[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)] 


3. App ディレクトリから次のコマンド ファイルのいずれかを起動、 **server** サブフォルダーです。

    + **windows の起動時** (Windows コンピューター) 
    + **launch-mac.command** (Mac OS X コンピューター)
    + **launch-linux.sh** (Linux コンピューター)

    >[AZURE.NOTE]Windows コンピューターで次のように入力します。 `R` PowerShell が要求すると、スクリプトを実行することを確認します。 Web ブラウザーでは、インターネットからダウンロードしたスクリプトであるため、実行しないよう警告されることがあります。 その場合は、ブラウザーがスクリプトの読み込みを開始するよう要求する必要があります。

    これにより、新しいアプリケーションをホストする Web サーバーがローカル コンピューター上で起動します。

2. Web ブラウザーで URL <a href="http://localhost:8000/" target="_blank">http://localhost:8000/</a> を開いて、アプリケーションを開始します。 

    データの読み込みに失敗します。 これは、問題は、アプリケーションは、認証されないユーザーとしてモバイル サービスにアクセスしようとしていますが、 _TodoItem_ テーブルには、今すぐ認証が必要です。

3. (省略可能) Web ブラウザーのスクリプト デバッガーを開き、ページを再読み込みします。 アクセス拒否エラーが発生することを確認します。 

次に、モバイル サービスのリソースを要求する前に認証を許可するようにアプリケーションを更新します。

##<a name="add-authentication"></a>アプリケーションに認証を追加する

>[AZURE.NOTE]ログインはポップアップで行われる、ためにを呼び出すように、 **ログイン** メソッドはボタンのクリックしてからイベントです。 そうしないと、多くのブラウザーではログイン ウィンドウが表示されません。

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

    これで、認証プロセスを処理する関数のセットが作成されます。 ユーザーは、Facebook ログインを使用して認証されます。 Facebook 以外の ID プロバイダーを使用している場合は、上の **login** メソッドに渡す値を *microsoftaccount*、*facebook*、*twitter*、*google*、*aad* のいずれかに変更します。

    >[AZURE.IMPORTANT]PhoneGap アプリで、次のプラグインをプロジェクトにも追加する必要があります。
    ><ul><li><code>phonegap plugin add https://git-wip-us.apache.org/repos/asf/cordova-plugin-device.git</code></li>
    ><li><code>phonegap plugin add https://git-wip-us.apache.org/repos/asf/cordova-plugin-inappbrowser.git</code></li></ul>

9. アプリケーションが実行されているブラウザーに戻り、ページを更新します。 

       When you are successfully logged-in, the app should run without errors, and you should be able to query Mobile Services and make updates to data.

    >[AZURE.NOTE]Internet Explorer を使用するときにログインした後にエラーが発生した可能性があります: <code>Cannot reach window opener. It may be on a different Internet Explorer zone</code>します。 これは、ポップアップが localhost (イントラネット) とは異なるセキュリティ ゾーン (インターネット) で実行されているためです。 このことがアプリケーションに影響するのは、localhost を使用する開発時だけです。 回避策として、**[インターネット オプション]** の **[セキュリティ]** タブを開き、**[ローカル イントラネット]**、**[サイト]** の順にクリックして、**[イントラネットのネットワークを自動的に検出する]** をオフにします。 テストが終了したら、この設定を必ず元に戻します。

## <a name="next-steps"> </a>次のステップ

次のチュートリアルでは、 [Authorize users with scripts], 、認証されたユーザーに基づいてモバイル サービスによって提供されるユーザー ID 値を受け取り、モバイル サービスから返されたデータをフィルター処理に使用します。 Html/javascript で Mobile Services を使用する方法の詳細についてください。 [Mobile Services HTML/JavaScript How-to Conceptual Reference]

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Next Steps]:#next-steps

<!-- Images. -->

[4]: ./media/mobile-services-html-get-started-users/mobile-services-selection.png
[5]: ./media/mobile-services-html-get-started-users/mobile-service-uri.png
[13]: ./media/mobile-services-html-get-started-users/mobile-identity-tab.png
[14]: ./media/mobile-services-html-get-started-users/mobile-portal-data-tables.png
[15]: ./media/mobile-services-html-get-started-users/mobile-portal-change-table-perms.png

<!-- URLs. -->
[Get started with Mobile Services]: mobile-services-html-get-started.md
[Authorize users with scripts]: mobile-services-javascript-backend-service-side-authorization.md
[Mobile Services HTML/JavaScript How-to Conceptual Reference]: mobile-services-html-how-to-use-client-library.md
 

