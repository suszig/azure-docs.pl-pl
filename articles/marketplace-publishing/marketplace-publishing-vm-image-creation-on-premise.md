<properties
   pageTitle="Azure Marketplace 向けの仮想マシン イメージのオンプレミスでの作成 | Microsoft Azure"
   description="VM イメージをオンプレミスで作成し、他のユーザーが購入できるように Azure Marketplace にデプロイするための手順を理解して実行します。"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="10/08/2015"
  ms.author="hascipio; v-divte"/>

# Azure Marketplace 向けの仮想マシン イメージのオンプレミスでの作成
Azure 仮想ハード ディスク (VHD) は、リモート デスクトップ プロトコルを使用してクラウドで直接作成することを強くお勧めします。 ただし、必要な場合は、VHD をダウンロードして、オンプレミスのインフラストラクチャを使用して開発できます。  

オンプレミスでの開発では、作成済みの VM のオペレーション システム VHD をダウンロードする必要があります。 これらの手順は、手順 3.3 の一部として実行します。  

## VHD イメージをダウンロードする
### BLOB URL の検出
VHD をダウンロードするには、まず、オペレーティング システム ディスクの BLOB URL を探します。

新しい blob の URL を見つける [Microsoft Azure ポータル](https://ms.portal.azure.com):

1.  移動して **参照** > **Vm**, 、デプロイされた VM を選択します。
2.   **構成**, を選択、 **ディスク** 並べて表示する、ディスクのブレードを開きます。

  ![図](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)

3.  選択、 **OS ディスク**, 、VHD の場所を含むディスクのプロパティを表示する別のブレードを開きます。
4.  この BLOB URL をコピーします。

  ![図](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)

5.  デプロイされた VM を削除します。バック ディスクは削除しません。 削除するのではなく、VM を停止することもできます。 VM の実行中は、オペレーティング システム VHD をダウンロードしないでください。

  ![図](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### VHD のダウンロード
使用して、VHD をダウンロードするには、blob の URL を確認した後、 [Azure ポータル](http://manage.windowsazure.com/) または PowerShell。  
> [AZURE.NOTE] このガイドの作成時に、VHD をダウンロードするための機能がまだ存在しない新しい Microsoft Azure ポータルで。  

**現在使用してオペレーティング システムの VHD をダウンロード [Azure ポータル](http://manage.windowsazure.com/)**

1.  まだサインインしていない場合は、Azure ポータルにサインインします。
2.  クリックして、 **ストレージ** ] タブをクリックします。
3.  VHD が格納されているストレージ アカウントを選択します。

  ![図](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)

4.  これにより、ストレージ アカウントのプロパティが表示されます。 選択、 **コンテナー** ] タブをクリックします。

  ![図](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)

5.  VHD が格納されているコンテナーを選択します。 既定では、ポータルから作成されるときに、VHD は vhds コンテナーに格納されます。

  ![図](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)

6.  URL を保存済みの URL と比較して、適切なオペレーティング システム VHD を選択します。
7.  クリックして **ダウンロード**します。

  ![図](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### PowerShell を使用した VHD のダウンロード
Azure ポータルを使用して、に加えて使用して、 [Save-azurevhd](http://msdn.microsoft.com/library/dn495297.aspx) コマンドレットでは、オペレーティング システムの VHD のダウンロードをします。

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
たとえば、次のように入力します。
        Save-azurevhd-"https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd"- LocalFilePath"C:\Users\Administrator\Desktop\baseimagevm.vhd"StorageKey のソース <String>

> [AZURE.NOTE] **Save-azurevhd** も、 **NumberOfThreads** させるのダウンロード可能な帯域幅の使用が最適な並列処理の向上に使用できるオプションです。

## VHD を Azure ストレージ アカウントにアップロードする
VHD をオンプレミスで準備した場合は、それらを Azure のストレージ アカウントにアップロードする必要があります。 この手順は、VHD をオンプレミスで作成した後、VM イメージの認定を取得する前に実行します。

### ストレージ アカウントとコンテナーの作成
VHD は、米国内のリージョンのストレージ アカウントにアップロードすることをお勧めします。 1 つの SKU のすべての VHD は、1 つのストレージ アカウント内の 1 つのコンテナーに配置する必要があります。

使用できるストレージ アカウントを作成する、 [Microsoft Azure ポータル](https://portal.azure.com/), 、PowerShell、または Linux コマンド ライン ツールです。  

**Microsoft Azure ポータルからのストレージ アカウントの作成**

1.  クリックして **新しい**します。
2.  選択 **ストレージ**します。
3.  ストレージ アカウント名を入力し、場所を選択します。

  ![図](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)

4.  クリックして **作成**します。
5.  作成したストレージ アカウント用のブレードが開きます。 ない場合は、選択 **参照** > **ストレージ アカウント**します。 [ストレージ アカウント] ブレードで、先ほど作成したストレージ アカウントを選択します。
6.  選択 **コンテナー**します。

  ![図](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 

7.  [コンテナー] ブレードで、次のように選択します。 **追加**, 、し、コンテナー名とコンテナーのアクセス許可を入力します。 選択 **プライベート** のコンテナーのアクセス許可。

> [AZURE.TIP] 発行する予定の SKU ごとに 1 つのコンテナーを作成することをお勧めします。

  ![図](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### PowerShell を使用したストレージ アカウントの作成
使用してストレージ アカウントの作成 PowerShell を使用して、 [New-azurestorageaccount](http://msdn.microsoft.com/library/dn495115.aspx) コマンドレットです。

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

使用して、そのストレージ アカウント内のコンテナーを作成することができ、 [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) コマンドレットです。

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [AZURE.NOTE] これらのコマンドは、PowerShell で、現在のストレージ アカウント コンテキストを設定されていると仮定します。   参照してください [Azure PowerShell を設定する](marketplace-publishing-powershell-setup.md) PowerShell セットアップの詳細についてです。
 
### Mac と Linux 用のコマンドライン ツールを使用したストレージ アカウントの作成
 [Linux コマンド ライン ツール](../virtual-machines/command-line-tools/), 、次のようにストレージ アカウントを作成します。

        azure storage account create mystorageaccount --location "West US"

コンテナーを次のように作成します。

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## VHD のアップロード
ストレージ アカウントとコンテナーを作成したら、準備した VHD をアップロードできます。 PowerShell、Linux コマンドライン ツール、またはその他の Azure Storage 管理ツールを使用できます。

### PowerShell を使用して VHD をアップロードする
使用して、 [Add-azurevhd](http://msdn.microsoft.com/library/dn495173.aspx) コマンドレットです。

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### Mac と Linux 用のコマンドライン ツールを使用した VHD のアップロード
 [Linux コマンド ライン ツール](../virtual-machines/command-line-tools/), 、以下を使用します。
        azure vm イメージを作成します。 <image name> -場所 <Location of the data center> -OS Linux <LocationOfLocalVHD>

## 関連項目
- [Marketplace 向けの仮想マシン イメージの作成](marketplace-publishing-vm-image-creation.md)
- [Azure PowerShell の設定](marketplace-publishing-powershell-setup.md)


