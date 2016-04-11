<properties 
    pageTitle="Cortana Analytics Process のデータをサンプリングする" 
    description="さまざまなストレージ環境のデータを探索する方法について説明します。" 
    services="machine-learning" 
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
    ms.date="10/20/2015" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Cortana Analytics Process のデータをサンプリングする

これは、 **メニュー** 記憶域のさまざまな環境からのデータをサンプリングする方法を説明するトピックへのリンク。 このタスクは、Cortana Analytics Process (CAP) の 1 ステップです。

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

## はじめに

このドキュメントでは、Azure BLOB ストレージに格納されたデータのサンプリングについて説明します。これは、プログラムを使用してこのデータをダウンロードしてから、Python のサンプル コードでサンプリングして行います。 これを行う手順は、次のとおりです。

このドキュメントでは、Cortana Analytics Process で一般的に使用されている 3 つの場所に格納されたデータをサンプリングする方法について説明します。

- **Azure blob コンテナーのデータ** をプログラムでダウンロードして、Python のサンプル コードでサンプリングしてサンプリングします。
- **SQL Server データ** SQL と Python プログラミング言語の両方を使用してサンプリングされます。 
- **テーブルのデータを hive** Hive クエリを使用してサンプリングされます。


