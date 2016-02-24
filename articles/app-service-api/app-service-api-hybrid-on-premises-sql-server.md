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

> <table border="1">
    <tr>
       <th><strong>Port</strong></th>
        <th>理由</th>
    </tr>
    <tr>
        <td>80</td>
        <td><strong>必須</strong> 証明書の検証用およびオプションでデータ接続用の HTTP ポート。</td>
    </tr>
    <tr>
        <td>使用します</td>
        <td><strong>省略可能</strong> データ接続用。 443 への送信接続ができない場合は、TCP ポート 80 を使用します。</td>
    </tr>
    <tr>
        <td>5671 と 9352</td>
        <td><strong>推奨</strong> ただし、データ接続の場合は省略可能。 このモードでは通常、スループットが高くなります。 これらのポートへの送信接続ができない場合は、TCP ポート 443 を使用します。</td>
    </tr>
</table>

- 到達できる必要があります、 *hostname*:*portnumber* 内部設置型リソースのです。 

この記事の手順では、オンプレミスのハイブリッド接続のエージェントをホストするコンピューターからブラウザーを使用していると想定しています。

スキップ」から開始する場合は、既にある SQL Server を構成およびを上記の条件を満たす環境をインストールできます [オンプレミス SQL Server データベースを作成する](#CreateSQLDB)です。

<a name="InstallSQL"></a>
## SQL Server Express をインストールし、TCP/IP を有効にして、オンプレミスの SQL Server データベースを作成する

このセクションでは、SQL Server Express をインストール、TCP/IP を有効にして、API アプリが処理できるように、データベースを作成する方法を表示、 [Azure プレビュー ポータル](https://portal.azure.com)します。

<a name="InstallSQLDB"></a>
### SQL Server Express をインストールする

1. SQL Server Express をインストールするには、いずれかをダウンロード **SQLEXPRWT_x64_ENU.exe** (64 ビット バージョン) または **SQLEXPR_x86_ENU.exe** (32 ビット バージョン) ファイルを目的のフォルダーに抽出します。 

2. SQL Server Express のインストール ファイルを抽出した後は、実行 **setup.exe**します。

3. ときに、 **SQL Server インストール センター** が表示され、選択 **SQL Server の新規スタンドアロン インストールまたは既存のインストールに機能の追加**します。 

    ![SQL Server インストール センター](./media/app-service-api-hybrid-on-premises-sql-server/sql-server-installation-center.png)

4. 次の手順については、表示されるまで、既定の選択と設定を受け入れ、 **インスタンスの構成** ページです。
    
5.  **インスタンスの構成** ページで、選択 **既定のインスタンス**, 、] をクリック **次**します。
    
    ![インスタンスの構成](./media/app-service-api-hybrid-on-premises-sql-server/instance-configuration.png)
    
    SQL Server の既定のインスタンスが静的ポート 1433 で SQL Server クライアントからの要求をリッスンします。ハイブリッド接続機能には静的ポート 1433 が必要です。 名前付きインスタンスは動的ポートと UDP を使用します。これはハイブリッド接続ではサポートされません。
    
6. 既定値を受け入れ、 **サーバーの構成** ] ページで、をクリックして **次**します。
    
7.  **データベース エンジンの構成** ] ページで、[ **認証モード**, 、選択 **混合モード (SQL Server 認証と Windows 認証)**, 、パスワードを指定します。
    
    ![データベース エンジンの構成](./media/app-service-api-hybrid-on-premises-sql-server/database-engine-configuration.png)
    
    このチュートリアルでは、SQL Server 認証を使用します。 後で必要になるため、入力したパスワードを忘れないでください。
    
8. ウィザードの残りの手順を実行し (進みながら既定値をそのまま使用)、インストールを完了します。

9. 閉じる、 **SQL Server インストール センター** ダイアログ。

### TCP/IP を有効にする
TCP/IP を有効にするには、SQL Server Express をインストールした際にインストールされた SQL Server 構成マネージャーを使用します。 手順に従います [for SQL Server の TCP/IP ネットワーク プロトコルの有効化](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) 続行する前にします。

<a name="CreateSQLDB"></a>
### オンプレミスの SQL Server データベースを作成する

1.  **SQL Server Management Studio**, 、インストールした SQL Server に接続します。  **サーバーの種類**, 、選択 **データベース エンジン**します。  **サーバー名**, 、使用する **localhost** またはを使用しているコンピューターの名前。 選択 **SQL Server 認証**, でログインし、 `sa` ユーザー名、および先ほど作成したパスワードです。

    ![サーバーへの接続](./media/app-service-api-hybrid-on-premises-sql-server/connect-to-server.png)
    
    場合、 **サーバーへの接続** に移動し、ダイアログが自動的に表示されない **オブジェクト エクスプ ローラー** 左のウィンドウで **接続**, 、クリックして **データベース エンジン**します。
    
2. SQL Server Management Studio を使用して、新しいデータベースを作成するには、右 **データベース** オブジェクト エクスプ ローラーで、をクリックしてに **新しいデータベース**します。
    
    ![新しいデータベース メニューの作成](./media/app-service-api-hybrid-on-premises-sql-server/new-database-menu.png)
    
3.  **新しいデータベース** ダイアログ ボックスで、入力 `LocalDatabase` クリックして、データベース名の **[ok]**します。 
    
    ![新しいデータベースを作成する](./media/app-service-api-hybrid-on-premises-sql-server/new-database.png)
    
### SQL Server テーブルを作成して設定する

1.  **SQL Server Management Studio** **オブジェクト エクスプ ローラー**, 、展開、 `LocalDatabase` エントリです。

    ![展開したデータベース](./media/app-service-api-hybrid-on-premises-sql-server/local-database-expanded.png)

2. 右クリック **テーブル** を選択し、 **Table...** 、コンテキスト メニュー オプション。

    ![新しいテーブルのメニュー](./media/app-service-api-hybrid-on-premises-sql-server/new-table-menu.png)

3. テーブルのデザイナー グリッドが表示されたら、次の図のとおりに列情報を入力します。

    ![新しいテーブルの列](./media/app-service-api-hybrid-on-premises-sql-server/table-def.png)

4. キーを押して **& lt;Ctrl キーを押し > S** 、新しいテーブルの定義を保存します。 テーブル名の入力を求められます。 入力 `Speakers` とキーを押します **OK**します。

    ![新しいテーブルの保存](./media/app-service-api-hybrid-on-premises-sql-server/save-new-table.png)

5.  **オブジェクト エクスプ ローラー**, 、新しく作成したを右クリックし `Speakers` テーブル、および選択 **編集上位 200 行** 、コンテキスト メニューからです。

    ![上位 200 行の編集](./media/app-service-api-hybrid-on-premises-sql-server/edit-top-200-rows.png)

6. テーブル データを入力/変更するグリッドが表示されたら、次の図に示すようなテスト データを入力します。

    ![テスト データ](./media/app-service-api-hybrid-on-premises-sql-server/speakers-data.png)

## デモ API アプリを作成して Azure にデプロイする 

このセクションでは、デモ API アプリを作成する方法について説明します。

1. Visual Studio 2013 を開き、選択 **ファイル > 新規 > プロジェクト**します。 

2. 選択、 **Visual c# > Web > ASP.NET Web アプリケーション** テンプレート、クリア、 **プロジェクトに application insights を追加** オプションで、プロジェクトの名前 *SpeakersList*, 、順にクリック **[ok]**します。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/new-project.png)

3.  **新しい ASP.NET プロジェクト** ダイアログ ボックスで、選択、 **Azure API アプリ** プロジェクト テンプレートを使用し、クリックして **[ok]**します。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/new-project-api-app.png)

4.  **ソリューション エクスプ ローラー**, を右クリックし、 **モデル** フォルダーをクリックして、 **追加 > クラス]** コンテキスト メニュー オプション。 

    ![](./media/app-service-api-hybrid-on-premises-sql-server/new-model-menu.png) 

5. 新しいファイルの名前 *Speaker.cs*, 、クリックして **追加**します。 

    ![](./media/app-service-api-hybrid-on-premises-sql-server/new-model-class.png) 

6. `Speaker.cs` ファイルの内容全体を次のコードに置き換えます。 

        namespace SpeakersList.Models
        {
            public class Speaker
            {
                public int Id { get; set; }
                public string Name { get; set; }
                public string EmailAddress { get; set; }
            }
        }

7.  **ソリューション エクスプ ローラー**, を右クリックし、 **コント ローラー** フォルダーをクリックして、 **追加 > コント ローラー]** コンテキスト メニュー オプション。 

    ![](./media/app-service-api-hybrid-on-premises-sql-server/new-controller.png) 

8.  **スキャフォールディングの追加** ダイアログ ボックスで、選択、 **Web API 2 コント ローラー - 空** と] チェック ボックスをクリックして **追加**します。 

    ![](./media/app-service-api-hybrid-on-premises-sql-server/add-scaffold.png) 

9. コント ローラーに名前 **SpeakersController** ] をクリック **追加**します。 

    ![](./media/app-service-api-hybrid-on-premises-sql-server/add-controller-name.png) 

10. `SpeakersController.cs` ファイルのコードを次のコードに置き換えます。 `connectionString` の <serverName> プレースホルダーと <password> プレースホルダーに独自の値が指定されていることを確認します。 <serverName> の値は SQL Server があるマシンの名前、<password> の値は SQL Server をインストールおよび構成したときに設定したパスワードです。

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

1. 開いている、 *App_Start/SwaggerConfig.cs* ファイルを開き、 **EnableSwaggerUI**:

    ![](./media/app-service-api-hybrid-on-premises-sql-server/swagger-ui-disabled.png) 

2. 次のコード行をコメント解除します。

            })
        .EnableSwaggerUi(c =>
            {

3. 完了したら、ファイルが次の図のようになったことを確認します。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/swagger-ui-enabled.png) 

### API アプリをテストする

1. API テスト ページを表示するには、ローカルでアプリを実行 **& lt;Ctrl > f5 キーを押して**します。 次のようなエラーが表示されます。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/error-forbidden.png) 

2. ブラウザーのアドレス バーで、追加 `/swagger` クリックして、URL の末尾に **& lt;入力 >**します。 これにより、前のセクションで有効にした Swagger UI が表示されます。  

    ![](./media/app-service-api-hybrid-on-premises-sql-server/swagger-ui.png) 

3. クリックして、 **スピーカー** セクションを展開します。

4. クリックして **GET/api/スピーカー** セクションを展開します。

5. をクリックして **試してみる!** 以前にデータベースに入力したデータを表示します。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/try-it-out.png) 

### API アプリのデプロイ

アプリをローカルでテストしたので、アプリを Azure にデプロイしましょう。

1.  **ソリューション エクスプ ローラー**, プロジェクト (ソリューションではありません) を右クリックし、クリックして、 **発行]**します。 

    ![プロジェクトの [発行] メニュー オプション](./media/app-service-api-hybrid-on-premises-sql-server/publish-menu.png)

2. クリックして、 **プロファイル** ] タブでをクリックし、 **Microsoft Azure API Apps (プレビュー)**します。 

    ![プロジェクトの [発行] メニュー オプション](./media/app-service-api-hybrid-on-premises-sql-server/publish-web.png)

3. クリックして **新規** 、Azure サブスクリプションに新しい API アプリをプロビジョニングします。

    ![既存の API サービス選択ダイアログ](./media/app-service-api-hybrid-on-premises-sql-server/publish-to-existing-api-app.png)

4.  **API アプリの作成** ] ダイアログ ボックスで、次を入力します。

    -  **API アプリの名前**, 、アプリの名前を入力します。 
    - 複数の Azure サブスクリプションがある場合は、使用するサブスクリプションを選択します。
    -  **App Service プラン**, を既存の App Service プランを選択するか選択 **App Service プランの新規作成** 新しいプランの名前を入力します。 
    -  **リソース グループ**, を既存のリソース グループから選択するかを選択、 **新しいリソース グループの作成** 、名前を入力します。 名前は一意にする必要があります。アプリ名をプレフィックスとして使用し、@ 記号を除いた Microsoft ID などの個人情報を追加することを検討してください。  
    -  **アクセス レベル**, [ **だれでも利用できる**です。 これは API を完全にパブリックにするオプションですが、このチュートリアルでは問題ありません。 後で使用してアクセスを制限することができます、 [Azure プレビュー ポータル](https://portal.azure.com)します。
    - リージョンを選択します。

    をクリックして **OK** 、サブスクリプションの API アプリを作成します。 

    ![Microsoft Azure Web アプリの構成ダイアログ](./media/app-service-api-hybrid-on-premises-sql-server/publish-new-api-app.png)

5. このプロセスには数分かかることがあります。プロセスが開始されたことを通知するダイアログが Visual Studio に表示されます。 クリックして **OK** 確認のダイアログ ボックスにします。

    ![API サービスの作成の開始の確認メッセージ](./media/app-service-api-hybrid-on-premises-sql-server/create-api-app-confirmation.png)

6. プロビジョニング プロセスは、リソース グループと API アプリを作成する、Azure サブスクリプションには、Visual Studio が表示の進行状況、 **Azure App Service アクティビティ** ウィンドウです。 

    ![[Azure App Service アクティビティ] ウィンドウによる状態の通知](./media/app-service-api-hybrid-on-premises-sql-server/app-service-activity.png)

7. API アプリがプロビジョニングされた後でプロジェクトを右クリックして **ソリューション エクスプ ローラー** 選択 **発行** を再び発行] ダイアログ ボックスを開きます。 前の手順で作成した発行プロファイルが選択されます。 クリックして **発行** 展開プロセスを開始します。 

    ![API アプリのデプロイ](./media/app-service-api-hybrid-on-premises-sql-server/publish2.png)

 **Azure App Service アクティビティ** ウィンドウは、展開の進行状況が表示され、いつ発行プロセスが完了が示されます。 

## ハイブリッド接続および BizTalk サービスを作成する ##

1. 移動し、ブラウザー、 [Azure プレビュー ポータル](https://portal.azure.com)します。 

2. クリックして、 **すべてを参照** 左側のオプションです。

3.  **参照** ブレードで、 **API Apps**します。

4.  **API Apps** ブレードで、API アプリを検索し、このボタンをクリックします。

5. API アプリのブレードで、[下の値 **API アプリ ホスト**します。  
 
    ![API App blade](./media/app-service-api-hybrid-on-premises-sql-server/api-app-blade-api-app-host.png)

6. ときに、 **API アプリ ホスト** ブレードが表示されたら、下にスクロールして、 **ネットワーク** セクションを **ハイブリッド接続**します。
    
    ![ハイブリッド接続](./media/app-service-api-hybrid-on-premises-sql-server/api-app-host-blade-hybrid-connections.png)
    
7.  **ハイブリッド接続** ブレードで、をクリックして **追加** > **新しいハイブリッド接続**します。
    
8.  **ハイブリッド接続の作成] ブレード**:
    -  **名前**, 、接続の名前を指定します。
    -  **Hostname**, 、SQL Server ホスト コンピューターのコンピューターの名前を入力します。
    -  **ポート**, 、入力 `1433` (SQL Server の既定のポート)。
    - クリックして **Biz Talk Service** し、BizTalk サービスの名前を入力します。
    
    ![ハイブリッド接続の作成](./media/app-service-api-hybrid-on-premises-sql-server/create-biztalk-service.png)
        
9. クリックして **OK** 2 回クリックします。 

    処理が完了したら、 **通知** 領域が緑色の点滅 **成功** と **ハイブリッド接続** ブレードが新しいハイブリッド接続ステータスが表示されます **接続されていない**します。
    
    ![作成したハイブリッド接続](./media/app-service-api-hybrid-on-premises-sql-server/hybrid-not-connected-yet.png)
    
これで、クラウド ハイブリッド接続のインフラストラクチャの重要な部分が完了しました。 次に、対応する内部設置型の部分を作成します。

<a name="InstallHCM"></a>
## オンプレミス ハイブリッド接続マネージャーをインストールして接続を完了する

[AZURE.INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

ハイブリッド接続のインフラストラクチャが完成したので、アプリケーションをテストしましょう。

<a name="CreateASPNET"></a>
## 完成した API アプリを Azure でテストする

1. Azure プレビュー ポータルで、API アプリ ホスト] ブレードに戻り、[値] をクリックして **URL**します。
    
2. API アプリのホスト ページが表示されたら、ブラウザーで、追加 `/swagger` ブラウザーのアドレス バーに URL を **& lt;入力 >**します。
    
3. クリックして、 **スピーカー** セクションを展開します。

4. クリックして **GET/api/スピーカー** セクションを展開します。

5. をクリックして **試してみる!** 以前にデータベースに入力したデータを表示します。

    ![](./media/app-service-api-hybrid-on-premises-sql-server/try-it-out-azure.png) 
    
**おめでとうございます!**Azure で実行され、ハイブリッド接続を使用してローカルのオンプレミスの SQL Server データベースに接続する、API アプリを作成しました。 

## 関連項目
[ハイブリッド接続の概要](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh Twist によるハイブリッド接続の紹介 (チャネル 9 ビデオ)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[ハイブリッド接続の概要](/services/biztalk-services/)

[BizTalk サービス: [ダッシュボード]、[監視]、[スケール]、[構成]、および [ハイブリッド接続] タブ](../biztalk-dashboard-monitor-scale-tabs/)

[シームレスなアプリケーションの移植性を使用して実際のハイブリッド クラウドをビルドする (チャネル 9 ビデオ)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[ハイブリッド接続を使用して Azure のモバイル サービスから内部設置型の SQL Server に接続する](../mobile-services-dotnet-backend-hybrid-connections-get-started.md)

[Connect to an on-premises SQL Server from Azure Mobile Services using Hybrid Connections (ハイブリッド接続を使用して Azure Mobile Services から内部設置型の SQL Server に接続する) (チャネル 9 ビデオ)](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

[ASP.NET の Id の概要](http://www.asp.net/identity)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

