<properties
   pageTitle="Azure SQL Database におけるデータベースとログインの管理 | Microsoft Azure"
   description="SQL Database でサーバーレベル プリンシパルなどのアカウントを使用してログインとデータベースを管理する方法。"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jeffreyg"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/25/2015"
   ms.author="rickbyh"/>

# Azure SQL Database におけるデータベースとログインの管理

Microsoft Azure SQL データベースで、サービスにサインアップするときに、プロビジョニング プロセスを作成という名前のデータベースを Azure SQL Database サーバー **マスター**, 、および Azure SQL Database サーバーのサーバー レベル プリンシパルであるログインします。 そのログインはサーバー レベルのプリンシパルに似ています (**sa**)、SQL Server の内部設置型インスタンスのです。

Azure SQL Database サーバーレベル プリンシパル アカウントには、サーバーレベルとデータベースレベルのセキュリティをすべて管理する権限が常にあります。 このトピックでは、SQL Database でサーバーレベル プリンシパルなどのアカウントを使用してログインとデータベースを管理する方法について説明します。

> [AZURE.IMPORTANT] SQL Database V12 では、包含データベース ユーザーを使用してデータベースで認証することができます。 包含データベース ユーザーはログインする必要がありません。 そのため、データベースは軽量になりますが、データベースへのアクセスを制御するサーバーレベルのプリンシパルの機能が減ります。 包含データベース ユーザーを有効にすると、セキュリティに重大な影響があります。 詳細については、次を参照してください。 [包含データベース ユーザー - ポータブルなデータベース](https://msdn.microsoft.com/library/ff929188.aspx), 、[包含データベース](https://technet.microsoft.com/library/ff929071.aspx), 、[CREATE USER (TRANSACT-SQL)](https://technet.microsoft.com/library/ms173463.aspx), 、[SQL データベースを使用して Azure Active Directory 認証に接続する](sql-database-aad-authentication.md)です。

## SQL Database のセキュリティ管理の概要

SQL Database におけるセキュリティ管理は、オンプレミスの SQL Server インスタンスにけるセキュリティ管理と似ています。 データベースレベルでのセキュリティ管理はほぼ同じで、使用できるパラメーターのみが異なります。 SQL Database は 1 つ以上の物理コンピューターに拡張できるため、Azure SQL Database のサーバーレベル管理については別の方法が使用されます。 次の表に、オンプレミスの SQL Server と Azure SQL Database のセキュリティ管理の相違点についてまとめています。

| 相違点 | オンプレミスの SQL Server | Azure SQL Database |
|------------------------------------------------|-----------------------------------------------------------------------------|--------------------------------------------------|
| サーバーレベル セキュリティを管理する場所         |  **セキュリティ** SQL Server Management Studio オブジェクト エクスプ ローラーでフォルダー       |  **マスター** データベースでは、Azure ポータル |
| Windows 認証                         | Active Directory ID | Azure Active Directory ID |
| ログインの作成に使用するサーバーレベル セキュリティ ロール | **securityadmin** 固定サーバー ロール | **loginmanager** データベース ロール、 **マスター** データベース |
| ログインを管理するためのコマンド                   | CREATE LOGIN、ALTER LOGIN、DROP LOGIN                                           | CREATE LOGIN、ALTER LOGIN、DROP LOGIN (パラメーターのいくつかの制限があるし、を接続する必要があります、 **マスター** データベースです)。 |
| すべてのログインの表示                     | sys.server_principals                                                       | sys.sql_logins (に接続している、 **マスター** データベースです)。|
| データベースの作成に使用するサーバー レベル ロール       | **dbcreator** 固定データベース ロール | **dbmanager**  データベース ロール、 **マスター** データベース |
| データベースの作成に使用するコマンド                | CREATE DATABASE                                                             | CREATE DATABASE (パラメーターのいくつかの制限があるし、を接続する必要があります、 **マスター** データベースです)。 |
| すべてのデータベースの一覧表示                  | sys.databases                                                               | sys.databases (に接続している、 **マスター** データベースです)。 |

## サーバーレベル管理と master データベース

Azure SQL Database サーバーは、データベースのグループを定義する抽象的なものです。 Azure SQL Database サーバーに関連付けられているデータベースは、マイクロソフトのデータ センターに置かれた別々の物理コンピューターの中に存在している可能性があります。 という名前の 1 つのデータベースを使用して、そのすべてのサーバー レベル管理を実行 **マスター**します。

 **マスター** データベースの追跡、ログインとログインがデータベースやその他のログインを作成する権限を持っています。 接続している、 **マスター** データベースの作成、変更、またはログインまたはデータベースを削除するたびにします。  **マスター** データベースには、 ``sys.sql_logins`` と ``sys.databases`` ログインおよびデータベースの表示を表示します。

> [AZURE.NOTE]  ``USE`` コマンドはデータベースの切り替えはサポートされていません。 ターゲット データベースに対して直接接続を確立します。

Azure SQL Database では、オンプレミスの SQL Server インスタンスと同じ方法で、ユーザーとオブジェクトのデータベースレベルのセキュリティを管理できます。 それぞれのコマンドで使用できるパラメーターのみが異なります。 詳細については、次を参照してください。 [Azure SQL Database のセキュリティ ガイドラインと制限事項](sql-database-security-guidelines.md)します。

## 包含データベース ユーザーの管理

サーバーレベル プリンシパルを使用してデータベースに接続することで、データベースに最初の包含データベース ユーザーを作成します。 ``CREATE USER``、``ALTER USER``、``DROP USER`` の各ステートメントを使用します。 次の例では、user1 という名前のユーザーを作成します。

```
CREATE USER user1 WITH password='<Strong_Password>';
```

> [AZURE.NOTE] 包含データベース ユーザーを作成するときに、強力なパスワードを使用する必要があります。 詳細については、次を参照してください。 [強力なパスワードの](https://msdn.microsoft.com/library/ms161962.aspx)です。

持つユーザーが追加の包含データベース ユーザーを作成することができます、 **ALTER ANY USER** 権限です。

SQL Database V12 のプレビュー機能では、包含データベース ユーザーとして Azure Active Directory ID をサポートしています。 詳細については、次を参照してください。 [SQL データベースを使用して Azure Active Directory 認証に接続する](sql-database-aad-authentication.md)です。

SQL Database では、包含データベース ユーザーを使用することをお勧めします。 詳細については、次を参照してください。 [包含データベース ユーザー - ポータブルなデータベース](https://msdn.microsoft.com/library/ff929188.aspx)します。 

## ログインの管理

サーバー レベル プリンシパル ログインを使用して master データベースに接続し、ログインを管理します。 ``CREATE LOGIN``、``ALTER LOGIN``、``DROP LOGIN`` の各ステートメントを使用できます。 次の例では、という名前のログイン **login1**:

```
-- first, connect to the master database
CREATE LOGIN login1 WITH password='<ProvidePassword>';
```

> [AZURE.NOTE] ログインを作成する場合は、強力なパスワードを使用する必要があります。 詳細については、次を参照してください。 [強力なパスワードの](https://msdn.microsoft.com/library/ms161962.aspx)です。

#### 新しいログインの使用

作成したログインを使用して Microsoft Azure SQL Database に接続するには、まず ``CREATE USER`` コマンドを使用して、各ログインにデータベースレベルの権限を付与する必要があります。 詳細については、次を参照してください。、 **ログインにデータベース アクセスの許可** 以下のセクションです。

一部のツールでは表形式データ ストリーム (TDS) の実装方法が異なるため、接続文字列でログインに Azure SQL Database サーバー名を付加することが必要になる場合があります。その際には、``<login>@<server>`` という表記を使用します。 この場合は、``@`` 記号を使用してログインと Azure SQL Database サーバー名を区切ります。 たとえば、現在のログインの名前 **login1** 、Azure SQL Database サーバーの完全修飾名は、 **servername.database.windows.net**, 、接続文字列の username パラメーターにする必要があります: **login1@servername**します。 この制限により、ログイン名として選択できるテキストが限定されます。 詳細については、次を参照してください。 [CREATE LOGIN (TRANSACT-SQL)](https://msdn.microsoft.com/library/ms189751.aspx)します。

## ログインに対するサーバーレベル権限の付与

サーバー レベル プリンシパル以外のログインでサーバー レベルのセキュリティを管理するには、Azure SQL データベースには、2 つのセキュリティ ロール: **loginmanager** ログインを作成するため、 **dbmanager** データベースを作成するためです。 内のユーザーのみ、 **マスター** データベースは、これらのデータベース ロールに追加できます。

> [AZURE.NOTE] ログインまたはデータベースを作成するに接続する必要があります、 **マスター** データベース (これは、論理的に表した **マスター**)。

### loginmanager ロール

ように、 **securityadmin** SQL Server の内部設置型インスタンスの固定サーバー ロール、 **loginmanager** Azure SQL Database のデータベース ロールは、ログインを作成する権限を持っています。 サーバー レベルのプリンシパル ログイン (プロビジョニング処理で作成された) またはのメンバーのみ、 **loginmanager** データベース ロールは、新しいログインを作成できます。 

### dbmanager ロール

Azure SQL Database **dbmanager** データベース ロールに似ていますが、 **dbcreator** SQL Server の内部設置型インスタンスの固定サーバー ロールです。 サーバー レベルのプリンシパル ログイン (プロビジョニング処理で作成された) またはのメンバーのみ、 **dbmanager** データベース ロールは、データベースを作成できます。 ユーザーのメンバーであると、 **dbmanager** データベース ロール、Azure SQL Database でデータベースを作成することができます ``CREATE DATABASE`` コマンド、master データベースにコマンドを実行する必要がありますが、そのします。 詳細については、次を参照してください。 [CREATE DATABASE (TRANSACT-SQL)](https://msdn.microsoft.com/library/dn268335.aspx)します。

### SQL Database のサーバーレベル ロールを割り当てる方法

データベースや他のログインを作成できるログインや関連するユーザーを作成するには、次の手順に従います。

1. 接続、 **マスター** データベースのサーバー レベルのプリンシパル ログイン (プロビジョニング処理で作成された) の資格情報または既存のメンバーの資格情報を使用して、 **loginmanager** データベース ロール。
2. ``CREATE LOGIN`` コマンドを使用して、ログインを作成します。 詳細については、次を参照してください。 [CREATE LOGIN (TRANSACT-SQL)](https://msdn.microsoft.com/library/ms189751.aspx)します。
3. ``CREATE USER`` コマンドを使用して、作成したログインの新しいユーザーを master データベースに作成します。 詳細については、次を参照してください。 [CREATE USER (TRANSACT-SQL)](https://msdn.microsoft.com/library/ms173463.aspx)します。
4. ストアド プロシージャを使用して ``sp_addrolememeber`` に新しいユーザーを追加する、 **dbmanager** データベース ロール、loginmanager ロールのデータベース ロール、またはその両方です。

次のコード例は、という名前のログインを作成する方法を示しています。 **login1**, 、という名前の対応するデータベース ユーザーと **login1User** データベースやに接続されているその他のログインを作成することは、 **マスター** データベース。

```
-- first, connect to the master database
CREATE LOGIN login1 WITH password='<ProvidePassword>';
CREATE USER login1User FROM LOGIN login1;
EXEC sp_addrolemember 'dbmanager', 'login1User';
EXEC sp_addrolemember 'loginmanager', 'login1User';
```

> [AZURE.NOTE] ログインを作成する場合は、強力なパスワードを使用する必要があります。 詳細については、次を参照してください。 [強力なパスワードの](https://msdn.microsoft.com/library/ms161962.aspx)です。

## ログインのデータベース アクセスの許可

すべてのログインを作成する必要があります、 **マスター** データベースです。 ログインが作成された後で、別のデータベースにそのログインのユーザー アカウントを作成できます。 Azure SQL Database では、オンプレミスの SQL Server インスタンスと同じ方法でデータベース ロールをサポートします。

別のデータベースにユーザー アカウントを作成するには (ログインまたはデータベースをまだ作成していないとします)、次の手順に従います。

1. 接続、 **マスター** データベース (持つログインを使用、 **loginmanager** と **dbmanager** ロール)。
2. ``CREATE LOGIN`` コマンドを使用して、新しいログインを作成します。 詳細については、次を参照してください。 [CREATE LOGIN (TRANSACT-SQL)](https://msdn.microsoft.com/library/ms189751.aspx)します。 Windows 認証はサポートされません。
3. ``CREATE DATABASE`` コマンドを使用して、新しいデータベースを作成します。 詳細については、次を参照してください。 [CREATE DATABASE (TRANSACT-SQL)](https://msdn.microsoft.com/library/dn268335.aspx)します。
4. 新しいデータベースとの接続を確立します (データベースの作成に使用したログインを使用します)。
5. ``CREATE USER`` コマンドを使用して、新しいデータベースに新しいユーザーを作成します。 詳細については、次を参照してください。 [CREATE USER (TRANSACT-SQL)](https://msdn.microsoft.com/library/ms173463.aspx)します。

次のコード例は、という名前のログインを作成する方法を示しています **login1** という名前のデータベースと **database1**:。

```
-- first, connect to the master database
CREATE LOGIN login1 WITH password='<ProvidePassword>';
CREATE DATABASE database1;
```

> [AZURE.NOTE] ログインを作成する場合は、強力なパスワードを使用する必要があります。 詳細については、次を参照してください。 [強力なパスワードの](https://msdn.microsoft.com/library/ms161962.aspx)です。

次の例では、という名前のデータベース ユーザーを作成する方法を示しています。 **login1User** データベースに **database1** ログインに対応する **login1**します。 次の例を実行するにする必要があります最初、database1 への新しい接続を持つログインを使用して、 **ALTER ANY USER** そのデータベースにアクセスを許可します。 メンバーとして接続しているすべてのユーザー、 **db_owner** データベースが作成され、ログインなどのの権限をロールが与えられます。

```
-- Establish a new connection to the database1 database
CREATE USER login1User FROM LOGIN login1;
```

Azure SQL Database におけるこのようなデータベースレベルの権限モデルは、オンプレミスの SQL Server インスタンスと同じです。 詳細については、SQL Server オンライン ブックの次のトピックをご覧ください。

- [ログイン、ユーザー、およびスキーマの管理方法に関するトピック](https://msdn.microsoft.com/library/aa337552.aspx) 
- [レッスン 2: データベース オブジェクトに対する権限の構成](https://msdn.microsoft.com/library/ms365345.aspx) 

> [AZURE.NOTE] Azure SQL Database での TRANSACT-SQL ステートメントのセキュリティに関連する可能性がありますでは、使用可能なパラメーターが若干異なります。 詳細については、特定のステートメントの構文に関するオンライン ブックを参照してください。 

## ログインとデータベースの表示


Azure SQL Database サーバーのログインとデータベースを表示するには、master データベースの ``sys.sql_logins`` ビューと ``sys.databases`` ビューをそれぞれ使用します。 次の例では、Azure SQL Database サーバーのすべてのログインとデータベースの一覧を表示する方法を示します。

```
-- first, connect to the master database
SELECT * FROM sys.sql_logins;
SELECT * FROM sys.databases; 
```

## 関連項目

[Azure SQL Database のセキュリティのガイドラインと制限事項](sql-database-security-guidelines.md) 
[Azure Active Directory 認証を使用して SQL Database に接続する](sql-database-aad-authentication.md)


