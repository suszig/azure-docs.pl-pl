
1. アプリを登録済みでない場合に移動、 [Submit an app page] で Windows ストア アプリのデベロッパー センターには、Microsoft アカウントでログオンし、順にクリックして **アプリ名**します。

    ![](./media/mobile-services-register-windows-store-app/mobile-services-submit-win8-app.png)

2. 選択 **一意の名前を予約して新しいアプリを作成** ] をクリック **続行**, にあるアプリの名前を入力し、 **アプリ名**, 、] をクリックして **アプリ名の予約**, 、順にクリック **保存**します。

    ![](./media/mobile-services-register-windows-store-app/mobile-services-win8-app-name.png)

    これでアプリケーションの新しい Windows ストア登録が作成されます。

3. Visual Studio で、チュートリアルを完了したときに作成したプロジェクトを開く [Get started with Mobile Services]します。

4. ソリューション エクスプ ローラーで、Windows ストア アプリ プロジェクトを右クリックし、をクリックして **ストア**, 、クリックして **アプリケーションを... ストアと関連付ける**します。 

    ![](./media/mobile-services-register-windows-store-app/mobile-services-store-association.png)

    これを表示、 **アプリケーションを Windows ストアと関連付ける** ウィザード。

5. ウィザードで、次のようにクリックします。 **サインイン** 手順 2. で登録したアプリケーションをクリックし、Microsoft アカウントでログイン] をクリックし **次**, 、順にクリック **関連付ける**します。

    この操作により、必要な Windows ストア登録情報がアプリケーション マニフェストに追加されます。   

6. (省略可能) ユニバーサル Windows アプリケーションの場合、Windows Phone ストア プロジェクトの手順 4 および 5 を繰り返します。 

6. 新しいアプリケーションの Windows デベロッパー センター ページに戻って **サービス**します。 

    ![](./media/mobile-services-register-windows-store-app/mobile-services-win8-edit-app.png) 

7. [サービス] ページをクリックして **Live サービス サイト** [ **Azure Mobile Services**します。

    ![](./media/mobile-services-register-windows-store-app/mobile-services-win8-edit2-app.png) 

8. をクリックして **API 設定**, 、選択を有効にする **モバイルやデスクトップ クライアント アプリ**, 、としてモバイル サービス URL を指定、 **ターゲット ドメイン**, の値を指定 `https://<mobile_service>.azure-mobile.net/login/microsoftaccount/` で **リダイレクト URL**, 、順にクリックして **保存**します。

    ![](./media/mobile-services-register-windows-store-app/mobile-services-win8-app-push-auth-2.png)

9.  **アプリ設定**, の値をメモしておいてください **クライアント ID**, 、**クライアント シークレット**, 、および **パッケージ セキュリティ id (SID)**します。 

    ![](./media/mobile-services-register-windows-store-app/mobile-services-win8-app-push-auth.png)

    >[AZURE.NOTE]クライアント シークレットとパッケージ SID は重要なセキュリティ資格情報です。 これらの機密情報は、他のユーザーと共有したり、アプリケーションで配信したりしないでください。

10. ログオン、 [Azure クラシック ポータル](https://manage.windowsazure.com/), 、] をクリックして **Mobile Services**, 、アプリケーションをクリックします。

11. をクリックして、 **Identity** タブで、入力、 **クライアント シークレット** と **パッケージ SID** 値が手順 4. で WNS から取得した、クリックして **保存**します。

    ![](./media/mobile-services-register-windows-store-app/mobile-push-tab.png)

13. クリックして、 **Identity** ] タブをクリックします。 通知、 **クライアント シークレット** と **パッケージ SID** 値が、既に前の手順で設定します。 入力、 **クライアント ID** 先ほどのメモの順にクリックして **保存**します。

    ![](./media/mobile-services-register-windows-store-app/mobile-services-identity-tab.png)
 
これで、アプリケーションで認証に Microsoft アカウントを使用する準備ができました。  

<!-- Anchors. -->

<!-- Images. -->
 

<!-- URLs. -->
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582


