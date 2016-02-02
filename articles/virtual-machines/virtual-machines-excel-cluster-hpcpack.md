<properties
 pageTitle="Excel と SOA 用の HPC Pack クラスター | Microsoft Azure"
 description="リソース マネージャーのデプロイ モデルを使用して HPC Pack クラスターを開始して Excel と SOA ワークロードを実行します。"
 services="virtual-machines"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>

<tags
 ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="11/11/2015"
 ms.author="danlep"/>


# Azure で HPC Pack クラスターを開始して Excel と SOA ワークロードを実行する

この記事では、Azure クイックスタート テンプレートまたは Azure PowerShell デプロイ スクリプトを使用して Azure インフラストラクチャ サービス (IaaS) に HPC Pack クラスターをデプロイする方法を示します。 HPC Pack で Microsoft Excel またはサービス指向アーキテクチャ (SOA) のワークロードを実行するように設計されている Azure Marketplace VM イメージを使用します。 クラスターを使用して、オンプレミスのクライアント コンピューターから簡単な Excel HPC サービスおよび SOA サービスを実行できます。 Excel の HPC サービスには、Excel ブックのオフロードと Excel ユーザー定義関数、または UDF が含まれます。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。


これから作成する HPC Pack クラスターの概要は次の図のようになります。

![Excel ワークロードを実行するノードを含む HPC クラスター][scenario]

## 前提条件

* **クライアント コンピューター** - Azure PowerShell のクラスター デプロイ スクリプト (このデプロイ方法を選択する場合) を実行したり、Excel および SOA のサンプル ジョブをクラスターに送信したりするには、Windows ベースのクライアント コンピューターが必要です。

* **Azure サブスクリプション** - アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、「 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/)します。

* **コア クォータ** - 場合によっては、コアのクォータを増やす必要があります。特に、マルチコア VM サイズのクラスター ノードをいくつかデプロイする場合に必要となる可能性があります。 Azure クイックスタート テンプレートを使用する場合、リソース マネージャーのコア クォータは Azure リージョンごとであることに注意してください。特定のリージョンのクォータを増やす必要がある可能性があります。 参照してください [Azure サブスクリプションの制限、クォータ、および制約](../azure-subscription-service-limits.md)します。 クォータを増やすをするには [オンライン カスタマー サポートに申請](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) 無償でします。


## 手順 1.Azure で HPC Pack クラスターをセットアップする

クラスターをセットアップする 2 つの方法を説明します。1 つめは Azure クイックスタート テンプレートと Azure ポータルを使用する方法で、2 つめは Azure PowerShell デプロイ スクリプトを使用する方法です。


### クイックスタート テンプレートを使用する

Azure クイックスタート テンプレートを使用すると、Azure ポータルで HPC Pack クラスターをすばやく簡単にデプロイできます。 プレビュー ポータルでテンプレートを開くと表示される簡単な UI で、クラスターの設定を入力します。 手順は次のようになります。

1. 参照してください、 [GitHub の HPC クラスターの作成テンプレート ページ](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)します。 必要な場合は、テンプレートとソース コードに関する情報を確認します。

2. Azure ポータルで **[Azure にデプロイ]** をクリックして、テンプレートによるデプロイを開始します。

    ![テンプレートを Azure にデプロイする][github]

3. プレビュー ポータルで以下の手順に従って、HPC クラスター テンプレートのパラメーターを入力します。

    a. **[テンプレートの編集]** ページで、**[保存]** をクリックします。

    ![テンプレートを保存する][template]

    b. **[パラメーター]** ページで、テンプレート パラメーターの値を入力します  (各設定の隣のアイコンをクリックするとヘルプ情報が表示されます)。 次の画面に示されているのはサンプルの値です。 この例では、1 つのヘッド ノードと 2 つのコンピューティング ノードで構成される *hpc01* という名前の新しい HPC Pack クラスターが、*hpc.local* ドメインに作成されます。 コンピューティング ノードは、Microsoft Excel を含む HPC Pack VM イメージから作成されます。

    ![パラメーターを入力する][parameters]
    >[AZURE.NOTE]ヘッド ノードから VM が自動的に作成される、 [最新の Marketplace イメージ](http://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) の Windows Server 2012 R2 上の HPC Pack 2012 R2。 現時点では、イメージは HPC Pack 2012 R2 Update 3 に基づいています。
    >
    >コンピューティング ノードの VM は、選択したコンピューティング ノード ファミリの最新のイメージから作成されます。 汎用の最新の HPC Pack 2012 R2 Update 3 コンピューティング イメージの **ComputeNode** オプションを選択します。 Microsoft Excel Professional Plus 2013 の評価版を含む最新の HPC Pack コンピューティング ノード イメージの **ComputeNodeWithExcel** オプションを選択します。 一般的な SOA セッション用または Excel UDF オフロード用にクラスターをデプロイする場合は、**ComputeNode** オプションを選択します (Excel はインストールされません)。
    >
    >使用する場合  **ComputeNodeWithExcel** 実稼働ワークロードでは、計算ノード上に Excel をアクティブにする有効な Excel ライセンスを提供する必要があります。 そうしないと、Excel の評価版は 30 日間の有効期限であり、Excel ブックを実行すると COMExeption (0x800AC472) で常に失敗します。 このような場合は、ヘッド ノードにログオンして HPC Cluster Manager コンソールから “%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe” をすべての Excel コンピューティング ノードで clusrun 実行し、さらにExcel の 30 日間の評価日数を再実装します。 猶予期間の再実装の最大回数は 2 回なので、それを過ぎると有効な Excel ライセンスを実装する必要があります。

    c. サブスクリプションを選択します。

    d. クラスターの新しいリソース グループを作成します (*hpc01RG* など)。

    e. リソース グループの場所を選択します (米国東部など)。

    f. **[法律条項]** ページで、条項を確認します。 同意する場合は、**[購入]** をクリックします。

    g. テンプレートの値の設定が完了したら、**[作成]** をクリックします。

    ![クラスターを作成する][create]

3.  デプロイが完了したら (通常約 30 分かかります)、クラスターのヘッド ノードからクラスターの証明書ファイルをエクスポートします。 後の手順でこのパブリック証明書をクライアント コンピューターにインポートし、セキュリティで保護された HTTP バインディングのサーバー側認証を提供します。

    a. Azure ポータルからリモート デスクトップでヘッド ノードに接続します。

     ![ヘッド ノードに接続する][connect]

    b. 標準的な手順で証明書マネージャーを使用して、秘密キーを含まないヘッド ノード証明書 (Cert:\LocalMachine\My の下にあります) をエクスポートします。 この例では、*CN = hpc01.eastus.cloudapp.azure.com* をエクスポートします。

    ![証明書をエクスポートする][cert]

### HPC Pack IaaS デプロイ スクリプトを使用する

HPC Pack IaaS デプロイ スクリプトは、HPC Pack クラスターをデプロイするためのもう 1 つの汎用性の高い方法です。 スクリプトは Azure サービス管理 (ASM) モードで実行しますが、テンプレートは Azure リソース マネージャー (ARM) モードで実行します。 また、スクリプトは Azure Global サービスまたは Azure China サービスのサブスクリプションと互換性があります。

**追加の前提条件**

* **Azure PowerShell** - [をインストールし、Azure PowerShell を構成](../powershell-install-configure.md) (バージョン 0.8.10 以降)、クライアント コンピューターにします。

* **HPC Pack IaaS デプロイメント スクリプト** - をダウンロードしてから、スクリプトの最新バージョンを開梱、 [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=44949)します。 実行して、スクリプトのバージョンを確認して `New-hpciaascluster.ps1 – バージョン`します。 この記事はバージョン 4.5.0 以降のスクリプトに基づきます。

**構成ファイルを作成する**

 HPC Pack IaaS デプロイ スクリプトは HPC クラスターのインフラストラクチャについて記載された XML 構成ファイルを入力として使用します。 Microsoft Excel を含むコンピューティング ノード イメージから作成された 1 個のヘッド ノードと 18 個のコンピューティング ノードで構成されるクラスターをデプロイするには、次のサンプル構成ファイルを実際の環境の値に置き換えます。 詳細については、構成ファイル、スクリプト フォルダーにある Manual.rtf ファイルを参照してくださいと [HPC Pack IaaS デプロイメント スクリプトで HPC クラスターを作成](virtual-machines-hpcpack-cluster-powershell-script.md)します。

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**構成ファイルに関する注意事項**

* ヘッド ノードの **VMName** は、**ServiceName** と正確に**一致している必要があります**。一致していないと、SOA ジョブの実行に失敗します。

* ヘッド ノード証明書が生成されてエクスポートされるように、**EnableWebPortal** を必ず指定してください。

* 構成後 PowerShell スクリプト PostConfig.ps1 では、Azure ストレージ接続文字列の設定、ヘッド ノードからのコンピューティング ノード ロールの削除、デプロイ後の全ノードのオンライン化など、ヘッド ノードでの特定の設定が構成されます。 スクリプトのサンプルを次に示します。

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**スクリプトを実行する**

1. 管理者としてクライアント コンピューターで PowerShell を開きます。

2. ディレクトリをスクリプト フォルダー (この例では、E:\IaaSClusterScript) に変更します。

    ```
    cd E:\IaaSClusterScript
```

4. Run the command below to deploy the HPC Pack cluster. This example assumes that the configuration file is located in E:\HPCDemoConfig.xml.
```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName

```

The HPC Pack deployment script will run for some time. One thing the script wil do is to export and download the cluster certificate and save it in the current user’s Documents folder on the client computer. The script will generate a message similar to the following. In a following step you'll import the certificate in the appropriate certificate store.
```
REST API または HPC Pack ヘッド ノード上の web ポータルを有効にします。 次のジョブを送信するまたは HPC web ポータルにアクセスしているコンピューター上の信頼されたルート証明機関の証明書ストアに証明書をインポートしてください。
 C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_2015070716
2011.cer
```

## Step 2. Offload Excel workbooks and run UDFs from an on-premises client

### Offload Excel workbooks

Follow these steps to offload an Excel workbook to run on the HPC Pack cluster in Azure. To do this, you must have Excel 2010 or 2013 already installed on the client computer.

1. Use one of the methods in Step 1 to deploy an HPC Pack cluster with the Excel compute node image. Obtain the cluster certificate file (.cer) and cluster username and password.

2. On the client computer, import the cluster certificate under Cert:\CurrentUser\Root.

3. Make sure Excel is installed. Create an Excel.exe.config file with the following contents in the same folder with Excel.exe on the client computer. This ensures that the HPC Pack 2012 R2 Excel COM add-in will be loaded successfully.
```
<?xml version="1.0"?>
<configuration>
    <startup useLegacyV2RuntimeActivationPolicy="true">
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
    </startup>
</configuration>
```
4.  Download the full [HPC Pack 2012 R2 Update 3 installation](http://www.microsoft.com/download/details.aspx?id=49922) and install the HPC Pack client,
or download and install the [HPC Pack 2012 R2 Update 3 client utilities](https://www.microsoft.com/download/details.aspx?id=49923) and the appropriate Visual C++ 2010 redistributable for your computer ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.  In this example, we use a sample Excel workbook named ConvertiblePricing_Complete.xlsb, available for download [here](https://www.microsoft.com/en-us/download/details.aspx?id=2939).

6.  Copy the Excel workbook to a working folder such as D:\Excel\Run.

7.  Open the Excel workbook. On the **Develop** ribbon, click **COM Add-Ins** and confirm that the HPC Pack Excel COM add-in is loaded successfully as shown in the following screen.

    ![Excel add-in for HPC Pack][addin]

8.  Edit the VBA macro HPCControlMacros in Excel by changing the commented lines as shown in the following script. Substitute appropriate values for your environment.

    ![Excel macro for HPC Pack][macro]
```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"
    
    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"
    
    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles
    
    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"
    
    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"

```

9.  Copy the Excel work book to an upload directory such as D:\Excel\Upload, as specified in the HPC_DependsFiles constant in the VBA macro.

10. Click the **Cluster** button on the worksheet to run the workbook on the Azure IaaS cluster.

### Run Excel UDFs

To run Excel UDFs, follow the preceding steps 1 – 3 to set up the client computer. For Excel UDFs, you don't need to have the Excel application installed on compute nodes, so you could choose a normal compute node image in Step 1 instead of the compute node image with Excel.

>[AZURE.NOTE] There is a 34 character limit in the Excel 2010 and 2013 cluster connector dialog box. If the full cluster name is longer, e.g. hpcexcelhn01.southeastasia.cloudapp.azure.com, it won't fit in the dialog box. The workaround is to set a machine wide variable e.g. *CCP_IAASHN* with the value of the long cluster name and input *%CCP_IAASHN%* in the dialog box as the cluster head node name. Note for Update 2 clusters, it requires the Update 2 QFE KB3085833 (download [here](http://www.microsoft.com/en-us/download/details.aspx?id=48725)) for SOA Session API on the client machine to support this workaround.

After the cluster is successfully deployed, continue with the following steps to run a sample built-in Excel UDF. For customized Excel UDFs, see these [resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) to build the XLLs and deploy them on the IaaS cluster.

1.  Open a new Excel workbook. On the **Develop** ribbon, click **Add-Ins**. Then, in the dialog box, click **Browse**, navigate to the %CCP_HOME%Bin\XLL32 folder, and select the sample ClusterUDF32.xll. If the ClusterUDF32 doesn't exist on the client machine, you can copy it from the %CCP_HOME%Bin\XLL32 folder on the head node.

    ![Select the UDF][udf]

2.  Click **File** > **Options** > **Advanced**. Under **Formulas** check **Allow user-defined XLL functions to run a compute cluster**. Then click **Options** and enter the full cluster name in **Cluster head node name**. (As noted previously this input box is limited to 34 characters, so a long cluster name may not fit. You may use machine wide variables here for long cluster names.)

    ![Configure the UDF][options]

3.  Click the cell with value =XllGetComputerNameC() and press Enter to run the UDF calculation on the IaaS cluster. The function will simply retrieve the name of the compute node on which the UDF runs. For the first run, a credentials dialog box prompts for the username and password to connect to the IaaS cluster.

    ![Run UDF][run]

    When there are a lot of cells to calculate, press Alt-Shift-Ctrl + F9 to run the calculation on all cells.

## Step 3. Run a SOA workload from an on-premises client

To run general SOA applications on the HPC Pack IaaS cluster, first use one of the methods in Step 1 to deploy the IaaS cluster, using a generic compute node image (because you will not need Excel on the compute nodes). Then follow these steps.

1. After retrieving the cluster certificate, import it on the client computer under Cert:\CurrentUser\Root.

2. Install the [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) and [HPC Pack 2012 R2 Update 3 client utilities](https://www.microsoft.com/download/details.aspx?id=49923) so you can develop and run SOA client applications.

3. Download the HellowWorldR2 [sample code](https://www.microsoft.com/download/details.aspx?id=41633). Open the HelloWorldR2.sln in Visual Studio 2010 or 2012.

4. Build the EchoService project first and deploy the service to the IaaS cluster in the same way you deploy to an on-premises cluster. For detailed steps, see the Readme.doc in HelloWordR2. Modify and build the HellowWorldR2 and other projects as described below to generate the SOA client applications running on an Azure IaaS cluster from an on-premises client computer.

### Use Http binding with Azure storage queue

To use Http binding with an Azure storage queue, make a few changes to the sample code.

* Update the cluster name.
```
前に
const 文字列ヘッドノード「[ヘッド ノード]」を =;
後例。
const 文字列ヘッドノード ="hpc01.eastus.cloudapp.azure.com"です。
または
const 文字列ヘッドノード ="hpc01.cloudapp.net"です。
```

* Optionally, use default TransportScheme in SessionStartInfo or explicitly set it to Http.
```
    info.TransportScheme = TransportScheme.Http;

```

* Use default binding for the BrokerClient.
```
前に
使用して (brokerclient に<IService1> クライアント = 新しい brokerclient に<IService1>(セッション、バインディング))
後
使用して (brokerclient に<IService1> クライアント = 新しい brokerclient に<IService1>(セッション))
```

    Or set explicitly using the basicHttpBinding.
```
BasicHttpBinding バインド = 新しい BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential) です。
バインドします。Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName です。   バインドします。Security.Transport.ClientCredentialType = HttpClientCredentialType.None です。
```

* Optionally, set the UseAzureQueue flag to true in SessionStartInfo. If not set, it will be set to true by default when the cluster name has Azure domain suffixes and the TransportScheme is Http.
```
    info.UseAzureQueue = true;

```

###Use Http binding without Azure storage queue

To do this, explicitly set UseAzureQueue flag to false in the SessionStartInfo.
```
    info.UseAzureQueue = false;

```

### NetTcp バインディングを使用する

NetTcp バインディングを使用するための構成は、オンプレミスのクラスターに接続する場合と似ています。 ヘッド ノード VM でいくつかのエンドポイントを開く必要があります。 Azure クラシック ポータルで次のようにします。


1. VM を停止します。

2. セッション用、ブローカー用、ブローカー ワーカー用、Data Services 用に、それぞれ TCP ポート 9090、9087、9091、9094 を追加します。

    ![エンドポイントを構成する][endpoint]

3. VM を起動します。

SOA クライアント アプリケーションでは、IaaS クラスターの完全な名前にヘッド名を変更する以外の変更は不要です。

## 次のステップ

* 参照してください [これらのリソース](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) 詳細については、HPC pack Excel ワークロードを実行します。

* 参照してください [Microsoft HPC pack SOA サービスを管理する](https://technet.microsoft.com/library/ff919412.aspx) の詳細については、HPC Pack での SOA サービス展開、管理します。



[scenario]: ./media/virtual-machines-excel-cluster-hpcpack/scenario.png 
[github]: ./media/virtual-machines-excel-cluster-hpcpack/github.png 
[template]: ./media/virtual-machines-excel-cluster-hpcpack/template.png 
[parameters]: ./media/virtual-machines-excel-cluster-hpcpack/parameters.png 
[create]: ./media/virtual-machines-excel-cluster-hpcpack/create.png 
[connect]: ./media/virtual-machines-excel-cluster-hpcpack/connect.png 
[cert]: ./media/virtual-machines-excel-cluster-hpcpack/cert.png 
[addin]: ./media/virtual-machines-excel-cluster-hpcpack/addin.png 
[macro]: ./media/virtual-machines-excel-cluster-hpcpack/macro.png 
[options]: ./media/virtual-machines-excel-cluster-hpcpack/options.png 
[run]: ./media/virtual-machines-excel-cluster-hpcpack/run.png 
[endpoint]: ./media/virtual-machines-excel-cluster-hpcpack/endpoint.png 
[udf]: ./media/virtual-machines-excel-cluster-hpcpack/udf.png 

