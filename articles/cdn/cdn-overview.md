<properties 
    pageTitle="Azure CDN の概要" 
    description="Azure Content Delivery Network (CDN) の概要と、CDN を使用して BLOB と静的コンテンツをキャッシュして高帯域幅コンテンツを配信する方法について説明します。" 
    services="cdn" 
    documentationCenter=".NET" 
    authors="camsoper" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cdn" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/02/2015" 
    ms.author="casoper"/>

# Azure Content Delivery Network (CDN) の概要

Azure Content Delivery Network (CDN) では、クラウド サービスで使用される Azure の BLOB と静的コンテンツを戦略的に配置された場所にキャッシュし、ユーザーへのコンテンツ配信に最大帯域幅を使用できます。 

CDN エンドポイントを管理できるようになりました既存の CDN 顧客の場合、 [Microsoft Azure ポータル](https://portal.azure.com)します。 


CDN では、世界各地の物理ノードにコンテンツをキャッシュすることによって、高帯域幅コンテンツを配信するためのグローバル ソリューションを開発者に提供します。 現在の CDN ノードの場所の一覧を表示するには、次を参照してください。 [Azure コンテンツ配信ネットワーク (CDN) POP 場所](cdn-pop-locations.md)します。

CDN を使用して Azure データをキャッシュすると、次のような利点があります。

- コンテンツ ソースから遠く離れた場所にいる、コンテンツの読み込みに数多くの "インターネット トリップ" が必要なアプリケーションを使用しているエンド ユーザーに対する、パフォーマンスとユーザー エクスペリエンスの向上
- 製品発表イベントの開始時のような、瞬間的高負荷を処理しやすくする大型の配信スケール 


>[AZURE.IMPORTANT] を作成したり、CDN エンドポイントを有効にする場合、世界中で反映されるまでに最大 90 分かかる場合があります。
 
最初にオブジェクトに対する要求が CDN に行われると、オブジェクトはオブジェクトのソースの元の場所から直接取得されます。  この配信元には、Azure ストレージ アカウント、Web アプリ、クラウド サービス、パブリック Web 要求を受け入れる任意のカスタム配信元などがあります。  CDN 構文を使用して要求が行われると、その要求は、要求が行われた場所から最も近い CDN エンドポイントにリダイレクトされ、オブジェクトへのアクセスが提供されます。 オブジェクトがそのエンドポイントで見つからない場合は、サービスから取得され、そのエンドポイントにキャッシュされます。有効期間 (TTL) の設定はキャッシュされたオブジェクトでも維持されています。

## Standard の機能

Standard CDN レベルには、以下の機能が含まれます。

- Azure と簡単に統合サービスなど、 [ストレージ](cdn-create-a-storage-account-with-cdn.md), 、Web アプリ、および Media Services
- [クエリ文字列のキャッシュ](cdn-query-string.md)
- [カスタム ドメイン名のサポート](cdn-map-content-to-custom-domain.md)
- [国のフィルタリング](cdn-restrict-access-by-country.md)
- [コア分析](cdn-analyze-usage-patterns.md)
- [カスタム コンテンツ配信元](cdn-map-content-to-custom-domain.md)
- [HTTPS のサポート](cdn-how-to-use-cdn.md#accessing-cached-content-over-https)
- 負荷分散
- DDOS 保護
- [高速消去](cdn-purge-endpoint.md)
- [REST API を介した管理](https://msdn.microsoft.com/library/mt634456.aspx)


## Premium の機能

Premium CDN レベルには Standard レベルのすべての機能に加え、以下の機能があります。

- [カスタマイズ可能なルール ベースのコンテンツ配信エンジン](cdn-rules-engine.md)
- [詳細な HTTP レポート](cdn-advanced-http-reports.md)
- [リアルタイム統計](cdn-read-time-stats.md)

