<properties 
pageTitle="Azure Cloud Services でのスタートアップ タスクの実行 | Microsoft Azure" 
description="スタートアップ タスクを使用すると、アプリ用にクラウド サービス環境を準備できます。スタートアップ タスクの動作のしくみおよびそれらを作成する方法について説明します" 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="12/07/2015" 
ms.author="adegeo"/>




# クラウド サービスのスタートアップ タスクを構成して実行する方法

ロールが開始する前に、スタートアップ タスクを使用して操作を実行できます。 対象となる操作としては、コンポーネントのインストール、COM コンポーネントの登録、レジストリ キーの設定、実行時間の長いプロセスの開始などがあります。
>[AZURE.NOTE] スタートアップ タスクを使用できるのはクラウド サービス Web ロールと worker ロールのみであり、Virtual Machines には使用できません。

## スタートアップ タスクの動作方法

スタートアップ タスクは、ロールを開始し、[スタートアップ] 要素内で [タスク] の要素を使用して、[ServiceDefinition.csdef] ファイルで定義されて前に実行されるアクションです。 多くの場合スタートアップ タスクはバッチ ファイルですが、コンソール アプリケーションまたは PowerShell スクリプトを開始するバッチ ファイルでもかまいません。

スタートアップ タスクに情報を渡すには環境変数を使用し、スタートアップ タスクから情報を受け取るにはローカル ストレージを使用します。 たとえば、環境変数を使用してインストールするプログラムへのパスを指定でき、ファイルをローカル ストレージに書き込んでおいて後からロールで読み取ることができます。

スタートアップ タスクでは、**TEMP** 環境変数によって指定されているディレクトリに情報およびエラーのログを記録できます。 スタートアップ タスクの中に、 **TEMP** に環境変数が解決される、 
*C:\\Resources\\temp\\[guid]. [rolename] \\RoleTemp* ディレクトリ、クラウドで実行するとします。

再起動と再起動の間に、スタートアップ タスクを何回でも実行できます。 たとえば、スタートアップ タスクはロールのリサイクルのたびに実行され、ロールのリサイクルには再起動が含まれない場合があります。 複数回実行されても問題がないように、スタートアップ タスクを作成する必要があります。

スタートアップ プロセスが完了するには、スタートアップ タスクが **errorlevel** (終了コード) ゼロで終了する必要があります。 スタートアップ タスクがゼロ以外の **errorlevel** で終了すると、ロールは開始しません。


## ロールのスタートアップ順序

Azure でのロールのスタートアップ手順を次に示します。

1. インスタンスは**開始中**とマークされ、トラフィックを受け取りません。

2. **taskType** 属性に従って、すべてのスタートアップ タスクが実行されます。
    - **単純な**タスクは、一度に 1 つずつ同期的に実行されます。
    - **バック グラウンド** タスクと**フォアグラウンド** タスクは、スタートアップ タスクと並列に非同期的に開始されます。
    > [AZURE.WARNING] IIS はスタートアップ プロセスのスタートアップ タスク ステージの間に完全に構成されない場合があるので、ロール固有のデータを使用できないことがあります。 役割に固有のデータを必要とするスタートアップ タスクを使用する必要があります [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx)します。

3. ロール ホスト プロセスが開始され、サイトが IIS に作成されます。

4. [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) メソッドが呼び出されます。

5. インスタンスは**準備完了**とマークされ、トラフィックがインスタンスにルーティングされるようになります。

6. [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) メソッドが呼び出されます。


## スタートアップ タスクの例

スタートアップ タスクは [ServiceDefinition.csdef] ファイルで定義されて、 **タスク** 要素。 **commandLine** 属性では、スタートアップ バッチ ファイルまたはコンソール コマンドの名前とパラメーターを指定します。**executionContext** 属性では、スタートアップ タスクの特権レベルを指定します。**taskType** 属性では、タスクの実行方法を指定します。

この例では、環境変数 **MyVersionNumber** をスタートアップ タスク用に作成し、値を "**1.0.0.0**" に設定しています。

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

次の例では、**Startup.cmd** バッチ ファイルは、TEMP 環境変数で指定されているディレクトリの StartupLog.txt ファイルに、"The current version is 1.0.0.0" という行を書き込みます。  `EXIT/B 0` でスタートアップ タスクが終了することにより、行、 **errorlevel** 0 です。

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [AZURE.NOTE] Visual Studio で、 **出力ディレクトリにコピー** スタートアップ バッチ ファイルのプロパティを設定する必要があります **常にコピー** 、スタートアップ バッチ ファイルが Azure でプロジェクトに正しくデプロイされるようにする (**approot\\bin** Web ロールで、 **approot** ワーカー ロールの場合)。

## タスクの属性の説明

次の属性の説明、 **タスク** [ServiceDefinition.csdef] ファイル内の要素。

**commandLine** -スタートアップ タスクのコマンド ラインを指定します。

- スタートアップ タスクを開始するコマンドと、オプションのコマンド ライン パラメーターです。
- 通常、これは .cmd または .bat バッチ ファイルのファイル名です。
- タスクは、展開の AppRoot\\Bin フォルダーの相対パスです。 環境変数は、タスクのパスとファイルを決定するときに展開されません。 環境変数の展開が必要な場合は、スタートアップ タスクを呼び出す小さな .cmd スクリプトを作成できます。
- コンソール アプリケーションであるか、または開始するバッチ ファイル、 [PowerShell スクリプト](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)します。

**executionContext** -スタートアップ タスクの特権レベルを指定します。 指定できる特権レベルは limited または elevated です。

- **制限**  
スタートアップ タスクは、ロールと同じ特権で実行されます。 ときに、 **executionContext** 属性 [ランタイム] の要素も **制限**, 、ユーザー特権が使用されます。

- **elevated**  
スタートアップ タスクは、管理者特権で実行されます。 これにより、ロール自体の特権レベルを上げることなく、プログラムのインストール、IIS の構成の変更、レジストリの変更、その他の管理者レベル タスクを実行できます。

> [AZURE.NOTE] スタートアップ タスクの特権レベルは、ロール自体と同じでなくてもかまいません。

**taskType** -スタートアップ タスクを実行する方法を指定します。

- **単純です**  
タスクは、[ServiceDefinition.csdef] ファイルで指定された順序で、一度に 1 つずつ同期的に実行されます。 ある **simple** スタートアップ タスクが 0 の **errorlevel** で終了すると、次の **simple** スタートアップ タスクが実行されます。 それ以上実行する **simple** スタートアップ タスクがない場合は、ロール自体が開始されます。
    > [AZURE.NOTE] **simple** タスクが 0 以外の **errorlevel** で終了した場合は、インスタンスがブロックされます。 後続の **simple** スタートアップ タスクおよびロール自体は開始されません。

    バッチ ファイルがで終了することを確認する、 **errorlevel** ゼロのコマンドを実行 `EXIT/B 0` バッチ ファイルの処理の最後にします。

- **バック グラウンド**  
タスクは、ロールのスタートアップと並行して、非同期的に実行されます。

- **フォア グラウンド**  
タスクは、ロールのスタートアップと並行して、非同期的に実行されます。 **foreground** タスクと **background** タスクの重要な違いは、**foreground** タスクではタスクが終了するまでロールがリサイクルまたはシャットダウンされなくなることです。 **background** タスクにはこのような制限はありません。

## 環境変数

環境変数は、スタートアップ タスクに情報を渡すための手段です。 たとえば、インストールするプログラムを含む BLOB へのパス、ロールで使用するポート番号、スタートアップ タスクの機能を制御する設定などを設定できます。

スタートアップ タスク用の環境変数の 2 つの種類があります。静的環境変数と、[RoleEnvironment] クラスのメンバーによる環境変数です。 [ServiceDefinition.csdef] ファイルの [環境] セクションには両方と、どちらも [Variable] の要素を使用し、 **名** 属性です。

静的環境変数を使用して、 **値** [変数] の要素の属性です。 上の例では、**MyVersionNumber** という名前の環境変数を作成し、静的な値 "**1.0.0.0**" を設定しています。 もう 1 つの例として、**StagingOrProduction** という名前の環境変数を作成し、手動で値 "**staging**" または "**production**" を設定して、**StagingOrProduction** 環境変数の値に基づいて異なるスタートアップ アクションを実行できます。

RoleEnvironment クラスのメンバーによる環境変数は使用しないでください、 **値** [変数] の要素の属性です。 代わりに、[RoleInstanceValue] 子要素の適切な **xPath** 属性値、[RoleEnvironment] クラスの特定のメンバーによる環境変数の作成に使用されます。 値、 **xPath** さまざまな [RoleEnvironment] の値にアクセスする属性を参照して [Azure の xPath 値](https://msdn.microsoft.com/library/azure/hh404006.aspx)します。



たとえば、ある環境変数を作成する"**true**"、インスタンスがコンピューティング エミュレーターで実行されている場合と"**false**"クラウドで実行するときに、次を使用して [変数] および [RoleInstanceValue] 要素。

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>

            

            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>

        </Environment>
    </Task>
</Startup>
```

## 次のステップ

いくつか実行する方法について説明 [スタートアップ タスクの一般的な](cloud-services-startup-tasks-common.md) をクラウド サービスです。

[パッケージ](cloud-services-model-and-package.md) 、クラウド サービスです。



[servicedefinition.csdef]: cloud-services-model-and-package.md#csdef 
[task]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task 
[startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup 
[runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime 
[environment]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment 
[variable]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable 
[roleinstancevalue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue 
[roleenvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx 

