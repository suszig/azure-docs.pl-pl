<properties 
   pageTitle="Azure Automation の変数資産 | Microsoft Azure"
   description="変数アセットとは、Azure Automation のすべての Runbook と DSC 構成に使用できる値です。  この記事では、変数の詳細およびテキスト作成とグラフィカル作成の両方で変数を使用する方法について説明します。"
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

変数アセットとは、オートメーション アカウント内のすべての Runbook と DSC 構成に使用できる値です。 これらは、Azure ポータル、Windows PowerShell、Runbook または DSC 構成内から作成、変更、取得することができます。 Automation 変数は、次のシナリオで役立ちます。

- 複数の Runbook または DSC 構成で値を共有する。

- 同じ Runbook または DSC 構成の複数のジョブで値を共有する。

- 管理ポータルから値を管理する、または Runbook または DSC 構成で使用される Windows PowerShell コマンドラインから値を管理する。

Automation 変数は、Runbook または DSC 構成でエラーが発生した場合でも継続して使用できるように保存されます。  また、1 つの Runbook または DSC 構成で設定された値を他のユーザーが使用できるようにしたり、次回実行時に同じ Runbook で使用したりすることができます。

変数が作成されると、暗号化して保存するように指定できます。  変数が暗号化と Azure Automation に安全に保存されてからその値を取得できません、 [Get-azureautomationvariable](http://msdn.microsoft.com/library/dn913772.aspx) コマンドレット、Azure PowerShell モジュールの一部として同梱されています。  暗号化された値を取得できる唯一の方法は、 **Get-automationvariable** runbook または DSC 構成で動作します。

>[AZURE.NOTE]Azure Automation でセキュリティで保護された資産には、資格情報、証明書、接続、および暗号化された変数が含まれます。 これらの資産は、各 Automation アカウント用に生成された一意のキーを使用して暗号化され、Azure Automation に保存されます。 このキーはマスター証明書によって暗号化され、Azure Automation に保存されます。 セキュリティで保護された資産を格納する前に、オートメーション アカウントのキーがマスター証明書を使用して復号化され、資産の暗号化に使用されます。

## 変数の型

Azure ポータルで変数を作成する場合、変数値を入力するための適切な制御をポータルに表示できるように、ドロップダウン リストからデータ型を指定する必要があります。 変数は、このデータ型に制限されませんが、別の型の値を指定する場合は、Windows PowerShell を使用して変数を設定する必要があります。 指定した場合 **が定義されていない**, 、変数の値に設定する **$null**, で値を設定する必要があります、 [Set-azureautomationvariable](http://msdn.microsoft.com/library/dn913767.aspx) コマンドレットまたは **Set-automationvariable** アクティビティ。  ポータルでは複雑な変数の型の値を作成したり、変更したりすることはできませんが、Windows PowerShell を使用すれば、任意の型の値を指定することが可能です。 複合型として返される、 [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx)します。

配列またはハッシュ テーブルを作成し、それを変数に保存することによって、複数の値を 1 つの変数に格納することができます。

## コマンドレットとワークフローのアクティビティ

Windows PowerShell で Automation 変数を作成および管理するには、次のテーブルのコマンドレットを使用します。 一部として出荷される、 [Azure PowerShell モジュール](../powershell-install-configure.md) Automation の runbook と DSC 構成で使用することができます。

|コマンドレット|説明|
|:---|:---|
|[Get-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913772.aspx)|既存の変数の値を取得します。|
|[New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx)|新しい変数を作成し、その値を設定します。|
|[Remove-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913775.aspx)|既存の変数を削除します。|
|[Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx)|既存の変数の値を設定します。|

次のテーブルのワークフロー アクティビティは、Runbook で Automation 変数にアクセスするために使用されます。 これらは、Runbook または DSC 構成でのみ使用できるものであり、Azure PowerShell モジュールには付属していません。

|ワークフロー アクティビティ|説明|
|:---|:---|
|Get-AutomationVariable|既存の変数の値を取得します。|
|Set-AutomationVariable|既存の変数の値を設定します。|

>[AZURE.NOTE] 変数の使用を避ける必要がありますの – Name パラメーターで **Get-automationvariable**  runbook またはデザイン時に runbook または DSC 構成と Automation 変数の間の依存関係を見つけにくくなるため、DSC 構成します。

## 新しい Automation 変数の作成

### Azure ポータルで新しい変数を作成するには

1. オートメーション アカウントからクリックして **資産** ウィンドウの上部にあります。
1. At the bottom of the window, click **Add Setting**.
1. クリックして **変数を追加**します。
1. ウィザードを完了し、チェック ボックスをクリックして新しい変数を保存します。


### Azure プレビュー ポータルで新しい変数を作成するには

1. クリックして、オートメーション アカウントから、 **資産** を開くには一部、 **資産** ブレードです。
1. クリックして、 **変数** を開くには一部、 **変数** ブレードです。
1. クリックして **変数を追加する** ブレードの上部にあります。
1. フォームに入力し、クリックして **作成** 新しい変数を保存します。


### Windows PowerShell で新しい変数を作成するには

 [New-azureautomationvariable](http://msdn.microsoft.com/library/dn913771.aspx) コマンドレットは、新しい変数を作成し、その初期値を設定します。 使用して値を取得する [Get-azureautomationvariable](http://msdn.microsoft.com/library/dn913772.aspx)します。 値が単純型である場合、その同じ型が返されます。 複合型である場合、 **PSCustomObject** が返されます。

次のサンプル コマンドは、文字列型の変数を作成してから、その値を返す方法を示しています。


    New-AzureAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' –Encrypted $false –Value 'My String'
    $string = (Get-AzureAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

次のサンプル コマンドは、複合型の変数を作成してから、そのプロパティを返す方法を示しています。 この場合は、仮想マシンのオブジェクトから **Get-azurevm** を使用します。

    $vm = Get-AzureVM –ServiceName "MyVM" –Name "MyVM"
    New-AzureAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $ vmValue.Name
    $vmIpAddress = $ vmValue.IpAddress



## Runbook または DSC 構成で変数を使用する

使用して、 **Set-automationvariable** runbook または DSC 構成でオートメーション変数の値を設定するアクティビティと **Get-automationvariable** に取得します。  使用しないでください、 **Set-azureautomationvariable** または  **Get-azureautomationvariable** DSC 構成ワークフローのアクティビティよりも低効率であるため、runbook のコマンドレットです。  使用してセキュリティで保護された変数の値を取得できません **Get-azureautomationvariable**します。  Runbook または DSC 構成内での新しい変数を作成する唯一の方法が使用するが、 [New-azureautomationvariable](http://msdn.microsoft.com/library/dn913771.aspx)  コマンドレットです。


### テキスト形式の Runbook のサンプル

#### 変数からの単純値の設定と取得

次のサンプル コマンドは、テキスト形式の Runbook で変数を設定し取得する方法を示しています。 このサンプルのことが前提と整数型の変数がという名前の *NumberOfIterations* と *NumberOfRunnings* という文字列型の変数と、 *SampleMessage* が既に作成されています。

    $NumberOfIterations = Get-AutomationVariable -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AutomationVariable -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AutomationVariable –Name NumberOfRunnings –Value (NumberOfRunngs += 1)


#### 変数での複雑なオブジェクトの設定および取得

次のサンプル コードは、テキスト形式の Runbook で複合値で変数を更新する方法を示しています。 このサンプルと Azure の仮想マシンが取得 **Get-azurevm** され、既存の Automation 変数に保存します。  説明したよう [変数の型](#variable-types), 、これは PSCustomObject として格納します。

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm


次のコードでは、この値は変数から取得され、仮想マシンを起動するために使用されます。

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### 変数でのコレクションの設定および取得

次のサンプル コードは、テキスト形式の Runbook で複合値のコレクションで変数を使用する方法を示しています。 このサンプルで複数の Azure 仮想マシンが取得 **Get-azurevm** され、既存の Automation 変数に保存します。  説明したよう [変数の型](#variable-types), 、これは Pscustomobject のコレクションとして格納します。

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

追加するグラフィカルな runbook で、 **Get-automationvariable** または **Set-automationvariable** をグラフィカル エディターの [ライブラリ] ウィンドウの変数を右クリックし、必要なアクティビティを選択しています。

![キャンバスへの変数の追加](media/automation-variables/variable-add-canvas.png)

#### 変数での値の設定

次の図は、グラフィカルな Runbook の単純値で変数を更新するサンプル アクティビティを示しています。 このサンプルを単一の Azure 仮想マシンを取得 **Get-azurevm** とコンピューター名は、String 型の既存の Automation 変数に保存します。  ともかくかどうか、 [リンクがパイプラインかシーケンス](automation-graphical-authoring-intro.md#links-and-workflow) 出力での 1 つのオブジェクトのみを聞こえてくるためです。

![単純変数の設定](media/automation-variables/set-simple-variable.png)

次の図は、グラフィカルな Runbook の複合値で変数を更新するために使用するアクティビティを示しています。 前の例から唯一変更を指定しない、 **フィールドのパスは** の **アクティビティの出力** 上、 **Set-automationvariable** アクティビティ オブジェクトのプロパティだけでなく、オブジェクトが格納されているようにします。  説明したよう [変数の型](#variable-types), 、これは PSCustomObject として格納します。

![複合変数の設定](media/automation-variables/set-complex-variable.png)

次の図は、複数の仮想マシンが変数に保存された状態での、前述の例と同様の機能を示しています。  A [シーケンス リンク](automation-graphical-authoring-intro.md#links-and-workflow) ここで使用する必要がありますように、 **Set-automationvariable** アクティビティが 1 つのコレクションとして仮想マシンのセット全体を受信します。  場合、 [パイプライン リンク](automation-graphical-authoring-intro.md#links-and-workflow) を使用している、 **Set-automationvariable** アクティビティは実行とは別にオブジェクトごとに、結果は、コレクションの最後の仮想マシンのみを保存していること。  説明したよう [変数の型](#variable-types), 、これは Pscustomobject のコレクションとして格納します。

![複合コレクション変数の設定](media/automation-variables/set-complex-variable-collection.png)

#### 変数からの値の取得

次の図は、グラフィカルな Runbook で変数を取得および使用するサンプル アクティビティを示しています。  最初のアクティビティは、前の例で変数に保存された仮想マシンを取得します。  リンクがある必要があります、 [パイプライン](automation-graphical-authoring-intro.md#links-and-workflow) ように、 **Start-azurevm** アクティビティの実行から送信されたオブジェクトごとに 1 回、 **Get-automationvariable** アクティビティ。  変数に格納されているのが 1 つのオブジェクトか複数のオブジェクトかに関係なく、同じように動作します。   **Start-azurevm** アクティビティは、各仮想マシンを表す PSCustomObject のプロパティを使用します。 

![複合変数の取得](media/automation-variables/get-complex-variable.png)

次の図は、グラフィカルな Runbook で変数に格納されているオブジェクトをフィルター処理する方法を示しています。  A [条件](automation-graphical-authoring-intro.md#links-and-workflow) 変数が設定されたときに停止されたバーチャル マシンのみをフィルター処理の前の例でのリンクに追加します。

![フィルター処理された複合変数の取得](media/automation-variables/get-complex-variable-filter.png)


## 関連記事:

- [グラフィカル作成でのリンク](automation-graphical-authoring-intro.md#links-and-workflow)
 

