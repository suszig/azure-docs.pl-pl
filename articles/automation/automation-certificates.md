<properties 
   pageTitle="Azure Automation の証明書資産 | Microsoft Azure"
   description="証明書は、runbook または Azure およびサードパーティのリソースに対して認証する DSC 構成でアクセスできるように、Azure Automation に安全に保存できます。この記事では、証明書の詳細およびテキスト作成とグラフィカル作成の両方で証明書を使用する方法について説明します。"
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


# Azure Automation の証明書資産

Runbook または DSC 構成を使用してアクセスできるように、Azure Automation で証明書を安全に保存することができます、 **Get-automationcertificate** アクティビティ。 Runbook と証明書認証を使用する DSC 構成を作成することができますか、Azure またはサード パーティのリソースに追加します。
>[AZURE.NOTE] Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables. これらの資産は暗号化され、生成される一意のキーを使用して、各オートメーション アカウントの Azure Automation に保存されます。 This key is encrypted by a master certificate and stored in Azure Automation. セキュリティで保護された資産を格納する前に、オートメーション アカウントのキーがマスター証明書を使用して復号化され、資産の暗号化に使用されます。

## Windows PowerShell コマンドレット

Windows PowerShell でオートメーション証明書資産を作成および管理するには、次の表のコマンドレットを使用します。 一部として出荷される、 [Azure PowerShell モジュール](../powershell-install-configure.md) Automation の runbook と DSC 構成で使用することができます。

| コマンドレット| 説明|
|:---|:---|
| [Get-azureautomationcertificate](http://msdn.microsoft.com/library/dn913765.aspx)| 証明書に関する情報を取得します。Get-AutomationCertificate アクティビティから取得できるのは、証明書自体のみです。|
| [New-azureautomationcertificate](http://msdn.microsoft.com/library/dn913764.aspx)| 新しい証明書を Azure Automation にインポートします。|
| [Remove-azureautomationcertificate](http://msdn.microsoft.com/library/dn913773.aspx)| 証明書を Azure Automation から削除します。|
| [Set-azureautomationcertificate](http://msdn.microsoft.com/library/dn913763.aspx)| 証明書ファイルのアップロードや .pfx のパスワードの設定など、既存の証明書のプロパティを設定します。|

## 証明書にアクセスするアクティビティ

次の表のアクティビティは、runbook または DSC 構成での証明書にアクセスに使用されます。

| アクティビティ| Description|
|:---|:---|
| Get-AutomationCertificate| Runbook または DSC 構成で使用する証明書を取得します。|

>[AZURE.NOTE] 変数の使用を避ける必要がありますので、これは、runbook または DSC 構成の間の依存関係の検出が複雑になるし、証明書資産のデザイン時に GetAutomationCertificate の – Name パラメーターでします。

## 新しい証明書の作成

新しい証明書を作成するときは、cer または pfx ファイルを Azure Automation にアップロードします。 証明書をエクスポート可能のマークを付けた場合、転送できる Azure Automation の証明書ストアからです。 エクスポートされていない場合、そののみ使用できます、runbook または DSC 構成内での署名します。

### Azure ポータルで新しい証明書を作成するには

1. オートメーション アカウントから、ウィンドウの上部にある **[資産]** をクリックします。
1. ウィンドウの下部にある **[設定の追加]** をクリックします。
1. **[資格情報の追加]** をクリックします。
2. **[資格情報の種類]** ドロップダウンで、**[証明書]** を選択します。
3. **[名前]** ボックスに証明書の名前を入力し、右矢印をクリックします。
4. .cer または .pfx ファイルを参照します。 .pfx ファイルを選択する場合は、パスワードおよびエクスポートを許可するかどうかを指定します。
1. チェック マークをクリックして証明書ファイルをアップロードし、新しい証明書資産を保存します。


### Azure プレビュー ポータルで新しい証明書を作成するには

1. オートメーション アカウントから、**[資産]** 部分をクリックして **[資産]** ブレードを開きます。
1. **[証明書]** 部分をクリックして **[証明書]** ブレードを開きます。
1. ブレード上部の **[証明書の追加]** をクリックします。
2. **[名前]** ボックスに証明書の名前を入力します。
2. **[証明書ファイルをアップロードします]** の **[ファイルの選択]** をクリックして、.cer または .pfx ファイルを参照します。 .pfx ファイルを選択する場合は、パスワードおよびエクスポートを許可するかどうかを指定します。
1. **[作成]** をクリックして、新しい証明書資産を保存します。


### Windows PowerShell で新しい証明書を作成するには

次のサンプル コマンドでは、新しいオートメーション証明書を作成してエクスポート可能に指定する方法について説明します。 これは既存の pfx ファイルをインポートします。

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
    
    New-AzureAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable

## 証明書の使用

使用する必要があります、 **Get-automationcertificate** 証明書を使用するアクティビティ。 使用することはできません、 [Get-azureautomationcertificate](http://msdn.microsoft.com/library/dn913765.aspx) コマンドレットでは証明書自体ではない証明書資産に関する情報を返すためです。

### テキストの Runbook のサンプル

次のサンプル コードでは、Runbook でクラウド サービスに証明書を追加する方法を示します。 このサンプルでは、パスワードは暗号化されたオートメーション変数から取得されます。

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AutomationVariable –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### グラフィカルな Runbook のサンプル

グラフィカル エディターの [ライブラリ] ウィンドウで証明書を右クリックして **[キャンバスに追加]** を選択することにより、**Get-AutomationCerticiate** をグラフィカルな Runbook に追加します。

![](media/automation-certificates/certificate-add-canvas.png)

次の図は、グラフィカルな Runbook で証明書を使用する例を示したものです。 これは、テキストの Runbook からクラウド サービスに証明書を追加する前述の例と同じです。

この例では、サービスに対する認証に接続オブジェクトを使用する **Send-TwilioSMS** アクティビティに対して設定された **UseConnectionObject** パラメーターを使用します。 A [パイプライン リンク](automation-graphical-authoring-intro.md#links-and-workflow) シーケンス リンクは Connection パラメーターが予期しないする 1 つのオブジェクトを含むを含むコレクションを返すためここで使用する必要があります。

![](media/automation-certificates/add-certificate.png)


## 関連項目

- [グラフィカル作成でのリンク](automation-graphical-authoring-intro.md#links-and-workflow)





