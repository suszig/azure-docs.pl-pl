<properties
    pageTitle="Storage の監視、診断、およびトラブルシューティング | Microsoft Azure"
    description="ストレージ分析、クライアント側ログ、他のサード パーティのツールなどの機能を使用して、Azure Storage 関連の問題を特定、診断、およびトラブルシューティングします。"
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/01/2015"
    ms.author="jahogg"/>

# Microsoft Azure Storage の監視、診断、およびトラブルシューティング

[AZURE.INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## 概要

クラウド環境でホストされる分散型アプリケーションの問題の診断およびトラブルシューティングは、従来の環境よりも複雑になる可能性があります。 アプリケーションは、PaaS や IaaS インフラストラクチャ、オンプレミス、モバイル デバイス、これらを組み合わせたものにデプロイできます。 一般に、アプリケーションのネットワーク トラフィックは公衆ネットワークと専用ネットワークを経由する可能性があり、アプリケーションでは、Microsoft Azure Storage のテーブル、BLOB、キュー、ファイルのほか、リレーショナル データベースやドキュメント データベースといった他のデータ ストアなどの複数のストレージ技術を使用している可能性があります。

このようなアプリケーションを適切に管理するためには、アプリケーションをプロアクティブに監視し、アプリケーションおよびアプリケーションが依存する技術をあらゆる側面から診断およびトラブルシューティングする方法を理解する必要があります。 Azure Storage サービスのユーザーは、(応答時間が通常より長いなどの) 予期しない動作変化を捉えるために、アプリケーションで使用している Storage サービスを継続して監視し、ログを使用して詳細なデータを収集し、問題を徹底的に分析する必要があります。 監視とログの両方から得られた診断情報を基に、アプリケーションで発生した問題の根本原因を特定できます。 そして、問題をトラブルシューティングし、問題を解決するための適切な手順を決定できます。 Azure Storage は Azure の中核サービスであり、顧客が Azure インフラストラクチャにデプロイするソリューションのほとんどでは、Azure Storage が重要な役割を担っています。 Azure Storage には、クラウド ベース アプリケーションのストレージの問題を簡単に監視、診断、およびトラブルシューティングできる機能が組み込まれています。

Azure のストレージ アプリケーションにおけるエンド ツー エンドのトラブルシューティングを実践的なガイド 』 を参照してください。 [エンド ツー エンドの Azure Storage のメトリックおよびログ、AzCopy、Message Analyzer を使用したトラブルシューティング](../storage-e2e-troubleshooting/)します。

+ [概要]
    + [本書の構成]
+ [ストレージ サービスの監視]
    + [サービス正常性の監視]
    + [容量監視]
    + [可用性監視]
    + [パフォーマンス監視]
+ [ストレージ問題の診断]
    + [サービス正常性の問題]
    + [パフォーマンスの問題]
    + [エラーの診断]
    + [ストレージ エミュレーターの問題]
    + [ストレージ ログ ツール]
    + [ネットワーク ログ ツールの使用]
+ [エンド ツー エンド トレース]
    + [ログ データの関連付け]
    + [クライアント要求 ID]
    + [サーバー要求 ID]
    + [タイムスタンプ]
+ [トラブルシューティング ガイダンス]
    + [メトリックが示す AverageE2ELatency が高く、AverageServerLatency が低い]
    + [メトリックでは AverageE2ELatency も AverageServerLatency も低いのにクライアントで大きな遅延が発生している]
    + [メトリックが高い AverageServerLatency を示す]
    + [キューのメッセージ配信で予期しない遅延が発生する]
    + [メトリックが PercentThrottlingError の増加を示す]
    + [メトリックが PercentTimeoutError の増加を示す]
    + [メトリックが PercentNetworkError の増加を示す]
    + [クライアントが HTTP 403 (許可されていません) のメッセージを受け取る]
    + [クライアントが HTTP 404 (見つからない) のメッセージを受け取る]
    + [クライアントが HTTP 409 (競合) のメッセージを受け取る]
    + [メトリックの示す PercentSuccess が低い、または分析ログ エントリの中にトランザクション ステータスが ClientOtherErrors の操作がある]
    + [ストレージ使用量の予期しない増加が容量メトリックに示される]
    + [多数の VHD が接続されている仮想マシンが予期せず再起動する]
    + [開発またはテストでのストレージ エミュレーターの使用に起因する問題]
    + [Azure SDK for .NET のインストールで問題が発生する]
    + [ストレージ サービスで別の問題が発生する]
+ [付録]
    + [付録 1: Fiddler を使用した HTTP および HTTPS トラフィックのキャプチャ]
    + [付録 2: Wireshark を使用したネットワーク トラフィックのキャプチャ]
    + [付録 3: Microsoft Message Analyzer を使用したネットワーク トラフィックのキャプチャ]
    + [付録 4: Excel を使用したメトリックおよびログ データの表示]
    + [付録 5: Application Insights for Visual Studio Team Services を使用した監視]

## <a name="introduction"></a>はじめに

このガイドでは、Azure Storage Analytics、Azure ストレージ クライアント ライブラリのクライアント側ログ、他のサード パーティのツールなどの機能を使用して、Azure Storage 関連の問題を識別、診断、およびトラブルシューティングする方法を説明します。

![][1]

*図 1 監視、診断、およびトラブルシューティング*

このガイドの主な対象読者は、Azure ストレージ サービスを使用するオンライン サービスの開発者と、そのようなオンライン サービスを管理する IT プロフェッショナルです。 このガイドの目的を次に示します。

- Azure ストレージ アカウントの正常性およびパフォーマンスを維持できるようにする。
- アプリケーションの問題が Azure Storage に関するものかどうかを判断するために必要なプロセスおよびツールを示す。
- Azure Storage に関する問題を解決するための実用的なガイダンスを提供する。

### <a name="how-this-guide-is-organized"></a>本書の構成

セクションでは、"[Monitoring your storage service]"正常性と Azure Storage Analytics Metrics (ストレージ メトリック) を使用して、Azure ストレージ サービスのパフォーマンスを監視する方法について説明します。

セクションでは、"[Diagnosing storage issues]"Azure Storage Analytics Logging (ストレージ ログ) を使用して問題を診断する方法について説明します。 Storage Client Library for .NET や Azure SDK for Java などのクライアント ライブラリの機能を使用して、クライアント側のログを有効にする方法についても説明します。

セクションでは、"[End-to-end tracing]"さまざまなログ ファイルとメトリック データに含まれる情報を関連付ける方法について説明します。

セクションでは、"[Troubleshooting guidance]"生じるストレージ関連の一般的な問題のトラブルシューティングの指針を提供します。

"[Appendices]"Wireshark や Netmon など他のツールを使用してネットワークを分析する HTTP または HTTPS メッセージを分析する Fiddler、パケット データを関連付ける Microsoft Message Analyzer はログ データに関する情報が追加されます。


## <a name="monitoring-your-storage-service"></a>ストレージ サービスの監視

Windows のパフォーマンス監視に詳しい人は、Storage メトリックのことを Windows パフォーマンス モニターのカウンターに相当する Azure Storage の機能だと考えることができます。 ストレージ メトリックでは、サービスの可用性、サービスに対する要求の合計数、またはサービスに対する要求の成功のパーセンテージなど包括的な一連のメトリック (Windows パフォーマンス モニターの用語で言えばカウンター) を検索します (使用可能なメトリックの完全な一覧を参照してください <a href="http://msdn.microsoft.com/library/azure/hh343264.aspx" target="_blank">Storage Analytics Metrics のテーブル スキーマ</a> MSDN)。 Storage サービスでメトリックを収集および集計する間隔は、1 時間または 1 分を指定できます。 メトリックを有効にし、ストレージ アカウントを監視する方法の詳細については、次を参照してください。 <a href="http://go.microsoft.com/fwlink/?LinkId=510865" target="_blank">ストレージ メトリックを有効にします。</a> 参照してください。

表示する時間単位のメトリックを選択できます、 [Azure ポータル](portal.azure.com) 、時間単位メトリックが特定のしきい値を超えたときに電子メールで管理者に通知するルールを構成して (詳細については、ページを参照してください。 <a href="http://msdn.microsoft.com/library/azure/dn306638.aspx" target="_blank">する方法: アラートの通知を受信し、Azure でアラート ルールの管理</a>)。 Storage サービスは、最大限メトリックを収集しますが、すべてのストレージ操作を記録するわけではありません。

Azure ポータルでは、ストレージ アカウントの可用性、要求の総数、平均待機時間などのメトリックを表示できます。 可用性が特定のレベルを下回った場合に管理者にアラートを送信する通知ルールもセットアップされています。 このデータを表示するには、調査のための 1 つの可能な領域にはテーブル サービスの成功率が 100% 未満が (詳細については、セクションを参照してください。"[Metrics show low PercentSuccess or analytics log entries have operations with transaction status of ClientOtherErrors]") です。

次の方法により、Azure アプリケーションを継続して監視し、アプリケーションが正常であり、予期されるパフォーマンスが実現されていることを確認する必要があります。

- アプリケーションのメトリックの基準値を決定します。こうしておくと、現行データと比較して Azure Storage およびアプリケーションの動作の重大な変化を見つけることができます。 メトリックの基準値はアプリケーションごとに異なることが多いため、アプリケーションのパフォーマンス テストを実行する際に決定する必要があります。
- 分単位のメトリックを記録し、それらを使用して、エラーの数や要求レートが急増するなどの予期しないエラーや異常がないかどうか積極的に監視します。
- 時間単位のメトリックを記録し、それらを使用して、エラーの数や要求レートの平均などの平均値を監視します。
- セクションで後で説明したように診断ツールを使用して潜在的な問題を調査して"[Diagnosing storage issues]."

下の図 3 のグラフは、時間単位メトリックでなされる平均値の算出によってアクティビティの急増が隠れてしまう状況を示しています。 時間単位メトリックには安定した要求レートが示されますが、分単位メトリックから、実際は変動していることが分かります。

![][3]

このセクションの残りの部分では、監視する必要があるメトリックとその理由について説明します。

### <a name="monitoring-service-health"></a>サービス正常性の監視

使用することができます、 [Azure ポータル](portal.azure.com) 、世界中のすべての Azure リージョンにストレージ サービス (および他の Azure サービス) のヘルスを確認します。 これにより、アプリケーションで使用しているリージョンの Storage サービスに制御不能な問題による影響が発生していないかどうか、素早く確認できます。

 [Azure ポータル](portal.azure.com) さまざまな Azure サービスに影響を与えるアクシデントについての通知こともできます。
注: この情報は、以前は Azure サービス ダッシュボード (<a href="http://status.azure.com" target="_blank">http://status.azure.com</a>) に履歴データと共に提供されていました。

中に、 [Azure ポータル](portal.azure.com) 正常性情報が収集から (徹底解剖監視) の Azure データ センター内部でしたすることも、Azure でホストされる web アプリケーションを複数の場所から定期的にアクセスする疑似トランザクションを生成する外部のアプローチを採用します。 このような外部から監視する方法の例としては、<a href="http://www.keynote.com/solutions/monitoring/web-monitoring" target="_blank">Keynote</a>、<a href="https://www.gomeznetworks.com/?g=1" target="_blank">Gomez</a>、Application Insights for Visual Studio Team Services が提供しているサービスがあります。 Visual Studio Team Services の詳細については、Application Insights は、「付録」を参照してください"[Appendix 5: Monitoring with Application Insights for Visual Studio Team Services]。"。

### <a name="monitoring-capacity"></a>容量監視

ストレージ メトリックは、一般に保管データの大部分を BLOB が占めるため、BLOB サービスの容量メトリックのみを保管します (現時点では、ストレージ メトリックを使用してテーブルおよびキューの容量を監視することはできません)。 このデータを検索することができます、 **$MetricsCapacityBlob** テーブル、Blob サービスの監視を有効にした場合。 ストレージ メトリックは、1 日に 1 回は、このデータを記録しの値を使用して、 **RowKey** 行にユーザー データに関連するエンティティが含まれるかどうかを判断 (値 **データ**) または分析データ (値 **analytics**)。 格納された各エンティティには、ストレージの使用量に関する情報が含まれています (**容量** バイト単位) とコンテナーの現在数 (**ContainerCount**) および blob (**ObjectCount**) ストレージ アカウントで使用します。 格納される容量メトリックの詳細については、 **$MetricsCapacityBlob** 表を参照してください <a href="http://msdn.microsoft.com/library/azure/hh343264.aspx" target="_blank">Storage Analytics Metrics のテーブル スキーマ</a> MSDN にします。

> [AZURE.NOTE] ストレージ アカウントの容量の限度に近づいていることを示す早期警告について、その値を監視する必要があります。 Azure ポータルで、ストレージの合計使用量が指定のしきい値を超えた場合または下回った場合に通知するアラート ルールを追加できます。

Blob などのさまざまなストレージ オブジェクトのサイズを推定する方法、ブログの投稿を参照してください。 <a href="http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx" target="_blank">Azure Storage の課金について - 帯域幅、トランザクション、容量 (ブログの投稿)</a>.

### <a name="monitoring-availability"></a>可用性監視

値を監視することによって、ストレージ アカウントに、ストレージ サービスの可用性を監視する必要があります、 **可用性** 1 時間または分単位のメトリック テーブル内の列、 **$MetricsHourPrimaryTransactionsBlob**, 、**$MetricsHourPrimaryTransactionsTable**, 、**$MetricsHourPrimaryTransactionsQueue**, 、**$MetricsMinutePrimaryTransactionsBlob**, 、**$MetricsMinutePrimaryTransactionsTable**, 、**$MetricsMinutePrimaryTransactionsQueue**, 、**$MetricsCapacityBlob**します。  **可用性** 列には、サービスや、行によって表される API 操作の可用性を示すパーセント値が含まれています (、 **RowKey** 行に、サービス全体または特定の API 操作のメトリックが含まれるかどうかを示しています)。

100% より低い値は、一部のストレージ要求が失敗したことを示します。 などのさまざまなエラーの種類の要求の数を示すメトリック データの他の列を確認する失敗した、理由を参照してください **ServerTimeoutError**します。 表示される場合があります **可用性** が一時的に 100%、サービスの中に一時的なサーバー タイムアウトなどの理由を切るがパーティションを移動の負荷要求を向上させるためには、クライアント アプリケーションで再試行ロジックは、このような断続的な条件を処理する必要があります。 ページ <a href="http://msdn.microsoft.com/library/azure/hh343260.aspx" target="_blank"></a> でストレージ メトリックを含むトランザクションの種類を一覧表示、 **可用性** 計算します。

 [Azure ポータル](portal.azure.com), 場合に、通知するアラートの規則を追加する **可用性** サービスが指定したしきい値を下回るためです。

"[Troubleshooting guidance]"このガイドのセクションでは、可用性に関連する一般的な記憶域サービス問題を記述します。

### <a name="monitoring-performance"></a>パフォーマンス監視

Storage サービスのパフォーマンスを監視するには、時間単位および分単位のメトリック テーブルにある次のメトリックを使用します。

- 値、 **AverageE2ELatency** と **AverageServerLatency** 列を表示する平均時間、ストレージ サービスまたは API 操作タイプが要求の処理に要したします。 **AverageE2ELatency** 要求の読み取りや、要求の処理にかかった時間に加えて、応答の送信にかかる時間を含む、エンド ツー エンドの待機時間の測定値は、(したがってが指定されているネットワークの待機時間に達すると、要求、ストレージ サービス) **AverageServerLatency** 、処理時間のみの測定値は、そのため、クライアントとの通信に関連するネットワーク接続に遅延を除外します。 参照してください"[Metrics show high AverageE2ELatency and low AverageServerLatency]"理由については、このガイドの後半である可能性がありますこれら 2 つの値が大きく異なる。
- 値、 **TotalIngress** と **TotalEgress** で受信されると、storage サービスまたは特定の API 操作タイプの列では、データの総量を示します (バイト単位)。
- 値、 **TotalRequests** 列は、API 操作の storage サービスが受信した要求の総数を表示します。 **TotalRequests** 記憶域サービスが受信した要求の合計数です。

一般には、調査が必要な問題が存在することを示すインジケーターとして、これらの値のいずれかに予期しない変化が生じていないか監視します。

 [Azure ポータル](portal.azure.com), 、指定したしきい値を超えているこのサービスのパフォーマンス メトリックのいずれかまたは下回った場合に通知するアラートの規則を追加することができます。

"[Troubleshooting guidance]"このガイドのセクションでは、パフォーマンスに関連する一般的な記憶域サービス問題を記述します。


## <a name="diagnosing-storage-issues"></a>ストレージ問題の診断

アプリケーションの問題を認識するようになる状況には次のようなものがあります。

- 大きな障害によってアプリケーションがクラッシュする、またはアプリケーションの実行が停止する。
- 前のセクションで説明されている監視するメトリックの基準値を大きく変える"[Monitoring your storage service]."
- アプリケーションのユーザーから、特定の操作が予期したとおりに完了しなかった、または、特定の機能が正常に使用できないとの報告を受ける。
- アプリケーション内部でエラーが発生し、ログ ファイルに記録される、または、他の何らかの通知方式を介して明らかになる。

一般に、Azure Storage サービスに関する問題は、大まかに次の 4 つのカテゴリに分けられます。

- アプリケーションのパフォーマンスに問題があり、ユーザーから報告されるか、パフォーマンス メトリックの変化から明らかになる。
- 1 つ以上のリージョンの Azure Storage インフラストラクチャに問題がある。
- アプリケーションでエラーが発生し、ユーザーから報告されるか、監視対象のエラー数メトリックのいずれかが増加して明らかになる。
- 開発およびテストでローカルのストレージ エミュレーターを使用しているときに、ストレージ エミュレーターの使用に特に関係する問題が発生する。

次のセクションでは、これらの 4 つのカテゴリのそれぞれの問題を診断およびトラブルシューティングするときに従う必要のある手順を大まかに説明します。 セクションでは、"[Troubleshooting guidance]"このガイドで後で発生する一般的な問題について詳しく説明しています。

### <a name="service-health-issues"></a>サービス正常性の問題

一般に、サービス正常性の問題は制御不能です。  [Azure ポータル](portal.azure.com) ストレージ サービスを含む Azure サービスに継続的な問題に関する情報を提供します。 ストレージ アカウントの作成時に読み取りアクセス geo 冗長ストレージを選択していた場合は、プライマリ ロケーションのデータが使用できなくなったときに、アプリケーションはセカンダリ ロケーションの読み取り専用コピーに一時的に切り替えることができます。 そのためには、アプリケーションが、プライマリとセカンダリのストレージ ロケーションの間で切り替えを実行し、制限された機能モードで読み取り専用データを使用して動作できる必要があります。 Azure ストレージ クライアント ライブラリを使用して、プライマリ ストレージから読み取れなくなった場合にセカンダリ ストレージから読み取れるようにする再試行ポリシーを定義できます。 また、アプリケーションは、セカンダリ ロケーションのデータが実際に一貫性のあるデータかどうかを認識できる必要もあります。 詳細については、<a href="http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/04/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx" target="_blank">Azure Storage の冗長オプションおよび読み取りアクセス地理冗長ストレージ</a>に関するブログ記事を参照してください。

### <a name="performance-issues"></a>パフォーマンスの問題

アプリケーションのパフォーマンスは主観的であり、特にユーザーの見方によって変わることがあります。 そのため、パフォーマンスに問題がある可能性がある場所を特定できるように、メトリックの基準値を設けておくことが重要です。 クライアント アプリケーションの観点から見ると、Azure Storage サービスのパフォーマンスはさまざまな要因によって影響を受ける可能性があります。 このような要因には Storage サービス、クライアント、ネットワーク インフラストラクチャに関するものがあります。したがって、パフォーマンス問題の根本原因を特定するための戦略を用意しておくことが重要です。

メトリックを使用してパフォーマンス問題の原因と考えられる場所を特定したら、ログ ファイルを使用して詳細な情報を見つけ、問題をさらに診断してトラブルシューティングすることができます。

セクションでは、"[Troubleshooting guidance]"の詳細については、一般的なパフォーマンス関連の問題をこのガイドで後述の説明があります。

### <a name="diagnosing-errors"></a>エラーの診断

アプリケーションのユーザーから、クライアント アプリケーションの報告したエラーについて通知されることがあります。 ストレージ メトリックのストレージ サービスから別のエラーの種類の数はなど記録も **NetworkError**, 、**ClientTimeoutError**, 、または **AuthorizationError**します。 ストレージ メトリックにはエラーの種類ごとの数しか記録されませんが、サーバー側のログ、クライアント側のログ、およびネットワーク ログを調べれば個々の要求に関する詳細を取得できます。 一般に、要求が失敗した理由は、Storage サービスから返される HTTP 状態コードに示されます。

> [AZURE.NOTE] 断続的なエラーが発生するされる必要があります。 一時的なネットワーク状態によるエラーやアプリケーション エラーなどです。

次の MSDN のリソースは、ストレージ関連の状態コードおよびエラー コードの理解に役立ちます。

- <a href="http://msdn.microsoft.com/library/azure/dd179357.aspx" target="_blank">REST API の一般的なエラー コード</a>
- <a href="http://msdn.microsoft.com/library/azure/dd179439.aspx" target="_blank">BLOB サービスのエラー コード</a>
- <a href="http://msdn.microsoft.com/library/azure/dd179446.aspx" target="_blank">キュー サービスのエラー コード</a>
- <a href="http://msdn.microsoft.com/library/azure/dd179438.aspx" target="_blank">テーブル サービスのエラー コード</a>

### <a name="storage-emulator-issues"></a>ストレージ エミュレーターの問題

Azure SDK には、開発用ワークステーションで実行できるストレージ エミュレーターが含まれています。 このエミュレーターは、Azure Storage サービスのほとんどの動作をシミュレートし、Azure サブスクリプションと Azure ストレージ アカウントがなくても Azure Storage サービスを使用するアプリケーションを実行できるため、開発やテストで役に立ちます。

"[Troubleshooting guidance]"このガイドのセクションでは、ストレージ エミュレーターの使用時に発生する一般的な問題を記述します。

### <a name="storage-logging-tools"></a>ストレージ ログ ツール

ストレージ ログによって、Azure ストレージ アカウントのストレージ要求をサーバー側でログに記録できます。 サーバー側のログ記録を有効にして、ログ データにアクセスする方法の詳細については、次を参照してください。 <a href="http://go.microsoft.com/fwlink/?LinkId=510867" target="_blank">サーバー側のログ記録の使用</a> 参照してください。

Storage Client Library for .NET では、アプリケーションで実行されたストレージ操作に関するクライアント側のログ データを収集できます。 クライアント側のログ記録を有効にして、ログ データにアクセスする方法の詳細については、次を参照してください。 <a href="http://go.microsoft.com/fwlink/?LinkId=510868" target="_blank">ストレージ クライアント ライブラリを使用してクライアント側のログ</a> 参照してください。

> [AZURE.NOTE] (SAS の許可エラー) などのいくつかの状況で、ユーザーは対象が見つからない要求データ、サーバー側ストレージ ログにエラーをレポートします。 ストレージ クライアント ライブラリのログ機能を使用して問題の原因がクライアント側にあるか調べたり、ネットワーク監視ツールを使用してネットワークを調べたりできます。

### <a name="using-network-logging-tools"></a>ネットワークのログ記録ツールの使用

クライアントとサーバーの間のトラフィックをキャプチャして、クライアントとサーバーの間で交換されたデータおよび基礎ネットワークの状態に関する詳細な情報を取得できます。 便利なネットワーク ログ ツールには次のようなものがあります。

- Fiddler (<a href="http://www.telerik.com/fiddler" target="_blank">http://www.telerik.com/fiddler</a>) は、無料の Web デバッグ プロキシです。HTTP および HTTPS の要求および応答メッセージのヘッダーおよびペイロード データを調べることができます。 詳細については、次を参照してください。"[Appendix 1: Using Fiddler to capture HTTP and HTTPS traffic]"です。
- Microsoft Network Monitor (Netmon) (<a href="http://www.microsoft.com/download/details.aspx?id=4865" target="_blank">http://www.microsoft.com/download/details.aspx?id=4865</a>) および Wireshark (<a href="http://www.wireshark.org/" target="_blank">http://www.wireshark.org/</a>) は、無料のネットワーク プロトコル アナライザーです。さまざまな種類のネットワーク プロトコルの詳細なパケット情報を表示できます。 Wireshark の詳細については、次を参照してください。"[Appendix 2: Using Wireshark to capture network traffic]"です。
- Microsoft Message Analyzer は、Netmon の後継となる Microsoft のツールです。ネットワーク パケット データをキャプチャできるほか、他のツールでキャプチャされたログ データを表示および分析できます。 詳細については、次を参照してください。"[Appendix 3: Using Microsoft Message Analyzer to capture network traffic]"です。
- クライアント コンピューターは、ネットワーク経由で Azure ストレージ サービスに接続できることを確認する基本的な接続テストを実行する場合は、することはできません、標準を使用して **ping** クライアント上のツールです。 ただし、使用することができます、 **tcping** ツールの接続を確認します。 **Tcping** はダウンロード <a href="http://www.elifulkerson.com/projects/tcping.php" target="_blank">http://www.elifulkerson.com/projects/tcping.php</a>します。

多くの場合、問題の診断には ストレージ ログおよび ストレージ クライアント ライブラリのログ データで十分ですが、状況によっては、これらのネットワーク ログ ツールを使用して取得できる詳細な情報が必要になります。 たとえば、Fiddler を使用して HTTP および HTTPS メッセージを表示し、Storage サービスとの間で送受信されたヘッダーおよびペイロード データを確認して、クライアント アプリケーションがどのようにストレージ操作を再試行したのかを調べることができます。 Wireshark などのパケット レベルで動作するプロトコル アナライザーを使用して TCP データを表示し、失われたパケットや接続問題をトラブルシューティングすることができます。 Message Analyzer は HTTP 層と TCP 層の両方で動作可能です。

## <a name="end-to-end-tracing"></a>エンド ツー エンド トレース

各種ログ ファイルを使用したエンド ツー エンド トレースは、潜在的な問題を調査するための有用な手法です。 メトリック データからの日時情報を使用すると、問題のトラブルシューティングに役立つ詳細な情報についてログ ファイルを調べるときにどこから開始するとよいかが分かります。

### <a name="correlating-log-data"></a>ログ データの関連付けを行う

クライアント アプリケーション、ネットワーク トレース、およびサーバー側ストレージ ログからのログを表示する際、各種ログ ファイルの要求を相関付ける機能が重要になります。 ログ ファイルには、相関 ID として役立つ多数の各種フィールドが含まれます。 クライアント要求 ID は、各種ログのエントリを相関付けるために使用する最も役立つフィールドです。 ただし、サーバー要求 ID またはタイムスタンプのどちらかを使用すると役立つ場合もあります。 以下のシナリオは、こうしたオプションの詳細を示しています。

### <a name="client-request-id"></a>クライアント要求 ID

ストレージ クライアント ライブラリは、要求ごとに固有のクライアント要求 ID を自動生成します。

- クライアント要求 id は、ストレージ クライアント ライブラリを作成するクライアント側ログ、 **クライアント要求 ID** 要求に関連するすべてのログ エントリのフィールドです。
- Fiddler によってキャプチャされたなどのネットワーク トレースの場合、クライアント要求 id は要求メッセージに表示されている、 **x-ms-クライアントの要求の id** HTTP ヘッダー値。
- サーバー側の Storage Logging ログの場合、クライアント要求 ID は [クライアント要求 ID] 列に表示されます。

> [AZURE.NOTE] 複数の要求、クライアントは、この値を割り当てることができますので、同じクライアント要求 id を共有することは (ストレージ クライアント ライブラリが割り当てられますが、
> 自動的に割り当てます)。 クライアントから再試行する場合には、必ず同じクライアント要求 ID を共有します。 クライアントからの送信されるバッチの場合、バッチのクライアント要求 ID は 1 つだけです。


### <a name="server-request-id"></a>サーバー要求 ID

Storage サービスにより、サーバー要求 ID が自動生成されます。

- サーバー側の Storage Logging ログで、サーバー要求 id が表示されます、 **要求 ID ヘッダー** 列です。
- 応答メッセージになど Fiddler によってキャプチャされたネットワーク トレースの場合で、サーバー要求 id が表示されます、 **x ms 要求 id** HTTP ヘッダー値。
- ストレージ クライアント ライブラリを作成するクライアント側ログに表示されます、サーバー要求 id、 **操作テキスト** サーバー応答の詳細を示すログ エントリの列です。

> [AZURE.NOTE] 記憶域サービス常に一意のサーバー要求 id を割り当て、受け取るすべての要求、クライアントからのすべての再試行およびバッチに含まれるすべての操作に固有のサーバー要求 id があるためです。

ストレージ クライアント ライブラリをスローした場合、 **StorageException** クライアントで、 **RequestInformation** プロパティを含む、 **RequestResult** を含むオブジェクトを **ServiceRequestID** プロパティです。 アクセスすることも、 **RequestResult** オブジェクトから、 **OperationContext** インスタンス。

次のコード例は、ユーザー設定を設定する方法を示します **ClientRequestId** アタッチすることによって値、 **OperationContext** オブジェクトをストレージ サービスを要求します。 取得する方法も示しています、 **ServerRequestId** 応答メッセージからの値。

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
    OperationContext oc = new OperationContext();
    oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

    try
    {
        CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
        ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
        var downloadToPath = string.Format("./{0}", blob.Name);
        using (var fs = File.OpenWrite(downloadToPath))
        {
            blob.DownloadToStream(fs, null, null, oc);
            Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
        }
    }
    catch (StorageException storageException)
    {
        Console.WriteLine("Storage exception {0} occurred", storageException.Message);
        // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
        foreach (var result in oc.RequestResults)
        {
                Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
        }
    }


### <a name="timestamps"></a>タイムスタンプ

またタイムスタンプを使用して、関連するログ エントリを見つけることもできます。ただし、クライアントとサーバー間で存在する可能性があるクロック スキューに注意してください。 サーバー側の項目は、クライアントのタイムスタンプを基準として前後に 15 分ずつずらして一致項目を検索する必要があります。 メトリックが含まれる BLOB 用の BLOB メタデータは、BLOB で格納されているメトリックの時間範囲を示しています。これは、同じ分数または時間で多くのメトリック BLOB が含まれる場合に役立ちます。

## <a name="troubleshooting-guidance"></a>トラブルシューティング ガイダンス

このセクションは、Azure Storage サービスを使用するときにご使用のアプリケーションで生じる可能性がある一般的な問題のいくつかを診断したりトラブルシューティングしたりするために役立ちます。 以下のリストを使用して、特定の問題に関連する情報を見つけてください。

**トラブルシューティングの決定木**

----------

問題は、ストレージ サービスのいずれかのパフォーマンスと関連がありますか。

- [メトリックが示す AverageE2ELatency が高く、AverageServerLatency が低い]
- [メトリックでは AverageE2ELatency も AverageServerLatency も低いのにクライアントで大きな遅延が発生している]
- [メトリックが高い AverageServerLatency を示す]
- [キューのメッセージ配信で予期しない遅延が発生する]

----------

問題は、ストレージ サービスのいずれかの可用性と関連がありますか。

- [メトリックが PercentThrottlingError の増加を示す]
- [メトリックが PercentTimeoutError の増加を示す]
- [メトリックが PercentNetworkError の増加を示す]

----------

クライアント アプリケーションは、ストレージ サービスから HTTP 4XX (404 など) の応答を受信しましたか。

- [クライアントが HTTP 403 (許可されていません) のメッセージを受け取る]
- [クライアントが HTTP 404 (見つからない) のメッセージを受け取る]
- [クライアントが HTTP 409 (競合) のメッセージを受け取る]

----------

[メトリックの示す PercentSuccess が低い、または分析ログ エントリの中にトランザクション ステータスが ClientOtherErrors の操作がある]

----------

[ストレージ使用量の予期しない増加が容量メトリックに示される]

----------

[多数の VHD が接続されている仮想マシンが予期せず再起動する]

----------

[開発またはテストでのストレージ エミュレーターの使用に起因する問題]

----------

[Azure SDK for .NET のインストールで問題が発生する]

----------

[ストレージ サービスで別の問題が発生する]

----------

### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>メトリックが示す AverageE2ELatency が高く、AverageServerLatency が低い

次の図、 [Azure ポータル](portal.azure.com) 例を示しています監視ツール、 **AverageE2ELatency** はよりもかなり高く、 **AverageServerLatency**します。

![][4]

ストレージ サービスにのみ、メトリックが計算される注 **AverageE2ELatency** 正常な要求に関してとは異なり **AverageServerLatency**, 、クライアントがデータを送信し、ストレージ サービスから確認を受信するまでが含まれています。 違いではこのため、 **AverageE2ELatency** と **AverageServerLatency** 中、応答速度が遅い、クライアント アプリケーション、またはネットワーク上の状態による可能性があります。

> [AZURE.NOTE] 表示することもできます **E2ELatency** と **ServerLatency** Storage logging 個々 のストレージ操作のデータを記録します。

#### クライアントのパフォーマンス上の問題に関する調査

クライアントの応答が低下する理由として、使用可能な接続またはスレッドの数が制限されていること、または CPU、メモリ、ネットワーク帯域幅などのリソースが不足していることが考えられます。 この問題は、クライアント コードをより効率的に変更する (たとえば、Storage サービスに対する非同期呼び出しを使用する)、または (コアおよびメモリを増加させた) より大きな仮想マシンを使用するように変更すると、解決できる場合があります。

テーブルおよびキュー サービスの場合、Nagle アルゴリズムも可能性があります高 **AverageE2ELatency** と比較して **AverageServerLatency**: 詳細については、投稿を参照してください。 <a href="http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx" target="_blank">小さな要求に不親切な Nagle アルゴリズム</a> に関する Microsoft Azure Storage チーム ブログ。 使用してコードで Nagle アルゴリズムを無効にすることができます、 **ServicePointManager** クラス、 **System.Net** 名前空間。 この操作は、既に開かれている接続に対しては影響を及ぼさないため、アプリケーションの中で Table サービスまたは Queue サービスを呼び出す前に実行する必要があります。 次の例は、 **Application_Start** worker ロールでのメソッドです。

    var storageAccount = CloudStorageAccount.Parse(connStr);
    ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
    tableServicePoint.UseNagleAlgorithm = false;
    ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
    queueServicePoint.UseNagleAlgorithm = false;

クライアント アプリケーションを送信する要求の数を表示するクライアント側のログおよび一般的な .NET の確認は、CPU、.NET ガベージ コレクション、ネットワーク使用率、メモリなど、クライアント パフォーマンスのボトルネックを関連するかを確認する必要があります (.NET クライアント アプリケーションのトラブルシューティングの出発点として参照してください <a href="http://msdn.microsoft.com/library/7fe0dd2y(v=vs.110).aspx" target="_blank">デバッグ、トレース、およびプロファイリング</a> MSDN)。

#### ネットワーク待ち時間に関する問題の調査

一般的に、エンド ツー エンドの待ち時間が長くなるのは、ネットワークの一時的な状態に起因します。 Wireshark や Microsoft Message Analyzer などのツールを使用して、ドロップされたパケットなど一時的および永続的なネットワークの問題を調査できます。

Wireshark を使用して、ネットワークの問題をトラブルシューティングする方法の詳細については、次を参照してください"[Appendix 2: Using Wireshark to capture network traffic]。"。

Microsoft Message Analyzer を使用して、ネットワークの問題をトラブルシューティングする方法の詳細については、次を参照してください"[Appendix 3: Using Microsoft Message Analyzer to capture network traffic]。"。

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>メトリックでは AverageE2ELatency も AverageServerLatency も低いのにクライアントで大きな遅延が発生している

このシナリオの場合、最も可能性が高い原因は、Storage サービスに到達するストレージ要求の遅延です。 クライアントの要求が BLOB サービスにうまく送られない原因を調査する必要があります。

要求送信でクライアントに遅延が生じる理由の 1 つとして、使用可能な接続またはスレッドの数が制限されていることが考えられます。 

クライアントが複数回再試行したかどうかを確認し、試行している場合はその理由を調査する必要があります。 クライアントが複数回再試行しているかどうかを確認するには、以下を実行できます。

- Storage Analytics のログを調べます。 再試行が複数回発生している場合は、クライアント要求 ID が同じでサーバー要求 ID が異なる複数の操作があります。
- クライアントのログを調べます。 詳細ログに、再試行が発生したことが示されます。
- コードをデバッグし、プロパティのチェック、 **OperationContext** 要求に関連付けられているオブジェクト。 操作が再試行された場合、 **RequestResults** プロパティには、複数の一意のサーバー要求 Id が含まれます。 各要求の開始時刻と終了時刻をチェックすることもできます。 詳細については、セクションのサンプル コードを参照してください。 [Server request ID]します。 

クライアントに問題がない場合、パケット損失などの考えられるネットワーク問題について調査してください。 Wireshark または Microsoft Message Analyzer などのツールを使用して、ネットワーク問題を調査できます。

Wireshark を使用して、ネットワークの問題をトラブルシューティングする方法の詳細については、次を参照してください"[Appendix 2: Using Wireshark to capture network traffic]。"。

Microsoft Message Analyzer を使用して、ネットワークの問題をトラブルシューティングする方法の詳細については、次を参照してください"[Appendix 3: Using Microsoft Message Analyzer to capture network traffic]。"。

### <a name="metrics-show-high-AverageServerLatency"></a>メトリックが高い AverageServerLatency を示す

高い場合 **AverageServerLatency** blob ダウンロード要求が同じ blob (または blob セット) を繰り返し要求を確認する、Storage Logging ログを使用する必要があります。 BLOB アップロード要求に関しては、クライアントが使用しているブロック サイズ (たとえば、サイズが 64 K を下回るブロックの場合、読み取りも 64 K 未満のチャンクでない限りはオーバーヘッドが生じる可能性があります)、および同じ BLOB に対して複数のクライアントがブロックを同時にアップロードしていないかどうかを調べてください。 超える要求の数が急増毎分のメトリックを確認することも必要があります、毎秒のスケーラビリティ ターゲット: も参照してください"[Metrics show an increase in PercentTimeoutError]."

高値が生じる場合 **AverageServerLatency** 同一の blob または blob の blob ダウンロード要求が繰り返される場合を要求し、Azure キャッシュまたは Azure コンテンツ配信ネットワーク (CDN) を使用してこうした blob をキャッシュする必要があります。 アップロード要求に関しては、ブロック サイズを大きくすることによってスループットを改善できます。 テーブルに対するクエリの場合、同じクエリ操作を実行し、データが頻繁に変更されないクライアントにクライアント側のキャッシュを実装することもできます。

高 **AverageServerLatency** 値は設計が不十分なテーブルやクエリ、またはスキャン操作の結果、前後にアンチ パターンが発生する可能性があります。 参照してください"[Metrics show an increase in PercentThrottlingError]"の詳細。

> [AZURE.NOTE] ここで留意するその他の問題を含む包括的なチェックリストを見つけることができます: [Microsoft Azure Storage のパフォーマンスとスケーラビリティに対するチェックリスト](storage-performance-checklist.md)します。

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>キューのメッセージ配信で予期しない遅延が発生する

アプリケーションがメッセージをキューに追加した時刻とキューからの読み取りが可能になる時刻の間に遅延が生じる場合、以下の手順で問題を診断する必要があります。

- アプリケーションにより、メッセージがキューに正常に追加されていることを確認します。 アプリケーションが再試行していないことを確認、 **AddMessage** メソッドが成功するまで何回か。 ストレージ クライアント ライブラリ ログには、ストレージ操作の再試行の反復が表示されます。
- キューにメッセージを追加した worker ロールと、処理に遅延があるかどうかを表示するキューからのメッセージを読み取る worker ロール間でクロック スキューがないことを検証します。
- キューからのメッセージを読み取る worker ロールに障害がないことを確かめます。 キュー クライアントが呼び出す場合、 **GetMessage** 、メッセージが不可視まではキューに残っているメソッドが、受信確認応答を **invisibilityTimeout** 期間が終了します。 有効期限が切れた時点で、メッセージは再び処理可能になります。
- キューの長さが時間の経過とともに大きくなっているかどうかを確認してください。 他のワーカーがキューに配置したすべてのメッセージを処理するのに十分な数のワーカーを使用できない場合に、大きくなる可能性があります。 また、要求の削除が失敗したかどうかを示すメトリック、およびメッセージのデキュー カウントを示すメトリックを調べてください。こうしたメトリックは、メッセージを削除する試行が何度も繰り返して失敗したことを示す場合があります。
- 予想よりも高くなっているキュー操作の Storage Logging ログを調べて **E2ELatency** と **ServerLatency** 通常より長い期間にわたって値。


### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>メトリックが PercentThrottlingError の増加を示す

Storage サービスのスケーラビリティ ターゲットを超えると、調整エラーが生じます。 このようにして Storage サービスは、1 つのクライアントやテナントがサービスを占有してしまうことがないようにします。 詳細については、次をご覧ください。 <a href="http://msdn.microsoft.com/library/azure/dn249410.aspx" target="_blank">Azure Storage のスケーラビリティおよびパフォーマンスのターゲット</a> ストレージ アカウントのスケーラビリティ ターゲットとストレージ アカウントのパーティションのパフォーマンスのターゲットの詳細についてです。

場合、 **PercentThrottlingError** 増加、スロット リング エラーで失敗した要求の割合で示されるメトリック、2 つのシナリオのいずれかを調査する必要があります。

- [PercentThrottlingError の一時的増加]
- [PercentThrottlingError エラーの永続的増加]

増加する **PercentThrottlingError** 多くの場合、ストレージ要求の数の増加すると同時に発生するか、最初にしたら、アプリケーションのテストをロードします。 また、ストレージ操作により、「503 サーバーはビジー状態」または「500 操作タイムアウト」HTTP ステータス メッセージがクライアントで示されることもあります。

#### <a name="transient-increase-in-PercentThrottlingError"></a>PercentThrottlingError の一時的増加

値が急増する場合 **PercentThrottlingError** アプリケーションの高度な処理の期間と一致する、クライアントで、指数 (非線形) バックオフの再試行戦略を実装する必要があります。 これは、パーティションにおける即座の負荷を軽減し、トラフィックの急増を除去するため、アプリケーションをのに役立ちます。 ストレージ クライアント ライブラリを使用して再試行ポリシーを実装する方法の詳細については、MSDN の「<a href="http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx" target="_blank">Microsoft.WindowsAzure.Storage.RetryPolicies Namespace</a>」を参照してください。

> [AZURE.NOTE] 値にもが急増することがあります **PercentThrottlingError** アプリケーションの高度な処理の期間で一致しない。 最も一般的な原因は、ストレージ サービスの負荷分散を向上させるためにパーティションを移動します。

#### <a name="permanent-increase-in-PercentThrottlingError"></a>PercentThrottlingError エラーの永続的増加

一貫して高い値を表示している場合 **PercentThrottlingError** 、アプリケーションのテストを次のトランザクションの量、または最初のロードを実行するときに、永続的増加し、アプリケーションがストレージ パーティションを使用する方法と、ストレージ アカウントのスケーラビリティ ターゲットに近づいているかどうかを評価する必要があります。 たとえば、(単一パーティションとしてカウントされる) キューで調整エラーが生じる場合、トランザクションを複数パーティションに分散するためにキューを追加することを考慮する必要があります。 テーブルで調整エラーが生じる場合、より広い範囲のパーティション キー値を使用して複数のパーティションにトランザクションを分散させるため、異なるパーティション スキームを使用することを考慮する必要があります。 この問題が生じる一般的な原因の 1 つは、日付をパーティション キーとして選択し、特定の日のすべてのデータを 1 つのパーティションに書き込むという、前後にあるアンチ パターンです。この場合、ロード中に、書き込みボトルネックとなるおそれがあります。 別のパーティション設計を考慮するか、Blob Storage を使用する方がよいソリューションとなるかどうかを考えてください。 また、トラフィックの急増が原因となって調整が生じているのか確認し、要求パターンを平滑化する方法を調べる必要もあります。

トランザクションを複数のパーティションに分散する場合であっても、ストレージ アカウントに設定されているスケーラビリティ限界に注意してください。 たとえば、10 個のキューを使用し、それぞれで毎秒最大で 2,000 メッセージ (各メッセージは 1 KB) を処理した場合、ストレージ アカウントの 1 秒あたり 20,000 メッセージという全体の限界に達します。 1 秒あたり 20,000 を超えるエンティティを処理する必要がある場合、複数のストレージ アカウントの使用を考慮しなければなりません。 また、要求とエンティティのサイズは、Storage サービスがクライアントを調整する時期に影響を与えることも考えておく必要があります。つまり、要求とエンティティが大きくなると、より早く調整される可能性があるのです。

クエリの設計が非効率的な場合も、テーブル パーティションのスケーラビリティ限界に達する原因となる可能性があります。 たとえば、1 つのパーティションのエンティティの 1% だけを選択するフィルターが指定されたクエリが、パーティション内のすべてのエンティティをスキャンする場合、各エンティティにアクセスする必要があります。 エンティティを読み取る操作それぞれが、パーティション内のトランザクション数合計に含まれるので、すぐにスケーラビリティ ターゲットに達してしまいます。

> [AZURE.NOTE] パフォーマンス テストと、アプリケーションにおけるな非効率的なクエリ設計が表示されます。

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>メトリックが PercentTimeoutError の増加を示す

メトリックが、表示が増加する **PercentTimeoutError** 記憶域サービスのいずれかです。 同時に、クライアントは、ストレージ操作により「500 操作タイムアウト」HTTP ステータス メッセージを大量に受け取ります。

> [AZURE.NOTE] パーティションを新しいサーバーに移動することにより残高要求の負荷を一時的に記憶域サービスとしてのタイムアウト エラーを参照してください可能性があります。

 **PercentTimeoutError** メトリックは、次のメトリックの集計: **ClientTimeoutError**, 、**AnonymousClientTimeoutError**, 、**SASClientTimeoutError**, 、**ServerTimeoutError**, 、**AnonymousServerTimeoutError**, 、および **SASServerTimeoutError**します。

サーバー タイムアウトは、サーバー上のエラーが原因で生じます。 クライアント タイムアウトが、サーバー上の操作は、クライアントによって指定されたタイムアウトを超えた使用して、ストレージ クライアント ライブラリを使用してクライアントが、操作のタイムアウト期間を設定するなど、 **ServerTimeout** のプロパティ、 **QueueRequestOptions** クラスです。

サーバー タイムアウトは、Storage サービスに問題があることを示します。この場合、さらに調査が必要です。 サービスのスケーラビリティ限界に達したかどうか示すメトリック、および問題の原因となる可能性があるトラフィックの急増を示すメトリックを使用できます。 問題が断続的な場合、サービスにおける負荷分散アクティビティが原因となっている可能性があります。 問題が永続的で、サービスのスケーラビリティ限界にアプリケーションが達したことが原因ではない場合には、サポートに問題を知らせてください。 クライアントのタイムアウトの場合、クライアントでタイムアウト値が適切に設定されているかどうかを判断し、クライアントで設定されているタイムアウト値を変更するか、Storage サービスにおける操作のパフォーマンスを改善する方法を調べてください。改善策としては、たとえば、テーブル クエリを最適化したり、メッセージのサイズを減らしたりできます。

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>メトリックが PercentNetworkError の増加を示す

メトリックが、表示が増加する **PercentNetworkError** 記憶域サービスのいずれかです。  **PercentNetworkError** メトリックは、次のメトリックの集計: **NetworkError**, 、**AnonymousNetworkError**, 、および **SASNetworkError**します。 このエラーは、クライアントがストレージ要求を出す際に Storage サービスによりネットワーク エラーが検出されると生じます。

多くの場合、このエラーの原因は、Storage サービスでタイムアウト期間が切れる前にクライアントが切断することです。 クライアントのコードを調べて、Storage サービスからクライアントが切断した理由とタイミングを把握してください。 また、Wireshark、Microsoft Message Analyzer、または Tcping を使用して、クライアントからネットワーク接続の問題を調査できます。 これらのツールは「、 [Appendices]します。

### <a name="the-client-is-receiving-403-messages"></a>クライアントが HTTP 403 (許可されていません) のメッセージを受け取る

クライアント アプリケーションが HTTP 403 (許可されていません) エラーをスローする場合、可能性の高い原因は、クライアントがストレージ要求を送信するときに期限切れの Shared Access Signature (SAS) を使用していることです (原因の他の可能性としては、クロック スキュー、無効なキー、空のヘッダーなどがあります)。 期限切れの SAS キーが原因の場合、サーバー側の Storage Logging ログ データのエントリが表示されません。 以下の表に、この問題が生じたときにストレージ クライアント ライブラリによって生成されるクライアント側のログのサンプルを示します。

ソース|詳細度|詳細度|クライアント要求 ID|操作テキスト
---|---|---|---|---
Microsoft.WindowsAzure.Storage|情報|3|85d077ab -…|場所 Primary、場所モード PrimaryOnly で操作を開始しています。
Microsoft.WindowsAzure.Storage|情報|3|85d077ab -…|Starting synchronous request to https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;sr=c&amp;si=mypolicy&amp;sig=OFnd4Rd7z01fIvh%2BmcR6zbudIH2F5Ikm%2FyhNYZEmJNQ%3D&amp;api-version=2014-02-14.
Microsoft.WindowsAzure.Storage|情報|3|85d077ab -…|応答を待機しています。
Microsoft.WindowsAzure.Storage|警告|2|85d077ab -…|応答の待機中に例外がスローされました。リモート サーバーがエラー「(403) 許可されていません」を返しました。
Microsoft.WindowsAzure.Storage|情報|3|85d077ab -…|応答を受け取りました。 状態コード = 403、要求 ID = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d、Content-MD5 = 、ETag = .
Microsoft.WindowsAzure.Storage|警告|2|85d077ab -…|操作中に例外がスローされました。リモート サーバーがエラー「(403) 許可されていません」を返しました。
Microsoft.WindowsAzure.Storage|情報|3 |85d077ab -…|操作を再試行する必要があるかどうかを検査しています。 再試行回数 = 0、HTTP 状態コード = 403、例外 = リモート サーバーがエラー「(403) 許可されていません」を返しました。
Microsoft.WindowsAzure.Storage|情報|3|85d077ab -…|次の場所が、場所モードに基づいてプライマリに設定されています。
Microsoft.WindowsAzure.Storage|エラー|1|85d077ab -…|再試行ポリシーは再試行を許可しませんでした。 リモート サーバーがエラー「(403) 許可されていません」を返して、失敗しました。

このシナリオでは、クライアントが SAS トークンをサーバーに送信する前にトークンが期限切れになった理由を調査しなければなりません。

- 一般に、クライアントがすぐに使用できるように SAS を作成する場合には開始時刻は設定しないでください。 現行時間を使用して SAS を生成するホストと、Storage サービスのクロックに小さな差異が存在すると、有効になっていない SAS を Storage サービスが受け取る可能性があります。
- SAS に非常に短い有効期限は設定しないでください。 やはり、SAS を生成するホストと Storage サービス間のクロックに小さい差異があると、予想されていたよりも明らかに早く SAS の有効期限が切れる恐れがあります。
- SAS キーのバージョン パラメーター (たとえば **sv = 2015年-04-05**) を使用しているストレージ クライアント ライブラリのバージョンに合わせてでしょうか。 最新バージョンを常に使用することをお勧めします [ストレージ クライアント ライブラリ](https://www.nuget.org/packages/WindowsAzure.Storage/)します。 SAS トークンのバージョン管理およびクライアント ライブラリ バージョンの依存関係の詳細については、[Microsoft Azure Storage の新機能](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/14/what-s-new-for-microsoft-azure-storage-at-teched-2014.aspx)に関するページを参照してください。
- ストレージ アクセス キーを再生成すると、既存の SAS トークンが無効になる可能性があります。 この状況は、SAS トークンを生成する際にクライアント アプリケーションがキャッシュする有効期限を長く設定していると問題となることがあります。

SAS トークンを生成するためのストレージ クライアント ライブラリを使用すると、有効なトークンの作成が容易です。 ただし、ストレージ REST API を使用して手動で SAS トークンを構築する場合は慎重にトピックを読む <a href="http://msdn.microsoft.com/library/azure/ee395415.aspx" target="_blank">Shared Access Signature を使用したアクセスの委任</a> 参照してください。

### <a name="the-client-is-receiving-404-messages"></a>クライアントが HTTP 404 (見つからない) のメッセージを受け取る
クライアント アプリケーションが HTTP 404 (未検出) メッセージをサーバーから受け取った場合は、クライアントが使用しようとしていたオブジェクト (エンティティ、テーブル、BLOB、コンテナー、キューなど) が Storage サービス内に存在しないことを意味します。 これには以下のようなさまざまな理由が考えられます。

- [クライアントまたは別のプロセスがそのオブジェクトを以前に削除した]
- [共有アクセス署名 (SAS) 認証の問題]
- [クライアント側の JavaScript コードにオブジェクトへのアクセス許可がない]
- [ネットワーク エラー]

#### <a name="client-previously-deleted-the-object"></a>クライアントまたは別のプロセスがそのオブジェクトを以前に削除した
クライアントが Storage サービスにあるデータの読み取り、更新、または削除を行おうとした場合であれば、通常、サーバー側のログで、問題のオブジェクトを Storage サービスから削除した過去の操作を簡単に特定できます。 多くの場合、ログ データに別のユーザーまたはプロセスがオブジェクトを削除したことが示されています。 ストレージ ログのサーバー側のログでは、operation-type 列および requested-object-key 列からクライアントがオブジェクトを削除したことが分かります。

クライアントがオブジェクトを挿入しようとした場合であれば、新規オブジェクトを作成するのに HTTP 404 (未検出) 応答が返される理由がすぐにはわからない可能性があります。 しかし、クライアントが BLOB を作成する場合は BLOB コンテナーを検出できなければなりませんし、メッセージを作成する場合はキューを検出できなければなりません。また、行を追加する場合はテーブルを検出できなければなりません。

ストレージ クライアント ライブラリのクライアント側ログを使用して、クライアントが Storage サービスに特定の要求を送ったときの状況を詳しく調べることができます。

ストレージ クライアント ライブラリによって生成された以下のクライアント側のログには、作成する BLOB 用のコンテナーをクライアントが検出できないという問題が示されています。 このログには、以下のストレージ操作の詳細が示されています。

要求 ID|操作
---|---
07b26a5d-...|**DeleteIfExists** メソッドは blob コンテナーを削除します。 この操作には、 **ヘッド** 、コンテナーの存在の確認を要求します。
e2d06d78…|**CreateIfNotExists** blob コンテナーを作成します。 この操作には、 **ヘッド** 要求、コンテナーの存在の有無をチェックします。  **ヘッド** 404 メッセージが返されますが、続行します。
de8b1c3c-...|**UploadFromStream** blob を作成します。  **配置** 要求が 404 メッセージで失敗

ログ エントリ:

要求 ID |  操作テキスト
---|---
07b26a5d-...|https://domemaildist.blob.core.windows.net/azuremmblobcontainer への同期要求を開始します。
07b26a5d-...|StringToSign = HEAD............x-ms-client-request-id:07b26a5d-....x-ms-date:Tue, 03 Jun 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-...|応答を待機しています。
07b26a5d-... | 応答を受け取りました。 状態コード 200、要求 ID = = eeead849…Content-md5 =、ETag = & quot; 0x8D14D2DC63D059B & quot;。
07b26a5d-... | 応答ヘッダーは正常に処理されました。残りの操作を処理しています。
07b26a5d-... | 応答の本文をダウンロードしています。
07b26a5d-... | 操作は正常に完了しました。
07b26a5d-... | https://domemaildist.blob.core.windows.net/azuremmblobcontainer への同期要求を開始します。
07b26a5d-... | StringToSign DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue、03 Jun 2014 を = 10時 33分: 12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container です。
07b26a5d-... | 応答を待機しています。
07b26a5d-... | 応答を受け取りました。 状態コード = 202、要求 ID = 6ab2a4cf-...、Content-MD5 = ETag = 。
07b26a5d-... | 応答ヘッダーは正常に処理されました。残りの操作を処理しています。
07b26a5d-... | 応答の本文をダウンロードしています。
07b26a5d-... | 操作は正常に完了しました。
e2d06d78-... | https://domemaildist.blob.core.windows.net/azuremmblobcontainer への非同期要求を開始します。</td>
e2d06d78-... | StringToSign = HEAD............x-ms-client-request-id:e2d06d78-....x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-...| 応答を待機しています。
de8b1c3c-... | https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt への同期要求を開始します。
de8b1c3c-... |  StringToSign = PUT...64.qCmF+TQLPhq/YYK50mP9ZQ==........x-ms-blob-type:BlockBlob.x-ms-client-request-id:de8b1c3c-....x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... | 要求データを書き込む準備をしています。
e2d06d78-... | 応答の待機中に例外がスローされました。リモート サーバーがエラー 404 (未検出) を返しました。
e2d06d78-... | 応答を受け取りました。 状態コード = 404、要求 ID = 353ae3bc-...、Content-MD5 =、ETag = 。
e2d06d78-... | 応答ヘッダーは正常に処理されました。残りの操作を処理しています。
e2d06d78-... | 応答の本文をダウンロードしています。
e2d06d78-... | 操作は正常に完了しました。
e2d06d78-... | https://domemaildist.blob.core.windows.net/azuremmblobcontainer への非同期要求を開始します。
e2d06d78-...|StringToSign = PUT...0.........x-ms-client-request-id:e2d06d78-....x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-... | 応答を待機しています。
de8b1c3c-... | 要求データを待機しています。
de8b1c3c-... | 応答を待機しています。
e2d06d78-... | 応答の待機中に例外がスローされました。リモート サーバーがエラー 409 (競合) を返しました。
e2d06d78-... | 応答を受け取りました。 状態コード = 409、要求 ID = c27da20e-...、Content-MD5 = 、ETag = 。
e2d06d78-... | エラー応答の本文をダウンロードしています。
de8b1c3c-... | 応答の待機中に例外がスローされました。リモート サーバーがエラー 404 (未検出) を返しました。
de8b1c3c-... | 応答を受け取りました。 状態コード = 404、要求 ID = 0eaeab3e-...、Content-MD5 = 、ETag = 。
de8b1c3c-...| 操作中に例外がスローされました。リモート サーバーがエラー 404 (未検出) を返しました。
de8b1c3c-... | 再試行ポリシーは再試行を許可しませんでした。 "リモート サーバーがエラーを返しました: (404) 未検出" で失敗しました。
e2d06d78-... | 再試行ポリシーは再試行を許可しませんでした。 "リモート サーバーがエラーを返しました: (409) 競合" で失敗しました。

この例では、ログにあるクライアントはからの要求、 **CreateIfNotExists** メソッド (要求 id e2d06d78…) からの要求を **UploadFromStream** メソッド (de8b1c3c 3c-...) は、クライアント アプリケーションでは、これらのメソッドを非同期的に呼び出してがあるために、これが発生します。 必ず、コンテナーを作成してからそのコンテナーの BLOB にデータをアップロードするように、クライアントの非同期コードを変更する必要があります。 すべてのコンテナーをあらかじめ作成しておくことをお勧めします。

#### <a name="SAS-authorization-issue"></a>共有アクセス署名 (SAS) 認証の問題

クライアント アプリケーションが、操作のために必要なアクセス許可が含まれていない SAS キーを使用しようとすると、Storage サービスは HTTP 404 (未検出) メッセージをクライアントに返します。 同時には、0 以外の値にも表示されます **SASAuthorizationError** メトリックです。

以下の表に、ストレージ ログのログ ファイルのサーバー側ログ メッセージの例を示します。

<table>
  <tr>
    <td>要求の開始時刻</td>
    <td>2014-05-30T06:17:48.4473697Z</td>
  </tr>
  <tr>
    <td>操作の種類</td>
    <td>GetBlobProperties</td>
  </tr>
  <tr>
    <td>要求の状態</td>
    <td>SASAuthorizationError</td>
  </tr>
  <tr>
    <td>HTTP 状態コード</td>
    <td>404</td>
  </tr>
  <tr>
    <td>認証の種類</td>
    <td>SAS</td>
  </tr>
  <tr>
    <td>サービスの種類</td>
    <td>BLOB</td>
  </tr>
  <tr>
    <td>要求 URL</td>
    <td>
    https://domemaildist.blob.core.windows.net/azureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;amp;sr=c&amp;amp;si=mypolicy&amp;amp;sig=XXXXX&amp;amp;api-version=2014-02-14&amp;amp;</td>
  </tr>
  <tr>
    <td>要求 ID ヘッダー</td>
    <td>a1f348d5-8032-4912-93ef-b393e5252a3b</td>
  </tr>
  <tr>
    <td>クライアント要求 ID</td>
    <td>2d064953-8436-4ee0-aa0c-65cb874f7929</td>
  </tr>
</table>

クライアント アプリケーションが、アクセス許可を付与されていない操作を実行しようとした理由を調べる必要があります。

#### <a name="JavaScript-code-does-not-have-permission"></a>クライアント側の JavaScript コードにオブジェクトへのアクセス許可がない

JavaScript クライアントを使用していて Storage サービスから HTTP 404 メッセージが返された場合は、以下の JavaScript エラーをブラウザーで確認する必要があります。

    SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
    SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.

> [AZURE.NOTE] Internet Explorer の F12 開発者ツールを使用してクライアント側の JavaScript の問題をトラブルシューティングするときに、ブラウザーとストレージ サービスの間で交換されるメッセージをトレースすることができます。

Web ブラウザーが実装されているために、これらのエラーが発生する、 <a href="http://www.w3.org/Security/wiki/Same_Origin_Policy" target="_blank">同じ生成元ポリシー</a> セキュリティの制限を web ページがページのドメインから別のドメインに API を呼び出すことを妨げるに由来します。

この JavaScript の問題を回避するには、クライアントがアクセスする Storage サービスにクロス オリジン リソース共有 (CORS) を構成します。 詳細については、次をご覧ください。 <a href="http://msdn.microsoft.com/library/azure/dn535601.aspx" target="_blank">クロス オリジン リソース共有 (CORS) Azure ストレージ サービスのサポート</a> 参照してください。

以下のコード サンプルは、Contoso ドメインで実行される JavaScript に対して Blob Storage サービスの BLOB へのアクセスを許可する BLOB サービスを構成する方法を示しています。

    CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
    // Set the service properties.
    ServiceProperties sp = client.GetServiceProperties();
    sp.DefaultServiceVersion = "2013-08-15";
    CorsRule cr = new CorsRule();
    cr.AllowedHeaders.Add("*");
    cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
    cr.AllowedOrigins.Add("http://www.contoso.com");
    cr.ExposedHeaders.Add("x-ms-*");
    cr.MaxAgeInSeconds = 5;
    sp.Cors.CorsRules.Clear();
    sp.Cors.CorsRules.Add(cr);
    client.SetServiceProperties(sp);

#### <a name="network-failure"></a>ネットワーク エラー

状況によっては、ネットワーク パケットの紛失が原因で、Storage サービスが HTTP 404 メッセージをクライアントに返すことがあります。 たとえば、クライアント アプリケーションがエンティティを Table サービスから削除するときに、クライアントが、Table サービスからの "HTTP 404 (未検出)" 状態メッセージを報告するストレージ例外をスローすることがあります。 Table Storage サービスのテーブルを調べると、要求どおりにサービスがエンティティを削除したことがわかります。

クライアントにおける例外の詳細には、要求に対してテーブル サービスによって割り当てられた要求 id (7e84f12d…) が含まれます。 この情報を使用するには、を検索して、サーバー側ストレージ ログに要求の詳細を検索、 **要求 id ヘッダー** ログ ファイル内の列です。 メトリックを使用して、このようなエラーが発生した時刻を特定し、エラーがメトリックに記録された時刻に基づいてログ ファイルを検索することもできます。 このログ エントリには、状態メッセージ "HTTP (404) Client Other Error" を出して削除が失敗したことが示されます。 同じログ エントリには、クライアントによって生成された要求 id も含まれています、 **クライアント要求 id** 列 (813ea74f…)。

サーバー側のログ記録は、同じエントリがもう 1 つも含まれています。 **クライアント要求 id** 正常な値 (813ea74f…) が、同じエンティティに対する同じクライアントから操作を削除します。 この成功した削除操作は、失敗した削除要求の直前に発生しています。

このシナリオの最も可能性の高い原因としては、クライアントがエンティティに対する削除要求を Table サービスに送信し、要求が成功したのに、(おそらく一時的なネットワーク上の問題で) サーバーからの確認を受信しなかったことが考えらます。 クライアントに自動的に、操作を再試行 (同じ **クライアント要求 id**)、およびエンティティは既に削除されているために、この再試行が失敗しました。

このような問題が頻繁に発生する場合は、クライアントが Table サービスからの確認を受信できない理由を調べる必要があります。 問題が断続的に発生する場合は、クライアントで "HTTP (404) 未検出" エラーをトラップしてログに記録する一方でクライアントの実行を継続できるようにする必要があります。

### <a name="the-client-is-receiving-409-messages"></a>クライアントが HTTP 409 (競合) のメッセージを受け取る

次の表の 2 つのクライアント操作に関するサーバー側のログから抜粋した内容を示しています。 **DeleteIfExists** と、その直後 **CreateIfNotExists** 同じ blob コンテナー名を使用します。 最初に、サーバーに送信する 2 つの要求のどちらのクライアント操作の結果に注意してください、 **GetContainerProperties** 存在をチェックするかどうかは、コンテナー、続く要求、 **DeleteContainer** または **CreateContainer** 要求します。

Timestamp|操作|結果|コンテナー名|クライアント要求 ID
---|---|---|---|---
05:10:13.7167225|GetContainerProperties|200|mmcont|c9f52c89-…
05:10:13.8167325|DeleteContainer|202|mmcont|c9f52c89-…
05:10:13.8987407|GetContainerProperties|404|mmcont|bc881924-…
05:10:14.2147723|CreateContainer|409|mmcont|bc881924-…

クライアント アプリケーションでコードを削除し、同じ名前を使用して blob コンテナーをすぐに作成し、: **CreateIfNotExists** メソッド (クライアント要求 ID bc881924-…) 最終的に HTTP 409 (競合) エラーで失敗します。 クライアントが BLOB コンテナー、テーブル、またはキューを削除した後、その名前が再び使用できるようになるまでに少し時間がかかります。

削除/再作成のパターンをよく使用する場合、クライアント アプリケーションは、新規コンテナーの作成時に必ず一意のコンテナー名を使用する必要があります。

### <a name="metrics-show-low-percent-success"></a>メトリックの示す PercentSuccess が低い、または分析ログ エントリの中にトランザクション ステータスが ClientOtherErrors の操作がある

 **PercentSuccess** メトリックは、操作の HTTP ステータス コードに基づいて成功した操作の割合をキャプチャします。 2 xx の状態コード操作、成功としてカウント 3 xx、4 および 5 xx の範囲を持つ操作は失敗としてカウントされ低い一方、 **PercentSucess** メトリックの値。 トランザクション状態でサーバー側ストレージ ログ ファイルで、これらの操作が記録されます **ClientOtherErrors**します。

これらの操作は正常に実行されているため、可用性などの他のメトリックに影響することはないことに留意してください。 正常に実行されても失敗の HTTP 状態コードを返される可能性のある操作の例を以下に示します。
- **ResourceNotFound** (未検出 404)、存在しない blob への GET 要求から次に例をします。
- **ResouceAlreadyExists** (競合 409) からの変更など、 **CreateIfNotExist** 操作リソースが既に存在します。
- **ConditionNotMet** (変更なし 304)、クライアントが送信などの条件付き操作からの変更など、 **ETag** 値および HTTP **なし-If-match** 最後の操作が更新された場合にのみ、イメージを要求するヘッダー。

ページで、ストレージ サービスが返す一般的な REST API エラー コードの一覧を入手できます。 <a href="http://msdn.microsoft.com/library/azure/dd179357.aspx" target="_blank">REST API の一般的なエラー コード</a>.

### <a name="capacity-metrics-show-an-unexpected-increase"></a>ストレージ使用量の予期しない増加が容量メトリックに示される


突然、ストレージ アカウントの使用量に予期しない変化が生じた場合は、まず、可用性メトリックを参照して理由を調べます。たとえば、領域を解放すると想定していたアプリケーション固有のクリーンアップ操作が、(領域の解放に使用する SAS トークンの期限が切れていたなどの理由で) 想定どおりに機能しなかったために削除要求の失敗の数が増加し、その結果、Blob Storage の使用量が増加した可能性があります。

### <a name="you-are-experiencing-unexpected-reboots"></a>予期せず再起動 Azure Virtual Machines のアタッチされている Vhd の数が多いが発生しています

同じストレージ アカウントにある VHD が Azure 仮想マシン (VM) に多数接続されている場合、個々のストレージ アカウントのスケーラビリティ ターゲットを超える可能性があります。これは VM が失敗する原因となります。 ストレージ アカウントの分単位メトリックを確認する必要があります (**TotalRequests**/**TotalIngress**/**TotalEgress**) ストレージ アカウントのスケーラビリティ ターゲットを超える急増します。 参照してください"[Metrics show an increase in PercentThrottlingError]"のストレージ アカウントで調整している場合の決定が発生しました。

一般に、入力操作または出力操作 VHD に対する仮想マシンの変換 **Get Page** または **Put Page** 、基礎ページ blob で操作します。 そのため、使用環境の IOPS の推定値を使用すると、単一のストレージ アカウントに配置できる VHD の数をアプリケーション固有の動作に基づいて調整できます。 1 つのストレージ アカウントに 40 個を超えるディスクを配置することはお勧めしません。 ストレージ アカウントにおける現在のスケーラビリティ ターゲット、特に使用しているストレージ アカウント タイプの合計要求レートと合計帯域幅についての詳細については、「<a href="http://msdn.microsoft.com/library/azure/dn249410.aspx" target="_blank">Azure Storage のスケーラビリティおよびパフォーマンスのターゲット</a>」を参照してください。
ストレージ アカウントのスケーラビリティ ターゲットを超過する場合、.複数のストレージ アカウントにVHDを配置して各個別アカウントにおけるアクティビティを減らす必要があります。

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>開発またはテストでのストレージ エミュレーターの使用に起因する問題

通常、開発およびテストでは、Azure ストレージ アカウントを使用せずに済むように、ストレージ エミュレーターを使用します。 ストレージ エミュレーターを使用する場合に発生する可能性のある一般的な問題を以下に示します。

- [機能 "X" がストレージ エミュレーターで機能しない]
- [ストレージ エミュレーターを使用するとエラー "HTTP ヘッダーのいずれかの値の形式が正しくありません" が発生する]
- [ストレージ エミュレーターの実行に管理者権限が必要になる]

#### <a name="feature-X-is-not-working"></a>機能 "X" がストレージ エミュレーターで機能しない

ストレージ エミュレーターは、ファイル サービスなどの Azure Storage サービスの機能をすべてサポートしているわけではありません。 詳細については、次をご覧ください。 <a href="http://msdn.microsoft.com/library/azure/gg433135.aspx" target="_blank">ストレージ エミュレーターと Azure ストレージ サービスとの違い</a> 参照してください。

ストレージ エミュレーターでサポートされない機能については、クラウドの Azure Storage サービスを使用してください。

#### <a name="error-HTTP-header-not-correct-format"></a>ストレージ エミュレーターを使用するとエラー "HTTP ヘッダーのいずれかの値の形式が正しくありません" が発生する

ローカル ストレージ エミュレーターとメソッドの呼び出しに対してストレージ クライアント ライブラリを使用して、アプリケーションをテストして **CreateIfNotExists** 「HTTP ヘッダーのいずれかの値が正しい形式では」エラー メッセージで失敗します。 これは、使用しているストレージ エミュレーターのバージョンが、使用しているストレージ クライアント ライブラリのバージョンをサポートしていないことを示しています。 ストレージ クライアント ライブラリは、ヘッダーを追加します **x ms バージョン** ことにより、すべての要求にします。 ストレージ エミュレーターが値を認識していないかどうか、 **x ms バージョン** ヘッダーは、要求を拒否します。

ストレージ クライアント ライブラリのログを使用して、値を参照してください、 **x ms バージョン ヘッダー** を送信する場合。 値を表示できる、 **x ms バージョン ヘッダー** Fiddler を使用して、クライアント アプリケーションからの要求をトレースする場合。

通常、ストレージ エミュレーターを更新せずに、最新バージョンのストレージ クライアント ライブラリをインストールして使用すると、この状況が発生します。 最新バージョンのストレージ エミュレーターをインストールするか、エミュレーターではなくクラウド ストレージを開発およびテストに使用する必要があります。

#### <a name="storage-emulator-requires-administrative-privileges"></a>ストレージ エミュレーターの実行に管理者権限が必要になる

ストレージ エミュレーターを実行する際に、管理者の資格情報を入力するように求められます。 これは、ストレージ エミュレーターを初めて初期化する場合にのみ発生します。 ストレージ エミュレーターを初期化した後は、再実行する際に管理者権限は必要ありません。

詳細については、次をご覧ください。 <a href="http://msdn.microsoft.com/library/azure/gg433132.aspx" target="_blank">コマンド ライン ツールを使用してストレージ エミュレーターを初期化する</a> (初期化することも、これは、管理者特権を要求しても Visual Studio でストレージ エミュレーター) 参照してください。

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Azure SDK for .NET のインストールで問題が発生する

SDK をインストールしようとすると、ローカル マシンへのストレージ エミュレーターのインストールのところで失敗します。 インストール ログには、以下のメッセージのいずれかが含まれています。

- CAQuietExec: エラー: SQL インスタンスにアクセスできません。
- CAQuietExec: エラー: データベースを作成できません

原因は、既存の LocalDB インストール環境の問題にあります。 既定では、Azure Storage サービスをシミュレートするときに、ストレージ エミュレーターは LocalDB を使用してデータを保持します。 SDK のインストールを試行する前に、コマンド プロンプト ウィンドウで次のコマンドを使用することによって、LocalDB インスタンスをリセットできます。

    sqllocaldb stop v11.0
    sqllocaldb delete v11.0
    delete %USERPROFILE%\WAStorageEmulatorDb3*.*
    sqllocaldb create v11.0

 **削除** コマンドは、ストレージ エミュレーターの以前のインストール環境から古いデータベース ファイルをすべてを削除します。

### <a name="you-have-a-different-issue-with-a-storage-service"></a>ストレージ サービスで別の問題が発生する

これまでのトラブルシューティング セクションに、ストレージ サービスで発生している問題が含まれていない場合は、以下のアプローチによって問題を診断およびトラブルシューティングする必要があります。

- メトリックを参照して、想定していた基準の動作から変化しているものがないか調べます。 メトリックから、問題が一時的か永続的か、また問題がどのストレージ操作に影響しているのかを判断できます。
- メトリック情報を使用してサーバー側のログ データを検索することで、発生している問題に関するより詳細な情報を得ることができます。 その情報を基に、問題をトラブルシューティングして解決できる可能性があります。
- サーバー側のログにある情報では問題をトラブルシューティングするのに不十分な場合、ストレージ クライアント ライブラリのクライアント側のログを使用してクライアント アプリケーションの動作を調べることができます。また、Fiddler、Wireshark、 Microsoft Message Analyzer などのツールを使用してネットワークを調べることもできます。

詳細については、Fiddler を使用して、次を参照してください"[Appendix 1: Using Fiddler to capture HTTP and HTTPS traffic]。"。

Wireshark の使用に関する詳細については、次を参照してください"[Appendix 2: Using Wireshark to capture network traffic]。"。

詳細については、Microsoft Message Analyzer を使用して、次を参照してください"[Appendix 3: Using Microsoft Message Analyzer to capture network traffic]。"。

## <a name="appendices"></a>付録

付録では、Azure Storage (およびその他のサービス) の問題を診断およびトラブルシューティングする際に役立つ可能性のあるいくつかのツールについて説明します。 これらのツールは Azure Storage には含まれていません。また、サードパーティの製品もあります。 そのため、これらの付録で説明するツールに、Microsoft Azure または Azure Storage のサポート契約は適用されません。したがって、評価プロセスの一環として、これらのツールのプロバイダーから入手可能なライセンスおよびサポート オプションを検討する必要があります。

### <a name="appendix-1"></a>付録 1: Fiddler を使用した HTTP および HTTPS トラフィックのキャプチャ

Fiddler は、クライアント アプリケーションと、使用する Azure Storage サービスの間の HTTP および HTTPS トラフィックを分析するのに役立つツールです。 Fiddler からをダウンロードします。 <a href="http://www.telerik.com/fiddler" target="_blank">http://www.telerik.com/fiddler</a>.

> [AZURE.NOTE] Fiddler は HTTPS トラフィックをデコードします。セキュリティへの影響を理解してそのしくみについては、これを理解するには、慎重に Fiddler のドキュメントを参照してください。

この付録では、Fiddler をインストールしたローカル マシンと Azure Storage サービスの間のトラフィックをキャプチャするための Fiddler の構成方法について簡単に説明します。

Fiddler を起動すると、ローカル マシンの HTTP および HTTPS トラフィックのキャプチャが開始されます。 以下に、Fiddler の操作に役立ついくつかのコマンドを示します。

- トラフィックのキャプチャを停止および開始します。 メイン メニューで **ファイル** ] をクリックし、 **トラフィックのキャプチャ** キャプチャのオンとオフを切り替えます。
- キャプチャしたトラフィック データを保存します。 メイン メニューで **ファイル**, 、] をクリックして **保存**, 、] をクリックし、 **すべてのセッション**: トラフィックをセッション アーカイブ ファイルに保存できます。 後で分析するためセッション アーカイブを再読み込みしたり、要請された場合にそのファイルを Microsoft サポートに送信したりできます。

Fiddler がキャプチャするトラフィックの量を制限するためで構成するフィルターを使用することができます、 **フィルター** ] タブをクリックします。 次のスクリーン ショットに送信されるトラフィックのみをキャプチャするフィルターが、 **contosoemaildist.table.core.windows.net** ストレージ エンドポイント。

![][5]

### <a name="appendix-2"></a>付録 2: Wireshark を使用したネットワーク トラフィックのキャプチャ

Wireshark は、さまざまなネットワーク プロトコルの詳細なパケット情報を表示できるネットワーク プロトコル アナライザーです。 Wireshark はからダウンロードできます。 <a href="http://www.wireshark.org/" target="_blank">http://www.wireshark.org/</a>.

以下の手順は、Wireshark をインストールしたローカル マシンから Azure ストレージ アカウントの Table サービスへのトラフィックに関する詳細なパケット情報をキャプチャする方法を示しています。

1.  ローカル マシンで Wireshark を起動します。
2.   **開始** セクションで、ローカル ネットワーク インターフェイスをインターネットに接続されているインターフェイスを選択します。
3.  クリックして **キャプチャ オプション**します。
4.  フィルターを追加する、 **Capture Filter** ] ボックスに貼り付けます。 たとえば、 **contosoemaildist.table.core.windows.net ホスト** table サービスのエンドポイントで送受信されるパケットのみをキャプチャするように Wireshark を構成、 **contosoemaildist** ストレージ アカウント。 キャプチャ フィルターの完全な一覧については、<a href="http://wiki.wireshark.org/CaptureFilters" target="_blank">http://wiki.wireshark.org/CaptureFilters</a> を参照してください。

    ![][6]

5.  クリックして **開始**します。 これで、Wireshark は、ローカル マシンでクライアント アプリケーションを使用しているときに Table サービス エンドポイントとの間で送受信されるすべてのパケットをキャプチャします。
6.  終了したら、[メイン メニューの [ **キャプチャ** し **停止**します。
7.  キャプチャしたデータを Wireshark キャプチャ ファイルを保存するメイン メニューでクリックして **ファイル** し **保存**します。

WireShark が内に存在するエラーを強調表示、 **packetlist** ウィンドウです。 使用することも、 **Expert Info** ウィンドウ (をクリックして **分析**, 、し、 **Expert Info**) エラーと警告の概要を表示します。

![][7]

アプリケーション層見られる形で TCP データを右クリックして、TCP データを表示する選択することもできます。 **Follow TCP Stream**します。 これは、特に、キャプチャ フィルターを使用せずにダンプをキャプチャした場合に便利です。 詳細については、<a href="http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html" target="_blank">ここを</a>を参照してください。

![][8]

> [AZURE.NOTE] Wireshark の使用に関する詳細については、次を参照してください。、 <a href="http://www.wireshark.org/docs/wsug_html_chunked/" target="_blank">『 Wireshark Users guide 』</a>します。

### <a name="appendix-3"></a>付録 3: Microsoft Message Analyzer を使用したネットワーク トラフィックのキャプチャ

Microsoft Message Analyzer を使用すると、Fiddler と同様の方法で HTTP および HTTPS トラフィックをキャプチャできるうえに、Wireshark と同様の方法でネットワーク トラフィックをキャプチャすることもできます。

#### Microsoft Message Analyzer を使用した Web トレース セッションの構成

Microsoft Message Analyzer アプリケーションを実行する、Microsoft Message Analyzer を使用して HTTP および HTTPS トラフィック用にし、web トレース セッションを構成する、 **ファイル** ] メニューのをクリックして **Capture/trace**します。 使用可能なトレース シナリオの一覧で選択 **Web プロキシ**します。 次に、 **Trace Scenario Configuration** パネルの **HostnameFilter** ] ボックスに、ストレージ エンドポイントの名前を追加 (これらの名前でを検索すること、 [Azure ポータル](portal.azure.com))。 たとえば、Azure ストレージ アカウントの名前は **contosodata**, に次のコードを追加する必要があります、 **HostnameFilter** ] ボックスに貼り付けます。

    contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net

> [AZURE.NOTE] 空白文字は、ホスト名を区切ります。

[トレースのデータ収集を開始する準備ができたら、 **Start With** ] ボタンをクリックします。

Microsoft Message Analyzer の詳細については **Web プロキシ** トレースは、「 <a href="http://technet.microsoft.com/library/jj674814.aspx" target="_blank">Pef-webproxy プロバイダー</a> technet します。

組み込み **Web プロキシ** Microsoft Message Analyzer でトレースは、Fiddler に基づいており、; をクライアント側の HTTPS トラフィックをキャプチャし、暗号化されていない HTTPS メッセージを表示します。  **Web プロキシ** トレースは、暗号化されていないメッセージにアクセスできるように、すべての HTTP および HTTPS トラフィック用のローカル プロキシを構成することによって機能します。

#### Microsoft Message Analyzer を使用したネットワーク問題の診断

Microsoft Message Analyzer を使用するだけでなく **Web プロキシ** クライアント アプリケーションとストレージ サービス間の Http/https トラフィックの詳細情報をキャプチャするトレースを組み込みを使用することもできます。 **Local Link Layer** ネットワークのパケット情報をキャプチャするトレース。 これにより、Wireshark を使用してキャプチャできるデータと同様のデータをキャプチャし、ドロップされたパケットなどのネットワーク問題を診断することができます。

次のスクリーン ショットのよう **Local Link Layer** トレースの **情報** にメッセージが、 **DiagnosisTypes** 列です。 あるアイコンをクリックすると、 **DiagnosisTypes** 列は、メッセージの詳細を表示します。 この例では、サーバーが、クライアントからの確認を受信しなかったためにメッセージ #305 を再転送しました。

![][9]

Microsoft Message Analyzer でトレース セッションを作成するときに、フィルターを指定して、トレースに含まれる不要な情報の量を削減できます。  **Capture/trace** トレースを定義するページでのクリックして、 **構成** リンク] の横に **Microsoft Windows NDIS PacketCapture**します。 以下のスクリーンショットは、3 つのストレージ サービスの IP アドレスに関する TCP トラフィックをフィルター処理する構成を示しています。

![][10]

Microsoft Message Analyzer Local Link Layer トレースの詳細については、次を参照してください。 <a href="http://technet.microsoft.com/library/jj659264.aspx" target="_blank">PEF NDIS-PacketCapture プロバイダー</a> technet します。

### <a name="appendix-4"></a>付録 4: Excel を使用したメトリックおよびログ データの表示

さまざまなツールを使用して、Excel に簡単に読み込んで表示したり分析したりできる区切り形式で、ストレージ メトリック データを Azure Table Storage からダウンロードすることができます。 Azure Blob Storage のストレージ ログ データは、既に、Excel に読み込み可能な区切り形式のデータです。 ただしにある情報に該当する列見出しを追加する必要 <a href="http://msdn.microsoft.com/library/azure/hh343259.aspx" target="_blank">Storage Analytics のログの形式</a> と <a href="http://msdn.microsoft.com/library/azure/hh343264.aspx" target="_blank">Storage Analytics Metrics のテーブル スキーマ</a>.

Blob Storage からダウンロードしたストレージ ログ データを Excel にインポートするには、以下の手順に従います。

-  **データ** ] メニューのをクリックして **テキストから**します。
- 表示をクリックするログ ファイルを参照 **インポート**します。
- 手順 1 で、 **テキスト ファイル ウィザード**, [ **区切り記号付き**です。

手順 1 で、 **テキスト ファイル ウィザード**, [ **セミコロン** のみを区切り文字としてとして二重引用符を選択し、 **テキスト修飾子**します。 クリックして **完了** し、ワークブックでデータを配置する場所を選択します。

### <a name="appendix-5"></a>付録 5: Application Insights for Visual Studio Team Services を使用した監視

パフォーマンスおよび可用性の監視の一環として、Visual Studio Team Services の Application Insights 機能を使用することもできます。 このツールは、以下を実行できます。

- Web サービスが使用可能および応答可能であることを確認できます。 アプリケーションが Web サイトであるか、Web サービスを使用するデバイス アプリである場合、世界中の複数の場所から数分ごとに URL をテストし、問題がある場合には通知することができます。
- Web サービスのパフォーマンス上の問題または例外をすばやく診断できます。 CPU やその他のリソースが限界まで使用されていることを検出したり、例外からスタック トレースを取得したり、ログ トレース全体を簡単に検索したりできます。 アプリのパフォーマンスの低下が許容できる限界を超えている場合には、電子メールを送信できます。 .NET と Java の両方の Web サービスを監視できます。

この資料の作成時点では、Application Insights はプレビュー段階です。 他の情報を見つけることができます。 <a href="http://msdn.microsoft.com/library/azure/dn481095.aspx" target="_blank">MSDN の Visual Studio チームのサービス向けの application Insights</a>.


<!--Anchors-->
[Introduction]: #introduction
[How this guide is organized]: #how-this-guide-is-organized

[Monitoring your storage service]: #monitoring-your-storage-service
[Monitoring service health]: #monitoring-service-health
[Monitoring capacity]: #monitoring-capacity
[Monitoring availability]: #monitoring-availability
[Monitoring performance]: #monitoring-performance

[Diagnosing storage issues]: #diagnosing-storage-issues
[Service health issues]: #service-health-issues
[Performance issues]: #performance-issues
[Diagnosing errors]: #diagnosing-errors
[Storage emulator issues]: #storage-emulator-issues
[Storage logging tools]: #storage-logging-tools
[Using network logging tools]: #using-network-logging-tools

[End-to-end tracing]: #end-to-end-tracing
[Correlating log data]: #correlating-log-data
[Client request ID]: #client-request-id
[Server request ID]: #server-request-id
[Timestamps]: #timestamps

[Troubleshooting guidance]: #troubleshooting-guidance
[Metrics show high AverageE2ELatency and low AverageServerLatency]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Metrics show low AverageE2ELatency and low AverageServerLatency but the client is experiencing high latency]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Metrics show high AverageServerLatency]: #metrics-show-high-AverageServerLatency
[You are experiencing unexpected delays in message delivery on a queue]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Metrics show an increase in PercentThrottlingError]: #metrics-show-an-increase-in-PercentThrottlingError
[Transient increase in PercentThrottlingError]: #transient-increase-in-PercentThrottlingError
[Permanent increase in PercentThrottlingError error]: #permanent-increase-in-PercentThrottlingError
[Metrics show an increase in PercentTimeoutError]: #metrics-show-an-increase-in-PercentTimeoutError
[Metrics show an increase in PercentNetworkError]: #metrics-show-an-increase-in-PercentNetworkError

[The client is receiving HTTP 403 (Forbidden) messages]: #the-client-is-receiving-403-messages
[The client is receiving HTTP 404 (Not found) messages]: #the-client-is-receiving-404-messages
[The client or another process previously deleted the object]: #client-previously-deleted-the-object
[A Shared Access Signature (SAS) authorization issue]: #SAS-authorization-issue
[Client-side JavaScript code does not have permission to access the object]: #JavaScript-code-does-not-have-permission
[Network failure]: #network-failure
[The client is receiving HTTP 409 (Conflict) messages]: #the-client-is-receiving-409-messages

[Metrics show low PercentSuccess or analytics log entries have operations with transaction status of ClientOtherErrors]: #metrics-show-low-percent-success
[Capacity metrics show an unexpected increase in storage capacity usage]: #capacity-metrics-show-an-unexpected-increase
[You are experiencing unexpected reboots of Virtual Machines that have a large number of attached VHDs]: #you-are-experiencing-unexpected-reboots
[Your issue arises from using the storage emulator for development or test]: #your-issue-arises-from-using-the-storage-emulator
[Feature "X" is not working in the storage emulator]: #feature-X-is-not-working
[Error "The value for one of the HTTP headers is not in the correct format" when using the storage emulator]: #error-HTTP-header-not-correct-format
[Running the storage emulator requires administrative privileges]: #storage-emulator-requires-administrative-privileges
[You are encountering problems installing the Azure SDK for .NET]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[You have a different issue with a storage service]: #you-have-a-different-issue-with-a-storage-service

[Appendices]: #appendices
[Appendix 1: Using Fiddler to capture HTTP and HTTPS traffic]: #appendix-1
[Appendix 2: Using Wireshark to capture network traffic]: #appendix-2
[Appendix 3: Using Microsoft Message Analyzer to capture network traffic]: #appendix-3
[Appendix 4: Using Excel to view metrics and log data]: #appendix-4
[Appendix 5: Monitoring with Application Insights for Visual Studio Team Services]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png

