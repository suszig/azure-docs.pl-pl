<properties
    pageTitle="Mobile Services の使用時に App Service はどのように役立つか"
    description="既存の Mobile Services プロジェクトに App Service がもたらす利点について説明します。"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="kirillg"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2015"
    ms.author="kirillg"/>

# <a name="getting-started"> </a>Mobile Services の使用時に App Service はどのように役立つか

##概要
既存の Mobile Service は安全で、引き続きサポートされます。 ただしが機能の一部、 *Azure App Service* 現在モバイル サービスでは利用されないなモバイル アプリのプラットフォームを提供します。  

- Web クライアントとモバイル クライアントの両方を含むアプリを、よりシンプルで簡単なコスト効率の良いものに
- Web ジョブ、カスタム CName、進化した監視を含む新しいホスト機能
- Office 365、Dynamics CRM、Salesforce やその他の重要な SaaS API との設定不要の統合
- Node.js と .NET だけでなく、Java や PHP のバックエンド コードもサポート
- Traffic Manager との設定不要の統合
- ハイブリッド接続に加えて、VNet を使用したオンプレミス リソースと VPN への接続
- 警告だけでなく、NewRelic や AppInsights も使用したアプリの監視とトラブルシューティング
- VM サイズなど、基盤となるコンピューティング リソースがより広範囲に
- 組み込みの自動スケール、負荷分散、パフォーマンス監視
- 組み込みのステージング、バックアップ、ロールバック、運用環境でのテスト機能

## 新しいホスト機能
 *Azure App Service* 、 *モバイル アプリ* バックエンド コードは Web アプリと API アプリと同じコンテナーで実行します。 したがって、このコンテナーの機能は、現在 Mobile Services にはないものも含めて、すべて利用することができます。

- Web ジョブを使用して、実行中のバックエンド ロジックを継続的に追加する
- バックエンド コードが常に実行されていることを確認する
- カスタムの CNames を使用して、使いやすくて安定した名前をモバイル バックエンド エンドポイントに提供する
- Traffic Manager を使用してアプリを地球スケールにする
- 任意のライブラリとパッケージが含まれます。 .NET では、デプロイするアセンブリはランタイムで使用されるアセンブリです。ホスト環境のバージョンとの競合はありません。
- (.NET の場合) MVC などの ASP.NET の任意の機能を活用します。


##接続、 *モバイル アプリ* SaaS API へ
*Azure App Service* モバイル アプリを Office 365、Dynamics、Salesforce、SAP、および詳細などの SaaS Api に接続できるようになります。 *Azure App Service* 認証でのユーザーの代わりを提供しの個々 の SaaS Api、プライマリ id とトークンを関連付けることによってを使用するすべての SaaS Api で真のシングル サインオンを実行することができます

##VNet を使用したオンプレミス データへのアクセス
Mobile Services により、現在既にハイブリッド接続を使用してオンプレミス リソースにアクセスすることができます。 ただし、VPN ソリューションの方が望ましい場合もあります。  *Azure App Service* Azure VNet をモバイル アプリ バックエンド コードを使用することができます。

##好みのバックエンド言語の使用
*Azure App Service* Java、PHP、および .NET と Node.js のモバイル サービスでサポートされているだけでなく Python を含む開発者プラットフォームに広範で豊富なサポートを提供します。

##自動スケールの設定
Mobile services では、バックエンド コードのすべてのインスタンスが S VM で実行されていました。 *Azure App Service* オプションの豊富なセットから Vm のサイズを選択することができます。 ことができますも簡単にスケール アップまたはスケール アウトをさまざまなパフォーマンス メトリックに基づいて、顧客の受信負荷を処理します。

##状況の把握
監視と警告を使用して問題にリアルタイムで対応し、自動的に担当者やチームに通知します。 高度なアプリ分析と New Relic や AppInsights の監視機能を統合して、モバイル アプリの実行状況に対する洞察を深めます。  *Azure App Service* を今すぐプログラムや警告をポータルの設定を使用してに基づくことがさまざまなパフォーマンス メトリック

##資産の安全性の保持
バックエンドとデータベースを自動的にバックアップします。 コードとデータは災害からセキュリティ保護されていて簡単に復元できるため、安心して業務を遂行することができます。

##準備、ステージング、運用開始
 *Azure App Service* 複数のプライベートなテスト用とステージング環境のモバイル アプリを作成できます。 これらを使用して、デプロイ前にテストを実行します。 ダウンタイムなしで運用環境と交換します。 Web アプリは事前に読み込まれ、優れたカスタマー エクスペリエンスを実現します。



まで *Azure App Service モバイル アプリ* 機能が一般公開、エクスペリエンスが提供されますシームレスな移行、既存の Mobile Services App Service へ移行するよう選択する必要があります。 調査を開始する一方、 *App Service* 活用 *App Service* これに従って既存の Mobile Service の [チュートリアル](app-service-mobile-migrating-from-mobile-services.md)します。
 

