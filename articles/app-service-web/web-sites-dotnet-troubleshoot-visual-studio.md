<properties 
    pageTitle="Visual Studio を使用した Azure App Service のトラブルシューティング" 
    description="Visual Studio 2013 に組み込まれているリモート デバッグ、トレース、ログ記録のツールを使用して、Azure の Web アプリケーションのトラブルシューティングを行う方法について説明します。" 
    services="app-service" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/16/2015" 
    ms.author="tdykstra"/>

# Visual Studio を使用した Azure App Service のトラブルシューティング

## 概要

このチュートリアルで実行中の web アプリのデバッグに役立つ Visual Studio ツールを使用する方法を示します [App Service](http://go.microsoft.com/fwlink/?LinkId=529714), でを実行して [デバッグ モード](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) リモートまたはアプリケーション ログ、および web サーバーのログを表示しています。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

学習内容:

* Visual Studio から利用できる Azure Web アプリケーション管理機能。
* Visual Studio のリモート ビューを使用して、リモート Web アプリケーションにすぐに変更を加える方法。
* Azure でプロジェクトが実行されているときに、Web アプリケーションと Web ジョブ両方をリモートからデバッグ モードで実行する方法。
* アプリケーションのトレース ログを作成する方法と、ログが作成されている最中にそれらを確認する方法。
* Web サーバーのログ (詳細なエラー メッセージ、失敗した要求トレースを含む) を確認する方法。
* Azure のストレージ アカウントに診断ログを送り、そこでログを確認する方法。

Visual Studio Ultimate がある場合も行えます [IntelliTrace](http://msdn.microsoft.com/library/vstudio/dd264915.aspx) デバッグします。 IntelliTrace については、このチュートリアルでは説明しません。

## <a name="prerequisites"></a>前提条件

このチュートリアルでは、開発環境、web プロジェクト、および Azure web アプリでの設定を [Azure と ASP.NET を使ってみる][GetStarted]します。 Web ジョブのセクションで、アプリケーションで作成する必要があります [Azure web ジョブ SDK を使ってみる][GetStartedWJ]します。

このチュートリアルで示すコード サンプルは、C# MVC Web アプリケーションに対応していますが、トラブルシューティング手順は Visual Basic および Web フォームの各アプリケーションでも同じです。

リモート デバッグには、Visual Studio 2013 または Visual Studio 2012 Update 4 が必要です。 リモート デバッグと **サーバー エクスプ ローラー** 機能の web ジョブ必要 [Visual Studio 2013 Update 4](http://go.microsoft.com/fwlink/?LinkID=510314) またはそれ以降。 このチュートリアルで取り上げるその他の機能も Visual Studio 2013 Express for Web および Visual Studio 2012 Express for Web で動作します。 

ストリーミング ログ機能は、.NET Framework 4 以降を対象とするアプリケーションでのみ動作します。

## <a name="sitemanagement"></a>Web アプリケーションの構成と管理

Visual Studio で使用できる構成設定、web アプリ管理機能のサブセットへのアクセスには、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715)します。 このセクションでは、その対象となる機能や設定について取り上げます。

1. 既に Visual Studio で Azure にサインインしていない場合はクリックして、 **Azure への接続** ボタン **サーバー エクスプ ローラー**します。

    または、アカウントへのアクセスを可能にする管理証明書をインストールします。 証明書をインストールする場合を右クリックし、 **Azure** 内のノード **サーバー エクスプ ローラー**, 、] をクリックし、 **サブスクリプションの管理** 、コンテキスト メニューです。  **Azure サブスクリプションの管理** ダイアログ ボックスで、をクリックして、 **証明書** タブをクリックし、をクリックし、 **インポート**します。 ダウンロードして、サブスクリプション ファイルをインポートする手順に従います (とも呼ばれます、 *.publishsettings* ファイル)、Azure アカウント用です。

    > [AZURE.NOTE]
    > サブスクリプション ファイルをダウンロードする場合は、ソース コード ディレクトリの外にあるフォルダー (Downloads フォルダーなど) に保存し、インポートが完了したらそのファイルを削除します。 悪意のあるユーザーがサブスクリプション ファイルへのアクセス許可を取得すると、Azure サービスを編集、作成、削除できるためです。

    Visual Studio から Azure リソースへの接続に関する詳細については、次を参照してください。 [アカウントの管理、サブスクリプション、および管理者の役割](http://go.microsoft.com/fwlink/?LinkId=324796#BKMK_AccountVCert)します。

2.  **サーバー エクスプ ローラー**, 、展開 **Azure** 展開 **App Service**します。

3. 作成した web アプリを含むリソース グループを展開し [Azure と ASP.NET の概要][GetStarted], 、web アプリのノードを右クリックして、クリックして **設定の表示**します。

    ![サーバー エクスプローラーの [設定の表示]](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewsettings.png)

     **Azure Web App** ] タブが表示され、存在して Visual Studio で利用できる web アプリの管理と構成タスクです。

    ![Azure Web アプリ ウィンドウ](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configtab.png)

    このチュートリアルでは、ログとトレースのドロップダウンを使用します。 また、リモート デバッグも使用し、さまざまな方法でこの機能を有効にします。
   
    このウィンドウで、アプリケーション設定と接続文字列のボックスの詳細については、次を参照してください。 [Azure Web Apps: アプリケーション文字列と接続文字列の動作](http://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)します。

    このウィンドウでは実行できない web アプリの管理タスクを実行する場合は、クリックして **管理ポータルで開く** Azure クラシック ポータルをブラウザー ウィンドウを開きます。 詳細については、次を参照してください。 [Web アプリの構成方法](/manage/services/web-sites/how-to-configure-websites/#howtochangeconfig)します。

## <a name="remoteview"></a>サーバー エクスプローラーでの Web アプリケーション ファイルへのアクセス

通常は、Web.config ファイルで `customErrors` フラグを `On` または `RemoteOnly` に設定して Web プロジェクトをデプロイします。これは、何か問題が発生したときに、役に立つエラー メッセージを表示しないことを意味します。 表示されるエラーの多くは、次のようなページになります。

**'/' アプリケーションのサーバー エラー:**

![あまり役に立たないエラー ページ](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror.png)

**エラーが発生しました:**

![あまり役に立たないエラー ページ](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror1.png)

**Web サイト側でページを表示できません**

![あまり役に立たないエラー ページ](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror2.png)

多くの場合、エラーの原因を最も簡単に見つける方法は、詳細なエラー メッセージを有効にすることです。その結果、既に示した最初のスクリーンショットのように、対応方法が表示されます。 ここでは、デプロイした Web.config ファイルの変更が必要です。 編集することが、 *Web.config* ファイルをプロジェクトと、プロジェクトを再配置または作成、 [Web.config の変換](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) とデバッグ ビルドをデプロイより簡単な方法がある: で **ソリューション エクスプ ローラー** 直接表示しを使用して、リモート web アプリケーション内のファイルを編集することができます、 *リモート ビュー* 機能します。

1.  **サーバー エクスプ ローラー**, 、展開 **Azure**, 、展開 **App Service**, 、web アプリがある、リソース グループを展開し、web アプリのノードを展開します。

    Web アプリケーションのコンテンツ ファイルとログ ファイルへのアクセス許可を付与するノードが表示されます。

2. 展開、 **ファイル** ] ノードをダブルクリック、 *Web.config* ファイルです。

    ![[Web.config] を開く](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfig.png)

    Visual Studio は、リモート Web アプリケーションから Web.config ファイルを開き、タイトル バーではファイル名 の横に [リモート] と表示されています。

3. 次の行を `system.web` 要素に追加します。

    `<customErrors mode="off"></customErrors>`

    ![Web.config の編集](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfigedit.png)

4. あまり役に立たないエラー メッセージを示しているブラウザーを最新の情報に更新すると、次の例のような詳細なエラー メッセージが表示されます。

    ![詳細なエラー メッセージ](./media/web-sites-dotnet-troubleshoot-visual-studio/detailederror.png)

    (示されているエラーが赤で表示されている行を追加することによって作成された *Views\Home\Index.cshtml*.)

Web.config ファイルを編集することは、Azure Web アプリケーションにあるファイルの読み取りと編集を可能にして、トラブルシューティングが容易にする状況における 1 つの例にすぎません。

## <a name="remotedebug"></a>Web アプリケーションのリモート デバッグ

詳細なエラー メッセージで十分な情報が表示されておらず、エラーをローカルで再現できない場合は、トラブルシューティングの別の方法として、リモートでデバッグ モードを実行することができます。 ブレークポイントの設定、メモリの直接操作、コードのステップ実行、さらにコード パスの変更を実行できます。 

リモート デバッグは、Visual Studio の各 Express Edition では機能しません。

このセクションで作成したプロジェクトを使用してリモートでデバッグする方法を示しています。 [Azure と ASP.NET の概要][GetStarted]します。

1. 作成した web プロジェクトを開く [Azure と ASP.NET の概要][GetStarted]します。

1. 開いている *controllers \homecontroller.cs*します。

2. `About()` メソッドを削除し、その位置に次のコードを挿入します。

        public ActionResult About()
        {
            string currentTime = DateTime.Now.ToLongTimeString();
            ViewBag.Message = "The current time is " + currentTime;
            return View();
        }

2. [ブレークポイントを設定する](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) 上、 `ViewBag.Message` 行です。

1.  **ソリューション エクスプ ローラー**, 、プロジェクトを右クリックし、クリックして、 **発行**します。

2.  **プロファイル** ドロップダウン リストで、同じプロファイルで使用したものを選択 [Azure と ASP.NET の概要][GetStarted]します。

3. クリックして、 **設定** タブをクリックし、変更 **構成** に **デバッグ**, 、順にクリック **発行**します。

    ![デバッグ モードでの発行](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-publishdebug.png)

4. デプロイが完了し、ブラウザーが起動して Web アプリケーションの Azure URL が表示されたら、ブラウザーを閉じます。

5. Visual Studio 2013 のみ: で **サーバー エクスプ ローラー**, 、web アプリを右クリックし、 **[デバッガーの**です。 

    ![デバッガーの接続](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-attachdebugger.png)

    ブラウザーが自動的に起動し、Azure で実行されているホーム ページが表示されます。 デバッグに必要な設定を Azure がサーバーに対して行う間、20 秒ほどの待ち時間が生じることがあります。 この待ち時間が生じるのは、Web アプリケーションでのデバッグ モードの初回実行時に限られます。 以後 48 時間は、デバッグを再度実行しても、待ち時間は生じません。

6. Visual Studio 2012 Update 4 以降。<a id="vs2012"></a>

    * Azure クラシック ポータルに移動 **設定 > アプリケーション設定** web アプリ、および [下へスクロールして、 **デバッグ** セクションです。

    * 設定 **リモート デバッグ** に **に**, 、設定と **Visual Studio のバージョン** に **2012年**します。

    * Visual Studio で **デバッグ** ] メニューのをクリックして **プロセスにアタッチする**です。

    *  **修飾子** ボックスでなく、web アプリの URL を入力、 `http://` プレフィックス。 

    * 選択 **プロセスを表示するすべてのユーザー**します。

    * 資格情報を求められたら、Web アプリケーションにコンテンツをデプロイする権限のあるユーザー名とパスワードを入力します。 これらの資格情報を取得するには、クラシック ポータルで web アプリのダッシュ ボード] タブに移動し、をクリックして **発行プロファイルのダウンロード**します。 テキスト エディターでファイルを開き、1 つ目の後にユーザー名とパスワードを検索します **userName =** と **userPWD =**です。 

    * プロセスを表示したときに、 **選択可能なプロセス** テーブルで **w3wp.exe**, 、] をクリックし、 **アタッチ**します。

    * ブラウザーを開き、Web アプリケーションの URL に移動します。

    デバッグに必要な設定を Azure がサーバーに対して行う間、20 秒ほどの待ち時間が生じることがあります。 この待ち時間が生じるのは、Web アプリケーションでのデバッグ モードの初回実行時に限られます。 以後 48 時間は、デバッグを再度実行しても、待ち時間は生じません。

6. クリックして **に関する** メニューにします。

    Visual Studio がブレークポイントで停止します。コードが実行されている場所は、ローカル コンピューターではなく Azure 上です。

7. `currentTime` 変数にマウスを合わせて、時刻値を表示します。

    ![Azure で実行されているコード内の変数をデバッグ モードで表示](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugviewinwa.png)

    表示される時刻は、Azure サーバーの時刻です。ローカル コンピューターとはタイム ゾーンが異なります。

8. `currentTime` 変数に新しい値 ("Now running in Azure" など) を入力します。

5. F5 キーを押して実行を継続します。

    Azure で実行中の [About] ページに、先ほど currentTime 変数に対して入力した新しい値が表示されます。

    ![[About] ページ (値の変更後)](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugchangeinwa.png)

## <a name="remotedebugwj"></a> Web ジョブのリモート デバッグ

このセクションで作成したプロジェクトと web アプリを使用してリモートでデバッグする方法を示しています。 [Azure web ジョブ SDK を使ってみる](websites-dotnet-webjobs-sdk.md)します。 このセクションで示す機能は、Visual Studio 2013 Update 4 以降でのみ使用できます。 リモート デバッグは、継続的な Web ジョブでのみ動作します。 スケジュールされたオンデマンドの Web ジョブでは、デバッグはサポートされていません。

1. 作成した web プロジェクトを開く [Azure web ジョブ SDK を使ってみる][GetStartedWJ]します。

1. ContosoAdsWebJob プロジェクトで開く *Functions.cs*します。

2. [ブレークポイントを設定する](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) の最初のステートメントで、 `GnerateThumbnail` メソッドです。

    ![ブレークポイントの設定](./media/web-sites-dotnet-troubleshoot-visual-studio/wjbreakpoint.png)

1.  **ソリューション エクスプ ローラー**, web プロジェクト (web ジョブ プロジェクトではない) を右クリックし、クリックして、 **発行**します。

2.  **プロファイル** ドロップダウン リストで、同じプロファイルで使用したものを選択 [Azure web ジョブ SDK を使ってみる](websites-dotnet-webjobs-sdk.md)します。

3. クリックして、 **設定** タブをクリックし、変更 **構成** に **デバッグ**, 、順にクリック **発行**します。

    Visual Studio によって Web プロジェクトと Web ジョブ プロジェクトがデプロイされ、ブラウザーで Web アプリケーションの Azure URL が表示されます。

5.  **サーバー エクスプ ローラー** 展開 **Azure > App Service >、リソース グループ > web アプリ > web ジョブ > Continuous**, を右クリックし、 **ContosoAdsWebJob**します。

7. クリックして **デバッガーをアタッチ**します。 

    ![デバッガーの接続](./media/web-sites-dotnet-troubleshoot-visual-studio/wjattach.png)

    ブラウザーが自動的に起動し、Azure で実行されているホーム ページが表示されます。 デバッグに必要な設定を Azure がサーバーに対して行う間、20 秒ほどの待ち時間が生じることがあります。 この待ち時間が生じるのは、Web アプリケーションでのデバッグ モードの初回実行時に限られます。 次回のデバッガーの接続では、初回の実行から 48 時間以内の場合、待ち時間は発生しません。

6. Contoso Ads ホーム ページを表示している Web ブラウザーで、新しい広告を作成します。 

    広告を作成すると、キュー メッセージが作成されます。キュー メッセージは Web ジョブによって取得され、処理されます。 Web ジョブ SDK がキュー メッセージを処理する関数を呼び出すと、コードがブレークポイントにヒットします。

7. デバッガーがブレークポイントで停止すると、プログラムがクラウドを実行している間に、変数の値を確認、変更することができます。 次の図では、デバッガーは、GenerateThumbnail メソッドに渡された blobInfo オブジェクトの内容を示しています。

    ![デバッガーの blobInfo オブジェクト](./media/web-sites-dotnet-troubleshoot-visual-studio/blobinfo.png)
 
5. F5 キーを押して実行を継続します。

    GenerateThumbnail メソッドはサムネイルの作成を完了します。

6. ブラウザーで、[インデックス] ページを更新して、サムネイルを表示します。

6. Visual Studio で、Shift キーを押しながら F5 キーを押してデバッグを停止します。

7.  **サーバー エクスプ ローラー**, 、ContosoAdsWebJob ノードを右クリックし、クリックして、 **ダッシュ ボードの表示**します。

8. Azure の資格情報を使用してサインインし、Web ジョブ名をクリックして、Web ジョブのページに移動します。 

    ![ContosoAdsWebJob をクリック](./media/web-sites-dotnet-troubleshoot-visual-studio/clickcaw.png)

    GenerateThumbnail 関数が最近実行されたことがダッシュボードに示されます。

    (クリックする、次回 **ダッシュ ボードの表示**, 、サインインする必要はありませんし、ブラウザーが web ジョブのページに直接移動します)。

9. 関数名をクリックすると、関数の実行について詳細が表示されます。

    ![機能の詳細](./media/web-sites-dotnet-troubleshoot-visual-studio/funcdetails.png)

場合、関数 [ログを書き込んだ](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs), 、クリックすることも **ToggleOutput** それを確認します。

## リモート デバッグに関する注意

* 運用環境におけるデバッグ モードの実行はお勧めできません。 運用環境の Web アプリケーションが複数のサーバー インスタンスにスケール アウトされていない場合、デバッグを行うと、Web サーバーが他の要求に応答できなくなります。 しかし、Web サーバーのインスタンスが複数存在する場合、デバッガーのアタッチ先となるインスタンスは無作為に決定されるため、そのインスタンスに後続のブラウザーの要求を確実に渡すことができません。 また、運用環境にデバッグ ビルドをデプロイすることも一般的ではありません。リリース ビルドに対してはコンパイラが最適化を行うため、ソース コードの状況を行レベルで把握することは不可能です。 運用環境の問題をトラブルシューティングするのに最も適しているリソースは、アプリケーション トレースと Web サーバーのログです。

* リモート デバッグ時、ブレークポイントで長時間停止させることは避けてください。 数分以上停止しているプロセスは、応答していないプロセスと見なされ、Azure によりシャットダウンされます。

* デバッグ中は、サーバーから Visual Studio にデータが送信されるため、帯域幅の使用料に影響が及ぶ可能性があります。 帯域幅使用料については、次を参照してください。 [Azure 料金計算ツール](/pricing/calculator/)します。

* 確認して、 `debug` の属性、 `compilation` 内の要素、 *Web.config* ファイルの設定を true にします。 デバッグ ビルド構成で発行するときは、true が既定値です。

        <system.web>
          <compilation debug="true" targetFramework="4.5" />
          <httpRuntime targetFramework="4.5" />
        </system.web>

* デバッグ対象となるコードにデバッガーがステップ インしない場合、[マイ コードのみ] の設定を変更してみてください。  詳細については、次を参照してください。 [ステップ実行をマイ コードのみに制限する](http://msdn.microsoft.com/library/vstudio/y740d9d3.aspx#BKMK_Restrict_stepping_to_Just_My_Code)です。

* リモート デバッグ機能を有効にしたときに、サーバー上でタイマーが開始され、48 時間後にこの機能が自動的に無効になります。 この 48 時間の上限はセキュリティとパフォーマンス上の理由で設定されています。 必要に応じて、この機能を何回でも簡単に有効に戻すことができます。 積極的にデバッグを実行している場合以外は、この機能を無効にしたままにすることをお勧めします。

* 手動でデバッガーを Web アプリケーション プロセス (w3wp.exe) だけでなく、任意のプロセスに接続できます。 Visual Studio でデバッグ モードを使用する方法の詳細については、次を参照してください。 [Visual Studio でのデバッグ](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx)します。

## <a name="logsoverview"></a>診断ログの概要

Azure の Web アプリケーションで動作する ASP.NET アプリケーションでは、次の種類のログが作成されます。

* **アプリケーション トレース ログ**<br/>
  アプリケーションでは、これらのログを作成のメソッドを呼び出すことによって、 [System.Diagnostics.Trace](http://msdn.microsoft.com/library/system.diagnostics.trace.aspx) クラスです。
* **Web サーバーのログ**<br/>
  Web サーバーは、Web アプリケーションに届くすべての HTTP 要求について、それぞれログ エントリを作成します。
* **詳細なエラー メッセージ ログ**<br/>
  失敗した HTTP 要求 (ステータス コード 400 以上の要求) について、より詳しい情報を記した HTML ページが Web サーバーによって作成されます。 
* **失敗した要求トレース ログ**<br/>
  失敗した HTTP 要求についての詳しいトレース情報を記録した XML ファイルが Web サーバーによって作成されます。 また、ブラウザーで XML の体裁を設定するための XSL ファイルも作成されます。
  
ログ出力は Web アプリケーションのパフォーマンスに影響を及ぼすため、Azure では、必要に応じてログの種類ごとにその有効と無効を切り替えることができるようになっています。 アプリケーション ログについては、特定の重大度レベルを超えるログだけを記録するように指定できます。 新しい Web アプリケーションを作成した時点ではすべてのログが既定で無効になります。

内のファイルにログが書き込まれる、 *LogFiles* 、FTP 経由でアクセス可能な web アプリのファイル システム内のフォルダーです。 Web サーバーのログとアプリケーションのログは、Azure のストレージ アカウントに出力することもできます。 ストレージ アカウントには、ファイル システムよりも大量のログを保持することができます。 ファイル システムを使用した場合、保存できるログの上限は 100 MB です。 (ファイル システムのログは、短期間のみ保持されます。 上限に達すると、古いログ ファイルは削除され、新しいログ ファイルのための領域が確保されます。)  

## <a name="apptracelogs"></a>アプリケーションのトレース ログの作成と表示

このセクションでは、次のタスクを実行します。

* 作成した web プロジェクトにトレース ステートメントを追加 [Azure と ASP.NET を使ってみる][GetStarted]します。
* プロジェクトをローカル実行したときのログを確認します。
* Azure で実行中のアプリケーションによって生成されたログを確認します。 

アプリケーションを作成する方法については、web ジョブでログを参照してください [Azure キュー ストレージを操作する方法、web ジョブ SDK のログを書き込む方法を使用して](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)します。 ログの表示とログを Azure に格納する方法の制御に関する次の手順は、Web ジョブによって作成されたアプリケーション ログにも適用されます。 

### アプリケーションへのトレース ステートメントの追加

1. 開いている *controllers \homecontroller.cs*, 、し、次のコードを追加するために、ファイルの内容に置き換えます `Trace` ステートメントと `using` の声明 `System.Diagnostics`:

        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.Diagnostics;
        using System.Linq;
        using System.Web;
        using System.Web.Configuration;
        using System.Web.Mvc;
        namespace MyExample.Controllers
        {
            public class HomeController : Controller
            {
                public ActionResult Index()
                {
                    Trace.WriteLine("Entering Index method");
                    ViewBag.Message = "Modify this template to jump-start your ASP.NET MVC application.";
                    Trace.TraceInformation("Displaying the Index page at " + DateTime.Now.ToLongTimeString());
                    Trace.WriteLine("Leaving Index method");
                    return View();
                }
        
                public ActionResult About()
                {
                    Trace.WriteLine("Entering About method");
                    ViewBag.Message = "Your app description page.";
                    Trace.TraceWarning("Transient error on the About page at " + DateTime.Now.ToShortTimeString());
                    Trace.WriteLine("Leaving About method");
                    return View();
                }
        
                public ActionResult Contact()
                {
                    Trace.WriteLine("Entering Contact method");
                    ViewBag.Message = "Your contact page.";
                    Trace.TraceError("Fatal error on the Contact page at " + DateTime.Now.ToLongTimeString());
                    Trace.WriteLine("Leaving Contact method");
                    return View();
                }
            }
        }
        
                
### ローカルでのトレース出力の表示

3. F5 キーを押してデバッグ モードでアプリケーションを実行します。

    既定のトレース リスナーにすべてのトレース出力の書き込み、 **出力** ウィンドウで、その他のデバッグ出力と一緒にします。 次の図は、`Index` メソッドに追加したトレース ステートメントからの出力結果を示したものです。

    ![[デバッグ] ウィンドウでのトレース](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugtracing.png)

    以降の手順では、コンパイルせずにデバッグ モードで、トレース出力を Web ページに表示する方法を紹介します。

2. プロジェクト フォルダーにあるアプリケーションの Web.config ファイルを開き、ファイル末尾の終了 `</configuration>` 要素の直前に `<system.diagnostics>` 要素を追加します。

        <system.diagnostics>
            <trace>
              <listeners>
                <add name="WebPageTraceListener"
                    type="System.Web.WebPageTraceListener, 
                    System.Web, 
                    Version=4.0.0.0, 
                    Culture=neutral,
                    PublicKeyToken=b03f5f7f11d50a3a" />
              </listeners>
            </trace>
          </system.diagnostics>

    `WebPageTraceListener` を使用すると、ブラウザーから `/trace.axd` にアクセスすることでトレース出力を表示できます。

3. Web.config ファイルの `<system.web>` に、次のような <a href="http://msdn.microsoft.com/library/vstudio/6915t83k(v=vs.100).aspx">trace 要素</a>を追加します。

        <trace enabled="true" writeToDiagnosticsTrace="true" mostRecent="true" pageOutput="false" />

3. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

4. ブラウザー ウィンドウのアドレス バーに追加 *trace.axd* URL、および、」を入力 (URL 例: http://localhost:53370/trace.axd) キーを押します。

5.  **アプリケーション トレース** ] ページで [ **の詳細の表示** 最初の行 (BrowserLink の行とは異なる) にします。

    ![trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd1.png)

     **要求の詳細** ページが表示されたら、[、 **トレース情報** セクションに追加したトレース ステートメントからの出力を参照してください、 `Index` メソッドです。

    ![trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd2.png)

    既定では、`trace.axd` の使用はローカルに限られます。 追加するかどうかは、リモート web アプリから使用可能にすると、 `localOnly="false"` に、 `trace` 内の要素、 *Web.config* ファイルは次の例に示すようにします。

        <trace enabled="true" writeToDiagnosticsTrace="true" localOnly="false" mostRecent="true" pageOutput="false" />

    ただしセキュリティ上の理由から、運用 Web アプリで `trace.axd` を有効にすることは、通常お勧めできません。もっと簡単に Azure の Web アプリのトレース ログを確認する方法を以降のセクションで説明します。

### Azure でのトレース出力の確認

1.  **ソリューション エクスプ ローラー**, web プロジェクトを右クリックし、クリックして、 **発行**します。

2.  **Web の発行** ] ダイアログ ボックスをクリックして **発行**します。

    ホーム ページにブラウザー ウィンドウを開いて、Visual Studio プロジェクトの更新を発行後 (クリアしたと仮定して **送信先 URL** 上、 **接続** ] タブ)。

3.  **サーバー エクスプ ローラー**, を web アプリを右クリックし、選択 **[ストリーミング ログ**します。 

    ![コンテキスト メニューからストリーミング ログを表示](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewlogsmenu.png)

     **出力** ログ ストリーミング サービスに接続し、その都度、ログを表示することがなく 1 分あたりの行の通知を追加] ウィンドウに表示します。

    ![コンテキスト メニューからストリーミング ログを表示](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-nologsyet.png)

4. アプリケーションのホーム ページを表示するブラウザー ウィンドウで [ **連絡先**します。

    数秒でエラー レベルからの出力に追加したトレース、 `Contact` メソッドに表示されます、 **出力** ウィンドウです。

    ![出力ウィンドウのエラー トレース](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-errortrace.png)

    Visual Studio にエラー レベルのトレースしか表示されていないのは、それが、ログ監視サービスを有効にしたときの既定の設定であるためです。 先ほどの設定ページを開いたときに見たように、新しい Azure の Web アプリケーションを作成したときは、すべてのログが既定で無効となります。

    ![アプリケーション ログ記録がオフ](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-apploggingoff.png)


    However, when you selected **View Streaming Logs**, Visual Studio automatically changed **Application Logging(File System)** to **Error**, which means error-level logs get reported. In order to see all of your tracing logs, you can change this setting to **Verbose**. When you select a severity level lower than error, all logs for higher severity levels are also reported. So when you select verbose, you also see information, warning, and error logs.  

4.  **サーバー エクスプ ローラー**, web アプリを右クリックし、[クリックして **設定の表示** 前と同じです。

5. 変更 **アプリケーション ログ記録 (ファイル システム)** に **Verbose**, 、クリックして **保存**します。
 
    ![トレース レベルを [詳細] に設定](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-applogverbose.png)

6. 表示されているブラウザー ウィンドウで、 **連絡先** ] ページで [ **ホーム**, 、順にクリックして **に関する**, 、順にクリック **連絡先**します。

    数秒すると、 **出力** ウィンドウがすべてのトレース出力を示しています。

    ![詳細レベルのトレース出力](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-verbosetraces.png)

    このセクションでは、ログの有効と無効の切り替えを Azure の Web アプリケーションの設定で行いました。 トレース リスナーの有効と無効は、Web.config ファイルで設定することもできます。 ただし、Web.config ファイルに変更を加えると、アプリケーション ドメインの再利用処理が実行されます。一方、Web アプリケーションの構成を介してログを有効にした場合は、そのようにはなりません。 問題の再現に時間がかかる場合や、発生のタイミングが散発的である場合、アプリケーション ドメインの再利用処理で問題が "解消" し、再発までしばらく待たなければならなくなります。 Azure で診断を有効にすると、そのようなことにはならず、すぐにエラー情報を収集することができます。

### 出力ウィンドウの機能

 **Azure ログ** のタブ、 **出力** ウィンドウがいくつかのボタンとテキスト ボックス。

![[ログ] タブのボタン](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-icons.png)

それぞれの機能を次に示します。

* クリア、 **出力** ウィンドウです。
* 右端での折り返しを有効または無効にする。
* ログの監視を開始または停止する。
* 監視するログを指定する。
* ログをダウンロードする。
* 検索文字列または正規表現によるフィルターをログに適用する。
* 閉じる、 **出力** ウィンドウです。

検索文字列または正規表現を入力した場合、ログ情報は、Visual Studio によってクライアント側でフィルタリングされます。 つまり、条件を入力するには、ログが表示された後、 **出力** ウィンドウを変更するフィルターの条件をログを再生成する必要はありません。

## <a name="webserverlogs"></a>Web サーバーのログの表示

Web サーバーのログには、Web アプリケーション上の HTTP アクティビティがすべて記録されます。 表示するために、 **出力** web アプリを有効にして、サーバーを監視することを Visual Studio に指示する必要があるウィンドウです。 

1.  **Azure Web アプリの構成** からを開いているタブ **サーバー エクスプ ローラー**, に Web サーバーのログ記録を変更する **に**, 、] をクリックし、 **保存**します。

    ![Web サーバーのログ記録を有効にする](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-webserverloggingon.png)

2.  **出力** ウィンドウで、をクリックして、 **監視する Azure ログの指定** ] ボタンをクリックします。
    
    ![監視する Azure ログの指定](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-specifylogs.png)

3.  **Azure ログ オプション** ダイアログ ボックスで、 **Web サーバー ログ**, 、順にクリック **OK**します。

    ![Web サーバーのログを監視する](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorwslogson.png)

4. Web アプリを表示するブラウザーのウィンドウでをクリックして **ホーム**, 、] をクリックし、 **に関する**, 、] をクリックし、 **連絡先**します。

    通常はアプリケーションのログが先に表示され、続けて Web サーバーのログが表示されます。 ログが表示されるまでにしばらく時間がかかる場合があります。 

    ![出力ウィンドウの Web サーバーのログ](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-wslogs.png)


Visual Studio で Web サーバーのログを初めて有効にしたとき、既定では、Azure によってファイル システムにログが書き込まれます。 代わりに、クラシック ポータルを使用して、Web サーバーのログの書き込み先として、ストレージ アカウントの BLOB コンテナーを指定することもできます。 詳細については、次を参照してください。、 **サイト診断** セクション [Web サイトを構成する方法](/manage/services/web-sites/how-to-configure-websites/#howtochangeconfig)します。 

クラシック ポータルを使用して、Azure ストレージ アカウントへの Web サーバーのログ記録を有効にした後、Visual Studio でログ記録を無効にした場合、Visual Studio でログ記録を再度有効にすると、ストレージ アカウントの設定が復元されます。 

## <a name="detailederrorlogs"></a>詳細なエラー メッセージ ログの表示

詳細なエラー ログでは、エラー応答コード (400 以上) が返された HTTP 要求について、いくつかの詳しい情報が確認できます。 表示するために、 **出力** ウィンドウで、web アプリを有効にして、サーバーを監視することを Visual Studio に指示する必要があります。

1.  **Azure Web アプリの構成** からを開いているタブ **サーバー エクスプ ローラー**, 、変更 **詳細なエラー メッセージ** に **に**, 、順にクリック **保存**します。

    ![詳細なエラー メッセージを有効にする](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailedlogson.png)

2.  **出力** ウィンドウで、をクリックして、 **監視する Azure ログの指定** ] ボタンをクリックします。

3.  **Azure ログ オプション** ダイアログ ボックスで、をクリックして **すべてのログ**, 、] をクリックし、 **[ok]**します。

    ![すべてのログを監視する](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorall.png)

4. ブラウザー ウィンドウのアドレス バーで、404 エラーの原因となるような余分な文字を URL に追加し (例: `http://localhost:53370/Home/Contactx`)、Enter キーを押します。

    Visual Studio で表示される詳細なエラー ログを数秒後 **出力** ウィンドウです。

    ![出力ウィンドウの詳細なエラー ログ](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorlog.png)

    Ctrl キーを押しながらリンクをクリックすると、次のような書式化されたログ出力がブラウザーに表示されます。

    ![ブラウザー ウィンドウの詳細なエラー ログ](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorloginbrowser.png)

## <a name="downloadlogs"></a>ファイル システムのログのダウンロード

監視できるすべてのログ、 **出力** としてウィンドウをダウンロードすることも、 *.zip* ファイルです。 

1.  **出力** ウィンドウで、をクリックして **ストリーミング ログのダウンロード**します。

    ![[ログ] タブのボタン](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadicon.png)

    ファイル エクスプ ローラーが開いて、 *ダウンロード* ダウンロード済みのファイルが選択されているフォルダーです。

    ![ダウンロードされたファイル](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadedfile.png)

2. 抽出、 *.zip* 、そのファイルは、次のフォルダー構造を参照してください。

    ![ダウンロードされたファイル](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilefolders.png)

    * アプリケーション トレース ログは *.txt* にファイルを *logfiles \application* フォルダーです。
    * Web サーバーのログは *.log* にファイルを *logfiles \http\rawlogs* フォルダーです。 などのツールを使用する [Log Parser](http://www.microsoft.com/download/details.aspx?displaylang=en&id=24659) を表示およびこれらのファイルを操作します。
    * 詳細なエラー メッセージのログは *.html* にファイルを *logfiles \detailederrors* フォルダーです。

    (、 *展開* ソース管理の発行によって作成されるファイルのフォルダーは、Visual Studio の発行に関連はありません。  *Git* フォルダーは、ソース管理に関連したトレースの発行、およびログ ファイル ストリーミング サービス)。  

## <a name="storagelogs"></a>ストレージ ログの表示

アプリケーション トレース ログを Azure のストレージ アカウントに送って、Visual Studio で表示することもできます。 ストレージ アカウントを作成するには、従来のポータルでストレージ ログを有効にしでそれらを表示、 **ログ** のタブ、 **Azure Web App** ウィンドウです。

次の 3 個の送信先のいずれかまたはすべてにログを送信することができます。

* ファイル システム。
* ストレージ アカウント テーブル。
* ストレージ アカウント BLOB。

送信先ごとに異なる重要度レベルを指定することもできます。 

テーブル形式を使用すると、ログの詳細をオンラインで簡単に表示でき、またストリーミングもサポートされています。テーブル内にあるログを照会することや、作成されている新しいログを表示することもできます。 BLOB 形式を使用すると、ログをファイルにダウンロードし、HDInsight を使ってそれらのログを簡単に解析できるようになります。HDInsight は、BLOB ストレージを処理する方法を把握しているためです。 詳細については、次を参照してください。 **Hadoop and MapReduce** で [データ ストレージ オプション (Azure で構築実際のクラウド アプリケーション)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options)します。

現在、ファイル システムのログは詳細レベルに設定されています。次の手順では、ストレージ アカウント テーブルに切り替える目的で情報レベルのログを設定する方法を説明します。 情報レベルを指定した場合、`Trace.TraceInformation`、`Trace.TraceWarning`、および `Trace.TraceError` の呼び出しで作成されたすべてのログが表示され、`Trace.WriteLine` で作成されたログは除外されます。

多くの場合、ストレージ アカウント形式はファイル システム形式と比較して、ログに対してより多くのストレージと、より長い期間にわたる保持を実現します。 アプリケーション トレース ログをストレージに送る別の利点は、ファイル システムのログでは得ることのできない詳しい情報をそれぞれのログと共に入手できることです。

5. 右クリック **ストレージ** クリックして Azure ノード下にある **ストレージ アカウントの作成**します。

![ストレージ アカウントの作成](./media/web-sites-dotnet-troubleshoot-visual-studio/createstor.png)

3.  **ストレージ アカウントの作成** ダイアログ ボックスで、ストレージ アカウントの名前を入力します。 

    名前は一意である必要があります (Azure ストレージ アカウントで同じ名前は使用できません)。 入力した名前が既に使用されている場合、変更を求められます。

    ストレージ アカウントにアクセスする url は、 *{name}*..core.windows.net 形式です。 

5. 設定、 **リージョンまたはアフィニティ グループ** に最も近いリージョンをドロップダウン リストです。

    この設定は、お使いのストレージ アカウントをホストしている Azure データセンターを指定します。 このチュートリアルでは、選択肢によって顕著な相違は生じませんが、運用 Web アプリケーションの場合は、待ち時間とデータ送信料金を最小限に抑えるために、Web サーバーとストレージ アカウントを同じリージョンに設定することが望まれます。 待ち時間を最小限にするには、Web アプリケーション (後で作成する) を Web アプリケーションにアクセスするブラウザーのできるだけ近くで実行します。

6. 設定、 **レプリケーション** ドロップ ダウン リスト **ローカル冗長**します。 

    Geo レプリケーションをストレージ アカウントに対して有効にすると、1 次拠点で重大な障害が発生した場合に備えて、保存したコンテンツは 2 次データセンターに複製されて、フェイルオーバーが可能になります。 Geo レプリケーションには追加費用が発生する場合があります。 また、テストおよび開発アカウントの場合は、一般的に Geo レプリケーションに対する課金は避けたいと考えるでしょう。 詳細については、次を参照してください。 [の作成、管理、またはストレージ アカウントを削除](../storage-create-storage-account/#replication-options)します。

5. クリックして **作成**します。 

    ![新しいストレージ アカウント ](./media/web-sites-dotnet-troubleshoot-visual-studio/newstorage.png)  

1. Visual Studio で **Azure Web App** ウィンドウで、をクリックして、 **ログ** タブをクリックし、をクリックして **管理ポータルでログを構成する**です。

    <!-- todo:screenshot of new portal if the VS page link goes to new portal -->
    ![ログの構成](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configlogging.png)

    開き、 **構成** クラシック ポータルで web アプリのタブをクリックします。

2. 従来のポータルで **構成** ] タブで、アプリケーション診断] セクションまで下へスクロールし、変更し、 **アプリケーション ログ記録 (テーブル ストレージ)** に **に**します。

3. 変更 **ログ記録レベル** に **情報**します。

4. クリックして **テーブル ストレージの管理**します。

    ![[テーブル ストレージの管理] をクリックします。](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-stgsettingsmgmtportal.png)

     **アプリケーション診断用のテーブル ストレージを管理** ボックスで、1 つ以上の操作をした場合、ストレージ アカウントを選択できます。 新しいテーブルを作成するか、既存のテーブルを使用することができます。

    ![テーブル ストレージの管理](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-choosestorageacct.png)

6.  **アプリケーション診断用のテーブル ストレージを管理** ボックスは、ボックスを閉じるチェック マークをクリックします。

6. 従来のポータルで **構成** ] タブ、[ **保存**します。

7. アプリケーションの web アプリを表示するブラウザーのウィンドウでをクリックして **ホーム**, 、] をクリックし、 **に関する**, 、順にクリック **連絡先**します。

    これらの Web ページを閲覧することによって生成されたログ情報がストレージ アカウントに書き込まれます。

8.  **ログ** のタブ、 **Azure Web App** Visual Studio のウィンドウ] をクリックして **更新** [ **診断の概要**します。

    ![[最新の情報に更新] をクリックする](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-refreshstorage.png)

     **診断の概要** では、既定では、直近 15 分間ログを説明します。 この時間を変更することで、表示するログの量を増やすことができます。 

    (「テーブルが見つかりません」エラーが発生した場合は、有効にした後、トレース対象のページを参照したことを確認して **アプリケーション ログ記録 (ストレージ)** ] をクリックした後、 **保存**.)

    ![ストレージ ログ](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-storagelogs.png)

    この画面に通知を参照してください **プロセス ID** と **スレッド ID** ログごとに、ファイル システム ログですることはありません。 Azure のストレージ テーブルを直接参照することで、さらに別のフィールドを確認できます。

8. クリックして **すべてのアプリケーション ログの表示**します。

    Azure のストレージ テーブル ビューアーにトレース ログ テーブルが表示されます。
   
    (「シーケンスに要素が含まれていません」エラーが発生した場合は開きます **サーバー エクスプ ローラー**, 、ストレージ アカウントの下のノードを展開、 **Azure** ノード、および右クリック **テーブル** ] をクリック **更新**.)

    ![テーブル形式で表示されたストレージ ログ](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracelogtableview.png)

    この画面には、他の画面では確認できないフィールドが表示されます。 専用のクエリ ビルダー UI を使用してクエリを作成し、ログにフィルターを適用することもできます。 詳細については、「テーブル リソースの操作 - エンティティのフィルター処理の使用」を参照していますください。 [サーバー エクスプ ローラーを使用したストレージ リソース](http://msdn.microsoft.com/library/ff683677.aspx)します。

7. 単一の行の詳細を確認するには、行の 1 つをダブルクリックします。

    ![サーバー エクスプローラーのトレース テーブル](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracetablerow.png)

## <a name="failedrequestlogs"></a>失敗した要求トレース ログの表示

失敗した要求トレース ログは、URL の書き換えや認証の問題など、IIS による HTTP 要求の処理を詳しく把握する必要がある状況で活用できます。 

Azure の Web アプリでは、同じ失敗した要求トレース機能が使用されています。この機能は、IIS 7.0 以降で利用できます。 ただし、ログに記録するエラーを指定するための IIS 設定にアクセスする必要はありません。 失敗した要求トレースを有効にすると、すべてのエラーがキャプチャされます。 

失敗した要求トレースは Visual Studio を使用して有効にできますが、それらを Visual Studio で表示することはできません。 これらのログは XML ファイル形式になっています。 ストリーミング ログ サービスはプレーン テキスト モードで読み取り可能と見なされるファイルのみを監視します。  *.txt*, 、*.html*, 、および *.log* ファイルです。

失敗した要求トレース ログは、ブラウザーから FTP で直接表示できるほか、FTP ツールを使ってローカル コンピューターにダウンロードした後、ローカルで表示することもできます。 このセクションでは、ブラウザーで直接閲覧する方法を説明します。

1.  **構成** のタブ、 **Azure Web アプリ** から開かれたウィンドウ **サーバー エクスプ ローラー**, 、変更 **失敗した要求トレース** に **に**, 、順にクリック **保存**します。

    ![失敗した要求トレースを有効にする](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequeston.png)

4. Web アプリケーションが表示されているブラウザー ウィンドウのアドレス バーで、余分な文字を URL に追加し、Enter キーを押して 404 エラーを発生させます。

    これにより、失敗した要求トレース ログが作成されます。そのログを表示またはダウンロードする方法については、以降の手順で説明します。

2. Visual Studio での **構成** のタブ、 **Azure Web App** ウィンドウで、] をクリックして **管理ポータルで開く**します。

3. Web アプリの [Azure ポータル (https://portal.azure.com)] ブレードで [ **設定 > デプロイ資格情報**, 、新しいユーザー名とパスワードを入力します。

    ![新しい FTP ユーザー名とパスワード](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)

    **ログインする際は、完全なユーザー名とその Web アプリ名プレフィックスを使用する必要があります。 たとえば、ユーザー名として「myid」と入力し、サイトが "myexample" の場合、"myexample\myid" としてログインします。

5. 新しいブラウザー ウィンドウで下に表示されている URL に移動 **FTP ホスト名** または **[FTPS ホスト名** で、 **Web アプリ** web アプリのブレードです。 

6. 先ほど作成した FTP 資格情報を使用してログインします (ユーザー名の Web アプリケーション名プレフィックスを含めること)。

    Web アプリケーションのルート フォルダーがブラウザーに表示されます。

6. 開いている、 *LogFiles* フォルダーです。

    ![LogFiles フォルダーを開く](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilesfolder.png)

7. W3SVC に数値の付いた名前のフォルダーを開きます。

    ![W3SVC フォルダーを開く](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfolder.png)

    このフォルダーには、失敗した要求トレースを有効にした後で記録されたすべてのエラーの XML ファイルに加え、ブラウザーで XML の体裁を設定するための XSL ファイルが格納されています。

    ![W3SVC フォルダー](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfoldercontents.png)

8. トレース情報を確認する失敗した要求の XML ファイルをクリックします。

    次の図は、サンプル エラーのトレース情報を部分的に示したものです。

    ![失敗した要求トレースをブラウザーで表示したところ](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequestinbrowser.png)


## <a name="nextsteps"></a>次のステップ

Azure の Web アプリケーションで作成されたログは Visual Studio を使って簡単に参照できることが確認できました。 次のセクションでは、関連トピックに関する他のリソースへのリンクを紹介します。

* Azure の Web アプリケーションのトラブルシューティング
* Visual Studio でのデバッグ 
* Azure でのリモート デバッグ
* ASP.NET アプリケーションでのトレース
* Web サーバーのログの分析
* 失敗した要求トレース ログの分析
* Cloud Services のデバッグ

### Azure の Web アプリケーションのトラブルシューティング

Azure App Service の Web アプリのトラブルシューティングの詳細については、以下のリソースを参照してください。

* [Web アプリケーションの監視方法](/manage/services/web-sites/how-to-monitor-websites/)
* [Visual Studio 2013 での Azure Web アプリのメモリ リークの調査](http://blogs.msdn.com/b/visualstudioalm/archive/2013/12/20/investigating-memory-leaks-in-azure-web-sites-with-visual-studio-2013.aspx)します。 マネージされるメモリの問題の分析に役立つ Visual Studio の機能に関する Microsoft ALM のブログ記事
* [Azure の web アプリのオンライン ツールについて知っておくべき](/blog/2014/03/28/windows-azure-websites-online-tools-you-should-know-about-2/)します。 Amit Apple によるブログの投稿です。

具体的なトラブルシューティングについての質問は、次のいずれかのフォーラムで投稿してください。

* [ASP.NET サイト上の Azure フォーラム](http://forums.asp.net/1247.aspx/1?Azure+and+ASP+NET)します。
* [MSDN 上の Azure フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/)します。
* [StackOverflow.com](http://www.stackoverflow.com)します。

### Visual Studio でのデバッグ 

Visual Studio でデバッグ モードを使用する方法の詳細については、次を参照してください。、 [Visual Studio でのデバッグ](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx) MSDN のトピックと [Visual Studio 2010 でのデバッグのヒント](http://weblogs.asp.net/scottgu/archive/2010/08/18/debugging-tips-with-visual-studio-2010.aspx)します。

### Azure でのリモート デバッグ

Azure の Web アプリと Web ジョブのリモート デバッグの詳細については、以下のリソースを参照してください。

* [リモート デバッグ Azure App Service Web Apps の概要](/blog/2014/05/06/introduction-to-remote-debugging-on-azure-web-sites/)します。
* [Introduction to Remote Debugging Azure App Service Web Apps part 2 - Inside Remote debugging (Azure App Service の Web アプリケーションのリモート デバッグの概要 2 - リモート デバッグの内部処理)](/blog/2014/05/07/introduction-to-remote-debugging-azure-web-sites-part-2-inside-remote-debugging/)
* [Introduction to Remote Debugging on Azure App Service Web Apps part 3 - Multi-Instance environment and GIT (Azure App Service の Web アプリケーションのリモート デバッグの概要 3 - マルチインスタンス環境と GIT)](/blog/2014/05/08/introduction-to-remote-debugging-on-azure-web-sites-part-3-multi-instance-environment-and-git/)
* [WebJobs Debugging (Web ジョブのデバッグ) (ビデオ)](https://www.youtube.com/watch?v=ncQm9q5ZFZs&list=UU_SjTh-ZltPmTYzAybypB-g&index=1)

Web アプリで、Azure Web API またはモバイル サービス バックエンドを使用しを参照してください、デバッグを実行する必要がある場合 [Visual Studio での .NET バックエンドのデバッグ](http://blogs.msdn.com/b/azuremobile/archive/2014/03/14/debugging-net-backend-in-visual-studio.aspx)します。

### ASP.NET アプリケーションでのトレース

ASP.NET トレースに関しては、最新かつ必要な情報をすべて網羅した解説がインターネットには存在しません。 そのため、過去に作成された入門者向けの資料を参考にするのが最善の方法となります。MVC がまだ存在していなかったために Web フォームを想定して書かれていますが、具体的な問題については、最新のブログで情報を補うことができます。 たとえば、以下のリソースが参考になります。

* [監視と遠隔測定 (Azure で実際のクラウド アプリケーションのビルド)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry)します。<br> 
  Azure クラウド アプリケーションをトレースするためのベスト プラクティスを掲載した E-Book の章。
* [ASP.NET トレース](http://msdn.microsoft.com/library/ms972204.aspx)<br/>
  最新とは言えませんが、基本的な事柄がわかりやすくまとめられています。
* [トレース リスナー](http://msdn.microsoft.com/library/4y5y10s7.aspx)<br/>
  トレース リスナーの概要情報が、 [WebPageTraceListener](http://msdn.microsoft.com/library/system.web.webpagetracelistener.aspx)します。
* [チュートリアル: ASP.NET トレースの System.Diagnostics トレースとの統合](http://msdn.microsoft.com/library/b0ectfxd.aspx)<br/>
  古い情報ですが、入門記事では扱っていないような詳しい情報が記載されています。
* [ASP.NET MVC Razor ビューでのトレース](http://blogs.msdn.com/b/webdev/archive/2013/07/16/tracing-in-asp-net-mvc-razor-views.aspx)<br/>
  Razor ビューでのトレースに加え、MVC アプリケーションでハンドルされない例外をすべてログに記録するためのエラー フィルターの作成方法についても説明されています。 すべての未処理の例外を Web フォーム アプリケーションにログインする方法についてにされている Global.asax サンプルを参照してください。 [エラー ハンドラーの完全な例](http://msdn.microsoft.com/library/bb397417.aspx) MSDN にします。 MVC または Web フォームで、特定の例外をログに記録すると共に、既定のフレームワークの処理はそのまま活かしておく必要がある場合、例外を捕捉してから再スローする方法を利用できます。その例を次に示します。

        try
        {
           // Your code that might cause an exception to be thrown.
        }
        catch (Exception ex)
        {
            Trace.TraceError("Exception: " + ex.ToString());
            throw;
        } 

* [診断トレースから Azure コマンド ライン (と glimpse に関する情報!) ログのストリーミング](http://www.hanselman.com/blog/StreamingDiagnosticsTraceLoggingFromTheAzureCommandLinePlusGlimpse.aspx)<br/>
  このチュートリアルで Visual Studio を使って行ったことをコマンド ラインで行う方法が解説されています。 [Glimpse](http://www.hanselman.com/blog/IfYoureNotUsingGlimpseWithASPNETForDebuggingAndProfilingYoureMissingOut.aspx) ASP.NET アプリケーションをデバッグするためのツールです。 
* [Web Apps のログと診断] - David ebbo 作成を使用した](/documentation/videos/azure-web-site-logging-and-diagnostics/) と [Web アプリ - David ebbo 作成からストリーミング ログ](/documentation/videos/log-streaming-with-azure-web-sites/)<br>
  Scott Hanselman と David Ebbo によるビデオ。

トレース コードを記述する代わりにはエラー ログ記録など、オープン ソースのログ記録フレームワークを使用する [ELMAH](http://nuget.org/packages/elmah/)します。 詳細については、次を参照してください。 [ELMAH についてまとめた Scott Hanselman のブログ記事](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)します。

Azure からストリーミング ログを取得する必要がある場合、必ずしも ASP.NET や System.Diagnostics トレースを使う必要はありません。 Azure の web アプリのストリーミング ログ サービスでは、いずれかがストリーミング *.txt*, 、*.html*, 、または *.log* ファイルで検出された、 *LogFiles* フォルダーです。 したがって、Web アプリケーションのファイル システムに書き込む独自のログ記録システムを作成することもできます。必要なファイルが自動的にストリーミングされ、ダウンロードされます。 行う必要があるにファイルを作成するアプリケーション コードを記述、 *d:\home\logfiles* フォルダーです。 

### Web サーバーのログの分析

Web サーバーのログの分析の詳細については、次のリソースを参照してください。

* [LogParser](http://www.microsoft.com/download/details.aspx?id=24659)<br/>
  Web サーバーのログのデータを表示するためのツール (*.log* ファイル)。
* [IIS のパフォーマンスの問題やアプリケーション エラーを LogParser でトラブルシューティングします。 ](http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser)<br/>
  Web サーバーのログを分析する際に活用できる Log Parser ツールについて基本的な事柄が説明されています。
* [LogParser の使用に関して Robert McMurray が執筆したブログ記事します。](http://blogs.msdn.com/b/robert_mcmurray/archive/tags/logparser/)<br/>
* [IIS 7.0、IIS 7.5、IIS 8.0 における HTTP ステータス コード](http://support.microsoft.com/kb/943891)

### 失敗した要求トレース ログの分析

Microsoft TechNet web サイト、 [を使用して失敗した要求トレース](http://www.iis.net/learn/troubleshoot/using-failed-request-tracing) セクションがこれらのログを使用する方法を理解しておくことがあります。 ただし、このドキュメントで扱う内容は、失敗した要求トレースを IIS で構成する作業が主体です。この作業を Azure Web Apps で行うことはできません。

### Cloud Services のデバッグ

Web アプリではなく、Azure クラウド サービスをデバッグする場合は、「 [クラウド サービスのデバッグ](http://msdn.microsoft.com/library/windowsazure/ee405479.aspx)します。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)
* Azure のポータルから Azure プレビュー ポータルへの変更ガイドについては、次を参照してください: [プレビュー ポータル内の移動に関するリファレンス。](http://go.microsoft.com/fwlink/?LinkId=529715)

[GetStarted]: web-sites-dotnet-get-started.md
[GetStartedWJ]: websites-dotnet-webjobs-sdk.md
 

