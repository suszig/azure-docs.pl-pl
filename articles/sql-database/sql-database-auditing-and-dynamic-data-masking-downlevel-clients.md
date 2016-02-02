<properties 
    pageTitle="SQL データベース ダウンレベル クライアント サポート Auditing|Microsoft Azure" 
    description="[監査] SQL データベース ダウンレベル クライアントのサポートについて説明します。" 
    services="sql-database" 
    documentationCenter="" 
    authors="nadavhelfman" 
    manager="jeffreyg" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/12/2015" 
    ms.author="nadavhelfman; ronitr"/>


# SQL データベース - 監査ダウンレベル クライアント サポートします。

[監査](sql-database-auditing-get-started.md) TDS リダイレクションに対応する SQL クライアントとは自動的に動作します。

TDS 7.4 を実装するクライアントもリダイレクトをサポートします。 この例外には一部のリダイレクション機能に対応していない JDBC 4.0 とリダイレクションが実装されていない Tedious for Node.JS があります。

「ダウンレベル クライアント」、つまり、TDS バージョンが 7.3 以前のクライアントの場合、接続文字列のサーバー FQDN を変更する必要があります。

接続文字列で元のサーバー FQDN: <*server name*>..database.windows.net を付けて

接続文字列の変更後のサーバー FQDN: <*server name*>. database.**セキュリティで保護された**. 準備完了

「ダウンレベル クライアント」には次が含まれます。

- .NET 4.0 以前
- ODBC 10.0 以前
- JDBC (JDBC は TDS 7.4 対応ですが、一部の TDS リダイレクション機能に対応していません。)
- Tedious (Node.JS 用)

**注記:** 上のサーバー FDQN 変更は SQL サーバー レベル監査ポリシーの適用にも役に立ちます。データベースごとの構成が必要ありません (一時的な軽減)。






