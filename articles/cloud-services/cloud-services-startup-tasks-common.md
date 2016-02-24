<properties 
pageTitle="Cloud Services 共通のスタートアップ タスク | Microsoft Azure" 
description="クラウド サービスの Web ロールや worker ロールで実行できる共通のスタートアップ タスクの例を示します。" 
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

# クラウド サービス共通のスタートアップ タスク

この記事では、クラウド サービスで実行できる共通のスタートアップ タスクの例を示します。 スタートアップ タスクを使用して、ロールを開始する前の操作を実行できます。 対象となる操作としては、コンポーネントのインストール、COM コンポーネントの登録、レジストリ キーの設定、実行時間の長いプロセスの開始などがあります。 

参照してください [今回](cloud-services-startup-tasks.md) 、スタートアップ タスクの動作とスタートアップ タスクを定義するエントリの作成方法を具体的に理解します。

ここで紹介するタスクの多くで使用するもの 

>[AZURE.NOTE] スタートアップ タスクは、クラウド サービス Web ロールとワーカー ロールにのみ、仮想マシンには適用されません。


## ロールを開始する前に環境変数を定義する

全体のロール用の環境変数を定義するには追加することで、 [Runtime] サービス定義ファイル内のロールの定義に要素。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">
        ...
        <Runtime>
            <Environment>
                <Variable name="MyEnvironmentVariable" value="MyVariableValue" />
            </Environment>
        </Runtime>
    </WebRole>
</ServiceDefinition>
```

他のタスクで共有されていないいる環境変数が特定のタスクに対して定義されている場合は、使用すると、 [Environment] 内の要素、 [Task] 要素。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
                <Environment>
                    <Variable name="MyEnvironmentVariable" value="MyVariableValue" />
                </Environment>
            </Task>
        </Startup>
    </WebRole>
</ServiceDefinition>
```

変数を使用することも、 [有効な Azure の XPath 値](https://msdn.microsoft.com/library/azure/hh404006.aspx) 展開について何かを参照します。 使用する代わりに、 `value` 属性を定義、 [RoleInstanceValue] 子要素です。

```xml
<Variable name="PathToStartupStorage">
    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
</Variable>
```


## AppCmd.exe を使用して IIS スタートアップを構成する

 [AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) のスタートアップ時に Azure の IIS 設定を管理するコマンド ライン ツールを使用することができます。 *AppCmd.exe* Azure でスタートアップ タスクで使用する構成設定に便利でコマンド ライン アクセスを提供します。 使用して *AppCmd.exe*, 、web サイトの設定を追加、変更、またはアプリケーションやサイトの削除できます。

ただしでの使用に注意する点がある *AppCmd.exe* スタートアップ タスクとして。

- スタートアップ タスクは、再起動の間に複数回実行されることがあります。 これは、ロールがリサイクルされる場合などに発生することがあります。
- いくつか *AppCmd.exe* アクションは、複数回にわたって実行される場合、エラーが発生します。 セクションを追加しようとしています。 *Web.config* 2 回、エラーが発生します。
- ゼロ以外の終了コードを返す場合、スタートアップ タスクが失敗するか、 **errorlevel**します。 これが起こる場合 *AppCmd.exe* 、エラーが発生します。

挙げた理由から、これは通常を確認することが賢明、 **errorlevel** 呼び出した後 *AppCmd.exe*, への呼び出しをラップする場合の対処方法は簡単である *AppCmd.exe* で、 *.cmd* ファイルです。 既知を検出した場合は **errorlevel** 応答を無視できます、それが返されます。 これについては、次の例で示します。

によって返される errorlevel *AppCmd.exe* winerror.h ファイルに記載されており、上でも確認できます [MSDN](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx)します。

### 例

この例に JSON の圧縮セクションと圧縮エントリを追加、 *Web.config* エラー処理とログを持つファイル。

関連するセクション、 [ServiceDefinition.csdef] ファイルもここでは、表示される設定が含まれて、 [executionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) 属性を `elevated` ように *AppCmd.exe* で設定を変更するための十分なアクセス許可、 *Web.config* ファイル。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WebRole>
</ServiceDefinition>
```

Startup.cmd バッチ ファイルでは、AppCmd.exe を使用する JSON の圧縮セクションと圧縮エントリを追加する、 *Web.config* ファイルです。 予期される **errorlevel** 183 の検証を使用してゼロに設定されています。EXE コマンド ライン プログラムを使用します。 予期しない errorlevel は StartupErrorLog.txt に記録されます。

    REM   *** Add a compression section to the Web.config file. ***
    %windir%\system32\inetsrv\appcmd set config /section:urlCompression /doDynamicCompression:True /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1
    
    REM   ERRORLEVEL 183 occurs when trying to add a section that already exists. This error is expected if this
    REM   batch file were executed twice. This can occur and must be accounted for in a Azure startup
    REM   task. To handle this situation, set the ERRORLEVEL to zero by using the Verify command. The Verify
    REM   command will safely set the ERRORLEVEL to zero.
    IF %ERRORLEVEL% EQU 183 DO VERIFY > NUL
    
    REM   If the ERRORLEVEL is not zero at this point, some other error occurred.
    IF %ERRORLEVEL% NEQ 0 (
        ECHO Error adding a compression section to the Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
        GOTO ErrorExit
    )
    
    REM   *** Add compression for json. ***
    %windir%\system32\inetsrv\appcmd set config  -section:system.webServer/httpCompression /+"dynamicTypes.[mimeType='application/json; charset=utf-8',enabled='True']" /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1
    IF %ERRORLEVEL% EQU 183 VERIFY > NUL
    IF %ERRORLEVEL% NEQ 0 (
        ECHO Error adding the JSON compression type to the Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
        GOTO ErrorExit
    )
    
    REM   *** Exit batch file. ***
    EXIT /b 0
    
    REM   *** Log error and exit ***
    :ErrorExit
    REM   Report the date, time, and ERRORLEVEL of the error.
    DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
    TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
    ECHO An error occurred during startup. ERRORLEVEL = %ERRORLEVEL% >> "%TEMP%\StartupLog.txt" 2>&1
    EXIT %ERRORLEVEL%


## ファイアウォール規則を追加する

Azure では、実質的に 2 つのファイアウォールがあります。 最初のファイアウォールは、仮想マシンと外部世界の間の接続を制御します。 これによって制御されますが、 [EndPoints] 内の要素、 [ServiceDefinition.csdef] ファイルです。

2 つ目のファイアウォールは、仮想マシンとその仮想マシン内の処理との間の接続を制御します。 これは `netsh advfirewall firewall` コマンド ライン ツールによって制御されます。この記事ではこちらに焦点を当てます。

Azure はお使いのロール内で開始されるプロセス用のファイアウォール規則を作成します。 たとえば、サービスやプログラムを開始すると、Azure はそのサービスがインターネットと通信するのに必要なファイアウォール規則を自動的に作成します。 ただし、お使いのロール外のプロセスによって開始されるサービス (たとえば、COM + サービス、または Windows Scheduler を使用して起動するプログラム) を作成する場合は、そのサービスへのアクセスを許可するファイアウォール規則を手動で作成する必要があります。 これらのファイアウォール規則はスタートアップ タスクを使用して作成できます。

スタートアップ タスクのファイアウォール ルールを作成する必要がありますが、 [executionContext][Task] の **管理者特権で**します。 次のスタートアップ タスクを追加、 [ServiceDefinition.csdef] ファイルです。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">
        ...
        <Startup>
            <Task commandLine="AddFirewallRules.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WebRole>
</ServiceDefinition>
```

ファイアウォール規則を追加するには、お使いのスタートアップ バッチ ファイルに適切な `netsh advfirewall firewall` コマンドを使用する必要があります。 この例では、スタートアップ タスクに TCP ポート 80 に対するセキュリティと暗号化が必要です。

    REM   Add a firewall rule in a startup task.
    
    REM   Add an inbound rule requiring security and encryption for TCP port 80 traffic.
    netsh advfirewall firewall add rule name="Require Encryption for Inbound TCP/80" protocol=TCP dir=in localport=80 security=authdynenc action=allow >> "%TEMP%\StartupLog.txt" 2>&1
    
    REM   If an error occurred, return the errorlevel.
    EXIT /B %errorlevel%


## 特定の IP アドレスをブロックする

指定された IP アドレスのセットに、Azure の web ロールのアクセスを制限するには、IIS を変更することによって **web.config** ファイルとロックを解除するコマンド ファイルの作成、 **ipSecurity** のセクションで、 **ApplicationHost.config** ファイルです。

最初に、ロックを解除するロールの開始時に実行するコマンド ファイルを作成、 **ipSecurity** のセクションで、 **ApplicationHost.config** ファイルです。 呼ばれる、web ロールのルート レベルに新しいフォルダーを作成 **スタートアップ** このフォルダー内には、というバッチ ファイルを作成し、 **startup.cmd**します。 このファイルのプロパティを設定 **常にコピー** を確実に配置されます。

次のスタートアップ タスクを追加、 [ServiceDefinition.csdef] ファイルです。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">
        ...
        <Startup>
            <Task commandLine="startup.cmd" executionContext="elevated" />
        </Startup>
    </WebRole>
</ServiceDefinition>
```

このコマンドを追加、 **startup.cmd** ファイル。

    %windir%\system32\inetsrv\AppCmd.exe unlock config -section:system.webServer/security/ipSecurity

これにより、 **startup.cmd** web ロールを初期化すると、必要なことを確認するたびに実行するバッチ ファイル **ipSecurity** セクションのロックが解除されています。

最後に、変更、 [system.webServer セクション](http://www.iis.net/configreference/system.webserver/security/ipsecurity#005) web ロールの **web.config** ファイルに次の例で示すように、アクセスを許可する IP アドレスの一覧を追加します。

このサンプル config **により** 定義されている 2 つを除くサーバーにアクセスするすべての ip アドレス

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are granted access-->
    <ipSecurity>
        <!--The following IP addresses are denied access-->
        <add allowed="false" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="false" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

このサンプル config **拒否** を除き、2 つ定義されているサーバーへのアクセスのすべての Ip です。

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are denied access-->
    <ipSecurity allowUnlisted="false">
        <!--The following IP addresses are granted access-->
        <add allowed="true" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="true" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

## PowerShell のスタートアップ タスクを作成する

Windows PowerShell スクリプトから直接呼び出すことはできません、 [ServiceDefinition.csdef] ファイルが、これらは、スタートアップ バッチ ファイル内から呼び出すことができます。

既定では、PowerShell では未署名のスクリプトは実行されません。 スクリプトを署名しない場合は、未署名のスクリプトを実行するように Windows PowerShell を構成する必要があります。 符号なしのスクリプトを実行する **ExecutionPolicy** に設定する必要があります **Unrestricted**します。  **ExecutionPolicy** 設定を使用して、に基づいて Windows PowerShell のバージョン。

    REM   Run an unsigned PowerShell script and log the output
    PowerShell -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1
        
    REM   If an error occurred, return the errorlevel.
    EXIT /B %errorlevel%


PowerShell 2.0 または 1.0 を実行するゲスト OS を使用している場合は、バージョン 2 の実行を強制できます。バージョン 2 を使用できない場合はバージョン 1 を使用してください。

    REM   Attempt to set the execution policy by using PowerShell version 2.0 syntax.
    PowerShell -Version 2.0 -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

    REM   If PowerShell version 2.0 isn't available. Set the execution policy by using the PowerShell
    IF %ERRORLEVEL% EQU -393216 (

       PowerShell -Command "Set-ExecutionPolicy Unrestricted" >> "%TEMP%\StartupLog.txt" 2>&1
       PowerShell .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1
    )

    REM   If an error occurred, return the errorlevel.
    EXIT /B %errorlevel%

## スタートアップ タスクからのファイルをローカル ストレージに作成する

スタートアップ タスクによって作成されたファイルをローカル ストレージ リソースに格納し、後でお使いのアプリケーションからアクセスできます。

ローカル ストレージ リソースを作成するには、追加、 [LocalResources] セクションに、 [ServiceDefinition.csdef] ファイルを追加し、 [LocalStorage] 子要素です。 ローカル ストレージ リソースに一意の名前を付与し、お使いのスタートアップ タスク用に適切なサイズを設定します。

スタートアップ タスクにローカル ストレージ リソースを使用するには、ローカル ストレージ リソースの場所を参照する環境変数を作成する必要があります。 これにより、スタートアップ タスクとアプリケーションがローカル ストレージ リソースのファイルを読み書きできます。

関連するセクション、 **ServiceDefinition.csdef** ファイルを次に示します。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">
        ...
        
        <LocalResources>
          <LocalStorage name="StartupLocalStorage" sizeInMB="5"/>
        </LocalResources>
        
        ...
        
        <Runtime>
            <Environment>
                <Variable name="PathToStartupStorage">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
                </Variable>
            </Environment>
        </Runtime>
        
        ...
        
        <Startup>
          <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
        </Startup>
    </WebRole>
</ServiceDefinition>
```

たとえば、この **Startup.cmd** バッチ ファイルは、 **PathToStartupStorage** ファイルを作成する環境変数 **MyTest.txt** ローカル ストレージの場所にします。

    REM   Create a simple text file.

    ECHO This text will go into the MyTest.txt file which will be in the    >  "%PathToStartupStorage%\MyTest.txt"
    ECHO path pointed to by the PathToStartupStorage environment variable.  >> "%PathToStartupStorage%\MyTest.txt"
    ECHO The contents of the PathToStartupStorage environment variable is   >> "%PathToStartupStorage%\MyTest.txt"
    ECHO "%PathToStartupStorage%".                                          >> "%PathToStartupStorage%\MyTest.txt"

    REM   Exit the batch file with ERRORLEVEL 0.

    EXIT /b 0

Azure sdk を使用してローカル ストレージにアクセスすることができます、 [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) メソッドです。 これにより、標準的なファイルの読み書き操作が有効になり、ローカル ストレージ リソースの内容の読み書きを実行できます。 

```csharp
string localStoragePath = Microsoft.WindowsAzure.ServiceRuntime.RoleEnvironment.GetLocalResource("StartupLocalStorage").RootPath;

string fileContent = System.IO.File.ReadAllText(System.IO.Path.Combine(localStoragePath, "MyTest.txt"));
```


## エミュレーターとクラウドでの実行を区別する

クラウドとコンピューティング エミュレーターで動作するスタートアップ タスクを区別することができます。 たとえば、エミュレーターで実行されている場合にのみ、SQL データの新しいコピーを使用するように設定できます。 または、エミュレーターで実行する場合は不要な、クラウドのパフォーマンスを最適化する何らかの操作を実行することもできます。

コンピューティング エミュレーターとクラウドに対して異なるアクションを実行するには、この機能は、環境変数を作成することで実現できます、 [ServiceDefinition.csdef] ファイルし、スタートアップ タスクで環境変数をテストします。

環境変数を作成するには、追加、 [Variable]/[RoleInstanceValue] 要素の XPath 値を作成および `/RoleEnvironment/Deployment/@emulated`です。 値、 **ComputeEmulatorRunning %** 環境変数になります `"true"` コンピューティング エミュレーターで実行する場合と `"false"` 、クラウドで実行するとします。


```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">

        ...
        
        <Runtime>
            <Environment>
                <Variable name="ComputeEmulatorRunning">
                    <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
                </Variable>
            </Environment>
        </Runtime>

    </WebRole>
</ServiceDefinition>
```

実行するタスクを使用できます、 **ComputeEmulatorRunning %** さまざまなアクションを実行する環境変数、ロールがクラウドまたはエミュレーターで実行されているかどうかに基づいています。 環境変数をチェックする .cmd シェル スクリプトはこちらです。

    REM   Check if this task is running on the compute emulator.

    IF "%ComputeEmulatorRunning%" == "true" (
        REM   This task is running on the compute emulator. Perform tasks that must be run only in the compute emulator.
        
    ) ELSE (
        REM   This task is running on the cloud. Perform tasks that must be run only in the cloud.
        
    )


## タスクが既に実行されていることを検出する

ロールが再起動されずに再利用され、スタートアップ タスクが再度実行される場合があります。 タスクが既にホスト VM で実行されたかどうかを示すフラグがあります。 一部のタスクは実行回数が考慮されず、複数回実行されることがあります。 ただし、タスクが複数回実行されることを回避する必要がある場合もあります。

タスクが既に実行されていることを検出するために最も簡単な方法でファイルを作成する、 **%temp%** タスクの開始を参照してフォルダーのタスクが正常に完了します。 次に示すサンプル cmd シェル スクリプトは、ユーザーの代わりにそれを実行します。

    REM   If Task1_Success.txt exists, then Application 1 is already installed.
    IF EXIST "%RoleRoot%\Task1_Success.txt" (
      ECHO Application 1 is already installed. Exiting. >> "%TEMP%\StartupLog.txt" 2>&1
      GOTO Finish
    )

    REM   Run your real exe task
    ECHO Running XYZ >> "%TEMP%\StartupLog.txt" 2>&1
    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

    IF %ERRORLEVEL% EQU 0 (
      REM   The application installed without error. Create a file to indicate that the task
      REM   does not need to be run again.

      ECHO This line will create a file to indicate that Application 1 installed correctly. > "%RoleRoot%\Task1_Success.txt"
      
    ) ELSE (
      REM   An error occurred. Log the error and exit with the error code.

      DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
      TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
      ECHO  An error occurred running task 1. Errorlevel = %ERRORLEVEL%. >> "%TEMP%\StartupLog.txt" 2>&1

      EXIT %ERRORLEVEL%
    )

    :Finish

    REM   Exit normally.
    EXIT /B 0


## タスクのベスト プラクティス
ここでは、お使いの Web ロールや worker ロールのタスクを構成するときに従う必要があるいくつかのベスト プラクティスを紹介します。

### 常にスタートアップ アクティビティをログに記録する

Visual Studio にはバッチ ファイルを順番に実行するデバッガーが用意されていないため、バッチ ファイルの操作に関するデータをできる限り多く入手することをお勧めします。 バッチ ファイルの出力をログ記録両方 **stdout** と **stderr**, 、すれば、重要な情報のデバッグおよびバッチ ファイルを修正しようとするとします。 両方 **stdout** と **stderr** 、StartupLog.txt をディレクトリ内のファイルを指す、 **%temp%** 環境変数のテキストを追加 `>>  "%TEMP%\\StartupLog.txt" 2>&1` ログに記録する特定の行の末尾にします。 たとえばにある setup.exe を実行するため、 **%pathtoapp1install** ディレクトリ。

    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

スタートアップ タスクからのすべてのログを `>> "%TEMP%\StartupLog.txt" 2>&1` を各行の末尾に追加することなく記録するには、2 つのスタートアップ バッチ ファイルが必要です。 最初のバッチ ファイルが 2 つ目のバッチ ファイルを呼び出し、2 つ目のバッチ ファイルのすべてのアクティビティをログに記録するようにリダイレクトします。 これは、適切なリダイレクトを実行するために必要です。

スタートアップ バッチ ファイルからのすべての出力をリダイレクトする方法を次に示します。 この例では、ServerDefinition.csdef ファイルが Startup1.cmd を呼び出すスタートアップ タスクを作成します。 Startup1.cmd は startup2.cmd を呼び出して、すべての出力を %temp%\\startuplog.txt を呼び出します。

ServiceDefinition.cmd:

```xml
<Startup>
    <Task commandLine="Startup1.cmd" executionContext="limited" taskType="simple" />
</Startup>
```

Startup1.cmd:

    REM   Startup1.cmd calls the main startup batch file, Startup2.cmd, redirecting all output
    REM   to the StartupLog.txt log file.

    REM   Log the startup date and time.
    ECHO Startup1.cmd: >> "%TEMP%\StartupLog.txt" 2>&1
    ECHO Current date and time: >> "%TEMP%\StartupLog.txt" 2>&1
    DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
    TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
    ECHO Starting up Startup2.cmd. >> "%TEMP%\StartupLog.txt" 2>&1

    REM   Call the Startup2.cmd batch file, redirecting all output to the StartupLog.txt log file.
    START /B /WAIT Startup2.cmd >> "%TEMP%\StartupLog.txt" 2>&1

    REM   Log the completion of Startup1.cmd.
    ECHO Returned to Startup1.cmd. >> "%TEMP%\StartupLog.txt" 2>&1

    IF ERRORLEVEL EQU 0 (
       REM   No errors occurred. Exit Startup1.cmd normally.
       EXIT /B 0
    ) ELSE (
       REM   Log the error.
       ECHO An error occurred. The ERRORLEVEL = %ERRORLEVEL%.  >> "%TEMP%\StartupLog.txt" 2>&1
       EXIT %ERRORLEVEL%
    )

Startup2.cmd:

    REM   This is the batch file where the startup steps should be performed. Because of the
    REM   way Startup2.cmd was called, all commands and their outputs will be stored in the
    REM   StartupLog.txt file in the directory pointed to by the TEMP environment variable.

    REM   If an error occurs, the following command will pass the ERRORLEVEL back to the
    REM   calling batch file.
    EXIT /B %ERRORLEVEL%

### スタートアップ タスクに適した executionContext を設定する

スタートアップ タスクに適切な特権を設定します。 ロールが通常の特権で実行されるときでも、スタートアップ タスクは管理者特権で実行する必要がある場合があります。

 [ExecutionContext][Task] 属性はスタートアップ タスクの特権レベルを設定します。 `executionContext="limited"` を使用すると、スタートアップ タスクにロールと同じ特権レベルが付与されます。 `executionContext="elevated"` を使用すると、スタートアップ タスクに管理者特権が付与されます。これにより、お使いのロールに管理者特権を与えることなく、スタートアップ タスクで管理者のタスクを実行できます。

高度な特権を必要とするスタートアップ タスクの例を使用するスタートアップ タスクは、 **AppCmd.exe** IIS を構成します。 **AppCmd.exe** 必要 `executionContext="elevated"`します。

### 適切な taskType を使用する

 [TaskType][Task] の実行方法、スタートアップ タスクが属性を決定します。 次の 3 つの値がある: **単純な**, 、**バック グラウンド**, 、および **フォア グラウンド**します。 background タスクと foreground タスクは非同期的に開始され、simple タスクは一度に 1 回のみ同期的に実行されます。

 **単純な** スタートアップ タスクでは、ServiceDefinition.csdef ファイルで、タスクが表示される順序によって、タスクの実行順序を設定することができます。 場合、 **単純な** タスクがゼロ以外の終了コードで終了し、スタートアップ手続きが停止し、ロールは開始されません。

違い **バック グラウンド** スタートアップ タスクと **フォア グラウンド** スタートアップ タスクは **フォア グラウンド** タスクまで実行されるロールが今後も、 **フォア グラウンド** タスクが終了します。 つまり、その場合、 **フォア グラウンド** タスクがハングまたはクラッシュまで、ロールはリサイクルされません、 **フォア グラウンド** タスクが強制終了します。 このため、 **バック グラウンド** タスクは、非同期スタートアップ タスクの機能が必要な場合以外は推奨、 **フォア グラウンド** タスクです。

### EXIT/B 0 でバッチ ファイルを終了する

場合にのみ、ロールが開始、 **errorlevel** それぞれ、単純スタートアップ タスクがゼロです。 すべてのプログラムの設定、 **errorlevel** (終了コード) 正しく、そのため、バッチ ファイルを末尾に、 `EXIT /B 0` すべてが正しく実行された場合。

ロールが開始されない一般的な原因は、スタートアップ バッチ ファイルの末尾に `EXIT /B 0` がないことです。

### スタートアップ タスクを複数回実行する

すべてのロールの再利用で再起動が実行されるとは限りませんが、すべてのロールの再利用ですべてのスタートアップ タスクが実行されます。 これは、再起動の間にスタートアップ タスクを複数回問題なく実行できる必要があることを意味します。 これについては説明 [上](#detect-that-your-task-has-already-run)します。

### ロールでアクセスする必要があるファイルをローカル ストレージに格納する

スタートアップ タスクの実行時にファイルをコピーまたは作成し、後からお使いのロールでアクセスするには、そのファイルをローカル ストレージに配置する必要があります。 参照してください、 [セクション](#create-files-in-local-storage-from-a-startup-task) 上です。

## 次のステップ

クラウドの確認 [サービス モデルとパッケージ](cloud-services-model-and-package.md)

方法の詳細について [タスク](cloud-services-startup-tasks.md) 動作します。

[作成および展開](cloud-services-how-to-create-deploy-portal.md) 、クラウド サービス パッケージです。


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Task]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Environment]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Variable]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
[Endpoints]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Endpoints
[LocalStorage]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalStorage
[LocalResources]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalResources
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue

