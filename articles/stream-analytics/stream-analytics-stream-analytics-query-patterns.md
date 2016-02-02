<properties
    pageTitle="Stream Analytics の一般的使用状況パターンのクエリ例 | Microsoft Azure"
    description="一般的な Azure Stream Analytics クエリのパターン "
    keywords="query examples"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="12/04/2015"
    ms.author="jeffstok"/>



# 一般的 Stream Analytics 使用状況パターンのクエリ例

## はじめに

Azure Stream Analytics でのクエリは示されている SQL に似たクエリ言語で表現 [ここ](https://msdn.microsoft.com/library/azure/dn834998.aspx)します。 このドキュメントでは、実際のシナリオに基づいて、いくつかの一般的なクエリ パターンの対処方法について説明します。 このドキュメントは作成中であり、継続的に新しいパターンで更新されます。

## クエリ例: データ型の変換

**説明**: 入力ストリームに、プロパティの型を定義します。
例: 車の重量が文字列として入力ストリームに送信されるとを実行する INT に変換する必要があることを合計します。

**入力**:

| 保存する| 時刻| 重量|
| --- | --- | --- |
| Honda| 2015-01-01T00:00:01.0000000Z| "1000"|
| Honda| 2015-01-01T00:00:02.0000000Z| "2000"|

**出力**:

| 保存する| 重量|
| --- | --- |
| Honda| 3000|

**解決策**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**説明**:
Weight フィールドに対して CAST ステートメントを使用して、その型を指定 (サポートされているデータ型の一覧を参照してください [ここ](https://msdn.microsoft.com/library/azure/dn835065.aspx))します。

## クエリ例: Like/Not like を使用したパターン マッチング

**説明**: イベントのフィールドの値が、一定のパターンと一致することを確認します。
例: が A で起動し、9 で終わるライセンス プレートを返す

**入力**:

| 保存する| LicensePlate| 時刻|
| --- | --- | --- |
| Honda| ABC-123| 2015-01-01T00:00:01.0000000Z|
| Toyota| AAA-999| 2015-01-01T00:00:02.0000000Z|
| Nissan| ABC-369| 2015-01-01T00:00:03.0000000Z|

**出力**:

| 保存する| LicensePlate| 時刻|
| --- | --- | --- |
| Toyota| AAA-999| 2015-01-01T00:00:02.0000000Z|
| Nissan| ABC-369| 2015-01-01T00:00:03.0000000Z|

**解決策**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**説明**:
LIKE ステートメントを使用して、LicensePlate フィールドの値 A で始まる、任意の文字列の 0 個以上の文字と 9 で終わることを確認します。

## クエリ例: 異なるケース/値に異なるロジックを指定する (CASE ステートメント)

**説明**: フィールドのいくつかの条件に基づいて異なる計算を提供します。
例: 1 の特殊なケースを行うのと同じ数の車の文字の説明を提供します。

**入力**:

| 保存する| 時刻|
| --- | --- |
| Honda| 2015-01-01T00:00:01.0000000Z|
| Toyota| 2015-01-01T00:00:02.0000000Z|
| Toyota| 2015-01-01T00:00:03.0000000Z|

**出力**:

| CarsPassed| 時刻|
| --- | --- | --- |
| 1 Honda| 2015-01-01T00:00:10.0000000Z|
| 2 Toyotas| 2015-01-01T00:00:10.0000000Z|

**解決策**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**説明**:
CASE 句では、(この例では、集計ウィンドウでの車の台数) でいくつかの条件に基づいて異なる計算を適用できます。

## クエリ例: 複数の出力にデータを送信する

**説明**: 1 つのジョブから複数の出力ターゲットにデータを送信します。
例: しきい値ベースのアラートのデータを分析し、blob ストレージへのすべてのイベントのアーカイブ

**入力**:

| 保存する| 時刻|
| --- | --- |
| Honda| 2015-01-01T00:00:01.0000000Z|
| Honda| 2015-01-01T00:00:02.0000000Z|
| Toyota| 2015-01-01T00:00:01.0000000Z|
| Toyota| 2015-01-01T00:00:02.0000000Z|
| Toyota| 2015-01-01T00:00:03.0000000Z|

**出力 1**:

| 保存する| 時刻|
| --- | --- |
| Honda| 2015-01-01T00:00:01.0000000Z|
| Honda| 2015-01-01T00:00:02.0000000Z|
| Toyota| 2015-01-01T00:00:01.0000000Z|
| Toyota| 2015-01-01T00:00:02.0000000Z|
| Toyota| 2015-01-01T00:00:03.0000000Z|

**出力 2**:

| 保存する| 時刻| カウント|
| --- | --- | --- |
| Toyota| 2015-01-01T00:00:10.0000000Z| 3|

**解決策**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time
    
    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**説明**:
INTO 句は、Stream Analytics を指示すると、このステートメントからデータを書き込む出力のです。
1 番目のクエリは、受け取ったデータを ArchiveOutput という名前の出力にパススルーします。
2 番目のクエリは、簡単な集計とフィルター処理を行い、結果を下流のアラート システムに送信します。
*注*: 複数の出力ステートメントで (WITH ステートメント) Cte の結果を再利用することもできます-これは、入力ソースに以下のリーダーを開いた場合の利点です。
例。

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## クエリ例: ユニークな値のカウント

**説明**: 時間枠内にストリームに表示される固有のフィールド値の数をカウントします。
例: 方法 2 の 2 番目のウィンドウに料金所を通過した自動車の一意の多くは、ですか?

**入力**:

| 保存する| 時刻|
| --- | --- |
| Honda| 2015-01-01T00:00:01.0000000Z|
| Honda| 2015-01-01T00:00:02.0000000Z|
| Toyota| 2015-01-01T00:00:01.0000000Z|
| Toyota| 2015-01-01T00:00:02.0000000Z|
| Toyota| 2015-01-01T00:00:03.0000000Z|

**出力:**

| カウント| 時刻|
| --- | --- |
| 2| 2015-01-01T00:00:02.000Z|
| 1| 2015-01-01T00:00:04.000Z|

**解決策:**

    WITH Makes AS (
        SELECT
            Make,
            COUNT(*) AS CountMake
        FROM
            Input TIMESTAMP BY Time
        GROUP BY
              Make,
              TumblingWindow(second, 2)
    )
    SELECT
        COUNT(*) AS Count,
        System.TimeStamp AS Time
    FROM
        Makes
    GROUP BY
        TumblingWindow(second, 1)

**説明:**
ウィンドウの数で一意では取得するために最初の集計を行います。
その後、得られたメーカーの数を集計します。期間内のすべてのユニークな値を渡して同じタイムスタンプを取得した後、最初のステップの2 つの期間を集計しないように 2 番目の集計期間を最小化する必要があります。

## クエリ例: 値が変化したかどうかを判定する

**説明**: 前の値が現在の値と異なるかを判断
例: 前の自動車は、現在の自動車と同じメーカー有料道路上でしょうか。

**入力**:

| 保存する| 時刻|
| --- | --- |
| Honda| 2015-01-01T00:00:01.0000000Z|
| Toyota| 2015-01-01T00:00:02.0000000Z|

**出力**:

| 保存する| 時刻|
| --- | --- |
| Toyota| 2015-01-01T00:00:02.0000000Z|

**解決策**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**説明**:
入力ストリーム イベントを 1 つ前を調べて、Make の値を取得するには、LAG を使用します。 次に、現在のイベントの Make と比較し、異なる場合はイベントを出力します。

## クエリ例: 期間内の最初のイベントを検索する

**説明**: 10 分間隔で最初の自動車を検索します。

**入力**:

| LicensePlate| 保存する| 時刻|
| --- | --- | --- |
| DXE 5291| Honda| 2015-07-27T00:00:05.0000000Z|
| YZK 5704| Ford| 2015-07-27T00:02:17.0000000Z|
| RMV 8282| Honda| 2015-07-27T00:05:01.0000000Z|
| YHN 6970| Toyota| 2015-07-27T00:06:00.0000000Z|
| VFE 1616| Toyota| 2015-07-27T00:09:31.0000000Z|
| QYF 9358| Honda| 2015-07-27T00:12:02.0000000Z|
| MDR 6128| BMW| 2015-07-27T00:13:45.0000000Z|

**出力**:

| LicensePlate| 保存する| 時刻|
| --- | --- | --- |
| DXE 5291| Honda| 2015-07-27T00:00:05.0000000Z|
| QYF 9358| Honda| 2015-07-27T00:12:02.0000000Z|

**解決策**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

問題を変更して、10 分ごとに特定のメーカーの最初の自動車を検索します。

| LicensePlate| 保存する| 時刻|
| --- | --- | --- |
| DXE 5291| Honda| 2015-07-27T00:00:05.0000000Z|
| YZK 5704| Ford| 2015-07-27T00:02:17.0000000Z|
| YHN 6970| Toyota| 2015-07-27T00:06:00.0000000Z|
| QYF 9358| Honda| 2015-07-27T00:12:02.0000000Z|
| MDR 6128| BMW| 2015-07-27T00:13:45.0000000Z|

**解決策**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## クエリ例: 期間内の最後のイベントを検索する

**説明**: 10 分間隔で最後の自動車を検索します。

**入力**:

| LicensePlate| 保存する| 時刻|
| --- | --- | --- |
| DXE 5291| Honda| 2015-07-27T00:00:05.0000000Z|
| YZK 5704| Ford| 2015-07-27T00:02:17.0000000Z|
| RMV 8282| Honda| 2015-07-27T00:05:01.0000000Z|
| YHN 6970| Toyota| 2015-07-27T00:06:00.0000000Z|
| VFE 1616| Toyota| 2015-07-27T00:09:31.0000000Z|
| QYF 9358| Honda| 2015-07-27T00:12:02.0000000Z|
| MDR 6128| BMW| 2015-07-27T00:13:45.0000000Z|

**出力**:

| LicensePlate| 保存する| 時刻|
| --- | --- | --- |
| VFE 1616| Toyota| 2015-07-27T00:09:31.0000000Z|
| MDR 6128| BMW| 2015-07-27T00:13:45.0000000Z|

**解決策**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**説明**:
クエリである 2 つの手順 – 1 つ目は、10 分の期間で最新のタイムスタンプを検索します。 第 2 のステップで、第 1 のクエリの結果と元のストリームを結合し、各期間で最後のタイムスタンプに一致するイベントを検索します。

## クエリ例: イベントがないことを検出する

**説明**: ストリームに、特定の条件に一致する値が含まれていないことを確認します。
例: 同じが 2 回続けて車に入力した有料道路 90 秒以内を教えてください。

**入力**:

| 保存する| LicensePlate| 時刻|
| --- | --- | --- |
| Honda| ABC-123| 2015-01-01T00:00:01.0000000Z|
| Honda| AAA-999| 2015-01-01T00:00:02.0000000Z|
| Toyota| DEF-987| 2015-01-01T00:00:03.0000000Z|
| Honda| GHI-345| 2015-01-01T00:00:04.0000000Z|

**出力**:

| 保存する| 時刻| CurrentCarLicensePlate| FirstCarLicensePlate| FirstCarTime|
| --- | --- | --- | --- | --- |
| Honda| 2015-01-01T00:00:02.0000000Z| AAA-999| ABC-123| 2015-01-01T00:00:01.0000000Z|

**解決策**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**説明**:
入力ストリーム イベントを 1 つ前を調べて、Make の値を取得するには、LAG を使用します。 次にそれを現在のイベントの Make と比較して、それらが同じ場合はイベントを出力し、LAG を使用して前の自動車についてのデータを取得します。

## クエリ例: 条件の期間を検出する

**説明**: の状態が発生した期間を検索します。
例が正しくないすべての自動車の原因となったバグ (20,000 ポンド超) – バグの期間を計算します。

**入力**:

| 保存する| 時刻| 重量|
| --- | --- | --- |
| Honda| 2015-01-01T00:00:01.0000000Z| 2000|
| Toyota| 2015-01-01T00:00:02.0000000Z| 25000|
| Honda| 2015-01-01T00:00:03.0000000Z| 26000|
| Toyota| 2015-01-01T00:00:04.0000000Z| 25000|
| Honda| 2015-01-01T00:00:05.0000000Z| 26000|
| Toyota| 2015-01-01T00:00:06.0000000Z| 25000|
| Honda| 2015-01-01T00:00:07.0000000Z| 26000|
| Toyota| 2015-01-01T00:00:08.0000000Z| 2000|

**出力**:

| StartFault| EndFault| FaultDurationSeconds|
| --- | --- | --- |
| 2015-01-01T00:00:01.0000000Z| 2015-01-01T00:00:08.0000000Z| 7|
| 2015-01-01T00:00:01.0000000Z| 2015-01-01T00:00:08.0000000Z| 7|
| 2015-01-01T00:00:01.0000000Z| 2015-01-01T00:00:08.0000000Z| 7|
| 2015-01-01T00:00:01.0000000Z| 2015-01-01T00:00:08.0000000Z| 7|
| 2015-01-01T00:00:01.0000000Z| 2015-01-01T00:00:08.0000000Z| 7|
| 2015-01-01T00:00:01.0000000Z| 2015-01-01T00:00:08.0000000Z| 7|

**解決策**:

    SELECT
        PrevGood.Time AS StartFault,
        ThisGood.Time AS Endfault,
        DATEDIFF(second, PrevGood.Time, ThisGood.Time) AS FaultDuraitonSeconds
    FROM
        Input AS ThisGood TIMESTAMP BY Time
        INNER JOIN Input AS PrevGood TIMESTAMP BY Time
        ON DATEDIFF(second, PrevGood, ThisGood) BETWEEN 1 AND 3600
        AND PrevGood.Weight < 20000
        INNER JOIN Input AS Bad TIMESTAMP BY Time
        ON DATEDIFF(second, PrevGood, Bad) BETWEEN 1 AND 3600
        AND DATEDIFF(second, Bad, ThisGood) BETWEEN 1 AND 3600
        AND Bad.Weight >= 20000
        LEFT JOIN Input AS MidGood TIMESTAMP BY Time
        ON DATEDIFF(second, PrevGood, MidGood) BETWEEN 1 AND 3600
        AND DATEDIFF(second, MidGood, ThisGood) BETWEEN 1 AND 3600
        AND MidGood.Weight < 20000
    WHERE
        ThisGood.Weight < 20000
        AND MidGood.Weight IS NULL

**説明**:
2 つの正常なイベントが不適切なイベントの間に、2 つのイベントが 1 つ以上の異常なイベントの前後に、最初のイベントをつまり間に、良好なイベントなしを募集しています。 間に 1 つの異常イベントがある 2 つの正常イベントを取得するには、2 つの JOIN を使用し、重みを調べてタイムスタンプを比較することによって正常 -> 異常 -> 正常を確認します。

「NULL が含まれる、またはイベントを含まない左外部結合」で学習したことを使用して、選択した 2 つの正常イベントの間で正常イベントが発生していないことを確認する方法を知ります。

これらを組み合わせて、間に他の正常イベントがない正常 -> 異常 -> 正常を取得します。 開始正常イベントと終了正常イベントの間の期間を計算して、バグの期間が得られます。

## 問い合わせ

詳細については、当社を参照してください [Azure Stream Analytics フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)。

## 次のステップ

- [Azure Stream Analytics の概要](stream-analytics-introduction.md)
- [Azure Stream Analytics の使用を開始します。](../stream.analytics.get.started.md)
- [Azure Stream Analytics ジョブをスケーリングします。](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics クエリ言語リファレンス](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API リファレンス](https://msdn.microsoft.com/library/azure/dn835031.aspx)






