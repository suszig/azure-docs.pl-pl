<properties 
    pageTitle="リソース マネージャーを使用した HDInsight の Apache Spark クラスターへのリソースの割り当て | Microsoft Azure" 
    description="リソース マネージャーを使用して HDInsight の Spark クラスターのパフォーマンスを向上させる方法について説明します。" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="paulettm" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/03/2015" 
    ms.author="nitinme"/>


# Azure HDInsight での Apache Spark クラスターのリソースの管理

リソース マネージャーは Spark クラスター ダッシュボードのコンポーネントであり、クラスターで実行される各アプリケーションによって使用されるコアや RAM などのリソースを管理できます。

## <a name="launchrm"></a>リソース マネージャーの起動方法

1.  [Azure ポータル](https://ms.portal.azure.com/), 、スタート ボードで、タイルをクリックして、Spark クラスターの場合、スタート画面に固定表示)。 [クラスターに移動することもできます。 **すべてを参照** > **HDInsight クラスター**します。 
 
2. Spark クラスター ブレードで、クリックして **ダッシュ ボード**します。 入力を求められたら、Spark クラスターの管理者資格情報を入力します。

    ![リソース マネージャーを起動する](./media/hdinsight-apache-spark-resource-manager/HDI.Cluster.Launch.Dashboard.png "Start Resource Manager")   

##<a name="scenariosrm"></a>リソース マネージャーを使用して問題を解決する方法

ここでは、Spark クラスターでよく発生することがある状況と、リソース マネージャーを使用してそれに対処する方法を説明します。

### HDInsight の Spark クラスターが遅い

HDInsight の Apache Spark クラスターはマルチテナント用に設計されているので、リソースは複数のコンポーネントに分割されます (Notebook、ジョブ サーバーなど)。 これにより、コンポーネントが実行するためのリソースを取得できないことを心配することなくすべての Spark コンポーネントを同時に使用できますが、リソースがフラグメント化されるため各コンポーネントは遅くなります。  これは、ニーズに基づいて調整できます。 


### Spark クラスターを含む Jupyter Notebook のみを使用します。 どうすればすべてのリソースを割り当てることができますか。

1.  **Spark ダッシュ ボード**, をクリックして、 **Spark UI** コアと、アプリケーションに割り当てることができる最大 ram 容量の最大数を確認するタブをクリックします。

    ![リソースの割り当て](./media/hdinsight-apache-spark-resource-manager/HDI.Spark.UI.Resource.png "Find resources allocated to a Spark cluster")

    上の画面キャプチャでは、割り当て可能な最大コア数は 7 (合計 8 コアのうち 1 つは使用中) であり、割り当て可能な最大 RAM は 9 GB (合計 12 GB の RAM のうち、2 GB はシステム用、1 GB は他のアプリケーションで使用中) です。

    実行しているすべてのアプリケーションを考慮する必要もあります。 実行中のアプリケーションを見ることができます、 **Spark UI** ] タブをクリックします。

    ![アプリケーションの実行](./media/hdinsight-apache-spark-resource-manager/HDI.Spark.UI.Running.Apps.png "Applications running on the cluster")

    
2. HDInsight の Spark ダッシュ ボード] をクリックして、 **リソース マネージャー** ] タブで値を指定し、 **既定のアプリケーション コア数** と **既定のワーカー ノードごとの実行プログラム メモリ**します。 他のプロパティは 0 に設定します。

    ![リソースの割り当て](./media/hdinsight-apache-spark-resource-manager/HDI.Spark.UI.Allocate.Resources.png "Allocate resources to your applications")

### Spark クラスターで BI ツールを使用していません。 どのようにしてリソースを取り戻せばいいですか。 

Thrift サーバーのコア数と Thrift サーバーの実行プログラム メモリを 0 と指定します。 コアやメモリを割り当て、thrift サーバーに変わり、 **待機している** 状態です。

![リソースの割り当て](./media/hdinsight-apache-spark-resource-manager/HDI.Spark.UI.No.Thrift.png "No resources to the thrift server")

##<a name="seealso"></a>関連項目

* [概要: Azure HDInsight での Apache Spark](hdinsight-apache-spark-overview.md)
* [HDInsight クラスターでの Spark のプロビジョニング](hdinsight-apache-spark-provision-clusters.md)
* [BI ツールを使用して HDInsight で Spark を使用して対話型データ分析を実行します。](hdinsight-apache-spark-use-bi-tools.md)
* [Machine Learning アプリケーションを作成するための HDInsight での Spark の使用](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [リアルタイム ストリーミング アプリケーションを作成するための HDInsight での Spark の使用](hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming.md)


[hdinsight-versions]: ../hdinsight-component-versioning/
[hdinsight-upload-data]: ../hdinsight-upload-data/
[hdinsight-storage]: ../hdinsight-use-blob-storage/


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: ../storage-create-storage-account/ 


