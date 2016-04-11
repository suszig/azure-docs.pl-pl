<properties
   pageTitle="Visual Studio Code を使用した ASP.NET 5 API アプリの作成"
   description="このチュートリアルでは、Visual Studio Code を使用して ASP.NET 5 API アプリを作成する方法について説明します。"
   services="app-service\api"
   documentationCenter=".net"
   authors="erikre"
   manager="wpickett"
   editor="jimbe"/>

<tags
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/04/2015" 
    ms.author="erikre"/>

# Visual Studio Code を使用した ASP.NET 5 API アプリの作成

[AZURE.INCLUDE [app-service-api-v2-note](../../includes/app-service-api-v2-note.md)]

## 概要

このチュートリアルは、使用して ASP.NET 5 API アプリを作成する方法を示します [Visual Studio Code](http://code.visualstudio.com//Docs/whyvscode)します。 ASP.NET 5 は、ASP.NET の刷新版です。 ASP.NET 5 は、.NET を使用して最新のクラウドベースの Web アプリケーションを構築するための、新しいオープン ソースのクロスプラットフォーム フレームワークです。 詳細については、次を参照してください。 [ASP.NET 5 の概要](http://docs.asp.net/en/latest/conceptual-overview/aspnet.html)します。 API アプリについては、次を参照してください [API apps とは?。](app-service-api-apps-why-best-platform.md)

> [AZURE.NOTE] このチュートリアルを完了するには、Microsoft Azure アカウントが必要です。 アカウントを持っていない場合は、 [無料評価版にサインアップ](/pricing/free-trial/) または [MSDN サブスクライバーの特典をアクティブ化](/pricing/member-offers/msdn-benefits-details/)します。 ことができますの無料評価版も、 [App Service アプリのサンプル](http://tryappservice.azure.com)します。

## 前提条件  

* インストールおよびセットアップ [Visual Studio Code](http://code.visualstudio.com/Docs/setup)します。
* インストール [Node.js](http://nodejs.org/download/). < br \ >
    [ノード](http://nodejs.org/) は JavaScript を使用して高速かつスケーラブルなサーバー アプリケーションを構築するためのプラットフォームです。 Node はランタイム (ノード) と [npm](http://www.npmjs.com/) は Node モジュールのパッケージ マネージャー。 このチュートリアルでは、npm を使用して、ASP.NET 5 API アプリをスキャフォールディングします。

## ASP.NET 5 と DNX のインストール
ASP.NET 5 と DNX は、OS X、Linux、Windows 上で動作する最新のクラウドや Web アプリを構築するための、効率の優れた .NET スタックです。 ASP.NET 5 および DNX は、一から設計し直され、クラウドにデプロイされるアプリまたはオンプレミスで実行されるアプリ用に最適化された開発フレームワークを提供します。 オーバーヘッドを最小に抑えたモジュラー コンポーネントから構成されるため、ソリューションを構築するときに柔軟性を保つことができます。

> [AZURE.NOTE] ASP.NET 5 と DNX (.NET Execution Environment) OS X、Linux では初期のベータかプレビュー状態です。 

このチュートリアルでは、最新の開発バージョンの ASP.NET 5 と DNX を使用してアプリケーションの構築を開始する方法について説明します。 安定したリリースされた、エクスペリエンスにする場合に、 [http://www.asp.net/vnext](http://www.asp.net/vnext)します。 次の手順は、Windows に固有の手順です。 OS X、Linux、および Windows のインストール手順の詳細を参照してください。 [をインストールする ASP.NET 5 と DNX](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx)します。 

1. .NET Version Manager (DNVM) をインストールするには、コマンド ウィンドウで次のコマンドを実行します。

    <pre class="prettyprint">
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "&{$Branch='dev';iex ((new-object net.webclient).DownloadString('https://raw.githubusercontent.com/aspnet/Home/dev/dnvminstall.ps1'))}"
    </pre> 

    DNVM スクリプトがダウンロードされ、ユーザー プロファイルに配置されます。 

2. PATH 環境変数に加えた変更を有効にするために、上記のコマンドを入力した後にログオフすることが必要になる場合があります。
3. コマンド ウィンドウで次のコマンドを実行して、DNVM の場所を確認します。 

    <pre class="prettyprint">
    where dnvm
    </pre> 

    コマンド ウィンドウに、次のようなパスが表示されます。

    ![dnvm の場所](./media/app-service-create-aspnet-api-app-using-vscode/00-where-dnvm.png)

4. これで、DNVM を利用できるようになりました。アプリケーションを実行するには、これを使って DNX をダウンロードする必要があります。 コマンド ウィンドウで、次のコマンドを実行します。

    <pre class="prettyprint">
    dnvm upgrade
    </pre> 

5. コマンド ウィンドウで次のコマンドを実行して、DNVM を確認し、アクティブなランタイムを表示します。

    <pre class="prettyprint">
    dnvm list
    </pre> 

    コマンド ウィンドウに、アクティブなランタイムの詳細が表示されます。

    ![dnvm の場所](./media/app-service-create-aspnet-api-app-using-vscode/00b-dnvm-list.png)

OS X、Linux、および Windows のインストール手順の詳細を参照してください。 [をインストールする ASP.NET 5 と DNX](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx)します。 

## API アプリの作成 

このセクションでは、新しい ASP.NET API アプリをスキャフォールディングする方法について説明します。 ノード パッケージ マネージャー (npm) を使用してインストールする [Yeoman](http://yeoman.io/), 、[Grunt](http://gruntjs.com/), 、および [Bower](http://bower.io/)します。 

1. Visual Studio Code と Node.js をインストールした後、管理者権限でコマンド プロンプトを開き、VSCode を使用するすべての ASP.NET プロジェクトを配置する場所に移動します。
2. コマンド ウィンドウで次のコマンドを入力して、Yeoman とサポート ツールをインストールします。

    <pre class="prettyprint">
    npm install -g yo grunt-cli generator-aspnet bower
    </pre> 

3. コマンド ウィンドウで次のコマンドを入力して、プロジェクト フォルダーを作成し、アプリをスキャフォールディングします。

    <pre class="prettyprint">
    yo aspnet
    </pre> 

4. スクロールを選択して、ジェネレーターによって提供される指示に従って、 **Web API アプリケーション** 型です。

    ![Yoman - ASP.NET 5 generator](./media/app-service-create-aspnet-api-app-using-vscode/01-yo-aspnet.png)

5. 新しい ASP.NET API アプリの名前を設定 **ContactsList**します。 この名前は、このチュートリアルの後半で示すコードで使用されます。 <br>
    Yoman によってという名前の新しいフォルダーが作成されます **ContactsList** と新しいアプリに必要なファイルです。
6. 開いている **Visual Studio Code**します。<br>
    入力して、VSCode をコマンド ウィンドウを開くことができます **コード。**します。
7.  **ファイル** メニューの [ **フォルダーを開く** し、ASP.NET API アプリがあるフォルダーを選択します。

    ![[フォルダー] ダイアログ ボックスの選択](./media/app-service-create-aspnet-api-app-using-vscode/02-open-folder.png)

    VSCode は、プロジェクトを読み込んで表示で、 **エクスプ ローラー** ウィンドウです。

    ![VSCode displaying Contact project](./media/app-service-create-aspnet-api-app-using-vscode/03-vscode-contacts-project.png)

8.  **VSCode**, から、 **ビュー** メニューの [ **コマンド パレット**します。
9.  **コマンド パレット** 、次のコマンドを入力してください。

    <pre class="prettyprint">
    dnx:dnu restore - (ContactsList)
    </pre> 

    入力を開始すると、一覧に完全なコマンド ラインが表示されます。

    ![Restore コマンド](./media/app-service-create-aspnet-api-app-using-vscode/04-dnu-restore.png)

    Restore コマンドは、アプリケーションを実行するために必要な NuGet パッケージをインストールします。 コマンド ウィンドウに表示されます **復元は完了** 準備ができます。

## API アプリの変更

変更して、 **ContactsList** アプリを追加することで、 **にお問い合わせください** クラスおよび **ContactsController** クラスです。

1. カーソルを置き、 **ContactsList** プロジェクト名とという名前の新しいフォルダーを追加 *モデル* 新しいフォルダー] アイコンを使用します。

    ![New Models folder](./media/app-service-create-aspnet-api-app-using-vscode/07-new-folder.png)

2. 右クリックし、 **モデル** という名前の新しいクラス ファイルを追加するフォルダーを *Contact.cs* を次のコード。

    <pre class="prettyprint">
    namespace ContactsList.Models
    {
        public class Contact
        {
            public int Id { get; set; }
            public string Name { get; set; }
            public string EmailAddress { get; set; }
        {
    }
    </pre>

3. 右クリックし、 **コント ローラー** フォルダーを追加し、 *ContactsController.cs* ファイルの次のように表示されるようにします。

    <pre class="prettyprint">
    using System.Collections.Generic;
    using Microsoft.AspNet.Mvc;
    using ContactsList.Models;
    
    namespace ContactsList.Controllers
    {
        [Route("api/[controller]")]
        public class ContactsController : Controller
        {
            // GET: api/Contacts
            [HttpGet]
            パブリックの IEnumerable & lt;連絡先 & gt;Get()
            {
                return new Contact[]{
                    new Contact { Id = 1, EmailAddress = "barney@contoso.com", Name = "Barney Poland"},
                    new Contact { Id = 2, EmailAddress = "lacy@contoso.com", Name = "Lacy Barrera"},
                    new Contact { Id = 3, EmailAddress = "lora@microsoft.com", Name = "Lora Riggs"}
                };
            {
        {
    }
    </pre>

4. 選択して、すべてのファイルを保存します **ファイル** > **すべて保存**します。
5.  **コマンド パレット**, 、アプリケーションをローカルで実行するには、次を入力してください。

    <pre class="prettyprint">
    dnx: kestrel - (ContactsList, Microsoft.AspNet.Hosting --server Kestrel --server.urls http://localhost:5001
    </pre>

    コマンド ウィンドウに表示されます *開始* にします。 コマンド ウィンドウが表示されない場合は *開始*, 、下部左隅示されていない VSCode のプロジェクトのエラーを確認します。

5. ブラウザーを開き、次の URL に移動します。

    **http://localhost:5001/api/連絡先**

    内容を表示することができますし、 *Contacts.json*します。 このファイルを表示すると、次の内容が表示されます。

    ![Returned json content](./media/app-service-create-aspnet-api-app-using-vscode/08-contacts-json.png)

## API アプリのメタデータの変更
ASP.NET API プロジェクトを API アプリに含まれている必要がありますとしてデプロイできるようにするメタデータ、 *apiapp.json* プロジェクトのルートにあるファイルです。

1. VSCode で、右クリックし、 *wwwroot* フォルダーの選択、 **新しいファイル** オプション。
2. 新しいファイルの名前 *apiapp.json*. < br \ >
    確認 *apiapp.json* では、 *wwwroot* フォルダーです。
3. 次のコードを追加、 *apiapp.json* ファイル。

    <pre class="prettyprint">
    {
        "$schema": "http://json-schema.org/schemas/2014-11-01/apiapp.json#",
        "id": "ContactsList",
        "namespace": "microsoft.com",
        "gateway": "2015-01-14",
        "version": "1.0.0",
        "title": "ContactsList",
        "summary": "",
        "author": "",
        "endpoints": null
    }
    </pre>

 *Apiapp.json* ファイル、動的 Swagger API 定義 JSON のエンドポイントを指定することができます。 ただしこのチュートリアルを、静的 API 定義ファイルを使用します。 動的 Swagger 生成を使用する例を参照してください。 [API アプリとして Web API プロジェクトを構成する](app-service-dotnet-create-api-app-visual-studio.md)です。

## 静的 Swagger API 定義の追加
静的 Swagger 2.0 API 定義ファイルを指定するには、Web ルートにフォルダーを作成し、そのフォルダーに API 定義ファイルを配置する必要があります。

1. VSCode で、という名前のフォルダーを作成 *メタデータ* で、 *wwwroot* フォルダーです。
2. 新しいを右クリックし *メタデータ* フォルダーという名前の新しいファイルを追加および *apiDefinition.swagger.json*します。 
3. 次の json 構文を新しいファイルに追加します。

    <pre class="prettyprint">
    {
      "swagger": "2.0",
      "info": {
        "version": "v1",
        "title": "ContactsList"
      },
      "host": "MUST REPLACE THIS WITH YOUR HOST URL",
      "schemes": [
        "https"
      ],
      "paths": {
        "/api/Contacts": {
          "get": {
            "tags": [
              "Contacts"
            ],
            "operationId": "Contacts_Get",
            "consumes": [],
            "produces": [
              "application/json",
              "text/json",
              "application/xml",
              "text/xml"
            ],
            "responses": {
              "200": {
                "description": "OK",
                "schema": {
                  "type": "array",
                  "items": {
                    "$ref": "#/definitions/Contact"
                  {
                {
              {
            },
            "deprecated": false
          },
          "post": {
            "tags": [
              "Contacts"
            ],
            "operationId": "Contacts_Post",
            "consumes": [
              "application/json",
              "text/json",
              "application/xml",
              "text/xml",
              "application/x-www-form-urlencoded"
            ],
            "produces": [
              "application/json",
              "text/json",
              "application/xml",
              "text/xml"
            ],
            "parameters": [
              {
                "name": "contact",
                "in": "body",
                "required": true,
                "schema": {
                  "$ref": "#/definitions/Contact"
                {
              {
            ],
            "responses": {
              "200": {
                "description": "OK",
                "schema": {
                  "$ref": "#/definitions/Object"
                {
              {
            },
            "deprecated": false
          {
        {
      },
      "definitions": {
        "Contact": {
          "type": "object",
          "properties": {
            "Id": {
              "format": "int32",
              "type": "integer"
            },
            "Name": {
              "type": "string"
            },
            "EmailAddress": {
              "type": "string"
            {
          {
        },
        "Object": {
          "type": "object",
          "properties": {}
        {
      {
    }
    </pre>

このチュートリアルの後半では、Azure ホスト URL が作成され、後でコピーすると上記のホスト URL のプレース ホルダー文字列が置き換わります。

## Azure プレビュー ポータルでの API アプリの作成

> [AZURE.NOTE] このチュートリアルを完了するには、Microsoft Azure アカウントが必要です。 アカウントを持っていない場合は、 [無料評価版にサインアップ](/pricing/free-trial/) または [MSDN サブスクライバーの特典をアクティブ化](/pricing/member-offers/msdn-benefits-details/)します。 ことができますの無料評価版も、 [App Service アプリのサンプル](http://tryappservice.azure.com)します。

1. ログイン、 [Azure プレビュー ポータル](https://portal.azure.com)します。

2. クリックして **新規** で、ポータルの左下です。

3. クリックして **Web + モバイル > API アプリ**します。

    ![Azure New API App](./media/app-service-create-aspnet-api-app-using-vscode/09-azure-newapiapp.png)

4. 値を入力 **名前**, 、ContactsList などです。

5. App Service プランを選択するか、新しいプランを作成します。 新しいプランを作成する場合は、価格レベル、および場所などのオプションを選択します。

    ![Azure New API App Blade](./media/app-service-create-aspnet-api-app-using-vscode/10-azure-newappblade.png)

6. クリックして **作成**します。

    ![API App Blade](./media/app-service-create-aspnet-api-app-using-vscode/11-azure-apiappblade.png)

    ままの場合、 **スタート画面にピン** チェック ボックスは、アプリを作成するときにオン] をクリックして、アプリを簡単に見つけることができます **ホーム** または **参照**します。 チェック ボックスをオフにした場合はクリックして **通知** 左側を API アプリの作成状態を確認し、通知をクリックすると、新しい API アプリのブレードに移動します。

7. クリックして **設定 > アプリケーション設定**します。

8. アクセスのレベルを設定 **パブリック (匿名)**します。

9. クリックして **保存**します。

    ![Azure App Settings](./media/app-service-create-aspnet-api-app-using-vscode/12-azure-appsettings.png)

## 新しい API アプリからの Git 発行の有効化

Git は、Azure の Web サイトをデプロイするために使用できる分散型バージョン コントロール システムです。 API アプリ用に記述したコードはローカルの Git リポジトリに格納されます。このコードをリモート リポジトリにプッシュして Azure にデプロイします。 このデプロイ方法は、API アプリが Web アプリに基づいているために API アプリで使用できる App Service Web アプリの機能です。つまり、Azure App Service の API アプリは、Web サービスをホストするための追加の機能を持つ Web アプリであるということです。  

特定の API アプリへの機能を管理し、ポータルで、 **API アプリ** ブレードで web アプリと共有されている機能の管理、 **API アプリ ホスト** ブレードです。 このセクションに移動して、 **API アプリ ホスト** ブレードで、Git デプロイメントの機能を構成します。

1. API アプリ] ブレードで [ **API アプリ ホスト**します。

    ![Azure API App Host](./media/app-service-create-aspnet-api-app-using-vscode/13-azure-apiapphost.png)

2. 検索、 **展開** のセクションで、 **API アプリ** ブレードをクリック **継続的なデプロイを設定**します。 必要に応じて、ブレードのこの部分まで下へスクロールしてください。

    ![Azure API App Host](./media/app-service-create-aspnet-api-app-using-vscode/14-azure-deployment.png)

3. クリックして **ソースの選択 > ローカル Git リポジトリ**します。

5. Click **OK**.

    ![Azure のローカル Git リポジトリ](./media/app-service-create-aspnet-api-app-using-vscode/15-azure-localrepository.png)

6. API アプリまたはその他の App Service アプリを発行するためのデプロイ資格情報をまだ設定していない場合は、ここで設定します。

    * クリックして **デプロイ資格情報を設定**します。

    * ユーザー名とパスワードを作成します。  このパスワードは、後で Git をセットアップするときに必要になります。

    * クリックして **保存**します。

    ![Azure デプロイメント資格情報](./media/app-service-create-aspnet-api-app-using-vscode/16-azure-credentials.png)

7.  **API アプリ ホスト** ブレードで、をクリックして **設定 > プロパティ**します。 デプロイ先のリモート Git リポジトリの URL は、[GIT URL] の下に表示されます。

8. コピー、 **GIT URL** チュートリアルの後半で使用するためです。

    ![Azure Git の URL](./media/app-service-create-aspnet-api-app-using-vscode/17-azure-giturl.png)

9. さらに、 **API アプリ** ブレードで、コピー、 **URL** "host"値を更新に使用すること、 *apiDefinition.swagger.json* ファイルです。

    ![Azure URL](./media/app-service-create-aspnet-api-app-using-vscode/18-azure-url.png)

10. VSCode で、開く、 *apiDefinition.swagger.json* ファイルをホスト値"する必要があります置換こので YOUR HOST URL"に置き換えます、 **URL** 前の手順でコピーします。
11. さらに、ホスト値の先頭から、"https://" 文字を削除します。
12. 変更を保存、 *apiDefinition.swagger.json* ファイルです。

## Azure App Service への API アプリの発行

このセクションでは、Azure App Service で実行されている API アプリにサンプル アプリケーションをデプロイするために、ローカル Git リポジトリを作成してそのリポジトリから Azure にプッシュします。

1. VSCode の左側のナビゲーション バーで、[Git] オプションを選択します。
2. Git がインストールされていない場合は指定されたリンクのいずれかを選択してインストール ([Chocolatey](https://chocolatey.org/packages/git) または [git-scm.com](http://git-scm.com/downloads))。 Git に慣れていない場合は、選択 **git-scm.com** と GitBash Windows コマンド プロンプトから Git を使用するオプションを選択します。 
3. Git がインストールされたら、VSCode を再起動し、左側のバーの [Git] オプションを選択します。
4. VSCode で、次のように選択します。 **git リポジトリの初期化** 、ワークスペースが git によるソース管理対象かどうかを確認します。 

    ![Git の初期化](./media/app-service-create-aspnet-api-app-using-vscode/19-initgit.png)

5. コミット メッセージを追加し、選択、 **すべてコミット** を確認します。

    ![Git すべてコミット](./media/app-service-create-aspnet-api-app-using-vscode/20-git-commit.png)

6. 検索して開く **GitBash**します。 代わりに、Windows コマンド プロンプトを使用できます。
7.  **GitBash**, 、VSCode プロジェクト フォルダーに移動します。 次に例を示します。

    <pre class="prettyprint">
    cd c:\VSCodeProjects\ContactsList
    </pre>

8. コピーしておいた (".git" で終わる) Git URL を使用して、作成した Web アプリ (API アプリ ホスト) に更新をプッシュするためのリモート参照を作成します。

    <pre class="prettyprint">
    git remote add azure [URL for remote repository]
    </pre>

9. 次のコマンドを入力して、変更内容を Azure にプッシュします。

    <pre class="prettyprint">
    git push azure master
    </pre>

    以前作成したパスワードを入力するように求められます。 **注: パスワードは表示されません。**

    上記のコマンドの出力は、デプロイが成功したことを示すメッセージで終わります。

    <pre class="prettyprint">
    remote: Deployment successful.
    To https://user@testsite.scm.azurewebsites.net/testsite.git
    [new branch]      master -> master
    </pre>

> [AZURE.NOTE] アプリに変更を加えた場合を選択して再発行することができます、 **すべてコミット** VSCode で確認し、入力、 **git push azure master** コマンドを **GitBash**します。

## Azure プレビュー ポータルでの API 定義の表示
API を API アプリにデプロイしたので、Azure プレビュー ポータルで API 定義を確認できるようになりました。 最初にゲートウェイを再起動します。これによって、API アプリの API 定義が変更されたことを Azure で認識できるようになります。 ゲートウェイは、API の管理とリソース グループ内の API アプリの承認を処理する Web アプリです。

1. Azure プレビュー ポータルに移動、 **API アプリ** ] をクリックして前に作成した API アプリのブレード、 **ゲートウェイ** リンクします。
2.  **ゲートウェイ** ブレードで、をクリックして **再起動**します。 これでこのブレードを閉じることができます。
3.  **API アプリ** ブレードで、をクリックして、 **再起動**します。 
4.  **API アプリ** ブレードで、をクリックして **API の定義**します。<br>
    [API 定義] ブレードに 2 つのメソッドが表示されます。 GET メソッドと POST メソッドがすぐに表示されない場合は、Azure によってアプリが更新されるまで数秒待ってください。 クリックして **API の定義** で、 **API アプリ** ブレードです。

## Azure でのアプリの実行
Azure プレビュー ポータルに移動、 **API アプリ ホスト** ] をクリックして API アプリのブレード **参照**します。 次に、追加 **Api/contacts** 連絡先の詳細を表示するため、URL の末尾にします。


## まとめ
このチュートリアルでは、Visual Studio Code を使用して API アプリを作成する方法について説明しました。 Visual Studio Code の詳細については、次を参照してください。 [Visual Studio Code](https://code.visualstudio.com/Docs/)します。 API アプリについては、次を参照してください [API apps とは?。](app-service-api-apps-why-best-platform.md)
 

