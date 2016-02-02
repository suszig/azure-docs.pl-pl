<properties 
    pageTitle="Azure Mobile Engagement Android SDK の統合" 
    description="Android SDK for Azure Mobile Engagement の最新の更新情報と更新手順について"
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
    ms.topic="article" 
    ms.date="08/10/2015" 
    ms.author="piyushjo" />


# Engagement Reach を Android に統合する方法

> [AZURE.IMPORTANT] このガイドの手順を実行する前に、「Engagement を Android に統合する方法」のドキュメントの統合手順を実行する必要があります。

## 標準統合

Reach SDK には **Android サポート ライブラリ (v4)** が必要です。

プロジェクトにライブラリを追加する最も簡単な方法 **Eclipse** は `右側をクリックしてプロジェクトを Android ツール]-> [サポート ライブラリの追加...] の [`します。

Eclipse を使用しない場合は、こちらの手順を読み取ることができます。

プロジェクトの SDK から Reach リソースのファイルをコピーします。

-   ファイルをコピー、 `res/レイアウト` に SDK とともに配布されたフォルダー、 `res/レイアウト` 、アプリケーションのフォルダーです。
-   ファイルをコピー、 `res/描画可能な` に SDK とともに配布されたフォルダー、 `res/描画可能な` 、アプリケーションのフォルダーです。

編集、 `AndroidManifest.xml` ファイル。

-   次のセクションを追加 (間、 `< アプリケーション >` と `</アプリケーション >` タグ)。

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

-   ブート時にクリックされなかったシステム通知を再生するには、このアクセス許可が必要です (アクセス許可がない場合、それらの通知はディスクに残りますが表示できなくなるため、必ず含める必要があります)。

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

-   通知 (アプリとシステムの両方) をコピーして、次のセクションを編集に使用されるアイコンを指定 (間、 `< アプリケーション >` と `</アプリケーション >` タグ)。

            <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />


> [AZURE.IMPORTANT] このセクションは、Reach キャンペーンの作成時にシステム通知を使用する場合に**必須**です。 Android では、アイコンのないシステム通知は表示されません。 そのため、このセクションを省略すると、エンド ユーザーは通知を受け取ることができません。

-   全体像を使用するシステム通知でキャンペーンを作成する場合は、次のアクセス許可を追加する必要があります。 (後、 `</アプリケーション >` タグ) を失っている場合。

            <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
            <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

  -   Android M とアプリケーションが Android API レベル 23 以上を対象に `WRITE_EXTERNAL_STORAGE` アクセス許可がユーザーの承認が必要です。 お読みください [ここ](mobile-engagement-android-integrate-engagement.md#android-m-permissions)します。

-   また、システム通知では、Reach キャンペーンでデバイスの音を鳴らしたりバイブレーションが動作したりするように指定できます。次のアクセス許可が宣言されているかどうかを確認する必要が動作するため、(後、 `</アプリケーション >` タグ)。

            <uses-permission android:name="android.permission.VIBRATE" />

    Reach キャンペーン マネージャーで音を鳴らすオプションやバイブレーションのオプションを有効にしても、このアクセス許可が存在しない場合は Android でシステム通知が表示されません。

-   使用して、アプリケーションをビルドする場合 **ProGuard** Android サポート ライブラリや Engagement jar に関連するエラーがあると、次の行を追加、 `proguard.cfg` ファイル。

            -dontwarn android.**
            -keep class android.support.v4.** { *; }


## Native Push (ネイティブのプッシュ)

Reach モジュールを構成したので、デバイスのキャンペーンを受信できるようにネイティブ プッシュ通知を設定する必要があります。

Android では次の 2 つのサービスをサポートしています。

  - Google Play デバイス: の後で [Google Cloud Messaging] 使用する、 [エンゲージメント ガイドに GCM を統合する方法](mobile-engagement-android-gcm-integrate.md) ガイドです。
  - Amazon デバイス: の後で [Amazon Device Messaging] 使用する、 [ADM を Engagement ガイドを統合する方法](mobile-engagement-android-adm-integrate.md) ガイドです。

Amazon と Google Play の両方のデバイスを対象とするコードは、1 つの AndroidManifest.xml/APK 内で開発することもできます。 ただし、アプリケーションに GCM のコードが見つかると、Amazon に送信する際に却下されることがあります。

その場合は、複数の APK を使用してください。

**お使いのアプリケーションで Reach キャンペーンを受信して表示する準備が整いました。**

## データプッシュを管理する方法

### 統合

サブクラスを作成する必要がある、アプリケーションは、Reach データ プッシュを受信できるようにする場合は、 `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` でそれを参照し、 `AndroidManifest.xml` ファイル (間、 `< アプリケーション >` や `</アプリケーション >` タグ)。

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

オーバーライドすることができ、 `onDataPushStringReceived` と `onDataPushBase64Received` コールバックします。 たとえば次のようになります。

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }
    
              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### カテゴリ

カテゴリ パラメーターは、データ プッシュ キャンペーンを作成するときに指定でき、データ プッシュをフィルター処理するために使用できます。 これはさまざまな種類のデータ プッシュを処理する複数のブロードキャスト レシーバーがある場合に役立ちます。 さまざまな種類をプッシュする場合、またはの `Base64` データやを解析前にそれらの種類を識別します。

### コールバックのリターン パラメーター

戻り値パラメーターを適切に処理するいくつかのガイドラインを `onDataPushStringReceived` と `onDataPushBase64Received`:

-   ブロードキャスト レシーバーが返す必要があります `null` データ プッシュを管理する方法がわからない場合は、コールバックにします。 ブロードキャスト レシーバーがデータ プッシュを管理するかどうかを指定するには、カテゴリを使用します。
-   ブロードキャスト レシーバーの 1 つを返す必要があります `true` データ プッシュを受け取る場合、コールバックでします。
-   ブロードキャスト レシーバーの 1 つを返す必要があります `false` 、コールバックする場合、データ プッシュを認識は何らかの理由で破棄します。 たとえば、返す `false` 受信データが有効な場合です。
-   あるブロードキャスト レシーバー場合 `true` 、もう 1 つを返します `false` 、同じデータ プッシュ、動作は定義、ことを決してしないでください。

リターン タイプは Reach の統計情報でのみ使用されます。

-   `返信` は、ブロードキャスト レシーバーのいずれかのいずれかを返す場合はインクリメント `true` または `false`します。
-   `Actioned (アクション)` は、ブロードキャスト レシーバーの 1 つ返す場合にのみインクリメントされます `true`します。

## キャンペーンをカスタマイズする方法

キャンペーンをカスタマイズするには、Reach SDK に用意されたレイアウトを修正します。

レイアウトに使用される識別子や識別子を使用するビューのタイプ (特にテキスト ビューやイメージ ビュー) はそのままにしてください。 領域を表示/非表示にするためだけに使用される一部のビューは変更してもかまいません。 用意されたレイアウトでタイプを変更する場合は、ソース コードを確認してください。

### 通知

通知にはシステム通知とアプリ内通知の 2 種類があり、別個のレイアウト ファイルが使用されます。

#### システム通知

システム通知をカスタマイズするには、**カテゴリ**を使用する必要があります。 移動する [カテゴリ](#categories)します。

#### アプリ内通知

既定では、アプリ内通知は Android メソッドに協力してくれた、現在のアクティビティ ユーザー インターフェイスに動的に追加されているビューを `addContentView()`します。 これは、通知オーバーレイと呼ばれます。 通知オーバーレイを使用すると、アプリケーションのレイアウトを変更する必要がないため、統合が簡単です。

通知オーバーレイの見た目を変更するには、ファイルだけで変更できます `engagement_notification_area.xml` 、ニーズにします。
> [AZURE.NOTE] ファイル `engagement_notification_overlay.xml` 通知オーバーレイの作成に使用される 1 つは、ファイルが含まれています `engagement_notification_area.xml`します。 また、ニーズに合わせて変更できます (オーバーレイ内部の通知領域の配置など)。

##### 通知レイアウトをアクティビティ レイアウトの一部に含める

オーバーレイを使用すると統合が簡単になりますが、場合によって不便であり、特定の条件では副作用が発生することもあります。 オーバーレイ システムはアクティビティ レベルでカスタマイズ可能であり、特定のアクティビティにおいて副作用を簡単に防ぐことができます。

Android の **include** ステートメントにより、既存のレイアウトに通知レイアウトを含めることができます。 修正後の例を次に示します `ListActivity` レイアウトがだけを含む、 `ListView`します。

**Engagement 統合前:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Engagement 統合後:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">
    
              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />
    
              <include layout="@layout/engagement_notification_area" />
    
            </LinearLayout>

この例では、元のレイアウトのトップ レベル要素としてリスト ビューが使用されていたため、親コンテナーを追加しました。 追加しました `android: layout_weight =「1」` で構成されたリスト ビューの下にビューを追加できる `android: layout_height ="fill_parent"`します。

Engagement Reach SDK では、このアクティビティに通知レイアウトが含まれていることが自動的に検知されるため、このアクティビティに対してオーバーレイを追加しません。
> [AZURE.TIP] アプリケーションで ListActivity を使用すると、表示される Reach オーバーレイによってリスト ビュー内のクリックされたアイテムに応答できなくなります。 これは既知の問題です。 この問題を解決するには、前述のサンプルのように独自のリスト アクティビティ レイアウトに通知レイアウトを組み込むことをお勧めします。

##### アクティビティごとにアプリケーション通知を無効にする

アクティビティに追加されるオーバーレイしたくないかどうかは、独自のレイアウトに通知レイアウトが含めない場合に、このアクティビティのオーバーレイを無効にすることができます、 `AndroidManifest.xml` 追加することで、 `メタ データ` セクションは次の例のようにします。

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a> カテゴリ

指定のレイアウトを修正すると、通知すべての見た目が修正されます。 カテゴリを使用して、通知対象のさまざまな外観 (場合によっては動作) を定義できます。 リーチ キャンペーンを作成する場合にカテゴリを指定できます。 また、カテゴリを使用してお知らせとポーリングもカスタマイズできます。これについては、このドキュメントの後半で説明します。

通知のカテゴリ ハンドラーを登録するには、アプリケーションが初期化される際に呼び出しを追加する必要があります。
> [AZURE.IMPORTANT] Android:process 属性に関する警告を参照してください \<android-sdk-engagement-process\> 続行する前に Android のトピックでエンゲージメントを統合する方法」にします。

次の例は、前述の警告を確認してのサブクラスを使用する `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

`MyNotifier` オブジェクトは、通知カテゴリ ハンドラーの実装です。 これはどちらの実装、 `EngagementNotifier` インターフェイスまたは既定の実装のサブクラス。 `EngagementDefaultNotifier`します。

同じ通知で複数のカテゴリを管理できます。次のように登録します。

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

既定のカテゴリ実装を置き換えるには、実装を次の例のように登録します。

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

ハンドラーで使用される現在のカテゴリを上書きするほとんどのメソッドにパラメーターとして渡されます `EngagementDefaultNotifier`します。

いずれかとして渡される、 `文字列` パラメーターに間接的に、 `EngagementReachContent` を持つオブジェクト、 `getCategory()` メソッドです。

通知作成プロセスのほとんどを変更するにはメソッドを再定義 `EngagementDefaultNotifier`, 、高度なカスタマイズ気軽に技術ドキュメントやソース コードを実行します。

##### アプリ内通知

特定のカテゴリに対して代替のレイアウトを使用するには、次の例のようにこれを実装します。

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }
    
              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }
    
    
              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }
    
              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

* * の使用例の `my_notification_overlay.xml` : * *

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">
    
              <include layout="@layout/my_notification_area" />
    
            </RelativeLayout>

オーバーレイのビュー識別子が標準のものとは異なることがわかります。 オーバーレイでは、各レイアウトで固有の識別子を使用することが重要です。

* * の使用例の `my_notification_area.xml` : * *

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">
    
              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">
    
                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">
    
                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />
    
                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">
    
                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />
    
                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />
    
                  </LinearLayout>
    
                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />
    
                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />
    
                </LinearLayout>
    
                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />
    
              </RelativeLayout>
    
            </merge>

通知領域のビュー識別子が標準のものとは異なることがわかります。 通知領域では、各レイアウトで固有の識別子を使用することが重要です。

このカテゴリの簡単な例により、アプリケーション通知 (アプリ内通知) が画面の上部に表示されます。 通知領域で使用される標準の識別子自体は変更していません。

再定義する必要を変更する場合、 `EngagementDefaultNotifier.prepareInAppArea` メソッドです。 技術ドキュメントとのソース コードを検索することをお勧め `EngagementNotifier` と `EngagementDefaultNotifier` する場合はこのレベルの高度なカスタマイズします。

##### システム通知

拡張することによって `EngagementDefaultNotifier`, をオーバーライドできます `onNotificationPrepared` を既定の実装によって用意された通知を変更します。

次に例を示します。

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

この例では、"ongoing" カテゴリの使用によりコンテンツのシステム通知が進行中のイベントとして表示されます。

ビルドする場合、 `通知` オブジェクトを一から、返すことができます `false` メソッドの呼び出しに `通知` で自身、 `NotificationManager`します。 保持する重要な場合は、 `contentIntent`, 、 `deleteIntent` とで使用される通知識別子 `EngagementReachReceiver`します。

このような実装の正しい例は次のとおりです。

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent
    
              /* Your customization */
              // builder.set...
    
              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;
    
              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier
    
              /* Return false, we notify ourselves */
              return false;
            }

##### 通知のみのアナウンス

のみのアナウンスをオーバーライドすることでカスタマイズできる通知でのクリックの管理 `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` 用意されたを変更する `インテント`します。 このメソッドを使用すると、フラグを簡単にチューニングできます。

追加する例については、 `SINGLE_TOP` フラグ。

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

従来の Engagement とは異なり、アクション URL を使用しないシステム通知でバックグラウンドのアプリケーションが起動されるようになりました。このため、このメソッドはアクション URL を使用しないアナウンスで呼び出すことができます。 インテントをカスタマイズする際にはそれを考慮してください。

実装することも `EngagementNotifier.executeNotifAnnouncementAction` を最初からです。

##### 通知のライフ サイクル

いくつかのライフ サイクル メソッドを呼び出す既定のカテゴリを使用する場合、 `EngagementReachInteractiveContent` オブジェクトの統計情報を報告し、キャンペーンの状態を更新します。

-   通知がアプリケーションに表示されるまたはステータス バーに、 `displayNotification` メソッドが呼び出されます (統計情報を報告) によって `EngagementReachAgent` 場合 `handleNotification` を返します `true`します。
-   通知を閉じる場合、 `exitNotification` メソッドが呼び出されて統計情報が報告され、次のキャンペーンが処理できるようことができます。
-   通知をクリックすると、 `actionNotification` が呼び出されると、統計情報を報告し、関連するインテントが起動されます。

場合は、実装の `EngagementNotifier` 既定の動作をバイパスを自分でこれらのライフ サイクル メソッドを呼び出す必要があります。 次の例では、既定の動作がバイパスされるいくつかのケースを示しています。

-   拡張しない `EngagementDefaultNotifier`, 、最初からカテゴリの処理を実装した場合などです。
-   オーバーライドした場合、システム通知に、 `onNotificationPrepared` であり、変更する `contentIntent` または `deleteIntent` で、 `通知` オブジェクトです。
-   オーバーライドした場合、アプリ内通知に `prepareInAppArea`, 、ともがマッピングを必ず `actionNotification` がいずれかの U.I コントロールです。

> [AZURE.NOTE] 場合 `handleNotification` をスローし、例外は、コンテンツを削除し、 `dropContent` が呼び出されます。 これは作成される統計情報のレポートに掲載され、次のキャンペーンが処理できるようになります。

### お知らせとポーリング

#### レイアウト

変更することができます、 `engagement_text_announcement.xml`, 、`engagement_web_announcement.xml` と `engagement_poll.xml` テキスト アナウンス、web アナウンスと投票をカスタマイズするファイルです。

これらのファイルは、タイトル領域とボタン領域で 2 つのレイアウトを共有します。 タイトルのレイアウトは `engagement_content_title.xml` と、バック グラウンドにそれにちなんだ描画可能なファイルを使用します。 アクションと終了ボタンのレイアウトは `engagement_button_bar.xml` と、バック グラウンドにそれにちなんだ描画可能なファイルを使用します。

投票で質問のレイアウトとその選択肢が動的に値が大きく複数回使用する、 `engagement_question.xml` レイアウト ファイルに関する質問については、および `engagement_choice.xml` 、選択肢です。

#### カテゴリ

##### 代替レイアウト

通知と同様に、キャンペーンのカテゴリを使用して、お知らせとポーリングの代替レイアウトを設定できます。

たとえば、テキスト アナウンスのカテゴリを作成することができますを拡張する `EngagementTextAnnouncementActivity` し、参照、 `AndroidManifest.xml` ファイル。

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

インテント フィルターのカテゴリは、既定のアナウンス アクティビティと差別化するために使用されます。

Reach SDK はインテント システムを使用して特定のカテゴリに対する正しいアクティビティを解決し、解決されない場合は既定のカテゴリにフォールバックします。

実装する必要が `MyCustomTextAnnouncementActivity`, 、だけでは、次の例のようにクラスを定義する必要があるレイアウトを変更する (ただし、同じビュー識別子を保持) するだけの場合。

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

テキスト アナウンスの既定のカテゴリを置き換えるには、単純に置き換える `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` を独自の実装です。

Web アナウンスと投票についても同様にカスタマイズできます。

Web アナウンスを拡張できます `EngagementWebAnnouncementActivity` 、アクティビティを宣言、 `AndroidManifest.xml` 次の例のようにします。

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    
              </intent-filter>
            </activity>

投票を拡張できます `EngagementPollActivity` を宣言しで、 `AndroidManifest.xml` 次の例のようにします。

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### 一から実装する

カテゴリは、のいずれかを拡張することがなく、アナウンス (および投票) アクティビティを実装できます、 `エンゲージメント * アクティビティ` Reach SDK が提供するクラス。 これは、標準のレイアウトと異なるビューを使用するレイアウトを定義する場合などに便利です。

高度な通知カスタマイズと同様に、標準実装のソース コードをご確認ください。

次の点に注意してください。Reach は (インテント フィルターに対応する) 特定のインテントと、コンテンツ識別子である追加パラメーターを使用してアクティビティを起動します。

Web サイトのキャンペーンを作成する際に指定したフィールドが含まれるコンテンツ オブジェクトを取得するには、次のようにします。

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;
    
              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
    
                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }
    
                setContentView(R.layout.my_text_announcement);
    
                /* Configure views by querying fields on mContent */
                // ...
              }
            }

統計情報についてコンテンツを表示するを報告する、 `onResume` イベント。

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

次に、忘れずにいずれかを呼び出す `actionContent(this)` または `exitContent(this)` 、アクティビティがバック グラウンドに移行する前に、コンテンツ オブジェクトにします。

いずれかが呼び出されない場合 `actionContent` または `exitContent`, 、統計情報は送信されず (は分析されませんキャンペーン)、さらに、次のキャンペーンが通知されないアプリケーション プロセスが再起動されるまでです。

印刷の向きやその他の構成の変更により、コードかどうか、アクティビティに移行するバック グラウンドか、標準実装場合は、ユーザーがアクティビティを終了して、コンテンツが報告されることを確認して特定する (を押す `ホーム` または `戻る`) が、印刷の向きを変更できません。

実装について注目する部分は次のとおりです。

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }
    
            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

呼び出した場合、参照できるよう `actionContent(this)` 、アクティビティを完了 `exitContent(this)` せず、影響を及ぼさずに安全に呼び出すことができます。

## テスト

「Android での Engagement の統合をテストする方法」を読んで、統合を検証してください。


[here]: http://developer.android.com/tools/extras/support-library.html#Downloading 
[google cloud messaging]: http://developer.android.com/guide/google/gcm/index.html 
[amazon device messaging]: https://developer.amazon.com/sdk/adm.html 

