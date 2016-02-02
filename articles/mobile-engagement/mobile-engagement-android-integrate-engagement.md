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


# Engagement を Android に統合する方法

> [AZURE.SELECTOR]
- [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)


この手順では、Engagement の分析機能と監視機能を Android アプリでアクティブ化するための最も簡単な方法について説明します。
> [AZURE.IMPORTANT] Android SDK の最小の API レベルは 10 以降 (Android 2.3.3 以降) である必要があります。

ユーザー、セッション、アクティビティ、クラッシュ、および技術に関するすべての統計をコンピューティングするために必要なログ レポートをアクティブ化するには、以下の手順で十分です。 イベント、エラー、ジョブなどその他の統計の計算に必要なログのレポートは、エンゲージメント API を使用して手動で行う必要があります (を参照してください [タグ付け API を Android で高度なモバイル エンゲージメントを使用する方法](mobile-engagement-android-use-engagement-api.md) これらの統計がアプリケーションに依存するためです。

## Android プロジェクトに Engagement SDK とサービスを埋め込む

から Android SDK をダウンロード [ここ](https://aka.ms/vq9mfn)
取得 `mobile engagement-VERSION.jar` にまとめて、 `libs` Android プロジェクトのフォルダー (がまだない場合は、libs フォルダーを作成する)。
> [AZURE.IMPORTANT]
> アプリケーション パッケージを ProGuard でビルドする場合は、いくつかのクラスを保持する必要があります。 次の構成スニペットを使うことができます。
>
>
            -パブリック クラスを保持 * android.os.IInterface の拡張
            -クラス com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {の保持
            <methods>;
            {

ランチャー アクティビティ内で次のメソッドを呼び出すことで、Engagement の接続文字列を指定します。

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

アプリケーションの接続文字列が Azure ポータルに表示されます。

-   が不足している場合は、次の Android のアクセス許可を追加 (前に、 `< アプリケーション >` タグ)。

            <uses-permission android:name="android.permission.INTERNET"/>
            <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

-   次のセクションを追加 (間、 `< アプリケーション >` と `</アプリケーション >` タグ)。

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

-   変更 `< アプリケーション名 >` 、アプリケーションの名前。

> [AZURE.TIP] `Android: ラベル` 属性では、携帯電話の「実行中のサービス」画面、エンドユーザーに表示される Engagement サービスの名前を選択することができます。この属性を設定することが推奨 `"< アプリケーション名 > サービス"` (例。`"AcmeFunGameService"`)。

指定する、 `android: プロセス` 属性により、Engagement サービスが (同じプロセスでの活動を実行するアプリケーションとは、メイン/UI スレッド応答が遅い可能性があります)、独自のプロセスで実行します。
> [AZURE.NOTE] 配置したすべてのコード `Application.onCreate()` とその他のアプリケーション コールバックは、Engagement サービスを含むすべてのアプリケーションのプロセスに実行されます。 望ましくない副作用が発生する可能性があります (Engagement のプロセス内の不要なメモリとスレッドの割り当て、重複するブロードキャストの受信側またはサービスなど)。

オーバーライドする場合は `Application.onCreate()`, の先頭に次のコード スニペットを追加することをお勧め、 `Application.onCreate()` 関数。

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;
    
               ... Your code...
             }

同じ処理を行うことができます `Application.onTerminate()`, 、`Application.onLowMemory()` と `Application.onConfigurationChanged(...)`します。

拡張することも `EngagementApplication` を拡張するのではなく `アプリケーション`: コールバック `Application.onCreate()` プロセスのチェックが行わと呼び出し `Application.onApplicationProcessCreate()` を場合のみ、現在のプロセスが Engagement サービスをホストしている 1 つは、同じ規則が、その他のコールバック。

## 基本的なレポート

### 推奨される方法: オーバー ロード、 `アクティビティ` クラス

ユーザー、セッション、アクティビティ、クラッシュ、技術に関する統計の計算に Engagement が必要なすべてのログのレポートをアクティブ化するためにのみ確認する必要がすべて、 `* アクティビティ` サブクラスを対応する継承 `エンゲージメント * アクティビティ` クラス (例: レガシー アクティビティが場合 `ListActivity`, 、拡張 `EngagementListActivity`)。

**エンゲージメントを使用しない場合:**

            package com.company.myapp;
    
            import android.app.Activity;
            import android.os.Bundle;
    
            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**エンゲージメントを使用する場合:**

            package com.company.myapp;
    
            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;
    
            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [AZURE.IMPORTANT] 使用する場合 `EngagementListActivity` または `EngagementExpandableListActivity`, にあることを確認する呼び出し `requestWindowFeature(...);` を呼び出す前に行われた `super.onCreate(...);`, 、それ以外の場合、クラッシュが発生します。

サブクラスの提供 `FragmentActivity` と `MapActivity`, を使用してアプリケーションの問題を回避するが、 **ProGuard**, で私たちは含まない `engagement.jar`します。

これらのクラスを見つけることができます、 `src` フォルダー、プロジェクトにコピーできます。 クラスは **JavaDoc** にも用意されています。

### 代替の方法: 呼び出す `startActivity()` と `endActivity()` 手動で

できないか、オーバー ロードしたくない場合、 `アクティビティ` クラス、代わりに起動しを呼び出して、アクティビティを終了することができます `EngagementAgent`のメソッドを直接します。
> [AZURE.IMPORTANT] Android SDK が呼び出すことはありません、 `endActivity()` メソッドをアプリケーションが閉じられる場合でも (android では、アプリケーションは実際には閉じられません)。 そのため、 *高* を呼び出すことをお勧め、 `startActivity()` メソッドに、 `onResume` のコールバック *すべて* のアクティビティの `endActivity()` メソッドに、 `onPause()` のコールバック *すべて* 、アクティビティ。 これは、セッションがリークしないことを保証する唯一の方法です。 セッションがリークした場合、(セッションが保留中である限り、サービスが接続されたままになるため)、Engagement サービスが Engagement バックエンドから切断されることはありません。

たとえば次のようになります。

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }
    
              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

この例とよく似て、 `されている EngagementActivity` クラスとそのバリアントを持つソース コードに示されて、 `src` フォルダーです。

## テスト

エミュレーターまたはデバイスでモバイル アプリを実行し、このアプリによって [監視] タブにセッションが登録されることを確認して、統合を検証してください。

この後のセクションはオプションです。

## 場所レポート

場所を報告する場合は、構成のいくつかの行を追加する必要があります (間、 `< アプリケーション >` と `</アプリケーション >` タグ)。

### 大まかなエリアの位置報告

大まかなエリアの位置報告では、デバイスに関連付けられた国、リージョン、地域をレポートできます。 このタイプの場所レポートでは、セル ID または WIFI に基づいたネットワークの場所のみを使用します。 デバイス エリアがセッションごとに最大 1 回レポートされます。 GPS を使用しないため、このタイプの場所レポートでは、まったくとは言わないまでも、電力消費量にほとんど影響がありません。

報告されたエリアを基に、ユーザー、セッション、イベント、エラーに関する地理的な統計をコンピューティングします。 また、リーチ キャンペーンの条件としても使用されます。

大まかなエリアの位置報告を有効にするには、この手順の前の構成を利用できます。

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

さらに、次のアクセス権限が不足している場合は追加する必要があります。

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

ままにして、または `ACCESS_FINE_LOCATION` 場合は、アプリケーションで既に使用します。

### リアル タイム場所レポート

リアル タイム場所レポートでは、デバイスに関連付けられた緯度と経度をレポートできます。 既定では、このタイプの場所レポートでは (セル ID または WIFI に基づいた) ネットワークの場所のみが使用されます。レポートは、アプリケーションを前景で実行した場合 (セッション中) にのみアクティブになります。

リアル タイム場所レポートは、統計のコンピューティングに使用することは*できません*。 このレポートの目的は、リーチ キャンペーンでリアル タイムの
地理フェンス操作 \<Reach-Audience-geofencing\> リーチ キャンペーンの条件。

リアルタイム場所レポートを有効にするには、この手順の前の構成を利用できます。

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

さらに、次のアクセス権限が不足している場合は追加する必要があります。

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

ままにして、または `ACCESS_FINE_LOCATION` 場合は、アプリケーションで既に使用します。

#### GPS ベースのレポート

既定では、リアル タイム場所レポートでは、ネットワーク ベースの場所のみを使用します。 GPS ベースの場所 (正確性が格段に優れています) を使用できるようにするには、構成オブジェクトを使用します。

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

さらに、次のアクセス権限が不足している場合は追加する必要があります。

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### 背景レポート

既定では、リアル タイム場所レポートは、アプリケーションを前景で実行した場合 (セッション中) にのみアクティブになります。 バックグラウンドでのレポートも有効にするには、構成オブジェクトを使用します。

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] アプリケーションを背景で実行した場合、GPS を有効にしても、ネットワーク ベースの場所のみがレポートされます。

バックグラウンド ロケーション レポートは、ユーザーがデバイスを再起動すると停止します。以下を追加することで、起動時に自動的に再開されるように設定できます。

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

さらに、次のアクセス権限が不足している場合は追加する必要があります。

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### Android M アクセス許可

Android M より、一部のアクセス許可が実行時に管理され、ユーザーの承認を必要とします。

Android API レベル 23 が対象の場合、新しいアプリのインストールでは既定で実行時のアクセス許可がオフになります。 それ以外の場合、既定でオンになります。

デバイスの設定メニューからアクセス許可の有効/無効を設定できます。 システム メニューでアクセス許可をオフにした場合、アプリケーションのバックグラウンド プロセスが停止します。これはシステム動作であり、バックグラウンドでプッシュを受信する機能には影響を与えません。

Mobile Engagement の場合、次のアクセス許可で実行時に承認が必要になります。

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`
- `WRITE_EXTERNAL_STORAGE` (これは Android API レベル 23 を対象とする) 場合のみ

外部ストレージはリーチの大きい画像機能でのみ使用されます。 このアクセス許可をユーザーに求めることが混乱を招くようであれば、Mobile Engagement のためにのみ使用した場合、このアクセス許可を削除できます。ただし、大きい画像機能を無効にする必要があります。

場所機能の場合、標準のシステム ダイアログを利用してユーザーにアクセス許可を要求する必要があります。 通知する必要がある場合は、ユーザーが承認されると、 `EngagementAgent` リアルタイムでアカウントにその変更を実行する (それ以外の場合、変更、処理、次回ユーザーがアプリケーションを起動)。

アクセス許可を要求する正の場合、結果を転送して、アプリケーションのアクティビティで使用するコード サンプルを次に示します `EngagementAgent`:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */
    
      /* Request permissions */
      requestPermissions();
    }
    
    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);
    
        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }
    
    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

## 詳細な報告

必要に応じて、アプリケーション固有のイベント、エラー、ジョブを報告する場合のメソッドを通じて Engagement API を使用する、 `EngagementAgent` クラスです。 このクラスのオブジェクトでは、呼び出すことによって取得ができます、 `EngagementAgent.getInstance()` 静的メソッドです。

Engagement API では、すべての Engagement の高度な機能を使用でき、
Engagement API を Android に (およびの技術文書の `EngagementAgent` クラス)。

## 高度な構成 (AndroidManifest.xml 内)

WIFI を使っているとき、または画面がオフのときにに統計がリアルタイムで送信されることを保証する場合は、次のオプションのアクセス権限を追加します。

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

クラッシュ レポートを無効にする場合は、これを追加 (間、 `< アプリケーション >` と `</アプリケーション >` タグ)。

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

既定では、Engagement サービスはログをリアルタイムで報告します。 アプリケーションがログを送信する回数が非常に多い場合は、ログをバッファーに格納して、一定時間ごとにまとめて報告することをお勧めします (これは "バースト モード" と呼ばれます) 。 これを行うには、これを追加 (間、 `< アプリケーション >` と `</アプリケーション >` タグ)。

            <meta-data android:name="engagement:burstThreshold" android:value="<interval between too bursts (in milliseconds)>"/>

バースト モードではわずかにバッテリーの寿命が延びますが、Engagement の監視に影響を与えます。すべてのセッションとジョブの実行時間は、バーストのしきい値に丸められます (つまり、バーストのしきい値よりも短いセッションとジョブは、認識されない場合があります)。 バーストのしきい値は、30000 (30 秒) よりも長くしないことをお勧めします。

既定では、Engagement サービスは、ネットワークが使用可能になると、すぐにサーバーとの接続を確立します。 接続を延期する場合はこれを追加 (間、 `< アプリケーション >` と `</アプリケーション >` タグ)。

            <meta-data android:name="engagement:connection:delay" android:value="<delay (in milliseconds)>"/>

既定では、セッションは最後のアクティビティが終了した後、10 秒経過した時点で終了します (アクティビティの終了は、通常は、[ホーム] または [戻る] キーを押す、電話をアイドル状態に設定する、または他のアプリケーションに移動することで発生します)。 これは、ユーザーがアプリケーションを終了した後、非常に短時間で戻ってくるたびにセッションが分割されることを避けるためです (このユーザーの行動は、画像の選択や通知の確認などを行うときに発生する可能性があります)。 このパラメーターを変更することができます。 これを行うには、これを追加 (間、 `< アプリケーション >` と `</アプリケーション >` タグ)。

            <meta-data android:name="engagement:sessionTimeout" android:value="<session timeout (in milliseconds)>"/>

## ログ レポートの無効化

### メソッド呼び出しを使用した場合

Engagement でログの送信を停止したい場合は、以下を呼び出します。

            EngagementAgent.getInstance(context).setEnabled(false);

この呼び出しは永続的であり、共有設定ファイルを使います。

この関数を呼び出したときに Engagement がアクティブの場合、サービスが停止するまで 1 分ほどかかることがあります。 ただし、次にアプリケーションが起動したときにサービスが起動することはありません。

ログ、同じ関数を呼び出すことで、もう一度レポートを有効にすることができます `true`します。

### 独自の統合 `PreferenceActivity`

この関数を呼び出す代わりに統合することもこの設定の既存の直接 `PreferenceActivity`します。

(目的のモード) の設定ファイルを使用する活動を構成するにはで、 `AndroidManifest.xml` ファイルと `アプリケーション メタデータ`:

-   `エンゲージメント: エージェント: 設定: 名` キーを使用して、共有設定ファイルの名前を定義します。
-   `エンゲージメント: エージェント: 設定: モード` キーを使用して、共有設定ファイルのモードを定義、として同じモードを使用する必要があります、 `PreferenceActivity`します。 モードは数値として渡す必要があります。コード内で定数フラグの組み合わせを使っている場合は、合計値を確認します。

常にエンゲージメントを使用して、 `エンゲージメント: キー` この設定を管理するための基本設定ファイル内のブール値のキー。

次の例の `AndroidManifest.xml` 既定値を示します。

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

追加できるよう、 `CheckBoxPreference` 次のようなを設定レイアウトにします。

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />


[device api]: http://go.microsoft.com/?linkid=9876094 

