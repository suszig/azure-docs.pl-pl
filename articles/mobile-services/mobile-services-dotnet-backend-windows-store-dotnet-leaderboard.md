<properties
    pageTitle=".NET バックエンドを使用した Windows ストアのランキング アプリの作成 | Azure Mobile Services"
    description="Azure Mobile Services の .NET バックエンドを使用して、Windows ストアのランキング アプリを作成する方法について説明します。"
    documentationCenter="windows"
    authors="rmcmurray"
    manager="wpickett"
    editor="jimbe"
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/19/2015"
    ms.author="glenga"/>


# Azure Mobile Services の .NET バックエンドによるランキング アプリケーションの作成

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


このチュートリアルでは、Azure Mobile Services と .NET バックエンドを使用して Windows ストア アプリケーションを作成する方法について説明します。 Azure Mobile Services は、組み込まれた認証、監視、プッシュ通知、その他の機能に加え、モバイル アプリケーション作成用のクロスプラットフォーム クライアント ライブラリによって、スケーラブルで、セキュリティで保護されたバックエンドを提供します。 モバイル サービス用の .NET バックエンドがに基づいて [ASP.NET Web API](http://asp.net/web-api), 、REST Api を作成する優れた方法を .NET 開発者に提供します。

## 概要

Web API は、オープン ソースのフレームワークであり、REST API を作成するための最も優れた方法を .NET 開発者に提供します。 Web API ソリューションは Azure Websites でホストするか、.NET バックエンドを使用して Azure Mobile Services でホストするか、またはカスタム プロセスでセルフホストすることもできます。 Mobile Services は、特にモバイル アプリケーション用に設計されたホスティング環境です。 Web API サービスを Mobile Services でホストすると、データ ストレージに加えて次の利点が得られます。

- ソーシャル プロバイダーと Azure Active Directory (AAD) による組み込み認証。
- デバイス固有の通知サービスによるアプリケーションへのプッシュ通知。
- アプリケーションからサービスへのアクセスを簡単にする包括的なクライアント ライブラリのセット。
- 組み込みのログおよび診断機能。

このチュートリアルでは、次のことについて説明します。

- Azure Mobile Services を使用して REST API を作成する。
- サービスを Azure に発行する。
- サービスを使用する Windows ストア アプリケーションを作成する。
- Entity Framework (EF) を使用して外部キー関係とデータ転送オブジェクト (DTO) を作成する。
- ASP.NET Web API を使用してカスタム API を定義する。

このチュートリアルでは [Visual Studio 2013 の最新の更新プログラム](http://go.microsoft.com/fwlink/p/?LinkID=390465)します。


## サンプル アプリケーションについて

*ランキング*は、ゲームのプレーヤーの一覧に、各プレーヤーのスコアと順位を示したものです。 ランキングは大きなゲームの一部である場合も、個別のアプリケーションの場合もあります。 ランキングは実際に使用されているアプリケーションですが、構造が単純なのでチュートリアルに適しています。 アプリケーションのスクリーン ショットを次に示します。

![][1]

アプリケーションを単純にするため、実際のゲームは作成しません。 その代わりに、プレーヤーを追加して、各プレーヤーのスコアを送信できます。 スコアを送信すると、モバイル サービスが新しいランキングを計算します。 バックエンドでは、モバイル サービスが 2 つのテーブルを含むデータベースを作成します。

- Player。 プレーヤーの ID と名前が格納されます。
- PlayerRank。 プレーヤーのスコアと順位が格納されます。

PlayerRank には、Player への外部キーがあります。 各プレーヤーには、0 個または 1 個の PlayerRank があります。

実際のランキング アプリケーションでは、プレーヤーが複数のゲームのスコアを送信できるように、PlayerRank にゲーム ID が格納される場合もあります。

![][2]

クライアント アプリケーションは、Player に対してすべての CRUD 操作を実行できます。 既存の PlayerRank エンティティを読み取ったり削除したりできますが、それらを直接作成したり更新したりはできません。 これは、ランク値がサービスによって計算されるためです。 その代わり、クライアントがスコアを送信すると、サービスによってすべてのプレーヤーの順位が更新されます。

完成したプロジェクトをダウンロード [ここ](http://code.msdn.microsoft.com/Leaderboard-App-with-Azure-9acf63af)します。


## プロジェクトを作成する

Visual Studio を起動し、新しい ASP.NET Web アプリケーション プロジェクトを作成します。 プロジェクトに "Leaderboard" という名前を付けます。

![][3]

Visual Studio 2013 では、ASP.NET Web アプリケーション プロジェクトに Azure Mobile Services 用のテンプレートが含まれています。 このテンプレートを選択し、**[OK]** をクリックします。

![][4]

このプロジェクト テンプレートには、サンプルのコントローラーとデータ オブジェクトが含まれています。

![][5]

これらはチュートリアルには必要ないので、プロジェクトから削除してかまいません。 また WebApiConfig.cs および LeaderboardContext.cs の TodoItem への参照を削除します。

## データ モデルを追加する

使用する [EF Code First](http://msdn.microsoft.com/data/ee712907#codefirst) データベース テーブルを定義します。 DataObjects フォルダーの下には、という名前のクラスを追加 `Player`します。

    using Microsoft.WindowsAzure.Mobile.Service;
    
    namespace Leaderboard.DataObjects
    {
        public class Player : EntityData
        {
            public string Name { get; set; }
        }
    }

という名前の別のクラスを追加 `PlayerRank`します。

    using Microsoft.WindowsAzure.Mobile.Service;
    using System.ComponentModel.DataAnnotations.Schema;
    
    namespace Leaderboard.DataObjects
    {
        public class PlayerRank : EntityData
        {
            public int Score { get; set; }
            public int Rank { get; set; }
    
            [ForeignKey("Id")]
            public virtual Player Player { get; set; }
        }
    }

これらのクラスはいずれも **EntityData** クラスから継承しています。 **EntityData** から派生することで、アプリケーションは Azure Mobile Services 用のクロスプラットフォーム クライアント ライブラリを使用して、データを簡単に使用できるようになります。 **EntityData** も簡単にアプリから [データベース書き込み競合を処理](mobile-services-windows-store-dotnet-handle-database-conflicts.md)します。

`PlayerRank` クラスには、 [ナビゲーション プロパティ](http://msdn.microsoft.com/data/jj713564.aspx) を指す、関連する `Player` エンティティです。  **[ForeignKey]** 属性は EF に示しますが、 `Player` プロパティが外部キーを表します。

## Web API コントローラーを追加する

次に、に対する Web API コント ローラーを追加、 `Player` と `PlayerRank`します。 通常の Web API コントローラーの代わりに、特に Azure Mobile Services 用に設計された "*table controller*" という名前の特殊なコントローラーを追加します。

コント ローラーのフォルダーを右クリックして >  **追加** > **新しいスキャフォールディング項目**します。

![][6]

**[スキャフォールディングの追加]** ダイアログ ボックスで、左側の **[共通]** を展開し、**[Azure Mobile Services]** を選択します。 次に、**[Azure Mobile Services テーブル コントローラー]** を選択します。 **[追加]** をクリックします。

![][7]

**[コントローラーの追加]** ダイアログ ボックスで、次の手順を実行します。

1.  **[モデル クラス]** で、Player を選択します。
2.  **[データ コンテキスト クラス]** で MobileServiceContext を選択します。
3.  コントローラーに "PlayerController" という名前を付けます。
4.  **[追加]** をクリックします。


これにより、PlayerController.cs という名前のファイルがプロジェクトに追加されます。

![][8]

派生したコント ローラー * * TableController<T>**.このクラスは継承 * * ApiController**, は、Azure Mobile Services 用に特化されています。

- ルーティング: の既定のルート、 **TableController** は `/tables/{id}`, ここで、 *table_name* エンティティ名に一致します。 したがって、Player コントローラーのルートは */tables/player/{id}* となります。 このルーティング規則により **TableController** モバイル サービスの一貫した [REST API](http://msdn.microsoft.com/library/azure/jj710104.aspx)します。
- データ アクセス: **TableController** クラスは、データベース操作に対して、データ アクセスの抽象化を定義する **IDomainManager** インターフェイスを使用します。 スキャフォールディングでは **EntityDomainManager** を使用します。これは、EF コンテキストをラップする **IDomainManager** の具象的な実装です。

次に、PlayerRank エンティティ用の第 2 のコントローラーを追加します。 同じ手順を実行しますが、モデル クラスに PlayerRank を選択します。 同じデータ コンテキスト クラスを使用し、新しいクラスは作成しません。 コントローラーに "PlayerRankController" という名前を付けます。

## DTO を使用して関連エンティティを返す

先ほど説明したとおり `PlayerRank` 、関連付けられている `Player` エンティティ。

    public class PlayerRank : EntityData
    {
        public int Score { get; set; }
        public int Rank { get; set; }
    
        [ForeignKey("Id")]
        public virtual Player Player { get; set; }
    }

Mobile Services のクライアント ライブラリではナビゲーション プロパティがサポートされず、ナビゲーション プロパティはシリアル化されません。 たとえば、GET に対する未加工の HTTP 応答をここでは `/tables/playerrank`:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Pragma: no-cache
    Content-Length: 97
    Content-Type: application/json; charset=utf-8
    Expires: 0
    Server: Microsoft-IIS/8.0
    Date: Mon, 21 Apr 2014 17:58:43 GMT
    
    [{"id":"1","rank":1,"score":150},{"id":"2","rank":3,"score":100},{"id":"3","rank":1,"score":150}]

注意して `Player` オブジェクト グラフには含まれません。 Player を含めるには、*データ転送オブジェクト* (DTO) を定義して、オブジェクト グラフを平坦化します。

DTO は、データがネットワークでどのように送信されるかを定義するオブジェクトです。 DTO は、ワイヤ形式をデータベース モデルとは異なる外観にする場合に便利です。 に対する DTO を作成する `PlayerRank`, 、という名前の新しいクラスを追加 `PlayerRankDto` 、DataObjects フォルダーにします。

    namespace Leaderboard.DataObjects
    {
        public class PlayerRankDto
        {
            public string Id { get; set; }
            public string PlayerName { get; set; }
            public int Score { get; set; }
            public int Rank { get; set; }
        }
    }

`PlayerRankController` クラス、LINQ を使用して **選択** に変換する方法 `PlayerRank` インスタンスを `PlayerRankDto` インスタンス。 更新プログラム、 `GetAllPlayerRank` と `GetPlayerRank` コント ローラー メソッドを次のようにします。

    // GET tables/PlayerRank
    public IQueryable<PlayerRankDto> GetAllPlayerRank()
    {
        return Query().Select(x => new PlayerRankDto()
        {
            Id = x.Id,
            PlayerName = x.Player.Name,
            Score = x.Score,
            Rank = x.Rank
        });
    }
    
    // GET tables/PlayerRank/48D68C86-6EA6-4C25-AA33-223FC9A27959
    public SingleResult<PlayerRankDto> GetPlayerRank(string id)
    {
        var result = Lookup(id).Queryable.Select(x => new PlayerRankDto()
        {
            Id = x.Id,
            PlayerName = x.Player.Name,
            Score = x.Score,
            Rank = x.Rank
        });
    
        return SingleResult<PlayerRankDto>.Create(result);
    }

これらの変更により、2 つの GET メソッドが返す `PlayerRankDto` クライアント オブジェクトです。  `PlayerRankDto.PlayerName` プロパティはプレーヤー名に設定します。 この変更を加えた後の応答例を次に示します。

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Pragma: no-cache
    Content-Length: 160
    Content-Type: application/json; charset=utf-8
    Expires: 0
    Server: Microsoft-IIS/8.0
    Date: Mon, 21 Apr 2014 19:57:08 GMT
    
    [{"id":"1","playerName":"Alice","score":150,"rank":1},{"id":"2","playerName":"Bob","score":100,"rank":3},{"id":"3","playerName":"Charles","score":150,"rank":1}]

JSON ペイロードにプレーヤー名が含まれています。

LINQ の Select ステートメントを使用する代わりに、AutoMapper を使用することもできます。 このオプションを使用するには追加のセットアップ コードが必要ですが、それによってドメイン エンティティから DTO への自動マッピングが可能になります。 詳細については、次を参照してください。 [データベース型と AutoMapper を使用した .NET バックエンドでのクライアント型の間のマッピング](http://blogs.msdn.com/b/azuremobile/archive/2014/05/19/mapping-between-database-types-and-client-type-in-the-net-backend-using-automapper.aspx)します。

## カスタム API を定義してスコアを送信する

`PlayerRank` エンティティには、 `ランク` プロパティです。 この値はサーバーによって計算され、クライアントがそれを設定する必要はありません。 クライアントはその代わりにカスタム API を使用して、プレーヤーのスコアを送信します。 サーバーは新しいスコアを取得すると、プレーヤーの順位をすべて更新します。

という名前のクラスを最初に、追加 `PlayerScore` DataObjects フォルダーにします。

    namespace Leaderboard.DataObjects
    {
        public class PlayerScore
        {
            public string PlayerId { get; set; }
            public int Score { get; set; }
        }
    }

`PlayerRankController` クラス、移動、 `MobileServiceContext` 変数コンス トラクターからクラス変数に。

    public class PlayerRankController : TableController<PlayerRank>
    {
        // Add this:
        MobileServiceContext context = new MobileServiceContext();
    
        protected override void Initialize(HttpControllerContext controllerContext)
        {
            base.Initialize(controllerContext);
    
            // Delete this:
            // MobileServiceContext context = new MobileServiceContext();
            DomainManager = new EntityDomainManager<PlayerRank>(context, Request, Services);
        }

次のメソッドを削除 `PlayerRankController`:

- `PatchPlayerRank`
- `PostPlayerRank`
- `DeletePlayerRank`

次のコードを追加 `PlayerRankController`:

    [Route("api/score")]
    public async Task<IHttpActionResult> PostPlayerScore(PlayerScore score)
    {
        // Does this player exist?
        var count = context.Players.Where(x => x.Id == score.PlayerId).Count();
        if (count < 1)
        {
            return BadRequest();
        }
    
        // Try to find the PlayerRank entity for this player. If not found, create a new one.
        PlayerRank rank = await context.PlayerRanks.FindAsync(score.PlayerId);
        if (rank == null)
        {
            rank = new PlayerRank { Id = score.PlayerId };
            rank.Score = score.Score;
            context.PlayerRanks.Add(rank);
        }
        else
        {
            rank.Score = score.Score;
        }
    
        await context.SaveChangesAsync();
    
        // Update rankings
        // See http://stackoverflow.com/a/575799
        const string updateCommand =
            "UPDATE r SET Rank = ((SELECT COUNT(*)+1 from {0}.PlayerRanks " +
            "where Score > (select score from {0}.PlayerRanks where Id = r.Id)))" +
            "FROM {0}.PlayerRanks as r";
    
        string command = String.Format(updateCommand, ServiceSettingsDictionary.GetSchemaName());
        await context.Database.ExecuteSqlCommandAsync(command);
    
        return Ok();
    }

`PostPlayerScore` メソッドには、 `PlayerScore` インスタンスが入力されます。 (クライアントは送信、 `PlayerScore` を HTTP POST 要求でします)。 このメソッドは、次の処理を実行します。

1.  新しい `PlayerRank` 選手にまだ含まれていないデータベースの場合。
2.  プレーヤーのスコアを更新する。
3.  すべてのプレーヤーの順位をバッチ更新する SQL クエリを実行する。

このメソッドのカスタム ルートは **[Route]** 属性で定義されます。

    [Route("api/score")]

このメソッドは、別のコントローラーに配置することもできます。 どちらの方法が優れているということはありません。お好みのコードの整理方法を選んでください。
詳細について、 **[Route]** 属性は、「 [属性は、Web API におけるルーティング](http://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)します。

## Windows ストア アプリケーションを作成する

ここでは、モバイル サービスを使用する Windows ストア アプリケーションについて説明します。 ただし、XAML や UI についてではなく、 主にアプリケーション ロジックについて取り上げます。 完成したプロジェクトをダウンロードする [ここ](http://code.msdn.microsoft.com/Leaderboard-App-with-Azure-9acf63af)します。

ソリューションに新しい Windows ストア アプリケーション プロジェクトを追加します。 ここでは、空のアプリケーション (Windows) テンプレートを使用しました。

![][10]

NuGet Package Manager を使用して、Mobile Services クライアント ライブラリを追加します。 Visual Studio で、**[ツール]** メニューの **NuGet パッケージ マネージャー]** をクリックします。 次に、**[パッケージ マネージャー コンソール]** を選択します。 [パッケージ マネージャー コンソール] ウィンドウで、次のコマンドを入力します。

    Install-Package WindowsAzure.MobileServices -Project LeaderboardApp

-Project スイッチは、パッケージをインストールするプロジェクトを指定します。

## モデル クラスを追加する

Models という名前のフォルダーを作成し、次のクラスを追加します。

    namespace LeaderboardApp.Models
    {
        public class Player
        {
            public string Id { get; set; }
            public string Name { get; set; }
        }
    
        public class PlayerRank
        {
            public string Id { get; set; }
            public string PlayerName { get; set; }
            public int Score { get; set; }
            public int Rank { get; set; }
        }
    
        public class PlayerScore
        {
            public string PlayerId { get; set; }
            public int Score { get; set; }
        }
    }

これらのクラスは、モバイル サービス内のデータ エンティティに直接対応しています。

## ビュー モデルを作成する

Model-View-View Model (MVVM) は、Model-View-Controller (MVC) の一種です。 MVVM パターンを使用すると、アプリケーション ロジックをプレゼンテーションから分離できます。

- モデルは、ドメイン データ (プレーヤー、プレーヤー順位、およびプレーヤー スコア) を表します。
- ビュー モデルは、ビューの抽象表現です。
- ビューは、ビュー モデルを表示し、ユーザー入力をビュー モデルに送信します。 Windows ストア アプリケーションでは、ビューは XAML で定義されます。

![][11]

という名前のクラスを追加 `LeaderboardViewModel`します。

    using LeaderboardApp.Models;
    using Microsoft.WindowsAzure.MobileServices;
    using System.ComponentModel;
    using System.Net.Http;
    using System.Threading.Tasks;
    
    namespace LeaderboardApp.ViewModel
    {
        class LeaderboardViewModel : INotifyPropertyChanged
        {
            MobileServiceClient _client;
    
            public LeaderboardViewModel(MobileServiceClient client)
            {
                _client = client;
            }
        }
    }

ビュー モデルがデータ バインドに参加できるように、ビュー モデルに **INotifyPropertyChanged** を実装します。

    class LeaderboardViewModel : INotifyPropertyChanged
    {
        MobileServiceClient _client;
    
        public LeaderboardViewModel(MobileServiceClient client)
        {
            _client = client;
        }
    
        // New code:
        // INotifyPropertyChanged implementation
        public event PropertyChangedEventHandler PropertyChanged;
    
        public void NotifyPropertyChanged(string propertyName)
        {
            if (PropertyChanged != null)
            {
                PropertyChanged(this,
                    new PropertyChangedEventArgs(propertyName));
            }
        }
    }

次に、監視可能なプロパティを追加します。 XAML では、これらのプロパティに対してデータ バインドが実行されます。

    class LeaderboardViewModel : INotifyPropertyChanged
    {
        // ...
    
        // New code:
        // View model properties
        private MobileServiceCollection<Player, Player> _Players;
        public MobileServiceCollection<Player, Player> Players
        {
            get { return _Players; }
            set
            {
                _Players = value;
                NotifyPropertyChanged("Players");
            }
        }
    
        private MobileServiceCollection<PlayerRank, PlayerRank> _Ranks;
        public MobileServiceCollection<PlayerRank, PlayerRank> Ranks
        {
            get { return _Ranks; }
            set
            {
                _Ranks = value;
                NotifyPropertyChanged("Ranks");
            }
        }
    
        private bool _IsPending;
        public bool IsPending
        {
            get { return _IsPending; }
            set
            {
                _IsPending = value;
                NotifyPropertyChanged("IsPending");
            }
        }
    
        private string _ErrorMessage = null;
        public string ErrorMessage
        {
            get { return _ErrorMessage; }
            set
            {
                _ErrorMessage = value;
                NotifyPropertyChanged("ErrorMessage");
            }
        }
    }

`IsPending` 非同期操作が保留中の間は、プロパティが true で、サービスです。  `ErrorMessage` プロパティは、サービスからのエラー メッセージを保持します。

最後に、サービス レイヤーへの呼び出しを行うメソッドを追加します。

    class LeaderboardViewModel : INotifyPropertyChanged
    {
        // ...
    
        // New code:
        // Service operations
        public async Task GetAllPlayersAsync()
        {
            IsPending = true;
            ErrorMessage = null;
    
            try
            {
                IMobileServiceTable<Player> table = _client.GetTable<Player>();
                Players = await table.OrderBy(x => x.Name).ToCollectionAsync();
            }
            catch (MobileServiceInvalidOperationException ex)
            {
                ErrorMessage = ex.Message;
            }
            catch (HttpRequestException ex2)
            {
                ErrorMessage = ex2.Message;
            }
            finally
            {
                IsPending = false;
            }
        }
    
        public async Task AddPlayerAsync(Player player)
        {
            IsPending = true;
            ErrorMessage = null;
    
            try
            {
                IMobileServiceTable<Player> table = _client.GetTable<Player>();
                await table.InsertAsync(player);
                Players.Add(player);
            }
            catch (MobileServiceInvalidOperationException ex)
            {
                ErrorMessage = ex.Message;
            }
            catch (HttpRequestException ex2)
            {
                ErrorMessage = ex2.Message;
            }
            finally
            {
                IsPending = false;
            }
        }
    
        public async Task SubmitScoreAsync(Player player, int score)
        {
            IsPending = true;
            ErrorMessage = null;
    
            var playerScore = new PlayerScore()
            {
                PlayerId = player.Id,
                Score = score
            };
    
            try
            {
                await _client.InvokeApiAsync<PlayerScore, object>("score", playerScore);
                await GetAllRanksAsync();
            }
            catch (MobileServiceInvalidOperationException ex)
            {
                ErrorMessage = ex.Message;
            }
            catch (HttpRequestException ex2)
            {
                ErrorMessage = ex2.Message;
            }
            finally
            {
                IsPending = false;
            }
        }
    
        public async Task GetAllRanksAsync()
        {
            IsPending = true;
            ErrorMessage = null;
    
            try
            {
                IMobileServiceTable<PlayerRank> table = _client.GetTable<PlayerRank>();
                Ranks = await table.OrderBy(x => x.Rank).ToCollectionAsync();
            }
            catch (MobileServiceInvalidOperationException ex)
            {
                ErrorMessage = ex.Message;
            }
            catch (HttpRequestException ex2)
            {
                ErrorMessage = ex2.Message;
            }
            finally
            {
                IsPending = false;
            }
         }
    }

## MobileServiceClient インスタンスを追加する

開いている、 *App.xaml.cs*ファイルを追加、 **MobileServiceClient** インスタンスを `アプリ` クラスです。

    // New code:
    using Microsoft.WindowsAzure.MobileServices;
    
    namespace LeaderboardApp
    {
        sealed partial class App : Application
        {
            // New code.
            // TODO: Replace 'port' with the actual port number.
            const string serviceUrl = "http://localhost:port/";
            public static MobileServiceClient MobileService = new MobileServiceClient(serviceUrl);
    
    
            // ...
        }
    }

ローカルでデバッグを行うときには、モバイル サービスが IIS Express で実行されます。 Visual Studio では、ローカル URL が http://localhost であるため、ランダムなポート番号が割り当てられます。*ポート*, ここで、 *ポート* のポート番号です。 ポート番号を確認するには、F5 キーを押して、Visual Studio でサービスをデバッグ用に開始します。 Visual Studio でブラウザーが起動し、サービス URL に移動します。 ローカル URL は、プロジェクト プロパティの **[Web]** でも確認できます。

## メイン ページを作成する

メイン ページで、ビュー モデルのインスタンスを追加します。 次に、ビュー モデルをページの **DataContext** として設定します。

    public sealed partial class MainPage : Page
    {
        // New code:
        LeaderboardViewModel viewModel = new LeaderboardViewModel(App.MobileService);
    
        public MainPage()
        {
            this.InitializeComponent();
            // New code:
            this.DataContext = viewModel;
        }
    
       // ...

前に述べたとおり、ここではアプリケーションの XAML のすべては示していません。 MVVM パターンの利点の 1 つは、プレゼンテーションをアプリケーション ロジックから分離することであるため、サンプル アプリケーションが気に入らない場合でも、UI を簡単に変更できます。

プレーヤーの一覧は、**ListBox** に表示されます。

    <ListBox Width="200" Height="400" x:Name="PlayerListBox"
        ItemsSource="{Binding Players}" DisplayMemberPath="Name"/>

ランキングは **ListView** に表示されます。

    <ListView x:Name="RankingsListView" ItemsSource="{Binding Ranks}" SelectionMode="None">
        
        <ListView.ItemTemplate>
            <DataTemplate>
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="*"/>
                        <ColumnDefinition Width="2*"/>
                        <ColumnDefinition Width="*"/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Text="{Binding Path=Rank}"/>
                    <TextBlock Text="{Binding Path=PlayerName}" Grid.Column="1"/>
                    <TextBlock Text="{Binding Path=Score}" Grid.Column="2"/>
                </Grid>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>

データ バインドはすべてビュー モデルをとおして実行されます。

## モバイル サービスを発行する

この手順では、モバイル サービスを Microsoft Azure に発行し、ライブ サービスを使用するようにアプリケーションを変更します。

ソリューション エクスプローラーで Leaderboard プロジェクトを右クリックし、**[発行]** をクリックします。

![][12]

**[発行]** ダイアログ ボックスで、**[Azure Mobile Services]** をクリックします。

![][13]

まだ Azure アカウントにサインインしていない場合は、**[サインイン]** をクリックします。

![][14]


既存のモバイル サービスを選択するか、または **[新規]** をクリックして新しく作成します。 次に、**[OK]** をクリックして発行します。

![][15]

発行プロセスで、自動的にデータベースが作成されます。 接続文字列を構成する必要はありません。

これで、ランキング アプリケーションをライブ サービスに接続する準備ができました。 次の 2 つが必要です。

- サービスの URL
- アプリケーション キー

これらはいずれも Azure クラシック ポータルから取得できます。 ポータルで **[Mobile Services]** をクリックし、モバイル サービスをクリックします。 サービス URL はダッシュボード タブに表示されています。 アプリケーション キーを取得するには、**[キーの管理]** をクリックします。

![][16]

**[アクセス キーの管理]** ダイアログ ボックスで、アプリケーション キーの値をコピーします。

![][17]


サービス URL とアプリケーション キーを **MobileServiceClient** コンストラクターに渡します。

    sealed partial class App : Application
    {
        // TODO: Replace these strings with the real URL and key.
        const string serviceUrl = "https://yourapp.azure-mobile.net/";
        const string appKey = "YOUR ACCESSS KEY";
    
        public static MobileServiceClient MobileService = new MobileServiceClient(serviceUrl, appKey);
    
       // ...

アプリケーションを実行すると、実際のサービスとの通信が開始されます。

## 次のステップ

* [説明の詳細については、Azure Mobile Services]
* [Web API の詳細について learn]
* [データベース書き込み競合の処理]
* [プッシュ通知の追加]。たとえば、ときにだれかが新しいプレーヤーを追加またはスコアを更新します。
* [認証の使用の開始を取得]







[overview]: #overview 
[about the sample app]: #about-the-sample-app 
[create the project]: #create-the-project 
[add data models]: #add-data-models 
[add web api controllers]: #add-web-api-controllers 
[use a dto to return related entities]: #use-a-dto-to-return-related-entities 
[define a custom api to submit scores]: #define-a-custom-api-to-submit-scores 
[create the windows store app]: #create-the-windows-store-app 
[add model classes]: #add-model-classes 
[create a view model]: #create-a-view-model 
[add a mobileserviceclient instance]: #add-a-mobileserviceclient-instance 
[create the main page]: #create-the-main-page 
[publish your mobile service]: #publish-your-mobile-service 
[next steps]: #next-steps 
[1]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/01leaderboard.png 
[2]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/02leaderboard.png 
[3]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/03leaderboard.png 
[4]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/04leaderboard.png 
[5]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/05leaderboard.png 
[6]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/06leaderboard.png 
[7]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/07leaderboard.png 
[8]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/08leaderboard.png 
[9]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/09leaderboard.png 
[10]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/10leaderboard.png 
[11]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/11leaderboard.png 
[12]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/12leaderboard.png 
[13]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/13leaderboard.png 
[14]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/14leaderboard.png 
[15]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/15leaderboard.png 
[16]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/16leaderboard.png 
[17]: ./media/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/17leaderboard.png 
[learn more about azure mobile services]: /develop/mobile/resources/ 
[learn more about web api]: http://asp.net/web-api 
[handle database write conflicts]: mobile-services-windows-store-dotnet-handle-database-conflicts.md 
[add push notifications]: ../notification-hubs-windows-store-dotnet-get-started.md 
[get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet 

