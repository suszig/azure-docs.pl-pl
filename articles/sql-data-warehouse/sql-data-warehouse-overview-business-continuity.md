<properties
   pageTitle="SQL Data Warehouse でのビジネス継続性の計画 | Microsoft Azure"
   description="SQL Data Warehouse でのビジネス継続性の概要。 "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sahaj08"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/22/2015"
   ms.author="sahajs"/>


# SQL Data Warehouse でのビジネス継続性の計画

この記事では、SQL Data Warehouse でビジネス継続性と障害復旧を計画するための基本事項について説明します。 データ ウェアハウスは、ビジネス情報の中央リポジトリであり、あらゆる規模の組織での分析やビジネス インテリジェンスに関する毎日の業務において重要な役割を果たします。 このため、データ ウェアハウスは信頼でき、回復性を持ち、継続的に操作できることが不可欠です。 特にこの記事では、SQL Data Warehouse でデータベースの復元や geo リストアを使用してユーザーのエラーや障害から回復する方法を説明します。

## 概念

ビジネス持続性と障害復旧計画は、次を最適化するために必要です。

**目標復旧時間 (RTO):** データベースは完全に破壊的なイベントの可用性の最大損失など後障害時復旧するまでの最大許容時間。

**目標復旧時点 (RPO):** 障害中のデータの最大損失などの破壊的なイベントから、データベースの復旧時に更新が失われる最大許容時間。

**推定復旧時間 (ERT):** 復元要求の後に完全に機能するために、データベースの推定時間。

## ビジネス継続性のシナリオ

**インフラストラクチャの障害から回復する:** このシナリオはディスク障害などのインフラストラクチャ問題から回復を示します。お客様は、フォールト トレラントで、可用性の高いインフラストラクチャによってビジネス継続性を実現したいと考えます。

**ユーザー エラーから回復する:** このシナリオはデータの破損または削除の意図しない、または付随的損害から回復を示します。 ユーザーが意図せずに、または偶発的にデータを変更または削除した場合、顧客は以前の任意の時点にデータベースを復元することで、ビジネスの継続性を確保したいと考えます。

**(DR) の障害から回復する:** このシナリオは、大災害からの回復を示します。 自然災害や地域的な障害などの破壊的な事象によってデータベースを使用できなくなるシナリオでは、お客様は別のリージョンでデータベースを回復し、業務を続行したいと考えます。


## ビジネス継続性の特徴

前述のシナリオで、SQL Data Warehouse によってデータベースの信頼性を高め、回復性と継続的な操作を行うことができるようにする方法について説明します。 


### データの冗長性

SQL Data Warehouse ではコンピューティングとストレージが分離されるため、すべてのデータは geo 冗長 Azure Storage (RA-GRS) に直接書き込まれます。 地理冗長ストレージでは、プライマリ リージョンから数百マイル離れたセカンダリ リージョンにデータがレプリケートされます。 プライマリとセカンダリの両方のリージョンで、データは別個のフォールト ドメインとアップグレード ドメインの間でそれぞれ 3 回レプリケートされます。 これにより、いずれかのリージョンが使用できなくなるような完全な地域的障害や災害が発生した場合でも、データの永続性が確保されます。 読み取りアクセス Geo 冗長ストレージの詳細については、[Azure storage 冗長オプション] を参照してください。

### データベースの復元

データベースの復元は、以前の任意の時点にデータベースが復元されるように設計されます。 Azure SQL Data Warehouse サービスは、少なくとも 8 時間ごとにストレージのスナップショットを自動的に取得し、それらを 7 日間保持して復元ポイントの個別セットを用意することで、すべてのデータベースを保護します。 これらのバックアップは RA-GRS Azure Storage に保存されるため、規定で地理冗長になります。 自動のバックアップと復元の機能は追加料金なしで使用でき、偶発的な破損や削除からデータベースをコストや管理不要で保護できます。 データベースの復元に関する詳細については、[ユーザー エラーから回復する] を参照してください。

### geo リストア

geo リストアは、破壊的なイベントが原因でデータベースを使用できなくなった場合にデータベースを回復するように設計されています。 サポート担当者に連絡して geo 冗長バックアップからデータベースをを復元し、任意の Azure リージョンに新しいデータベースを作成できます。 バックアップは geo 冗長であるため、災害によってデータベースにアクセスできない場合でも、それを使用してデータベースを回復できます。 geo リストア機能によって、追加料金は発生しません。


## 次のステップ
その他の SQL Database エディションのビジネス継続性機能については、[SQL Database のビジネス継続性の概要] を参照してください。

<!--Image references-->

<!--Article references-->
[business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Finalize a recovered database]: ../sql-database/sql-database-recovered-finalize.md
[Azure storage redundancy options]: storage-redundancy/#read-access-geo-redundant-storage-ra-grs.md
[SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Recover from user error]: sql-data-warehouse-business-continuity-recover-from-user-error.md

<!--MSDN references-->
[Create database restore request]: http://msdn.microsoft.com/library/azure/dn509571.aspx
[Database operation status]: http://msdn.microsoft.com/library/azure/dn720371.aspx
[Get restorable dropped database]: http://msdn.microsoft.com/library/azure/dn509574.aspx
[List restorable dropped databases]: http://msdn.microsoft.com/library/azure/dn509562.aspx

<!--Other Web references-->



