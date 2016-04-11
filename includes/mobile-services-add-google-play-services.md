1. Android Studio のツールバーにあるアイコンをクリックするかをクリックして、Android SDK Manager を開く **ツール** ]-> [ **Android** ]-> [ **SDK Manager** メニュー。 プロジェクトで使用される Android SDK の目的のバージョンを探してを開き **Google APIs**, がインストールされていない場合。

2. 下へスクロールして **Extras**, 、展開し、選択 **Google Play Services**, 、次のようにします。 クリックして **パッケージをインストール**します。 SDK のパスはこの後の手順で使用するので、メモしておいてください。 

    ![](./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png)


3. 開いている、 **build.gradle** アプリケーション ディレクトリ内のファイルです。

    ![](./media/mobile-services-android-get-started-push/android-studio-push-build-gradle.png)

4. 次の行を追加 *の依存関係*: 

        compile 'com.google.android.gms:play-services-base:6.5.87'

5.  *DefaultConfig*, 、変更 *minSdkVersion* 9 にします。
 
6. クリックして、 **Sync Project with Gradle Files** ツール バーのアイコン。

7. 開いている **AndroidManifest.xml** このタグを追加し、 *アプリケーション* タグ。

        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />
 





