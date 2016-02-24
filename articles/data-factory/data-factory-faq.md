<properties 
    pageTitle="Azure Data Factory - よく寄せられる質問" 
    description="Azure データ ファクトリについてよく寄せられる質問です。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/01/2015" 
    ms.author="spelluru"/>

# Azure Data Factory - よく寄せられる質問

## 一般的な質問

### Azure Data Factory とは何ですか。

Data Factory は、データの移動や変換を調整し自動化するクラウドベースのデータ統合サービスです。 原材料を機械で加工して最終製品を作成する工場と同じように、Data Factory は生データを収集してすぐに使用できる情報に変換する既存のサービスを調整します。 

Data Factory は、オンプレミスとクラウドのデータ ソースおよび SaaS で動作し、データの取り込み、準備、変換、分析、および発行を行います。  サービスを作成する Data Factory を使用するようにサービスを使用してデータを変換するデータ フロー パイプラインを管理する [Azure HDInsight (Hadoop)](http://azure.microsoft.com/documentation/services/hdinsight/) と [Azure Batch](http://azure.microsoft.com/documentation/services/batch/) 、ビッグ データのコンピューティング ニーズに応えるの [Azure Machine Learning](http://azure.microsoft.com/documentation/services/machine-learning/) 分析ソリューションを運用します。  単なる表形式の監視ビューではなく、Data Factory のリッチな視覚化機能を使用してデータ パイプライン間の系列と依存関係をすばやく表示します。 1 つの統一されたビューからすべてのデータ フロー パイプラインを監視し、問題を簡単に特定して監視アラートを設定します。

参照してください [の概要と主要な概念](data-factory-introduction.md) 詳細です。 
 
### Data Factory では顧客のどのような課題が解決されますか。

Azure Data Factory は、従来のリレーショナル ストレージと非構造化データを対象とした多様なデータ保存、処理、および移動のサービスを機敏に活用できると共に、完全に管理されたサービスの制御と監視の機能を提供します。

### Data Factory の対象ユーザーはだれですか。


- Hadoop とその他のシステム間における統合サービスの構築を担当しているデータ開発者。
    - 絶えず変化と拡大を続けるデータ環境への対応と統合を行う必要がある。
    - 情報生成システム用のカスタム コードを記述する必要があり、高価な管理が困難、高可用性ではありませんが、フォールト トレランスを提供

- 多様化するデータを IT インフラストラクチャに組み込む方法を探している IT プロフェッショナル。
    - 組織内のすべてのデータから豊富なビジネスの洞察を導き出す必要がある。
    - コンピューティングとストレージのリソースを管理し、オンプレミスとクラウドの間でコストと規模のバランスをとる必要がある。
    - 新しいビジネス ニーズに対応するためにさまざまなソースと処理を短時間で追加しつつ、すべてのコンピューティング資産とストレージ資産の全体像を常に把握する必要がある。

### Azure Data Factory の料金の詳細はどこで確認できますか。

参照してください [Data Factory の料金の詳細ページ][adf-pricing-details] の Azure Data Factory の料金の詳細。  

### Azure Data Factory の利用はどのように開始するのですか。

- Azure Data Factory の概要については、次を参照してください。 [Azure Data Factory の概要][adf-introduction]します。
- クイック チュートリアルについては、次を参照してください。 [Azure Data Factory を使ってみる][adfgetstarted]します。
- 包括的なドキュメントを参照してください。 [Azure Data Factory のドキュメント][adf-documentation-landingpage]します。

  
### 顧客は Data Factory にどのようにアクセスしますか。

顧客が通じて Data Factory へのアクセスを取得できます、 [Azure ポータル][azure-portal]します。

### Data Factory を利用可能なリージョンはどこですか。

Data Factory は米国西部と北ヨーロッパで使用できます。 Data Factory で使用されるコンピューティング サービスとストレージ サービスは、その他のリージョンでも利用できます。
 
### Data Factory/パイプライン/アクティビティ/データセットの数の制限値はいくつですか。
 
参照してください **Azure Data Factory の制限** のセクションで、 [Azure サブスクリプションとサービスの制限、クォータ、および制約](../azure-subscription-service-limits.md#data-factory-limits) 記事です。


### Azure Data Factory サービスではどのような作成エクスペリエンスまたは開発者エクスペリエンスが提供されますか。

データ ファクトリを生成または作成する方法には、以下のものがあります。

- **Azure ポータル**します。 Azure ポータルの Data Factory ブレードは、データ ファクトリ、およびリンクされたサービスを作成するための優れたユーザー インターフェイスです。  **Data Factory エディター**, 、これは、ポータルの一部でも、これらの成果物の JSON の定義を指定することによって、リンクされたサービス、テーブル、データ セット、およびパイプラインを簡単に作成できます。 参照してください [Data Factory エディター][data-factory-editor] 、エディターの概要については、 [Data Factory を使ってみる][datafactory-getstarted] ポータルやエディターを使用して作成し、データ ファクトリの展開の例についてです。   
- **Azure PowerShell**します。 PowerShell ユーザーがポータル UI ではなく PowerShell の使用を希望している場合は、Azure PowerShell の一部として出荷される Azure Data Factory コマンドレットを使用してデータ ファクトリを作成し、デプロイできます。 参照してください [の Azure PowerShell を使用して Azure Data Factory の監視と管理][create-data-factory-using-powershell] 単純な例について、 [チュートリアル: の移動し、Data Factory を使用してログ ファイルを処理][adf-tutorial] PowerShell を使用して高度な例については広告を作成するページがデータ ファクトリを展開します。 参照してください [Data Factory コマンドレット リファレンス][adf-powershell-reference] Data Factory コマンドレットの包括的なドキュメントについては、MSDN ライブラリのコンテンツ。  
- **Visual Studio**します。 また、Visual Studio を使用して、プログラムによりデータ ファクトリを作成、監視、管理できます。 参照してください [の作成、監視、および Data Factory .NET SDK を使用して Azure データ ファクトリを管理](data-factory-create-data-factories-programmatically) の詳細については、資料です。  
- **.NET クラス ライブラリ**します。 Data Factory .NET SDK を使用すると、プログラムでデータ ファクトリを作成できます。 参照してください [の作成、監視、および .NET SDK を使用してデータ ファクトリを管理][create-factory-using-dotnet-sdk] .NET SDK を使用してデータ ファクトリの作成のチュートリアルです。 参照してください [Data Factory クラス ライブラリ リファレンス][msdn-class-library-reference] の Data Factory .NET SDK の包括的なドキュメントです。  
- **REST API**します。 Azure Data Factory サービスで公開されている REST API を使用して、データ ファクトリを作成およびデプロイすることもできます。 参照してください [Data Factory REST API リファレンス][msdn-rest-api-reference] の Data Factory REST API の包括的なドキュメントです。 

### Data Factory の名前を変更できますか。
いいえ、できません。 その他の Azure リソースと同様に、Azure data factory の名前を変更できません。 

## アクティビティ - FAQ
### どのようなデータ ソースとアクティビティがサポートされますか。

参照してください [データ移動アクティビティ](data-factory-data-movement-activities.md) と [データ変換のアクティビティ](data-factory-data-transformation-activities.md) サポートされているデータ ソースとアクティビティに関する記事です。  

### アクティビティはいつ実行されますか。
 **可用性** 、出力データ テーブルの構成設定では、アクティビティが実行されるタイミングによって決定されます。 アクティビティは、入力データのすべての依存関係が満たされているかどうかを確認 (つまり、 **準備** 状態) 実行を開始する前にします。

## コピー アクティビティ - FAQ
### 1 つのパイプラインに複数のアクティビティを設定する方法とアクティビティごとに別個のパイプラインを使用する方法ではどちらの方法がよいですか。 
パイプラインでは、関連する複数のアクティビティをまとめることが想定されています。  論理的には、複数のアクティビティを接続するテーブルがパイプラインの外部の他のアクティビティによって使用されない場合は、1 つのパイプラインにそれらのアクティビティを保持できます。 これにより、パイプラインのアクティブな期間を揃えるためにこれを連結する必要はありません。 また、パイプライン内部のテーブルのデータの整合性が、パイプラインを更新するときによりうまく保持されるようになります。 パイプラインを更新するとき、基本的にパイプライン内のすべてのアクティビティが停止、削除された後、もう一度作成されます。 作成操作の観点では、パイプラインの 1 つの JSON ファイルで関連するアクティビティ内のデータのフローを簡単に見ることができるようになる可能性があります。 

## HDInsight アクティビティ - FAQ

### HDInsight でサポートされているリージョンはどこですか。

次の記事の利用可能な地域を参照してください。 または [HDInsight の料金詳細][hdinsight-supported-regions]します。

### オンデマンドの HDInsight クラスターはどのリージョンで使用されますか。

オンデマンドの HDInsight クラスターは、クラスターで使用するように指定したストレージが存在するのと同じリージョンに作成されます。    

### 追加のストレージ アカウントを HDInsight クラスターに関連付けるにはどうすればよいですか。

独自の HDInsight クラスター (BYOC - Bring Your Own Cluster: クラスターを自分で用意する) を使用している場合は、次のトピックをご覧ください。 

- [代替のストレージ アカウントとメタストアでの HDInsight クラスターの使用][hdinsight-alternate-storage]
- [HDInsight ハイブでの追加のストレージ アカウントの使用][hdinsight-alternate-storage-2]

出荷時のデータ サービスによって作成された、オンデマンドでクラスターを使用している場合は、HDInsight 用の追加のストレージ アカウントにリンクされるので、データのファクトリのサービスが自動的に登録できます考慮すべきサービスを指定する必要があります。 オンデマンドのリンクされたサービスの JSON 定義で使用して **additionalLinkedServiceNames** プロパティを次の JSON スニペットで示すように、代替のストレージ アカウントを指定します。
 
    {
        "name": "MyHDInsightOnDemandLinkedService",
        "properties":
        {
            "type": "HDInsightOnDemandLinkedService",
            "clusterSize": 1,
            "timeToLive": "00:01:00",
            "linkedServiceName": "LinkedService-SampleData",
            "additionalLinkedServiceNames": [ "otherLinkedServiceName1", "otherLinkedServiceName2" ] 
        }
    } 

上記の例において、otherLinkedServiceName1 と otherLinkedServiceName2 は、HDInsight クラスターが代替ストレージ アカウントにアクセスするために必要な資格情報がその定義に含まれているリンクされたサービスを表します。

## スライス - FAQ

### スライスを再実行するにはどうすればよいですか。
スライスを再実行するには、次のどちらかの方法を使用します。 

- クリックして **実行** で、コマンド バーで、 **データ スライス** スライス、ポータルのブレードです。 
- 実行 **セット AzureRmDataFactorySliceStatus** 状態に設定 **PendingExecution** のあるスライスです。   
    
        Set-AzureRmDataFactorySliceStatus -Status PendingExecution -ResourceGroupName $ResourceGroup -DataFactoryName $df -TableName $table -StartDateTime "02/26/2015 19:00:00" -EndDateTime "02/26/2015 20:00:00" 

参照してください [セット AzureRmDataFactorySliceStatus][set-azure-datafactory-slice-status] 、コマンドレットに関する詳細。 

### スライスの処理にかかった時間を調べるにはどうすればよいですか。
1. クリックして **データセット** タイルを **DATA FACTORY** data factory のブレードです。
2. 特定のデータセットをクリックして、 **データセット** ブレードです。
3. 興味のあるスライスを選択、 **最近のスライス** ボックスの一覧で、 **テーブル** ブレードです。
4. [アクティビティの実行] をクリックして、 **アクティビティの実行** ボックスの一覧で、 **データ スライス** ブレードです。 
5. クリックして **プロパティ** タイルを **アクティビティの実行の詳細** ブレードです。 
6. 確認する必要があります、 **期間** フィールド値に表示します。 これが、スライスの処理にかかった時間です。   

### 実行中のスライスを停止するにはどうすればよいですか
使用することができます、パイプラインの実行を停止する場合は、 [Suspend AzureRmDataFactoryPipeline](https://msdn.microsoft.com/library/mt603721.aspx) コマンドレットです。 現時点では、パイプラインを中断しても、進行中のスライスの実行は停止しません。 進行中の実行が完了すると、追加のスライスは取得されません。

すべての実行をすぐに停止するには、パイプラインをいったん削除した後で再作成するしかありません。 パイプラインを削除する場合は、パイプラインによって使用されているテーブルとリンクされたサービスを削除する必要はありません。 



[image-rerun-slice]: ./media/data-factory-faq/rerun-slice.png

[adfgetstarted]: data-factory-get-started.md
[adf-introduction]: data-factory-introduction.md
[adf-troubleshoot]: data-factory-troubleshoot.md
[data-factory-editor]: data-factory-editor.md
[datafactory-getstarted]: data-factory-get-started.md
[create-data-factory-using-powershell]: data-factory-monitor-manage-using-powershell.md
[adf-tutorial]: data-factory-tutorial.md
[create-factory-using-dotnet-sdk]: data-factory-create-data-factories-programmatically.md
[msdn-class-library-reference]: https://msdn.microsoft.com/library/dn883654.aspx
[msdn-rest-api-reference]: https://msdn.microsoft.com/library/dn906738.aspx

[adf-powershell-reference]: https://msdn.microsoft.com/library/dn820234.aspx 
[adf-documentation-landingpage]: http://go.microsoft.com/fwlink/?LinkId=516909
[azure-portal]: http://portal.azure.com
[set-azure-datafactory-slice-status]: https://msdn.microsoft.com/library/mt603522.aspx

[adf-pricing-details]: http://go.microsoft.com/fwlink/?LinkId=517777
[hdinsight-supported-regions]: http://azure.microsoft.com/pricing/details/hdinsight/
[hdinsight-alternate-storage]: http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx
[hdinsight-alternate-storage-2]: http://blogs.msdn.com/b/cindygross/archive/2014/05/05/use-additional-storage-accounts-with-hdinsight-hive.aspx
 

