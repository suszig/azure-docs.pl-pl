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


# C# API アプリ

C# API アプリを使用すると、*ロジック アプリの実行中に*単純な C# の式を簡単に実行できます。

## この API アプリを使用するタイミング

この API アプリの重要なシナリオは、記述するコードのライフサイクルをロジック アプリと同じにする場合と、このコードを他のシナリオで呼び出さ*ない*ようにする場合です。

一方、ロジック アプリに依存しないライフサイクルを持つ再利用可能なコードのスニペットが必要な場合は、WebJobs API アプリを使用して簡単なコード式を作成し、これらをロジック アプリから呼び出す必要があります。

最後に、他のパッケージを追加したい場合は、Base64 エンコード バイナリ文字列 (BLOB ストレージからの出力など) としてコネクタにアセンブリ (.dll) で渡す必要があります。 パッケージとアセンブリの柔軟性を高めたい場合は、WebJob が適しているでしょう。

使用して、 [JavaScript API アプリ](app-service-logic-javascript-api.md) JS で式を記述を希望する場合。

## C# API アプリを作成します。

C# API アプリを使用するには、まず C# API アプリのインスタンスを作成する必要があります。 これは、ロジック アプリの作成時にインラインで作成するか、Azure Marketplace から C# API アプリを選択することで作成できます。

## Logic Apps デザイナー画面での c# API アプリの使用

### トリガー

ロジック アプリ サービスが (で定義した間隔) をポーリングするトリガーを作成して、それ以外が返された場合 `false`, 、ロジック アプリが実行されますが、それ以外の場合、もう一度確認する次のポーリング間隔まで待機します。

トリガーへの入力は次のとおりです。
- **C# の式**  - 評価される式。 関数内で呼び出さして返す必要があります `false` 、ロジック アプリを実行したくない場合にし、ロジック アプリを実行している場合以外の何でも返すことができます。 応答の内容をロジック アプリのアクションで使用できるようになります。

たとえば、1 時間のうち 15 ～ 30 分の間のみロジック アプリを実行する簡単なトリガーを作成できます。

```
var d = new DateTime.Now; return (d.Minute > 15) && (d.Minute < 30);
```

### アクション

同様に、実行するアクションを指定できます。

アクションへの入力は次のとおりです。
- **C# の式**  - 評価される式。 含める必要があります、 `を返す` ステートメントを任意のコンテンツを取得します。
- **コンテキスト オブジェクト** -、トリガーに渡すことができる省略可能なコンテキスト オブジェクト。 数制限はありませんが、ベースは、JObject である必要がありますだけプロパティを定義する `{...}`, 、され (値として渡されます JToken 不完全名)、キー名を使用してスクリプトにオブジェクトを参照できます。
- **ライブラリ** -スクリプトのコンパイルに含まれる .dll ファイルの省略可能な配列です。 この配列は次の構造を使用し、出力として .dll を使用して BLOB ストレージ コネクタの次に最適に機能します。

```javascript
[{"filename": "name.dll", "assembly": {Base64StringFromConnector}, "usingstatment": "using Library.Reference;"}]
```

たとえば、Office 365 使用している場合は、**新しい電子メール**をトリガーします。 返されるオブジェクトは次のとおりです。

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

ただし、Yammer の投稿にこれらの添付ファイルをアップロードする場合、 Yammer 添付ファイルのスキーマは少し異なります。 これで、ロジック アプリ内これを解析することができます。 コンテキスト オブジェクトが渡すだけです: `@triggerBody()`, 、式を渡します。

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

このアクションは、結果オブジェクトの関数から返されたオブジェクトを返します。 このため、Yammer API アプリで参照できます `@body('csapi') .results` の **添付ファイル** プロパティです。

## コネクタでできること

これでコネクタが作成されたため、このコネクタを Logic App を使用してビジネス フローに追加することができます。 を参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md).

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [管理と監視 API アプリとコネクタの](../app-service-api/app-service-api-manage-in-portal.md)します。





[creating a logic app]: app-service-logic-create-a-logic-app.md 

