<properties 
   pageTitle="復旧計画への Azure Automation Runbook の追加 | Microsoft Azure" 
   description="この記事では、Azure Site Recovery において、Azure Automation を使用して復旧計画を拡張し、Azure への復旧中に複雑なタスクを実行可能にする方法について説明します。" 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="powershell"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required" 
   ms.date="12/14/2015"
   ms.author="ruturajd@microsoft.com"/>



# 復旧計画への Azure Automation Runbook の追加

このチュートリアルは、Azure Site Recovery を Azure と統合する方法を説明します。
復旧計画に拡張性を持たを自動化できます。 復旧計画
Azure Site Recovery を使用して、セカンダリ クラウドへのレプリケーションと Azure のシナリオへのレプリケーションの両方の保護、仮想マシンの回復を調整することができます。 復旧計画により、復旧を**常に正確**、**反復可能**で、**自動化**されるようにすることもできます。 Azure Automation との統合を拡張するが、仮想マシンを Azure で失敗する場合、
回復では、プランし、runbook、ので、強力な自動化タスクを実行する機能を使用できます。

ない耳にした Azure Automation まだ、サインアップします。
[here](http://azure.microsoft.com/services/automation/) and
サンプル スクリプトをダウンロードします。
[here](http://azure.microsoft.com/documentation/scripts/). 読み取り
[Azure サイトの詳細
Recovery] (http://azure.microsoft.com/services/site-recovery/) と
復旧計画を使用した Azure への復旧を調整する方法
[here](http://azure.microsoft.com/blog/?p=166264).

このチュートリアルで見ていきます Azure Automation を統合する方法
復旧計画に runbook です。 私たちの自動化は簡単なタスクを前の手順
手動による介入を必要し、複数のステップに変換する方法を参照してください。
シングル クリックして回復操作に回復します。 方法を見ていきますが、
うまくいかない場合、単純なスクリプトのトラブルシューティングを行うことができます。

## Azure でのアプリケーションの保護

はじめに、2 台の仮想マシンで構成される単純なアプリケーションについて説明します。 ここでは、Fabrikam の HRweb アプリケーションがあります。 Fabrikam-HRweb-frontend と Fabrikam-Hrweb-backend は、Azure Site Recovery を使用して Azure で保護されている 2 台の仮想マシンです。 Azure Site Recovery を使用して仮想マシンを保護するには、次の手順に従います。

1.  仮想マシンの保護を有効にします。

2.  仮想マシンの初期レプリケーションが完了したことを確認します。
    およびレプリケートしています。

3.  初期レプリケーションが完了して、レプリケーションの状態が [保護済み] になるまで待機します。

![](media/site-recovery-runbook-automation/01.png)
--------------------------------------------------

このチュートリアルでは、Fabrikam HRweb アプリケーションの復旧計画を作成し、Azure にアプリケーションがフェールオーバーするようにします。 次に、この復旧計画を、フェールオーバーした Azure 仮想マシンにエンドポイントを作成する Runbook と統合し、ポート 80 で Web ページを提供します。

まず、アプリケーションの復旧計画を作成します。

## 復旧計画の作成

Azure にアプリケーションを復旧するには、復旧計画を作成する必要があります。
復旧計画を使用して復旧の順序を指定できます、
仮想マシン。 グループ 1 に配置された仮想マシンは回復し、
1 つは、起動し、グループ 2 の仮想マシンは次のします。

以下のような復旧計画を作成します。

![](media/site-recovery-runbook-automation/12.png)

詳細、復旧計画に関するドキュメントを読む [ここ](https://msdn.microsoft.com/library/azure/dn788799.aspx "ここ")します。

次に、Azure Automation で必要なアーティファクトを作成します。

## オートメーション アカウントとアセットの作成

Runbook を作成するには、Azure Automation アカウントが必要です。 そうしない場合
によって示された Azure Automation] タブに移動し、アカウントが既にあります。
![](media/site-recovery-runbook-automation/02.png)新しいアカウントを作成します。

1.  アカウントに識別する名前を付けます。

2.  アカウントを配置する地理的なリージョンを指定します。

ASR コンテナーと同じリージョンに、アカウントを配置することをお勧めします。

![](media/site-recovery-runbook-automation/03.png)

次に、以下のアセットをアカウントに作成します。

### アセットとしてのサブスクリプション名の追加

1.  新しい設定の追加 ![](media/site-recovery-runbook-automation/04.png) Azure オートメーションの資産とする] を選択します。 ![](media/site-recovery-runbook-automation/05.png)

2.  変数の型として **[String]** を選択します。

3.  変数名に **AzureSubscriptionName** を指定します。

    ![](media/site-recovery-runbook-automation/06.png)

4.  変数値として、実際の Azure サブスクリプション名を指定します。

    ![](media/site-recovery-runbook-automation/07_1.png)

[設定] ページからサブスクリプションの名前がわかります。
Azure ポータルで自分のアカウントです。

### アセットとしての Azure ログイン資格情報の追加

Azure Automation では、Azure PowerShell を使用して、接続します
サブスクリプションしていなければ、成果物があります。 これにする必要があります。
Microsoft アカウントまたは職場または学校のアカウントを使用して認証します。
安全に使用する、資産にアカウントの資格情報を格納することができます。
runbook。

1.  新しい設定の追加 ![](media/site-recovery-runbook-automation/04.png) Azure Automation アセットし、[ ![](media/site-recovery-runbook-automation/09.png)

2.  [資格情報の種類] には、**[Windows PowerShell 資格情報]** を選択します。

3.  名前に **AzureCredential** を指定します。

    ![](media/site-recovery-runbook-automation/10.png)

4.  サインインで使用するユーザー名とパスワードを指定します。

以上で、これらの両方の設定が、アセットで使用できます。

![](media/site-recovery-runbook-automation/11.png)

使用してサブスクリプションに接続する方法の詳細について
powershell が与えられます
[here](../install-configure-powershell.md).

次に、追加の Azure Automation で runbook を作成するが、
フェールオーバー後は、フロント エンドの仮想マシンのエンドポイントです。

## Azure Automation コンテキスト

ASR は、コンテキスト変数を作成するのに役立つ runbook に渡されます
確定的なスクリプトです。 クラウド サービスと仮想マシンの名前は予測可能ともいえますが、常にそうであるとは限りません。これは、シナリオによっては、Azure でサポートされていない文字が原因で、仮想マシン名の名前が変更されている可能性がある場合などが考えられるためです。 したがって、この情報が*コンテキスト*の一部として、ASR 復旧計画に渡されます。

以下に、コンテキスト変数がどのようになっているかについて、例を示します。

        {"RecoveryPlanName":"hrweb-recovery",
    
        "FailoverType":"Test",
    
        "FailoverDirection":"PrimaryToSecondary",
    
        "GroupId":"1",
    
        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":
    
                {"CloudServiceName":"pod02hrweb-Chicago-test",
    
                "RoleName":"Fabrikam-Hrweb-frontend-test"}
    
                }
    
        }

以下の表には、コンテキスト内の各変数の名前と説明が示されています。

 **変数名**| **説明**
---|---
 RecoveryPlanName| 実行される計画の名前です。同じスクリプトを使用して名前を基にアクションを実行するのに役に立ちます。
 FailoverType| テスト用のフェールオーバーか、計画されたフェールオーバーか、それとも計画外のフェールオーバーかを指定します。
 FailoverDirection| プライマリへの復旧か、それともセカンダリへの復旧かを指定します。
 GroupID| 復旧計画が実行される場合に、その計画内のグループ番号を識別します。
 VmMap| グループ内のすべての仮想マシンの配列です。
 VMMap キー| VM ごとの一意のキー (GUID) です。これは、該当する仮想マシンの VMM ID と同じです。
 RoleName| 復元される Azure VM の名前 です。
 CloudServiceName| 仮想マシンが作成される Azure Cloud Service の名前です。


コンテキストで VmMap キーを識別するには、ASR の [VM のプロパティ] ページに移動し、VM GUID プロパティを参照することもできます。

![](media/site-recovery-runbook-automation/13.png)

## Automation Runbook の作成

次に、フロントエンドの仮想マシンでポート 80 を開く、Runbook を作成します。

1.  名前を持つ Azure Automation アカウントで新しい runbook を作成します。
    **OpenPort80**

    ![](media/site-recovery-runbook-automation/14.png)

2.  Runbook の [作成者] ビューに移動し、ドラフト モードに移行します。

3.  まず、復旧計画のコンテキストとして使用する変数を指定します。

    ```
        param (
            [Object]$RecoveryPlanContext
        )
    ```

4.  次に、資格情報を使用してサブスクリプションに接続し、
    サブスクリプション名

    ```
        $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

        # Connect to Azure
        $AzureAccount = Add-AzureAccount -Credential $Cred
        $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
        Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    ```

    ここで、**AzureCredential** と **AzureSubscriptionName** という Azure アセットを使用することに注意してください。

5.  ここで、エンドポイントの詳細と、エンドポイントを公開する仮想マシンの GUID を指定します。 この場合は、フロントエンドの仮想マシンです。

    ```
        # Specify the parameters to be used by the script
        $AEProtocol = "TCP"
        $AELocalPort = 80
        $AEPublicPort = 80
        $AEName = "Port 80 for HTTP"
        $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
    ```

    これにより、Azure エンドポイント プロトコル、VM のローカル ポート、およびそのマップされたパブリック ポートが指定されます。 これらの変数は、仮想マシンにエンドポイントを追加する Azure のコマンドで必要なパラメーターです。 VMGUID には、稼働に必要な仮想マシンの GUID が保持されます。

6.  スクリプトは、特定の VM GUID のコンテキストを抽出ようになりましたし、
    これによって参照される仮想マシンにエンドポイントを作成します。

    ```
        #Read the VM GUID from the context
        $VM = $RecoveryPlanContext.VmMap.$VMGUID

        if ($VM -ne $null)
        {
            # Invoke pipeline commands within an InlineScript

            $EndpointStatus = InlineScript {
                # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
                # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

                $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                    Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                    Update-AzureVM
                Write-Output $Status
            }
        }
    ```

7. この操作が完了すると、発行をヒット ![](media/site-recovery-runbook-automation/20.png) 、スクリプトの実行に使用できるようにします。

以下に、完全なスクリプトを参考として示します。

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## 復旧計画へのスクリプトの追加

スクリプトの準備ができたら、先ほど作成した復旧計画に追加できます。

1.  復旧計画を作成した後にスクリプトを追加するを選択して、
    グループ 2 です。 ![](media/site-recovery-runbook-automation/15.png)

2.  スクリプト名を指定します。 これは、復旧計画内で表示するだけの目的で使用する、このスクリプトのフレンドリ名です。

3.  Azure スクリプトへのフェールオーバーでは、Azure Automation を選択します。
    アカウント名です。

4.  Azure Runbook で、作成した Runbook を選択します。

![](media/site-recovery-runbook-automation/16.png)

## プライマリ側スクリプト

Azure へのフェールオーバーを実行するときに、プライマリ側スクリプトを実行することもできます。 これらのスクリプトは、フェールオーバー中に、VMM サーバーで実行されます。 
プライマリ側スクリプトはプレシャットダウンおよびシャットダウン後のステージでのみ使用可能です。 これは、プライマリ サイトが、障害の発生時には通常使用できなくなると思われるためです。
計画外のフェールオーバー中に、プライマリ サイトの操作で選択した場合にのみ、プライマリ側スクリプトを実行を試みます。 到達できないか、またはタイムアウトが発生した場合、フェールオーバーは仮想マシンの復旧を続行します。
プライマリ側スクリプトは、Azure へのフェールオーバー中に、Azure の VMM 保護がない VMware/物理/Hyper-V の各サイトで使用できなくなります。
ただし、Azure からオンプロミスにフェールバックすると、VMware を除くすべてのターゲットでプライマリ側スクリプト (Runbook) を 使用することができます。

## 復旧計画のテスト

Runbook を計画に追加した後は、テストを行うことができます。
フェールオーバー アクションに表示します。 テストを実行することは推奨は、常に
アプリケーションと確実に復旧計画のテスト フェールオーバー
エラーはありません。

1.  復旧計画を選択し、テスト フェールオーバーを開始します。

2.  プランの実行中に、runbook があるかどうかを確認できます。
    実行されたか、その状態からです。

    ![](media/site-recovery-runbook-automation/17.png)

3.  詳細な runbook の実行状態を確認することもできます。
    runbook の Azure Automation ジョブのページです。

    ![](media/site-recovery-runbook-automation/18.png)

4.  フェールオーバーが完了したら、Runbook の実行結果とは別に、[Azure 仮想マシン] ページにアクセスし、エンドポイントを参照すると、実行が成功したかどうかを確認できます。

![](media/site-recovery-runbook-automation/19.png)

## サンプルのスクリプト

このチュートリアルでは、一般的に利用される、Azure 仮想マシンにエンドポイントを追加するためにタスクの自動化を、手順を追って説明しました。また、Azure Automation を使用して、その他の強力な自動化タスクを行うことができます。 Microsoft および Azure Automation コミュニティが提供するサンプル Runbook を使うと、独自のソリューションを作成できます。また、ユーティリティ Runbook は大きめのオートメーション タスクの構成ブロックとして使用できます。 ギャラリーから使用を開始し、Azure Site Recovery を使用して、アプリケーションの強力な 1 回のクリックの復旧プランを作成します。

## その他のリソース

[Azure Automation の概要](http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Automation の概要")

[サンプル Azure Automation スクリプト](http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "サンプル Azure Automation スクリプト")







