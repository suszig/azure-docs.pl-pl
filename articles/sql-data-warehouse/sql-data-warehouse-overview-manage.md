<properties
   pageTitle="SQL Data Warehouse の管理ツール | Microsoft Azure"
   description="SQL Data Warehouse の管理ツールの紹介。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="HappyNicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/22/2015"
   ms.author="mausher;nicw;barbkess;JRJ@BigBangData.co.uk;"/>

# SQL Data Warehouse の管理ツール
このトピックでは、SQL Data Warehouse を管理するためのツールやオプションを説明して比較し、ニーズに合ったツールを選択できるようにします。 適切なツールを選択するには、管理対象のデータベースの数、タスク、タスクの実行頻度を考慮します。

## Azure クラシック ポータル
[Azure クラシック ポータル] は、web ベース クラシック ポータルですることができます作成、更新、およびデータベースを削除およびデータベース リソースの監視します。 このツールは、Azure を使い始めたばかりの場合や、管理対象のデータ ウェアハウス データベースの数が少ない場合、またはすばやく作業を行う必要がある場合に便利です。 

ポータルには、現在と過去のパフォーマンス DWU 設定、使用中のストレージ量、成功および失敗した SQL 接続、およびインスタンスで実行されているクエリやそれらの詳細を把握できる視覚化情報とデータのセットが網羅されたメトリックが含まれています。  

## Visual Studio での SQL Server Data Tools   
[SQL Server Data Tools]Visual Studio で (SSDT) は、お使いのコンピューターで実行されへの接続、管理、および、クラウドでデータベースを開発することがでくクライアント ツールです。 Visual Studio やその他の統合開発環境 (IDE) を使い慣れているアプリケーション開発者には、Visual Studio の SSDT の使用をお勧めします。 

SSDT には SQL Server Explorer が含まれており、これを使用して SQL Data Warehouse データベースに対する視覚化、接続、およびスクリプトを実行できます。 SQL Data Warehouse にすばやく接続するにをクリックするだけ、 **Visual Studio で開く** Azure クラシック ポータルで詳細、データベースを表示するときに、コマンド バーのボタンをクリックします。  

SQL Data Warehouse に対するサポートを含む [SQL Server Data Tools] (SSDT) の最新バージョンをダウンロードすることができます。

## コマンド ライン ツール
オプションとして、PowerShell または sqlcmd コマンド ライン ツールを使用して SQL Data Warehouse を管理したり、Azure リソース開発を自動化したりすることができます。 多数の論理サーバーを管理したり、運用環境でリソースの変更をデプロイしたりする場合は、タスクをスクリプト化した後、自動化する必要があるため、これらのツールを使用することをお勧めします。

## 次のステップ
介したヘッドのこれらのツールを使用して、[接続] のトピックを開始します。

<!--Image references-->

<!--Article references-->
[connection]: sql-data-warehouse-develop-connections.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure Classic Portal]: http://portal.azure.com/
