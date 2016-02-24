<properties 
    pageTitle="Azure Redis Cache のサンプル" 
    description="Azure Redis Cache の使用方法について説明します" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="12/03/2015" 
    ms.author="sdanie"/>

# Azure Redis Cache のサンプル 

このトピックでは、キャッシュへの接続、キャッシュ内のデータの読み取りと書き込み、ASP.NET Redis Cache プロバイダーの使用などのシナリオを含め、Azure Redis Cache のサンプルの一覧を示します。 サンプルの一部はダウンロード可能なプロジェクトです。ステップ バイ ステップ ガイダンスが用意されているサンプルやコード スニペットが含まれているサンプルもありますが、ダウンロード可能なプロジェクトにはリンクしていません。

## Hello world サンプル

このセクションのサンプルでは、Azure Redis Cache インスタンスへの接続およびさまざまな言語と Redis クライアントを使用したキャッシュ内のデータの読み取りと書き込みの基本を示します。

 [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) サンプルを使用してさまざまなキャッシュ操作を実行する方法を示しています、 [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET クライアントです。

このサンプルは次の方法を示しています。

-   さまざまな接続オプションを使用する
-   同期操作と非同期操作を使用し、キャッシュ間でオブジェクトを読み書きする
-   Redis MGET/MSET コマンドを使用し、指定されたキーの値を返す
-   Redis トランザクション操作を実行する
-   Redis リストと並べ替えられたセットを使用する
-   JsonConvert シリアライザーを使用し、.NET オブジェクトを保存する
-   Redis セットを使用し、タグ付けを実装する

詳細については、次を参照してください。、 [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) github、および他の用途シナリオのマニュアルを参照してください、 [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) 単体テストします。

[Python で Azure Redis Cache を使用する方法](cache-python-get-started.md) Python を使用して Azure Redis Cache を使用する方法を示していますと [redis py](https://github.com/andymccurdy/redis-py) クライアントです。

 [PHP の例](https://msdn.microsoft.com/library/azure/dn690470.aspx#PHPExample) PHP で Azure Redis Cache の使用を開始する方法を説明し、 [predis](https://github.com/nrk/predis) クライアントです。

[キャッシュ内で .NET オブジェクトを扱う](https://msdn.microsoft.com/library/azure/dn690521.aspx#Objects) を読み書きし、それらを Azure Redis Cache インスタンスから読み取るために .NET オブジェクトをシリアル化する方法を示しています。 

## ASP.NET SignalR のスケールアウト バックプレーンとしての Redis Cache の使用

 [、スケール アウト ASP.NET SignalR のバック プレーンとしての Redis Cache の使用](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) サンプルでは、SignalR のバック プレーンとして Azure Redis Cache を使用する方法を示しています。 バック プレーンの詳細については、次を参照してください。 [Redis を使用した SignalR のスケール アウト](http://www.asp.net/signalr/overview/performance/scaleout-with-redis)します。

## Redis Cache の顧客クエリのサンプル

このサンプルでは、キャッシュ内のデータへのアクセスと永続ストレージ内のデータへのアクセスとのパフォーマンスを比較します。 このサンプルには 2 つのプロジェクトがあります。

-   [Redis Cache でデータをキャッシュしてパフォーマンスを向上させる方法のデモ](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
-   [デモ用のデータベースおよびキャッシュのシード](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## ASP.NET セッション状態と出力キャッシュ

 [ASP.NET SessionState と outputcache の格納の Azure Redis Cache の使用](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) サンプルではどの Azure Redis Cache を使用して ASP.NET セッションと Redis の SessionState と OutputCache プロバイダーを使用して、出力キャッシュを格納します。

## MAML を使用した Azure Redis Cache の管理

 [Azure Redis Cache の管理 Azure 管理ライブラリを使用して](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) サンプルでは使用する方法 Azure 管理ライブラリを管理 (作成/更新/削除) してキャッシュします。 

## カスタム監視のサンプル

 [アクセス Redis Cache の監視データ](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) サンプルでは、Azure ポータルの外部から Azure Redis Cache の監視データをアクセスする方法を示しています。

## PHP と Redis を使用して記述された Twitter 形式の複製

 [Retwis](https://github.com/SyntaxC4-MSFT/retwis) サンプルは、「Redis Hello World します。 Redis と PHP で記述された最小の Twitter 形式ソーシャル ネットワーク クローンは、それを使用して、 [Predis](https://github.com/nrk/predis) クライアントです。 このソース コードは非常に単純に設計されており、同時にさまざまな Redis データ構造を示しています。

## 帯域幅モニター

 [帯域幅モニター](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) サンプルでは、クライアントで使用される帯域幅を監視することができます。 帯域幅を測定するには、キャッシュ クライアント コンピューターでサンプルを実行し、キャッシュを呼び出し、帯域幅モニターのサンプルで報告された帯域幅を観察します。

