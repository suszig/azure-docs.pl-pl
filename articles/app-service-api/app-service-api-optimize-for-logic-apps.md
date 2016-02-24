
<properties
    pageTitle="Logic Apps 向けの API アプリの拡張"
    description="この記事では、Logic Apps に API アプリを対応させるための方法について説明します"
    services="app-service\logic"
    documentationCenter=".net"
    authors="sameerch"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-logic"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/15/2015"
    ms.author="sameerch"/>

# Logic Apps 向けの API アプリの拡張 #

この記事では、API アプリを Logic Apps に対応させるための API 定義を定義する方法について説明します。 これにより、Logic Apps デザイナーで使用したときの API アプリのエンド ユーザー エクスペリエンスが向上します。

## 前提条件

初めて使用する場合は、 [API apps](app-service-api-apps-why-best-platform.md) で [Azure App Service](../app-service/app-service-value-prop-what-is.md), 、マルチパートのシリーズを読むことをお勧め [API アプリの作成](app-service-dotnet-create-api-app.md)


## 表示名の追加 ##
Logic Apps デザイナーには操作、フィールド、パラメーターの名前が表示されますが、これらはプログラムによって生成された名前のため読みづらいことがあります。 読みやすさを向上させるために、Logic Apps デザイナーでは、ここで使用可能なに表示より読みやすいテキスト値と呼ばれる、 *表示名* 操作、フィールド、およびパラメーターの既定名に代えてします。 これを実現するために、Logic Apps デザイナーは、API アプリによって提供される swagger メタデータ内に特定のプロパティがあるかどうかを調べます。  表示名として、次のプロパティが使用されます。

* 操作 (アクションやトリガー)  
  値、 **概要** 存在以外の場合はプロパティの値 **operationId** プロパティです。 Swagger 2.0 仕様は最大 120 文字を使用できます、 **概要** プロパティです。

* パラメーター (入力)  
  値、 **x ms サマリー** 場合は存在しない場合、拡張機能プロパティの値、 **名前** プロパティです。  **X ms サマリー** 拡張プロパティは、コードで動的に設定する必要があります。 その方法については、このトピックの「カスタム属性を使用した拡張プロパティの注釈の設定」セクションで説明します。  **名前** 、///コメントを使用してプロパティを設定することができます。 その方法については、このトピックの「API 定義の生成時の XML コメントの使用」セクションで説明します。

* スキーマ フィールド (出力応答)  
  値、 **x ms サマリー** 場合は存在しない場合、拡張機能プロパティの値、 **名前** プロパティです。  **X ms サマリー** 拡張プロパティは、コードで動的に設定する必要があります。 その方法については、このトピックの「カスタム属性を使用した拡張プロパティの注釈の設定」セクションで説明します。  **名前** 、///コメントを使用してプロパティを設定することができます。 その方法については、このトピックの「API 定義の生成時の XML コメントの使用」セクションで説明します。

**注:** は以下に表示名の長さが 30 文字に対応すること勧めします。

### API 定義の生成時の XML コメントの使用

一般的な方法を使用して API コント ローラーに注釈を付けるには、Visual Studio を使用して開発用 [XML コメント](https://msdn.microsoft.com/library/b2s063f7.aspx)します。  コンパイルされたときに [/doc](https://msdn.microsoft.com/library/3260k4x7.aspx), 、コンパイラは、XML ドキュメント ファイルを作成します。  API アプリ SDK に含まれている Swashbuckle ツールセットでは、API メタデータを生成するときにこれらのコメントを組み込むことができます。この操作を実現するには、次の手順に従って API プロジェクトを構成します。

1. Visual Studio でプロジェクトを開きます。

2.  **ソリューション エクスプ ローラー**, プロジェクトを右クリックして、選択 **プロパティ**します。

    ![プロジェクトのプロパティ](./media/app-service-api-optimize-for-logic-apps/project-properties.png)

3. プロジェクトのプロパティ ページが表示されたら、次の手順を実行します。

    - 選択、 **構成** の設定が適用されます。 通常は、指定した設定がデバッグ ビルドとリリース ビルドの両方に適用されるように、[すべての構成] を選択します。
    
    - 選択、 **ビルド** 左側のタブをクリックします。
    
    - いることを確認、 **XML ドキュメント ファイル** オプションがオンになっています。 Visual Studio では、プロジェクトの名前に基づいて既定のファイル名が付けられます。 この値は、名前付け規則によって求められる値に設定することも、そのまま使用することもできます。

    ![XML ドキュメントのプロパティを設定](./media/app-service-api-optimize-for-logic-apps/xml-documentation-file-property.png)

4. 開いている、 *SwaggerConfig.cs* ファイル (プロジェクトに **App_Start** フォルダー)。

5. 追加 **を使用して** ディレクティブの先頭に、 *SwaggerConfig.cs* 用のファイル、 **システム** と **System.Globalization** 名前空間。

        using System;
        using System.Globalization;
 
6. 検索、 *SwaggerConfig.cs* ファイルへの呼び出しの **GetXmlCommentsPath**, 、実行されるように、行をコメント解除します。 Visual Studio がへの呼び出しに下線がこのメソッドを実装していないので **GetXmlCommentsPath** と現在のコンテキストで定義されていないことを示します。 ここでは問題ありません。 次の手順でこのメソッドを実装します。

7. 次の実装を追加、 **GetXmlCommentsPath** メソッドを **SwaggerConfig** クラス (で定義されている、 *SwaggerConfig.cs* ファイル)。 このメソッドは、前の手順でプロジェクトの設定に指定した XML ドキュメント ファイルを返すのみです。

        public static string GetXmlCommentsPath()
        {
            return String.Format(CultureInfo.InvariantCulture, 
                                 @"{0}\bin\ContactsList.xml", 
                                 AppDomain.CurrentDomain.BaseDirectory);
        }

8. 最後に、コントローラー メソッドの XML コメントを指定します。 そのためには、API アプリのコントローラー ファイルの 1 つを開き、ドキュメント化するコントローラー メソッドの前の空行に「///」を入力します。 Visual Studio により、コメント セクションが自動的に挿入されます。このセクション内に、メソッドの概要、パラメーター、戻り値の情報を指定できます。 

API アプリをビルドして発行すると、ドキュメント ファイルがペイロードに含まれ、API アプリの残りの部分と共にアップロードされることを確認できます。

## 高度な操作やプロパティの分類

Logic Apps デザイナー上で操作、パラメーター、プロパティを表示するための領域は限られています。 加えて、API アプリでは、多数の操作やプロパティを定義できます。 狭い領域に大量の情報を表示すると、デザイナーが使いにくくなります。 

Logic Apps デザイナーでは、あふれる情報に対応するために、API アプリの操作とプロパティをユーザー定義のカテゴリにグループ化できます。 API アプリは、操作とプロパティの適切な分類を使用して最初に最も基本的な有用な操作やプロパティを表示することにより、ユーザー エクスペリエンスを向上させることができます。  

この機能を提供するために、Logic Apps デザイナーは、API アプリの swagger API 定義内で特定のカスタム ベンダー拡張プロパティを検索します。 このプロパティの名前は **x ms 視界** 次の値を受け取ることができます。

* 空または"none"  
  これらの操作とプロパティは、ユーザーがすぐに表示できます。

* [詳細設定]  
  これらの操作とプロパティの詳細としては、既定で表示されません。 ただし、ユーザーは、必要に応じて簡単にこれらの操作とプロパティにアクセスできます。

* 「内部」  
  これらの操作とプロパティはシステムや内部プロパティとして扱われ、ユーザーが直接使用を意図していません。  そのため、これらの操作とプロパティはデザイナーで非表示に設定され、コード ビューでのみ使用できます。  このようなプロパティを指定することも、 **x ms スケジューラ推奨** 拡張プロパティを Logic Apps デザイナーを使用して値を設定します。  例については、上の記事を参照してください。 [API アプリにトリガーを追加する](app-service-api-dotnet-triggers.md)です。


## カスタム属性を使用した拡張プロパティの注釈の設定

既に説明したように、カスタムのベンダー拡張プロパティは、Logic Apps デザイナーが使用できるさまざまな情報を提供する注釈を API メタデータに設定するために使用します。  API アプリを記述する静的なメタデータを使用する場合を直接編集できます、 */metadata/apiDefinition.swagger.json* によってプロジェクトが必要な拡張プロパティを手動で追加します。

動的メタデータを使用する API アプリの場合は、カスタム属性を使用してコードに注釈を設定できます。  操作フィルターを定義し、 *SwaggerConfig.cs* ファイルにカスタム属性を検索するために必要なベンダー拡張を追加します。  この方法が、動的に生成するため以下で詳しく説明されている、 **x ms サマリー** 拡張機能プロパティです。

1. という属性クラスを定義 **CustomSummaryAttribute** 、コードの注釈に使用されます。

        [AttributeUsage(AttributeTargets.All)]
        public class CustomSummaryAttribute : Attribute
        {
            public string SummaryText { get; internal set; }

            public CustomSummaryAttribute(string summaryText)
            {
                this.SummaryText = summaryText;
            }
        }

2. 呼ばれる操作フィルターを定義する **AddCustomSummaryFilter** 操作パラメーター内でこのカスタム属性を検索します。


        using Swashbuckle.Swagger;

        ...

        public class AddCustomSummaryFilter : IOperationFilter
        {
            public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
            {
                if (operation.parameters == null)
                {
                    // no parameter
                    return;
                }

                foreach (var param in operation.parameters)
                {
                    var summaryAttributes = apiDescription.ParameterDescriptions.First(x => x.Name.Equals(param.name))
                                            .ParameterDescriptor.GetCustomAttributes<CustomSummaryAttribute>();

                    if (summaryAttributes != null && summaryAttributes.Count > 0)
                    {
                        // add x-ms-summary extension
                        if (param.vendorExtensions == null)
                        {
                            param.vendorExtensions = new Dictionary<string, object>();
                        }
                        param.vendorExtensions.Add("x-ms-summary", summaryAttributes[0].SummaryText);
                    }
                }
            }
        }

3. 編集、 *SwaggerConfig.cs* ファイルし、上記で定義したフィルター クラスを追加します。


            GlobalConfiguration.Configuration
                .EnableSwagger(c =>
                    {
                        ...
                        c.OperationFilter<AddCustomSummaryFilter>();
                        ...
                    }

4. 使用して、 **CustomSummaryAttribute** クラスを次のコード スニペットに示すように、コードの注釈を設定します。

        /// <summary>
        /// Send Message
        /// </summary>
        /// <param name="queueName">The name of the Storage Queue</param>
        /// <param name="messageText">The message text to be sent</param>
        public void SendMessage(
            [CustomSummary("Queue Name")] string queueName,
            [CustomSummary("Message Text")] string messageText)
        {
             ...
        }

    上記の API アプリをビルドすると、次の API メタデータが生成されます。


            ...
            "post": {
                ...
                "parameters": [
                    {
                        "name": "queueName",
                        "in": "query",
                        "description": "The name of the Storage Queue",
                        "required": true,
                        "x-ms-summary": "Queue Name",
                        "type": "string"
                    },
                    {
                        "name": "messageText",
                        "in": "query",
                        "description": "The message text to be sent",
                        "required": true,
                        "x-ms-summary": "Message Text",
                        "type": "string"
                    }
                ],
                ...

5. 同様に、スキーマのフィルターを定義できます **AddCustomSummarySchemaFilter** に自動的に注釈を付ける、 **x ms サマリー** 拡張プロパティに、スキーマ モデルの例を次に示します。

        public class AddCustomSummarySchemaFilter: ISchemaFilter
        {
            public void Apply(Schema schema, SchemaRegistry schemaRegistry, Type type)
            {
                SetCustomSummary(schema, type.GetCustomAttribute<CustomSummaryAttribute>());

                if (schema.properties != null)
                {
                    foreach (var property in schema.properties)
                    {
                        var summaryAttribute = type.GetProperty(property.Key).GetCustomAttribute<CustomSummaryAttribute>();
                        SetCustomSummary(property.Value, summaryAttribute);
                    }
                }
            }

            private static void SetCustomSummary(Schema schema, CustomSummaryAttribute summaryAttribute)
            {
                if (summaryAttribute != null)
                {
                    if (schema.vendorExtensions == null)
                    {
                        schema.vendorExtensions = new Dictionary<string, object>();
                    }
                    schema.vendorExtensions.Add("x-ms-summary", summaryAttribute.SummaryText);
                }
            }
        }

## 概要

この記事では、Logic Apps デザイナーで使用したときの API アプリのエンド ユーザー エクスペリエンスを向上させる方法について説明しました。  ベスト プラクティスとして、すべての操作 (アクション、トリガー)、パラメーター、プロパティに対して適切なフレンドリ名を付けることをお勧めします。  また、基本的な操作については 5 個以下にすることをお勧めします。  入力パラメーターについては基本的なプロパティの数を 4 個以下、プロパティについては 5 個以下にすることをお勧めします。 残りの操作とプロパティについては、高度な操作とプロパティであることを示すマークを付ける必要があります。
 

