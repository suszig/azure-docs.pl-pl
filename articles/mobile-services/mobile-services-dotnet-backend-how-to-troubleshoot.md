<properties
    pageTitle="Mobile Services .NET バックエンドのトラブルシューティング | Microsoft Azure"
    description=".NET バックエンドを使用する Mobile Services で問題を診断して解決する方法について説明します。"
    services="mobile-services"
    documentationCenter=""
    authors="wesmc7777"
    manager="dwrede"
    editor="mollybos"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="12/01/2015" 
    ms.author="wesmc;ricksal"/>

# Mobile Services .NET バックエンドのトラブルシューティング

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


Mobile Services を使用した開発は、通常は手間もかからないのですが、場合によっては問題が発生することもあります。 このチュートリアルでは、Mobile Services .NET バックエンドで発生する可能性がある一般的な問題のトラブルシューティングの手法をいくつか説明します。

1. [HTTP デバッグ](#HttpDebugging)
2. [ランタイム デバッグ](#RuntimeDebugging)
3. [診断ログの分析](#Logs)
4. [デバッグによるクラウド アセンブリの問題の解決](#AssemblyResolution)
5. [Entity Framework の移行に関するトラブルシューティング](#EFMigrations)

<a name="HttpDebugging"></a>
## HTTP デバッグ

Mobile Services でアプリケーションを開発するには、通常、使用しているプラットフォーム (Windows ストア、iOS、Android など) 向けの Mobile Services クライアント SDK を活用します。 ただし、HTTP レベルに下がって、ネットワーク上で直接の HTTP 呼び出しが実行された時点を観察すると役立つことがあります。 この方法は、特に接続の問題とシリアル化の問題をデバッグするときに便利です。 Mobile Services .NET バックエンドでは、この方法を Visual Studio のローカル デバッグおよびリモート デバッグ (詳細については、次のセクションを参照) と組み合わせて使用して、サービス コードを起動するまでに HTTP 呼び出しがたどったパスを明確に把握できます。

HTTP トラフィックを送信し、調査するために、任意の HTTP デバッガーを使用できます。 [Fiddler](http://www.telerik.com/fiddler) は通常この目的の開発者で使用する一般的なツールです。 Mobile Services では、開発者が作業しやすくなるように、Web ベースの HTTP デバッガー ("テスト クライアント" とも呼びます) がモバイル サービスに付属していて、外部のツールをできるだけ使用しないで済みます。 ローカル モバイル サービスをホストする場合のような URI で使用可能になります [http://localhost:59233](http://localhost:59233) であり、クラウドでホストする場合、URI は、フォームの [http://todo-list.azure-mobile.net](http://todo-list.azure-mobile.net)します。 次の手順は、サービスがホストされている場所に関係なく、同じように機能します。

1. 開いている Mobile Services サーバー プロジェクトを開始 **Visual Studio 2013 Update 2** またはそれ以降。 お持ちでない場合、便利な作成を選択して **ファイル**, 、**新規**, 、**プロジェクト**, を選択して、 **クラウド** ノードし、 **Microsoft Azure Mobile Services** テンプレートです。
2. ヒット **f5 キーを押して**, をビルドおよびプロジェクトを実行します。 [スタート] ページで、次のように選択します。 **試してみる**します。

    >[AZURE.NOTE]
    > サービスがローカルでホストされている場合は、リンクをクリックすると次のページに進みます。 ただし、クラウドでホストされている場合は、一連の資格情報を入力するよう求められます。 これにより、認証されていないユーザーは、開発者の API とペイロードに関する情報にアクセスできなくなります。 ページを参照するためには、ログインに使用する必要がある、 **空のユーザー名** と **アプリケーション キー** パスワードとして。 移動して、アプリケーション キーに Azure クラシック ポータルで使用できる、 **ダッシュ ボード** タブに移動して、モバイル サービスを選択して **キーの管理**します。
    >
    > ![Authentication prompt to access help page][HelpPageAuth]

3. 表示される画面 ("ヘルプ ページ" と呼びます) に、モバイル サービスが使用可能にしている HTTP API がすべて一覧で表示されます。 Api のいずれかを選択 (Visual Studio での Mobile Services プロジェクト テンプレートの使用を開始すると、表示されるはずです **GET Tables/todoitem**)。

    ![Help page][HelpPage]

4. 結果のページでは、に関するドキュメントと JSON 要求と、API が期待される応答の例を示します。 選択、 **試して** ] ボタンをクリックします。

    ![Test page for an API][HelpPageApi]

5. これが "テスト クライアント" で、HTTP 要求を送信して API のテストに使用できます。 選択 **送信**します。

    ![Test client][TestClient]

6. ブラウザーのウィンドウに、モバイル サービスから返される HTTP 応答が表示されます。

    ![Test client with HTTP response][TestClientResponse]

これで、Web ブラウザーを使用して、モバイル サービスによって公開されるさまざまな HTTP API を調査できます。

<a name="RuntimeDebugging"></a>
## ランタイム デバッグ

.NET バックエンドの重要な機能の 1 つに、ローカルでサービス コードをデバッグできるだけでなく、クラウド環境でライブで実行されているコードもデバッグできる機能があります。 次の手順に従います。

1. デバッグするには、モバイル サービス プロジェクトを開く **Visual Studio 2013 Update 2** またはそれ以降。
2. シンボルの読み込みを構成します。 移動し、 **デバッグ** メニューをクリック **オプションと設定**します。 いることを確認 **マイ コードのみを有効にする** がオフになって、 **ソース サーバー サポートを有効にする** がオンになっています。

    ![Configure symbol loading][SymbolLoading]

3. 選択、 **シンボル** 左側のノード、URI を使用して、[SymbolSource] サーバーへの参照を追加および [http://srv.symbolsource.org/pdb/Public](http://srv.symbolsource.org/pdb/Public)します。 Mobile Services .NET バックエンドのシンボルが、すべての新しいリリースで使用可能になります。

    ![Configure symbol server][SymbolServer]

4. デバッグするコードの一部にブレークポイントを設定します。 たとえばにブレークポイントを設定、 **GetAllTodoItems()** のメソッド、 **TodoItemController** Visual Studio で Mobile Services プロジェクト テンプレートに付属します。
5. キーを押して、ローカルでサービスをデバッグ **f5 キーを押して**します。 Visual Studio が Mobile Services .NET バックエンドのシンボルをダウンロードするため、最初の読み込みに時間がかかる場合があります。
6. 前の HTTP デバッグのセクションで説明したように、テスト クライアントを使用して、ブレークポイントを設定したメソッドに HTTP 要求を送信します。 要求を送信するなど、 **GetAllTodoItems()** メソッドを選択して **GET Tables/todoitem** ヘルプ ページで [ **試すことが** し **送信**します。
7. 設定したブレークポイントで visual Studio が一時停止し、ソース コードの完全なスタック トレースがで利用できる、 **呼び出し履歴** Visual Studio のウィンドウです。

    ![Hitting a breakpoint][Breakpoint]

8. これで Azure にサービスを発行できるようになったので、上で実行したようにデバッグを使用できます。 プロジェクトで右クリックして発行 **ソリューション エクスプ ローラー** を選択すると、 **発行**します。
9.  **設定** [発行ウィザードのタブ、 **デバッグ** 構成します。 これにより、関連するデバッグ シンボルがコードと共に発行されます。

    ![Publish debug][PublishDebug]

10. サービスが正常に発行されると、開く **サーバー エクスプ ローラー** を展開し、 **Azure** と **Mobile Services** ノードです。 必要に応じてサインインします。
11. 発行したモバイル サービスを右クリックし、選択 **[デバッガーの**です。

    ![デバッガーの接続][AttachDebugger]

12. 手順 6 で実行したように、HTTP 要求を送信して、ブレークポイントを設定したメソッドを起動します。 今回は、Azure でホストされるモバイル サービスのヘルプ ページとテスト クライアントを使用します。
13. ブレークポイントで Visual Studio が一時停止します。

これで、ローカルでの開発と Azure のライブ モバイル サービスに対する開発に Visual Studio デバッガーの機能を十分に利用できます。

<a name="Logs"></a>
## 診断ログの分析

モバイル サービスでは顧客からの要求を処理するときに、さまざまな役立つ診断情報を生成します。また、発生した例外も取得します。 その他に、活用することで追加のログでコント ローラー コードをインストルメントすることができます、 [**ログ**](http://msdn.microsoft.com/library/microsoft.windowsazure.mobile.service.apiservices.log.aspx) プロパティで使用できる、 [**サービス**](http://msdn.microsoft.com/library/microsoft.windowsazure.mobile.service.tables.tablecontroller.services.aspx) のプロパティすべて [**TableController**](http://msdn.microsoft.com/library/microsoft.windowsazure.mobile.service.tables.tablecontroller.aspx)します。

ローカルでデバッグ ログは、Visual Studio に表示されます **出力** ウィンドウです。

![Logs in Visual Studio Output window][LogsOutputWindow]

クラウドで実行されているサービス インスタンスのログは、Visual Studio でモバイル サービスを右クリックして利用可能なサービスを Azure に発行した後 **サーバー エクスプ ローラー** を選択して **ログを表示する**です。

![Logs in Visual Studio Server Explorer][LogsServerExplorer]

同じログも利用の Azure クラシック ポータルで、 **ログ** 、モバイル サービス] タブをクリックします。

![Azure クラシック ポータルのログ][LogsPortal]

<a name="AssemblyResolution"></a>
## デバッグによるクラウド アセンブリの問題の解決

モバイル サービスを Azure に発行すると、モバイル サービスは Mobile Services のホスティング環境によって読み込まれ、コントローラー コードをホストする HTTP パイプラインに対してシームレスにアップグレードとパッチが適用されます。 これによって参照されるすべてのアセンブリが含まれます、 [.NET バックエンド NuGet パッケージ](http://www.nuget.org/packages?q=%22mobile+services+.net+backend%22): チームが常にこれらのアセンブリの最新バージョンを使用してサービスを更新します。

それが参照することでバージョン管理の競合が発生する可能性があります *異なるメジャー バージョン* 必要なアセンブリ (異なる *マイナー* バージョンが許可される)。 この現象は、Mobile Services .NET バックエンドが使用するいずれかのパッケージの最新バージョンにアップグレードするよう NuGet から求められるときに頻繁に発生します。

>[AZURE.NOTE] Mobile Services は現在 ASP.NET 5.1; でのみ対応ASP.NET 5.2 は現在サポートされていません。 お使いの ASP.NET NuGet パッケージを 5.2.* にアップグレードすると、デプロイ後にエラーが発生する可能性があります。

更新されたサービスを Azure に発行するときに、そのいずれかのパッケージをアップグレードする場合は、次のように競合を示す警告ページが表示されます。

![Help page indicating assembly loading conflict][HelpConflict]

この現象と共に、次と類似する例外メッセージがサービス ログに記録されます。

    Found conflicts between different versions of the same dependent assembly 'Microsoft.ServiceBus': 2.2.0.0, 2.3.0.0. Please change your project to use version '2.2.0.0' which is the one currently supported by the hosting environment.

この問題の修正は容易です。必要なアセンブリのバージョンをサポートされているバージョンに戻して、サービスを再発行するだけです。

<a name="EFMigrations"></a>
## Entity Framework の移行に関するトラブルシューティング

SQL Database と共に Mobile Services .NET バックエンドを使用している場合、Entity Framework (EF) をデータ アクセス テクノロジとして使用することで、データベースのクエリの実行とデータベースへのオブジェクトの保存が可能となります。 開発者に代わって EF が処理する 1 つの重要な側面は、どのようにデータベースの列 (とも呼ばれます *スキーマ*) としてコードに指定したモデル クラスを変更します。 このプロセスと呼ばれる [Code First Migrations](http://msdn.microsoft.com/data/jj591621)します。

移行は複雑となる場合があり、データベースの状態が EF モデルと常に同期されていないと成功しない可能性があります。 移行とモバイル サービスと生じる可能性のあるエラーを処理する方法については、次を参照してください。 [.NET バックエンド モバイル サービスにデータ モデルの変更を加える方法](mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)します。

<!-- IMAGES -->

[HelpPageAuth]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/6.png
[HelpPage]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/7.png
[HelpPageApi]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/8.png
[TestClient]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/9.png
[TestClientResponse]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/10.png
[SymbolLoading]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/1.png
[SymbolServer]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/2.png
[Breakpoint]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/3.png
[PublishDebug]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/4.png
[AttachDebugger]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/5.png
[LogsOutputWindow]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/11.png
[LogsServerExplorer]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/12.png
[LogsPortal]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/13.png
[HelpConflict]: ./media/mobile-services-dotnet-backend-how-to-troubleshoot/14.png


<!-- Links -->
[SymbolSource]:http://symbolsource.org
