<properties 
    pageTitle="Elastic Database ツールの用語集 | Microsoft Azure" 
    description="エラスティック データベース ツールで使用される用語の説明" 
    services="sql-database" 
    documentationCenter="" 
    manager="jeffreyg" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/04/2015" 
    ms.author="ddove;sidneyh"/>

# Elastic Database ツールの用語集
に関する次の用語の定義、 [エラスティック データベース ツール](sql-database-elastic-scale-introduction.md), 、Azure SQL Database の機能です。 ツールを使用する管理 [のシャード マップ](sql-database-elastic-scale-shard-map-management.md), を含めると、 [クライアント ライブラリ](sql-database-elastic-database-client-library.md), 、 [分割/マージ ツール](sql-database-elastic-scale-overview-split-and-merge.md), 、[エラスティック プール](sql-database-elastic-pool.md), と [クエリ](sql-database-elastic-query-overview.md)します。 

これらの用語を使用 [エラスティック データベース ツールを使用してシャードを追加する](sql-database-elastic-scale-add-a-shard.md) と [RecoveryManager クラスを使用してシャード マップに関する問題の解決](sql-database-elastic-database-recovery-manager.md)します。

![Elastic Scale 用語][1]

**データベース**: Azure SQL データベース。 

**データ依存ルーティング**: により、アプリケーションは、特定のシャーディング キーを持つシャードに接続する機能です。 比較する **マルチシャード クエリ**します。

**グローバル シャード マップ**: 対応するシャード内のシャーディング キーとの間のマップ、 **シャード セット**します。 グローバル シャード マップが格納されている、 **シャード マップ マネージャー**します。 比較する **ローカル シャード マップ**します。

**リスト シャード マップ**: シャーディング キーは個別にマップされたシャード マップ。 比較する **範囲シャード マップ**します。   

**ローカル シャード マップ**: シャードに格納され、ローカルのシャード マップには、シャードに存在するシャードレットのマッピングが含まれます。

**マルチシャード クエリ**: 複数のシャードに対してクエリを発行する機能 UNION ALL セマンティクス (「ファンアウト クエリ」とも呼ばれます) を使用して結果セットが返されます。 比較する **データ依存ルーティング**します。

**範囲シャード マップ**: シャード分散戦略が連続値の複数の範囲に基づくはシャード マップ。 

**テーブルを参照する**: シャード化されず、シャード間でレプリケートされるテーブル。 たとえば、郵便番号を参照テーブルに格納できます。 

**シャード**: シャード化されたデータ セットからデータを格納する Azure SQL データベース。 

**シャードの弾力性**: 両方を実行する機能 **水平方向のスケーリング** と **垂直スケーリング**します。

**シャード化テーブル**: テーブル、つまり、データがシャーディング キー値に基づいてシャード間で分散されます。 

**シャーディング キー**: シャード間でデータを分散する方法を決定する列の値。 値の型は、次のいずれかを指定できます: **int**, 、**bigint**, 、**varbinary**, 、または **uniqueidentifier**します。 

**シャード セット**: シャード マップ マネージャーの同じシャード マップに属するシャードのコレクション。  

**シャードレット**: すべてのシャード上のシャーディング キーの単一の値に関連付けられているデータ。 シャードレットは、シャード化テーブルを再分散する際に使用できる最小データ移動単位です。 

**シャード マップ**: シャーディング キーと対応するシャード間のマッピングのセットです。

**シャード マップ マネージャー**: シャード マップ、シャードの場所、および 1 つまたは複数のシャード セットのマッピングを含む管理オブジェクトおよびデータ ストアです。

![マッピング][2]


##動詞

**水平方向のスケーリング**: スケール アウト (または内) の動作、シャードのコレクションを追加または次に示すように、シャード マップに対するシャードを削除します。

![水平および垂直方向のスケーリング][3]

**マージ**: 2 つのシャードから 1 つのシャードにシャードレットを移動し、それに応じてシャード マップを更新します。

**シャードレットの移動**: を別のシャードに 1 つのシャードレットを移動する操作。 

**シャード**: データをシャーディング キーに基づいて複数のデータベース構造を持つ同じ水平方向にパーティション分割の操作です。

**分割**: 1 つのシャードから別の (通常は新しい) シャードに複数のシャードレットを移動する操作。 シャーディング キーは分割ポイントとしてユーザーから提供されます。

**垂直方向のスケーリング**: スケール アップ (またはダウン) の個々 のシャードのパフォーマンス レベル。 たとえば、シャードを Standard から Premium に変更 (結果、コンピューティング リソースが増大)。 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png
 
