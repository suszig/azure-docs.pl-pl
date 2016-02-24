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

#HDInsight で Apache Mahout と Linux ベースの Hadoop を使用した映画のリコメンデーションの生成 (プレビュー)

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

使用する方法について、 [Apache Mahout](http://mahout.apache.org) 映画のリコメンデーションを生成する Azure HDInsight での機械学習ライブラリです。

Mahout は、 [機械学習][ml] ライブラリの 1 つです。 Mahout には、フィルター処理、分類、クラスタリングなどデータを処理するためのアルゴリズムが含まれています。 この記事では、リコメンデーション エンジンを使用し、友人たちが鑑賞した映画に基づいて映画のリコメンデーションを生成します。

> [AZURE.NOTE] このドキュメントの手順では、HDInsight クラスター (プレビュー) での Linux ベースの Hadoop が必要です。 Windows ベースのクラスターで Mahout を使用する方法の詳細については、次を参照してください [Windows ベースの hadoop を HDInsight で Apache Mahout を使用して映画のリコメンデーションを生成。](hdinsight-mahout.md)

##前提条件

* HDInsight クラスターでの Linux ベースの Hadoop 1 つを作成する方法の詳細については、次を参照してください [HDInsight での Linux ベースの Hadoop の使用の概要。][getstarted]

##Mahout のバージョン

詳細については、HDInsight クラスターに含まれている Mahout のバージョンは、次を参照してください。 [HDInsight のバージョンと Hadoop コンポーネント](hdinsight-component-versioning.md)します。

> [AZURE.WARNING] Mahout の別のバージョンを HDInsight クラスターにアップロードすることはできますが、HDInsight クラスターと共に提供するコンポーネントのみが完全にサポートされてし、を分離し、これらのコンポーネントに関連する問題を解決するには Microsoft サポートが支援します。
>
> カスタム コンポーネントについては、問題のトラブルシューティングを進めるための支援として、商業的に妥当な範囲のサポートを受けることができます。 これにより問題が解決する場合もあれば、オープン ソース テクノロジに関して、深い専門知識が入手できる場所への参加をお願いすることになる場合もあります。 たとえば、使用できますが、このような数多くのコミュニティ サイトがある: [HDInsight についての MSDN フォーラム](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), 、[http://stackoverflow.com](http://stackoverflow.com)します。 Apache プロジェクトがプロジェクトのサイトを持つも [http://apache.org](http://apache.org), 、たとえば: [Hadoop](http://hadoop.apache.org/), 、[Spark](http://spark.apache.org/)します。

##<a name="recommendations"></a>リコメンデーションについて

Mahout で提供される機能の 1 つが、リコメンデーション エンジンです。 データは、`userID`、`itemId`、`prefValue` (項目に対するユーザーの嗜好) の形式で受け付けられます。 Mahout に共起分析を実行できます: _項目の優先順位を持つユーザーがその他の項目もこれらの基本設定に必要が_です。 次に Mahout は、項目の嗜好が似ているユーザーを特定します。これはリコメンデーションの作成に使用できます。

映画を使用した非常にシンプルな例を次に示します。

* __共起__: Joe、Alice と Bob すべて好きな映画として _スター ウォーズ_, 、_The Empire Strikes Back_, 、および _、Jedi の返品_します。 Mahout では、これらの映画のいずれかを好きなユーザーが他の 2 作品も好きであると判断します。

* __共起__: Bob と Alice も好きな映画 _The Phantom Menace_, 、_クローンの攻撃_, 、および _、Sith の復讐_します。 Mahout では、この例の最初の 3 作品を好きなユーザーがこれらの 3 作品も好きであると判断します。

* __類似性のリコメンデーション__: Joe は、最初の 3 作品を好き、Mahout では映画嗜好が似て、その他、Joe がまだ観ていない (好み/順位)。 この場合、Mahout で _The Phantom Menace_, 、_クローンの攻撃_, 、および _、Sith の復讐_します。

##データを読み込む

便利なことに、 [GroupLens Research][movielens] Mahout と互換性のある形式で映画の評価データを提供します。 次の手順を使用してデータをダウンロードし、クラスターの既定のストレージに読み込みます。

1. SSH を使用して、Linux ベースの HDInsight クラスターに接続します。 接続時に使用するアドレスは `CLUSTERNAME-ssh.azurehdinsight.net` ポートは `22`です。

    SSH を使用して HDInsight に接続する方法の詳細については、次のドキュメントを参照してください。

    * **Linux、Unix または OS X クライアント**: を参照してください [Linux、OS X または Unix から Linux ベースの HDInsight クラスターへの接続](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Windows クライアント**: を参照してください [Windows から Linux ベースの HDInsight クラスターへの接続](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

2. MovieLens 100k アーカイブをダウンロードします。これには、1,700 本の映画に対する 1,000 人のユーザーによる評価 100,000 件が含まれています。

        curl -O http://files.grouplens.org/datasets/movielens/ml-100k.zip

3. 次のコマンドを使用して、アーカイブを抽出します。

        unzip ml-100k.zip

    これはという名前の新しいフォルダーに内容を抽出 **100 k**します。

4. HDInsight のストレージにデータをコピーするには、次のコマンドを使用します。

        cd ml-100k
        hdfs dfs -put u.data /example/data


    このファイルに含まれているデータの構造は `userID`、`movieID`、`userRating`、および `timestamp` です。これは、各ユーザーによって映画に対してどれだけ高い評価が付けられているか示しています。 次にデータの例を示します。


        196 242 3   881250949
        186 302 3   891717742
        22  377 1   878887116
        244 51  2   880606923
        166 346 1   886397596

##ジョブを実行する

次のコマンドを実行して、リコメンデーション ジョブを実行します。

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /example/data/u.data -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] ジョブは完了するには数分かかる場合があり、複数の MapReduce ジョブを実行することがあります。

##出力を表示する

1. ジョブが完了したら、次のコマンドを使用して、生成された出力を表示します。

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    出力は次のように表示されます。

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    最初の列は `userID` です。 '[' と ']' の間に入る値は `movieId`:`recommendationScore` です。

2. 含まれているその他のデータの一部、 **100 k** ディレクトリを使用して、データの複数のユーザーをわかりやすいようにできます。 最初に、次のコマンドを使用してデータをダウンロードします。

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt

    これは、出力データがという名前のファイルにコピーされます **recommendations.txt** 現在のディレクトリにします。

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

    キーを押して **CTRL + X**, 、**Y**, 、最後に **Enter** データを保存します。

3. 次のコマンドを使用し、実行可能ファイルを作成します。

        chmod +x show_recommendations.py

4. Python スクリプトを実行します。 以下では、`u.data` と `u.item` ファイルがある ml-100k ディレクトリにいるものと仮定しています。

        ./show_recommendations.py 4 u.data u.item recommendations.txt

    これは、ユーザー ID 4 に対して生成されたリコメンデーションを表示します。

    *  **U.data** ファイルを使用して、ユーザーが評価した映画を取得
    *  **U.item** ファイルを使用して、映画の名前を取得
    *  **Recommendations.txt** を使用してこのユーザーの映画のリコメンデーションの取得

    このコマンドの出力は次のように表示されます。

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##一時データを削除する

Mahout ジョブは、ジョブの処理中に作成された一時データを削除しません。 このサンプル ジョブでは `--tempDir` パラメーターを指定し、一時ファイルを特定のパスに分離して簡単に削除できるようにしています。 一時ファイルを削除するには、次のコマンドを使用します。

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] コマンドを再度実行する場合は、出力ディレクトリを削除しなければなりません。 このディレクトリを削除するには、次を使用します。
>
> ```hdfs dfs -rm -f -r /example/data/mahoutout```

##次のステップ

ここまで、Mahout の使用方法を学習し、HDInsight でデータを操作するその他の方法を確認してきました。

* [HDInsight での Hive の使用](hadoop-use-hive.md)
* [HDInsight での Pig の使用](hadoop-use-pig.md)
* [HDInsight での MapReduce の使用](hadoop-use-mapreduce.md)

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
 
