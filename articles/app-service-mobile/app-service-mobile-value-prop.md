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

# <a name="getting-started"> </a>Mobile Apps とは

[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

Azure App Service は、プロの開発者のための完全に管理されたサービスとしてのプラットフォーム (PaaS) 製品であり、Web、モバイル、および統合シナリオ向けに豊富な機能を提供します。 *Mobile Apps* で *Azure App Service* 拡張性の高い、グローバルに利用可能なモバイル アプリケーション開発プラットフォームを提供して、企業の開発者やシステム インテグレーター向けのモバイル開発者に豊富な機能を提供します。

![Mobile Apps](./media/app-service-mobile-value-prop/overview.png)

##モバイル アプリを使用する理由
*Mobile Apps* で *Azure App Service* 企業の開発者やシステム インテグレーター向けのモバイル開発者に豊富な機能を提供する、拡張性の高い、グローバルに利用可能なモバイル アプリケーション開発プラットフォームを提供しています。 モバイル アプリでは、次のことを実行できます。

- **ネイティブ コンパイルおよびクロス プラットフォーム アプリのビルド** - ネイティブ iOS、Android、および Windows を構築するかどうかアプリケーションまたはクロスプラット フォームの Xamarin や Cordova (Phonegap)、アプリを利用できます App Service のネイティブ Sdk を使用しています。  
- **エンタープライズ システムに接続する** - Mobile Apps を分単位で企業のサインオンを追加し、企業のオンプレミスに接続またはクラウド リソースを使用します。
- **SaaS Api への接続を簡単に** -40 を超える SaaS API コネクタを使用することができますを簡単に統合アプリ SaaS Api の企業が今日で使用します。 CRM と請求システムの両方でアカウントの状態を更新する場合、 モバイル アプリから企業の SaaS API をすぐに使用できます。
- **同期のオフライン対応アプリの構築** - モバイル ワーカー生産性の高いアプリを構築して、オフラインでも動作しのエンタープライズ データ ソースや SaaS Api との接続があるときに、バック グラウンドでデータを同期するモバイル アプリを使用します。
- **秒単位で数百万のプッシュ通知** -適切なタイミングときに送信されるユーザーのニーズにカスタマイズされ、あらゆるデバイスでインスタント プッシュ通知を使用して顧客を対象にします。

## モバイル アプリの概念
- **シングル サインオン** - Azure Active Directory、Facebook、Google、Twitter、および Microsoft アカウントなどの id プロバイダーの成長し続けるの一覧から選択し、数分でアプリに認証を追加するモバイル アプリを利用しています。
- **オフライン同期**  - Mobile Apps では、簡単に構築するための堅牢なあり応答性の高いアプリを使用できる接続が利用できなくなったときにオフライン作業し、デバイスがオンラインに戻ったときに、企業のバックエンド システムを同期する従業員です。 オフライン同期機能は、すべてのクライアント プラットフォームでサポートされており、SQL、テーブル ストレージ、Mongo、Document DB などのすべてのデータ ソース、および Office 365、Salesforce、Dynamics、オンプレミス データベースなどのすべての SaaS API で動作します。
- **プッシュ通知** -モバイル アプリは、きわめて拡張性の高いモバイル プッシュ通知エンジン、Notification Hubs、iOS、Android、Windows、使用して、対象ユーザーの動的セグメントに数百万もの個人用プッシュ通知を送信することは、または Kindle デバイスを数秒です。 Notification Hubs は既存の任意のアプリ バックエンドに簡単に接続できます。そのバックエンドがオンプレミスでホストされているか、クラウドでホストされているかは関係ありません。
- **自動スケーリング** - App Service では、すばやくスケール アップすることができますやアウト処理、将来の顧客負荷にします。 VM の数やサイズを手動で選択するか、負荷またはスケジュールに基づいてモバイル アプリ バックエンドのスケール設定を行うように自動スケールを設定します。

## Getting Started (概要) ##
をモバイル アプリを開始するには、次の [Get Started] チュートリアルです。

Azure App Service プラットフォームの詳細については、次を参照してください。 [Azure App Service]します。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](https://tryappservice.azure.com/?appServiceName=mobile), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

<!-- URLs. -->
[Migrate your Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md

[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[Get Started]: app-service-mobile-ios-get-started.md

 

