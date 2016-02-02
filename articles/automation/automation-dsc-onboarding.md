<properties 
   pageTitle="Azure Automation DSC による管理のための物理および仮想マシンのオンボード |Microsoft Azure" 
   description="Azure Automation DSC による管理のためのマシンの設定方法" 
   services="automation" 
   documentationCenter="dev-center-name" 
   authors="coreyp-at-msft" 
   manager="stevenka" 
   editor="tysonn"/>

<tags
   ms.service="automation"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="TBD" 
   ms.date="11/23/2015"
   ms.author="coreyp"/>


# Azure Automation DSC による管理のためのマシンのオンボード

## Azure Automation DSC でマシンを管理する理由

ような [PowerShell Desired State Configuration](https://technet.microsoft.com/library/dn249912.aspx), 、Azure Automation Desired State Configuration では、すべてのクラウドまたは内部設置型データ センターの DSC ノード (物理および仮想マシン) のシンプルでありながら強力な構成管理サービスです。 これにより、一元化された安全な場所から、何千ものマシンでの迅速かつ簡単なスケーラビリティが可能になります。 マシンのオンボード、宣言型構成の割り当て、指定した必要な状態に準拠した各マシンを示すレポートの表示を簡単に行うことができます。 Azure Automation DSC 管理レイヤーは、Azure Automation 管理レイヤーが PowerShell スクリプトに対応する DSC に対応します。 つまり、同様に Azure Automation は、Powershell スクリプトを管理することができますを Azure Automation DSC を使用する利点の詳細については、「に DSC 構成を管理する [Azure Automation DSC の概要](automation-dsc-overview/)します。

以下のさまざまなマシンを管理する際に、Azure Automation DSC を使用できます。

*    Azure virtual machines (classic)

*    Azure virtual machines

*    Physical / virtual Windows machines on-premises, or in a cloud other than Azure

*    Physical / virtual Linux machines on-premises, in Azure, or in a cloud other than Azure


次のセクションでは、各種類のマシンを Azure Automation DSC にオンボードできる方法の概要を示します。

## Azure Virtual Machines (クラシック)

Azure Automation DSC では、Azure ポータルまたは PowerShell を使用して、構成管理用に Azure Virtual Machines (クラシック) を簡単にオンボードすることができます。 Azure VM Desired State Configuration 拡張機能を使用することで、管理者が VM にリモート接続しなくても、内部で VM を Azure Automation DSC に登録できます。 進行状況を追跡またはトラブルシューティングを行う手順が記載されて Azure VM Desired State Configuration の拡張機能が非同期的に実行されるため、 [* * トラブルシューティング Azure 仮想マシン オンボーディング * *](#troubleshooting-azure-virtual-machine-onboarding) 以下のセクションです。


### Azure ポータル

[Azure プレビュー ポータル](http://portal.azure.com/), をクリックして **参照** ]-> [ **仮想マシン (クラシック)**します。オンボードする Windows VM を選択します。仮想マシンのダッシュボード ブレードで、**[すべての設定]**、**[拡張機能]**、**[追加]**、**[Azure Automation DSC]**、**[作成]** の順にクリックします。入力、 [PowerShell DSC ローカル構成マネージャー値](https://technet.microsoft.com/library/dn249922.aspx?f=255&MSPPError=-2147217396) のユース ケース、オートメーション アカウントの登録キーと登録 URL および必要に応じてノードの構成を VM に割り当てる必要です。

![](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)


登録 URL を検索して表示するコンピューターのオンボードするオートメーション アカウントのキー、 [* * * * 登録をセキュリティで保護](#secure-registration) 以下のセクションです。

### PowerShell

    # log in to both Azure Service Management and Azure Resource Manager
    Add-AzureAccount
    Login-AzureRmAccount
    
    # fill in correct values for your VM / Automation Account here
    $VMName = ""
    $ServiceName = ""
    $AutomationAccountName = ""
    $AutomationAccountResourceGroup = ""
    
    # fill in the name of a Node Configuration in Azure Automation DSC, for this VM to conform to
    $NodeConfigName = ""
    
    # get Azure Automation DSC registration info
    $Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
    $RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo
    
    # use the DSC extension to onboard the VM for management with Azure Automation DSC
    $VM = Get-AzureVM -Name $VMName -ServiceName $ServiceName
    
    $PublicConfiguration = ConvertTo-Json -Depth 8 @{
      SasToken = ""
      ModulesUrl = "https://eus2oaasibizamarketprod1.blob.core.windows.net/automationdscpreview/RegistrationMetaConfigV2.zip"
      ConfigurationFunction = "RegistrationMetaConfigV2.ps1\RegistrationMetaConfigV2"
    
    # update these DSC agent Local Configuration Manager defaults if they do not match your use case.
    # See https://technet.microsoft.com/library/dn249922.aspx?f=255&MSPPError=-2147217396 for more details
    Properties = @{
       RegistrationKey = @{
         UserName = 'notused'
         Password = 'PrivateSettingsRef:RegistrationKey'
    }
      RegistrationUrl = $RegistrationInfo.Endpoint
      NodeConfigurationName = $NodeConfigName
      ConfigurationMode = "ApplyAndMonitor"
      ConfigurationModeFrequencyMins = 15
      RefreshFrequencyMins = 30
      RebootNodeIfNeeded = $False
      ActionAfterReboot = "ContinueConfiguration"
      AllowModuleOverwrite = $False
      }
    }
    
    $PrivateConfiguration = ConvertTo-Json -Depth 8 @{
      Items = @{
         RegistrationKey = $RegistrationInfo.PrimaryKey
      }
    }
    
    $VM = Set-AzureVMExtension `
     -VM $vm `
     -Publisher Microsoft.Powershell `
     -ExtensionName DSC `
     -Version 2.6 `
     -PublicConfiguration $PublicConfiguration `
     -PrivateConfiguration $PrivateConfiguration `
     -ForceUpdate
    
    $VM | Update-AzureVM

## Azure Virtual Machines

Azure Automation DSC では、Azure ポータル、Azure リソース マネージャーのテンプレート、または PowerShell を使用して、構成管理用に Azure Virtual Machines (クラシック) を簡単にオンボードすることができます。 Azure VM Desired State Configuration 拡張機能を使用することで、管理者が VM にリモート接続しなくても、内部で VM を Azure Automation DSC に登録できます。 進行状況を追跡またはトラブルシューティングを行う手順が記載されて Azure VM Desired State Configuration の拡張機能が非同期的に実行されるため、 [* * トラブルシューティング Azure 仮想マシン オンボーディング * *](#troubleshooting-azure-virtual-machine-onboarding) 以下のセクションです。


### Azure ポータル

[Azure プレビュー ポータル](http://portal.azure.com/), 、仮想マシンの追加をする Azure Automation アカウントに移動します。 Automation アカウントのダッシュボードで、**[DSC ノード]**、**[Azure VM の追加]** の順にクリックします。

**[オンボードする仮想マシンの選択]** で、オンボードする 1 つ以上の Azure Virtual Machines を選択します。

![](./media/automation-dsc-onboarding/DSC_Onboarding_2.png)


**登録データを構成する**, 、入力、 [PowerShell DSC ローカル構成マネージャーの値](https://technet.microsoft.com/library/dn249922.aspx?f=255&MSPPError=-2147217396) を VM に割り当てるのユース ケースと、必要に応じてノードの構成に必要なです。

![](./media/automation-dsc-onboarding/DSC_Onboarding_3.png)


### Azure リソース マネージャーのテンプレート

Azure Virtual Machines は、Azure リソース マネージャーのテンプレートを使用して Azure Automation DSC にデプロイおよびオンボードできます。 参照してください [DSC 拡張機能を使用して VM と Azure Automation DSC 構成](http://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) テンプレートの例についてその onboards Azure Automation DSC に既存の仮想マシン。 登録キーと実行される登録 URL を見つけるため、このテンプレートの入力として次を参照してください。、 [* * * * 登録をセキュリティで保護](#secure-registration) 以下のセクションです。

### PowerShell

[レジスタ AzureRmAutomationDscNode](https://msdn.microsoft.com/library/mt603833.aspx) コマンドレットを使用して、PowerShell を使用した Azure プレビュー ポータルで仮想マシンの追加することができます。

## オンプレミス、または Azure 以外のクラウド内の物理/仮想 Windows マシン

オンプレミスの Windows マシンと (Amazon Web Services などの) Azure 以外のクラウド内の Windows マシンも、インターネットへの発信アクセスが可能な限り、いくつかの簡単な手順で Azure Automation DSC にオンボードすることができます。

1. 最新バージョンを確認 [WMF 5](http://www.microsoft.com/en-us/download/details.aspx?id=48729) を Azure Automation DSC にオンボード マシンにインストールされています。

2. ローカル環境のマシンで、管理者として PowerShell コンソールまたは PowerShell ISE を開きます。 このマシンにも最新バージョンの WMF 5 がインストールされている必要があります。

3. 以下の Azure PowerShell モジュールを使用して、Azure リソース マネージャーに接続します。
        `ログイン AzureRmAccount`

4. 以下を使用して、ノードをオンボードする Automation アカウントから、オンボードするマシンの PowerShell DSC メタ構成をダウンロードします。

    `Get AzureRmAutomationDscOnboardingMetaconfig-ResourceGroupName MyResourceGroup - AutomationAccountName MyAutomationAccount-computername MyServer1、MyServer2 - OutputFolder C:\Users\joe\Desktop`

5. 必要に応じて、表示し、一致するように、必要に応じて、出力フォルダーに metaconfigurations を更新、 [PowerShell DSC ローカル構成マネージャーのフィールドと値](https://technet.microsoft.com/library/dn249922.aspx?f=255&MSPPError=-2147217396) する、既定の設定では、ユース ケースが一致しない場合。

6. 以下を使用して、オンボードするマシンに PowerShell DSC メタ構成をリモートで適用します。

    `Set-dsclocalconfigurationmanager-パス C:\Users\joe\Desktop\DscMetaConfigs-computername MyServer1、MyServer2`

7. PowerShell DSC メタ構成をリモートで適用できない場合は、手順 4. の出力フォルダーをオンボードする各マシンにコピーします。 次に、オンボードする各マシンで Set-DscLocalConfigurationManager をローカルで呼び出します。

8. Azure ポータルまたはコマンドレットを使用して、オンボードするマシンがこの時点で Azure Automation アカウントに登録されている DSC ノードとして示されていることを確認します。

## オンプレミス、Azure、または Azure 以外のクラウド内の物理/仮想 Linux マシン

オンプレミスの Linux マシン、Azure の Linux マシン、Azure 以外のクラウド内の Linux マシンも、インターネットへの発信アクセスが可能な限り、いくつかの簡単な手順で Azure Automation DSC にオンボードすることができます。

1. 最新バージョンの確認、 [DSC Linux エージェント](http://www.microsoft.com/en-us/download/details.aspx?id=49150) を Azure Automation DSC にオンボード マシンにインストールされています。

2. 場合、 [PowerShell DSC ローカル構成マネージャーの既定値](https://technet.microsoft.com/library/dn249922.aspx?f=255&MSPPError=-2147217396) のユース ケースに一致します。

    *    On each Linux machine to onboard to Azure Automation DSC, use Register.py to onboard using the PowerShell DSC Local Configuration Manager defaults:

        `/opt/microsoft/dsc/Scripts/Register.py < オートメーション アカウントの登録キー >< オートメーション アカウントの登録 URL >`

    *    To find the registration key and registration URL for your Automation account, see the [**Secure registration**](#secure-registration) section below.

    PowerShell DSC Local Configuration Manager の既定値がユース ケースに適して**い****ない**場合は、手順 3. から 手順 9. に従います。 それ以外の場合は、手順 9. に直接進みます。

3. ローカル環境の Windows マシンで、管理者として PowerShell コンソールまたは PowerShell ISE を開きます。 このコンピューターには、最新の必要があります [WMF 5](http://www.microsoft.com/en-us/download/details.aspx?id=48729) をインストールします。

4. 以下の Azure PowerShell モジュールを使用して、Azure リソース マネージャーに接続します。

    `ログイン AzureRmAccount`

5.  以下を使用して、ノードをオンボードする Automation アカウントから、オンボードするマシンの PowerShell DSC メタ構成をダウンロードします。

    `Get AzureRmAutomationDscOnboardingMetaconfig-ResourceGroupName MyResourceGroup - AutomationAccountName MyAutomationAccount-computername MyServer1、MyServer2 - OutputFolder C:\Users\joe\Desktop_`

6.  必要に応じて、表示し、一致するように、必要に応じて、出力フォルダーに metaconfigurations を更新、 [PowerShell DSC ローカル構成マネージャーのフィールドと値](http://https://technet.microsoft.com/library/dn249922.aspx?f=255&MSPPError=-2147217396) する、既定の設定では、ユース ケースが一致しない場合。

7.  以下を使用して、オンボードするマシンに PowerShell DSC メタ構成をリモートで適用します。

    $SecurePass = ConvertTo-SecureString -string "<root password>" -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential "root", $SecurPass
    $Opt = New-CimSessionOption -UseSs1:$true -SkipCACheck:$true -SkipCNCheck:$true -SkipRevocationCheck:$true
    
    # need a CimSession for each Linux machine to onboard
    
    $Session = New-CimSession -Credential:$Cred -ComputerName:<your Linux machine> -Port:5986 -Authentication:basic -SessionOption:$Opt
    
    Set-DscLocalConfigurationManager -CimSession $Session –Path C:\Users\joe\Desktop\DscMetaConfigs

8.  PowerShell DSC メタ構成をリモートで適用できない場合は、オンボードする各 Linux マシンについて、手順 5. のフォルダーからそのマシンに対応するメタ構成を Linux マシンにコピーします。 まず `SetDscLocalConfigurationManager.py` ローカルで各 Linux マシンにする Azure Automation DSC にオンボードします。

    `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py – configurationmof < メタ ファイルへのパス >`

9.  Azure ポータルまたはコマンドレットを使用して、オンボードするマシンがこの時点で Azure Automation アカウントに登録されている DSC ノードとして示されていることを確認します。

## セキュリティで保護された登録

マシンは WMF 5 DSC 登録プロトコルを介して Azure Automation アカウントに安全にオンボードすることができます。これにより、DSC ノードを PowerShell DSC V2 のプルまたはレポート サーバー (Azure Automation DSC を含む) に対して認証することができます。 ノードは**登録 URL** でサーバーに登録し、**登録キー**を使用して認証します。 登録時に、DSC ノードと DSC プル/レポート サーバーは、登録後のサーバーに対する認証に使用するこのノードの一意の証明書をネゴシエートします。 このプロセスは、ノードが侵害され、悪意のある動作を行っている場合などに、オンボードされたノードが別のノードの代わりに動作しないようにするためのものです。 登録後、登録キーは再度認証に使用されず、ノードから削除されます。

DSC 登録プロトコルに必要な情報は、Azure プレビュー ポータルの **[キーの管理]** ブレードから取得できます。 Automation アカウントの **[要点]** パネルにあるキー アイコンをクリックして、このブレードを開きます。

![](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

*    Registration URL is the URL field in the Manage Keys blade.

*    Registration key is the Primary Access Key or Secondary Access Key in the Manage Keys blade. Either key can be used.


セキュリティ強化のため、Automation アカウントのプライマリおよびセカンダリ アクセス キーを (**[キーの管理]** ブレードで) いつでも再生成して、以前のキーを使用して今後ノードが登録されないようにすることができます。

## Azure 仮想マシンのオンボードに関するトラブルシューティング

Azure Automation DSC を使用すると、構成管理のための Azure Windows VM を簡単にオンボードできます。 内部で、Azure VM Desired State Configuration 拡張機能を使用して、VM を Azure Automation DSC に登録します。 Azure VM Desired State Configuration 拡張機能は非同期に実行されるため、その進行状況の追跡とその実行に関するトラブルシューティングが重要な場合があります。
>[AZURE.NOTE] Azure VM Desired State Configuration 拡張機能を使用する Azure Automation DSC への Azure Windows VM のオンボード方法では、ノードが Azure Automation に登録済みと表示されるまでに最大で 1 時間かかる場合があります。 これは、VM を Azure Automation DSC にオンボードするために必要な Windows Management Framework 5.0 を、Azure VM DSC 拡張機能が VM にインストールするためです。

トラブルシューティングを行う場合や、Azure VM Desired State Configuration 拡張機能の状態を表示する場合は、Azure プレビュー ポータルでオンボードされている VM に移動してから、**[すべての設定]**、**[拡張機能]**、**[DSC]** の順にクリックします。 詳細は、**[詳細な状態の表示]** をクリックして表示できます。

[![](./media/automation-dsc-onboarding/DSC_Onboarding_5.png)] (https://technet.microsoft.com/library/dn249912.aspx)

## 証明書の有効期限と登録

登録すると、各ノードは、1 年後に有効期限が切れる認証用の一意の証明書を自動的にネゴシエートします。 現時点では、PowerShell DSC 登録プロトコルは、有効期限が近づいたときに証明書を自動的に更新することはできないため、1 年後にノードを再登録する必要があります。 再登録する前に、各ノードで Windows Management Framework 5.0 RTM が実行されていることを確認します。 ノードの認証証明書の有効期限が切れるときにノードが再登録されない場合、ノードは Azure Automation と通信できなくなり、[反応なし] とマークされます。 再登録は、ノードを初めて登録したのと同じ方法で実行されます。 証明書の有効期限が切れる 90 日以内または証明書の有効期限が切れた後で再登録を実行すると、新しい証明書が生成されて使用されます。

## 関連記事

* [Azure Automation DSC の概要](automation-dsc-overview.md)
* [Azure Automation DSC のコマンドレット](https://msdn.microsoft.com/library/mt244122.aspx)
* [Azure Automation DSC の料金](http://azure.microsoft.com/pricing/details/automation/)








