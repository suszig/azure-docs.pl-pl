<properties
    pageTitle=".NET 多層アプリケーション | Microsoft Azure"
    description="Service Bus キューを使用して層間で通信する多層アプリケーションを Azure で開発するのに役立つ .NET チュートリアルです。"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/07/2015"
    ms.author="sethm"/>

# Azure Service Bus キューを使用する .NET 多層アプリケーション

## はじめに

Microsoft Azure 向けアプリケーションは、Visual Studio および無料の Azure SDK for .NET を使用して簡単に開発できます。 VisualStudio を所有していない場合でも、SDK をインストールすると自動的に Visual Studio Express がインストールされるため、Azure 向けの開発を完全に無料で始めることができます。 この記事は、Azure を初めて使用するユーザーを対象としています。 使用するアプリケーションがこのチュートリアルを完了すると、
多階層アプリケーションのしくみを実証する、
多階層アプリケーションの動作を示しています。

学習内容:

-   1 回のダウンロードとインストールで、コンピューターを
    Azure 開発用にする方法
-   Azure 開発のための Visual Studio の使用方法
-   Web ロールおよび Worker ロールを使用して
    多階層アプリケーションを作成する方法
-   Service Bus キューを使用して各層間で通信する方法

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

このチュートリアルでは、Azure のクラウド サービスで多層アプリケーションを構築して実行します。 フロントエンドは ASP.NET MVC Web ロール、バックエンドは worker ロールになります。 同じ多層アプリケーションを作成するときに、クラウド サービスではなく Azure Web サイトにデプロイされる Web プロジェクトとしてフロントエンドを実装することもできます。 Azure の web サイトのフロント エンドで別途行う操作方法については、「、 [次のステップ](#nextsteps) セクションです。

次のスクリーン ショットに、完成したアプリケーションを示します。

![][0]

> [AZURE.NOTE] Azure では、ストレージ キュー機能も提供します。 Azure のストレージ キューと Service Bus キューの詳細については、次を参照してください。 [Azure キューと Azure Service Bus キューの比較][sbqueuecomparison]します。  

## シナリオの概要: ロール間通信

注文を送信して処理するには、Web ロールで実行されているフロントエンド UI コンポーネントと、
web ロールで実行する中間層ロジックと対話する必要があります。
対話する必要があります。 この例では、Service Bus で仲介されたメッセージングを
層間の通信に使用します。

Web 層と中間層との間で仲介されたメッセージングを使用すると、
2 つのコンポーネントの結合を解除できます。 直接メッセージング (TCP または HTTP) とは異なり、
Web 層と中間層は直接接続されません。代わりに
作業単位をメッセージとして Service Bus にプッシュする信頼性の高い
中間層で作業単位を使用および処理する準備ができるまで確実に保持されます。

Service Bus 仲介型メッセージングをサポートする 2 つのエンティティを提供します。
仲介されたメッセージングをサポートします。 キューの場合、キューに送信される各メッセージが
単一の受信者によって使用されます。 トピックはパブリッシュ/サブスクライブ
パブリッシュされた各メッセージは、行われた場所と、そのトピックに登録されているサブスクリプションが使用できるパターンです。 サブスクリプションごとに論理的に
固有のメッセージ キューが保持されます。 また、サブスクリプションを
フィルタの規則により構成することもできます。この規則により、サブスクリプション キューに渡されるメッセージのセットが
フィルターと一致するものに制限されます。 次のコード例します。
Service Bus キューにアクセスするために必要なライブラリが含まれています。

![][1]

この通信メカニズムには、直接メッセージングと比較した場合に、
メッセージング]。

-   **一時的な結合の解除:** 非同期メッセージング パターンの場合、
    プロデューサーとコンシューマーを同時にオンラインにする必要がありません。 サービス
    コンシューマー側の受信準備が完了するまで、確実に Bus にメッセージを
    格納します。 これにより、分散型アプリケーションのコンポーネント
    メンテナンスやコンポーネント クラッシュの場合でも
    システム全体に影響を与えずに
    自動的に切断できます。 コンシューマー側アプリケーションがさらに、のみがあります。
    1 日の特定の時間にのみオンラインにすることもできます。

-   **負荷平準化。** 多くのアプリケーションでは、システム負荷が時間の
    時間、各作業単位に必要な処理時間は、
    通常一定に保たれます。 メッセージ プロデューサーとメッセージ コンシューマーの間を
    コンシューマー側アプリケーション (ワーカー) はだけピーク時ではなく、平均負荷に対応できる準備する必要のあるキュー手段
    意味します。 キューの深さが増大し、受信コントラクト
    増減します。 このため、アプリケーション負荷への対応に必要な
    インフラストラクチャの観点から直接費用を節約できます。

-   **負荷分散:** 負荷の増大に合わせて、キューからの読み取りのために
    Worker プロセスをさらに追加できます。 各メッセージは、1 つの Worker プロセスのみで
    処理されます。 また、このプルベースの負荷分散では、
    worker マシンでもの最適な使用を有効にします
    処理能力の観点からの worker マシンが異なるため、
    Worker マシンの処理能力が異なっていても可能です。 多くの場合、このパターンを
    呼ばれる、 *競合するコンシューマー* パターンです。

    ![][2]

以降のセクションでは、このアーキテクチャを実装するコードについて説明します。

## 開発環境を設定する

Azure アプリケーションの開発を開始する前に、ツールをダウンロードして、開発環境を設定します。

1.  Azure SDK for .NET をインストールするには、次のリンクをクリックします。

    [ツールと SDK の入手][]

2.  使用している Visual Studio のバージョンに対応するリンクを選択します。 このチュートリアルの手順では、Visual Studio 2013 を使用します。

    ![][32]

4.  インストール ファイルの実行または保存を求めるメッセージが表示されたら、
    **実行**します。

    ![][3]

5.  Web Platform Installer で、クリックして **インストール** し、インストールを実行します。

    ![][33]

6.  すべてのものがある場合、インストールが完了したら、
    アプリの開発を開始するために必要なです。 SDK には、ことができるツールが含まれています。
    Visual Studio で Azure アプリケーションの開発。 Visual Studio が
    インストールされていない場合は、無料の
    Visual Studio Express for Web をインストールすることも可能です。

## Service Bus の名前空間を設定する

次の手順では、サービス名前空間を作成し、Shared Access Signature (SAS) キーを取得します。 名前空間のアプリケーション境界を提供します。
各アプリケーションの境界を指定します。 SAS キーは、します。
サービス名前空間がシステムによって生成されます。
されます。 名前空間と SAS キーの組み合わせ
アクセスを認証する Service Bus の資格情報を提供します。
想定しています。

### Azure クラシック ポータルを使用してサービス名前空間を設定する

1.  ログオン、 [Azure クラシック ポータル][]します。

2.  ポータルの左側のナビゲーション ウィンドウで次のようにクリックします。
    **Service Bus**します。

3.  ポータルの下のウィンドウで、クリックして **作成**します。

    ![][6]

4.   **新しい名前空間を追加** ページで、名前空間の名前を入力します。 その名前が使用できるかどうかがすぐに自動で確認されます。

    ![][7]

5.  入力した名前が利用できることを確認できたら、名前空間を
    ホストする国または地域を選択します (
    コンピューター リソースをデプロイするのと同じ国/地域を
    使用してください)。 また、選択することを確認 **メッセージング** 名前空間に **型** フィールド、および **標準** で、 **メッセージング層** フィールドです。

    > [AZURE.IMPORTANT] 選択、 **同じリージョン** するかを選択します。
    アプリケーションをデプロイします。 そうすることで、パフォーマンスが最高になります。

6.  OK チェック マークをクリックします。 これで、システムによってサービス名前空間が
    作成され有効になります。 システムでアカウントのリソースがプロビジョニングされるまで
    数分かかる場合があります。

    ![][27]

7.  メイン ウィンドウで、使用するサービス名前空間の名前をクリックします。

    ![][30]

8. クリックして **接続情報**します。

    ![][31]

9.   **接続情報にアクセス** ] ウィンドウで、SAS キーとキー名を含む接続文字列を検索します。

    ![][35]

10.  これらの資格情報を書き留めておくか、クリップボードにコピーします。

## Web ロールを作成する

このセクションでは、アプリケーションのフロントエンドを作成します。 まず、します。
アプリケーションで表示するさまざまなページを作成します。
その後、項目をサービス バスに送信するためのコードを追加します。
キューおよびキューに関するステータス情報を表示します。

### プロジェクトを作成する

1.  管理者特権を使用して、Microsoft Visual Studio
    2013 または Microsoft Visual Studio Express を起動します。 管理者特権で Visual Studio を開始するには、
    管理者特権を持つ studio を右クリックし **Microsoft Visual
    Studio 2013 (または Microsoft Visual Studio Express)** と
    クリックし、 **管理者として実行**します。 Azure コンピューティング エミュレーター
    この記事ではで後述します Visual Studio を
    管理者特権で起動されます。

    Visual Studio での **ファイル** ] メニューのをクリックして **新規**, 、し、
    クリックして **プロジェクト**します。

    ![][8]

2.   **インストールされたテンプレート**, [ **Visual c#**, 、クリックして **クラウド** と
    クリックして **Azure クラウド サービス**します。 プロジェクト名を
    **MultiTierApp**します。 クリックして **OK**します。

    ![][9]

3.   **.NET Framework 4.5** の役割をダブルクリックして **ASP.NET Web
    ロール**します。

    ![][10]

4.  ポインターを合わせる **WebRole1** [ **Azure クラウド サービス ソリューション**, 、] をクリックして
    鉛筆のアイコンを web ロールの名前と **FrontendWebRole**します。 クリックして **OK**します。 (「FrontEnd」ではなく「Frontend」と入力してください。小文字の "e" です)。

    ![][11]

5.   **新しい ASP.NET プロジェクト** ダイアログ ボックスで、 **テンプレートを選択して** 一覧で、クリックして **MVC**,、
    クリックして **OK**します。

    ![][12]

6.   **ソリューション エクスプ ローラー**, を右クリックして **参照**, 、] をクリックし、
    **NuGet パッケージの管理** または **ライブラリ パッケージ参照の追加**します。

7.  選択 **オンライン** ] ダイアログ ボックスの左側にあります。 検索
    "**Service Bus**"を選択し、 **Microsoft Azure サービス
    Bus** 項目。 次に、インストールを完了し、このダイアログ ボックスを閉じます。

    ![][13]

8.  これで、必要なクライアント アセンブリを参照できるようになり、
    新しいコード ファイルが追加されました。

9.   **ソリューション エクスプ ローラー**, を右クリックして **モデル** ] をクリック **追加**,、
    クリックし、 **クラス**します。  **名前** ボックスで、名前を入力します
    **OnlineOrder.cs**します。 クリックして **追加**します。

### Web ロール用のコードの作成

このセクションでは、アプリケーションで表示するさまざまなページを作成します。

1.  Visual Studio の OnlineOrder.cs ファイルで
    既存の名前空間の定義を次のコードに置き換えます。

        namespace FrontendWebRole.Models
        {
            public class OnlineOrder
            {
                public string Customer { get; set; }
                public string Product { get; set; }
            }
        }

2.   **ソリューション エクスプ ローラー**, 、ダブルクリック
    **Controllers \homecontroller.cs**します。 次の追加 **を使用します。**
    名前空間を含めるファイルの先頭に追加し、Service Bus と同様に
    モデルの作成した Service Bus。

        using FrontendWebRole.Models;
        using Microsoft.ServiceBus.Messaging;
        using Microsoft.ServiceBus;

3.  また、Visual Studio の HomeController.cs ファイルで
    既存の名前空間の定義を次のコードに置き換えます。 このコードには、
    項目のキューへの送信を処理するためのメソッドが含まれています。

        namespace FrontendWebRole.Controllers
        {
            public class HomeController : Controller
            {
                public ActionResult Index()
                {
                    // Simply redirect to Submit, since Submit will serve as the
                    // front page of this application.
                    return RedirectToAction("Submit");
                }

                public ActionResult About()
                {
                    return View();
                }

                // GET: /Home/Submit.
                // Controller method for a view you will create for the submission
                // form.
                public ActionResult Submit()
                {
                    // Will put code for displaying queue message count here.

                    return View();
                }

                // POST: /Home/Submit.
                // Controller method for handling submissions from the submission
                // form.
                [HttpPost]
                // Attribute to help prevent cross-site scripting attacks and
                // cross-site request forgery.  
                [ValidateAntiForgeryToken]
                public ActionResult Submit(OnlineOrder order)
                {
                    if (ModelState.IsValid)
                    {
                        // Will put code for submitting to queue here.

                        return RedirectToAction("Submit");
                    }
                    else
                    {
                        return View(order);
                    }
                }
            }
        }

4.   **ビルド** ] メニューのをクリックして **ソリューションのビルド** をこれまで作業の精度をテストします。

5.  ここで、ビューを作成、 **Submit()** メソッドを
    前に作成します。 内で右クリックし、 **Submit()** メソッドを選択し、
    **ビューを追加する**です。

    ![][14]

6.  ビューを作成するためのダイアログ ボックスが表示されます。  **テンプレート** 一覧で、選択 **作成**します。  **モデル クラス** 一覧で、クリックして、 **OnlineOrder** クラスです。

    ![][15]

7.  クリックして **追加**します。

8.  次に、アプリケーションの表示名を変更します。  **ソリューション エクスプ ローラー**, をダブルクリックして、
    **Views\Shared\\_Layout.cshtml** ファイルを Visual で開く
    Studio エディターで開きます。

9.  箇所をすべて **My ASP.NET Application** と
    **Litware's Products**します。

10. 削除、 **ホーム**, 、**に関する**, 、および **連絡先** リンクします。 以下の強調表示されたコードを削除してください。

    ![][28]

11. 最後に、キューに関する情報を表示できるように、送信ページを
    変更します。  **ソリューション エクスプ ローラー**, をダブルクリックして、
    **Views\Home\Submit.cshtml** ファイルを Visual Studio で開く
    エディターで開きます。 後に次の行を追加 **& lt; h2 > Submit & lt;/h2 >**します。 ここでは、
     **ViewBag.MessageCount** が空です。 この値は後で入力します。

        <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>

12. これで、UI の実装が終わりました。 キーを押して **f5 キーを押して** を実行する、
    期待どおりに表示されることを確認します。

    ![][17]

### Service Bus キューに項目を送信するためのコードの作成

次に、項目をキューに送信するためのコードを追加します。 まず、します。
Service Bus キューの接続を含むクラスを作成します。
作成します。 次に、接続を初期化しますから
Global.aspx.cs から接続を初期化します。 最後に、更新、送信コードを表示
HomeController.cs で作成した送信コードを更新し、項目を実際に
Service Bus のキューです。

1.   **ソリューション エクスプ ローラー**, を右クリックして **FrontendWebRole** (ロールでなく、プロジェクトを右クリック)。 クリックして **追加**, 、クリックして **クラス**します。

2.  クラスに「QueueConnector.cs」という名前を付けます。 クリックして **追加** クラスを作成します。

3.  接続情報をカプセル化して、Service Bus のキューへの接続を初期化するコードを追加します。 QueueConnector.cs に次のコードを追加し、値を入力 **名前空間** (サービス名前空間) と **yourKey**, から以前に取得した SAS キーは、 [Azure クラシック ポータル][]します。

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Web;
        using Microsoft.ServiceBus.Messaging;
        using Microsoft.ServiceBus;

        namespace FrontendWebRole
        {
            public static class QueueConnector
            {
                // Thread-safe. Recommended that you cache rather than recreating it
                // on every request.
                public static QueueClient OrdersQueueClient;

                // Obtain these values from the portal.
                public const string Namespace = "your service bus namespace";

                // The name of your queue.
                public const string QueueName = "OrdersQueue";

                public static NamespaceManager CreateNamespaceManager()
                {
                    // Create the namespace manager which gives you access to
                    // management operations.
                    var uri = ServiceBusEnvironment.CreateServiceUri(
                        "sb", Namespace, String.Empty);
                    var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey");
                    return new NamespaceManager(uri, tP);
                }

                public static void Initialize()
                {
                    // Using Http to be friendly with outbound firewalls.
                    ServiceBusEnvironment.SystemConnectivity.Mode =
                        ConnectivityMode.Http;

                    // Create the namespace manager which gives you access to
                    // management operations.
                    var namespaceManager = CreateNamespaceManager();

                    // Create the queue if it does not exist already.
                    if (!namespaceManager.QueueExists(QueueName))
                    {
                        namespaceManager.CreateQueue(QueueName);
                    }

                    // Get a client to the queue.
                    var messagingFactory = MessagingFactory.Create(
                        namespaceManager.Address,
                        namespaceManager.Settings.TokenProvider);
                    OrdersQueueClient = messagingFactory.CreateQueueClient(
                        "OrdersQueue");
                }
            }
        }

    このチュートリアルで後では、名前を格納する方法を学習します、 **名前空間** と SAS キーの構成ファイル内の値。

4.  これで、確実、 **初期化** メソッドが呼び出されます。  **ソリューション エクスプ ローラー**, をダブルクリックして **Global.asax\Global.asax.cs**します。

5.  下位に次の行を追加、 **Application_Start**
    メソッドを呼び出します。

        FrontendWebRole.QueueConnector.Initialize();

6.  最後に、事前に作成した web コードを更新するには
    アイテムをキューに送信します。  **ソリューション エクスプ ローラー**,、
    ダブルクリックして **controllers \homecontroller.cs**します。

7.  更新プログラム、 **Submit()** メッセージの数を取得するには、次のようにメソッド
    キューです。

        public ActionResult Submit()
        {
            // Get a NamespaceManager which allows you to perform management and
            // diagnostic operations on your Service Bus queues.
            var namespaceManager = QueueConnector.CreateNamespaceManager();

            // Get the queue, and obtain the message count.
            var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
            ViewBag.MessageCount = queue.MessageCount;

            return View();
        }

8.  更新プログラム、 **Submit (OnlineOrder order)** を送信するには、次のようにメソッド
    キューに注文情報。

        public ActionResult Submit(OnlineOrder order)
        {
            if (ModelState.IsValid)
            {
                // Create a message from the order.
                var message = new BrokeredMessage(order);

                // Submit the order.
                QueueConnector.OrdersQueueClient.Send(message);
                return RedirectToAction("Submit");
            }
            else
            {
                return View(order);
            }
        }

9.  ここで再び、アプリケーションを実行します。 注文を
    メッセージの数が増加します。

    ![][18]

## クラウド構成マネージャー

 [GetSetting][] メソッドで、
[Microsoft.WindowsAzure.Configuration.CloudConfigurationManager][] クラス
構成ストアから構成設定を読み取ることができます、
プラットフォームです。 たとえば、次のコードの実行
web またはワーカー ロールで、 [GetSetting][] メソッドの読み取り、
ServiceConfiguration.cscfg ファイルで、標準的なコードが実行されている場合、
コンソール アプリ、 [GetSetting][] メソッドは、app.config ファイルを読み取ります。

Service Bus 名前空間の接続文字列を格納するかどうかは、
使用する構成ファイル、 [GetSetting][] メソッドが接続を読み取る
インスタンス化に使用できる文字列、 [NamespaceMananger][] オブジェクトです。 Visual Studio Online アカウントは
使用して、 [NamespaceMananger][] 、Service Bus 名前空間を構成するインスタンス
プログラムを使用しています。 同じ接続文字列を使用するには、クライアントをインスタンス化
オブジェクト (たとえば [QueueClient][], 、[TopicClient][], 、および [EventHubClient][]
オブジェクト) を送信するなどの実行時の操作を実行して、
メッセージを受信します。

### 接続文字列

クライアントをインスタンス化する (サービス バスなど [QueueClient][])、接続文字列としての構成情報を表すことができます。 その接続文字列を使用してクライアントの型をインスタンス化する `CreateFromConnectionString()` メソッドがクライアント側には用意されています。 たとえば、次の構成セクションがあるとします。

    <ConfigurationSettings>
    ...
        <Setting name="Microsoft.ServiceBus.ConnectionString" value="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedSecretIssuer=RootManageSharedAccessKey;SharedSecretValue=[yourKey]" />
    </ConfigurationSettings>

次のコードでは、接続文字列を取得し、キューを作成し、キューへの接続を初期化します。

    QueueClient Client;

    string connectionString =
     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

    var namespaceManager =
     NamespaceManager.CreateFromConnectionString(connectionString);

    if (!namespaceManager.QueueExists(QueueName))
    {
        namespaceManager.CreateQueue(QueueName);
    }

    // Initialize the connection to Service Bus queue.
    Client = QueueClient.CreateFromConnectionString(connectionString, QueueName);

次のセクションのコードを使用して、 [CloudConfigurationManager][Microsoft.WindowsAzure.Configuration.CloudConfigurationManager] クラスです。

## worker ロールを作成する

次に、送信された注文を処理する Worker ロールを
作成します。 この例では、 **と Service Bus キュー Worker ロール** Visual Studio プロジェクト テンプレートです。 まず、Visual Studio のサーバー エクスプローラーを使用して必要な資格情報を取得します。

1. Visual Studio を Azure アカウントに接続していることを確認します。

2.  Visual Studio での **ソリューション エクスプ ローラー** を右クリックし、
    **ロール** の下のフォルダー、 **MultiTierApp** プロジェクトです。

3.  クリックして **追加**, 、] をクリックし、 **新しいワーカー ロール プロジェクト**します。  **新しいロール プロジェクトの追加** ] ダイアログ ボックスが表示されます。

    ![][26]

4.   **新しいロール プロジェクトの追加** ] ダイアログ ボックスをクリックする **と Service Bus キュー Worker ロール**します。

    ![][23]

5.   **名前** ボックスに、プロジェクトの名前 **OrderProcessingRole**します。 クリックして **追加**します。

6.   **サーバー エクスプ ローラー**, 、サービス名前空間の名前を右クリックし、クリックして **プロパティ**します。 Visual Studio で **プロパティ** ] ウィンドウで、最初のエントリに必要な承認資格情報を含んだ名前空間のエンドポイントが含まれる接続文字列が含まれています。 たとえば、次のスクリーン ショットのようになります。 ダブルクリックして **ConnectionString**, 、キーを押します **Ctrl + C** にこの文字列をクリップボードにコピーします。

    ![][24]

7.   **ソリューション エクスプ ローラー**, を右クリックし、 **[orderprocessingrole]** 手順 5 で作成した (を右 **OrderProcessingRole** [ **ロール**, 、およびクラスではなく)。 クリックし、 **プロパティ**します。

8.   **設定** のタブ、 **プロパティ** ] ダイアログ ボックスの内側をクリック、 **値** のボックス **Microsoft.ServiceBus.ConnectionString**, 、し、手順 6. でコピーしたエンドポイント値を貼り付けます。

    ![][25]

9.  作成、 **OnlineOrder** をキューから処理するときに、注文を表すクラス。 作成済みのクラスを再利用することができます。  **ソリューション エクスプ ローラー**, を右クリックし、 **OrderProcessingRole** プロジェクト (右クリックして、プロジェクトをロールではありません)。 クリックして **追加**, 、] をクリックし、 **既存項目の**です。

10. サブフォルダーに移動 **frontendwebrole \models**, をダブルクリックし、 **OnlineOrder.cs** このプロジェクトに追加します。

11.  **WorkerRole.cs**, の値を置き換える、 **QueueName** に変数が **WorkerRole.cs** から `"ProcessingQueue"` に `"OrdersQueue"` 次のコードに示すようにします。

        // The name of your queue.
        const string QueueName = "OrdersQueue";

12. WorkerRole.cs ファイルの先頭に次の using ステートメントを追加します。

        using FrontendWebRole.Models;

13. `Run()` 関数の `OnMessage` の呼び出し内で、`try` 句に次のコードを追加します。

        Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
        // View the message as an OnlineOrder.
        OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
        Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
        receivedMessage.Complete();

14. これでアプリケーションが完成しました。 前の手順で実行したように
    ソリューション エクスプ ローラーで、MultiTierApp のプロジェクトを右クリックしてアプリケーション
    選択すると **スタートアップ プロジェクトとして設定**, 、し、f5 キーを押します。 なお、
    メッセージ カウントは増えない、ワーカー ロールの項目を処理するため
    キューおよびマークからとして、次の操作を完了します。 トレース出力を確認できます。
    Azure コンピューティング エミュレーター UI を表示するワーカー ロールです。 クラスターの
    タスク バーの通知領域にあるエミュレーター アイコンを右クリックし、
    タスク バーの領域を選択すると、 **[コンピューティング エミュレーター UI**します。

    ![][19]

    ![][20]

## 次のステップ  

Service Bus の詳細については、次のリソースを参照してください。  

* [Azure Service Bus][sbmsdn]  
* [Service Bus サービス ページ][sbwacom]  
* [Service Bus キューの使用方法][sbwacomqhowto]  

多層シナリオの詳細またはクラウド サービスにアプリケーションを展開する方法については、以下を参照してください。  

* [ストレージ テーブル、キュー、BLOB を使用する .NET 多層アプリケーションに関するページ][mutitierstorage]  

Azure のクラウド サービスではなく、Azure Web サイトに多層アプリケーションのフロントエンドを実装することもできます。 Web サイトとクラウド サービスの違いの詳細については、次を参照してください。 [Azure 実行モデル][executionmodels]します。

このチュートリアルで作成したアプリケーションをクラウド サービス Web ロールではなく標準的な Web プロジェクトとして実装するには、次の相違点に注意しつつ、このチュートリアルの手順に従ってください。

1. プロジェクトを作成する場合、 **ASP.NET MVC Web アプリケーション** プロジェクト テンプレート、 **Web** カテゴリの代わりに、 **クラウド サービス** 内のテンプレート、 **クラウド** カテゴリ。 表示されるまでは、MVC アプリケーションを作成するのと同じ手順に従います、 **クラウド構成マネージャー** セクションです。

2. worker ロールを作成する際には、Web ロールのときの手順と同様に、新しい個別のソリューションに作成します。 ただし、クラウド サービス プロジェクトに worker ロールのみを作成します。 そのうえで、worker ロールを作成するときと同じ手順に従います。

3. フロントエンドとバックエンドは個別にテストできるほか、個別の Visual Studio インスタンスで両方を同時にテストすることもできます。

Azure の web サイトにフロント エンドを展開する方法については、次を参照してください。 [Azure App Service で ASP.NET web アプリの作成](../app-service-web/web-sites-dotnet-get-started.md)します。 Azure クラウド サービスにバックエンドを展開する方法については、次を参照してください。 [.NET 多層アプリケーションを使用してストレージ テーブル、キュー、および Blob][mutitierstorage]します。


  [0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
  [1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
  [sbqueuecomparison]: service-bus-azure-and-service-bus-queues-compared-contrasted.md
  [2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
  [Get Tools and SDK]: http://go.microsoft.com/fwlink/?LinkId=271920
  [3]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-3.png


  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
  [NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

  [EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

  [Azure classic portal]: http://manage.windowsazure.com
  [6]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/sb-queues-03.png
  [7]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/sb-queues-04.png
  [8]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-09.png
  [9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
  [10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
  [11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
  [12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
  [13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
  [14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
  [15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
  [16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-35.png
  [17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
  [18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

  [19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
  [20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
  [23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
  [24]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBExplorerProperties.png
  [25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
  [26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
  [27]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-27.png
  [28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png
  [30]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/sb-queues-09.png
  [31]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/sb-queues-06.png
  [32]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-41.png
  [33]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-42-webpi.png
  [35]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/multi-web-45.png
  [sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-how-to-use-queues.md  
  [mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
  [executionmodels]: ../cloud-services/fundamentals-application-models.md


