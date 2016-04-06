<properties
    pageTitle="SQL Database 監査の使用 | Microsoft Azure"
    description="SQL Database 監査の使用"
    services="sql-database"
    documentationCenter=""
    authors="jeffgoll"
    manager="jeffreyg"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/12/2015"
    ms.author="jeffreyg; ronitr"/>
 
# SQL Database 監査の使用
Azure SQL Database 監査は、データベース イベントを追跡し、監査したイベントを Azure ストレージ アカウントの監査ログに書き込みます 監査は通常、Basic、Standard、と Premium の各サービス層で使用できます。

監査と規制遵守の維持、データベースの利用状況を理解およびビジネス上の懸念がある可能性やセキュリティ違犯の疑いのある不一致や異常に関する洞察が役立ちます。

監査ツールは、標準準拠を強化し促進しますが、準拠を保証するものではありません。 標準準拠をサポートする Azure プログラムの詳細については、「[Microsoft Azure のトラスト センター](http://azure.microsoft.com/support/trust-center/compliance)」を参照してください。

+ [Azure SQL Database の監査の基本]
+ [データベースに対する監査を設定する]
+ [監査ログとレポートを分析する]

##<a id="subheading-1"></a>Azure SQL Database の監査の基本

以下のセクションでは、Azure ポータルを使用した監査の構成について説明します。 必要な場合も [set up auditing for your database using the Classic Azure Classic Portal]します。

SQL Database 監査により、以下のことが可能になります。

- **保持** 選択したイベントの監査記録します。 監査するデータベース活動のカテゴリを定義できます。
- **レポート** データベース活動のです。 事前に構成したレポートとダッシュボードを使用して、活動とイベントのレポートをすぐに使用できます。
- **分析** レポートします。 疑わしいイベント、異常な活動、および傾向を発見できます。

> [AZURE.NOTE] 新しいセキュリティ上の脅威の可能性があることを示す異常なデータベース操作で警告を受信できます **する脅威検出** 機能を今すぐプレビューします。 [脅威の検出] は [監査の構成] ブレードでオンにして構成することができます。 参照してください [脅威の検出の概要](sql-database-threat-detection-get-started.md) 詳細です。

次のイベント カテゴリの監査を構成できます。

**プレーンな SQL** と **パラメーター化された SQL** として分類は、収集された監査ログ  

- **データへのアクセス**
- **スキーマの変更 (DDL)**
- **データの変更 (DML)**
- **アカウント、ロール、アクセス許可 (DCL)**
- **ストアド プロシージャ**, 、**ログイン** 、 **トランザクション管理**します。

各イベント カテゴリの監査 **成功** と **エラー** 操作は個別に構成されます。

監査対象の活動やイベントの詳細については、[監査ログ形式のリファレンス (doc ファイルのダウンロード)](http://go.microsoft.com/fwlink/?LinkId=506733) を参照してください。

監査ログは、Azure ストレージ アカウントに格納されます。 古いログを削除するまでの監査ログのリテンション期間の定義できます。

特定のデータベースに対して、または既定のサーバー ポリシーとして、監査ポリシーを定義できます。 既定のサーバー監査ポリシーは、特定のオーバーライド データベース監査ポリシーが定義されていないサーバー上のすべてのデータベースに適用されます。

使用している場合は、チェックを監査を設定する前に、 [「ダウンレベル クライアント」](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)します。


##<a id="subheading-2"></a>データベースに対する監査を設定する

1. 起動、 [Azure ポータル](https://portal.azure.com) https://portal.azure.com にします。 また、起動することも、 [従来の Azure クラシック ポータル](https://manage.windowsazure.com/) https://manage.windowsazure.com/ でします。 詳しい手順は次のようになります。

2. 監査する SQL Database または SQL Server の [設定] ブレードに移動します。 [設定] ブレードで、選択 **監査と脅威の検出**します。

    ![ナビゲーション ウィンドウ][1]

3. 監査構成ブレードでオフにして **ON** 監査します。

4. 選択 **ストレージの詳細** を監査ログ ストレージ] ブレードを開きます。 ログを保存する Azure ストレージ アカウントを選択し、リテンション期間を選択します。 **ヒント:** 監査レポートのテンプレートを最大限に活用するすべての監査済みデータベースの同じストレージ アカウントを使用します。

    ![ナビゲーション ウィンドウ][2]

5. をクリックして **イベントの監査** を監査するイベントをカスタマイズします。  **イベントによってログに記録** ブレードで、] をクリックして **成功** と **エラー** をすべてのイベントを記録または個別のイベント カテゴリを選択します。


6. チェックすることができます、 **サーバーからの継承の監査設定** ことを指定するこのデータベースにあるチェック ボックスがそのサーバーの設定に従って監査されます。 このオプションをオンにすると、このコンテキストでサーバーの監査設定を表示または変更するリンクが表示されます。

    ![ナビゲーション ウィンドウ][3]

7. 監査の設定を構成した後に **ON** 脅威の検出し、セキュリティ通知を受信する電子メールを構成します。 参照してください、 [脅威検出作業の開始](sql-database-threat-detection-get-started.md) 詳細についてはページです。

8. クリックして **保存**します。



##<a id="subheading-3"></a>監査ログとレポートを分析する

監査ログがストア テーブルのコレクションに集計された、 **SQLDBAuditLogs** セットアップ中に選択した Azure ストレージ アカウント内のプレフィックス。 [Azure ストレージ エクスプローラー](http://azurestorageexplorer.codeplex.com/)などのツールを使用してログ ファイルを表示できます。

事前構成されたレポート テンプレートが、 [ダウンロードできる Excel スプレッドシート](http://go.microsoft.com/fwlink/?LinkId=403540) ログ データをすばやく分析できます。 監査ログでテンプレートを使用するには、Excel 2013 またはそれ以降、および Power Query が必要です ([ここ](http://www.microsoft.com/download/details.aspx?id=39379)でダウンロードできます)。

Power Query を使用すると、Azure ストレージ アカウントから直接 Excel テンプレートに、監査ログをインポートできます。 その後監査レコードを確認し、ログ データを使用しダッシュ ボードとレポートを作成できます。


![Navigation Pane][4]


##<a id="subheading-4"></a>従来の Azure クラシック ポータルを使用してデータベースの監査を設定する

1. 起動、 [従来の Azure クラシック ポータル](https://manage.windowsazure.com/) https://manage.windowsazure.com/ でします。

2. SQL データベース] をクリックして]、[クリックして、監査する SQL Server、 **監査とセキュリティ** ] タブをクリックします。

3. 監査を設定 **有効**します。

    ![ナビゲーション ウィンドウ][5]

4. 編集、 **イベントによってログ記録** を監査するイベントをカスタマイズするのには、必要に応じて。

    ![ナビゲーション ウィンドウ][6]

5. 選択、 **ストレージ アカウント** 構成と **保有**します。

    ![ナビゲーション ウィンドウ][7]

6. クリックして **保存**します。




##<a id="subheading-5">運用環境での使用手法</a>
このセクションでは、上のスクリーン キャプチャーについて説明します。 [Azure ポータル](https://portal.azure.com)または[従来の Azure クラシック ポータル](https://manage.windowsazure.com/)を使用できます。


##<a id="subheading-6"></a>ストレージ キーの再生成

運用環境では、ストレージ キーを最新の情報に定期的に更新することが推奨されます。 キーを最新の情報に更新する場合は、監査ポリシーを再度保存する必要があります。 このプロセスは次のとおりです。


1. 監査構成ブレード スイッチでは、 **ストレージ アクセス キー** から *プライマリ* に *セカンダリ* ] をクリック **保存**します。

    ![][8]

2. ストレージ構成ブレードに移動し、 **を再生成** 、 *プライマリ アクセス キー*します。

3. 監査構成ブレードに戻り、スイッチ、 **ストレージ アクセス キー** から *セカンダリ* に *プライマリ* ] をクリック **保存**します。

4. ストレージの UI に戻り、 **を再生成** 、 *セカンダリ アクセス キー* (準備として、次のキー更新サイクル)。
  
##<a id="subheading-7"></a>Automation
Azure SQL Database で監査を構成する際、使用できる PowerShell コマンドレットがいくつかあります。

- [Get-AzureRMSqlDatabaseAuditingPolicy](https://msdn.microsoft.com/library/azure/mt603731.aspx)
- [Get-AzureRMSqlServerAuditingPolicy](https://msdn.microsoft.com/library/azure/mt619329.aspx)
- [Remove-AzureRMSqlDatabaseAuditing](https://msdn.microsoft.com/library/azure/mt603796.aspx)
- [Remove-AzureRMSqlServerAuditing](https://msdn.microsoft.com/library/azure/mt603574.aspx)
- [Set-AzureRMSqlDatabaseAuditingPolicy](https://msdn.microsoft.com/library/azure/mt603531.aspx)
- [Set-AzureRMSqlServerAuditingPolicy](https://msdn.microsoft.com/library/azure/mt603794.aspx)
- [Use-AzureRMSqlServerAuditingPolicy](https://msdn.microsoft.com/library/azure/mt619353.aspx)




<!--Anchors-->
[Azure SQL Database Auditing basics]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Set up auditing for your database using the Classic Azure Classic Portal]: #subheading-4
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Automation]: #subheading-7


<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_storage_account.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_inherit_from_server.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_report_template.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_classic_portal_enable.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_classic_portal_events.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_classic_portal_storage.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_storage_key_rotation.png


 


