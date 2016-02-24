<properties 
pageTitle="Azure Cloud Services でのスタートアップ タスクの実行 | Microsoft Azure" 
description="スタートアップ タスクを使用すると、アプリ用にクラウド サービス環境を準備できます。 スタートアップ タスクの動作のしくみおよびそれらを作成する方法について説明します" 
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

>[AZURE.NOTE] スタートアップ タスクは、クラウド サービス Web ロールとワーカー ロールにのみ、仮想マシンには適用されません。

## スタートアップ タスクの動作方法

スタートアップ タスクは、ロールを開始してで定義された前に実行したアクション、 [ServiceDefinition.csdef] ファイルを使用して、 [Task] 内の要素、 [Startup] 要素。 多くの場合スタートアップ タスクはバッチ ファイルですが、コンソール アプリケーションまたは PowerShell スクリプトを開始するバッチ ファイルでもかまいません。

スタートアップ タスクに情報を渡すには環境変数を使用し、スタートアップ タスクから情報を受け取るにはローカル ストレージを使用します。 たとえば、環境変数を使用してインストールするプログラムへのパスを指定でき、ファイルをローカル ストレージに書き込んでおいて後からロールで読み取ることができます。

スタートアップ タスクで指定されたディレクトリに情報およびエラーを記録できます、 **TEMP** 環境変数です。 スタートアップ タスクの中に、 **TEMP** に環境変数が解決される、 
*C:\\Resources\\temp\\[guid] です。[rolename] \\RoleTemp* ディレクトリ、クラウドで実行するとします。

再起動と再起動の間に、スタートアップ タスクを何回でも実行できます。 たとえば、スタートアップ タスクはロールのリサイクルのたびに実行され、ロールのリサイクルには再起動が含まれない場合があります。 複数回実行されても問題がないように、スタートアップ タスクを作成する必要があります。

スタートアップ タスクの終了には、 **errorlevel** (または終了コード) 0 スタートアップ プロセスを完了するのです。 スタートアップ タスクがゼロ以外で終わるかどうか **errorlevel**, 、ロールは開始されません。


## ロールのスタートアップ順序

Azure でのロールのスタートアップ手順を次に示します。

1. インスタンスとしてマークされている **開始** トラフィックを受信しません。

2. 」の手順に従ってすべてのスタートアップ タスクが実行される、 **taskType** 属性です。
    -  **単純な** タスクが同期的に実行される 1 つずつです。
    -  **バック グラウンド** と **フォア グラウンド** タスクは非同期的に並行して開始、スタートアップ タスク。  
       
    > [AZURE.WARNING] IIS が構成されていません完全に起動プロセスのスタートアップ タスク ステージ中にロールに固有のデータが使用できないようにします。 役割に固有のデータを必要とするスタートアップ タスクを使用する必要があります [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx)します。

3. ロール ホスト プロセスが開始され、サイトが IIS に作成されます。

4.  [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) メソッドが呼び出されます。

5. インスタンスとしてマークされている **準備** され、インスタンスにトラフィックをルーティングします。

6.  [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) メソッドが呼び出されます。


## スタートアップ タスクの例

スタートアップ タスクの定義、 [ServiceDefinition.csdef] ファイルの [、 **タスク** 要素。  **CommandLine** 属性は、スタートアップ バッチ ファイル出力またはコンソール コマンドのパラメーターと名前を指定、 **executionContext** 属性はスタートアップ タスクの特権レベルを指定し、 **taskType** 属性、タスクの実行方法を指定します。

この例では、環境変数で **MyVersionNumber**, をスタートアップ タスク用に作成され、値に設定"**1.0.0.0**"です。

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

次の例では、 **Startup.cmd** バッチ ファイルは「現在のバージョンは 1.0.0.0」StartupLog.txt ファイルに、TEMP 環境変数で指定したディレクトリに行を書き込みます。  `EXIT /B 0` でスタートアップ タスクが終了することにより、行、 **errorlevel** 0 です。

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [AZURE.NOTE] Visual Studio で、 **出力ディレクトリにコピー** スタートアップ バッチ ファイルのプロパティを設定する必要があります **常にコピー** 、スタートアップ バッチ ファイルが Azure でプロジェクトに正しくデプロイされるようにする (**approot\\bin** Web ロールで、 **approot** ワーカー ロールの場合)。

## タスクの属性の説明

次の属性の説明、 **タスク** 内の要素、 [ServiceDefinition.csdef] ファイル。

**commandLine** -スタートアップ タスクのコマンドラインを指定します。

- スタートアップ タスクを開始するコマンドと、オプションのコマンド ライン パラメーターです。
- 通常、これは .cmd または .bat バッチ ファイルのファイル名です。
- タスクは、展開の AppRoot\\Bin フォルダーの相対パスです。 環境変数は、タスクのパスとファイルを決定するときに展開されません。 環境変数の展開が必要な場合は、スタートアップ タスクを呼び出す小さな .cmd スクリプトを作成できます。
- コンソール アプリケーションであるか、または開始するバッチ ファイル、 [PowerShell スクリプト](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)します。

**executionContext** -スタートアップ タスクの特権レベルを指定します。 指定できる特権レベルは limited または elevated です。

- **制限**  
スタートアップ タスクは、ロールと同じ特権で実行されます。 ときに、 **executionContext** 属性を [Runtime] 要素も **制限**, 、ユーザー特権が使用されます。

- **管理者特権**  
スタートアップ タスクは、管理者特権で実行されます。 これにより、ロール自体の特権レベルを上げることなく、プログラムのインストール、IIS の構成の変更、レジストリの変更、その他の管理者レベル タスクを実行できます。  

> [AZURE.NOTE] スタートアップ タスクの特権レベルは、ロール自体と同じである必要はありません。

**taskType** -スタートアップ タスクを実行する方法を指定します。

- **単純です**  
タスクが同期的に実行で指定された順序で、一度に 1 つずつ、 [ServiceDefinition.csdef] ファイルです。 1 台 **単純な** でスタートアップ タスクが終了、 **errorlevel** 0 の場合、次の **単純な** スタートアップ タスクを実行します。 なくなる場合 **単純な** スタートアップ タスクを実行する、ロール自体が開始されます。   

    > [AZURE.NOTE] 場合、 **単純な** タスクがゼロ以外で終了 **errorlevel**, 、インスタンスはブロックされます。 その後 **単純な** スタートアップ タスクと、ロール自体が開始されません。

    バッチ ファイルがで終了することを確認する、 **errorlevel** ゼロのコマンドを実行 `EXIT /B 0` バッチ ファイルの処理の最後にします。

- **バック グラウンド**  
タスクは、ロールのスタートアップと並行して、非同期的に実行されます。

- **フォア グラウンド**  
タスクは、ロールのスタートアップと並行して、非同期的に実行されます。 主な違い、 **フォア グラウンド** と **バック グラウンド** タスクは、 **フォア グラウンド** タスクにより、ロールのリサイクルやシャット ダウン タスクが終了するまでからです。  **バック グラウンド** タスクには、この制限はありません。

## 環境変数

環境変数は、スタートアップ タスクに情報を渡すための手段です。 たとえば、インストールするプログラムを含む BLOB へのパス、ロールで使用するポート番号、スタートアップ タスクの機能を制御する設定などを設定できます。

スタートアップ タスク用の環境変数の 2 つの種類があります。静的環境変数と環境変数のメンバーに基づいた、 [RoleEnvironment] クラスです。 両方がされている、 [Environment] のセクションで、 [ServiceDefinition.csdef] ファイル、および両方の使用、 [Variable] 要素と **名** 属性です。

静的環境変数を使用して、 **値** の属性、 [Variable] 要素。 上記の例は、環境変数を作成し **MyVersionNumber** の静的な値を持つ"**1.0.0.0**"です。 別の例を作成すること、 **StagingOrProduction** 環境変数の値を手動で設定できる"**ステージング**「または」**運用**"の値に基づいて異なるスタートアップ操作を実行する、 **StagingOrProduction** 環境変数です。

RoleEnvironment クラスのメンバーによる環境変数は使用しないでください、 **値** の属性、 [Variable] 要素。 代わりに、 [RoleInstanceValue] 子要素に、適切な **xPath** 属性の値を使用して特定のメンバーによる環境変数の作成を [RoleEnvironment] クラスです。 値、 **xPath** 属性にアクセスするさまざまな [RoleEnvironment] 値を参照して [Azure の xPath 値](https://msdn.microsoft.com/library/azure/hh404006.aspx)です。



たとえば、ある環境変数を作成する"**true**"、インスタンスがコンピューティング エミュレーターで実行されている場合と"**false**"クラウドで実行するときに、次を使用して [Variable] と [RoleInstanceValue] 要素。

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
    
            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->
    
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


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Task]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Environment]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Variable]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
