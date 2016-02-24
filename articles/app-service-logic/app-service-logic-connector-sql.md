<properties
   pageTitle="Logic Apps での SQL コネクタの使用 | Microsoft Azure App Service"
   description="SQL コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="11/30/2015"
   ms.author="sameerch"/>


# Microsoft SQL コネクタの使用開始とロジック アプリへの追加
オンプレミスの SQL Server または Azure SQL Database に接続し、情報またはデータの作成と変更を行います。 コネクタを Logic Apps で使用して、"ワークフロー" の一部としてデータを取得、処理、またはプッシュできます。 SQL コネクタをワークフローで使用すると、さまざまなシナリオを実現できます。 たとえば、次のようなことができます。

- Web またはモバイル アプリケーションを使用して、SQL データベースに存在するデータの一部を公開する。
- ストレージ用の SQL データベース テーブルにデータを挿入する。 たとえば、従業員レコードの入力や販売注文の更新などを実行できます。
- データを SQL から抽出してビジネス プロセスで使用する。 たとえば、顧客レコードを取得し、SalesForce にそれらの顧客レコードを書き込むことができます。

この SQL コネクタをビジネス ワークフローに追加し、ロジック アプリ内のそのワークフローの一部としてデータを処理できます。 

## トリガーとアクション
*トリガー* は、発生するイベントです。 たとえば、注文が更新された、新しい顧客が追加された、といったイベントがあります。  *アクション* トリガーの結果を示します。 たとえば、注文が更新されたときに、営業担当者にアラートを送信します。 または、新しい顧客が追加されたときに、ウェルカム メールを新しい顧客に送信します。

SQL コネクタは、ロジック アプリでトリガーまたはアクションとして使用でき、JSON 形式と XML 形式のデータをサポートします。 パッケージ設定に含まれるすべてのテーブルに対して (詳細はこのトピックで後述します)、JSON アクションと XML アクションのセットがあります。

SQL コネクタでは、次のトリガーとアクションを使用できます。

トリガー | アクション
--- | ---
データのポーリング | <ul><li>テーブルに挿入</li><li>テーブルの更新</li><li>テーブルから選択</li><li>テーブルから削除</li><li>ストアド プロシージャの呼び出し</li></ul>

## SQL コネクタを作成する

コネクタは、ロジック アプリ内で作成することも、Azure Marketplace から直接作成することもできます。 Marketplace からコネクタを作成するには、次の操作を実行します。  

1. Azure のスタート画面では、次のように選択します。 **Marketplace**します。
2. 「SQL コネクタ」を検索して選択し、選択 **作成**します。
3. 名前、App Service プラン、その他のプロパティを入力します。
4. 次のパッケージの設定を入力します。

    名前 |必要な | 説明
--- | --- | ---
サーバー名 |[はい |SQL Server の名前を入力します。 たとえば、入力 *sqlserver/sqlexpress* または *SQLserver.mydomain.com*します。
ポート |いいえ |既定は 1433年です。
ユーザー名 |[はい |SQL Server にログインできるユーザー名を入力します。 オンプレミスの SQL Server に接続する場合は、SQL 認証資格情報を入力します。
パスワード |[はい |ユーザー名とパスワードを入力します。
データベース名 |[はい |接続しているデータベースを入力します。 たとえば、入力 *顧客* または *dbo/注文*します。
内部設置型 |[はい |既定値は False です。 Azure SQL データベースに接続する場合は False を入力します。 オンプレミスの SQL Server に接続する場合は True を入力します。
Service Bus 接続文字列 |いいえ |オンプレミスに接続する場合は、Service Bus relay の接続文字列を入力してください<br/><br/>[。ハイブリッド接続マネージャーを使用して](app-service-logic-hybrid-connection-manager.md)<br/>[サービス バスの料金](http://azure.microsoft.com/pricing/details/service-bus/)
パートナー サーバー名 |いいえ |プライマリ サーバーが使用できない場合は、代替またはバックアップ サーバーとしてパートナー サーバーを入力できます。
テーブル |いいえ |コネクタによって更新可能なデータベース テーブルを一覧表示します。 たとえば、入力 *OrdersTable* または *EmployeeTable*します。 テーブルが入力されない場合はすべてのテーブルを使用できます。 このコネクタをアクションとして使用するには有効なテーブルまたはストアド プロシージャが必要です。
ストアド プロシージャ |いいえ |コネクタで呼び出すことができる既存のストアド プロシージャを入力します。 たとえば、入力 *sp_IsEmployeeEligible* または *sp_CalculateOrderDiscount*します。 このコネクタをアクションとして使用するには有効なテーブルまたはストアド プロシージャが必要です。
データの存在確認クエリ |トリガーは、サポート |SQL ステートメントを使用して、すべてのデータが SQL Server データベース テーブルのポーリングに使用できるかどうかを確認します。 これは、存在するデータの行数を表す数値を返します。 例: SELECT COUNT(*) from table_name。
データ ポーリング クエリ | トリガーのサポート | SQL Server データベース テーブルをポーリングするための SQL ステートメント。任意の数の SQL ステートメントをセミコロンで区切って指定できます。このステートメントはトランザクション的に実行され、データがロジック アプリで安全に保存される場合にのみコミットされます。例: SELECT * FROM table_name;DELETE FROM table_name。<br/><br/>**注**<br/>削除することによって、無限ループを回避するポーリング ステートメントを指定する必要があります、移動、または選択したデータを同じデータの更新が再度ポーリングされません。

5. 完了すると、パッケージの設定は、次のようになります。  
![][1]  

6. 選択 **作成**します。 


## コネクタをトリガーとして使用する
SQL テーブルからデータをポーリングし、そのデータを別のテーブルに追加してデータを更新する単純なロジック アプリを見てみましょう。

SQL コネクタをトリガーとして使用する入力、 **データの存在確認クエリ** と **データ ポーリング クエリ** 値。 **データの存在確認クエリ** が入力したスケジュールに従って実行され、すべてのデータが使用できるかどうかをします。 このクエリはスカラー値のみを返すため、頻繁に実行しても問題のないように調整および最適化することができます。

**データ ポーリング クエリ** データの存在確認クエリでは、使用可能なデータが示されている場合にのみ実行します。 このステートメントはトランザクション内で実行され、抽出されたデータがワークフローで永続的に保存される場合にのみコミットされます。 同じデータが無限に再抽出されないようにすることが重要です。 この実行のトランザクション的な性質を使用してデータを削除するか更新することで、次回データがクエリされるときに収集されないようにすることができます。

> [AZURE.NOTE] このステートメントによって返されたスキーマでは、コネクタで使用可能なプロパティを識別します。 すべての列に名前を付ける必要があります。

#### データの存在確認クエリの例

    SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### データ ポーリング クエリの例

    SELECT *, GetData() as 'PollTime' FROM [Order]
        WHERE OrderStatus = 'ProcessedForCollection'
        ORDER BY Id DESC;
    UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
        WHERE Id =
        (SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### トリガーを追加する
1. ロジック アプリを作成または編集するときに、トリガーとして作成した SQL コネクタを選択します。 使用できるトリガーが一覧表示されます: **Poll Data (JSON)** と **Poll Data (XML)**:  
![][5]

2. 選択、 **Poll Data (JSON)** トリガー、頻度を入力し、✓] をクリックします。  
![][6]

3. これで、トリガーは、ロジック アプリで構成されたものとして表示されます。 トリガーの出力が表示され、後続のアクションの入力として使用できます。  
![][7]

## コネクタをアクションとして使用する
SQL テーブルからデータをポーリングする単純なロジック アプリのシナリオを使用して、データを別のテーブルに追加し、データを更新します。

SQL コネクタをアクションとして使用するには、SQL コネクタを作成したときに入力したテーブルまたはストアド プロシージャの名前を入力します。

1. トリガーの後ろに (または [このロジックを手動で実行] を選択して)、作成した SQL コネクタをギャラリーから追加します。 このような挿入操作のいずれかを選択 *Insert Into TempEmployeeDetails (JSON)*:  
![][8]

2. 挿入して、[✓] をクリックするレコードの入力値を入力します。  
![][9]

3. ギャラリーから、作成済みの同じ SQL コネクタを選択します。 このような選択、同じテーブルに対する更新アクションをアクションとして *Update EmployeeDetails*:  
![][11]

4. 更新アクションの入力値を入力し、[✓] をクリックします。  
![][12]

ポーリングされるテーブルに新しいレコードを追加すると、ロジック アプリをテストできます。

## 可能な処理と不可能な処理

SQL クエリ | サポートされています | サポートされていません
--- | --- | ---
WHERE 句 | <ul><li>演算子:、または、=、<>、<、< =、>、> = し、などの</li><li>によって複数のサブ条件を組み合わせて指定することができます '(' および')'</li><li>文字列リテラル、Datetime (で囲まれて一重引用符)、数値 (数字を含める必要がありますのみ)</li><li>厳密に形式でなければなりません、二項式、このような ((operand operator operand) や (オペランド演算子オペランド)) *</li></ul> | <ul><li>演算子: Between、IN</li><li>ADD()、MAX() NOW()、POWER() などのようにすべての組み込み機能</li><li>算術演算子と同様に *、-、+ など</li><li>文字列連結を使用するとします。</li><li>すべての結合</li><li>NULL と Null ではないです。</li><li>16 進数のように、数値以外の文字を含む任意の数字</li></ul>
(選択クエリ) 内のフィールド | <ul><li>コンマで区切られた有効な列名。 テーブル名のプレフィックスには、(一度に 1 つのテーブルでコネクタの動作) は許可されません。</li><li>名前をエスケープすることができます ' [' と ']'</li></ul> | <ul><li>TOP、DISTINCT などのキーワード</li><li>Street + 市区町村 + Zip AS アドレスなどのエイリアス</li><li>ADD()、MAX() NOW()、POWER() などのように、すべての組み込み関数</li><li>などの算術演算子、*、-、+ など</li><li>文字列連結を使用して +</li></ul>

#### ヒント

- 高度なクエリでは、ストアド プロシージャを作成し、execute ストアド プロシージャ API を使用して実行することをお勧めします。
- 内部クエリを使用する場合は、ストアド プロシージャ内で使用します。
- 複数の条件を結合するには、'AND' 演算子と 'OR' 演算子を使用できます。

## ハイブリッド構成 (省略可能)

> [AZURE.NOTE] この手順は、ファイアウォールの背後にあるオンプレミス SQL サーバーを使用している場合にのみ必要です。

App Service では、 ハイブリッド構成マネージャーを使用して、オンプレミス システムに安全に接続します。 コネクタでオンプレミスの SQL Server を使用する場合は、ハイブリッド接続マネージャーが必要です。

> [AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

参照してください [Hybrid Connection Manager を使用して](app-service-logic-hybrid-connection-manager.md)します。


## コネクタでできること
コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png

