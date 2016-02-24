<properties 
    pageTitle="分析用のストレージ環境にデータを読み込む | Microsoft Azure" 
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
    ms.date="10/19/2015" 
    ms.author="bradsev" />

# 分析用のストレージ環境にデータを読み込む

## はじめに

Cortana Analytics プロセス (CAP) では、データのインジェストまたはさまざまな処理したり、プロセスの各段階で最も適切な方法で分析する別の記憶域環境に読み込まれる必要があります。 CAP での処理に通常使用されるデータの取り込み先として、Azure BLOB ストレージ、Azure SQL Database、Azure VM 上の SQL Server、HDInsight (Hadoop)、Azure Machine Learning などがあります。 

これは、 **メニュー** にこれらのデータを取り込む方法について説明するトピックへのリンク対象の環境のデータを格納および処理できます。

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

分析の目的を達成するためにデータを取り込む必要があるターゲット環境は、技術上およびビジネス上のニーズと、データの最初の保存場所、形式、およびサイズによって決まります。 予測モデルを構築するために必要なさまざまなタスクを実行するために、複数の環境間でデータを移動する必要があるシナリオも珍しくありません。 この一連のタスクには、データ探索、前処理、クリーニング、サンプリング、モデル トレーニングなどが含まれます。

