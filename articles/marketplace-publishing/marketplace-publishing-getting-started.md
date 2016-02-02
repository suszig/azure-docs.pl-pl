<properties
   pageTitle="プランを作成して Marketplace にデプロイする方法の概要 |Microsoft Azure"
   description="承認済みの Microsoft 販売者になり、仮想マシン イメージ、テンプレート、データ サービス、または開発者サービスを作成して Azure Marketplace にデプロイするために必要な手順を理解します。"
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
   ms.author="hascipio" />


# Azure Marketplace へのプランの発行方法

この記事は、販売者がソリューションを作成して Azure Marketplace にデプロイし、Azure の他の顧客やパートナーが購入および使用できるようにするのに役立ちます。

まず、発行元は、会社がどのような種類のソリューションを提供するかを定義します。 Azure Marketplace では複数のソリューションがサポートされており、Azure Marketplace に正常に発行するには、各ソリューションでわずかに異なる作業を行う必要があります。

ソリューションの種類:

- 仮想マシン イメージ
- 開発者サービス
- データ サービス
- ソリューション テンプレート

いくつかの手順は、異なる種類のソリューションで共通して使用されます。 この記事では、ソリューションの種類ごとに必要な手順の概要を示します。
> [AZURE.NOTE] Azure Marketplace では、作業を開始する前に、事前承認が必要です。 これは、データ サービスの発行元に対しては適用されません。

| | 仮想マシン イメージ| 開発者サービス| データ サービス| ソリューション テンプレート|
|----|----|----|----|----|
| **事前承認を得る**| [Microsoft Azure 認定 ][link-certification]| [Microsoft Azure 認定 ][link-certification]| 該当なし| [Microsoft Azure 認定 ][link-certification]|
| **手順 1: 販売者アカウントを登録します。**| [Microsoft 販売者のアカウント: 作成と登録 ][link-accts]| [Microsoft 販売者のアカウント: 作成と登録 ][link-accts]| [Microsoft 販売者のアカウント: 作成と登録 ][link-accts]| [Microsoft 販売者のアカウント: 作成と登録 ][link-accts]|
| **手順 2: プランを作成します。**| [一般的な技術以外の前提条件](marketplace-publishing-pre-requisites.md)| [一般的な技術以外の前提条件](marketplace-publishing-pre-requisites.md)| [一般的な技術以外の前提条件](marketplace-publishing-pre-requisites.md)| [一般的な技術以外の前提条件](marketplace-publishing-pre-requisites.md)|
| | [VM の技術の前提条件 ][link-single-vm-prereq]| 開発者サービスの技術的な前提条件| [データ サービスの技術的な前提条件](marketplace-publishing-data-service-creation-prerequisites.md)| [ソリューション テンプレートの技術的な前提条件](marketplace-publishing-solution-template-creation-prerequisites.md)|
| | [VM イメージの発行ガイド ][link-single-vm]| 開発者サービスの発行ガイド| [データ サービスの発行ガイド](marketplace-publishing-data-service-creation.md)| [ソリューション テンプレートの発行ガイド](marketplace-publishing-solution-template-creation.md)|
| | [Azure Marketplace マーケティング コンテンツ ガイド ][link-pushstaging]| [Azure Marketplace マーケティング コンテンツ ガイド ][link-pushstaging]| [Azure Marketplace マーケティング コンテンツ ガイド ][link-pushstaging]| [Azure Marketplace マーケティング コンテンツ ガイド ][link-pushstaging]|
| **手順 3: ステージング環境に提案をプッシュします。**| [ステージング環境で、VM サービスをテストします。](marketplace-publishing-vm-image-test-in-staging.md)| ステージング環境での開発者サービス プランをテストします。| [ステージング環境でのデータ サービス プランをテストします。](marketplace-publishing-data-service-test-in-staging.md)| [ステージング環境でのソリューション テンプレートをテストします。](marketplace-publishing-solution-template-test-in-staging.md)|
| **ステップ 4: 市場への提案を配置します。**| [Marketplace の ][link-pushprod]| [Marketplace の ][link-pushprod]| [Marketplace の ][link-pushprod]| [Marketplace の ][link-pushprod]|

## サポート

- [[サポート技術的-全般] のパブリッシャーとしてサポートを受ける][suppt-general]
- [販売者 insights の ][suppt-rpt-insights]
- [[サポート技術的 rpt 支払] reporting 支払いを理解します。][suppt-rpt-payouts]
- [Marketplace の ][suppt-common]

## その他のリソース

- 使用されるポータルに関する詳細については、次を参照してください。 [必要がありますポータル](marketplace-publishing-portals.md)します。

**仮想マシン**

- [Azure PowerShell の設定](marketplace-publishing-powershell-setup.md)
- [内部設置型仮想マシンのイメージを作成します。](marketplace-publishing-vm-image-creation-on-premise.md)
- [Azure プレビュー ポータルで Windows を実行する仮想マシンの作成します。](../virtual-machines-windows-tutorial/)

**Data Services**

- [データ サービスの OData のマッピング](marketplace-publishing-data-service-creation-odata-mapping.md)
- [データ サービスの OData マッピング ノード](marketplace-publishing-data-service-creation-odata-mapping-nodes.md)
- [データ サービスの OData マッピングの例](marketplace-publishing-data-service-creation-odata-mapping-examples.md)


[suppt-general]: marketplace-publishing-get-publisher-support.md 
[suppt-rpt-insights]: marketplace-publishing-report-seller-insights.md 
[suppt-rpt-payouts]: marketplace-publishing-report-payout.md 
[suppt-common]: marketplace-publishing-support-common-issues.md 
[link-certification]: marketplace-publishing-azure-certification.md 
[link-accts]: marketplace-publishing-accounts-creation-registration.md 
[link-single-vm]: marketplace-publishing-vm-image-creation.md 
[link-single-vm-prereq]: marketplace-publishing-vm-image-creation-prerequisites.md 
[link-multi-vm]: marketplace-publishing-solution-template-creation.md 
[link-multi-vm-prereq]: marketplace-publishing-solution-template-creation-prerequisites.md 
[link-datasvc]: marketplace-publishing-data-service-creation.md 
[link-datasvc-prereq]: marketplace-publishing-data-service-creation-prerequisites.md 
[link-devsvc]: marketplace-publishing-dev-service-creation.md 
[link-devsvc-prereq]: marketplace-publishing-dev-service-creation-prerequisites.md 
[link-pushstaging]: marketplace-publishing-push-to-staging.md 
[link-pushprod]: marketplace-publishing-push-to-production.md 

