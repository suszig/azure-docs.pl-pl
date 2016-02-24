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

#Pig コマンド (SSH) を使用して Linux ベースのクラスターで Pig ジョブを実行する

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

このドキュメントでは、Secure Shell (SSH) を使用して Linux ベースの Azure HDInsight クラスターに接続し、Pig コマンドを使用して Pig Latin ステートメントを対話的にまたはバッチ ジョブとして実行する方法を順を追って説明します。

Pig Latin プログラミング言語では、入力データに適用される変換を記述し、目的の出力を生成することができます。

> [AZURE.NOTE] Linux ベースの Hadoop サーバーの使用に慣れているが HDInsight は、という場合は、次を参照してください。 [Linux ベース HDInsight の使用方法](hdinsight-hadoop-linux-information.md)します。

##<a id="prereq"></a>前提条件

この記事の手順を完了するには、次のものが必要です。

* Linux ベースの HDInsight (HDInsight で Hadoop を使用) クラスター

* SSH クライアント SSH クライアントを備えた Linux、Unix、および Mac OS Windows ユーザーは [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) などのクライアントをダウンロードする必要があります。

##<a id="ssh"></a>SSH を使用した接続

SSH コマンドを使用して、HDInsight クラスターの完全修飾ドメイン名 (FQDN) に接続します。 FQDN が、クラスターを指定した名前になります **. >.azurehdinsight.net**します。 次の例、という名前のクラスターに接続 **myhdinsight**します。

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**SSH 認証に証明書キーを指定した場合** HDInsight クラスターを作成したときに、クライアント システム上の秘密キーの場所を指定する必要があります。

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**SSH 認証のパスワードを指定した場合** HDInsight クラスターを作成したときに、入力を求められたらパスワードを指定する必要があります。

HDInsight で SSH の使用に関する詳細については、次を参照してください。 [SSH Linux、Unix、OS X から HDInsight 上の Linux ベースの Hadoop を使用する](hdinsight-hadoop-linux-use-ssh-unix.md)です。

###PuTTY (Windows ベースのクライアント)

Windows ではビルトイン SSH クライアントは提供されません。 使用することをお勧め **PuTTY**, からダウンロードできる [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)します。

PuTTY の使用の詳細については、次を参照してください。 [SSH Windows から Linux ベースの Hadoop を使用する ](hdinsight-hadoop-linux-use-ssh-windows.md)です。

##<a id="pig"></a>Pig コマンドの使用

2. 接続したら、次のコマンドを使用して Pig コマンド ライン インターフェイス (CLI) を起動します。

        pig

    少し経ってから `grunt>` プロンプトが表示されます。

3. 次のステートメントを入力します。

        LOGS = LOAD 'wasb:///example/data/sample.log';

    このコマンドは、sample.log ファイルの内容をログに読み込みます。 ファイルの内容を表示するには、次のコマンドを使用します。

        DUMP LOGS;

4. 次のコマンドを使用して、正規表現を適用してデータを変換し、各レコードのログ レベルのみを抽出します。

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    使用することができます **ダンプ** 変換後のデータの表示にします。 例では `DUMP LEVELS;` が使用されます。

5. 次のステートメントを使用して、変換を適用します。 各手順の後に `DUMP` を使用して、変換の結果を表示します。

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
    <td>FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;</td><td>一意のログ レベル値とそれが出現した回数を含む新しい データ セットを作成します。 これは FREQUENCIES に格納されます。</td>
    </tr>
    <tr>
    <td>RESULT = order FREQUENCIES by COUNT desc;</td><td>数が多い順にログ レベルを並べ替えて、RESULT に格納します。</td>
    </tr>
    </table>

6. 変換の結果は `STORE` ステートメントで保存することもできます。 次の保存など、 `RESULT` に、 **/example/data/pigout** ディレクトリに、クラスターの既定のストレージ コンテナー。

        STORE RESULT into 'wasb:///example/data/pigout';

    > [AZURE.NOTE] という名前のファイルで指定したディレクトリ内のデータが格納されている **一部 nnnnn**します。 ディレクトリが既に存在する場合は、エラーが発生します。

7. エラーを解決するには、次のステートメントを入力します。

        QUIT;

###Pig Latin バッチ ファイル

Pig コマンドを使用して、ファイルに含まれた Pig Latin を実行することもできます。

3. Grunt プロンプトを終了した後でという名前のファイルにパイプ STDIN に次のコマンドを使用して **pigbatch.pig**します。 このファイルは、SSH セッションでログインしているアカウントのホーム ディレクトリに作成されます。

        cat > ~/pigbatch.pig

4. 次の行を入力または貼り付けて、Ctrl + D キーを押します。

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. 次を使用して実行する、 **pigbatch.pig** Pig コマンドを使用してファイルです。

        pig ~/pigbatch.pig

    バッチ ジョブが完了したら、次の出力が表示されます (前の手順の `DUMP RESULT;` での出力と同じ内容です)。

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>概要

このように、Pig コマンドでは、Pig Latin を使用して MapReduce 操作を対話的に実行できるだけでなく、バッチ ファイルに格納されたステートメントも実行できます。

##<a id="nextsteps"></a>次のステップ

HDInsight での Pig に関する全般的な情報

* [HDInsight での Pig と Hadoop の使用](hdinsight-use-pig.md)

HDInsight での Hadoop のその他の使用方法に関する情報

* [HDInsight での Hive と Hadoop の使用](hdinsight-use-hive.md)

* [HDInsight での MapReduce と Hadoop の使用](hdinsight-use-mapreduce.md)

