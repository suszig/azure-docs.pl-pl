<properties 
    pageTitle="SaaS コネクタ API アプリのデプロイおよび構成" 
    description="Azure Marketplace から Azure のサブスクリプションにインストールした SaaS コネクタを構成する方法について説明します。" 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/03/2015" 
    ms.author="tdykstra"/>

# Azure App Service での SaaS コネクタ API アプリのデプロイおよび構成

[AZURE.INCLUDE [app-service-api-v2-note](../../includes/app-service-api-v2-note.md)]

## 概要

このチュートリアルは、インストール、構成、およびテストする方法を示しています、 [ソフトウェアとしてのサービス (SaaS) コネクタ](../app-service-logic-what-are-bizTalk-api-apps.md) で [Azure App Service](/documentation/services/app-service/) プログラムを使用して、その呼び出しのため、モバイル アプリなどからです。 SaaS コネクタは、 [API アプリ](app-service-api-apps-why-best-platform.md) Office 365、Salesforce、Facebook、Dropbox などの SaaS プラットフォームとの対話を簡素化します。 カスタム .NET API アプリを作成する、事前にパッケージされたコネクタの表示を使用せずに場合 [ASP.NET API アプリから SaaS プラットフォームへの接続](app-service-api-dotnet-connect-to-saas.md)します。 

たとえば、Dropbox アカウント内のファイルを読み書きするために HTTP 要求をコーディングする必要がある場合、Dropbox を直接操作するための認証プロセスは複雑になります。 Dropbox コネクタを使用すれば複雑な認証の処理を避けることができるため、業務固有のコードの作成に集中できます。

> [AZURE.NOTE] ロジック アプリから SaaS コネクタを使用する場合、ここに示す手順は必要ありません。 ロジック アプリで SaaS コネクタを使用する方法については、次を参照してください。 [新しいロジック アプリの作成](../app-service-logic/app-service-logic-create-a-logic-app.md) と [コネクタでカスタムの OAUTH アプリケーションを使用して](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps&announcementId=4af1e4c5-d220-4457-97d8-d08e427ae6c1)します。
 
このチュートリアルでは例として DropBox コネクタを使用し、次の手順を順番に説明します。

* Dropbox コネクタをインストール、 [リソース グループ](../resource-group-overview.md) 、Azure サブスクリプションにします。 
* Dropbox サービスに接続できるように、Dropbox コネクタを構成します  (この手順を実行するには、Dropbox アカウントが必要になります)。
* 認証されたユーザーのみがリソース グループに含まれている API アプリにアクセスできるように、リソース グループを構成します。
* ユーザー認証と Dropbox 認証の両方が機能することをテストして確認します。

App Service での認証についての詳細については、次を参照してください。 [API apps と mobile apps の認証](../app-service/app-service-authentication-overview.md)します。 

## Dropbox コネクタのインストール

1. 移動して、 [Azure preview portal] のホーム ページおよびクリック **Marketplace**します。

    ![Azure プレビュー ポータルの Marketplace](./media/app-service-api-connnect-your-app-to-saas-connector/marketplace.png)

2. Dropbox を検索し、 **Dropbox コネクタ** アイコン。

    ![Dropbox コネクタのクリック](./media/app-service-api-connnect-your-app-to-saas-connector/searchdb.png)
 
3. クリックして **作成**します。

    ![Click Create](./media/app-service-api-connnect-your-app-to-saas-connector/clickcreate.png)
 
5.  **Dropbox コネクタ** ブレードで、 **App Service プラン** ] をクリックして **新規作成**, 、し、次に、 **App Service プランの新規作成** ボックスに「dropboxplan」と入力します。 

    App Service プランに関する詳細については、次を参照してください。 [Azure App Service プランの詳細な概要](azure-web-sites-web-hosting-plans-in-depth-overview.md)します。 

4. [ **リソース グループ**, 、] をクリックして **新規作成**, 、し、次に、 **新しいリソース グループの作成** ボックス DropboxRG」と入力します。

    リソース グループの詳細については、次を参照してください。 [リソース グループを使用した Azure リソースの管理に](../resource-group-overview.md)します。

7. 選択、無料 **価格レベル**します。 (一覧に表示されない、クリックして **すべて表示**します。 をクリックした後 **F1 Free**, をクリックして、 **選択** ] ボタンをクリックします)。

    有料の料金レベルを使用できますが、このチュートリアルでは必要ありません。
 
11. 選択、 **場所** しやすい。  

9. <a id="gateway"></a>Dropboxconnector"既定"、 **名前** ] をクリックし、コネクタの **作成**します。 

    ![Click create](./media/app-service-api-connnect-your-app-to-saas-connector/createdropbox.png) 

    Azure App Service によってリソース グループには、リソース グループに Dropbox コネクタ API アプリを作成し、 *ゲートウェイ* web アプリです。 このゲートウェイは、リソース グループのすべての API アプリへのアクセスを管理します。 

    クリックしてリソースの作成の進行状況をチェックする **通知** Azure プレビュー ポータルのホーム ページにします。

3. Azure では、コネクタの作成が完了したら、クリックして **参照 > リソース グループ > DropboxRG**します。
 
     **リソース グループ** DropboxRG のブレードでは、リソース グループに、コネクタとゲートウェイが表示されます。

    ![リソース グループの図](./media/app-service-api-connnect-your-app-to-saas-connector/rgdiagram.png) 

## Dropbox アカウントと Dropbox コネクタの構成

Dropbox アカウントへの API のアクセスを有効にするには、Dropbox の開発者向けサイトで Dropbox アプリを作成する必要があます。 クライアント ID とクライアント シークレットの値をその Dropbox アプリから Dropbox コネクタにコピーして、認証済みの要求のみを受け入れるようにコネクタを設定します。

### <a id="createdbapp"></a>Dropbox アプリの作成

[AZURE.INCLUDE [app-service-api-create-dropbox-app](../../includes/app-service-api-create-dropbox-app.md)]

### <a id="copysettings"></a>Azure Dropbox コネクタへの Dropbox アプリ設定のコピーとその逆方向のコピー 

[AZURE.INCLUDE [app-service-api-exchange-dropbox-settings](../../includes/app-service-api-exchange-dropbox-settings.md)]

### 認証されたアクセスを要求するための Dropbox コネクタの設定

既定では、コネクタの **アクセス レベル** に設定されている **内部**, 、つまりのみで呼び出せるよう他の API アプリと同じリソース グループ内の web アプリです。 ただし、Dropbox では、認証されたユーザーにのみ Dropbox アカウントへのアクセスが許可されるため、ユーザー認証を要求するアクセス レベル設定に変更する必要があります。

1. 戻り、 **設定** ブレードで、およびクリック **アプリケーション設定**します。

2.  **アプリケーション設定** ブレードで、 **アクセス レベル** に **パブリック (認証済み)**, 、] をクリックし、 **保存**します。 
    
    ![[パブリック (認証済み)] に設定](./media/app-service-api-connnect-your-app-to-saas-connector/pubauth.png)

これで Dropbox コネクタの構成が完了し、発信呼び出しは Dropbox アカウントにアクセスできるようになり、着信呼び出しは認証されたユーザーから行うことが必須となりました。 次のセクションではユーザーの認証に使用する認証プロバイダーを指定します。

## ゲートウェイの構成

前述の [以前](#gateway), 、ゲートウェイは、リソース グループ内のすべての API アプリへのアクセスを管理する特殊な web アプリです。 ユーザーを認証するようにゲートウェイをセットアップするには、Azure Active Directory、Google、Twitter などの認証プロバイダーを選択します。 Dropbox コネクタを正常に呼び出すには、選択したプロバイダーによるユーザーの認証が必要になります。

- この手順を実行するには、 [、ゲートウェイを構成する](app-service-api-dotnet-add-authentication.md#configure-the-gateway) のセクションで、 [API アプリの保護](app-service-api-dotnet-add-authentication.md) チュートリアルでは、し、DropboxRG リソース グループで、ゲートウェイを構成する手順に従います。

## ユーザーと Dropbox の認証を検証するためのテスト

認証プロバイダーを使用するように DropboxRG リソース グループのゲートウェイを構成した後で、Dropbox コネクタをテストできます。

ほとんどの場合はコードから呼び出してコネクタを使用するため、その実現方法を説明するチュートリアルも作成されています。 ただし、アプリの他の部分を接続する前に、コネクタが動作していることを検証する必要が生じる場合もあります。 このチュートリアルでは、ブラウザーと単純な REST クライアント ツールを使用して、先ほどインストールして構成した Dropbox コネクタを通じて Dropbox サービスを操作できることを確認する方法を示します。

次の手順は、Chrome ブラウザー開発者ツールと Postman REST クライアント ツールを使用してこれらの手順を実行する方法を示しています。 これはほんの一例であり、他のブラウザーやツールでも同じ手順を実行することができます。 "Advanced REST Client" は、使用できる別の Chrome アドインです。

### エンド ユーザーとしてのログイン

新しいブラウザー ウィンドウで次の手順に従います。 使用する認証プロバイダーによって、プライベートまたは匿名でウィンドウを使用することが必要になる場合があります。

2. ゲートウェイと構成した認証プロバイダー用のログイン URL に移動します。 URL のパターンは次のとおりです。 

        http://[gatewayurl]/login/[providername]

    ゲートウェイの URL を取得できます、 **ゲートウェイ** ブレードで、 [Azure preview portal]します。 (に移動する、 **ゲートウェイ** ブレードに表示される図内のゲートウェイをクリックして、 **リソース グループ** ブレードです)。

    ![ゲートウェイ URL](./media/app-service-api-connnect-your-app-to-saas-connector/gatewayurl.png)

    [providername] の値は、Facebook の場合は "facebook"、Twitter の場合は "twitter"、Azure Active Directory の場合は "aad" です。

    Azure Active Directory のサンプルのログイン URL を次に示します。

        https://dropboxrgaeb4ae60b7cb4f3d966dfa43.azurewebsites.net/login/aad/

3. ブラウザーにログイン ページが表示されたら資格情報を入力します。 
 
    Azure Active Directory のログインを構成した場合に表示されるユーザーのいずれかとしてログイン、 **ユーザー** タブの [Azure Active Directory] タブで作成したアプリケーションを [Azure portal], 、admin@contoso.onmicrosoft.com などです。

    ログインに成功すると、ログインが完了したことを示すページが表示されます。

    ![](./media/app-service-api-connnect-your-app-to-saas-connector/logindone.png)

### Dropbox へのユーザーの ID の提供

Dropbox API を使用して Dropbox の承認を取得するには、Dropbox にユーザーの資格情報を提供する必要があります。  そのプロセスは Azure により開始されますが、そのプロセスを開始するには、ブラウザーで専用ゲートウェイの URL に移動する必要があります。  この URL を取得するには、HTTP Post 要求をゲートウェイに送ります。

ゲートウェイへの HTTP Post 要求には、ログイン時に Azure から提供された認証トークンを含める必要があります。 ブラウザーの要求の場合、トークンは Cookie に格納されているため、自動的に追加されますが、REST クライアント ツールを使用する HTTP Post 要求の場合は、Cookie からトークンを取得して HTTP Post 要求の要求ヘッダーに追加する必要があります。

1. ログインが完了したことを示すメッセージが表示されているブラウザー ウィンドウで、ブラウザーの開発者ツールに移動し、`x-zumo-auth` Cookie を検索します。 次の手順で Cookie の値をコピーできるように、このウィンドウは開いたままにします。
 
    Chrome で Cookie の値を取得するには、次の手順に従います。

    - F12 キーを押して開発者ツールを開きます。
    - 移動して、 **リソース** ] タブをクリックします。
    - ゲートウェイ サイトの cookie を検索し、トリプル クリックして、 **値** の `x-zumo-auth` cookie をすべて選択します。 (Cookie の値をすべて取得したことを確認してください。 ダブルクリックした場合、取得しているのは最初の部分のみである可能性があります)。  

    ![トークンのコピー](./media/app-service-api-connnect-your-app-to-saas-connector/copytoken.png) 

4. 新しいブラウザー タブまたはウィンドウで、ゲートウェイへの HTTP Post 要求を作成して送信し、同意の URL を要求します。 HTTP ヘッダーとして `x-zumo-auth` トークンを含めます。

    URL のパターンは次のとおりです。

        [gatewayurl]/api/consent/list?api-version=2015-01-14&redirecturl=[a URL you want the browser to go to after you authenticate]

    次に例を示します。

        https://dropboxrgaeb4ae60b7cb4f3d966dfa43.azurewebsites.net/api/consent/list?api-version=2015-01-14&redirecturl=https://portal.azure.com

    Chrome の Postman で要求を送信するには、次の手順に従います。

    - 入力、 **要求 URL** 上記で説明しました。
    - 方法を設定 **Post**します。
    - `x-zumo-auth` という名前のヘッダーを追加します。
    - ヘッダーに **値** フィールドからコピーした値を貼り付け、 `x-zumo-auth` cookie です。
    - クリックして **送信**します。
     
    次の図に、Chrome の Postman ツールを示します。

    ![同意 URL の送信](./media/app-service-api-connnect-your-app-to-saas-connector/sendforconsent.png)

    応答には、Dropbox へのユーザーのログイン プロセスを開始するために使用する URL が含まれています  (メソッドのドロップダウンに設定したにもかかわらず Get メソッドはサポートされていませんかを示すエラー応答を取得するかどうかは **Post**, 、ゲートウェイの URL がない HTTP、HTTPS であることを確認します)。

    ![同意 URL](./media/app-service-api-connnect-your-app-to-saas-connector/getconsenturl.png)

2. HTTP Post 要求への応答で受信した URL にアクセスします。

    この URL への応答により、ブラウザーを Dropbox サイトにリダイレクトします。ユーザーはこのサイトでアプリにログインし、アプリからそのユーザーのアカウントへのアクセスに同意します。
    
    Dropbox が指定したリダイレクト URL にブラウザーをリダイレクトするログインと同意のプロセスを完了すると、(たとえば、Azure プレビュー ポータル https://portal.azure.com を使用している例では、その後に場合)。 Web アプリから呼び出した場合は、Web アプリに表示される次のページがこのページになります。  ログインまたは同意プロセスでエラーがあった場合、リダイレクト URL にクエリ文字列変数 `error` が含まれていた可能性があるため、アプリで URL を確認する必要があります。

3. 次のセクションで使用するため、このブラウザ ウィンドウは開いたままにします。

### Dropbox コネクタ API の呼び出し 

Azure では次の 3 つの認証トークンが管理されるようになりました。

- Azure Active Directory など、ゲートウェイ用に構成した認証プロバイダーの認証トークン。
- Dropbox の認証トークン。
- Azure が作成した認証トークン ("zumo" トークン)。

Dropbox を操作するために HTTP 要求を行うときに使用する必要があるのは、zumo トークンのみです。 バックグラウンドで Azure によって zumo トークンは他の 2 つと関連付けられます。Dropbox への呼び出しが行われるときに、コネクタがユーザーの代わりに他の 2 つを提供します。

次の手順では、Dropbox アカウント内のファイルを参照するために、Dropbox コネクタ への Get 要求を行います。 Get 要求にはブラウザー ウィンドウを使用できるうえ、ブラウザー ウィンドウの Cookie には既に zumo トークンが格納されているため、URL に移動するだけで Dropbox のデータが返されます。 

1. Azure プレビュー ポータルを開いているブラウザーのウィンドウに戻り、 **API アプリ** Dropbox コネクタのブレードです。 

2. API アプリの URL をコピーします。
 
4. クリックして、 **API の定義** アイコンは、コネクタで使用できる API メソッドを参照してください。

    ![API App blade](./media/app-service-api-connnect-your-app-to-saas-connector/apiappblade.png) 

    ![API の定義](./media/app-service-api-connnect-your-app-to-saas-connector/apidef.png) 

2. ゲートウェイで認証に使用したブラウザー ウィンドウで、フォルダーからファイル名を取得する GET メソッドを呼び出します。 URL パターンを次に示します。

        [connectorurl]/folder/[foldername]
   
3. たとえば、コネクタの URL は https://dropboxrg784237ad3e7.azurewebsites.net ブログ フォルダー内のファイルを表示する場合は、URL は次のようになります。

        https://dropboxrg784237ad3e7.azurewebsites.net/folder/blog

    API 呼び出しへの応答は、ブラウザーによってさまざまです。Chrome を使用している場合は、次の例のような応答が表示されます。

    ![フォルダー要求への GET 応答](./media/app-service-api-connnect-your-app-to-saas-connector/dbresponse.png) 

## 次のステップ

SaaS コネクタをインストール、構成、テストする方法を説明しました。 詳細については、次のリソースを参照してください。

* [コネクタの使用](../app-service-logic/app-service-logic-connectors-list.md)
* [API Apps と Mobile Apps の認証](../app-service/app-service-authentication-overview.md)  

[Azure preview portal]: https://portal.azure.com/
[Azure portal]: https://manage.windowsazure.com/
 

