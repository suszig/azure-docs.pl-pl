<properties 
   pageTitle="Azure モバイル エンゲージメント関連のトラブルシューティング ガイド- APIs" 
   description="Azure Mobile Engagement 関連のトラブルシューティング ガイド - API" 
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
   ms.date="06/18/2015"
   ms.author="piyushjo"/>


# API の問題のトラブルシューティング ガイド

次に説明するのは、管理者が、API を使用して Azure モバイル エンゲージメントを操作する方法に関して発生する可能性のある問題です。

## 構文の問題

### 問題

- 構文エラーは API を使用して (または予期しない動作によって) 発生します。

### 原因

- 構文の問題:
    - 使用している特定の API の構文を調べて、オプションを使用できることをご確認ください。
    - API の使用方法に関する一般的な問題は、リーチ API とプッシュ API を混同することです (多くのタスクは、プッシュ API の代わりにリーチ API を使用して実行する必要があります)。
    - もう 1 つの SDK の統合と API の使用方法に関する一般的な問題は、SDK キーと API キーを混同することです。
    - API に接続するスクリプトは、少なくとも 10 分ごとにデータを送信する必要があります。これを行わないと、接続はタイムアウトします (特に、データを待機している監視 API では一般的です)。 タイムアウトを回避するために、スクリプトは 10 分ごとに XMPP ping を送信してサーバーとのセッションを維持します。

### 関連項目

- [API ドキュメント ][link 4]
- [XMPP プロトコル情報](http://xmpp.org/extensions/xep-0199.html)

## Azure モバイル エンゲージメント UI で使用できるアクションと同じアクションを、API を使用して実行できません

### 問題

- Azure モバイル エンゲージメント UI から実行するアクションは、関連する Azure モバイル エンゲージメント API では機能しません。

### 原因

- Azure モバイル エンゲージメント UI から同じアクションを実行できることを確認すると、Azure モバイル エンゲージメントのこの機能と SDK が正しく統合されていることが分かります。

### 関連項目

- [UI ドキュメント ][link 1]

## エラー メッセージ

### 問題

- エラー コードは、実行時またはログ中に表示される API を使用します。

### 原因

- 次に、参照と事前のトラブルシューティングのために、一般的な API 状態コード番号の一覧を示します。

        200        Success.
        200        Account updated: device registered, associated, updated, or removed from the current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in the response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). The parameters provided to the API or service are invalid. In this case, the HTTP response will embed more details. Make sure to test for the MIME type of the response as the payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check the AppID and SDK key).
        402        Billing lock. The application is either off its quotas or is currently in a bad billing state.
        403        The application is not enabled or the specific API is disabled for this application.
        403        Unauthorized access to the project or application, invalid access key (the key must match the one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), the suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and the campaign’s property named, manual Push must be set to true.
        403        The email address is already associated to another account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        The email address is not associated with an account. Please create or update the account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying to edit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for the first time.
        409        Name already associated to a different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or the period is too large to be displayed (the server didn’t retrieve the analytics because the user request is for a period that is too large).
        503        Analytics not available yet (the requested information is not computed yet for an application).
        504        The server was not able to handle your request in a reasonable time (if you make multiple calls to an API very quickly, try to make one call at a time and spread the calls out over time).


### 関連項目

- [API ドキュメント - 特定の API ][link 4]

## サイレント障害

### 問題

- API のアクションは、実行時またはログ中にエラー メッセージが表示されずに失敗します。

### 原因

- 正しく統合されていない場合は、Azure モバイル エンゲージメント UI の多くの項目が無効になりますが、API からエラー メッセージが表示されません。実行していることを確認するには、UI から同じ機能をテストしてください。
- Azure モバイル エンゲージメントと、使用する Azure モバイル エンゲージメントの多くの高度な機能を、事前に別の手順で SDK を使用して個別にアプリと統合する必要があります。

### 関連項目

- [トラブルシューティング ガイド - SDK ][link 25]



[link 1]: mobile-engagement-user-interface.md 
[link 2]: mobile-engagement-troubleshooting-guide.md 
[link 3]: mobile-engagement-how-tos.md 
[link 4]: http://go.microsoft.com/fwlink/?LinkID=525553 
[link 5]: http://go.microsoft.com/fwlink/?LinkID=525554 
[link 6]: http://go.microsoft.com/fwlink/?LinkId=525555 
[link 7]: https://account.windowsazure.com/PreviewFeatures 
[link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement 
[link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/ 
[link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/ 
[link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/ 
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

