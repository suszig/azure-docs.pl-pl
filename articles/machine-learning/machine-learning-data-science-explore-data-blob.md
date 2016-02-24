<properties 
    pageTitle="Pandas を使用して Azure BLOB ストレージのデータを探索する | Microsoft Azure" 
    description="Pandas を使用して Azure BLOB コンテナーに格納されているデータを探索する方法について説明します。" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2015" 
    ms.author="fashah;garye;bradsev" /> 

#Pandas を使用して Azure BLOB ストレージのデータを探索する

これは、 **メニュー** ツールを使用して、記憶域のさまざまな環境からデータを探索する方法を説明するトピックへのリンク。 このタスクは、Cortana Analytics Process (CAP) の 1 ステップです。

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## はじめに

このドキュメントを使用して Azure blob コンテナーに格納されているデータを探索する方法を説明する [Pandas](http://pandas.pydata.org/) Python パッケージです。

## 前提条件
この記事では、以下のことを前提としています。

* Azure のストレージ アカウントが作成されている。 手順を知りたい場合は、次を参照してください [Azure ストレージ アカウントの作成。](../hdinsight-get-started.md#storage)
* Azure BLOB ストレージ アカウントにデータが格納されている。

## Pandas データ フレームにデータを読み込む
データセットを探索および操作するには、データを BLOB ソースからローカル ファイルにダウンロードする必要があります。このローカル ファイルは、Pandas データ フレームに読み込むことができます。 この手順に必要な操作は次のとおりです。

1. BLOB サービスを使用する次の Python コード サンプルで Azure BLOB からデータをダウンロードします。 次のコードの変数を、実際の値に置き換えます。 

        from azure.storage import BlobService
        import tables
        
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))


2. ダウンロードしたファイルから、Pandas データ フレームにデータを読み込みます。

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

これで、データを探索し、このデータセットでの特徴を生成する準備が整いました。

##<a name="blob-dataexploration"></a>Pandas を使用してデータの探索の例

次に、Pandas を使用してデータを探索する方法の例をいくつかを示します。

1. 検査、 **行と列の数** 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. **検査** 最初または最少数 **行** 、データセットを次に示すようにします。

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. チェック、 **データ型** 次のサンプル コードを使用して、インポートされた各列
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. チェック、 **基本的な統計** の次のように設定するデータの列
 
        dataframe_blobdata.describe()
    
5. 次のように、各列の値のエントリの数を確認します。

        dataframe_blobdata['<column_name>'].value_counts()

6. **欠損値のカウント** と、次のサンプル コードを使用して各列のエントリの実際の数

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  あれば **欠損値** データの特定の列に対するに削除できます次のようにします。

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    不足値を置き換えるを別の方法として、モード関数を使用した以下のものを実行します。
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. 作成、 **ヒストグラム** 変数の分布をプロットする可変個の箱を使用してプロット 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. 見て **相関関係** 、散布図を使用するか、組み込みの相関関数を使用して変数の間

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

