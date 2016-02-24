<properties 
    pageTitle="Media Encoder Standard を使用して資産をエンコードする方法" 
    description="このトピックでは、.NET を使用し、Media Encoder Standard で資産をエンコードする方法について紹介します。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako,anilmur" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/02/2015"  
    ms.author="juliako"/>


#Media Encoder Standard を使用して資産をエンコードする方法

エンコード ジョブは Media Services で最も一般的な処理の 1 つです。 エンコード ジョブを作成することで、メディア ファイルをあるエンコードから別のエンコードに変換できるようになります。 エンコードには、Media Services の組み込み Media Encoder を使用できます。 Media Services のパートナーから提供されているエンコーダーを使うこともできます。サードパーティのエンコーダーは Azure Marketplace から入手できます。 

このトピックでは、.NET を使用し、Media Encoder Standard (MES) で資産をエンコードする方法について紹介します。 メディア エンコーダー スタンダードが構成されている説明されているエンコーダー プリセットのいずれかを使用して [ここ](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409)します。

常に中間ファイルをアダプティブ ビットレート MP4 セットにエンコードしを使用して、目的の形式に変換することを推奨、 [動的パッケージング](media-services-dynamic-packaging-overview.md)します。 動的パッケージを活用するには、コンテンツの配信元となるストリーミング エンドポイントのオンデマンド ストリーミング ユニットを 1 つ以上取得する 必要があります。 詳細については、次を参照してください。 [Media services の規模変更方法](media-services-manage-origins.md#scale_streaming_endpoints)します。

出力資産がストレージで暗号化されている場合は、資産配信のポリシーを構成する必要があります。 詳細については、次を参照してください。 [アセット配信ポリシーを構成する](media-services-dotnet-configure-asset-delivery-policy.md)です。

###MES の形式

[形式とコーデック](media-services-media-encoder-standard-formats.md)

###MES のプリセット

メディア エンコーダー スタンダードが構成されている説明されているエンコーダー プリセットのいずれかを使用して [ここ](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409)します。

###MES の入力メタデータと出力メタデータ

エンコーダーの入力のメタデータが説明されている [ここ](http://msdn.microsoft.com/library/azure/dn783120.aspx)します。

エンコーダーの出力のメタデータが説明されている [ここ](http://msdn.microsoft.com/library/azure/dn783217.aspx)します。


##サンプルのダウンロード

取得してからサンプルを実行 [ここ](http://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/)します。

##例

次のコード サンプルでは、Media Services SDK を使用して次のタスクを実行します。

- エンコード ジョブを作成します。
- Media Encoder Standard エンコーダーの参照を取得します。
- "H264 Multiple Bitrate 720p" プリセットを使用するように指定します。 すべてのプリセットをご覧 [ここ](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409)します。 また、これらのプリセットが従う必要があります、スキーマを確認することもできます。 [ここ](https://msdn.microsoft.com/library/mt269962.aspx) トピックです。
- 1 つのエンコード タスクをジョブに追加します。 
- エンコードする入力資産を指定します。
- エンコードされた資産が含まれる出力資産を作成します。
- ジョブの進行状況を確認するイベント ハンドラーを追加します。
- ジョブを送信します。
        
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        

            // Create a task with the encoding details, using a string preset.
            // In this case "H264 Multiple Bitrate 720p" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "H264 Multiple Bitrate 720p",
                TaskOptions.None);
        
            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);
        
            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
            return job.OutputMediaAssets[0];
        }
        
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
                    break;
                case JobState.Canceling:
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    Console.WriteLine("Please wait...\n");
                    break;
                case JobState.Canceled:
                case JobState.Error:
        
                    // Cast sender as a job.
                    IJob job = (IJob)sender;
        
                    // Display or log error details as needed.
                    break;
                default:
                    break;
            }
        }
        
        
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
            return processor;
        }


##Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##関連項目 

[.NET を使用してメディア エンコーダー スタンダードを使用してサムネイルを生成する方法](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services エンコードの概要](media-services-encode-asset.md)
