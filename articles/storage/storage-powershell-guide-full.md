<properties
    pageTitle="Azure Storage での Azure PowerShell の使用 | Microsoft Azure"
    description="Azure Storage 用の Azure PowerShell コマンドレットを使用して、ストレージ アカウントの作成と管理、BLOB、テーブル、キュー、およびファイルの操作、ストレージ分析の構成と照会、共有アクセス署名の作成を行う方法について説明します。"
    services="storage"
    documentationCenter="na"
    authors="robinsh" 
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/07/2015"
    ms.author="robinsh"/>


# Azure Storage での Azure PowerShell の使用

## 概要

このガイドでは、使用する方法を見ていきます、 [ストレージ用の Azure サービス管理コマンドレット](https://msdn.microsoft.com/library/azure/dn806401.aspx) さまざまな Azure Storage での開発と管理のタスクを実行します。

Azure PowerShell は、Windows PowerShell から Azure を管理するコマンドレットを提供するモジュールです。 タスクベースのコマンドライン シェルとスクリプト言語であり、システム管理に特化して設計されています。 PowerShell を使用すると、Azure サービスやアプリケーションの管理を容易に制御して自動化できます。 使用して実行できるタスクと同じタスクを実行するコマンドレットを使用するなど、 [Azure ポータル](portal.azure.com)します。

使用した経験があることを前提と [Azure Storage](http://azure.microsoft.com/documentation/services/storage/) と [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx)します。 また、PowerShell と Azure Storage の使用方法を示すための多くのスクリプトを用意しています。 各スクリプトの実行前に、使用する構成に基づいてスクリプト変数を更新する必要があります。

このガイドの最初のセクションでは、Azure Storage と PowerShell について簡単に説明します。 詳細な情報および手順についてから開始、 [Azure Storage で Azure PowerShell を使用するための前提条件](#prerequisites-for-using-azure-powershell-with-azure-storage)します。


## 5 分で始める、Azure Storage と PowerShell の使用

このセクションでは、PowerShell から Azure Storage にアクセスする方法を 5 分で説明します。

**Azure を初めて使用する場合:** Microsoft Azure サブスクリプションと、そのサブスクリプションに関連付けられた Microsoft アカウントを入手してください。 Azure の購入オプションについては、次を参照してください。 [無料評価版](http://azure.microsoft.com/pricing/free-trial/), 、[購入オプション](http://azure.microsoft.com/pricing/purchase-options/), 、および [メンバー プラン](http://azure.microsoft.com/pricing/member-offers/) (MSDN、Microsoft Partner Network、BizSpark、および他の Microsoft プログラムのメンバー) 用です。

参照してください [アカウントの管理、サブスクリプション、および管理者の役割](https://msdn.microsoft.com/library/azure/hh531793.aspx) 詳細については、Azure サブスクリプション。

**Microsoft Azure アカウントとサブスクリプションを作成済みである場合:**

1.  ダウンロードしてインストール [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409)します。
2.  Windows PowerShell Integrated Scripting Environment (ISE) の起動: ローカル コンピューターで、**[スタート]** メニューを表示します。 「**管理ツール**」と入力し、クリックして実行します。 **[管理ツール]** ウィンドウで、**[Windows PowerShell ISE]** を右クリックし、**[管理者として実行]** をクリックします。
3.  **Windows PowerShell ISE** で、**[ファイル]**、**[新規作成]** の順にクリックし、新しいスクリプト ファイルを作成します。
4.  ここでは、Azure Storage にアクセスするための基本的な PowerShell コマンドを示すシンプルなスクリプトを取り上げます。 このスクリプトでは、Azure アカウントをローカルの PowerShell 環境に追加するための Azure アカウント資格情報が最初に求められます。 次に、既定の Azure サブスクリプションが設定され、Azure で新しいストレージ アカウントが作成されます。 さらに、この新しいストレージ アカウントで新しいコンテナーが作成され、既存の画像ファイル (BLOB) がこのコンテナーにアップロードされます。 このスクリプトにより、コンテナー内のすべての BLOB がリストされると、ローカル コンピューターに新しい格納先ディレクトリが作成され、画像ファイルがダウンロードされます。
5.  次のコードのセクションで、**#begin** と **#end** の間のスクリプトを選択します。 Ctrl キーを押しながら C キーを押し、クリップボードにコピーします。

    #begin
    # Update with the name of your subscription.
    $SubscriptionName="YourSubscriptionName"
    
    # Give a name to your new storage account. It must be lowercase!
    $StorageAccountName="yourstorageaccountname"
    
    # Choose "West US" as an example.
    $Location = "West US"
    
    # Give a name to your new container.
    $ContainerName = "imagecontainer"
    
    # Have an image file and a source directory in your local computer.
    $ImageToUpload = "C:\Images\HelloWorld.png"
    
    # A destination directory in your local computer.
    $DestinationFolder = "C:\DownloadImages"
    
    # Add your Azure account to the local PowerShell environment.
    Add-AzureAccount
    
    # Set a default Azure subscription.
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
    
    # Create a new storage account.
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location
    
    # Set a default storage account.
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    
    # Create a new container.
    New-AzureStorageContainer -Name $ContainerName -Permission Off
    
    # Upload a blob into a container.
    Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload
    
    # List all blobs in a container.
    Get-AzureStorageBlob -Container $ContainerName
    
    # Download blobs from the container:
    # Get a reference to a list of all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName
    
    # Create the destination directory.
    New-Item -Path $DestinationFolder -ItemType Directory -Force  
    
    # Download blobs into the local destination directory.
    $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
    #end

5.  **Windows PowerShell ISE** で、Ctrl キーを押しながら V キーを押してスクリプトを貼り付けます。 **[ファイル]**、**[保存]** の順にクリックします。 **[名前を付けて保存]** ダイアログ ウィンドウで、スクリプト ファイルの名前を "mystoragescript" などとして入力します。 **[保存]** をクリックします。

6.  ここで、構成設定に基づいてスクリプト変数を更新する必要があります。 たとえば、**$SubscriptionName** 変数を自分のサブスクリプションで更新する必要があります。 その他の変数は、スクリプトで指定されたままにすることも、必要に応じて更新することも可能です。

    - **$SubscriptionName:** この変数は、自分のサブスクリプション名で更新する必要があります。 次の 3 つの方法のいずれかに従って、サブスクリプションの名前を確認します。

        a. **Windows PowerShell ISE** で、**[ファイル]**、**[新規作成]** の順にクリックし、新しいスクリプト ファイルを作成します。 次のスクリプトを新しいスクリプト ファイルにコピーし、**[デバッグ]**、**[実行/続行]** の順にクリックします。 次のスクリプトでは、Azure アカウントをローカルの PowerShell 環境に追加するための Azure アカウント資格情報が最初に求められ、次に、ローカルの PowerShell セッション接続されているすべてのサブスクリプションが表示されます。 このチュートリアルに従って作業する際に使用するサブスクリプションの名前を書き留めます。

            Add-AzureAccount
                Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName

        b. 見つけてにサブスクリプションの名前をコピーする、 [Azure ポータル](portal.azure.com), 、左側の [ハブ] メニューをクリックして **サブスクリプション**します。 このガイドのスクリプトを実行する際に使用するサブスクリプションの名前をコピーします。

        ![Azure ポータル][image2]

        c. 見つけてにサブスクリプションの名前をコピーする、 [Azure Classic Portal](https://manage.windowsazure.com/), 、スクロール ダウンして、クリックして **設定** ポータルの左側にあります。 **[サブスクリプション]** をクリックすると、お客様のサブスクリプションの一覧が表示されます。 このガイドで取り上げられているスクリプトを実行する際に使用するサブスクリプションの名前をコピーします。

        ![Azure クラシック ポータル][image1]

    - **$StorageAccountName:** スクリプトの所定の名前を使用するか、ストレージ アカウントの新しい名前を入力します。 **重要:** ストレージ アカウントの名前は、Azure 上で一意である必要があります。 また、小文字にする必要もあります。

    - **$Location:** スクリプトの所定の "West US" を使用するか、その他の Azure の場所 (East US、North Europe など) を選択します。

    - **$ContainerName:** スクリプトの所定の名前を使用するか、コンテナーの新しい名前を入力します。

    - **$ImageToUpload:** ローカル コンピューター上の画像へのパスを入力します。たとえば、"C:\Images\HelloWorld.png" などです。

    - **$DestinationFolder:** Azure Storage からダウンロードしたファイルを格納するローカル ディレクトリへのパスを入力します。たとえば、"C:\DownloadImages" などです。

7.  "mystoragescript.ps1" ファイル内のスクリプト変数を更新したら、**[ファイル]**、**[保存]** の順にクリックします。 次に、**[デバッグ]** をクリックし、**[実行/続行]** をクリックするか **F5** キーを押してスクリプトを実行します。

スクリプトを実行すると、ダウンロードした画像ファイルを格納するローカルの格納先フォルダーの準備が整います。 次のスクリーンショットは、この出力の例を示しています。

![BLOB をダウンロードする][image3]

> [AZURE.NOTE] 「5 分で始める、Azure Storage と PowerShell の使用」のセクションでは、Azure Storage で Azure PowerShell を使用する方法について簡単に説明しました。 詳細と手順については、以下のセクションを参照することをお勧めします。

## Azure Storage で Azure PowerShell を使用するための前提条件

このガイドで取り上げている PowerShell コマンドレットを実行するには、既に説明したように Azure サブスクリプションとアカウントが必要です。

Azure PowerShell は、Windows PowerShell から Azure を管理するコマンドレットを提供するモジュールです。 Azure PowerShell をインストールおよびセットアップについては、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md)します。 このガイドを使用する前に、最新の Azure PowerShell モジュールをダウンロードしてインストールするか、アップグレードすることをお勧めします。

Azure PowerShell コンソール、標準の Windows PowerShell コンソール、または Windows PowerShell Integrated Scripting Environment (ISE) でコマンドレットを実行できます。 たとえば、**Azure PowerShell コンソール**を開くには、[スタート] メニューで、「Microsoft Azure PowerShell」と入力して右クリックし、[管理者として実行] をクリックします。 **Windows PowerShell ISE** を開くには、[スタート] メニューで、「管理ツール」と入力し、クリックして実行します。 [管理ツール] ウィンドウで、[Windows PowerShell ISE] を右クリックし、[管理者として実行] をクリックします。

## Azure でストレージ アカウントを管理する方法

### 既定の Azure サブスクリプションを設定する方法

Azure PowerShell を使用して Azure Storage を管理するには、Azure Active Directory 認証または証明書ベースの認証を使用して、Azure でのクライアント環境を認証する必要があります。 詳細については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md) チュートリアルです。 このガイドでは、Azure Active Directory 認証を使用します。

1.  Azure PowerShell コンソールまたは Windows PowerShell ISE で、次のコマンドを入力し、Azure アカウントをローカルの PowerShell 環境に追加します。

    `Add-AzureAccount`

2.  [Microsoft Azure へのサインイン] ウィンドウで、アカウントに関連付けられた電子メール アドレスとパスワードを入力します。 Azure により資格情報が認証および保存され、ウィンドウが閉じます。

3.  次のコマンドを実行し、ローカルの PowerShell 環境内の Azure アカウントを表示して、自分のアカウントが含まれていることを確認します。

    `Get-AzureAccount`

4.  次のコマンドレットを実行し、ローカルの PowerShell セッションに接続されているすべてのサブスクリプションを表示して、自分のサブスクリプションが含まれていることを確認します。

    `Get-azuresubscription |Format-table SubscriptionName、IsDefault IsCurrent、CurrentStorageAccountName`

5.  既定の Azure サブスクリプションを設定するには、Select-AzureSubscription コマンドレットを実行します。

        $SubscriptionName = 'Your subscription Name'
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

6.  Get-AzureSubscription コマンドレットを実行することで、既定のサブスクリプションの名前を確認します。

    `Get-azuresubscription-既定値`

7.  Azure Storage に使用できるすべての PowerShell コマンドレットを確認するには、次を実行します。

    `Get-command - モジュール Azure - 名詞 * ストレージ *`

### 新しい Azure ストレージ アカウントを作成する方法

Azure Storage を使用するには、ストレージ アカウントが必要です。 コンピューターを構成してサブスクリプションに接続できるようにすると、新しい Azure ストレージ アカウントを作成できます。

1.  Get-AzureLocation コマンドレットを実行し、使用できるデータセンターの場所をすべて見つけます。

    `Get-azurelocation |表の書式設定のプロパティ名、AvailableServices、StorageAccountTypes`

2.  New-AzureStorageAccount コマンドレットを実行し、新しいストレージ アカウントを作成します。 次の例では、"West US" というデータセンターに新しいストレージ アカウントを作成します。

        $location = "West US"
        $StorageAccountName = "yourstorageaccount"
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location


> [AZURE.IMPORTANT] ストレージ アカウントの名前は Azure 内で一意であり、小文字でなければなりません。 名前付け規則と制限については、次を参照してください。 [Azure ストレージ アカウントについて](storage-create-storage-account.md) と [名前付けおよび参照するコンテナー、Blob、およびメタデータ](http://msdn.microsoft.com/library/azure/dd135715.aspx)します。

### 既定の Azure ストレージ アカウントを設定する方法

サブスクリプションで複数のストレージ アカウントを持つことができます。 それらのいずれかのアカウントを選択し、同じ PowerShell セッションのすべてのストレージ コマンドに対する既定のストレージ アカウントとして設定できます。 そうすることにより、ストレージ コンテキストを明示的に指定しなくても、Azure PowerShell ストレージ コマンドを実行できます。

1.  サブスクリプションの既定のストレージ アカウントを設定するために、Set-AzureSubscription コマンドレットを実行します。

        $SubscriptionName = "Your subscription name"
        $StorageAccountName = "yourstorageaccount"  
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

2.  次に、Get-AzureSubscription コマンドレットを実行し、ストレージ アカウントが既定のサブスクリプション アカウントに関連付けられていることを確認します。 このコマンドは、現在のサブスクリプションにおけるサブスクリプション プロパティを返します。これには、現在のストレージ アカウントが含まれます。

        Get-AzureSubscription –Current


### サブスクリプション内のすべての Azure ストレージ アカウントをリストする方法

Azure サブスクリプションごとに最大 100 個のストレージ アカウントを作成できます。 制限に関する最新情報については、次を参照してください。 [Azure サブスクリプションとサービスの制限、クォータ、および制約](../azure-subscription-service-limits.md)します。

次のコマンドレットを実行し、現在のサブスクリプションのストレージ アカウントの名前とステータスを確認します。

    Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus

### Azure ストレージ コンテキストを作成する方法

Azure ストレージ コンテキストは、ストレージ資格情報をカプセル化するための PowerShell 内のオブジェクトです。 後続のコマンドレットを実行する際にストレージ コンテキストを使用すると、ストレージ アカウントとそのアクセス キー明示的に指定しなくても、要求を認証できます。 ストレージ コンテキストは、ストレージ アカウント名とアクセス キーを使用する、共有アクセス署名 (SAS) トークンを使用する、接続文字列または匿名を使用するなどのさまざまな方法で作成できます。 詳細については、次を参照してください。 [New-azurestoragecontext](http://msdn.microsoft.com/library/azure/dn806380.aspx)します。

次の 3 つの方法のいずれかを使用してストレージ コンテキストを作成します。

- 実行、 [Get-azurestoragekey](http://msdn.microsoft.com/library/azure/dn495235.aspx) コマンドレット、Azure ストレージ アカウントのプライマリ ストレージ アクセス キーを使用します。 次を呼び出す、 [New-azurestoragecontext](http://msdn.microsoft.com/library/azure/dn806380.aspx) コマンドレットをストレージ コンテキストを作成します。

        $StorageAccountName = "yourstorageaccount"
        $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
        $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

- Azure ストレージ コンテナーの共有アクセス署名トークンを生成し、これを使用してストレージ コンテキストを作成します。

        $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
        $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken

    詳細については、次を参照してください。 [New-azurestoragecontainersastoken](http://msdn.microsoft.com/library/azure/dn806416.aspx) と [共有アクセス署名、第 1 部: SAS モデルについて ](storage-dotnet-shared-access-signature-part-1.md)します。

- 場合によっては、新しいストレージ コンテキストの作成時にサービスのエンドポイントを指定することもあります。 これは、BLOB サービスでストレージ アカウントのカスタム ドメイン名を登録した場合に必要になることがあります。または、アクセスしているストレージ リソースに対して共有アクセス署名を使用します。 次に示すように、接続文字列でサービスのエンドポイントを設定し、このエンドポイントを使用して新しいストレージ コンテキストを作成します。

        $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
        $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString


ストレージ接続文字列を構成する方法の詳細については、次を参照してください。 [接続文字列の構成](storage-configure-connection-string.md)します。

これで、コンピューターを設定できました。また、Azure PowerShell を使用してサブスクリプションとストレージ アカウントを管理する方法を確認できました。 次のセクションでは、Azure BLOB と BLOB のスナップショットを管理する方法について確認します。

## Azure BLOB の管理方法

Azure BLOB ストレージは、HTTP または HTTPS 経由で世界中のどこからでもアクセスできるテキストやバイナリ データなど、大量の非構造化データを格納するためのサービスです。 このセクションでは、Azure BLOB Storage サービスの概念について理解しているユーザーを対象としています。 詳細については、次を参照してください。 [.NET から Blob ストレージを使用する方法](storage-dotnet-how-to-use-blobs.md) と [Blob サービスの概念](http://msdn.microsoft.com/library/azure/dd179376.aspx)します。

### コンテナーを作成する方法

Azure Storage のすべての BLOB はコンテナーに格納する必要があります。 New-AzureStorageContainer コマンドレットを使用して、プライベート コンテナーを作成できます。

    $StorageContainerName = "yourcontainername"
    New-AzureStorageContainer -Name $StorageContainerName -Permission Off

> [AZURE.NOTE] **Off**、**BLOB**、**Container** という 3 つのレベルの匿名読み取りアクセスがあります。 BLOB に対する匿名アクセスを許可しない場合は、Permission パラメーターを **Off** に設定します。 既定では、新しいコンテナーはプライベートであり、アカウント所有者のみがアクセスできます。 BLOB リソースに対する匿名パブリック読み取りアクセスを許可するが、コンテナー メタデータまたはコンテナー内の BLOB の一覧に対するアクセスは許可しない場合は、Permission パラメーターを **BLOB** に設定します。 BLOB リソース、コンテナー メタデータ、コンテナー内の BLOB の一覧に対する完全パブリック読み取りアクセスを許可する場合は、Permission パラメーターを **Container** に設定します。 詳細については、次を参照してください。 [Azure ストレージ リソースへのアクセスの管理](storage-manage-access-to-resources.md)します。

### BLOB をコンテナーにアップロードする方法

Azure BLOB Storage では、ブロック BLOB とページ BLOB がサポートされています。 詳細については、次を参照してください。 [ブロック Blob およびページ Blob について](http://msdn.microsoft.com/library/azure/ee691964.aspx)します。

内の blob をコンテナーにアップロードするにを使用して、 [Set-azurestorageblobcontent](http://msdn.microsoft.com/library/azure/dn806379.aspx) コマンドレットです。 既定では、このコマンドにより、ローカル ファイルがブロック BLOB にアップロードされます。 BLOB の種類を指定する場合は、-BlobType パラメーターを使用します。

次の例の実行、 [Get-childitem](http://technet.microsoft.com/library/hh849800.aspx) コマンドレットを指定したフォルダー内のすべてのファイルを取得し、パイプライン演算子を使用して次のコマンドレットに渡します。  [Set-azurestorageblobcontent](http://msdn.microsoft.com/library/azure/dn806379.aspx) コマンドレットをコンテナーに、ローカル ファイルをアップロードします。

    Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"

### BLOB をコンテナーからダウンロードする方法

次の例は、BLOB をコンテナーからダウンロードする方法を示しています。 この例では、まず、ストレージ アカウント コンテキストを使用して Azure Storage への接続を確立します。このコンテキストには、ストレージ アカウント名とそのプライマリ アクセス キーが含まれます。 次に、例では、取得を使用して blob の参照、 [Get-azurestorageblob](http://msdn.microsoft.com/library/azure/dn806392.aspx) コマンドレットです。 次に、この例を使用して、 [Get-azurestorageblobcontent](http://msdn.microsoft.com/library/azure/dn806418.aspx) コマンドレットはローカルの格納先フォルダーに blob をダウンロードします。

    #Define the variables.
    $ContainerName = "yourcontainername"
    $DestinationFolder = "C:\DownloadImages"
    
    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    
    #List all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx
    
    #Download blobs from a container.
    New-Item -Path $DestinationFolder -ItemType Directory -Force
    $blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx

### あるストレージ コンテナーから別のストレージ コンテナーに BLOB をコピーする方法

BLOB は、ストレージ アカウントとリージョン間にまたがって非同期的にコピーできます。 次の例は、2 つの異なるストレージ アカウントで、BLOB をあるストレージ コンテナーから別のストレージ コンテナーにコピーする方法を示しています。 この例では、まずソースとコピー先のストレージ アカウントの変数を設定し、次にそれぞれのアカウントのストレージ コンテキストを作成します。 次に、例では、blob コピー元のコンテナーから移行先のコンテナーを使用する、 [Start-azurestorageblobcopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) コマンドレットです。 この例では、ソースとコピー先のストレージ アカウントとコンテナーが既に存在していると想定しています。

    #Define the source storage account and context.
    $SourceStorageAccountName = "yoursourcestorageaccount"
    $SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
    $SrcContainerName = "yoursrccontainername"
    $SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey
    
    #Define the destination storage account and context.
    $DestStorageAccountName = "yourdeststorageaccount"
    $DestStorageAccountKey = "Storage key for yourdeststorageaccount"
    $DestContainerName = "destcontainername"
    $DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey
    
    #Get a reference to blobs in the source container.
    $blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext
    
    #Copy blobs from one container to another.
    $blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext

この例で実行するのは非同期コピーです。 実行して各コピーの状態を監視することができます、 [Get-azurestorageblobcopystate](http://msdn.microsoft.com/library/azure/dn806406.aspx) コマンドレットです。

### セカンダリの場所から BLOB をコピーする方法

RA-GRS が有効になっているアカウントのセカンダリの場所から BLOB をコピーすることができます。

    #define secondary storage context using a connection string constructed from secondary endpoints. 
    $SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
    Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext

### BLOB を削除する方法

BLOB を削除するには、まず BLOB の参照を取得し、次にその Remove-AzureStorageBlob コマンドレットを呼び出します。 次の例では、特定のコンテナー内のすべての BLOB を削除します。 この例では、まずストレージ アカウントの変数を設定し、次にストレージ コンテキストを作成します。 次に、例を使用して blob の参照を取得、 [Get-azurestorageblob](http://msdn.microsoft.com/library/azure/dn806392.aspx) コマンドレットを実行し、 [Remove-azurestorageblob](http://msdn.microsoft.com/library/azure/dn806399.aspx) コマンドレットを Azure storage のコンテナーから blob を削除します。

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "containername"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    
    #Get a reference to all the blobs in the container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx
    
    #Delete blobs in a specified container.
    $blobs| Remove-AzureStorageBlob

## Azure BLOB のスナップショットを管理する方法

Azure では BLOB のスナップショットを作成できます。 スナップショットは、ある時点で作成された読み取り専用の BLOB です。 作成されたスナップショットは、読み取り、コピー、削除はできますが、変更はできません。 スナップショットを使用すると、BLOB をある時点での表示内容のままバックアップできます。 詳細については、次を参照してください。 [Blob のスナップショットを作成する](http://msdn.microsoft.com/library/azure/hh488361.aspx)します。

### BLOB のスナップショットを作成する方法

Blob のスナップショットを作成するには、まず blob の参照を取得し、しを呼び出す、 `ICloudBlob.CreateSnapshot` メソッドをします。 次の例では、まずストレージ アカウントの変数を設定し、次にストレージ コンテキストを作成します。 次に、例では、取得を使用して blob の参照、 [Get-azurestorageblob](http://msdn.microsoft.com/library/azure/dn806392.aspx) コマンドレットを実行し、 [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) スナップショットを作成する方法です。

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "yourcontainername"
    $BlobName = "yourblobname"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    
    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName
    
    #Create a snapshot of the blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

### BLOB のスナップショットをリストする方法

1 個の BLOB に対して必要な数だけスナップショットを作成できます。 BLOB に関連付けられたスナップショットをリストして、現在のスナップショットを追跡できます。 次のコードの例は、定義済みの blob と呼び出し、 [Get-azurestorageblob](http://msdn.microsoft.com/library/azure/dn806392.aspx) コマンドレットは、その blob のスナップショットの一覧を使用します。

    #Define the blob name.
    $BlobName = "yourblobname"
    
    #List the snapshots of a blob.
    Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }

### BLOB のスナップショットをコピーする方法

BLOB のスナップショットをコピーして復元できます。 詳細な情報と制限については、次を参照してください。 [Blob のスナップショットを作成する](http://msdn.microsoft.com/library/azure/hh488361.aspx)します。 次の例では、まずストレージ アカウントの変数を設定し、次にストレージ コンテキストを作成します。 さらに、コンテナーと BLOB 名変数を定義します。 使用して blob の参照を取得、 [Get-azurestorageblob](http://msdn.microsoft.com/library/azure/dn806392.aspx) コマンドレットを実行し、 [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) スナップショットを作成する方法です。 次に、例では、実行、 [Start-azurestorageblobcopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) コマンドレットでは、ソース blob に対して ICloudBlob オブジェクトを使用して、blob のスナップショットのコピーをします。 この例を実行する前に、必ず構成に基づいて変数を更新してください。 次の例では、ソースとコピー先のコンテナーとソース BLOB が既に存在していると想定しています。

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    
    #Define the variables.
    $SrcContainerName = "yoursourcecontainername"
    $DestContainerName = "yourdestcontainername"
    $SrcBlobName = "yourblobname"
    $DestBlobName = "CopyBlobName"
    
    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName
    
    #Create a snapshot of a blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()
    
    #Copy the snapshot to another container.
    Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName

これで、Azure PowerShell で Azure BLOB と BLOB スナップショットを管理する方法を確認できました。 次のセクションでは、テーブル、キュー、ファイルを管理する方法について確認します。

## Azure テーブルとテーブル エンティティを管理する方法

Azure Table ストレージ サービスは NoSQL データストアであり、これを使用することで、構造化された非リレーショナル データの巨大なセットを格納して照会できます。 このサービスのメイン コンポーネントは、テーブル、エンティティ、プロパティです。 テーブルは、エンティティのコレクションです。 エンティティは、プロパティのセットです。 各エンティティには、最大 252 個のプロパティを含むことができます。これらはすべて名前と値のペアです。 このセクションでは、Azure Table ストレージ サービスの概念について理解しているユーザーを対象としています。 詳細については、次を参照してください。 [テーブル サービス データ モデルについて](http://msdn.microsoft.com/library/azure/dd179338.aspx) と [.NET からテーブル ストレージを使用する方法](storage-dotnet-how-to-use-tables.md)します。

以下のサブセクションでは、Azure PowerShell を使用して Azure Table ストレージ サービスを管理する方法について説明します。 **テーブル**の**作成**、**削除**、**取得**、**テーブル エンティティの追加**、**照会**、**削除**の各シナリオについて説明します。

### テーブルの作成方法

各テーブルは、Azure ストレージ アカウント内に存在します。 次の例は、Azure Storage にテーブルを作成する方法を示しています。 この例では、まず、ストレージ アカウント コンテキストを使用して Azure Storage への接続を確立します。このコンテキストには、ストレージ アカウント名とそのアクセス キーが含まれます。 次に、使用して、 [New-azurestoragetable](http://msdn.microsoft.com/library/azure/dn806417.aspx) コマンドレットを Azure Storage にテーブルを作成します。

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey
    
    #Create a new table.
    $tabName = "yourtablename"
    New-AzureStorageTable –Name $tabName –Context $Ctx

### テーブルを取得する方法

あるストレージ アカウント内の 1 個またはすべてのテーブルを照会して取得できます。 次の例では、特定のテーブルを使用して、取得、 [Get-azurestoragetable](http://msdn.microsoft.com/library/azure/dn806411.aspx) コマンドレットです。

    #Retrieve a table.
    $tabName = "yourtablename"
    Get-AzureStorageTable –Name $tabName –Context $Ctx

パラメーターなしで Get-AzureStorageTable コマンドレットを呼び出した場合は、ストレージ アカウントのすべてのストレージ テーブルを取得します。

### テーブルを削除する方法

ストレージ アカウントからテーブルを削除するにを使用して、 [Remove-azurestoragetable](http://msdn.microsoft.com/library/azure/dn806393.aspx) コマンドレットです。

    #Delete a table.
    $tabName = "yourtablename"
    Remove-AzureStorageTable –Name $tabName –Context $Ctx

### テーブル エンティティを管理する方法

現在、Azure PowerShell では、テーブル エンティティを直接管理するコマンドレットは用意されていません。 指定されているクラスを使用するテーブル エンティティに対する操作を実行する、 [.NET 用 Azure ストレージ クライアント ライブラリ](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)します。

#### テーブル エンティティを追加する方法

テーブルにエンティティを追加するには、まず、エンティティのプロパティを定義するオブジェクトを作成します。 1 個のエンティティは、最大 255 個のプロパティを含むことができます。これには、**PartitionKey**、**RowKey**、**Timestamp** の 3 個のシステム プロパティも含まれます。 **PartitionKey** と **RowKey** の値を挿入または更新することはユーザーが担当します。 一方、サーバーは **Timestamp** の値を管理します。この値は変更できません。 **PartitionKey** と **RowKey** が組み合わさって、テーブル内の各エンティティを一意に識別します。

-   **PartitionKey**: エンティティが格納されるパーティションを決定します。
-   **RowKey**: パーティション内のエンティティを一意に識別します。

1 個のエンティティに対して最大 252 個のカスタム プロパティを定義できます。 詳細については、次を参照してください。 [テーブル サービス データ モデルについて](http://msdn.microsoft.com/library/azure/dd179338.aspx)します。

次の例は、エンティティをテーブルに追加する方法を示しています。 この例では、employee テーブルを取得して、そこに複数のエンティティを追加する方法を取り上げます。 まず、ストレージ アカウント コンテキストを使用して Azure Storage への接続を確立します。このコンテキストには、ストレージ アカウント名とそのアクセス キーが含まれます。 特定のテーブルを使用して、次に、取得、 [Get-azurestoragetable](http://msdn.microsoft.com/library/azure/dn806411.aspx) コマンドレットです。 テーブルが存在しない場合、 [New-azurestoragetable](http://msdn.microsoft.com/library/azure/dn806417.aspx) コマンドレットは Azure Storage にテーブルを作成するために使用します。 次に、各エンティティのパーティションと行キーを指定することでテーブルにエンティティを追加する、カスタム関数 Add-Entity が定義されます。 [エンティティ関数呼び出し、 [New-object](http://technet.microsoft.com/library/hh849885.aspx) コマンドレット、 [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) エンティティ オブジェクトを作成するクラス。 その後、 [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) テーブルに追加するには、このエンティティ オブジェクトのメソッドです。

    #Function Add-Entity: Adds an employee entity to a table.
    function Add-Entity() {
        [CmdletBinding()]
        param(
           $table,
           [String]$partitionKey,
           [String]$rowKey,
           [String]$name,
           [Int]$id
        )
    
      $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
      $entity.Properties.Add("Name", $name)
      $entity.Properties.Add("ID", $id)
    
      $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
    }
    
    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"
    
    #Retrieve the table if it already exists.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore
    
    #Create a new table if it does not exist.
    if ($table -eq $null)
    {
       $table = New-AzureStorageTable –Name $TableName -Context $Ctx
    }
    
    #Add multiple entities to a table.
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Steven -Id 4

#### テーブル エンティティを照会する方法

テーブルを照会する、 [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) クラスです。 次の例では、このガイドのエンティティの追加方法に関するセクションで指定されたスクリプトを実行済みであると想定しています。 この例では、まず、ストレージ コンテキストを使用して Azure Storage への接続を確立します。このコンテキストには、ストレージ アカウント名とそのアクセス キーが含まれます。 次に、以前に作成した"Employee"テーブルを使用して、取得を試みます、 [Get-azurestoragetable](http://msdn.microsoft.com/library/azure/dn806411.aspx) コマンドレットです。 呼び出す、 [New-object](http://technet.microsoft.com/library/hh849885.aspx) Microsoft.WindowsAzure.Storage.Table.TableQuery クラスでのコマンドレットは、新しいクエリ オブジェクトを作成します。 そして、文字列フィルターで指定されたとおりの 1 を値とする "ID" 列があるエンティティを見つけます。 詳細については、次を参照してください。 [クエリを実行するテーブルとエンティティ](http://msdn.microsoft.com/library/azure/dd894031.aspx)します。 このクエリを実行すると、フィルター条件に一致するすべてのエンティティが返されます。

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary;
    $TableName = "Employees"
    
    #Get a reference to a table.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx
    
    #Create a table query.
    $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery
    
    #Define columns to select.
    $list = New-Object System.Collections.Generic.List[string]
    $list.Add("RowKey")
    $list.Add("ID")
    $list.Add("Name")
    
    #Set query details.
    $query.FilterString = "ID gt 0"
    $query.SelectColumns = $list
    $query.TakeCount = 20
    
    #Execute the query.
    $entities = $table.CloudTable.ExecuteQuery($query)
    
    #Display entity properties with the table format.
    $entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties[“ID”].Int32Value}} -AutoSize

#### テーブル エンティティを削除する方法

パーティション キーと行キーを使用してエンティティを削除できます。 次の例では、このガイドのエンティティの追加方法に関するセクションで指定されたスクリプトを実行済みであると想定しています。 この例では、まず、ストレージ コンテキストを使用して Azure Storage への接続を確立します。このコンテキストには、ストレージ アカウント名とそのアクセス キーが含まれます。 次に、以前に作成した"Employee"テーブルを使用して、取得を試みます、 [Get-azurestoragetable](http://msdn.microsoft.com/library/azure/dn806411.aspx) コマンドレットです。 例では、テーブルが存在する場合、 [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) 、パーティションと行キー値に基づいてエンティティを取得します。 次に、エンティティを渡す、  [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) メソッドを削除します。

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    
    #Retrieve the table.
    $TableName = "Employees"
    $table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore
    
    #If the table exists, start deleting its entities.
    if ($table -ne $null) {
       #Together the PartitionKey and RowKey uniquely identify every  
       #entity within a table.
       $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve(“Partition2”, "Row1"))
       $entity = $tableResult.Result;
    if ($entity -ne $null)
    {
       #Delete the entity.$table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
    }

## Azure キューとキュー メッセージを管理する方法

Azure Queue ストレージは、HTTP または HTTPS を使用した認証された呼び出しを介して世界中のどこからでもアクセスできる大量のメッセージを格納するためのサービスです。 このセクションでは、Azure Queue ストレージ サービスの概念について理解しているユーザーを対象としています。 詳細については、次を参照してください。 [.NET からキュー ストレージを使用する方法](storage-dotnet-how-to-use-queues.md)します。

このセクションでは、Azure PowerShell を使用して Azure Queue ストレージ サービスを管理する方法を取り上げます。 キュー メッセージの**挿入**と**削除**、および**キューの作成**、**削除**、および**取得**の各シナリオについて説明します。

### キューの作成方法

次の例では、まず、ストレージ アカウント コンテキストを使用して Azure Storage への接続を確立します。このコンテキストには、ストレージ アカウント名とそのアクセス キーが含まれます。 次に、 [New-azurestoragequeue](http://msdn.microsoft.com/library/azure/dn806382.aspx) コマンドレットを"queuename"という名前のキューを作成します。

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $QueueName = "queuename"
    $Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx

Azure キュー サービスの名前付け規則については、次を参照してください。 [名前付けのキューおよびメタデータの](http://msdn.microsoft.com/library/azure/dd179349.aspx)します。

### キューを取得する方法

あるストレージ アカウント内の特定のキューまたはすべてのキューの一覧を照会して取得できます。 次の例では、指定されたキューを使用して、取得、 [Get-azurestoragequeue](http://msdn.microsoft.com/library/azure/dn806377.aspx) コマンドレットです。

    #Retrieve a queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx

呼び出した場合、 [Get-azurestoragequeue](http://msdn.microsoft.com/library/azure/dn806377.aspx) コマンドレット パラメーターを指定せず、すべてのキューの一覧を取得しています。

### キューを削除する方法

キューとキューに含まれるすべてのメッセージを削除するには、Remove-AzureStorageQueue コマンドレットを呼び出します。 次の例は、Remove-AzureStorageQueue コマンドレットを使用して特定のキューを削除する方法を示しています。

    #Delete a queue.
    $QueueName = "yourqueuename"
    Remove-AzureStorageQueue –Name $QueueName –Context $Ctx

#### メッセージをキューに挿入する方法

既存のキューにメッセージを挿入する最初の新しいインスタンスを作成、 [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) クラスです。 次を呼び出す、 [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) メソッドです。 CloudQueueMessage は、文字列 (UTF-8 形式) またはバイト配列から作成できます。

次の例は、メッセージをキューに追加する方法を示しています。 この例では、まず、ストレージ アカウント コンテキストを使用して Azure Storage への接続を確立します。このコンテキストには、ストレージ アカウント名とそのアクセス キーが含まれます。 指定したキューを使用して、次に、取得、 [Get-azurestoragequeue](https://msdn.microsoft.com/library/azure/dn806377.aspx) コマンドレットです。 キューが存在する場合、 [New-object](http://technet.microsoft.com/library/hh849885.aspx) コマンドレットの使用のインスタンスを作成、 [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) クラスです。 その後、 [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) をキューに追加するには、このメッセージ オブジェクトのメソッドです。 次のコードでは、キューを取得し、メッセージ "MessageInfo" を挿入します。

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    
    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx
    
    #If the queue exists, add a new message.
    if ($Queue -ne $null) {
       # Create a new message using a constructor of the CloudQueueMessage class.
       $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo
    
       #Add a new message to the queue.
       $Queue.CloudQueue.AddMessage($QueueMessage)
    }

#### 次のメッセージでデキューする方法

コードでは、2 つの手順でキューからメッセージをデキューします。 呼び出すと、 [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) メソッドでは、キュー内に次のメッセージを取得します。 **GetMessage** から返されたメッセージは、このキューからメッセージを読み取る他のコードから参照できなくなります。 キューからのメッセージの削除を完了するにも呼び出す必要があります、 [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) メソッドです。 このようにメッセージを 2 つの手順で削除することで、ハードウェアまたはソフトウェアの問題が原因でコードによるメッセージの処理が失敗した場合に、コードの別のインスタンスで同じメッセージを取得し、もう一度処理することができます。 コードでは、メッセージが処理された直後に **DeleteMessage** を呼び出します。

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    
    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx
    
    $InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)
    
    #Get the message object from the queue.
    $QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
    #Delete the message.
    $Queue.CloudQueue.DeleteMessage($QueueMessage)

## Azure ファイル共有とファイルを管理する方法

Azure File ストレージは、標準的な SMB プロトコルを使用して、アプリケーション用の共有ストレージを提供します。 Microsoft Azure の仮想マシンとクラウド サービスでは、マウントされている共有を介して、アプリケーション コンポーネント間でファイル データを共有できます。オンプレミスのアプリケーションでは、File ストレージ API または Azure PowerShell を介して、共有内のファイル データにアクセスできます。

Azure File ストレージの詳細については、次を参照してください。 [Windows で Azure File ストレージを使用する方法](storage-dotnet-how-to-use-files.md) と [ファイル サービス REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx)します。

## ストレージ分析を設定して照会する方法

使用する [Azure Storage Analytics](storage-analytics.md) 、Azure ストレージ アカウントのメトリックを収集し、ストレージ アカウントに送信される要求に関するデータを記録します。 ストレージ メトリックを使用すると、ストレージ アカウントの正常性を監視でき、ストレージ ログを使用すると、ストレージ アカウントに関する問題の診断とトラブルシューティングができます。
既定では、Storage サービスに対してストレージ メトリックは有効になっていません。 Azure ポータルまたは Windows PowerShell を使用して監視を有効にできます。また、ストレージ クライアント ライブラリを使用したプログラムで監視を有効にすることもできます。 ストレージ ログはサーバー側で発生し、ストレージ アカウント内の成功した要求と失敗した要求の両方について詳細を記録することが可能になります。 これらのログを使用すると、読み取り、書き込み、削除の各操作の詳細、失敗した要求の原因を確認できます。

有効にし、PowerShell を使用してストレージ メトリック データを表示する方法については、次を参照してください。 [PowerShell を使用してストレージ メトリックを有効にする方法](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell)します。

PowerShell を使用し、ストレージ ログのデータを有効にして取得する方法の詳細については、
[PowerShell を使用してストレージ ログを有効にする方法](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) と [Storage Logging ログ データの検索](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata)します。
ストレージ メトリックとストレージ ログを使用して、記憶域に関する問題のトラブルシューティングの詳細については、次を参照してください。 [監視、診断、および Microsoft Azure Storage のトラブルシューティング](storage-monitoring-diagnosing-troubleshooting.md)します。

## Shared Access Signature (SAS) や格納されているアクセス ポリシーの選択を管理する方法

共有アクセス署名は、Azure Storage を使用するあらゆるアプリケーションのセキュリティ モデルの重要な部分となります。 アカウント キーを知らせずに、ストレージ アカウントへの制限付きアクセス許可をクライアントに付与する場合に便利です。 既定では、ストレージ アカウントの所有者のみがそのアカウントを使って BLOB、テーブル、キューなどにアクセスできます。 サービスやアプリケーションで、アクセス キーを共有せずに他のクライアントでそれらのリソースを使えるようにするには、次の 3 つの方法があります。

- 匿名ユーザーにコンテナーや BLOB への読み込みアクセスを許可するようにコンテナーのアクセス許可を設定します。 これはテーブルやキューでは許可されません。
- 共有アクセス署名を使用して、特定の期間のコンテナー、BLOB、キュー、テーブルへの制限付きアクセス権を付与します。
- 保存されたアクセス ポリシーを使用して、コンテナー、BLOB、キュー、テーブルに対する共有アクセス署名の制御を追加します。 保存されたアクセス ポリシーによって、開始時刻、有効期限、署名の許可の変更や、署名が発行された後の取り消しが行えるようになります。

共有アクセス署名の形式は、次の 2 つのいずれかです。

- **アドホック SAS**: アドホック SAS を作成すると、開始時刻、有効期限、SAS へのアクセス許可がすべて、SAS URI で指定されます。 この種類の SAS は、コンテナー、BLOB、テーブル、キューで作成できます。これは取り消しできません。
- **保存されているアクセス ポリシーのある SAS:** 保存されているアクセス ポリシーは、リソース コンテナー (BLOB コンテナー、テーブル、またはキュー) で定義されており、これを使用して、1 つ以上の共有アクセス署名のコンテナーを管理できます。 保存されているアクセス ポリシーに SAS を関連付けると、SAS は、保存されているアクセス ポリシーに定義されている制約 (開始時刻、有効期限、およびアクセス許可) を継承します。 この種類の SAS は、取り消しできません。

詳細については、次を参照してください。 [共有アクセス署名のチュートリアル](storage-dotnet-shared-access-signature-part-1.md) と [Azure ストレージ リソースへのアクセスの管理](storage-manage-access-to-resources.md)します。

次のセクションでは、Azure テーブルに対する共有アクセス署名トークンと保存されたアクセス ポリシーを作成する方法を学習します。Azure PowerShell では、コンテナー、BLOB、キューに対して類似するコマンドレットが提供されます。このセクションで、スクリプトを実行するには、ダウンロード、 [Azure PowerShell version 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) またはそれ以降。

### ポリシー ベースの Shared Access Signature トークンを作成する方法

New-AzureStorageTableStoredAccessPolicy コマンドレットを使用して新しい保存アクセス ポリシーを作成します。 次を呼び出す、 [New-azurestoragetablesastoken](http://msdn.microsoft.com/library/azure/dn806400.aspx) コマンドレットを Azure ストレージ テーブルに対する新しいポリシー ベースの共有アクセス署名トークンを作成します。

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
    New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx

### アドホック (取り消し不可) の Shared Access Signature トークンを作成する方法

使用して、 [New-azurestoragetablesastoken](http://msdn.microsoft.com/library/azure/dn806400.aspx) 新しいアドホック (取り消し不可) 共有アクセス署名トークンを作成する Azure ストレージ テーブルに対するコマンドレット。

    New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx

### 保存されているアクセス ポリシーを作成する方法

次のように New-AzureStorageTableStoredAccessPolicy コマンドレットを使用して、Azure ストレージ テーブルの新しく保存されているアクセス ポリシーを作成します。

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx

### 保存されているアクセス ポリシーを更新する方法

次のように Set-AzureStorageTableStoredAccessPolicy コマンドレットを使用して、Azure ストレージ テーブルの既存の保存されているアクセス ポリシーを更新します。

    Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx

### 保存されているアクセス ポリシーを削除する方法

次のように Remove-AzureStorageTableStoredAccessPolicy コマンドレットを使用して、Azure ストレージ テーブルの保存されているアクセス ポリシーを削除します。

    Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx

## 米国政府対応の Azure Storage や Azure China を使用する方法

Azure 環境などでは、Microsoft Azure の展開が独立した [米国政府の Azure Government](http://azure.microsoft.com/features/gov/), 、[グローバル Azure の AzureCloud](https://manage.windowsazure.com), 、および [中国の 21 vianet が運営している Azure の AzureChinaCloud](http://www.windowsazure.cn/)します。 米国政府対応の新しい Azure 環境や Azure China をデプロイできます。

AzureChinaCloud で Azure Storage を使用するには、AzureChinaCloud に関連付けられたストレージ コンテキストを作成する必要があります。 次の手順に従って開始します。

1.  実行、 [Get-azureenvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) コマンドレットを使用できる Azure 環境を参照してください。

    `Get-azureenvironment`

2.  Azure China のアカウントを Windows PowerShell に追加します。

    `Add-azureaccount-環境 AzureChinaCloud`

3.  AzureChinaCloud アカウントのストレージ コンテキストを作成します。

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud


Azure ストレージを使用する [米国Azure Government](http://azure.microsoft.com/features/gov/), 、新しい環境を定義し、その環境で新しいストレージ コンテキストを作成する必要があります。

1. 呼び出す、 [Add-azureenvironment](http://msdn.microsoft.com/library/azure/dn790364.aspx) コマンドレットをプライベート データ センターの場合は、新しい Azure 環境を作成します。

        Add-AzureEnvironment -Name $EnvironmentName -PublishSettingsFileUrl $publishSettingsFileUrl -ServiceEndpoint $serviceEndpoint -ManagementPortalUrl $managementPortalUrl -StorageEndpoint $storageEndpoint -ActiveDirectoryEndpoint $activeDirectoryEndpoint -ResourceManagerEndpoint $resourceManagerEndpoint -GalleryEndpoint $galleryEndpoint -ActiveDirectoryServiceEndpointResourceId $activeDirectoryServiceEndpointResourceId -GraphEndpoint $graphEndpoint -SubscriptionDataFile $subscriptionDataFile

2. 実行、 [New-azurestoragecontext](http://msdn.microsoft.com/library/azure/dn806380.aspx) コマンドレットを次に示すようにこの新しい環境の新しいストレージ コンテキストを作成します。

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment $EnvironmentName


詳細については、次を参照してください。

- [Microsoft Azure Government 開発者向けガイド](../azure-government-developer-guide.md)します。
- [グローバル Azure の AzureCloud と中国の 21 vianet が運営する Azure の AzureChinaCloud の相違点](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## 次のステップ

このガイドでは、Azure PowerShell を使用して Azure Storage を管理する方法を確認しました。 詳細についての関連記事とリソースがあります。

- [Azure Storage のドキュメント](http://azure.microsoft.com/documentation/services/storage/)
- [Azure Storage の PowerShell コマンドレット](http://msdn.microsoft.com/library/azure/dn806401.aspx)
- [Windows PowerShell リファレンス](https://msdn.microsoft.com/library/ms714469.aspx)


[image1]: ./media/storage-powershell-guide-full/Subscription_currentportal.png 
[image2]: ./media/storage-powershell-guide-full/Subscription_Previewportal.png 
[image3]: ./media/storage-powershell-guide-full/Blobdownload.png 
[getting started with azure storage and powershell in 5 minutes]: #getstart 
[prerequisites for using azure powershell with azure storage]: #pre 
[how to manage storage accounts in azure]: #manageaccount 
[how to set a default azure subscription]: #setdefsub 
[how to create a new azure storage account]: #createaccount 
[how to set a default azure storage account]: #defaultaccount 
[how to list all azure storage accounts in a subscription]: #listaccounts 
[how to create an azure storage context]: #createctx 
[how to manage azure blobs and blob snapshots]: #manageblobs 
[how to create a container]: #container 
[how to upload a blob into a container]: #uploadblob 
[how to download blobs from a container]: #downblob 
[how to copy blobs from one storage container to another]: #copyblob 
[how to delete a blob]: #deleteblob 
[how to manage azure blob snapshots]: #manageshots 
[how to create a blob snapshot]: #createshot 
[how to list snapshots of a blob]: #listshot 
[how to copy a snapshot of a blob]: #copyshot 
[how to manage azure tables and table entities]: #managetables 
[how to create a table]: #createtable 
[how to retrieve a table]: #gettable 
[how to delete a table]: #remtable 
[how to manage table entities]: #mngentity 
[how to add table entities]: #addentity 
[how to query table entities]: #queryentity 
[how to delete table entities]: #deleteentity 
[how to manage azure queues and queue messages]: #managequeues 
[how to create a queue]: #createqueue 
[how to retrieve a queue]: #getqueue 
[how to delete a queue]: #remqueue 
[how to manage queue messages]: #mngqueuemsg 
[how to insert a message into a queue]: #addqueuemsg 
[how to de-queue at the next message]: #dequeuemsg 
[how to manage azure file shares and files]: #files 
[how to set and query storage analytics]: #stganalytics 
[how to manage shared access signature (sas) and stored access policy]: #sas 
[how to use azure storage for u.s. government and azure china]: #gov 
[next steps]: #next 

