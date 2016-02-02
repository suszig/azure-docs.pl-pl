<properties 
    pageTitle="CDN を使用する方法 | Microsoft Azure" 
    description="Azure Content Delivery Network (CDN) を使用して、BLOB と静的コンテンツをキャッシュすることにより、高帯域幅コンテンツを配信する方法について説明します。" 
    services="cdn" 
    documentationCenter=".net" 
    authors="camsoper" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cdn" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="12/02/2015" 
    ms.author="casoper"/>



# Azure 用 CDN の使用

Azure Content Delivery Network (CDN) は、Azure で任意の HTTP アプリケーションを拡張するための基本的要素です。 CDN は、Azure のお客様にグローバル ソリューションを提供するもので、エンド ユーザーに近い場所にコンテンツをキャッシュして配信します。 そのため、ユーザーからの要求は、毎回オリジンを参照する代わりに、最適なパフォーマンスが得られる CDN エッジ POP にインテリジェントにルーティングされます。 これにより、パフォーマンスとユーザー エクスペリエンスが大幅に向上します。 現在の CDN ノードの場所の一覧
CDN ノードの場所を参照してください [Azure コンテンツ配信ネットワーク (CDN) POP 場所](cdn-pop-locations.md)します。

CDN を使用して Azure データをキャッシュすることには、次のような利点があります。

- コンテンツ ソースから遠く離れた場所にいる、コンテンツの読み込みに数多くの HTTP 要求が必要なアプリケーションを使用するエンド ユーザーに対する、パフォーマンスとユーザー エクスペリエンスの向上。
- 製品発表イベントの開始時のような、瞬間的高負荷を処理しやすくする大型の配信スケール。
- ユーザー要求を分散させ、コンテンツをグローバル エッジ POP から配信することによる、配信元へのトラフィックの削減。

>[AZURE.TIP] Azure CDN では、さまざまな配信元からコンテンツを配信できます。 Azure 内で統合された配信元には、App Service、Cloud Services、Blob Storage、Media Services が含まれます。 パブリックにアクセスできる Web アドレスを使用して、カスタム配信元を定義することもできます。

## CDN を有効にする方法

1. 配信元を指すエンドポイントで CDN プロファイルを作成します。

    CDN プロファイルは、CDN エンドポイントのコレクションです。 各プロファイルには、1 つ以上の CDN エンドポイントが含まれます。 CDN プロファイルを作成すると、選択した配信元を使用して、新しい CDN エンドポイントを作成できます。
    >[AZURE.NOTE] 1 つの Azure サブスクリプションは、4 つの CDN プロファイルに限定されます。 各 CDN プロファイルは、4 つの CDN エンドポイントに制限されます。
    >
    > CDN の価格は、CDN プロファイル レベルで適用されます。 Standard と Premium の CDN 機能を組み合わせて使用する場合は、複数の CDN プロファイルが必要になります。

    CDN のプロファイルとエンドポイントの作成の詳細なチュートリアルについては、次を参照してください。 [azure コンテンツ配信ネットワークを有効にする方法](cdn-create-new-endpoint.md)します。

2. CDN 構成を設定する

    有効化するさまざまな機能、CDN エンドポイントなど [キャッシュ ポリシー](cdn-caching-policy.md), 、[クエリ文字列のキャッシュ](cdn-query-string.md), 、[ルール エンジン](cdn-rules-engine.md), などです。 詳細については、左側の **[管理]** メニューを参照してください。

3. CDN コンテンツにアクセスする

    CDN にキャッシュされたコンテンツにアクセスするには、ポータルで提供される CDN URL を使用します。たとえば、キャッシュされた blob のアドレスは、次のようななります: `http://&lt;identifier&gt;.azureedge.net/&lt;myPublicContainer&gt;/&lt;BlobName&gt;`

## Azure Storage からのコンテンツのキャッシュ

CDN が Azure ストレージ アカウントで有効になると、パブリック コンテナーに含まれる、匿名アクセスで利用できる BLOB が CDN によってキャッシュされます。 Azure CDN では、パブリックで利用可能な BLOB だけをキャッシュできます。 BLOB を匿名アクセスでパブリックに利用できるようにするには、コンテナーをパブリックとして指定する必要があります。 そうすると、そのコンテナー内のすべての BLOB が匿名読み取りアクセスで利用可能になります。 コンテナー データをパブリックにすることや、アクセスをコンテナー内の BLOB だけに制限することもできます。 参照してください [Azure ストレージ リソースへのアクセスの管理](../storage/storage-manage-access-to-resources.md) コンテナーと blob のアクセス制御を管理する方法についてです。

最適なパフォーマンスを実現するには、10 GB より小さいサイズの BLOB の配信に CDN のエッジ キャッシュを使用します。

ストレージ アカウントに対して CDN アクセスを有効にすると、管理ポータルで、次の形式で CDN ドメイン名: `http://&lt;identifier&gt;.azureedge.net/`します。このドメイン名を使ってパブリック コンテナー内の BLOB にアクセスできます。たとえば、myaccount というストレージ アカウントに music というパブリック コンテナーがある場合は、次の URL のどちらかを使えば、そのコンテナー内の BLOB にアクセスできます。

- **Azure Blob サービスの URL**: `http://myAccount.blob.core.windows.net/music/`
- **Azure CDN の URL**: `http://&lt;identifier&gt;.azureedge.net/music/`

## Azure Websites からのコンテンツのキャッシュ

Web サイトから CDN を有効にして、画像、スクリプト、スタイルシートなどの Web コンテンツをキャッシュすることができます。 参照してください [を Azure CDN と Azure の web サイトを統合](../app-service-web/cdn-websites-with-cdn.md)します。

Web サイトに対して CDN アクセスを有効にすると、管理ポータルで、次の形式で CDN ドメイン名: `http://&lt;identifier&gt;.azureedge.net/`します。このドメイン名を使用して、Web サイトからオブジェクトを取得できます。たとえば、パブリック コンテナー名が cdn で、画像ファイルの名前が music.png の場合、ユーザーは次の 2 つのどちらかの URL でオブジェクトにアクセスできます。

- **Azure の web サイトの URL**: `http://mySiteName.azurewebsites.net/cdn/music.png`
- **Azure CDN の URL**: `http://&lt;identifier&gt;.azureedge.net/cdn/music.png`

## Azure クラウド サービスからのコンテンツのキャッシュ

Azure クラウド サービスによって提供される CDN にオブジェクトをキャッシュすることができます。

クラウド サービスのキャッシュには、次の制約があります。


- 静的コンテンツをキャッシュする場合のみ CDN を使用する。
    >[AZURE.WARNING] 揮発性が高いコンテンツまたは動的コンテンツをキャッシュすると、パフォーマンスに悪影響を与えたり、コンテンツの問題が発生したりする可能性があり、コストの増加につながります。
- クラウド サービスが運用環境にデプロイされている必要がある。
- クラウド サービスが HTTP を使用してポート 80 でオブジェクトを提供する必要がある。
- クラウド サービスでは、キャッシュまたは配信するコンテンツをクラウド サービスの /cdn フォルダーに配置する必要がある。

クラウド サービスで、管理ポータルで、次の形式で CDN ドメイン名に対して CDN アクセスが有効と: `http://&lt;identifier&gt;.azureedge.net/`します。このドメイン名を使用して、クラウド サービスからオブジェクトを取得できます。たとえば、クラウド サービス名が myHostedService で、コンテンツを配信する ASP.NET Web ページが music.aspx の場合、ユーザーは次の 2 つのどちらかの URL を使用してオブジェクトにアクセスできます。


- **Azure クラウド サービスの URL**: `http://myHostedService.cloudapp.net/music.aspx`
- **Azure CDN の URL**: `http://&lt;identifier&gt;.azureedge.net/music.aspx`

## カスタム配信元からコンテンツをキャッシュする

パブリックにアクセスできる Web アプリケーションから提供されるオブジェクトを CDN にキャッシュできます。

カスタム配信元のキャッシュには、次の制約があります。

- 静的コンテンツをキャッシュする場合のみ CDN を使用する。
    >[AZURE.WARNING] 揮発性が高いコンテンツまたは動的コンテンツをキャッシュすると、パフォーマンスに悪影響を与えたり、コンテンツの問題が発生したりする可能性があり、コストの増加につながります。
- カスタム発信元のコンテンツは、パブリック IP アドレスを持つサーバーでホストする必要があります。 CDN のエッジ ノードでは、ファイアウォールの内側にあるイントラネット サーバーから資産を取得できません。

カスタムの配信元の Azure ポータルで、次の形式で CDN ドメイン名に対して CDN アクセスが有効と: `http://&lt;identifier&gt;.azureedge.net/`します。このドメイン名を使用して、カスタム配信元からオブジェクトを取得できます。たとえば、www.contoso.com にあるサイトで、コンテンツを配信する ASP.NET Web ページが music.aspx の場合、ユーザーは次の 2 つのどちらかの URL を使用してオブジェクトにアクセスできます。


- **カスタム配信元 URL**: `http://www.contoso.com/music.aspx`
- **Azure CDN の URL**: `http://&lt;identifier&gt;.azureedge.net/music.aspx`

## クエリ文字列を含む特定のコンテンツのキャッシュ

クエリ文字列を使用して、発信元から取得したオブジェクトを区別できます。 たとえば、発信元が別のグラフを表示した可能性がある場合、クエリ文字列を渡して必要な特定のグラフを取得することができます。 次に例を示します。

`http://&lt;identifier&gt;.azureedge.net/chart.aspx?item=1`

クエリ文字列は、文字列リテラルとして渡されます。2 つのパラメーターを受け取るなど、サービスがあるかどうかは `でしょうか。 領域 = 2 & item = 1` 続けて発生元を使用して、を呼び出すと `ですか? item = 1 & area = 2`, 、同じオブジェクトの 2 つのコピーがキャッシュされます。

クエリ文字列をキャッシュする方法の詳細については、次を参照してください。 [CDN のキャッシュ動作を制御するクエリ文字列を含む要求](cdn-query-string.md)します。

## HTTPS 経由でのキャッシュされたコンテンツへのアクセス

Azure では、HTTPS 呼び出しを使用して CDN からコンテンツを取得できます。 これにより、セキュリティ コンテンツの種類の混在に関する警告が発生することなく、CDN にキャッシュされたコンテンツをセキュリティで保護された Web ページに組み込むことができます。

HTTPS を使用して CDN コンテンツにアクセスする場合、次の制約があります。


- CDN によって提供される証明書を使用する必要がある。 サード パーティの証明書はサポートされません。
- CDN ドメインを使用して、コンテンツにアクセスする必要がある。 現時点では CDN がカスタム証明書をサポートしていないために、HTTPS サポートではカスタム ドメイン名 (CNAME) を使用できません。

CDN コンテンツに対して HTTPS を有効にする方法の詳細については、次を参照してください。 [azure コンテンツ配信ネットワーク (CDN) を有効にする方法](cdn-create-new-endpoint.md)します。


## カスタム ドメインを使用したキャッシュされたコンテンツへのアクセス

CDN の HTTP エンドポイントをカスタム ドメイン名にマップし、その名前を使用して CDN からオブジェクトを要求できます。

カスタム ドメインのマッピングの詳細については、次を参照してください。 [マップ コンテンツ配信ネットワーク (CDN) コンテンツをカスタム ドメインをする方法](cdn-map-content-to-custom-domain.md)します。

## CDN をプログラムで管理する

Microsoft Azure CDN は、プログラムでを使用して管理できる、 [CDN リソース プロバイダーの REST API](https://msdn.microsoft.com/library/mt634456.aspx)します。


## 関連項目

- [Azure のコンテンツ配信ネットワークを有効にする方法](cdn-create-new-endpoint.md)
- [Azure コンテンツ配信ネットワーク (CDN) の概要](cdn-overview.md)
- [Azure CDN エンドポイントを削除します。](cdn-purge-endpoint.md)
- [CDN のリソース プロバイダーの REST API](https://msdn.microsoft.com/library/mt634456.aspx)






