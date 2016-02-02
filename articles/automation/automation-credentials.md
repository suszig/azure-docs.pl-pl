<properties 
   pageTitle="Azure Automation での資格情報資産 | Microsoft Azure"
   description="Azure Automation での資格情報資産には、runbook または DSC 構成でアクセスされるリソースへの認証に使用できるセキュリティ資格情報が含まれています。この記事では、資格情報資産を作成し、runbook または DSC 構成で使用する方法について説明します。"
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

Automation の資格情報資産を保持して、 [PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential)  ユーザー名やパスワードなどのセキュリティ資格情報を格納するオブジェクト。 Runbook と DSC 構成は、認証に PSCredential オブジェクトを受け取るコマンドレットを使用して可能性があります。 またはユーザー名と一部のアプリケーションや認証を必要とするサービスを提供する PSCredential オブジェクトのパスワードを抽出可能性があります。 Azure Automation に安全に保存および DSC 構成で、runbook でアクセスできる資格情報のプロパティ、 [Get-automationpscredential](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) アクティビティ。
>[AZURE.NOTE] Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables. これらの資産は暗号化され、生成される一意のキーを使用して、各オートメーション アカウントの Azure Automation に保存されます。 This key is encrypted by a master certificate and stored in Azure Automation. セキュリティで保護された資産を格納する前に、オートメーション アカウントのキーがマスター証明書を使用して復号化され、資産の暗号化に使用されます。 

## Windows PowerShell コマンドレット

Windows PowerShell でオートメーション資格情報資産を作成および管理するには、次の表のコマンドレットを使用します。 一部として出荷される、 [Azure PowerShell モジュール](../powershell-install-configure.md) Automation の runbook と DSC 構成で使用することができます。

| コマンドレット| 説明|
|:---|:---|
| [Get AzureAutomationCredential](http://msdn.microsoft.com/library/dn913781.aspx)| 資格情報資産に関する情報を取得します。**Get-AutomationCredential** アクティビティから取得できるのは、資格情報自体のみです。|
| [新しい AzureAutomationCredential](http://msdn.microsoft.com/library/azure/jj554330.aspx)| 新しい Automation 資格情報を作成します。|
| [Remove-azureautomationcredential](http://msdn.microsoft.com/library/azure/jj554330.aspx)| Automation 資格情報を削除します。|
| [Set-azureautomationcredential](http://msdn.microsoft.com/library/azure/jj554330.aspx)| 既存の Automation 資格情報のプロパティを設定します。|

## Runbook アクティビティ

次の表のアクティビティは、runbook と DSC 構成で資格情報にアクセスに使用されます。

| アクティビティ| Description|
|:---|:---|
| Get-AutomationPSCredential| Runbook または DSC 構成で使用する資格情報を取得します。返します。、 [System.Management.Automation.PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) オブジェクトです。|

>[AZURE.NOTE] 変数の使用を避ける必要がありますので、これは、runbook または DSC 構成の間の依存関係の検出が複雑になるし、資格情報資産のデザイン時に Get-automationpscredential の – Name パラメーターでします。

## 新しい資格情報の作成

### Azure ポータルで新しい資格情報を作成するには

1. Automation アカウントから、ウィンドウ上部にある **[資産]** をクリックします。
1. ウィンドウの下部にある **[設定の追加]** をクリックします。
1. **[資格情報の追加]** をクリックします。
2. **[資格情報の種類]** ドロップダウンで、**[PowerShell 資格情報]** を選択します。
1. ウィザードを完了し、チェック ボックスをクリックして新しい資格情報を保存します。


### Azure プレビュー ポータルで新しい資格情報を作成するには

1. オートメーション アカウントから、**[資産]** 部分をクリックして **[資産]** ブレードを開きます。
1. **[資格情報]** 部分をクリックして、**[資格情報]** ブレードを開きます。
1. ブレード上部の **[資格情報の追加]** をクリックします。
1. フォームに入力し、**[作成]** をクリックして新しい資格情報を保存します。


### Windows PowerShell を使用して新しい PowerShell 資格情報を作成するには

次のサンプル コマンドでは、新しいオートメーション資格情報を作成する方法について説明します。 最初に名前とパスワードで PSCredential オブジェクトを作成し、それを使用して資格情報資産を作成します。 または、**Get-Credential** コマンドレットを使用し、プロンプトに従って名前とパスワードを入力することもできます。

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

グラフィカル エディターの [ライブラリ] ウィンドウで資格情報を右クリックして **[キャンバスに追加]** を選択することにより、**Get-AutomationPSCredential** アクティビティをグラフィカルな Runbook に追加します。


![キャンバスに資格情報を追加する](media/automation-credentials/credential-add-canvas.png)

次の図は、グラフィカルな Runbook で資格情報を使用する例を示したものです。 」の説明に従って、runbook 用の Azure リソースに認証を提供する場合は、使用されているが [Azure リソースに対する認証を構成する](#automation-configuring.md)します。 最初のアクティビティは、Azure サブスクリプションへのアクセス権を持つ資格情報を取得します。 その後、**Add-AzureAccount** アクティビティはこの資格情報を使用して、その後にあるすべてのアクティビティに認証を提供します。 A [パイプライン リンク](automation-graphical-authoring-intro.md#links-and-workflow) ためここでは、 **Get-automationpscredential** は 1 つのオブジェクトを受け取るします。

![キャンバスに資格情報を追加する](media/automation-credentials/get-credential.png)

## DSC で PowerShell 資格情報の使用

Azure Automation の DSC 構成では **Get-AutomationPSCredential** を使用して資格情報資産を参照できますが、必要に応じて、パラメーターを使用して資格情報資産を渡すこともできます。 詳細については、次を参照してください。 [Azure Automation DSC での構成をコンパイルする](automation-dsc-compile.md#credential-assets)します。

## 関連記事:

- [グラフィカル作成でのリンク](automation-graphical-authoring-intro.md#links-and-workflow)







