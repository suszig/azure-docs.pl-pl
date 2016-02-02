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

Microsoft Azure SQL Database では、サービスをサインアップすると、プロビジョニング処理が実行され、Azure SQL Database サーバー、**master** という名前のデータベース、ログインが作成されます。このログインは Azure SQL Database サーバーのサーバーレベル プリンシパルとなります。 このログインは、オンプレミスの SQL Server インスタンスにおけるサーバーレベル プリンシパル (**sa**) に似ています。

Azure SQL Database サーバーレベル プリンシパル アカウントには、サーバーレベルとデータベースレベルのセキュリティをすべて管理する権限が常にあります。 このトピックでは、SQL Database でサーバーレベル プリンシパルなどのアカウントを使用してログインとデータベースを管理する方法について説明します。
> [AZURE.IMPORTANT] SQL Database V12 では、ユーザーは包含データベース ユーザーを使用して、データベースを認証できます。 包含データベース ユーザーはログインする必要がありません。 そのため、データベースは軽量になりますが、データベースへのアクセスを制御するサーバーレベルのプリンシパルの機能が減ります。 包含データベース ユーザーを有効にすると、セキュリティに重大な影響があります。 詳細については、次を参照してください。 [包含データベース ユーザー - ポータブルなデータベース](https://msdn.microsoft.com/library/ff929188.aspx), 、[包含データベース](https://technet.microsoft.com/library/ff929071.aspx), 、[CREATE USER (TRANSACT-SQL)](https://technet.microsoft.com/library/ms173463.aspx), 、[SQL データベースを使用して Azure Active Directory 認証に接続する](sql-database-aad-authentication.md)します。

## SQL Database のセキュリティ管理の概要

SQL Database におけるセキュリティ管理は、オンプレミスの SQL Server インスタンスにけるセキュリティ管理と似ています。 データベースレベルでのセキュリティ管理はほぼ同じで、使用できるパラメーターのみが異なります。 SQL Database は 1 つ以上の物理コンピューターに拡張できるため、Azure SQL Database のサーバーレベル管理については別の方法が使用されます。 次の表に、オンプレミスの SQL Server と Azure SQL Database のセキュリティ管理の相違点についてまとめています。

| 相違点| オンプレミスの SQL Server| Azure SQL Database|
|------------------------------------------------|-----------------------------------------------------------------------------|--------------------------------------------------|
| サーバーレベル セキュリティを管理する場所| SQL Server Management Studio のオブジェクト エクスプローラーの **[セキュリティ]** フォルダー| **master** データベースと Azure ポータル経由|
| Windows 認証| Active Directory ID| Azure Active Directory ID|
| ログインの作成に使用するサーバーレベル セキュリティ ロール| **securityadmin** 固定サーバー ロール| **master** データベースの **loginmanager** データベース ロール|
| ログインを管理するためのコマンド| CREATE LOGIN、ALTER LOGIN、DROP LOGIN| CREATE LOGIN、ALTER LOGIN、DROP LOGIN (一部のパラメーターが制限されます。また、**master** データベースに接続する必要があります)。|
| すべてのログインの表示| sys.server_principals| sys.sql_logins (**master** データベースに接続する必要があります)。|
| データベースの作成に使用するサーバー レベル ロール| **dbcreator** 固定データベース ロール| **dbmanager**  データベース ロール、 **マスター** データベース|
| データベースの作成に使用するコマンド| CREATE DATABASE| CREATE DATABASE (一部のパラメーターが制限されます。また、**master** データベースに接続する必要があります)。|
| すべてのデータベースの一覧表示| sys.databases| sys.databases (に接続している、 **マスター** データベースです)。|

## サーバーレベル管理と master データベース

Azure SQL Database サーバーは、データベースのグループを定義する抽象的なものです。 Azure SQL Database サーバーに関連付けられているデータベースは、マイクロソフトのデータ センターに置かれた別々の物理コンピューターの中に存在している可能性があります。 そのすべてに対してサーバーレベル管理を実行するには、**master** という名前の 1 つのデータベースを使用します。

**master** データベースでは、ログインと、どのログインにデータベースや他のログインを作成するためのアクセス許可があるかが追跡されます。 ログインまたはデータベースの作成 (CREATE)、変更 (ALTER)、削除 (DROP) を実行する際は、常に **master** データベースに接続する必要があります。  **マスター** データベースには、 `sys.sql_logins` と `sys.databases` ログインおよびデータベースの表示を表示します。
> [AZURE.NOTE] `使用` コマンドはデータベースの切り替えはサポートされていません。 ターゲット データベースに対して直接接続を確立します。

Azure SQL Database では、オンプレミスの SQL Server インスタンスと同じ方法で、ユーザーとオブジェクトのデータベースレベルのセキュリティを管理できます。 それぞれのコマンドで使用できるパラメーターのみが異なります。 詳細については、次を参照してください。 [Azure SQL Database のセキュリティ ガイドラインと制限事項](sql-database-security-guidelines.md)します。

## 包含データベース ユーザーの管理

サーバーレベル プリンシパルを使用してデータベースに接続することで、データベースに最初の包含データベース ユーザーを作成します。 使用して、 `CREATE USER`, 、`ALTER USER`, 、または `DROP USER` ステートメントです。 次の例では、user1 という名前のユーザーを作成します。

```
CREATE USER user1 WITH password='<Strong_Password>';
```

> [AZURE.NOTE] 包含データベース ユーザーを作成する際は、強力なパスワードを使用する必要があります。 詳細については、次を参照してください。 [強力なパスワードの](https://msdn.microsoft.com/library/ms161962.aspx)します。

**ALTER ANY USER** 権限を持つユーザーは、追加の包含データベース ユーザーを作成できます。

SQL Database V12 のプレビュー機能では、包含データベース ユーザーとして Azure Active Directory ID をサポートしています。 詳細については、次を参照してください。 [SQL データベースを使用して Azure Active Directory 認証に接続する](sql-database-aad-authentication.md)します。

SQL Database では、包含データベース ユーザーを使用することをお勧めします。 詳細については、次を参照してください。 [包含データベース ユーザー - ポータブルなデータベース](https://msdn.microsoft.com/library/ff929188.aspx)します。

## ログインの管理

サーバー レベル プリンシパル ログインを使用して master データベースに接続し、ログインを管理します。 使用することができます、 `CREATE LOGIN`, 、`ALTER LOGIN`, 、または `DROP LOGIN` ステートメントです。 次の例では、**login1** という名前のログインを作成します。

```
-- first, connect to the master database
CREATE LOGIN login1 WITH password='<ProvidePassword>';
```

> [AZURE.NOTE] ログインを作成する際には、強力なパスワードを使用する必要があります。 詳細については、次を参照してください。 [強力なパスワードの](https://msdn.microsoft.com/library/ms161962.aspx)します。

#### 新しいログインの使用

を作成したログインを使用して Microsoft Azure SQL データベースに接続するために割り当てる必要があります最初に各ログイン データベース レベルのアクセス許可を使用して、 `CREATE USER` コマンドです。 詳細については、後述する「**ログインのデータベース アクセスの許可**」セクションを参照してください。

いくつかのツールでは、表形式データ ストリーム (TDS) を異なる方法で実装するため、Azure SQL データベース サーバー名を使用して接続文字列内のログインに追加する必要があります、 `< ログイン > @< サーバー >` 表記します。このような場合は、ログインと Azure SQL データベース サーバー名を区切る、 `@` シンボルです。たとえば、現在のログインの名前 **login1** 、Azure SQL Database サーバーの完全修飾名は、 **servername.database.windows.net**, 、接続文字列の username パラメーターにする必要があります: **login1@servername**します。この制限により、ログイン名として選択できるテキストが限定されます。詳細については、次を参照してください。 [CREATE LOGIN (TRANSACT-SQL)](https://msdn.microsoft.com/library/ms189751.aspx)します。

## ログインに対するサーバーレベル権限の付与

サーバーレベル プリンシパル以外のログインによってサーバーレベルのセキュリティを管理するために、Azure SQL Database では、ログイン作成用の **loginmanager** と、データベース作成用の **dbmanager** という 2 つのセキュリティ ロールを用意しています。 これらのデータベース ロールには、**master** データベースのユーザーのみを追加することができます。
> [AZURE.NOTE] ログインまたはデータベースを作成するには、(**master** の論理表現である) **master** データベースに接続する必要があります。

### loginmanager ロール

オンプレミスの SQL Server インスタンスの **securityadmin** 固定サーバー ロールと同様に、Azure SQL Database の **loginmanager** データベース ロールにはログインを作成する権限があります。 新しいログインを作成できるのは、サーバーレベル プリンシパル ログイン (プロビジョニング処理で作成されたもの) または **loginmanager** データベース ロールのメンバーのみです。

### dbmanager ロール

Azure SQL Database **dbmanager** データベース ロールに似ていますが、 **dbcreator** SQL Server の内部設置型インスタンスの固定サーバー ロールです。 データベースを作成できるのは、サーバーレベル プリンシパル ログイン (プロビジョニング処理で作成されたもの) または **dbmanager** データベース ロールのメンバーのみです。 ユーザーのメンバーであると、 **dbmanager** データベース ロール、Azure SQL Database でデータベースを作成することができます `CREATE DATABASE` コマンド、master データベースにコマンドを実行する必要がありますが、そのします。 詳細については、次を参照してください。 [CREATE DATABASE (TRANSACT-SQL)](https://msdn.microsoft.com/library/dn268335.aspx)します。

### SQL Database のサーバーレベル ロールを割り当てる方法

データベースや他のログインを作成できるログインや関連するユーザーを作成するには、次の手順に従います。

1. サーバーレベル プリンシパル ログイン (プロビジョニング処理で作成されたもの) の資格情報、または **loginmanager** データベース ロールの既存のメンバーの資格情報を使用して **master** データベースに接続します。
2. 使用してログインを作成、 `CREATE LOGIN` コマンドです。 詳細については、次を参照してください。 [CREATE LOGIN (TRANSACT-SQL)](https://msdn.microsoft.com/library/ms189751.aspx)します。
3. Master データベースを使用して、そのログインの新しいユーザーを作成、 `CREATE USER` コマンドです。 詳細については、次を参照してください。 [CREATE USER (TRANSACT-SQL)](https://msdn.microsoft.com/library/ms173463.aspx)します。
4. ストアド プロシージャを使用して `sp_addrolememeber` に新しいユーザーを追加する、 **dbmanager** データベース ロール、loginmanager ロールのデータベース ロール、またはその両方です。

次のコード例は、ログイン (**login1**) と対応するデータベース ユーザー (**login1User**) を作成する方法を示しています。このユーザーは、**master** データベースへの接続中に、データベースまたは他のログインを作成することができます。

```
-- first, connect to the master database
CREATE LOGIN login1 WITH password='<ProvidePassword>';
CREATE USER login1User FROM LOGIN login1;
EXEC sp_addrolemember 'dbmanager', 'login1User';
EXEC sp_addrolemember 'loginmanager', 'login1User';
```

> [AZURE.NOTE] ログインを作成する際には、強力なパスワードを使用する必要があります。 詳細については、次を参照してください。 [強力なパスワードの](https://msdn.microsoft.com/library/ms161962.aspx)します。

## ログインのデータベース アクセスの許可

すべてのログインは **master** データベースに作成する必要があります。 ログインが作成された後で、別のデータベースにそのログインのユーザー アカウントを作成できます。 Azure SQL Database では、オンプレミスの SQL Server インスタンスと同じ方法でデータベース ロールをサポートします。

別のデータベースにユーザー アカウントを作成するには (ログインまたはデータベースをまだ作成していないとします)、次の手順に従います。

1. **master** データベースに接続します (**loginmanager** ロールと **dbmanager** ロールを持つログインを使用します)。
2. 使用して新しいログインを作成、 `CREATE LOGIN` コマンドです。 詳細については、次を参照してください。 [CREATE LOGIN (TRANSACT-SQL)](https://msdn.microsoft.com/library/ms189751.aspx)します。 Windows 認証はサポートされません。
3. 使用して新しいデータベースを作成、 `CREATE DATABASE` コマンドです。 詳細については、次を参照してください。 [CREATE DATABASE (TRANSACT-SQL)](https://msdn.microsoft.com/library/dn268335.aspx)します。
4. 新しいデータベースとの接続を確立します (データベースの作成に使用したログインを使用します)。
5. 使用して新しいデータベースに新しいユーザーを作成、 `CREATE USER` コマンドです。 詳細については、次を参照してください。 [CREATE USER (TRANSACT-SQL)](https://msdn.microsoft.com/library/ms173463.aspx)します。

次のコード例では、**login1** という名前のログインと、**database1** という名前のデータベースの作成方法を示します。

```
-- first, connect to the master database
CREATE LOGIN login1 WITH password='<ProvidePassword>';
CREATE DATABASE database1;
```

> [AZURE.NOTE] ログインを作成する際には、強力なパスワードを使用する必要があります。 詳細については、次を参照してください。 [強力なパスワードの](https://msdn.microsoft.com/library/ms161962.aspx)します。

次の例では、**database1** という名前のデータベースに、ログイン **login1** に対応する **login1User** という名前のデータベース ユーザーを作成する方法を示します。 次の例を実行するには、まず database1 データベース内に **ALTER ANY USER** アクセス許可を持つログインを使用して、そのデータベースに新しく接続する必要があります。 **db_owner** ロールのメンバーとして接続するすべてのユーザーは、データベースを作成したログインのように、そのアクセス許可を持ちます。

```
-- Establish a new connection to the database1 database
CREATE USER login1User FROM LOGIN login1;
```

Azure SQL Database におけるこのようなデータベースレベルの権限モデルは、オンプレミスの SQL Server インスタンスと同じです。 詳細については、SQL Server オンライン ブックの次のトピックをご覧ください。

- [管理ログイン、ユーザー、およびスキーマの方法に関するトピック](https://msdn.microsoft.com/library/aa337552.aspx)
- [レッスン 2: データベース オブジェクトに対するアクセス許可を構成します。](https://msdn.microsoft.com/library/ms365345.aspx)

> [AZURE.NOTE] Azure SQL Database のセキュリティ関連の Transact-SQL ステートメントでは、使用できるパラメーターが多少異なる場合があります。 詳細については、特定のステートメントの構文に関するオンライン ブックを参照してください。 

## ログインとデータベースの表示

Azure SQL Database サーバーのログインおよびデータベースを表示するには、master データベースを使用 `sys.sql_logins` と `sys.databases` をそれぞれ表示します。 次の例では、Azure SQL Database サーバーのすべてのログインとデータベースの一覧を表示する方法を示します。

```
-- first, connect to the master database
SELECT * FROM sys.sql_logins;
SELECT * FROM sys.databases; 
```

## 関連項目

[Azure SQL Database のセキュリティのガイドラインと制限事項](sql-database-security-guidelines.md) 
[Azure Active Directory 認証を使用して SQL データベースに接続します。](sql-database-aad-authentication.md)





