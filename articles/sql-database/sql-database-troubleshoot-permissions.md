<properties
    pageTitle="Azure SQL データベースのアクセス許可およびアクセスのトラブルシューティング"
    description="共通のアクセス許可、アクセス、ユーザー、およびログインの問題をトラブルシューティングするための簡単な手順"
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="msmets"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/11/2015"
    ms.author="v-shysun"/>

#一般的な Azure SQL Database のアクセス許可およびアクセスの問題のトラブルシューティング
Azure SQL Database へのアクセスの付与および削除を行う簡単な手順については、このトピックをご覧ください。 より包括的な情報については、次を参照してください。

- [Azure SQL Database におけるデータベースとログインの管理](sql-database-manage-logins.md)
- [SQL Database の保護](sql-database-security)
- [SQL Server データベース エンジンと Azure SQL Database のセキュリティ センター](https://msdn.microsoft.com/library/bb510589)

##論理サーバーの管理パスワードを変更するには
-  [Azure ポータル](https://portal.azure.com) ] をクリックして **の SQL Server**, をリストから、サーバーを選択してクリックして **パスワードのリセット**します。
##権限のある IP アドレスのみがサーバーへのアクセスを許可されるようにするには
- 参照してください [方法: SQL データベースのファイアウォールの設定を構成](sql-database-configure-firewall-settings.md)します。

##ユーザー データベースに包含データベース ユーザーを作成するには
- 使用して、 [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx) ステートメント包含データベースを参照してくださいと [ユーザー - ポータブルなデータベース](https://msdn.microsoft.com/library/ff929188.aspx)します。

## Azure Active Directory を使用して包含データベース ユーザーを認証するには
- 接続を参照してください。 [Azure Active Directory 認証を使用して SQL データベース](sql-database-aad-authentication.md)します。

## 仮想マスター データベースで高い特権を持つユーザーの追加ログインを作成するには
-を使用して、 [CREATE LOGIN](https://msdn.microsoft.com/library/ms189751.aspx) ステートメントのログインの管理を参照してくださいと [Azure SQL Database におけるデータベースとログインを管理する](sql-database-manage-logins.md) 詳細についてです。


