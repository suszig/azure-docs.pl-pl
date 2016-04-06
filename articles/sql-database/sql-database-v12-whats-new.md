<properties 
    pageTitle="SQL Database V12 の新機能 |Microsoft Azure" 
    description="クラウドで Azure SQL Database を使用しているビジネス システムが今すぐ V12 にアップグレードでメリットを得られる理由を説明します。" 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jeffreyg" 
    editor=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/11/2015" 
    ms.author="genemi"/>


# SQL Database V12 の新機能


このトピックでは、Azure SQL Database の新しい V12 バージョンを V11 バージョンと比べたときの多くのメリットについて説明します。


V12 には継続的に機能が追加されます。 したがって、Azure のサービス更新情報に関する Web ページにアクセスし、次のフィルターを使用することをお勧めします。


- フィルタ リングして、 [SQL Database サービス](http://azure.microsoft.com/updates/?service=sql-database)します。
- [General availability フィルター [(GA) のアナウンス](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) SQL Database の機能についてです。


SQL データベースのリソースの制限に関する最新の情報が記載されています。<br/>[Azure SQL Database のリソース制限](sql-database-resource-limits.md)します。


## SQL Server との強化されたアプリケーションの互換性


SQL Database V12 の主要な目的は、Microsoft SQL Server 2014 との互換性を強化することでした。 その他の領域では、V12 はプログラミングの重要な領域で SQL Server との対応を実現しました。 次に例を示します。


- [ウィンドウ関数](http://msdn.microsoft.com/library/bb934097.aspx), と [経由](http://msdn.microsoft.com/library/ms189461.aspx) 
- [XML インデックス](http://msdn.microsoft.com/library/bb934097.aspx) と [選択的 XML インデックス](http://msdn.microsoft.com/library/jj670104.aspx)
- [変更の追跡](http://msdn.microsoft.com/library/bb933875.aspx)
- [SELECT...INTO](http://msdn.microsoft.com/library/ms188029.aspx)
- [フルテキスト検索](http://msdn.microsoft.com/library/ms142571.aspx)


参照してください [ここ](sql-database-transact-sql-information.md) SQL データベースでサポートされていない機能の小さなセットに対してです。


## Premium のパフォーマンスの向上、新しいパフォーマンス レベル


V12 では、すべての Premium パフォーマンス レベルに割り当てられているデータベース スループット ユニット (DTU) を追加料金なしで 25% 引き上げました。 さらに大きなパフォーマンスの向上は、次のような新機能で実現できます。


- メモリ内のサポート [列ストア インデックス](http://msdn.microsoft.com/library/gg492153.aspx)します。
- [行によるテーブル パーティション](http://msdn.microsoft.com/library/ms187802.aspx) 関連の機能強化 [TRUNCATE TABLE](http://msdn.microsoft.com/library/ms177570.aspx)します。
- 動的管理ビューの可用性 [(Dmv)](http://msdn.microsoft.com/library/ms188754.aspx) 監視し、パフォーマンスのチューニングに役立ちます。


### 信頼性の高いパフォーマンス


クライアントが Azure 仮想マシン (VM) で実行されるとき、クライアント プログラムが SQL Database V12 に接続する場合、VM で次のポート範囲を開く必要があります。

- 11000 ～ 11999
- 14000 ～ 14999


クリックして [ここ](sql-database-develop-direct-route-ports-adonet-v12.md) SQL Database V12 用のポートの詳細について。 これらのポートは、SQL Database V12 のパフォーマンスの強化に必要です。


## クラウド SaaS ベンダーのサポートの充実


V12 でのみ、新しい Standard パフォーマンス レベルの S3 と、パブリック プレビューのリリースしました [エラスティック データベース プール](sql-database-elastic-pool.md)します。
これは、クラウド SaaS ベンダー向けに設計されたソリューションです。  エラスティック データベース プールには次のメリットがあります。


- DTU をデータベース間で共有して、多数のデータベースのコストを削減できます。
- 実行 [弾力性データベース ジョブ](sql-database-elastic-jobs-overview.md) を大規模にデータベースを管理します。


## セキュリティの強化


セキュリティは、ビジネスをクラウドで遂行しているユーザーにとっての主要な懸案事項です。 V12 でリリースされた最新のセキュリティ機能は、次のとおりです。


- [行レベルのセキュリティ](http://msdn.microsoft.com/library/dn765131.aspx) (RLS)
- [動的データ マスク](sql-database-dynamic-data-masking-get-started.md)
- [包含データベース](http://msdn.microsoft.com/library/ff929188.aspx)
- [アプリケーション ロール](http://msdn.microsoft.com/library/ms190998.aspx) REVOKE GRANT、DENY で管理されます。
- [透過的なデータ暗号化](http://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) (TDE)
- [Azure Active Directory 認証を使用して SQL Database に接続する](sql-database-aad-authentication.md)
 - SQL Database は、Azure Active Directory の認証をサポートするようになりました。この認証は、Azure Active Directory (Azure AD) の ID を使用して SQL Database に接続するメカニズムです。 Azure Active Directory 認証を使用すると、データベース ユーザーの ID や他の Microsoft サービスを一元管理できます。
- [常に暗号化](https://msdn.microsoft.com/library/mt163865.aspx) (プレビュー) で暗号化透過的にアプリケーション行い、クライアントが SQL データベースの暗号化キーを共有せずにクライアント アプリケーション内の機密データを暗号化できるようにします。


## 復旧が必要なときのビジネス継続性の向上


V12 では、目標復旧時点 (PRO) と推定復旧時間 (ERT) が大幅に向上しています。


| ビジネス継続性に関係する機能 | 以前のバージョン | V12 |
| :-- | :-- | :-- |
| geo リストア | • RPO は < 24 時間です。<br/>• ERT < 12 時間です。 | • RPO < 1 時間です。<br/>• ERT < 12 時間です。 |
| Standard geo レプリケーション | • RPO は < 30 分です。<br/>• ERT は < 2 時間です。 | • RPO < 5 秒です。<br/>• ERT < 30 秒です。 |
| アクティブ geo レプリケーション | • RPO < 5 分。<br/>• ERT は < 1 時間です。 | • RPO < 5 秒です。<br/>• ERT < 30 秒です。 |


参照してください [SQL Database のビジネス継続性](sql-database-business-continuity.md) の詳細。


## 今すぐアップグレードすることをお勧めするその他の理由


Azure SQL Database を今すぐ V11 から V12 にアップグレードすることをお勧めする理由は多数あります。


- SQL Database V12 には、V11 にない機能が多数あります。
- V12 には新しい機能を追加していくものの、V11 に新機能は追加されません。
- ほとんどの新機能は、Microsoft SQL Server 向けにリリースされる前に、SQL Database V12 でリリースされます。


## V12 を既に使っている場合


以前のバージョンの SQL Database サービスでデータベースまたは論理サーバーが実行されているかどうかは、次の方法で簡単に確認できます。


1. 移動して、 [Azure ポータル](http://portal.azure.com/)します。
2. クリックして **参照**します。
3. クリックして **の SQL Server**します。
4. サーバーまたはデータベースの隣のアイコンから、現在の状態がわかります。
 - ![V12 サーバーのアイコン](./media/sql-database-v12-whats-new/v12_icon.png) **V12 の論理サーバー**
 - ![以前のバージョンのサーバーのアイコン](./media/sql-database-v12-whats-new/earlier_icon.png) **以前のバージョンの論理サーバー**


データベースで `SELECT @@version;` ステートメントを実行し、次のような結果を確認することで、バージョンを確認することもできます。


- **12**.0.2000.10 & nbsp; *(バージョン V12)*
- **11**.0.9228.18 & nbsp; *(バージョン V11)*


V12 の論理サーバーでのみ、V12 データベースをホストできます。 V12 サーバーは V12 データベースのみをホストできます。


まだ V12 で実行されていない場合は、次の手順に従って、論理サーバーをアップグレードできます [インプレース SQL Database V12 へアップグレード](sql-database-v12-upgrade.md)します。


## <a name="V12AzureSqlDbPreviewGaTable"></a> 一般公開リージョン


- 2015 年 7 月 31 日までにすべてのリージョンが完全一般公開 (GA) に昇格しました。
- V12 は 2014 年 12 月にリリースされましたが、プレビューの段階でした。

[Microsoft Azure プレビューの使用法の補足条項](http://azure.microsoft.com/support/legal/preview-supplemental-terms/)します。


