<properties 
   pageTitle="Azure モバイル エンゲージメント ユーザー インターフェイス - Reach (リーチ) 方法"
   description="Azure モバイル エンゲージメントのユーザー インターフェイスの概要" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="05/12/2015"
   ms.author="piyushjo"/>


# エンドユーザーに通知するためのプッシュの利用と管理を始める方法

SDK とアプリを完全統合したら、UI のリーチ セクションでアプリのユーザーにプッシュ通知を実行できます。

## 最初のプッシュ通知キャンペーンの実行

-    Confirm that your Reach is integrated into your app with the SDK. 

-    Select your application


![First1][1]

-    Go to the "Reach" Section and Click "New announcement"


![First2][2]

-    Create a new campaign and name it

 ![First3][3]

-    Select how the notification should be delivered, as In-app only


![First4][4]

-    Create the message you want to push


![First5][5]

-    You may write a title on the notification (Optional).

-    Write push message content.

-    You can upload an image. Be aware that the size of the file cannot exceed 32,768 bytes.

-    You also have the ability to select further options, but for the focus of this tutorial, we will see that later.

-    Select the content type as Notification only


![First6][6]

-    Create your push campaign and it will appear in your campaign list.


![First7][7]

## プッシュ通知キャンペーンのテスト

![Test1][8]

-    Register your device.

-    Click on the checkbox of the device you want to push.

-    Click on the "Test" button to send the push to the device.


![Test2][9]

-    Activate the campaign


![Test3][10]

-    Now that you have created your campaign you just need to activate it for the notification to be pushed to your users.


## 個人用に設定されたプッシュの送信

-    This example creates a push where a custom rebate code is entered into the push notification.


![Personalize1][11]

個人用設定は、アプリ情報タグからマーカーを置き換えることで動作するため、最初にユーザーが適切なアプリ情報を定義したことを確認する必要があります。 この例では、対象とするユーザーには、rebate_code という名前のアプリ情報タグが定義されています。
上記のように、プッシュ通知の内容には、マーカー ${rebate_code} が含まれます。これは、アプリ情報タグの実際の内容で置き換えられることを示します。

> 警告: アプリ情報タグがユーザーに対して定義されていない場合、ユーザーはプッシュを受け取りません。

-    Result


![Personalize2][12]

### 通知のテキストのさらなるカスタマイズが可能

![Personalize3][13]

-    Including the title of the notification,

-    And the content of the message.

-    Choose the type of announcement (Text view or Web view)


![Personalize4][14]

### 次の情報でアナウンスメントの本文もカスタマイズ可能

-    The action URL, should you want to customize the landing page

-    The title,

-    The body of the message.



## プッシュ通知の区別 (アプリの内部または外部)

-    Choose the type of notification you will push, select your application, go to the "Reach" section, select or create a push campaign and go to the "Notification" section.

-    Click on the "delivery mode" you want.

-    Click on the "Restrict Activities" checkbox when you want the notification occurs on specific activities (screens).


![Differentiate1][15]

### [アプリ外のみ] 配信モード

![Differentiate2][16]

[アプリ外のみ] 配信モードは、アプリケーションが閉じている場合にプッシュ通知を提供します。 これは標準のプッシュ通知です。
[アプリ外のみ] を選択する場合は、アプリケーションをビルドしているプラットフォーム (APNS または GCM) から証明書を既に提供している必要があります。

### 関連項目

-  [Apple プッシュ通知サービス – 証明書](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), 、Google クラウド メッセージング-証明書] (http://developer.android.com/google/gcm/index.html)

### [アプリ内のみ] 配信モード

![Differentiate3][17]

[アプリ内のみ] 配信モードは、アプリケーションが実行中の場合にプッシュ通知を提供します。
この通知では、APNS および GCM のシステムを実行する必要はありません。
アプリ内配信システムを使用して、エンドユーザーに到達できます。
通知を完全にカスタマイズし、通知が表示されるアクティビティ (画面) を決定できます。

### [随時] 配信モード

[随時] 配信モードを選択すると、アプリケーションが実行されているかどうかにかかわらずエンドユーザーに到達することができます。
[常時] を選択する場合は、アプリケーションをビルドしているプラットフォーム (APNS または GCM) から証明書を既に提供している必要があります。

## プッシュ キャンペーンのスケジュール

### キャンペーン開始の計画

![Shedule1][18]

今日が 3 月 21 日で、アナウンスメントを 3 月 22 日の午前 0 時に行うことを計画しています。 
プッシュを行うために、インターフェイスの前にとどまっている必要はありません。 通知を送信する正確な時刻 (分単位) を事前に計画することができます。
-    Un-check the "None" checkbox and select a start time 

-    Choose the date and the time you want to start the push campaign.


### キャンペーン終了の計画

![Shedule2][19]

キャンペーンを 3 月 25 日の午後 3時 00 分に終了しますが、その時刻には不在になることがわかっています。
プッシュを行うために、インターフェイスの前にとどまっている必要はありません。 キャンペーンを終了する正確な時刻 (分単位) を事前に計画することができます。
-    Click on the "None" checkbox or select a end time

-    Choose the date and the time you want to finish the push campaign.


### 手動でのキャンペーンの終了

![Shedule3][20]

既定では、[なし] チェック ボックスがオンになっています。
つまり、キャンペーンは [リーチ] セクションでアクティブにするとすぐに開始し、[リーチ] セクションで終了したときに終了します。

> 注: 終了日を設定せずに作成されたキャンペーンは、プッシュをデバイス上にローカルに保存するため、手動でキャンペーンを終了させても、次回アプリを開いたときにそのプッシュを表示します。

## テキスト ビューでのプッシュ通知の強化

### テキスト ビューとは

![TextView1][21]

テキスト ビューとは、テキスト コンテンツを含むポップアップです。 このポップアップは、エンドユーザーがプッシュ通知をクリックした後に表示されます。
テキスト ビューでは、エンドユーザーにより多くの内容を表示することができます。 アプリのページへのジャンプ、ストアへのリダイレクト、Web ページを開く、電子メールの送信、地理的にローカライズされた検索の開始などのアクションの呼び出しを提示することもできます。

### 例: テキスト ビュー

-    Create your Push notification campaign in the "Reach" section and give your campaign a name


![TextView2][22]

-    Write the message that will appear on the notification.

-    Select the Announcement Content Type of “text”


![TextView3][23]

> 注: テキスト ビューをプッシュした場合は、常に通知が最初に表示されます。

- テキストを定義します (テキスト アナウンスメントの内容を選択した後、サブ セクションが表示され、表示するテキストを定義することができます)。

![TextView4][24]

-    Write the title that will appear at the top of the message.

-    Write the main content of the text view.

-    Write the content that will appear on the action button (an action button enables the application to make a specific action such as opening a page of the application, redirecting to an App store or any kind of sources you can provide).

-    Write the content that will appear on the exit button (by clicking on the exit button, the text view will disappear.)

-    Create your push notification campaign and it will appear on the campaign list.


![TextView5][25]

-    Activate your push notification campaign to send the text view to your users.


![TextView6][26]

-    Result


![TextView7][27]

-    The user receives the notification and click on it.

-    The text view appears as a pop-up allowing the user to interact with it.


## Web ビューでのプッシュ通知の強化

### Web ビューとは

![WebView1][28]

Web ビューとは、Web コンテンツを含むポップアップです。 このポップアップは、エンドユーザーがプッシュ通知をクリックしたときに表示されます。
Web ビューでは、エンドユーザーとの対話を増やすことができます。
アプリ ストアへのリダイレクト、Web ページを開く、電子メールの送信、地理的にローカライズされた検索の開始などのアクションの呼び出しを提示することもできます。

### 例: Web ビュー

-    Create your Push campaign in the "Reach" section and give your campaign a name.


![WebView2][29]

-    Write the message that will appear on the notification.

-    Select the Announcement Content Type as “web”


![WebView3][30]

### アナウンスメントの種類について:

- 通知のみ: シンプルな標準的な通知です。 ユーザーがクリックすると、他のビューが表示されず、操作に関連付けられた内容のみが発生します。
- テキストのアナウンス: ユーザーにテキスト ビューを閲覧するよう促す通知です。
- Web のアナウンス:  ユーザーに Web ビューを閲覧するよう促す通知です。
「Web アナウンスメント」の内容を選択します。

> 注: Web ビューをプッシュした場合は、常に通知が最初に表示されます。

- Web コンテンツを定義します (Web アナウンスメントの内容を選択した後、サブ セクションが表示され、表示する Web ビューのコンテンツを定義することができます)。


![WebView4][31]

-    Write the title that will appear at the top of the message (optional).

-    Write your HTML code here.

-    Click on the source editing mode button to switch edition and see how it looks like.

-    Write the content that will appear on the action button (an action button enables the application to make a specific action such as opening a page of the application, redirecting to a Store or any kind of sources you can provide).

-    Write the content that will appear on the exit button (by clicking on the exit button, the web view will disappear).

-    Result


![WebView5][32]

-    The user receive the notification and click on it.

-    The text view appears as a pop-up allowing the user to interact with it.





[1]: ./media/mobile-engagement-how-tos/First1.png 
[2]: ./media/mobile-engagement-how-tos/First2.png 
[3]: ./media/mobile-engagement-how-tos/First3.png 
[4]: ./media/mobile-engagement-how-tos/First4.png 
[5]: ./media/mobile-engagement-how-tos/First5.png 
[6]: ./media/mobile-engagement-how-tos/First6.png 
[7]: ./media/mobile-engagement-how-tos/First7.png 
[8]: ./media/mobile-engagement-how-tos/Test1.png 
[9]: ./media/mobile-engagement-how-tos/Test2.png 
[10]: ./media/mobile-engagement-how-tos/Test3.png 
[11]: ./media/mobile-engagement-how-tos/Personalize1.png 
[12]: ./media/mobile-engagement-how-tos/Personalize2.png 
[13]: ./media/mobile-engagement-how-tos/Personalize3.png 
[14]: ./media/mobile-engagement-how-tos/Personalize4.png 
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png 
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png 
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png 
[18]: ./media/mobile-engagement-how-tos/Schedule1.png 
[19]: ./media/mobile-engagement-how-tos/Schedule2.png 
[20]: ./media/mobile-engagement-how-tos/Schedule3.png 
[21]: ./media/mobile-engagement-how-tos/TextView1.png 
[22]: ./media/mobile-engagement-how-tos/TextView2.png 
[23]: ./media/mobile-engagement-how-tos/TextView3.png 
[24]: ./media/mobile-engagement-how-tos/TextView4.png 
[25]: ./media/mobile-engagement-how-tos/TextView5.png 
[26]: ./media/mobile-engagement-how-tos/TextView6.png 
[27]: ./media/mobile-engagement-how-tos/TextView7.png 
[28]: ./media/mobile-engagement-how-tos/WebView1.png 
[29]: ./media/mobile-engagement-how-tos/WebView2.png 
[30]: ./media/mobile-engagement-how-tos/WebView3.png 
[31]: ./media/mobile-engagement-how-tos/WebView4.png 
[32]: ./media/mobile-engagement-how-tos/WebView5.png 
[link 1]: mobile-engagement-user-interface.md 
[link 2]: mobile-engagement-troubleshooting-guide.md 
[link 3]: mobile-engagement-how-tos.md 
[link 4]: http://go.microsoft.com/fwlink/?LinkID=525553 
[link 5]: http://go.microsoft.com/fwlink/?LinkID=525554 
[link 6]: http://go.microsoft.com/fwlink/?LinkId=525555 
[link 7]: https://account.windowsazure.com/PreviewFeatures 
[link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement 
[link 9]: http://azure.microsoft.com/services/mobile-engagement/ 
[link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/ 
[link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/ 
[link 12]: mobile-engagement-user-interface-navigation.md 
[link 13]: mobile-engagement-user-interface-home.md 
[link 14]: mobile-engagement-user-interface-my-account.md 
[link 15]: mobile-engagement-user-interface-analytics.md 
[link 16]: mobile-engagement-user-interface-monitor.md 
[link 17]: mobile-engagement-user-interface-reach.md 
[link 18]: mobile-engagement-user-interface-segments.md 
[link 19]: mobile-engagement-user-interface-dashboard.md 
[link 20]: mobile-engagement-user-interface-settings.md 
[link 21]: mobile-engagement-troubleshooting-guide-analytics.md 
[link 22]: mobile-engagement-troubleshooting-guide-apis.md 
[link 23]: mobile-engagement-troubleshooting-guide-push-reach.md 
[link 24]: mobile-engagement-troubleshooting-guide-service.md 
[link 25]: mobile-engagement-troubleshooting-guide-sdk.md 
[link 26]: mobile-engagement-troubleshooting-guide-sr-info.md 
[link 27]: ../mobile-engagement-how-tos-first-push.md 
[link 28]: ../mobile-engagement-how-tos-test-campaign.md 
[link 29]: ../mobile-engagement-how-tos-personalize-push.md 
[link 30]: ../mobile-engagement-how-tos-differentiate-push.md 
[link 31]: ../mobile-engagement-how-tos-schedule-campaign.md 
[link 32]: ../mobile-engagement-how-tos-text-view.md 
[link 33]: ../mobile-engagement-how-tos-web-view.md 

