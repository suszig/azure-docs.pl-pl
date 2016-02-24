

1. アプリを登録済みでない場合は、移動、[アプリ送信のページ] に Windows ストア アプリのデベロッパー センターでは、Microsoft アカウントでログオンし、をクリックして **アプリ名**します。

    ![](./media/mobile-services-notification-hubs-register-windows-store-app/mobile-services-submit-win8-app.png)

2. アプリケーションの名前を入力 **アプリ名**, 、] をクリックして **アプリ名の予約**, 、] をクリックし、 **保存**します。

    ![](./media/mobile-services-notification-hubs-register-windows-store-app/mobile-services-win8-app-name.png)

    これでアプリケーションの新しい Windows ストア登録が作成されます。

3. Visual Studio で、チュートリアルを完了したときに作成したプロジェクトを開く **モバイル サービスを使ってみる**します。

4. ソリューション エクスプ ローラーでプロジェクトを右クリックし、をクリックして **ストア**, 、クリックして **アプリケーションを... ストアと関連付ける**します。 

    ![](./media/mobile-services-notification-hubs-register-windows-store-app/mobile-services-store-association.png)

    これを表示、 **アプリケーションを Windows ストアと関連付ける** ウィザード。

5. ウィザードで、次のようにクリックします。 **サインイン** し、Microsoft アカウントでログインします。

6. 手順 2. で登録したアプリを選択して [ **次**, 、順にクリック **関連付ける**します。

    ![](./media/mobile-services-notification-hubs-register-windows-store-app/mobile-services-select-app-name.png)

    この操作により、必要な Windows ストア登録情報がアプリケーション マニフェストに追加されます。    

7. (省略可能) 手順 4 ～ 6 を繰り返して、ユニバーサル Windows アプリの Windows Phone ストア プロジェクトも登録します。

8. 新しいアプリケーションの Windows デベロッパー センター ページに戻って **サービス**します。 

    ![](./media/mobile-services-notification-hubs-register-windows-store-app/mobile-services-win8-edit-app.png) 

9. [サービス] ページをクリックして **Live サービス サイト** [ **Azure Mobile Services**します。

    ![](./media/mobile-services-javascript-backend-register-windows-store-app/mobile-services-win8-edit2-app.png)

10. クリックして **サービスの認証** の値をメモしておきます **クライアント シークレット** と **パッケージ セキュリティ id (SID)**します。 

    ![](./media/mobile-services-notification-hubs-register-windows-store-app/mobile-services-win8-app-push-auth.png)

    > [AZURE.NOTE] クライアント シークレットとパッケージ SID は重要なセキュリティ資格情報です。 これらの機密情報は、他のユーザーと共有したり、アプリケーションで配信したりしないでください。

11. ログオン、 [Azure クラシック ポータル](https://manage.windowsazure.com/), 、] をクリックして **Mobile Services**, 、アプリケーションをクリックします。

    ![](./media/mobile-services-notification-hubs-register-windows-store-app/mobile-services-selection.png)

12. をクリックして、 **プッシュ** タブし、入力、 **クライアント シークレット** と **パッケージ SID** 、WNS から取得した値が順にクリックして **保存**です。

    >[AZURE.NOTE]下部にあるリンクを参照してくださいが古いモバイル サービスを使用してこのチュートリアルを完了すると、 **プッシュ** というタブ **[拡張プッシュの**です。 このリンクをクリックすると、モバイル サービスが更新され、Notification Hubs と統合されます。 この変更を元に戻すことはできません。 運用環境のモバイル サービスで拡張プッシュ通知を有効にする方法の詳細については、<a href="http://go.microsoft.com/fwlink/p/?LinkId=391951">このガイダンス</a>を参照してください。 

    ![](./media/mobile-services-notification-hubs-register-windows-store-app/mobile-push-tab.png)

    >[AZURE.NOTE]WNS の資格情報を設定すると拡張プッシュ通知に、 **プッシュ** タブ ポータルで、共有されている通知ハブへのアプリに対して通知ハブを構成します。

<!-- URLs. -->
[Get started with Mobile Services]: ../articles/mobile-services-windows-store-get-started.md
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582

