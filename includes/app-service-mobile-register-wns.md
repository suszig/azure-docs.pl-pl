
1. Visual Studio ソリューション エクスプ ローラーで、Windows ストア アプリ プロジェクトを右クリックし、クリックして **ストア** > **アプリケーションを... ストアと関連付ける**します。 

    ![アプリを Windows ストアと関連付ける](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
    
2. ウィザードで、次のようにクリックします。 **次**, 、Microsoft アカウントでサインイン、にあるアプリの名前を入力 **新しいアプリ名の予約**, 、] をクリックし、 **予約**します。

3. アプリケーションの登録が正常に作成された後新しいアプリ名の選択をクリックして **次**, 、] をクリックし、 **関連付ける**します。 この操作により、必要な Windows ストア登録情報がアプリケーション マニフェストに追加されます。 

7. Windows Store アプリ用に以前に作成したものと同じ登録を使用して、Windows Phone Store アプリ プロジェクトに対してステップ 1 と 3 を繰り返します。  

7. 移動し、 [Windows デベロッパー センター](https://dev.windows.com/en-us/overview), 、Microsoft アカウントでサインインで新しいアプリケーションの登録] をクリックして **マイ アプリ**, の順に展開 **サービス** > **プッシュ通知**します。 

8.  **プッシュ通知** ] ページで [ **Live サービス サイト** [ **Microsoft Azure Mobile Services**します。

9.  **アプリ設定** ] タブの値をメモしておいてください **クライアント シークレット** と **パッケージ SID**します。 

    ![デベロッパー センターでのアプリの設定](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] クライアント シークレットとパッケージ SID は重要なセキュリティ資格情報です。 これらの値は、他のユーザーと共有したり、アプリケーションで配信したりしないでください。
