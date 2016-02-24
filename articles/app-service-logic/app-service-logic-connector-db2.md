<properties
   pageTitle="Microsoft Azure App Service での DB2 コネクタの使用 | Microsoft Azure"
   description="DB2 コネクタでロジック アプリのトリガーとアクションを使用する方法"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="gplarsen"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="12/03/2015"
   ms.author="plarsen"/>

# DB2 コネクタ

Microsoft Connector for DB2 は、Azure App Service を介してアプリケーションを IBM DB2 データベースに格納されているリソースに接続するための API アプリです。 コネクタには、Azure Service Bus Relay を使用したオンプレミスの DB2 サーバーへの Azure ハイブリッド接続を含む、TCP/IP ネットワーク接続経由でリモート DB2 サーバー コンピューターに接続するための Microsoft クライアントが含まれています。 コネクタでは、次のデータベース操作がサポートされています。

- SELECT を使用して行を読み取る
- SELECT COUNT の後に SELECT を使用して、行をポーリングして読み取る
- INSERT を使用して 1 行または複数 (バルク) 行を追加する
- UPDATE を使用して 1 行または複数 (バルク) 行を変更する
- DELETE を使用して 1 行または複数 (バルク) 行を削除する
- SELECT CURSOR の後に UPDATE WHERE CURRENT OF CURSOR を使用して、行を読み取って変更する
- SELECT CURSOR の後に UPDATE WHERE CURRENT OF CURSOR を使用して、行を読み取って削除する
- CALL を使用して、入力パラメーター、出力パラメーター、戻り値、結果セットを指定したプロシージャを実行する
- SELECT、INSERT、UPDATE、DELETE を使用したカスタム コマンドおよび複合操作

## トリガーとアクション
コネクタでは、次に示す、ロジック アプリのトリガーとアクションをサポートしています。

トリガー | アクション
--- | ---
<ul><li>データのポーリング</li></ul> | <ul><li>一括挿入</li><li>挿入</li><li>一括更新</li><li>更新</li><li>呼び出し</li><li>一括削除</li><li>削除</li><li>[</li><li>条件付き更新</li><li>EntitySet へのポスト</li><li>条件付き削除</li><li>1 つのエンティティを選択します。</li><li>削除</li><li>EntitySet を Upsert</li><li>カスタム コマンド</li><li>複合操作</li></ul>


## DB2 コネクタの作成
コネクタは、次の例に示すように、ロジック アプリ内で定義することも、Azure Marketplace から定義することもできます。  

1. Azure のスタート画面では、次のように選択します。 **Marketplace**します。
2.  **すべて** ブレードで、「 **db2** で、 **検索すべて** ボックス、および enter キーをクリックします。
3. 検索では、すべての結果ペインで、選択 **DB2 コネクタ**します。
4. DB2 コネクタの説明] ブレードで、次のように選択します。 **作成**します。
5. DB2 コネクタのパッケージ ブレードで、名前 (例:"Db2ConnectorNewOrders")、App Service プラン、その他のプロパティを入力します。
6. 選択 **パッケージ設定**, 、し、次のパッケージ設定を入力します。  

    名前 |必要な | 説明
--- | --- | ---
接続文字列 |[はい |DB2 クライアントの接続文字列 (たとえば、"ネットワーク アドレス = %servername;ネットワーク ポート 50,000 です。ユーザー ID = username です。パスワード = パスワード; Initial Catalog = サンプルです。パッケージ コレクション NWIND; を = 既定のスキーマ NWIND =") です。
テーブル |[はい |テーブル、ビュー、エイリアスおよび (例: 例と共に swagger ドキュメントを生成する OData 操作に必要な名前のコンマ区切りの一覧"*NEWORDERS*") です。
手順 |[はい |コンマ区切りリスト プロシージャと関数の名前 (例。"SPORDERID")。
OnPremise |いいえ |Azure Service Bus Relay を使用するオンプレミスをデプロイします。
ServiceBusConnectionString |いいえ |Azure Service Bus Relay の接続文字列です。
PollToCheckData |いいえ |(例: ロジック アプリのトリガーで使用するカウントの選択ステートメント"SELECT COUNT(\*) SHIPDATE が NULL である NEWORDERS から") です。
PollToReadData |いいえ |(例: ロジック アプリのトリガーで使用する SELECT ステートメント"を選択 \ * SHIPDATE が更新プログラムの NULL である NEWORDERS から") です。
PollToAlterData |いいえ |更新または削除 (例: ロジック アプリのトリガーで使用するステートメント"UPDATE NEWORDERS SET SHIPDATE = CURRENT DATE WHERE CURRENT OF &lt;CURSOR&gt;")。

7. 選択 **OK**, 、し、[ **作成**します。
8. 完了すると、パッケージの設定は、次のようになります。  
![][1]


## データを追加する DB2 コネクタ アクションを使用したロジック アプリ ##
"API の挿入" または "エンティティへの投稿" OData 操作を使用して DB2 テーブルにデータを追加するようにロジック アプリ アクションを定義できます。 たとえば、新しい顧客注文レコードを挿入するには、ID 列で定義されたテーブルに対する SQL INSERT ステートメントを処理して、ID 値または処理された行をロジック アプリに返します (SELECT ORDID FROM FINAL TABLE (INSERT INTO NWIND.NEWORDERS (CUSTID,SHIPNAME,SHIPADDR,SHIPCITY,SHIPREG,SHIPZIP) VALUES (?,?,?,?,?,?)))。

> [AZURE.TIP] DB2 接続"*EntitySet へのポスト*"id 列の値を返すと"*API 挿入*"の影響を受ける行が返されます

1. Azure のスタート画面では、次のように選択します。 **+** (加算記号) **Web + モバイル**, 、し **ロジック アプリ**します。
2. 名前 (例:"NewOrdersDb2")、App Service プラン、その他のプロパティ、および選択 **作成**します。
3. Azure のスタート画面で、先ほど作成したロジック アプリを選択 **設定**, 、し **トリガーとアクション**します。
4. トリガーとアクション] ブレードで、次のように選択します。 **を最初から作成** ロジック アプリ テンプレート内で。
5. API Apps パネルの [ **繰り返し**, 、頻度と間隔を設定し、 **チェック マーク**します。
6. API Apps パネルの [ **DB2 コネクタ**, 、選択操作のリストを展開して **NEWORDER 挿入**します。
7. パラメーターの一覧を展開して、次の値を入力します。  

    名前 |値
--- | --- 
CUSTID |10042
出荷先名 |限定的な K Kountry ストア 
SHIPADDR |12 オーケストラの指揮 Terrace
[SHIPCITY] |ざわめきざわめき 
SHIPREG |WA
SHIPZIP |99362 

8. 選択、 **チェック マーク** アクション設定を保存し、 **保存**します。
9. 設定は次のようになります。  
![][3]

10.  **すべての実行** の下に一覧表示 **操作**, 、一覧の先頭の項目 (最後に実行) を選択します。 
11.  **ロジック アプリの実行** ブレードで、 **アクション** 項目 **db2connectorneworders**します。
12.  **ロジック アプリのアクション** ブレードで、 **入力リンク**します。 DB2 コネクタは、入力値を使用して、パラメーター化された INSERT ステートメントを処理します。
13.  **ロジック アプリのアクション** ブレードで、 **出力リンク**します。 入力は次のようになります。  
![][4]

#### 知っておくべきこと

- ロジック アプリのアクション名が形成される際、DB2 テーブル名は切り詰められます。 たとえば、操作 **NEWORDERS に挿入** に切り捨てられます **NEWORDER に挿入**します。
- ロジック アプリを保存した後に **トリガーとアクション**, 、ロジック アプリは、操作を処理します。 ロジック アプリが操作を処理するまでに数秒 (たとえば、3 ～ 5 秒) の遅延が発生する場合があります。 必要に応じてクリックして **を今すぐ実行** 操作を処理します。
- DB2 コネクタは、属性を使用して EntitySet メンバーを定義します。これには、メンバーが、既定の列と生成される列 (ID など) のどちらで DB2 列に対応しているかなどが含まれます。 ロジック アプリでは、値が必要な DB2 列を示すために、EntitySet メンバー ID 名の横に赤いアスタリスクが表示されます。 ORDID メンバーの値は入力しないでください。これは、DB2 ID 列に対応しています。 入力できるのは、その他のオプションのメンバー (ITEMS、ORDDATE、REQDATE、SHIPID、FREIGHT、SHIPCTRY) の値です。これらは、既定値が設定された DB2 列に対応しています。 
- DB2 コネクタは、"EntitySet への投稿" に対する応答をロジック アプリに返します。これには、準備された SQL INSERT ステートメントの DRDA SQLDARD (SQL Data Area Reply Data) から派生した ID 列の値が含まれます。 DB2 サーバーは、既定値が設定された列に対して、挿入された値を返しません。  


## バルク データを追加する DB2 コネクタ アクションを使用したロジック アプリ ##
API の一括挿入操作を使用して DB2 テーブルにデータを追加するようにロジック アプリ アクションを定義できます。 たとえば、2 つの新しい顧客注文レコードを挿入するには、ID 列で定義されたテーブルに対して行の値の配列を使用して SQL INSERT ステートメントを処理し、処理された行をロジック アプリに返します (SELECT ORDID FROM FINAL TABLE (INSERT INTO NWIND.NEWORDERS (CUSTID,SHIPNAME,SHIPADDR,SHIPCITY,SHIPREG,SHIPZIP) VALUES (?,?,?,?,?,?)))。

1. Azure のスタート画面では、次のように選択します。 **+** (加算記号) **Web + モバイル**, 、し **ロジック アプリ**します。
2. 名前 (例:"NewOrdersBulkDb2")、App Service プラン、その他のプロパティ、および選択 **作成**します。
3. Azure のスタート画面で、先ほど作成したロジック アプリを選択 **設定**, 、し **トリガーとアクション**します。
4. トリガーとアクション] ブレードで、次のように選択します。 **を最初から作成** ロジック アプリ テンプレート内で。
5. API Apps パネルの [ **繰り返し**, 、頻度と間隔を設定し、 **チェック マーク**します。
6. API Apps パネルの [ **DB2 コネクタ**, 、選択操作のリストを展開して **新規に一括挿入**します。
7. 入力、 **行** 配列として値。 たとえば、次の内容をコピーして貼り付けます。

    ```
    [{"CUSTID":10081,"SHIPNAME":"Trail's Head Gourmet Provisioners","SHIPADDR":"722 DaVinci Blvd.","SHIPCITY":"Kirkland","SHIPREG":"WA","SHIPZIP":"98034"},{"CUSTID":10088,"SHIPNAME":"White Clover Markets","SHIPADDR":"305 14th Ave. S. Suite 3B","SHIPCITY":"Seattle","SHIPREG":"WA","SHIPZIP":"98128","SHIPCTRY":"USA"}]
    ```

8. 選択、 **チェック マーク** アクション設定を保存し、 **保存**します。 設定は次のようになります。  
![][6]

9.  **すべての実行** の下に一覧表示 **操作**, 、一覧の先頭項目 (最後に実行)] をクリックします。
10.  **ロジック アプリの実行** ブレードで、をクリックして、 **アクション** 項目。
11.  **ロジック アプリのアクション** ブレードで、をクリックして、 **入力リンク**します。 出力は次のようになります。  
[][7]
12.  **ロジック アプリのアクション** ブレードで、をクリックして、 **出力リンク**します。 出力は次のようになります。  
![][8]

#### 知っておくべきこと

- ロジック アプリのアクション名が形成される際、DB2 テーブル名は切り詰められます。 たとえば、操作 **NEWORDERS に一括挿入** に切り捨てられます **新規に一括挿入**します。
- ID 列 (例: ORDID)、null 許容列 (例: SHIPDATE)、および既定値が設定された列 (例: ORDDATE、REQDATE、SHIPID、FREIGHT、SHIPCTRY) を省略すると、DB2 データベースで値が生成されます。
- "today" と "tomorrow" を指定すると、DB2 コネクタにより、"CURRENT DATE" 関数と "CURRENT DATE + 1 DAY" 関数 (REQDATE など) が生成されます。 


## データの読み取り、変更、削除を行う DB2 コネクタ トリガーを使用したロジック アプリ ##
API のデータのポーリング複合操作を使用して DB2 テーブルのデータをポーリングして読み取るようにロジック アプリのトリガーを定義できます。 たとえば、1 つ以上の新しい顧客注文レコードを読み取り、そのレコードをロジック アプリに返すことができます。 DB2 接続のパッケージ/アプリの設定は次のようになります。

    App Setting | Value
--- | --- | ---
PollToCheckData |SHIPDATE が NULL である NEWORDERS から COUNT(\*) を選択します。
PollToReadData |選択 \ * SHIPDATE が更新プログラムの NULL である NEWORDERS から
PollToAlterData | <no value specified>


また、API のデータのポーリング複合操作を使用して DB2 テーブルのデータのポーリング、読み取り、変更を行うようにロジック アプリのトリガーを定義することもできます。 たとえば、1 つ以上の新しい顧客注文レコードを読み取り、行の値を更新して、(更新前に) 選択されたレコードをロジック アプリに返すことができます。 DB2 接続のパッケージ/アプリの設定は次のようになります。

    App Setting | Value
--- | --- | ---
PollToCheckData |SHIPDATE が NULL である NEWORDERS から COUNT(\*) を選択します。
PollToReadData |選択 \ * SHIPDATE が更新プログラムの NULL である NEWORDERS から
PollToAlterData |更新プログラム NEWORDERS 設定 SHIPDATE = 現在の日付 WHERE CURRENT OF & lt;カーソル & gt;


さらに、API のデータのポーリング複合操作を使用して DB2 テーブルのデータのポーリング、読み取り、削除を行うようにロジック アプリのトリガーを定義できます。 たとえば、1 つ以上の新しい顧客注文レコードを読み取り、行を削除して、(削除前に) 選択されたレコードをロジック アプリに返すことができます。 DB2 接続のパッケージ/アプリの設定は次のようになります。

    App Setting | Value
--- | --- | ---
PollToCheckData |SHIPDATE が NULL である NEWORDERS から COUNT(\*) を選択します。
PollToReadData |選択 \ * SHIPDATE が更新プログラムの NULL である NEWORDERS から
PollToAlterData |削除 NEWORDERS WHERE CURRENT OF & lt;カーソル & gt;

この例では、ロジック アプリは、DB2 テーブル内のデータをポーリングして読み取り、更新した後で、もう一度データを読み込みます。

1. Azure のスタート画面では、次のように選択します。 **+** (加算記号) **Web + モバイル**, 、し **ロジック アプリ**します。
2. 名前 (例:"ShipOrdersDb2")、App Service プラン、その他のプロパティ、および選択 **作成**します。
3. Azure のスタート画面で、先ほど作成したロジック アプリを選択 **設定**, 、し **トリガーとアクション**します。
4. トリガーとアクション] ブレードで、次のように選択します。 **を最初から作成** ロジック アプリ テンプレート内で。
5. API Apps パネルの [ **DB2 コネクタ**, 、頻度と間隔を設定し、 **チェック マーク**します。
6. API Apps パネルの [ **DB2 コネクタ**, 、選択操作のリストを展開して **NEWORDERS から選択**します。
7. 選択、 **チェック マーク** アクション設定を保存し、 **保存**します。 設定は次のようになります。  
![][10]  
8. クリックして、 **トリガーとアクション** ブレードでクリックし、閉じる、 **設定** ブレードです。
9.  **すべての実行** の下に一覧表示 **操作**, 、一覧の先頭項目 (最後に実行)] をクリックします。
10.  **ロジック アプリの実行** ブレードで、をクリックして、 **アクション** 項目。
11.  **ロジック アプリのアクション** ブレードで、をクリックして、 **出力リンク**します。 出力は次のようになります。  
![][11]


## データを削除する DB2 コネクタ アクションを使用したロジック アプリ ##
"API の削除" または "エンティティへの投稿" OData 操作を使用して DB2 テーブルからデータを削除するようにロジック アプリ アクションを定義できます。 たとえば、新しい顧客注文レコードを挿入するには、ID 列で定義されたテーブルに対する SQL INSERT ステートメントを処理して、ID 値または処理された行をロジック アプリに返します (SELECT ORDID FROM FINAL TABLE (INSERT INTO NWIND.NEWORDERS (CUSTID,SHIPNAME,SHIPADDR,SHIPCITY,SHIPREG,SHIPZIP) VALUES (?,?,?,?,?,?)))。

## DB2 コネクタを使用してデータを削除するロジック アプリの作成 ##
Azure Marketplace 内から新しいロジック アプリを作成した後、顧客注文を削除するアクションとして DB2 コネクタを使用できます。 たとえば、DB2 コネクタの条件付き削除操作を使用して SQL の DELETE ステートメントを処理することができます (DELETE FROM NEWORDERS WHERE ORDID >= 10000)。

1. Azure の [ハブ] メニュー **開始** ボード] をクリックして **+** (プラス記号)、次のようにクリックします。 **Web + モバイル**, 、] をクリックし、 **ロジック アプリ**します。 
2.  **ロジック アプリの作成** ブレードで、型 a **名前**, 、たとえば **RemoveOrdersDb2**します。
3. 他の設定 (サービス プラン、リソース グループなど) の値を選択または定義します。
4. 設定は次のようになります。 クリックして **作成**:  
![][12]  
5.  **設定** ブレードで、をクリックして **トリガーとアクション**します。
6.  **トリガーとアクション** ブレードで、 **ロジック アプリ テンプレート** 一覧で、クリックして **を最初から作成**します。
7.  **トリガーとアクション** ブレードで、 **API Apps** リソース グループ内のパネルをクリックして **繰り返し**します。
8. ロジック アプリのデザイン サーフェイスで、をクリックして、 **定期的な** 項目は、設定、 **頻度** と **間隔**, 、たとえば **日** と **1**, 、順にクリックし、 **チェック マーク** 定期的な予定の項目の設定を保存します。
9.  **トリガーとアクション** ブレードで、 **API Apps** リソース グループ内のパネルをクリックして **DB2 コネクタ**します。
10. ロジック アプリのデザイン サーフェイスで、をクリックして、 **DB2 コネクタ** アクション項目、省略記号をクリックして (**...**) 操作の一覧を展開し、クリックする **条件付き削除の n**します。
11. DB2 コネクタ アクション アイテムで、次のように入力します。 **ORDID ge 10000** の、 **エントリのサブセットを識別する式**します。
12. をクリックして、 **チェック マーク** アクションの設定を保存し **保存**します。 設定は次のようになります。  
![][13]  
13. クリックして、 **トリガーとアクション** ブレードでクリックし、閉じる、 **設定** ブレードです。
14.  **すべての実行** の下に一覧表示 **操作**, 、一覧の先頭項目 (最後に実行)] をクリックします。
15.  **ロジック アプリの実行** ブレードで、をクリックして、 **アクション** 項目。
16.  **ロジック アプリのアクション** ブレードで、をクリックして、 **出力リンク**します。 出力は次のようになります。  
![][14]

**注:** ロジック アプリ デザイナーには、テーブル名が切り詰められます。 操作など **NEWORDERS から条件を削除する** に切り捨て **条件付き削除の N から**します。


> [AZURE.TIP] 次の SQL ステートメントを使用すると、サンプル テーブルおよびストアド プロシージャを作成できます。 

次の DB2 SQL DDL ステートメントを使用して、サンプルの NEWORDERS テーブルを作成できます。
 
    CREATE TABLE ORDERS (  
        ORDID INT NOT NULL GENERATED BY DEFAULT AS IDENTITY (START WITH 10000, INCREMENT BY 1) ,  
        CUSTID INT NOT NULL ,  
        EMPID INT NOT NULL DEFAULT 10000 ,  
        ORDDATE DATE NOT NULL DEFAULT CURRENT DATE ,  
        REQDATE DATE DEFAULT CURRENT DATE ,  
        SHIPDATE DATE ,  
        SHIPID INT NOT NULL DEFAULT 10000,  
        FREIGHT DECIMAL (9,2) NOT NULL DEFAULT 0.00 ,  
        SHIPNAME CHAR (40) NOT NULL ,  
        SHIPADDR CHAR (60) NOT NULL ,  
        SHIPCITY CHAR (20) NOT NULL ,  
        SHIPREG CHAR (15) NOT NULL ,  
        SHIPZIP CHAR (10) NOT NULL ,  
        SHIPCTRY CHAR (15) NOT NULL DEFAULT 'USA' ,  
        PRIMARY KEY(ORDID)  
        )  
 
    CREATE UNIQUE INDEX XORDID ON ORDERS (ORDID ASC)  



次の DB2 DDL ステートメントを使用して、サンプルの SPORDERID ストアド プロシージャを作成できます。
 
    CREATE OR REPLACE PROCEDURE NWIND.SPORDERID (IN ORDERID VARCHAR(128))  
        DYNAMIC RESULT SETS 1  
    P1: BEGIN  
        DECLARE CURSOR1 CURSOR WITH RETURN FOR  
            SELECT * FROM NWIND.NEWORDERS  
                WHERE ORDID = ORDERID;  
        OPEN CURSOR1;  
    END P1  
    ') 


## ハイブリッド構成 (省略可能)

> [AZURE.NOTE] この手順は、ファイアウォールの背後にある DB2 コネクタ-オンプレミスを使用している場合にのみ必要です。

App Service では、 ハイブリッド構成マネージャーを使用して、オンプレミス システムに安全に接続します。 コネクタでオンプレミスの IBM DB2 Server for Windows を使用する場合は、ハイブリッド接続マネージャーが必要です。

参照してください [Hybrid Connection Manager を使用して](app-service-logic-hybrid-connection-manager.md)します。


## コネクタでできること
コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

REST API を使用した API Apps を作成します。 参照してください [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。


<!--Image references-->
[1]: ./media/app-service-logic-connector-db2/ApiApp_Db2Connector_Create.png
[2]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Create.png
[3]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_TriggersActions.png
[4]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Outputs.png
[5]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Create.png
[6]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_TriggersActions.png
[7]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Inputs.png
[8]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Outputs.png
[9]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Create.png
[10]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_TriggersActions.png
[11]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Outputs.png
[12]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Create.png
[13]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_TriggersActions.png
[14]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Outputs.png


