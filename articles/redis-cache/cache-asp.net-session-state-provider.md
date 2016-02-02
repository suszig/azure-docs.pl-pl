<properties 
   pageTitle="Cache ASP.NET セッション状態プロバイダー"
   description="Azure Redis Cache を使用して ASP.NET セッション状態を格納する方法を説明します。"
   services="redis-cache"
   documentationCenter="na"
   authors="steved0x"
   manager="dwrede"
   editor="tysonn" />
<tags 
   ms.service="cache"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="cache-redis"
   ms.workload="tbd"
   ms.date="12/03/2015"
   ms.author="sdanie" />


# Azure Redis Cache の ASP.NET セッション状態プロバイダー

Azure Redis Cache には、セッション状態プロバイダーが用意されています。セッション状態プロバイダーを使用すると、セッション状態を、メモリ内や SQL Server データベースにではなく、キャッシュに格納することができます。 キャッシュ セッション状態プロバイダーを使用するには、まず対象のキャッシュを構成し、Redis Cache Session State NuGet パッケージを使用して、キャッシュに必要な構成を ASP.NET アプリケーションに対して行います。

ユーザー セッションの状態をなんらかの形で格納しないのは、実際のクラウド アプリケーションでは実用的でない場合が多いですが、方法によっては、パフォーマンスとスケーラビリティに与える影響が大きくなります。 状態を格納する必要がある場合は、状態の量を少なくし、Cookie に格納することをお勧めします。 この方法を利用できない場合は、ASP.NET セッション状態と分散型メモリ内キャッシュのプロバイダーを使用することを次にお勧めします。 パフォーマンスとスケーラビリティの観点から最もお勧めできないのが、データベースを利用したセッション状態プロバイダーを使用する方法です。 このトピックでは、Azure Redis Cache の ASP.NET セッション状態プロバイダーを使用する方法について説明します。 その他のセッション状態オプションについては、次を参照してください。 [ASP.NET セッション状態オプション](#aspnet-session-state-options)します。

## ASP.NET セッション状態をキャッシュに格納する

Visual Studio で Redis Cache Session State NuGet パッケージを使用してクライアント アプリケーションを構成するには、**ソリューション エクスプローラー**でプロジェクトを右クリックし、**[NuGet パッケージの管理]** をクリックします。

![Azure Redis Cache Manage NuGet パッケージ](./media/cache-asp.net-session-state-provider/IC729541.png)

[オンライン検索] ボックスに「**RedisSessionStateProvider**」と入力し、結果の中からそのプロバイダーを選択して、[インストール] をクリックします。
>[AZURE.IMPORTANT] 使用する必要があります、premium 層から、クラスタ リング機能を使用している場合は、 [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.0 上またはで例外がスローされます。 これは、互換性に影響する変更です。詳細については、次を参照してください。 [v2.0.0 互換性に影響する変更の詳細](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details)します。

![Azure Redis Cache セッション状態プロバイダー](./media/cache-asp.net-session-state-provider/IC751730.png)

Redis セッション状態プロバイダー NuGet パッケージは、StackExchange.Redis.StrongName パッケージに依存します。 StackExchange.Redis.StrongName パッケージは、プロジェクト内に存在しなければインストールされます。 厳密な名前を持つ StackExchange.Redis.StrongName パッケージ以外に、厳密な名前を持たない StackExchange.Redis バージョンもあります。 厳密な名前を持たない StackExchange.Redis バージョンをプロジェクトで使用している場合は、Redis セッション状態プロバイダー NuGet パッケージのインストールの前か後にこのバージョンをアンインストールする必要があります。アンインストールしなかった場合は、名前の競合がプロジェクトで発生します。 これらのパッケージの詳細については、次を参照してください。 [構成 .NET キャッシュ クライアント](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients)します。

NuGet パッケージによって、必要なアセンブリ参照がダウンロードされて追加されます。さらに、web.config ファイルには、ASP.NET アプリケーションが Redis Cache Session 状態プロバイダーを使用するために必要な構成を記述した以下のセクションが追加されます。

    <sessionStatemode="Custom" customProvider="MySessionStateStore">
        <providers>
        
        <add name="MySessionStateStore"type="Microsoft.Web.Redis.RedisSessionStateProvider"host="127.0.0.1"accessKey="" ssl="false"/>
        </providers>
    </sessionState>

コメント化されたセクションには、属性の例と各属性のサンプル設定が記述されています。

属性の構成には、Microsoft Azure ポータルのキャッシュ ブレードの値を使用してください。その他の値は適宜構成します。 キャッシュのプロパティにアクセスする方法の詳細については、次を参照してください。 [キャッシュ設定の Redis 構成](cache-configure.md#configure-redis-cache-settings)します。

-   **host**: キャッシュ エンドポイントを指定します。
-   **port**: SSL の設定に応じて、非 SSL ポートまたは SSL ポートを使用します。
-   **accessKey**: キャッシュのプライマリ キーまたはセカンダリ キーを使用します。
-   **ssl**: キャッシュとクライアント間の通信を SSL で保護する場合は true、保護しない場合は false を指定します。 必ず適切なポートを指定してください。
    -   既定では、新しいキャッシュに対して非 SSL ポートは無効になっています。 SSL ポートを使用するには、この設定に true を指定します。 非 SSL ポートを有効にする方法の詳細については、次を参照してください。、 [アクセス ポート](cache-configure.md#access-ports) セクション、 [、キャッシュを構成する](cache-configure.md) トピックです。
-   **throwOnError**: 失敗時に例外がスローされるようにする場合は true、操作の失敗時にエラー メッセージが表示されないようにする場合は false を指定します。 静的 Microsoft.Web.Redis.RedisSessionStateProvider.LastException プロパティをチェックすることでエラーを確認できます。 既定値は true です。
-   **retryTimeoutInMilliseconds**: 失敗した操作がこの時間に再試行されます。ミリ秒単位で指定します。 最初は 20 ミリ秒後に再試行され、その後 retryTimeoutInMilliseconds の時間が経過するまで 1 秒ごとに再試行されます。 この時間を過ぎるとすぐに、操作が最後に 1 回再試行されます。 操作が失敗した場合、throwOnError 設定に応じて、例外がスローされて呼び出し元に戻ります。 既定値は 0 です。これは再試行されないことを意味します。
-   **databaseId**: キャッシュ出力データに使用するデータベースを指定します。 指定されていない場合は、既定値の 0 が使用されます。
-   **applicationName** – としての redis のキーが格納されている `{< アプリケーション名 > _ < セッション ID >} _Data`します。これによって、複数のアプリケーションで同じキーを共有できます。このパラメーターは省略可能です。指定されていない場合は、既定値が使用されます。
-   **connectionTimeoutInMilliseconds**: この設定によって、StackExchange.Redis クライアントの connectTimeout 設定を上書きすることができます。 指定されていない場合は、connectTimeout 設定の既定値である 5000 が使用されます。 詳細については、次を参照してください。 [StackExchange.Redis 構成モデル](http://go.microsoft.com/fwlink/?LinkId=398705)します。
-   **operationTimeoutInMilliseconds**: この設定によって、StackExchange.Redis クライアントの syncTimeout 設定を上書きすることができます。 指定されていない場合は、syncTimeout 設定の既定値である 1000 が使用されます。 詳細については、次を参照してください。 [StackExchange.Redis 構成モデル](http://go.microsoft.com/fwlink/?LinkId=398705)します。

これらのプロパティの詳細についてで元のブログ投稿の告知を参照してください。 [Redis 向け ASP.NET セッション状態プロバイダーを発表](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)します。

web.config の標準の InProc セッション状態プロバイダー セクションを忘れずにコメント アウトしてください。


これらの手順を実行すると、アプリケーションが Redis Cache セッション状態プロバイダーを使用するように構成されます。 アプリケーションでセッション状態を使用すると、状態が Azure Redis Cache インスタンスに格納されます。
>[AZURE.NOTE] 既定のメモリ内の ASP.NET セッション状態プロバイダーに格納できるデータと異なり、キャッシュに格納されるデータは、シリアル化可能である必要があります。 Redis のセッション状態プロバイダーを使用するときは、セッション状態に格納されるデータ型がシリアル化可能であることを確認してください。

## ASP.NET セッション状態のオプション

- メモリ内のセッション状態プロバイダー: このプロバイダーでは、メモリ内にセッション状態が格納されます。 このプロバイダーを使用する利点は、単純で高速なことです。 ただし、メモリ内プロバイダーを使用する場合、このプロバイダーは分散型でないため、Web Apps は拡張できません。

- SQL Server のセッション状態プロバイダー: このプロバイダーでは、SQL Server 内にセッション状態が格納されます。 永続的なストレージにセッション状態を保持するには、このプロバイダーを使用する必要があります。 Web アプリは拡張できますが、セッションに SQL Server を使用すると、Web アプリのパフォーマンスに影響を与えます。

- Redis Cache セッション状態プロバイダーなどの分散型メモリ内セッション状態プロバイダー: このプロバイダーを使用すると、両方の長所を活用できます。Web アプリで、単純かつ高速で、スケーラブルなセッション状態プロバイダーを使用できます。ただし、このプロバイダーではセッション状態がキャッシュに格納されるため、一時的なネットワーク障害など、分散型メモリ内キャッシュとの通信時に関するすべての要素をアプリで考慮する必要があります。キャッシュの使用に関するベスト プラクティスについては、次を参照してください。 [キャッシュに関するガイダンス](https://github.com/mspnp/azure-guidance/blob/master/Caching.md) Microsoft Patterns & Practices から [Azure クラウド アプリケーションの設計および実装に関するガイダンス](https://github.com/mspnp/azure-guidance)します。

セッション状態および他のベスト プラクティスの詳細については、次を参照してください。 [Web 開発に関するベスト プラクティス (Azure で構築実際のクラウド アプリケーション)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices)します。

## 次のステップ

チェック アウト、 [Azure Redis Cache 用の ASP.NET 出力キャッシュ プロバイダー](cache-asp.net-output-cache-provider.md)します。






