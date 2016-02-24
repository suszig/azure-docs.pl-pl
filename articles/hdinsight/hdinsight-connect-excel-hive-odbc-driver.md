<properties
   pageTitle="Hive ODBC ドライバーを使用した Excel から Hadoop への接続 | Microsoft Azure"
   description="Excel 用の Microsoft Hive ODBC ドライバーを使用できるようにセットアップし、HDInsight クラスターのデータを照会する方法を説明します。"
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="paulettm"
   tags="azure-portal"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/15/2015"
   ms.author="jgao"/>

#Microsoft Hive ODBC ドライバーを使用した Excel から Hadoop への接続

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsoft のビッグ データ ソリューションでは、Microsoft ビジネス インテリジェンス (BI) コンポーネントを Azure HDInsight を使用して展開されている Apache Hadoop クラスターを統合します。 たとえば、Microsoft Hive Open Database Connectivity (ODBC) ドライバーを使用すれば、HDInsight で Hadoop クラスターの Hive データ ウェアハウスに Excel を接続できます。

また、Microsoft Power Query for Excel アドインを使用して Excel から HDInsight クラスターや、その他の (HDInsight 以外の) Hadoop クラスターなどのデータ ソースを接続することもできます。 詳細については、Power Query のインストールとは、次を参照してください。 [Excel から Power Query を使用した HDInsight への接続][hdinsight-power-query]します。

> [AZURE.NOTE] この記事の手順では、Linux または Windows ベースの HDInsight クラスターで使用できますが、Windows は、クライアント ワークステーションに必要です。

**前提条件**:

この記事を読み始める前に、次の項目を用意する必要があります。

- **HDInsight クラスター**します。 1 つを作成するを参照してください。 [Azure HDInsight の概要][hdinsight-get-started]します。
- **ワークステーション** Office 2013 Professional Plus、Office 365 Pro Plus、Excel 2013 Standalone、または Office 2010 Professional Plus とします。


##Microsoft Hive ODBC ドライバーのインストール

ダウンロードしてから [Microsoft Hive ODBC ドライバーをインストール、 [ダウンロード センター][hive-odbc-driver-download]します。

このドライバーは Windows 7、Windows 8、Windows 10、Windows Server 2008 R2、Windows Server 2012 のいずれかの 32 ビットまたは 64 ビット バージョンにインストールすることができ、これによって Azure HDInsight (バージョン 1.6 以降) および Azure HDInsight Emulator (v.1.0.0.0 以降) への接続が許可されます。 ODBC ドライバーを使用するアプリケーションのバージョンに合致したバージョンをインストールする必要があります。 このチュートリアルでは、Office Excel のドライバーが使用されます。

##Hive ODBC データ ソースの作成

次の手順に従って、Hive ODBC データ ソースを作成します。

1. Windows 8 または Windows 10 では、Windows キーを押してスタート画面を開くし、入力 **データソース**します。
2. をクリックして **(32 ビット) に、ODBC データ ソース設定** または **(64 ビット) に、ODBC データ ソース設定** Office バージョンに応じて、します。 Windows 7 を使用している場合は、選択 **ODBC データ ソース (32 ビット)** または **ODBC データ ソース (64 ビット)** から **管理ツール**します。 これが起動、 **ODBC データ ソース アドミニストレーター** ダイアログ。

    ![ODBC データ ソース アドミニストレーター][img-hdi-simbahiveodbc-datasource-admin]

3. ユーザー DNS から、次のようにクリックします。 **追加** を開くには、 **データ ソースの新規作成** ウィザード。
4. 選択 **Microsoft Hive ODBC ドライバー**, 、クリックして **完了**します。 これが起動、 **Microsoft Hive ODBC ドライバーの DNS セットアップ** ダイアログ。

5. 次の値を入力または選択します。

プロパティ|説明
---|---
データ ソース名|データ ソースに名前を付けます。
ホスト|次に、 <HDInsightClusterName>。 >.azurehdinsight.net です。 たとえば、「myHDICluster.azurehdinsight.net」と入力します。
Port|最初の起動時にドメインに参加しているマシンになるように VM をプロビジョニングするには、 <strong>使用します</strong>. (このポートは 563 から 443 に変更されました)。
データベース|最初の起動時にドメインに参加しているマシンになるように VM をプロビジョニングするには、 <strong>既定値</strong>.
Hive サーバーの種類|[ <strong>選択します。</strong>
メカニズム|[ <strong>Azure HDInsight サービス</strong>
HTTP パス|空白のままにします。
ユーザー名|HDInsight クラスター ユーザーのユーザー名を入力します。 これは、クラスターのプロビジョニング処理中に作成されるユーザー名です。 簡易作成オプションを使用する場合、既定のユーザー名は <strong>admin</strong>.
パスワード|HDInsight クラスター ユーザーのパスワードを入力します。
</table>

クリックすると、注意すべき重要なパラメーターがある **詳細オプション**:

パラメーター|説明
---|---
ネイティブ クエリの使用|これを選択すると、ODBC ドライバーは TSQL を HiveQL に変換しません。 純粋な HiveQL ステートメントを送信していることを確認している場合にのみ使用します。 SQL Server または Azure SQL Database に接続している場合は、オフのままにします。
ブロック単位でフェッチされた行|大量のレコードをフェッチする場合、このパラメーターを調整してパフォーマンスを最適化する必要がある場合があります。
既定の文字列の列の長さ、バイナリ列の長さ、10 進数の列の桁数|データ型の長さおよび精度は、データが返される方法に影響する可能性があります。 精度が失われたり、切り捨てられたりするために間違った情報が返されます。


    ![Advanced options][img-HiveOdbc-DataSource-AdvancedOptions]

6. クリックして **テスト** データ ソースをテストします。 データ ソースが正しく構成されている場合と表示 *テストは正常に完了しました!*します。
7. クリックして **OK** テスト] ダイアログを閉じます。 新しいデータ ソースは、今すぐに表示されます、 **ODBC データ ソース アドミニストレーター**します。
8. クリックして **OK** ウィザードを終了します。

##HDInsight クラスターから Excel へのデータのインポート

ここでは、上記の手順で作成した ODBC データ ソースを使用して、Hive テーブルから Excel ブックへデータをインポートする方法を説明します。

1. Excel で新しいブックまたは既存のブックを開きます。
2.  **データ** ] タブ、[ **その他のデータ ソースに**, 、順にクリック **[データ接続ウィザード** を起動する、 **データ接続ウィザード**します。

    ![データ接続ウィザードを開く][img-hdi-simbahiveodbc.excel.dataconnection]

3. 選択 **ODBC DSN** クリックして、データ ソースとして **次**します。
4. ODBC データ ソースから、前の手順で作成したデータ ソース名を選択し、をクリックして **次**します。
5. ウィザードで、クラスターのパスワードを再入力し、をクリックし、 **テスト** に必要な場合は、もう一度構成を確認します。
6. クリックして **OK** テスト] ダイアログを閉じます。
7. Click **OK**. 待機、 **データベースの選択とテーブル** ダイアログを開きます。 この処理には数秒かかります。
8. クリックして、インポートするテーブルを選択して **次**します。  *Hivesampletable* は HDInsight クラスターに付属する hive テーブルのサンプルです。  作成していない場合は、ここで選択できます。 実行の詳細については Hive クエリと Hive テーブルの作成を参照してください [HDInsight での Hive の使用][hdinsight-use-hive]します。
8. クリックして **完了**します。
9.  **データのインポート** ダイアログ ボックスで、変更したり、クエリを指定します。 これを行うには、次のようにクリックします。 **プロパティ**します。 この処理には数秒かかります。
10. をクリックして、 **定義** タブをクリックし、追加 **LIMIT 200** select ステートメントで、Hive に、 **コマンド テキスト** ] ボックスに貼り付けます。 変更によって、返されるレコード セットが 200 に制限されます。

    ![接続のプロパティ][img-hdi-simbahiveodbc-excel-connectionproperties]

11. クリックして **OK** 接続のプロパティ] ダイアログを閉じます。
12. をクリックして **OK** を閉じる、 **データのインポート** ダイアログ。  
13. クリックして、パスワードを再入力 **OK**します。 データが Excel にインポートされるまでに、数秒かかります。

##次のステップ

この記事では、Microsoft Hive ODBC ドライバーを使用して HDInsight サービスから Excel にデータを取得する方法を学習しました。 同様に、SQL Database に HDInsight サービスからデータを取得することもできます。 また、HDInsight サービスにデータをアップロードすることもできます。 詳細については、次を参照してください。

- [HDInsight を使用したフライトの遅延データの分析][hdinsight-analyze-flight-data]
- [データを HDInsight へアップロードする方法][hdinsight-upload-data]
- [HDInsight での Sqoop の使用] [hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png

