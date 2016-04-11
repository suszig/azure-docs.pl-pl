<properties
   pageTitle="JavaScript API"
   description="JavaScript API"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/29/2015"
   ms.author="stepsic"/>

#JavaScript API アプリ
JavaScript API アプリからの単純な JavaScript の式を実行する簡単な方法を使用する *ロジック アプリを実行しながら*します。 

##この API アプリを使用するタイミング
この API アプリの重要なシナリオは、ロジック アプリと同じであるへの書き込みを行うコードのライフ サイクルの場合 *いない* の他のシナリオで呼び出されるコードです。

一方、ロジック アプリに依存しないライフサイクルを持つ再利用可能なコードのスニペットが必要な場合は、WebJobs API アプリを使用して簡単なコード式を作成し、これらをロジック アプリから呼び出す必要があります。

最後に、他のパッケージを追加したい場合は、JavaScript API アプリを使用してすべてのライブラリを追加できないため、WebJobs API アプリを使用する必要があります。 

使用して、 [c# API アプリ](app-service-logic-cs-api.md) で c# 式を記述を希望する場合。

##JavaScript API アプリを作成する
JavaScript API アプリを使用するには、まず JavaScript API アプリのインスタンスを作成する必要があります。 これは、ロジック アプリの作成時にインラインで作成するか、Azure Marketplace から JavaScript API アプリを選択することで作成できます。

##Logic Apps デザイナー画面で JavaScript API アプリを使用する
###トリガー
ロジック アプリ サービスが (定義した間隔で) ポーリングするトリガーを作成できます。任意のコンテンツが返された場合はロジック アプリが実行され、それ以外の場合は、次のポーリング間隔まで待機してもう一度確認します。

トリガーへの入力は次のとおりです。
- **JavaScript 式**  - 評価される式。 これは関数内で呼び出され、ロジック アプリを実行しない場合は `false` を返す必要がありますが、ロジック アプリを実行する場合はこれ以外の何でも返すことができます。 応答の内容をロジック アプリのアクションで使用できるようになります。
- **コンテキスト オブジェクト** -、トリガーに渡すことができる省略可能なオブジェクトです。 必要な数だけプロパティを定義できますが、最上位のエンティティをオブジェクトにする必要があります (`{ "bar" : 0}` など)。

たとえば、1 時間のうち 15 ～ 30 分の間のみロジック アプリを実行する簡単なトリガーを作成できます。

```
var d = new Date(); return (d.getMinutes() > 15) && (d.getMinutes() < 30);
```

###アクション

同様に、実行するアクションを指定できます。 

アクションへの入力は次のとおりです。
- **JavaScript 式**  - 評価される式。 含める必要があります、 `return` ステートメントを任意のコンテンツを取得します。 
- **コンテキスト オブジェクト** -、トリガーに渡すことができる省略可能なオブジェクトです。 必要な数だけプロパティを定義できますが、最上位のエンティティをオブジェクトにする必要があります (`{ "bar" : 0}` など)。

たとえば、Office 365 のトリガーを使用している **新しい電子メール**します。 返されるオブジェクトは次のとおりです。
```
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

```
return Attachments.map(function(obj){var a = obj.Content; a.FileName = obj.Name; return a;})
```

このアクションは、関数から返された JSON を返します。 このため、Yammer API アプリで参照できます `@body('javascriptapi')` の **添付ファイル** プロパティです。

## コネクタでできること
これでコネクタが作成されたため、ロジック アプリを使用してこのコネクタをビジネス フローに追加することができます。 参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [管理と監視 API アプリとコネクタの](../app-service-api/app-service-api-manage-in-portal.md)です。

<!--References -->

<!--Links -->
[Creating a Logic App]: app-service-logic-create-a-logic-app.md

