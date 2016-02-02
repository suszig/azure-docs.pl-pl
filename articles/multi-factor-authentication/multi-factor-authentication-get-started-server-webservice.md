<properties 
    pageTitle="MFA Server モバイル アプリ Web サービスの概要" 
    description="Azure Multi-Factor Authentication アプリでは、他にもアウトオブバンドの認証オプションが提供されています。これにより MFA Server で、ユーザーへのプッシュ通知を許可できます。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/01/2015" 
    ms.author="billmath"/>


# MFA Server モバイル アプリ Web サービスの概要

Azure Multi-Factor Authentication アプリでは、他にもアウトオブバンドの認証オプションが提供されています。 Azure Multi-Factor Authentication は、ログインするときにユーザーに自動音声通話または SMS 送信を行うのではなく、ユーザーのスマート フォンまたはタブレットの Azure Multi-Factor Authentication アプリにプッシュ通知を行います。 ユーザーはアプリで [認証] をタップ (または PIN を入力し、[認証] をタップ) するだけでログインできます。

Azure Multi-Factor Authentication アプリを使用するには、アプリがモバイル アプリ Web サービスと正常に通信できるように以下が必要です。

- ハードウェアおよびソフトウェアの要件については、「ハードウェアとソフトウェアの要件」を参照してください。
- Azure Multi-Factor Authentication Server の V6.0 以降を使用している必要があります
- モバイル アプリ Web サービスが、Microsoft® Internet Information Services (IIS) 6.x または IIS 7.x を実行している、インターネットに接続された Web サーバーにインストールされている必要があります
- IIS 6.x を使用している場合は、ASP.NET v2.0.50727 がインストールおよび登録され、[許可] に設定されるようにします
- IIS 7.x を使用するときに必要なロール サービスには、ASP.NET および IIS 6 メタベース互換が含まれます
- モバイル アプリ Web サービスにパブリック URL よりアクセスできる必要があります
- モバイル アプリ Web サービスは、SSL 証明書で保護されている必要があります。
- Azure Multi-Factor Authentication Web サービス SDK が、IIS 6.x または IIS 7.x にインストールされている必要があります
- Azure Multi-Factor Authentication Web サービス SDK は、SSL 証明書で保護されている必要があります。
- モバイル アプリ Web サービスは SSL 経由で Azure Multi-Factor Authentication Web サービス SDK に接続できる必要があります
- モバイル アプリ Web サービスは、「PhoneFactor Admins」と呼ばれるセキュリティ グループのメンバーであるサービス アカウントの資格情報を使用して Azure Multi-Factor Authentication Web サービス SDK で認証できる必要があります。 このサービス アカウントとグループは、Azure Multi-Factor Authentication Server がドメインに参加しているサーバーで実行されている場合は、Active Directory に存在します。 このサービス アカウントとグループは、Azure Multi-Factor Authentication Server がドメインに参加していない場合、Azure Multi-Factor Authentication Server のローカルに存在します。


Azure Multi-Factor Authentication Server 以外のサーバーでユーザー ポータルをインストールするには、次の 3 つの手順が必要です。

1. Web サービス SDK のインストール
2. モバイル アプリ Web サービスのインストール
3. Azure Multi-Factor Authentication Server でモバイル アプリ設定を構成する
4. エンドユーザー向けに Azure Multi-Factor Authentication アプリをアクティブ化します

## Web サービス SDK のインストール

Azure Multi-Factor Authentication Server に Azure Multi-Factor Authentication Web サービス SDK がまだインストールされていない場合、そのサーバーに移動し、Azure Multi-Factor Authentication Server を開きます。 Web サービス SDK] アイコンをクリックし、[Web サービス SDK のインストール] をクリックしてください. ボタンをクリックし、指示に従って操作します。 Web サービス SDK は、SSL 証明書で保護されている必要があります。 この目的で自己署名証明書を使用できますが、ユーザー ポータル Web サーバー上のローカル コンピューター アカウントの「信頼されたルート証明機関」ストアにインポートすることで、SSL 接続を開始するときにこの証明書が信頼されます。

<center>![セットアップ](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## モバイル アプリ Web サービスのインストール

モバイル アプリ Web サービスをインストールする前に、次の点に留意してください。

- Azure Multi-Factor Authentication ユーザー ポータルが、インターネットに接続されたサーバー上に既にインストールされている場合、Web サービス SDK のユーザー名、パスワード、および URL は、ユーザー ポータルの web.config ファイルからコピーできます。
- インターネットに接続された Web サーバーで Web ブラウザーを開き、web.config ファイルに入力された Web サービス SDK の URL に移動することをお勧めします。 ブラウザーが Web サービスを正常に取得できる場合、資格情報が求められます。 web.config ファイルに入力したユーザー名とパスワードを、ファイルに表示されているとおりに入力します。 証明書の警告およびエラーが表示されないことを確認してください。
- リバース プロキシまたはファイアウォールがモバイル アプリ Web サービス web サーバーの前に座っていると、SSL オフロードを実行するには、モバイル アプリ Web サービスの web.config ファイルを編集して次のキーを追加する場合、 <appSettings> セクション モバイル アプリ Web サービスが https ではなく http を使用できるようにします。ただし SSL は、ファイアウォール/リバース プロキシへのモバイル アプリからが必要です。 <add key="SSL_REQUIRED" value="false"/>

### モバイル アプリ Web サービスをインストールするには

<ol>
<li>Azure Multi-factor Authentication Server に Windows エクスプ ローラーを開き、Azure Multi-factor Authentication Server がインストールされているフォルダーに移動します (例: C:\Program \azure Multi-factor Authentication)。モバイル アプリ Web サービスをインストールするサーバーに対し、必要に応じて Azure Multi-Factor AuthenticationPhoneAppWebServiceSetup インストール ファイルの 32 ビットまたは 64 ビット バージョンを選択します。インストール ファイルをインターネットに接続するサーバーにコピーします。</li>

<li>インターネットに接続された web サーバー上には、管理者権限を持つセットアップ ファイルを実行する必要があります。これを行う最も簡単な方法が、管理者としてコマンド プロンプトを開き、インストール ファイルのコピー先の場所に移動します。</li>

<li>多要素 AuthenticationMobileAppWebServiceSetup インストール ファイルを実行、必要に応じて、サイトを変更および仮想ディレクトリを「PA」などの短い名前に変更します。ユーザーはアクティブ化時に、モバイル デバイスにモバイル アプリ Web サービス URL を入力する必要がありますのでは、短い仮想ディレクトリ名を使用することをお勧めします。</li>

<li>Azure 多要素 AuthenticationMobileAppWebServiceSetup のインストールが終わったら、C:\inetpub\wwwroot\PA (または仮想ディレクトリ名に基づく適切なディレクトリ) を参照し、web.config ファイルを編集します。</li>

<li>WEB_SERVICE_SDK_AUTHENTICATION_USERNAME と WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD キーを検索し、ユーザー名とは、PhoneFactor Admins セキュリティのメンバーであるサービス アカウントのパスワードに値を設定 (前のセクションの要件を参照してください) をグループ化します。既にインストールされている場合は、Azure Multi-Factor Authentication ユーザー ポータルの ID として使用されるのと同じアカウントである可能性があります。行末尾 (value=””/>) の引用符の間にユーザー名およびパスワードを入力するようにしてください。修飾されたユーザー名 (例: domain \username または machine \username など) を使用することをお勧めします。</li>

<li>PfMobile App Web Service_pfwssdk_PfWsSdk 設定を見つけて、値を"http://localhost:4898/PfWsSdk.asmx"から Azure 多要素認証サーバー (https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx など) で実行されている Web サービス SDK の URL に変更します。この接続では SSL が使用されているため、IP アドレスではなくサーバー名で Web サービス SDK を参照する必要があります。これは、SSL 証明書がサーバー名に対して発行され、使用される URL が証明書の名前に一致する必要があるためです。サーバー名がインターネットに接続されているサーバーで IP アドレスに解決されない場合、そのサーバーの hosts ファイルにエントリを追加し、Azure Multi-Factor Authentication Server の名前を IP アドレスにマッピングします。変更が加えられた後は、web.config ファイルを保存します。</li>

<li>モバイル アプリ Web サービスの [(Default Web Site など) がインストールされている web サイトまだ公的署名証明書にバインドされていない、証明書をサーバーにインストールされていない場合は、IIS マネージャーを起動し、web サイトに証明書をバインドします。</li>

<li>任意のコンピューターから web ブラウザーを開き、モバイル アプリ Web サービスがインストールされている URL に移動します (例: https://www.publicwebsite.com/PA)。証明書の警告またはエラーが表示されないことを確認します。</li>

### Azure Multi-Factor Authentication Server でモバイル アプリ設定を構成する

モバイル アプリ Web サービスがインストールされたため、ポータルから Azure Multi-Factor Authentication Server を構成する必要があります。

#### Azure Multi-Factor Authentication Server のモバイル アプリ設定を構成するには

1. Azure Multi-Factor Authentication Server で [ユーザー ポータル] アイコンをクリックします。 ユーザーによる認証方法の制御が許可される場合は、[設定] タブの [ユーザーに認証方法の選択を許可する] の下にある [モバイル アプリ] を確認します。 この機能が有効になっていないと、エンドユーザーはモバイル アプリのアクティブ化を完了するために、ヘルプ デスクに連絡する必要があります。
2. [ユーザーにモバイル アプリのアクティブ化を許可する] ボックスを確認します。
3. [ユーザー登録を許可する] ボックスを確認します。
4. [モバイル アプリ] アイコンをクリックします。
5. Azure Multi-Factor AuthenticationMobileAppWebServiceSetup をインストールするときに作成された仮想ディレクトリで使用される URL を入力します。 所定の場所にアカウント名を入力することがあります。 この会社名はモバイル アプリケーションに表示されます。 空欄のままにすると、Azure 管理ポータルで作成された Multi-Factor Auth Provider の名前が表示されます。



<center>![セットアップ](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>





