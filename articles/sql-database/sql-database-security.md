<properties
   pageTitle="SQL Database のセキュリティの概要"
   description="認証、承認、接続のセキュリティ、暗号化、コンプライアンスなどに関するクラウドと SQL Server オンプレミスの違いなど、Azure SQL Database と SQL Server のセキュリティの詳細について説明します。"
   services="sql-database"
   documentationCenter=""
   authors="tmullaney"
   manager="jeffreyg"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/21/2015"
   ms.author="thmullan;jackr"/>


# SQL Database の保護

## 概要

この記事は、Azure SQL Database を使用してアプリケーションのデータ層をセキュリティで保護するための基本事項について説明します。 具体的には、この記事が、データの保護、アクセスを制限するためのリソースの概要を取得しで作成されたデータベースでのアクティビティの監視、 [データベース ファイルの使用」チュートリアルを](sql-database-get-started.md)します。 SQL のすべてのエディションで利用できるセキュリティ機能の完全な詳細については、 [SQL Server データベース エンジンと Azure SQL Database セキュリティ センター](https://msdn.microsoft.com/library/bb510589)します。 その他の情報が記載されても、 [セキュリティ、および Azure SQL データベースのテクニカル ホワイト ペーパー](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF)。 

## 接続のセキュリティ

接続のセキュリティとは、ファイアウォール ルールと接続の暗号化を使用して、データベースへの接続を制限し、保護する方法のことです。

ファイアウォール ルールはサーバーとデータベースの両方で使用され、明示的にホワイト リストに登録されていない IP アドレスからの接続試行を拒否します。 新しいデータベースへの接続を試行するために、アプリケーションまたはクライアント コンピューターのパブリック IP アドレスを許可するには、まず Microsoft Azure クラシック ポータル、REST API、または PowerShell を使用して、サーバーレベルのファイアウォール ルールを作成する必要があります。 ベスト プラクティスとして、可能な限りサーバーのファイアウォールにより許可される IP アドレスの範囲を制限する必要があります。 詳細については、次を参照してください。 [Azure SQL Database ファイアウォール](https://msdn.microsoft.com/library/ee621782)します。

データがデータベースとの間で "送受信中" である間は、常に Azure SQL Database への接続をすべて (SSL/TLS を使用して) 暗号化する必要があります。 アプリケーションの接続文字列では、接続を暗号化するパラメーターを指定する必要があり、 *いない* それ以外の場合 (これは、自動的に実行 Azure クラシック ポータルから接続文字列をコピーする場合)、サーバー証明書を信頼する、接続がサーバーの身元が確認されず、および「- the-中間」攻撃を受けやすくなります。 ADO.NET ドライバー、たとえば、これらの接続文字列パラメーターは **Encrypt = True** と **TrustServerCertificate = False**します。 詳細については、次を参照してください。 [Azure SQL データベース接続の暗号化と証明書の検証](https://msdn.microsoft.com/library/azure/ff394108#encryption)します。


## 認証

認証とは、データベースへの接続時に ID を証明する方法のことです。 SQL Database は、2 種類の認証をサポートしています。

 - **SQL 認証**, 、ユーザー名とパスワードを使用します。
 - **Azure の Active Directory 認証**, 、管理され、ドメインを統合を Azure Active Directory によって管理される id を使用しのサポート

データベースの論理サーバーを作成したときに、ユーザー名とパスワードによる "サーバー管理" ログインを指定したとします。 これらの資格情報を使用すると、データベース所有者、つまり "dbo" として、そのサーバーにある任意のデータベースを認証できます。 Azure Active Directory 認証を使用する場合は、"Azure AD 管理者" という、Azure AD ユーザーとグループを管理できるサーバー管理者を別に作成する必要があります。 この管理者は、通常のサーバー管理者が実行できるすべての操作も実行できます。 参照してください [SQL データベースを使用して Azure Active Directory 認証に接続する](sql-database-aad-authentication.md) を Azure Active Directory 認証を有効にする Azure AD 管理者を作成する方法のチュートリアルです。

ベスト プラクティスとしては、アプリケーションで別のアカウントを使用して認証することをお勧めします。この方法により、アプリケーションに付与されるアクセス許可を制限し、アプリケーション コードが SQL インジェクション攻撃に対して脆弱な場合に、悪意のあるアクティビティのリスクを軽減できます。 作成する推奨されるアプローチです、 [包含データベース ユーザー](https://msdn.microsoft.com/library/ff929188), 、アプリケーションの 1 つのデータベースに直接認証できます。 サーバー管理者権限のログインでユーザー データベースに接続しているときに、次の T-SQL コマンドを実行することで、SQL 認証を使用する包含データベース ユーザーを作成できます。

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Azure AD 管理者を作成した場合、Azure AD 管理者権限でユーザー データベースに接続しているときに、次の T-SQL コマンドを実行することで、Azure Active Directory 認証を使用する包含データベース ユーザーを作成できます。

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

いずれの場合でも、アプリケーションの接続文字列では、サーバー管理者のログインではなく、このユーザー資格情報を指定してデータベースに接続します。

SQL Database の認証の詳細については、次を参照してください。 [Azure SQL Database におけるデータベースの管理とログイン](sql-database-manage-logins.md)します。


## Authorization
承認とは、Azure SQL Database で許可される操作を示すものであり、ユーザー アカウントのロールのメンバーシップとアクセス許可によって制御されます。 ベスト プラクティスとして、必要最低限の特権をユーザーに付与することをお勧めします。 Azure SQL Database では、T-SQL のロールを使用して承認を容易に管理できます。

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

接続しているサーバー管理者のアカウントは db_owner のメンバーであり、データベース内ですべての操作を実行する権限を持ちます。 スキーマのアップグレードやその他の管理操作をデプロイするために、このアカウントを保存します。 アクセス許可が限定された "ApplicationUser" アカウントを使用して、アプリケーションで必要な最小限の特権により、アプリケーションをデータベースに接続します。

ユーザーが Azure SQL Database で実行できる操作をさらに制限する方法がいくつかあります。

* [データベース ロール](https://msdn.microsoft.com/library/ms189121) 以外の db_datareader と db_datawriter を使用してより強力なアプリケーション ユーザー アカウントまたはより権限の小さな管理アカウントを作成します。
* 詳細な [権限](https://msdn.microsoft.com/library/ms191291) 制御できる操作は個々 の列、テーブル、ビュー、プロシージャ、およびデータベース内の他のオブジェクトを使用します。
* [権限借用](https://msdn.microsoft.com/library/vstudio/bb669087) と [モジュール署名](https://msdn.microsoft.com/library/bb669102) 安全にアクセス許可を一時的に昇格することができます。
* [行レベルのセキュリティ](https://msdn.microsoft.com/library/dn765131) 使用できる行をユーザー数の上限にアクセスできます。
* [データ マスク](sql-database-dynamic-data-masking-get-started.md) 機密データの露出を制限するために使用できます。
* [ストアド プロシージャ](https://msdn.microsoft.com/library/ms190782) データベースに対して実行できるアクションを制限するために使用できます。

Azure クラシック ポータルまたは Azure リソース マネージャー API を使用したデータベースと論理サーバーの管理は、ポータル ユーザー アカウントのロールの割り当てによって制御されます。 このトピックの詳細については、次を参照してください。 [Azure ポータルでのロールベースのアクセス制御](../role-based-access-control-configure.md)します。


## 暗号化

Azure SQL データベースが"静止"状態にあるときに、データを暗号化することによって、データを保護できますか、データベース ファイルとを使用して、バックアップに格納されている [透過的なデータ暗号化](http://go.microsoft.com/fwlink/?LinkId=526242)します。 データベースを暗号化するには、データベースの所有者として接続し、次のコマンドを実行します。

```
CREATE DATABASE ENCRYPTION KEY
   WITH ALGORITHM = AES_256
   ENCRYPTION BY SERVER CERTIFICATE ##MS_TdeCertificate##;

ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

その他の方法で機密データを暗号化するには、次を検討してください。

* [セル レベルの暗号化](https://msdn.microsoft.com/library/ms179331.aspx) 異なる暗号化キーを持つ特定の列またはセルのデータを暗号化します。
* ハードウェア セキュリティ モジュールか、暗号化キー階層の集中管理する場合は、使用を検討して [Azure VM で SQL Server と Azure Key Vault](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx)します。
* [常に暗号化](https://msdn.microsoft.com/library/mt163865.aspx) (プレビュー) で暗号化透過的にアプリケーション行い、クライアントが SQL データベースの暗号化キーを共有せずにクライアント アプリケーション内の機密データを暗号化できるようにします。

## 監査

データベースの監査イベントと追跡イベントは、規制遵守の維持や、疑わしいアクティビティの特定に役立ちます。 SQL Database の監査により、Azure ストレージ アカウントの監査ログにデータベースのイベントを記録できます。 また SQL Database の監査を Microsoft Power BI と統合することにより、詳細なレポートと分析が容易になります。 詳細については、次を参照してください。 [SQL Database の監査を使ってみる](sql-database-auditing-get-started.md)します。

## コンプライアンス

アプリケーションがさまざまなセキュリティ コンプライアンスの要件を満たすのに役立つ上記の機能以外にも、Azure SQL Database は定期的な監査に参加し、さまざまなコンプライアンス基準に認定されています。 詳細については、次を参照してください。、 [Microsoft Azure トラスト センター](http://azure.microsoft.com/support/trust-center/), の最新の一覧を見つけることができますが、 [SQL Database コンプライアンス証明書](http://azure.microsoft.com/support/trust-center/services/)します。

