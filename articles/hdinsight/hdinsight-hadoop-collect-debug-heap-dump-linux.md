<properties
    pageTitle="HDInsight で Hadoop サービスのヒープ ダンプを有効にする | Microsoft Azure"
    description="デバッグと分析のために Linux ベースの HDInsight クラスターから Hadoop サービスのヒープ ダンプを有効にする"
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
    ms.date="12/04/2015"
    ms.author="larryfr"/>


#Linux ベースの HDInsight で Hadoop サービスのヒープ ダンプを有効にする (プレビュー)

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

ヒープ ダンプには、ダンプが作成された時点の変数の値を含む、アプリケーションのメモリのスナップショットが含まれています。 これらは、実行時に発生する問題を診断するのに便利です。

> [AZURE.NOTE] この記事の情報は、Linux ベースの HDInsight にのみ適用されます。 Windows ベースの HDInsight については、次を参照してください [Windows ベースの HDInsight で Hadoop サービスの有効化のヒープ ダンプ。](hdinsight-hadoop-collect-debug-heap-dumps.md)

## <a name="whichServices"></a>サービス

次のサービスのヒープ ダンプを有効にできます。

*  **hcatalog** -tempelton
*  **hive** -hiveserver2、metastore、derbyserver
*  **mapreduce** -jobhistoryserver
*  **yarn** -resourcemanager、nodemanager、timelineserver
*  **hdfs** -datanode、secondarynamenode、namenode

HDInsight によって実行されるマップと削減のプロセスに対して、ヒープ ダンプを有効にすることもできます。

## <a name="configuration"></a>ヒープ ダンプ構成について

ヒープ ダンプは、サービスの開始時にオプション (opts またはパラメーターとも呼ばれる) を JVM に渡すことによって有効になります。 ほとんどの Hadoop サービスでは、サービスを開始するのに使用されるシェル スクリプトを変更することでこれを実現できます。

各スクリプトには、エクスポートが **\*\_OPTS**, 、JVM に渡されるオプションが含まれています。 たとえば、 **hadoop env.sh** スクリプトで始まる行 `export HADOOP_NAMENODE_OPTS=` NameNode サービスのオプションが含まれています。

これらが MapReduce サービスの子プロセスである場合は、マップと削減のプロセスが多少異なります。 マップや削減のプロセスはそれぞれ子コンテナーで実行され、これらの JVM オプションを含む 2 つのエントリがあります。 含まれる両方 **mapred-site.xml**:

* **mapreduce.admin.map.child.java.opts**
* **mapreduce.admin.reduce.child.java.opts**

> [AZURE.NOTE] Ambari がクラスター内のノードで変更をレプリケートする処理は、Ambari を使用してスクリプトと mapred-site.xml 設定の両方を変更することをお勧めします。 参照してください、 [を使用して Ambari](#using-ambari) 具体的な手順についてのセクションです。

###ヒープ ダンプを有効にする

次のオプションは、OutOfMemoryError が発生したときに、ヒープ ダンプを有効にします。

    -XX:+HeapDumpOnOutOfMemoryError

 **+** このオプションが有効になっていることを示します。 既定では無効になっています。

> [AZURE.WARNING] ヒープ ダンプは無効な HDInsight で Hadoop サービスの既定では、ように、ダンプ ファイルが大きくなることができます。 トラブルシューティングのために有効にした場合は、問題を再現してダンプ ファイルを収集したら、忘れずに無効にしてください。

###ダンプの場所

ダンプ ファイルの既定の場所は、現在の作業ディレクトリです。 次のオプションを使用して、ファイルの保存先を制御できます。

    -XX:HeapDumpPath=/path

たとえば、`-XX:HeapDumpPath=/tmp` を使用すると、ダンプは /tmp ディレクトリに保存されます。

###スクリプト

スクリプトをトリガーすることもできます。 ときに、 **OutOfMemoryError** に発生します。 たとえば、通知をトリガーすると、エラーが発生したことがわかります。 これは、次のオプションを使用して制御されます。

    -XX:OnOutOfMemoryError=/path/to/script

> [AZURE.NOTE] Hadoop は分散システムであるためで、サービスが実行されているクラスター内のすべてのノードで使用する任意のスクリプトを配置する必要があります。
>
> また、スクリプトはサービスが実行するアカウントからアクセスできる場所にあり、実行権限を提供する必要もあります。 たとえば、スクリプトを `/usr/local/bin` に保存し、`chmod go+rx /usr/local/bin/filename.sh` を使用して読み取りと実行権限を付与できます。

##Ambari の使用

サービスの構成を変更するには、次の手順を実行します。

1. クラスターの Ambari Web UI を開きます。 URL は https://YOURCLUSTERNAME.azurehdinsight.net になります。

    メッセージが表示されたら、HTTP のアカウント名 (既定値: admin) とクラスターのパスワードを使用してサイトを認証します。

    > [AZURE.NOTE] 必要がありますをもう一度 Ambari でのユーザー名とパスワード。 その場合は、同じアカウント名とパスワードを再入力してください。

2. 左側の一覧を使用して、変更するサービス領域を選択します。 たとえば、 **HDFS**します。 中央の領域で、選択、 **Configs** ] タブをクリックします。

    ![HDFS Configs タブが選択されている Ambari Web の画像](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. 使用して、 **フィルター]** エントリ、入力 **opts**します。 これには、このテキストを含むものに対してのみ構成項目の一覧をフィルター処理はされ、シェル スクリプトを検索する簡単な方法は、または **テンプレート** をこれらのオプションの設定を使用できます。

    ![フィルター処理された一覧](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. 検索、 **\*\_OPTS** をヒープ ダンプを有効にし、有効にするオプションを追加するサービスのエントリ。 次の図では、追加した `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` に、 **HADOOP\_NAMENODE\_OPTS** エントリ。

    ![HADOOP_NAMENODE_OPTS with -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

    > [AZURE.NOTE] ヒープを有効にすると、マップのダンプまたは、子プロセスの削減、というフィールド参照代わりに **mapreduce.admin.map.child.java.opts** と **mapreduce.admin.reduce.child.java.opts**します。

    使用して、 **保存** 変更を保存] ボタンをクリックします。 変更を説明する短いメモを入力できます。

5. 変更が適用されたら、 **再起動が必要** 1 つまたは複数のサービスの横にアイコンが表示されます。

    ![再起動が必要アイコンと [再起動] ボタン](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. は、再起動が必要な各サービスを選択し、使用して、 **サービス アクション** ボタンを **メンテナンス モードを有効にする**です。 これは、再起動したときに、このサービスからアラートが生成されないようにします。

    ![[メンテナンス モードの有効化] メニュー](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. 使用してメンテナンス モードを有効にすると、 **再起動** ボタンにサービスを **再起動すべて影響を受ける**

    ![[すべて再起動] エントリ](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

    > [AZURE.NOTE] エントリ、 **再起動** ボタンは、他のサービスの異なる場合があります。

8. サービスを再開するを使用して、 **サービス アクション** ボタン **メンテナンス モードの無効化**します。 これにより、Ambari はサービスのアラートの監視を再開します。

