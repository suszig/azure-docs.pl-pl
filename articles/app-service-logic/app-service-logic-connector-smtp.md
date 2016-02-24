<properties
   pageTitle="Logic Apps での SMTP コネクタの使用 | Microsoft Azure App Service"
   description="SMTP コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="11/11/2015"
   ms.author="rajram"/>


# SMTP コネクタの使用開始とロジック アプリへの追加
SMTP サーバーに接続して、電子メール (添付ファイルを含むメールを含む) を送信します。 SMTP コネクタの [電子メールの送信] アクションを使用すると、指定されたメール アドレスにメールを送信できます。

ロジック アプリはさまざまなデータ ソースを基にトリガーでき、ワークフローの一環としてデータの取得と処理のためのコネクタを提供します。 この SMTP コネクタをビジネス ワークフローに追加し、ロジック アプリ内のそのワークフローの一部としてデータを処理できます。 


## トリガーとアクション
*トリガー* は、発生するイベントです。 たとえば、注文が更新された、新しい顧客が追加された、といったイベントがあります。  *アクション* トリガーの結果を示します。 たとえば、注文が更新されたり、新しい顧客が追加されたりすると、その新しい顧客に電子メールを送信します。

SMTP コネクタは、ロジック アプリでトリガーまたはアクションとして使用でき、JSON 形式と XML 形式のデータをサポートします。 現時点では、このコネクタにはトリガーはありません。

SMTP コネクタでは、次のトリガーとアクションを使用できます。

トリガー | アクション
--- | ---
なし | 電子メールの送信


## SMTP コネクタの作成
コネクタは、ロジック アプリ内で作成することも、Azure Marketplace から直接作成することもできます。 Marketplace からコネクタを作成するには、次の操作を実行します。  

1. Azure のスタート画面では、次のように選択します。 **Marketplace**します。
2. 選択 **API Apps** 「SMTP コネクタ」を検索します。
3. **作成** コネクタです。
4. 名前、App Service プラン、その他のプロパティを入力します。
5. 次のパッケージの設定を入力します。

    名前 | 必須 |  説明
    --- | --- | ---
    ユーザー名 | あり | SMTP サーバーに接続できるユーザー名を入力します。
    パスワード | あり | ユーザー名のパスワードを入力します。
    サーバー アドレス | あり | SMTP サーバーの名前または IP アドレスを入力します。
    サーバー ポート | あり | SMTP サーバーのポート番号を入力します。
    SSL を有効にする | いいえ | 入力 *true* セキュリティで保護された SSL/TLS チャネルで SMTP を使用します。

6. 選択 **作成**します。

> [AZURE.IMPORTANT] 一部の SMTP サーバー (SendGrid および gmail など) に、このコネクタがどのように動作するかの問題があります。  SendGrid からメールを送信する場合、 [GitHub リポジトリ](https://github.com/logicappsio/SendGridAPI) SendGrid Api で直接が連動するカスタム API を持ちます。

## ロジック アプリで SMTP コネクタを使用する
コネクタが作成されたら、ロジック アプリのアクションとして SMTP コネクタを使用できます。 これを行うには、次の手順を実行します。

1.  新しいロジック アプリを作成します。  
![][2]
2.  開く **トリガーとアクション** を Logic Apps デザイナーを開き、ワークフローを構成します。  
![][3]
3.  SMTP コネクタが右側のギャラリーの [このリソース グループの API Apps] セクションに表示されます。 それを選択します。  
![][4]
4.  SMTP コネクタを選択すると、ワークフロー デザイナーに自動的に追加されます。

これで、SMTP コネクタを構成して、ワークフローで使用できます。 選択、 **電子メールの送信** アクションと、入力プロパティを構成します。

    Property | Description
    --- | ---
    To | Enter the email address of recipient(s). Separate multiple email addresses using a semicolon (;). For example, enter: *recipient1@domain.com;recipient2@domain.com*.
    Cc | Enter the email address of the carbon copy recipient(s). Separate multiple email addresses using a semicolon (;). For example, enter: *recipient1@domain.com;recipient2@domain.com*.
    Subject | Enter the subject of the email.
    Body | Enter body of the email.
    Is HTML | When this property is set to true, the contents of the body are sent as HTML.
    Bcc | Enter the email address of recipient(s) for blind carbon copy. Separate multiple email addresses using a semicolon (;). For example, enter: *recipient1@domain.com;recipient2@domain.com*.
    Importance | Enter the Importance of the email. The options are Normal, Low, and High.
    Attachments | Attachments to be sent along with the email. It contains the following fields: <ul><li>Content (String)</li><li>Content transfer Encoding (Enum) (“none”|”base64”)</li><li>Content Type (String)</li><li>Content ID (String)</li><li>File Name (String)</li></ul>

![][5]  
![][6]

## コネクタでできること
コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。

<!--Image references-->
[1]: ./media/app-service-logic-connector-smtp/img1.PNG
[2]: ./media/app-service-logic-connector-smtp/img2.PNG
[3]: ./media/app-service-logic-connector-smtp/img3.png
[4]: ./media/app-service-logic-connector-smtp/img4.PNG
[5]: ./media/app-service-logic-connector-smtp/img5.PNG
[6]: ./media/app-service-logic-connector-smtp/img6.PNG

