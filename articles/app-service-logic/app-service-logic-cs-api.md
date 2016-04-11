<properties
   pageTitle="C# API アプリ"
   description="C# API アプリ"
   services="app-service\logic"
   documentationCenter=".net"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/29/2015"
   ms.author="jehollan"/>

#C# API アプリ
C# API アプリからの単純な c# 式を実行する簡単な方法を使用する *ロジック アプリを実行しながら*します。

##この API アプリを使用するタイミング
この API アプリの重要なシナリオは、ロジック アプリと同じであるへの書き込みを行うコードのライフ サイクルの場合 *いない* の他のシナリオで呼び出されるコードです。

一方、ロジック アプリに依存しないライフサイクルを持つ再利用可能なコードのスニペットが必要な場合は、WebJobs API アプリを使用して簡単なコード式を作成し、これらをロジック アプリから呼び出す必要があります。

最後に、他のパッケージを追加したい場合は、Base64 エンコード バイナリ文字列 (Blob Storage からの出力など) としてコネクタにアセンブリ (.dll) で渡す必要があります。  パッケージとアセンブリの柔軟性を高めたい場合は、WebJob が適しているでしょう。

使用して、 [JavaScript API アプリ](app-service-logic-javascript-api.md) JS で式を記述を希望する場合。

##C# API アプリを作成します。
C# API アプリを使用するには、まず C# API アプリのインスタンスを作成する必要があります。 これは、ロジック アプリの作成時にインラインで作成するか、Azure Marketplace から C# API アプリを選択することで作成できます。

##Logic Apps デザイナー画面での c# API アプリの使用
###トリガー
ロジック アプリサービスが (定義した間隔で) ポーリングするトリガーを作成できます。`false` 以外が返された場合はロジック アプリが実行され、それ以外の場合は、次のポーリング間隔まで待機してもう一度確認します。

トリガーへの入力は次のとおりです。
- **C# の式**  - 評価される式。 これは関数内で呼び出され、ロジック アプリを実行しない場合は `false` を返す必要がありますが、ロジック アプリを実行する場合はこれ以外の何でも返すことができます。 応答の内容をロジック アプリのアクションで使用できるようになります。

たとえば、1 時間のうち 15 ～ 30 分の間のみロジック アプリを実行する簡単なトリガーを作成できます。

```
var d = new DateTime.Now; return (d.Minute > 15) && (d.Minute < 30);
```

###アクション

同様に、実行するアクションを指定できます。 

アクションへの入力は次のとおりです。
- **C# の式**  - 評価される式。 含める必要があります、 `return` ステートメントを任意のコンテンツを取得します。 
- **コンテキスト オブジェクト** -、トリガーに渡すことができる省略可能なコンテキスト オブジェクト。 数制限はありませんが、ベースは、JObject である必要がありますだけプロパティを定義する `{ ... }`, 、され (値として渡されます JToken 不完全名)、キー名を使用してスクリプトにオブジェクトを参照できます。
- **ライブラリ** -スクリプトのコンパイルに含まれる .dll ファイルの省略可能な配列です。  この配列は次の構造を使用し、出力として .dll を使用して Blob Storage コネクタの次に最適に機能します。

```javascript
[{"filename": "name.dll", "assembly": {Base64StringFromConnector}, "usingstatment": "using Library.Reference;"}]
```

たとえば、Office 365 のトリガーを使用している **新しい電子メール**します。 返されるオブジェクトは次のとおりです。

```javascript
{
    ...
    "Attachments" : [
        {
            "name" : "Picture.png",
            "content" : {
                "ContentData" : "iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFAQMAAAC3obSmAAAABGdBTUEAALGPC/xhBQAAAAFzUkdCAK7OHOkAAAAGUExURf///wAAAFXC034AAAASSURBVAjXY2BgCGBgYOhgKAAABEIBSWDJEbYAAAAASUVORK5CYII=",
                "ContentType" : "image/png",
                "ContentTransferEncoding" : "Base64"
            }
        },  
        {
            "name" : "File.txt",
            "content" : {
                "ContentData" : "Don't worry, be happy!",
                "ContentType" : "text/plain",
                "ContentTransferEncoding" : "None"
            }
        }   
    ]
}
```

ただし、Yammer の投稿にこれらの添付ファイルをアップロードする場合、 Yammer 添付ファイルのスキーマは少し異なります。 これをロジック アプリ内で解析できるようになりました。 コンテキスト オブジェクトの場合は、`@triggerBody()` を渡すだけです。式の場合は、次のように渡します。

```javascript
JArray YammerAttachments = new JObject();
foreach(var obj in (JArray)Attachments)
{
    JObject att = new JObject();
    att["Content"] = obj["content"];
    att["FileName"] = obj["Name"];
    YammerAttachments.Add(att); 
}
return YammerAttachments;
```

このアクションは、結果オブジェクトの関数から返されたオブジェクトを返します。 このため、Yammer API アプリで参照できます `@body('csapi').results` の **添付ファイル** プロパティです。

## コネクタでできること
これでコネクタが作成されたため、ロジック アプリを使用してこのコネクタをビジネス フローに追加することができます。 参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [管理と監視 API アプリとコネクタの](../app-service-api/app-service-api-manage-in-portal.md)です。

<!--References -->

<!--Links -->
[Creating a Logic App]: app-service-logic-create-a-logic-app.md

