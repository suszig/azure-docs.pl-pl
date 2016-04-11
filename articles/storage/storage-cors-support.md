<properties 
    pageTitle="クロス オリジン リソース共有 (CORS) のサポート | Microsoft Azure" 
    description="Microsoft Azure ストレージ サービスでの CORS のサポートを有効にする方法について説明します。" 
    services="storage" 
    documentationCenter=".net" 
    authors="tamram" 
    manager="carolz" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/03/2015" 
    ms.author="tamram;andtyler"/>

# Azure ストレージ サービスでのクロス オリジン リソース共有 (CORS) のサポート

バージョン 2013-08-15 以降の Azure ストレージ サービスでは、BLOB、Table、Queue の各サービスでクロス オリジン リソース共有 (CORS) をサポートしています。 CORS は、あるドメインで実行されている Web アプリケーションが別のドメイン内にあるリソースにアクセスできるようにする HTTP 機能です。 Web ブラウザーと呼ばれるセキュリティ制限を実装する [の同一オリジン ポリシー](http://www.w3.org/Security/wiki/Same_Origin_Policy) web ページから別のドメインに Api を呼び出すことを防止します。CORS は、別のドメインの Api を呼び出して 1 つのドメイン (元のドメイン) を許可する安全な方法を提供します。 参照してください、 [CORS の仕様](http://www.w3.org/TR/cors/) CORS の詳細。

CORS ルールを呼び出すことによって各ストレージ サービスに対して個別に設定できる [Set Blob Service Properties](https://msdn.microsoft.com/library/hh452235.aspx), 、[Set Queue Service Properties](https://msdn.microsoft.com/library/hh452232.aspx), 、および [Set Table Service Properties](https://msdn.microsoft.com/library/hh452240.aspx)します。 サービスに対して CORS ルールを設定した場合、別のドメインからサービスに対して行われた要求が正しく認証されると、指定したルールに従ってその要求を許可するかどうかが評価されます。

>[AZURE.NOTE] CORS は認証メカニズムではないことに注意してください。 CORS が有効な場合、ストレージ リソースに対する要求はすべて認証署名が適切であるか、パブリック リソースに対して行う必要があります。

## CORS 要求について

元のドメインからの CORS 要求は、次の 2 つの異なる要求で構成されている場合があります。

- サービスによって課されている CORS の制限を照会するプレフライト要求。 要求メソッドがない場合は、プレフライト要求が必要な [の簡単な方法](http://www.w3.org/TR/cors/), 、GET、HEAD、または POST を意味します。

- 目的のリソースに対して行う実際の要求。

### プレフライト要求

プレフライト要求では、アカウント所有者がストレージ サービスに対して設定した CORS の制限を照会します。 Web ブラウザー (または他のユーザー エージェント) は、要求ヘッダー、メソッド、元のドメインを含む OPTIONS 要求を送信します。 ストレージ サービスでは、あらかじめ構成された一連の CORS ルールに基づいて目的の操作を評価します。CORS ルールには、ストレージ リソースに対する実際の要求で指定できる元のドメイン、要求メソッド、要求ヘッダーを指定します。

CORS がサービスに対して有効になっており、プレフライト要求と一致する CORS ルールがある場合、サービスはステータス コード 200 (OK) で応答し、必要な Access-Control ヘッダーを応答に含めます。

CORS がサービスに対して有効になっていない場合、またはプレフライト要求と一致する CORS ルールがない場合、サービスはステータス コード 403 (Forbidden) で応答します。

OPTIONS 要求に必須の CORS ヘッダー (Origin ヘッダーと Access-Control-Request-Method ヘッダー) が含まれていない場合、サービスはステータス コード 400 (Bad request) で応答します。

プレフライト要求は、要求されたリソースではなく、サービス (BLOB、Queue、Table) に対して評価されることに注意してください。 要求を正常に終了させるには、アカウント所有者がアカウント サービス プロパティの一部として CORS を有効にしておく必要があります。

### 実際の要求

プレフライト要求が受け入れられ、応答が返されると、ブラウザーはストレージ リソースに対する実際の要求をディスパッチします。 プレフライト要求が拒否されると、ブラウザーは実際の要求を直ちに拒否します。

実際の要求は、ストレージ サービスに対する通常の要求として扱われます。 Origin ヘッダーが存在する場合は要求が CORS 要求であることを示しており、サービスは一致する CORS ルールを確認します。 一致が見つかった場合は、Access-Control ヘッダーを応答に追加し、クライアントに送り返します。 一致が見つからない場合は、CORS Access-Control ヘッダーを返しません。

## Azure ストレージ サービスの CORS の有効化

CORS ルールは、サービス レベルで設定します。そのため、各サービス (BLOB、Queue、Table) に対して個別に CORS を有効または無効にする必要があります。 既定では、各サービスの CORS は無効になっています。 CORS を有効にするには、バージョン 2013-08-15 以降を使用して適切なサービス プロパティを設定し、CORS ルールをサービス プロパティに追加する必要があります。 有効または無効にする方法の詳細について CORS ルールを設定する方法と、サービスに対して CORS を参照してください [Set Blob Service Properties](https://msdn.microsoft.com/library/hh452235.aspx), 、[Set Queue Service Properties](https://msdn.microsoft.com/library/hh452232.aspx), 、および [Set Table Service Properties](https://msdn.microsoft.com/library/hh452240.aspx)します。

サービス プロパティ設定操作で指定した 1 つの CORS ルールのサンプルを次に示します。

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

CORS ルールに含まれている各要素は次のとおりです。

- **AllowedOrigins**: CORS を使用して、ストレージ サービスに対する要求が許可される元のドメイン。 元のドメインとは、要求が発行されたドメインです。 元のドメインは、ユーザー エージェントがサービスに送信した元のドメインと、大文字と小文字の違いも含めて正確に一致する必要があります。 また、ワイルドカード文字 '*' を使用して、すべての元のドメインからの CORS を使用した要求を許可することもできます。 ドメイン上の例で [http://www.contoso.com](http://www.contoso.com) と [http://www.fabrikam.com](http://www.fabrikam.com) CORS を使用して、サービスに対する要求を行うことができます。

- **AllowedMethods**: 元のドメインが CORS 要求で使用するメソッド (HTTP 要求の動詞)。 上記の例では、PUT 要求と GET 要求のみが許可されます。

- **AllowedHeaders**: 元のドメインが CORS 要求に指定できる要求ヘッダー。 上記の例では、x-ms-meta-data、x-ms-meta-target、x-ms-meta-abc で始まるすべてのメタデータ ヘッダーが許可されます。 ワイルドカード文字 '*' は、指定したプレフィックスで始まるすべてのヘッダーが許可されることを示しています。

- **ExposedHeaders**: CORS 要求への応答で送信され、ブラウザーが要求の発行元に公開できる応答ヘッダー。 上記の例では、x-ms-meta で始まるすべてのヘッダーを公開するようブラウザーに指示しています。

- **MaxAgeInSeconds**: ブラウザーがプレフライト OPTIONS をキャッシュする最大時間を要求します。

Azure ストレージ サービスの両方のヘッダーを指定のサポート、 **AllowedHeaders** と **ExposedHeaders** 要素。 ヘッダーのカテゴリを許可するには、そのカテゴリに共通するプレフィックスを指定します。 たとえば、指定する *x ms メタ** プレフィックスが指定のヘッダーを x ms メタで始まるすべてのヘッダーと一致するルールを設定するとします。

CORS ルールには、次の制限事項が適用されます。

- ストレージ サービス (BLOB、Table、Queue) ごとに指定できる CORS ルールは最大 5 個です。
- 要求で設定されているすべての CORS ルールの最大サイズ (XML タグを除く) は、2 KB を超えることはできません。
- 許可されるヘッダー、公開されるヘッダー、許可される元のドメインの長さは、256 文字を超えることはできません。
- 許可されるヘッダーと公開されるヘッダーとして、次のいずれかを使用できます。
  - リテラル ヘッダー。 な正確なヘッダー名を指定よう **x に ms のメタ-処理された**します。 最大 64 個のリテラル ヘッダーを要求で指定できます。
  - 場所ヘッダーのプレフィックスはなどの指定のヘッダーをプレフィックスとして付加 **x ms メタ データ*** します。 このような方法でプレフィックスを指定した場合は、指定のプレフィックスで始まるすべてのヘッダーが許可または公開されます。 最大 2 個のプレフィックスが指定されたヘッダーを要求で指定できます。
- 指定されたメソッド (または HTTP 動詞)、 **AllowedMethods** 要素は、Azure ストレージ サービス Api でサポートされているメソッドに従う必要があります。 サポートされるメソッドは、DELETE、GET、HEAD、MERGE、POST、OPTIONS、PUT です。

## CORS ルールの評価ロジックについて

ストレージ サービスでは、プレフライト要求や実際の要求を受け取ると、適切なサービス プロパティ設定操作でサービスに設定された CORS ルールに基づいてその要求を評価します。 CORS ルールは、サービス プロパティ設定操作の要求本文に設定された順序で評価されます。

CORS ルールは、次のように評価されます。

1. 最初に、要求の元のドメインは、ドメインの一覧に照らし合わせてチェックされます、 **AllowedOrigins** 要素。 元のドメインが一覧に含まれている場合、またはワイルドカード文字 '*' によってすべてのドメインが許可されている場合、ルールの評価は次に進みます。 元のドメインが含まれていない場合、要求は失敗します。

2. 次に、要求のメソッド (または HTTP 動詞) が照合されているメソッドの **AllowedMethods** 要素。 メソッドが一覧に含まれている場合、ルールの評価は次に進みます。含まれていない場合、要求は失敗します。

3. 要求が、要求の元のドメインとメソッドのルールと一致した場合、要求を処理するためにそのルールが選択され、それ以降のルールは評価されません。 要求が成功する前に、要求で指定されたヘッダーと照合されますヘッダーの一覧、 **AllowedHeaders** 要素。 送信したヘッダーが許可されるヘッダーと一致しない場合、要求は失敗します。

ルールは要求本文に含まれている順序で処理されるため、元のドメインに関する最も制限の厳しいルールを一覧の先頭に指定して、そのルールが最初に評価されるようにすることをお勧めします。 制限の緩いルール (すべての元のドメインを許可するルールなど) は一覧の末尾に指定します。

### 例 – CORS ルールの評価

次の例では、ストレージ サービスに CORS ルールを設定する操作の要求本文の一部を示しています。 参照してください [Set Blob Service Properties](https://msdn.microsoft.com/library/hh452235.aspx), 、[Set Queue Service Properties](https://msdn.microsoft.com/library/hh452232.aspx), 、および [Set Table Service Properties](https://msdn.microsoft.com/library/hh452240.aspx) 要求の作成の詳細。

    <Cors>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>PUT,HEAD</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>*</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
        </CorsRule>
    </Cors>

次に、以下の CORS 要求について考えてみましょう。

要求||| 応答||
---|---|---|---|---
**メソッド** |**Origin (配信元)** |**要求ヘッダー** |**ルールの一致** |**結果**
**PUT** | http://www.contoso.com |x-ms-blob-content-type | 最初のルール |成功
**GET** | http://www.contoso.com| x-ms-blob-content-type | 2 番目のルール |成功
**GET** | http://www.contoso.com| x-ms-blob-content-type | 2 番目のルール | 失敗

最初の要求は最初のルールと一致します (元のドメインが許可される元のドメインと一致し、メソッドが許可されるメソッドと一致し、ヘッダーが許可されるヘッダーと一致します)。そのため、成功します。

2 番目の要求は、メソッドが許可されるメソッドと一致しないため、最初のルールと一致しません。 しかし、2 番目のルールと一致するため、成功します。

3 番目の要求は、元のドメインとメソッドが 2 番目のルールと一致します。そのため、それ以降のルールは評価されません。 ただし、 *x-ms-クライアントの要求-id ヘッダー* の 3 番目のルールのセマンティクスは許可されている成功するものの、要求に失敗したため、2 番目のルールで許可されていません。

>[AZURE.NOTE] この例より制限の厳しいも前より制限の緩いルールでは、一般に推奨される運用方法は最も制限の厳しいルールを最初に一覧表示します。

## Vary ヘッダーの設定方法について

 *Vary* ヘッダーは、一連の要求を処理する、サーバーによって選択された条件についてブラウザーまたはユーザー エージェントにアドバイスする要求ヘッダー フィールドで構成された標準 http/1.1 ヘッダーです。  *Vary* ヘッダーは主にプロキシ、ブラウザー、および応答のキャッシュ方法を判断する使用の Cdn でキャッシュを使用します。 詳細については、仕様を参照してください、 [Vary ヘッダー](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)します。

ブラウザーまたは別のユーザー エージェントでは、CORS 要求の応答をキャッシュする際に、元のドメインを許可される元のドメインとしてキャッシュします。 キャッシュがアクティブな間に 2 つ目のドメインがストレージ リソースに対して同じ要求を発行した場合、ユーザー エージェントはキャッシュされている元のドメインを取得します。 2 つ目のドメインはキャッシュされているドメインと一致しないため、要求は失敗します (一致する場合は成功します)。 場合によっては、Azure ストレージ設定、Vary ヘッダー **オリジン** 要求元のドメインとキャッシュされた元の異なるサービスに後続の CORS 要求を送信するユーザー エージェントに指示します。

Azure のストレージ設定、 *Vary* ヘッダーを **オリジン** 実際の GET/HEAD 要求、次の場合。

- 要求元のドメインと CORS ルールで定義されている許可される元のドメインが完全に一致する。 完全に一致するためには、CORS ルールにワイルドカード文字 ' * ' が含まれていない必要があります。

- 要求元のドメインと一致するルールはないが、ストレージ サービスに対して CORS が有効になっている。

GET/HEAD 要求がすべての元のドメインを許可する CORS ルールと一致する場合、応答はすべての元のドメインが許可されていることを示し、ユーザー エージェントのキャッシュがアクティブな間は、すべての元のドメインからの後続の要求が許可されます。

GET/HEAD 以外のメソッドに対する応答はユーザー エージェントによってキャッシュされないため、これらのメソッドを使用する要求では、ストレージ サービスが Vary ヘッダーを設定することはありません。

前に説明した例に基づいて、Azure ストレージが GET/HEAD 要求にどのように応答するかを次の表に示します。

Request|アカウントの設定とルール evaluation||| の結果Response|||
---|---|---|---|---|---|---|---|---
**要求に origin ヘッダー** | **このサービスに指定された CORS ルールが** | **照合ルールが存在するすべてのオリジンを許可する (*)** | **正確な元のドメインと一致するルールが存在する** | **応答には、Origin に Vary ヘッダーのセットが含まれています** | **。応答には、アクセス制御の許可-オリジンが含まれています:"*"** | **応答には、アクセス コントロール-公開-ヘッダーが含まれています。**
| なし| なし| なし| なし| なし| なしなし
| なし| あり || なし| なし| あり || なしなし
| なし| あり || あり || なし| なし| あり |[はい]
| あり || なし| なし| なし| なし| なしなし
| あり || あり || なし| あり || あり || なし[はい]
| あり || あり || なし| なし| あり || なしなし
| あり || あり || あり || なし| なし| あり |[はい]


## CORS 要求への課金

成功したプレフライト要求は、任意のアカウントの storage サービスに対して CORS を有効にした場合に課金されます (を呼び出して [Set Blob Service Properties](https://msdn.microsoft.com/library/hh452235.aspx), 、[Set Queue Service Properties](https://msdn.microsoft.com/library/hh452232.aspx), 、または [Set Table Service Properties](https://msdn.microsoft.com/library/hh452240.aspx))。 料金を最小限にすることを検討、 **MaxAgeInSeconds** ユーザー エージェントが、要求をキャッシュするため、大きな値にルールを CORS 内の要素。

失敗したプレフライト要求に対しては課金されません。

## 次のステップ

[Set Blob Service Properties](https://msdn.microsoft.com/library/hh452235.aspx)

[Set Queue Service Properties](https://msdn.microsoft.com/library/hh452232.aspx)

[Set Table Service Properties](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C のクロス オリジン リソース共有の仕様](http://www.w3.org/TR/cors/)
 


