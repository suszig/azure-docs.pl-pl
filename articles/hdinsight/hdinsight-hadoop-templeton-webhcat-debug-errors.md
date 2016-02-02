<properties
 pageTitle="HDInsight の WebHCat エラーの説明と解決策"
 description="HDInsight で WebHCat から返される一般的なエラーの原因とその解決策について説明します。"
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="paulettm"
 editor="cgronlun"
 tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="12/04/2015"
 ms.author="larryfr"/>


# HDInsight で WebHCat (Templeton) から受信したエラーの説明と解決策

WebHCat (旧称 Templeton) を使用して HDInsight を使用するときに、エラーが発生することがあります。 このドキュメントでは、一般的なエラーのガイダンスとして、エラーが発生する理由と、そのエラーを解決策として考えられるものを説明します。

## WebHCat とは

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) 用の REST API は、 [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), 、Hadoop のテーブルおよびストレージの管理レイヤー。 WebHCat は、HDInsight クラスターで既定で有効になっており、ジョブの送信、ジョブの状態の取得などの操作をクラスターにログインすることなく行うために、さまざまなツールで使用されます。

## 構成の変更

> [AZURE.IMPORTANT] このドキュメントに示すエラーのいくつかは、構成されている最大値が超過したことが原因で発生します。 解決策に値が変更可能であることが示されている場合は、次のどちらかの方法で値を変更する必要があります。

* **Windows** クラスターの場合: スクリプト アクションを使用して、クラスターの作成時に値を構成します。 詳細については、次を参照してください。 [script action の開発](hdinsight-hadoop-script-actions.md)します。

* **Linux** クラスターの場合: Ambari (Web または REST API) を使用して値を変更します。 詳細については、を参照してください [Ambari を使用した HDInsight の管理](hdinsight-hadoop-manage-ambari.md)。

### 既定の構成

WebHCat のパフォーマンスに影響を与えたり、超過したときにエラーが発生したりする既定の構成値を次に示します。

| 設定| 実行内容| 既定値|
| ------- | ------------ | ------------- |
| [yarn.scheduler.capacity.maximum-アプリケーションの ][maximum-applications]| 同時にアクティブにできるジョブの最大数 (保留中または実行中)| 10,000|
| [templeton.exec.max procs ][max-procs]| 同時に処理できる要求の最大数| 20|
| [mapreduce.jobhistory.max 年齢 ms ][max-age-ms]| ジョブの履歴が保持される日数| 7 日|

## 要求が多すぎます

**HTTP 状態コード**: 429

| 原因| 解決策|
| ----- | ---------- |
| WebHCat によって 1 分間に処理される同時要求の最大数を超過した (既定値は 20)。| 送信しないことをようにワークロードを複数の同時要求の最大数を減らすか、変更することで同時要求の上限を増やして `templeton.exec.max procs`します。参照してください [構成の変更](#modifying-configuration) の詳細|

## Server unavailable

**HTTP 状態コード**: 503

| 原因| 解決策|
| ---------------- | ------------------- |
| これは、通常、クラスターのプライマリ ヘッドノードとセカンダリ ヘッドノードの間のフェールオーバー中に発生します。| 2 分待ってから操作をやり直してください。|

## Bad request Content: Could not find job

**HTTP 状態コード**: 400

| 原因| 解決策|
| ---------------- | ------------------- |
| ジョブ履歴クリーナーによってジョブの詳細がクリーンアップされた。| ジョブ履歴の既定の保有期間は 7 日間です。これは、変更によって変更できます `mapreduce.jobhistory.max 年齢 ms`します。参照してください [構成の変更](#modifying-configuration) の詳細|
| フェールオーバーによってジョブが強制終了した。| 最大で 2 分間にわたってジョブの送信を再試行してください。|
| 無効なジョブ ID が使用された。| ジョブ ID が正しいことを確認してください。|

## Bad gateway

**HTTP 状態コード**: 502

| 原因| 解決策|
| ---------------- | ------------------- |
| WebHCat プロセス内で内部ガベージ コレクションが行われている。| ガベージ コレクションが終了するまで待つか、または WebHCat サービスを再起動します。|
| ResourceManager サービスからの応答の待機中にタイムアウトが発生した。アクティブなアプリケーションの数が構成された最大値 (既定値は 10,000) に達した場合に発生することがあります。| 現在実行中のジョブを完了するかを変更して同時実行ジョブの制限を上げます待つ `yarn.scheduler.capacity.maximum アプリケーション`します。参照してください [構成の変更](#modifying-configuration) の詳細|
| すべてのジョブを取得中に、 [GET/jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) 中に呼び出し `フィールド` に設定されている  `*`| 取得しない *すべて* ジョブの詳細、代わりに使用して `jobid` 特定のジョブ id を超えるジョブの詳細を取得します。または、使用しない `フィールド`|
| ヘッドノードのフェールオーバー中に WebHCat サービスがダウンした。| 2 分待ってから操作をやり直してください。|
| WebHCat を通じて送信された 500 個を超えるジョブが保留中になっている。| 現在保留中のジョブが完了するのを待ってから、次のジョブを送信します。|


[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html 
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html 
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml 

