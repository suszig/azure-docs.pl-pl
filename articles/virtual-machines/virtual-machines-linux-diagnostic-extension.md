
<properties
        pageTitle="VM 拡張機能による Linux VM の監視 |Microsoft Azure"
        description="Linux の診断拡張機能を使用して Azure 内の Linux VM のパフォーマンスと診断データを監視する方法を説明します。"
        services="virtual-machines"
        documentationCenter=""
        authors="NingKuang"
        manager="timlt"
        editor=""
        tags="azure-service-management"/>

<tags
        ms.service="virtual-machines"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="07/20/2015"
        ms.author="Ning"/>


# Linux の診断拡張機能を使用した Linux VM のパフォーマンスと診断データの監視

## はじめに

Linux 診断拡張機能は、Microsoft Azure で実行されている Linux VM を監視するのに役立ちます。次の機能があります。

- Linux VM のシステム パフォーマンス、診断、syslog データを収集して、ユーザーのストレージ テーブルにアップロードします。
- ユーザーは収集およびアップロードされるデータ メトリックをカスタマイズできます。
- ユーザーは指定したログ ファイルを指定したストレージ テーブルにアップロードできます。

バージョン 2.0 では、以下のデータが含まれます。

- システム、セキュリティ、アプリケーション ログを含む、すべての Linux Rsyslog ログ。
- 指定されたすべてのシステム データ [ドキュメント](https://scx.codeplex.com/wikipage?title=xplatproviders")します。
- ユーザーが指定したログ ファイル。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


## 拡張機能を有効にする方法
有効にする、拡張機能、 [Azure ポータル](https://ms.portal.azure.com/#), 、Azure PowerShell または Azure CLI スクリプトです。

システムとパフォーマンスを表示および構成データ直接 Azure のポータルでは、以下の [手順](http://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/ "Windows ブログへの URL")します。


この記事では、Azure CLI コマンドを使用して拡張機能を有効化および構成する方法を説明します。直接ストレージ テーブルからデータを読み取って表示できます。


## 前提条件
- Microsoft Azure Linux エージェント バージョン 2.0.6 またはそれ以降。
大部分の Azure VM Linux ギャラリー イメージにはバージョン 2.0.6 以降が含まれています。 実行することができます **WAAgent のバージョン** VM にインストールされているバージョンを確認します。 VM が 2.0.6 より前のバージョンを実行するが場合は、これらを次 [指示](https://github.com/Azure/WALinuxAgent "instructions") に更新します。
- [Azure CLI](./xplat-cli-install.md)します。 次の [このガイダンス](./xplat-cli-install.md) 、コンピューターに Azure CLI 環境を設定します。 Azure CLI をインストールすると後を使用して、 **azure** コマンドをコマンド ライン インターフェイス (Bash、Terminal、command プロンプト)、Azure CLI コマンドにアクセスします。 たとえば、実行 **azure vm 拡張機能の設定 - ヘルプ** 詳しい使用方法については、実行 **azure ログイン** Azure にログインするには実行 **azure vm の一覧** Azure 上にあるすべての仮想マシンの一覧にします。
- データを格納するためのストレージ アカウント。 データをストレージにアップロードするには、事前にストレージ アカウント名とアクセス キーを作成しておく必要があります。


## Azure CLI コマンドを使用して Linux 診断拡張機能を有効にする

###  シナリオ 1.  既定のデータ セットで拡張機能を有効にする
バージョン 2.0 以降では、以下のデータが既定で収集されます。

- すべての Rsyslog 情報 (システム ログ、セキュリティ ログ、アプリケーション ログ)。  
- 基本システム データのコア セット、データ セット全体については、この [ドキュメント](https://scx.codeplex.com/wikipage?title=xplatproviders)します。
追加のデータを有効にする場合は、シナリオ 2 および 3 の手順を続行してください。

手順 1. 次の内容を含む PrivateConfig.json という名前のファイルを作成します。

    {
        "storageAccountName":"the storage account to receive data",
        "storageAccountKey":"the key of the account"
    }

手順 2. 実行 **azure vm 拡張機能は、vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2 を設定します。* ----private-config-path PrivateConfig.json** します。


###   シナリオ 2.  パフォーマンス モニターのメトリックをカスタマイズする  
このセクションでは、パフォーマンスと診断データのテーブルをカスタマイズする方法について説明します。

手順 1. 次の例で示されている内容を含む、PrivateConfig.json という名前のファイルを作成します。 収集する特定のデータを指定します。

サポートされているすべてのプロバイダーと変数は、この接続文字列 [ドキュメント](https://scx.codeplex.com/wikipage?title=xplatproviders)します。 スクリプトにさらにクエリを追加することにより、複数のクエリを使用して複数のテーブルにそれを格納することができます。

Rsyslog データは既定で常に収集されます。

    {
        "storageAccountName":"storage account to receive data",
        "storageAccountKey":"key of the account",
        "perfCfg":[
            {"query":"SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation","table":"LinuxMemory"
            }
          ]
    }


手順 2. 実行 **azure vm 拡張機能は、vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2 を設定します。*
---private-config-path PrivateConfig.json** します。


###   シナリオ 3.  独自のログ ファイルをアップロードする
このセクションでは、ストレージ アカウントに特定のログ ファイルを収集およびアップロードする方法について説明します。
ログ ファイルのパスを指定し、ログを格納するテーブル名を指定する必要があります。 複数の file/table エントリをスクリプトに追加することで、複数のログ ファイルを使用できます。

手順 1. 次の内容を含む PrivateConfig.json という名前のファイルを作成します。

    {
        "storageAccountName":"the storage account to receive data",
        "storageAccountKey":"key of the account",
        "fileCfg":[
            {"file":"/var/log/mysql.err",
             "table":"mysqlerr"
            }
          ]
    }


手順 2. 実行 **azure vm 拡張機能は、vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2 を設定します。*
---private-config-path PrivateConfig.json** します。


###   シナリオ 4.  Linux の監視拡張機能を無効にする
手順 1. 次の内容を含む PrivateConfig.json という名前のファイルを作成します。

    {
        "storageAccountName":"the storage account to receive data",
        "storageAccountKey":"the key of the account",
        “perfCfg”:[],
        “enableSyslog”:”False”
    }


手順 2. 実行 **azure vm 拡張機能は、vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2 を設定します。*
---private-config-path PrivateConfig.json** します。


## データを確認する
パフォーマンスと診断のデータが Azure Storage テーブルに保存されます。 レビュー [ここ](storage-ruby-how-to-use-table-storage.md) に Azure CLI スクリプトを使用してストレージ テーブルのデータにアクセスする方法について説明します。

さらに、次の UI ツールを使用してデータにアクセスすることもできます。

1.  Visual Studio のサーバー エクスプローラーを使用します。 ストレージ アカウントに移動します。 VM を約 5 分間稼働した後、既定のテーブル「LinuxCpu」、「LinuxDisk」、「LinuxMemory」、および「Linuxsyslog」を確認する必要があります。 データを表示するには、テーブル名をダブルクリックします。
2.  使用 [Azure ストレージ エクスプ ローラー](https://azurestorageexplorer.codeplex.com/ "Azure ストレージ エクスプ ローラー") データにアクセスします。

![image](./media/virtual-machines-linux-diagnostic-extension/no1.png)

シナリオ 2 および 3 で指定した fileCfg または perfCfg を有効にした場合、前述のツールを使用して既定以外のデータを表示することもできます。



## 既知の問題
- バージョン 2.0 の場合、Rsyslog 情報およびユーザー指定のログ ファイルには、スクリプトからのみアクセスできます。
- バージョン 2.0 の場合、最初にスクリプトを使用して Linux 診断拡張機能を有効にすると、ポータルからはデータを表示できません。 最初にポータルから拡張機能を有効にした場合は、スクリプトも正常に機能します。

