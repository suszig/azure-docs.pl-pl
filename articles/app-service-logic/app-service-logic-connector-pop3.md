<properties
   pageTitle="Logic Apps での POP3 コネクタの使用 | Microsoft Azure App Service"
   description="POP3 コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法"
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


# POP3 コネクタの使用開始とロジック アプリへの追加
POP3 サーバーに接続して、電子メール (添付ファイルを含むメールを含む) を取得します。

ロジック アプリはさまざまなデータ ソースを基にトリガーでき、フローの一環としてデータの取得と処理のためのコネクタを提供します。 この POP3 コネクタをビジネス ワークフローに追加し、ロジック アプリ内のそのワークフローの一部としてデータを処理できます。 


## Logic App 用の POP3 コネクタを作成する ##
POP3 コネクタを使用するには、まず POP3 コネクタ API アプリのインスタンスを作成する必要があります。 これは、ロジック アプリ デザイナーから直接作成することも、ロジック アプリ デザイナーの外から作成することも実行できます。 デザイナーの外からインスタンスを作成する場合の手順は次のとおりです。

1.  Azure ポータルのホーム ページから Azure Marketplace を開きます。
2.  [すべて] の下で "POP3 コネクタ" を検索します。
3.  次のように、POP3 コネクタを構成します。

    ![][1]
    - **場所** -コネクタをデプロイするように、地理的な場所を選択します
    - **サブスクリプション** -を作成するには、このコネクタを使用したサブスクリプションを選択します。
    - **リソース グループ** - 選択するか、コネクタが属するリソース グループの作成
    - **Web ホスティング プラン** - 選択するか、web ホスティング プランの作成
    - **価格レベル** -コネクタの価格レベルを選択します
    - **名前** -POP3 コネクタの名前を指定
    - **[パッケージ設定]**
        - **ユーザー名** POP3 サーバーに接続するユーザー名を指定
        - **パスワード** POP3 サーバーに接続するためのパスワードを指定します。
        - **サーバーのアドレス** POP3 サーバーの名前または IP アドレス指定
        - **サーバー ポート** POP3 サーバーのポート番号を指定します。
        - **SSL を有効にする** セキュリティで保護された SSL/TLS チャネルで POP3 を使用する場合は true を指定してください。
4.  [作成] をクリックします。 新しい POP3 コネクタが作成されます。
5.  API アプリのインスタンスが作成されたら、同じリソース グループ内に、この POP3 コネクタを使用するロジック アプリを作成できます。

## Logic App で POP3 コネクタを使用する ##
API アプリが作成されたら、Logic App のトリガーとして POP3 コネクタを使用できます。 そのためには、次の手順を実行する必要があります。

1.  新しい Logic App を作成し、POP3 コネクタと同じリソース グループを選択します。

    ![][2]
2.  [トリガーとアクション] を選択して Logic Apps デザイナーを開き、フローを構成します。

    ![][3]
3.  POP3 コネクタが右側のギャラリーの [API Apps in this resource group (このリソース グループの API Apps)] セクションに表示されます。 それを選択します。

    ![][4]
4.  [POP3 コネクタ] をクリックして、POP3 コネクタの API アプリをエディターにドロップできます。

5.  これで、フローで POP3 コネクタを使用できるようになりました。 [Get Email (メールの取得)] トリガーを選択し、頻度と間隔を構成します。 今後は、POP3 トリガーを使って取得したメールを、フローの他のアクションで使用できます。
         

    ![][5]
    ![][6]

## コネクタでできること
コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。


<!--Image references-->
[1]: ./media/app-service-logic-connector-pop3/img1.PNG
[2]: ./media/app-service-logic-connector-pop3/img2.PNG
[3]: ./media/app-service-logic-connector-pop3/img3.png
[4]: ./media/app-service-logic-connector-pop3/img4.PNG
[5]: ./media/app-service-logic-connector-pop3/img5.PNG
[6]: ./media/app-service-logic-connector-pop3/img6.PNG

