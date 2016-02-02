<properties
   pageTitle="Marketplace のソリューション テンプレート プランのテスト | Microsoft Azure"
   description="Azure Marketplace のソリューション テンプレート プランをテストする方法を理解します。"
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
   ms.date="12/04/2015"
   ms.author="hascipio; v-divte" />


# ステージングでのソリューション テンプレート プランのテスト

ステージングとは、プランをプライベートの「サンドボックス」にデプロイすることです。そこでは、本番にプッシュする前に、機能をテストおよび検証できます。 ステージングでは、プランはそれをデプロイしたお客様に表示されるのと同じように表示されます。 プランをステージングにプッシュするには認定を受ける必要があります。

オファーをステージングすると後、は、表示およびテストで提供、 [Azure ポータル](https://portal.azure.com/)します。

ステージング環境に提案をプッシュし、テストで次の手順に従って、 [Azure ポータル](https://portal.azure.com/):

1.  移動して、 [公開ポータル](https://publish.windowsazure.com) > **ソリューション テンプレート** ] タブ > 提案 > **発行** > **Push to Staging**します。
2.  プランのプレビューおよびテストに使用する Azure サブスクリプションの一覧を指定します。
3.  前の手順で使用したサブスクリプション ID を使用して、Azure プレビュー ポータルにサインインします。
4.  以下の点について、Azure プレビュー ポータルで少なくとも 1 回テストを実行します。
  - マーケティング コンテンツが Azure Marketplace に正しく表示されているか
  - トポロジのエンド ツー エンドのデプロイメント
  - パフォーマンス テストとストレス テストの実行
  - トポロジがベスト プラクティスに準拠しているか

## 次のステップ

結果に満足できるかどうかは、最終的なオファーの公開フェーズに進むことができます **手順 4**:  [市場への提案を展開する](marketplace-publishing-push-to-production.md)します。 問題があれば、プランに変更を加え、もう一度認定を要求してください。
> [AZURE.NOTE] マーケティング コンテンツの変更であれば、認定は必要ありません。

参照してください [作業の開始: オファーを Azure Marketplace に発行する方法](marketplace-publishing-getting-started.md) ガイドについては、すべてのパブリッシャーのタスクにします。





