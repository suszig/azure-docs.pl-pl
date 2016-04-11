<properties
    pageTitle="Azure Mobile Engagement の使用"
    description="Azure Mobile Engagement を使用して、Android アプリの分析やプッシュ通知を送信する方法について説明します。"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="12/02/2015"
    ms.author="piyushjo" />

# Android アプリ用 Azure Mobile Engagement の使用

> [AZURE.SELECTOR]
- [Windows ユニバーサル](mobile-engagement-windows-store-dotnet-get-started.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-get-started.md)
- [iOS | Obj C](mobile-engagement-ios-get-started.md)
- [iOS | Swift](mobile-engagement-ios-swift-get-started.md)
- [Android](mobile-engagement-android-get-started.md)
- [Cordova](mobile-engagement-cordova-get-started.md)

このトピックでは、Azure Mobile Engagement を使用してアプリの使用状況を理解し、Android アプリケーションのセグメント化されたユーザーにプッシュ通知を送信する方法について説明します。
このチュートリアルでは、Mobile Engagement を使用した簡単なブロードキャスト シナリオのデモンストレーションを行います。 このシナリオでは、基本的なデータを収集する空の Android アプリを作成し、Google Cloud Messaging (GCM) を使ってプッシュ通知を受信します。

このチュートリアルには、次のものが必要です。

+ ダウンロードできます (ことを前提と Android Studio を使用して)、Android SDK [ここで](http://go.microsoft.com/fwlink/?LinkId=389797)
+ 、 [Mobile Engagement Android SDK]

> [AZURE.IMPORTANT] Android アプリ向けの他のすべてのモバイル エンゲージメント チュートリアルの前提条件は、このチュートリアルを完了して、これを完了するには、アクティブな Azure アカウントが必要です。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、「<a href="http://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure の無料試用版サイト</a>」を参照してください。

##<a id="setup-azme"></a>Android アプリ用のモバイル エンゲージメントの設定

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>アプリを Mobile Engagement のバックエンドに接続します

このチュートリアルでは、データを収集してプッシュ通知を送信するために必要な最小限のセットである「基本的な統合」について説明します。 完全な統合のドキュメントについては記載されて、 [Mobile Engagement Android SDK の統合](../mobile-engagement-android-sdk-overview/)

統合の例を示すために、Android Studio で基本的なアプリを作成します。

###新しい Android プロジェクトを作成する

1. 開始 **Android Studio**, 、し、ポップアップ ウィンドウに次のように選択します。 **新しい Android Studio プロジェクトを開始**します。

    ![][1]

2. アプリ名と会社のドメインを入力します。 後で使用するために入力内容をメモします。 クリックして **次**します。

    ![][2]

3. ターゲット フォーム ファクターと API レベルを選択し、クリックして **次**します。

    >[AZURE.NOTE] Mobile Engagement では、API レベル 10 の最小値 (Android 2.3.3) が必要です。

    ![][3]

4. 選択 **Blank Activity** ここをクリックしてこのアプリの唯一の画面となる **次**します。

    ![][4]

5. 最後に、同様に、既定値のままに **完了**します。

    ![][5]

Mobile Engagement を統合するデモ アプリが Android Studio によって作成されます。

###SDK ライブラリをプロジェクトに含める

SDK ライブラリをダウンロードして統合する

1. ダウンロード、 [Mobile Engagement Android SDK]します。
2. アーカイブ ファイルをコンピューターのフォルダーに抽出します。
3. SDK の現在のバージョン用の .jar ライブラリを探し、クリップボードにコピーします。

      ![][6]

4. 移動し、 **プロジェクト** セクション (1) と、[libs] フォルダー (2) に .jar を貼り付けます。

      ![][7]

5. ライブラリを読み込んでプロジェクトを最新にします。

      ![][8]

###接続文字列を使用してアプリを Mobile Engagement のバックエンドに接続します。

1. 次のコード行をアクティビティ作成の中にコピーします (アプリケーションの 1 か所でのみ実行する必要があります。通常はメイン アクティビティの中で実行します)。 このサンプル アプリの MainActivity を [src]、[main]、[java] フォルダーから開き、次を追加します。

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. Alt キーを押しながら Enter を押すか、次の import ステートメントを追加して、参照を解決します。

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. アプリの Azure クラシック ポータルに戻り **接続情報]** ページとコピー、 **接続文字列**します。

      ![][9]

4. 次の例に示すように、`setConnectionString` パラメーターに貼り付けて設定を置き換えます。

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

###アクセス権限とサービス宣言を追加する

1. 次のアクセス権限をプロジェクトの Manifest.xml の `<application>` タグの直前または直後に追加します。

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. 次の設定を `<application>`タグと`</application>` タグの間に追加して、エージェント サービスを宣言します。

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. 貼り付けたばかりのコードの label 内の `"<Your application name>"` をアプリケーション名に置き換えます。 これに表示されます、 **設定** ] メニューの [ユーザーがデバイスで実行されているサービスを表示できます。 たとえば、このラベルに「サービス」という語句を追加できます。

###画面を Mobile Engagement に送信する

データを送信してユーザーがアクティブであることを確認するには、少なくとも 1 つの画面 (アクティビティ) を Mobile Engagement のバックエンドに送信する必要があります。

移動して **MainActivity.java** の基本クラスを置き換えるには、次の追加と **MainActivity** に **されている EngagementActivity**:

    public class MainActivity extends EngagementActivity {

この簡単なサンプル シナリオでは、次の行をコメント アウト (除外) する必要があります。

    // setSupportActionBar(toolbar);

これまで残しておきたいかどうかは、「基本的なレポートを」シナリオを確認してください、 [Advanced Android Integration]

##<a id="monitor"></a>リアルタイム監視を使用してアプリを接続する

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>プッシュ通知とアプリ内メッセージングを有効にする

Mobile Engagement を導入すると、プッシュ通知とアプリ内メッセージングを利用して、ユーザーにキャンペーン情報を提供できます。 このモジュールは、Mobile Engagement ポータルで REACH として呼び出されます。
次のセクションでは、それらを受信するようにアプリをセットアップします。

### アプリ内メッセージングの有効化

1. Manifest.xml の `<application>` タグと `</application>` タグの間に、次のアプリ内メッセージングのリソースをコピーします。

        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
        </activity>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
            </intent-filter>
        </receiver>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
            <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
            </intent-filter>
        </receiver>

2. 次の手順に従って、プロジェクトにリソースをコピーします。
    1. SDK ダウンロード コンテンツに戻り、'res' フォルダーをコピーします。

         ![][13]

    2. Android Studio に戻り、プロジェクト ファイルの 'main' ディレクトリを選択して貼り付け、リソースをプロジェクトに追加します。

         ![][14]

###通知のアイコンを指定します。

次の XML スニペットを Manifest.xml の `<application>` タグと `</application>` タグの間に貼り付けます。

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

これにより、システムおよびアプリ内通知の両方に表示されるアイコンが定義されます。 これはアプリ内通知では任意ですが、システム通知では必須です。 Android では無効なアイコンが設定されたシステム通知は拒否されます。

いずれかに存在するアイコンを使用しているかどうかを確認、 **描画可能な** フォルダー (のような ``engagement_close.png``)。 **mipmap** フォルダーはサポートされていません。

>[AZURE.NOTE] 使用しないでください、 **ランチャー** アイコン。 このアイコンの解像度は異なっており、通常はサポート対象外の mipmap フォルダー内にあります。

実際のアプリに適したあたりの通知アイコンを使用する [Android の設計ガイドライン](http://developer.android.com/design/patterns/notifications.html)します。

>[AZURE.TIP] 確かに正しいアイコンの解像度を使用することができます見る [これらの例](https://www.google.com/design/icons)します。
下へスクロールして、 **通知** セクションで、アイコンをクリックし、をクリックし、 `PNGS` アイコンの描画可能なセットをダウンロードします。 アイコンのバージョンごとに、使用する描画可能なフォルダーと解像度を確認できます。

##API キーを使用して Google Cloud Messaging プロジェクトを作成する

[AZURE.INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

###GCM のプッシュ通知を受信するようにアプリを設定する

1. Google Play コンソールから取得した `project number` を置き換えた後、Manifest.xml の `<application>` タグと `</application>` タグ間に次を貼り付けます。 \n は意図的に付けられています。プロジェクト番号の末尾には必ずこれを付けてください。

        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />

2. 次のコードを、Manifest.xml の `<application>` タグと `</application>` タグの間に貼り付けます。 パッケージ名に置き換えます <Your package name>.

        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
        android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<Your package name>" />
            </intent-filter>
        </receiver>

3. 強調表示された最新のアクセス権限セットを、`<application>` タグの前に追加します。 `<Your package name>` をアプリケーションの実際のパッケージ名で置き換えます。

        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

###Mobile Engagement に GCM API キーへのアクセス権限を付与する

Mobile Engagement にプッシュ通知の送信を許可するには、API キーへのアクセス権限を付与する必要があります。 これは、キーを構成して Mobile Engagement ポータルに入力することで実行します。

1. Mobile Engagement ポータルに移動します

    Azure クラシック ポータルには、このプロジェクトで使用しているアプリ] をクリックしてでいることを確認、 **の関心を引く** 下部にあるボタンをクリックします。

    ![][15]

2. をクリックし、 **設定** ]-> [ **ネイティブ プッシュ通知** GCM キーを入力するにはセクション。

    ![][16]

3. をクリックして、 **編集** の前にアイコン **API キー** で、 **GCM 設定** セクションの次のようにします。

    ![][17]

4. ポップアップでは、前に取得した GCM サーバー キーを貼り付けてをクリックして **Ok**します。

    ![][18]

##<a id="send"></a>アプリへ通知を送信する

アプリにプッシュ通知を送信する単純なプッシュ通知キャンペーンを作成します。

1. 移動、 **到達** モバイル エンゲージメント ポータルでタブをクリックします。

2. クリックして **新しいお知らせ** 、プッシュ通知キャンペーンを作成します。

    ![][20]

3. 次の手順に従って、キャンペーンの最初のフィールドを設定します。

    ![][21]

    a. キャンペーンの名前を付けます。

    b. 選択、 **配信タイプ** として *システム通知シンプル]-> [*: これは、タイトルと数行のテキストを単純な Android のプッシュ通知の種類。

    c. 選択 **配信時刻** として *いつでも* アプリ通知を受信するか、アプリが起動するかどうかを許可するようにします。

    d. 通知のテキストの種類で、 **タイトル** でとなります太字にプッシュします。

    e. 入力し、 **メッセージ**

4. スクロール ダウンしで、 **コンテンツ** ] セクションの [ **通知のみ**します。

    ![][22]

5. 最も基本的なキャンペーンの設定が完了しました。 ここでもう一度下へスクロールし、クリックして、 **作成** ] ボタンをクリックしてキャンペーンを保存します。

6. 最後の手順: クリックして **Activate** プッシュ通知を送信してキャンペーンをアクティブにします。

    ![][24]

<!-- URLs. -->
[Mobile Engagement Android SDK]: https://aka.ms/vq9mfn
[Mobile Engagement Android SDK documentation]: https://aka.ms/tujlkm
[Advanced Android Integration]: https://azure.microsoft.com/en-us/documentation/articles/mobile-engagement-android-integrate-engagement/#basic-reporting

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[13]: ./media/mobile-engagement-android-get-started/copy-resources.png
[14]: ./media/mobile-engagement-android-get-started/paste-resources.png
[15]: ./media/mobile-engagement-android-get-started/engage-button.png
[16]: ./media/mobile-engagement-android-get-started/engagement-portal.png
[17]: ./media/mobile-engagement-android-get-started/native-push-settings.png
[18]: ./media/mobile-engagement-android-get-started/api-key.png
[20]: ./media/mobile-engagement-android-get-started/new-announcement.png
[21]: ./media/mobile-engagement-android-get-started/campaign-first-params.png
[22]: ./media/mobile-engagement-android-get-started/campaign-content.png
[24]: ./media/mobile-engagement-android-get-started/campaign-activate.png


