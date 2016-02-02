<properties
   pageTitle="Marketplace の VM プランのテスト | Microsoft Azure"
   description="Azure Marketplace の VM イメージをテストする方法を理解します。"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/09/2015"
   ms.author="hascipio" />


# ステージングでの Azure Marketplace の VM プランのテスト

ステージングとは、SKU をプライベートの「サンドボックス」にデプロイすることです。そこから、Marketplace にデプロイする前に、機能をテストおよび検証できます。 ステージングでは、SKU はそれをデプロイしたお客様に表示されるのと同じように表示されます。 VM イメージをステージングにプッシュするには認定を受ける必要があります。

## 手順 1: ステージングへのプランのプッシュ

1. **[発行]** タブで **[ステージングにプッシュ]** をクリックします。

  ![図](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)

2. 発行ポータルでエラーが通知される場合は、それらを修正します。
3.  **して段階的な製品やサービスにアクセスできるユーザーですか?** ] ダイアログ ボックスで、プレビューで提案に使用する Azure サブスクリプションの一覧を入力、 [Azure プレビュー ポータル](https://portal.azure.com)します。
4. サインイン、 [Azure プレビュー ポータル](http://portal.azure.com) では、Azure サブスクリプションのいずれかを使用して、前の手順で表示します。
5. プランを検索して、VM イメージについて以下の点を検証します。
  1. マーケティング コンテンツが Marketplace に正しく表示されていることを確認します。

      ![img-map-portal](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

  2. VM イメージのエンド ツー エンドのデプロイメント。

> [AZURE.IMPORTANT] プランは、本番にプッシュする準備が整っている発行ポータル (**[発行]** > **[本番にプッシュするための承認を要求]**) を介して Microsoft に通知するまで、ステージング状態のままです。 この時期が、プランのライブへの移行の準備段階で必要な項目をチームのすべてのメンバーにチェックさせるのに最適です。

> データ センター全体をレプリケーションするには、最大 48 時間かかります。 プランに表示されます、レプリケーションが完了した後、 [Azure Marketplace](http://azure.microsoft.com/marketplace)します。

## 次のステップ

これで、プランが「ステージング」とその機能をテストしたコンテンツをマーケティング、次のように最終的な発行段階に進むことができます **手順 4**: [市場への提案を展開する](marketplace-publishing-push-to-production.md)します。

## 関連項目

- [作業の開始: オファーを Azure Marketplace に発行する方法](marketplace-publishing-getting-started.md)





