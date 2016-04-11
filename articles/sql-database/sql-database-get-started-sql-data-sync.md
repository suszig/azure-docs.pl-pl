<properties
    pageTitle="SQL Database データ同期の概要"
    description="このチュートリアルでは、Azure SQL データ同期 (プレビュー) の概要について説明します。"
    services="sql-database"
    documentationCenter=""
    authors="spelluru"
    manager="JennieHubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2015"
    ms.author="jhubbard"/>


#Azure SQL データ同期の概要 (プレビュー)
このチュートリアルでは、Azure クラシック ポータルを使用して、Azure SQL データ同期の基礎を学習します。

このチュートリアルは、SQL Server および Azure SQL Database を使用した経験がほとんどない読者を対象に作成されています。 このチュートリアルでは、ハイブリッド (SQL Server および SQL Database インスタンス) 同期グループを作成し、全面的に構成して、設定したスケジュールに従って同期します。

> [AZURE.NOTE] 以前、MSDN にある Azure SQL データ同期設定完了の技術ドキュメントは、単一 .pdf ファイルをダウンロードするでは今すぐ [ここ](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)します。

## 手順 1: Azure SQL Database への接続

1. サインイン、 [旧ポータル](http://manage.windowsazure.com)します。

2. クリックして **SQL データベース** 左側のウィンドウでします。

3. クリックして **同期** ページの下部にあります。 同期] をクリックすると、一覧の項目に表示するときに追加できます - **新しい同期グループ** と **新しい同期エージェント**します。

4. 新しい SQL データ同期エージェント ウィザードを起動する] をクリックして **新しい同期エージェント**します。

5. 前に、エージェントを追加していない場合は **は、ここにダウンロード] をクリック**します。

    ![Image1](./media/sql-database-get-started-sql-data-sync/SQLDatabaseScreen-Figure1.PNG)


## ステップ 2. クライアント エージェントの追加
この手順が必要になるのは、同期グループにオンプレミス SQL Server データベースを含める場合だけです。 
同期グループにあるのが SQL Database のインスタンスのみである場合は、手順 4 をスキップします。

<a id="InstallRequiredSoftware"></a>
### ステップ 2a. 必要なソフトウェアのインストール
クライアント エージェントをインストールするコンピューターに次のソフトウェアがインストールされていることを確認してください。

- **.NET Framework 4.0**

 .NET Framework 4.0 をインストールする [ここ](http://go.microsoft.com/fwlink/?linkid=205836)します。

- **Microsoft SQL Server 2008 R2 SP1 System CLR Types (x86)**

 Microsoft SQL Server 2008 R2 SP1 System CLR Types (x86) をインストールする  [ここで](http://www.microsoft.com/download/en/details.aspx?id=26728)

- **Microsoft SQL Server 2008 R2 SP1 Shared Management Objects (x86)**

 Microsoft SQL Server 2008 R2 SP1 Shared Management Objects (x86) をインストールするにはから [ここで](http://www.microsoft.com/download/en/details.aspx?id=26728)



<a id="InstallClient"></a>
### ステップ 2b. 新しいクライアント エージェントのインストール

指示に従って、 [(SQL データ同期) クライアント エージェントのインストール](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf) エージェントをインストールします。



<a id="RegisterSSDb"></a>
### ステップ 2c. 新しい SQL データ同期エージェント ウィザードの完了

1.  新しい SQL データ同期エージェント ウィザードに戻ります。
2.  わかりやすいエージェント名を付けます。
3.  ドロップダウン リストで選択して、 **地域** (データ センター)、このエージェントをホストします。
4.  ドロップダウン リストで選択して、 **サブスクリプション** このエージェントをホストします。
5.  右矢印をクリックします。



## ステップ 3. SQL Server データベースのクライアント エージェントへの登録

クライアント エージェントをインストールした後、同期グループに含めるオンプレミス SQL Server データベースをすべてエージェントに登録します。
エージェントを含むデータベースを登録する」の手順をに従って [クライアント エージェントを SQL Server データベースを登録](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)します。



## ステップ 4. 同期グループの作成


<a id="StartNewSGWizard"></a>
### ステップ 4a. 新しい同期グループ ウィザードの開始

1.  戻り、 [旧ポータル](http://manage.windowsazure.com)します。
2.  クリックして **SQL データベース**します。
3.  クリックして **[同期の追加** ページの下部にある、ドロワから [新しい同期グループを選択します。

    ![Image2](./media/sql-database-get-started-sql-data-sync/NewSyncGroup-Figure2.png)



<a id=""></a>
### ステップ 4b. 基本設定の入力


1.  わかりやすい同期グループ名を入力します。
2.  ドロップダウン リストで選択して、 **地域** (データ センター)、この同期グループをホストします。
3. 右矢印をクリックします。

    ![Image3](./media/sql-database-get-started-sql-data-sync/NewSyncGroupName-Figure3.PNG)


<a id="DefineHubDB"></a>
### ステップ 4c. 同期ハブの定義

1. ドロップダウン リストで、同期グループのハブとして機能する SQL Database インスタンスを選択します。
2. この SQL データベース インスタンスの資格情報を入力 **ハブ ユーザー名** と **ハブ パスワード**します。
3. SQL データ同期によってユーザー名とパスワードが確認されるのを待ちます。 資格情報が確認されると、[パスワード] ボックスの右側に緑色のチェック マークが表示されます。
4. ドロップダウン リストで選択して、 **競合の解決** ポリシーです。

 **ハブを優先** のハブ データベースに書き込まれたすべての変更は、同じ参照データベース レコードの変更を上書きする、参照データベースに書き込まれます。 機能的には、ハブに書き込まれた最初の変更内容が他のデータベースに反映されることになります。


 **クライアント側に合わせる** のハブに書き込まれた変更内容は参照データベースの変更内容によって上書きされます。 機能的には、ハブに書き込まれた最後の変更内容は、他のデータベースに保持され反映されている変更内容ということになります。

5.  右矢印をクリックします。

    ![Image4](./media/sql-database-get-started-sql-data-sync/NewSyncGroupHub-Figure4.PNG)

<a id="AddRefDB"></a>
### ステップ 4d. 参照データベースの追加


同期グループに追加するデータベースごとに、この手順を繰り返します。

1. ドロップダウン リストで、追加するデータベースを選択します。

    ドロップダウン リストに表示されるデータベースは、エージェントに登録された SQL Server データベースと SQL Database インスタンスの両方を含みます。
2.  このデータベースの資格情報の入力 **USERNAME** と **パスワード**します。
3.  ドロップダウン リストで選択して、 **同期の方向** このデータベースにします。

    **双方向** - 参照データベースの変更内容がハブ データベースに書き込まれ、ハブ データベースに対する変更は、参照データベースに書き込まれます。

    **ハブから同期** -データベースがハブから更新プログラムを受信します。 変更内容をハブに送信しません。

    **ハブへ同期** -データベースがハブへの更新を送信します。 ハブの変更内容はこのデータベースに書き込まれません。

4.  同期グループの作成を終了するには、ウィザードの右下にあるチェック マークをクリックします。 SQL データ同期によって資格情報が確認されるのを待ちます。 資格情報が確認されると、緑色のチェック マークが表示されます。

5.  チェック マークをもう一度クリックします。 戻ります、 **同期** ] ページで [SQL データベース。 この同期グループが他の同期グループおよびエージェントと並んで一覧表示されています。

    ![Image5](./media/sql-database-get-started-sql-data-sync/NewSyncGroupReference-Figure5.PNG)


## ステップ 5. 同期するデータの定義

Azure SQL データ同期では、同期するテーブルと列を選択できます。 また、列を絞り込んで、特定の値 (年齢 >= 65 など) を持つ行だけを同期する場合は、Azure の SQL データ同期ポータルを使用し、「同期するテーブル、列、および行の選択」の説明を参照して、同期するデータを定義します。

1.  戻り、 [旧ポータル](http://manage.windowsazure.com)します。
2.  クリックして **SQL データベース**します。
3.  クリックして、 **同期** ] タブをクリックします。
4.  この同期グループの名前をクリックします。
5.  クリックして、 **同期規則** ] タブをクリックします。
6.  同期グループのスキーマを指定するデータベースを選択します。
7.  右矢印をクリックします。
8.  クリックして **スキーマの更新**します。
9.  データベースの各テーブルで、同期の対象とする列を選択します。
    - サポートされていないデータ型の列は選択できません。
    - デーブルの列が選択されていない場合、そのテーブルは同期グループに含められません。
    - すべてのテーブルの選択および選択解除を行うには、画面の下部にある [選択] をクリックします。
10. をクリックして **保存**, 、同期グループの準備が完了するまで待ちます。
11. データ同期のランディング ページに戻るには、画面の左上 (同期グループ名の上) にある左向き矢印をクリックします。

    ![Image6](./media/sql-database-get-started-sql-data-sync/NewSyncGroupSyncRules-Figure6.PNG)

## ステップ 6. 同期グループの構成

同期グループはいつでも、データ同期のランディング ページの下部にある [同期] をクリックして同期できます。
同期グループを定期的に同期する場合は、同期グループを構成します。

1.  戻り、 [旧ポータル](http://manage.windowsazure.com)します。
2.  クリックして **SQL データベース**します。
3.  クリックして、 **同期** ] タブをクリックします。
4.  この同期グループの名前をクリックします。
5.  クリックして、 **構成** ] タブをクリックします。
6.  **AUTOMATIC SYNC**
    - 同期グループを設定した頻度で同期を構成するのには、クリックして **ON**します。 この場合でも、[同期] をクリックして随時同期することができます。
    - クリックして **OFF** 同期をクリックした場合にのみを同期する同期グループを構成します。
7.  **[同期の頻度]**
    - [自動同期] が [オン] の場合は、同期の頻度を設定します。 頻度は 5 分から 1 か月の範囲で指定します。
8.  クリックして **保存**します。

![Image7](./media/sql-database-get-started-sql-data-sync/NewSyncGroupConfigure-Figure7.PNG)

おめでとうございます。 SQL Database インスタンスと SQL Server データベースの両方を含む同期グループを作成しました。

## 次のステップ
SQL Database と SQL データ同期の詳細については、以下を参照してください。

* [完全な SQL データ同期テクニカル ドキュメントのダウンロード](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)
* [SQL Database の概要](sql-database-technical-overview.md)
* [データベースのライフサイクル管理](https://msdn.microsoft.com/library/jj907294.aspx)
 

 

