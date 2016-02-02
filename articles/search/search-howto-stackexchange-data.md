<properties
    pageTitle="Azure Search を使用して StackExchange のデータを検索する方法 | Microsoft Azure | ホスト型クラウド検索サービス"
    description="Microsoft Azure のホスト型クラウド検索サービスである Azure Search を利用し、REST 検索を実行する方法について説明します。"
    services="search"
    documentationCenter=""
    authors="liamca"
    manager="pablocas"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="11/04/2015"
    ms.author="liamca"/>


# Azure Search を使用して StackExchange のデータを検索する方法

この記事はチュートリアルを行うことができますに基本のフルテキスト検索機能の一部をまとめてを [Azure Search](https://azure.microsoft.com/services/search/)します。 スタックの交換が行われたデータを利用して [利用可能な](https://archive.org/details/stackexchange) Creative Commons の使用方法を次の [帰属](http://blog.stackoverflow.com/2009/06/attribution-required/)します。

## 使用の開始

これらの機能の一部を強調するために、ユーザーが操作するための Azure Search インデックスを作成しました。このインデックスには、2015 年 10 月 14 日の時点での Programmer StackExchange のデータが含まれています。 注: ここでは、このデータを使用して独自の Azure Search インデックスを作成する方法についても後で説明します。

まず、非常に簡単なフルテキスト検索クエリから始めましょう。この検索クエリでは、ユーザーが「azure」という単語を入力すると、Azure に関連する StackExchange の投稿が見つかります。 次のリンクをクリックしてこれを試し、実際の動作を確認してください。

> [http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=stackexchange&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28%26search=azure](http://fiddle.jshell.net/liamca/gkvfLe6s/?index=stackexchange&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28%26search=azure)

この例では、"azure" という単語を検索パラメーターとして渡すだけで、返された結果が JSON 形式で表示されます。 その他のクエリの例を次に示すので、お試しください。

-   `ファセット`: データセットを検索すると、データをフィルター処理は結果を表示するために役立ちます。 これを実装するには、通常、ユーザーに表示される一連のカテゴリ (ファセット) を利用します。 次に、ここで利用できるファセットの例をいくつか示します。
  - **Tags**: 質問の多くにはタグが関連付けられているため、ユーザーは特定のカテゴリを掘り下げることができます。
  - **Dates**: ユーザーは、特定の期間に投稿または回答された質問のみを表示することができます。
  - **ユーザー**: を参照してください、特定のユーザーからの結果を制限したりすることがあります
この例では"azure"を検索しましたが tagsCollection と acceptedAnswerDisplayName ユーザー名の場合、ファセットのカウントを返すされます。

> <http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=stackexchange&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28%26search=azure%26facet=tagsCollection%26facet=acceptedAnswerDisplayName>

-   `フィルタ リング`: ユーザーがファセットを選択した後は、する別の検索を実行しますが、そのファセットの値に結果を制限するフィルターを活用します。 たとえば、"Azure" の検索結果を "architecture" というタグが付いた結果に限定し、viewCount の降順で並べ替えます。

> <http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=stackexchange&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28%26search=azure%26$filter=tagsCollection/any(t:+t+eq+'architecture')%26$orderby=viewCount+desc>

-   `スペルの誤り`: の新しい (プレビュー) をサポート [Lucene クエリ式](https://msdn.microsoft.com/library/mt589323.aspx) あいまい結果の一致と特定のフィールドに対する検索を制限するなどのいくつかのとてもおしゃれなクエリを実行することもできます。 次の例では、タイトル フィールドで "visualize" という単語を検索しますが、~ はあいまい一致を示すため、"visualise" や "visualizing" などの結果も返されます。

> <http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=stackexchange&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28&search%3Dtitle%3Avisualise~%26querytype%3Dfull%26searchMode%3Dall%26%24select%3Dtitle>

-   `スコア付けと調整`: [スコアリング プロファイル](https://msdn.microsoft.com/library/azure/dn798928.aspx) が search サービスによって返される結果を調整するために非常に役に立ちます。 実際には、利用できるぞも [Lucene クエリ式](https://msdn.microsoft.com/library/mt589323.aspx) を個別のフィールドと、その場での条件にスコア付けを適用します。 たとえば、タイトル フィールドで "visualize" または "chart" という単語を検索したいが、その中でも "chart" という単語が含まれる項目により重みを付ける場合は、次のようになります。

> <http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=stackexchange&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26search%3Dtitle%3Avisualise+OR+title%3Achart%5E3+%26querytype%3Dfull%26searchMode%3Dall%26%24select%3Dtitle>

  このデータセットには、ユーザーに関連した結果を増やすために使用できるフィールドがほかにも多数あります。 たとえば、次のフィールドを使用できます。

  - **Magnitude** は、answerCount、commentCount、favoriteCount、viewCount などの数値フィールドでスコアリングし、数が多くなった場合に検索結果が増加します。
  - **Freshness** は、creationDate や lastActivityDate などの日時フィールドでスコアリングし、作成された項目や最近アクティブになった項目をブーストします。
  - **フィールドの重み**は、質問の本文内で検索テキストが見つかった場合、回答で見つかった場合よりも関連性が高くなることを示します。

ほかに使用できるものを次に示します。

-   [`提案`] (https://msdn.microsoft.com/library/azure/mt131377.aspx): ユーザーが検索ボックスに入力するには便利にオートコンプリート機能のタイトル、タグ、ユーザー名のようなフィールドを使用します。

-   `の推奨事項`: 多くの場合、Apache Mahout などのツールが必要か、Azure Machine Learning のようなユーザーからの問い合わせを表示するための推奨事項を作成するためには、表示、利用可能性があるが幸運にもこのデータセットは既にいくつかの推奨事項です。

さまざまな種類のクエリを試すには、次の JSFiddle ページを自由にお使いください。 このインデックスの作成方法の詳細については、以下を参照してください。 Twitter アカウントに直接自分に通知するためにも気軽 [@liamca](https://twitter.com/liamca)します。

## この Azure Search インデックスの作成方法

Brent は、SQL データベースにデータをステージングする方法を示すことで、面倒な作業の多くを既に行いました。 このプロセスのデータのステージングを参照する場合は、彼を確認してください [チュートリアル](http://www.brentozar.com/archive/2014/01/how-to-query-the-stackexchange-databases/)します。 それ以外の場合は、2015 年 10 月 15 日以降に、一部のデータが事前に設定された Azure SQL データベースをご利用いただくか、ここで作成した Azure Search インデックスをお試しいただくことができます。 この詳細については、ステージングされた Azure SQL Database からのデータのインポートに関するセクションで後述します。  
Brent が説明されているを超えて行ったことだけがによって使用される Azure SQL データベースのビューを作成するが、 [Azure Search インデクサー](https://msdn.microsoft.com/library/azure/dn946891.aspx) クロールしだということに興味があるテーブルのグループからデータを取り込みます。 このビューがどのように定義されているかを次に示します。

    CREATE VIEW StackExchangeSearchData as
    SELECT
          PQ.[Id]
          ,PQ.[AcceptedAnswerId]
          ,PQ.[AnswerCount]
          ,PQ.[Body]
          ,PQ.[ClosedDate]
          ,PQ.[CommentCount]
          ,PQ.[CommunityOwnedDate]
          ,PQ.[CreationDate]
          ,PQ.[FavoriteCount]
          ,PQ.[LastActivityDate]
          ,PQ.[LastEditDate]
          ,PQ.[LastEditorDisplayName]
          ,PUQ.DisplayName OwnerDisplayName
          ,PUQ.Reputation OwnerReputation
          ,PQ.[Score]
          ,reverse(REPLACE(LTRIM(REPLACE(reverse(REPLACE(REPLACE (PQ.[Tags], '>' , '],' ), '<' , '[' )), ISNULL(',', '0'), ' ')), ' ', ISNULL(',', '0'))) TagsCollection
          ,PQ.[Title]
          ,PQ.[ViewCount]
          ,PA.[Body] AcceptedAnswerBody
          ,PA.[Score] AcceptedAnswerScore
          ,PUQ.DisplayName AcceptedAnswerDisplayName
          ,PUQ.Reputation AcceptedAnswerReputation
          ,PA.[FavoriteCount] AcceptedAnswerFavoriteCount
          ,PL.[RelatedPostId]
      FROM [StackExchange].[dbo].[Posts] PQ
      LEFT OUTER JOIN [StackExchange].[dbo].[Posts] PA
      and PQ.AcceptedAnswerId = PA.Id
      LEFT OUTER JOIN [StackExchange].[dbo].[PostLinks] PL
      on PQ.Id = PL.Id
      LEFT OUTER JOIN [StackExchange].[dbo].[Users] PUQ
      on PQ.OwnerUserId = PUQ.Id
      LEFT OUTER JOIN [StackExchange].[dbo].[Users] PUA
      on PA.[OwnerUserId] = PUA.Id
      WHERE PQ.PostTypeId = 1

これが完了することができますしを使用して、 [Azure Classic Portal](https://portal.azure.com) ビュー内のフィールドのスキーマに基づいて、Azure Search インデックスを作成し、上記の Azure SQL のビューから"インポート Data"にします。 ステージングした Azure SQL データベースを使用する場合に利用できる読み取り専用の接続文字列を次に示します。

    Server=tcp:azs-playground.database.windows.net,1433;Database=StackExchange;User ID=reader@azs-playground;
    Password=EdrERBt3j6mZDP;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;



