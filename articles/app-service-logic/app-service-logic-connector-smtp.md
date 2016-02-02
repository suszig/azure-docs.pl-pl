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

*トリガー*とは、発生するイベントを指します。 たとえば、注文が更新された、新しい顧客が追加された、といったイベントがあります。 *アクション*は、トリガーの結果です。 たとえば、注文が更新されたり、新しい顧客が追加されたりすると、その新しい顧客に電子メールを送信します。

SMTP コネクタは、ロジック アプリでトリガーまたはアクションとして使用でき、JSON 形式と XML 形式のデータをサポートします。 現時点では、このコネクタにはトリガーはありません。

SMTP コネクタでは、次のトリガーとアクションを使用できます。

 トリガー| アクション
--- | ---
 なし| 電子メールの送信


## SMTP コネクタの作成

コネクタは、ロジック アプリ内で作成することも、Azure Marketplace から直接作成することもできます。 Marketplace からコネクタを作成するには、次の操作を実行します。

1. Azure のスタート画面で、**[Marketplace]** を選択します。
2. **[API Apps]** を選択し、"SMTP コネクタ" を検索します。
3. コネクタを**作成**します。
4. 名前、App Service プラン、その他のプロパティを入力します。
5. 次のパッケージの設定を入力します。

    名前| 必須| 説明
   --- | --- | ---
    ユーザー名| あり| SMTP サーバーに接続できるユーザー名を入力します。
    パスワード| あり| ユーザー名のパスワードを入力します。
    サーバー アドレス| あり| SMTP サーバーの名前または IP アドレスを入力します。
    サーバー ポート| あり| SMTP サーバーのポート番号を入力します。
    SSL を有効にする| いいえ| *true* を入力すると、セキュリティで保護された SSL/TLS チャネルで SMTP を使用します。

6. **[作成]** を選択します。

> [AZURE.IMPORTANT] SMTP サーバーによっては、このコネクタの動作方法によって問題が発生する場合があります (SendGrid および Gmail)。 SendGrid からメールを送信する場合、 [GitHub リポジトリ](https://github.com/logicappsio/SendGridAPI) SendGrid Api で直接が連動するカスタム API を持ちます。

## ロジック アプリで SMTP コネクタを使用する

コネクタが作成されたら、ロジック アプリのアクションとして SMTP コネクタを使用できます。 これを行うには、次の手順を実行します。

1.  新しいロジック アプリを作成します。  
![][2]
2.  **[トリガーとアクション]** を開き、Logic Apps デザイナーを開いてワークフローを構成します。  
![][3]
3.  SMTP コネクタが右側のギャラリーの [このリソース グループの API Apps] セクションに表示されます。 それを選択します。  
![][4]
4.  SMTP コネクタを選択すると、ワークフロー デザイナーに自動的に追加されます。

これで、SMTP コネクタを構成して、ワークフローで使用できます。 **Send Email** アクションを選択し、入力プロパティを次のように構成します。

 プロパティ| 説明
--- | ---
 To| 受信者のメール アドレスを入力します。セミコロン (;) を使用して複数のメール アドレスを区切ります。たとえば、入力: *recipient1@domain.com;recipient2@domain.com*します。
 [Cc]| カーボン コピーの受信者のメール アドレスを入力します。セミコロン (;) を使用して複数のメール アドレスを区切ります。たとえば、入力: *recipient1@domain.com;recipient2@domain.com*します。
 [件名]| メールの件名を入力します。
 本文| メールの本文を入力します。
 [Is HTML]| このプロパティが true に設定されている場合は、本文の内容は HTML として送信されます。
 [Bcc]| ブラインド カーボン コピーの受信者のメール アドレスを入力します。セミコロン (;) を使用して複数のメール アドレスを区切ります。たとえば、入力: *recipient1@domain.com;recipient2@domain.com*します。
 [重要度]| メールの重要度を入力します。[標準]、[低]、[高] から選択できます。
 [添付ファイル]| メールと共に送信される添付ファイル。次のフィールドが含まれています: <ul><li>コンテンツ (文字列)</li><li>コンテンツ転送エンコード (「なし」(列挙)| "base64")</li><li>コンテンツの種類 (文字列)</li><li>コンテンツ ID (文字列)</li><li>ファイル名 (文字列)</li></ul>

![][5]  
![][6]

## コネクタでできること

コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 を参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md).
>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。



[1]: ./media/app-service-logic-connector-smtp/img1.PNG 
[2]: ./media/app-service-logic-connector-smtp/img2.PNG 
[3]: ./media/app-service-logic-connector-smtp/img3.png 
[4]: ./media/app-service-logic-connector-smtp/img4.PNG 
[5]: ./media/app-service-logic-connector-smtp/img5.PNG 
[6]: ./media/app-service-logic-connector-smtp/img6.PNG 

