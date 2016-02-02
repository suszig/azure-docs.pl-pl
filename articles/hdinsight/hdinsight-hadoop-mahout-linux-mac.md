<properties
    pageTitle="Mahout と Linux ベースの HDInsight を使用したリコメンデーションの生成 | Microsoft Azure"
    description="Apache Mahout 機械学習ライブラリを使用して Linux ベースの HDInsight (Hadoop) で映画のリコメンデーションを生成する方法について説明します。"
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
    ms.date="11/06/2015"
    ms.author="larryfr"/>


# HDInsight で Apache Mahout と Linux ベースの Hadoop を使用した映画のリコメンデーションの生成 (プレビュー)

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

使用する方法について、 [Apache Mahout](http://mahout.apache.org) 映画のリコメンデーションを生成する Azure HDInsight での機械学習ライブラリです。

Mahout は、 [機械学習 ][ml] ライブラリの 1 つです。 Mahout には、フィルター処理、分類、クラスタリングなどデータを処理するためのアルゴリズムが含まれています。 この記事では、リコメンデーション エンジンを使用し、友人たちが鑑賞した映画に基づいて映画のリコメンデーションを生成します。
> [AZURE.NOTE] このドキュメントの手順には、HDInsight クラスターの Linux ベースの Hadoop が必要です (プレビュー)。 Windows ベースのクラスターで Mahout を使用する方法の詳細については、を参照してください [Windows ベースの hadoop を HDInsight で Apache Mahout を使用して映画のリコメンデーションを生成](hdinsight-mahout.md)。

## 前提条件

* HDInsight クラスターでの Linux ベースの Hadoop 1 つを作成する方法の詳細については、を参照してください [HDInsight ][getstarted]。

## Mahout のバージョン

詳細については、HDInsight クラスターに含まれている Mahout のバージョンは、次を参照してください。 [HDInsight のバージョンと Hadoop コンポーネント](hdinsight-component-versioning.md)します。
> [AZURE.WARNING] Mahout の別のバージョンを HDInsight クラスターにアップロードできますが、全面的にサポートされるのは HDInsight クラスターに用意されているコンポーネントだけであり、これらのコンポーネントに関連する問題の分離と解決については、Microsoft サポートが支援します。
>
> カスタム コンポーネントについては、問題のトラブルシューティングを進めるための支援として、商業的に妥当な範囲のサポートを受けることができます。 これにより問題が解決する場合もあれば、オープン ソース テクノロジに関して、深い専門知識が入手できる場所への参加をお願いすることになる場合もあります。 たとえば、使用できますが、このような数多くのコミュニティ サイトがある: [HDInsight についての MSDN フォーラム](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), 、[http://stackoverflow.com](http://stackoverflow.com)します。 Apache プロジェクトがプロジェクトのサイトを持つも [http://apache.org](http://apache.org), 、たとえば: [Hadoop](http://hadoop.apache.org/), 、[Spark](http://spark.apache.org/)します。

## <a name="recommendations"></a>リコメンデーションについて

Mahout で提供される機能の 1 つが、リコメンデーション エンジンです。 このエンジンでは、データの形式で `userID`, 、`itemId`, 、および `prefValue` (項目に対するユーザーの嗜好)。 Mahout に共起分析を実行できます。 項目の優先順位を持つ (_u) もあるこれらの基本設定他。 次に Mahout は、項目の嗜好が似ているユーザーを特定します。これはリコメンデーションの作成に使用できます。

映画を使用した非常にシンプルな例を次に示します。

* __共起__: Joe、Alice、および Bob すべて立った _Star Wars_、Empire Strikes は、(_t) および _Return of、Jedi_ です。 Mahout では、これらの映画のいずれかを好きなユーザーが他の 2 作品も好きであると判断します。

* __共起__: Bob と Alice も好きな映画、Clones_ の _Attack と、推薦の _Revenge のファントム Menace_ (_t)。 Mahout では、この例の最初の 3 作品を好きなユーザーがこれらの 3 作品も好きであると判断します。

* __類似性のリコメンデーション__: Joe は、この例の最初の 3 作品を好きな映画として選びました。Mahout では、嗜好が似ている他のユーザーが好きな映画の中で、Joe がまだ観ていない映画を調べます (好み/順位)。 この場合、Mahout は、機種ファントム Menace_、Clones_ の _Attack、推薦の _Revenge をお勧めします。

## データを読み込む

便利なことに、 [GroupLens Research ][movielens] Mahout と互換性のある形式で映画の評価データを提供します。 次の手順を使用してデータをダウンロードし、クラスターの既定のストレージに読み込みます。

1. SSH を使用して、Linux ベースの HDInsight クラスターに接続します。 接続時に使用するアドレスは `CLUSTERNAME ssh.azurehdinsight.net` ポートは `22`します。

    SSH を使用して HDInsight に接続する方法の詳細については、次のドキュメントを参照してください。

    * **Linux、Unix または OS X クライアント**:を参照してください [Linux、OS X または Unix から Linux ベースの HDInsight クラスターへの接続](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Windows クライアント**:を参照してください [Windows から Linux ベースの HDInsight クラスターへの接続](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

2. MovieLens 100k アーカイブをダウンロードします。これには、1,700 本の映画に対する 1,000 人のユーザーによる評価 100,000 件が含まれています。

        curl -O http://files.grouplens.org/datasets/movielens/ml-100k.zip

3. 次のコマンドを使用して、アーカイブを抽出します。

        unzip ml-100k.zip

    これは **ml-100k** という名前の新しいフォルダーに内容を抽出します。

4. HDInsight のストレージにデータをコピーするには、次のコマンドを使用します。

        cd ml-100k
        hdfs dfs -put u.data /example/data

    このファイルに含まれるデータの構造を持つ `userID`, 、`movieID`, 、`userRating`, 、および `タイムスタンプ`, を指定する各ユーザーがビデオを評価する方法は、高です。 次にデータの例を示します。

        196 242 3   881250949
        186 302 3   891717742
        22  377 1   878887116
        244 51  2   880606923
        166 346 1   886397596


## ジョブを実行する

次のコマンドを実行して、リコメンデーション ジョブを実行します。

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /example/data/u.data -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] ジョブが完了するまでに数分かかる場合があり、複数の MapReduce ジョブを実行することがあります。

## 出力を表示する

1. ジョブが完了したら、次のコマンドを使用して、生成された出力を表示します。

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    出力は次のように表示されます。

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    最初の列は、 `userID`します。 含まれる値 ' [' と ']' は `movieId`:`recommendationScore`します。

2. **ml-100k** ディレクトリに含まれているデータの中には、データをよりユーザー フレンドリーにするために使用できるものもあります。 最初に、次のコマンドを使用してデータをダウンロードします。

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt

    これにより、出力データが現在のディレクトリの **recommendations.txt** という名前のファイルにコピーされます。

3. 次のコマンドを使用し、リコメンデーション出力のデータの映画の名前を検索する新しい Python スクリプトを作成します。

     nano show_recommendations.py

 エディターが開いたら、ファイルの内容として次のコードを使用します。

     #!/usr/bin/env python
    
     import sys
    
     if len(sys.argv) != 5:
             print "Arguments: userId userDataFilename movieFilename recommendationFilename"
             sys.exit(1)
    
     userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]
    
     print "Reading Movies Descriptions"
     movieFile = open(movieFilename)
     movieById = {}
     for line in movieFile:
             tokens = line.split("|")
             movieById[tokens[0]] = tokens[1:]
     movieFile.close()
    
     print "Reading Rated Movies"
     userDataFile = open(userDataFilename)
     ratedMovieIds = []
     for line in userDataFile:
             tokens = line.split("\t")
             if tokens[0] == userId:
                     ratedMovieIds.append((tokens[1],tokens[2]))
     userDataFile.close()
    
     print "Reading Recommendations"
     recommendationFile = open(recommendationFilename)
     recommendations = []
     for line in recommendationFile:
             tokens = line.split("\t")
             if tokens[0] == userId:
                     movieIdAndScores = tokens[1].strip("[]\n").split(",")
                     recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                     break
     recommendationFile.close()
    
     print "Rated Movies"
     print "------------------------"
     for movieId, rating in ratedMovieIds:
             print "%s, rating=%s" % (movieById[movieId][0], rating)
     print "------------------------"
    
     print "Recommended Movies"
     print "------------------------"
     for movieId, score in recommendations:
             print "%s, score=%s" % (movieById[movieId][0], score)
     print "------------------------"

 **Ctrl-X**、**Y**、**Enter** の順に押して、データを保存します。

3. 次のコマンドを使用し、実行可能ファイルを作成します。

        chmod +x show_recommendations.py

4. Python スクリプトを実行します。 次は、100 k ディレクトリでは前提としています。 ここで、 `u.data` と `u.item` ファイルがあります。

        ./show_recommendations.py 4 u.data u.item recommendations.txt

    これは、ユーザー ID 4 に対して生成されたリコメンデーションを表示します。

    * **u.data** ファイルを使用して、ユーザーが評価した映画を取得します
    * **u.item** ファイルを使用して、映画の名前を取得します
    * **recommendations.txt** を使用して、このユーザーの映画のリコメンデーションを取得します

    このコマンドの出力は次のように表示されます。

        ムービーの説明を読む
        読み取り映画の評価
        読み取りの推奨事項
        映画の評価
        ------------------------
        (1997) を模倣するには、評価 = 3
        Ulee のゴールド (1997) 評価 = 5
        Incognito (1997) 評価 = 5
        1 つ飛んできた上、カッコウの入れ子 (1975 年)、レーティング 4 =
        イベントの期間 (1997)、評価 = 4
        クライアント、(1994 年)、評価 = 3
        評価 liar Liar (1997) = 5
        Scream (1996 年)、レーティング 4 =
        Star Wars (1977)、評価 = 5
        (1998 年)、Singer を wedding 評価 = 5
        評価 starship Troopers (1997) = 4
        空軍 1 つ (1997)、評価 = 5
        陰謀理論的に (1997)、評価 = 3
        (1997) を問い合わせるには、評価 = 5
        インディアナ Jones と最後の十字軍 (1989 年)、評価 = 3
        がっかりしているメジャー (1998 年) が評価 = 5
        7 (Se7en) (1995 年) の評価 = 4
        警察官土地 (1997)、評価 = 5
        失われた高速道路 (1997) 評価 = 5
        割り当て、(1997 年)、評価 = 5
        兄弟 2000 (1998 年) をダイビング評価 = 5
        評価の生成 (1997) = 2
        景色 (1997)、評価 = 5
        (1997) (& o) で評価 = 5
        ------------------------
        推奨されるビデオ
        ------------------------
        チベット (1997) で 7 年間のスコア付け = 5.0
        インディアナ Jones と最後の十字軍 (1989 年)、スコア付け = 5.0
        Jaws (1975 年) のスコア付け = 5.0
        理解し、感情 (1995 年) スコア = 5.0
        独立記念日 (ID4) (1996 年) のスコア付け = 5.0
        私の親友のスコア付け (1997) を Wedding = 5.0
        Jerry Maguire (1996 年) のスコア付け = 5.0
        Scream 2 (1997) スコア = 5.0
        (1996 年) に、強制終了、時間のスコア付け = 5.0
        ロック (1996 年)、スコア付け = 5.0
        ------------------------

## 一時データを削除する

Mahout ジョブは、ジョブの処理中に作成された一時データを削除しません。  `--TempDir` 削除を容易に特定のパスに一時ファイルを抽出するサンプル ジョブでパラメーターを指定します。 一時ファイルを削除するには、次のコマンドを使用します。

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] コマンドを再実行する場合、出力ディレクトリも削除する必要があります。 このディレクトリを削除するには、次を使用します。
>
> `hdfs dfs - rm-f-r/example/data/mahoutout`

## 次のステップ

ここまで、Mahout の使用方法を学習し、HDInsight でデータを操作するその他の方法を確認してきました。

* [HDInsight での hive します。](hadoop-use-hive.md)
* [HDInsight での pig](hadoop-use-pig.md)
* [HDInsight での MapReduce](hadoop-use-mapreduce.md)


[build]: http://mahout.apache.org/developers/buildingmahout.html 
[movielens]: http://grouplens.org/datasets/movielens/ 
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip 
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md 
[upload]: hdinsight-upload-data.md 
[ml]: http://en.wikipedia.org/wiki/Machine_learning 
[forest]: http://en.wikipedia.org/wiki/Random_forest 
[management]: https://manage.windowsazure.com/ 
[enableremote]: ./media/hdinsight-mahout/enableremote.png 
[connect]: ./media/hdinsight-mahout/connect.png 
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png 
[tools]: https://github.com/Blackmist/hdinsight-tools 

