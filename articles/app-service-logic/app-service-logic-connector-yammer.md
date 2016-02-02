<properties
   pageTitle="Logic Apps での Yammer コネクタの使用 | Microsoft Azure App Service"
   description="Yammer コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="11/30/2015"
   ms.author="sameerch"/>



# ロジック アプリで Yammer コネクタを使用する

Yammer、メッセージの投稿アクション、新しいメッセージを取得するトリガーに接続します。

ロジック アプリはさまざまなデータ ソースを基にトリガーでき、フローの一環としてデータの取得と処理のためのコネクタを提供します。

## Logic App 用の Yammer コネクタを作成する

Yammer コネクタを使用するには、まず Yammer コネクタ API アプリのインスタンスを作成する必要があります。 そのためには、次の手順に従います。

1. Azure のスタート画面で、**[Marketplace]** を選択します。
2. "Yammer コネクタ" を検索して選択し、**[作成]** を選択します。
3. 次のように、Yammer コネクタを構成します。  
![][1]

    - **[場所]** - コネクタをデプロイする地域を選択します。
    - **[サブスクリプション]** - コネクタを作成するサブスクリプションを選択します。
    - **[リソース グループ]** - コネクタを格納するリソース グループを選択するか、作成します。
    - **[App Service プラン]** - Web ホスティング プランを選択または作成します。
    - **[価格レベル]** - コネクタの価格レベルを選択します。
    - **[名前]** - Yammer コネクタの名前を指定します。

4. [作成] をクリックします。 新しい Yammer コネクタが作成されます。
5. API アプリのインスタンスを作成したら、Yammer コネクタを使用するロジック アプリを作成できます。

## Logic App で Yammer コネクタを使用する

API アプリを作成すると、Logic App のトリガーやアクションとして Yammer コネクタを使用できます。 そのためには、次の手順を実行する必要があります。

1.  新しいロジック アプリを作成します。  
![][2]

2.  [トリガーとアクション]、Logic Apps デザイナーを開き、フローを構成するを開きます。  
![][3]

3.  Yammer コネクタは、右側のギャラリーでの API Apps [このリソース グループ] セクションで表示されます。  
![][4]

4. [Yammer コネクタ] をクリックして、Yammer コネクタの API アプリをエディターにドロップできます。 [承認] ボタンをクリックします。 Yammer 資格情報を入力します。 許可する] をクリックします。  
![][5]  
![][6]  
![][7]

これで、フローで Yammer コネクタを使用できるようになりました。

## Yammer コネクタをトリガーとして使用する

今後は、Yammer トリガー ([新しいメッセージ]) を使って取得した新しいメッセージを、フローの他のアクションで使用できます。 Yammer トリガーの入力プロパティを次のように構成します。

- **グループ ID** - 新しいメッセージを取得する必要があるグループの ID。 グループ ID を指定しない場合は、メッセージが次のフィードから取得されます。 グループ ID は、Yammer のグループ URL から取得できます。

    例: 次の URL のグループ ID は"5453203 です"。  
https://www.yammer.com/microsoft.com/#/threads/inGroup?type=in_group&feedId=5453203  
![][8]  
![][9]

## Yammer コネクタを使用してメッセージを投稿する

Yammer コネクタは、ロジック アプリ内でアクションとして使用することもできます。 最初に、ロジック アプリのトリガーを指定するか、[このロジックを手動で実行］ をオンにします (下記参照)。 Yammer コネクタを追加し、必要に応じて承認し、[メッセージの投稿] アクションを選択します。 メッセージの投稿] アクションの入力プロパティをように構成します。

- **[メッセージ テキスト]** - 投稿するメッセージのテキスト コンテンツ。
- **グループ ID** - 新しいメッセージを投稿する必要があるグループの ID を指定します。 グループ ID を指定しない場合は、メッセージがすべての会社フィードに投稿されます。 グループ ID は、Yammer のグループ URL から取得できます。

    例: 次の URL のグループ ID は"5453203 です"。  
https://www.yammer.com/microsoft.com/#/threads/inGroup?type=in_group&feedId=5453203
-   **タグ ユーザー** -メッセージにタグ付けする必要があるユーザー ネットワーク名の配列。  
![][10]  
![][11]

## コネクタでできること

コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 を参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md).
>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。



[1]: ./media/app-service-logic-connector-yammer/img1.PNG 
[2]: ./media/app-service-logic-connector-yammer/img2.PNG 
[3]: ./media/app-service-logic-connector-yammer/img3.png 
[4]: ./media/app-service-logic-connector-yammer/img4.png 
[5]: ./media/app-service-logic-connector-yammer/img5.PNG 
[6]: ./media/app-service-logic-connector-yammer/img6.PNG 
[7]: ./media/app-service-logic-connector-yammer/img7.png 
[8]: ./media/app-service-logic-connector-yammer/img8.PNG 
[9]: ./media/app-service-logic-connector-yammer/img9.PNG 
[10]: ./media/app-service-logic-connector-yammer/img10.PNG 
[11]: ./media/app-service-logic-connector-yammer/img11.PNG 

