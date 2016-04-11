<properties
    pageTitle="VM の SQL Server へのデータベースの移行 | Mirosoft Azure"
    description="オンプレミスのユーザー データベースを Azure 仮想マシンの SQL Server に移行する方法について説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="carlrabeler"
    manager="jeffreyg"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/02/2015"
    ms.author="carlrab"/>


# Azure VM の SQL Server へのデータベースの移行

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


オンプレミスの SQL Server ユーザー データベースを Azure VM の SQL Server に移行する方法は多数あります。 この記事はについて簡単にさまざまな方法を説明する、さまざまなシナリオに最適な方法をお勧めし、含める、 [チュートリアル](#azure-vm-deployment-wizard-tutorial) を使用するときに、 **Microsoft Azure の仮想マシンに SQL Server データベースの配置** ウィザード。

## 主な移行方法

主な移行方法は次のとおりです。

- Microsoft Azure VM への SQL Server Database のデプロイ ウィザードを使用する
- 圧縮機能を使用してオンプレミスのバックアップの実行し、そのバックアップ ファイルを Azure 仮想マシンに手動でコピーする
- URL へのバックアップを実行し、その URL から Azure 仮想マシンに復元する
- データとログ ファイルをデタッチしてから、Azure BLOB ストレージにコピーし、その後、URL から Azure VM の SQL Server にアタッチする
- オンプレミスの物理マシンを HYPER-V VHD に変換して Azure BLOB ストレージにアップロードし、アップロードしたその VHD を使用して、新しい VM としてデプロイする
- Windows の Import/Export サービスを使用して、ハード ドライブを発送する

> [AZURE.NOTE] AlwaysOn の展開がある場合、オンプレミスで検討することもを使用して、 [Azure のレプリカの追加ウィザード](virtual-machines-sql-server-extend-on-premises-alwayson-availability-groups.md) Azure とし、フェールオーバーに移行するための方法としてレプリカを作成します。

## 移行方法の選択

最適なデータ転送のパフォーマンス、データベース ファイルの圧縮されたバックアップ ファイルを使用して、Azure VM への移行は一般に値が最適な方法です。 これは、メソッドを [Microsoft Azure VM のウィザードに SQL Server データベースの配置](#azure-vm-deployment-wizard-tutorial) を使用します。 このウィザードは、圧縮されたデータベースのバックアップ ファイルが 1 TB より小さいとき、SQL Server 2005 以降で実行されているオンプレミスのユーザー データベースを、SQL Server 2014 以降に移行する際に使用することをお勧めします。

データベースのバックアップ ファイルが大きすぎるか、移行先の SQL Server インスタンスが SQL Server 2014 以降ではないことが原因でウィザードを使用できない場合、移行プロセスは手動で行います。このプロセスでは、通常、データベースを最初にバックアップしてから、そのデータベース バックアップを Azure にコピーし、最後にデータベースを復元します。 データベース ファイル自体を Azure にコピーすることもできます。 この手動による Azure VM へのデータベースの移行プロセスは、複数の方法で実行できます。

> [AZURE.NOTE] 古いバージョンの SQL Server から SQL Server 2014 または SQL Server 2016 にアップグレードするときに、変更が必要かどうかを検討してください。 移行プロジェクトの一環として、新しいバージョンの SQL Server でサポートされない機能への依存関係すべてを解決することをお勧めします。 サポートされるエディションとシナリオの詳細については、次を参照してください。 [SQL Server にアップグレード](https://msdn.microsoft.com/library/bb677622.aspx)します。

次の表に、主な移行方法の一覧を示します。ここでは、それぞれの方法を使用するタイミングとして最適な状況についても説明します。

| メソッド  | 移行元データベースのバージョン  |  移行先データベースのバージョン | 移行元データベースのバックアップ サイズ制限  | メモ  |
|---|---|---|---|---|
| [Microsoft Azure VM への SQL Server Database のデプロイ ウィザードを使用する](#azure-vm-deployment-wizard-tutorial) | SQL Server 2005 以降 | SQL Server 2014 以降 | > 1 TB  | 最も高速で簡単な方法。Azure 仮想マシンの新規または既存の SQL Server インスタンスに移行するときにいつでも使用できます。 |
| [圧縮機能を使用してオンプレミスのバックアップの実行し、そのバックアップ ファイルを Azure 仮想マシンに手動でコピーする](#backup-to-file-and-copy-to-vm-and-restore) | SQL Server 2005 以降 | SQL Server 2005 以降 | [Azure VM ストレージの制限](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | ウィザードを使用できない場合にのみ使用。たとえば、移行先データベースのバージョンが SQL Server 2012 SP1 CU2 より以前の場合や、データベースのバックアップ サイズが 1 TB (SQL Server 2016 の場合は 12.8 TB) を超えている場合は、ウィザードを使用できません。 |
| [URL へのバックアップを実行し、その URL から Azure 仮想マシンに復元する](#backup-to-url-and-restore) | SQL Server 2012 SP1 CU2 以上 | SQL Server 2012 SP1 CU2 以上 | > 1 TB (SQL Server 2016 の場合は < 12.8 TB) | 一般的に、 [URL へのバックアップ](https://msdn.microsoft.com/library/dn435916.aspx) は等価では、パフォーマンス ウィザードを使用して非常に容易ではありません |
| [データとログ ファイルをデタッチしてから、Azure BLOB ストレージにコピーし、その後、URL から Azure 仮想マシンの SQL Server にアタッチする](#detach-and-copy-to-url-and-attach-from-url) | SQL Server 2005 以降 | SQL Server 2014 以降 | [Azure VM ストレージの制限](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | しようとした場合は、このメソッドを使用して [Azure Blob ストレージ サービスを使用してこれらのファイルを保存](https://msdn.microsoft.com/library/dn385720.aspx) し、特に大規模なデータベースでの Azure VM で実行されている SQL Server に接続 |
| [オンプレミスのマシンを HYPER-V VHD に変換して Azure BLOB ストレージにアップロードし、アップロードしたその VHD を使用して、新しい仮想マシンをデプロイする](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) | SQL Server 2005 以降 | SQL Server 2005 以降 | [Azure VM ストレージの制限](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | 使用する場合 [ご自身の SQL Server ライセンス](../data-management-azure-sql-database-and-sql-server-iaas/), の他のユーザー データベースやシステム データベースに応じて、データベースの移行の一環として連携してシステムとユーザー データベースを移行するときに、または SQL Server の古いバージョンで実行するデータベースを移行するときにします。 |
| [Windows の Import/Export サービスを使用して、ハード ドライブを発送する](#ship-hard-drive) | SQL Server 2005 以降 | SQL Server 2005 以降 | [Azure VM ストレージの制限](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | 使用して、 [Windows インポート/エクスポート サービス](../storage-import-export-service/) ときに手動でのコピーが遅すぎるなど、非常に大規模なデータベース |

## Azure VM のデプロイ ウィザードのチュートリアル

使用して、 **Microsoft Azure の仮想マシンに SQL Server データベースの配置** SQL Server 2014 または Azure の仮想マシンで SQL Server 2016 するデータベース (最大 1 TB) の SQL Server 2005、SQL Server 2008、SQL Server 2008 R2、SQL Server 2012、SQL Server 2014 または SQL Server 2016 のオンプレミス ユーザーを移行する Microsoft SQL Server Management Studio のウィザードです。 このウィザードを使用すると、ユーザー データベースが、既存の Azure 仮想マシン、または移行プロセス中にウィザードによって作成された SQL Server が含まれる Azure VM のいずれかに移行されます。 データベースを新しいバージョンの SQL Server に移行すると、そのデータベースは、プロセス中に自動的にアップグレードされます。

### 最新の Microsoft Azure VM への SQL Server データベースのデプロイ ウィザードを入手する

最新バージョンがあることを確認する SQL Server 用 Microsoft SQL Server Management Studio の最新バージョンを使用して、 **Microsoft Azure の仮想マシンに SQL Server データベースの配置** ウィザード。 このウィザードの最新バージョンには、Azure クラシック ポータルの最新の更新プログラムが組み込まれており、ギャラリーの最新の Azure VM イメージがサポートされます (以前のバージョンのウィザードは機能しないことがあります)。 SQL Server 用 Microsoft SQL Server Management Studio の最新バージョンを入手する [ダウンロード](http://go.microsoft.com/fwlink/?LinkId=616025) を移行すると、インターネットにデータベースへの接続にクライアント コンピューターにインストールします。

### 既存の Azure 仮想マシンと SQL Server インスタンスを構成する (該当する場合)

既存の Azure VM に移行するには、次の構成手順を実行します。

- 手順に従って、接続する] で SQL Server VM インスタンスに SSMS から別のコンピューター」の「別コンピューターからの接続を有効にするには、Azure VM と SQL Server インスタンスを構成する [Azure で SQL Server 仮想マシンをプロビジョニング](../virtual-machines-provision-sql-server/#SSMS)します。 ウィザードを使用して移行する場合、サポートされるのは、ギャラリーの SQL Server 2014 および SQL Server 2016 のイメージのみです。
- Microsoft Azure ゲートウェイの SQL Server クラウド アダプター サービスのオープン エンドポイントを、プライベート ポート 11435 で構成します。 このポートは、Microsoft Azure VM における SQL Server 2014 または SQL Server 2016 のプロビジョニングの一環として作成されます。 また、クラウド アダプターでは、既定のポート 11435 での着信 TCP 接続を許可する Windows ファイアウォール ルールも作成されます。 このエンドポイントにより、ウィザードがクラウド アダプター サービスを利用して、バックアップ ファイルをオンプレミスのインスタンスから Azure VMにコピーできます。 詳細については、次を参照してください。 [for SQL Server クラウド アダプター](https://msdn.microsoft.com/library/dn169301.aspx)します。

    ![クラウド アダプターのエンドポイントの作成](./media/virtual-machines-migrate-onpremises-database/cloud-adapter-endpoint.png)

### Microsoft Azure VM への SQL Server データベースのデプロイ ウィザードを実行する

1. Microsoft SQL Server Management Studio for Microsoft SQL Server 2016 を開き、Azure VMに移行するユーザー データベースを含む SQL Server のインスタンスに接続します。
2. 移行するデータベースを右クリックし、[タスク] をポイントして、[Microsoft Azure VM にデプロイする] をクリックします。

    ![ウィザードの開始](./media/virtual-machines-migrate-onpremises-database/start-wizard.png)

3. [説明] ページで [次へ] をクリックします。
4. [ソース設定] ページで、Azure VM に移行するデータベースを含む SQL Server インスタンスに接続します。
5. バックアップ ファイルの一時的な場所を指定します。 リモート サーバーに接続する場合は、ネットワーク ドライブを指定する必要があります。

    ![移行元の設定](./media/virtual-machines-migrate-onpremises-database/source-settings.png)

6. [次へ] をクリックします。
7. [Microsoft Azure ログイン] ページで、[ログイン] をクリックし、Azure アカウントにログインします。
8. 使用するサブスクリプションを選択し、[次へ] をクリックします。

    ![Azure のサインイン](./media/virtual-machines-migrate-onpremises-database/azure-signin.png)

9. [デプロイの設定] ページでは、新規または既存のクラウド サービス名と仮想マシン名を指定できます。

 - 新しいクラウド サービス名と仮想マシン名を指定し、SQL Server 2014 ギャラリーまたは SQL Server 2016 ギャラリーのイメージを使用して、新しい Azure 仮想マシンで新しいクラウド サービスを作成します。

     - 新しいクラウド サービス名を指定する場合は、使用するストレージ アカウントを指定します。

     - 既存のクラウド サービス名を指定する場合は、ストレージ アカウントが取得され、入力されます。

 - 既存のクラウド サービス名と新しい仮想マシン名を指定して、既存のクラウド サービスで新しい Azure 仮想マシンを作成します。 SQL Server 2014 ギャラリーまたは SQL Server 2016 ギャラリーのイメージのみを指定します。
 - 既存のクラウド サービス名と仮想マシン名を指定して、既存の Azure 仮想マシンを使用します。 これには、SQL Server 2014 ギャラリーまたは SQL Server 2016 ギャラリーのイメージを使用して作成されたイメージが必要です。

        ![Deploymnent Settings](./media/virtual-machines-migrate-onpremises-database/deployment-settings.png)

10. [設定] のクリック
  - 既存のクラウド サービス名と仮想マシン名を指定した場合は、ユーザー名とパスワードを入力するように求められます。

        ![Azure machine settings](./media/virtual-machines-migrate-onpremises-database/azure-machine-settings.png)

    - 新しい仮想マシン名を指定した場合は、ギャラリーのイメージの一覧からイメージを選択し、次の情報を入力するように求められます。
      - イメージ - SQL Server 2014 または SQL Server 2016 のみを選択
        - ユーザー名
        - 新しいパスワード
        - パスワードの確認
        - 場所
        - サイズ
    - また、[この新しい Microsoft Azure 仮想マシンに対して自動生成された証明書を使用する] チェック ボックスをオンにして、[OK] をクリックします。

    ![Azure の新しいコンピューターの設定](./media/virtual-machines-migrate-onpremises-database/azure-new-machine-settings.png)

11. 移行先データベース名を指定します (移行元データベース名と異なる場合)。 移行先データベースが既に存在する場合、既存のデータベースは上書きされず、データベース名が自動的にインクリメントされます。
12. [次へ] をクリックし、[完了] をクリックします。

    ![結果](./media/virtual-machines-migrate-onpremises-database/results.png)

13. ウィザードが完了したら、仮想マシンに接続し、データベースが移行されていることを確認します。
14. 新しいバーチャル マシンを作成した場合は、手順に従って、接続に SQL Server 仮想マシン インスタンスに SSMS からの別のコンピューター セクションに、Azure の仮想マシンと SQL Server インスタンスを構成 [Azure で SQL Server 仮想マシンをプロビジョニング](../virtual-machines-provision-sql-server/#SSMS)します。

## ファイルへのバックアップと VM へのコピーおよび復元

SQL Server 2014 より前の SQL Server に移行する場合、またはバックアップ ファイルが 1 TB を超える場合、Microsoft Azure VM への SQL Server データベースのデプロイ ウィザードは使用できません。この方法は、このような場合に使用します。 VM ディスクの最大サイズは 1 TB であるため、バックアップ ファイルが 1 TB を超える場合、そのファイルはストライプする必要があります。 この手動による方法を使用してユーザー データベースを移行するには、次の一般的な手順を実行します。

1.  オンプレミスの場所へのデータベースの完全バックアップを実行します。
2.  必要な SQL Server バージョンを使用して、仮想マシンを作成またはアップロードします。
3.  」の手順を使用して仮想マシンをプロビジョニング [Azure で SQL Server 仮想マシンをプロビジョニング](../virtual-machines-provision-sql-server/#SSMS)します。
4.  リモート デスクトップ、Windows エクスプローラー、またはコマンド プロンプトのコピー コマンドを使用して、バックアップ ファイルを VM にコピーします。

## URL へのバックアップと復元

使用して、 [URL へのバックアップ](https://msdn.microsoft.com/library/dn435916.aspx) メソッドと、バックアップ ファイルが 1 TB を超えるからと SQL Server 2016 に移行するためには、ウィザードの Microsoft Azure VM の SQL Server データベースの配置を使用することはできません。 データベースが 1 TB 未満の場合、または SQL Server 2016 より前のバージョンの SQL Server を実行している場合は、ウィザードを使用することをお勧めします。 SQL Server 2016 では、ストライプ バックアップ セットがサポートされています。このバックアップ セットは、パフォーマンスを確保するために使用することが推奨されます。また、BLOB ごとのサイズ制限を超える場合は必須です。 非常に大規模なデータベースでの使用、 [Windows インポート/エクスポート サービス](../storage-import-export-service/) をお勧めします。

## デタッチして URL をコピーし、URL からアタッチ

しようとした場合は、このメソッドを使用して [Azure Blob ストレージ サービスを使用してこれらのファイルを保存](https://msdn.microsoft.com/library/dn385720.aspx) し、特に大規模なデータベースでの Azure VM で実行されている SQL Server に接続します。 この手動による方法を使用してユーザー データベースを移行するには、次の一般的な手順を実行します。

1.  オンプレミスのデータベース インスタンスからデータベース ファイルをデタッチします。
2.  デタッチされたデータベース ファイルを使用して Azure blob ストレージにコピー、 [AZCopy コマンド ライン ユーティリティ](../storage-use-azcopy/)します。
3.  データベース ファイルを、Azure URL から Azure VM の SQL Server のインスタンスにアタッチします。

## VM に変換して URL にアップロードし、新しい VM としてデプロイ

この方法は、オンプレミスの SQL Server インスタンスのすべてのシステムとユーザー データベースを、Azure 仮想マシンに移行するときに使用します。 この手動による方法を使用して SQL Server インスタンス全体を移行するには、次の一般的な手順を実行します。

1.  使用して、物理または仮想マシンを HYPER-V Vhd に変換 [Microsoft Virtual Machine Converter](http://technet.microsoft.com/library/dn873998.aspx)します。
2.  使用して、VHD ファイルを Azure ストレージにアップロード、 [Add-azurevhd コマンドレット](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx)します。
3.  アップロードした VHD を使用して、新しい仮想マシンをデプロイします。

> [AZURE.NOTE] アプリケーション全体を移行するには、使用を検討して [Azure Site Recovery](../services/site-recovery/)] です。

## ハード ドライブの発送

使用して、 [Windows の Import/export サービス方法](../storage-import-export-service/) 大量のファイル データをネットワーク経由のアップロードが非常に高価なまたは適切でない状況での Azure Blob ストレージに転送します。 このサービスでは、そのデータを含む 1 台以上のハード ドライブを Azure データ センターに発送します。このデータ センターでデータがストレージ アカウントにアップロードされます。

## 次のステップ

Azure Virtual Machines で SQL Server を実行する方法の詳細については、次を参照してください。 [Azure の仮想マシンの概要 [SQL Server](virtual-machines-sql-server-infrastructure-services.md)します。


