<properties 
    pageTitle="Azure Blob ストレージとの間のデータの移動 | Microsoft Azure" 
    description="Azure Blob ストレージとの間のデータの移動" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2015" 
    ms.author="bradsev;sunliangms;sachouks;mohabib" />


# Azure Blob ストレージとの間のデータの移動

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

## はじめに

以下のリンクから、Azure Blob ストレージとの間でデータを移動するために使用するテクノロジについてのガイダンスを参照してください。

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

最適な方法は、シナリオによって異なります。  [Advanced Analytics Process and Technology (ADAPT) Azure Machine Learning でのシナリオ](../machine-learning-data-science-plan-sample-scenarios.md) 記事では、さまざまな高度な分析プロセスで使用されるデータ サイエンス ワークフローで必要なリソースを決定できます。
> [AZURE.NOTE] Azure blob ストレージに完全な概要についてを参照してください [Azure Blob の基礎](../storage-dotnet-how-to-use-blobs.md) と  [Azure Blob サービス](https://msdn.microsoft.com/library/azure/dd179376.aspx)します。 

> [AZURE.TIP] 代わりに、使用することができます [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) を作成して、Azure blob ストレージからデータをダウンロードするパイプラインのスケジュール、発行済み Azure Machine Learning web サービスに渡す予測分析の結果を受信し、結果をストレージにアップロードします。 詳細については、次を参照してください。 [Azure Data Factory および Azure Machine Learning を使用して予測パイプラインを作成する](../data-factory/data-factory-create-predictive-pipelines.md)します。

## 前提条件

このドキュメントは、Azure サブスクリプション、ストレージ アカウント、そのアカウントに対応するストレージ キーがあることを前提としています。 データのアップロード/ダウンロードを行う前に、Azure Storage のアカウント名とアカウント キーを確認しておく必要があります。

- Azure サブスクリプションを設定するを参照してください。 [無料評価版の 1 か月](https://azure.microsoft.com/pricing/free-trial/)します。
- ストレージ アカウントを作成する手順について、およびアカウントとキー情報を取得するためには、「 [は Azure ストレージ アカウント](../storage-create-storage-account.md)します。










