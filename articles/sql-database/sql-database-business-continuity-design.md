<properties 
   pageTitle="ビジネス継続性のための SQL Database の設計" 
   description="選択するためのガイダンス。このセクションでは、どの BCDR 機能をいつ使用するかを選択する方法についてのガイダンスが提供されます。 これには、SQL DB を使用して自動的に取得できることに関する説明が含まれます。"
   services="sql-database" 
   documentationCenter="" 
   authors="elfisher" 
   manager="jeffreyg" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="11/16/2015"
   ms.author="elfish"/>

#ビジネス継続性のための設計

ビジネス継続性のためにアプリケーションを設計するには、次の質問に答える必要があります。

1. アプリケーションを障害から保護するために適切なビジネス継続性の機能はどれでしょうか。
2. どのレベルの冗長性とレプリケーション トポロジを使用するでしょうか。

##geo リストアを使用する場合

SQL Database では、既定ですべてのデータベースにおける基本的な保護が組み込まれています。 これは地理冗長 Azure ストレージ (GRS) にデータベースのバックアップを格納することで行われます。 この方法を選択する場合、特別に構成したり、別のリソースを割り当てる必要はありません。 これらのバックアップを使用すると、あらゆるリージョンでgeo リストアのコマンドを使用してデータベースを回復できます。 使用 [障害からの回復](sql-database-disaster-recovery.md) 地理リストアを使用して、アプリケーションを回復するの詳細については、セクションです。 

アプリケーションが次の条件を満たす場合は組み込まれている保護を使用する必要があります。

1. ミッション クリティカルとして考慮されない場合。 拘束力のある SLA はないため、24 時間以上のダウンタイムで財務責任が課せられることはありません。
2. データ変更レートが低い場合 (1 時間あたりのトランザクションなど)。 RPO が 1 時間でも大規模なデータの損失は発生しません。
3. アプリケーションは費用重視なので、geo レプリケーションの追加コストは正当化できません。 

> [AZURE.NOTE] 地理的復元は、システム停止時に、バックアップからのアクティブなデータベースを復元する、特定のリージョンにコンピューティング容量を事前割り当てられません。 サービスでは、そのリージョンの既存のデータベースへの影響を最小限に抑え、容量の要求を最優先する形でgeo リストアのリクエストに関連するワークロードを管理します。 そのため、データベースの復旧時間は、同じリージョンで同時に回復するその他のデータベースの量によって異なります。 

##geo レプリケーションを使用する場合

geo レプリケーションは、プライマリから別のリージョンにレプリカ データベース (セカンダリ) を作成します。 これにより、データベースには必要なデータとコンピューティング リソースを持たせ、回復後のアプリケーションのワークロードをサポートすることが保証されます。 参照してください [障害からの回復](sql-database-disaster-recovery.md) フェールオーバーを使用して、アプリケーションを回復する」セクション。

アプリケーションが次の条件を満たす場合は geo レプリケーションを使用する必要があります。

1. ミッション クリティカルな場合。 アグレッシブな RPO と RTO で、拘束力のある SLA がある場合。 データや可用性の損失により財務責任が発生します。 
2. データ変更レートが高い場合 (1 分または 1 秒あたりのトランザクションなど)。 既存の保護に関連した RPO が 1 時間の場合は、許容できないデータ損失が発生する可能性が高くなります。
3. geo レプリケーションの使用に関連するコストは、潜在的な財務責任とビジネスの関連する損失を大幅に下回る場合。

> [AZURE.NOTE] データベースの Geo レプリケーションはサポートされていません、アプリケーションは、基本的な層を使用している場合

##Standard かアクティブ geo レプリケーションを選択するとき

Standard レベルのデータベースには、アクティブ geo レプリケーションを使用するオプションがないため、Standard のデータベースを使用し、前述の条件を満たす場合は、標準 geo レプリケーションが有効にできるはずです。 一方で Premium のデータベースではどちらのオプションも選択できます。 標準 geo レプリケーションは、よりシンプルで低コストの障害復旧ソリューションとして設計されており、特に停電などの予期しないイベントから保護する場合にのみソリューションを使用するアプリケーションに最適です。 標準 geo レプリケーションでは、障害復旧のペアのリージョンのみを復旧に使用でき、各プライマリに対してセカンダリは 1 つずつしか作成できません。 アプリケーションのアップグレード シナリオでは、追加のセカンダリが必要になる場合があります。 したがって、このシナリオが使用するアプリケーションにとって重要な場合は、代わりにアクティブ geo レプリケーションを有効にする必要があります。 参照してください [ダウンタイムのないアプリケーションをアップグレード](sql-database-business-continuity-application-upgrade.md) さらに詳細です。 

> [AZURE.NOTE] アクティブ Geo レプリケーションには、したがって読み取り専用ワークロードに対して追加の容量を提供するセカンダリ データベースに読み取り専用のアクセスもサポートしています。 

##geo レプリケーションを有効にする方法

geo レプリケーションは Azure クラシック ポータルを使用するか REST API や PowerShell コマンドを呼び出すことによって有効にできます。

###Azure クラシック ポータル

[AZURE.VIDEO sql-database-enable-geo-replication-in-azure-portal]

1. ログイン、 [Azure クラシック ポータル](https://portal.Azure.com)
2. 画面の左側にある次のように選択します **参照** し、[ **SQL データベース。**
3. 選択に、データベース ブレードに移動し、 **Geo Replication map** ] をクリック **Geo レプリケーションの構成**します。
4. geo レプリケーションのブレードに移動します。 ターゲット リージョンを選択します。 
5. [Create Secondary] のブレードに移動します。 ターゲット リージョンから既存のサーバーを選択するか、新しく作成します。
6. セカンダリの種類の選択 (*Readable* または *non-readable*)
7. クリックして **作成** 構成を完了するには

> [AZURE.NOTE] Geo レプリケーションのブレードで DR のペアのリージョンとしてマークする *推奨*します。 Premium レベルのデータベースを使用する場合は、別のリージョンを選択できます。 Standard のデータベースを使用している場合は、変更できません。 Premium データベースでがセカンダリの種類の選択 (*Readable* または *non-readable*)。 標準のデータベースのみを選択できます、 *non-readable* セカンダリ。


###PowerShell

使用して、 [新規 AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx) PowerShell コマンドレットを Geo レプリケーションの構成を作成します。 このコマンドは同期コマンドです。プライマリ データベースとセカンダリ データベースが同期状態になるとこのコマンドから制御が戻ります。 

Premium または Standard データベースの読み取りできないセカンダリで geo レプリケーションを構成するには:
        
    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "None"

Premium データベースの読み取り可能なセカンダリで geo レプリケーションを作成するには:

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "All"
         

###REST API 

使用 [Create Database](https://msdn.microsoft.com/library/mt163685.aspx) API を *createMode* に設定 *NonReadableSecondary* または *セカンダリ* をプログラムで Geo レプリケーションのセカンダリ データベースを作成します。

この API は非同期になります。 使用して返された後に、 [レプリケーション リンクの取得](https://msdn.microsoft.com/library/mt600778.aspx) API がこの操作の状態を確認します。  *ReplicationState* 操作が完了すると、応答本文のフィールドは CATCHUP の値になります。


##フェールオーバーの構成を選択する方法 

ビジネス継続性向けのアプリケーションを設計する際は、いくつかの構成オプションを検討する必要があります。 アプリケーションのデプロイメント トポロジと、停止時に最も脆弱になるアプリケーションの部分によって選択肢が異なります。 参照してください [災害復旧を使用して Geo レプリケーション用のクラウド ソリューションの設計](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ガイダンスです。

