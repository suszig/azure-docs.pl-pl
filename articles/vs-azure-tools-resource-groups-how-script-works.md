<properties
    pageTitle="Azure リソース グループ プロジェクトの配置スクリプトの概要 |Microsoft Azure"
    description="Azure リソース グループのデプロイメント プロジェクトの PowerShell スクリプトの動作について説明します。"
    services="visual-studio-online"
    documentationCenter="na"
    authors="kempb"
    manager="douge"
    editor="tlee" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="11/17/2015"
    ms.author="kempb" />

# Azure リソース グループ プロジェクトのデプロイメント スクリプトの概要

Azure リソース グループのデプロイメント プロジェクトは、ファイルとその他のアーティファクトを Azure にステージングおよびデプロイする作業を支援します。 Visual Studio で Azure リソース マネージャーの展開プロジェクトを作成するときに、PowerShell スクリプトと呼ばれる **Deploy-azureresourcegroup.ps1** プロジェクトに追加します。 このトピックでは、このスクリプトで実行される操作と、Visual Studio の内部と外部の両方でのスクリプトの実行方法について説明します。

## このスクリプトで実行される操作

Deploy-AzureResourceGroup.ps1 スクリプトは、デプロイメント ワークフローで重要な次の 2 つの操作を実行します。

- テンプレートのデプロイメントに必要なファイルまたはアーティファクトのアップロード
- テンプレートのデプロイ

スクリプトの最初の部分では、デプロイメント用のファイルとアーティファクトをアップロードし、スクリプト内の最後のコマンドレットで実際にテンプレートをデプロイします。 たとえば、スクリプトを使用して仮想マシンを構成する必要がある場合、デプロイメント スクリプトは最初に構成スクリプトを Azure ストレージ アカウントへセキュリティで保護された方法でアップロードします。 これにより、プロビジョニング中に仮想マシンを構成するために Azure リソース マネージャーで構成スクリプトを使用できるようになります。

すべてのテンプレートのデプロイが余分な成果物は、アップロードする必要が必要があるため、スイッチ パラメーターと呼ばれる *uploadArtifacts* が評価されます。 そのようなアイテムは、アップロードする場合に、設定、 *uploadArtifacts* のスクリプトの呼び出し時に切り替えます。 メインのテンプレート ファイルとパラメーター ファイルはアップロードする必要がないことに注意してください。 構成スクリプト、ネストしたデプロイメント テンプレート、およびアプリケーション ファイルなど、その他のファイルのみをアップロードする必要があります。

## スクリプトの詳細な説明

Azure PowerShell スクリプト Deploy-AzureResourceGroup.ps1 の特定のセクションでの実行内容について、次に説明します。

>[AZURE.NOTE] これには、バージョン 1.0 の Deploy-azureresourcegroup.ps1 スクリプトが説明します。

1.  Azure リソース マネージャーのデプロイメント プロジェクトに必要なパラメーターを宣言します。 いくつかのパラメーターには、プロジェクトの作成時に設定された既定値があります。 スクリプト内でこれらの既定値を変更したり、スクリプトを実行する前に異なるパラメーター値を追加したりすることができます。

    ```
    Param(
      [string] [Parameter(Mandatory=$true)] $ResourceGroupLocation,
      [string] $ResourceGroupName = 'AzureResourceGroup1',
      [switch] $UploadArtifacts,
      [string] $StorageAccountName,
      [string] $StorageAccountResourceGroupName,
      [string] $StorageContainerName = $ResourceGroupName.ToLowerInvariant() + '-stageartifacts',
      [string] $TemplateFile = '..\Templates\azuredeploy.json',
      [string] $TemplateParametersFile = '..\Templates\azuredeploy.parameters.json',
      [string] $ArtifactStagingDirectory = '..\bin\Debug\staging',
      [string] $AzCopyPath = '..\Tools\AzCopy.exe',
      [string] $DSCSourceFolder = '..\DSC'
    )
    ```

    |パラメーター|説明|
    |---|---|
    |$ResourceGroupLocation|領域またはデータ センターの場所、リソース グループなど、 **米国西部** または **東アジア**します。|
    |$ResourceGroupName|Azure リソース グループの名前。|
    |$UploadArtifacts|アーティファクトをシステムから Azure にアップロードする必要があるかどうかを示すバイナリ値。|
    |$StorageAccountName|アーティファクトがアップロードされる Azure ストレージ アカウントの名前。|
    |$StorageAccountResourceGroupName|ストレージ アカウントを含む Azure リソース グループの名前。|
    |$StorageContainerName|アーティファクトをアップロードするために使用するストレージ コンテナーの名前。|
    |$TemplateFile|Azure リソース グループ プロジェクト内のデプロイメント ファイル (`<app name>.json`) へのパス。|
    |$TemplateParametersFile|Azure リソース グループ プロジェクト内のパラメーター ファイル (`<app name>.parameters.json`) へのパス。|
    |$ArtifactStagingDirectory|PowerShell スクリプトのルート フォルダーを含む、アーティファクトがローカルにアップロードされているシステム上のパス。 このパスには、絶対パス、またはスクリプトの場所に対する相対パスを指定できます。|
    |$AzCopyPath|PowerShell スクリプトのルート フォルダーを含む、AzCopy.exe ツールがその .zip ファイルをコピーするパス。 このパスには、絶対パス、またはスクリプトの場所に対する相対パスを指定できます。|
    |$DSCSourceFolder|PowerShell スクリプトのルート フォルダーを含む、DSC (Desired State Configuration) ソース フォルダーへのパス。 このパスには、絶対パス、またはスクリプトの場所に対する相対パスを指定できます。 参照してください [Azure PowerShell DSC (Desired State Configuration) 拡張機能の概要](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx), 詳細については、適用可能な場合です。|

1.  アーティファクトを Azure にアップロードする必要があるかどうかを確認します。 必要ない場合は、手順 11 に進みます。 それ以外の場合は、次の手順を実行します。

1.  相対パスを含むすべての変数を絶対パスに変換します。 たとえば、`..\Tools\AzCopy.exe` のようなパスを `C:\YourFolder\Tools\AzCopy.exe` に変更します。 また、変数を初期化 *ArtifactsLocationName* と *ArtifactsLocationSasTokenName* を null にします。 *ArtifactsLocation* と *SaSToken* テンプレート パラメーターである可能性があります。 パラメーター ファイル内を読み取った後にそれらの値が null の場合、スクリプトは、それらの値を生成します。

    Azure ツールは、パラメーター値を使用して *_artifactsLocation* と *_artifactsLocationSasToken* アイテムを管理するテンプレートです。 PowerShell スクリプトが、これらの名前を持つパラメーターを見つけて、パラメーターの値が指定されていない場合、スクリプトはアーティファクトをアップロードして、これらのパラメーターの適切な値を返します。 その後、`@OptionsParameters` を通じてコマンドレットにそれらの値を渡します。

    |変数|説明|
    |---|---|
    |ArtifactsLocationName|Azure アーティファクトが配置されている場所のパス。|
    |ArtifactsLocationSasTokenName|Service Bus を認証するためにスクリプトによって使用される SAS (Shared Access Signature) トークン名。 参照してください [Service Bus での共有アクセス署名認証](service-bus-shared-access-signature-authentication.md) の詳細。|

    ```
    if ($UploadArtifacts) {
    # Convert relative paths to absolute paths if needed
    $AzCopyPath = [System.IO.Path]::Combine($PSScriptRoot, $AzCopyPath)
    $ArtifactStagingDirectory = [System.IO.Path]::Combine($PSScriptRoot, $ArtifactStagingDirectory)
    $DSCSourceFolder = [System.IO.Path]::Combine($PSScriptRoot, $DSCSourceFolder)

    Set-Variable ArtifactsLocationName '_artifactsLocation' -Option ReadOnly
    Set-Variable ArtifactsLocationSasTokenName '_artifactsLocationSasToken' -Option ReadOnly

    $OptionalParameters.Add($ArtifactsLocationName, $null)
    $OptionalParameters.Add($ArtifactsLocationSasTokenName, $null)
    ```

1.  このセクションのチェックをするかどうか、 <app name>. parameters.json ファイル (「パラメーター ファイル」と呼ばれる) という名前の親ノードを持つ **パラメーター** (で、 `else` ブロック) します。 それ以外の場合、親ノードはありません。 いずれの形式も受け入れられます。
    
    ```
    if ($JsonParameters -eq $null) {
            $JsonParameters = $JsonContent
        }
        else {
            $JsonParameters = $JsonContent.parameters
        }
    ```

1.  JSON パラメーターのコレクションを反復処理します。 パラメーター値を割り当てた場合 *_artifactsLocation* または *_artifactsLocationSasToken*, 、変数を設定 *$OptionalParameters* それら値を使用します。 これにより、指定されたパラメーター値をスクリプトが誤って上書きすることを防止できます。

    ```
    $JsonParameters | Get-Member -Type NoteProperty | ForEach-Object {
        $ParameterValue = $JsonParameters | Select-Object -ExpandProperty $_.Name

        if ($_.Name -eq $ArtifactsLocationName -or $_.Name -eq $ArtifactsLocationSasTokenName) {
            $OptionalParameters[$_.Name] = $ParameterValue.value
        }
    }
    ```

1.  デプロイメント用のアーティファクトを保持するために使用されるストレージ アカウント リソースのストレージ アカウント キーとコンテキストを取得します。

    ```
    $StorageAccountKey = (Get-AzureRMStorageAccountKey -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Key1

    $StorageAccountContext = (Get-AzureRmStorageAccount -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Context
    ```

1.  仮想マシンを構成するために PowerShell DSC を使用している場合、DSC 拡張機能では、アーティファクトが 1 つの zip ファイル内に含まれている必要があります。 そのため、DSC 構成用の .zip アーカイブ ファイルを作成します。 これを行うには、$DSCSourceFolder が存在するかどうかを確認します。 DSC 構成が存在する場合は、それを削除して、dsc.zip という名前の新しい圧縮ファイルを作成します。

    ```
    # Create DSC configuration archive
    if (Test-Path $DSCSourceFolder) {
    Add-Type -Assembly System.IO.Compression.FileSystem
        $ArchiveFile = Join-Path $ArtifactStagingDirectory "dsc.zip"
        Remove-Item -Path $ArchiveFile -ErrorAction SilentlyContinue
        [System.IO.Compression.ZipFile]::CreateFromDirectory($DSCSourceFolder, $ArchiveFile)
    }
    ```

1.  Azure アーティファクトのパスがパラメーター ファイルに指定されていない場合は、アーティファクトをアップロードするときに使用する PowerShell スクリプトのパスを設定します。 これを行うには、ストレージ アカウントのエンドポイント パスとストレージ コンテナー名の組み合わせを使用して、パスを作成します。 次に、この新しいパスでパラメーター ファイルを更新します。

    ```
    # Generate the value for artifacts location if it is not provided in the parameter file
    $ArtifactsLocation = $OptionalParameters[$ArtifactsLocationName]
    if ($ArtifactsLocation -eq $null) {
        $ArtifactsLocation = $StorageAccountContext.BlobEndPoint + $StorageContainerName
        $OptionalParameters[$ArtifactsLocationName] = $ArtifactsLocation
    }
    ```

1.  使用して、 **AzCopy** ユーティリティ (に含まれる、 **ツール** 、Azure リソース グループ デプロイ プロジェクトのフォルダー)、ローカル ストレージのドロップ パスから、オンラインの Azure ストレージ アカウントにすべてのファイルをコピーします。 この手順に失敗した場合、必要なアーティファクトがないとデプロイメントは成功しないため、スクリプトを終了します。

    ```
    # Use AzCopy to copy files from the local storage drop path to the storage account container
    & $AzCopyPath """$ArtifactStagingDirectory""", $ArtifactsLocation, "/DestKey:$StorageAccountKey", "/S", "/Y", "/Z:$env:LocalAppData\Microsoft\Azure\AzCopy\$ResourceGroupName"
    if ($LASTEXITCODE -ne 0) { return }
    ```

1.  アーティファクトの場所に対する SAS トークンがパラメーター ファイルに指定されていない場合は、その値を作成して、オンライン ストレージ コンテナーへの一時的な読み取り専用アクセスを提供します。 次に、その SAS トークンを "optionalParameter" としてコマンドラインに渡します。 コマンドラインで渡されるパラメーターは、パラメーター ファイルに指定された値より優先されることに注意してください。

    ```
    # Generate the value for artifacts location SAS token if it is not provided in the parameter file
    $ArtifactsLocationSasToken = $OptionalParameters[$ArtifactsLocationSasTokenName]
    if ($ArtifactsLocationSasToken -eq $null) {
       # Create a SAS token for the storage container - this gives temporary read-only access to the container
       $ArtifactsLocationSasToken = New-AzureStorageContainerSASToken -Container $StorageContainerName -Context $StorageAccountContext -Permission r -ExpiryTime (Get-Date).AddHours(4)
       $ArtifactsLocationSasToken = ConvertTo-SecureString $ArtifactsLocationSasToken -AsPlainText -Force
       $OptionalParameters[$ArtifactsLocationSasTokenName] = $ArtifactsLocationSasToken
    }
    ```

1.  リソース グループがまだ存在していない場合は作成し、デプロイメントの成功を妨げる検証エラーがないか、テンプレートおよびパラメーター ファイルを確認します。

    ```
    # Create or update the resource group using the specified template file and template parameters file
    New-AzureRMResourceGroup -Name $ResourceGroupName -Location $ResourceGroupLocation -Verbose -Force -ErrorAction Stop

    Test-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterFile $TemplateParametersFile @OptionalParameters -ErrorAction Stop
    ```

1. 最後に、テンプレートをデプロイします。 このコードでは、タイムスタンプを使用してデプロイメントの一意の名前が作成されます。

    ```
    New-AzureRMResourceGroupDeployment -Name ((Get-ChildItem $TemplateFile).BaseName + '-' + ((Get-Date).ToUniversalTime()).ToString('MMdd-HHmm')) `
        -ResourceGroupName $ResourceGroupName `
        -TemplateFile $TemplateFile `
        -TemplateParameterFile $TemplateParametersFile `
        @OptionalParameters `
        -Force -Verbose
    ```

## リソース グループのデプロイ

### Visual Studio でリソース グループをデプロイするには

1. Azure リソース グループ プロジェクトのショートカット メニューで **展開** > **新しい展開**します。

    ![][0]

1.  **リソース グループにデプロイ** ] ダイアログ ボックスを選択するか、既存のリソース グループに配置を選択するドロップダウン リスト ボックスに **& lt;Gt; を新規作成する (& r)** 新しいリソース グループを作成します。

    ![][1]

1. 求められた場合は、リソース グループ名と場所を入力してください。、 **リソース グループの作成** ] ダイアログ ボックスを選択し、、 **作成** ] ボタンをクリックします。

    ![][2]

1. 選択、 **パラメーターの編集** を表示するボタン、 **パラメーターの編集** ] ダイアログ ボックスし、不足しているパラメーターの値を入力します。

    ![][3]

    >[AZURE.NOTE] 必要なパラメーターに値が必要がある場合このダイアログ ボックスに自動的に、展開するときに表示されます。

    ![][4]

1. 終了したらパラメーターの値を入力して、選択、 **保存** ボタンをクリックし、選択、 **展開** ] ボタンをクリックします。

    デプロイメント スクリプト (Deploy-AzureResourceGroup.ps1) が実行され、テンプレートがすべてのアーティファクトと共に Azure にデプロイされます。

### PowerShell を使用してリソース グループをデプロイするには

Visual Studio の配置コマンドを使用せず、スクリプトを実行するし、UI、スクリプトは、ショートカット メニューで選択 **PowerShell ISE で開く**します。

![][5]


## コマンド デプロイメントの例

### 既定値を使用したデプロイ

この例では、既定のパラメーター値を使用してスクリプトを実行する方法を示します。 (場所のパラメーターには既定値がないため、明示的に指定する必要があります。)

`.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus`

### 既定値をオーバーライドするデプロイ

この例では、既定値とは異なるテンプレートおよびパラメーター ファイルをデプロイするスクリプトの実行方法を示します。

```
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus –TemplateFile ..\templates\AnotherTemplate.json –TemplateParametersFile ..\templates\AnotherTemplate.parameters.json
```

### ステージング用の UploadArtifacts を使用したデプロイ

この例では、リリース フォルダーからアーティファクトをアップロードして、既定以外のテンプレートをデプロイするスクリプトの実行方法を示します。

```
.\Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory ..\bin\release\staging
```

この例では、Visual Studio Online の Azure PowerShell タスク内のスクリプトを実行する方法を示します。

```
$(Build.StagingDirectory)/AzureResourceGroup1/Scripts/Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory $(Build.StagingDirectory)
```

## 次のステップ
詳細については、Azure リソース マネージャーを参照して [Azure リソース マネージャーの概要](resource-group-overview.md)します。

[0]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy1c.png
[1]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy2bc.png
[2]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy3bc.png
[3]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy4bc.png
[4]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy5c.png
[5]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy6c.png
