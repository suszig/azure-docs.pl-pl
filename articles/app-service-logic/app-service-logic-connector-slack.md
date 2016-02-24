<properties 
    pageTitle="Logic Apps での Slack コネクタの使用 | Microsoft Azure App Service"
    description="Slack コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法"
    authors="rajeshramabathiran" 
    manager="dwrede" 
    editor="" 
    services="app-service\logic" 
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/11/2015"
    ms.author="rajram"/>

# Slack コネクタの使用開始とロジック アプリへの追加
Slack チャネルに接続し、チームにメッセージを投稿します。 コネクタを "ワークフロー" の一部として Logic Apps で使用し、さまざまなタスクを実行できます。 ワークフローで Slack コネクタを使用すると、他のコネクタを使用してさまざまなシナリオを実現できます。 たとえば、使用、 [Facebook コネクタ](app-service-logic-connector-facebook.md) 、ワークフロー、Slack チャネルにメッセージを投稿します。 

## トリガーとアクション
*トリガー* は、発生するイベントです。 たとえば、注文が更新された、新しい顧客が追加された、といったイベントがあります。  *アクション* トリガーの結果を示します。 たとえば、注文が更新されたときに、営業担当者にアラートを送信します。 または、新しい顧客が追加されたときに、ウェルカム メールを新しい顧客に送信します。 

Slack コネクタは、ロジック アプリでアクションとして使用でき、JSON 形式と XML 形式のデータをサポートします。 現時点では、Slack コネクタには使用可能なトリガーはありません。 

Slack コネクタでは、次のトリガーとアクションを使用できます。 

トリガー | アクション
--- | ---
なし | Post Message

## Slack コネクタの作成
コネクタは、ロジック アプリ内で作成することも、Azure Marketplace から直接作成することもできます。 Marketplace からコネクタを作成するには、次の操作を実行します。 

1. Azure のスタート画面では、次のように選択します。 **Marketplace**します。
2. 選択 **API Apps** 「Slack コネクタ」を検索します。
3. 名前、App Service プラン、その他のプロパティを入力します。  
![][1] 

4. クリックして **作成**します。 

## ロジック アプリのアクションとしてのコネクタの使用

> [AZURE.IMPORTANT] コネクタとロジック アプリは、同じリソース グループに常に作成する必要があります。 

Slack コネクタが作成されると、ロジック アプリにアクションとして追加できます。 

1.  ロジック アプリ内で開く **トリガーとアクション**します。  
    [新しいロジック アプリを作成します。](app-service-logic-create-a-logic-app.md)

2.  Slack コネクタが右側のギャラリーに表示されます。  
![][2]

3.  作成済みの Slack コネクタを選択すると、ロジック アプリに自動的に追加されます。 
4.  選択 **承認**します。 Slack アカウントにサインインします。 最後に、Slack アカウントにアクセスする権限をコネクタに付与するように求められます。 選択 **承認**:  
![][3]  
![][4]  
![][5]  
![][6]  
    
5.  これで、フローで Slack コネクタを使用できるようになりました。 メッセージの投稿アクション提供されています。  
![][7]


[メッセージの投稿] の使い方を簡単に見てみましょう。 このアクションを使用すると、任意の Slack チャネルにメッセージを投稿することができます。  
![][8]

"Post Message" アクションの入力プロパティを次のように構成します。

プロパティ | 説明
--- | ---
テキスト | 投稿するメッセージのテキストを入力します。
チャネル名 | このメッセージが投稿される Slack チャネルを入力します。 チャネルが入力されていない場合は、メッセージが #general に投稿されます。
高度なプロパティ | **Bot ユーザー名**: このメッセージに使用する bot の名前。 これが入力されていない場合、メッセージは "Bot" として投稿されます。<p><p>**Icon URL**: このメッセージのアイコンとして使用される画像を示す URL です。<p><p>**Icon Emoji**: このメッセージのアイコンとして使用される絵文字です。 このプロパティは、アイコンの URL プロパティを上書きします。


Slack コネクタでは REST API が使用できるため、ロジック アプリの外部で Slack コネクタを使用できます。 Slack コネクタを開き、選択 **API の定義**:  
![][9]


## コネクタでできること
コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。


<!-- Image reference -->
[1]: ./media/app-service-logic-connector-slack/img1.PNG
[2]: ./media/app-service-logic-connector-slack/img2.PNG
[3]: ./media/app-service-logic-connector-slack/img3.PNG
[4]: ./media/app-service-logic-connector-slack/img4.PNG
[5]: ./media/app-service-logic-connector-slack/img5.PNG
[6]: ./media/app-service-logic-connector-slack/img6.PNG
[7]: ./media/app-service-logic-connector-slack/img7.PNG
[8]: ./media/app-service-logic-connector-slack/img8.PNG
[9]: ./media/app-service-logic-connector-slack/img9.PNG

