### 圧縮のサポート  
大量のデータセットを処理すると、I/O およびネットワークにボトルネックが生じる可能性があります。 そのため、データを圧縮して保存すると、ネットワークでのデータ転送速度が上昇してディスク領域を節約できるだけでなく、ビッグ データの処理性能を大幅に高めることができます。 現時点では、圧縮は Azure BLOB やオンプレミスのファイル システムなど、ファイルベースのデータ ストアでサポートされています。  

データセットの圧縮を指定するには、使用、 **圧縮** 次の例に示すように、データセット JSON のプロパティ。   

    {  
        "name": "AzureBlobDataSet",  
        "properties": {  
            "availability": {  
                "frequency": "Day",  
                "interval": 1  
            },  
            "type": "AzureBlob",  
            "linkedServiceName": "StorageLinkedService",  
            "typeProperties": {  
                "fileName": "pagecounts.csv.gz",  
                "folderPath": "compression/file/",  
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            }  
        }  
    }  
 
なお、 **圧縮** セクションには 2 つのプロパティ。  
  
- **型:** になる可能性が圧縮コーデック **GZIP**, 、**Deflate** または **BZIP2**します。  
- **レベル:** になる可能性が圧縮比率 **最適** または **Fastest**します。 
    - **最も高速な:** 、生成されたファイルが最適に圧縮されていない場合でも、圧縮操作をできるだけ早く完了する必要があります。 
    - **最適な**: 圧縮操作を最適に圧縮する、場合でも、操作を完了に時間がかかります。 
    
    参照してください [圧縮レベル](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) についても説明します。 

前述のサンプル データセットをコピー アクティビティの出力として使用し、コピー アクティビティで出力データを GZIP コーデックによって最適な圧縮率で圧縮してから、Azure BLOB Storage の pagecounts.csv.gz という名前のファイルに圧縮データを書き込む場合を考えます。   

入力データセットの JSON で compression プロパティを指定すると、パイプラインでソースから圧縮データを読み取ることができ、出力データセットの JSON で compression プロパティを指定すると、コピー アクティビティにより出力先に圧縮データを書き込むことができます。 いくつかのサンプル シナリオを次に示します。 

- Azure BLOB から GZIP 圧縮データを読み取り、展開して、生成されたデータを Azure SQL Database に書き込みます。 この場合、Azure BLOB 入力データセットを、compression JSON プロパティを使用して定義します。 
- オンプレミスのファイル システムのプレーンテキスト ファイルからデータを読み取り、GZip 形式で圧縮して、圧縮データを Azure BLOB に書き込みます。 この場合、Azure BLOB 出力データセットを、compression JSON プロパティを使用して定義します。  
- Azure BLOB から GZIP 圧縮データを読み取って展開し、BZIP2 で圧縮して、生成されたデータを Azure BLOB に書き込みます。 この場合、Azure BLOB 入力データセットは圧縮タイプを GZIP に設定して定義し、Azure BLOB 出力データセットは圧縮タイプを BZIP2 に設定して定義します。   

