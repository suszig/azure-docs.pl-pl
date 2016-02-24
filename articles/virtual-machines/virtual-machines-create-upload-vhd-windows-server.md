<properties
    pageTitle="Powershell を使用した Windows Server VHD の作成とアップロード | Microsoft Azure"
    description="作成し、Windows Server ベースのバーチャル ハード_ディスク (VHD)、従来のデプロイ モデルおよび Azure Powershell を使用してのアップロードについて説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/10/2015"
    ms.author="cynthn"/>

# Windows Server VHD の作成と Azure へのアップロード

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


この記事では、イメージを使用してそのイメージを基にした仮想マシンを作成するための、オペレーティング システムの仮想ハード ディスク (VHD) をアップロードする方法について説明します。 ディスクと Microsoft Azure で Vhd についての詳細については、次を参照してください。 [はディスクとバーチャル マシンの Vhd](virtual-machines-disks-vhds.md)します。



## 前提条件

この記事では、以下のことを前提としています。

1. **Azure サブスクリプション** -があるない場合は、 [無料 Azure アカウントを開く](/pricing/free-trial/?WT.mc_id=A261C142F): クレジットを取得する有料の Azure サービスを使用することができますをこれらの使用後にも、アカウントは維持まで無料の web サイトなどの Azure のサービスを使用します。 明示的に設定を変更して課金を求めない限り、クレジット カードに課金されることはありません。 できます [MSDN サブスクライバーの特典をアクティブ化](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): MSDN サブスクリプション クレジットにより、毎月提供される Azure の有料サービスを使用することができます。

2. **Microsoft Azure PowerShell** -Microsoft Azure PowerShell モジュールをインストールし、サブスクリプションを使用するように構成があります。 モジュールをダウンロードするを参照してください。 [Microsoft Azure のダウンロード](http://azure.microsoft.com/downloads/)します。 インストールして、モジュールの構成のチュートリアルが記載されて [ここ](../powershell-install-configure.md)します。 使用して、 [Add-azurevhd](http://msdn.microsoft.com/library/azure/dn495173.aspx) コマンドレットは VHD をアップロードします。

3. **Windows オペレーティング システムの .vhd ファイルに格納されているし、仮想マシンに接続がサポートされている** -.vhd ファイルを作成するツールは複数あります。 たとえば、Hyper-V を使用して仮想マシンを作成し、オペレーティング システムにインストールすることができます。 手順については、次を参照してください。 [Hyper-v 役割のインストールと仮想マシンの構成](http://technet.microsoft.com/library/hh846766.aspx)します。 オペレーティング システムに関する詳細については、「 [Microsoft Azure の仮想マシンの Microsoft サーバー ソフトウェア サポート](http://go.microsoft.com/fwlink/p/?LinkId=393550)します。

> [AZURE.IMPORTANT] Microsoft Azure では、VHDX 形式はサポートされていません。 ディスクを HYPER-V マネージャーを使用して VHD 形式に変換または [CONVERT-VHD コマンドレット](http://technet.microsoft.com/library/hh848454.aspx)します。 詳細については、これを「 [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx)します。

## 手順 1: VHD を準備する 

VHD を Azure にアップロードする前に、Sysprep ツールを使用して一般化する必要があります。 一般化によって、VHD をイメージとして使用できるように準備します。 詳細については、Sysprep は、次を参照してください。 [sysprep の使用方法: An Introduction](http://technet.microsoft.com/library/bb457073.aspx)します。

オペレーティング システムがインストールされている仮想マシンから、次の手順を実行します。

1. オペレーティング システムにサインインします。

2. 管理者としてコマンド プロンプト ウィンドウを開きます。 ディレクトリに移動 **%windir%\system32\sysprep**, 、し、実行 `sysprep.exe`します。

    ![コマンド プロンプト ウィンドウを開く](./media/virtual-machines-create-upload-vhd-windows-server/sysprep_commandprompt.png)

3.   **システム準備ツール** ] ダイアログ ボックスが表示されます。

    ![Sysprep の開始](./media/virtual-machines-create-upload-vhd-windows-server/sysprepgeneral.png)

4.   **システム準備ツール**, を選択 **入力システムのうち Box Experience (OOBE)** ことを確認し、 **Generalize** がオンになっています。

5.   **シャット ダウン オプション**, [ **シャット ダウン**します。

6.  Click **OK**.

## 手順 2: Azure ストレージ アカウントを作成する、またはアカウントの情報を取得する

.vhd ファイルをアップロードする場所を用意するために、Azure のストレージ アカウントが必要です。 この手順では、アカウントを作成する方法と、既存のアカウントから必要な情報を取得する方法について説明します。

### オプション 1: ストレージ アカウントを作成する

1. Azure クラシック ポータルにサインインします。

2. コマンド バー] をクリックして **新規**します。

3. クリックして **Data Services** > **ストレージ** > **簡易作成**します。

    ![ストレージ アカウントの簡易作成](./media/virtual-machines-create-upload-vhd-windows-server/Storage-quick-create.png)

4. 次のようにフィールドを指定します。

 -  **URL**, 、ストレージ アカウントの URL で使用するサブドメイン名を入力します。 文字数は 3 ～ 24 文字で、アルファベット小文字と数字を使用できます。 この名前は、サブスクリプションの BLOB、キュー、テーブル リソースへのアクセスに使用する URL のホスト名になります。
 - 選択、 **場所またはアフィニティ グループ** ストレージ アカウントにします。 アフィニティ グループを使用すると、クラウド サービスとストレージを同じデータセンターに配置できます。
 - 使用するかどうかを決定 **geo レプリケーション** ストレージ アカウントにします。 geo レプリケーションは既定で有効です。 このオプションでは、ユーザーのコスト負担なしで、データが 2 次拠点にコピーされるため、1 次拠点で大規模な障害が発生した場合に、2 次拠点にストレージをフェールオーバーできます。 2 次拠点は自動的に割り当てられ、変更することはできません。 法律上の要件または組織のポリシー上、クラウド方式のストレージの場所を厳格に管理する必要がある場合は、geo レプリケーションを無効にすることができます。 ただし、後で Geo レプリケーションを有効に戻すと、既存データを 2 次拠点にコピーするためのデータ転送料金が 1 回だけ発生することに注意してください。 Geo レプリケーションなしのストレージ サービスも割引価格で提供されています。 詳細については、次を参照してください。 [の作成、管理、またはストレージ アカウントを削除](../storage-create-storage-account/#replication-options)します。

      ![ストレージ アカウントの詳細の入力](./media/virtual-machines-create-upload-vhd-windows-server/Storage-create-account.png)

5. クリックして **ストレージ アカウントの作成**します。 アカウントは今すぐ下に表示されます。 **ストレージ**します。

    ![ストレージ アカウントの作成に成功](./media/virtual-machines-create-upload-vhd-windows-server/Storagenewaccount.png)

6. 次に、アップロードした VHD のコンテナーを作成します。 ストレージ アカウント名をクリックし、クリックして **コンテナー**します。

    ![ストレージ アカウントの詳細](./media/virtual-machines-create-upload-vhd-windows-server/storageaccount_detail.png)

7. クリックして **コンテナーを作成する**です。

    ![ストレージ アカウントの詳細](./media/virtual-machines-create-upload-vhd-windows-server/storageaccount_container.png)

8. 型 a **名前** コンテナーと選択、 **アクセス** ポリシーです。

    ![コンテナー名](./media/virtual-machines-create-upload-vhd-windows-server/storageaccount_containervalues.png)

    > [AZURE.NOTE] 既定では、コンテナーはプライベートなのアカウント所有者のみがアクセスできます。 コンテナーのプロパティとメタデータではありませんが、コンテナー内の blob に対するパブリック読み取りアクセスを許可するようを使用して、 **パブリック Blob** オプション。 コンテナーと blob に対する完全パブリック読み取りアクセスを許可するようを使用して、 **パブリック コンテナー** オプション。

### オプション 2: ストレージ アカウントの情報を取得する

1.  Azure クラシック ポータルにサインインします。

2.  ナビゲーション ペインから **ストレージ**します。

3.  ストレージ アカウントの名前をクリックし、クリックして **ダッシュ ボード**します。

4.  ダッシュ ボードで、[ **サービス**, 、URL をコピーし、貼り付けての保存するには、クリップボード アイコンをクリックして、Blob の URL にマウスをします。 この URL は、VHD をアップロードするコマンドを構築するときに使用します。

## 手順 3: Azure PowerShell からサブスクリプションに接続する

.vhd ファイルをアップロードする前に、コンピューターと Azure のサブスクリプションの間にセキュリティで保護された接続を確立する必要があります。 接続の確立には、Microsoft Azure Active Directory 方式または証明書方式を使用できます。

> [AZURE.TIP] Azure PowerShell を開始するを参照してください。 [をインストールして、Microsoft Azure PowerShell を構成する方法](../install-configure-powershell.md)します。 一般情報は、次を参照してください。 [Microsoft Azure コマンドレットの概要を取得します。](https://msdn.microsoft.com/library/azure/jj554332.aspx)

### オプション 1: Microsoft Azure AD を使用する

1. Azure PowerShell コンソールを開きます。

2. 次のコマンドを入力します。  
       `Add-AzureAccount`

3.  サインイン ウィンドウで、職場または学校のアカウントのユーザー名とパスワードを入力します。

4. Azure により資格情報が認証および保存され、ウィンドウが閉じます。

### オプション 2: 証明書を使用する

1. Azure PowerShell コンソールを開きます。

2.  次のコマンドを入力します。
          `Get-AzurePublishSettingsFile`

3. ブラウザー ウィンドウが開き、.publishsettings ファイルをダウンロードするよう求められます。 これには、Microsoft Azure のサブスクリプションについての情報と証明書が含まれています。

    ![ブラウザーのダウンロード ページ](./media/virtual-machines-create-upload-vhd-windows-server/Browser_download_GetPublishSettingsFile.png)

3. .publishsettings ファイルを保存します。

4. 次のコマンドを入力します。
       `Import-AzurePublishSettingsFile <PathToFile>`

    ここで、`<PathToFile>` は .publishsettings ファイルへの完全なパスです。

## ステップ 4: .vhd ファイルをアップロードする

.vhd ファイルをアップロードするときは、BLOB ストレージ内であればどこにでも .vhd ファイルを置くことができます。

1. 前の手順で使用した Azure PowerShell ウィンドウで、次のようにコマンドを入力します。

    `Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>`

    各値の説明:
    - **BlobStorageURL** ストレージ アカウントの URL は、
    - **YourImagesFolder** イメージを格納する blob ストレージ内のコンテナーには
    - **VHDName** 、名前を指定するバーチャル ハード_ディスクを識別するために表示する場合は、Azure 旧ポータル
    - **PathToVHDFile** は完全なパスと、.vhd ファイルの名前

    ![PowerShell Add-AzureVHD](./media/virtual-machines-create-upload-vhd-windows-server/powershell_upload_vhd.png)

Add-azurevhd コマンドレットの詳細については、次を参照してください。 [Add-azurevhd](http://msdn.microsoft.com/library/dn495173.aspx)します。

## 手順 5: カスタム イメージの一覧にイメージを追加する

> [AZURE.TIP] Azure クラシック ポータルではなく、イメージを追加する Azure PowerShell を使用する、 **Add-azurevmimage** コマンドレットです。 次に例を示します。

>   `Add-AzureVMImage -ImageName <ImageName> -MediaLocation <VHDLocation> -OS <OSType>`

1. Azure クラシック ポータルでは、下にある **すべてのアイテム**, 、] をクリックして **仮想マシン**します。

2. [バーチャル マシン] をクリックして **イメージ**します。

3. クリックして **イメージを作成**します。

    ![PowerShell Add-AzureVHD](./media/virtual-machines-create-upload-vhd-windows-server/Create_Image.png)

4.  **VHD からイメージを作成** ウィンドウ。

    - 指定の **名前**します。

    - 指定の **説明**します。

    -  **VHD の URL**, を開くには、フォルダー ボタンをクリックして、 **クラウド ストレージの参照** ウィンドウです。 .Vhd ファイルを見つけてクリックし、 **開く**します。

    ![VHD の選択](./media/virtual-machines-create-upload-vhd-windows-server/Select_VHD.png)

5.   **VHD からイメージを作成** ] ウィンドウで、 **オペレーティング システム ファミリ**, 、オペレーティング システムを選択します。 確認 **この VHD に関連付けられている仮想マシンで Sysprep を実行しました** オペレーティング システムを一般化したことを確認する **OK**します。

    ![Add Image](./media/virtual-machines-create-upload-vhd-windows-server/Create_Image_From_VHD.png)

6. 前の手順を完了すると、新しいイメージが一覧に表示を選択すると、 **イメージ** ] タブをクリックします。

    ![カスタム イメージ](./media/virtual-machines-create-upload-vhd-windows-server/vm_custom_image.png)

    この新しいイメージは、「 **マイ イメージ** 仮想マシンを作成する場合。 手順については、次を参照してください。 [Windows を実行するカスタム仮想マシンを作成する方法](virtual-machines-windows-create-custom.md)します。

    ![カスタム イメージからの VM の作成](./media/virtual-machines-create-upload-vhd-windows-server/create_vm_custom_image.png)

    > [AZURE.TIP] このエラー メッセージと、仮想マシンを作成しようとすると、エラーが発生した場合は、"VHD https://XXXXX YYYY バイトの仮想サイズがサポートされていません。 このサイズは整数 (MB 単位) でなければなりません。」というエラー メッセージが表示される場合は、VHD が整数の MB ではないことを意味します。VHD のサイズを修正する必要があります。 使用してください、 **Add-azurevmimage** 、イメージを追加する Azure クラシック ポータルではなく PowerShell コマンドレット (手順 5 を参照)。 Azure コマンドレットは、VHD が Azure 要件を満たしていることを確認します。

## 次のステップ ##

仮想マシンを作成したら、SQL Server 仮想マシンを作成してみてください。 手順については、次を参照してください。 [Microsoft Azure で SQL Server 仮想マシンのプロビジョニング](virtual-machines-provision-sql-server.md)します。

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Create a storage account in Azure]: #createstorage
[Step 3: Prepare the connection to Azure]: #prepAzure
[Step 4: Upload the .vhd file]: #upload

