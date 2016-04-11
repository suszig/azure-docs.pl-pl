<properties
   pageTitle="VETR を使用した EAI ロジック アプリの作成 (Azure App Service の Logic Apps) | Microsoft Azure"
   description="BizTalk XML サービスの検証機能、エンコード機能、および変換機能"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/07/2015"
   ms.author="rajram"/>


# VETR を使用した EAI ロジック アプリの作成

ほとんどの Enterprise Application Integration (EAI) シナリオでは、ソースと出力先との間でデータの仲介をします。 このようなシナリオには、多くの場合、次のような共通の要件があります。

- 異なるシステムから受け取るデータが正しい形式であることを確認する。
- 受信データに対して "ルックアップ" を実行して意思決定を行う。
- データの形式を変換する。 ある形式から別の形式 (たとえば、CRM システムのデータ形式から ERP システムのデータ形式) にデータを変換する。
- 目的のアプリケーションまたはシステムにデータを伝送するルートを作成する。

この記事では、一般的な統合パターンの "一方向メッセージ仲介" または VETR (検証、強化、変換、ルート) と呼ばれるパターンについて説明します。 VETR パターンは、ソース エンティティと、出力先エンティティとの間でデータを仲介します。 通常は、ソースも出力先もデータ ソースです。

注文を受け付ける Web サイトの場合を考えます。 ユーザーは、HTTP を使用してシステムに注文をポストします。 その間、システムは受信データが正確かどうかを検証したうえで正規化し、次の処理のために Service Bus キューに保持します。 システムは、キューから注文を取り出し、特定の形式で待ち受けます。 このため、エンド ツー エンド フローは、次のようになります。

**HTTP** → **検証** → **変換** → **Service Bus**

![基本的な VETR フロー][1]

次の BizTalk API は、このパターンを作成する際に役立ちます。

* **HTTP トリガー** -メッセージ イベントをトリガーするソース
* **検証** -着信データの正確性の検証
* **変換** -データの受信形式を下流のシステムで必要な変換
* **Service Bus コネクタ** -データを送信する転送先エンティティ


## 基本的な VETR パターンの構築
### 基本

Azure ポータルで [ **+ 新規**, [ **Web + モバイル**, 、し、[ **ロジック アプリ**します。 名前、場所、サブスクリプション、リソース グループ、および動作する場所を選択します。 リソース グループは、アプリのコンテナーとして機能します。また、アプリのリソースはすべて同じリソース グループに移動します。

次に、トリガーとアクションを追加します。


## HTTP トリガーの追加
1.  **ロジック アプリ テンプレート**, [ **を最初から作成**します。
1. 選択 **HTTP リスナー** 新しいリスナーを作成するギャラリーからです。 それを呼び出す **HTTP1**します。
2. 設定、 **応答を自動的に送信するか。** false に設定します。 トリガーの動作を設定して構成 _HTTP メソッド_ に _POST_ と設定 _相対 URL_ に _/OneWayPipeline_:  
    ![HTTP トリガー][2]
3. 緑色のチェックマークをオンにして、トリガーを完了します。

## 検証アクションの追加

ここでは、トリガーが起動したとき (HTTP エンドポイントで呼び出しが受信されたとき) に常に実行されるアクションを入力します。

1. 追加 **BizTalk XML Validator** ギャラリーから名前を付けます _(Validate1)_ インスタンスを作成します。
2. 受信 XML メッセージを検証する XSD スキーマを構成します。 選択、 _検証_ アクションを選択 _triggers('httplistener').outputs します。コンテンツ_ の値として、 _inputXml_ パラメーター。

これで、検証アクションは、HTTP リスナー後の最初のアクションになります。 

![BizTalk XML 検証][3]

同様にして、他のアクションも追加します。 

## 変換アクションの追加
受信データを正規化するために変換を構成します。

1. 追加 **BizTalk 変換サービス** ギャラリーからです。
2. 受信 XML メッセージを変換する変換を構成するには、選択、 **変換** この API が呼び出されたときに実行するアクションとして動作します。 選択 ```triggers(‘httplistener’).outputs.Content``` の値として _inputXml_します。 *マップ* 省略可能なパラメーターは、受信データがすべての構成済みの変換を含む照合し、スキーマと一致するものだけが表示されます。
3. 最後に、検証が成功した場合にのみ、変換が実行されるようにします。 この条件を構成するには、右、上にある歯車アイコンを選択し、選択 _満たされる条件を追加_します。 条件を ```equals(actions('xmlvalidator').status,'Succeeded')``` に設定します。  

![BizTalk 変換][4]


## Service Bus コネクタの追加
次に、データを書き込む場所である出力先 (Service Bus キュー) を追加します。

1. 追加、 **Service Bus コネクタ** ギャラリーからです。 設定、 **名** に _Servicebus1_, 、設定されて **接続文字列** 、service bus インスタンスへの接続文字列を次のように設定します。 **エンティティ名** に _キュー_, 、および skip **サブスクリプション名**です。
2. 選択、 **メッセージの送信** アクションとセット、 **コンテンツ** をアクションに対応するフィールド _actions('transformservice').outputs します。OutputXml_します。
3. 設定、 **コンテンツ タイプ** フィールドを *アプリケーションまたは xml*:  

![Service Bus][5]


## HTTP 応答の送信
パイプライン処理の完了後、成功した場合と失敗した場合のそれぞれについて HTTP 応答を返すように構成します。手順は次のとおりです。

1. 追加、 **HTTP リスナー** し、ギャラリーから、 **HTTP 応答の送信** アクション。
2. 設定 **応答 ID** 送信 *メッセージ*します。
2. 設定 **応答コンテンツ** に *パイプライン処理が完了しました*します。
3. **応答のステータス コード** に *200* HTTP 200 OK を示すためにします。
4. 右、上部のメニューで、ドロップダウンを選択し、選択 **満たされる条件を追加**します。  次のように、条件を設定します。  
    ```@equals(actions('azureservicebusconnector').status,'Succeeded')```  <br/>
5. 失敗した場合も HTTP 応答を送信するために、同じ手順を繰り返します。 変更 **条件** 次のようにします。  
```@not(equals(actions('azureservicebusconnector').status,'Succeeded'))``` <br/>
6. 選択 **OK** し **作成**します。



## 完了
メッセージが HTTP エンドポイントに送信されるたびに、エンドポイントがアプリをトリガーし、上記で作成したアクションが実行されます。 作成したこのようなロジック アプリを管理するには、次のように選択します。 **参照** し、Azure ポータルで **Logic Apps**します。 確認するアプリを選択すると、詳細が表示されます。

いくつかの役に立つトピック:

[管理し、API Apps とコネクタの監視](app-service-logic-monitor-your-connectors.md)  <br/>
[Logic Apps を監視する](app-service-logic-monitor-your-logic-apps.md)

<!--image references -->
[1]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BasicVETR.PNG
[2]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/HTTPListener.PNG
[3]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkXMLValidator.PNG
[4]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkTransforms.PNG
[5]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/AzureServiceBus.PNG


