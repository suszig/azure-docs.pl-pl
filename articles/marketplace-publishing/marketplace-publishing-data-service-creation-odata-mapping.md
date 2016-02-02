<properties
   pageTitle="Marketplace 用のデータ サービスを作成するためのガイド |Microsoft Azure"
   description="Azure Marketplace で購入できるデータ サービスを作成、認定、デプロイする方法について詳しく説明します。"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

   <tags
      ms.service="marketplace-publishing"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="12/06/2015"
      ms.author="hascipio; avikova" />


# CSDL を使用した既存の Web サービスの OData へのマッピング

この記事では、CSDL を使用して既存のサービスを OData 互換サービスにマッピングする方法について説明します。 また、サービスの呼び出し経由でクライアントからの入力要求を変換し、OData 互換フィード経由で出力 (データ) を返すマッピング ドキュメント (CSDL) を作成する方法についても説明します。 Microsoft Azure Marketplace は、OData プロトコルを使用してエンドユーザーにサービスを公開しています。 コンテンツ プロバイダー (データ所有者) が公開するサービスは、REST、SOAP など多様な形式で公開されます。

## CSDL とその構造

CSDL (Conceptual Schema Definition Language) は、Azure Marketplace に対して共通の XML 表現で Web サービスまたはデータベース サービスを記述する方法を定義した仕様です。

**要求フロー**の概要:

  `クライアントには、Azure が]-> [Marketplace] の [コンテンツ プロバイダーの Web サービス (Get、Post、Delete、Put)`

**データ フロー**は反対方向です。

  `クライアント <-Azure Marketplace <-コンテンツのプロバイダーの web サービス`

**図 1** は、Azure Marketplace にアクセスして、クライアントがコンテンツ プロバイダー (発行元のサービス) のデータを取得する方法を示します。 CSDL は、コンテンツ プロバイダーのサービスと要求側クライアント間で渡される要求とデータを処理するマッピング/変換コンポーネントに使用されます。

*図 1: Azure Marketplace 経由の要求側クライアントからコンテンツ プロバイダーへのフローの詳細*

  ![図](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Atom の背景情報は、Atom 発行と Azure Marketplace の拡張機能を構築する OData プロトコル参照してください: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

リンクの上からの抜粋します。
    *"(OData と呼ぶ)、Open Data プロトコルの目的は、データ サービスとして公開されるリソースに対して CRUD スタイル操作 (作成、読み取り、更新および削除) の REST ベースのプロトコルを提供します。 "データ サービス" とは、それぞれにゼロ個以上の "エントリ" がある 1 つ以上の "コレクション" から公開されたデータがあるエンドポイントです。エントリは型指定された名前と値のペアで構成されます。 OData は、マイクロソフトによって発行された OASIS (Organization for、進歩の Structured Information Standards) 標準でできるように、サーバー、クライアントまたはロイヤリティまたは制限なしでツールをビルドする必要のあるすべてのユーザーことができます。"*

### CSDL で定義する必要がある 3 つの重要な項目があります。

- **エンドポイント** サービス プロバイダーの
  サービスの Web アドレス (URI)
- **データ パラメーター** サービス プロバイダーへの入力として渡される
  データのコンテンツ プロバイダーのサービスに送信されるパラメーターの定義を入力します。
- **スキーマ** を要求するサービスに返されるデータの
  コンテナー、コレクションとテーブルには、変数や列、およびデータ型を含め、コンテンツ プロバイダーのサービスによって提供されているデータのスキーマです。

次の図は、クライアントが OData ステートメントを入力して (コンテンツ プロバイダーの Web サービスを呼び出して)、結果/データを取得するフローの概要です。

  ![図](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### 手順:

1. クライアントは、XML に定義されている入力パラメーターを指定した要求を、サービス呼び出しによってAzure Marketplace に送信します。
2. CSDL は、サービス呼び出しの検証に使用されます。
    - 書式設定されたサービス呼び出しは、Azure Marketplace によってコンテンツ プロバイダー サービスに送信されます。
3. Web サービスを実行し、Http 動詞のアクションを preforms (GET)
  データは、CSDL で定義されたマッピングを使用して (存在する場合)、要求されたデータが XML 形式でクライアントに公開を Azure Marketplace に返されます。
4. データがある場合、クライアントには XML または JSON 形式で送信されます。

## 定義

### OData ATOM pub

ATOM pub の拡張機能です。各エントリで 1 行の結果セットが表されます。 エントリのコンテンツ部分は、キーと値のペアとして行の値を含むように強化されます。 詳細については、参照してください。
[https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### CSDL - Conceptual Schema Definition Language

データベースで公開される関数 (SPROC) とエンティティを定義できます。 詳細についてはこちら: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)
> [AZURE.TIP] 記事が表示されない場合は、**他のバージョン**のドロップダウンをクリックしてバージョンを選択してください。

### EDM - Entry Data Model

- 概要: [http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx][overviewlink]
- プレビュー: [http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx][previewlink]
- データ型: [http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx][datatypeslink]
次に、クライアントが OData ステートメントを入力して (コンテンツ プロバイダーの Web サービスを呼び出して)、結果/データを取得するフロー (左から右方向) の詳細を示します。

  ![図](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)


## CSDL の基本

CSDL (Conceptual Schema Definition Language) は、Azure Marketplace に対して共通の XML 表現で Web サービスまたはデータベース サービスを記述する方法を定義した仕様です。 CSDL では、**データ ソースを Azure Marketplace にデータを渡す**ために重要な部分を記述します。その主要部分を次に示します。

- パブリックに使用できる関数について説明したインターフェイス情報 (FunctionImport ノード)
- メッセージ要求 (入力) とメッセージ応答 (出力) すべてのデータ型情報 (EntityContainer/EntitySet/EntityType ノード)
- 使用するトランスポート プロトコルに関するバインディング情報 (Header ノード)
- 指定したサービスを特定するためのアドレス情報 (BaseURI 属性)

簡単に説明すると、CSDL とは、サービス要求側とサービス プロバイダー間のプラットフォームや言語に依存しない規約です。 クライアントは CSDL を使用して、Web サービス/データベース サービスを特定し、パブリックに使用できる関数を呼び出すことができます。

### CSDL とデータベースまたはコレクションとの関連付け

**CSDL の仕様**

CSDL は、Web サービスを記述するための XML 文法です。 仕様には 4 つの主要な要素に分かれています。 EntitySet、FunctionImport です。名前空間、および EntityType します。

この抽象化をさらにわかりやすくするために、CSDL をテーブルに関連付けてみましょう。

繰り返しになりますが、

  CSDL とは、**サービス要求側**と**サービス プロバイダー**間のプラットフォームや言語に依存しない規約です。 **クライアント**は CSDL を使用して、**Web サービス/データベース サービス**を特定し、パブリックに使用できる**関数**を呼び出すことができます。

データ サービスの場合、4 つの部分の CSDL は、データベース、テーブル、列、ストアド プロシージャの観点から考えることができます。

これらを次のようにデータ サービスと関連付けます。

- EntityContainer ~ = データベース
- EntitySet ~ = テーブル
- EntityType ~ = 列
- FunctionImport ~ = ストアド プロシージャ

**許可される HTTP 動詞**
- 取得 (コレクションを返します)、db から値を返す
- POST – db (戻り id の URI は、コレクション内の新しいエントリの作成) からデータと省略可能な戻り値を渡すために使用
- 削除-(コレクションを削除) データベースからデータを削除
- 配置: データベースにデータを更新 (コレクションを置換または作成)

## メタデータ/マッピング ドキュメント

メタデータ/マッピング ドキュメントは、コンテンツ プロバイダーの既存の Web サービスをマッピングするために使用されます。このマッピングによって、Azure Marketplace システムによる OData Web サービスとして公開できます。 メタデータ/マッピング ドキュメントは CSDL に基づいており、CSDL にいくつかの拡張機能を実装することで、Azure Marketplace 経由の REST ベース Web サービスのニーズに対応しています。 拡張機能は、 [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) 名前空間。

次に、CSDL の例: (コピーと貼り付け、下の XML エディターと変更、サービスを一致させるために CSDL の例です。 貼り付けて CSDL のマッピング [DataService] タブで、サービスを作成するときに、  [Azure Marketplace の発行ポータル](https://publish.windowsazure.com))します。

**条件:**
用語の CSDL は、関連する、 [公開ポータル](https://publish.windowsazure.com) UI (PPUI) 用語です。
- MyWebOffer に関連する"Title"、PPUI では、します。
- 関連する、PPUI で MyCompany **発行者表示名** で、 [Microsoft デベロッパー センター](http://dev.windows.com/registration?accountprogram=azure) UI
- Web デザイナーまたはデータ サービス (、PPUI でプラン) に関連する API

**階層:**
  会社 (コンテンツ プロバイダー) は、計画がある、つまりサービス API を使用して行をオファーを所有しています。

### WebService CSDL の例

(C# アプリケーションと同様に) Web アプリケーション エンドポイントを公開しているサービスへの接続

        <?xml version="1.0" encoding="utf-8"?>
        
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        
          <EntityContainer Name="MyOffer">
        
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        
        
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        
        
        
        
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        
        <d:RequestBody d:httpMethod="POST">
                
                
            
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        
                        <Name>{name}</Name>
                        
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />
    
        
        <d:ErrorHandling>
        
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>
    
            
        </EntityContainer>
        
        
        
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID" d:IsPrimaryKey="True" Type="Int32"  Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount" Type="Double"   Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"   Type="String"   Nullable="false" d:Map="./City"/>
        <Property Name="State"  Type="String"   Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime" Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [AZURE.TIP] 資料の例については CSDL Web サービスを参照 [、既存のマッピングの例の web サービスを通じて CSDLs OData を](marketplace-publishing-data-service-creation-odata-mapping-examples.md)

### DataService CSDL の例

データベース テーブルまたはビューをエンドポイントとして公開しているサービスに接続します。
下の例では、2 つの基本データ用の Api ベースの API の CSDL (テーブルではなく、ビューで使用できます)。

        <?xml version="1.0"?>
        
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        
        <EntityContainer Name="MyOfferContainer">
        
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## 関連項目

- この記事を参照している場合は、学習や、特定のノードとそのパラメーターを理解することに興味のある、 [データ サービスの OData マッピング ノード](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) 定義し、説明、例については、および使用するケースのコンテキストのです。
- この記事を参照している例を確認したい場合は、 [データ サービスの OData マッピング例](marketplace-publishing-data-service-creation-odata-mapping-examples.md) コードの構文とコンテキストを理解しているサンプル コードを参照してください。
- この記事を参照データ サービスを Azure Marketplace に発行するための指定されたパスに戻り、 [データ サービスの発行ガイド](marketplace-publishing-data-service-creation.md)します。




[overviewlink]: http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx 
[previewlink]: http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx 
[datatypeslink]: http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx 

