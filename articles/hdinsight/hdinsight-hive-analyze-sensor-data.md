<properties
    pageTitle="Hive と Hadoop を使用したセンサー データの分析 | Microsoft Azure"
    description="Hive クエリ コンソールを HDInsight (Hadoop) と共に使用してセンサー データを分析してから、Microsoft Excel の Power View を使用してデータを視覚化する方法について説明します。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/04/2015" 
    ms.author="larryfr"/>

#HDInsight の Hadoop で Hive クエリ コンソールを使用したセンサー データの分析

Hive クエリ コンソールを HDInsight (Hadoop) と共に使用してセンサー データを分析してから、Microsoft Excel の Power View を使用してデータを視覚化する方法について説明します。

> [AZURE.NOTE] このドキュメントの手順では、Windows ベースの HDInsight クラスターにのみ機能します。

このサンプルでは、Hive を使用して、暖房、換気、および空調 (HVAC) システムによって生成された履歴データを処理し、設定した温度を正確に維持できないシステムを識別します。 学習内容:

- 値をコンマで区切った (CSV) 形式で格納されたデータを照会するための HIVE テーブルの作成
- データを分析するための HIVE クエリの作成
- 分析したデータを取得するための、Microsoft Excel を使用した HDInsight への接続 (Open Database Connectivity (ODBC) を使用)
- Power View を使用したデータの視覚化

![A diagram of the solution architecture](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

##前提条件

* HDInsight (Hadoop) クラスター: を参照してください [HDInsight のプロビジョニングの Hadoop クラスター](hdinsight-provision-clusters.md) については、クラスターを作成します。

* Microsoft Excel 2013

    > [AZURE.NOTE] Microsoft Excel を使用したデータ視覚化の使用する [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US)します。

* [Microsoft Hive ODBC ドライバー](http://www.microsoft.com/download/details.aspx?id=40886)

##サンプルを実行するには

1. Web ブラウザーで次の URL に移動します。 `<clustername>` を、使用する HDInsight クラスターの名前に置き換えます。

        https://<clustername>.azurehdinsight.net

    入力を要求されたら、このクラスターをプロビジョニングするときに使用した管理者ユーザー名とパスワードを使用して認証を実行します。

2. 表示された web ページをクリックして、 **Getting Started ギャラリー** ] タブで、下にある、 **Solutions with Sample Data** カテゴリ、] をクリックして、 **センサー データの分析** サンプルです。

3. Web ページに記載されている手順に従って、サンプルを完了します。

