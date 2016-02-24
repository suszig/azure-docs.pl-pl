<properties 
   pageTitle="Data Lake Tools for Visual Studio を使用する U-SQL スクリプトの開発 | Azure" 
   description="Data Lake Tools for Visual Studio のインストール方法と、U-SQL スクリプトの開発およびテスト方法について説明します。 " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="12/02/2015"
   ms.author="jgao"/>

# チュートリアル: Data Lake Tools for Visual Studio を使用する U-SQL スクリプトの開発

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Data Lake Tools for Visual Studio をインストールし、それを使用して U-SQL スクリプトを作成およびテストする方法を説明します。

U-SQL は、Data Lake 内のすべてのデータを準備、変換、分析するための拡張性に優れた言語です。 詳細については、[U SQL Reference] (http://go.microsoft.com/fwlink/p/?LinkId=691348) を参照してください。


**前提条件**

- **Visual Studio 2015、Visual Studio 2013 update 4、または Visual Studio 2012 (Visual C++ インストール済み)。** 
- **Microsoft Azure SDK for .NET バージョン 2.7 以上**します。  使用してインストール、 [Web プラットフォーム インストーラー](http://www.microsoft.com/web/downloads/platform.aspx)します。
- **[Visual Studio のツールの data Lake](http://aka.ms/adltoolsvs)**します。 

    Data Lake Tools for Visual Studio がインストールされると、Visual Studio に [Data Lake] メニューが表示されます。
    
    ![U-SQL Visual Studio のメニュー](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **次の 2 つのセクションに従って進みます [Azure ポータルを使用して Azure Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md)**します。

    - [Azure Data Lake 分析アカウントを作成する](data-lake-analytics-get-started-portal.md#create_adl_analytics_account)です。
    - [SearchLog.tsv を既定のデータ湖のストレージ アカウントにアップロード](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account)します。

    Data Lake Tools では、Data Lake Analytics アカウントの作成はサポートされません。  そのため、Azure ポータル、Azure PowerShell、.NET SDK、または Azure CLI を使用して作成する必要があります。 Data Lake Analytics ジョブを実行するには、いくつかのデータが必要です。 Data Lake Tools でデータのアップロードがサポートされていても、このチュートリアルに従いやすくするため、サンプル データのアップロードにはポータルを使用します。 

## Connect to Azure

**Data Lake Analytics に接続するには**

1. Visual Studio を開きます。
2.  **U SQL** ] メニューをクリックして **オプションと設定**します。
4. クリックして **サイン イン**, 、または **ユーザーの変更** 他のユーザーがサインイン時に、サインインの指示に従って場合。
5. クリックして **OK** オプションと設定] ダイアログを閉じます。

**Data Lake Analytics アカウントを参照するには**

1. Visual Studio で開きます **サーバー エクスプ ローラー** 押下時 **CTRL + ALT + S**します。
2.  **サーバー エクスプ ローラー**, 、展開 **Azure**, 、順に展開 **Data Lake 分析**します。 Data Lake Analytics アカウントが複数ある場合は、そのリストが表示されます。 Visual Studio で Data Lake Analytics アカウントを作成することはできません。 アカウントを作成するを参照してください。 [Azure ポータルを使用して Azure Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md) または [Azure PowerShell を使用して Azure Data Lake 分析を使ってみる](knoa-get-started-powershell.md)します。

## ソース データ ファイルのアップロード

一部のデータをアップロードした、 **前提条件となる** チュートリアルの前半の「します。  

独自のデータを使用したい場合は、以下の手順を使用して Data Lake Tools からデータをアップロードします。

**使用する Azure Data Lake アカウントにファイルをアップロードするには**

1.  **サーバー エクスプ ローラー**, 、展開 **Azure**, 、展開 **Data Lake 分析**, 、Data Lake 分析アカウントを展開し、 **ストレージ アカウント**します。 既定の Data Lake ストレージ アカウント、リンクされている Data Lake ストレージ アカウント、リンクされている Azure ストレージ アカウントが表示されます。 既定の Data Lake アカウントには、「既定のストレージ アカウント」と表示されます。
2. 既定のデータ湖のストレージ アカウントを右クリックし、をクリックし、 **エクスプ ローラー**します。  Data Lake Tools for Visual Studio Explorer ウィンドウが開きます。  左側にツリー ビューが、右側にコンテンツ ビューが表示されます。
3. ファイルをアップロードするフォルダーを参照します。 
4. 任意の空白領域を右クリックし、 **アップロード**します。 

    ![U-SQL Visual Studio プロジェクト U-SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-upload-files.png)

**Azure BLOB ストレージ アカウントにファイルをアップロードするには**

1.  **サーバー エクスプ ローラー**, 、展開 **Azure**, 、展開 **Data Lake 分析**, 、Data Lake 分析アカウントを展開し、 **ストレージ アカウント**します。 既定の Data Lake ストレージ アカウント、リンクされている Data Lake ストレージ アカウント、リンクされている Azure ストレージ アカウントが表示されます。 
2. Azure ストレージ アカウントを展開します。
3. クリックしてファイルをアップロードするコンテナーを右クリックして **エクスプ ローラー**します。
4. ファイルをアップロードするフォルダーを参照します。 
5. 任意の空白領域を右クリックし、 **アップロード**します。 

## U-SQL スクリプトの開発 

Data Lake Analtyics ジョブは U-SQL 言語で記述されます。 U SQL の詳細については、次を参照してください。 [U SQL 言語を使ってみる](data-lake-analytics-u-sql-get-started.md) と [U SQL 言語リファレンス](http://go.microsoft.com/fwlink/?LinkId=691348)します。

**Data Lake Analytics ジョブを作成して送信するには** 

1.  **ファイル** ] メニューのをクリックして **新規**, 、クリックして **プロジェクト**します。
2. 選択、 **U SQL プロジェクト** 型です。

    ![新しい U-SQL Visual Studio プロジェクト](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Click **OK**. Visual studio を使用したソリューションの作成、 **Script.usql** ファイルです。
4. 次のスクリプトを入力して **Script.usql**:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    この U SQL スクリプトが、ソース ファイルを使用してデータを読み取る **Extractors.Tsv()**, を使用して csv ファイルを作成および **Outputters.Csv()**します。 
    
    ソース ファイルを別の場所にコピーしたのでない限り、2 つのパスを変更しないでください。  存在しない場合、Data Lake Analytics は出力フォルダーを作成します。
    
    既定の Data Lake アカウントに格納されたファイルの相対パスを使用する方が簡単です。 絶対パスを使用することもできます。  たとえば、次のように入力します。 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    絶対パスを使用して、リンクされたストレージ アカウント内のファイルにアクセスする必要があります。  リンクされた Azure ストレージ アカウントに格納されているファイルの構文は以下のとおりです。
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] パブリック blob またはコンテナーのパブリック アクセス権限を持つ azure の Blob コンテナーは現在サポートされていません。  

    次の機能に注意してください。

    - **IntelliSense**
     
        自動的に作成された名前とメンバーが、行セット、クラス、データベース、スキーマ、ユーザー定義オブジェクト (UDO) に対して表示されます。 
         
        カタログ エンティティ (データベース、スキーマ、テーブル、UDO など) に対する IntelliSense は、コンピューティング アカウントと関連付けられています。 現在のアクティブなコンピューティング アカウント、データベース、スキーマを上部のツールバーで確認でき、ドロップダウン リストで切り替えることができます。 
 
    - **オート フォーマット** 
    
        [編集] -> [詳細設定] でコードの構造に基づいて Scope スクリプトのインデントを変更できます。
  
        - ドキュメントのフォーマット (Ctrl + E、D): ドキュメント全体をフォーマットします   
        - 選択範囲のフォーマット (Ctrl + K、Ctrl + F): 選択範囲をフォーマットします。 範囲が選択されていない場合、このショートカットはカーソルがある行をフォーマットします。  
        
        すべてのフォーマット ルールは、[ツール] -> [オプション] -> [テキスト エディター] -> [SIP] -> [フォーマット] で構成できます。  
    - **スマート インデント**
     
        スクリプトの作成中に、Data Lake Tools for Visual Studio は式を自動的にインデントできます。 この機能は既定では無効になっているので、ユーザーは [U-SQL] -> [オプションと設定] -> [スイッチ] -> [スマート インデントを有効にする] をオンにして有効にする必要があります。

    - **定義に移動してすべての参照を検索**
    
        行セット/パラメーター/列/UDO などの名前を右クリックし、[定義へ移動] (F12) をクリックすると、その定義に移動できます。 [すべての参照の検索] (Shift + F12) をクリックすると、すべての参照が表示されます。 

    - **Azure パスの挿入**
        
        Azure のファイル パスを憶えていてスクリプトを作成するときに手動で入力しなくても、Data Lake Tools for Visual Studio では、エディターを右クリックして [Azure パスの挿入] をクリックするだけで簡単に挿入できます。 [Azure BLOB ブラウザー] ダイアログでファイルに移動します。 Click **OK**. ファイルのパスは、コードに挿入されます。 

5. Data Lake Analytics アカウント、データベース、スキーマを指定します。 選択できる **(local)** テスト目的でスクリプトをローカルで実行します。 詳細については、次を参照してください。 [実行 U SQL ローカル](#run-u-sql-locally)します。

    ![U-SQL Visual Studio プロジェクトの送信](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

    詳細については、次を参照してください。 [U-SQL カタログ](data-lake-analytics-use-u-sql-catalog.md)します。

5.  **ソリューション エクスプ ローラー**, を右クリックして **Script.usql**, 、クリックして **ビルド スクリプト**します。 [出力] ペインで結果を確認します。
6.  **ソリューション エクスプ ローラー**, を右クリックして **Script.usql**, 、クリックして **送信スクリプト**します。 必要に応じて、クリックすることも **送信** Script.usql ウィンドウからです。  先のスクリーンショットをご覧ください。  詳細オプションを使用して送信するには、[送信] ボタンの横にある下矢印をクリックします。
7. 指定 **ジョブ名**, 、確認、 **Analytics アカウント**, 、] をクリックし、 **送信**します。 送信が完了すると、Data Lake Tools for Visual Studio の [結果] ウィンドウに送信結果とジョブのリンクが示されます。

    ![U-SQL Visual Studio プロジェクトの送信](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)

8. 最新のジョブの状態を表示して、画面を更新するには、[更新] をクリックする必要があります。 いつジョブの成功が表示、 **ジョブ グラフ**, 、**メタ データ操作**, 、**状態の履歴**, 、**診断**:

    ![U-SQL Visual Studio Data Lake Analytics ジョブ パフォーマンス グラフ](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

    * [ジョブの概要]。 現在のジョブの概要情報が表示されます (状態、進行状況、実行時間、ランタイムの名前、送信者など)。   
    * [ジョブの詳細]。 このジョブに関する詳細情報が表示されます (スクリプト、リソース、頂点実行ビューなど)。
    * [ジョブ グラフ]。 ジョブの情報が 4 つのグラフで視覚的に提供されます (進行状況、データの読み取り、データの書き込み、実行時間、ノードごとの平均実行時間、入力スループット、出力スループット)。
    * [メタデータ操作]。 すべてのメタデータ操作が表示されます。
    * [状態の履歴]。 
    * [診断]。 Data Lake Tools for Visual Studio がジョブの実行を自動的に診断します。 エラーやパフォーマンスの問題がジョブにある場合、アラートが表示されます。 詳細についてはジョブの診断 (リンクは TBD) を参照してください。 
    
**ジョブの状態を確認するには**

1. サーバー エクスプ ローラーから、展開 **Azure**, 、展開 **Data Lake 分析**, 、Data Lake 分析アカウント名を展開
2. ダブルクリックして **ジョブ** にジョブを一覧表示します。
2. 状態を確認するにはジョブをクリックします。

**ジョブの出力を表示するには**

1.  **サーバー エクスプ ローラー**, 、展開 **Azure**, 、展開 **Data Lake 分析**, 、Data Lake 分析アカウントを展開し、 **ストレージ アカウント**, 、既定のデータ湖のストレージ アカウントを右クリックし、 **エクスプ ローラー**します。 
2.  ダブルクリックして **出力** フォルダーを開く
3.  ダブルクリックして **adltools.csv からの SearchLog**します。


###ジョブの再生 

ジョブの再生を使用すると、ジョブ実行の進行状況を監視し、視覚的にパフォーマンス上の問題やボトルネックを検出できます。 この機能は、ジョブの実行が完了する前 (つまり、ジョブの実行中) だけでなく実行完了後にも使用できます。 ジョブの実行中に再生を行うと、現時点までの進行状況を再生できます。 

**ジョブの実行の進行状況を表示するには**  

1. クリックして **ロード プロファイル** 右上隅にします。 前のスクリーンショットを参照してください。
2. 左下隅の [再生] ボタンをクリックすると、ジョブの実行の進行状況が表示されます。 
3. クリックして、再生中に **一時停止** するか、停止するか、直接には、特定の位置の進行状況バーをドラッグします。 


###ヒート マップ 

Data Lake Tools for Visual Studio にはユーザーが選択できるカラー オーバーレイがあり、各ステージの進行状況、データ I/O、実行時間、I/O スループットがジョブ ビューに表示されます。 これにより、ジョブのプロパティの潜在的な問題と分布が直接かつ直観的にわかります。 ドロップダウン リストから表示するデータ ソースを選択できます。  

## U-SQL のローカル実行

Visual Studio で U-SQL をローカル実行し、次のことができます。 

- C# アセンブリと共に U-SQL スクリプトをローカル実行します。 
- ローカルで C# アセンブリをデバッグします。 
- Azure Data Lake Analytics サービスの場合と同様に、サーバー エクスプローラーでローカルのテーブル、アセンブリを表示します。 

表示されます、 *ローカル* 、インストーラーを Visual Studio でアカウントを作成、 *タグ* フォルダーにある *C:\LocalRunRoot*します。 DataRoot フォルダーが使用されます。

- ストア メタデータを含むテーブルが、Db、Tvf など。
- 特定のスクリプトの場合: 入力/出力パスで相対パスが参照される場合、DataRoot が検索されます (入力の場合は、スクリプトのパスも)。
- アセンブリを登録し、相対パスを利用しようとすると、DataRoot フォルダーは参照されません (詳細については、「ローカル実行時にアセンブリを使用する」を参照してください)。

### 既知の問題と制限

- U-SQL ローカル実行では、ファイルセットにローカルでクエリを実行できません。 参照してください [U SQL ファイルセット](https://msdn.microsoft.com/library/azure/mt621294.aspx)します。 これは将来解決されます。
- ジョブ プランが 1 回のプロセスで連続的に実行されるため、並列性が低く、パフォーマンスが低下します。 
- Visual Studio では、ローカル実行のジョブ グラフが表示されませ。 これは将来解決されます。 
- ローカル アカウントに対し、サーバー エクスプローラーでテーブル/DB などを作成できません。
- 相対パスが参照されている場合:

    - スクリプト入力で (EXTRACT * FROM “/path/abc”) - DataRoot パスとスクリプト パスの両方が検索されます。 
    - スクリプト出力で (OUTPUT TO “path/abc”): DataRoot パスが出力フォルダーとして使用されます。 
    - アセンブリ登録で (CRREATE ASSEMBLY xyz FROM “/path/abc”): スクリプト パスが検索されます。DataRoot パスではありません。 
    - 登録された TVF/View または他のメタデータ エンティティで: DataRoot パスが検索されます。スクリプト パスではありません。 
    
    Data Lake サービスで実行されたスクリプトの場合、既定のストレージ アカウントがルート フォルダーとして使用され、適宜検索されます。

### U-SQL スクリプトのローカル テスト
U SQL スクリプトを開発する方法については、次を参照してください。 [開発の U SQL スクリプト](#develop-and-test-u-sql-scripts)します。 ビルドを U SQL スクリプトをローカルで実行するには、次のように選択します。 **(Local)** 、クラスター内には、ドロップ ダウン リスト、クリックして **送信**します。 正しいデータを参照していることを確認してください。絶対パスを参照するか、DataRoot フォルダーの下にデータを配置します。 

![U-SQL Visual Studio プロジェクトのローカル送信](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-local-run.png)

スクリプトを右クリックし、をクリックして **ローカル計画の実行** でコンテキスト メニューのまたはキーを押して **ctrl キーを押しながら f5 キー** トリガーのローカルに実行します。

### ローカル実行でアセンブリを使用する

カスタマイズされた C# ファイルを実行する方法は 2 つあります。

- 分離コード ファイルにアセンブリを記述します。アセンブリは自動的に登録され、スクリプトが完了すると削除されます。 
- C# アセンブリ プロジェクトを作成し、下のようなスクリプトを利用し、ローカル アカウントに出力 dll を登録します。 パスは、DataRoot フォルダーではなく、スクリプトに相対となることに注意してください。

![U-SQL ローカル実行でアセンブリを使用する](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-local-run-assembly.png)
 
### スクリプトと C# アセンブリをローカルでデバッグする

Azure Data Lake Analytics サービスに提出し、登録しなくても C# アセンブリをデバッグできます。 分離コード ファイルと参照 C# プロジェクトの両方にブレークポイントを設定できます。

**ローカルのコードの分離コード ファイルをデバッグするには**
1.  分離コード ファイルにブレークポイントを設定します。 
2.  キーを押して **f5 キーを押して** ローカルでスクリプトをデバッグします。

次の手順は、Visual Studio 2015 でのみ機能します。 以前の Visual Studio では、pdb ファイルを手動で追加する必要があります。 

**参照先の c# プロジェクトのローカルのコードをデバッグするには**
1.  C# アセンブリ プロジェクトを作成してビルドし、出力 dll を生成します。
2.  U-SQL ステートメントを使用して dll を登録します。

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
3.  C# コードにブレークポイントを設定します。
4.  キーを押して **f5 キーを押して** c# dll ローカルでの参照を含むスクリプトをデバッグします。  
 








##関連項目

さまざまなツールを使用して Data Lake Analytics の使用を開始する方法については、以下をご覧ください。

- [Azure ポータルで Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-portal.md)
- [Azure PowerShell で Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-powershell.md)
- [.NET SDK で Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-net-sdk.md)

開発に関するその他のトピックも参照してください。 

- [Analyze weblogs using Data Lake Analytics (Data Lake Analytics を使用したウェブログの分析)](data-lake-analytics-analyze-weblogs.md)
- [Data Lake Tools for Visual Studio を使用する U-SQL スクリプトの開発](data-lake-analytics-data-lake-tools-get-started.md)
- [Azure Data Lake Analytics U-SQL 言語の使用](data-lake-analytics-u-sql-get-started.md)
- [Data Lake Analytics ジョブの U-SQL ユーザー定義演算子の開発](data-lake-analytics-u-sql-user-defined-operators.md)


