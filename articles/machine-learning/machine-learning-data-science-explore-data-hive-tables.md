<properties
    pageTitle="Hive クエリを使用して Hive テーブルのデータを探索する | Microsoft Azure"
    description="Hive クエリを使用して Hive テーブルのデータを探索します。"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="paulettm" 
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2015"
    ms.author="hangzh;bradsev" />

# Hive クエリを使用して Hive テーブルのデータを探索する 

これは、 **メニュー** ツールを使用して、記憶域のさまざまな環境からデータを探索する方法を説明するトピックへのリンク。 このタスクは、Cortana Analytics Process (CAP) の 1 ステップです。

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## はじめに

このドキュメントでは、Hive テーブルのデータの探索に使用される Hive スクリプトの例を紹介します。

## 前提条件
この記事では、以下のことを前提としています。

* Azure のストレージ アカウントが作成されている。 手順を知りたい場合は、次を参照してください [Azure ストレージ アカウントの作成。](../hdinsight-get-started.md#storage)
* HDInsight サービスでカスタマイズされた Hadoop クラスターがプロビジョニングされている。  手順を知りたい場合は、次を参照してください。 [高度な分析用 Azure HDInsight Hadoop クラスターをカスタマイズする](machine-learning-data-science-customize-hadoop-cluster.md)です。
* データが Azure HDInsight Hadoop クラスターの Hive テーブルにアップロードされている。 されていない場合に従ってください [のハイブ テーブルにデータを作成してロード](machine-learning-data-science-move-hive-tables.md) のハイブ テーブルに最初にデータをアップロードします。
* クラスターへのリモート アクセスが有効になっている。 手順を知りたい場合は、次を参照してください。 [Hadoop クラスターのヘッド ノードにアクセス](machine-learning-data-science-customize-hadoop-cluster.md#headnode)します。
* Hive クエリを送信する方法の手順を知りたい場合は、次を参照してください [Hive クエリを送信する方法。](machine-learning-data-science-move-hive-tables.md#submit)

## データ探索のための Hive クエリ スクリプトの例

1. パーティションごとの所見の数を取得する
    `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`

2. 1 日ごとの所見の数を取得する
    `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`

3. カテゴリ列内のレベルを取得する  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`

4. 2 つのカテゴリ列の組み合わせ内のレベルの数を取得する
    `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`

5. 数値型列の分布を取得する  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`

6. 2 つのテーブルの結合からレコードを抽出する

        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## タクシー乗車データ シナリオのその他のクエリ スクリプト

[NYC タクシー乗車データ](http://chriswhong.com/open-data/foil_nyc_taxi/) シナリオに固有のクエリの例も、[Github リポジトリ](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)に用意されています。 これらのクエリには、指定されたデータ スキーマが既にあり、すぐに送信して実行できる状態になっています。

 

