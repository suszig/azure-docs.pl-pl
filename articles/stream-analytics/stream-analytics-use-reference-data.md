<properties 
    pageTitle="Stream Analytics で参照データとルックアップ テーブルを使用する | Microsoft Azure" 
    description="Stream Analytics クリエで参照データを使用する" 
    keywords="lookup table, reference data"
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
    ms.workload="data-services" 
    ms.date="12/04/2015" 
    ms.author="jeffstok"/>

# Stream Analytics の入力ストリームでの参照データまたはルックアップ テーブルの使用

参照データ (別名、ルックアップテーブル) は、静的または本来はあまり変更されない有限のデータ セットです。参照の実行やデータ ストリームとの相互の関連付けに使用されます。 Azure Stream Analytics ジョブ内の参照データを使用するのには一般的に使用して、 [参照データ Join](https://msdn.microsoft.com/library/azure/dn949258.aspx) 、クエリにします。 Stream Analytics は、参照データのストレージ レイヤーとして Azure Blob ストレージを使用して Azure Data Factory の参照を持つデータでく変換やおよびから参照データとして使用するための Azure Blob ストレージにコピー [と任意の数のクラウド ベースの内部設置型のデータ ストア](./articles/data-factory-data-movement-activities.md)します。 参照データは、BLOB (入力構成に定義された) のシーケンスとしてモデル化され、BLOB の名前内で指定された日付/時刻の昇順で並べられます。 これは、 **のみ** は日付/時刻を使用して、シーケンスの末尾に追加をサポート **大きい** シーケンスの最後の blob で指定されたものです。

## 参照データの構成

参照データを構成するには、まず型の入力を作成する必要が **参照データ**します。 次の表では、参照データ入力とその説明を作成するときに指定する必要がある各プロパティについて説明します。

<table>
<tbody>
<tr>
<td>プロパティ名</td>
<td>説明</td>
</tr>
<tr>
<td>入力のエイリアス</td>
<td>この入力を参照するジョブ クエリで使用されるわかりやすい名前。</td>
</tr>
<tr>
<td>ストレージ アカウント</td>
<td>BLOB ファイルが配置されるストレージ アカウントの名前。 Stream Analytics のジョブと同じサブスクリプションにある場合は、ドロップ ダウンから選択することができます。</td>
</tr>
<tr>
<td>ストレージ アカウント キー</td>
<td>ストレージ アカウントに関連付けられている秘密キー。 ストレージ アカウントが Stream Analytics のジョブと同じサブスクリプションにある場合は、自動的に設定されます。</td>
</tr>
<tr>
<td>ストレージ コンテナー</td>
<td>コンテナーにより、Microsoft Azure BLOB サービスに格納される BLOB が論理的にグループ化されます。 BLOB を BLOB サービスにアップロードするとき、その BLOB のコンテナーを指定する必要があります。</td>
</tr>
<tr>
<td>パスのパターン</td>
<td>指定されたコンテナー内に BLOB を配置するために使用されるファイル パス。 このパス内に、次の 2 つの変数のいずれかまたは両方のインスタンスを指定できます。<BR>{date}、{time}<BR>例 1: products/{date}/{time}/product-list.csv<BR>例 2: products/{date}/product-list.csv
</tr>
<tr>
<td>日付形式 [省略可能]</td>
<td>指定したパス パターン内で {date} を使用した場合は、サポートされている形式のドロップ ダウンから、ファイルを編成する日付形式を選択できます。 例: YYYY/MM/DD</td>
</tr>
<tr>
<td>時刻形式 [省略可能]</td>
<td>指定したパス パターン内で {time} を使用した場合は、サポートされている形式のドロップ ダウンから、ファイルを編成する時刻形式を選択できます。 例: HH</td>
</tr>
<tr>
<td>イベントのシリアル化の形式</td>
<td>クエリを予想どおりに動作させるには、入ってくるデータ ストリームに使用しているシリアル化形式が Stream Analytics で認識される必要があります。 参照データの場合、サポートされている形式は CSV と JSON です。</td>
</tr>
<tr>
<td>エンコード</td>
<td>現在のところ、UTF-8 が、唯一サポートされているエンコード形式です。</td>
</tr>
</tbody>
</table>

## スケジュールに従った参照データの生成

参照データが変更頻度の低いデータセットである場合、参照データの更新をサポートするには、{date} および {time} トークンを使用する入力構成でパス パターンを指定します。 Stream Analytics はこのパス パターンに基づいて、更新された参照データ定義を取得します。 たとえば、日付形式が "YYYY-MM-DD" で、時刻形式が "HH:mm" の ````"/sample/{date}/{time}/products.csv"```` は、更新された BLOB ````"/sample/2015-04-16/17:30/products.csv"```` を UTC タイム ゾーンの 2015 年 4 月 16 日の午後 5:30 に回収するように Stream Analytics に通知します。

> [AZURE.NOTE] 現在のコンピューター時間が、blob 名でエンコードされた時刻と一致する場合にのみ、Stream Analytics ジョブは、blob の更新を探します。 たとえば、ジョブは、/sample/2015-04-16/17:30/products.csv を、2015 年 4 月 16 日 UTC タイム ゾーンの午後 5 時 30 分と午後 5 時 30 分 59.9 秒の間に検索します。 マシン クロックが 5:31PM になると、/sample/2015-04-16/17:30/products.csv の検索は停止され、/sample/2015-04-16/17:31/products.csv の検索が開始されます。 これに対する例外は、ジョブが時間をさかのぼってデータを再処理する必要がある場合、またはジョブが最初に開始される場合です。 開始時点で、ジョブは、指定されたジョブ開始時刻より前に生成された最新の BLOB を探します。 これは、ジョブの開始時に空ではない参照データ セットが必ず存在するようにするために実行されます。 それが見つからない場合、ジョブは失敗し、診断通知がユーザーに表示されます。

[Azure Data Factory](http://azure.microsoft.com/documentation/services/data-factory/) Stream Analytics での参照データの定義を更新するために必要な更新された blob を作成するタスクを調整するために使用できます。 Data Factory は、データの移動や変換を調整し自動化するクラウドベースのデータ統合サービスです。 Data Factory はサポート [多数のクラウドへの接続のベースと内部設置型のデータ ストア](./articles/data-factory-data-movement-activities.md) と指定した定期的なスケジュールでデータを簡単に移動します。 詳細とステップ バイ ステップ ガイダンスを事前に定義されたスケジュールで更新する Stream Analytics の参照データを生成する Data Factory パイプラインを設定する方法については、この確認 [GitHub サンプル](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs)します。

## 参照データの更新に関するヒント ##

1. 参照データの BLOB を上書きしても、Stream Analytics は BLOB の再読み込みを行いません。場合によっては、その上書きが原因でジョブが失敗することがあります。 参照データを変更することをお勧めはジョブの入力で定義した同じコンテナーとパスのパターンを使用して、新しい blob を追加し、日付/時刻を使用して、 **大きい** シーケンスの最後の blob で指定されたものです。
2.  参照データの BLOB の並び替えは、BLOB の “最終変更” 時刻では行われません。{date} および {time} の置換文字を使用して BLOB 名に指定されている時刻と日付によってのみ行われます。
3.  場合によって、ジョブは時間をさかのぼる必要があります。したがって、参照データの BLOB は変更することも削除することもできません。

## 問い合わせ
詳細については、参照してください、 [Azure Stream Analytics フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## 次のステップ
モ ノのインターネットからのデータをストリーム分析する管理サービスである、 Stream Analytics の概要です。 このサービスの詳細については、以下の情報をご覧ください。

- [Azure Stream Analytics の使用](stream-analytics-get-started.md)
- [Azure Stream Analytics ジョブのスケーリング](stream-analytics-scale-jobs.md)
- [Stream Analytics Query Language Reference (Stream Analytics クエリ言語リファレンス)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics management REST API reference (Azure ストリーム分析の管理 REST API リファレンス)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

