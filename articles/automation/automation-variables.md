<properties 
   pageTitle="Azure Automation の変数資産 | Microsoft Azure"
   description="変数アセットとは、すべての runbook と Azure Automation での DSC 構成に使用可能な値です。この記事では、変数の詳細およびテキスト作成とグラフィカル作成の両方で変数を使用する方法について説明します。"
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


# Azure Automation での変数アセット

変数アセットとは、すべての runbook と automation アカウントでの DSC 構成に使用可能な値です。 作成、変更、および Azure のポータルでは、Windows PowerShell から取得される、runbook または DSC 構成します。 Automation 変数は、次のシナリオで役立ちます。

- 複数の Runbook または DSC 構成で値を共有する。

- 同じ runbook または DSC 構成から複数のジョブ間で値を共有します。

- ポータルまたは runbook または DSC 構成で使用される Windows PowerShell コマンドラインから値を管理します。

常に、runbook または DSC 構成が失敗した場合でも使用できるように、automation 変数が保存されます。 値を同じ runbook または DSC 構成、次回の実行によって、別の作業で使用して、またはを使用する 1 つの runbook によって設定することもできます。

変数が作成されると、暗号化して保存するように指定できます。 変数が暗号化と Azure Automation に安全に保存されてからその値を取得できません、 [Get-azureautomationvariable](http://msdn.microsoft.com/library/dn913772.aspx) コマンドレット、Azure PowerShell モジュールの一部として同梱されています。 暗号化された値を取得できる唯一の方法は、 **Get-automationvariable** runbook または DSC 構成で動作します。
>[AZURE.NOTE]Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables. これらの資産は暗号化され、生成される一意のキーを使用して、各オートメーション アカウントの Azure Automation に保存されます。 This key is encrypted by a master certificate and stored in Azure Automation. セキュリティで保護された資産を格納する前に、Automation アカウントのキーがマスター証明書を使用して復号化され、資産の暗号化に使用されます。

## 変数の型

Azure ポータルで変数を作成する場合、変数値を入力するための適切な制御をポータルに表示できるように、ドロップダウン リストからデータ型を指定する必要があります。 変数は、このデータ型に制限されませんが、別の型の値を指定する場合は、Windows PowerShell を使用して変数を設定する必要があります。 指定した場合 **が定義されていない**, 、変数の値に設定する **$null**, で値を設定する必要があります、 [Set-azureautomationvariable](http://msdn.microsoft.com/library/dn913767.aspx) コマンドレットまたは **Set-automationvariable** アクティビティ。 ポータルでは複雑な変数の型の値を作成したり、変更したりすることはできませんが、Windows PowerShell を使用すれば、任意の型の値を指定することが可能です。 複合型として返される、 [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx)します。

配列またはハッシュ テーブルを作成し、それを変数に保存することによって、複数の値を 1 つの変数に格納することができます。

## コマンドレットとワークフローのアクティビティ

Windows PowerShell で Automation 変数を作成および管理するには、次のテーブルのコマンドレットを使用します。 一部として出荷される、 [Azure PowerShell モジュール](../powershell-install-configure.md) Automation の runbook と DSC 構成で使用することができます。

| コマンドレット| 説明|
|:---|:---|
| [Get AzureAutomationVariable](http://msdn.microsoft.com/library/dn913772.aspx)| 既存の変数の値を取得します。|
| [新しい AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx)| 新しい変数を作成し、その値を設定します。|
| [削除 AzureAutomationVariable](http://msdn.microsoft.com/library/dn913775.aspx)| 既存の変数を削除します。|
| [Set-azureautomationvariable](http://msdn.microsoft.com/library/dn913767.aspx)| 既存の変数の値を設定します。|

次のテーブルのワークフロー アクティビティは、Runbook で Automation 変数にアクセスするために使用されます。 のみ使用でき、runbook または DSC 構成で使用するため、Azure PowerShell モジュールの一部として添付されていません。

| ワークフロー アクティビティ| 説明|
|:---|:---|
| Get-AutomationVariable| 既存の変数の値を取得します。|
| Set-AutomationVariable| 既存の変数の値を設定します。|

>[AZURE.NOTE] 変数の使用を避ける必要がありますの – Name パラメーターで **Get-automationvariable**  runbook またはデザイン時に runbook または DSC 構成と Automation 変数の間の依存関係を見つけにくくなるため、DSC 構成します。

## 新しい Automation 変数の作成

### Azure ポータルで新しい変数を作成するには

1. Automation アカウントから、ウィンドウ上部にある **[資産]** をクリックします。
1. ウィンドウの下部にある **[設定の追加]** をクリックします。
1. **[変数の追加]** をクリックします。
1. ウィザードを完了し、チェック ボックスをクリックして新しい変数を保存します。


### Azure プレビュー ポータルで新しい変数を作成するには

1. Automation アカウントから、**[資産]** 部分をクリックして **[資産]** ブレードを開きます。
1. **[変数]** 部分をクリックして **[変数]** ブレードを開きます。
1. ブレード上部の **[変数の追加]** をクリックします。
1. フォームに入力し、**[作成]** をクリックして新しい変数を保存します。


### Windows PowerShell で新しい変数を作成するには

[New-azureautomationvariable](http://msdn.microsoft.com/library/dn913771.aspx) コマンドレットは、新しい変数を作成し、その初期値を設定します。 使用して値を取得する [Get-azureautomationvariable](http://msdn.microsoft.com/library/dn913772.aspx)します。 値が単純型である場合、その同じ型が返されます。 値が複合型である場合、**PSCustomObject** が返されます。

次のサンプル コマンドは、文字列型の変数を作成してから、その値を返す方法を示しています。


    New-AzureAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' –Encrypted $false –Value 'My String'
    $string = (Get-AzureAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

次のサンプル コマンドは、複合型の変数を作成してから、そのプロパティを返す方法を示しています。 この場合、**Get-AzureVM** から仮想マシンのオブジェクトが使用されます。

    $vm = Get-AzureVM –ServiceName "MyVM" –Name "MyVM"
    New-AzureAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $ vmValue.Name
    $vmIpAddress = $ vmValue.IpAddress

## Runbook または DSC 構成で変数を使用する

使用して、 **Set-automationvariable** runbook または DSC 構成でオートメーション変数の値を設定するアクティビティと **Get-automationvariable** に取得します。 使用しないでください、 **Set-azureautomationvariable** または  **Get-azureautomationvariable** DSC 構成ワークフローのアクティビティよりも低効率であるため、runbook のコマンドレットです。 また、**Get-AzureAutomationVariable** を使用して、セキュリティで保護された変数の値を取得することもできません。 Runbook または DSC 構成内での新しい変数を作成する唯一の方法が使用するが、 [New-azureautomationvariable](http://msdn.microsoft.com/library/dn913771.aspx)  コマンドレットです。


### テキスト形式の Runbook のサンプル

#### 変数からの単純値の設定と取得

次のサンプル コマンドは、テキスト形式の Runbook で変数を設定し取得する方法を示しています。 このサンプルでは、*NumberOfIterations* および *NumberOfRunnings* という整数型の変数と、*SampleMessage* という文字列型の変数が既に作成されていることを前提にしています。

    $NumberOfIterations = Get-AutomationVariable -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AutomationVariable -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AutomationVariable –Name NumberOfRunnings –Value (NumberOfRunngs += 1)

#### 変数での複雑なオブジェクトの設定および取得

次のサンプル コードは、テキスト形式の Runbook で複合値で変数を更新する方法を示しています。 このサンプルでは、Azure 仮想マシンは **Get-AzureVM** で取得され、既存の Automation 変数に保存されます。 説明したよう [変数の型](#variable-types), 、これは PSCustomObject として格納します。

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm

次のコードでは、この値は変数から取得され、仮想マシンを起動するために使用されます。

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }

#### 変数でのコレクションの設定および取得

次のサンプル コードは、テキスト形式の Runbook で複合値のコレクションで変数を使用する方法を示しています。 このサンプルでは、複数の Azure 仮想マシンは **Get-AzureVM** で取得され、既存の Automation 変数に保存されます。 説明したよう [変数の型](#variable-types), 、これは Pscustomobject のコレクションとして格納します。

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

次のコードでは、このコレクションは変数から取得され、各仮想マシンを起動するために使用されます。

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }

### グラフィカルな Runbook のサンプル

グラフィカルな Runbook で、グラフィカル エディターの [ライブラリ] ウィンドウの変数を右クリックして目的のアクティビティを選択することにより、**Get-AutomationVariable** または **Set-AutomationVariable** を追加します。

![キャンバスへの変数の追加](media/automation-variables/variable-add-canvas.png)

#### 変数での値の設定

次の図は、グラフィカルな Runbook の単純値で変数を更新するサンプル アクティビティを示しています。 このサンプルでは、1 つの Azure 仮想マシンは **Get-AzureVM** で取得され、コンピューター名は既存の Automation 変数に文字列型で保存されます。 ともかくかどうか、 [リンクがパイプラインかシーケンス](automation-graphical-authoring-intro.md#links-and-workflow) 出力での 1 つのオブジェクトのみを聞こえてくるためです。

![単純変数の設定](media/automation-variables/set-simple-variable.png)

次の図は、グラフィカルな Runbook の複合値で変数を更新するために使用するアクティビティを示しています。 前述の例から唯一変更された点は、単にオブジェクトのプロパティでなく、オブジェクトが格納されるようにするため、**Set-AutomationVariable** アクティビティで**アクティビティの出力**の**フィールド パス**を指定していないことです。 説明したよう [変数の型](#variable-types), 、これは PSCustomObject として格納します。

![複合変数の設定](media/automation-variables/set-complex-variable.png)

次の図は、複数の仮想マシンが変数に保存された状態での、前述の例と同様の機能を示しています。 A [シーケンス リンク](automation-graphical-authoring-intro.md#links-and-workflow) ここで使用する必要がありますように、 **Set-automationvariable** アクティビティが 1 つのコレクションとして仮想マシンのセット全体を受信します。 場合、 [パイプライン リンク](automation-graphical-authoring-intro.md#links-and-workflow) を使用している、 **Set-automationvariable** アクティビティは実行とは別にオブジェクトごとに、結果は、コレクションの最後の仮想マシンのみを保存していること。 説明したよう [変数の型](#variable-types), 、これは Pscustomobject のコレクションとして格納します。

![複合コレクション変数の設定](media/automation-variables/set-complex-variable-collection.png)

#### 変数からの値の取得

次の図は、グラフィカルな Runbook で変数を取得および使用するサンプル アクティビティを示しています。 最初のアクティビティは、前の例で変数に保存された仮想マシンを取得します。 リンクがある必要があります、 [パイプライン](automation-graphical-authoring-intro.md#links-and-workflow) ように、 **Start-azurevm** アクティビティの実行から送信されたオブジェクトごとに 1 回、 **Get-automationvariable** アクティビティ。 変数に格納されているのが 1 つのオブジェクトか複数のオブジェクトかに関係なく、同じように動作します。 **Start-AzureVM** アクティビティは、各仮想マシンを表す PSCustomObject のプロパティを使用します。

![複合変数の取得](media/automation-variables/get-complex-variable.png)

次の図は、グラフィカルな Runbook で変数に格納されているオブジェクトをフィルター処理する方法を示しています。 A [条件](automation-graphical-authoring-intro.md#links-and-workflow) 変数が設定されたときに停止されたバーチャル マシンのみをフィルター処理の前の例でのリンクに追加します。

![フィルター処理された複合変数の取得](media/automation-variables/get-complex-variable-filter.png)


## 関連記事:

- [グラフィカル作成でのリンク](automation-graphical-authoring-intro.md#links-and-workflow)






