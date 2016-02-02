<properties
    pageTitle="SQL Server のバックアップと復元 | Microsoft Azure"
    description="Azure Virtual Machines で実行されている SQL Server データベースのバックアップと復元に関する考慮事項について説明します。"
    services="virtual-machines"
    documentationCenter="na"
    authors="rothja"
    manager="jeffreyg"
    editor="monicar"
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="11/13/2015"
    ms.author="jroth" />


# Azure Virtual Machines おける SQL Server のバックアップと復元

## 概要

SQL Server データベースのデータのバックアップは、アプリケーション エラーやユーザー エラーによるデータ損失から保護する戦略の重要な要素です。 これは、Azure Virtual Machines (VM) で実行されている SQL Server にも同様に当てはまります。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。

Azure VM で実行されている SQL Server では、バックアップ ファイルの保存先として接続されたディスクを使用するネイティブなバックアップおよび復元手法を使用できます。 ただし、に基づいて、Azure の仮想マシンにアタッチできるディスクの数に制限は、 [仮想マシンのサイズ](virtual-machines-size-specs.md)します。 また、ディスク管理のオーバーヘッドも考慮する必要があります。

SQL Server 2014 以降では、Microsoft Azure BLOB ストレージへのバックアップと復元が可能です。 SQL Server 2016 には、このオプションの拡張機能も用意されています。 また、データベース ファイルが Microsoft Azure BLOB ストレージに保存されている場合、SQL Server 2016 では、Azure のスナップショットを使用して、ほぼ瞬時にバックアップを作成し、迅速に復元できます。 この記事は、これらのオプションの概要を説明し、追加情報が掲載されて [SQL Server のバックアップと復元 Microsoft Azure Blob ストレージ サービスと](https://msdn.microsoft.com/library/jj919148(v=sql.130).aspx)します。
>[AZURE.NOTE] 非常に大規模なデータベースをバックアップするためのオプションの詳細については、次を参照してください。 [テラバイト規模の SQL Server データベースのバックアップ方法 Azure Virtual Machines の](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx)します。

以降のセクションには、Azure 仮想マシンでサポートされているさまざまなバージョンの SQL Server に固有の情報が記載されています。

## データベース ファイルが Microsoft Azure BLOB サービスに保存されている場合のバックアップに関する考慮事項

データベース ファイルが Microsoft Azure BLOB ストレージに保存されている場合、データベースのバックアップを実行する理由や、基になるバックアップ テクノロジ自体が変わります。 データベース ファイルを Azure blob ストレージに格納する方法についての詳細については、次を参照してください。 [Azure で SQL Server データ ファイル](https://msdn.microsoft.com/library/jj919148.aspx)します。

- Microsoft Azure では、Microsoft Azure サービスの一部として、ハードウェアやメディアの障害からの保護が提供されるため、この保護のためにデータベースのバックアップを実行する必要はなくなりました。

- ユーザー エラーからの保護、保存目的、規制上の理由、または管理目的で、データベースのバックアップを引き続き実行する必要があります。

- Microsoft SQL Server 2016 Community Technology Preview 3 (CTP3) の SQL Server ファイル スナップショット バックアップ機能を使用して、ほぼ瞬時のバックアップと迅速な復元を実行できます。 詳細については、次を参照してください。 [Azure でのデータベース ファイルのファイル スナップショット バックアップ](https://msdn.microsoft.com/library/mt169363.aspx)します。

## Microsoft SQL Server 2016 Community Technology Preview 3 (CTP3) でのバックアップと復元

Microsoft SQL Server 2016 Community Technology Preview 3 (CTP3) をサポートしています、 [Azure blob したバックアップし、復元](https://msdn.microsoft.com/library/jj919148.aspx) 機能については、SQL Server 2014 と以下で説明します。 ただし、次の拡張機能も用意されています。

- **ストライピング**: Microsoft Azure BLOB ストレージにバックアップする場合、SQL Server 2016 では、複数の BLOB へのバックアップをサポートしているので、最大 12.8 TB の大規模なデータベースをバックアップできます。

- **スナップショット バックアップ**: Azure のスナップショットを使用することで、SQL Server ファイル スナップショット バックアップでは、Azure BLOB ストレージ サービスを使用して保存されたデータベース ファイルをほぼ瞬時にバックアップし、迅速に復元できます。 この機能により、バックアップと復元のポリシーを簡素化することができます。 ファイル スナップショット バックアップでは、ポイントインタイム リストアもサポートしています。 詳細については、次を参照してください。 [Azure でのデータベース ファイルのスナップショット バックアップ](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx)します。

- **マネージ バックアップのスケジュール設定**: Azure への SQL Server マネージ バックアップでカスタム スケジュールがサポートされるようになりました。 詳細については、次を参照してください。 [SQL Server Managed Backup to Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx)します。

>[AZURE.NOTE] Azure Blob ストレージを使用する場合に、SQL Server 2016 の機能のチュートリアルについては、次を参照してください。 [チュートリアル: SQL Server 2016 データベースで、Microsoft Azure Blob ストレージ サービスを使用して](https://msdn.microsoft.com/library/dn466438.aspx)します。

## SQL Server 2014 でのバックアップと復元

SQL Server 2014 には、次の拡張機能が用意されています。

1. **Azure へのバックアップと復元**:

 - SQL Server Management Studio で、*SQL Server Backup to URL* がサポートされるようになりました。 SQL Server Management Studio で Backup タスクまたは復元タスクを使用するときや、メンテナンス プラン ウィザードを使用するときに、Azure にバックアップするオプションを利用できるようになりました。 詳細については、次を参照してください。 [SQL Server Backup to URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx)します。
 - *Azure への SQL Server マネージ バックアップ*は、バックアップ管理の自動化を可能にする新機能です。 この機能は、Azure マシンで実行されている SQL Server 2014 インスタンスのバックアップ管理を自動化する際に特に役立ちます。 詳細については、次を参照してください。 [SQL Server Managed Backup to Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx)します。
 - *自動アックアップ*は、Azure の SQL Server VM の既存のデータベースと新しいデータベースのすべてを対象とした *Azure への SQL Server マネージ バックアップ*を自動的に有効にする別の自動化機能です。 詳細については、次を参照してください。 [Azure 仮想マシンにおける SQL Server の自動バックアップ](virtual-machines-sql-server-automated-backup.md)します。
 - Azure への SQL Server 2014 のバックアップのすべてのオプションの概要については、次を参照してください。 [SQL Server のバックアップと復元 Microsoft Azure Blob ストレージ サービスと](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx)します。

1. **暗号化**: SQL Server 2014 では、バックアップの作成時におけるデータの暗号化をサポートしています。 複数の暗号化アルゴリズムと、証明書または非対称キーの使用をサポートしています。 詳細については、次を参照してください。 [バックアップの暗号化](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx)します。

## SQL Server 2012 でのバックアップと復元

SQL Server 2012 における SQL Server のバックアップと復元の詳細については、次を参照してください。 [データベース バックアップと復元の SQL Server (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx)します。

SQL Server 2012 SP1 Cumulative Update 2 以降では、Azure BLOB ストレージ サービスとの間でバックアップと復元を実行できます。 この拡張機能を使用して、Azure 仮想マシンで実行されている SQL Server 上またはオンプレミスのインスタンス上の SQL Server データベースをバックアップできます。 詳細については、次を参照してください。 [SQL Server のバックアップと Azure Blob ストレージ サービスによる復元](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)します。

Azure BLOB ストレージ サービスを使用する利点として、接続されるディスク数の上限 (16 個) を回避できること、管理しやすいこと、移行または障害復旧のために、Azure 仮想マシンで実行されている SQL Server インスタンスの別のインスタンスまたはオンプレミスのインスタンスでバックアップ ファイルを直接利用できることなどがあります。 SQL Server のバックアップに Azure blob ストレージ サービスを使用する利点の一覧については、次を参照してください。、 *利点* セクション [SQL Server のバックアップと Azure Blob ストレージ サービスによる復元](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)します。

ベスト プラクティスの推奨事項とトラブルシューティング情報については、次を参照してください。 [バックアップと復元に関するベスト プラクティス (Azure Blob ストレージ サービス)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx)します。

## Azure 仮想マシンでサポートされる他のバージョンの SQL Server でのバックアップと復元

SQL Server のバックアップと復元が SQL Server 2008 R2 では、次を参照してください。 [バックアップおよび復元する SQL Server データベースの (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx)します。

SQL Server のバックアップと復元が SQL Server 2008 では、次を参照してください。 [バックアップおよび復元する SQL Server データベースの (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx)します。

## 次のステップ

Azure VM で SQL Server の配置を計画しているが場合、は、次のチュートリアルではプロビジョニングのガイダンスについてを見つけることができます: [Azure で SQL Server 仮想マシンをプロビジョニング](virtual-machines-provision-sql-server.md)します。

バックアップと復元を使用してデータを移行できますが、Azure VM の SQL Server へのより簡単なデータ移行パスが存在する可能性があります。 移行オプションと推奨事項の詳細については、次を参照してください。 [Azure VM 上の SQL Server にデータベースを移行する](virtual-machines-migrate-onpremises-database.md)します。

その他のレビュー [Azure Virtual Machines における SQL Server を実行するためのリソース](virtual-machines-sql-server-infrastructure-services.md)します。





