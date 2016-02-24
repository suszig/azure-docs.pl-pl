<properties 
    pageTitle="Azure Mobile Engagement で個人用に設定された通知を送信する" 
    description="名前などのユーザー プロファイル情報を通知に含めることで個人用に設定した通知を送信する方法"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="all" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/07/2015" 
    ms.author="piyushjo" />

#ユーザー名を追加し、通知を個人用に設定する

アプリ ユーザーに通知にもっと注目してもらうには、通知を個人に合わせて設定します。 たとえば、アプリ ユーザーの名前を選択し、個々のユーザーに合わせて通知をカスタマイズすると非常に効果的です。 ここで注意が必要です。この方法を使いすぎると、一部のユーザーにとって気味が悪く感じられる可能性があるため、ユーザーの名前を通知に追加する方法は慎重に利用する必要があります。 また、この方法を利用する前に、モバイル アプリで個人情報の提供に関して完全な同意を与えるかどうかをユーザーが選択できるようにします。 

技術的には、Azure Mobile Engagement を利用し、以下の手順で通知をカスタマイズします。ユーザー名を通知に追加するシナリオが利用されています。 アプリ情報またはタグの概念が利用されます。その値はモバイル アプリに統合されている SDK か API で渡されます。 これらのアプリ情報またはタグは次のように利用できます。

1. アプリ情報の値に基づき、通知の対象を特定のユーザーにするために 
2. 通知内でデバイス/ユーザーに固有の値で置換されるプレースホルダーとして (そのデバイスに通知が送信されます) 

> [AZURE.IMPORTANT] 通知を送信する速度が低下をこの追加の処理の各通知の使用のアプリ情報の値を置き換えることにより表示される注意してください。 

##Mobile Engagement ポータルでアプリ情報を登録する

1) まず、Azure ポータルでのアプリ情報またはタグを登録します。 移動して **設定** ]-> [ **Tag (App-info)** このします。  

![][1]  

2) をクリックして **新しいタグ (アプリ情報)** し、名前を付けて *user_name* として型を *文字列* ] をクリック **送信**します。 

![][2]

3) 最後に次のように登録されているこのアプリケーション情報が表示されます。

![][3]

##クライアント SDK からアプリ情報を送信する

ここでは Windows ユニバーサル アプリの例を使用しますが、Microsoft の他の SDK にもこれに相当する手法があります。 

おそらくは認証後に名前などのプロファイル情報をユーザーから取得する方法がモバイル アプリにある場合、`SendAppInfo` メソッドをここで呼び出し、先に登録した `user_name` アプリ情報の値を Mobile Engagement サービス バックエンドに入力します。 

    Dictionary<object, object> appInfo = new Dictionary<object, object>();
    appInfo.Add("user_name", str);
    EngagementAgent.Instance.SendAppInfo(appInfo); 

##個人用に設定した通知を送信する

これを使用して通知を送信するすべての設定できるようになった **user_name**します。 

1) Mobile Engagement ポータルに移動し、 **到達** して、通知を作成するタブは、通知のタイトルまたは本文に任意の場所は、次の形式でこのプレース ホルダーを使用できます。 

![][4]  

> [AZURE.NOTE] User_name アプリケーションの情報が設定されていない、すべてのユーザーには、任意の通知は表示されません。 

2) ときにモバイル エンゲージメントには、このアプリケーション情報を確認され、プレース ホルダーの値を置換し、この通知を送信するデバイスを選択します。  
例では、設定した場合の `str = "Scott"` デバイスとユーザーの登録がのアプリ情報に関連付けられたに取得 **user_name = SCOTT** このユーザーと、このユーザーを次の形式でのアプリのプッシュ通知を参照してください。 

![][5]  

<!-- Images. -->
[1]: ./media/mobile-engagement-send-personalized-notifications/app-info.png
[2]: ./media/mobile-engagement-send-personalized-notifications/create-app-info.png
[3]: ./media/mobile-engagement-send-personalized-notifications/app-info-user-name.png
[4]: ./media/mobile-engagement-send-personalized-notifications/personal-notification.png
[5]: ./media/mobile-engagement-send-personalized-notifications/notification.png


