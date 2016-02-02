<properties 
    pageTitle="BizTalk Services の管理および開発タスク一覧 | Microsoft Azure BizTalk Services" 
    description="" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="msftman" 
    manager="dwrede" 
    editor="cgronlun"/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/02/2015" 
    ms.author="deonhe"/>


# BizTalk Services の管理および開発タスク一覧

## はじめに

Microsoft Azure BizTalk Services を使用する場合、考慮する必要があるオンプレミスおよびクラウドベースのコンポーネントがいくつかあります。 まずはじめに、次のプロセス フローを考慮します。

| 手順| 責任者| タスク| 関連リンク|
|----|----|----|----|

1.|Administrator|Microsoft アカウントまたは、組織のアカウントを作成 | を使用して Microsoft Azure サブスクリプションの作成します。[Azure クラシック ポータル](http://go.microsoft.com/fwlink/p/?LinkID=213885)|
| 2 |。Administrator|作成または BizTalk サービスをプロビジョニングします |。[Azure クラシック ポータルを使用して BizTalk サービスを作成する](http://go.microsoft.com/fwlink/p/?LinkID=302280)|
| 3 |。Administrator|個人または会社の BizTalk サービス deployment| を登録します。[を登録して、BizTalk で BizTalk サービス デプロイの更新サービスのポータル](https://msdn.microsoft.com/library/azure/hh689837.aspx)|
| 4 |。Administrator|キューまたはトピック ターゲットを使用して、アプリケーションでは、BizTalk アダプター サービスを使用して、内部設置型の基幹業務 (LOB) システムに接続する場合に適用されますか。 Azure Service Bus 名前空間を作成します。 この名前空間、Service Bus 発行者名、およびサービス バス発行者キーの値を開発者に提供します |。[方法: 作成または Service Bus Service 名前空間を変更する]() と [取得発行者名および発行者キーの値]()|
| 5 |。Developer|SDK をインストールし、Visual Studio で BizTalk サービス プロジェクトを作成します |。[Azure BizTalk サービス SDK をインストール]() と [高度なメッセージング エンドポイントを Azure に作成]()|
| 6 |。Developer|Azure でホストされる BizTalk サービスに、BizTalk サービス プロジェクトを展開します |。[のデプロイおよび更新、BizTalk Services プロジェクト](https://msdn.microsoft.com/library/azure/hh689881.aspx)|
| 7 |。Administrator|EDI を使用している場合に適用されます。 パートナーを追加し、Microsoft Azure BizTalk Services ポータルでアグリーメントを作成できます。 アグリーメントを作成するときは、ブリッジまたはアグリーメントの設定に開発者が作成した変換を追加することができます |。[EDI、AS2、および EDIFACT を構成するのには、BizTalk サービス ポータル](https://msdn.microsoft.com/library/azure/hh689853.aspx)|
| 8 |。Administrator|パフォーマンス メトリックなど、BizTalk サービスのヘルスを監視する Azure クラシック ポータルを使用します |。[BizTalk サービス: ダッシュ ボード、監視、およびスケール] タブ](http://go.microsoft.com/fwlink/p/?LinkID=302281)|
できません |。Administrator|ブリッジ ファイルで処理されるように、BizTalk サービスと追跡メッセージが使用するアイテムを管理する Microsoft Azure BizTalk サービス ポータルを使用します |。[Biztalk サービス ポータル](https://msdn.microsoft.com/library/azure/dn874043.aspx)|
| 10 |。Administrator|BizTalk サービスをバックアップするバックアップのスケジュールを作成します |。[ビジネス継続性と BizTalk サービスでの障害復旧](https://msdn.microsoft.com/library/azure/dn509557.aspx) |
## 次のステップ

[チュートリアルとサンプル](https://msdn.microsoft.com/library/azure/hh689895.aspx)

[Visual Studio でプロジェクトを作成します。](https://msdn.microsoft.com/library/azure/hh689811.aspx)

[Azure BizTalk サービス SDK をインストールします。](https://msdn.microsoft.com/library/azure/hh689760.aspx)

## 概念

[Visual Studio でプロジェクトを作成します。](https://msdn.microsoft.com/library/azure/hh689811.aspx)
[EDI、AS2、および EDIFACT メッセージング (企業)](https://msdn.microsoft.com/library/azure/hh689898.aspx)
## その他のリソース

[ソース、宛先、およびブリッジ メッセージング エンドポイントを追加します。](https://msdn.microsoft.com/library/azure/hh689877.aspx)
[学習およびメッセージ マップおよび変換の作成](https://msdn.microsoft.com/library/azure/hh689905.aspx)
[BizTalk アダプター サービス (BAS) の使用](https://msdn.microsoft.com/library/azure/hh689889.aspx)
[Azure BizTalk サービス](http://go.microsoft.com/fwlink/p/?LinkID=303664)





