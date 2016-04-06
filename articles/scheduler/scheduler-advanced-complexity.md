<properties 
 pageTitle="Azure Scheduler で複雑なスケジュールと高度な繰り返しを構築する方法" 
 description="" 
 services="scheduler" 
 documentationCenter=".NET" 
 authors="krisragh" 
 manager="dwrede" 
 editor=""/>
<tags 
 ms.service="scheduler" 
 ms.workload="infrastructure-services" 
 ms.tgt_pltfrm="na" 
 ms.devlang="dotnet" 
 ms.topic="article" 
 ms.date="12/04/2015" 
 ms.author="krisragh"/>

# Azure Scheduler で複雑なスケジュールと高度な繰り返しを構築する方法  

## 概要

ジョブは、Azure Scheduler の中核となる、 *スケジュール*します。 スケジュールは、Scheduler でジョブを実行するタイミングと方法を決定します。

Azure Scheduler では、ジョブに対して 1 回限りの別々のスケジュールと、定期的なスケジュールを指定できます。 *1 回限り* 効果的にスケジュールは、指定した時間に 1 回起動、される *定期的な* 1 回だけ実行をスケジュールします。 定期的なスケジュールは、事前に定義された頻度で起動します。

この柔軟性により、Azure Scheduler では、次のようなさまざまなビジネス シナリオをサポートできます。

-   定期的なデータ クリーンアップ: 毎日が 3 か月よりも古いすべてのツイートを削除する例。
-   アーカイブ: たとえば、毎月、バックアップ サービスに請求書の履歴を送信する
-   外部データの要求: たとえば、15 分ごとに、NOAA から新しいスキー場の気象レポートを取得する
-   画像処理: たとえば、すべての平日のピーク時以外に、クラウド コンピューティングを利用して、その日にアップロードされた画像を圧縮する


この記事では、Azure Scheduler で作成できるジョブの例を、手順に沿って説明します。 説明には、各スケジュールを表す JSON データを使用します。 使用する場合、 [Scheduler REST API](https://msdn.microsoft.com/library/azure/dn528946.aspx), 、この同じ JSON を使用することができます [Azure Scheduler ジョブを作成する](https://msdn.microsoft.com/library/azure/dn528937.aspx)です。

## サポートされるシナリオ

このトピックに示す多くの例では、Azure Scheduler がサポートするさまざまなシナリオについて説明します。 これらの例では、多くの動作パターンに対応するスケジュールの作成方法を幅広く示します。以下に、いくつかの例を示します。

-   特定の日時に 1 回実行する
-   明示した回数だけ実行を繰り返す
-   すぐに実行し繰り返す 
-   実行を繰り返すたび *n* 分、時間、日、週、または特定の時刻に開始月
-   実行し、特定の日、週の特定の日または月の特定の日でのみが、毎週または毎月の頻度で再び発生します。
-   ある期間内で複数回実行を繰り返す。たとえば、毎月の最終金曜日と月曜日、毎日の午前 5 時 15 分と午後 5 時 15 分など

## 日付と日付/時刻

Azure Scheduler ジョブにおける日付に従って、 [ISO 8601 仕様](http://en.wikipedia.org/wiki/ISO_8601) 日付のみを含みます。

次の Azure Scheduler ジョブにおける日付/時刻の参照、 [ISO 8601 仕様](http://en.wikipedia.org/wiki/ISO_8601) 日付と時刻の両方のパーツが含まれています。 UTC オフセットを指定していない日付/時刻は UTC と見なされます。  

## 方法: JSON や REST API を使用した、スケジュールの作成

この記事と、Azure Scheduler REST API の JSON の例を使用して単純なスケジュールを作成する [最初にクラウド サービスの作成](https://msdn.microsoft.com/library/azure/dn528943.aspx), 、[ジョブ コレクションを作成する](https://msdn.microsoft.com/library/azure/dn528940.aspx), 、および [最後にジョブを作成](https://msdn.microsoft.com/library/azure/dn528937.aspx)します。 ジョブを作成するときに、JSON を使用して、以下に抜粋したようなスケジュール設定と繰り返しを指定できます。

    {
        "startTime": "2012-08-04T00:00Z", // optional
         …
        "recurrence":                     // optional
        {
            "frequency": "week",     // can be "year" "month" "day" "week" "hour" "minute"
            "interval": 1,                // optional, how often to fire (default to 1)
            "schedule":                   // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // optional (default to recur infinitely)
            "endTime": "2012-11-04",      // optional (default to recur infinitely)
        },
        …
    }
    
## 概要: ジョブのスキーマの基礎

次の表に、ジョブの繰り返しとスケジュール設定に関連する主要な要素の概要を示します。 

|**JSON での名前**|**説明**|
|:--|:--|
|**_startTime_**|_startTime_ 日付時刻します。 簡単なスケジュール _startTime_ は最初の発生せず、複雑なスケジュールの場合、ジョブが同時に起動 _startTime_します。|
|**_recurrence_**| _繰り返し_ オブジェクト、ジョブの繰り返しの規則を指定し、ジョブが実行されます。 Recurrence オブジェクトは、要素をサポートしている _間隔、間隔、endTime、count、_ と _スケジュール_します。 場合 _定期的な_ が定義されている _頻度_ が必要です。 その他の要素 _定期的な_ は省略可能です。|
|**_frequency_**| _頻度_ ジョブが繰り返し出現頻度の単位を表す文字列。 サポートされる値は _"minute"、"hour"、"day"、"week"_ または _"month"_|
|**_interval_**| _間隔_ 正の整数は、間隔を示します、 _頻度_ ジョブを実行する頻度を決定します。 たとえば場合、 _間隔_ は 3 と _頻度_ "week"の場合は、ジョブには 3 週間おきが繰り返されます。 Azure Scheduler でサポートする最大 _間隔_ の 18 か月、週単位の頻度の 78 週、または毎日の実行は 548 日の月単位の頻度] にします。 時間と分単位の頻度は、サポートされている範囲は、1 < = _間隔_ < = 1000年です。|
|**_endTime_**| _EndTime_ 文字列で指定した日付と時刻を過ぎると、ジョブは実行されません。 無効です、 _endTime_ 過去にします。 ない場合 _endTime_ または数を指定すると、ジョブは無限に実行します。 両方とも _endTime_ と _カウント_ 同じジョブに対して指定することはできません。|
|**_count_**|<p> _カウント_ は、正の整数 (0 より大きい) このジョブが完了する前に実行する回数を指定します。</p><p> _カウント_ 完了済みとして判定されるまで、ジョブの実行回数を表します。 毎日実行されるジョブなどの _カウント_ 5 と月曜日の開始日、金曜日に実行すると、ジョブが完了します。 開始日が過去の場合は、最初の実行は、作成時刻から計算されます。</p><p>ない場合 _endTime_ または _カウント_ を指定すると、ジョブは無限に実行します。 両方とも _endTime_ と _カウント_ 同じジョブに対して指定することはできません。</p>|
|**_スケジュール_**|ジョブに頻度を指定すると、繰り返しのスケジュールに基づいて、そのジョブの繰り返しを変更できます。 A _スケジュール_ 分、時間、曜日、月の日および週の番号に基づいた変更が含まれています。|

## 概要: ジョブのスキーマの既定値、制限、および例

この概要の後で、これらの各要素の詳細について説明します。

|**JSON での名前**|**値の型**|**必須**|**既定値**|**有効な値**|**例**|
|:---|:---|:---|:---|:---|:---|
|**_startTime_**|String|いいえ|なし|ISO-8601 の日付/時刻|<code>"startTime" : "2013-01-09T09:30:00-08:00"</code>|
|**_recurrence_**|オブジェクト|いいえ|なし|recurrence オブジェクト|<code>"recurrence" : { "frequency" : "monthly", "interval" : 1 }</code>|
|**_frequency_**|String|あり|なし|"minute"、"hour"、"day"、"week"、"month"|<code>"frequency" : "hour"</code> |
|**_interval_**|Number|いいえ|1|1 ～ 1000。|<code>"interval":10</code>|
|**_endTime_**|String|いいえ|なし|将来の時刻を表す日付/時刻の値|<code>"endTime" : "2013-02-09T09:30:00-08:00"</code> |
|**_count_**|Number|いいえ|なし|>= 1|<code>"count": 5</code>|
|**_スケジュール_**|オブジェクト|いいえ|なし|schedule オブジェクト|<code>"schedule" : { "minute" : [30], "hour" : [8,17] }</code>|

## 詳細: _startTime_

次の表では _startTime_ ジョブの実行方法を制御します。

|**startTime の値**|**繰り返しなし**|**定期的なアイテム。 スケジュールなし**|**スケジュールありの繰り返し**|
|:--|:--|:--|:--|
|**開始時刻なし**|すぐに 1 回実行|すぐに 1 回実行。 2 回目以降のジョブは、最後の実行時間から計算して実行|<p>すぐに 1 回実行</p><p>後続のジョブは定期的なスケジュールに基づいて実行します。</p>|
|**開始時刻が過去に設定されている**|すぐに 1 回実行|<p>開始時刻後に将来の最初の実行時刻を計算し、その時点で実行</p><p>最後の実行時刻から oncalculating の目以降のジョブを実行します。</p><p>詳細については、この表の後の例を参照してください。</p>|<p>ジョブは _同時に_ 指定した開始時刻。 最初に見つかった位置が、開始時刻から計算されたスケジュールに基づく</p><p>後続のジョブは定期的なスケジュールに基づいて実行します。</p>|
|**開始時刻が将来または現在に設定されている**|指定した開始時刻に 1 回実行|<p>指定した開始時刻に 1 回実行</p><p>2 回目以降のジョブは、最後の実行時間から計算して実行</p>|<p>ジョブは _同時に_ 指定した開始時刻。 最初に見つかった位置が、開始時刻から計算されたスケジュールに基づく</p><p>後続のジョブは定期的なスケジュールに基づいて実行します。</p>|

動作の例を見て、 _startTime_ では、以前は、 _定期的な_ は _スケジュール_します。  現在の時刻が 2015年-04-08 であると仮定 13時 00分 _startTime_ 2015年-04-07 は、14時 00分と _定期的な_ 2 日ごと (で定義されている _頻度_: 日と _間隔_: 2 です)。なお、 _startTime_ 、過去にあり、現在の時刻より前に発生します。

これらの条件下で、 _最初の実行_ 2015年-04-09 14:00\ になります。 Scheduler エンジンは、開始時刻から実行を計算します。  過去のインスタンスはすべて破棄されます。 エンジンは、将来発生する次回のインスタンスを使用します。  したがって、この場合、 _startTime_ 午後 2 時 2015年-04-07 は、次のインスタンスがその時点では、2015年-04-09 を午後 2 時から 2 日であるためです。 

最初の実行が、注も startTime 2015-04-05 をする 14 時 00 分または 2015年-04-01 14:00\ します。 最初の実行後、スケジュールを使用して 2 回目以降の実行が計算されます。よって、2 回目以降の実行は、2015 年 4 月 11 日午後 2 時 00 分、2015 年 4 月 13 日午後 2 時 00 分、2015 年 4 月 15 日午後 2 時 00 分のように続きます。

最後に、ジョブにスケジュールが設定されている場合に、スケジュールに時および/または分が設定されていないと、既定では、最初の実行に指定した時および/または分が、それぞれ使用されます。

## 詳細: _スケジュール_

一方で、 _スケジュール_ できます _制限_ ジョブの実行の数。  たとえば、ジョブに"month"の頻度は、 _スケジュール_ 31 日にのみの実行、ジョブ実行することのみがある月に、31 で<sup>st</sup> 日です。

一方で、 _スケジュール_ こともできます _展開_ ジョブの実行の数。 たとえば、ジョブに"month"の頻度は、 _スケジュール_ 1 と 2 月の日に実行、ジョブで実行する、1<sup>st</sup> と 2<sup>nd</sup> の月に 1 回ではなく月の日。

schedule の要素を複数指定した場合は、評価の順序は大きい要素から小さい要素の順序となります。つまり、週番号、月の日、曜日、時間、および分という順序です。

次の表 _スケジュール_ の要素を詳しくします。

|**JSON での名前**|**説明**|**有効な値**|
|:---|:---|:---|
|**分**|ジョブを実行する時刻 (分)|<ul><li>整数、または</li><li>整数の配列</li></ul>|
|**hours**|ジョブを実行する時刻 (時)|<ul><li>整数、または</li><li>整数の配列</li></ul>|
|**weekDays**|ジョブを実行する曜日。 週単位の頻度だけを指定できます。|<ul><li>"Monday"、"Tuesday"、"Wednesday"、"Thursday"、"Friday"、"Saturday"または"Sunday"</li><li>上記の値 (最大配列サイズは 7) のいずれかの配列</li></ul>_いない_ 大文字小文字を区別|
|**monthlyOccurrences**|ジョブを実行する月の日にちを指定します。 月単位の頻度だけを指定できます。|<ul><li>MonthlyOccurence オブジェクトの配列。</li></ul> <pre>{"day": _日_,、<br />  "occurrence": _に出現する位置_<br />}</pre><p> _日_ は週の曜日、ジョブの実行、{sunday} は毎週日曜日の月のです。 必須。</p><p>発生する _に出現する位置_ 、月間の日付の例: {Sunday,-1} は月の最終日曜日という意味です。 省略可能。</p>|
|**monthDays**|ジョブが実行される月の日にち。 月単位の頻度だけを指定できます。|<ul><li>任意の値 < =-1 と >-31 します。</li><li>任意の値 > 1 = と < 31 です。</li><li>上記の値の配列</li></ul>|

## 例: 繰り返しのスケジュール

次に、schedule オブジェクトとそのサブ要素に注目して、さまざまな繰り返しの例を示します。

次のすべてのスケジュールであると想定して、 _間隔_ が 1 に設定します。 また、1 ついると仮定適切な頻度では、 _スケジュール_ – たとえば frequency に"day"を使用してをスケジュールして"monthDays"を変更できません。 そのような制限は、上で説明しています。

|**例**|**説明**|
|:---|:---|
|<code>{"hours":[5]}</code>|毎日午前 5 時に実行。 Azure Scheduler は "hours" の値を、"minutes" の値と 1 つずつ突き合わせ、ジョブを実行するすべての時刻の一覧を作成します。|
|<code>{"minutes":[15],"hours":[5]}</code>|毎日午前 5 時 15 分に実行|
|<code>{"minutes":[15],"hours":[5,17]}</code>|毎日午前 5 時 15 分と 午後 5 時 15 分に実行|
|<code>{"minutes":[15,45],"hours":[5,17]}</code>|毎日午前 5 時 15 分、午前 5 時 45 分、午後 5 時 15 分、および午後 5 時 45 分に実行|
|<code>{"minutes":[0,15,30,45]}</code>|15 分ごとに実行|
|<code>{hours":[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23]}</code>|1 時間ごとに実行。 このジョブは、1 時間ごとに実行されます。 分? 궸뫮궢궲궵궻믴뱗똛궚귞귢귡궔궼갂 _aspect、 _startTime_, 、指定されている場合、または指定されていない場合、作成時刻順です。 たとえば、開始時刻または作成時刻が午後 12 時 25 分である場合 (どちらが適用される場合でも)、ジョブは 0 時 25 分、1 時 25 分、2 時 25 分...23 時 25 分に実行されます。 スケジュールはジョブと同等 _頻度_ "hour"、 _間隔_ 1、および no _スケジュール_します。 違いは、このスケジュールを別に使用できることを _頻度_ と _間隔_ も他のジョブを作成します。 たとえば場合、 _頻度_ が"month"スケジュールは動作毎日ではなく月 1 回だけ _頻度_ が"day"|
|<code>{minutes:[0]}</code>|毎正時に実行。 このジョブも 1 時間ごとに実行されますが、正時に実行されます。(例、午前 12 時、午前 1 時、午前 2 時)。これは、frequency に "hour"、startTime に 0 分を設定し、さらに頻度が "day" であれば、schedule を設定しなかったジョブと同等です。ただし、頻度が "week" または "month" の場合は、このスケジュールは、それぞれ週 に 1 日だけ、または月に 1 日だけ実行します。|
|<code>{"minutes":[15]}</code>|正時から 15 分経過後に実行。 1 時間ごとに実行。午前 0 時 15 分から開始し、午前 1 時 15 分、午前 2 時 15 分...午後 10 時 15 分と続き、最後に午後 11 時 15 分に実行します。|
|<code>{"hours":[17],"weekDays":["saturday"]}</code>|毎週土曜日の午後 5 時に実行|
|<code>{hours":[17],"weekDays":["monday","wednesday","friday"]}</code>|毎週月曜日、水曜日、および金曜日の午後 5 時に実行|
|<code>{"minutes":[15,45],"hours":[17],"weekDays":["monday","wednesday","friday"]}</code>|毎週月曜日、水曜日、および金曜日の午後 5 時 15 分と午後 5 時 45 分に実行|
|<code>{"hours":[5,17],"weekDays":["monday","wednesday","friday"]}</code>|毎週月曜日、水曜日、および金曜日の午前 5 時 と午後 5 時に実行|
|<code>{"minutes":[15,45],"hours":[5,17],"weekDays":["monday","wednesday","friday"]}</code>|毎週月曜日、水曜日、および金曜日の午前 5 時 15 分、午前 5 時 45 分、午後 5 時 15 分、および午後 5 時 45 分に実行|
|<code>{"minutes":[0,15,30,45], "weekDays":["monday","tuesday","wednesday","thursday","friday"]}</code>|平日に 15 分ごとに実行|
|<code>{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday","tuesday","wednesday","thursday","friday"]}</code>|平日の午前 9 時と午後 4 時 45 分の間、15 分ごとに実行|
|<code>{"weekDays":["sunday"]}</code>|日曜日の開始時刻に実行|
|<code>{"weekDays":["tuesday", "thursday"]}</code>|火曜日と木曜日の開始時刻に実行|
|<code>{"minutes":[0],"hours":[6],"monthDays":[28]}</code>|毎月 28 日の午前 6 時に実行 (頻度を月と仮定した場合)|
|<code>{"minutes":[0],"hours":[6],"monthDays":[-1]}</code>|月の最終日の午前 6 時に実行。 月の最終日にジョブを実行する場合は、28、29、30、または 31 という日にちではなく -1 を使用します。|
|<code>{"minutes":[0],"hours":[6],"monthDays":[1,-1]}</code>|毎月の最初と最後の日の午前 6 時に実行|
|<code>{monthDays":[1,-1]}</code>|毎月の最初と最後の日の開始時刻に実行|
|<code>{monthDays":[1,14]}</code>|毎月 1 日と 14 日の開始時刻に実行|
|<code>{monthDays":[2]}</code>|月の 2 日の開始時刻に実行|
|<code>{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday","occurrence":1}]}</code>|毎月の最初の金曜日の午前 5 時に実行|
|<code>{"monthlyOccurrences":[{"day":"friday","occurrence":1}]}</code>|毎月の最初の金曜日の開始時刻に実行|
|<code>{"monthlyOccurrences":[{"day":"friday","occurrence":-3}]}</code>|毎月、月の最後から 3 番目の金曜日の開始時刻に実行|
|<code>{"minutes":[15],"hours":[5],"monthlyOccurrences":[{"day":"friday","occurrence":1},{"day":"friday","occurrence":-1}]}</code>|毎月の最初と最後の金曜日の午前 5 時 15 分に実行|
|<code>{"monthlyOccurrences":[{"day":"friday","occurrence":1},{"day":"friday","occurrence":-1}]}</code>|毎月の最初と最後の金曜日の開始時刻に実行|
|<code>{"monthlyOccurrences":[{"day":"friday","occurrence":5}]}</code>|毎月の第 5 金曜日の開始時刻に実行 第 5 金曜日にのみ実行するスケジュールとなっているため、月に第 5 金曜日がない場合は、これは実行されません。 月の最終金曜日にジョブを実行する場合は、occurrence に対して 5 ではなく -1 を使用することを検討してください。|
|<code>{"minutes":[0,15,30,45],"monthlyOccurrences":[{"day":"friday","occurrence":-1}]}</code>|月の最終金曜日に 15 分ごとに実行|
|<code>{"minutes":[15,45],"hours":[5,17],"monthlyOccurrences":[{"day":"wednesday","occurrence":3}]}</code>|毎月第 3 水曜日の午前 5 時 15 分、午前 5 時 45 分、午後 5 時 15 分、および午後 5 時 45 分に実行|

## 関連項目
 

 [Scheduler とは](scheduler-intro.md)
 
 [Azure Scheduler の概念、用語集、エンティティ階層構造](scheduler-concepts-terms.md)

 [Azure ポータル内で Scheduler を使用した作業開始](scheduler-get-started-portal.md)

 [Azure Scheduler のプランと課金](scheduler-plans-billing.md)

 [Azure Scheduler REST API リファレンス](https://msdn.microsoft.com/library/dn528946)

 [Azure Scheduler PowerShell コマンドレット リファレンス](scheduler-powershell-reference.md)

 [Azure Scheduler の高可用性と信頼性](scheduler-high-availability-reliability.md)

 [Azure Scheduler の制限、既定値、エラー コード](scheduler-limits-defaults-errors.md)

 [Azure Scheduler 送信認証](scheduler-outbound-authentication.md)
 
  


