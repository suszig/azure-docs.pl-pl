<properties 
    pageTitle="エラスティック データベース プールにユーザーを追加する方法" 
    description="プール内の各 db に特権を持つユーザーを追加する必要があります。" 
    metaKeywords="azure sql database elastic databases credentials" 
    services="sql-database" documentationCenter=""  
    manager="jeffreyg" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/03/2015" 
    ms.author="ddove; sidneyh" />

# エラスティック データベース プールにユーザーを追加する方法

 **弾力性データベース ジョブ** 機能 (プレビュー) では、グループ全体、データベースのカスタム定義のコレクションを含むデータベースの TRANSACT-SQL スクリプトを実行することができます、 **弾力性データベース プール** または **エラスティック データベース シャード セット** Azure SQL データベースにします。 スクリプトを実行するには、ジョブを実行する各データベースに適切なアクセス許可を持つユーザーを追加する必要があります。 詳細については、次を参照してください [Azure SQL Database におけるデータベースの管理とログイン](sql-database-manage-logins.md) または [Your SQL Azure データベースにユーザーの追加。](http://azure.microsoft.com/blog/2010/06/21/adding-users-to-your-sql-azure-database/)

## 前提条件
* インストール、 [弾力性ジョブ コンポーネント](sql-database-elastic-jobs-service-installation.md)します。 

## データベースにユーザーを追加する方法

1.  最初に接続 **マスター** 弾力性データベース プール内のデータベースが存在して、インストール時に提供された同じ資格情報を使用して、新しいログインを作成する Azure SQL Database サーバーの **弾力性データベース ジョブ**します。

        CREATE LOGIN login1 WITH password='<ProvidePassword>';

2. プール内の各データベースにログインし、同じ名前とパスワードを使用してユーザーを作成します。 ユーザーには、ジョブを実行するための十分な権限が必要です。 このコードは、各データベースで実行する必要があります。

        CREATE USER admin1 FROM LOGIN login1;
        
3. ユーザーには、ジョブに指定されたスクリプトを実行するための十分な権限が必要です。 使用して、 [ALTER ROLE](https://msdn.microsoft.com/library/ms189775.aspx) にスクリプトを正常に実行する最低限必要なアクセス許可をユーザーに提供します。 

## 次のステップ

Azure ポータルでジョブを作成および管理を参照してください。 [の作成と弾力性データベース ジョブを管理する](sql-database-elastic-jobs-create-and-manage.md)です。 PowerShell を使用してジョブを作成するには、「 [を作成し、PowerShell (プレビュー) を使用して SQL Database エラスティック データベース ジョブの管理](sql-database-elastic-jobs-powershell.md)

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->

 
