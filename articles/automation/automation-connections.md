<properties 
   pageTitle="Azure Automation での接続資産 | Microsoft Azure"
   description="Azure Automation の接続資産には、Runbook または DSC 構成から外部サービスまたはアプリケーションに接続するために必要な情報が含まれます。 この記事では、接続の詳細およびテキスト作成とグラフィカル作成の両方で接続を使用する方法について説明します。"
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

# Azure Automation での接続資産

Automation の接続資産には、Runbook または DSC 構成から外部サービスまたはアプリケーションに接続するために必要な情報が含まれます。 これには、URL やポートなどの接続情報に加えて、ユーザー名とパスワードなど認証に必要な情報も含まれる場合があります。 接続の値は、複数の変数を作成する場合とは対照的に、1 つの資産内の特定のアプリケーションに接続するためのプロパティをすべて保持します。 ユーザーは、1 つの場所で接続のための値を編集でき、1 つのパラメーターで Runbook または DSC 構成に接続の名前を渡すことができます。 接続のプロパティは、DSC 構成で、runbook でアクセスできる、 **Get-automationconnection** アクティビティ。

指定する必要があります、接続を作成すると、 *接続の種類*します。 接続の種類は、一連のプロパティを定義しているテンプレートです。 接続では、その接続の種類で定義されている各プロパティの値を定義します。 接続の種類が統合モジュールで Azure Automation に追加されるかで作成された、 [Azure Automation API](http://msdn.microsoft.com/library/azure/mt163818.aspx)します。 接続を作成するときに使用できる接続の種類は、オートメーション アカウントにインストールされているものだけです。

>[AZURE.NOTE] Azure Automation でセキュリティで保護された資産には、資格情報、証明書、接続、および暗号化された変数が含まれます。 これらの資産は、各 Automation アカウント用に生成された一意のキーを使用して暗号化され、Azure Automation に保存されます。 このキーはマスター証明書によって暗号化され、Azure Automation に保存されます。 セキュリティで保護された資産を格納する前に、Automation アカウントのキーがマスター証明書を使用して復号化され、資産の暗号化に使用されます。

## Windows PowerShell コマンドレット

作成およびの一部として Windows PowerShell、宇宙船でのオートメーションの接続を管理する次の表のコマンドレットが使用される、 [Azure PowerShell モジュール](../powershell-install-configure.md) Automation の runbook と DSC 構成で使用することができます。

|コマンドレット|説明|
|:---|:---|
|[Get-AzureAutomationConnection](http://msdn.microsoft.com/library/dn921828.aspx)|接続を取得します。 接続のフィールドの値のハッシュ テーブルが含まれます。|
|[New-AzureAutomationConnection](http://msdn.microsoft.com/library/dn921825.aspx)|新しい接続を作成します。|
|[Remove-AzureAutomationConnection](http://msdn.microsoft.com/library/dn921827.aspx)|既存の接続を削除します。|
|[Set-AzureAutomationConnectionFieldValue](http://msdn.microsoft.com/library/dn921826.aspx)|既存の接続の特定のフィールドの値を設定します。|

## アクティビティ

次の表のアクティビティは、Runbook または DSC 構成で接続にアクセスするために使用されます。

|アクティビティ|説明|
|---|---|
|Get-AutomationConnection|使用する接続を取得します。 接続のプロパティのハッシュ テーブルを返します。|

>[AZURE.NOTE] 変数の使用を避ける必要がありますの – Name パラメーターで **get-AutomationConnection** デザイン時に runbook または DSC 構成との接続資産の間の依存関係を見つけにくくなるためです。

## 新しい接続の作成

### Azure ポータルで新しい接続を作成するには

1. オートメーション アカウントからクリックして **資産** ウィンドウの上部にあります。
1. At the bottom of the window, click **Add Setting**.
1. Click **Add Connection**.
2.  **接続の種類** ] ボックスの一覧を作成する接続の種類を選択します。  ウィザードでその特定の種類のプロパティが表示されます。
1. ウィザードを完了し、チェック ボックスをクリックして新しい接続を保存します。


### Azure プレビュー ポータルで新しい接続を作成するには

1. クリックして、オートメーション アカウントから、 **資産** を開くには一部、 **資産** ブレードです。
1. クリックして、 **接続** を開くには一部、 **接続** ブレードです。
1. クリックして **接続を追加する** ブレードの上部にあります。
2.  **型** ] ボックスの一覧を作成する接続の種類を選択します。  フォームにその特定の種類のプロパティが表示されます。
1. Complete the form and click **Create** to save the new connection.



### Windows PowerShell で新しい接続を作成するには

Windows PowerShell を使用すると新しい接続を作成、 [New-azureautomationconnection](http://msdn.microsoft.com/library/dn921825.aspx) コマンドレットです。 このコマンドレットはという名前のパラメーター **ConnectionFieldValues** 予想される、 [ハッシュ テーブル](http://technet.microsoft.com/en-us/library/hh847780.aspx) 各接続の種類によって定義されたプロパティの値を定義します。


次のサンプル コマンドに新しい接続を作成する [Twilio](http://www.twilio.com) でテキスト メッセージを送受信できるテレフォニー サービスであります。  Twilio 接続種類を含むサンプルの統合モジュールは [スクリプト センター](http://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8)します。  この接続の種類では、Twilio への接続時のアカウント検証に必要なアカウント SID と承認トークンのプロパティが定義されています。  必要な [このモジュールをダウンロード](http://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) して動作するには、このサンプル コードについては、オートメーション アカウントにインストールします。

    $AccountSid = "DAf5fed830c6f8fac3235c5b9d58ed7ac5"
    $AuthToken  = "17d4dadfce74153d5853725143c52fd1"
    $FieldValues = @{"AccountSid" = $AccountSid;"AuthToken"=$AuthToken}

    New-AzureAutomationConnection -AutomationAccountName "MyAutomationAccount" -Name "TwilioConnection" -ConnectionTypeName "Twilio" -ConnectionFieldValues $FieldValues


## Runbook または DSC 構成での接続の使用

Runbook の DSC 構成で接続を取得、 **Get-automationconnection** コマンドレットです。  このアクティビティは、接続のさまざまなフィールドの値を取得し、そのまま返します、 [ハッシュ テーブル](http://go.microsoft.com/fwlink/?LinkID=324844) runbook または DSC 構成で適切なコマンドを使用して、これを使用することができます。

### テキストの Runbook のサンプル
次のサンプル コマンドでは、前の例の Twilio 接続を使用して Runbook からテキスト メッセージを送信する方法を示します。  ここで使用する Send-TwilioSMS アクティビティには 2 つのパラメーター セットがあり、それぞれ異なる方法で Twilio サービスに対する認証を行います。  一方は接続オブジェクトを使用し、もう一方はアカウント SID と承認トークンに別々のパラメーターを使用します。  このサンプルでは両方の方法を示します。

    $Con = Get-AutomationConnection -Name "TwilioConnection"
    $NumTo = "14255551212"
    $NumFrom = "15625551212"
    $Body = "Text from Azure Automation."

    #Send text with connection object.
    Send-TwilioSMS -Connection $Con -From $NumFrom -To $NumTo -Body $Body

    #Send text with connection properties.
    Send-TwilioSMS -AccountSid $Con.AccountSid -AuthToken $Con.AuthToken $Con -From $NumFrom -To $NumTo -Body $Body

### グラフィカルな Runbook のサンプル

追加する、 **Get-automationconnection** をグラフィカル エディターの [ライブラリ] ウィンドウ内の接続を右クリックし、グラフィカルな runbook にアクティビティ **[キャンバスに追加**します。

![](media/automation-connections/connection-add-canvas.png)

次の図は、グラフィカルな Runbook で接続を使用する例を示したものです。  これは、前に示したテキスト Runbook から Twilio を使用してテキスト メッセージを送信する例と同じものです。  この例では、 **UseConnectionObject** パラメーター、 **送信 TwilioSMS** 接続オブジェクトをサービスに対する認証に使用するアクティビティ。  A [パイプライン リンク](automation-graphical-authoring-intro.md#links-and-workflow) は Connection パラメーターは 1 つのオブジェクトを受け取るのでここで使用します。

PowerShell 式での値を使用する理由、 **に** 定数値の代わりにパラメーターが複数の番号に送信できるように、このパラメーターが文字列配列値の型が受け取ることです。  PowerShell 式を使用すると、1 つの値または配列を提供できます。

![](media/automation-connections/get-connection-object.png)

次の図は上と同じ例、 **SpecifyConnectionFields** 、AccountSid および AuthToken パラメーターを認証に接続オブジェクトを使用するのではなく個別に指定するものと想定するパラメーターのセットです。  この場合は、オブジェクト自体ではなく接続のフィールドを指定します。  

![](media/automation-connections/get-connection-properties.png)



## 関連記事:

- [グラフィカル作成でのリンク](automation-graphical-authoring-intro.md#links-and-workflow)
 

