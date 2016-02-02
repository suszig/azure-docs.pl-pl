<properties
    pageTitle="モバイル アプリとは"
    description="App Service が企業のモバイル アプリにもたらす利点について説明します。"
    services="app-service\mobile"
    documentationCenter=""
    authors="kirillg"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2015"
    ms.author="kirillg"/>


# <a name="getting-started"> </a>モバイル アプリとは?

[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

Azure App Service は、プロの開発者のための完全に管理されたサービスとしてのプラットフォーム (PaaS) 製品であり、Web、モバイル、および統合シナリオ向けに豊富な機能を提供します。 *Azure App Service* の *Mobile Apps* は、拡張性が高くグローバルに使用可能な、企業の開発者やシステム インテグレーター向けのモバイル アプリ開発プラットフォームで、モバイル開発者に豊富な機能のセットを提供します。

![Mobile Apps](./media/app-service-mobile-value-prop/overview.png)

## モバイル アプリを使用する理由

*Azure App Service* の *Mobile Apps* は、拡張性が高くグローバルに使用可能な、企業の開発者やシステム インテグレーター向けのモバイル アプリ開発プラットフォームで、モバイル開発者に豊富な機能のセットを提供します。 モバイル アプリでは、次のことを実行できます。

- **ネイティブ アプリとクロスプラットフォーム アプリの構築** - iOS、Android、Windows のネイティブ アプリを構築する場合でも、Xamarin や Cordova (Phonegap) のクロスプラットフォーム アプリを構築する場合でも、ネイティブ SDK を使用して App Service を活用することができます。
- **エンタープライズ システムへの接続** - Mobile Apps を使用して、企業へのサインオンを数分で追加し、企業のオンプレミス リソースまたはクラウド リソースに接続することができます。
- **SaaS API に簡単に接続** - 40 を超える SaaS API コネクタを使用して、所属する企業が現在使用している SaaS API に、アプリを簡単に統合することができます。 CRM と請求システムの両方でアカウントの状態を更新する場合、 モバイル アプリから企業の SaaS API をすぐに使用できます。
- **同期を使用したオフライン対応のアプリの構築** - オフラインでも動作し、企業のデータ ソースや SaaS API と接続したときに Mobile Apps を使用してバックグラウンドでデータを同期するアプリを構築することにより、モバイル ユーザーの生産性が向上します。
- **瞬時に多数のユーザーへプッシュ通知** - 顧客のニーズに合わせてカスタマイズされ、適切なタイミングで送信される、任意のデバイスへの即時のプッシュ通知によって顧客の関心を引きます。

## モバイル アプリの概念

- **シングル サインオン** - Azure Active Directory、Facebook、Google、Twitter、Microsoft アカウントなど、増え続ける ID プロバイダーの一覧から選択し、Mobile Apps を利用して数分でアプリに認証を追加します。
- **オフライン同期**  - Mobile Apps では、簡単に構築するための堅牢なあり応答性の高いアプリを使用できる接続が利用できなくなったときにオフライン作業し、デバイスがオンラインに戻ったときに、企業のバックエンド システムを同期する従業員です。 オフライン同期機能は、すべてのクライアント プラットフォームでサポートされており、SQL、テーブル ストレージ、Mongo、Document DB などのすべてのデータ ソース、および Office 365、Salesforce、Dynamics、オンプレミス データベースなどのすべての SaaS API で動作します。
- **プッシュ通知** - Mobile Apps は、非常に拡張性の高いモバイル プッシュ通知エンジンである Notification Hubs を提供します。これは、iOS、Android、Windows、または Kindle デバイスを使用して、数秒で数百万もの個人用プッシュ通知を対象ユーザーの動的セグメントに送信することができます。 Notification Hubs は既存の任意のアプリ バックエンドに簡単に接続できます。そのバックエンドがオンプレミスでホストされているか、クラウドでホストされているかは関係ありません。
- **自動スケール** - App Service では、スケール アップやスケール アウトをすばやく実行して、将来の顧客負荷に対処することができます。 VM の数やサイズを手動で選択するか、負荷またはスケジュールに基づいてモバイル アプリ バックエンドのスケール設定を行うように自動スケールを設定します。

## Getting Started (概要)

モバイル アプリを使用して開始するには、」チュートリアルに従います。

Azure App Service プラットフォームの詳細については、[Azure App Service] を参照してください。
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](https://tryappservice.azure.com/?appServiceName=mobile), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。



[migrate your mobile service to app service]: app-service-mobile-migrating-from-mobile-services.md 
[azure app service]: ../app-service/app-service-value-prop-what-is.md 
[get started]: app-service-mobile-ios-get-started.md 

