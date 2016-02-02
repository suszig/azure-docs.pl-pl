<properties 
    pageTitle="ハイブリッド接続を使用して Azure App Service の API アプリからオンプレミスの SQL Server に接続する" 
    description="Microsoft Azure で API アプリを作成し、それをオンプレミスの SQL Server データベースに接続する"
    services="app-service\api" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/03/2015" 
    ms.author="tdykstra"/>


# ハイブリッド接続を使用して Azure App Service の API アプリからオンプレミスの SQL Server に接続する

[AZURE.INCLUDE [app-service-api-v2-note](../../includes/app-service-api-v2-note.md)]

ハイブリッド接続が接続できる [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) 、静的 TCP ポートを使用する内部設置型リソースへの API アプリ。 サポートされているリソースには、Microsoft SQL Server、MySQL、HTTP Web APIs、Mobile Services、およびほとんどのカスタム Web サービスが含まれます。

このチュートリアルでは、App Service の API アプリを作成する方法を学習します、 [Azure プレビュー](http://go.microsoft.com/fwlink/?LinkId=529715) 、新しいハイブリッド接続機能を使用して、ローカル オンプレミス SQL Server データベースに接続します。 このチュートリアルは、Azure または SQL Server を使用した経験がない読者を対象に作成されています。
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 前提条件

このチュートリアルを完了するには、以下の製品が必要です。 すべて無料版を利用できるため、Azure ソリューションの開発を完全に無料で始めることができます。

- **Azure サブスクリプション** - 無料サブスクリプションについてを参照してください [Azure 無料試用版](/pricing/free-trial/)します。

- **Visual Studio** を参照してください、Visual Studio 2013 または Visual Studio 2015 の無料評価版をダウンロードする - [Visual Studio のダウンロード](http://www.visualstudio.com/downloads/download-visual-studio-vs)します。 続行する前に、次のいずれかをインストールします。 (このチュートリアルのスクリーン ショットは、Visual Studio 2013 を使用して取得しました)

- **SQL Server 2014 Express with Tools** -Microsoft SQL Server Express の無料版をダウンロード、 [Microsoft Web プラットフォーム データベースのページ](https://www.microsoft.com/en-us/download/details.aspx?id=42299)します。 このチュートリアルで後で確認する方法 [SQL Server インストール](#InstallSQLDB) を適切に構成されていることを確認します。

- **SQL Server Management Studio Express** - これは SQL Server 2014 express with Tools のダウンロード、上記で説明したが、個別にインストールする必要がある場合は、ダウンロードしてインストールしてから、 [SQL Server Express のダウンロード ページ](https://www.microsoft.com/en-us/download/details.aspx?id=42299)します。

このチュートリアルでは、Azure サブスクリプションを持っている、Visual Studio 2013 をインストールしている、および .NET Framework 3.5 をインストールしているか有効にしていると想定しています。 このチュートリアルでは、Azure のハイブリッド接続機能 (静的 TCP ポートの既定のインスタンス) を使用する構成に SQL Server 2014 Express をインストールする方法を説明します。 SQL Server をインストールしていない場合は、チュートリアルを開始する前に、上記の場所から SQL Server 2014 Express with Tools をダウンロードしてください (ただし、インストールはしないでください)。

### メモ

ハイブリッド接続でオンプレミスの SQL Server または SQL Server Express のデータベースを使用するには、TCP/IP が静的ポートで有効になっている必要があります。 SQL Server の既定のインスタンスは静的ポート 1433 を使用しますが、名前付きインスタンスは静的ポート 1433 を使用しません。

オンプレミス ハイブリッド接続マネージャーのエージェントをインストールするコンピューターの条件は、次のとおりです。

- 次を使用して Azure に送信接続できること

><table border="1">
>    <tr>
>       <th><strong>Port</strong></th>
>        <th>理由</th>
>    </tr>
>    <tr>
>        <td>80</td>
>        <td><strong>必須</strong> 証明書の検証用およびオプションでデータ接続用の HTTP ポート。</td>
>    </tr>
>    <tr>
>        <td>443</td>
>        <td><strong>省略可能</strong> データ接続用。443 への送信接続ができない場合は、TCP ポート 80 を使用します。</td>
>    </tr>
>    <tr>
>        <td>5671 と 9352</td>
>        <td><strong>推奨。</strong> ただし、データ接続の場合は省略可能。このモードでは通常、スループットが高くなります。これらのポートへの送信接続ができない場合は、TCP ポート 443 を使用します。</td>
>    </tr>
></table>

- オンプレミス リソースの *hostname*:*portnumber* に到達できること

この記事の手順では、オンプレミスのハイブリッド接続のエージェントをホストするコンピューターからブラウザーを使用していると想定しています。

スキップ」から開始する場合は、既にある SQL Server を構成およびを上記の条件を満たす環境をインストールできます [オンプレミス SQL Server データベースを作成する](#CreateSQLDB)します。

<a name="InstallSQL"></a>
## SQL Server Express をインストールし、TCP/IP を有効にして、オンプレミスの SQL Server データベースを作成する

このセクションでは、SQL Server Express をインストール、TCP/IP を有効にして、API アプリが処理できるように、データベースを作成する方法を表示、 [Azure プレビュー ポータル](https://portal.azure.com)します。

<a name="InstallSQLDB"></a>
### SQL Server Express をインストールする

1. SQL Server Express をインストールするには、**SQLEXPRWT_x64_ENU.exe** (64 ビット バージョン) ファイルまたは **SQLEXPR_x86_ENU.exe** (32 ビット バージョン) ファイルのいずれかをダウンロードして、目的のフォルダーに抽出します。

2. SQL Server Express のインストール ファイルを抽出したら、**setup.exe** を実行します。

3. **[SQL Server インストール センター]** が表示されたら、**[SQL Server の新規スタンドアロン インストールを実行するか、既存のインストールに機能を追加します]** をクリックします。

    ![SQL Server インストール センター](./media/app-service-api-hybrid-on-premises-sql-server/sql-server-installation-center.png)

4. 指示に従って、**[インスタンスの構成]** ページが表示されるまで、既定の選択と設定を使用します。

5. **[インスタンスの構成]** ページで、**[既定のインスタンス]** を選択し、**[次へ]** をクリックします。

    ![インスタンスの構成](./media/app-service-api-hybrid-on-premises-sql-server/instance-configuration.png)

    SQL Server の既定のインスタンスが静的ポート 1433 で SQL Server クライアントからの要求をリッスンします。ハイブリッド接続機能には静的ポート 1433 が必要です。 名前付きインスタンスは動的ポートと UDP を使用します。これはハイブリッド接続ではサポートされません。

6. **[サーバーの構成]** ページの既定値をそのまま使用し、**[次へ]** をクリックします。

7. **[データベース エンジンの構成]** ページの **[認証モード]** で、**[混合モード (SQL Server 認証と Windows 認証)]** を選択して、パスワードを入力します。

    ![データベース エンジンの構成](./media/app-service-api-hybrid-on-premises-sql-server/database-engine-configuration.png)

    このチュートリアルでは、SQL Server 認証を使用します。 後で必要になるため、入力したパスワードを忘れないでください。

8. ウィザードの残りの手順を実行し (進みながら既定値をそのまま使用)、インストールを完了します。

9. **[SQL Server インストール センター]** ダイアログを閉じます。

### TCP/IP を有効にする

TCP/IP を有効にするには、SQL Server Express をインストールした際にインストールされた SQL Server 構成マネージャーを使用します。 手順に従います [for SQL Server の TCP/IP ネットワーク プロトコルの有効化](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) 続行する前にします。

<a name="CreateSQLDB"></a>
### オンプレミスの SQL Server データベースを作成する

1. **SQL Server Management Studio** で、インストールした SQL Server に接続します **[サーバーの種類]** には、**[データベース エンジン]** を選択します。 **[サーバー名]** には、**[localhost]**、または使用しているコンピューターの名前を使用します。 選択 **SQL Server 認証**, でログインし、 `sa` ユーザー名、および先ほど作成したパスワードです。

    ![サーバーへの接続](./media/app-service-api-hybrid-on-premises-sql-server/connect-to-server.png)

    **[サーバーへの接続]** ダイアログが自動的に表示されない場合は、左パネルの **[オブジェクト エクスプローラー]** に移動し、**[接続]**、**[データベース エンジン]** の順にクリックします。

2. SQL Server Management Studio を使用して新しいデータベースを作成するには、オブジェクト エクスプローラーで **[データベース]** を右クリックしてから、**[新しいデータベース]** をクリックします。

    ![新しいデータベース メニューの作成](./media/app-service-api-hybrid-on-premises-sql-server/new-database-menu.png)

3. **新しいデータベース** ダイアログ ボックスで、入力 `ローカル ・ データベース` クリックして、データベース名の **[ok]**します。

    ![新しいデータベースを作成する](./media/app-service-api-hybrid-on-premises-sql-server/new-database.png)

### SQL Server テーブルを作成して設定する

1. **SQL Server Management Studio** **オブジェクト エクスプ ローラー**, 、展開、 `ローカル ・ データベース` エントリです。

    ![展開したデータベース](./media/app-service-api-hybrid-on-premises-sql-server/local-database-expanded.png)

2. **[テーブル]** を右クリックして、コンテキスト メニューから **[テーブル...]** オプションを選択します。

    ![新しいテーブルのメニュー](./media/app-service-api-hybrid-on-premises-sql-server/new-table-menu.png)

3. テーブルのデザイナー グリッドが表示されたら、次の図のとおりに列情報を入力します。

    ![新しいテーブルの列](./media/app-service-api-hybrid-on-premises-sql-server/table-def.png)

4. キーを押して **< Ctrl > S** 、新しいテーブルの定義を保存します。 テーブル名の入力を求められます。 入力 `スピーカー` とキーを押します **OK**します。

    ![新しいテーブルの保存](./media/app-service-api-hybrid-on-premises-sql-server/save-new-table.png)

5. **オブジェクト エクスプ ローラー**, 、新しく作成したを右クリックし `スピーカー` テーブル、および選択 **編集上位 200 行** 、コンテキスト メニューからです。

    ![上位 200 行の編集](./media/app-service-api-hybrid-on-premises-sql-server/edit-top-200-rows.png)

6. テーブル データを入力/変更するグリッドが表示されたら、次の図に示すようなテスト データを入力します。

    ![テスト データ](./media/app-service-api-hybrid-on-premises-sql-server/speakers-data.png)

## デモ API アプリを作成して Azure にデプロイする

このセクションでは、デモ API アプリを作成する方法について説明します。

1. Visual Studio 2013 を開き、**[ファイル]、[新規]、[プロジェクト]** の順に選択します。

2. **[Visual C#] > [Web] > [ASP.NET Web アプリケーション]** テンプレートを選択し、**[プロジェクトに Application Insights を追加する]** オプションをオフにして、プロジェクトに *SpeakersList* と名前を付け、**[OK]** をクリックします。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/new-project.png)

3. **[新しい ASP.NET プロジェクト]** ダイアログで、**[Azure API アプリ]** プロジェクト テンプレートを選択して、**[OK]** をクリックします。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/new-project-api-app.png)

4. **ソリューション エクスプローラー**で **[モデル]** フォルダーを右クリックし、コンテキスト メニューで **[追加]、[クラス]** の順にクリックします。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/new-model-menu.png)

5. 新しいファイルに *Speaker.cs* と名前を付け、**[追加]** をクリックします。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/new-model-class.png)

6. 内容全体を置き換える、 `Speaker.cs` を次のコード ファイル。

        namespace SpeakersList.Models
        {
            public class Speaker
            {
                public int Id { get; set; }
                public string Name { get; set; }
                public string EmailAddress { get; set; }
            }
        }

7. **ソリューション エクスプローラー**で **[コントローラー]** フォルダーを右クリックし、コンテキスト メニューで **[追加]、[コントローラー]** の順にクリックします。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/new-controller.png)

8. **[スキャフォールディングの追加]** ダイアログで、**[Web API 2 コントローラー - 空]** オプションを選択し、**[追加]** をクリックします。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/add-scaffold.png)

9. コントローラーに **SpeakersController** という名前を付け、**[追加]** をクリックします。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/add-controller-name.png)

10. コードで置き換え、 `SpeakersController.cs` 次のコード ファイル。 < サーバー名 > の値を指定することを確認し、< password > プレース ホルダーに、 `connectionString`します。 < ServerName > の値は、SQL Server が、配置されているコンピューター名で、< パスワード > 値はインストールして、SQL Server を構成するときに設定する 1 つです。
> [AZURE.NOTE] 次のコード スニペットには、パスワードの情報が含まれています。 これは、デモを簡潔にするためです。 実際の運用環境では、コードに資格情報を格納しないでください。 代わりに、参照する、 [ASP.NET と Azure へのパスワード (および他の機密データ) の展開のベスト プラクティス](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure)します。

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http;
    using SpeakersList.Models;
    using System.Data.Sql;
    using System.Data.SqlClient;
    
    namespace SpeakersList.Controllers
    {
        public class SpeakersController : ApiController
        {
            [HttpGet]
            public IEnumerable<Speaker> Get()
            {
                // Instantiate List object that will be populated and returned
                // from this method.
                List<Speaker> speakers = new List<Speaker>();
    
                // Build database connection string.
                string connectionString = "Server=<serverName>;" +
                                          "Initial Catalog=LocalDatabase;" +
                                          "User Id=sa;Password=<password>;" +
                                          "Asynchronous Processing=true;";
    
                // Build query string to select all three columns from the Speakers table.
                string queryString = "SELECT Id, EmailAddress, Name FROM dbo.Speakers;";
    
                // Instantiate the SqlConnection object using the connection string.
                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    // Instantiate the SqlCommand object using the connection and query strings.
                    SqlCommand command = new SqlCommand(queryString, connection);
    
                    // Open the connection to the database.
                    connection.Open();
    
                    // Execute the command and return a SqlDataReader object
                    // that can be iterated.
                    SqlDataReader reader = command.ExecuteReader();
                    try
                    {
                        // Read a row from the SqlDataReader object
                        while (reader.Read())
                        {
                            // For each row, use the returned data to instantiate
                            // and add to the List a new Speaker object.
                            speakers.Add(new Speaker 
                                { 
                                    Id = Convert.ToInt32(reader[0]), 
                                    EmailAddress = reader[1].ToString(), 
                                    Name = reader[2].ToString() 
                                }
                            );
                        }
                    }
                    finally
                    {
                        // Close the SqlDataReader object.
                        reader.Close();
                    }
                }
                // Return the List of Speaker objects.
                return speakers;
            }
        }
    }


### Swagger UI の有効化

Swagger UI を有効にすると、呼び出すクライアント コードを記述しなくても、API アプリを簡単にテストできます。

1. *App_Start/SwaggerConfig.cs* ファイルを開き、**EnableSwaggerUI** を探します。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/swagger-ui-disabled.png)

2. 次のコード行をコメント解除します。

            })
        .EnableSwaggerUi(c =>
            {

3. 完了したら、ファイルが次の図のようになったことを確認します。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/swagger-ui-enabled.png)

### API アプリをテストする

1. API テスト ページを表示するには、ローカルでアプリを実行 **< Ctrl > f5 キーを押して**します。 次のようなエラーが表示されます。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/error-forbidden.png)

2. ブラウザーのアドレス バーで、追加 `/swagger` クリックして、URL の末尾に **< Enter >**します。 これにより、前のセクションで有効にした Swagger UI が表示されます。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/swagger-ui.png)

3. **[Speakers]** セクションをクリックして展開します。

4. **[GET /api/speakers]** をクリックしてそのセクションを展開します。

5. **[Try it out!]** をクリックして、前の手順でデータベースに入力したデータを表示します。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/try-it-out.png)

### API アプリのデプロイ

アプリをローカルでテストしたので、アプリを Azure にデプロイしましょう。

1. **ソリューション エクスプローラー**で、プロジェクト (ソリューションではありません) を右クリックしてから、**[発行]** をクリックします。

    ![プロジェクトの ](./media/app-service-api-hybrid-on-premises-sql-server/publish-menu.png)

2. **[プロファイル]** タブ、**[Microsoft Azure API Apps (プレビュー)]** の順にクリックします。

    ![プロジェクトの ](./media/app-service-api-hybrid-on-premises-sql-server/publish-web.png)

3. **[新規]** をクリックして、Azure サブスクリプションに新しい API アプリをプロビジョニングします。

    ![既存の API サービス選択ダイアログ](./media/app-service-api-hybrid-on-premises-sql-server/publish-to-existing-api-app.png)

4. **[API アプリの作成]** ダイアログで、次のように入力します。

    - **[API アプリの名前]** にアプリケーションの名前を入力します。
    - 複数の Azure サブスクリプションがある場合は、使用するサブスクリプションを選択します。
    - **[App Service プラン]** で、既存の App Service プランを選択するか、**[App Service プランの新規作成]** を選択して新しいプランの名前を入力します。
    - **[リソース グループ]** で、既存のリソース グループを選択するか、**[新しいリソース グループの作成]** を選択して名前を入力します。 名前は一意にする必要があります。アプリ名をプレフィックスとして使用し、@ 記号を除いた Microsoft ID などの個人情報を追加することを検討してください。
    - **[アクセス レベル]** で、**[すべてのユーザーが利用できる]** を選択します。 これは API を完全にパブリックにするオプションですが、このチュートリアルでは問題ありません。 後で使用してアクセスを制限することができます、 [Azure プレビュー ポータル](https://portal.azure.com)します。
    - リージョンを選択します。

    **[OK]** をクリックします。サブスクリプションに API アプリが作成されます。

    ![Microsoft Azure Web アプリの構成ダイアログ](./media/app-service-api-hybrid-on-premises-sql-server/publish-new-api-app.png)

5. このプロセスには数分かかることがあります。プロセスが開始されたことを通知するダイアログが Visual Studio に表示されます。 [確認] ダイアログで **[OK]** をクリックします。

    ![API サービスの作成の開始の確認メッセージ](./media/app-service-api-hybrid-on-premises-sql-server/create-api-app-confirmation.png)

6. プロビジョニング プロセスが Azure サブスクリプションでリソース グループと API アプリを作成している間、Visual Studio の **[Azure App Service アクティビティ]** ウィンドウに進行状況が表示されます。

    ![[Azure App Service アクティビティ] ウィンドウによる状態の通知](./media/app-service-api-hybrid-on-premises-sql-server/app-service-activity.png)

7. API アプリがプロビジョニングされたら、**ソリューション エクスプローラー**でプロジェクトを右クリックして **[発行]** を選択します。発行ダイアログが再び開きます。 前の手順で作成した発行プロファイルが選択されます。 **[発行]** をクリックしてデプロイ プロセスを開始します。

    ![API アプリのデプロイ](./media/app-service-api-hybrid-on-premises-sql-server/publish2.png)

**[Azure App Service アクティビティ]** ウィンドウにデプロイの進行状況が表示され、いつ発行プロセスが完了するかが示されます。

## ハイブリッド接続および BizTalk サービスを作成する

1. 移動し、ブラウザー、 [Azure プレビュー ポータル](https://portal.azure.com)します。

2. 左側の **[すべてを参照]** オプションをクリックします。

3. **[参照]** ブレードで、**[API アプリ]** を選択します。

4. **[API アプリ]** ブレードで、API アプリを検索してクリックします。

5. API アプリのブレードで、**[API アプリ ホスト]** の下の値をクリックします。

    ![API App ブレード](./media/app-service-api-hybrid-on-premises-sql-server/api-app-blade-api-app-host.png)

6. **[API アプリ ホスト]** ブレードが表示されたら、**[ネットワーク]** セクションまで下にスクロールして、**[ハイブリッド接続]** をクリックします。

    ![ハイブリッド接続](./media/app-service-api-hybrid-on-premises-sql-server/api-app-host-blade-hybrid-connections.png)

7. **[ハイブリッド接続]** ブレードで、**[追加]**、**[新しいハイブリッド接続]** の順にクリックします。

8. **[ハイブリッド接続の作成] ブレード**で、次の手順を実行します。
    - **[名前]** に、接続の名前を入力します。
    - **[ホスト名]** には、SQL Server ホスト コンピューターのコンピューター名を入力します。
    - **ポート**, 、入力 `1433年` (SQL Server の既定のポート)。
    - **[BizTalk サービス]** をクリックし、BizTalk サービスの名前を入力します。

    ![Create a hybrid connection](./media/app-service-api-hybrid-on-premises-sql-server/create-biztalk-service.png)

9. **[OK]** を 2 回クリックします。

    処理が完了すると、**[通知]** 領域に緑色の "**SUCCESS**" という文字が点滅します。**[ハイブリッド接続]** ブレードには、状態が **[未接続]** の新しいハイブリッド接続が表示されます。

    ![作成したハイブリッド接続](./media/app-service-api-hybrid-on-premises-sql-server/hybrid-not-connected-yet.png)

これで、クラウド ハイブリッド接続のインフラストラクチャの重要な部分が完了しました。 次に、対応するオンプレミスの部分を作成します。

<a name="InstallHCM"></a>
## オンプレミスのハイブリッド接続マネージャーをインストールして接続を完了する

[AZURE.INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

ハイブリッド接続のインフラストラクチャが完成したので、アプリケーションをテストしましょう。

<a name="CreateASPNET"></a>
## 完成した API アプリを Azure でテストする

1. Azure プレビュー ポータルで、[API アプリ ホスト] ブレードに戻り、**[URL]** の下の値をクリックします。

2. API アプリのホスト ページが表示されたら、ブラウザーで、追加 `/swagger` ブラウザーのアドレス バーに URL を **< Enter >**します。

3. **[Speakers]** セクションをクリックして展開します。

4. **[GET /api/speakers]** をクリックしてそのセクションを展開します。

5. **[Try it out!]** をクリックして、前の手順でデータベースに入力したデータを表示します。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/try-it-out-azure.png)

**ご利用ありがとうございます。**Azure で実行され、ハイブリッド接続を使用してローカルのオンプレミスの SQL Server データベースに接続する、API アプリを作成しました。

## 関連項目

[ハイブリッド接続の概要](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh twist によるハイブリッド接続 (チャネル 9 ビデオ) が導入されています](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[ハイブリッド接続の概要](/services/biztalk-services/)

[BizTalk サービス: ダッシュ ボード、モニター、スケール、構成、およびハイブリッド接続] タブ](../biztalk-dashboard-monitor-scale-tabs/)

[シームレスなアプリケーションの移植性 (Channel 9 のビデオ) と実際のハイブリッド クラウドの構築](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[ハイブリッド接続を使用して Azure のモバイル サービスから内部設置型 SQL Server に接続します。](../mobile-services-dotnet-backend-hybrid-connections-get-started.md)

[ハイブリッド接続 (チャネル 9 ビデオ) を使用して Azure Mobile Services から内部設置型 SQL Server に接続します。](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

[ASP.NET Identity の概要](http://www.asp.net/identity)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]





