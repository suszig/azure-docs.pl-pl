<properties
 pageTitle="仮想マシンの拡張機能とその機能 | Microsoft Azure"
 description="Azure 仮想マシンに使用できる拡張機能について、提供または改善される内容ごとにまとめて説明します。"
 services="virtual-machines"
 documentationCenter=""
 authors="squillace"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="infrastructure-services"
 ms.date="12/08/2015"
 ms.author="rasquill"/>
#仮想マシンの拡張機能とその機能について

Microsoft Azure では、Microsoft 製およびサード パーティ プロバイダー製の両方の VM 拡張機能を提供しています。これらの機能によって、Azure Virtual Machines の生産性を向上させるのに役立つセキュリティ、ランタイム、デバッグ、管理などの機能が有効化されます。 このトピックでは、使用に合わせて Windows 仮想マシンおよび Linux Virtual Machines の両方に対して Azure VM 拡張機能により提供される各種機能について説明し、それぞれについてのドキュメントを示します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



詳細については、VM エージェントと VM 拡張機能をサポートするためにその動作は、次を参照してください。 [VM エージェントと VM 拡張機能の概要](virtual-machines-extensions-install.md)します。

##Azure VM 拡張機能

VM 拡張機能では、パスワードのリセットや RDP の構成などの基本的な機能をはじめとして、VM で使用する重要な機能のほとんどを実装します。 新しい拡張機能は永続的に追加されるため、Azure 内で VM がサポートする機能の数は増加し続けます。 既定では、いくつかの基本的な VM 拡張機能がインストールされているイメージ ギャラリーから VM を作成するときなど **IaaSDiagnostics** (現在 Windows Vm のみ)、 **VMAccess**, 、および **BGInfo** (現在のところ Windows のみ)。 ただし、機能の更新と新しい拡張機能の流れは一定であるため、どの期間でも Windows と Linux の両方にすべての拡張機能が実装されるわけではありません。

##接続と基本的な管理

次の拡張機能は、VM を作成および実行した後にその基本的な接続を有効化、再有効化、または無効化するのに欠かせません。

|VM 拡張機能の名前|機能の説明|詳細情報
|---|---|---|
|VMAccessAgent (Windows)|ユーザー情報と RDP および SSH の接続構成を作成、更新、リセットします。|[Windows](virtual-machines-extensions-customscript.md)
|VMAccessForLinux (Linux)|ユーザー情報と RDP および SSH の接続構成を作成、更新、リセットします。|[Linux](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess)

##デプロイと構成の管理

次の拡張機能は、デプロイと構成の管理に関するさまざまな種類のシナリオと機能をサポートします。 以下の VM の操作と管理に関するセクションも参照してください。

|VM 拡張機能の名前|機能の説明|詳細情報|
|---|---|---|
|**MSEnterpriseApplication**|Windows System Center によるサポート向けの機能を実装します。|[System Center 2012 R2 Virtual Machine Roles (System Center 2012 R2 の仮想マシン ロール)](http://social.technet.microsoft.com/wiki/contents/articles/18274.system-center-2012-r2-virtual-machine-role-authoring-guide-resource-extension-package.aspx)|
|**Octopus 展開** (DSC 拡張機能に基づく)|開発環境、テスト環境、運用環境への ASP.NET Web アプリケーションおよび Windows サービスの自動デプロイをサポートします。|[Getting Started with Octopus Deploy (Octopus Deploy の概要)](http://docs.octopusdeploy.com/display/OD/Getting%20started)|
|**Visual Studio リリース マネージャー** (DSC 拡張機能に基づく)|Visual Studio での継続的なデプロイをサポートします。|[リリース管理による配置の自動化](https://msdn.microsoft.com/Library/vs/alm/Release/overview)|
|**CentosChefClient**|||
|**ChefClient**|Windows 上で Chef クライアントを作成します (以下の DSC 拡張機能も使用できます)。|[Chef と Microsoft Azure に関するページ](https://www.getchef.com/solutions/azure/)|
|**LinuxChefClient**|||
|**DockerExtension**|リモートの Docker コマンドをサポートする Docker デーモンをインストールします。|[Docker 仮想マシン拡張機能を使用する方法](virtual-machines-docker-vm-extension.md)より広範な情報を参照してください、 [Docker VM 拡張機能のユーザー ガイド](https://github.com/Azure/azure-docker-extension/blob/master/README.md)|
|**DSC**|PowerShell DSC (必要な状態の構成) 拡張機能。|[Azure PowerShell DSC (Desired State Configuration) extension (Azure PowerShell DSC (必要な状態の構成) 拡張機能)](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx)|
|**PuppetEnterpriseAgent**|Puppet Enterprise の機能を実装します。 |[Puppet on Azure (Azure で Puppet を使用する)](http://puppetlabs.com/solutions/microsoft)|
|**CustomScriptExtension** (Windows)**CustomScriptForLinux** (Linux)|任意のタイミング (スタートアップ時または有効期間中) に VM でカスタム スクリプトを呼び出します。|[カスタム スクリプト拡張機能](virtual-machines-extensions-customscript.md) | [Linux](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript)|
|**AzureCATExtensionHandler**|によって収集された診断データを消費 **IaaSDiagnostics** など、他のいくつかのデータ ソースと [Azure Storage Analytics Metrics](https://msdn.microsoft.com/library/azure/hh343270.aspx) SAP ホスト コントロール プロセスで消費されるに適切な集計のデータ セットに変換|[Azure Enhanced Monitoring for SAP (Azure での SAP の監視の強化)](http://azure.microsoft.com/blog/2014/06/04/azure-enhanced-monitoring-for-sap/)|

##セキュリティと保護

このセクションの拡張機能は、Azure VM に欠かせないセキュリティ機能を提供します。

|VM 拡張機能の名前|機能の説明|詳細情報|
|---|---|---|
|**CloudLinkSecureVMWindowsAgent**|Microsoft Azure のお客様に、マルチテナント共有インフラストラクチャで仮想マシンのデータを暗号化する機能と、Azure ストレージ インフラストラクチャ上の暗号化されたデータの暗号化キーを完全に制御する機能を提供します。|[Securing Microsoft Azure Virtual Machines leveraging BitLocker and Native OS encryption (BitLocker とネイティブ OS 暗号化を使用した Microsoft Azure Virtual Machines の保護)](http://www.cloudlinktech.com/azure)|
|**McAfeeEndpointSecurity**|悪意のあるソフトウェアから VM を保護します。|[McAfee](https://www.mcafeeasap.com/MarketingContent/default.aspx)|
|**TrendMicroDSA**|侵入の検知と防止、ファイアウォール、マルウェア対策、Web レピュテーション、ログの検査、および整合性監視を提供する Trend Micro Deep Security プラットフォームを有効にします。|[How to install and configure Trend Micro Deep Security as a Service on an Azure VM (Azure VM に Trend Micro Deep Security をサービスとしてインストールし、構成する方法)](virtual-machines-install-trend.md)|
|**PortalProtectExtension**|Microsoft SharePoint 環境を脅威から保護します。|[Securing Your SharePoint Deployment on Azure (Azure 上への SharePoint のデプロイの保護)](http://blog.trendmicro.com/securing-sharepoint-deployment-azure/)|
|**IaaSAntimalware**|Azure Cloud Services および Virtual Machines 向けの Microsoft マルウェア対策は、ウイルスやスパイウェアなどの悪意のあるソフトウェアを識別して削除するリアルタイム保護機能であり、既知のマルウェアや不要なソフトウェアがシステムへのインストールまたは実行を試みた場合に警告する構成可能なアラートを備えています。|[マルウェア対策についてのドキュメントをダウンロードする](http://go.microsoft.com/fwlink/?linkid=398023&clcid=0x409)|
|**SymantecEndpointProtection**|Symantec Endpoint Protection 12.1.4 により、物理システムと仮想システム間でのセキュリティとパフォーマンスを有効にします。|[How to install and configure Symantec Endpoint Protection on an Azure VM (Azure VM に Symantec Endpoint Protection をインストールし、構成する方法)](virtual-machines-install-symantec.md)

##VM の操作と管理

一般的な操作管理機能と動作をサポートします。 上記のデプロイと構成の管理に関するセクションも参照してください。

|**VM 拡張機能の名前**|機能の説明|詳細情報|
|---|---|---|
|**AzureVmLogCollector**|使用することができます、 **AzureVMLogCollector** 拡張機能からオンデマンドでのログの 1 回限りの収集を実行する 1 つまたは複数 (web ロールとワーカー ロールの両方) からクラウド サービスの Vm と転送、収集されたファイルを Azure のストレージ アカウントに、Vm のいずれかに、リモートでログオンします。 |[AzureLogCollector 拡張機能](virtual-machines-extensions-log-collector.md)|
|**IaaSDiagnostics**|有効、無効化、および Azure 診断を構成してによっても使用されます、 **AzureCATExtensionHandler** SAP 監視をサポートします。|[Microsoft Azure Virtual Machine Monitoring with Azure Diagnostics Extension (Azure 診断の拡張機能を使用した Microsoft Azure の仮想マシンの監視)](http://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/)|
|**OSPatchingForLinux**|Azure VM 管理者を有効にして、カスタマイズされた構成での VM OS の更新を自動化します。 OSPatching 拡張機能を使用すると、OS パッチをインストールする頻度と時期の指定、インストールするパッチの指定、および更新後の再起動動作の構成など、仮想マシンの OS の更新を構成できます。|[OS のブログ投稿の拡張機能の修正プログラム](http://azure.microsoft.com/blog/2014/10/23/automate-linux-vm-os-updates-using-ospatching-extension/)します。 関連項目、readme ファイルとソースに Github の [OS 修正プログラムの適用拡張](https://github.com/Azure/azure-linux-extensions)します。|

##開発およびデバッグ

これらの VM 拡張機能は Visual Studio 関連の機能に対するサポートを提供するものです。ここでは機能一覧を網羅するために掲載されており、直接使用することは意図されていません。

|VM 拡張機能の名前|機能の説明|詳細情報|
|---|---|---|
|**VS14CTPDebugger**|Azure SDK 2.4 を使用した VS からのリモート デバッグをサポートします。|[リモート デバッグと診断](https://msdn.microsoft.com/library/y7f5zaaa.aspx)|
|**VS2013Debugger**|Azure SDK 2.4 を使用した VS からのリモート デバッグをサポートします。||
|**VS2012Debugger**|Azure SDK 2.4 を使用した VS からのリモート デバッグをサポートします。||
|**RemoteDebugVS14CTP**|Azure SDK 2.3 を使用した VS からのリモート デバッグをサポートします。||
|**RemoteDebugVS2013**|Azure SDK 2.3 を使用した VS からのリモート デバッグをサポートします。||
|**RemoteDebugVS2012**|Azure SDK 2.3 を使用した VS からのリモート デバッグをサポートします。||
|**WebDeployForVSDevTest**|Windows Server に IIS と Web Deploy をインストールおよび構成します。 この拡張機能の削除または無効化はサポートされていません。|

##その他の機能

これらの拡張機能では、役立つ可能性があるその他の VM 機能に対するサポートを提供します。

|VM 拡張機能の名前|機能の説明|詳細情報|
|---|---|---|
|**BGInfo**|RDP の使用時に、有用なサーバー情報を統合した図をデスクトップに表示します。|[BGInfo 拡張機能](https://msdn.microsoft.com/library/dn606289.aspx)|
|**HpcVmDrivers**|Windows Server 2012 R2 または Windows Server 2012 を実行しているサイズが A8 または A9 の VM で、リモート ダイレクト メモリ アクセス (RDMA) ネットワーク デバイスのドライバーのインストール、構成、管理を行います。 クラスター化された A8 または A9 の VM で、並列 MPI アプリケーション の実行中に RDMA ネットワークを使用できるようになります。|[A8、A9、A10、A11 コンピューティング集中型インスタンスについて](virtual-machines-a8-a9-a10-a11-specs.md)

