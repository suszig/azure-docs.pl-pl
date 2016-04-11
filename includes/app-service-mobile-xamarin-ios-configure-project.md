####Xamarin Studio での iOS プロジェクトの構成

1. Xamarin.Studio で開く **Info.plist**, 、および更新、 **Bundle Identifier** 新しいアプリ ID で作成したバンドル ID を持つ

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)

2. 下へスクロールして **Background Modes** を確認し、 **Enable Background Modes** ボックスおよび **リモート通知** ボックス。 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)

3. 開くソリューション パネルでプロジェクトをダブルクリックして **プロジェクト オプション**します。

4.  選択 **iOS Bundle Signing** [ **ビルド**, 、し、対応する選択 **Identity** と **プロビジョニング プロファイル** だけこのプロジェクトを設定する必要があります。 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

    これで、プロジェクトではコード署名のために新しいプロファイルを使用するようになります。 公式の Xamarin デバイス プロビジョニングのドキュメントについて、次を参照してください。 [Xamarin Device Provisioning]します。

####Visual Studio での iOS プロジェクトの構成

1. Visual Studio でプロジェクトを右クリックし、をクリックして **プロパティ**します。

2. [プロパティ ページ] をクリックして、 **iOS アプリケーション** タブをクリックし、更新、 **識別子** 前に作成した ID を持つ。

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)

3.  **IOS Bundle Signing** タブで、対応する選択 **Identity** と **プロビジョニング プロファイル** だけこのプロジェクトを設定する必要があります。 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    これで、プロジェクトではコード署名のために新しいプロファイルを使用するようになります。 公式の Xamarin デバイス プロビジョニングのドキュメントについて、次を参照してください。 [Xamarin Device Provisioning]します。

4. ダブルクリックして開きを有効にし、Info.plist **RemoteNotifications** [バック グラウンド モードです。 



[Xamarin Device Provisioning]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
