<properties
    pageTitle="HDInsight Linux クラスターの Hadoop で Hue を使用する | Microsoft Azure"
    description="HDInsight Linux の Hadoop クラスターに Hue をインストールし、使用する方法について説明します。"
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="paulettm"
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/05/2015" 
    ms.author="nitinme"/>

# HDInsight Hadoop クラスターに Hue をインストールして使用する

HDInsight Linux クラスターに Hue をインストールし、トンネリングを利用して Hue に要求を送信する方法について学習します。

## Hue とは

Hue は Hadoop クラスターとの情報のやりとりに使用される一連の Web アプリケーションです。 Hue を利用して Hadoop クラスターに関連付けられているストレージを閲覧したり (HDInsight クラスターの場合、WASB)、Hive ジョブや Pig スクリプトを実行したりできます。HDInsight Hadoop クラスターにインストールした Hue では次のコンポーネントがサポートされます。

* Beeswax Hive エディター
* Pig
* メタストア マネージャー
* Oozie
* FileBrowser (WASB の既定のコンテナーと通信)
* ジョブ ブラウザー


## スクリプト アクションを使用した Hue のインストール

 [Https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/install-hue-uber-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/install-hue-uber-v01.sh) スクリプト アクションを使用して、HDInsight クラスターを色合いをインストールします。 このセクションでは、Azure クラシック ポータルを使用してクラスターをプロビジョニングする際にこのスクリプトを使用する方法について説明します。

> [AZURE.NOTE] このスクリプトを使用してクラスターを作成するのに Azure PowerShell または HDInsight .NET SDK を使用することもできます。 これらのメソッドの使用に関する詳細については、次を参照してください。 [Script Action を使用する HDInsight のカスタマイズ クラスタ](hdinsight-hadoop-customize-cluster-linux.md)します。

1. 」の手順を使用してプロビジョニングを開始 [Linux 上の HDInsight クラスターをクラスター](hdinsight-hadoop-provision-linux-clusters.md#portal), が、プロビジョニングを完了できません。

    > [AZURE.NOTE] 色合いを HDInsight クラスターにインストールすることをお勧めのヘッドノードのサイズが少なくとも A4 (8 コア、14 GB メモリ)。

2.  **オプションの構成** ブレードで、 **スクリプトのアクション**, 、し、次のように、情報を提供します。

    ![Hue のスクリプト アクション パラメーターを指定します。](./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Provide script action parameters for Hue")

    * __名前__: スクリプト アクションのフレンドリ名を入力します。
    * __スクリプト URI__: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/install-hue-uber-v01.sh
    * __HEAD__: このチェック ボックス
    * __ワーカー__: 空白のままにします。
    * __ZOOKEEPER__: 空白のままにします。
    * __パラメーター__: スクリプトが必要ですが、 **クラスター管理用パスワード** をパラメーターとして。 これはクラスターのプロビジョニング時に指定したパスワードです。 パスワードのプロビジョニング時に重要な考慮事項を以下に示します。
        * クラスターのユーザー名が "admin" の場合は、パスワードを単一引用符で囲んで指定するだけで済みます。
        * クラスターのユーザー名が "admin" 以外の場合は、パラメーターを `-u [username] [password in single quotes]` として指定する必要があります。

3. 下部にある、 **Script Action**, を使用して、 **選択** 構成を保存] ボタンをクリックします。 最後に、使用して、 **選択** の下部にあるボタン、 **オプションの構成** オプションの構成情報の保存先のブレードです。

4. 」の説明に従って、クラスターのプロビジョニングを引き続き [Linux 上の HDInsight クラスターをクラスター](hdinsight-hadoop-provision-linux-clusters.md#portal)します。

## HDInsight クラスターで Hue を使用する

SSH トンネリングは、実行後、クラスターの Hue にアクセスする唯一の方法です。 SSH によるトンネリングでは、Hue が実行されているクラスターのヘッドノードにトラフィックが直接進むことができます。 クラスターがプロビジョニングを完了したら、次の手順で HDInsight Linux クラスターで Hue を使用します。

1. 内の情報を使用して [を使用して SSH トンネリング Ambari web UI、ResourceManager、JobHistory、NameNode、Oozie、およびその他の web UI にアクセスする](hdinsight-linux-ambari-ssh-tunnel.md) HDInsight クラスターに、クライアント システムからのトラフィックに対して SSH トンネルを作成し、プロキシとしての SSH トンネルを使用する Web ブラウザーを構成します。

2. SSH トンネルを作成し、これをトラフィックのプロキシとして使用するようにブラウザーを構成したら、ヘッド ノードのホスト名を見つける必要があります。 次の手順に従って、この情報を Ambari から取得します。

    1. ブラウザーでは、https://CLUSTERNAME.azurehdinsight.net に移動します。 ダイアログ ボックスが表示されたら、認証に管理者のユーザー名とパスワードを使用してサイトに入ります。
    
    2. ページの上部にあるメニューから選択 __ホスト__します。
    
    3. 始まるエントリを選択して __hn0__します。 ページが開くと、一番上にホスト名が表示されます。 ホスト名の形式は __hn0 CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net__します。 これは Hue に接続するときに使用するホスト名です。

2. SSH トンネルを作成し、ブラウザーがプロキシ トラフィックを構成すると後、は、ブラウザーを使用して、http://HOSTNAME:8888 で色合いポータルを開きます。 HOSTNAME を前の手順で Ambari から取得した名前に変えます。

    > [AZURE.NOTE] 最初にログインするときは、色合いポータルにログインするアカウントを作成する促されます。 ここで指定した資格情報はポータルに制限され、クラスターのプロビジョニング時に指定した管理者または SSH ユーザーの資格情報には関連しません。

    ![Hue ポータルにログインする](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Specify credentials for Hue portal")

### Hive クエリを実行する

1. 色合いポータルからクリックして **クエリ エディター**, 、順にクリック **Hive** Hive エディターを開きます。

    ![Hive を使用する](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Use Hive")

2.  **支援** ] タブの [ **データベース**, 、はず **hivesampletable**します。 これは HDInsight のすべての Hadoop クラスターに含まれるサンプル テーブルです。 右側のペインにサンプル クエリを入力しの出力を参照してください、 **結果** 、下のウィンドウ] タブの画面キャプチャで示すようにします。

    ![Hive クエリを実行する](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "Run Hive query")

    使用することも、 **グラフ** 結果のビジュアル表現を表示するタブをクリックします。

### クラスター ストレージを参照する

1. 色合いポータルからクリックして **ファイル ブラウザー** 、メニュー バーの右上隅にあります。

2. 既定では、ファイル ブラウザーが開きで、 **/ユーザー/myuser** ディレクトリ。 パスのユーザー ディレクトリの直前の斜線をクリックし、クラスターに関連付けられている Azure ストレージ コンテナーのルートに移動します。

    ![ファイル ブラウザーを使用する](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "Use file browser")

3. ファイルまたはフォルダーを右クリックし、利用可能な操作を表示します。 使用して、 **アップロード** 右隅にある現在のディレクトリにファイルをアップロード] ボタンをクリックします。 使用して、 **新規** を新しいファイルやディレクトリを作成する] ボタンをクリックします。

> [AZURE.NOTE] 色合いファイル ブラウザーに表示できるは、HDInsight クラスターに関連付けられている既定のコンテナーの内容のみです。 追加のストレージ アカウント/コンテナーがクラスターに関連付けられている場合、ファイル ブラウザーではアクセスできません。 ただし、Hive ジョブの場合、クラスターに関連付けられている追加のコンテナーに常にアクセスできます。 たとえば、Hive エディターに `dfs -ls wasb://newcontainer@mystore.blob.core.windows.net` コマンドを入力すると、追加コンテナーのコンテンツを表示できます。 このコマンドで、 **新しいコンテナー** クラスターに関連付けられている既定のコンテナーではありません。

## 重要な考慮事項

1. Hue のインストールに使用されるスクリプトはクラスターのヘッド ノード 0 にのみ Hue をインストールします。

2. インストール中、複数の Hadoop サービス (HDFS、YARN、MR2、Oozie) が再起動し、構成を更新します。 スクリプトが Hue のインストールを完了した後、他の Hadoop サービスが起動するまで少し時間がかかる場合があります。 最初はこれが Hue のパフォーマンスに影響を与えることがあります。 すべてのサービスが起動すると、Hue が完全に機能します。

3.  Hue は Hive の現在の既定である Tez ジョブを認識しません。 Hive 実行エンジンとして MapReduce を使用する場合、スクリプトで次のコマンドを使用するようにスクリプトを更新します。

        set hive.execution.engine=mr;

4.  Linux クラスターの場合、サービスをヘッド ノード 0 で実行し、Resource Manager をヘッド ノード 1 で実行するシナリオがありえます。 そのようなシナリオの場合、Hue を利用してクラスターで「実行中」のジョブの詳細を表示するとき、エラーが発生する可能性があります (下の画像を参照)。 ただし、ジョブが完了したときにジョブの詳細を表示できます。

    ![Hue ポータル エラー](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Hue portal error")

    これは既知の問題によるものです。 この問題を回避するには、アクティブな Resource Manager もヘッド ノード 0 で実行されるように Ambari を変更します。

5.  HDInsight クラスターが `wasb://` で Azure Storage を使用するとき、Hue は WebHDFS を認識します。 そのため、スクリプト アクションで使用されるカスタム スクリプトは WebWasb をインストールします。これは WASB と通信するための WebHDFS 互換サービスです。 色合いポータルという場所での HDFS 場合でも (マウス ポインターを移動するときのように、 **ファイル ブラウザー**)、これは WASB として解釈する必要があります。


## 次のステップ

- [インストールして HDInsight クラスターで Spark を使用する](hdinsight-hadoop-spark-install-linux.md) を使用する方法についてのクラスターをインストールして HDInsight Hadoop クラスターで Spark を使用してカスタマイズします。 Spark はビッグ データ分析アプリケーションのパフォーマンスを向上するメモリ内の処理をサポートする、オープン ソースの並列処理のフレームワークです。

- [HDInsight クラスターに Giraph をインストール](hdinsight-hadoop-giraph-install-linux.md)します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに Giraph をインストールします。 Giraph は、Hadoop でグラフの処理を実行するために使用でき、Azure HDInsight で使用できます。

- [HDInsight クラスターに Solr をインストール](hdinsight-hadoop-solr-install-linux.md)します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに Solr をインストールします。 Solr は、格納されたデータに対して強力な検索操作を実行することができます。

- [HDInsight クラスターで R をインストール](hdinsight-hadoop-r-scripts-linux.md)します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに R をインストールします。 R は、統計計算用のオープン ソースの言語および環境です。 R は、数百の組み込み統計関数と、関数型プログラミングとオブジェクト指向のプログラミングの特徴を結合した独自のプログラミング言語を提供します。 また、広範なグラフィカル機能も提供します。

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install-linux.md

