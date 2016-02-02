<properties
   pageTitle="Logic Apps での Chatter コネクタの使用 | Microsoft Azure App Service"
   description="Chatter コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法"
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



# Chatter コネクタの使用開始とロジック アプリへの追加

Chatter に接続して、メッセージの投稿またはフィードの検索を実行します。 たとえば、Chatter フィードを検索して、特定の項目が見つかったときに、Sales グループにその Chatter メッセージを投稿できます。

この Chatter コネクタをビジネス ワークフローに追加し、ロジック アプリ内のそのワークフローの一部としてデータを処理できます。

## トリガーとアクション

トリガーは、新しい Chatter メッセージの着信など、特定のイベントに応じて新しいインスタンスを起動します。 アクションは、結果のような新しい Chatter メッセージを受け取った後し、もう 1 つの Chatter グループ、または Facebook や Twitter などの別のソーシャル メディア サイトに、メッセージを投稿します。

Chatter コネクタは、ロジック アプリでトリガーまたはアクションとして使用でき、JSON 形式と XML 形式のデータをサポートします。 Chatter コネクタでは、次のトリガーとアクションを使用できます。

 トリガー| アクション
--- | ---
 新しいメッセージ| <ul><li>メッセージを投稿</li><li>検索</li></ul>


## ロジック アプリ用の Chatter コネクタを作成する

コネクタは、ロジック アプリ内で作成することも、Azure Marketplace から直接作成することもできます。 Marketplace からコネクタを作成するには、次の操作を実行します。

1. Azure のスタート画面で、**[Marketplace]** を選択します。
2. "Chatter コネクタ" を検索して選択し、**[作成]** を選択します。
3. 名前、App Service プラン、その他のプロパティを入力します。  
    ![][1]
    - **[場所]** - コネクタをデプロイする地域を選択します。
    - **[サブスクリプション]** - コネクタを作成するサブスクリプションを選択します。
    - **[リソース グループ]** - コネクタを格納するリソース グループを選択するか、作成します。
    - **[Web ホスティング プラン]** - Web ホスティング プランを選択するか、作成します。
    - **[価格レベル]** - コネクタの価格レベルを選択します。
    - **[名前]** - Chatter コネクタの名前を指定します。

4. **[作成]** を選択します。


## ロジック アプリで Chatter コネクタを使用する

API アプリが作成されたら、ロジック アプリのトリガーやアクションとして Chatter コネクタを使用できます。 これを行うには、次の手順を実行します。

1. ロジック アプリで **[トリガーとアクション]** を選択してロジック アプリ デザイナーを開き、フローを構成します。

2. Chatter コネクタは、ギャラリーにリストされます。  
    ![][4]
3. Chatter コネクタを選択すると、自動的にデザイナーに追加されます。 選択 **Authorize**, 、資格情報を入力し、選択 **許可**:  
    ![][5]
    ![][6]
    ![][7]

これで、フローで Chatter コネクタを使用できるようになりました。 今後は、Chatter トリガー ([新しいメッセージ]) を使って取得した新しいメッセージを、フローの他のアクションで使用できます。 Chatter トリガーの入力プロパティを次のように構成します。

**[グループ ID]** - 新しいメッセージを取得する必要があるグループの ID を入力します。 グループ ID が指定されていない場合、ユーザーのフィードから新しいメッセージを取得します。  
    ![][8]
    ![][9]


同様に、[Post Message (メッセージを投稿)] アクションを選択すると、フローの Chatter アクションを使用してメッセージを投稿できます。 [Post Message (メッセージを投稿)] アクションの入力プロパティを次のように構成します。  
    - **メッセージ テキスト** -投稿するメッセージのテキストの内容
    - **グループ ID** -新しいメッセージを投稿する必要があるグループの ID を指定します。 グループ ID を指定しない場合は、メッセージがユーザーのフィードに投稿されます。
    -   **ファイル名** -このメッセージに添付されるファイルの名前
    -   **コンテンツ データ** -添付ファイルのコンテンツ データ
    -   **コンテンツ タイプの** -添付ファイルのコンテンツの種類
    -   **コンテンツ転送エンコード** -添付ファイルのコンテンツ転送エンコード ("none"|"base64")
    -   **メンション** -ユーザー名の配列にこのメッセージにタグ付け
    -   **ハッシュタグ** -メッセージと共に投稿されるハッシュタグの配列

![][10]
![][11]

## コネクタでできること

コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 を参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md).
>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。




[1]: ./media/app-service-logic-connector-chatter/img1.PNG 
[2]: ./media/app-service-logic-connector-chatter/img2.PNG 
[3]: ./media/app-service-logic-connector-chatter/img3.png 
[4]: ./media/app-service-logic-connector-chatter/img4.png 
[5]: ./media/app-service-logic-connector-chatter/img5.PNG 
[6]: ./media/app-service-logic-connector-chatter/img6.PNG 
[7]: ./media/app-service-logic-connector-chatter/img7.PNG 
[8]: ./media/app-service-logic-connector-chatter/img8.PNG 
[9]: ./media/app-service-logic-connector-chatter/img9.PNG 
[10]: ./media/app-service-logic-connector-chatter/img10.PNG 
[11]: ./media/app-service-logic-connector-chatter/img11.PNG 

