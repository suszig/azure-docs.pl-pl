<properties
   pageTitle="Logic Apps での Office 365 コネクタの使用 | Microsoft Azure App Service"
   description="Office 365 コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法"
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



# Office 365 コネクタの使用開始とロジック アプリへの追加

Office 365 アカウントに接続して、電子メールの送受信、および予定表と連絡先の管理を実行できます。 電子メールの送受信や取得、予定表のイベントの作成や削除、連絡先の作成、更新、取得や削除など、さまざまなアクションを実行できます。

ロジック アプリはさまざまなデータ ソースを基にトリガーでき、フローの一環としてデータの取得と処理のためのコネクタを提供します。 この Office 365 コネクタをビジネス ワークフローに追加し、ロジック アプリ内のそのワークフローの一部としてデータを処理できます。

**基本的なアクション**

- 新しい電子メール (トリガー)
- メールの送信
- メールへの返信
- 送信イベント
- 連絡先の追加

## O365 コネクタの API アプリの作成

コネクタは、ロジック アプリ内で作成することも、Azure Marketplace から直接作成することもできます。 Marketplace からコネクタを作成するには、次の操作を実行します。

1. Azure のスタート画面で、**[Marketplace]** を選択します。
2. "Office 365 コネクタ" を検索して選択し、**[作成]** を選択します。
3.  リソース グループにホスティング プランの詳細を提供する API アプリの名前を選択して、Office 365 コネクタを構成します。  
![][21]


## ロジック アプリの作成

ここでは、電子メールを受信したときに起動する簡単なロジック アプリを作成します (受信先は、sales@contoso.com などの営業用の問い合わせ電子メール ID とします)。 また、このロジック アプリでは、イベントの作成、差出人の詳細を含めた連絡先の追加、個人用アカウントへの電子メールの送信を行い、最後に、受信を確認したことを知らせる返信を送信します。

1.  Azure ポータルにサインインして、[新規]、[Web + モバイル]、[ロジック アプリ] をクリックします。  
![][1]

2.  ロジック アプリの作成] ページで、名前、app service プラン、場所など、必要な情報を提供します。  
![][2]

3.  トリガーとアクション] をクリックし、ロジック アプリ エディターが開きます。  
![][3]

4.  フローに追加するギャラリー内の [このリソース グループの API Apps] セクションから Office 365 のトリガーを選択します。  
![][4]

6.  Office 365 に接続するには、アカウントにアクセスできるようにロジック アプリを承認する必要があります。 Office 365 のログイン資格情報を提供するには、[承認] をクリックします。  
![][5]

7.  Office 365 サインイン ページにリダイレクトされ、Office 365 アカウントの資格情報を使って認証することができます。  
![][6]  
![][7]

8.  承認が完了する、Office 365 トリガーが表示されます。  
![][8]

9.  "新しい電子メール" トリガーを選択すると、入力パラメーターが表示されます。

10. トリガーの頻度を [分] を変更し、[✓] をクリックします。  
![][9]

11. Office 365 の「新しい電子メール」トリガーが構成され、出力パラメーターも表示を確認できます。  
![][10]

12. ギャラリーの [最近使用した項目] セクションから [Office 365 コネクタ] を選択すると、新しい "Office 365" アクションが追加されます。

13. アクションと「送信イベント」アクションが表示の入力パラメーターの一覧から、「送信イベント」を選択します。  
![][11]

14. イベントの詳細を指定し、[✓] をクリックします。  
![][12]

15. ギャラリーの [最近使用した項目] セクションから [Office 365 コネクタ] を選択すると、新しい "Office 365" アクションが追加されます。

16. アクションと連絡先の追加の操作が表示されますの入力パラメーターの一覧から [連絡先の追加を選択します。  
![][13]

17. [電子メール アドレス] フィールドの横に '+] をクリックし、トリガーから 'From' 出力フィールドの値を選択します。  
![][14]

18. [✓] をクリックし、アクションの構成が完了しました。  
![][15]

19. ギャラリーの [最近使用した項目] セクションから [Office 365 コネクタ] を選択すると、新しい "Office 365" アクションが追加されます。

20. アクションと電子メールの送信の操作が表示されますの入力パラメーターの一覧から、電子メールの送信を選択します。  
![][19]

21. 電子メールの送信に必要な詳細を入力します。 以下のような文字列を入力すると、メッセージを作成できます。 "電子メールの送信" アクションが構成されたら、[✓] をクリックします:

        Body - @concat('You got a new sales enquiry from',triggers().output.body.From)

    ![][20]
22. ギャラリーの [最近使用した項目] セクションから [Office 365 コネクタ] を選択すると、新しい "Office 365" アクションが追加されます。

23. アクションと '返信先' の操作が表示されますの入力パラメーターの一覧からは、返信を選択します。  
![][16]

24. クリックして ' +' 'から' の横にあるフィールドとトリガーの出力から値を選択してメッセージ id で、[✓] をクリックします。  
![][17]

25. ロジック アプリ エディターの画面で [OK] をクリックし、[作成] をクリックします。 作成が完了するまで約 30 秒かかります。

26. トリガーを構成したアカウントに電子メールを送信すると、個人用のメール アカウントに電子メール、Office のメール アカウントに予定表のイベントおよび連絡先が表示されます。 また、営業の問い合わせに対してすぐに回答することを知らせる返信も受け取ります。

## コネクタでできること

コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 を参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md).
> [AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。



[1]: ./media/app-service-logic-connector-office365/1_New_Logic_App.png 
[2]: ./media/app-service-logic-connector-office365/2_Logic_App_Settings.png 
[3]: ./media/app-service-logic-connector-office365/3_Logic_App_Editor.png 
[4]: ./media/app-service-logic-connector-office365/4_Select_Office365_Gallery.png 
[5]: ./media/app-service-logic-connector-office365/5_Office365_Authorize.png 
[6]: ./media/app-service-logic-connector-office365/6_Office365_Login.png 
[7]: ./media/app-service-logic-connector-office365/7_Office365_User_Consent.png 
[8]: ./media/app-service-logic-connector-office365/8_Office365_Trigger.png 
[9]: ./media/app-service-logic-connector-office365/9_Office365_Trigger_Settings.png 
[10]: ./media/app-service-logic-connector-office365/10_Office365_Trigger_Configured.png 
[11]: ./media/app-service-logic-connector-office365/11_Office365_Actions_List.png 
[12]: ./media/app-service-logic-connector-office365/12_Office365_Create_Event_Inputs.png 
[13]: ./media/app-service-logic-connector-office365/13_Office365_Add_Contact_Inputs.png 
[14]: ./media/app-service-logic-connector-office365/14_Office365_Add_Contact_Email_FromTrigger.png 
[15]: ./media/app-service-logic-connector-office365/15_Office365_Add_Contacts_Configured.png 
[16]: ./media/app-service-logic-connector-office365/16_Office365_Reply_To_Inputs.png 
[17]: ./media/app-service-logic-connector-office365/17_Office365_Reply_To_MessageId.png 
[18]: ./media/app-service-logic-connector-office365/18_Office365_Reply_To_Configured.png 
[19]: ./media/app-service-logic-connector-office365/19_Office365_Send_Inputs.png 
[20]: ./media/app-service-logic-connector-office365/20_Office365_Send_Configured.png 
[21]: ./media/app-service-logic-connector-office365/21-create-new-o365-api-app.png 

