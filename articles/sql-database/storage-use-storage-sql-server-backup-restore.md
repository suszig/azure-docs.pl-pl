<properties
    pageTitle="Azure Storage を使用した SQL Server のバックアップと復元の方法 | Microsoft Azure"
    description="SQL Server および SQL Database を Azure Storage にバックアップします。 SQL データベースを Azure Storage にバックアップすることのメリットについて、また SQL Server および Azure Storage のどのコンポーネントが必要かについて説明します。"
    services="sql-database, virtual-machines"
    documentationCenter=""
    authors="carlrabeler"
    manager="jeffreyg"
    editor="tysonn"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="vm-windows-sql-server"
    ms.topic="article"
    ms.date="10/20/2015"
    ms.author="carlrab"/>



# Azure Storage を使用した SQL Server のバックアップと復元の方法

## 概要

SQL Server のバックアップを Azure Blob Storage サービスに書き込むことができる機能は SQL Server 2012 SP1 CU2 でリリースされました。 この機能を使用すると、オンプレミスの SQL Server データベースまたは Azure 仮想マシンの SQL Server データベースを使用する Azure BLOB サービスとの間でバックアップおよび復元できます。 クラウドへのバックアップには、高い可用性、無制限の社外ストレージのgeo レプリケーション、クラウドとの間でのデータ移行の容易さという利点があります。   Transact-SQL または SMO を使用して BACKUP または RESTORE ステートメントを発行できます。 さらに、データベース ファイルは Azure blob に格納され、SQL Server 2016 を使用して、使用できます [ファイル スナップショット バックアップ](http://msdn.microsoft.com/library/mt169363.aspx) をほぼ瞬時のバックアップと非常にすばやく復元を実行します。

## SQL Server のバックアップに Azure BLOB サービスを使用する利点

バックアップには一般的に、ストレージの管理、ストレージ障害のリスク、社外ストレージへのアクセス、デバイスの構成などの課題があります。 これらの課題は、オンプレミスのデータベース インスタンスと Azure 仮想マシンのデータベース インスタンスの両方に存在します。 次に示しているのは、SQL Server のバックアップに Azure Blob Storage サービスのストレージを使用する主な利点です。

* 柔軟性と信頼性が高い無制限の社外ストレージ: Azure BLOB にバックアップを保存することで、社外ストレージへのアクセスが柔軟かつ簡単に行え、便利になります。 SQL Server のバックアップを変更、既存のスクリプトやジョブを使用すると同じくらい簡単にすることができます、オフサイト ストレージを作成する、 **BACKUP TO URL** 構文です。 社外ストレージは通常、運用データベースから十分に離れた場所に設置して、一度の災害によって社外ストレージと運用データベースの両方が影響を受けないようにする必要があります。 選択することによって [geo レプリケートした、Azure blob](../storage/storage-redundancy.md), 、リージョン全体に影響を与える障害が発生した場合の保護の追加のレイヤーがあります。 
* バックアップ アーカイブ: Azure Blob Storage サービスは、バックアップ アーカイブによく利用されているテープに代わる、優れた手法を提供しています。 テープ ストレージでは、場合によって社外設備への物理的な搬送やメディア保護の対策が必要です。 Azure Blob Storage へのバックアップの保存では、可用性と持続性に優れた高速アーカイブが可能です。
* ハードウェア管理のオーバーヘッドが不要: Azure サービスではハードウェア管理のオーバーヘッドはありません。 Azure サービスのハードウェア管理では、ハードウェア障害に対する冗長性実現と保護のためにgeo レプリケーションが行われます。
* 現在、Azure 仮想マシンで実行されている SQL Server のインスタンスについては、アタッチされたディスクを作成することで Azure Blob Storage サービスへのバックアップを実行できます。 ただし、バックアップ用に Azure 仮想マシンにアタッチできるディスクの数には制限があります。 この制限はインスタンス サイズが XL の場合は 16 ディスクです。インスタンス サイズが小さくなるほど、このディスク数は少なくなります。 Azure BLOB への直接バックアップを有効にすると、実質的に無制限のストレージにアクセスできます。
* Azure BLOB に格納されたバックアップはいつでもどこでも使用でき、データベースのアタッチ/デタッチまたは VHD のダウンロードやアタッチの必要なく、オンプレミスの SQL Server、または Azure 仮想マシンで実行されている別の SQL Server に復元するために簡単にアクセスできます。
* コスト面の利点: 使用するサービスに対してのみ課金されます。 社外ストレージやバックアップ アーカイブ用にこのサービスを選択することで、高い費用対効果を得られます。 参照してください、 [Azure 料金計算ツール](http://go.microsoft.com/fwlink/?LinkId=277060 "料金計算ツール"), 、および [Azure の料金に関する記事](http://go.microsoft.com/fwlink/?LinkId=277059 "料金に関する記事") の詳細。
* ストレージのスナップショットを活用します。 データベース ファイルは Azure blob に格納され、SQL Server 2016 を使用している、ときに行うこともできます [ファイル スナップショット バックアップ](http://msdn.microsoft.com/library/mt169363.aspx) をほぼ瞬時のバックアップと復元の非常に簡単に実行します。

詳細については、次を参照してください。 [SQL Server のバックアップと Azure Blob ストレージ サービスによる復元](http://go.microsoft.com/fwlink/?LinkId=271617)します。

次の 2 つのセクションでは、Azure Blob Storage サービスのコンポーネントについて、さらに Azure Blob Storage サービスとの間のバックアップまたは復元に使用する SQL Server のコンポーネントについて概要を示します。 重要なのは、それらのコンポーネントを理解するだけでなく、コンポーネント間のやり取りも把握してから、Azure Blob Storage サービスとの間でバックアップまたは復元を行うことです。

Azure アカウントの作成がこのプロセスの最初の手順です。 ストレージ アカウントの作成を SQL Server 2014 を使用して単純な復元を実行する完全なチュートリアルを参照してください。 [Azure ストレージ サービスへの SQL Server のバックアップと復元の概要](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx)します。 ストレージ アカウントの作成を SQL Server 2014 を使用して単純な復元を実行する完全なチュートリアルを参照してください。 [チュートリアル: SQL Server 2016 データベースで、Microsoft Azure Blob ストレージ サービスを使用して](https://msdn.microsoft.com/library/dn466438.aspx)します。

## Azure Blob Storage サービスのコンポーネント

* ストレージ アカウント: ストレージ アカウントはすべてのストレージ サービスの出発点となります。 Azure Blob Storage サービスにアクセスするには、まず Azure のストレージ アカウントを作成します。 Azure Blob ストレージ サービスの詳細については、次を参照してください [Azure Blob ストレージ サービスを使用する方法。](http://azure.microsoft.com/develop/net/how-to-guides/blob-storage/)

* コンテナー: コンテナーは一連の BLOB のグループ化に使用され、格納できる BLOB の数に制限はありません。 SQL Server のバックアップを Azure BLOB サービスに書き込むには、少なくとも root コンテナーが作成されている必要があります。

* BLOB: 任意の種類およびサイズのファイルです。 Blob は、次の URL 形式を使用してアドレス指定できます。 `https://<storage account>.blob.core.windows.net/<container>/<blob>`
ページ Blob の詳細については、次を参照してください [理解ブロック blob およびページ Blob。](http://msdn.microsoft.com/library/azure/ee691964.aspx)

## SQL Server のコンポーネント

* URL: URL は一意のバックアップ ファイルへの Uniform Resource Identifier (URI) を参照します。 URL を使用して SQL Server バックアップ ファイルの場所と名前を指定します。 URL は、コンテナーだけでなく実際の BLOB を参照する必要があります。 BLOB が存在しない場合は作成されます。 既存の BLOB を指定した場合、> WITH FORMAT オプションを指定していないと、BACKUP は失敗します。
BACKUP コマンドで URL を指定する例を次に示します。
**`http[s]://ACCOUNTNAME.Blob.core.windows.net/<CONTAINER>/<FILENAME.bak>`

<b>注:</b> HTTPS は必須ではありませんが、使用することをお勧めします。
<b>重要</b>
バックアップ ファイルをコピーして Azure BLOB ストレージ サービスにアップロードする場合、そのファイルを復元処理に使用する予定があれば、ストレージ オプションの BLOB の種類としてページ BLOB を使用する必要があります。 BLOB の種類としてブロック BLOB を使用すると、RESTORE がエラーで失敗します。

* 資格情報: Azure Blob Storage サービスに対する接続と認証に必要な情報は資格情報として保存されます。  SQL Server から Azure BLOB に対してバックアップを書き込んだり復元したりするには、SQL Server 資格情報を作成する必要があります。 詳細については、次を参照してください。 [SQL Server 資格情報](https://msdn.microsoft.com/library/ms189522.aspx)します。

## Azure BLOB を使用した SQL Server データベースのバックアップと復元 - 概念とタスク:

**概念、考慮事項、コード サンプル:**

[Azure BLOB ストレージ サービスを使用した SQL Server のバックアップと復元](http://go.microsoft.com/fwlink/?LinkId=271617)

**概要チュートリアル:**

[チュートリアル: Microsoft Azure ストレージ サービス内の SQL Server データ ファイル](https://msdn.microsoft.com/library/dn466438.aspx)

**ベスト プラクティス、トラブルシューティング:**

[バックアップと復元に関するベスト プラクティス (Azure BLOB ストレージ サービス)](http://go.microsoft.com/fwlink/?LinkId=272394)

