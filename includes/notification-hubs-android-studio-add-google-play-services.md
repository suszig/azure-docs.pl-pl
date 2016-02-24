1. Android Studio のツールバーにあるアイコンをクリックするかをクリックして、Android SDK Manager を開く **ツール** ]-> [ **Android** ]-> [ **SDK Manager** メニュー。 プロジェクトで使用される Android SDK の目的のバージョンを探してをクリックして開きます **パッケージの詳細を表示する**, 、選択 **Google APIs**, がインストールされていない場合は、です。

2. クリックして、 **SDK ツール** ] タブをクリックします。 Google Play Service をインストールしていない場合はクリックして **Google Play Services** 次のようにします。 クリックして **適用** をインストールします。 
 
    SDK のパスは後の手順で使用するので、メモしておいてください。 

    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)


3. 開いている、 **build.gradle** アプリケーション ディレクトリ内のファイルです。

    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)

4. 次の行を追加 *の依存関係*: 

        compile 'com.google.android.gms:play-services-base:6.5.87'

5.  *DefaultConfig*, 、変更 *minSdkVersion* 9 にします。
 
6. クリックして、 **Sync Project with Gradle Files** ツール バーのアイコン。

7. 開いている **AndroidManifest.xml** このタグを追加し、 *アプリケーション* タグ。

        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />
 





