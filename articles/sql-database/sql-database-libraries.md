<properties
    pageTitle="SQL Database と SQL Server の接続ライブラリ"
    description="クライアント プログラムが Azure SQL Database または Microsoft SQL Server に接続する際に使用できる各ドライバーの最小バージョン番号を示します。 Microsoft ではなくコミュニティからリリースされているドライバーに関するバージョン情報へのリンクが用意されています。"
    services="sql-database"
    documentationCenter=""
    authors="pehteh"
    manager="jeffreyg"
    editor="genemi"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/01/2015"
    ms.author="pehteh"/>

# SQL Database と SQL Server の接続ライブラリ

このトピックでは、クライアント プログラムが Azure SQL Database または Microsoft SQL Server に接続するときに使用できる各ライブラリおよびドライバーの最小バージョン番号を示します。

## Microsoft からリリースされているドライバー ライブラリの表

Microsoft からリリースされているライブラリを次の表に示します。  **ライブラリ** 列を各ライブラリのダウンロードのリンクを提供します。  **バージョン** 列は、Azure SQL Database および Microsoft SQL Server と対話するために推奨される最小バージョンを表示します。

| プラットフォーム | オペレーティング システム | ライブラリ<br/>ダウンロード | バージョン<br/>ドライバーの | 説明<br/>ドライバーの | 詳細<br/>情報 |
| :--- | :--- | :--- | :--- | :--- | :-- |
| .NET | クロスプラットフォーム (.NET) | [ADO.NET、System.Data.SqlClient](http://www.microsoft.com/download/details.aspx?id=30653) | 4.5 以降 | .NET Framework 用 SQL Server プロバイダー | . |
| PHP | Windows | [PHP for SQL Server](http://www.microsoft.com/download/details.aspx?id=20098) | 2.0 以降 | SQL Server 用 PHP ドライバー | [リンク](http://msdn.microsoft.com/library/dn865013.aspx) |
| Java | Windows | [JDBC for SQL Server](https://www.microsoft.com/download/details.aspx?id=11774) | 2.0 以降 |  標準の JDBC API を介してデータベース接続を提供する Type 4 JDBC ドライバー | [リンク](http://msdn.microsoft.com/library/dn425070.aspx) |
| ODBC | Windows | [ODBC for SQL Server](http://www.microsoft.com/download/details.aspx?id=36434) | 11.0 以降 | Microsoft ODBC Driver for SQL Server | [リンク](http://msdn.microsoft.com/library/jj730308.aspx) |
| ODBC | Suse Linux | [ODBC for SQL Server](http://www.microsoft.com/download/details.aspx?id=34687) | 11.0 以降 | Microsoft ODBC Driver for SQL Server | . |
| ODBC | Redhat Linux | [ODBC for SQL Server](http://www.microsoft.com/download/details.aspx?id=34687) | 11.0 以降 | Microsoft ODBC Driver for SQL Server | . |

### ODBC のサポート

データ ソース名 (DSN) ウィザードを使用して、Azure SQL Database のデータ ソースを定義する、次のようをクリックして、 **と SQL Server 認証ログイン ID と、ユーザーが入力したパスワードを使用して** オプションを選択、 **追加の構成オプションの既定の設定を取得する SQL Server への接続**します。 ユーザー名とパスワードとして、Azure SQL Database サーバーに接続する入力 **ログイン ID** と **パスワード**します。 クリア、 **既定の設定を取得する SQL Server に接続しています.** チェック ボックスをオンにします。 をクリック **する既定のデータベースの変更:** 一覧に表示していない場合でも、Azure SQL データベースの名前を入力します。 ウィザードがいくつかの言語を一覧表示される注意してください、 **に SQL Server システム メッセージの言語を変更:** ] ボックスの一覧です。

このリリースの Microsoft Azure SQL Database では英語のみがサポートされているため、言語には英語を選択してください。 Microsoft Azure SQL Database でサポートされない **ミラー サーバー** または **データベースのアタッチ**, 、ので空のままにそれらの項目。 クリックして **接続をテストする**です。

SQL Server 2008 Native Client ODBC ドライバーを使用する場合、 **接続のテスト** ボタンが原因で、エラーを **master.dbo.syscharsets** はサポートされていません。 このエラーは無視し、DSN を保存して使用してください。

### OLEDB for DB2 および SQL Server (DRDA 設計向け)

Microsoft OLE DB Provider for DB2 バージョン 5.0 (Data Provider) では、IBM DB2 データベース向けの分散アプリケーションを作成できます。 Data Provider では、分散型リレーショナル データベース アーキテクチャ (DRDA) アプリケーション リクエスターとして機能する DB2 用の Microsoft ネットワーク クライアントと Microsoft SQL Server のデータ アクセス アーキテクチャを活用します。 Data Provider は、Microsoft コンポーネント オブジェクト モデル (COM) OLE DB コマンドとデータ型を、DRDA プロトコル コード ポイントとデータ形式に変換します。

詳細については、次を参照してください。

- [Microsoft OLE DB Provider for DB2 バージョン 5.0](http://msdn.microsoft.com/library/dn745875.aspx)
- [Microsoft OLEDB Provider for DB2 v4.0 for Microsoft SQL Server 2012](http://www.microsoft.com/download/details.aspx?id=29100)

## サード パーティ製のライブラリ

> [AZURE.IMPORTANT] 次の表では、サード パーティのライセンス条項に基づいてサード パーティからリリースされているライブラリが表示されます。 これらのライブラリの使用にあたっては、該当するサード パーティのライセンスを確認しその内容に従う必要があります。 これらのライブラリについては、お客様がその使用に伴うリスクを負います。 Microsoft は、ユーザーの利便性のためにのみこの情報を提供し、明示的または黙示的にかかわらず、これらの情報についていかなる責任も負わないものとします。 いかなる記述も、その種類を問わず、Microsoft による保証を暗示するものではありません。
<br/><br/>更新およびを使用してこの「サード パーティ製のライブラリ」セクションにある情報を保持する開発者のパブリック コミュニティ責任はユーザーが、 [azure コンテンツ](http://github.com/Azure/azure-content/) によって所有されているリポジトリ **Azure** GitHub.com にします。 Microsoft では、このセクションを更新することを開発者に奨励しています。 Microsoft の担当者 *いない* 社外他の人が専門的な知識各サード パーティ製のライブラリにいるために、このセクションにある情報を保持します。  ありがとうございます。

他の会社、コミュニティなど、サード パーティからリリースされているライブラリを次の表に示します。 Microsoft からリリースされているライブラリについては、このトピックの前のセクションで紹介しています。

| プラットフォーム | ライブラリ |
| :-- | :-- |
| Python | [pymssql *(org、stable)*](http://pymssql.org/en/stable/)<br/><br/>[pymssql *(org)*](http://pymssql.org/) |
| Node.js | [面倒な *(npmjs)*](http://www.npmjs.com/package/tedious) |
| Node.js | [NODE-MSSQL *(github、patriksimek)*](https://github.com/patriksimek/node-mssql)<br/><br/>[NODE-MSSQL *(npmjs)*](https://www.npmjs.com/package/node-mssql)<br/><br/>[ノードのコネクタ MSSQL *(npmjs)*](https://www.npmjs.com/package/node-mssql-connector) |
| Node.js | [Edge.js *(github com、tjanczuk)*](https://github.com/tjanczuk/edge)<br/><br/>[Edge.js *(tjanczuk、github io)*](http://tjanczuk.github.io/edge/) |
| . | [FreeTDS *(org)*](http://www.freetds.org/) |

<!--
https://en.wikipedia.org/wiki/Draft:Microsoft_SQL_Server_Libraries/Drivers
-->


