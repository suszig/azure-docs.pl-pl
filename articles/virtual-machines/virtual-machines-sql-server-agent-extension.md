<properties
    pageTitle="SQL Server IaaS Agent 拡張機能 | Microsoft Azure"
    description="このトピックでは、クラシック デプロイ モデルで作成されたリソースを使用し、Azure で SQL Server を実行する VM が自動化機能を使用できるようにする SQL Server Agent 拡張機能について説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="jeffgoll"
    manager="jeffreyg"
   editor="monicar"    
   tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/02/2015"
    ms.author="jeffreyg"/>


# SQL Server IaaS Agent 拡張機能

この拡張機能を使用すると、Azure Virtual Machines の SQL Server で、この記事に掲載されている特定のサービスを使用できるようになります。これらのサービスは、この拡張機能がインストールされている場合にのみ使用できます。 この拡張機能は、Azure ポータルの SQL Server Gallery Images の場合に自動的にインストールされます。 Azure VM Guest Agent がインストールされている Azure の任意の SQL Server VM にインストールできます。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


## 前提条件

Powershell コマンドレットを使用するための要件:

- 最新の Azure コマンドライン SDK [こちらで入手できます](http://azure.microsoft.com/downloads/)

VM で拡張機能を使用するための要件:

- Azure VM Guest Agent
- Windows Server 2012、Windows Server 2012 R2 以降
- SQL Server 2012、SQL Server 2014 以降

## 拡張機能で使用できるサービス

- **SQL の自動化されたバックアップ**: このサービスで、VM 内の SQL Server の既定インスタンスについて、すべてのデータベースのバックアップが自動的にスケジュールされます。 このサービスに関する詳細を表示するには、次を参照してください。 [Azure 仮想マシンにおける SQL Server の自動バックアップ](virtual-machines-sql-server-automated-backup.md)します。
- **SQL 自動修正**: このサービスでは、VM に更新できるを実行する、ワークロードのピーク時の更新プログラムを回避するためのメンテナンス期間を構成することができます。 このサービスに関する詳細を表示するには、次を参照してください。 [Azure 仮想マシンにおける SQL Server の自動修正](virtual-machines-sql-server-automated-patching.md)します。

## Powershell を使用した拡張機能の追加

使用して、SQL Server VM をプロビジョニングする場合、 [Azure ポータル](https://portal.azure.com/), 、拡張機能は自動的にインストールされています。 SQL Server vm をプロビジョニング、 [Azure クラシック ポータル](https://manage.windowsazure.com), 、または、独自の SQL ライセンスを持っている vm の場合は、次の Azure PowerShell コマンドレットを使用して既存の VM にこの拡張機能を追加できます。

**Set-AzureVMSqlServerExtension**

### 構文

Set-azurevmsqlserverextension [-VM] <IPersistentVM> [-バージョン] <string>] [AutoBackupSettings < AutoBackupSettings > ][-autopatchingsetttings <autopatchingsetttings>] [の確認 ][-whatif] [<CommonParameters>]
> [AZURE.NOTE] –Version パラメーターは、省略することをお勧めします。 省略すると、最新バージョンの拡張機能が既定値になります。

### 例

    Get-AzureVM –ServiceName serviceName –Name vmName | Set-AzureVMSqlServerExtension –AutoBackupSettings $abs | Update-AzureVM**

## 拡張機能の状態を確認します。

この拡張機能とそれに関連付けられているサービスの状態を確認する場合は、いずれかのポータルを使用できます。 既存の VM の詳細で、**[設定]** の**[拡張機能]** を確認します。

また、次の Azure PowerShell コマンドレットを使用することもできます。

**Get-AzureVMSqlServerExtension**

### 構文

Get AzureVMSqlServerExtension [-VM] <IPersistentVM>] [-バージョン] <string>] [<CommonParameters>]
> [AZURE.NOTE] –Version パラメーターは省略できます。 省略すると、最新バージョンの拡張機能が既定値になります。

### 例

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## Powershell を使用した拡張機能の削除

VM からこの拡張機能を削除する場合は、次の Azure Powershell コマンドレットを使用します。

**Remove-AzureVMSqlServerExtension**

### 構文

Remove-azurevmsqlserverextension VM <IPersistentVM> [<CommonParameters>]





