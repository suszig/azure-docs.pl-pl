<properties 
    pageTitle="SQL Data Warehouse データベース監査の使用 | Microsoft Azure" 
    description="SQL Data Warehouse データベース監査の使用" 
    services="sql-data-warehouse" 
    documentationCenter="" 
    authors="twounder" 
    manager="" 
    editor=""/>

<tags 
    ms.service="sql-data-warehouse" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/06/2015" 
    ms.author="twounder"/>
 
# SQL Data Warehouse データベース監査の使用 
<p> Azure SQL Data Warehouse 監査は、データベース イベントを追跡し、監査したイベントを Azure Storage アカウントの監査ログに書き込みます

監査と規制遵守の維持、データベースの利用状況を理解およびビジネス上の懸念がある可能性やセキュリティ違犯の疑いのある不一致や異常に関する洞察が役立ちます。 

監査ツールは、標準準拠を強化し促進しますが、準拠を保証するものではありません。 標準準拠をサポートする Azure プログラムの詳細については、「<a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Microsoft Azure のトラスト センター</a>」を参照してください。

+ [データベース監査の基本] 
+ [データベースに対する監査を設定する]
+ [監査ログとレポートを分析する]

##<a id="subheading-1"></a>Azure SQL Data Warehouse データベース監査の基本


SQL Data Warehouse データベース監査により、以下のことが可能になります。

- **保持** 選択したイベントの監査記録します。 監査するデータベース活動のカテゴリを定義することができます。
- **レポート** データベース活動のです。 事前に構成したレポートとダッシュボードを使用して、活動とイベントのレポートをすぐに使用できます。
- **分析** レポートします。 疑わしいイベント、異常な活動、および傾向を発見できます。

次のイベント カテゴリの監査を構成できます。

**プレーンな SQL** と **パラメーター化された SQL** として分類は、収集された監査ログ  

- **データへのアクセス**
- **スキーマの変更 (DDL)**
- **データの変更 (DML)**
- **アカウント、ロール、アクセス許可 (DCL)**
- **ストアド プロシージャ**, 、**ログイン** 、 **トランザクション管理**します。

各イベント カテゴリの監査 **成功** と **エラー** 操作は個別に構成されます。

監査対象の活動やイベントの詳細については、<a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">監査ログ形式のリファレンス (doc ファイルのダウンロード)</a> を参照してください。 

監査ログは、Azure ストレージ アカウントに格納されます。 監査ログの保有期間を定義することができます。

特定のデータベースに対して、または既定のサーバー ポリシーとして、監査ポリシーを定義できます。 既定のサーバー監査ポリシーは、特定のオーバーライド データベース監査ポリシーが定義されていないサーバー上のすべてのデータベースに適用されます。

監査を使用している場合は、チェックを監査をセットアップする前に、 [「ダウンレベル クライアント」](sql-data-warehouse-auditing-downlevel-clients.md)します。


##<a id="subheading-2"></a>データベースに対する監査を設定する

1. <a href="https://portal.azure.com" target="_blank">Azure ポータル</a>を開きます。

2. 監査する SQL Data Warehouse データベース/SQL Server の構成ブレードに移動します。 クリックして、 **設定** に手前に表示し、[設定] ブレードでボタンをクリックして **監査**します。

    ![][1]

3. 監査構成ブレードで、選択を解除して、 **サーバーからの監査設定の継承** チェック ボックスをオンします。 これにより、特定のデータベースの設定を指定できます。
    
    ![][2]

4. 次をクリックして監査を有効に、 **ON** ] ボタンをクリックします。

    ![][3]

5. 監査構成ブレードで、[選択 **ストレージの詳細** を監査ログ ストレージ] ブレードを開きます。 ログを保存する Azure ストレージ アカウントを選択し、保持期間を選択します。 **ヒント:** 、事前に構成されたレポートのテンプレートを最大限に活用するすべての監査済みデータベースの同じストレージ アカウントを使用します。

    ![][4]

6. クリックして、 **[ok]** ストレージの詳細構成を保存] ボタンをクリックします。


7. [ **イベントによってログ記録**, 、] をクリックして **成功** と **エラー** をすべてのイベントを記録または個別のイベント カテゴリを選択します。


8. データベースの監査を構成している場合は、データの監査が正しくキャプチャされるように、クライアントの接続文字列を変更する必要がある場合があります。 チェック、 [接続文字列でサーバーの FDQN を変更](sql-data-warehouse-auditing-downlevel-clients.md) ダウンレベルのクライアント接続用のトピックです。

9. Click **OK**.


##<a id="subheading-3">監査ログとレポートを分析する</a>

監査ログがストア テーブルのコレクションに集計された、 **SQLDBAuditLogs** セットアップ中に選択した Azure ストレージ アカウント内のプレフィックス。 <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure ストレージ エクスプローラー</a>などのツールを使用してログ ファイルを表示できます。

事前に構成されたダッシュボード レポート テンプレートが<a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">ダウンロードできる Excel スプレッドシート</a>として用意されているので、ログ データをすぐに分析できます。 監査ログでテンプレートを使用するには、Excel 2013 またはそれ以降、および Power Query が必要です (<a href="http://www.microsoft.com/download/details.aspx?id=39379">ここ</a>でダウンロードできます)。 

テンプレートでは架空のサンプル データを使用しています。Power Query を設定して Azure ストレージ アカウントから監査ログを直接インポートできます。 

レポート テンプレートの使用手順の詳細については、<a href="http://go.microsoft.com/fwlink/?LinkId=506731">方法 (doc のダウンロード)</a> を参照してください。

![][5]


##<a id="subheading-4">運用環境での使用手法</a>
このセクションでは、上のスクリーン キャプチャーについて説明します。 <a href="https://portal.azure.com" target="_blank">Azure ポータル</a>または<a href= "https://manage.windowsazure.com/" target="_bank">従来の Azure クラシック ポータル</a>を使用できます。
 

##<a id="subheading-5"></a>ストレージ キーの再生成

運用環境では、ストレージ キーを最新の情報に定期的に更新することが推奨されます。 鍵を最新の情報に更新する場合は、ポリシーを再度保存する必要があります。 このプロセスは次のとおりです。


1. (上記のセクションの監査を設定)、監査構成ブレードで切り替えて、 **ストレージ アクセス キー** から *プライマリ* に *セカンダリ* と **保存**します。
![][4]
2. ストレージ構成ブレードに移動し、 **を再生成** 、 *プライマリ アクセス キー*します。

3. 監査構成ブレードに戻り、スイッチ、 **ストレージ アクセス キー** から *セカンダリ* に *プライマリ* とキーを押します **保存**します。

4. ストレージの UI に戻り、 **を再生成** 、 *セカンダリ アクセス キー* (次の準備としてキー更新サイクルです。
  
##<a id="subheading-6"></a>Automation
Azure SQL Database で監査を構成する際、使用できる PowerShell コマンドレットがいくつかあります。 auditing コマンドレットにアクセスにするには、Azure リソース マネージャー モードで PowerShell を実行する必要があります。

> [AZURE.NOTE]   [Azure リソース マネージャー](https://msdn.microsoft.com/library/dn654592.aspx) モジュールは現在プレビュー段階です。 Azure モジュールと同じ管理機能を提供しないことがあります。

Azure リソース マネージャー モードを実行中に、`Get-Command *AzureSql*` を実行して、使用できるコマンドレットの一覧を表示します。


<!--Anchors-->
[Database Auditing basics]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-get-started/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-get-started/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-get-started/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-get-started/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-get-started/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->


 


