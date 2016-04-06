<properties
   pageTitle="Azure SQL Database のセキュリティのガイドラインと制限事項 | Microsoft Azure"
   description="Microsoft Azure SQL Database のガイドラインとセキュリティに関連する制限事項について説明します。"
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
   ms.date="11/24/2015"
   ms.author="rickbyh"/>

# Azure SQL Database のセキュリティのガイドラインと制限事項

このトピックでは、Microsoft Azure SQL Database のガイドラインとセキュリティに関連する制限事項について説明します。 Azure SQL Database のセキュリティを管理するときには、以下の点に注意してください。

## ファイアウォール

Azure SQL Database サービスは TCP ポート 1433 経由でのみ利用できます。 コンピューターから SQL Database にアクセスするには、ファイアウォールで TCP ポート 1433 での TCP 通信の発信を許可する必要があります。 接続処理の一部として、Azure 仮想マシンからの接続は worker ロールごとに異なる別の IP アドレスとポートにリダイレクトされます。 ポート番号の範囲は、11000 ～ 11999 です。

使用する必要があります、Azure SQL Database サーバーに接続するには、最初に、前に、 [Azure Classic Portal](https://portal.azure.com) または [Azure プラットフォームの旧ポータル](https://manage.windowsazure.com/microsoft.onmicrosoft.com#Workspaces/All/dashboard) Azure SQL データベース ファイアウォールを構成します。 コンピューターまたは Azure から Azure SQL Database サーバーへの接続試行を有効にする、サーバーレベルのファイアウォール設定を作成する必要があります。 また、Azure SQL Database サーバー内の特定のデータベースへのアクセスを制御する場合は、それぞれのデータベースに対するデータベースレベルのファイアウォール ルールを作成します。 詳細については、次を参照してください。 [Azure SQL Database ファイアウォール](sql-database-firewall-configure.md)します。

## 接続の暗号化と証明書の検証

SQL Database とアプリケーション間のすべての通信は、常に暗号化 (SSL) を必要とします。 クライアント アプリケーションが接続時に証明書を検証しない場合、SQL Database への接続は "man in the middle" 攻撃を受けやすくなります。 

アプリケーション コードやツールで証明書を検証するには、暗号化された接続を明示的に要求し、サーバー証明書は信頼しないようにします。 アプリケーション コードやツールが、暗号化された接続を要求しない場合でも、暗号化された接続を受け付けることはできます。 ただし、サーバー証明書は検証されず、"man in the middle" 攻撃を受けやすくなります。

ADO.NET アプリケーション コードで証明書を検証するには、データベース接続文字列で ``Encrypt=True`` と ``TrustServerCertificate=False`` を設定します。 詳細については、次を参照してください。 [コード サンプル: ADO.NET を使用した Azure SQL Database に接続するためのロジックを再試行](https://msdn.microsoft.com/library/azure/ee336243.aspx)します。

SQL Server Management Studio も、証明書の検証をサポートしています。  **サーバーへの接続** ダイアログ ボックスで、をクリックして **接続を暗号化** で、 **接続のプロパティ** ] タブをクリックします。 

> [AZURE.NOTE] SQL Server Management Studio は SQL Server 2008 R2 以前のバージョンで SQL データベースへの接続をサポートしていません。

SQLCMD は SQL Server 2008 以降で SQL Database をサポートしていますが、SQL Server 2008 R2 より前のバージョンでは証明書の検証をサポートしていません。 SQL Server 2008 R2 以降の SQLCMD で証明書を検証するには、``-N`` コマンドライン オプションを使用し、``-C`` オプションは使用しないでください。 -N オプションを使用することで、SQLCMD は暗号化された接続を要求するようになります。 ``-C`` オプションを使用しないことで、SQLCMD はサーバー証明書を暗黙的に信頼しなくなり、証明書の検証が強制されるようになります。 

補足技術情報については、次を参照してください。 [Azure SQL データベースの接続セキュリティ](http://social.technet.microsoft.com/wiki/contents/articles/2951.windows-azure-sql-database-connection-security.aspx#comment-4847) 、TechNet Wiki サイトの記事です。

## 認証

Active Directory 認証 (統合セキュリティ) は、SQL Database V12 のプレビューで使用できます。 AD 認証を構成する方法の詳細については、次を参照してください。 [SQL データベースを使用して Azure Active Directory 認証に接続する](sql-database-aad-authentication.md)です。 プレビューを使用しない場合、ユーザーは SQL Database に接続するたびに、資格情報 (ログイン名とパスワード) を入力する必要があります。 SQL Server 認証の詳細については、次を参照してください。 [認証モードの選択](https://msdn.microsoft.com/library/ms144284.aspx) SQL Server オンライン ブック。 

[SQL Database V12](sql-database-v12-whats-new.md) データベースで包含データベース ユーザーを使用して認証することができます。 詳細については、次を参照してください。 [包含データベース ユーザー - ポータブルなデータベース](https://msdn.microsoft.com/library/ff929188.aspx), 、[CREATE USER (TRANSACT-SQL)](https://technet.microsoft.com/library/ms173463.aspx), 、および [包含データベース](https://technet.microsoft.com/library/ff929071.aspx)します。

> [AZURE.NOTE] Microsoft では、包含データベース ユーザーを使用してスケーラビリティを強化することをお勧めします。

データベース エンジンは、30 分以上アイドル状態が続くと接続を閉じます。 接続する前に、もう一度ログインする必要があります。

SQL Database への継続したアクティブな接続では、少なくとも 10 時間ごとに再認証が必要になります (データベース エンジンによって実行されます)。 データベース エンジンは最初に送信されたパスワードを使用して再認証を試みるため、ユーザー入力は必要ありません。 パフォーマンス上の理由により、パスワードが SQL Database でリセットされると、接続プールが原因で接続がリセットされた場合でも、接続は再認証されません。 これは、オンプレミスの SQL Server とは異なる動作です。 接続が最初に承認されて以来パスワードが変更されている場合は、その接続を終了し、新しいパスワードを使用して新しい接続を作成する必要があります。 KILL DATABASE CONNECTION 権限を持つユーザーを使用して SQL データベースへの接続を明示的に終了、 [KILL](https://msdn.microsoft.com/library/ms173730.aspx) コマンドです。

## ログインとユーザー

SQL Database でのログインとユーザーの管理には、制限があります。

サーバーレベル プリンシパル ログインには、次の制限が適用されます。

- サーバーレベル プリンシパル ログインに対応する master データベースのデータベース ユーザーは、変更または削除できません。 
- サーバー レベル プリンシパル ログインは、2 つのデータベース ロールのメンバーではありませんが **dbmanager** と **loginmanager** で、 **マスター** データベース、これら 2 つのロールに与えられるすべてのアクセス許可があります。

> [AZURE.NOTE] このログインはサーバーのプロビジョニング中に作成され、に似ていますが、 **sa** SQL Server のインスタンスにログインします。

すべてのログインに、次の制限が適用されます。

- 既定の言語は、英語 (米国)です。
- アクセスする、 **マスター** データベース、ログインはすべては、ユーザー アカウントに割り当てる必要があります、 **マスター** データベースです。  **マスター** データベースはサポートが含まれているデータベース ユーザーではありません。
- 接続文字列でデータベースを指定しない場合に接続されます、 **マスター** 既定ではデータベースです。
- 接続している、 **マスター** の実行時に、データベース、 ``CREATE/ALTER/DROP LOGIN`` と ``CREATE/ALTER/DROP DATABASE`` ステートメントです。 
- ADO.NET アプリケーションで ``CREATE/ALTER/DROP LOGIN`` と ``CREATE/ALTER/DROP DATABASE`` ステートメントを実行する場合、パラメーター化コマンドは使用できません。 詳細については、次を参照してください。 [コマンドおよびパラメーター](https://msdn.microsoft.com/library/ms254953.aspx)します。
- ``CREATE/ALTER/DROP DATABASE`` と ``CREATE/ALTER/DROP LOGIN`` ステートメントを実行する場合、これらの各ステートメントは、Transact-SQL バッチ内の唯一のステートメントである必要があります。 一致しないと、エラーが発生します。 たとえば、以下の Transact-SQL は、データベースが存在するかどうかを確認します。 存在する場合は、``DROP DATABASE`` ステートメントが呼び出され、データベースが削除されます。 ``DROP DATABASE`` ステートメントはバッチ内の唯一のステートメントではないので、これを実行すると Transact-SQL はエラーになります。

```
IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
     DROP DATABASE [database_name];
GO
```

- ``CREATE USER`` ステートメントを ``FOR/FROM LOGIN`` オプションと共に実行する場合、これが Transact-SQL バッチ内の唯一のステートメントである必要があります。
- ``ALTER USER`` ステートメントを ``WITH LOGIN`` オプションと共に実行する場合、これが Transact-SQL バッチ内の唯一のステートメントである必要があります。
- サーバー レベル プリンシパル ログインのみとのメンバー、 **dbmanager** データベース ロール、 **マスター** データベースを実行する権限がある、 ``CREATE DATABASE`` と ``DROP DATABASE`` ステートメントです。
- サーバー レベル プリンシパル ログインのみとのメンバー、 **loginmanager** データベース ロール、 **マスター** データベースを実行する権限がある、 ``CREATE LOGIN``, 、``ALTER LOGIN``, 、および ``DROP LOGIN`` ステートメントです。
- ユーザーに対して ``CREATE/ALTER/DROP`` を実行するには、データベースに対する ``ALTER ANY USER`` 権限が必要です。
- データベース ロールの所有者は、追加または他のデータベース ユーザーに、またはそのデータベース ロールを削除するときに、次のエラーが発生する可能性があります: **ユーザーまたはロール 'Name' がこのデータベースに存在しません。** このエラーは、所有者からはユーザーが見えないために発生します。 この問題を解決するには、そのユーザーに対する ``VIEW DEFINITION`` 権限をロールの所有者に許可します。 

ログインとユーザーの詳細については、次を参照してください。 [Azure SQL Database におけるデータベースとログインを管理する](sql-database-manage-logins.md)です。

## セキュリティのベスト プラクティス

Azure SQL Database アプリケーションのセキュリティの脅威に対する脆弱性を低くするには、以下の点を考慮します。

- 常に最新の更新プログラムを使用します。SQL Database に接続するときには、セキュリティの脆弱性を防ぐために、常に最新バージョンのツールとライブラリを使用します。
- TCP ポート 1433 で受信接続をブロックします。アプリケーションが SQL Database と通信するために必要なのは、TCP ポート 1433 での発信接続のみです。 そのコンピューターの他のアプリケーションも受信通信を必要としない場合は、ファイアウォールが TCP ポート 1433 の受信接続のブロックを続けることを確認してください。
- インジェクションの脆弱性を防ぎます。アプリケーションに SQL インジェクションの脆弱性がないようにするには、可能な限りパラメーター化クエリを使用します。 また、コードを徹底的に見直し、アプリケーションをデプロイする前に侵入テストを実行します。


## 関連項目

[Azure SQL Database ファイアウォール](sql-database-firewall-configure.md)

[ファイアウォール設定の構成方法 (Azure SQL Database)](sql-database-configure-firewall-settings.md)

[Azure SQL Database におけるデータベースとログインの管理](sql-database-manage-logins.md)

[SQL Server データベース エンジンと Azure SQL Database のセキュリティ センター](https://msdn.microsoft.com/library/bb510589)


