<properties
   pageTitle="HDInsight クラスターでの Hadoop Pig と SSH の使用 | Microsoft Azure"
   description="SSH で Linux ベースの Hadoop クラスターに接続し、Pig コマンドを使用して Pig Latin ステートメントを対話的に実行するか、バッチ ジョブとして実行する方法について説明します。"
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


# Pig コマンド (SSH) を使用して Linux ベースのクラスターで Pig ジョブを実行する

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

このドキュメントでは、Secure Shell (SSH) を使用して Linux ベースの Azure HDInsight クラスターに接続し、Pig コマンドを使用して Pig Latin ステートメントを対話的にまたはバッチ ジョブとして実行する方法を順を追って説明します。

Pig Latin プログラミング言語では、入力データに適用される変換を記述し、目的の出力を生成することができます。
> [AZURE.NOTE] Linux ベースの Hadoop サーバーの使用に慣れているが HDInsight は、という場合は、次を参照してください。 [Linux ベース HDInsight の使用方法](hdinsight-hadoop-linux-information.md)します。

## <a id="prereq"></a>前提条件

この記事の手順を完了するには、次のものが必要です。

* Linux ベースの HDInsight (HDInsight で Hadoop を使用) クラスター

* SSH クライアント SSH クライアントを備えた Linux、Unix、および Mac OS Windows ユーザーのなど、クライアントをダウンロードする必要があります [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)します。

## <a id="ssh"></a>SSH による接続します。

SSH コマンドを使用して、HDInsight クラスターの完全修飾ドメイン名 (FQDN) に接続します。 FQDN はクラスターに指定した名前で、その後、**.azurehdinsight.net** が続きます。 以下の例では、**myhdinsight** という名前のクラスターに接続します。

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**HDInsight クラスターの作成時に SSH 認証に証明書キーを指定した場合は**、クライアント システムの秘密キーの場所を指定する必要があることがあります。

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**HDInsight クラスターの作成時に SSH 認証のパスワードを指定した場合は**、パスワードの入力を求められます。

HDInsight で SSH の使用に関する詳細については、次を参照してください。 [SSH Linux、Unix、OS X から HDInsight 上の Linux ベースの Hadoop を使用する](hdinsight-hadoop-linux-use-ssh-unix.md)します。

### PuTTY (Windows ベースのクライアント)

Windows ではビルトイン SSH クライアントは提供されません。 使用することをお勧め **PuTTY**, からダウンロードできる [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)します。

PuTTY の使用の詳細については、次を参照してください。 [SSH Windows から Linux ベースの Hadoop を使用する ](hdinsight-hadoop-linux-use-ssh-windows.md)します。

## <a id="pig"></a>Pig コマンドを使用します。

2. 接続したら、次のコマンドを使用して Pig コマンド ライン インターフェイス (CLI) を起動します。

        pig

    しばらくすると表示されます、 `grunt >` プロンプトです。

3. 次のステートメントを入力します。

        LOGS = LOAD 'wasb:///example/data/sample.log';

    このコマンドは、sample.log ファイルの内容をログに読み込みます。 ファイルの内容を表示するには、次のコマンドを使用します。

        DUMP LOGS;

4. 次のコマンドを使用して、正規表現を適用してデータを変換し、各レコードのログ レベルのみを抽出します。

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    変換後のデータを表示するには、**DUMP** を使用します。 ここでは、使用して `ダンプのレベル`します。

5. 次のステートメントを使用して、変換を適用します。 使用 `ダンプ` 各手順の後に、変換の結果を表示します。

   <table>
    <tr>
    <th>ステートメント</th><th>実行内容</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;</td><td>ログ レベルに null 値を含む行を削除し、結果を FILTEREDLEVELS に格納します。</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;</td><td>ログ レベルで列をグループ化し、結果を GROUPEDLEVELS に格納します。</td>
    </tr>
    <tr>
    <td>FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;</td><td>一意のログ レベル値とそれが出現した回数を含む新しい データ セットを作成します。これは FREQUENCIES に格納されます。</td>
    </tr>
    <tr>
    <td>RESULT = order FREQUENCIES by COUNT desc;</td><td>数が多い順にログ レベルを並べ替えて、RESULT に格納します。</td>
    </tr>
    </table>

6. 使用して、変換の結果を保存することも、 `ストア` ステートメントです。 次の保存など、 `結果` に、 **/example/data/pigout** ディレクトリに、クラスターの既定のストレージ コンテナー。

        STORE RESULT into 'wasb:///example/data/pigout';

    > [AZURE.NOTE] データは、**part-nnnnn** という名前のファイルの指定したディレクトリに保存されます。 ディレクトリが既に存在する場合は、エラーが発生します。

7. エラーを解決するには、次のステートメントを入力します。

        QUIT;


### Pig Latin バッチ ファイル

Pig コマンドを使用して、ファイルに含まれた Pig Latin を実行することもできます。

3. エラーを解決したら、次のコマンドを使用して、STDIN を **pigbatch.pig** という名前のファイルにパイプします。 このファイルは、SSH セッションでログインしているアカウントのホーム ディレクトリに作成されます。

        cat > ~/pigbatch.pig

4. 次の行を入力または貼り付けて、Ctrl + D キーを押します。

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. 次の Pig コマンドを使用して、**pigbatch.pig** ファイルを実行します。

        pig ~/pigbatch.pig

    バッチ ジョブが完了すると、使用されている場合と同じである、次の出力が表示されます `DUMP RESULT;` 前の手順でします。

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <a id="summary"></a>概要

このように、Pig コマンドでは、Pig Latin を使用して MapReduce 操作を対話的に実行できるだけでなく、バッチ ファイルに格納されたステートメントも実行できます。

## <a id="nextsteps"></a>次のステップ

HDInsight での Pig に関する全般的な情報

* [HDInsight での Hadoop での Pig の使用](hdinsight-use-pig.md)

HDInsight での Hadoop のその他の使用方法に関する情報

* [HDInsight での Hadoop の Hive を使用します。](hdinsight-use-hive.md)

* [HDInsight での Hadoop の MapReduce を使用します。](hdinsight-use-mapreduce.md)





