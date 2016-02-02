<properties 
   pageTitle="Azure Mobile Engagement ユーザー インターフェイス - Reach (リーチ)" 
   description="Azure Mobile Engagement でプッシュ通知を利用してアプリケーションのユーザーに通知する方法について説明します。" 
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
   ms.date="11/29/2015"
   ms.author="piyushjo"/>



# プッシュ通知を利用してアプリケーションのユーザーに通知する方法

この記事では、**Mobile Engagement** ポータルの **[リーチ]** タブについて説明します。 **Mobile Engagement** ポータルはモバイル アプリの監視と管理に使用します。 ポータルを使い始めるには、最初に **Azure Mobile Engagement** アカウントを作成する必要があることに注意してください。 詳細については、次を参照してください。 [Azure モバイル エンゲージメント アカウントを作成する](mobile-engagement-create-account.md)します。

UI の Reach (リーチ) セクションは、プッシュ キャンペーン管理ツールで、プッシュ通知キャンペーンや機能を作成、編集、アクティブ化、完了、モニターしたり、統計を取得したりできます。また、Reach API (と低レベルのプッシュ API の一部の要素) 経由でアクセスすることも可能です。 API と UI のいずれを使用する場合でも、リーチ キャンペーンを使用する前に、Azure Mobile Engagement と Reach の両方を、SDK を使用する各プラットフォーム用のアプリケーションに統合する必要があります。
>[AZURE.NOTE] **Mobile Engagement** ポータル UI の多くのセクションには、**[ヘルプの表示]** ボタンが含まれています。 このボタンを押すと、セクションに関する文脈上の情報が表示されます。


## 4 種類のプッシュ通知

1.    Announcements - allow you to send advertising messages to users that redirect them to another location inside your app or to send them to a webpage or store outside of your app. 

2.    Polls - allow you to gather information from end users by asking them questions.

3.    Data Pushes - allow you to send a binary or base64 data file. The information contained in a data push is sent to your application to modify your users' current experience in your app. Your application needs to be able to process the data in a data push.



## 各キャンペーンで表示されるリアルタイム統計の 3 つのカテゴリ

1.    Pushed - how many pushes were sent based on the criteria specified in the campaign. 

2.    Replied - how many users reacted to the notification by either opening it from outside of app or closing it in the app. 

3.    Actioned - how many users clicked on the link in the notification to be redirected to a new location in the app, to a store, or to a web browser. 


> [AZURE.NOTE]: キャンペーンの詳細な統計情報は、Reach API Stats 経由で確認できます。



## キャンペーンの詳細

キャンペーンの名前の上にカーソルを合わせるか、クリックして開くとキャンペーンを編集、複製、削除したり、まだアクティブ化されていないキャンペーンをアクティブ化したりできます。 すでにアクティブなキャンペーンの名前の上にカーソルを合わせるか、クリックして開くとそのキャンペーンの複製を作成できますが、 一度アクティブ化したキャンペーンを変更することはできません。

![Reach1][18]

## Reach のフィードバック

キャンペーンの詳細または統計情報を表示するには、クリックし、 すでにアクティブなキャンペーンの詳細ビューから統計ビューに切り替えて、統計の簡易ビューから詳細ビューに切り替えると、詳細情報を確認できます (保有している権限に基づきます)。 過去のキャンペーンからのリーチのフィードバック情報を新しいキャンペーンの対象条件として使用もできます。 リーチのフィードバックの統計は、Reach API の **Stats** を使用して収集できます。 また、過去のキャンペーンに基づいてプッシュ キャンペーンの対象をカスタマイズすることも可能です。


![Reach2][19]


## 関連項目

- [概念 ][link 6]
- [トラブルシューティング ガイド サービス ][link 24]




[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png 
[2]: ./media/mobile-engagement-user-interface-home/home1.png 
[3]: ./media/mobile-engagement-user-interface-home/home2.png 
[4]: ./media/mobile-engagement-user-interface-home/home3.png 
[5]: ./media/mobile-engagement-user-interface-home/home4.png 
[6]: ./media/mobile-engagement-user-interface-home/home5.png 
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png 
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png 
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png 
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png 
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png 
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png 
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png 
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png 
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png 
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png 
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png 
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png 
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png 
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png 
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png 
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png 
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png 
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png 
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png 
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png 
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png 
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png 
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png 
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png 
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png 
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png 
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png 
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png 
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png 
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png 
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png 
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png 
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png 
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png 
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png 
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png 
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png 
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png 
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png 
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png 
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png 
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png 
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png 
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png 
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png 
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png 
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png 
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png 
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png 
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png 
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png 
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png 
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
[link 27]: mobile-engagement-user-interface-reach-campaign.md 
[link 28]: mobile-engagement-user-interface-reach-criterion.md 
[link 29]: mobile-engagement-user-interface-reach-content.md 

