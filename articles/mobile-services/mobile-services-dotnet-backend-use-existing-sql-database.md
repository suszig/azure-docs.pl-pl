<properties
    pageTitle="Mobile Services .NET バックエンドによる既存の SQL データベースを使用するサービスの作成 | Microsoft Azure"
    description="既存のクラウドまたはオンプレミス SQL データベースと .NET ベースのモバイル サービスを使用する方法について説明します。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor="mollybos"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="11/09/2015"
    ms.author="glenga"/>


# Mobile Services .NET バックエンドによる既存の SQL データベースを使用するサービスの作成

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


Mobile Services .NET バックエンドを使用すると、モバイル サービスの作成で簡単に既存の資産を利用できます。 特に興味深いシナリオの 1 つに、他のアプリケーションで既に使用した可能性がある (オンプレミスまたはクラウドに配置された) 既存の SQL データベースを使用して、既存のデータをモバイル クライアントが利用できるようにする方法があります。 データベース モデルの要件をここでは、(または *スキーマ*)、既存のソリューションの操作を続行するためには変更されません。

<a name="ExistingModel"></a>
## 既存のデータベース モデルを調査する

このチュートリアルでは、モバイル サービスによって作成されたデータベースを使用しますが、作成される既定のモデルは使用しません。 代わりに、手元にある既存のアプリケーションを表す任意のモデルを手動で作成します。 代わりに、内部設置型データベースに接続する方法の詳細についてをチェック アウト [ハイブリッド接続を使用して Azure のモバイル サービスから内部設置型 SQL Server への接続](mobile-services-dotnet-backend-hybrid-connections-get-started.md)します。

1. モバイル サービスのサーバー プロジェクトを作成することで開始 **Visual Studio 2013 Update 2** または [モバイル サービス] タブで、サービスのダウンロード可能なクイック スタート プロジェクトを使用して、 [Azure クラシック ポータル](http://manage.windowsazure.com)します。 このチュートリアルの目的はものとサーバー プロジェクト名が **ShoppingService**します。

2. 作成、 **Customer.cs** ファイル、 **モデル** フォルダーと、次の実装を使用します。 アセンブリへの参照を追加する必要があります。 **System.ComponentModel.DataAnnotations** をプロジェクトにします。

        using System.Collections.Generic;
        using System.ComponentModel.DataAnnotations;

        namespace ShoppingService.Models
        {
            public class Customer
            {
                [Key]
                public int CustomerId { get; set; }

                public string Name { get; set; }

                public virtual ICollection<Order> Orders { get; set; }

            }
        }

3. 作成、 **Order.cs** ファイル、 **モデル** フォルダーと、次の実装を使用します。

        using System.ComponentModel.DataAnnotations;

        namespace ShoppingService.Models
        {
            public class Order
            {
                [Key]
                public int OrderId { get; set; }

                public string Item { get; set; }

                public int Quantity { get; set; }

                public bool Completed { get; set; }

                public int CustomerId { get; set; }

                public virtual Customer Customer { get; set; }

            }
        }

    これら 2 つのクラスがあることに注意、 *リレーションシップ*: すべて **順序** 、1 つに関連付けられた **顧客** と **顧客** 関連付けることができます複数 **注文**します。 既存のデータ モデルでリレーションシップがあることは一般的です。

4. 作成、 **ExistingContext.cs** ファイル、 **モデル** フォルダーように実装するとします。

        using System.Data.Entity;

        namespace ShoppingService.Models
        {
            public class ExistingContext : DbContext
            {
                public ExistingContext()
                    : base("Name=MS_TableConnectionString")
                {
                }

                public DbSet<Customer> Customers { get; set; }

                public DbSet<Order> Orders { get; set; }

            }
        }

上の構造は、既存のアプリケーションに以前から使用している可能性がある既存の Entity Framework モデルに似ています。 この段階では、モデルは Mobile Services をどのような方法によっても認識していないことに注意してください。

<a name="DTOs"></a>
## モバイル サービス用にデータ転送オブジェクト (DTO) を作成する

モバイル サービスで使用するデータ モデルの複雑さは任意に決めることができます。よって、互いにさまざまなリレーションシップを持つ数百個のエンティティを含めることもできます。 モバイル アプリケーションを作成する場合、通常求められるのは、データ モデルの簡素化とリレーションシップの排除 (または手動でのリレーションシップの処理) です。これは、アプリケーションとサービスの間を移動するペイロードを最小限に抑えることを目的としています。 このセクションでは、一連の簡素化されたオブジェクト ("データ転送オブジェクト" または "DTO" とも呼ばれます) を作成し、データベースに保存しているデータにマッピングします。ただし、これらのオブジェクトに含める一連のプロパティは、モバイル アプリケーションで最小限必要なものに留めます。

1. 作成、 **MobileCustomer.cs** ファイルで、 **DataObjects** 、次の実装を使用して、サービス プロジェクトのフォルダー。

        using Microsoft.WindowsAzure.Mobile.Service;

        namespace ShoppingService.DataObjects
        {
            public class MobileCustomer : EntityData
            {
                public string Name { get; set; }
            }
        }

    このクラスはのようなことに注意してください、 **顧客** リレーションシップ プロパティを除き、モデル内のクラス **順序** を削除します。 Mobile Services オフライン同期では正しく動作するオブジェクトは、一連の必要な *システム プロパティ* オプティミスティック同時実行のためが表示されますが、この DTO を継承する [**EntityData**](http://msdn.microsoft.com/library/microsoft.windowsazure.mobile.service.entitydata.aspx), 、これらのプロパティが含まれています。 Int ベース **CustomerId** 元のモデルのプロパティが文字列ベースで置き換えられる **Id** プロパティから **EntityData**, 、となります、 **Id** モバイル サービスで使用します。

2. 作成、 **MobileOrder.cs** ファイルで、 **DataObjects** サービス プロジェクトのフォルダーです。

        using Microsoft.WindowsAzure.Mobile.Service;
        using Newtonsoft.Json;
        using System.ComponentModel.DataAnnotations;
        using System.ComponentModel.DataAnnotations.Schema;

        namespace ShoppingService.DataObjects
        {
            public class MobileOrder : EntityData
            {
                public string Item { get; set; }

                public int Quantity { get; set; }

                public bool Completed { get; set; }

                [NotMapped]
                public int CustomerId { get; set; }

                [Required]
                public string MobileCustomerId { get; set; }

                public string MobileCustomerName { get; set; }
            }
        }

     **顧客** リレーションシップ プロパティに置き換えられました、 **顧客** 名、および **MobileCustomerId** プロパティが、クライアントでリレーションシップを手動でモデルを使用できます。 ここでは無視できますが、 **CustomerId** プロパティには、そののみが後で使用します。

3. 確認システム プロパティの追加とに、 **EntityData** 基本クラス、Dto にモデルの型よりも多くのプロパティがあります。 これらのプロパティを保存する場所が必要なのは明らかなため、元のデータベースにいくつかの列を追加します。 これによりデータベースが変更されますが、変更は単なる追加 (スキーマへの新しい列の追加) であるため既存のアプリケーションを損なうことはありません。 先頭に次のステートメントを追加するには、 **Customer.cs** と **Order.cs**:

        using System.ComponentModel.DataAnnotations.Schema;
        using Microsoft.WindowsAzure.Mobile.Service.Tables;
        using System;

4. その後、次のようにこれらの追加のプロパティを、それぞれのクラスに追加します。

        [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
        [Index]
        [TableColumn(TableColumnType.CreatedAt)]
        public DateTimeOffset? CreatedAt { get; set; }

        [TableColumn(TableColumnType.Deleted)]
        public bool Deleted { get; set; }

        [Index]
        [TableColumn(TableColumnType.Id)]
        [MaxLength(36)]
        public string Id { get; set; }

        [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
        [TableColumn(TableColumnType.UpdatedAt)]
        public DateTimeOffset? UpdatedAt { get; set; }

        [TableColumn(TableColumnType.Version)]
        [Timestamp]
        public byte[] Version { get; set; }

4. 先ほど追加したシステム プロパティには、データベースの操作で透過的に発生する組み込みの動作 (作成日時/更新日時の自動更新など) がいくつか備わっています。 これらの動作を有効にする必要がありますに変更を行う **ExistingContext.cs**します。 ファイルの先頭に、次を追加します。

        using System.Data.Entity.ModelConfiguration.Conventions;
        using Microsoft.WindowsAzure.Mobile.Service.Tables;
        using System.Linq;

5. 本体で **ExistingContext**, 、オーバーライド [**OnModelCreating**](http://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx):

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceTableColumn", (property, attributes) => attributes.Single().ColumnType.ToString()));

            base.OnModelCreating(modelBuilder);
        }

5. ここで、いくつかのサンプル データをデータベースに入力します。 ファイルを開く **WebApiConfig.cs**します。 新規作成 [**IDatabaseInitializer**](http://msdn.microsoft.com/library/gg696323.aspx) 内で構成し、 **登録** メソッドを次のようにします。

        using Microsoft.WindowsAzure.Mobile.Service;
        using ShoppingService.Models;
        using System;
        using System.Collections.Generic;
        using System.Collections.ObjectModel;
        using System.Data.Entity;
        using System.Web.Http;

        namespace ShoppingService
        {
            public static class WebApiConfig
            {
                public static void Register()
                {
                    ConfigOptions options = new ConfigOptions();

                    HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

                    Database.SetInitializer(new ExistingInitializer());
                }
            }

            public class ExistingInitializer : ClearDatabaseSchemaIfModelChanges<ExistingContext>
            {
                protected override void Seed(ExistingContext context)
                {
                    List<Order> orders = new List<Order>
                    {
                        new Order { OrderId = 10, Item = "Guitars", Quantity = 2, Id = Guid.NewGuid().ToString()},
                        new Order { OrderId = 20, Item = "Drums", Quantity = 10, Id = Guid.NewGuid().ToString()},
                        new Order { OrderId = 30, Item = "Tambourines", Quantity = 20, Id = Guid.NewGuid().ToString() }
                    };

                    List<Customer> customers = new List<Customer>
                    {
                        new Customer { CustomerId = 1, Name = "John", Orders = new Collection<Order> {
                            orders[0]}, Id = Guid.NewGuid().ToString()},
                        new Customer { CustomerId = 2, Name = "Paul", Orders = new Collection<Order> {
                            orders[1]}, Id = Guid.NewGuid().ToString()},
                        new Customer { CustomerId = 3, Name = "Ringo", Orders = new Collection<Order> {
                            orders[2]}, Id = Guid.NewGuid().ToString()},
                    };

                    foreach (Customer c in customers)
                    {
                        context.Customers.Add(c);
                    }

                    base.Seed(context);
                }
            }
        }

<a name="Mapping"></a>
## DTO とモデルとの間にマッピングを確立する

モデルの型があるようになりました **顧客** と **順序** と Dto **MobileCustomer** と **MobileOrder**, 、2 つの間の自動的な変換をバックエンドに指示する必要しますが、あります。 ここで Mobile Services は使用 [**AutoMapper**](http://automapper.org/), 、プロジェクトで既に参照されている、オブジェクト リレーショナル マッパーです。

1. 先頭に次のコードを追加 **WebApiConfig.cs**:

        using AutoMapper;
        using ShoppingService.DataObjects;

2. マッピングを定義するには、次の追加、 **登録** のメソッド、 **WebApiConfig** クラスです。

        Mapper.Initialize(cfg =>
        {
            cfg.CreateMap<MobileOrder, Order>();
            cfg.CreateMap<MobileCustomer, Customer>();
            cfg.CreateMap<Order, MobileOrder>()
                .ForMember(dst => dst.MobileCustomerId, map => map.MapFrom(x => x.Customer.Id))
                .ForMember(dst => dst.MobileCustomerName, map => map.MapFrom(x => x.Customer.Name));
            cfg.CreateMap<Customer, MobileCustomer>();

        });

これで、AutoMapper はオブジェクトを互いにマッピングします。 対応する名前を持つすべてのプロパティを照合する、たとえば **MobileOrder.CustomerId** に自動的にマップされます **Order.CustomerId**します。 表に、上記のように、カスタム マッピングを構成することができます、 **MobileCustomerName** プロパティを **名前** のプロパティ、 **顧客** リレーションシップ プロパティです。

<a name="DomainManager"></a>
## ドメイン専用のロジックを実装する

次の手順を実装するのには、 [**MappedEntityDomainManager**](http://msdn.microsoft.com/library/dn643300.aspx), 、これは、マッピングしたデータ ストアとクライアントからの HTTP トラフィックを処理するコント ローラーの間で抽象化レイヤーとして機能します。 Dto という観点でのみ、次のセクションで、コント ローラーを記述できること、および **MappedEntityDomainManager** 追加ここでは、専用のロジックを実装する場所も提供元のデータ ストアとの通信を処理します。

1. 追加、 **MobileCustomerDomainManager.cs** に、 **モデル** プロジェクトのフォルダーです。 次の実装を貼り付けます。

        using AutoMapper;
        using Microsoft.WindowsAzure.Mobile.Service;
        using ShoppingService.DataObjects;
        using System.Data.Entity;
        using System.Linq;
        using System.Net.Http;
        using System.Threading.Tasks;
        using System.Web.Http;
        using System.Web.Http.OData;

        namespace ShoppingService.Models
        {
            public class MobileCustomerDomainManager : MappedEntityDomainManager<MobileCustomer, Customer>
            {
                private ExistingContext context;

                public MobileCustomerDomainManager(ExistingContext context, HttpRequestMessage request, ApiServices services)
                    : base(context, request, services)
                {
                    Request = request;
                    this.context = context;
                }

                public static int GetKey(string mobileCustomerId, DbSet<Customer> customers, HttpRequestMessage request)
                {
                    int customerId = customers
                       .Where(c => c.Id == mobileCustomerId)
                       .Select(c => c.CustomerId)
                       .FirstOrDefault();

                    if (customerId == 0)
                    {
                        throw new HttpResponseException(request.CreateNotFoundResponse());
                    }
                    return customerId;
                }

                protected override T GetKey<T>(string mobileCustomerId)
                {
                    return (T)(object)GetKey(mobileCustomerId, this.context.Customers, this.Request);
                }

                public override SingleResult<MobileCustomer> Lookup(string mobileCustomerId)
                {
                    int customerId = GetKey<int>(mobileCustomerId);
                    return LookupEntity(c => c.CustomerId == customerId);
                }

                public override async Task<MobileCustomer> InsertAsync(MobileCustomer mobileCustomer)
                {
                    return await base.InsertAsync(mobileCustomer);
                }

                public override async Task<MobileCustomer> UpdateAsync(string mobileCustomerId, Delta<MobileCustomer> patch)
                {
                    int customerId = GetKey<int>(mobileCustomerId);

                    Customer existingCustomer = await this.Context.Set<Customer>().FindAsync(customerId);
                    if (existingCustomer == null)
                    {
                        throw new HttpResponseException(this.Request.CreateNotFoundResponse());
                    }

                    MobileCustomer existingCustomerDTO = Mapper.Map<Customer, MobileCustomer>(existingCustomer);
                    patch.Patch(existingCustomerDTO);
                    Mapper.Map<MobileCustomer, Customer>(existingCustomerDTO, existingCustomer);

                    await this.SubmitChangesAsync();

                    MobileCustomer updatedCustomerDTO = Mapper.Map<Customer, MobileCustomer>(existingCustomer);

                    return updatedCustomerDTO;
                }

                public override async Task<MobileCustomer> ReplaceAsync(string mobileCustomerId, MobileCustomer mobileCustomer)
                {
                    return await base.ReplaceAsync(mobileCustomerId, mobileCustomer);
                }

                public override async Task<bool> DeleteAsync(string mobileCustomerId)
                {
                    int customerId = GetKey<int>(mobileCustomerId);
                    return await DeleteItemAsync(customerId);
                }
            }
        }

    このクラスの重要な部分は、 **GetKey** メソッドは元のデータ モデル内で、オブジェクトの ID プロパティを検索する方法を指示します。

2. 追加、 **MobileOrderDomainManager.cs** に、 **モデル** プロジェクトのフォルダー。

        using AutoMapper;
        using Microsoft.WindowsAzure.Mobile.Service;
        using ShoppingService.DataObjects;
        using System.Linq;
        using System.Net.Http;
        using System.Threading.Tasks;
        using System.Web.Http;
        using System.Web.Http.OData;

        namespace ShoppingService.Models
        {
            public class MobileOrderDomainManager : MappedEntityDomainManager<MobileOrder, Order>
            {
                private ExistingContext context;

                public MobileOrderDomainManager(ExistingContext context, HttpRequestMessage request, ApiServices services)
                    : base(context, request, services)
                {
                    Request = request;
                    this.context = context;
                }

                protected override T GetKey<T>(string mobileOrderId)
                {
                    int orderId = this.context.Orders
                        .Where(o => o.Id == mobileOrderId)
                        .Select(o => o.OrderId)
                        .FirstOrDefault();

                    if (orderId == 0)
                    {
                        throw new HttpResponseException(this.Request.CreateNotFoundResponse());
                    }
                    return (T)(object)orderId;
                }

                public override SingleResult<MobileOrder> Lookup(string mobileOrderId)
                {
                    int orderId = GetKey<int>(mobileOrderId);
                    return LookupEntity(o => o.OrderId == orderId);
                }

                private async Task<Customer> VerifyMobileCustomer(string mobileCustomerId, string mobileCustomerName)
                {
                    int customerId = MobileCustomerDomainManager.GetKey(mobileCustomerId, this.context.Customers, this.Request);
                    Customer customer = await this.context.Customers.FindAsync(customerId);
                    if (customer == null)
                    {
                        throw new HttpResponseException(Request.CreateBadRequestResponse("Customer with name '{0}' was not found", mobileCustomerName));
                    }
                    return customer;
                }

                public override async Task<MobileOrder> InsertAsync(MobileOrder mobileOrder)
                {
                    Customer customer = await VerifyMobileCustomer(mobileOrder.MobileCustomerId, mobileOrder.MobileCustomerName);
                    mobileOrder.CustomerId = customer.CustomerId;
                    return await base.InsertAsync(mobileOrder);
                }

                public override async Task<MobileOrder> UpdateAsync(string mobileOrderId, Delta<MobileOrder> patch)
                {
                    Customer customer = await VerifyMobileCustomer(patch.GetEntity().MobileCustomerId, patch.GetEntity().MobileCustomerName);

                    int orderId = GetKey<int>(mobileOrderId);

                    Order existingOrder = await this.Context.Set<Order>().FindAsync(orderId);
                    if (existingOrder == null)
                    {
                        throw new HttpResponseException(this.Request.CreateNotFoundResponse());
                    }

                    MobileOrder existingOrderDTO = Mapper.Map<Order, MobileOrder>(existingOrder);
                    patch.Patch(existingOrderDTO);
                    Mapper.Map<MobileOrder, Order>(existingOrderDTO, existingOrder);

                    // This is required to map the right Id for the customer
                    existingOrder.CustomerId = customer.CustomerId;

                    await this.SubmitChangesAsync();

                    MobileOrder updatedOrderDTO = Mapper.Map<Order, MobileOrder>(existingOrder);

                    return updatedOrderDTO;
                }

                public override async Task<MobileOrder> ReplaceAsync(string mobileOrderId, MobileOrder mobileOrder)
                {
                    await VerifyMobileCustomer(mobileOrder.MobileCustomerId, mobileOrder.MobileCustomerName);

                    return await base.ReplaceAsync(mobileOrderId, mobileOrder);
                }

                public override Task<bool> DeleteAsync(string mobileOrderId)
                {
                    int orderId = GetKey<int>(mobileOrderId);
                    return DeleteItemAsync(orderId);
                }
            }
        }

    この場合、 **InsertAsync** と **UpdateAsync** メソッドでリレーションシップを適用して各 **順序** に関連付けられた有効である必要があります **顧客**します。  **InsertAsync** 入力するかがわかります、 **MobileOrder.CustomerId** にマップするプロパティ、 **Order.CustomerId** プロパティです。 検索することでその値を取得、 **顧客** が、一致する **MobileOrder.MobileCustomerId**します。 これは、既定では、クライアントは Mobile Services の ID を認識専用のため (**MobileOrder.MobileCustomerId**) の **顧客**, 、外部キーを設定するために必要な実際のプライマリ キーとは異なります (**MobileOrder.CustomerId**) から **順序** に **顧客**します。 これは、挿入操作を容易にするために、このサービスの内部でのみ使用されます。

これで、DTO をクライアントに公開するコントローラーを作成する準備ができました。

<a name="Controller"></a>
## DTO を使用して TableController を実装する

1.  **コント ローラー** フォルダー、ファイルを追加 **MobileCustomerController.cs**:

        using Microsoft.WindowsAzure.Mobile.Service;
        using Microsoft.WindowsAzure.Mobile.Service.Security;
        using ShoppingService.DataObjects;
        using ShoppingService.Models;
        using System.Linq;
        using System.Threading.Tasks;
        using System.Web.Http;
        using System.Web.Http.Controllers;
        using System.Web.Http.OData;

        namespace ShoppingService.Controllers
        {
            public class MobileCustomerController : TableController<MobileCustomer>
            {
                protected override void Initialize(HttpControllerContext controllerContext)
                {
                    base.Initialize(controllerContext);
                    var context = new ExistingContext();
                    DomainManager = new MobileCustomerDomainManager(context, Request, Services);
                }

                public IQueryable<MobileCustomer> GetAllMobileCustomers()
                {
                    return Query();
                }

                public SingleResult<MobileCustomer> GetMobileCustomer(string id)
                {
                    return Lookup(id);
                }

                [AuthorizeLevel(AuthorizationLevel.Admin)]
                protected override Task<MobileCustomer> PatchAsync(string id, Delta<MobileCustomer> patch)
                {
                    return base.UpdateAsync(id, patch);
                }

                [AuthorizeLevel(AuthorizationLevel.Admin)]
                protected override Task<MobileCustomer> PostAsync(MobileCustomer item)
                {
                    return base.InsertAsync(item);
                }

                [AuthorizeLevel(AuthorizationLevel.Admin)]
                protected override Task DeleteAsync(string id)
                {
                    return base.DeleteAsync(id);
                }
            }
        }

    AuthorizeLevel 属性を使用して、コントローラー上の挿入/更新/削除操作へのパブリック アクセスを制限していることに注意します。 このシナリオでは、Customer の一覧は読み取り専用ですが、新しい Order を作成し、既存の顧客と関連付けることはできます。

2.  **コント ローラー** フォルダー、ファイルを追加 **MobileOrderController.cs**:

        using Microsoft.WindowsAzure.Mobile.Service;
        using ShoppingService.DataObjects;
        using ShoppingService.Models;
        using System.Linq;
        using System.Threading.Tasks;
        using System.Web.Http;
        using System.Web.Http.Controllers;
        using System.Web.Http.Description;
        using System.Web.Http.OData;

        namespace ShoppingService.Controllers
        {
            public class MobileOrderController : TableController<MobileOrder>
            {
                protected override void Initialize(HttpControllerContext controllerContext)
                {
                    base.Initialize(controllerContext);
                    ExistingContext context = new ExistingContext();
                    DomainManager = new MobileOrderDomainManager(context, Request, Services);
                }

                public IQueryable<MobileOrder> GetAllMobileOrders()
                {
                    return Query();
                }

                public SingleResult<MobileOrder> GetMobileOrder(string id)
                {
                    return Lookup(id);
                }

                public Task<MobileOrder> PatchMobileOrder(string id, Delta<MobileOrder> patch)
                {
                    return UpdateAsync(id, patch);
                }

                [ResponseType(typeof(MobileOrder))]
                public async Task<IHttpActionResult> PostMobileOrder(MobileOrder item)
                {
                    MobileOrder current = await InsertAsync(item);
                    return CreatedAtRoute("Tables", new { id = current.Id }, current);
                }

                public Task DeleteMobileOrder(string id)
                {
                    return DeleteAsync(id);
                }
            }
        }

3. これで、サービスを実行する準備が整いました。 キーを押して **f5 キーを押して** ヘルプ ページに組み込まれているテスト クライアントを使用してデータを変更するとします。

両方のコント ローラーの実装が、Dto を排他的に使用を行うことに注意してください **MobileCustomer** と **MobileOrder** 基になるモデルに依存しないとします。 これらの Dto は容易に JSON にシリアル化し、すべてのプラットフォームで Mobile Services クライアント SDK とデータを交換するために使用できます。 たとえば、Windows ストア アプリケーションを作成する場合、対応するクライアント側の型は以下のようになります。 この型は、他のクライアント プラットフォームでも同様です。

    using Microsoft.WindowsAzure.MobileServices;
    using System;

    namespace ShoppingClient
    {
        public class MobileCustomer
        {
            public string Id { get; set; }

            public string Name { get; set; }

            [CreatedAt]
            public DateTimeOffset? CreatedAt { get; set; }

            [UpdatedAt]
            public DateTimeOffset? UpdatedAt { get; set; }

            public bool Deleted { get; set; }

            [Version]
            public string Version { get; set; }

        }

    }

次の手順では、サービスにアクセスするクライアント アプリケーションを作成できます。

