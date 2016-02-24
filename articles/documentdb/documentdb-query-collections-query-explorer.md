<properties 
    pageTitle="クエリ エクスプローラーを使用した、DocumentDB コレクションに対する SQL クエリの作成、編集、実行 | Microsoft Azure" 
    description="DocumentDB コレクションに対する SQL クエリを作成、編集、実行するための Azure ポータル ツールである DocumentDB クエリ エクスプローラーについて説明します。" 
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="12/01/2015" 
    ms.author="anhoh"/>

# クエリ エクスプローラーを使用した、DocumentDB コレクションに対する SQL クエリの作成、編集、実行 #

この記事の概要を説明する、 [Microsoft Azure DocumentDB](http://azure.microsoft.com/services/documentdb/) クエリ エクスプ ローラーを作成するには、Microsoft Azure ポータルのツールは、編集、および DocumentDB コレクションに対してクエリを実行します。 

このチュートリアルを完了すると、次の質問に回答できるようになります。  

-   Web ブラウザーを使用して DocumentDB コレクションに対するクエリを簡単に作成、編集、実行するにはどうすればよいか。
-   Web ブラウザーを使用して DocumentDB のクエリ結果ページ間を簡単に移動するにはどうすればよいか。
-   DocumentDB クエリの構文エラーをトラブルシューティングするにはどうすればよいか。 

##<a id="Launch"></a>クエリ エクスプローラーの起動と移動##

クエリ エクスプローラーは、DocumentDB のアカウント、データベース、コレクションのいずれかのブレードから起動することができます。
  
1. DocumentDB アカウントまたはデータベース ブレードの上部にあるをクリックするだけの **クエリ エクスプ ローラー** コマンドです。

    ![クエリ エクスプローラー コマンドのスクリーンショット](./media/documentdb-query-collections-query-explorer/queryexplorercommand.png)

2. または、各ブレードの下部には、 **Developer Tools** レンズが含まれています、 **クエリ エクスプ ローラー** を並べて表示します。
    
    ![クエリ エクスプローラーのパーツのスクリーンショット](./media/documentdb-query-collections-query-explorer/queryexplorerpart.png) 

2. このタイルをクリックすると、クエリ エクスプローラーが起動します。

     **データベース** と **コレクション** ドロップダウン リスト ボックスには、クエリ エクスプ ローラーを起動した状況に応じて事前設定されます。  たとえば、データベース ブレードから起動した場合には、現在のデータベースが設定されます。 コレクション ブレードから起動した場合には、現在のコレクションが設定されます。

    ![クエリ エクスプローラーのスクリーンショット](./media/documentdb-query-collections-query-explorer/queryexplorerinitial.png)

##<a id="Create"></a>クエリ エクスプローラーを使用したクエリの作成、編集、実行##

クエリ エクスプローラーを使用すると、DocumentDB コレクションに対するクエリを簡単に作成、編集、実行することができ、基本的なキーワードや値を含めて強調表示できるので、クエリの作成エクスペリエンスが向上します。  

- 最初にクエリ エクスプローラーを開くと、既定のクエリ SELECT * FROM 句が表示されます。  既定のクエリをそのまま使用または独自に作成し、クリックして、 **クエリを実行** 結果を表示するボタンをクリックします。 クエリ エクスプ ローラーは、」の説明に従って、DocumentDB SQL クエリ言語をサポートしている [DocumentDB のクエリ](documentdb-sql-query.md)します。

    ![クエリ エクスプローラーのクエリ結果のスクリーンショット](./media/documentdb-query-collections-query-explorer/queryresults1.png) 

- 複数のクエリを実行する 1 つをクリックおよびクリックを入力することも、 **クエリを実行** 結果を表示するボタンをクリックします。

    ![クエリ エクスプローラーの強調表示と実行のスクリーンショット](./media/documentdb-query-collections-query-explorer/queryexplorerhighlightandrun.png) 

- ファイル使用して、既存の内容を読み込むことができます、 **ファイルの読み込み** コマンドです。

    ![クエリ エクスプローラーの [ファイルの読み込み] のスクリーンショット](./media/documentdb-query-collections-query-explorer/loadqueryfile.png) 

- 既定では、クエリ エクスプローラーは 100 個ずつ結果を返します。  場合は、クエリの結果が 100 を超える個を単に使用して、 **次のページ** と **前のページ** 結果セット内を移動するためのコマンドです。

    ![クエリ エクスプローラーの改ページ位置の自動調整のサポート](./media/documentdb-query-collections-query-explorer/queryresultspagination.png)

- 成功したクエリ要求の課金の現在の結果セットが表示されているなど、や経由でアクセスすることが多くの結果があるかどうかについても説明、 **次のページ** コマンドは、既に説明したようにします。

    ![クエリ エクスプローラーのクエリ情報のスクリーンショット](./media/documentdb-query-collections-query-explorer/queryinformation.png)

- 同様に、クエリがエラーになった場合には、トラブルシューティングに役立つようなエラーの一覧がクエリ エクスプローラーに表示されます。

    ![クエリ エクスプローラーのクエリ エラーのスクリーンショット](./media/documentdb-query-collections-query-explorer/queryerror.png)

##<a name="NextSteps"></a>次のステップ

- DocumentDB の詳細については、次のようにクリックします。 [ここ](http://azure.com/docdb)します。
- クエリ エクスプ ローラーでサポートされている DocumentDB SQL 文法の詳細については、次のようにクリックします。 [ここ](documentdb-sql-query.md)します。
 

