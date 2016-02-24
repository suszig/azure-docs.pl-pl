<properties
    pageTitle="DocumentDB Document Explorer を使用した JSON ドキュメントの表示、編集、作成、およびアップロード | Microsoft Azure"
    description="DocumentDB で JSON ドキュメントを表示、編集、作成、アップロードするための Azure ポータル ツールである DocumentDB Document Explorer について説明します。"
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
    ms.date="10/26/2015"
    ms.author="anhoh"/>

# DocumentDB Document Explorer を使用した JSON ドキュメントの表示、編集、作成、およびアップロード #

この記事の概要を説明する、 [Microsoft Azure DocumentDB](http://azure.microsoft.com/services/documentdb/) ドキュメント エクスプ ローラーを表示するには、Azure ポータル ツールの編集、作成、および使用して JSON ドキュメントをアップロードします。

このチュートリアルを完了すると、次の質問に回答できるようになります。  

-   Web ブラウザーを使用して個々の DocumentDB ドキュメントを簡単に作成、表示、編集、および削除するにはどうすればよいか。
-   Web ブラウザーを使用して DocumentDB ドキュメントのシステム プロパティを簡単に表示するにはどうすればよいか。
-   Web ブラウザーを使用して DocumentDB へのドキュメントの一括インジェストを簡単に実行するにはどうすればよいか。

##<a id="Launch"></a>Document Explorer を起動します。##

Document Explorer は、DocumentDB アカウント、データベース、およびコレクションのブレードのいずれからも起動することができます。  

1. DocumentDB アカウントまたはデータベース ブレードの上部にあるをクリックするだけの **Document Explorer** コマンドです。

    ![Document Explorer コマンドのスクリーンショット](./media/documentdb-view-JSON-document-explorer/documentexplorercommand.png)
 
2. または、各ブレードの下部には、 **Developer Tools** レンズを含む、 **Document Explorer** 部分です。

    ![Document Explorer パーツのスクリーンショット](./media/documentdb-view-JSON-document-explorer/documentexplorerpart.png)

2. タイルをクリックするだけで、Document Explorer が起動します。

    <p> **データベース** と **コレクション** ドロップダウン リスト ボックスには、Document Explorer を起動した状況に応じて事前設定されます。  たとえば、データベース ブレードから起動した場合には、現在のデータベースが設定されます。  コレクション ブレードから起動した場合には、現在のコレクションが設定されます。

    ![Document Explorer のスクリーンショット](./media/documentdb-view-JSON-document-explorer/documentexplorerinitial.png)

##<a id="Create"></a>Document Explorer を使用したドキュメントの表示、作成、および編集##

Document Explorer では、ドキュメントを簡単に作成、編集、および削除することができます。  

- ドキュメントを作成するにはクリックして、 **ドキュメントの作成** コマンドと、最小限の JSON スニペットが提供されます。

    ![Document Explorer のドキュメントの作成操作のスクリーンショット](./media/documentdb-view-JSON-document-explorer/createdocument.png)

- 入力するか、作成し、をクリックする、ドキュメントの JSON コンテンツを貼り付けるだけで、 **保存** 、ドキュメントがコミットするコマンドです。

    ![Document Explorer の [保存] コマンドのスクリーンショット](./media/documentdb-view-JSON-document-explorer/savedocument1.png)

    > [AZURE.NOTE] "Id"プロパティを指定しない場合は、Document Explorer 自動的に id プロパティが追加し、id の値として GUID が生成されます。

- JSON ファイル、MongoDB、SQL Server、CSV ファイル、Azure テーブル ストレージ、Amazon DynamoDB、HBase からデータがあるか他の DocumentDB コレクションから DocumentDB を使用する [データ移行ツール](documentdb-import-data.md) 、データをすばやくインポートします。

- 既存のドキュメントを編集するに簡単にドキュメント エクスプ ローラーで選択して、ドキュメントを編集する] をクリックし、自由に、 **保存** コマンドです。

    ![Document Explorer のドキュメントの編集機能のスクリーンショット](./media/documentdb-view-JSON-document-explorer/editdocument.png)

- ドキュメントの編集中に現在の編集内容を破棄する必要がある場合は、破棄コマンドをクリックして破棄アクションを確認するたけで、前の状態のドキュメントが再度読み込まれます。

    ![Document Explorer の [破棄] コマンドのスクリーンショット](./media/documentdb-view-JSON-document-explorer/discardedit.png)

- クリックすると、それを選択してドキュメントを削除することができます、 **削除** コマンド、および削除を確認します。 確認後、ドキュメントは Document Explorer の一覧から即座に削除されます。

    ![Document Explorer の [削除] コマンドのスクリーンショット](./media/documentdb-view-JSON-document-explorer/deletedocument.png)

- Document Explorer では、新規または編集されたドキュメントに有効な JSON が含まれているかどうかが検証されます。  不適切なセクションをマウスでポイントして、検証エラーの詳細を表示することもできます。

    ![無効な JSON が強調表示された Document Explorer のスクリーンショット](./media/documentdb-view-JSON-document-explorer/invalidjson1.png)

- また、Document Explorer では、無効な JSON コンテンツを含むドキュメントを保存できません。

    ![Document Explorer の無効な JSON の保存エラーのスクリーンショット](./media/documentdb-view-JSON-document-explorer/invalidjson2.png)

- 最後に、Document Explorer を使用すると、簡単をクリックして、現在読み込まれているドキュメントのシステム プロパティを表示、 **プロパティ** コマンドです。

    ![Document Explorer のドキュメントのプロパティ ビューのスクリーンショット](./media/documentdb-view-JSON-document-explorer/documentproperties.png)

    > [AZURE.NOTE] タイムスタンプ (_ts) プロパティが内部的にエポック時間として表されますが、Document Explorer では、人間が読める GMT 形式で値が表示されます。

##<a id="Navigate"></a>ドキュメント エクスプ ローラーのナビゲーション オプションと高度な設定##

Document Explorer はさまざまなナビゲーション オプションと高度な設定を備えています。

1. 既定では、Document Explorer は選択されたコレクション内の最初の最大 100 個 (作成日が早いもの順) のドキュメントを読み込みます。  選択して追加のドキュメント (で 100 個単位) を読み込むことができます、 **の詳細を読み込む** Document Explorer ブレードの下部にあるオプション。  既定の動作を変更するには、Document Explorer ブレードの上部にある [設定] コマンドをクリックします。

    ![Document Explorer 設定ブレードのスクリーンショット](./media/documentdb-view-JSON-document-explorer/documentexplorersettings.png)


2. 設定ブレードでは、ページごとに返す項目数を調整したり、WHERE 句を指定して Document Explorer グリッドで一致するドキュメントを読み込んだりすることができます。  詳細については、DocumentDB SQL の文法 [ここ](documentdb-sql-query.md)します。

    ![Document Explorer 設定ブレードのスクリーンショット](./media/documentdb-view-JSON-document-explorer/documentexplorersettings2.png)

    > [AZURE.NOTE] クリックする必要があります Document Explorer 設定を変更したら、 **更新** 新しい設定を適用するためにコマンドです。  設定は、現在のブラウザー セッションでのみ維持されます。
    
3.  **データベース** と **コレクション** 元となる、現在表示されているドキュメントを閉じるし、Document Explorer を再起動しなくてもコレクションを簡単に変更するのにはドロップダウン リスト ボックスを使用できます。  

4. Document Explorer では、現在読み込まれているドキュメントのセットを ID プロパティでフィルター処理することもできます。  その方法は、フィルター ボックスに入力するだけです。

    ![フィルターが強調表示された Document Explorer のスクリーンショット](./media/documentdb-view-JSON-document-explorer/documentexplorerfilter.png)

    Document Explorer の一覧の結果は、指定された条件に基づいてフィルター処理されます。

    ![フィルターの結果が表示された Document Explorer のスクリーンショット](./media/documentdb-view-JSON-document-explorer/documentexplorerfilterresults.png)


    > [AZURE.IMPORTANT] The Document Explorer filter functionality only filters from the ***currently*** loaded set of documents and does not perform a query against the currently selected collection.

5. Document Explorer に読み込まれたドキュメントの一覧を更新するだけでクリックして、 **更新** ブレードの上部にあるコマンドです。

    ![Document Explorer の [最新の情報に更新] コマンドのスクリーンショット](./media/documentdb-view-JSON-document-explorer/documentexplorerrefresh.png)

##<a id="BulkAdd"></a>Document Explorer を使用したドキュメントの一括追加##

Document Explorer では、1 つまたは複数の既存の JSON ドキュメントの一括インジェストをサポートしています。  

1. アップロード プロセスを開始する] をクリックして、 **文書に追加** コマンドです。

    ![Document Explorer の一括取り込み機能のスクリーンショット](./media/documentdb-view-JSON-document-explorer/adddocument1.png)

2. 新しいブレードが開きます。  [参照] をクリックしてエクスプローラー ウィンドウを開き、アップロードする JSON ドキュメントを 1 つ以上選択します。

    ![Document Explorer の一括取り込みプロセスのスクリーンショット](./media/documentdb-view-JSON-document-explorer/adddocument2.png)

    > [AZURE.NOTE] Document Explorer では、アップロード処理ごとに最大 100 個の JSON ドキュメントがサポートされています。

3. 期待どおりであれば、選択、したら、 **アップロード** ] ボタンをクリックします。  ドキュメントが自動的に Document Explorer グリッドに追加されます。操作の進捗状況に合わせてアップロード結果が表示されます。 インポートが失敗すると、ファイルごとに報告されます。

    ![Document Explorer の一括取り込みの結果のスクリーンショット](./media/documentdb-view-JSON-document-explorer/adddocument3.png)

4. 操作が完了すると、アップロードする別のドキュメントを最大 100 個選択することができます。

##<a name="NextSteps"></a>次のステップ

- DocumentDB の詳細については、次のようにクリックします。 [ここ](http://azure.com/docdb)します。
- クリックしてコードで、 [ここ](documentdb-get-started.md)します。

 

