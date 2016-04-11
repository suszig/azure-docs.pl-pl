<properties 
   pageTitle="Azure Automation での資格情報資産 | Microsoft Azure"
   description="Azure Automation の資格情報資産には、Runbook または DSC 構成によってアクセスされるリソースの認証に使用できるセキュリティ資格情報が含まれます。 この記事では、資格情報資産を作成し、Runbook または DSC 構成でそれを使用する方法について説明します。"
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/23/2015"
   ms.author="bwren" />

# Azure Automation での資格情報資産

Automation の資格情報資産を保持して、 [PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential)  ユーザー名やパスワードなどのセキュリティ資格情報を格納するオブジェクト。 Runbook および DSC 構成では、認証に PSCredential オブジェクトを受け付けるコマンドレットを使用したり、PSCredential オブジェクトのユーザー名とパスワードを抽出して認証を必要とするアプリケーションやサービスに提供したりできます。 Azure Automation に安全に保存および DSC 構成で、runbook でアクセスできる資格情報のプロパティ、 [Get-automationpscredential](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) アクティビティ。

>[AZURE.NOTE] Azure Automation でセキュリティで保護された資産には、資格情報、証明書、接続、および暗号化された変数が含まれます。 これらの資産は、各 Automation アカウント用に生成された一意のキーを使用して暗号化され、Azure Automation に保存されます。 このキーはマスター証明書によって暗号化され、Azure Automation に保存されます。 セキュリティで保護された資産を格納する前に、Automation アカウントのキーがマスター証明書を使用して復号化され、資産の暗号化に使用されます。 

## Windows PowerShell コマンドレット

Windows PowerShell で Automation 資格情報資産を作成および管理するには、次の表のコマンドレットを使用します。  一部として出荷される、 [Azure PowerShell モジュール](../powershell-install-configure.md) Automation の runbook と DSC 構成で使用することができます。

|コマンドレット|説明|
|:---|:---|
|[Get-AzureAutomationCredential](http://msdn.microsoft.com/library/dn913781.aspx)|資格情報資産に関する情報を取得します。 取得できるは、資格情報自体のみから **Get-automationcredential** アクティビティ。|
|[New-AzureAutomationCredential](http://msdn.microsoft.com/library/azure/jj554330.aspx)|新しい Automation 資格情報を作成します。|
|[Remove-AzureAutomationCredential](http://msdn.microsoft.com/library/azure/jj554330.aspx)|Automation 資格情報を削除します。|
|[Set-AzureAutomationCredential](http://msdn.microsoft.com/library/azure/jj554330.aspx)|既存の Automation 資格情報のプロパティを設定します。|

## Runbook アクティビティ

次の表のアクティビティは、Runbook および DSC 構成で資格情報にアクセスするために使用されます。

|アクティビティ|説明|
|:---|:---|
|Get-AutomationPSCredential|Runbook または DSC 構成で使用する資格情報を取得します。 返します。、 [System.Management.Automation.PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) オブジェクトです。|

>[AZURE.NOTE] 変数の使用を避ける必要がありますので、これは、runbook または DSC 構成の間の依存関係の検出が複雑になるし、資格情報資産のデザイン時に Get-automationpscredential の – Name パラメーターでします。

## 新しい資格情報の作成


### Azure ポータルで新しい変数を作成するには

1. オートメーション アカウントからクリックして **資産** ウィンドウの上部にあります。
1. At the bottom of the window, click **Add Setting**.
1. Click **Add Credential**.
2. In the **Credential Type** dropdown, select **PowerShell Credential**.
1. ウィザードを完了し、チェック ボックスをクリックして新しい資格情報を保存します。


### Azure プレビュー ポータルで新しい資格情報を作成するには

1. クリックして、オートメーション アカウントから、 **資産** を開くには一部、 **資産** ブレードです。
1. クリックして、 **資格情報** を開くには一部、 **資格情報** ブレードです。
1. クリックして **資格情報を追加** ブレードの上部にあります。
1. Complete the form and click **Create** to save the new credential.


### Windows PowerShell を使用して新しい PowerShell 資格情報を作成するには

次のサンプル コマンドでは、新しい Automation 資格情報を作成する方法について説明します。 最初に名前とパスワードで PSCredential オブジェクトを作成し、それを使用して資格情報資産を作成します。 また、使用することが、 **Get-credential** コマンドレットを名とパスワードを入力するように求められます。

    $user = "MyDomain\MyUser"
    $pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
    $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
    New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred

## PowerShell 資格情報の使用

DSC 構成で、runbook で資格情報資産を取得する、 **Get-automationpscredential** アクティビティ。 返されます。、 [PSCredential オブジェクト](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) アクティビティまたは、PSCredential パラメーターを必要とするコマンドレットで使用できます。 また、資格情報オブジェクトのプロパティを取得して個別に使用することもできます。 オブジェクトが、ユーザー名とセキュリティ保護されたパスワードのプロパティまたは使用することができます、 **GetNetworkCredential** を返すメソッドを [NetworkCredential](http://msdn.microsoft.com/library/system.net.networkcredential.aspx) セキュリティ保護されていないバージョンのパスワードを提供するオブジェクト。

### テキストの Runbook のサンプル

次のサンプル コマンドでは、Runbook で PowerShell 資格情報を使用する方法を示します。 この例では、資格情報を取得し、そのユーザー名とパスワードを変数に代入します。

    $myCredential = Get-AutomationPSCredential -Name 'MyCredential'
    $userName = $myCredential.UserName
    $securePassword = $myCredential.Password
    $password = $myCredential.GetNetworkCredential().Password


### グラフィカルな Runbook のサンプル

追加する、 **Get-automationpscredential** をグラフィカル エディターの [ライブラリ] ウィンドウで資格情報を右クリックし、グラフィカルな runbook にアクティビティ **[キャンバスに追加**します。


![キャンバスに資格情報を追加する](media/automation-credentials/credential-add-canvas.png)

次の図は、グラフィカルな Runbook で資格情報を使用する例を示したものです。  」の説明に従って、runbook 用の Azure リソースに認証を提供する場合は、使用されているが [Azure リソースに対する認証を構成する](#automation-configuring.md)です。  最初のアクティビティは、Azure サブスクリプションへのアクセス権を持つ資格情報を取得します。   **Add-azureaccount** アクティビティがその後にあるすべてのアクティビティの認証機能を提供し、この資格情報を使用します。  A [パイプライン リンク](automation-graphical-authoring-intro.md#links-and-workflow) ためここでは、 **Get-automationpscredential** は 1 つのオブジェクトを受け取るします。  

![キャンバスに資格情報を追加する](media/automation-credentials/get-credential.png)

## DSC での PowerShell 資格情報の使用
Azure Automation での DSC 構成を使用して資格情報資産を参照できる **Get-automationpscredential**, 、資格情報資産渡すこともできますでパラメーターを使用して必要な場合です。 詳細については、次を参照してください。 [Azure Automation DSC での構成をコンパイルする](automation-dsc-compile.md#credential-assets)です。

## 関連記事:

- [グラフィカル作成でのリンク](automation-graphical-authoring-intro.md#links-and-workflow)

 

