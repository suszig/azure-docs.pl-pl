<properties
    pageTitle=".NET バックエンド モバイル サービスのバックエンド タスクのスケジュール | Microsoft Azure"
    description="Azure Mobile Services のスケジューラを使用し、日程に基づいて実行される .NET バックエンド ジョブを定義します。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="12/07/2015"
    ms.author="glenga"/>

# Mobile Services での繰り返し発生するジョブのスケジュール

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


> [AZURE.SELECTOR]
- [.NET バックエンド](mobile-services-dotnet-backend-schedule-recurring-tasks.md)
- [Javascript バックエンド](mobile-services-schedule-recurring-tasks.md)

このトピックでは、Azure クラシック ポータルのジョブ スケジューラ機能を使用して、定義したスケジュールに基づいて実行されるサーバー スクリプト コードを定義する方法について説明します。 このスクリプトは、リモート サービス (ここでは Twitter) に対する確認を定期的に行い、結果を新しいテーブルに格納します。 スケジュールできる定期的なタスクには、次のようなものがあります。

+ 古いデータ レコードまたは重複しているデータ レコードの取得。
+ 外部データ (ツイート、RSS エントリ、場所情報など) の要求と格納。
+ 格納されている画像の処理とサイズ変更。

このチュートリアルでは、ジョブ スケジューラを使用してスケジュールされたジョブを作成する手順について説明します。このジョブでは、Twitter にツイート データを要求し、そのツイートを新しい Updates テーブルに格納します。

>[AZURE.NOTE]このチュートリアルでは、to Twitter ライブラリ サード パーティの LINQ を使用して、Twitter v1.1 OAuth 2.0 アクセスを単純化します。 API への OAuth 2.0 アクセスを簡単にします。 このチュートリアルを完了するには、LINQ to Twitter の NuGet パッケージをダウンロードしてインストールする必要があります。 詳細については、次を参照してください。、 [LINQ to Twitter CodePlex project]します。

##<a name="get-oauth-credentials"></a>Twitter v1.1 API へのアクセスを登録して資格情報を保存する

[AZURE.INCLUDE [mobile-services-register-twitter-access](../../includes/mobile-services-register-twitter-access.md)]

&nbsp;&nbsp;7. Visual Studio のソリューション エクスプローラーで、モバイル サービス プロジェクトの web.config ファイルを開き、`MS_TwitterConsumerKey` と `MS_TwitterConsumerSecret` の各アプリケーション設定を見つけて、これらのキーの値を、ポータルで設定した Twitter のコンシューマー キーとコンシューマー シークレットの値に置き換えます。

&nbsp;&nbsp;8. 同じセクションで、次の新しいアプリケーション設定を追加し、プレースホルダーを、ポータルのアプリケーション設定で設定した Twitter のアクセス トークンとアクセス トークン シークレットの値に置換します。

    <add key="TWITTER_ACCESS_TOKEN" value="**your_access_token**" />
    <add key="TWITTER_ACCESS_TOKEN_SECRET" value="**your_access_token_secret**" />

モバイル サービスをローカル コンピューター上で実行するときに、モバイル サービスはこれら保存されている設定を使用するため、スケジュールされたジョブを発行する前にテストすることができます。 Azure 内で実行する場合は、モバイル サービスは代わりにポータル内で設定した値を使用し、これら プロジェクト内の設定を無視します。

##<a name="install-linq2twitter"></a>LINQ to Twitter ライブラリをダウンロードしてインストールする

1.  **ソリューション エクスプ ローラー** Visual Studio で、[プロジェクト名を右クリックして [ **NuGet パッケージの管理**します。

2. 左ペインで選択、 **オンライン** カテゴリで、検索 `linq2twitter`, 、] をクリックして **インストール** 上、 **linqtotwitter** をパッケージ化し、読み取りし、ライセンス契約に同意します。

    ![][1]

    これにより、Linq to Twitter ライブラリがモバイル サービス プロジェクトに追加されます。

次に、ツイートを格納するための新しいテーブルを作成する必要があります。

##<a name="create-table"></a>新しい Updates テーブルを作成する

1. Visual Studio の [ソリューション エクスプ ローラーで、DataObjects フォルダーを右クリックし、展開 **追加**, 、] をクリックして **クラス**,   、型 `Updates` の **名前**, 、クリックして **追加**します。

    これにより、Updates クラスに対応する新しいプロジェクト ファイルが作成されます。

2. 右クリック **参照** > **参照の追加]** > **Framework** [ **アセンブリ**, 、確認し、 **System.ComponentModel.DataAnnotations** ] をクリック **[ok]**します。

    ![][7]

    これで新しいアセンブリの参照が追加されます。

2. この新しいクラスで、次のコードを追加 **を使用して** ステートメント。

        using Microsoft.WindowsAzure.Mobile.Service;
        using System.ComponentModel.DataAnnotations;

3. 置き換える、 **更新** クラス定義を次のコード。

        public class Updates
        {
            [Key]
            public int UpdateId { get; set; }
            public long TweetId { get; set; }
            public string Text { get; set; }
            public string Author { get; set; }
            public DateTime Date { get; set; }
        }

4. Models フォルダーを展開し、データ モデルのコンテキスト ファイルを開きます (名前付き *service_name*Context.cs) を返す、型指定された次のプロパティを追加 **DbSet**:

        public DbSet<Updates> Updates { get; set; }

    DbSet に最初にアクセスしたときにデータベース内に作成される Updates テーブルは、ツイート データを格納する目的でサービスによって使用されます。

    >[AZURE.NOTE] 既定のデータベース初期化子を使用する場合、Entity Framework は削除し、Code First のモデル定義内でデータ モデルの変更が検出されるたびに、データベースを再作成します。 このようなデータ モデルの変更を行ってデータベース内で既存のデータを保持するには、Code First Migrations を使用する必要があります。 Azure 内の SQL Database に対して、既定の初期化子を使用することはできません。 詳細については、次を参照してください。 [を使用して Code First Migrations をデータ モデルを更新する方法](mobile-services-dotnet-backend-use-code-first-migrations.md)します。

次に、Twitter にアクセスしてツイート データを新しい Updates テーブルに格納することを目的とした、スケジュールされたジョブを作成します。

##<a name="add-job"></a>新しいスケジュール済みジョブを作成する

1. ScheduledJobs フォルダーを展開し、SampleJob.cs プロジェクト ファイルを開きます。

    このクラスから継承 **ScheduledJob**, 、スケジュール可能な Azure 旧ポータルで固定されたスケジュールでまたはオンデマンドで実行するジョブを表します。

2. SampleJob.cs の内容を次のコードに置き換えます。

        using System;
        using System.Linq;
        using System.Threading;
        using System.Threading.Tasks;
        using System.Web.Http;
        using Microsoft.WindowsAzure.Mobile.Service;
        using Microsoft.WindowsAzure.Mobile.Service.ScheduledJobs;
        using LinqToTwitter;
        using todolistService.Models;
        using todolistService.DataObjects;

        namespace todolistService
        {
            // A simple scheduled job which can be invoked manually by submitting an HTTP
            // POST request to the path "/jobs/sample".
            public class SampleJob : ScheduledJob
            {
                private todolistContext context;
                private string accessToken;
                private string accessTokenSecret;

                protected override void Initialize(ScheduledJobDescriptor scheduledJobDescriptor,
                    CancellationToken cancellationToken)
                {
                    base.Initialize(scheduledJobDescriptor, cancellationToken);

                    // Create a new context with the supplied schema name.
                    context = new todolistContext();
                }

                public async override Task ExecuteAsync()
                {
                    // Try to get the stored Twitter access token from app settings.
                    if (!(Services.Settings.TryGetValue("TWITTER_ACCESS_TOKEN", out accessToken) |
                    Services.Settings.TryGetValue("TWITTER_ACCESS_TOKEN_SECRET", out accessTokenSecret)))
                    {
                        Services.Log.Error("Could not retrieve Twitter access credentials.");
                    }

                    // Create a new authorizer to access Twitter v1.1 APIs
                    // using single-user OAUth 2.0 credentials.
                    MvcAuthorizer auth = new MvcAuthorizer();
                    SingleUserInMemoryCredentialStore store =
                        new SingleUserInMemoryCredentialStore()
                    {
                        ConsumerKey = Services.Settings.TwitterConsumerKey,
                        ConsumerSecret = Services.Settings.TwitterConsumerSecret,
                        OAuthToken = accessToken,
                        OAuthTokenSecret = accessTokenSecret
                    };

                    // Set the credentials for the authorizer.
                    auth.CredentialStore = store;

                    // Create a new LINQ to Twitter context.
                    TwitterContext twitter = new TwitterContext(auth);

                    // Get the ID of the most recent stored tweet.
                    long lastTweetId = 0;
                    if (context.Updates.Count() > 0)
                    {
                        lastTweetId = (from u in context.Updates
                                       orderby u.TweetId descending
                                       select u).Take(1).SingleOrDefault()
                                                    .TweetId;
                    }

                    // Execute a search that returns a filtered result.
                    var response = await (from s in twitter.Search
                                          where s.Type == SearchType.Search
                                          && s.Query == "%23mobileservices"
                                          && s.SinceID == Convert.ToUInt64(lastTweetId + 1)
                                          && s.ResultType == ResultType.Recent
                                          select s).SingleOrDefaultAsync();

                    // Remove retweets and replies and log the number of tweets.
                    var filteredTweets = response.Statuses
                        .Where(t => !t.Text.StartsWith("RT") && t.InReplyToUserID == 0);
                    Services.Log.Info("Fetched " + filteredTweets.Count()
                        + " new tweets from Twitter.");

                    // Store new tweets in the Updates table.
                    foreach (Status tweet in filteredTweets)
                    {
                        Updates newTweet =
                            new Updates
                            {
                                TweetId = Convert.ToInt64(tweet.StatusID),
                                Text = tweet.Text,
                                Author = tweet.User.Name,
                                Date = tweet.CreatedAt
                            };

                        context.Updates.Add(newTweet);
                    }

                    await context.SaveChangesAsync();
                }
                protected override void Dispose(bool disposing)
                {
                    base.Dispose(disposing);
                    if (disposing)
                    {
                        context.Dispose();
                    }
                }
            }
        }

    上記のコードでは、文字列を置き換える必要があります _todolistService_ と _todolistContext_ は、名前空間と、ダウンロードしたプロジェクトの DbContext *mobile & #95; service & #95; 名前*サービスと *mobile & #95; service & #95; 名前*それぞれのコンテキスト。

    上記のコードで、 **ExecuteAsync** オーバーライド メソッドは、というハッシュタグが含まれる最近のツイートを要求する保存された資格情報を使用して Twitter クエリ API を呼び出し `#mobileservices`します。 テーブルに格納される前に、重複しているツイートやリプライが結果から削除されます。

##<a name="run-job-locally"></a>スケジュールされたジョブをローカルでテストする

スケジュールされたジョブを Azure に発行してポータルに登録する前に、ローカルでテストすることができます。

1. Visual Studio で、モバイル サービス プロジェクトがスタートアップ プロジェクトとして設定された状態で、F5 キーを押します。

    これにより、モバイル サービス プロジェクトが開始され、開始ページで新しいブラウザー ウィンドウが表示されます。

2. クリックして **試してみる**, 、] をクリックし、 **POST jobs/{jobname}**します。

    ![][8]

4. クリックして **試すことが**, 、型 `Sample` として、 **{jobname}** パラメーター] をクリックし、 **送信**します。

    ![][9]

    これで新しい POST 要求がサンプル ジョブ エンドポイントに送信されます。 ローカル サービス、 **ExecuteAsync** メソッドが起動します。 このメソッドにブレークポイントを設定してコードをデバッグできます。

3. サーバー エクスプ ローラーで、 **データ接続**, 、**MSTableConnectionString**, 、および **テーブル**; を右クリックして **更新** ] をクリック **テーブル データの表示**します。

    この新しいツイートは、データ テーブル内の行として入力されます。

##<a name="register-job"></a>サービスを発行して新しいジョブを登録する

ジョブを登録する必要があります、 **スケジューラ** タブがあるので、モバイル サービスを使用すると、定義したスケジュールに従って実行できます。

3. モバイル サービス プロジェクトを Azure に対して再発行します。

4.  [Azure classic portal], をモバイル サービス] をクリックして、アプリをクリックします。

2. クリックして、 **スケジューラ** ] タブの [ **[作成**します。

    >[AZURE.NOTE]モバイル サービスを実行すると <em>Free</em> 層、ことができるだけの時間に 1 つのスケジュールされたジョブを実行します。 有料レベルでは、10 個までのスケジュールされたジョブを同時に実行できます。

3. [スケジューラ] ダイアログ ボックスで、次のように入力します。 _サンプル_ の、 **ジョブ名**, を、スケジュールの間隔と単位を設定して、チェック ボタンをクリックします。

    ![][4]

    という名前の新しいジョブを作成この **サンプル**します。

4. 作成した新しいジョブをクリックし、クリックして **一度だけ実行** スクリプトをテストします。

    これにより、ジョブが実行されます。ただし、スケジューラ内では無効の状態のままです。 このページから、いつでもジョブを有効にし、スケジュールを変更することができます。

    >[AZURE.NOTE]POST 要求は、スケジュールされたジョブの開始に使用できます。 ただし、認証の既定はユーザーを対象にしており、要求のヘッダーにアプリケーション キーを含める必要があることを意味します。

4. (省略可能) [Azure classic portal], 、モバイル サービスに関連付けられているデータベースの管理] をクリックします。

    ![][6]

5. Azure クラシック ポータルで、クエリを実行してアプリによって加えられた変更を表示します。 クエリは次のようになりますが、スキーマ名として `todolist` の代わりにモバイル サービスの名前を使用します。

        SELECT * FROM [todolist].[Updates]

これで、スケジュールされた新しいジョブがモバイル サービスに作成されました。 このジョブは、無効化または変更するまで、スケジュールに従って実行されます。

<!-- Anchors. -->
[Register for Twitter access and store credentials]: #get-oauth-credentials
[Download and install the LINQ to Twitter library]: #install-linq2twitter
[Create the new Updates table]: #create-table
[Create a new scheduled job]: #add-job
[Test the scheduled job locally]: #run-job-locally
[Publish the service and register the job]: #register-job
[Next steps]: #next-steps

<!-- Images. -->
[1]: ./media/mobile-services-dotnet-backend-schedule-recurring-tasks/add-linq2twitter-nuget-package.png
[2]: ./media/mobile-services-dotnet-backend-schedule-recurring-tasks/mobile-services-selection.png
[3]: ./media/mobile-services-dotnet-backend-schedule-recurring-tasks/mobile-schedule-new-job-cli.png
[4]: ./media/mobile-services-dotnet-backend-schedule-recurring-tasks/create-new-job.png
[5]: ./media/mobile-services-dotnet-backend-schedule-recurring-tasks/sample-job-run-once.png
[6]: ./media/mobile-services-dotnet-backend-schedule-recurring-tasks/manage-sql-azure-database.png
[7]: ./media/mobile-services-dotnet-backend-schedule-recurring-tasks/add-component-model-reference.png
[8]: ./media/mobile-services-dotnet-backend-schedule-recurring-tasks/mobile-service-start-page.png
[9]: ./media/mobile-services-dotnet-backend-schedule-recurring-tasks/mobile-service-try-this-out.png

<!-- URLs. -->
[Azure classic portal]: https://manage.windowsazure.com/
[Register your apps for Twitter login with Mobile Services]: mobile-services-how-to-register-twitter-authentication.md
[Twitter Developers]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[App settings]: http://msdn.microsoft.com/library/windowsazure/b6bb7d2d-35ae-47eb-a03f-6ee393e170f7
[LINQ to Twitter CodePlex project]: http://linqtotwitter.codeplex.com/

