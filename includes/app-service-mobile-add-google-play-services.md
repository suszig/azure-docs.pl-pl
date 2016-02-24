1. Android Studio のツールバーにあるアイコンをクリックするかをクリックして、Android SDK Manager を開く **ツール** ]-> [ **Android** ]-> [ **SDK Manager** メニュー。 プロジェクトで使用される Android SDK の目的のバージョンを探してを開き **Google APIs**, がインストールされていない場合。

2. 移動して **ファイル**, 、**プロジェクト構造**します。 [プッシュ通知を有効にし、 **通知**します。

3. 開いている **AndroidManifest.xml** このタグを追加し、 *アプリケーション* タグ。

        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />
 





