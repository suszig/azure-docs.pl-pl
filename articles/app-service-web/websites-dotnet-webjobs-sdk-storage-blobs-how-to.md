<properties 
    pageTitle="Web ジョブ SDK で Azure Blob Storage を使用する方法" 
    description="Web ジョブ SDK で Azure Blob Storage を使用する方法を説明します。 コンテナーに新しい BLOB が表示されたら、プロセスをトリガーし、 有害な BLOB を処理します。" 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/22/2015" 
    ms.author="tdykstra"/>

# Web ジョブ SDK で Azure Blob Storage を使用する方法

## 概要

このガイドでは、Azure BLOB が作成または更新されたときにプロセスをトリガーする方法を示す C# コード サンプルについて説明します。 コード サンプルを使用して [web ジョブ SDK](websites-dotnet-webjobs-sdk.md) バージョン 1.x します。

Blob を作成する方法を示すコード サンプルでは、次を参照してください。 [web ジョブ sdk Azure キュー ストレージを使用する方法](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)します。 
        
このガイドがわかっていると想定しています。 [の Visual Studio で web ジョブ プロジェクトを作成する方法は、ストレージ アカウントにそのポイントを文字列](websites-dotnet-webjobs-sdk-get-started.md)します。

## <a id="trigger"></a> BLOB が作成または更新されたときに、関数を開始する方法

このセクションでは、`BlobTrigger` 属性を使用する方法を示しています。 

> **注:** web ジョブ SDK は新しいまたは変更された blob を監視するログ ファイルをスキャンします。 このプロセスは本質的に時間がかかります。関数は、blob が作成されてから数分またはそれ以上経過しないとトリガーされない可能性があります 。  推奨される方法は、blob を作成して使用するときに、キュー メッセージを作成する、アプリケーションは、blob をすぐに処理する必要がある場合、 [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) 属性の代わりに、 `BlobTrigger` 、blob を処理する関数での属性です。

### 拡張子を持つ BLOB 名の 1 つのプレース ホルダー  

次のコード サンプルに表示されるテキスト blob のコピー、 *入力* するためのコンテナー、 *出力* コンテナー。

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

属性コンストラクターが、コンテナー名と BLOB 名のプレース ホルダーを指定する文字列パラメーターを取得します。 という名前の blob の場合、この例では *Blob1.txt* で作成された、 *入力* 関数がという名前の blob を作成するコンテナー、 *Blob1.txt* で、 *出力* コンテナーです。 

次のコード サンプルに示すように、BLOB 名のプレース ホルダーを持つ名前のパターンを指定できます。

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

このコードは「元-」で始まる名前の BLOB のみをコピーします。 たとえば、 *入力* で、 *入力* にコンテナーをコピー *copy-blob1.txt* で、 *出力* コンテナーです。

名前に波括弧がある BLOB 名に新しいパターンを指定する必要がある場合は、波括弧を二重にします。 たとえば、内の blob を検索する場合、 *イメージ* 次のように名前を持つコンテナー。

        {20140101}-soundfile.mp3

これをパターンとして使用します。

        images/{{20140101}}-{name}

この例で、 *名前* プレース ホルダーの値になります *soundfile.mp3*します。 

### 別の BLOB 名と拡張子のプレース ホルダー

次のコード サンプルは、表示される blob がコピーされる際にファイル拡張子を変更、 *入力* するためのコンテナー、 *出力* コンテナーです。 コードの拡張子をログに記録、 *入力* blob し、の拡張機能の設定、 *出力* blob に *.txt*します。

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a id="types"></a> BLOB にバインドすることができる種類

次の種類の `BlobTrigger` 属性を使用できます。

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* その他の種類によって逆シリアル化 [ICloudBlobStreamBinder](#icbsb) 

Azure のストレージ アカウントを直接操作する場合は、メソッド シグネチャに `CloudStorageAccount` パラメーターを追加することもできます。

## <a id="string"></a> 文字列にバインドすることによってテキスト BLOB のコンテンツを取得する

テキスト BLOB がある場合は、  `BlobTrigger`を `string` パラメーターに適用できます。 次のコード サンプルでは、テキスト BLOB を `logMessage` という名前の `string` パラメーターにバインドします。 関数は、そのパラメーターを使用して Web ジョブ SDK のダッシュボードに、BLOB の内容を記述します。 
 
        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a> ICloudBlobStreamBinder を使用してシリアル化した BLOB の内容を取得する

次のコード サンプルでは、`ICloudBlobStreamBinder` を実装するクラスを使用して `BlobTrigger` 属性を有効化して BLOB を `WebImage` 型にバインドします。

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

`WebImage` バインド コードは、`ICloudBlobStreamBinder` から派生した `WebImageBinder` クラスに提供されます。

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a id="poison"></a> 有害な BLOB の処理方法

`BlobTrigger` 関数が失敗した場合、失敗が一時的なエラーによって発生した場合は、SDK は再度関数を呼び出します。 失敗が BLOB のコンテンツによって発生した場合は、BLOB の処理を試みるたびに関数は失敗します。 既定では、SDK は特定の BLOB に対して最大 5 回、関数を呼び出します。 SDK がという名前のキューにメッセージを追加する場合は 5 回目が失敗して、 *web ジョブ-blobtrigger-有害*します。

再試行回数の最大値の設定は変更可能です。 同じ [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) 有害な blob の処理と有害キュー メッセージの処理の設定を使用します。 

有害な BLOB のキュー メッセージは次のプロパティを持つ JSON オブジェクトです。

* FunctionId (形式の *WebJob name*します。関数*。Function name*, 、たとえば: WebJob1.Functions.CopyBlob)
* BLOB の種類 ("BlockBlob" か "PageBlob")
* コンテナー名
* BlobName
* ETag (BLOB のバージョン識別子。たとえば、"0x8D1DC6E70A277EF")

次のコード サンプルでは、`CopyBlob` 関数は、呼び出されるたびに障害が発生するコードを含んでいます。 SDK は呼び出しを最大試行回数実行すると、メッセージが有害 BLOB キューに作成され、そのメッセージは、`LogPoisonBlob` 関数によって処理されます。 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }
        
        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

SDK は、自動的にJSON メッセージを逆シリアル化します。 ここでは`PoisonBlobMessage` クラスです。 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a> BLOB のポーリング アルゴリズム

Web ジョブ SDK は、アプリケーションの起動時に `BlobTrigger` 属性が指定するすべてのコンテナーをスキャンします。 大容量のストレージ アカウントでは、このスキャンに時間がかかるため、新しい BLOB が見つかり、`BlobTrigger` 関数が実行されるまでにしばらく時間がかかります。

アプリケーションの起動後に新しいまたは変更された BLOB を検出するために、SDK は定期的にBLOB ストレージ ログを読み取ります。 BLOB のログはバッファーされ、10 分程度ごとに物理的に記述されるので、BLOB が作成されるか更新され、対応する `BlobTrigger` 関数が実行されるにはかなり時間がかかる可能性があります。 

`Blob` 属性を使用して作成する BLOB には例外があります。 Web ジョブ SDK は新しい BLOB を作成すると、一致するすべての`BlobTrigger` 関数に新しい BLOB をすぐに渡します。 そのため、一連の BLOB 入力と BLOB 出力がある場合は、SDK は効率的に処理できます。 ただし、その他の方法で作成または更新された BLOB に対して 低遅延で実行される BLOB 処理関数を使用する場合は、`BlobTrigger` ではなく `QueueTrigger` をお勧めします。

### <a id="receipts"></a> BLOB の配信確認メッセージ

Web ジョブ SDK では、`BlobTrigger` 関数は同一の新しいまたは更新された BLOB を複数回呼び出しません。 これは維持することにより、 *の配信確認メッセージを blob* 、特定の blob バージョンが処理されているかどうかを判断するためです。

という名前のコンテナーに blob の配信確認メッセージが格納されている *azure web ジョブ-ホスト* 、AzureWebJobsStorage 接続文字列で指定された Azure ストレージ アカウントにします。 Blob の受信確認では、次の情報があります。

* Blob に対して呼び出された関数 ("*WebJob name*します。関数*。Function name*"例:"WebJob1.Functions.CopyBlob")
* コンテナーの名前
* BLOB の種類 ("BlockBlob" か "PageBlob")
* BLOB の名前
* ETag (BLOB のバージョン識別子。たとえば、"0x8D1DC6E70A277EF")

強制的に blob の再処理する場合、配信確認メッセージからその blob を手動で削除することができます、 *azure web ジョブ-ホスト* コンテナーです。

## <a id="queues"></a>キューの記事で扱う関連トピック

Blob に固有ではないシナリオの「処理、キュー メッセージによってトリガーされる blob 処理する方法については、web ジョブ SDK [web ジョブ sdk Azure キュー ストレージを使用する方法](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)します。 

その記事では、以下のような関連トピックが紹介されています。

* Async 関数
* 複数のインスタンス
* グレースフル シャットダウン
* 関数本体での Web ジョブ SDK 属性の使用
* コード内で SDK の接続文字列を設定する。
* コードの Web ジョブ SDK コンストラクター パラメーター値の設定
* 有害 BLOB を処理するために `MaxDequeueCount` を構成します。
* 手動での関数のトリガー
* ログの書き込み

## <a id="nextsteps"></a> 次のステップ

このガイドでは、Azure BLOB を操作するための一般的なシナリオの処理方法を示すコードのサンプルを提供しました。 Azure web ジョブおよび web ジョブ SDK を使用する方法の詳細については、次を参照してください。 [Azure WebJobs の推奨リソース](http://go.microsoft.com/fwlink/?linkid=390226)します。
 

