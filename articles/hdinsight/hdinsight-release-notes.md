<properties
    pageTitle="Azure HDInsight の Hadoop コンポーネントのリリース ノート | Microsoft Azure"
    description="Azure HDInsight の Hadoop コンポーネントの最新のリリース ノートとバージョン。 Hadoop、Apache Storm、HBase に関する開発のヒントと詳細情報を示します。"
    services="hdinsight"
    documentationCenter=""
    editor="cgronlun"
    manager="paulettm"
    authors="nitinme"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/01/2015"
    ms.author="nitinme"/>


# Azure HDInsight の Hadoop コンポーネントのリリース ノート

## HDInsight 3.1 の 2015 年 12 月 2 日リリース ノート

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight (Windows) 2.1.10.763.1931434 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight (Windows) 3.0.6.763.1931434 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight (Windows) 3.1.4.763.1931434 (HDP 2.1.15.0-2374 - 変更なし)
* HDInsight (Windows) 3.2.7.763.1931434 (HDP 2.2.7.1-34 - 変更なし)
* HDInsight (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34 - 変更なし)
* HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* SDK 1.5.8

このリリースには、次の更新プログラムが含まれています。

| タイトル                                           | 説明                                          | 影響を受ける領域 (例: サービス、コンポーネント、SDK) | クラスターの種類 (例: Hadoop、HBase、Storm) | JIRA (該当する場合) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| すべての HDInsight クラスターに HDInsight 3.3 バージョンが追加され、HDInsight バージョンが更新されました | このリリースでは、HDInsight v3.3 (HDP 2.3 に基づく) が追加され、他の HDP バージョンも更新されました。 HDP 2.3 リリース ノートは [ここ](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) HDInsight のバージョンの詳細についてを参照して [ここ](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-component-versioning/)します。| サービス    | すべて| 該当なし

## HDInsight の 2015 年 11 月 30 日リリース ノート

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight (Windows) 2.1.10.757.1923908 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight (Windows) 3.0.6.757.1923908 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight (Windows) 3.1.4.757.1923908 (HDP 2.1.15.0-2374 - 変更なし)
* HDInsight (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* SDK 1.5.8

このリリースには、次の更新プログラムが含まれています。

| タイトル                                           | 説明                                          | 影響を受ける領域 (例: サービス、コンポーネント、SDK) | クラスターの種類 (例: Hadoop、HBase、Storm) | JIRA (該当する場合) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| すべての HDInsight クラスターの HDInsight バージョンと HDInsight 3.2 クラスターの HDP バージョンの更新 (Windows と Linux) | このリリースでは、HDInsight と HDP のバージョンが更新されています | サービス    | すべて| 該当なし


## HDInsight の 2015 年 10 月 27 日リリース ノート

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight (Windows) 2.1.10.726.1866228 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight (Windows) 3.0.6.726.1866228 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight (Windows) 3.1.4.726.1866228 (HDP 2.1.15.0-2374 - 変更なし)
* HDInsight (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* SDK 1.5.8

このリリースには、次の更新プログラムが含まれています。

| タイトル                                           | 説明                                          | 影響を受ける領域 (例: サービス、コンポーネント、SDK) | クラスターの種類 (例: Hadoop、HBase、Storm) | JIRA (該当する場合) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| すべての HDInsight クラスターの HDInsight のバージョンの更新 (Windows と Linux) | このリリースでは、HDInsight と HDP のバージョンが更新されています | サービス    | すべて| 該当なし
| 大文字のクラスターを含む Jupyter for Windows Spark クラスターを修正しました | DNS 名が大文字で指定されたクラスターは、元の要求チェックのため、Jupyter Notebook で問題が発生していました。 今回の修正では、Jupyter の構成の DNS 名を小文字に変更されました。 | サービス    | HDInsight Spark (Windows)| 該当なし


## HDInsight 3.1 の 2015 年 10 月 20 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* HDInsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* SDK 1.5.8

このリリースには、次の更新プログラムが含まれています。

| タイトル                                           | 説明                                          | 影響を受ける領域 (例: サービス、コンポーネント、SDK) | クラスターの種類 (例: Hadoop、HBase、Storm) | JIRA (該当する場合) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 既定 HDP バージョンが HDP 2.2 に変更されました | HDInsight Windows クラスターの既定のバージョンが HDP 2.2 に変更されました。 HDInsight バージョン 3.2 (HDP 2.2) が 2015 年 2 月から一般公開されました。 この変更によって、Azure ポータル、PowerShell コマンドレット、または SDK を使用してクラスターをプロビジョニングするときに、クラスター バージョンを明示的に選択していない場合の既定のバージョンのみが変わります。 | サービス    | すべて| 該当なし                  |
|1 つの Virtual Network の Linux クラスターに複数の HDInsight をデプロイする場合の VM 名の形式の変更 | 今回のリリースで、1 つの仮想ネットワークに複数の HDInsight Linux クラスターをデプロイできるようになりました。 この作業の一環として、クラスター内の仮想マシン名の形式が headnode\ *、workernode\ * および zookeepernode\ * からに変更 hn\ *、wn\ * および zk\ * それぞれ。 仮想ネットワーク名は変更される可能性があるため、仮想ネットワーク名の形式に直接依存することは推奨されません。 ローカル コンピューターで "hostname -f" を使用するか、Ambari API を使用して、ホスト一覧と、コンポーネントからホストへのマッピングを取得してください。 さらに詳しい情報を検索する [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) と [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md)します。 | サービス | Linux 上の HDInsight クラスター | 該当なし |
| 構成の変更 | HDInsight 3.1 クラスターでは、次の構成は有効になりました。 <ul><li>tez.yarn.ats.enabled yarn.log.server.url. これにより、アプリケーション Timeline Server とログ サーバーのログを処理するためにできるようになります。</li></ul>HDInsight 3.2 クラスターの次の構成が変更されています。 <ul><li>mapreduce.fileoutputcommitter.algorithm.version を 2 に設定されています。 これにより、FileOutputCommitter の V2 バージョンを使用できます。</li></ul> | サービス | すべて | 該当なし |


## HDInsight の 2015 年 9 月 9 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.675.1768697 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight 3.0.6.675.1768697 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight 3.1.4.675.1768697 (HDP 2.1.15.0-2334 - 変更なし)
* HDInsight 3.2.6.675.1768697 (HDP 2.2.6.1-0012 - 変更なし)
* SDK 1.5.8

このリリースには、次の更新プログラムが含まれています。

| タイトル                                           | 説明                                          | 影響を受ける領域 (例: サービス、コンポーネント、SDK) | クラスターの種類 (例: Hadoop、HBase、Storm) | JIRA (該当する場合) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| すべての HDInsight クラスターの HDInsight のバージョンの更新 | このリリースでは、HDInsight のバージョンが更新されています | サービス    | すべて| 該当なし                  |

## HDInsight の 2015 年 7 月 31 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.640.1695824 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight 3.0.6.640.1695824 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight 3.1.4.640.1695824 (HDP 2.1.15.0-2334 - 変更なし)
* HDInsight 3.2.6.640.1695824 (HDP 2.2.6.1-0012 - 変更なし)
* SDK 1.5.8

このリリースには、次の更新プログラムが含まれています。

| タイトル                                           | 説明                                          | 影響を受ける領域 (例: サービス、コンポーネント、SDK) | クラスターの種類 (例: Hadoop、HBase、Storm) | JIRA (該当する場合) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Spark クラスター ノードの再イメージ化ワークフローの修正 | 再イメージ化後に Spark クラスター ノードが復元できないバグを修正しました | サービス    | Spark| 該当なし                  |


## HDInsight の 2015 年 7 月 31 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.635.1684502 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight 3.0.6.635.1684502 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight 3.1.4.635.1684502 (HDP 2.1.15.0-2334 - 変更なし)
* HDInsight 3.2.6.635.1684502 (HDP 2.2.6.1-0012 - 変更なし)
* SDK 1.5.8

このリリースには、次の更新プログラムが含まれています。

| タイトル                                           | 説明                                          | 影響を受ける領域 (例: サービス、コンポーネント、SDK) | クラスターの種類 (例: Hadoop、HBase、Storm) | JIRA (該当する場合) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| すべての HDInsight クラスターの HDInsight のバージョンの更新 | このリリースでは、HDInsight のバージョンが更新されています | サービス    | すべて| 該当なし                  |


## HDInsight の 2015 年 7 月 7 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.610.1630216 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight 3.0.6.610.1630216 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight 3.1.4.610.1630216 (HDP 2.1.15.0-2334 - 変更なし)
* HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* SDK 1.5.8


このリリースには、次の更新プログラムが含まれています。

| タイトル                                           | 説明                                          | 影響を受ける領域 (例: サービス、コンポーネント、SDK) | クラスターの種類 (例: Hadoop、HBase、Storm) | JIRA (該当する場合) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDInsight 3.2 クラスターの更新された HDP バージョン | このリリースでは、HDInsight 3.2 は HDP 2.2.6.1-0012 をデプロイします | サービス    | すべて                                                 | 該当なし                  |


## HDInsight の 2015 年 06 月 26 日リリース ノート

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.601.1610731 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight 3.0.6.601.1610731 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight 3.1.4.601.1610731 (HDP 2.1.15.0-2334 - 変更なし)
* HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* SDK 1.5.8


このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>HDInsight 3.2 クラスターの更新された HDP バージョン</td>
<td>このリリースでは、HDInsight 3.2 は HDP 2.2.6.1 をデプロイします</td>
<td>サービス</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

</table>

## HDInsight の 2015 年 06 月 18 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.596.1601657 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight 3.0.6.596.1601657 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* SDK 1.5.8


このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>追加で開かれた HTTPS ポート</td>
<td>クラウド サービスがクラスターなどの 5 つのポート 8001 に 8005 を開くようになりました https:// で<clustername>.azurehdinsight.net:8001/ します。 これらの URL に対する要求は、ポート 443 と同じ基本認証パスワード メカニズムを使用して認証されます。 これらのポートは、アクティブなヘッドノード上の同じポートにバインドされます。 ヘッド ノードとクラスターの外部へのルートでこれらのポートでリッスンする顧客サービスを作成するスクリプトのアクションを使用できます。</td>
<td>クラウド サービス</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>HDInsight 3.2 の断続的な MapReduce のシャッフルの問題</td>
<td>たまにタスクの失敗で大規模なクラスターで Mapreduce のシャッフルまれな断続的な競合条件を修正します。 参照してください <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">mapreduce-6361</a> の詳細。</td>
<td>Hadoop Core</td>
<td>すべて</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE-6361</a></td>
</tr>

<tr>
<td>HDInsight 3.2 向けの最新の Azure Java SDK 2.2 への移行</td>
<td>WASB ドライバーによって使用される Azure SDK for Java の最新バージョンに移行しました。 最新の SDK にはいくつかの修正が加えられています。リリース ノートは、https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt で入手できます。</td>
<td>Hadoop Core</td>
<td>すべて</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP-11959</a></td>
</tr>

<tr>
<td>HDInsight 3.1 クラスター向けの HDP 2.1.15 への移行</td>
<td>リリースでは、Hortonworks リリース ノートは <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">ここ</a>します。</td>
<td>HDP</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

</table>

## HDInsight の 2015 年 06 月 4 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.583.1575584 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight 3.0.6.583.1575584 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight 3.1.3.583.1575584 (HDP 2.1.12.1-0003 - 変更なし)
* HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* SDK 1.5.8


このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>Storm クラスターの 502 bad gateway エラーの修正</td>
<td>このリリースでは、再起動後に Web サイトがダウンする原因となるジョブの送信 API に影響を及ぼすバグを修正しました。</td>
<td>サービス</td>
<td>Storm</td>
<td>該当なし</td>
</tr>

</table>

## HDInsight の 2015 年 06 月 1 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.577.1563827 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight 3.0.6.577.1563827 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight 3.1.3.577.1563827 (HDP 2.1.12.1-0003 - 変更なし) の場合)
* HDInsight 3.2.4.577.1563827 (HDP 2.2.6.0-2800 - 変更なし)
* SDK 1.5.8


このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>さまざまなバグの修正</td>
<td>このリリースでは、クラスターのプロビジョニングに関連するバグを修正しました。</td>
<td>サービス</td>
<td>すべてのクラスターの種類</td>
<td>該当なし</td>
</tr>

</table>

## HDInsight の 2015 年 5 月 27 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* その他のクラスター バージョンおよび SDK は、このリリースの一部としてデプロイされません。


このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>HDP 2.2 の更新</td>
<td>HDInsight 3.2 のこのリリースには HDP 2.2.6 が含まれ、HDInsight のいくつかの重要なバグが修正されています。 完全なリリース ノートについては、「 <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 のリリース ノート</a>します。</td>
<td>HDP</td>
<td>すべてのクラスターの種類</td>
<td>該当なし</td>
</tr>

<tr>
<td>既定の Yarn コンテナーのメモリ構成の変更</td>
<td>この更新では、Node Manager によって起動される YARN コンテナー (yarn.nodemanager.resource.memory-mb および yarn.scheduler.maximum-allocation-mb) が既定で使用できるメモリが 5,632 MB に増えました。 従来この値は 4,608 MB に減らされていましたが、さまざまなジョブの実行に基づいて、この新しい値の方がほとんどのジョブにおいてより高い信頼性とパフォーマンスが得られることが確認されたため、より適切な既定値として採用されました。 従来どおり、このメモリ構成に対する重要な依存関係がある場合は、クラスターの作成時に明示的に設定してください。</td>
<td>HDP</td>
<td>すべてのクラスターの種類</td>
<td>該当なし</td>
</tr>

<tr>
<td>HBase クラスターと Storm クラスターの既定の構成パリティ</td>
<td>この更新では、Hbase クラスターと Storm クラスターが Hadoop クラスターと同じ YARN 構成の値を使用するように再び設定されました。 これは、すべてのクラスターの種類のパリティに適用されます。</td>
<td>HDP</td>
<td>HBase、Storm</td>
<td>該当なし</td>
</tr>

</table>

## HDInsight の 2015 年 5 月 20 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.564.1542093 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight 3.0.6.564.1542093 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* SDK 1.5.8

このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>SCP.NET EventHub のサポート</td>
<td>HDInsight Storm の更新されたクラスター パッケージにより、SCP.NET の新しい機能が提供されます。 トポロジ ビルダーで、EventHubSpout または Java スパウトをより簡単に使用できる新しい API にアクセスできます。 コントラクトが更新されているため、新しいクラスターを使用するには、SCP.NET クライアント SDK を更新する必要があります。 新しい API の詳細、使用法、および (バグの修正を含む) リリース ノートについては、SCP.NET nuget パッケージに含まれる Readme を参照してください。</td>
<td>VS のツール</td>
<td>Storm HDInsight 3.2 クラスター</td>
<td>該当なし</td>
</tr>

<tr>
<td>JDBC ドライバーの更新</td>
<td>sqljdbc_4.1.5605.100 で、SQL Server でサポートされているバージョンにドライバーを更新しました。</td>
<td>メタストア</td>
<td>すべて</td>
<td>該当なし</td>
</tr>
</table>

## HDInsight の 2015 年 4 月 27 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.537.1486660 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight 3.0.6.537.1486660 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight 3.1.3.537.1486660 (HDP 2.1.12.0-2329 - 変更なし)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK 1.5.8

このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>DLL 依存関係の修正</td>
<td>単体テスト フレームワークで HDInsight 依存関係を削除しました。</td>
<td>SDK</td>
<td>Hadoop は、</td>
<td>該当なし</td>
</tr>

<tr>
<td>競合状態のバグの修正</td>
<td>クラスター作成要求が状態をポーリングする前に PUT 要求の受理を待機するようになりました。</td>
<td>SDK</td>
<td>Hadoop は、</td>
<td>該当なし</td>
</tr>
</table>

## HDInsight の 2015 年 4 月 14 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - 変更なし)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6

このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>Tez バグの修正</td>
<td>Apache TEZ 2214 と TEZ 1923 の修正はこの HDI 3.2 リリースに含まれます。 これらは、Tez で大量のデータのシャッフルを必要とする特定の Hive クエリ に特に必要です。
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## HDInsight の 2015 年 4 月 6 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - 変更なし)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - 変更なし)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - 変更なし)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6

このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>HDInsight .NET SDK 1.5.6</td>
<td>Linux で HDInsight の内部クラスを一部削除する更新を実施します。</td>
<td>SDK</td>
<td>Hadoop は、</td>
<td>該当なし</td>
</tr>

<tr>
<td>Avro Library 1.5.6</td>
<td>追加 <b>KnownTypeAttribute</b> メソッドの <b>GetAllKnownTypes</b>します。 GetAllKnownTypes メソッドの型が Null の場合の NullReferenceException を修正しました。</td>
<td>SDK</td>
<td>Hadoop は、</td>
<td>該当なし</td>
</tr>

<tr>
<td>バグの修正</td>
<td>サービスに対するさまざまなバグの修正</td>
<td>サービス</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

</table>
<br>

## HDInsight の 2015 年 4 月 1 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK 1.5.5

このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>.NET SDK を介した Windows クラスターでのリモート デスクトップの資格情報を有効または無効にする機能</td>
<td>Windows クラスターで RDP 資格情報の有効化または無効化をプログラムでサポートします。</td>
<td>SDK</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>クラスターのプロビジョニング中にリモート デスクトップ資格情報を有効にする機能</td>
<td>クラスターの作成中にリモート デスクトップの資格情報をプログラムで有効にします。 これにより、クラスターを最初にプロビジョニングしてからリモート デスクトップを有効にするといった 2 段階のプロセスが削除されます。</td>
<td>SDK</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>Python 2.7.8 へのアップグレード</td>
<td>HDInsight クラスターの Python を Python 2.7.8 にアップグレードしました。これには HDInsight バージョン 2.1、3.0、3.1、3.2 の重要なセキュリティ上の修正が含まれます。</td>
<td>サービス</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>YARN 構成の変更</td>
<td>HDInsight バージョン 3.1 と 3.2 のすべてのクラスターの種類について、YARN 構成 yarn.resourcemanager.max-completed-applications を 1000 に変更しました。 この値は、YARN UI で完了したアプリケーションの一覧のみを制御します。 UI に表示されるアプリケーションの一覧の前に送信されたアプリケーションに関する情報は、履歴サーバーから直接入手できます。</td>
<td>YARN</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>HBase クラスターでのノード サイズの変更</td>
<td>HBase クラスターで HDInsight バージョン 3.1 と 3.2 のノード (上下) サイズを変更できるようになりました。</td>
<td>サービス</td>
<td>HBase</td>
<td>該当なし</td>
</tr>

<tr>
<td>JDBC のアップグレード</td>
<td>SQL JDBC ドライバーは、HDInsight バージョン 3.2 の sqljdbc_4.0.2206.100 のバージョンにアップグレードされます。 このバージョンには、重要なセキュリティの機能強化が含まれています。</td>
<td>HDP</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>JVM 構成の更新</td>
<td>HDInsight バージョン 3.1 と 3.2 の JVM 構成 networkaddress.cache.ttl を既定値の -1 から 300 秒に更新しました。 この構成値は、ネーム サービスから正常な名前参照のキャッシュ ポリシーを制御します。 これにより、HBase クラスターの拡張と縮小に関連するバグが修正されます。</td>
<td>サービス</td>
<td>HBase</td>
<td>該当なし</td>
</tr>

<tr>
<td>Azure Storage SDK for Java 2.0 へのアップグレード</td>
<td>HDInsight バージョン 3.2 が Azure Storage SDK for Java の最新バージョンを使用するようアップグレードされます。 これには、現在の 0.6.0 バージョンの重要なバグ修正がいくつか含まれます。</td>
<td>HDP</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>Apache WASB のソース コードへのアップグレード</td>
<td>HDInsight バージョン 3.2 で Apache Hadoop から WASB ファイル システム ドライバー用の最新のコードを使用するようになりました。 この変更により、WASB ドライバー パッケージ、別個
jar です。 これは単にパッケージの変更だけであり、WASB ドライバーの動作への変更は含まれません。 この JAR ファイルの名前は、hadoop-azure-2.6.0.jar です。</td>
<td>HDP</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>HDInsight 3.2 での Jar ファイル名の更新</td>
<td>この HDInsight バージョン 3.2 への更新にはいくつかのバグ修正が含まれ、HDP の一部としてパッケージ化されたいくつかの内部 jar がアップグレードされています。 これらの JAR 注ファイル
内部の HDP パッケージに、お客様のアプリケーションで直接使用しないでください。 HDP へのアップグレードによってお客様のアプリケーションを損なわないように、アプリケーションは独自のバージョンの JAR でパッケージ化する必要があります。</td>
<td>HDP</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

</table>
<br>

## HDInsight の 2015 年 3 月 3 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.488.1375841 (HDP 1.3.9.0-01351 - 変更なし)
* HDInsight 3.0.6.488.1375841 (HDP 2.0.9.0-2097 - 変更なし)
* HDInsight 3.1.3.488.1375841 (HDP 2.1.10.0-2290 - 変更なし)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-変更なし)
* SDK 1.5.0 (変更なし)

このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA</th>
</tr>


<tr>
<td>信頼性に関する機能強化</td>
<td>クラスター作成に関する負荷の増加に応じてスケールが可能なサービスに修正しました。</td>
<td>サービス</td>
<td>すべて</td>
<td>該当なし</td>
</tr>



</table>
<br>

## HDInsight の 2015 年 2 月 18 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.471.1342507 (HDP 1.3.9.0-01351 - 変更なし)
* HDInsight 3.0.6.471.1342507 (HDP 2.0.9.0-2097 - 変更なし)
* HDInsight 3.1.3.471.1342507 (HDP 2.1.10.0-2290 - 変更なし)
* HDInsight 3.2.3.471.1342507 (HDP-2.2.10.0-変更なし)
* SDK 1.5.0

このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>HDInsight 3.2 クラスター</td>
<td>Hadoop 2.6/HDP2.2 は HDInsight 3.2 クラスターで使用できます。 これにはすべてのオープン ソース コンポーネントの主要な更新プログラムが含まれます。 詳細については、「HDInsight の新機能と <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">HDP 2.2.0.0 リリース ノート</a>します。</td>
<td>オープンソース ソフトウェア</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>Linux での HDInsight (プレビュー)</td>
<td>Ubuntu Linux で実行されているクラスターをデプロイできます。 詳細については、「Linux での HDInsight の概要」をご覧ください。</td>
<td>サービス</td>
<td>Hadoop は、</td>
<td>該当なし</td>
</tr>

<tr>
<td>Storm の一般公開</td>
<td>Apache Storm クラスターは一般で使用できます。 詳細については、「HDInsight での Storm 使用方法の概要」をご覧ください。</td>
<td>サービス</td>
<td>Storm</td>
<td>該当なし</td>
</tr>

<tr>
<td>仮想マシン サイズ</td>
<td>Azure HDInsight は、その他の仮想マシンの種類やサイズでも使用可能です。 HDInsight は、汎用目的でビルドされた A2 ～ A7 サイズ、ソリッド ステート ドライブ (SSD) と 60% 高速のプロセッサを特徴とする D シリーズ ノード、高速ネットワーク用 InfiniBand をサポートする A8 と A9 サイズを利用できます。</td>
<td>サービス</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>クラスター スケーリング</td>
<td>実行する HDInsight クラスター用データ ノードの数は、削除または再作成しなくても変更できます。 現時点では、Hadoop クエリと Apache Storm クラスターの種類にのみこの機能がありますが、Apache HBase クラスターの種類も近日中にサポートされます。 詳細については、HDInsight クラスターの管理に関するページを参照してください。</td>
<td>サービス</td>
<td>Hadoop、Storm</td>
<td>該当なし</td>
</tr>

<tr>
<td>Visual Studio ツール</td>
<td>Apache Storm のツール一式に加えて、Visual Studio で Apache Hive のツールが更新され、ステートメント入力候補、ローカル検証、改善されたデバッグ サポートが追加されました。 詳細については、「HDInsight Hadoop Tools for Visual Studio の使用開始」をご覧ください。</td>
<td>ツール</td>
<td>Hadoop は、</td>
<td>該当なし</td>
</tr>

<tr>
<td>DocumentDB 用 Hadoop コネクタ</td>
<td>DocumentDB 用 Hadoop コネクタを使用すれば、DocumentDB コレクション全体またはデータベース アカウント全体に格納されているスキーマのない JSON ドキュメントで複雑な集計、分析、操作を実行できます。 詳細とチュートリアルについては、「DocumentDB と HDInsight を使用した Hadoop ジョブの実行」をご覧ください。</td>
<td>サービス</td>
<td>Hadoop は、</td>
<td>該当なし</td>
</tr>

<tr>
<td>バグの修正</td>
<td>HDInsight サービスに対するさまざまなマイナー バグ修正を行いました。 顧客向け動作に変更はありません。</td>
<td>サービス</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

</table>
<br>

## HDInsight の 2015 年 2 月 6 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.463.1325367 (HDP 1.3.9.0-01351 - 変更なし)
* HDInsight 3.0.6.463.1325367 (HDP 2.0.9.0-2097 - 変更なし)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* SDK の該当なし

このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>バグの修正</td>
<td>HDInsight サービスに対するさまざまなマイナー バグ修正を行いました。 顧客向け動作に変更はありません。</td>
<td>サービス</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>HDP 2.1 メンテナンスの更新</td>
<td>HDInsight 3.1 が更新され、HDP 2.1.10.0 がデプロイされます。 詳細については、次を参照してください。 <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">hdp-2.1.10 リリース ノート</a>します。 </td>
<td>オープンソース ソフトウェア</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>HDP バイナリの更新</td>
<td>HBase の一部の JAR ファイル名が更新されています。 これらの JAR ファイルは HBase で内的に使用するため、顧客がこれらの JAR ファイル名に依存することはありません。 学習した内容は次のとおりです。
<ul>
<li>./lib/jetty-6.1.26.hwx.jar</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.jar</li>
<li>./lib/jetty-util-6.1.26.hwx.jar</li>
</ul>
</td>
<td>オープンソース ソフトウェア</td>
<td>HBase</td>
<td>該当なし</td>
</tr>

</table>
<br>

## HDInsight の 2015 年 1 月 29 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.455.1309616 (HDP 1.3.9.0-01351 - 変更なし)
* HDInsight 3.0.6.455.1309616 (HDP 2.0.9.0-2097 - 変更なし)
* HDInsight 3.1.2.455.1309616 (HDP 2.1.9.0-2196 - 変更なし)
* SDK の該当なし

このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>影響を受ける領域
(たとえば、サービス、コンポーネント、または SDK)</p></th>
<th>クラスターの種類 (例: Hadoop、HBase、Storm)</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>

<td>バグの修正</td>
<td>Azure のアップグレード中に HDInsight クラスターの信頼性を向上させる重要な修正をいくつか行いました。</td>
<td>サービス</td>
<td>すべて</td>
<td>該当なし</td>
</tr>



</table>
<br>

## HDInsight の 2015 年 1 月 5 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.420.1246118 (HDP 1.3.9.0-01351 - 変更なし)
* HDInsight 3.0.6.420.1246118 (HDP 2.0.9.0-2097 - 変更なし)
* HDInsight 3.1.2.420.1246118 (HDP 2.1.9.0-2196 - 変更なし)


このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>コンポーネント</th>
<th>クラスターの種類</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>Twitter の傾向分析と Mahout に基づく映画のリコメンデーションのサンプル</td>
<td><p>このリリースでは、HDInsight クエリ コンソールに 2 つの追加のサンプルがあります。</p>

<p><b>Twitter の傾向分析</b><br>
Twitter などのサイトが公開している API を介して収集したデータは、現在の動向を分析して把握するための有益な情報源となります。 このチュートリアルで Hive を使用して、特定の単語を含むツイートを最も多く送信した Twitter ユーザーの一覧を取得する方法について説明します。 </p>

<p><b>Mahout 映画のリコメンデーション</b><br>
Apache Mahout は、Apache Hadoop の機械学習ライブラリです。 Mahout には、フィルター処理、分類、クラスタリングなどデータを処理するためのアルゴリズムが含まれています。 このチュートリアルでは、リコメンデーション エンジンを使用し、友人たちが鑑賞した映画に基づいて映画のリコメンデーションを生成します。</p></td>
<td>クエリ コンソール</td>
<td>Hadoop は、</td>
<td>該当なし</td>
</tr>

<tr>
<td>Hive 構成 hive.auto.convert.join.noconditionaltask.size の既定値への変更</td>
<td><p>このサイズ構成は、自動変換されたマップの結合に適用されます。 この値は、メモリに収まるハッシュ マップに変換できるテーブルのサイズの合計を表します。 以前のリリースで、この値は既定値の 10MB から 128MB に増えました。 ただし、128MB という新しい値がメモリ不足でジョブが失敗する原因になっていました。 このリリースでは、既定値を 10 MB に戻します。 ユーザーは、クエリとテーブル サイズに応じて、クラスターの作成中にこの値を上書きできます。 この設定およびそれを上書きする方法の詳細については、次を参照してください。 <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">Optimize Auto Join Conversion</a> 参照してください。 </p></td>
<td>Hive</td>
<td>Hadoop、HBase</td>
<td>該当なし</td>
</tr>

</table>
<br>

## HDInsight の 2014 年 12 月 23 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.10.420.1246783 (HDP バージョンは変更なし)
* HDInsight 3.0.6.420.1246783 (HDP バージョンは変更なし)
* HDInsight 3.1.1.420.1246783 (HDP バージョンは変更なし)

このリリースには、次の更新プログラムが含まれています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>コンポーネント</th>
<th>クラスターの種類</th>
<th>JIRA (該当する場合)</th>
</tr>


<tr>
<td>負荷が高すぎることによる断続的なクラスター作成エラー</td>
<td><p>クラスター作成中の HDP パッケージのダウンロード アルゴリズムの改良により、負荷の増加によるエラーをより確実に処理できます。</p></td>
<td>サービス</td>
<td>Hadoop、Hbase、Storm</td>
<td>該当なし</td>
</tr>



</table>
<br>

## HDInsight の 2014 年 12 月 18 日リリース

このリリースでは、コンポーネントに対して以下の更新を実施しています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>コンポーネント</th>
<th>クラスターの種類</th>
<th>JIRA (該当する場合)</th>
</tr>

<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">クラスターのカスタマイズの一般公開</a></td>
<td><p>カスタマイズ機能により、Apache Hadoop エコシステムから利用できるプロジェクトで Azure HDInsight クラスターをカスタマイズできます。 この新しい機能により、Hadoop プロジェクトを実験し、Azure HDInsight にデプロイできます。 これを使用して、 **Script Action** 機能で、カスタム スクリプトを使用して任意の方法で Hadoop クラスターを変更できます。 このカスタマイズ機能は、Hadoop、HBase、および Storm などあらゆる種類の HDInsight クラスターで利用できます。 一般的なインストール プロセスを文書化してこの機能の能力を示すためには、 <a href = "hdinsight-hadoop-spark-install.md" target="_blank">Spark</a>, 、<a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, 、<a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>, 、および <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a> モジュールです。 またこのリリースでは、ユーザーが Azure ポータルを使用してカスタム スクリプト アクションを指定する機能が追加され、ヘルパー メソッドを使用してカスタム スクリプト アクションを作成するためのガイドラインとベスト プラクティスの提供、さらにスクリプト アクションをテストする方法に関するガイドラインが提供されます。 </p></td>
<td>機能の一般公開</td>
<td>すべて</td>
<td>該当なし</td>
</tr>


</table>
<br>

## HDInsight の 2014 年 12 月 5 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* HDInsight SDK N/A

このリリースでは、コンポーネントに対して以下の更新を実施しています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>コンポーネント</th>
<th>クラスターの種類</th>
<th>JIRA (該当する場合)</th>
</tr>

<tr>
<td>バグの修正: Hive DDL でテーブルに大量のパーティションを追加する際に断続的にエラーが発生する。 </td>
<td><p>Hive テーブルに対してパーティションを大量に追加しようとした際に、Hive メタストア データベースとの間に断続的に接続エラーが発生した場合には、Hive DDL にエラーが発生することがあります。 このエラーが発生した場合には、この Hive エラー ログに以下の文言が表示されます。 </p><p>"ERROR [main]: ql.Driver (SessionState.java:printError(547)) - FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. MetaException(message:java.lang.RuntimeException: commitTransaction was called but openTransactionCalls = 0. This probably indicates that there are unbalanced calls to openTransaction/commitTransaction)"</p></td>
<td>Hive</td>
<td>Hadoop、HBase</td>
<td>HIVE-482 (これは、内部 JIRA の一種です。このため、外部から引用することはできません。 ここには参考までに記載しています)</td>
</tr>

<tr>
<td>バグの修正: HDInsight クエリ コンソールが応答を停止することがある</td>
<td>このエラーが発生すると、WebHCat ランチャー ジョブの WebHCat ログに以下の文言が表示されます。 <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): Could not load history file {wasb url to the history file}"</p></td>
<td>WebHCat</td>
<td>Hadoop は、</td>
<td>HIVE-482 (これは、内部 JIRA の一種です。このため、外部から引用することはできません。 ここには参考までに記載しています)</td>
</tr>

<tr>
<td>バグの修正: HBase クエリの待機時間が急激に増加することがある</td>
<td>このエラーが発生すると、HBase クエリによる待ち時間が突然 3 秒に増加します。 </td>
<td>HDInsight クラスター ゲートウェイ</td>
<td>HBase</td>
<td>該当なし</td>
</tr>

<tr>
<td>HDP JAR ファイルの名称変更</td>
<td>HDI クラスター バージョン 3.0 では、HDP によってインストールされた内部 JAR ファイルへの変更のいくつかあります。 jetty-6.1.26.jar が jetty 6.1.26.hwx.jar に置き換えられました。 jetty-6.1.26.jar util は jetty util-6.1.26.hwx.jar に置き換えられました。 これらの変更は、Hadoop、Mahout、WebHCat および Oozie プロジェクトに適用されます。</td>
<td>Hadoop、Mahout、WebHCat、Oozie</td>
<td>Hadoop、HBase</td>
<td>該当なし</td>
</tr>

</table>
<br>


## HDInsight の 2014 年 11 月 25 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.9.382.1169709 (2014 年 11 月 14 日から変更なし)
* HDInsight 3.0.5.382.1169709 (2014 年 11 月 14 日から変更なし)
* HDInsight 3.1.1.382.1169709 (2014 年 11 月 14 日から変更なし)
* HDInsight SDK 1.4.0

このリリースでは、コンポーネントに対して以下の更新を実施しています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>コンポーネント</th>
<th>クラスターの種類</th>
<th>JIRA (該当する場合)</th>
</tr>

<tr>
<td>アプリケーション ログへのアクセス</td>
<td>クラスターで実行されたアプリケーションをプログラムを使って列挙し、アプリケーションに固有またはコンテナーごとのログをダウンロードできるようになりました。この機能は、問題のあるアプリケーションのデバッグに役立ちます。</td>
<td>SDK</td>
<td>Hadoop は、</td>
<td>該当なし</td>
</tr>

<tr>
<td>IHdInsightClient.DeleteCluster でのリージョン名の指定 </td>
<td>Azure HDInsight SDK を使用する場合は、地域名を指定する機能を提供する **DeleteCluster**します。 これまでは、別々のリージョンに同じ名前のリソースが 1 つずつある場合に、どちらかだけを削除することはできませんでした。この更新では、そのような問題が解消します。</td>
<td>SDK</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>ClusterDetails.DeploymentId</td>
<td> **ClusterDetails** オブジェクトを返します。、 **DeploymentID** 、クラスターの一意の識別子を表すフィールドです。 この情報は、同じ名前のクラスターを作成しようとしても常に一意の状態が確保されます。</td>
<td>SDK</td>
<td>すべて</td>
<td>該当なし</td>
</tr>
</table>
<br>

## HDInsight の 2014 年 11 月 14 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

このリリースでは、以下の新機能、コンポーネントの更新、バグ修正が実施されます。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>コンポーネント</th>
<th>クラスターの種類</th>
<th>JIRA (該当する場合)</th>
</tr>

<tr>
<td>スクリプト アクション (プレビュー):</td>
<td>クラスター カスタマイズ機能のプレビュー版であり、カスタム スクリプトを使用して任意の方法で Hadoop クラスターを変更できます。 この機能により、ユーザーは、Apache Hadoop エコシステムから入手できるプロジェクトを検証し、Azure HDInsight クラスターにデプロイできます。 このカスタマイズ機能は、Hadoop、HBase、Storm など、すべての種類の HDInsight クラスターで使用できます。</td>
<td>新機能</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>Azure Websites および Azure Storage のログ分析用に既成のジョブを用意</td>
<td>HDInsight クエリ コンソールには、既にあるデータまたはサンプルのデータを使用するソリューションで役立つ初心者向けギャラリーが用意されています。
<p>**データを使用するソリューション**:<br>
データ分析のなかでも特によくあるもののいくつかについて、ジョブを作成しました。自分でソリューションを作成する際の第一歩として役立ちます。 各ジョブには既にお持ちのデータを使って、どのように動作するかを確認できます。 その後準備ができたら、これまでに得た知識を使って、既成のジョブを手本にソリューションを作成します。</p>
<p>**サンプル データを操作するソリューション**:<br>
Web ログやセンサー データの分析など、基本的なシナリオをいくつかなぞっていくことによって、HDInsight の使い方を学習できます。 HDInsight を使ってデータを分析する方法だけでなく、その他のアプリケーションやサービスにこのデータを接続する方法も学べます。 こちらは、Microsoft Excel に接続してデータを視覚化する場合などに役立ちます。</p></td>
<td>クエリ コンソール</td>
<td>Hadoop は、</td>
<td>該当なし</td>
</tr>

<tr>
<td>Templeton のメモリ リークを修正</td>
<td>クラスターを長時間実行したり、1 秒あたり何百回ものジョブ リクエストを送信したりした場合に問題になっていた Templeton のメモリ リークを修正しました。 この Templeton 5xx という種のエラーについては、回避策としてサービスを再起動する必要がありました。 今回の更新により、再起動は不要になりました。</td>
<td>Templeton</td>
<td>すべて</td>
<td>https://issues.apache.org/jira/browse/HADOOP-11248</td>
</tr>
</table>
<br>


**注**: クラスターのカスタマイズで使用できる新しい機能を示すためには、Script Action を使用してクラスターに Spark と R モジュールをインストールする手順が記載されています。 詳細については、以下を参照してください。

* [HDInsight クラスターに Spark 1.0 をインストールして使用する](hdinsight-hadoop-spark-install.md)
* [HDInsight Hadoop クラスターに R をインストールして使用する](hdinsight-hadoop-r-scripts.md)




## HDInsight の 2014 年 11 月 7 日リリース

今回のリリースがデプロイされている HDInsight クラスターのバージョン番号は、以下のとおりです。

* HDInsight 2.1 2.1.9.374.1153876
* HDInsight 3.0 3.0.5.374.1153876
* HDInsight 3.1 3.1.1.374.1153876

このリリースでは、コンポーネントに対して以下の更新を実施しています。

<table border="1">
<tr>
<th>タイトル</th>
<th>説明</th>
<th>コンポーネント</th>
<th>クラスターの種類</th>
<th>JIRA (該当する場合)</th>
</tr>

<tr>
<td>HDP 2.1.7</td>
<td>このリリースは、Hortonworks Data Platform (HDP) 2.1.7 に基づくものです。 詳細については、次を参照してください。 <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">hdp-2.1.7 リリース ノート</a>します。</td>
<td>HDP</td>
<td>すべて</td>
<td>該当なし</td>
</tr>

<tr>
<td>YARN タイムライン サーバー</td>
<td>YARN Timeline Server (別名 Generic Application History Server) は、既定で有効になっています。 Timeline Server は、アプリケーション ID、アプリケーション名、アプリケーションの状態、アプリケーションの送信時刻、アプリケーションの完了時刻など、完了済みのアプリケーションに関する全般的な情報を提供するものです。

このアプリケーション情報は、ヘッド ノードから URI (http://headnodehost:8188) にアクセスするか、YARN コマンド (yarn application –list –appStates ALL) を実行すると取得できます。

この情報も取得できますリモート https://{ClusterDnsName で、REST api} します。 azurehdinsight.net/ws/v1/applicationhistory/ します。

詳細についてを参照してください。 <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN Timeline Server</a>します。</td>
<td>サービス、YARN</td>
<td>Hadoop、HBase</td>
<td>該当なし</td>
</tr>

<tr>
<td>クラスター デプロイメント ID</td>
<td>SDK の最新バージョン (1.3.3.1.5426.29232) より、ユーザーは、HDInsight が各クラスターに対して発行した一意の ID にアクセスできます。 これにより、作成とドロップのシナリオで DNS 名を再利用する場合に、ユーザーがクラスターのインスタンスを一意に特定できるようになります。</td>
<td>SDK</td>
<td>すべて</td>
<td>該当なし</td>
</tr>
</table>
<br>

**注**: このリリースでポータルに表示されないか、SDK によって、または Windows PowerShell によって返される完全なバージョン番号を妨げるバグが修正されました。

## 2014 年 10 月 15 日リリース

この修正プログラムのリリースでは、Templeton のヘビー ユーザーに影響を与えた Templeton のメモリ リークを修正しました。 場合によっては、要求の実行に十分なメモリがないために、Templeton を何回も実行したユーザーに 500 エラー コードとしてエラーが表示されることがあります。 Templeton サービスを再起動すると、この問題を回避できます。 この問題は修正されています。


## 2014 年 10 月 7 日リリース

* Ambari を使用したときにエンドポイント、"https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}"を *host_name* フィールドは、ホスト名だけではなくノードの完全修飾ドメイン名 (FQDN) を返します。 返す代わりに、たとえば、"**headnode0**「,、fqdn」**headnode0 {。:Clusterdns} .azurehdinsight .net**"です。 この変更は、1 つの仮想ネットワークに HBase や Hadoop などの複数のクラスターの種類をデプロイできるシナリオの実現を容易にするために必須でした。 このシナリオは、Hadoop のバックエンド プラットフォームとして HBase を使用する場合などが該当します。

* HDInsight クラスターの既定のデプロイメントのために、新しいメモリ設定が用意されています。 前の既定のメモリ設定は、デプロイされる CPU コアの数についてガイダンスの考慮が適切ではありませんでした。 今回の新しいメモリ設定は、Hortonworks による推奨に基づき、既定の設定を改善するものです。 メモリ設定の変更については、クラスター構成の変更に関する SDK リファレンス ドキュメントをご覧ください。 既定の 4 CPU コア (8 コンテナー) HDInsight クラスターで使用される新しいメモリ設定を次の表に示します (前のリリースで使用された値もかっこ内に記載します)。

<table border="1">
<tr><th>コンポーネント</th><th>メモリの割り当て</th></tr>
<tr><td> yarn.scheduler.minimum-allocation</td><td>768 MB (以前は 512 MB)</td></tr>
<tr><td> yarn.scheduler.maximum-allocation</td><td>6144 MB (変更なし)</td></tr>
<tr><td>yarn.nodemanager.resource.memory</td><td>6144 MB (変更なし)</td></tr>
<tr><td>mapreduce.map.memory</td><td>768 MB (以前は 512 MB)</td></tr>
<tr><td>mapreduce.map.java.opts</td><td>opts=-X m x 512 m (以前は -X m x 410 m)</td></tr>
<tr><td>mapreduce.reduce.memory</td><td>1536 MB (以前は 1024 MB)</td></tr>
<tr><td>mapreduce.reduce.java.opts</td><td>opts=-X m x 1024 m (以前は -X m x 819 m)</td></tr>
<tr><td>yarn.app.mapreduce.am.resource</td><td>768 MB (以前は 1024 MB)</td></tr>
<tr><td>yarn.app.mapreduce.am.command</td><td>opts=-X m x 512 m (以前は -X m x 819 m)</td></tr>
<tr><td>mapreduce.task.io.sort</td><td>256 MB (以前は 200 MB)</td></tr>
<tr><td>tez.am.resource.memory</td><td>1536 MB (変更なし)</td></tr>

</table><br>

HDInsight で使用される Hortonworks Data Platform で YARN および MapReduce によって使用されるメモリ構成設定の詳細については、次を参照してください。 [HDP メモリ構成設定の決定](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html)します。 Hortonworks は、適切なメモリ設定を計算するツールも提供しています。

Azure PowerShell と HDInsight SDK のエラー メッセージに関連します。"*クラスターが HTTP サービス アクセス用に構成されていない*"。

* このエラーは既知 [互換性の問題](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) HDInsight SDK または Azure PowerShell のバージョンとクラスターのバージョンの相違のために起こることがあります。 8 月 15 日以降に作成されたクラスターは、仮想ネットワークへの新しいプロビジョニング機能をサポートします。 古いバージョンの HDInsight SDK または Azure PowerShell でこの機能が正しく解釈されません。 一部のジョブ送信の操作で失敗する結果になります。 HDInsight SDK Api または Azure PowerShell コマンドレットを使用する場合 (**使用 AzureRmHDInsightCluster** または **Invoke AzureRmHDInsightHiveJob**) ジョブを送信するには、これらの操作が失敗のエラー メッセージで"*クラスター <clustername> HTTP サービスのアクセスが構成されていない*." (操作によっては)、入手することもが他のエラー メッセージなど、"*クラスターに接続できません*"です。

* これらの互換性の問題は、HDInsight SDK および Azure PowerShell の最新バージョンで解決されます。 HDInsight SDK をバージョン 1.3.1.6 以降に、Azure PowerShell ツールをバージョン 0.8.8 以降に更新することをお勧めします。 最新の HDInsight SDK へのアクセスを取得できます [](http://nuget.codeplex.com/wikipage?title=Getting%20Started) と Azure PowerShell ツールに [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure/)します。



## HDInsight 3.1 の 2014 年 9 月 12 日リリース

* このリリースは、Hortonworks Data Platform (HDP) 2.1.5 に基づくものです。 このリリースで修正されたバグの一覧は、次を参照してください。、 [このリリースで修正](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) Hortonworks のサイトでのページです。
* Pig ライブラリ フォルダーで、ファイル "avro-mapred-1.7.4.jar" が "avro-mapred-1.7.4-hadoop2.jar" に変更されています。 これらのファイルの内容には、重要でない小さなバグ修正が含まれます。 ファイルの名前が変更されたときにファイルが壊れるのを避けるため、JAR ファイルの名前と直接依存関係を持たないことをお勧めします。


## 2014 年 8 月 21 日リリース

* 以下の WebHCat 構成 (HIVE-7155) を追加しました。この構成は Templeton コントローラーのジョブに対し、既定のメモリ上限を 1 GB に設定します  (前の既定値は 512 MB)。

     templeton.mapper.memory.mb (=1024)

    * これにより、メモリ上限が低いために特定の Hive クエリで発生する次のエラーに対処します。"コンテナーは物理メモリ上限を超過して実行しています"
    * クラスター作成時に Azure PowerShell で以下のコマンドを使用すると、この構成値を使用していない既定値の 512 に戻すことができます。

        Add-AzureRmHDInsightConfigValues -Core @{"templeton.mapper.memory.mb"="512";}


* Zookeeper ロールのホスト名に変更されました *zookeeper*します。 これはクラスター内の名前解決には影響しますが、外部の REST API には影響しません。 使用するコンポーネントがある場合、 *zookeepernode* ホスト名、新しい名前を使用するように更新する必要があります。 3 つの zookeeper ノードの新しい名前は以下のとおりです。
    * zookeeper0
    * zookeeper1
    * zookeeper2
* HBase のバージョン サポート マトリックスが更新されました。 運用環境の HBase ワークロードでサポートされるのは、HDInsight バージョン 3.1 (HBase バージョン 0.98) のみです。 プレビューで利用可能だったバージョン 3.0 は、今後はサポートされなくなります。

## 2014 年 8 月 15 日より前に作成されたクラスターに関する留意事項

Azure PowerShell または HDInsight SDK エラー メッセージ"クラスター <clustername> is not configured for HTTP services access"(または操作によっては、その他のエラー:「クラスターに接続できません」) Azure PowerShell または HDInsight SDK とクラスター間でのバージョンの相違のために発生する可能性があります。 8 月 15 日以降に作成されたクラスターは、仮想ネットワークへの新しいプロビジョニング機能をサポートします。 Azure PowerShell または HDInsight SDK の以前のバージョンでは、この機能が正しく解釈されません。その結果、ジョブの送信の操作で失敗します。 HDInsight SDK API または Azure PowerShell コマンドレットを使用して Use-AzureRmHDInsightCluster や Invoke-AzureRmHDInsightHiveJob などのジョブを送信する場合、これらの操作は上に示したエラー メッセージで失敗する可能性があります。

これらの互換性の問題は、HDInsight SDK および Azure PowerShell の最新バージョンで解決されます。 HDInsight SDK をバージョン 1.3.1.6 以降に、Azure PowerShell ツールをバージョン 0.8.8 以降に更新することをお勧めします。 最新の HDInsight SDK へのアクセスを取得できます [NuGet][nuget-link]します。 使用して Azure PowerShell ツールにアクセスすることができます [Microsoft Web Platform Installer][webpi-link]します。


## 2014 年 7 月 28 日リリース

* **新しいリージョンで利用可能な HDInsight**: HDInsight の地理的なプレゼンスを 3 つの領域に拡張しています。 HDInsight の顧客は、これらのリージョンでクラスターを作成できます。
    * 東アジア
    * 米国中北部
    * 米国中南部
* HDInsight バージョン 1.6 (HDP 1.1、Hadoop 1.0.3) と HDInsight バージョン 2.1 (HDP1.3、Hadoop 1.2) は、Azure ポータルから削除されます。 Azure PowerShell コマンドレットを使用してこれらのバージョンの Hadoop クラスターの作成を行う [新規 AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) またはを使用して、 [HDInsight SDK](http://msdn.microsoft.com/library/azure/dn469975.aspx)します。 参照してください、 [HDInsight コンポーネントのバージョン](../hdinsight-component-versioning/) 詳細については、ページです。
* このリリースでは、Hortonworks Data Platform (HDP) が以下のように変更されました。

<table border="1">
<tr><th>HDP</th><th>変更点</th></tr>
<tr><td>HDP 1.3 / HDI 2.1</td><td>変更なし</td></tr>
<tr><td>HDP 2.0 / HDI 3.0</td><td>変更なし</td></tr>
<tr><td>HDP 2.1 / HDI 3.1</td><td>zookeeper: ['3.4.5.2.1.3.0-1948'] -> ['3.4.5.2.1.3.2-0002']</td></tr>


</table><br>

## 2014 年 6 月 24 日リリース

このリリースには、以下のように、HDInsight サービスの拡張機能が含まれています。

* **HDP 2.1 可用性**: HDInsight 3.1 (HDP 2.1 が含まれています) が利用できると、新しいクラスターの既定のバージョンです。
* **HBase-Azure ポータルの改良**。 私たちが HBase クラスターで利用可能にプレビューされます。 ポータルで数回クリックするだけで HBase クラスターが作成できます。 

HBase を使用すると、大規模なデータセットを処理する対話的な Web サイトから、何百万ものエンド ポイントからのセンサーのデータやテレメトリ データを格納するサービスまで、さまざまなリアルタイムのワークロードを HDInsight で構築できます。 次の手順としては Hadoop ジョブでこれらのワークロードのデータを分析しますが、これは Azure PowerShell や Hive クラスター ダッシュボードを使用した HDInsight で実現可能です。

### Apache Mahout が HDInsight 3.1 にプレインストール

 [Mahout](http://hortonworks.com/hadoop/mahout/) は、他のクラスター構成することがなく、Mahout ジョブを実行するために HDInsight 3.1 Hadoop クラスターにプレインストールされています。 たとえば、リモート デスクトップ プロトコル (RDP) を使用して Hadoop クラスターにリモート接続したり、以下のように、追加の手順なしで Hello world Mahout コマンドを実行したりできます。

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L  

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

この手順の詳細については、ドキュメントを参照して、 [Breiman 例](https://mahout.apache.org/users/classification/breiman-example.html) Apache Mahout web サイトにします。


### HDInsight 3.1 で Hive クエリに Tez を使用可能

HDInsight 3.1 で Hive 0.13 が利用可能になり、Tez を使用してクエリを実行できます。これを活用してパフォーマンスを大幅に向上できます。
既定では、Hive クエリでの Tez の使用は有効ではありません。 これを使用するにはオプトインが必要です。 以下のコード スニペットを実行すると Tez を有効にできます。

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

Hortonworks では、Tez を使用した Hive クエリのパフォーマンスの向上について、標準ベンチマークによる詳細なブレークダウンを公開しています。 詳細については、「 [Enterprise Hadoop 用の Apache Hive 13 のベンチマーク](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/)します。

Tez で Hive を使用する方法の詳細については、次を参照してください。 [Hive on Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez)します。

###グローバル対応
Hadoop 2.2 対応の HDInsight のリリースに伴い、Azure が利用できるすべての主要な地域で HDInsight が利用可能になりました。 具体的には、西ヨーロッパと東南アジアのデータセンターがオンラインになりました。 これで、お客様は、距離的に近く、コンプライアンス要件が似ていると思われる地域にあるデータセンター内のクラスターを見つけることができます。


###クラスター バージョン間でできることとできないこと

**HDInsight 3.1 クラスターで使用される Oozie メタストアは HDInsight 2.1 クラスターとの下位互換性がないと、以前のバージョンでは使用できません**します。

HDInsight 3.1 クラスターでデプロイされたカスタムの Oozie メタストア データベースは、HDInsight 2.1 クラスターでは再利用できません。 これは、そのメタストアが最初に HDInsight 2.1 クラスターで作成されたものであったとしても同じです。 HDInsight 2.1 クラスターで必要されるメタストアと互換性がなくなったため、HDInsight 3.1 クラスターで使用する場合にメタストア スキーマがアップグレードのため、このシナリオはサポートされていません。 HDInsight 3.1 クラスターで使用されていた Oozie メタストアを再利用しようと試みると、HDInsight 2.1 クラスターが使用できなくなります。

**Oozie メタストアはクラスター間で共有できない**

Oozie メタストアは特定のクラスターに接続されるものであり、クラスター間で共有できません。

###重大な変更

**プレフィックスの構文**:
のみ、"wasb://"構文は HDInsight 3.1 と 3.0 クラスターでサポートされています。 旧バージョンの "asv://" 構文は、HDInsight 2.1 および 1.6 クラスターではサポートされますが、HDInsight 3.1 クラスターまたは 3.0 クラスターではサポートされません。 つまり、この明示的に使用する HDInsight 3.1 または 3.0 クラスターに送信するジョブの"asv://"構文は失敗します。 代わりに、"wasb://" 構文を使用する必要があります。 また、既存のメタストアにより、asv:// 構文を使用してリソースへの明示的な参照を含むジョブを作成し、HDInsight 3.1 または 3.0 クラスターに送信すると、そのジョブは失敗します。 これらのメタストアは、"wasb://" 構文を使用してリソースをアドレス指定するように再作成する必要があります。


**ポート**: HDInsight サービスで使用されるポートを変更します。 これまで使用されてきたポート番号は、Windows オペレーティング システムの一時的なポート範囲内の番号です。 ポートは、短期間のインターネット プロトコル ベースの通信に対して一時的に事前定義される範囲から自動的に割り当てられます。 今回 Hortonworks Data Platform (HDP) サービスに許可された新しいポート番号のセットは、この範囲の外部にあり、ヘッドノードで実行するサービスが使用するポート番号の競合の可能性が回避されます。 新しいポート番号によって重大な変更が発生することはありません。 使用される番号は以下のとおりです。

 **HDInsight 1.6 (HDP 1.1)**
<table border="1">
<tr><th>名前</th><th>値</th></tr>
<tr><td>dfs.http.address</td><td>namenodehost:30070</td></tr>
<tr><td>dfs.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>dfs.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>dfs.datanode.ipc.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>dfs.secondary.http.address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.job.tracker.http.address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.task.tracker.http.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.history.server.http.address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table><br>

 **HDInsight 3.1 と 3.0 (HDP 2.1 および 2.0)**
<table border="1">
<tr><th>名前</th><th>値</th></tr>
<tr><td>dfs.namenode.http-address</td><td>namenodehost:30070</td></tr>
<tr><td>dfs.namenode.https-address</td><td>headnodehost:30470</td></tr>
<tr><td>dfs.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>dfs.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>dfs.datanode.ipc.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>dfs.namenode.secondary.http-address</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.nodemanager.webapp.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table><br>

###依存関係

HDInsight 3.x (HDP2.x) に以下の依存関係が追加されました。

* guice-servlet
* optiq-core
* javax.inject
* activation
* jsr305
* geronimo-jaspic_1.0_spec
* jul-to-slf4j
* java-xmlbuilder
* ant
* commons-compiler
* jdo-api
* commons-math3
* paranamer
* jaxb-impl
* stringtemplate
* eigenbase-xom
* jersey-servlet
* commons-exec
* jaxb-api
* jetty-all-server
* janino
* xercesImpl
* optiq-avatica
* jta
* eigenbase-properties
* groovy-all
* hamcrest-core
* mail
* linq4j
* jpam
* jersey-client
* aopalliance
* geronimo-annotation_1.0_spec
* ant-launcher
* jersey-guice
* xml-apis
* stax-api
* asm-commons
* asm-tree
* wadl
* geronimo-jta_1.1_spec
* guice
* leveldbjni-all
* velocity
* jettison
* snappy-java
* jetty-all
* commons-dbcp

HDInsight 3.x (HDP2.x) から削除された依存関係は以下のとおりです。

* jdeb
* kfs
* sqlline
* ivy
* aspectjrt
* json
* core
* jdo2-api
* avro-mapred
* datanucleus-enhancer
* jsp
* commons-logging-api
* commons-math
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* snappy

###バージョンの変更

HDInsight 2.x (HDP1.x) と HDInsight 3.x (HDP2.x) との間で以下のバージョンが変更されました。

* metrics-core: ['2.1.2'] -> ['3.0.0']
* derbynet: ['10.4.2.0'] -> ['10.10.1.1']
* datanucleus: ['rdbms-3.0.8'] -> ['rdbms-3.2.9']
* jasper-compiler: ['5.5.12'] -> ['5.5.23']
* log4j: ['1.2.15', '1.2.16'] -> ['1.2.16', '1.2.17']
* derbyclient: ['10.4.2.0'] -> ['10.10.1.1']
* httpcore: ['4.2.4'] -> ['4.2.5']
* hsqldb: ['1.8.0.10'] -> ['2.0.0']
* jets3t: ['0.6.1'] -> ['0.9.0']
* protobuf-java: ['2.4.1'] -> ['2.5.0']
* derby: ['10.4.2.0'] -> ['10.10.1.1']
* jasper: ['runtime-5.5.12'] -> ['runtime-5.5.23']
* commons-daemon: ['1.0.1'] -> ['1.0.13']
* datanucleus-core: ['3.0.9'] -> ['3.2.10']
* datanucleus-api-jdo: ['3.0.7'] -> ['3.2.6']
* zookeeper: ['3.4.5.1.3.9.0-01320'] -> ['3.4.5.2.1.3.0-1948']
* bonecp: ['0.7.1.RELEASE'] -> ['
* 0.8.0.RELEASE']


### ドライバー
SQL Server の Java Database Connnectivity (JDBC) ドライバーは HDInsight によって内部的に使用され、外部の操作では使用されません。 Open Database Connectivity (ODBC) を使用して HDInsight に接続する場合は、Microsoft Hive ODBC ドライバーを使用してください。 詳細については、次を参照してください。 [Excel から Microsoft Hive ODBC ドライバーを使用した HDInsight への接続](../hdinsight-connect-excel-hive-odbc-driver.md)します。


### バグの修正

このリリースでは、以下のバージョンの HDInsight でいくつかのバグが修正されました。

* HDInsight 2.1 (HDP 1.3)
* HDInsight 3.0 (HDP 2.0)
* HDInsight 3.1 (HDP 2.1)


## Hortonworks リリース ノート

各バージョンの HDInsight クラスターで使用される Hortonworks Data Platforms (HDP) のリリース ノートは、以下の場所から利用可能です。

* HDInsight version 3.1 に基づく Hadoop ディストリビューションを使用して、 [Hortonworks Data Platform 2.1.7][hdp-2-1-7]します。 これが、2014 年 11 月 7 日以降の Azure ポータルの使用時に作成される既定の Hadoop クラスターです。 HDInsight 3.1 クラスターに基づいて作成された 2014 年 11 月 7 日前に、作成、 [Hortonworks Data Platform 2.1.1][hdp-2-1-1]

* Hdinsight クラスター バージョン 3.0 に基づく Hadoop ディストリビューションを使用して、 [Hortonworks Data Platform 2.0][hdp-2-0-8]します。

* HDInsight バージョン 2.1 に基づいている Hadoop ディストリビューションを使用して、 [Hortonworks Data Platform 1.3][hdp-1-3-0]します。

* HDInsight バージョン 1.6 に基づく Hadoop ディストリビューションを使用して、 [Hortonworks Data Platform 1.1][hdp-1-1-0]します。

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
 


