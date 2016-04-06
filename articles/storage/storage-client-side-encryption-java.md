<properties 
    pageTitle="Java による Microsoft Azure Storage のクライアント側の暗号化 | Microsoft Azure" 
    description="Java 用 Azure Storage クライアント ライブラリはクライアント側の暗号化と Azure Key Vault との統合を支援して、Azure Storage アプリケーションのセキュリティを最大限に高めます。" 
    services="storage" 
    documentationCenter="java" 
    authors="dineshm" 
    manager="carolz" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/07/2015" 
    ms.author="tamram"/>


# Java による Microsoft Azure Storage のクライアント側の暗号化   

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## 概要  
 [Java 用 Azure ストレージ クライアント ライブラリ](https://www.nuget.org/packages/WindowsAzure.Storage) では、Azure ストレージにアップロードして、クライアントにダウンロード中にデータを復号化する前にクライアント アプリケーション内のデータを暗号化します。 ライブラリとの統合もサポートしています。 [Azure Key Vault](http://azure.microsoft.com/services/key-vault/) をストレージ アカウント キーの管理。

## エンベロープ手法による暗号化と復号化    
暗号化と復号化のプロセスは、エンベロープ手法に倣います。  

### エンベロープ手法による暗号化  
エンベロープ手法による暗号化は、次の方法で機能します。  

1.  Azure ストレージ クライアント ライブラリは、1 回使用の対称キーであるコンテンツ暗号化キー (CEK) を生成します。  

2.  ユーザー データは、この CEK を使用して暗号化されます。   

3.  CEK は、キー暗号化キー (KEK) を使用してラップ (暗号化) されます。 KEK は、キー識別子によって識別され、非対称キー ペアまたは対称キーのどちらでもよく、ローカルに管理することも、Azure Key Vault に保存することもできます。  
ストレージ クライアント ライブラリ自体が KEK にアクセスすることはありません。 ライブラリは、Key Vault によって提供されるキー ラップ アルゴリズムを呼び出すだけです。 ユーザーは、必要な場合は、キー ラップ/ラップ解除にカスタム プロバイダーを使用できます。  

4.  暗号化されたデータは、Azure Storage サービスにアップロードされます。 ラップされたキーは追加の暗号化メタデータと共にメタデータとして (BLOB に) 格納されるか、暗号化されたデータ (キュー メッセージやテーブル エンティティ) によって補間されます。

### エンベロープ手法による復号化  
エンベロープ手法による復号化は、次の方法で機能します。  

1.  クライアント ライブラリでは、ユーザーがキー暗号化キー (KEK) をローカルまたは Azure Key Vault のいずれかで管理することを前提としています。 ユーザーは、暗号化に使用された特定のキーを把握しておく必要はありません。 代わりに、さまざまなキー識別子をキーに解決する Key Resolver を設定、使用できます。  

2.  クライアント ライブラリは、暗号化されたデータおよびサービスに格納されている暗号化に関する情報 (ある場合) をダウンロードします。  

3.  次に、ラップされたコンテンツ暗号化キー (CEK) が、キー暗号化キー (KEK) によりラップ解除 (復号化) されます。 ここでも、クライアント ライブラリが KEK にアクセスすることはありません。 これは、単にカスタムの、または Key Vault プロバイダーのラップ解除アルゴリズムを起動するだけです。  

4.  次に、コンテンツ暗号化キー (CEK) を使用して、暗号化されたユーザー データを復号化します。

## 暗号化メカニズム  
ストレージ クライアント ライブラリを使用して [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) ユーザー データを暗号化します。 具体的には、 [暗号ブロック チェーン (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) AES を使用してモードです。 サービスごとに動作が多少異なるため、以下でそれぞれについて説明します。

### BLOB  
現在、クライアント ライブラリでは BLOB 全体の暗号化のみがサポートされています。 ユーザーを使用して場合に、暗号化を具体的には、サポート、 **アップロード*** メソッドまたは **openOutputStream** メソッドです。 ダウンロードについては、完全ダウンロードと一部の範囲のダウンロードの両方がサポートされています。  

暗号化中、クライアント ライブラリは 16 バイトのランダムな初期化ベクトル (IV) と 32 バイトのランダムなコンテンツ暗号化キー (CEK) を生成し、この情報を使用して BLOB データのエンベロープ暗号化を実行します。 ラップされた CEK と一部の追加暗号化メタデータが、サービスの暗号化された BLOB と共に、BLOB メタデータとして格納されます。 

>**警告:**  
>BLOB のメタデータを編集またはアップロードする場合は、このメタデータを保持している必要があります。 このメタデータなしで新しいメタデータをアップロードした場合、ラップされた CEK、IV、およびその他のメタデータは失われ、BLOB コンテンツが再度取得可能になることはありません。

使用して、blob 全体のコンテンツの取得は、暗号化された blob をダウンロード、 **ダウンロード*/openInputStream** 便利なメソッドです。 ラップされた CEK はラップ解除され、復号化されたデータをユーザーに返すために IV (この場合 BLOB メタデータとして格納された) と共に使用されます。

任意の範囲のダウンロード (**downloadRange*** メソッド) で暗号化された blob は、少量の要求された範囲を正常に復号化に使用できるその他のデータを取得するために、ユーザーが指定した範囲を調整します。  

このスキームを使用して、すべての BLOB 型 (ブロック BLOB、ページ BLOB、および追加 BLOB) を暗号化/復号化できます。

### キュー  
キュー メッセージの書式は一定でないため、クライアント ライブラリでは、メッセージ テキストで初期化ベクトル (IV) と暗号化されたコンテンツ暗号化キー (CEK) を含むカスタム書式が定義されます。  

暗号化中、クライアント ライブラリは 16 バイトのランダムな IV と 32 バイトのランダムな CEK を生成し、この情報を使用してキュー メッセージ テキストのエンベロープ暗号化を実行します。 次に、ラップされた CEK と追加の暗号化メタデータのいくつかが、暗号化されたキュー メッセージに追加されます。 この変更されたメッセージ (下記参照) は、サービスに格納されます。

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

復号化中、ラップされたキーがキュー メッセージから抽出され、ラップ解除されます。 また、IV もキュー メッセージから抽出され、これをラップ解除されたキーと共に使用して、キュー メッセージ データが復号化されます。 暗号化メタデータは小さいため (500 バイト未満)、キュー メッセージの 64 KB という上限を考慮したとき、影響が小さくて済みます。

### テーブル  
クライアント ライブラリでは、挿入および置換操作のエンティティ プロパティの暗号化がサポートされます。 

>**注:**  
>現在、マージはサポートされていません。 別のキーを使用してプロパティのサブセットが以前に暗号化されている場合、新しいプロパティをマージしたり、メタデータを更新したりするとデータ損失が発生します。 マージでは、追加のサービス呼び出しをして既存のエンティティをサービスから読み込むか、プロパティごとに新しいキーを使用する必要があります。いずれの方法も、パフォーマンス上の理由でお勧めできません。

テーブル データの暗号化は、次のように機能します。  

1.  暗号化するプロパティをユーザーが指定します。  

2.  クライアント ライブラリは 16 バイトのランダムな初期化ベクトル (IV) と 32 バイトのランダムなコンテンツ暗号化キー (CEK) を各エンティティごとに生成し、プロパティごとに新しい IV を派生させることで暗号化する個々のプロパティでエンベロープ暗号化を実行します。 暗号化されたプロパティは、バイナリ データとして格納されます。  

3.  次に、ラップされた CEK と追加の暗号化メタデータが、2 つの追加の予約済みプロパティとして格納されます。 最初の予約済みプロパティ (_ClientEncryptionMetadata1) は文字列プロパティで、IV、バージョン、およびラップされたキーに関する情報を保持します。 2 番目の予約済みプロパティ (_ClientEncryptionMetadata2) はバイナリ プロパティで、暗号化されたプロパティに関する情報を保持します。 この 2 番目のプロパティ (_ClientEncryptionMetadata2) 内の情報自体が暗号化されます。  

4.  これらの暗号化に必要な追加の予約済みプロパティにより、ユーザーが所有できるカスタム プロパティは 252 ではなく、250 個になります。 エンティティの合計サイズは、1 MB 未満である必要があります。  

    暗号化できるのは、文字列プロパティのみであることに注意してください。 他の種類のプロパティを暗号化する必要がある場合は、文字列に変換する必要があります。 暗号化された文字列はバイナリ プロパティとしてサービスで保存され、復号化された後、文字列に変換されて戻されます。

    テーブルの場合、暗号化ポリシーに加え、ユーザーは暗号化するプロパティを指定する必要があります。 これを実行するには、[Encrypt] 属性を指定するか (TableEntity から派生した POCO エンティティ用)、または要求オプションで暗号化リゾルバーを指定します。 暗号化リゾルバーは、パーティション キー、行キー、プロパティ名を取得するデリゲートで、プロパティを暗号化するかどうかを示すブール値を返します。 暗号化時、クライアント ライブラリはこの情報を使用して、ネットワークへの書き込み時にプロパティを暗号化するかどうかを決定します。 また、デリゲートは、プロパティの暗号化方法に関するロジックを使用する可能性にも備えます。 (X の場合、プロパティ A を暗号化し、それ以外の場合はプロパティ A および B を暗号化するなど。)エンティティの読み込み中、またはクエリの実行中は、この情報を指定する必要はありません。

### バッチ操作  
バッチ操作では、そのバッチ操作のすべての行で同じ KEK が使用されます。これは、クライアント ライブラリで、各バッチ操作あたり 1 つの options オブジェクト (従って、1 ポリシー/KEK) のみが許可されるためです。 ただし、クライアント ライブラリは、バッチ内の行ごとに 1 つの新しいランダム IV とランダム CEK を内部的に生成します。 ユーザーは、暗号化リゾルバーでこの動作を定義することで、バッチの各操作で個別のプロパティを暗号化することも選択できます。

### クエリ  
クエリ操作を実行するには、結果セット内のすべてのキーを解決できる Key Resolver を指定する必要があります。 クエリの結果に含まれたエンティティをプロバイダーに解決できない場合、クライアント ライブラリでエラーがスローされます。 クエリでサーバー側のプロジェクションが実行される場合、クライアント ライブラリは既定で、特別な暗号化メタデータ プロパティ (_ClientEncryptionMetadata1 および _ClientEncryptionMetadata2) を選択した列に追加します。

## Azure Key Vault  
Azure Key Vault は、クラウド アプリケーションやサービスで使用される暗号化キーとシークレットをセキュリティで保護するために役立ちます。 Azure Key Vault を使用すると、キーとシークレット (認証キー、ストレージ アカウント キー、データ暗号化キー、PFX ファイル、パスワードなど) をハードウェア セキュリティ モジュール (HSM) で保護されたキーを使用して暗号化できます。 詳細については、次を参照してください。 [Azure Key Vault は何ですか?](../articles/key-vault-whatis.md)します。

ストレージ クライアント ライブラリは Key Vault のコア ライブラリを使用して、Azure 全体でのキー管理用の一般的なフレームワークを提供します。 Key Vault 拡張機能ライブラリを使用すると追加のメリットも得られます。 拡張機能ライブラリには、シンプルかつシームレスな対称/RSA ローカルおよびクラウドのキー プロバイダーに関する便利な機能や、集計またはキャッシュに関する機能が用意されています。

### インターフェイスと依存関係  
次の 3 種類の Key Vault パッケージがあります。  
- azure-keyvault-core には、IKey と IKeyResolver が含まれています。 これは、依存関係のない小型パッケージです。 Java 用ストレージ クライアント ライブラリでは、依存関係としてそれを定義します。  

- azure-keyVault には Key Vault REST クライアントが含まれています。  

- azure-keyvault-extensions には、暗号化アルゴリズム、RSAKey、および SymmetricKey の実装を含む拡張機能コードが含まれています。 これは、コアおよび KeyVault 名前空間に依存し、集計リゾルバー (複数のキー プロバイダーを使用する必要がある場合) およびキャッシュ キー リゾルバーを定義する機能を提供します。 ストレージ クライアント ライブラリはこのパッケージに直接依存しませんが、Azure Key Vault を使用してキーを格納するか、Key Vault 拡張機能を使用してローカルおよびクラウドの暗号化プロバイダーを使用する必要がある場合はこのパッケージが必要です。  

  Key Vault は値の高いマスター キー向けで、Key Vault ごとのスロットルの上限はこれを念頭に設計されています。 Key Vault を使用してクライアント側の暗号化を実行するときに推奨されるモデルは、Key Vault 内のシークレットやローカルにキャッシュされたシークレットとして格納された対象マスター キーを使用することです。 次の操作を実行する必要があります。  

1.  シークレットをオフラインで作成し、Key Vault にアップロードします。  

2.  シークレットのベース識別子をパラメーターとして使用して、シークレットの現在のバージョンを暗号化用に解決し、この情報をローカルでキャッシュします。 キャッシュ用の CachingKeyResolver の使用: ユーザーが自身のキャッシュ ロジックを実装することは想定されていません。  

3.  暗号化ポリシーの作成時に、入力としてキャッシュ リゾルバーを使用します。
Key Vault の使用方法についての詳細については、暗号化コードのサンプルを参照できます。 <fix URL>  

## ベスト プラクティス  
暗号化のサポートは、Java 用のストレージ クライアント ライブラリでのみ使用できます。

>**重要:**  
>クライアント側の暗号化を使用する場合は、次の重要な点に注意してください。
>  
>- 暗号化された BLOB を読み書きするときは、完全 BLOB アップロード コマンドと範囲/完全 BLOB ダウンロード コマンドを使用してください。 Put Block、Put Block List、Write Pages、Clear Pages、または Append Block などのプロトコル操作で暗号化された BLOB に書き込まないでください。暗号化された BLOB が壊れたり、読み取り不可能になったりすることがあります。  
>
>- テーブルの場合、同様の制約があります。 暗号化メタデータを更新せずに暗号化されたプロパティを更新する行為は避けてください。  
>
>- 暗号化された BLOB にメタデータを設定する場合、メタデータの設定は付加的には行われないため、復号化に必要な暗号化関連メタデータが上書きされる可能性があります。 これはスナップショットにも該当します。暗号化された BLOB のスナップショットを作成するときにメタデータを指定しないでください。 メタデータを設定する必要がある場合を呼び出すことを確認する、 **downloadAttributes** メソッドはまずを現在の暗号化メタデータを取得するメタデータが設定されている間の同時書き込みを回避します。  
>
>- 有効にする、 **requireEncryption** は暗号化されたデータでのみ動作するユーザーの既定の要求オプションのフラグ。 詳細については、以下をご覧ください。  

## クライアント API / インターフェイス  
EncryptionPolicy オブジェクトの作成では、キーのみ (IKey の実装)、リゾルバーのみ (IKeyResolver の実装)、またはその両方を指定できます。 IKey は基本的なキーの種類で、キー識別子を使用して識別され、ラップ/ラップ解除のロジックを指定します。 IKeyResolver は復号化プロセス中のキーの解決に使用します。 これは、IKey で指定されたキー識別子を返す ResolveKey メソッドを定義します。 これは、複数の場所で管理されている複数のキーの中から選択するための機能を提供します。  
- 暗号化では、キーは常に使用され、キーがないとエラーが発生します。  
- 復号化の場合:  
    - キーを取得するよう指定した場合にキー リゾルバーが起動します。 リゾルバーが指定されていても、キー識別子のマッピングがない場合、エラーがスローされます。  
    - リゾルバーが指定されていない場合にキーが指定されると、そのキーの識別子が必須キー識別子と一致すると、そのキーが使用されます。 識別子が一致しなければ、エラーがスローされます。  

       [暗号化のサンプル](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) <fix URL>blob、キュー、およびテーブル、Key Vault 統合と共により詳細なエンド ツー エンドのシナリオを示してください。

### RequireEncryption モード  
すべてのアップロードとダウンロードを暗号化する必要がある場合、オプションで操作のモードを有効にすることができます。 このモードでは、暗号化ポリシーを設定せずにデータをアップロードしようとしたり、サービスで暗号化されていないデータをダウンロードしようとしたりすると、クライアントで失敗します。  **RequireEncryption** 要求オプション オブジェクトのフラグは、この動作を制御します。 アプリケーションの Azure ストレージに格納されているすべてのオブジェクトによって暗号化されるかどうかは、設定することができます、 **requireEncryption** service クライアント オブジェクトの既定の要求オプションのプロパティです。   

たとえば、使用して **CloudBlobClient.getDefaultRequestOptions().setRequireEncryption(true)** にすべての blob クライアント オブジェクトを使用して実行する操作に対して暗号化を要求します。

### BLOB サービス暗号化  
作成、 **BlobEncryptionPolicy** オブジェクトし、要求オプションに設定 (API ごと、またはを使用してクライアント レベルで **DefaultRequestOptions**)。 その他の操作はすべて、クライアント ライブラリが内部的に処理します。

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);
    
    // Set the encryption policy on the request options.
    BlobRequestOptions options = new BlobRequestOptions();
    options.setEncryptionPolicy(policy);
    
    // Upload the encrypted contents to the blob.
    blob.upload(stream, size, null, options, null);
    
    // Download and decrypt the encrypted contents from the blob.
    ByteArrayOutputStream outputStream = new ByteArrayOutputStream(); blob.DownloadToStream(outputStream, null, options, null);

### Queue サービス暗号化  
作成、 **QueueEncryptionPolicy** オブジェクトし、要求オプションに設定 (API ごと、またはを使用してクライアント レベルで **DefaultRequestOptions**)。 その他の操作はすべて、クライアント ライブラリが内部的に処理します。

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);
    
    // Create the encryption policy to be used for upload and download.
    QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);
    
    // Add message
    QueueRequestOptions options = new QueueRequestOptions();
    options.setEncryptionPolicy(policy);
    
    queue.addMessage(message, 0, 0, options, null);
    
    // Retrieve message
    CloudQueueMessage retrMessage = queue.retrieveMessage(30, options, null);

### Table サービス暗号化  
暗号化ポリシーを作成して、要求オプションに設定、だけでなくいずれかを指定してください、 **EncryptionResolver** で **TableRequestOptions**, 、またはエンティティの getter と setter の [暗号化] 属性を設定します。

### リゾルバーの使用  

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);
    
    // Create the encryption policy to be used for upload and download.
    TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);
    
    TableRequestOptions options = new TableRequestOptions() 
    options.setEncryptionPolicy(policy);
    options.setEncryptionResolver(new EncryptionResolver() {
        public boolean encryptionResolver(String pk, String rk, String key) {
            if (key == "foo")
            {
                return true;
            }
            return false;
        }
    });
    
    // Insert Entity
    currentTable.execute(TableOperation.insert(ent), options, null);
    
    // Retrieve Entity
    // No need to specify an encryption resolver for retrieve
    TableRequestOptions retrieveOptions = new TableRequestOptions() 
    retrieveOptions.setEncryptionPolicy(policy);
    
    TableOperation operation = TableOperation.retrieve(ent.PartitionKey, ent.RowKey, DynamicTableEntity.class);
    TableResult result = currentTable.execute(operation, retrieveOptions, null);

### 属性の使用  
上記のように、エンティティで TableEntity を実装する場合、プロパティの getter および setter できる属性で修飾する、[暗号化] を指定することではなく、 **EncryptionResolver**します。

    private string encryptedProperty1;

    @Encrypt
    public String getEncryptedProperty1 () {
        return this.encryptedProperty1;
    }
    
    @Encrypt
    public void setEncryptedProperty1(final String encryptedProperty1) {
        this.encryptedProperty1 = encryptedProperty1;
    }

## 暗号化とパフォーマンス  
ストレージ データを暗号化すると、パフォーマンスのオーバーヘッドが増えることに注意してください。 コンテンツ キーと IV を生成する必要があり、コンテンツ自体を暗号化する必要があります。また、追加のメタデータをフォーマットおよびアップロードする必要もあります。 このオーバーヘッドは、暗号化されるデータの量によって異なります。 開発中に、アプリケーションのパフォーマンスを常にテストすることをお勧めします。

## 次のステップ  
ダウンロード、 [Java Maven パッケージ用の Azure ストレージ クライアント ライブラリ](<fix URL>)  
ダウンロード、 [GitHub からソース コードを Java 用 Azure ストレージ クライアント ライブラリ](https://github.com/Azure/azure-storage-java)   
Azure Key Vault Maven ダウンロード [コア](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/), 、[クライアント](http://www.nuget.org/packages/Microsoft.Azure.KeyVault/), 、および [拡張](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/) パッケージ
参照してください、 [Azure Key Vault のドキュメント](../articles/key-vault-whatis.md)  

