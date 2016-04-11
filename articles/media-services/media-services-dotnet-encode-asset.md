<properties 
    pageTitle="Azure Media Encoder を使用して資産をエンコードする方法" 
    description="Azure Media Encoder を使用して Media Services でメディア コンテンツをエンコードする方法について説明します。 コード サンプルは C# で記述され、Media Services SDK for .NET を利用しています。" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="12/08/2015"  
    ms.author="juliako"/>


#Azure Media Encoder を使用して資産をエンコードする方法


> [AZURE.SELECTOR]
- [REST ()](media-services-rest-encode-asset.md)
- [.NET](media-services-dotnet-encode-asset.md)
- [ポータル](media-services-manage-content.md#encode)

##概要

インターネット経由でデジタル ビデオを配信するには、メディアを圧縮する必要があります。 デジタル ビデオ ファイルはサイズがとても大きく、インターネット経由で配信したり、顧客のデバイスでうまく表示したりできない可能性があります。 エンコードはビデオやオーディオを圧縮するプロセスです。圧縮することでメディア コンテンツが見やすくなります。

エンコード ジョブは Media Services で最も一般的な処理の 1 つです。 エンコード ジョブを作成することで、メディア ファイルをあるエンコードから別のエンコードに変換できるようになります。 エンコードには、Media Services の組み込み Media Encoder を使用できます。 Media Services のパートナーから提供されているエンコーダーを使うこともできます。サードパーティのエンコーダーは Azure Marketplace から入手できます。 エンコーディング タスクの詳細を指定するには、エンコーダー用に定義されたプリセット文字列を使用するか、プリセット構成ファイルを使用します。 使用できるプリセットの種類を表示するには、次を参照してください。 [Azure Media Services 用のタスク プリセット](https://msdn.microsoft.com/library/azure/dn619392.aspx)します。 サード パーティのエンコーダーを使用している場合は [ファイルを検証](https://msdn.microsoft.com/library/azure/dn750842.aspx)します。

常に中間ファイルをアダプティブ ビットレート MP4 セットにエンコードしを使用して、目的の形式に変換することを推奨、 [動的パッケージング](media-services-dynamic-packaging-overview.md)します。 動的パッケージを活用するには、コンテンツの配信元となるストリーミング エンドポイントのオンデマンド ストリーミング ユニットを 1 つ以上取得する 必要があります。 詳細については、次を参照してください。 [Media services の規模変更方法](media-services-manage-origins.md#scale_streaming_endpoints)します。

出力資産がストレージで暗号化されている場合は、資産配信のポリシーを構成する必要があります。 詳細については、次を参照してください。 [アセット配信ポリシーを構成する](media-services-dotnet-configure-asset-delivery-policy.md)です。

##1 つのエンコード タスクを持つジョブの作成 

Azure メディア エンコーダーでエンコードをするときは、指定されているタスク構成プリセットを使用することができます [ここ](https://msdn.microsoft.com/library/azure/dn619389.aspx)します。

###Media Services SDK for .NET の使用  

次 **EncodeToAdaptiveBitrateMP4Set** メソッドは、エンコード ジョブを作成し、ジョブにエンコード タスクを 1 つを追加します。 このタスクは、"Azure Media Encoder" を使用して、"H264 アダプティブ ビットレート MP4 セット 720p"へのエンコードを行います。 

    static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
    {
        var encodingPreset = "H264 Adaptive Bitrate MP4 Set 720p";

        IJob job = _context.Jobs.Create(String.Format("Encoding {0} into to {1}",
                                inputAsset.Name,
                                encodingPreset));

        var mediaProcessors = GetLatestMediaProcessorByName("Azure Media Encoder");

        ITask encodeTask = job.Tasks.AddNew("Encoding", mediaProcessors, encodingPreset, TaskOptions.None);
        
        encodeTask.InputAssets.Add(inputAsset);

        // Specify the storage-encrypted output asset.
        encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), 
            AssetCreationOptions.StorageEncrypted);


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

###Media Services SDK for .NET Extensions の使用

    static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
    {
        // 1. Prepare a job with a single task to transcode the specified mezzanine asset
        //    into a multi-bitrate asset.
        IJob job = _context.Jobs.CreateWithSingleTask(
            MediaProcessorNames.AzureMediaEncoder,
            MediaEncoderTaskPresetStrings.H264AdaptiveBitrateMP4Set720p,
            asset,
            "Adaptive Bitrate MP4",
            AssetCreationOptions.None);

        Console.WriteLine("Submitting transcoding job...");

        // 2. Submit the job and wait until it is completed.
        job.Submit();
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;

        Console.WriteLine("Transcoding job finished.");

        IAsset outputAsset = job.OutputMediaAssets[0];

        return outputAsset;
    } 

##チェーン タスクによるジョブの作成 

アプリケーション シナリオの多くで、一連のタスク処理が開発者によって作成されます。 Media Services では、一連のチェーン タスクを作成できます。 各タスクはそれぞれ異なる処理手順を実行し、異なるメディア プロセッサを使うことができます。 チェーン タスクでは、あるタスクから別のタスクに資産を渡しながら一連のタスクを順番に実行できます。 ただし、ジョブで実行されるタスクが必ず順番に実行されなければならないということではありません。 チェーン タスクをチェーンを作成する場合 **ITask** 1 つのオブジェクトが作成される **IJob** オブジェクトです。

>[AZURE.NOTE] 現在はジョブごとに 30 タスクという制限です。 30 以上のタスクをつなげる必要がある場合、別のジョブを作成してタスクを設定します。

次 **CreateChainedTaskEncodingJob** メソッドを次の 2 つのチェーン タスクを含むジョブを作成します。 そのため、このメソッドは、2 つの出力資産を含むジョブを返します。

    
    public static IJob CreateChainedTaskEncodingJob(IAsset asset)
    {
        // Declare a new job.
        IJob job = _context.Jobs.Create("My task-chained encoding job");

        // Set up the first task to encode the input file.

        // Get a media processor reference
        IMediaProcessor processor = GetLatestMediaProcessorByName("Azure Media Encoder");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My encoding task",
            processor,
           "H264 Adaptive Bitrate MP4 Set 720p",
            TaskOptions.ProtectedConfiguration);

        // Specify the input asset to be encoded.
        task.InputAssets.Add(asset);

        // Specify the storage-encrypted output asset.
        task.OutputAssets.AddNew("My storage-encrypted output asset",
            AssetCreationOptions.StorageEncrypted);

        // Set up the second task to decrypt the encoded output file from 
        // the first task.

        // Get another media processor instance
        IMediaProcessor decryptProcessor = GetLatestMediaProcessorByName("Storage Decryption");

        // Declare the decryption task. 
        ITask decryptTask = job.Tasks.AddNew("My decryption task",
            decryptProcessor,
            string.Empty,
            TaskOptions.None);

        // Specify the input asset to be decrypted. This is the output 
        // asset from the first task. 
        decryptTask.InputAssets.Add(task.OutputAssets[0]);

        // Specify an output asset to contain the results of the job. 
        // This should have AssetCreationOptions.None. 
        decryptTask.OutputAssets.AddNew("My decrypted output asset",
            AssetCreationOptions.None);

        // Use the following event handler to check job progress. 
        job.StateChanged += new
            EventHandler<JobStateChangedEventArgs>(JobStateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish. 
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        //return job that contains two output assets.
        return job;
    }


##Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##関連項目 

[Media Services Encoding の概要](media-services-encode-asset.md)

 
