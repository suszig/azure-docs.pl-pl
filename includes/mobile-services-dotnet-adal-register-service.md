## <a name="register-mobile-service-aad"></a>モバイル サービスを登録する Azure Active Directory

このセクションでは、Azure Active Directory にモバイル サービスを登録し、シングル サインオン権限の借用を許可するアクセス許可を構成します。

1. Azure Active Directory と Azure Active Directory に登録する方法] の後にアプリケーションを登録トピックです。

2. [Azure clasic ポータル](https://manage.windowsazure.com/), し、Azure Active Directory 拡張機能に戻り、active directory をクリックして

3. **[アプリケーション]** タブをクリックし、目的のアプリケーションをクリックします。

4. **[マニフェストの管理]** をクリックします。 その後、**[マニフェストのダウンロード]** をクリックし、アプリケーション マニフェストをローカル ディレクトリに保存します。

   ![](./media/mobile-services-dotnet-adal-register-service/mobile-services-aad-app-manage-manifest.png)

5. Visual Studio でアプリケーション マニフェスト ファイルを開きます。 ファイルの最上部で、次のように表示されたアプリケーションのアクセス許可の行を見つけます。

        "oauth2Permissions": [],

    その行を、次のアプリケーション アクセス許可で置き換え、ファイルを保存します。

        "oauth2Permissions": [
            {
                "adminConsentDescription": "Allow the application access to the mobile service",
                "adminConsentDisplayName": "Have full access to the mobile service",
                "id": "b69ee3c9-c40d-4f2a-ac80-961cd1534e40",
                "isEnabled": true,
                "origin": "Application",
                "type": "User",
                "userConsentDescription": "Allow the application full access to the mobile service on your behalf",
                "userConsentDisplayName": "Have full access to the mobile service",
                "value": "user_impersonation"
            }
        ],

6. [Azure クラシック ポータル](https://manage.windowsazure.com/), 、] をクリックして **マニフェストの管理** をクリックして、アプリケーションをもう一度の **マニフェストのアップロード**します。 更新したアプリケーション マニフェストの場所を参照し、マニフェストをアップロードします。



[how to register with the azure active directory]: ../articles/mobile-services/mobile-services-how-to-register-active-directory-authentication.md 

