<properties 
   pageTitle="Azure SQL Database Web および Business Edition 終了に関する FAQ |Microsoft Azure"
   description="Azure SQL Web および Business データベースの提供終了日と新しいサービス階層の機能について説明しています。"
   services="sql-database"
   documentationCenter="na"
   authors="stevestein"
   manager="jeffreyg"
   editor="monicar" />
<tags 
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/30/2015"
   ms.author="sstein" />

# Web および Business Edition の終了に関する FAQ

Azure SQL Web データベースおよび Business データベースは提供終了になりました。 Basic、Standard、Premium、およびエラスティック階層は、提供終了になった Web データベースおよび Business データベースの代わりです。

Web データベースおよび Business データベースをアップグレードする際、SQL Database サービスでは、そのワークロード履歴に基づいた適切なサービス階層とパフォーマンス レベル (価格レベル) をお勧めしています。

**お勧めの価格レベルは次のとおりです。**

- [Azure ポータルを使用し、SQL Database V12 にアップグレードします](sql-database-upgrade-server-portal.md)
- [PowerShell を使用し、SQL Database V12 にアップグレードします](sql-database-upgrade-server-powershell.md)
- [Web または Business データベースの価格レベルを変更します](sql-database-service-tier-advisor.md)
 


## Azure ポータルで Web Edition および Business Edition のデータベースが提供終了と表示されるのはどうしてですか

Web Edition および Business Edition のデータベースは、2015年 9 月以降利用できなくなるため、ポータルでは、Web データベースおよび Business データベースに提供終了のラベル表示をしています。 Web Edition および Business Edition のデータベースはまだ通常どおりプロビジョニングおよび管理できますが、新しいデータベースでは、Basic、Standard、または Premium サービス階層を使用することが最善であるということを提供終了のラベルで注意喚起しています。 また、すべての Web Edition および Business Edition のデータベースを Standard、Basic、または Premium にアップグレードする必要があることも、提供終了のラベルで注意喚起しています。 既存の Web または Business データベースを新しいサービス階層にアップグレードする方法の詳細については、次を参照してください。 [を新しいサービス階層 SQL Database Web/ビジネス データベースをアップグレード](sql-database-upgrade-new-service-tiers.md)します。

## 既存の Web Edition および Business Edition のデータベースをアップグレードする最適な新しいサービス階層はどれでしょうか

既存の Web データベースまたは Business データベースに適した新しいサービス階層とパフォーマンス レベルを選択する場合、アプリケーションで必要な具体的な機能およびパフォーマンスを考慮する必要があります。 

適切な新しいサービス階層を選択するための詳細について、上記に記載価格レベルの提案を使用して、参照してください [を新しいサービス階層 SQL Database Web/ビジネス データベースをアップグレード](sql-database-upgrade-new-service-tiers.md)します。

## Microsoft が新しいサービス階層を導入するのはなぜですか

Azure SQL Database には、お客様からのフィードバックに基づいて、リレーショナル データベースのワークロードをより簡単にサポートする新しいサービス層が導入がされました。 新しい階層は、予測可能なパフォーマンスを実現するよう、アプリケーションの少量から多量のトランザクション要件に 7 段階の範囲で対応するよう設計されています。 新しい階層では、一連のビジネス継続性機能と強化された稼働時間 SLA が提供され、低料金でより大きなデータベース サイズを実現し、課金処理も改善されています。

## 新しいサービス階層に関する説明はどこで確認できますか

新しいサービス階層とパフォーマンス モデルの詳細については、次を参照してください。 [サービス階層](sql-database-service-tiers.md)します。 新しいサービス階層の価格情報についてを参照してください。 [SQL Database 料金](http://azure.microsoft.com/pricing/details/sql-database/)します。

## Basic、Standard、および Premium で利用できなくなった機能は何ですか

フェデレーション機能が Web および Business Edition で提供終了となります。 各自のデータベースをスケール アウトする必要があるお客様は、代わりに使用または移行することをお勧め [エラスティック データベース ツール](sql-database-elastic-scale-get-started.md) の [Azure SQL Database](sql-database-elastic-scale-get-started.md), 、構築およびシャーディングを使用するアプリケーションの管理が単純化します。 アプリケーションで .NET クライアント ライブラリを使用すると、データをシャードにマッピングして OLTP リクエストを適切なデータベースにルーティングする方法を定義できます。 データをシャードにどのように割り振るかを再構成する管理操作に役立つように、お客様の Azure サブスクリプションの中でご利用いただける Azure クラウド サービス テンプレートが付属しています。 他に、 [エラスティック データベース ツール](sql-database-elastic-scale-get-started.md), を作成して発行を引き続き [カスタム シャーディング パターンし、プラクティスのガイダンス](https://msdn.microsoft.com/library/azure/dn764977.aspx) 協力による詳細なお客様との契約に基づきます。 スケール アウト機能を必要がある新しいお客様のチェック アウトを行う [エラスティック データベース ツール](sql-database-elastic-scale-get-started.md) をそれぞれのオプションを評価する Microsoft サポートにお問い合わせやします。

また、Premium データベースのデータベースのコピー操作も変わります。 以前はプレミアム データベース クォータが限られており、データベースを作成してください. コピーとしては、T-SQL では、Business データベースと同じレートで課金されている予約済みリソースなしで Suspended Premium データベースを作成します。 現在では、Premium クォータはより自由に使用できるようになり、Suspended Premium はサポートされなくなりました。 これからは、データベースのコピーは、ソースと同じエディションとパフォーマンス レベルで作成され、それに応じて課金されます。 必要に応じて、コピーされたデータベースを別のサービス階層やパフォーマンス レベルにダウングレードしてコストを下げることができます。 今回のリリースの一環として、既存の Suspended Premium データベースは Business Edition に変換される予定です。 ポイントインタイム リストアの導入により、データベースのバックアップ コピーを作成する必要性が軽減されると予想されます。

## Basic、Standard、および Premium での課金処理に改善はありますか

Basic、Standard、Premium の Azure SQL Database は時間単位で課金され、各データベースは 24 時間に 4 回スケールアップまたはスケールダウンすることができます。 1 時間ごとに選択する最上位のサービス階層とパフォーマンス レベルに基づいて固定率で課金されます。 また、請求書には、1 か月の (Basic、S1、P2 などの) パフォーマンス レベルごとのデータベース日数および時間の内訳が記載され、簡単に確認できるようになっています。 Web および Business データベースは引き続き、データベース サイズに基づくデータベース ユニットを使用して課金されます。 参照してください、 [SQL Database の料金のページ](http://azure.microsoft.com/pricing/details/sql-database/) を料金や新しいサービス階層間の違いの詳細を参照してください。


## 関連項目

[Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)

[Web および Business の価格](https://azure.microsoft.com/pricing/details/sql-database/web-business/)

[サービス階層](sql-database-service-tiers.md)

[SQL データベース Web/Business データベースを新しいサービス階層にアップグレードする](sql-database-upgrade-new-service-tiers.md)

