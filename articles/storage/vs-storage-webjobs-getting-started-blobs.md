<properties
    pageTitle="BLOB ストレージと Visual Studio 接続済みサービスの概要 (Web ジョブ プロジェクト) | Microsoft Azure"
    description="Visual Studio 接続済みサービスを使用して Azure Storage に接続した後、Web ジョブ プロジェクトで BLOB ストレージの使用を開始する方法について説明します。"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/16/2015"
    ms.author="tarcher"/>

# Azure BLOB ストレージと Visual Studio 接続済みサービスの概要 (Web ジョブ プロジェクト)

## 概要

この記事では、Azure BLOB が作成または更新されたときにプロセスをトリガーする方法を示す C# コード サンプルについて説明します。 コード サンプルを使用して、 [web ジョブ SDK](websites-dotnet-webjobs-sdk.md) バージョン 1.x します。 追加すると、ストレージ アカウントを web ジョブ プロジェクトを Visual Studio を使用して **[接続済みサービス** ] ダイアログ ボックスで、適切な Azure Storage の NuGet パッケージがインストールされている、適切な .NET 参照がプロジェクトに追加およびストレージ アカウントの接続文字列が App.config ファイルで更新されます。



## BLOB が作成または更新されたときに、関数を開始する方法

このセクションでは使用する方法、 **BlobTrigger** 属性です。

 **注:** web ジョブ SDK は新しいまたは変更された blob を監視するログ ファイルをスキャンします。 このプロセスは本質的に時間がかかります。関数は、blob が作成されてから数分またはそれ以上経過しないとトリガーされない可能性があります 。  推奨される方法は、blob を作成して使用するときに、キュー メッセージを作成する、アプリケーションは、blob をすぐに処理する必要がある場合、 [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) 属性の代わりに、 **BlobTrigger** 、blob を処理する関数での属性です。

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

## BLOB にバインドすることができる種類

使用することができます、 **BlobTrigger** 属性は、次の種類。

* **文字列**
* **TextReader**
* **Stream**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* その他の種類によって逆シリアル化 [ICloudBlobStreamBinder](#icbsb)

Azure ストレージ アカウントを直接操作するかどうか、追加することも、 **CloudStorageAccount** メソッド シグネチャのパラメーターです。

## 文字列にバインドすることによってテキスト BLOB のコンテンツを取得する

テキスト blob が予想される場合 **BlobTrigger** に適用できる、 **文字列** パラメーター。 次のコード サンプルに、テキスト blob にバインドする **文字列** という名前のパラメーター **logMessage**します。 関数は、そのパラメーターを使用して Web ジョブ SDK のダッシュボードに、BLOB の内容を記述します。

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## ICloudBlobStreamBinder を使用してシリアル化した BLOB の内容を取得する

次のコード サンプルを実装するクラスを使用して **ICloudBlobStreamBinder** を有効にする、 **BlobTrigger** を blob にバインドする属性、 **WebImage** 型です。

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

 **WebImage** に追加されたバインド コード、 **WebImageBinder** から派生したクラス **ICloudBlobStreamBinder**します。

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

## 有害な BLOB の処理方法

ときに、 **BlobTrigger** 関数に失敗すると、SDK は呼び出し、失敗が一時的なエラーが発生した場合にします。 失敗が BLOB のコンテンツによって発生した場合は、BLOB の処理を試みるたびに関数は失敗します。 既定では、SDK は特定の BLOB に対して最大 5 回、関数を呼び出します。 SDK がという名前のキューにメッセージを追加する場合は 5 回目が失敗して、 *web ジョブ-blobtrigger-有害*します。

再試行回数の最大値の設定は変更可能です。 同じ [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) 有害な blob の処理と有害キュー メッセージの処理の設定を使用します。

有害な BLOB のキュー メッセージは次のプロパティを持つ JSON オブジェクトです。

* FunctionId (形式の *WebJob name*します。関数*。Function name*, 、たとえば: WebJob1.Functions.CopyBlob)
* BLOB の種類 ("BlockBlob" か "PageBlob")
* コンテナー名
* BlobName
* ETag (BLOB のバージョン識別子。たとえば、"0x8D1DC6E70A277EF")

次のコード サンプルでは、 **CopyBlob** 関数が呼び出されるたびに障害が発生するコードです。 SDK は呼び出しを最大回数と、メッセージが有害 blob キューに作成されでそのメッセージを処理、 **LogPoisonBlob** 関数です。

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

SDK は、自動的にJSON メッセージを逆シリアル化します。 ここでは、 **PoisonBlobMessage** クラス。

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### BLOB のポーリング アルゴリズム

Web ジョブ SDK で指定されたすべてのコンテナーをスキャンする **BlobTrigger** アプリケーションの起動時に属性です。 大容量のストレージ アカウントでこのスキャン時間がかかり、新しい blob が見つかりまで、しばらくありますので、 **BlobTrigger** 関数を実行します。

アプリケーションの起動後に新しいまたは変更された BLOB を検出するために、SDK は定期的にBLOB ストレージ ログを読み取ります。 Blob のログはバッファーされ、10 分ごとに物理的に程度記述またはそのため、そのため可能性がありますはかなり時間、blob が作成されるか更新され、対応する **BlobTrigger** 関数を実行します。

使用して作成する blob の例外がある、 **Blob** 属性です。 Web ジョブ SDK では、新しい blob を作成、新しい blob をすぐに渡します、一致するすべてに **BlobTrigger** 関数です。 そのため、一連の BLOB 入力と BLOB 出力がある場合は、SDK は効率的に処理できます。 使用して低待機時間が実行される blob 処理関数を作成またはその他の方法で更新された blob の場合は、お勧め **QueueTrigger** なく **BlobTrigger**します。

### BLOB の配信確認メッセージ

Web ジョブ SDK を使用することを確認するない **BlobTrigger** 関数は、同一の新しいまたは更新された blob 複数回呼び出しません。 これは維持することにより、 *の配信確認メッセージを blob* 、特定の blob バージョンが処理されているかどうかを判断するためです。

という名前のコンテナーに blob の配信確認メッセージが格納されている *azure web ジョブ-ホスト* 、AzureWebJobsStorage 接続文字列で指定された Azure ストレージ アカウントにします。 Blob の受信確認では、次の情報があります。

* Blob に対して呼び出された関数 ("*WebJob name*します。関数*。Function name*"例:"WebJob1.Functions.CopyBlob")
* コンテナーの名前
* BLOB の種類 ("BlockBlob" か "PageBlob")
* BLOB の名前
* ETag (BLOB のバージョン識別子。たとえば、"0x8D1DC6E70A277EF")

強制的に blob の再処理する場合、配信確認メッセージからその blob を手動で削除することができます、 *azure web ジョブ-ホスト* コンテナーです。

## キューの記事で扱う関連トピック

Blob に固有ではないシナリオの「処理、キュー メッセージによってトリガーされる blob 処理する方法については、web ジョブ SDK [web ジョブ sdk Azure キュー ストレージを使用する方法](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)します。

その記事では、以下のような関連トピックが紹介されています。

* Async 関数
* 複数のインスタンス
* グレースフル シャットダウン
* 関数本体での Web ジョブ SDK 属性の使用
* コード内で SDK の接続文字列を設定する。
* コードの Web ジョブ SDK コンストラクター パラメーター値の設定
* 構成 **MaxDequeueCount** の有害 blob を処理します。
* 手動での関数のトリガー
* ログの書き込み

## 次のステップ

この記事では、Azure BLOB を操作するための一般的なシナリオの処理方法を示すコードのサンプルを提供しました。 Azure web ジョブおよび web ジョブ SDK を使用する方法の詳細については、次を参照してください。 [Azure WebJobs の推奨リソース](http://go.microsoft.com/fwlink/?linkid=390226)します。

