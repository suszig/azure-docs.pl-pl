<properties
    pageTitle="Azure AD B2C プレビュー: Android アプリケーションからの Web API の呼び出し |Microsoft Azure"
    description="この記事が、アプリを作成する方法を示します "To-Do List" app that calls a node.js web API using OAuth 2.0 bearer tokens. Both the Android app and web api use Azure AD B2C to manage user identities and authenticate users."
    services="active-directory-b2c"
    documentationCenter="android"
    authors="brandwe"
    manager="msmbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="11/19/2015"
    ms.author="brandwe"/>


# Azure AD B2C プレビュー: Android アプリケーションからの Web API の呼び出し

Azure AD B2C を使用すると、強力なセルフサービス方式の ID 管理機能をわずかな手順で Android アプリと Web API に追加できます。 この記事では、OAuth 2.0 ベアラー トークンを使用して node.js Web API を呼び出す Android の "To-Do List" アプリを作成する方法を示します。 Android のアプリと web API の両方は、Azure AD B2C を使用して、ユーザー id を管理するには
ユーザーを認証します。

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

> [AZURE.NOTE]
    このクイック スタートを完全に機能させるためには、前提条件として、Azure AD で B2C によって保護されている Web API が存在する必要があります。 すぐに使用できる .Net と node.js 用の Web API が既にビルドされています。 このチュートリアルでは、node.js Web API のサンプルが構成されていると想定しています。 
    参照してください、 [Node.js チュートリアル Azure AD B2C Web API](active-directory-b2c-devquickstarts-api-node.md)します。


> [AZURE.NOTE]
    この記事では、Azure AD B2C でサインイン、サインアップ、プロファイルの管理を実装する方法については説明しません。 ユーザーが既に認証された後での Web API の呼び出しに焦点を合わせています。
    まだインストールしていない場合、これを最初にください、 [.NET Web アプリケーション チュートリアル入門](active-directory-b2c-devquickstarts-web-dotnet.md) に Azure AD B2C の基本について説明します。


保護されたリソースにアクセスする必要がある Android クライアントに対しては、Azure AD は Active Directory 認証ライブラリ (ADAL) を提供します。 ADAL の唯一の目的は、アプリがアクセス トークンを容易に取得できるようにすることです。 それがどれほど簡単であるかを示すために、ここで、次を実行する Android To-Do List アプリケーションを作成します。

-   アクセスを使用して To-Do List API を呼び出すためのトークンを取得、 [OAuth 2.0 認証プロトコル](https://msdn.microsoft.com/library/azure/dn645545.aspx)します。
-   ユーザーの To-Do List を取得します。
-   ユーザーのサインアウト処理を行います。



### 手順 1: Azure AD B2C ディレクトリの取得

Azure AD B2C を使用するには、ディレクトリ (つまり、テナント) を作成しておく必要があります。 ディレクトリは、ユーザー、アプリ、グループなどをすべて格納するためのコンテナーです。 ない場合
既に、やり [B2C ディレクトリを作成する](active-directory-b2c-get-started.md) に進む前にします。

### 手順 2: アプリケーションの作成

次に、B2C ディレクトリ内にアプリを作成する必要があります。これによって、 アプリと安全に通信するために必要ないくつかの情報が Azure AD に提供されます。 ここでは、アプリと Web API の両方が単一の**アプリケーション ID**で表されます。これは、アプリと Web API が 1 つの論理アプリを構成するためです。 アプリを作成するには
次の [手順](active-directory-b2c-app-registration.md)します。 このとき、

- アプリケーションに **Web アプリまたは Web API** を含めます。
- 入力 `urn: ietf:wg:oauth:2.0:oob` として、 **応答 URL** -このコード サンプルの既定の URL であります。
- アプリケーション用の**アプリケーション シークレット**を作成し、それをメモしておきます。 このプロジェクトはすぐに必要になります。
- アプリケーションに割り当てられた**アプリケーション ID** をメモしておきます。 こちらもすぐに必要になります。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

### 手順 3: ポリシーを作成する

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Azure AD の B2C によってすべてのユーザー エクスペリエンスを定義、 [* * * * ポリシー](active-directory-b2c-reference-policies.md)します。 このアプリケーションでは、3 種類を含む 
identity エクスペリエンス - サインアップ、サインイン、および Facebook でサインインします。 」の説明に従って、各種類の 1 つのポリシーを作成する必要が、 
[ポリシーのリファレンス資料](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)します。 3 つのポリシーを作成するときは、必ず以下の操作を行ってください。

- サインアップ ポリシーで、**表示名**と他のいくつかのサインアップ属性を選択します。
- すべてのポリシーで、アプリケーション クレームとして**表示名**と**オブジェクト ID** を選択します。 その他のクレームも選択できます。
- ポリシーの作成後、各ポリシーの**名前**をメモしておきます。 プレフィックスを持つ必要があります `b2c_1_`します。 これらのポリシー名はすぐに必要になります。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

3 つのポリシーの作成が正常に完了したら、いつでもアプリをビルドできます。

この記事では、作成したポリシーの使用方法については説明しません。 Azure AD の B2C でポリシーのしくみについて学習する場合
まず、 [.NET Web アプリケーション チュートリアル入門](active-directory-b2c-devquickstarts-web-dotnet.md)します。

### 手順 4: コードをダウンロードする

このチュートリアルのコードは保持 [github](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android)します。 移動するようにサンプルをビルドするには 
[.zip としてスケルトン プロジェクトをダウンロード](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/skeleton.zip) またはスケルトンを複製します。

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-Android.git
```

> [AZURE.NOTE] **スケルトンをダウンロードすることは、このチュートリアルを完了するために必要な作業です。**完全に機能するアプリケーションを Android 上に実装するのは複雑な作業であるため、**スケルトン**には、この後のチュートリアルを完了した後で実行される UX コードが含まれています。 これは、開発者の時間を短縮するための手段です。 UX コードは B2C を Android アプリケーションに追加するトピックと深い関わりはありません。

完成したアプリケーションも [.zip として利用可能な](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip) または、
`完了` 同じリポジトリの分岐します。


Maven を使用して構築するために、最上位レベルで pom.xml を使用することができます。


  * 手順に従って、 [maven android 用の前提条件セクション](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)
  * SDK 21 でエミュレーターをセットアップします。
  * リポジトリを複製したルート フォルダーに移動します。
  * コマンド mvn clean install を実行します。
  * cd samples\hello を実行して、ディレクトリをクイック スタート サンプルに変更します。
  * コマンド mvn android:deploy android:run を実行します。
  * アプリが起動します。
  * テスト ユーザー資格情報を入力して、試行します。

aar パッケージに加え、jar パッケージも送信されます。

### 手順 5: Android ADAL をダウンロードして Android Studio ワークスペースに追加する

このライブラリを Android プロジェクトで簡単に使用できるようにするために、複数のオプションが用意されています。

* ソース コードを使用して、このライブラリを Eclipse にインポートし、アプリケーションにリンクします。
* Android Studio を使用する場合は、*aar* パッケージ形式を使用して、バイナリを参照します。



#### オプション 1: Gradle 経由のバイナリ (推奨)

Maven 中央リポジトリからバイナリを取得できます。 AAR パッケージは AndroidStudio でプロジェクトに次のように含めることができます (例: で `build.gradle`)。

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:2.0.1-alpha') {
        exclude group: 'com.android.support'
    } // Recent version is 2.0.1-alpha
}
```

#### オプション 2. Maven 経由の aar

Eclipse で m2e プラグインを使用している場合は、内の依存関係を指定できます、 `pom.xml` ファイル。

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>2.0.1-alpha</version>
    <type>aar</type>
</dependency>
```

#### オプション 3: Git 経由のソース (最終手段として)

SDK のソース コードを Git 経由で取得するには、次のように入力します。

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src
    
    use the branch "convergence"

### 手順 6: 構成ファイルを設定する

先ほど B2C ポータルでセットアップした構成を使用して、Android プロジェクトを構成します。

開いている `helpes/Constants.java` し、次の値を入力します。

```

package com.microsoft.aad.taskapplication.helpers;

import com.microsoft.aad.adal.AuthenticationResult;

public class Constants {

    public static final String SDK_VERSION = "1.0";
    public static final String UTF8_ENCODING = "UTF-8";
    public static final String HEADER_AUTHORIZATION = "Authorization";
    public static final String HEADER_AUTHORIZATION_VALUE_PREFIX = "Bearer ";

    // -------------------------------AAD
    // PARAMETERS----------------------------------
    public static String AUTHORITY_URL = "https://login.microsoftonline.com/hypercubeb2c.onmicrosoft.com/";
    public static String CLIENT_ID = "<your application id>";
    public static String[] SCOPES = {"<your application id>"};
    public static String[] ADDITIONAL_SCOPES = {""};
    public static String REDIRECT_URL = "<redirect uri>";
    public static String CORRELATION_ID = "";
    public static String USER_HINT = "";
    public static String EXTRA_QP = "";
    public static String FB_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNIN_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNUP_POLICY = "B2C_1_<your policy>";
    public static boolean FULL_SCREEN = true;
    public static AuthenticationResult CURRENT_RESULT = null;
    // Endpoint we are targeting for the deployed WebAPI service
    public static String SERVICE_URL = "http://localhost:3000/tasks";

    // ------------------------------------------------------------------------------------------

    static final String TABLE_WORKITEM = "WorkItem";
    public static final String SHARED_PREFERENCE_NAME = "com.example.com.test.settings";


}
```
**SCOPES** - ユーザーがログインしているサーバーから要求するサーバーに渡すスコープです。 B2C プレビューでは client_id を渡します。 ただし、これは、読み取りスコープに代わる予定です。 このドキュメントはその時点で更新されます。
**ADDITIONAL_SCOPES** - アプリケーションで使用できるその他のスコープです。 今後この使用されます。
**CLIENT_ID** -ポータルから取得したアプリケーション ID
**REDIRECT_URL** -ポストバックされますトークン考えてリダイレクトします。
**EXTRA_QP** - URL エンコード形式でサーバーに渡す追加のパラメーター。
**FB_POLICY** -呼び出すポリシー。 このチュートリアルで最も重要な部分です。
**EMAIL_SIGNIN_POLICY** - 呼び出すポリシー。 このチュートリアルで最も重要な部分です。
**EMAIL_SIGNUP_POLICY** - 呼び出すポリシー。 このチュートリアルで最も重要な部分です。

### 手順 7: プロジェクトに Android ADAL への参照を追加する

> [AZURE.NOTE]  ADAL for Android は、インテント ベースのモデルを使用して認証を呼び出します。 インテントは、アプリの橋渡し的役割を果たします。 このサンプルでは (ADAL for Android の使用全般に言えることですが) インテントを利用し、アプリ間で情報を受け渡ししています。 


最初に、使用する Intents() を含め、対象アプリケーションのレイアウトを Android に伝える必要があります。 これらのインテントについては後で詳しく説明します。

プロジェクトの AndroidManifest.xml ファイルを更新して、必要なインテントをすべて含めます。

```
   <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.microsoft.aad.taskapplication"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="11"
        android:targetSdkVersion="19" />

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.microsoft.aad.adal.AuthenticationActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:exported="true"
            android:label="@string/title_login_hello_app" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.LoginActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.UsersListActivity"
            android:label="@string/title_activity_feed" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.SettingsActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.AddTaskActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.ToDoActivity"
            android:label="@string/app_name" >
        </activity>
    </application>

</manifest>    
```

ここでは 5 つのアクティビティを使用します。ご覧のとおり、次のアクティビティが定義されています。

**AuthenticationActivity** - ADAL によって提供されるアクティビティです。ログインの Webview として機能します。

**LoginActivity** - サインイン ポリシーと各ポリシーのボタンを表示します。

**SettingsActivity** - アプリの設定を実行時に変更するために必要となります。

**AddTaskActivity** - Azure AD によって保護された REST API にタスクを追加するために必要となります。

**ToDoActivity** - タスクを表示するメイン アクティビティです。



### 手順 8: ログイン アクティビティを作成する

それではメインのアクティビティを作成し、それを呼び出す `LoginActivity`します。

という名前のファイルを作成する `LoginActivity.java`

アクティビティを初期化し、UI を制御するボタンをいくつか追加する必要があります。 この作業も非常に単純であり、Android コードの記述経験があれば馴染みのあるものです。

```
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;

/**
 * Created by brwerner on 9/17/15.
 */
public class LoginActivity extends Activity {

    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signin);
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        Button button = (Button) findViewById(R.id.signin_facebook);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.FB_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signin_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNIN_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signup_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNUP_POLICY);
                startActivity(intent);

            }
        });

    }

}
```
ここで作成しているのは、(トークンが必要になったときに ADAL を呼び出す) ToDoActivity インテントを呼び出すボタンです。独自のアクティビティを参照として指定したうえで、追加パラメーターを指定しています。 この余分なパラメーターを渡し、 `intent.putExtra()` メソッドです。 ここに表示される"thePolicy"で指定したものとして定義 `Constants.java`します。 インテントが認証時に呼び出すポリシーを把握するために必要となります。

### 手順 9: Settings アクティビティを作成する

これは、Settings UI の内容を作成するアクティビティです。

という名前のファイルを作成する `SettingsActivity.java`

設定の読み取りや保存など単純な処理を行っています。

```
 package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.microsoft.aad.taskapplication.helpers.Constants;

/**
 * Settings page to try broker by options
 */
public class SettingsActivity extends Activity {

    //private CheckBox checkboxAskBroker, checkboxCheckBroker;
    private Switch fullScreenSwitch;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        loadSettings();
//      checkboxAskBroker = (CheckBox) findViewById(R.id.askInstall);
//      checkboxCheckBroker = (CheckBox) findViewById(R.id.useBroker);

        Button save = (Button) findViewById(R.id.settingsSave);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView textView = (TextView)findViewById(R.id.authority);
                Constants.AUTHORITY_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.resource);
            //    Constants.SCOPES = textView.getText().toString();

                textView = (TextView)findViewById(R.id.clientId);
                Constants.CLIENT_ID = textView.getText().toString();

                textView = (TextView)findViewById(R.id.extraQueryParameters);
                Constants.EXTRA_QP = textView.getText().toString();

                textView = (TextView)findViewById(R.id.redirectUri);
                Constants.REDIRECT_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                Constants.SERVICE_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                textView.setText(Constants.SERVICE_URL);

                textView = (TextView)findViewById(R.id.fb_signin);
                textView.setText(Constants.FB_POLICY);

                textView = (TextView)findViewById(R.id.email_signin);
                textView.setText(Constants.EMAIL_SIGNIN_POLICY);

                textView = (TextView)findViewById(R.id.email_signup);
                textView.setText(Constants.EMAIL_SIGNUP_POLICY);

                textView = (TextView)findViewById(R.id.correlationId);
                textView.setText(Constants.CORRELATION_ID);

                fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
                Constants.FULL_SCREEN = fullScreenSwitch.isChecked();

                finish();
            }
        });


    }

    private void loadSettings() {
        TextView textView = (TextView)findViewById(R.id.authority);
        textView.setText(Constants.AUTHORITY_URL);
        textView = (TextView)findViewById(R.id.resource);
        textView.setText(Constants.SCOPES[0]);
        textView = (TextView)findViewById(R.id.clientId);
        textView.setText(Constants.CLIENT_ID);
        textView = (TextView)findViewById(R.id.extraQueryParameters);
        textView.setText(Constants.EXTRA_QP);
        textView = (TextView)findViewById(R.id.redirectUri);
        textView.setText(Constants.REDIRECT_URL);
        textView = (TextView)findViewById(R.id.serviceUrl);
        textView.setText(Constants.SERVICE_URL);
        textView = (TextView)findViewById(R.id.fb_signin);
        textView.setText(Constants.FB_POLICY);
        textView = (TextView)findViewById(R.id.email_signin);
        textView.setText(Constants.EMAIL_SIGNIN_POLICY);
        textView = (TextView)findViewById(R.id.email_signup);
        textView.setText(Constants.EMAIL_SIGNUP_POLICY);
        textView = (TextView)findViewById(R.id.correlationId);
        textView.setText(Constants.CORRELATION_ID);

        fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
        fullScreenSwitch.setChecked(Constants.FULL_SCREEN);
    }

    private void saveSettings(String key, boolean value) {
        SharedPreferences prefs = SettingsActivity.this.getSharedPreferences(
                Constants.SHARED_PREFERENCE_NAME, Activity.MODE_PRIVATE);
        Editor prefsEditor = prefs.edit();
        prefsEditor.putBoolean(key, value);
        prefsEditor.commit();
    }
}
```

### 手順 10: AddTask アクティビティを作成する

このアクティビティは、REST API エンドポイントにタスクを追加する目的で使用されます。 これも非常に単純です。

という名前のファイルを作成する `AddTaskActivity.java`

次のコードを記述します。

```
package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;

public class AddTaskActivity extends Activity {

    EditText textField;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_task);
        textField = (EditText) findViewById(R.id.taskToAdd);
        Button button = (Button) findViewById(R.id.postTaskbutton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (textField.getText().toString() != null
                        && !textField.getText().toString().trim().isEmpty()
                        && Constants.CURRENT_RESULT != null) {

                    TodoListHttpService service = new TodoListHttpService();
                    try {
                        service.addItem(textField.getText().toString(), Constants.CURRENT_RESULT.getAccessToken());
                    } catch (Exception e) {
                        SimpleAlertDialog.showAlertDialog(getApplicationContext(), "Exception caught", e.getMessage());
                    }
                    finish();
                }
            }
        });
    }

}
```

### 手順 11: ToDoList アクティビティを作成する

いよいよ最も重要なアクティビティについて取り上げます。このアクティビティによって、ポリシーに対応するトークンを Azure AD から取得し、そのトークンを使用して Task REST API サーバーを呼び出すことができます。

という名前のファイルを作成する `ToDoActivity.java`

次のコードを記述します (各呼び出しについては後で説明します)。

```

package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationCallback;
import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.adal.AuthenticationResult;
import com.microsoft.aad.adal.AuthenticationSettings;
import com.microsoft.aad.adal.UserIdentifier;
import com.microsoft.aad.adal.PromptBehavior;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.InMemoryCacheStore;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;
import com.microsoft.aad.taskapplication.helpers.Utils;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;


import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ToDoActivity extends Activity {



    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;
    private static AuthenticationResult sResult;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;


    /**
     * Initializes the activity
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_todo_items);
        CookieSyncManager.createInstance(getApplicationContext());
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        // Clear previous sessions
        clearSessionCookie();
        try {
            // Provide key info for Encryption
            if (Build.VERSION.SDK_INT < 18) {
                Utils.setupKeyForSample();
            }

        Button button = (Button) findViewById(R.id.addTaskButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, AddTaskActivity.class);
                startActivity(intent);
            }
        });

        button = (Button) findViewById(R.id.appSettingsButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, SettingsActivity.class);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.switchUserButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, LoginActivity.class);
                startActivity(intent);

            }
        });


        final TextView name = (TextView)findViewById(R.id.userLoggedIn);


        mLoginProgressDialog = new ProgressDialog(this);
        mLoginProgressDialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        mLoginProgressDialog.setMessage("Login in progress...");
        mLoginProgressDialog.show();
        // Ask for token and provide callback
        try {
            mAuthContext = new AuthenticationContext(ToDoActivity.this, Constants.AUTHORITY_URL,
                    false);
            String policy = getIntent().getStringExtra("thePolicy");

            if(Constants.CORRELATION_ID != null &&
                    Constants.CORRELATION_ID.trim().length() !=0){
                mAuthContext.setRequestCorrelationId(UUID.fromString(Constants.CORRELATION_ID));
            }

            AuthenticationSettings.INSTANCE.setSkipBroker(true);

            mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES, policy, Constants.CLIENT_ID,
                    Constants.REDIRECT_URL, getUserInfo(), PromptBehavior.Always,
                    "nux=1&" + Constants.EXTRA_QP,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onError(Exception exc) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }
                            SimpleAlertDialog.showAlertDialog(ToDoActivity.this,
                                    "Failed to get token", exc.getMessage());
                        }

                        @Override
                        public void onSuccess(AuthenticationResult result) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }

                            if (result != null && !result.getToken().isEmpty()) {
                                setLocalToken(result);
                                updateLoggedInUser();
                                getTasks();
                                ToDoActivity.sResult = result;
                                Toast.makeText(getApplicationContext(), "Token is returned", Toast.LENGTH_SHORT)
                                        .show();

                                if (sResult.getUserInfo() != null) {
                                    name.setText(result.getUserInfo().getDisplayableId());
                                    Toast.makeText(getApplicationContext(),
                                            "User:" + sResult.getUserInfo().getDisplayableId(), Toast.LENGTH_SHORT)
                                            .show();
                                }
                            } else {
                                //TODO: popup error alert
                            }
                        }
                    });
        } catch (Exception e) {
            SimpleAlertDialog.showAlertDialog(ToDoActivity.this, "Exception caught", e.getMessage());
        }
        Toast.makeText(ToDoActivity.this, TAG + "done", Toast.LENGTH_SHORT).show();
    } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }}
```


 作成されていないメソッドに対してまだなど、これは依存することがあります `updateLoggedInUser()`, 、`clearSessionCookie()` と `getTasks()`します。 それらのメソッドは、この後で作成します。 現時点では Android Studio でエラーが発生しますが、それらは無視してかまいません。

パラメーターの説明:

  * *** スコープ *** へのアクセスを要求しようとしているスコープが必要です。 B2C プレビューの時点では Clientid と同じですが、将来変更される予定です。
  * *** ポリシー *** のポリシーは、ユーザーを認証する方が適しています。
  * *** CLIENT_ID *** が必要とは、azure Ad ポータルから取得します。
  * redirectUri はパッケージ名として設定できます。 acquireToken 呼び出しの場合は、必須ではありません。
  * *** getUserInfo() *** 方法の場合、ユーザーが既にキャッシュ内を検索し、存在しない場合、ユーザーの入力を求めますか、アクセス トークンが無効です。 このメソッドは後で作成します。
  * ***PromptBehavior.always*** キャッシュと cookie をスキップする資格情報を要求することができます。
  * *** コールバック *** トークンの認証コードを交換した後に呼び出されます。

  callback はオブジェクト AuthenticationResult を返します。これには accesstoken、期限切れの日付、idtoken の情報が含まれています。

> [AZURE.NOTE]  Microsoft Intune のポータル サイト アプリでは、ブローカー コンポーネントを提供しています。そのアプリがユーザーのデバイスにインストールされている可能性があります。 それによってデバイス上のすべてのアプリケーションに SSO が提供されるため、その点を見越した開発を行うようお勧めします。 ADAL for Android は、Authenticator で作成されたユーザー アカウントが 1 つ存在する場合、ブローカー アカウントを使用します。 ブローカーを使用するには、開発者がブローカー用に特殊な redirectUri を登録する必要があります。 RedirectUri は msauth://packagename/Base64UrlencodedSignature の形式です。 スクリプトを使用して、アプリの redirecturi を取得できる `brokerRedirectPrint.ps1` API 呼び出しを使用して、または `mContext.getBrokerRedirectUri()`します。 署名は、Google Play ストアからの署名証明書に関連付けられています。

 ブローカー ユーザーをスキップするには、次のようにコードを記述します。

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```

> [AZURE.NOTE] この B2C クイック スタートのサンプル コードは簡潔にするために、ブローカーをスキップしています。

次に、いくつかのヘルパー メソッドを作成します。Task API の認証メソッドを呼び出すとき、トークンの取得だけは、これらのヘルパー メソッドで行います。

**同じファイルで** と呼ばれる `ToDoActivity.java`

```
    private void getToken(final AuthenticationCallback callback) {

        String policy = getIntent().getStringExtra("thePolicy");

        // one of the acquireToken overloads
        mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES,
                policy, Constants.CLIENT_ID, Constants.REDIRECT_URL, getUserInfo(),
                PromptBehavior.Always, "nux=1&" + Constants.EXTRA_QP, callback);
    }
```

さらに、目的のトークンが格納される AuthenticationResult をグローバル定数に設定するメソッドと、グローバル定数から取得するメソッドを追加します。 これが必要なためにもかかわらず `ToDoActivity.java` を使用して **sResult** では、フローは、他のアクティビティが動作するトークンへのアクセスを必要はありません (などのタスクを追加、 `AddTaskActivity.java`)

```

    private AuthenticationResult getLocalToken() {
        return Constants.CURRENT_RESULT;
    }

    private void setLocalToken(AuthenticationResult newToken) {


        Constants.CURRENT_RESULT = newToken;
    }
```
### 手順 12: UserIdentifier を取得するメソッドを作成する

ADAL for Android は、**UserIdentifier** オブジェクトの形式でユーザーを表します。 このようにユーザーを管理することで、呼び出しに使用されているのが同じユーザーであるかどうかを把握し、キャッシュを利用するか、新たな呼び出しをサーバーに対して行うかの判断を行うことができます。 簡単に作成、作成、 `getUserInfo()` で使用する UserIdentifier 返す `acquireToken()`します。 さらに、キャッシュに格納されている UserIdentifier の ID を効率よく取得する getUniqueId() メソッドを作成します。

```
  private String getUniqueId() {
        if (sResult != null && sResult.getUserInfo() != null
                && sResult.getUserInfo().getUniqueId() != null) {
            return sResult.getUserInfo().getUniqueId();
        }

        return null;
    }

    private UserIdentifier getUserInfo() {

        final TextView names = (TextView)findViewById(R.id.userLoggedIn);
        String name = names.getText().toString();
        return new UserIdentifier(name, UserIdentifier.UserIdentifierType.OptionalDisplayableId);
    }
```

### 手順 13: ヘルパー メソッドを作成する

Cookie をクリアしたり、AuthenticationCallback を実装したりするためのヘルパー メソッドを作成する必要があります。 これらは、ToDo アクティビティを呼び出す際にクリーンな状態を形成する目的で、このサンプルでのみ使用されます。

**同じファイルで** と呼ばれる `ToDoActivity.java`

```

    private void clearSessionCookie() {

        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.removeSessionCookie();
        CookieSyncManager.getInstance().sync();
    }
```

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }
```

### 手順 14: Task API を呼び出す

必要な Activity はこれで完成です。あとは肝心なトークンを取得する必要があります。タスク サーバーにアクセスするための API を作成しましょう。

当社 `getTasks` サーバー内のタスクを表す配列を提供します。

記述を始めましょう、 `getTask` 最初。

**同じファイルで** と呼ばれる `ToDoActivity.java`

```
    private void getTasks() {
        if (sResult == null || sResult.getToken().isEmpty())
            return;

        List<String> items = new ArrayList<>();
        try {
            items = new TodoListHttpService().getAllItems(sResult.getToken());
        } catch (Exception e) {
            items = new ArrayList<>();
        }

        ListView listview = (ListView) findViewById(R.id.listViewToDo);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1, android.R.id.text1, items);
        listview.setAdapter(adapter);
    }
```



 初回実行時にテーブルを初期化するメソッドも作成しましょう。

 **同じファイルで** と呼ばれる `ToDoActivity.java`

```
    private void initAppTables() {
        try {
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

        } catch (Exception e) {
            createAndShowDialog(new Exception(
                    "There was an error creating the Mobile Service. Verify the URL"), "Error");
        }
    }
```

 このコードが機能するためには、他にもいくつかのメソッドが必要になります。 それを次に記述しましょう。

### エンドポイント URL ジェネレーターを作成する

 接続先となるエンドポイント URL を生成する必要があります。 同じクラス ファイルでこの処理を行うことにしましょう。

 **同じファイルで** と呼ばれる `ToDoActivity.java`

 ```
    private URL getEndpointUrl() {
        URL endpoint = null;
        try {
            endpoint = new URL(Constants.SERVICE_URL);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        return endpoint;
    }
 ```


以降のコードで要求にアクセス トークンを追加します。

### 手順 15:UX メソッドを作成する

Android でアプリを動作させるためには、いくつかのコールバックを処理する必要があります。 これらは `createAndShowDialog` と `onResume()`します。 この作業は非常に単純であり、Android コードの記述経験があれば馴染みのあるものです。

では、実際に記述してみましょう。

**同じファイルで** と呼ばれる `ToDoActivity.java`

```
    @Override
    public void onResume() {
        super.onResume(); // Always call the superclass method first

        updateLoggedInUser();
        // User can click logout, it will come back here
        // It should refresh list again
        getTasks();
    }
```

今度は、ダイアログ コールバックの処理です。

**同じファイルで** と呼ばれる `ToDoActivity.java`


```
    /**
     * Creates a dialog and shows it
     *
     * @param exception The exception to show in the dialog
     * @param title     The dialog title
     */
    private void createAndShowDialog(Exception exception, String title) {
        createAndShowDialog(exception.toString(), title);
    }

    /**
     * Creates a dialog and shows it
     *
     * @param message The dialog message
     * @param title   The dialog title
     */
    private void createAndShowDialog(String message, String title) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setMessage(message);
        builder.setTitle(title);
        builder.create().show();
    }
```

これで完了です。 必要、 `ToDoActivity.java` ファイルをコンパイルします。 この時点で、プロジェクト全体も問題なくコンパイルできます。



### 手順 16: サンプル アプリを実行する

最後に、構築したアプリを Android Studio と Eclipse の両方で実行します。 アプリにサインアップまたはサインインし、サインインしているユーザーのタスクを作成します。 サインアウトして、別のユーザーとしてもう一度サインインし、そのユーザーのタスクを作成します。

API でタスクがユーザーごとに保存されたことを確認します。これは、API が、受信したアクセス トークンからユーザーID を抽出したためです。

リファレンスについては、完全なサンプルは [.zip をここでは現状](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip),、
または、GitHub からプロジェクトを複製することができます。

`git クローン--ブランチの完全な https://github.com/AzureADQuickStarts/B2C-NativeClient-Android`


### 重要な情報

#### 暗号化

ADAL は、既定では SharedPreferences のトークンとストアを暗号化します。 詳細を確認するには、StorageHelper クラスを参照できます。 Android は、秘密キーのセキュリティ保護された記憶域として AndroidKeyStore for 4.3(API18) を導入しています。 ADAL はこの API18 以降を使用します。 下位バージョンの SDK に ADAL を使用する場合は、AuthenticationSettings.INSTANCE.setSecretKey で秘密キーを提供する必要があります。

#### Webview のセッション Cookie

Android Webview は、アプリを閉じた後、セッションの Cookie をクリアしません。 これは、次のサンプル コードを処理できます。
```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
Cookie の詳細: http://developer.android.com/reference/android/webkit/CookieSyncManager.html





