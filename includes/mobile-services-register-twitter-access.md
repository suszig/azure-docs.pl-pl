

新しい Twitter v1.1 API では、リソースにアクセスする前にアプリケーションの認証が求められます。 まず、OAuth 2.0 を使用して、アクセスの要求に必要な資格情報を取得する必要があります。 次に、モバイル サービス用のアプリケーション設定にその資格情報を安全に保存します。

1. まだ行っていない場合、トピックの手順を完了します。 <a href="../articles/mobile-services/mobile-services-how-to-register-twitter-authentication.md/" target="_blank">Mobile Services での Twitter ログイン用のアプリケーションの登録</a>. 
  
    Twitter v1.1 API にアクセスできるようにするために必要な資格情報は、Twitter が生成します。 この資格情報は Twitter デベロッパーの Web サイトから取得できます。 

2. 次に、[ <a href="http://go.microsoft.com/fwlink/p/?LinkId=268300" target="_blank">Twitter デベロッパー</a> web サイト、Twitter アカウント資格情報でサインインし、Twitter アプリケーションを選択します。

3.  **Keys and Access Tokens** アプリのタブで、次の値を書き留めます。

    + **コンシューマー キー**
    + **コンシューマー シークレット**
    + **アクセス トークン**
    + **アクセス トークン シークレット**

4. ログオン、 [Azure クラシック ポータル](https://manage.windowsazure.com/), をクリックして **Mobile Services**, 、モバイル サービスをクリックします。

5. をクリックして、 **Identity** タブで、入力、 **コンシューマー キー** と **コンシューマー シークレット** をクリックして、Twitter から取得した値 **保存**します。 

    ![](./media/mobile-services-register-twitter-access/mobile-identity-tab-twitter-only.png)

2. をクリックして、 **構成** ] タブで、下へスクロールして **アプリ設定** を入力し、 **名前** と **値** 、Twitter サイトから取得した次のそれぞれの [ **保存**します。

    + `TWITTER_ACCESS_TOKEN`
    + `TWITTER_ACCESS_TOKEN_SECRET`

    ![](./media/mobile-services-register-twitter-access/mobile-schedule-job-app-settings.png)

    これにより、アプリケーション設定に Twitter アクセス トークンが保存されます。 コンシューマー資格情報と同様に、 **Identity** ] タブがアクセス資格情報は、暗号化されてアプリ設定にも保存され、スクリプト ファイルでハードコーディングすることがなくサーバー スクリプトでアクセスできます。 詳細については、[アプリ設定] を参照してください。

<!-- URLs. -->
[Mobile Services server script reference]: http://go.microsoft.com/fwlink/?LinkId=262293
[Register your apps for Twitter login with Mobile Services]: ../articles/mobile-services/mobile-services-how-to-register-twitter-authentication.md
[Twitter Developers]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[App settings]: http://msdn.microsoft.com/library/azure/b6bb7d2d-35ae-47eb-a03f-6ee393e170f7
