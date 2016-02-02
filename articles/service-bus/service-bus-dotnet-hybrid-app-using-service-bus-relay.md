<properties
    pageTitle="ハイブリッド オンプレミス/クラウド アプリケーション (.NET) | Microsoft Azure"
    description="Azure Service Bus Relay を使用して .NET オンプレミス/クラウド ハイブリッド アプリケーションを作成する方法について説明します。"
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
    ms.topic="get-started-article"
    ms.date="10/07/2015"
    ms.author="sethm"/>


# Azure Service Bus Relay を使用する .NET オンプレミス/クラウド ハイブリッド アプリケーション

## はじめに

次を使用して Microsoft Azure でハイブリッド クラウドのアプリケーションを簡単に開発できます。
Visual Studio 2013 と無料の Azure SDK for .NET。 この記事
Azure を初めて使用するユーザーを対象としています。 30 分以内に
30 分を複数の Azure リソースを使用するアプリケーションと、クラウドで実行があります。

学習内容:

-   Web ソリューションで使用する Web サービスを作成するか、または
    既存のものを適合させる方法
-   Azure Service Bus リレーを使用して、Azure アプリケーションと、
    別の場所でホストされる Web サービスの間でデータを共有する方法

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## Service Bus Relay がハイブリッド ソリューションで役立つ理由

ビジネス ソリューションは、カスタム コードの組み合わせで構成されます。
これらのコードは、独自の新たなビジネス要件や、
デプロイ済みのソリューションおよびシステムで提供されている
既存の機能に対応するように記述されています。

ソリューション設計者は、スケーリング要件への対応を容易にして
運用コストを削減するために、クラウドを使い始めています。 設計者はこの過程で
ソリューションの構成要素として活用しようとしている既存のサービス資産が、
企業のファイアウォールの内側にあり、クラウド ソリューションから
簡単に到達してアクセスできないことに気付きました。 内部サービスの多くは、
企業のネットワーク エッジで簡単に公開できるような形で
作成またはホストされていません。

Service Bus relay は既存の使用事例
Windows Communication Foundation (WCF) web サービスを取得するユースケース、および
それらのサービスから企業ネットワークの外部にあるソリューションへ、
企業のネットワーク インフラストラクチャを大幅に変更することなく
安全にアクセスできるようにするユースケース向けに設計されています。 このような Service Bus Relay サービスは依然として
既存の内部環境でホストされていますが、
着信セッションおよびクラウドでホストされる Service Bus に要求します。 サービス バスもそれらのサービスを不正アクセスからを使用して保護 [Shared Access Signature](https://msdn.microsoft.com/library/dn170478.aspx) (SAS) 認証します。

## ソリューション シナリオ

このチュートリアルでは、商品在庫ページに商品の一覧を表示する ASP.NET MVC Web サイトを作成します。

![][0]

このチュートリアルは、製品情報が既存の内部設置型システムにあり、
Service Bus Relay を使用してそのシステムに到達することを
構成します。 これは、単純なで実行される web サービスでシミュレートします。
メモリ内の一連の製品で返されます。 クラスターの
コンソール アプリケーションを自分のコンピューターで実行できるようになり、
Web ロールを Azure にデプロイできるようになります。 この方法により、
Azure データ センターで実行される Web ロールが、
自分のコンピューターがほぼ確実に、1 つ以上のファイアウォールおよび
ネットワーク アドレス変換 (NAT) 層の内側にある場合でも、
そのコンピューターに呼び出されます。

次に、完成した Web アプリケーションの開始ページのスクリーンショットを示します。

![][1]

## 開発環境を設定する

Azure アプリケーションの開発を開始する前に、ツールを入手して、開発環境を設定します。

1.  Azure SDK for で .NET インストール [ツールと SDK の][]します。

2.  使用している Visual Studio のバージョンに対応する **[SDK のインストール]** をクリックします。 このチュートリアルの手順では、Visual Studio 2013 を使用します。

    ![][42]

4.  インストーラーの実行や保存を求めるメッセージが表示されたら、**[実行]** をクリックします。

    ![][2]

5.  **Web Platform Installer** の **[インストール]** をクリックし、インストールの手順を進めます。

    ![][3]

6.  インストールが完了すると、開発に必要なツールがすべて
    アプリケーションの開発を始める必要です。 SDK には、Visual Studio で Microsoft Azure アプリケーションを
    簡単に開発するためのツールが用意されています。 Visual Studio が
    インストールされていない場合、無料の
    Visual Studio Express をインストールすることもできます。

## サービス名前空間の作成

Azure で Service Bus 機能を使用するには、最初にサービス名前空間を作成する必要があります。 名前空間は、アプリケーション内で Service Bus リソースをアドレス指定するためのスコープ コンテナーを提供します。

名前空間と Service Bus メッセージング エンティティのいずれかを使用して管理することができます、 [Azure クラシック ポータルの [][] ポータル内から新しい名前空間が Visual Studio サーバー エクスプ ローラーで作成のみこともできます。

### Azure クラシック ポータルを使用して名前空間を作成する

1.  サインイン、 [Azure クラシック ポータルの [][]します。

2.  ポータルの左側のナビゲーション ウィンドウで次のようにクリックします。
    **Service Bus**します。

3.  ポータルの下のウィンドウで、**[作成]** をクリックします。

    ![][5]

4.  **[新しい名前空間を追加する]** ダイアログ ボックスで、名前空間の名前を入力します。
    その名前が使用できるかどうかがすぐに自動で確認されます。
    ![][6]

5.  入力した名前が利用できることを確認できたら、名前空間を
    ホストする国または地域を選択します (
    コンピューター リソースをデプロイするのと同じ国/地域を
    使用してください)。
    > [AZURE.IMPORTANT] 選択、 *同じリージョン* 用に選択します。
    選択してください。 そうすることで、パフォーマンスが最高になります。

6.  ダイアログ ボックスの他のフィールドは、既定値 (**[メッセージング]** と **[Standard]** レベル) のままにして、チェック マークをクリックします。 これで、システムによってサービス名前空間が作成され、有効になります。 システムがアカウントのリソースを準備し 終わるまでに、数分間かかる場合があります。

    ![][38]

作成した名前空間は Azure クラシック ポータルに表示されます。アクティブ化するまで少し時間がかかります。 ステータスが **[アクティブ]** になるのを待ってから、次に進みます。

## 名前空間の既定の管理資格情報の取得

メッセージング エンティティの作成など、新しい名前空間の管理操作を実行するには、その名前空間の資格情報を取得する必要があります。

1.  メイン ウィンドウで、使用するサービス名前空間の名前をクリックします。

    ![][39]

2.  **[接続情報]** をクリックします。

    ![][40]

3.  **[接続情報へのアクセス]** ウィンドウで、SAS キーとキー名を含む接続文字列を見つけます。

    ![][45]

4.  これらの資格情報を書き留めておくか、クリップボードにコピーします。

## オンプレミスのサーバーを作成する

まず、仮のオンプレミスの商品カタログ システムを構築します。 その方法は
非常に簡単です。このサービスは、統合しようとしている
完全なサービス表示による内部設置型商品カタログ システムとして
表示できます。

このプロジェクトは、Visual Studio コンソール アプリケーションとして開始します。 この
Service Bus の NuGet パッケージを使用して、Service Bus の
ライブラリおよび構成設定を追加します。 NuGet Visual Studio の拡張機能により、
ライブラリおよびツールを Visual Studio または Visual Studio Express に
簡単にインストールおよび更新できます。 Service Bus NuGet パッケージは、
Service Bus API の取得およびアプリケーションの構成を
すべて Service Bus の依存関係で最も簡単に行う方法です。 NuGet および Service Bus の使用については、
Service Bus パッケージを参照してください [NuGet Service Bus パッケージのを使用して][]します。

### プロジェクトを作成する

1.  管理者特権を使用して、Microsoft Visual を開始します。
    Studio 2013 または Microsoft Visual Studio Express を起動します。 To
    管理者特権で Visual Studio を起動を右クリックしてください
    **Microsoft Visual Studio 2013 (または Microsoft Visual Studio Express)** ] をクリックし、 **管理者として実行**します。

2.  Visual Studio での **ファイル** ] メニューのをクリックして **新規**, 、し、
    クリックして **プロジェクト**します。

    ![][10]

3.  **インストールされたテンプレート**, [ **Visual c#**, 、] をクリックして * * コンソール
    アプリケーション]** をクリックします。  **名** ボックスで、名前を入力します
    **ProductsServer**:

    ![][11]

4.  **[OK]** をクリックして **ProductsServer** プロジェクトを作成します。

5.  ソリューション エクスプ ローラーで右クリック **ProductsServer**, 、し、
    クリックして **プロパティ**します。

6.  クリックして、 **アプリケーション** 、左側のタブ、いることを確認し、* **.net
    Framework 4 * * または **.NET Framework 4.5** で表示される、 **ターゲット フレームワーク** ] ボックスの一覧です。 表示されていない場合は、リストから選択し、プロジェクトを読み込み直すかどうかを確認するメッセージが表示されたら **[はい]** をクリックします。

    ![][12]

7.  既に Visual Studio 用の NuGet パッケージ マネージャーをインストールしている場合は、次のステップに進みます。 それ以外の場合を参照してください [NuGet][] ] をクリック [Install NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)します。 メッセージに従って NuGet パッケージ マネージャーをインストールし、Visual Studio を再起動します。

7.  ソリューション エクスプ ローラーで右クリック **参照**, 、] をクリックし、
    **NuGet パッケージの管理**します。

8.  **[NuGet]** ダイアログ ボックスの左側の列で、**[オンライン]** をクリックします。

9.  右側の列をクリックして、 **検索** ボックスに、入力"**Service Bus**"を選択し、* * Microsoft
    Azure Service Bus]** の項目を選択します。 クリックして **インストール** を完了する、
    インストールし、ダイアログ ボックスを閉じる。

    ![][13]

    これで、必要なクライアント アセンブリを参照できるようになりました。

9.  商品のコントラクト用に新しいクラスを追加します。 ソリューション エクスプローラーで、
    右クリックし、 **ProductsServer** プロジェクトを **追加**, 、] をクリックし、
    **Class**.

    ![][14]

10. **[名前]** ボックスに「**ProductsContract.cs**」と入力します。 次に、
    クリックして **追加**します。

11. **ProductsContract.cs**, 、名前空間の定義を
次のコードをサービスのコントラクトを定義します。

    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }

12. Program.cs では、名前空間の定義を
コードは、プロファイル サービスとそのホストを追加します。

    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website.
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }

13. ソリューション エクスプ ローラーでダブルクリックして、 **App.config** ファイル
    Visual Studio エディターでそのファイルを開きます。 次の
    **< システムです。ServiceModel >** を次の XML コードです。 このとき、
    置換 *yourServiceNamespace* 、サービスの名前に置き換えます
    名前空間、および *yourKey* 以前に取得した SAS キーを持つ
    Azure クラシック ポータル: から

        <system.serviceModel>
          <extensions>
             <behaviorExtensions>
                <add name="transportClientEndpointBehavior" type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Version=2.6.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
              </behaviorExtensions>
              <bindingExtensions>
                 <add name="netTcpRelayBinding" type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Version=2.6.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
              </bindingExtensions>
          </extensions>
          <services>
             <service name="ProductsServer.ProductsService">
               <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts"
        behaviorConfiguration="products"/>
             </service>
          </services>
          <behaviors>
             <endpointBehaviors>
               <behavior name="products">
                 <transportClientEndpointBehavior>
                    <tokenProvider>
                       <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                    </tokenProvider>
                 </transportClientEndpointBehavior>
               </behavior>
             </endpointBehaviors>
          </behaviors>
        </system.serviceModel>

14. F6 キーを押すか、**[ビルド]** メニューの **[ソリューションのビルド]** をクリックしてアプリケーションをビルドし、ここまでの作業に問題がないことを確認します。

## ASP.NET MVC アプリケーションを作成する

このセクションでは、商品サービスから取得したデータを表示する単純な ASP.NET アプリケーションを構築します。

### プロジェクトを作成する

1.  Microsoft Visual Studio 2013 が管理者特権で実行されていることを確認します。 そうでなければ、
    管理者特権で Visual Studio を起動を右クリックしてください
    **Microsoft Visual Studio 2013 (または Microsoft Visual Studio Express)** ] をクリックし、 **管理者として実行**します。 この記事の後半で説明した Microsoft Azure コンピューティング エミュレーターは、
    管理者特権で visual Studio は起動します。

2.  Visual Studio での **ファイル** ] メニューのをクリックして **新規**, 、し、
    クリックして **プロジェクト**します。

3.  **[インストールされたテンプレート]** で **[Visual C#]** をクリックし、**[ASP.NET Web アプリケーション]** をクリックします。 プロジェクト名として、「**ProductsPortal**」と入力します。 次に、
    クリックして **OK**します。

    ![][15]

4.  **テンプレートを選択して** 一覧で、クリックして **MVC**,、
    クリックして **OK**します。

    ![][16]

5.  ソリューション エクスプ ローラーで右クリックして **モデル** ] をクリックし、 **追加**,、
    クリックし、 **クラス**します。  **名** ボックスで、名前を入力します
    **Product.cs**します。 **[追加]** をクリックします。

    ![][17]

### Web アプリケーションを変更する

1.  Visual Studio の Product.cs ファイルで、既存の名前空間の定義を
    名前空間の定義を次のコードです。

        // Declare properties for the products inventory.
        namespace ProductsWeb.Models
        {
            public class Product
            {
                public string Id { get; set; }
                public string Name { get; set; }
                public string Quantity { get; set; }
            }
        }

2.  Visual Studio の HomeController.cs ファイルで、既存の名前空間の定義を
名前空間の定義を次のコードです。

    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
    
        }
    }

3.  ソリューション エクスプローラーで、Views\Shared フォルダーを展開します。

    ![][18]

4.  ダブルクリックして **_Layout.cshtml** を Visual Studio エディターで開きます。

5.  "**My ASP.NET Application**" となっている箇所をすべて "**LITWARE's Products**" に置き換えます。

6. **Home**、**About**、および **Contact** の各リンクを削除します。 次の例では、強調表示されたコードを削除します。

    ![][41]

7.  ソリューション エクスプローラーで Views\Home フォルダーを展開します。

    ![][20]

8.  **Index.cshtml** をダブルクリックして、Visual Studio エディターで開きます。
ファイルの内容全体を次のコードに置き換えます。

    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
        ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
        <tr>
            <th>
                @Html.DisplayNameFor(model => model.Name)
            </th>
            <th></th>
            <th>
                @Html.DisplayNameFor(model => model.Quantity)
            </th>
        </tr>
    
    @foreach (var item in Model) {
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.Name)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Quantity)
            </td>
        </tr>
    }
    
    </table>

9.  作業の精度を検証するこれまでキーを押して **F6** または
    **Ctrl + Shift + B** プロジェクトをビルドします。


### ローカルでのアプリケーションの実行

アプリケーションを実行して、動作を確認します。

1.  **ProductsPortal** がアクティブなプロジェクトであることを確認します。 次の
    ソリューション エクスプ ローラーでプロジェクト名を右クリックし、
    スタートアップ プロジェクト] を選択します。
2.  **Visual Studio** で、F5 キーを押します。
3.  アプリケーションがブラウザーに表示され、実行されます。

    ![][21]

## アプリケーションを AZURE にデプロイする準備を完了する

アプリケーションは、Azure クラウド サービスまたは Azure の Web サイトにデプロイできます。 Web サイトとクラウド サービスの違いの詳細については、次を参照してください。 [Azure 実行モデルに関する][executionmodels]します。 Azure の web サイトにアプリケーションを展開する方法については、次を参照してください。 [Azure の web サイトに ASP.NET Web アプリケーションを配置する](http://azure.microsoft.com/develop/net/tutorials/get-started/)します。 このセクションには、アプリケーションを Azure クラウド サービスにデプロイする詳細な手順が記載されています。

アプリケーションをクラウド サービスにデプロイするには、クラウド サービス プロジェクトのデプロイメント プロジェクトをソリューションに追加します。 このデプロイメント プロジェクトには、アプリケーションをクラウドで適切に実行するために必要な構成情報が含まれています。

1.  アプリケーションをクラウドにデプロイできるようにするには、ソリューション エクスプローラーで **ProductsPortal** プロジェクトを右クリックし、**[変換]** をクリックしてから **[Azure クラウド サービス プロジェクトへの変換]** をクリックします。

    ![][22]

2.  アプリケーションをテストするには、F5 キーを押します。

3.  これにより、Azure コンピューティング エミュレーターが起動します。 コンピューティング エミュレーターでは、Azure でのアプリケーションの実行をローカル コンピューターでエミュレートします。 エミュレーターが起動されたことは、システム トレイの表示によって確認できます。

       ![][23]

4.  この場合も、ブラウザーにはローカルで実行中のアプリケーションが表示されます。これは、先ほど通常の ASP.NET MVC 4 アプリケーションとして実行したときと同じように表示され、動作します。

## 統合する

次の手順では、オンプレミスの商品サーバーと ASP.NET MVC アプリケーションを連結します。

1.  「ASP.NET MVC アプリケーションの作成」セクションで作成した **ProductsPortal** プロジェクトを閉じている場合は、Visual Studio でもう一度開きます。

2.  「オンプレミス サーバーの作成」セクションに記載されている手順に従い、NuGet パッケージを Reference プロジェクトに追加します。 ソリューション エクスプローラーで **[参照]** を右クリックし、 **[NuGet パッケージの管理]** をクリックします。

3.  "Service Bus" を検索して、**[Microsoft Azure Service Bus]** 項目を選択します。 次に、インストールを完了し、このダイアログ ボックスを閉じます。

4.  ソリューション エクスプローラーで **ProductsPortal** プロジェクトを右クリックし、**[追加]** をクリックしてから **[既存の項目]** をクリックします。

5.  **ProductsServer** コンソール プロジェクトの **ProductsContract.cs** ファイルに移動します。 ProductsContract.cs を強調表示する] をクリックします。 **[追加]** の横の下向き矢印をクリックしてから、**[リンクとして追加]** をクリックします。

    ![][24]

6.  開き、 **HomeController.cs** Visual Studio エディターでファイルし、名前空間の定義を次のコードに置き換えます。 *yourServiceNamespace* は実際のサービス名前空間の名前、*yourKey* は SAS キーに置き換えてください。 これで、クライアントからオンプレミスのサービスを呼び出し、その結果を返すことができます。

        namespace ProductsWeb.Controllers
        {
            using System.Linq;
            using System.ServiceModel;
            using System.Web.Mvc;
            using Microsoft.ServiceBus;
            using Models;
            using ProductsServer;
    
            public class HomeController : Controller
            {
                // Declare the channel factory.
                static ChannelFactory<IProductsChannel> channelFactory;
    
                static HomeController()
                {
                    // Create shared secret token credentials for authentication.
                    channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                        "sb://yourServiceNamespace.servicebus.windows.net/products");
                    channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                        TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                            "RootManageSharedAccessKey", "yourKey") });
                }
    
                public ActionResult Index()
                {
                    using (IProductsChannel channel = channelFactory.CreateChannel())
                    {
                        // Return a view of the products inventory.
                        return this.View(from prod in channel.GetProducts()
                                         select
                                             new Product { Id = prod.Id, Name = prod.Name,
                                                 Quantity = prod.Quantity });
                    }
                }
            }
        }

7.  ソリューション エクスプローラーで **ProductsPortal** ソリューションを右クリックし、**[追加]**、**[既存のプロジェクト]** の順にクリックします。

8.  **ProductsServer** プロジェクトに移動し、**ProductsServer.csproj** ソリューション ファイルをダブルクリックして追加します。

9.  ソリューション エクスプローラーで **ProductsPortal** ソリューションを右クリックして、**[プロパティ]** をクリックします。

10. 左側で、**[スタートアップ プロジェクト]** をクリックします。 左側で、**[マルチ スタートアップ プロジェクト]** をクリックします。 **ProductsServer**、**ProductsPortal.Azure**、**ProductsPortal** がこの順序で表示され、**ProductsServer** と **ProductsPortal.Azure** の動作に **[開始]** が設定され、**ProductsPortal** の動作に **[なし]** が設定されているようにしてください。

      ![][25]

11. 引き続き **[プロパティ]** ダイアログ ボックスで、左側の **[ProjectDependencies]** をクリックします。

12. **[プロジェクト]** リストで **[ProductsServer]** をクリックします。 **[ProductsPortal]** がオフ、**[ProductsPortal.Azure]** がオンになっていることを確認します。 次に、**[OK]** をクリックします。

    ![][26]

## アプリケーションの実行

1.  Visual Studio の **[ファイル]** メニューで **[すべて保存]** をクリックします。

2.  F5 キーを押して、アプリケーションをビルドして実行します。 次のスクリーンショットのように、まず、オンプレミスのサーバー (**ProductsServer** コンソール アプリケーション) を開始し、次に **ProductsWeb** アプリケーションをブラウザー ウィンドウで開始する必要があります。 この時点で、オンプレミスのシステムの商品サービスから取得された商品在庫一覧データが表示されます。

    ![][1]

## Azure へのアプリケーションのデプロイ

1.  ソリューション エクスプローラーで **ProductsPortal** プロジェクトを右クリックし、**[Microsoft Azure への発行]** をクリックします。

2.  すべてのサブスクリプションを表示するために、サインインする必要が生じることがあります。

    **[他のサブスクリプションを表示するためにサインイン]** をクリックします。

    ![][27]

3.  Microsoft アカウントを使用してサインインします。

8.  **[次へ]** をクリックします。 ホストされるサービスがサブスクリプションにまだ含まれていない場合は、ホステッド サービスの作成を求めるメッセージが表示されます。 ホストされるサービスには、Azure サブスクリプションにおけるアプリケーションのコンテナーとして機能します。 アプリケーションを識別する名前を入力し、アプリケーションが最適化される対象リージョンを選択します (このリージョンからアプリケーションにアクセスするユーザーは、読み込み時間が短くなることが期待できます)。

9.  アプリケーションの発行先のホストされるサービスを選択します。 その他の設定については、次の図に示す既定のままにします。 **[次へ]** をクリックします。

    ![][33]

10. 最後のページで、**[発行]** をクリックしてデプロイメント プロセスを開始します。

    ![][34]
これには約 5 ～ 7 分かかります。 これは最初の発行であるため、Azure が Virtual Machines (VM) を準備して、セキュリティを強化し、アプリケーションをホストするためのWeb ロールを VM 上に作成して、その Web ロールにコードをデプロイします。最後に、Load Balancer とネットワークを構成して、アプリケーションを発行します。

11. 発行の進行中に、**[Azure のアクティビティ ログ]** ウィンドウでアクティビティを監視できます。通常、このウィンドウは Visual Studio または Visual Web Developer の下部にドッキングされています。

    ![][35]

12. デプロイメントが完了したら、監視ウィンドウで **[Web サイトの URL]** リンクをクリックして、Web サイトを表示できます。

    ![][36]

    Web サイトはオンプレミスのサーバーに依存するため、この Web サイトを正しく機能させるには、**ProductsServer** アプリケーションをローカルに実行する必要があります。 クラウドの Web サイトで要求を実行すると、次のスクリーンショットにある "GetProducts" の出力が示すように、オンプレミスのコンソール アプリケーションで要求が呼び出されることを確認できます。

    ![][37]

Web サイトとクラウド サービスの違いの詳細については、次を参照してください。 [Azure 実行モデルに関する][executionmodels]します。

## アプリケーションの停止と削除

アプリケーションをデプロイした後、そのアプリケーションを無効にして
750 時間/月 (31 日/月) のサーバー無料期間内に
他のアプリケーションを作成およびデプロイできるようにしたい場合もあります。

Azure では、消費されたサーバー時間の 1 時間単位の 料金が Web ロール インスタンスに
課金されます。 サーバー時間は、アプリケーションをデプロイした時点で消費されます。
インスタンスが実行されておらず停止の状態であっても、サーバー時間は消費されます。
無料アカウントには、これらの web ロール インスタンスをホストするための
専用 VM サーバー時間 (750 時間/月 (31 日/月)) が含まれています。

次のステップでは、アプリケーションの停止と削除の方法を
想定しています。

1.  サインイン、 [Azure クラシック ポータル][], をクリックして **クラウド サービス**, 、サービスの名前をクリックします。

2.  **[ダッシュボード]** タブをクリックし、**[停止]** をクリックすると、アプリケーションが一時的に停止されます。 **[開始]** をクリックすることにより、再度開始できます。 **[削除]** をクリックすると、アプリケーションが Azure から完全に削除されます。復元することはできません。

    ![][43]

## 次のステップ

Service Bus の詳細については、次のリソースを参照してください。

* [Azure Service Bus ][sbwacom]
* [[Sbwacomqhowto] のサービス バス キューの使用方法][sbwacomqhowto]



[0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png 
[1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png 
[get tools and sdk]: http://go.microsoft.com/fwlink/?LinkId=271920 
[nuget]: http://nuget.org 
[2]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-3.png 
[3]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-42-webpi.png 
[azure classic portal]: http://manage.windowsazure.com 
[5]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/sb-queues-03.png 
[6]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/sb-queues-04.png 
[using the nuget service bus package]: https://msdn.microsoft.com/library/azure/dn741354.aspx 
[10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-1.png 
[11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png 
[12]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-3.png 
[13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png 
[14]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-4.png 
[15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png 
[16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png 
[17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.jpg 
[18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-10.jpg 
[20]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-11.png 
[21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png 
[22]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-21.png 
[23]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-22.png 
[24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png 
[25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png 
[26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png 
[27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-33.png 
[30]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-36.png 
[31]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-37.png 
[32]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-38.png 
[33]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-39.png 
[34]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-40.png 
[35]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-41.png 
[36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png 
[37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png 
[38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-27.png 
[39]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/sb-queues-09.png 
[40]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/sb-queues-06.png 
[41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png 
[42]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-41.png 
[43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png 
[45]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-45.png 
[sbwacom]: /documentation/services/service-bus/ 
[sbwacomqhowto]: service-bus-dotnet-how-to-use-queues.md 
[executionmodels]: ../cloud-services/fundamentals-application-models.md 

