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

<p> Azure SQL データ ウェアハウスの監査と、データベース イベントと監査したイベントを監査ログを Azure ストレージ アカウントに書き込みますが追跡されます。

監査と規制遵守の維持、データベースの利用状況を理解およびビジネス上の懸念がある可能性やセキュリティ違犯の疑いのある不一致や異常に関する洞察が役立ちます。

監査ツールは、標準準拠を強化し促進しますが、準拠を保証するものではありません。 Azure の詳細については、標準準拠をサポートするプログラムを参照してください、 <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure のトラスト センター</a>します。

+ [データベース監査の基本]
+ [データベースの監査を設定する]
+ [監査ログとレポートを分析する]

## <a id="subheading-1"></a>Azure SQL データ ウェアハウスのデータベース監査の基本

SQL Data Warehouse データベース監査により、以下のことが可能になります。

- 選択したイベントの監査証跡の**保持**。 監査するデータベース活動のカテゴリを定義することができます。
- データベース活動の**レポート**。 事前に構成したレポートとダッシュボードを使用して、活動とイベントのレポートをすぐに使用できます。
- レポートの**分析**。 疑わしいイベント、異常な活動、および傾向を発見できます。

次のイベント カテゴリの監査を構成できます。

収集された監査ログが次のように分類される**プレーンな SQL** および**パラメーター化された SQL**

- **データへのアクセス**
- **スキーマの変更 (DDL)**
- **データの変更 (DML)**
- **アカウント、ロール、アクセス許可 (DCL)**
- **ストアド プロシージャ**、**ログイン**、**トランザクション管理**。

各イベント カテゴリに対し、**成功**および**失敗**した操作の監査が個別に構成されます。

アクティビティとイベントの監査についてさらに詳細については、「、 <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">監査ログ形式のリファレンス (doc ファイルのダウンロード)</a>します。

監査ログは、Azure ストレージ アカウントに格納されます。 監査ログの保有期間を定義することができます。

特定のデータベースに対して、または既定のサーバー ポリシーとして、監査ポリシーを定義できます。 既定のサーバー監査ポリシーは、特定のオーバーライド データベース監査ポリシーが定義されていないサーバー上のすべてのデータベースに適用されます。

監査を使用している場合は、チェックを監査をセットアップする前に、 [「ダウンレベル クライアント」](sql-data-warehouse-auditing-downlevel-clients.md)します。


## <a id="subheading-2"></a>データベースに対する監査を設定します。

1. 起動、 <a href="https://portal.azure.com" target="_blank">Azure ポータル</a>します。

2. 監査する SQL Data Warehouse データベース/SQL Server の構成ブレードに移動します。 上部の [**設定**] ボタンをクリックし、[設定] ブレードで [**監査**] を選択します。

    ![][1]

3. 監査構成ブレードで、最初に **[監査設定をサーバーから継承]** チェック ボックスをオフにします。 これにより、特定のデータベースの設定を指定できます。

    ![][2]

4. 次に、**[オン]** ボタンをクリックして監査を有効にします。

    ![][3]

5. 監査構成ブレードで、**[ストレージの詳細]** を選択して [監査ログ ストレージ] ブレードを選択します。 ログを保存する Azure ストレージ アカウントを選択し、保持期間を選択します。 **ヒント:** すべての監査済みデータベースに同じストレージ アカウントを使用して、事前に構成したレポートのテンプレートを活用します。

    ![][4]

6. **[OK]** ボタンをクリックして、記憶域の詳細構成を保存します。

7. [**イベントごとのログ記録**] で、[**成功**] および [**失敗**] を、すべてのイベントについて、または個別のイベント カテゴリについて記録します。

8. データベースの監査を構成している場合は、データの監査が正しくキャプチャされるように、クライアントの接続文字列を変更する必要がある場合があります。 チェック、 [接続文字列でサーバーの FDQN を変更](sql-data-warehouse-auditing-downlevel-clients.md) ダウンレベルのクライアント接続用のトピックです。

9. **[OK]** をクリックします。


## <a id="subheading-3">監査ログとレポートを分析します。</a>

監査ログは、設定時に選択した Azure ストレージ アカウントで、**SQLDBAuditLogs** というプレフィックスを使用してストア テーブルのコレクションに集計されます。 などのツールを使用してログ ファイルを表示する <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure ストレージ エクスプ ローラー</a>します。

構成済みのダッシュ ボード レポート テンプレートが、 <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">ダウンロードできる Excel スプレッドシート</a> ログ データをすばやく分析できます。 監査ログでテンプレートを使用する Excel 2013 またはそれ以降、および Power Query ダウンロードすることが必要 <a href="http://www.microsoft.com/download/details.aspx?id=39379">ここ</a>します。

テンプレートでは架空のサンプル データを使用しています。Power Query を設定して Azure ストレージ アカウントから監査ログを直接インポートできます。

詳細レポート テンプレートの使用方法については、読み取り、 <a href="http://go.microsoft.com/fwlink/?LinkId=506731">(doc のダウンロード) をどのように</a>します。

![][5]


## <a id="subheading-4">運用環境での使用手法</a>

このセクションでは、上のスクリーン キャプチャーについて説明します。 いずれか <a href="https://portal.azure.com" target="_blank">Azure ポータル</a> または <a href= "https://manage.windowsazure.com/" target="_bank">従来の Azure クラシック ポータル</a> 使用することがあります。


## <a id="subheading-5"></a>ストレージ キーの再生成

運用環境では、ストレージ キーを最新の情報に定期的に更新することが推奨されます。 鍵を最新の情報に更新する場合は、ポリシーを再度保存する必要があります。 このプロセスは次のとおりです。


1. (上記のセクションの監査を設定)、監査構成ブレードで切り替えて、 **ストレージ アクセス キー** から *プライマリ* に *セカンダリ* と **保存**します。
![][4]
2. ストレージ構成ブレードに移動し、*プライマリ アクセス キー*を**再生成**します。

3. 監査構成ブレードに戻り、[**ストレージ アクセス キー**] を [*セカンダリ*] から [*プライマリ*] に切り替え、[**保存**] をクリックします。

4. ストレージの UI に戻り、*セカンダリ アクセス キー*を**再生成** (次のキー更新サイクルの準備として) します。

## <a id="subheading-6"></a>オートメーション

Azure SQL Database で監査を構成する際、使用できる PowerShell コマンドレットがいくつかあります。 auditing コマンドレットにアクセスにするには、Azure リソース マネージャー モードで PowerShell を実行する必要があります。
> [AZURE.NOTE] [Azure リソース マネージャー](https://msdn.microsoft.com/library/dn654592.aspx) モジュールは現在プレビュー段階です。 Azure モジュールと同じ管理機能を提供しないことがあります。

Azure リソース マネージャー モードのときは、実行 `Get-command * 仮想 *` を利用可能なコマンドレットを一覧表示します。












[database auditing basics]: #subheading-1 
[set up auditing for your database]: #subheading-2 
[analyze audit logs and reports]: #subheading-3 
[1]: ./media/sql-data-warehouse-auditing-get-started/sql-data-warehouse-auditing.png 
[2]: ./media/sql-data-warehouse-auditing-get-started/sql-data-warehouse-auditing-inherit.png 
[3]: ./media/sql-data-warehouse-auditing-get-started/sql-data-warehouse-auditing-enable.png 
[4]: ./media/sql-data-warehouse-auditing-get-started/sql-data-warehouse-auditing-storage-account.png 
[5]: ./media/sql-data-warehouse-auditing-get-started/sql-data-warehouse-auditing-dashboard.png 

