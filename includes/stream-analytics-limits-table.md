<properties 
   pageTitle="Stream Analytics に関する制限の表"
   description="Stream Analytics コンポーネントおよび接続のシステム制限および推奨サイズについて説明します。"
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="10/22/2015"
   ms.author="jeffstok" />

| 制限の種類 | 制限       | 説明 |
|----------------- | ------------|--------- |
| リージョンあたりのサブスクリプションごとのストリーミング ユニットの最大数 | 50 | 連絡して、サブスクリプションを 50 より大きいにできる場合は、ストリーミング ユニットを増やすには要求 [Microsoft サポート](https://support.microsoft.com/en-us)します。 |
| ストリーミング ユニットの最大スループット | 1 MB/秒* | SU ごとの最大スループットは、シナリオによって異なります。 実際のスループットは低下する可能性があり、クエリの複雑さとパーティション分割によって異なります。 詳細な情報については記載されて、 [スループットの向上のための Scale Azure Stream Analytics ジョブ](../articles/stream-analytics/stream-analytics-scale-jobs.md) 記事です。 |
| SELECT ステートメントのクエリに関する制限 | クエリごとに 5 つの出力 | この制限は将来に増加する可能性があります。 |


