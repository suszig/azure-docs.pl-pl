<properties 
    pageTitle="Windows ユニバーサル アプリ SDK コンテンツ" 
    description="Azure Mobile Engagement 向け Windows ユニバーサル アプリ SDK の内容詳細"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/10/2015" 
    ms.author="piyushjo" />


# Windows ユニバーサル アプリ SDK コンテンツ

このドキュメントでは、アプリケーションで SDK により展開されるコンテンツについて説明します。

## `/Resources` フォルダー

このフォルダーには、Mobile Engagement が必要とするすべてのリソースが含まれています。 アプリに適合するように、これらをカスタマイズすることもできます。

- `EngagementConfiguration.xml` : [モバイル エンゲージメントの構成ファイルでは、これは、モバイル エンゲージメントの設定 (モバイル エンゲージメント接続文字列、クラッシュ レポートなど) をカスタマイズすることができます。

### /html フォルダー

- `EngagementNotification.html` : `通知` web ビュー html デザイン。

- `EngagementAnnouncement.html` : `アナウンス` web ビュー html デザイン。

### /images フォルダー

- `EngagementIconNotification.png` : 通知の左側に表示されるブランド アイコンに換えますこの自分のブランド アイコン。

- `EngagementIconOk.png` : `Ok` 操作または検証ボタンの reach コンテンツ ページのアイコン。

- `EngagementIconNOK.png` : `NOK` アイコン reach コンテンツ ページの検証ボタンが無効になっているときに使用します。

- `EngagementIconClose.png` : `閉じる` reach 通知や閉じるボタンのコンテンツのアイコン。

### /overlay フォルダー

- `EngagementBaseOverlay.cs` : で使用される基本コード、 `アナウンス` と `通知` オーバーレイです。

- `EngagementOverlayAnnouncement.xaml` : `アナウンス` xaml デザイン。

- `EngagementOverlayAnnouncement.xaml.cs` : `EngagementOverlayAnnouncement.xaml` リンク コード。

- `EngagementOverlayNotification.xaml` : `通知` xaml デザイン。

- `EngagementOverlayNotification.xaml.cs` : `EngagementOverlayNotification.xaml` リンク コード。

- `EngagementPageOverlay.cs` : `オーバーレイ` アナウンスと通知は、コードを表示します。





