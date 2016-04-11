<properties 
    pageTitle="Microsoft Azure Storage での同時実行制御の管理" 
    description="BLOB、キュー、テーブル、およびファイル サービスの同時実行制御を管理する方法" 
    services="storage" 
    documentationCenter="" 
    authors="jasonnewyork" 
    manager="tadb" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/03/2015" 
    ms.author="jahogg"/>

# Microsoft Azure Storage での同時実行制御の管理

## 概要 

最新のインターネットを基盤とするアプリケーションでは、複数のユーザーが同時にデータを表示し、更新することが一般的です。 このような場合、アプリケーション開発者は予測可能なエクスペリエンスをエンド ユーザーに提供する方法を注意深く検討する必要があり、特に複数のユーザーが同じデータを更新できる場合はこれが重要になります。 開発者は、データの同時実行制御の主な戦略として、次の 3 つの方法を検討する場合が一般的です。  


1.  オプティミスティック同時実行制御 – アプリケーションがデータを更新する場合、更新処理の一部として、データがそのアプリケーションに最後に読み込まれた後に更新されていないか確認します。 たとえば、wiki のページを表示している 2 人のユーザーが同じページを更新しようとしている場合、wiki のプラットフォームは、2 番目の更新が最初の更新を上書きせず、両方のユーザーが自身の更新の成否を把握できるようにする必要があります。 この戦略は、Web アプリケーションで最も広く使用されています。
2.  ペシミスティック同時実行制御 - 更新を実行しようとするアプリケーションがオブジェクトをロックし、ロックが解除されるまで他のユーザーがデータを更新できないようにします。 たとえば、マスターとスレーブの間でデータを複製する場合、マスターのみが更新を実行するため、通常は、データを一定期間、排他的にロックして、該当するデータを他から更新できないようにします。
3.  最終書き込み者を優先 - アプリケーションが最初にデータを読み込んだ後に他のアプリケーションが該当するデータを更新したかどうかを検証せず、すべての更新操作を許可する手法です。 この戦略 (つまり正式な戦略を持たない) は、通常、データが分割されており、複数のユーザーが同時に同じデータにアクセスする可能性がほとんどない場合に使用されます。 また、有効期限が短いデータ ストリームの処理にも有効です。  

この記事では、Azure Storage プラットフォームで上記 3 つの同時実行制御すべてについて最高品質のサポートが提供されていることで、開発がいかに簡素化されるかについて概要を説明します。  

## Azure Storage – クラウド開発を簡素化
Azure ストレージ サービスでは、上記 3 つの戦略をすべてサポートしていますが、特に特徴的なのは、オプティミスティック同時実行制御とペシミスティック同時実行制御のフル サポートを提供していることです。強力な整合性モデルを備えているため、Storage サービスでデータの挿入や更新の処理がコミットされた後にサービスがそのデータにアクセスした場合でも、確実に最新のデータが提供されます。 最終的な整合性モデルを使用するストレージ プラットフォームでは、あるユーザーが書き込んだ後に他のユーザーがそのデータを表示できるようになるまでにはタイム ラグが発生します。このため、エンド ユーザーがこの不整合の影響を受けないようにするために、クライアント アプリケーションの開発は複雑になります。  

また、開発者は、適切な同時実行制御の戦略を選択する以外にも、特に複数のトランザクションで同一のオブジェクトを変更する場合に、ストレージ プラットフォームで変更がどのように分離されるかについて把握しておく必要があります。 Azure Storage サービスではスナップショット分離の手法を使用して、同一パーティション内での読み取り処理と書き込み処理が同時に発生できるようにしています。 他の分離レベルとは異なり、スナップショット分離では、更新の処理中でもすべての読み込み処理に対してデータのスナップショットを提供することにより整合性を確保します。これは基本的に、更新トランザクションの処理中に最後にコミットされた値を返すことで実現しています。  

## BLOB サービスでの同時実行制御の管理
BLOB サービスでの BLOB およびコンテナーへのアクセスを管理する場合、オプティミスティック同時実行制御とペシミスティック同時実行制御のいずれかのモデルを使用できます。 明示的に戦略を指定しない場合は、最終書き込み者優先が既定となります。  

### BLOB およびコンテナーでのオプティミスティック同時実行制御  

Storage サービスでは、格納されているすべてのオブジェクトに識別子が割り当てられます。 オブジェクトで更新処理が実行されるたびに、この識別子が更新されます。 この識別子は、HTTP プロトコルで定義されている ETag (エントリ タグ) ヘッダーを使用して、HTTP GET 応答の一部としてクライアントに返されます。 該当するオブジェクトをユーザーが更新しようとすると、条件ヘッダーが付属している元の ETag が送信され、特定の条件を満たしているときにのみ更新が行われます。この条件には、"If-Match" ヘッダーが使用されます。これによって、更新要求で指定された ETag の値が Storage サービスに格納されている値と同一であることが、Storage サービスによって確認されます。  

このプロセスの概要は次のとおりです。  

1.  Storage サービスから BLOB を取得します。この応答に含まれる HTTP ETag ヘッダーの値が、Storage サービスに格納されているオブジェクトの現在のバージョンを示します。
2.  手順 1. で取得した ETag 値を含む blob を更新するときに、 **If-match** サービスに送信した要求の条件ヘッダー。
3.  サービスによって、要求内の ETag の値と BLOB の現在の ETag の値が比較されます。
4.  Blob の現在の ETag 値が、ETag のバージョンが異なる場合、 **If-match** 条件ヘッダー、要求は、サービスでは、クライアントに 412 エラーを返します。 これは、クライアントがこの BLOB を取得した後に、別のプロセスがこれを更新したことを示しています。
5.  Blob の現在の ETag 値の ETag と同じバージョンである場合、 **If-match** 条件ヘッダー、要求は、サービスは、要求された操作を実行し、新しいバージョンが作成されたことを表示する blob の現在の ETag 値を更新します。  

次の c# スニペット (Client Storage Library 4.2.0 を使用) を作成する方法の簡単な例を示しています、 **If-match AccessCondition** 以前いずれかを取得または挿入された blob のプロパティからアクセスされる ETag 値に基づいています。 使用して、 **AccessCondition** オブジェクトと blob を更新: **AccessCondition** オブジェクトを追加、 **If-match** ヘッダーを要求します。 別のプロセスが BLOB を更新した場合、BLOB のサービスによって HTTP 412 (Precondition Failed) のステータス メッセージが返されます。 完全なサンプルをダウンロードできる [ここ](http://code.msdn.microsoft.com/windowsazure/Managing-Concurrency-using-56018114)します。  

    // Retrieve the ETag from the newly created blob
    // Etag is already populated as UploadText should cause a PUT Blob call 
    // to storage blob service which returns the etag in response.
    string orignalETag = blockBlob.Properties.ETag;
     
    // This code simulates an update by a third party.
    string helloText = "Blob updated by a third party.";
     
    // No etag, provided so orignal blob is overwritten (thus generating a new etag)
    blockBlob.UploadText(helloText);
    Console.WriteLine("Blob updated. Updated ETag = {0}", 
    blockBlob.Properties.ETag);
     
    // Now try to update the blob using the orignal ETag provided when the blob was created
    try
    {
        Console.WriteLine("Trying to update blob using orignal etag to generate if-match access condition");
        blockBlob.UploadText(helloText,accessCondition:
        AccessCondition.GenerateIfMatchCondition(orignalETag));
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        {
            Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
            // TODO: client can decide on how it wants to handle the 3rd party updated content.
        }
        else
            throw;
    }  

ストレージ サービスも含まれています条件ヘッダーのサポートなど **場合-変更-以降**, 、**場合-未変更の状態のため** と **なし-If-match** これらの組み合わせとします。 詳細については、次を参照してください。 [Blob サービス操作の条件ヘッダーの指定](http://msdn.microsoft.com/library/azure/dd179371.aspx) MSDN にします。  

次の表などの条件ヘッダーをそのまま使用するコンテナー操作 **If-match** を要求に応答で ETag 値を返します。  

操作   |コンテナーの ETag 値を返す|  条件ヘッダーを受け取る|
------------|-----------------------|------------------------------------|
コンテナーの作成|   あり|    いいえ|
コンテナーのプロパティの取得|   あり|    いいえ|
コンテナーのメタデータの取得| あり|    いいえ|
コンテナーのメタデータの設定| あり|    あり|
コンテナー ACL の取得|  あり|    いいえ|
コンテナー ACL の設定|  あり|    あり (*)|
Delete Container|   いいえ| あり|
Lease Container|    あり|    あり|
BLOBs の一覧| いいえ| いいえ  

(*) SetContainerACL で定義されたアクセス許可はキャッシュされます。このアクセス許可の更新の伝達には 30 秒間かかり、その間は更新の整合性は保証されません。  

次の表などの条件ヘッダーをそのまま使用する blob 操作 **If-match** を要求に応答で ETag 値を返します。  

操作   |ETag 値を返す |条件ヘッダーを受け取る|
-----------|-------------------|----------------------------|
Put Blob|   あり|    あり|
Get Blob|   あり|    あり|
Get Blob Properties|    あり|    あり|
Set Blob Properties|    あり|    あり|
Get Blob Metadata|  あり|    あり|
Set Blob Metadata|  あり|    あり|
Lease Blob (*)| あり|    あり|
Snapshot Blob|  あり|    あり|
BLOB のコピー|  あり|    Yes (コピー元とコピー先 BLOB に対して)|
Abort Copy Blob|    いいえ| いいえ|
Delete Blob|    いいえ| あり|
Put Block|  いいえ| いいえ|
Put Block List| あり|    あり|
Get Block List| あり|    いいえ|
Put Page|   あり|    あり|
Get Page Ranges|    あり|    あり

(*) Lease BLOB では、BLOB の ETag は変更されません。  

### BLOB でのペシミスティック同時実行制御
ロックするには排他的に使用するための blob を取得できます、 [リース](http://msdn.microsoft.com/library/azure/ee691972.aspx) にします。 リースを取得すると、必要に応じてリース期間を 15 ～ 60 秒または無制限に設定できます。この期間、BLOB が排他的にロックされます。 リース期間が有限の場合は、これを延長することができます。また、完了したリースは解放できます。 期限が切れた有限のリースは、BLOB サービスで自動的に解放されます。  

リースでは、排他的書き込みと共有読み取り、排他的書き込みと排他的読み取り、共有書き込みと排他的読み取りなど、さまざまな同期戦略がサポートされています。 リースが存在する場合、Storage サービスは排他的書き込み (put、set、delete の各操作) を強制的に実行しますが、読み込み操作の排他性を確保するために、開発者はすべてのクライアント アプリケーションがリース ID を使用し、また有効なリース ID は同時に 1 つのクライアントのみが保持するようにする必要があります。 読み込み操作にリース ID を使用しない場合、共有読み取りになります。  

次の C# スニペットの例では、ある BLOB で 30 秒間の排他的リースを取得し、BLOB の内容を更新します。その後でリースを解放します。 BLOB が既に有効なリースを保持している場合、新しいリースを取得しようとすると、BLOB サービスは "HTTP (409) Conflict" 状態を結果として返します。 使用して次のスニペット、 **AccessCondition** ストレージ サービスに blob を更新する要求を行うときは、リースの情報をカプセル化するオブジェクト。 完全なサンプルをダウンロードできる [ここ](http://code.msdn.microsoft.com/windowsazure/Managing-Concurrency-using-56018114)します。  

    // Acquire lease for 15 seconds
    string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
    Console.WriteLine("Blob lease acquired. Lease = {0}", lease);
     
    // Update blob using lease. This operation will succeed
    const string helloText = "Blob updated";
    var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
    blockBlob.UploadText(helloText, accessCondition: accessCondition);
    Console.WriteLine("Blob updated using an exclusive lease");
     
    //Simulate third party update to blob without lease
    try
    {
        // Below operation will fail as no valid lease provided
        Console.WriteLine("Trying to update blob without valid lease");
        blockBlob.UploadText("Update without lease, will fail");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
            Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
        else
            throw;
    }  

リース ID を渡さずにリースを取得した BLOB に対して書き込み操作を要求すると、412 エラーで失敗します。 呼び出しの前に、リースの有効期限が切れた場合、 **UploadText** メソッドは、リース ID を渡すも、によって、要求が失敗した、 **412** エラー。 リースの有効期限とリース id の管理の詳細については、次を参照してください。、 [Lease Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx) REST に関するドキュメントです。  

次の BLOB 操作では、ペシミスティック同時実行制御の管理にリースを使用できます。  


-   Put Blob
-   Get Blob
-   BLOB のプロパティの取得
-   BLOB のプロパティの設定
-   BLOB のメタデータの取得
-   BLOB のメタデータの設定
-   BLOB の削除
-   Put Block
-   Put Block List
-   Get Block List
-   Put Page
-   ページ範囲の取得
-   Snapshot Blob - リースが存在する場合にオプションでリース ID を使用
-   Copy Blob - コピー先の BLOB でリースが存在する場合はリース ID が必要
-   Abort Copy Blob - コピー先の BLOB で無制限のリースが存在する場合はリース ID が必要
-   Lease Blob  

### コンテナーでのペシミスティック同時実行制御
コンテナーのリースでは、BLOB でサポートされるものと同じ同期戦略 (排他的書き込みと共有読み取り、排他的書き込みと排他的読み取り、共有書き込みと排他的読み取り) がサポートされます。ただし、BLOB とは異なり、Storage サービスで排他的に実行できるのは削除操作のみです。 アクティブなリースを使用してコンテナーを削除するには、クライアントが削除要求に有効なリース ID を含める必要があります。 それ以外のコンテナー操作は、リース ID を含めなくてもリースされたコンテナーに対して正常に実行されます。この場合、各操作は共有操作になります。 更新操作 (put または set) または読み込み操作を排他的に行う必要がある場合、開発者はすべてのクライアントがリース ID を使用し、有効なリース ID を持つクライアントが一度に 1 つのみになるようにする必要があります。  

次のコンテナー操作では、ペシミスティック同時実行制御の管理にリースを使用できます。  

-   コンテナーの削除
-   コンテナーのプロパティの取得
-   コンテナーのメタデータの取得
-   コンテナーのメタデータの設定
-   コンテナー ACL の取得
-   コンテナー ACL の設定
-   Lease Container  

詳細情報  

- [BLOB サービス操作の条件ヘッダーの指定](http://msdn.microsoft.com/library/azure/dd179371.aspx)
- [Lease Container](http://msdn.microsoft.com/library/azure/jj159103.aspx)
- [Lease Blob ](http://msdn.microsoft.com/library/azure/ee691972.aspx) 

## Table サービスでの同時実行制御の管理
エンティティを扱っている場合、Table サービスではオプティミスティック同時実行制御の確認が既定の動作として使用されます。一方、BLOB サービスの場合は、オプティミスティック同時実行制御の確認を実行するように明示的に選択する必要があります。 これ以外の相違点としては、Table サービスではエンティティの同時実行制御しか管理できませんが、BLOB サービスではコンテナーと BLOB の両方の同時実行制御を管理できる点があります。  

オプティミスティック同時実行制御を使用して、Table ストレージ サービスからエンティティを取得した後に他のプロセスがそれを更新していないか確認するには、Table サービスがエンティティを返す時に受け取った ETag の値を使用できます。 このプロセスの概要は次のとおりです。  

1.  Table ストレージ サービスからエンティティを取得します。この応答に含まれている ETag の値によって、ストレージ サービスでこのエンティティに関連付けられている現在の識別子を識別できます。
2.  エンティティを更新するときに、必須では、手順 1. で取得した ETag 値を含める **If-match** 、サービスに送信する要求のヘッダー。
3.  サービスによって、要求内の ETag の値とエンティティの現在の ETag の値が比較されます。
4.  エンティティの現在の ETag 値が必須である ETag と異なる場合 **If-match** ヘッダー、要求は、サービスでは、クライアントに 412 エラーを返します。 これは、クライアントがこのエンティティを取得した後に、別のプロセスがこれを更新したことを示しています。
5.  エンティティの現在の ETag 値が必須の ETag と同じである場合 **If-match** 、要求のヘッダーまたは **If-match** ヘッダーにワイルドカード文字 (*) が含まれています、サービスは、要求された操作を実行し、更新されたことを表示するエンティティの現在の ETag 値を更新します。  

Blob サービスとは異なり、table サービスが必要である、クライアントが含める注、 **If-match** 更新要求にヘッダー。 ただし、これは、無条件に (最終書き込み者優先戦略) を更新し、クライアントを設定する場合は、同時実行制御チェックを省略、 **If-match** をワイルドカード文字 (*)、要求のヘッダー。  

次の C# スニペットは、前に電子メール アドレスが更新されたときに作成または取得された顧客エンティティを示しています。 最初の挿入操作または取得操作で、顧客のオブジェクトに ETag の値が格納されます。このサンプルでは置換操作を実行するときに同じオブジェクトのインスタンスを使用するため、ETag の値が自動的に Table サービスに返され、サービスが同時実行制御の違反を確認できるようになっています。 別のプロセスが Table ストレージ内のエンティティを更新した場合、サービスから HTTP 412 (Precondition Failed) のステータス メッセージが返されます。 完全なサンプルをダウンロードできる [ここ](http://code.msdn.microsoft.com/windowsazure/Managing-Concurrency-using-56018114)します。  

    try
    {
        customer.Email = "updatedEmail@contoso.org";
        TableOperation replaceCustomer = TableOperation.Replace(customer);
        customerTable.Execute(replaceCustomer);
        Console.WriteLine("Replace operation succeeded.");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == 412)
            Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
        else
            throw; 
    }  

同時実行制御チェックを明示的に無効に設定する必要があります、 **ETag** のプロパティ、 **従業員** オブジェクトを"*"置換操作を実行する前にします。  

customer.ETag = "*";  

次の表は、Table エンティティで ETag の値がどのように使用されるかをまとめたものです。  

操作   |ETag 値を返す |If-Match 要求ヘッダーが必須|
------------|-------------------|--------------------------------|
エンティティのクエリ| あり|    いいえ|
エンティティの挿入|  あり|    いいえ|
エンティティの更新|  あり|    あり|
エンティティの統合|   あり|    あり|
エンティティの削除|  いいえ| あり|
エンティティの挿入または置換|   あり|    いいえ|
エンティティの挿入または統合| あり|    いいえ 

注意してください、 **Insert or Replace Entity** と **Insert or Merge Entity** 操作 *いない* 、ETag の値は table サービスに送信されないため、同時実行制御チェックを実行します。  

一般に、テーブルを利用するスケーラブルなアプリケーションを開発するときは、オプティミスティック同時実行制御を採用する必要があります。 ペシミスティック同時実行制御のロックが必要な場合、テーブルにアクセスする際には、テーブルに対する操作が行われる前に各テーブルに指定した BLOB を関連付け、BLOB のリースの取得を試行するという手法を利用できます。 この手法では、テーブルに対する操作が行われる前にすべてのデータのアクセス パスでリースが確実に取得されるようにアプリケーションを設計する必要があります。 また、リース期間は最低 15 秒であるため、スケーラビリティについては慎重に考慮する必要があります。  

詳細情報  

- [エンティティに対する操作](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## キュー サービスでの同時実行制御の管理
キュー サービスでの同時実行制御について注意が必要なケースとして、複数のクライアントが 1 つのキューからメッセージを取得する場合があります。 キューからメッセージを取得するときの応答には、メッセージと、メッセージを削除するときに必要な pop receipt 値が含まれます。 メッセージはキューから自動的に削除されることはありませんが、取得された後、visibilitytimeout パラメーターで指定された期間は他のクライアントに表示されなくなります。 メッセージを取得するクライアントは、メッセージの処理後、応答の TimeNextVisible 要素で指定された時刻までにメッセージを削除します。この時刻は visibilitytimeout パラメーターの値に基づいて算出されます。 visibilitytimeout の値をメッセージの取得時刻に加算することで、TimeNextVisible の値が決定されます。  

キュー サービスでは、オプティミスティックおよびペシミスティックのいずれの同時実行制御もサポートしていません。このため、キューからメッセージを取得して処理するクライアントは、元の値が変わらないようにメッセージを処理する必要があります。 SetQueueServiceProperties、SetQueueMetaData、SetQueueACL、UpdateMessage などの更新操作では、最終書き込み者優先戦略が適用されます。  

詳細情報  

- [キュー サービスの REST API](http://msdn.microsoft.com/library/azure/dd179363.aspx)
- [Get Messages](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## ファイル サービスでの同時実行制御の管理
ファイル サービスでは、SMB と REST の 2 種類のプロトコルを使用してエンドポイントにアクセスできます。 REST サービスではオプティミスティック同時実行制御とペシミスティック同時実行制御のいずれもサポートされておらず、すべての更新操作は最終書き込み者優先戦略に従います。 ファイル共有をマウントする SMB クライアントではファイル システムのロック機構を使用して、共有ファイルへのアクセスを管理できます。ペシミスティック同時実行制御も実行できます。 SMB クライアントがファイルを開くときに、ファイルのアクセス権と共有モードの両方が指定されます。 ファイルのアクセス権のオプションが "Write" または "Read/Write" に設定され、同時にファイル共有モードが "None" に設定された場合、そのファイルは閉じられるまで SMB クライアントによってロックします。 SMB クライアントがロックしているファイルに REST 操作を実行しようとすると、REST サービスはステータス コード 409 (Conflict) およびエラー コード SharingViolation を返します。  

SMB クライアントがファイルを開いて削除する場合、そのファイルで開かれているハンドルが他のすべての SMB クライアントで閉じられるまで、ファイルは保留中の削除とマークされます。 保留中の削除であるとマークされたファイルで REST 操作を実行しようとすると、ステータス コード 409 (Conflict) とエラー コード SMBDeletePending が返されます。 SMB クライアントはファイルを閉じる前に保留中の削除を示すフラグを削除できるため、ステータス コード 404 (Not Found) は返されません。 つまり、ステータス コード 404 (Not Found) は、ファイルが削除済みである場合のみ返されることが想定されます。 SMB で保留中の削除状態にあるファイルは、List Files の結果に含まれないことに注意してください。また、REST Delete File および REST Delete Directory の各操作はアトミックにコミットされるため、保留中の削除状態になることはありません。  

詳細情報  

- [Managing File Locks (ファイルのロックの管理)](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## 概要と次のステップ
Microsoft Azure Storage サービスは、非常に複雑なオンライン アプリケーションのニーズに対応するように設計されていますが、開発者は、同時実行制御やデータの整合性など、当然のように想定されるようになった主要設計項目を再検討したり、妥協したりする必要がありません。  

このブログで参照したサンプル アプリケーションの完全版は、次のページからダウンロードできます。  

- [Managing Concurrency using Azure Storage - Sample Application (Azure Storage での同時実行制御の管理 - サンプル アプリケーション)](http://code.msdn.microsoft.com/windowsazure/Managing-Concurrency-using-56018114)  

Azure Storage の詳細については、以下を参照してください。  

- [Microsoft Azure Storage のホーム ページ](http://azure.microsoft.com/services/storage/)
- [Microsoft Azure Storage の概要](storage-introduction.md)
- 記憶域の概要 [Blob](storage-dotnet-how-to-use-blobs.md), 、[テーブル](storage-dotnet-how-to-use-tables.md) と [キュー](storage-dotnet-how-to-use-queues.md)
- Storage Architecture – [Microsoft Azure Storage: 強力な一貫性と高可用クラウド ストレージ サービス](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

 

