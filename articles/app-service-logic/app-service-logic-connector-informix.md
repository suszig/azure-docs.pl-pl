<properties
   pageTitle="Microsoft Azure App Service での Informix コネクタの使用 | Microsoft Azure"
   description="Informix コネクタでロジック アプリのトリガーとアクションを使用する方法"
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

# Informix コネクタ

Microsoft Connector for Informix は、Azure App Service を介してアプリケーションを IBM Informix データベースに格納されているリソースに接続するための API アプリです。 コネクタには、Azure Service Bus Relay を使用したオンプレミスの Informix サーバーへの Azure ハイブリッド接続を含む、TCP/IP ネットワーク接続経由でリモート Informix サーバー コンピューターに接続するための Microsoft クライアントが含まれています。 コネクタでは、次のデータベース操作がサポートされています。

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


## Informix コネクタの作成
コネクタは、次の例に示すように、ロジック アプリ内で定義することも、Azure Marketplace から定義することもできます。  

1. Azure のスタート画面では、次のように選択します。 **Marketplace**します。
2.  **すべて** ブレードで、「 **informix** で、 **検索すべて** ボックス、および enter キーをクリックします。
3. 検索では、すべての結果ペインで、選択 **Informix コネクタ**します。
4. Informix コネクタの説明] ブレードで、次のように選択します。 **作成**します。
5. Informix コネクタのパッケージ ブレードで、名前 (例:"InformixConnectorNewOrders")、App Service プラン、その他のプロパティを入力します。
6. 選択 **パッケージ設定**, 、次のパッケージ設定を入力します。

    名前 |必要な | 説明
--- | --- | ---
接続文字列 |[はい |Informix クライアントの接続文字列 (たとえば、"ネットワーク アドレス = %servername;ネットワーク ポート = 9089 です。ユーザー ID = username です。パスワード = パスワード; Initial Catalog = nwind; 既定のスキーマ informix を =") です。
テーブル |[はい |テーブル、ビュー、エイリアスおよび (例: 例と共に swagger ドキュメントを生成する OData 操作に必要な名前のコンマ区切りの一覧"NEWORDERS")。
手順 |[はい |コンマ区切りリスト プロシージャと関数の名前 (例。"SPORDERID")。
OnPremise |いいえ |Azure Service Bus Relay を使用するオンプレミスをデプロイします。
ServiceBusConnectionString |いいえ |Azure Service Bus Relay の接続文字列です。
PollToCheckData |いいえ |(例: ロジック アプリのトリガーで使用するカウントの選択ステートメント"SELECT COUNT(\*) SHIPDATE が NULL である NEWORDERS から") です。
PollToReadData |いいえ |(例: ロジック アプリのトリガーで使用する SELECT ステートメント"を選択 \ * SHIPDATE が更新プログラムの NULL である NEWORDERS から") です。
PollToAlterData |いいえ |更新または削除 (例: ロジック アプリのトリガーで使用するステートメント"UPDATE NEWORDERS SET SHIPDATE = CURRENT DATE WHERE CURRENT OF &lt;CURSOR&gt;")。

7. 選択 **OK**, 、し、[ **作成**します。
8. 完了すると、パッケージの設定は、次のようになります。  
![][1]


## データを追加する Informix コネクタ アクションを使用したロジック アプリ ##
"API の挿入" または "エンティティへの投稿" OData 操作を使用して Informix テーブルにデータを追加するようにロジック アプリ アクションを定義できます。 たとえば、新しい顧客注文レコードを挿入するには、ID 列で定義されたテーブルに対する SQL INSERT ステートメントを処理して、ID 値または処理された行をロジック アプリに返します (SELECT ORDID FROM FINAL TABLE (INSERT INTO NEWORDERS (CUSTID,SHIPNAME,SHIPADDR,SHIPCITY,SHIPREG,SHIPZIP) VALUES (?,?,?,?,?,?)))。

> [AZURE.TIP] Informix 接続"*EntitySet へのポスト*"id 列の値を返すと"*API 挿入*"の影響を受ける行が返されます

1. Azure のスタート画面では、次のように選択します。 **+** (加算記号) **Web + モバイル**, 、し **ロジック アプリ**します。
2. 名前 (例:"NewOrdersInformix")、App Service プラン、その他のプロパティ、および選択 **作成**します。
3. Azure のスタート画面で、先ほど作成したロジック アプリを選択 **設定**, 、し **トリガーとアクション**します。
4. トリガーとアクション] ブレードで、次のように選択します。 **を最初から作成** ロジック アプリ テンプレート内で。
5. API Apps パネルの [ **繰り返し**, 、頻度と間隔を設定し、 **チェック マーク**します。
6. API Apps パネルの [ **Informix コネクタ**, 、選択操作のリストを展開して **NEWORDER 挿入**します。
7. パラメーターの一覧を展開して、次の値を入力します。  

    名前 |値
--- | --- 
CUSTID |10042
SHIPID |10000
出荷先名 |限定的な K Kountry ストア 
SHIPADDR |12 オーケストラの指揮 Terrace
[SHIPCITY] |ざわめきざわめき 
SHIPREG |WA
SHIPZIP |99362 

8. 選択、 **チェック マーク** アクション設定を保存し、 **保存**します。
9. 設定は次のようになります。  
![][3]  
10.  **すべての実行** の下に一覧表示 **操作**, 、一覧の先頭の項目 (最後に実行) を選択します。 
11.  **ロジック アプリの実行** ブレードで、 **アクション** 項目 **informixconnectorneworders**します。
12.  **ロジック アプリのアクション** ブレードで、 **入力リンク**します。 Informix コネクタは、入力値を使用して、パラメーター化された INSERT ステートメントを処理します。
13.  **ロジック アプリのアクション** ブレードで、 **出力リンク**します。 入力は次のようになります。  
![][4]

#### 知っておくべきこと

- ロジック アプリのアクション名が形成される際、Informix テーブル名は切り詰められます。 たとえば、操作 **NEWORDERS に挿入** に切り捨てられます **NEWORDER に挿入**します。
- ロジック アプリを保存した後に **トリガーとアクション**, 、ロジック アプリは、操作を処理します。 ロジック アプリが操作を処理するまでに数秒 (たとえば、3 ～ 5 秒) の遅延が発生する場合があります。 必要に応じてクリックして **を今すぐ実行** 操作を処理します。
- Informix コネクタは、属性を使用して EntitySet メンバーを定義します。これには、メンバーが、既定の列と生成される列 (ID など) のどちらで Informix 列に対応しているかなどが含まれます。 ロジック アプリでは、値が必要な Informix 列を示すために、EntitySet メンバー ID 名の横に赤いアスタリスクが表示されます。 ORDID メンバーの値は入力しないでください。これは、Informix ID 列に対応しています。 入力できるのは、その他のオプションのメンバー (ITEMS、ORDDATE、REQDATE、SHIPID、FREIGHT、SHIPCTRY) の値です。これらは、既定値が設定された Informix 列に対応しています。 
- Informix コネクタは、EntitySet への投稿に対する応答をロジック アプリに返します。これには、準備された SQL INSERT ステートメントの DRDA SQLDARD (SQL Data Area Reply Data) から派生した ID 列の値が含まれます。 Informix サーバーは、既定値が設定された列に対して、挿入された値を返しません。  


## バルク データを追加する Informix コネクタ アクションを使用したロジック アプリ ##
API の一括挿入操作を使用して Informix テーブルにデータを追加するようにロジック アプリ アクションを定義できます。 たとえば、2 つの新しい顧客注文レコードを挿入するには、ID 列で定義されたテーブルに対して行の値の配列を使用して SQL INSERT ステートメントを処理し、処理された行をロジック アプリに返します (SELECT ORDID FROM FINAL TABLE (INSERT INTO NEWORDERS (CUSTID,SHIPNAME,SHIPADDR,SHIPCITY,SHIPREG,SHIPZIP) VALUES (?,?,?,?,?,?)))。

1. Azure のスタート画面では、次のように選択します。 **+** (加算記号) **Web + モバイル**, 、し **ロジック アプリ**します。
2. 名前 (例:"NewOrdersBulkInformix")、App Service プラン、その他のプロパティ、および選択 **作成**します。
3. Azure のスタート画面で、先ほど作成したロジック アプリを選択 **設定**, 、し **トリガーとアクション**します。
4. トリガーとアクション] ブレードで、次のように選択します。 **を最初から作成** ロジック アプリ テンプレート内で。
5. API Apps パネルの [ **繰り返し**, 、頻度と間隔を設定し、 **チェック マーク**します。
6. API Apps パネルの [ **Informix コネクタ**, 、選択操作のリストを展開して **新規に一括挿入**します。
7. 入力、 **行** 配列として値。 たとえば、次の内容をコピーして貼り付けます。  

    ```
    [{"custid":10081,"shipid":10000,"shipname":"Trail's Head Gourmet Provisioners","shipaddr":"722 DaVinci Blvd.","shipcity":"Kirkland","shipreg":"WA","shipzip":"98034"},{"custid":10088,"shipid":10000,"shipname":"White Clover Markets","shipaddr":"305 14th Ave. S. Suite 3B","shipcity":"Seattle","shipreg":"WA","shipzip":"98128","shipctry":"USA"}]
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

- ロジック アプリのアクション名が形成される際、Informix テーブル名は切り詰められます。 たとえば、操作 **NEWORDERS に一括挿入** に切り捨てられます **新規に一括挿入**します。
- Informix データベースでは、テーブル名と列名の大文字と小文字が区別される場合があります。 たとえば、一括挿入操作の配列の列名は、大文字 ("CUSTID") ではなく小文字 ("custid") で指定することが必要な場合があります。
- ID 列 (例: ORDID)、null 許容列 (例: SHIPDATE)、および既定値が設定された列 (例: ORDDATE、REQDATE、SHIPID、FREIGHT、SHIPCTRY) を省略すると、Informix データベースで値が生成されます。
- "today" と "tomorrow" を指定すると、Informix コネクタにより、"CURRENT DATE" 関数と "CURRENT DATE + 1 DAY" 関数 (REQDATE など) が生成されます。 


## データの読み取り、変更、削除を行う Informix コネクタ トリガーを使用したロジック アプリ ##
API のデータのポーリング複合操作を使用して Informix テーブルのデータをポーリングして読み取るようにロジック アプリのトリガーを定義できます。 たとえば、1 つ以上の新しい顧客注文レコードを読み取り、そのレコードをロジック アプリに返すことができます。 Informix 接続のパッケージ/アプリの設定は次のようになります。

    App Setting | Value
--- | --- | ---
PollToCheckData |SHIPDATE が NULL である NEWORDERS から COUNT(\*) を選択します。
PollToReadData |選択 \ * SHIPDATE が更新プログラムの NULL である NEWORDERS から
PollToAlterData | <no value specified>


また、API のデータのポーリング複合操作を使用して Informix テーブルのデータのポーリング、読み取り、変更を行うようにロジック アプリのトリガーを定義することもできます。 たとえば、1 つ以上の新しい顧客注文レコードを読み取り、行の値を更新して、(更新前に) 選択されたレコードをロジック アプリに返すことができます。 Informix 接続のパッケージ/アプリの設定は次のようになります。

    App Setting | Value
--- | --- | ---
PollToCheckData |SHIPDATE が NULL である NEWORDERS から COUNT(\*) を選択します。
PollToReadData |選択 \ * SHIPDATE が更新プログラムの NULL である NEWORDERS から
PollToAlterData |更新プログラム NEWORDERS 設定 SHIPDATE = 現在の日付 WHERE CURRENT OF & lt;カーソル & gt;


さらに、API のデータのポーリング複合操作を使用して Informix テーブルのデータのポーリング、読み取り、削除を行うようにロジック アプリのトリガーを定義できます。 たとえば、1 つ以上の新しい顧客注文レコードを読み取り、行を削除して、(削除前に) 選択されたレコードをロジック アプリに返すことができます。 Informix 接続のパッケージ/アプリの設定は次のようになります。

    App Setting | Value
--- | --- | ---
PollToCheckData |SHIPDATE が NULL である NEWORDERS から COUNT(\*) を選択します。
PollToReadData |選択 \ * SHIPDATE が更新プログラムの NULL である NEWORDERS から
PollToAlterData |削除 NEWORDERS WHERE CURRENT OF & lt;カーソル & gt;

この例では、ロジック アプリは、Informix テーブル内のデータをポーリングして読み取り、更新した後で、もう一度データを読み込みます。

1. Azure のスタート画面では、次のように選択します。 **+** (加算記号) **Web + モバイル**, 、し **ロジック アプリ**します。
2. 名前 (例:"ShipOrdersInformix")、App Service プラン、その他のプロパティ、および選択 **作成**します。
3. Azure のスタート画面で、先ほど作成したロジック アプリを選択 **設定**, 、し **トリガーとアクション**します。
4. トリガーとアクション] ブレードで、次のように選択します。 **を最初から作成** ロジック アプリ テンプレート内で。
5. API Apps パネルの [ **Informix コネクタ**, 、頻度と間隔を設定し、 **チェック マーク**します。
6. API Apps パネルの [ **Informix コネクタ**, 、選択操作のリストを展開して **NEWORDERS から選択**します。
7. 選択、 **チェック マーク** アクション設定を保存し、 **保存**します。 設定は次のようになります。  
![][10]
8. クリックして、 **トリガーとアクション** ブレードでクリックし、閉じる、 **設定** ブレードです。
9.  **すべての実行** の下に一覧表示 **操作**, 、一覧の先頭項目 (最後に実行)] をクリックします。
10.  **ロジック アプリの実行** ブレードで、をクリックして、 **アクション** 項目。
11.  **ロジック アプリのアクション** ブレードで、をクリックして、 **出力リンク**します。 出力は次のようになります。  
![][11]


## データを削除する Informix コネクタ アクションを使用したロジック アプリ ##
"API の削除" または "エンティティへの投稿" OData 操作を使用して Informix テーブルからデータを削除するようにロジック アプリ アクションを定義できます。 たとえば、新しい顧客注文レコードを挿入するには、ID 列で定義されたテーブルに対する SQL INSERT ステートメントを処理して、ID 値または処理された行をロジック アプリに返します (SELECT ORDID FROM FINAL TABLE (INSERT INTO NEWORDERS (CUSTID,SHIPNAME,SHIPADDR,SHIPCITY,SHIPREG,SHIPZIP) VALUES (?,?,?,?,?,?)))。

## Informix コネクタを使用してデータを削除するロジック アプリの作成 ##
Azure Marketplace 内から新しいロジック アプリを作成した後、顧客注文を削除するアクションとして Informix コネクタを使用できます。 たとえば、Informix コネクタの条件付き削除操作を使用して SQL の DELETE ステートメントを処理することができます (DELETE FROM NEWORDERS WHERE ORDID >= 10000)。

1. Azure の [ハブ] メニュー **開始** ボード] をクリックして **+** (プラス記号)、次のようにクリックします。 **Web + モバイル**, 、] をクリックし、 **ロジック アプリ**します。 
2.  **ロジック アプリの作成** ブレードで、型 a **名前**, 、たとえば **RemoveOrdersInformix**します。
3. 他の設定 (サービス プラン、リソース グループなど) の値を選択または定義します。
4. 設定は次のようになります。 クリックして **作成**:  
![][12]
5.  **設定** ブレードで、をクリックして **トリガーとアクション**します。
6.  **トリガーとアクション** ブレードで、 **ロジック アプリ テンプレート** 一覧で、クリックして **を最初から作成**します。
7.  **トリガーとアクション** ブレードで、 **API Apps** リソース グループ内のパネルをクリックして **繰り返し**します。
8. ロジック アプリのデザイン サーフェイスで、をクリックして、 **定期的な** 項目は、設定、 **頻度** と **間隔**, 、たとえば **日** と **1**, 、順にクリックし、 **チェック マーク** 定期的な予定の項目の設定を保存します。
9.  **トリガーとアクション** ブレードで、 **API Apps** リソース グループ内のパネルをクリックして **Informix コネクタ**します。
10. ロジック アプリのデザイン サーフェイスで、をクリックして、 **Informix コネクタ** アクション項目、省略記号をクリックして (**...**) 操作の一覧を展開し、クリックする **条件付き削除の n**します。
11. Informix コネクタ アクション アイテムで、次のように入力します。 **ordid ge 10000** の、 **エントリのサブセットを識別する式**します。
12. をクリックして、 **チェック マーク** アクションの設定を保存し **保存**します。 設定は次のようになります。  
![][13]
13. クリックして、 **トリガーとアクション** ブレードでクリックし、閉じる、 **設定** ブレードです。
14.  **すべての実行** の下に一覧表示 **操作**, 、一覧の先頭項目 (最後に実行)] をクリックします。
15.  **ロジック アプリの実行** ブレードで、をクリックして、 **アクション** 項目。
16.  **ロジック アプリのアクション** ブレードで、をクリックして、 **出力リンク**します。 出力は次のようになります。  
![][14]

**注:** ロジック アプリ デザイナーには、テーブル名が切り詰められます。 たとえば操作 **NEWORDERS から条件を削除する** に切り捨てられます **条件付き削除の n**します。


> [AZURE.TIP] 次の SQL ステートメントを使用すると、サンプル テーブルおよびストアド プロシージャを作成できます。 

次の Informix SQL DDL ステートメントを使用して、サンプルの NEWORDERS テーブルを作成できます。
 
    create table neworders (  
        ordid serial(10000) unique ,  
        custid int not null ,  
        empid int not null default 10000 ,  
        orddate date not null default today ,  
        reqdate date default today ,  
        shipdate date ,  
        shipid int not null default 10000 ,  
        freight decimal (9,2) not null default 0.00 ,  
        shipname char (40) not null ,  
        shipaddr char (60) not null ,  
        shipcity char (20) not null ,  
        shipreg char (15) not null ,  
        shipzip char (10) not null ,  
        shipctry char (15) not null default ''USA'' 
        )


次の Informix DDL ステートメントを使用して、サンプルの SPOERID ストアド プロシージャを作成できます。
 
    create procedure sporderid ( ord_id int)  
        returning int, int, int, date, date, date, int, decimal (9,2), char (40), char (60), char (20), char (15), char (10), char (15)  
        define xordid, xcustid, xempid, xshipid int;  
        define xorddate, xreqdate, xshipdate date;  
        define xfreight decimal (9,2);  
        define xshipname char (40);  
        define xshipaddr char (60);  
        define xshipcity char (20);  
        define xshipreg, xshipctry char (15);  
        define xshipzip char (10);  
        select ordid, custid, empid, orddate, reqdate, shipdate, shipid, freight, shipname, shipaddr, shipcity, shipreg, shipzip, shipctry  
            into xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry  
            from neworders where ordid = ord_id;  
        return xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry;  
    end procedure; 


## ハイブリッド構成 (省略可能)

> [AZURE.NOTE] この手順は、ファイアウォールの背後にある DB2 コネクタ-オンプレミスを使用している場合にのみ必要です。

App Service では、 ハイブリッド構成マネージャーを使用して、オンプレミス システムに安全に接続します。 コネクタでオンプレミスの IBM DB2 Server for Windows を使用する場合は、ハイブリッド接続マネージャーが必要です。

参照してください [Hybrid Connection Manager を使用して](app-service-logic-hybrid-connection-manager.md)します。


## コネクタでできること
コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

REST API を使用した API Apps を作成します。 参照してください [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。


<!--Image references-->
[1]: ./media/app-service-logic-connector-informix/ApiApp_InformixConnector_Create.png
[2]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Create.png
[3]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_TriggersActions.png
[4]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Outputs.png
[5]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Create.png
[6]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_TriggersActions.png
[7]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Inputs.png
[8]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Outputs.png
[9]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Create.png
[10]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_TriggersActions.png
[11]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Outputs.png
[12]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Create.png
[13]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_TriggersActions.png
[14]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Outputs.png



