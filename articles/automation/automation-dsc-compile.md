<properties 
   pageTitle="Azure Automation DSC での構成のコンパイル | Microsoft Azure" 
   description="Desired State Configuration (DSC) 構成をコンパイルする 2 つの方法 (Azure ポータルで行う方法と Windows PowerShell を使用する方法) の概要。 " 
   services="automation" 
   documentationCenter="na" 
   authors="coreyp-at-msft" 
   manager="stevenka" 
   editor="tysonn"/>

<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na" 
   ms.date="10/15/2015"
   ms.author="coreyp"/>
   
#Azure Automation DSC での構成のコンパイル#

Azure Automation を使用して、Desired State Configuration (DSC) 構成をコンパイルする方法は、2 とおりあります。Azure ポータルで行う方法と、Windows PowerShell を使用する方法です。 次の表は、各方法の特徴を確認し、どちらの方法をどのような場合に使用するかを判断する際に役立ちます。 

###Azure プレビュー ポータル###
- 対話型ユーザー インターフェイスを使用する、最も簡単な方法
- フォームに単純なパラメーター値を入力する
- ジョブの状態を簡単に追跡できる
- Azure ログオンを使用してアクセスを認証する

###Windows PowerShell###
- Windows PowerShell コマンドレットを使用してコマンド ラインから呼び出す
- 複数のステップで自動化されたソリューションに含めることができる
- 単純なパラメーター値と複雑なパラメーター値を指定する
- ジョブの状態を追跡できる
- PowerShell コマンドレットをサポートするクライアントが必要
- ConfigurationData を渡す
- 資格情報を使用する構成をコンパイルする

コンパイルの方法を決定したら、以下の各手順に従い、コンパイルを開始できます。

##Azure プレビュー ポータルを使用した DSC 構成のコンパイル##

1.  オートメーション アカウントからクリックして **構成**します。
2.  構成をクリックし、ブレードを開きます。
3.  クリックして **コンパイル**します。
4.  構成にパラメーターが含まれていない場合、コンパイルの実行を確認するメッセージが表示されます。 構成にパラメーターがある場合、 **コンパイル構成** ブレードが開き、パラメーター値を指定できます。 参照してください、 <a href="#basic-parameters">**基本パラメーター**</a> パラメーターの詳細については後述します。
5.   **コンパイル ジョブ** ブレードも開くので、コンパイル ジョブの状態、および Azure Automation DSC プル サーバーに配置される原因とそのノードの構成 (MOF の構成ドキュメント) を追跡することができます。

##Windows PowerShell を使用した DSC 構成のコンパイル##

使用する [`Start-AzureRmAutomationDscCompilationJob`](https://msdn.microsoft.com/library/mt244118.aspx) を Windows PowerShell でのコンパイルを開始します。 次のサンプル コードと呼ばれる DSC 構成のコンパイルを開始する **SampleConfig**します。

    Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig" 
 
`Start-AzureRmAutomationDscCompilationJob` は、ジョブの状態を追跡するのに使用できるコンパイル ジョブ オブジェクトを返します。 このコンパイル ジョブ オブジェクトを使用することができますし、 [`Get-AzureRmAutomationDscCompilationJob`](https://msdn.microsoft.com/library/mt244120.aspx) コンパイル ジョブの状態を判断して [`Get-AzureRmAutomationDscCompilationJobOutput`](https://msdn.microsoft.com/library/mt244103.aspx) (出力) のストリームを表示します。 次のサンプル コードのコンパイルを開始する、 **SampleConfig** の構成が完了したら、そのストリームを表示するまでの待機します。
    
    $CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
    
    while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)           
    {$CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
        Start-Sleep -Seconds 3
    
    }
    
    $CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any 


##基本パラメーター##

DSC 構成のパラメーターの宣言 (パラメーターの種類、プロパティなど) は、Azure Automation Runbook と同じように動作します。 参照してください [Azure Automation で runbook を開始](automation-starting-a-runbook.md) を runbook のパラメーターの詳細を参照してください。

次のコードの例では、2 つのパラメーターと呼ばれる **FeatureName** と **IsPresent**, 、内のプロパティの値を決定する、 **ParametersExample.sample** ノード構成のコンパイル中に生成します。

    Configuration ParametersExample {
        param(
        [Parameter(Mandatory=$true)]
    
        [string] $FeatureName,
    
        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    
    )
    
    $EnsureString = "Present"
    if($IsPresent -eq $false) {
        $EnsureString = "Absent"
    
    }
    
    Node "sample" {
    
        WindowsFeature ($FeatureName + "Feature") {
            Ensure = $EnsureString
            Name = $FeatureName
            }
        }
    }

Azure Automation DSC ポータルまたは Azure PowerShell で、基本パラメーターを使用する DSC 構成をコンパイルできます。

###ポータル###

ポータルをクリックするとパラメーター値を入力することができます **コンパイル**します。

![alt text](./media/automation-dsc-compile/DSC_compiling_1.png)

###PowerShell###

PowerShell 内のパラメーターに必要な [hashtable](http://technet.microsoft.com/library/hh847780.aspx) 、パラメーター名に一致する、キーと値は、パラメーターの値。

    $Parameters = @{
            "FeatureName" = "Web-Server"
            "IsPresent" = $False
    }
    
    
    Start-AzureRMAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters 
    

PSCredentials をパラメーターとして渡す方法については、次を参照してください。 <a href="#credential-assets">**の資格情報資産**</a> 以下です。

##ConfigurationData##

**構成データ** すべてが、PowerShell DSC を使用しているときに特定の構成を環境から構造的な構成を切り離すことができます。 参照してください [PowerShell DSC では、"Where"から分離することで、「What」](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) について詳しく学習する **ConfigurationData**します。

>[AZURE.NOTE] 使用する **ConfigurationData** Azure ポータルではなく Azure PowerShell を使用して Azure Automation DSC でのコンパイル時にします。

次の例の DSC 構成を使用して **ConfigurationData** を使用して、 **$ConfigurationData** と **$AllNodes** キーワードです。 必要になります、 [**xWebAdministration** モジュール](https://www.powershellgallery.com/packages/xWebAdministration/) この使用例。

     Configuration ConfigurationDataSample {
        Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite
    
        Write-Verbose $ConfigurationData.NonNodeData.SomeMessage 
    
        Node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
        {
    
            xWebsite Site
            {
    
                Name = $Node.SiteName
                PhysicalPath = $Node.SiteContents
                Ensure   = "Present"
            }
    
        }
 
    }

PowerShell を使用して Azure Automation DSC プル サーバーに 2 つのノード構成を追加する上記の DSC 構成をコンパイルすることができます: **ConfigurationDataSample.MyVM1** と **ConfigurationDataSample.MyVM3**:

    $ConfigData = @{
        AllNodes = @(
            @{
                NodeName = "MyVM1"
                Role = "WebServer"
            },
            @{
                NodeName = "MyVM2"
                Role = "SQLServer"
            },
            @{
                NodeName = "MyVM3"
                Role = "WebServer"
    
            }
    
        )
    
        NonNodeData = @{
            SomeMessage = "I love Azure Automation DSC!"
    
        }
    
    } 
    
    Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ConfigurationDataSample" -ConfigurationData $ConfigData


##資産##

資産の参照方法は、Azure Automation DSC 構成や Runbook と同じです。 詳細については、以下を参照してください。

- [証明書](automation-certificates.md)
- [接続](automation-connections.md)
- [資格情報](automation-credentials.md)
- [変数](automation-variables.md)

###資格情報資産###
Azure Automation での DSC 構成を使用して資格情報資産を参照できる **Get-automationpscredential**, 、資格情報資産渡すこともできますでパラメーターを使用して必要な場合です。 構成のパラメーターを受け取る場合 **PSCredential** 型、PSCredential オブジェクトではなく、パラメーターの値として Azure Automation の資格情報資産の名前の文字列を渡す必要があります。 バックグラウンドで、この名前の Azure Automation 資格情報資産が取得され、構成に渡されます。

ノード構成 (MOF 構成ドキュメント) で資格情報を安全に保持するには、ノード構成 MOF ファイルで資格情報を暗号化する必要があります。 Azure Automation ではさらに、MOF ファイル全体を暗号化します。 ただし、現時点では、ノード構成 MOF 作成時に資格情報をプレーンテキストで出力することを許可するように PowerShell DSC に指定する必要があります。PowerShell DSC は、コンパイル ジョブによって生成された MOF ファイル全体を Azure Automation が暗号化することを認識していないためです。

生成されたノード構成 Mof にプレーン テキストで出力される資格情報の問題がない PowerShell DSC がわかるを使用して <a href="#configurationdata">**ConfigurationData**</a>します。 渡す必要があります `PSDscAllowPlainTextPassword = $true` を介して **ConfigurationData** DSC 構成で表示され、資格情報を使用する各ノードのブロックの名前にします。

次の例は、Automation 資格情報資産を使用する DSC 構成の例です。

    Configuration CredentialSample {
    
       $Cred = Get-AutomationPSCredential -Name "SomeCredentialAsset"
    
        Node $AllNodes.NodeName { 
    
            File ExampleFile { 
                SourcePath = "\\Server\share\path\file.ext" 
                DestinationPath = "C:\destinationPath" 
                Credential = $Cred 
    
            }
    
        }
    
    }

PowerShell を使用して Azure Automation DSC プル サーバーに 2 つのノード構成を追加する上記の DSC 構成をコンパイルすることができます:  **CredentialSample.MyVM1** と **CredentialSample.MyVM2**します。


    $ConfigData = @{
        AllNodes = @(
             @{
                NodeName = "*"
                PSDscAllowPlainTextPassword = $True
            },
    
            @{
                NodeName = "MyVM1"
            },
    
            @{
                NodeName = "MyVM2"
            }
        )
    } 
    
    
    
    Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "CredentialSample" -ConfigurationData $ConfigData

