<properties
    pageTitle="Stream Analytics の JSON 出力 | Microsoft Azure"
    description="Stream Analytics で、Azure DocumentDB for JSON 出力をターゲットにして、構造化されていない JSON データに対するデータ アーカイブと待機時間の短いクエリを有効にする方法について説明します。"
    keywords="JSON output"
    documentationCenter=""
    services="stream-analytics,documentdb"
    authors="jeffstokes72"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="11/23/2015"
    ms.author="jeffstok"/>


# Stream Analytics から Azure DocumentDB for JSON 出力をターゲットにする

Stream Analytics を絞り込む [Azure DocumentDB](http://azure.microsoft.com/services/documentdb/) JSON の出力は、JSON データの非構造化に関するデータのアーカイブおよび待機時間の短いクエリを有効にします。 この統合を実装する最適な方法について説明します。

DocumentDB に精通している方を見て [DocumentDB のラーニング パス](https://azure.microsoft.com/documentation/learning-paths/documentdb/) 開始します。

## 出力ターゲットとしての DocumentDB の基礎

Stream Analytics で Azure DocumentDB 出力を使用すると、ストリーム処理の結果を JSON 出力として DocumentDB コレクションに書き込むことができます。 Stream Analytics は、データベース内にコレクションを作成せず、代わりにユーザーが前もってコレクションを作成するように要求します。 これは、パフォーマンス、一貫性とを使用して直接、コレクションの容量が調整できるようにし、透明の DocumentDB コレクションの課金のコストができるように、 [DocumentDB Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)します。 ストリーミング ジョブのコレクションを論理的に分離するには、ストリーミング ジョブごとに 1 つの DocumentDB データベースを使用することをお勧めします。

DocumentDB コレクションの一部のオプションの詳細を以下に示します。

## 調整の整合性、可用性、および待機時間

DocumentDB では、アプリケーション要件を満たすために、データベースやコレクションを微調整し、一貫性、可用性、待機時間の間で妥協点を見つけることができます。 読み取りおよび書き込みの待機時間に対してシナリオで求められる読み取りの一貫性レベルに応じて、データベース アカウントでの一貫性レベルを選択することができます。 また、DocumentDB では、コレクションへの各 CRUD 操作に対する同期インデックス作成も、既定で有効になっています。 この機能も、DocumentDB で書き込みと読み取りのパフォーマンスを制御するための便利なオプションの 1 つです。 このトピックについては、確認、 [データベースとクエリの一貫性レベルを変更する](../articles/documentdb-consistency-levels.md) 記事です。

## パフォーマンス レベルの選択

DocumentDB コレクションは、3 種類のパフォーマンス レベル (S1、S2、S3) で作成できます。このレベルによって、そのコレクションへの CRUD に利用できるスループットが決まります。 さらに、コレクションのインデックス作成レベルや一貫性レベルも、パフォーマンスに影響します。 参照してください [今回](../articles/documentdb-performance-levels.md) これらのパフォーマンス レベルの詳細を理解するためです。

## Stream Analytics からのアップサート

Stream Analytics を DocumentDB と統合することで、特定のドキュメント ID 列に基づき、レコードを DocumentDB コレクションに挿入または更新できるようになります。 この機能は、*アップサート*とも呼ばれています。

Stream Analytics では、オプティミスティック アップサート手法を採用しています。この手法では、ドキュメント ID の競合が原因で挿入に失敗した場合にのみ更新が実行されます。 この更新は Stream Analytics によって PATCH として実行されるため、ドキュメントに対する部分更新が可能になります。つまり、新しいプロパティの追加や既存のプロパティの置き換えは段階的に実行されます。 アレイ全体に上書きを取得するには、配列、JSON ドキュメントの結果の配列プロパティの値の変更がマージしないことに注意してください。

## DocumentDB でのデータのパーティション分割

DocumentDB のコレクションを使用すると、クエリ パターンとアプリケーションのパフォーマンス ニーズの両方に応じてデータをパーティション分割することができます。 各コレクションに格納できるデータは最大 10 GB で、現時点ではコレクションをスケールアップ (またはオーバーフロー) する方法はありません。 スケールアウトの場合、Stream Analytics では、特定のプレフィックスを持つ複数のコレクションに書き込むことができます (使用方法の詳細については以下を参照)。 Stream Analytics を使用して、一貫性のある [ハッシュ パーティション リゾルバー](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) ユーザーに基づく戦略が PartitionKey 列をその出力レコードをパーティション分割を提供します。 ストリーミング ジョブの開始時における特定のプレフィックスを持つコレクションの数が出力パーティションの数として使用され、ジョブはその出力パーティションに並行して書き込みを行います (DocumentDB コレクション = 出力パーティション)。 1 つの S3 コレクションと遅延インデックス作成で挿入のみを実行する場合、予想される書き込みスループットは約 0.4 MB/秒です。 複数のコレクションを使用することで、スループットの向上と容量の増加が実現できます。

将来パーティション数を増やす予定がある場合は、ジョブを停止し、既存のコレクションから新しいコレクションへとデータの再パーティション分割を行った後、Stream Analytics ジョブを再開することが必要になる場合があります。 PartitionResolver の使用方法、再パーティション分割、サンプル コードなどの詳細は、フォローアップ記事に含まれる予定です。 記事 [DocumentDB でパーティション分割](../articles/documentdb-partition-data.md#developing-a-partitioned-application) もこの詳細情報を提供します。

## JSON 出力の DocumentDB 設定

Stream Analytics で DocumentDB を出力として作成すると、以下に示すように、情報を求めるプロンプトが表示されます。 このセクションでは、各プロパティの定義について説明します。

![documentdb stream analytics 出力画面](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output.png)

-   **出力のエイリアス** – ASA クエリ内でこの出力を意味するエイリアス。
-   **アカウント名** – DocumentDB アカウントの名前またはエンドポイント URI。
-   **アカウント キー** – DocumentDB アカウントの共有アクセス キー。
-   **データベース** – DocumentDB データベース名。
-   **コレクション名のパターン** – 使用するコレクションのコレクション名のパターン。 コレクション名の形式は、オプションの {partition} トークンを使用して構成できます。この場合、パーティションは 0 から開始します。 有効な入力値のサンプルを次に示します。  
   1) MyCollection – 1 つのコレクション"MyCollection"という名前が存在する必要があります。  
   2 \) – このようなコレクションが存在する必要があります: MyCollection {partition}"MyCollection0"、"MyCollection1"や"MyCollection2"です。
-   **パーティション キー** – コレクション間で出力をパーティション分割するためのキーの指定に使用される、出力イベント内のフィールドの名前。 コレクションの出力が 1 つの場合は、PartitionId など、任意の出力列を使用できます。
-   **Document ID** – 省略可能です。 挿入操作または更新操作の基準となるプライマリ キーを指定するために使用される、出力イベント内のフィールドの名前。





