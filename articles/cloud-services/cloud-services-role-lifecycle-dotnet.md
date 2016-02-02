<properties 
pageTitle="クラウド サービスのライフ サイクル イベントの処理 |Microsoft Azure" 
description=".NET でクラウド サービスのロールのライフサイクル メソッドを使用する方法について説明します。" 
services="cloud-services" 
documentationCenter=".net" 
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


# .NET で Web または Worker ロールのライフサイクルをカスタマイズする

拡張するワーカー ロールを作成するとき、 [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) を上書きすることのできるメソッドを提供するクラスは、ライフ サイクル イベントに応答します。 Web ロールの場合、このクラスは任意であり、必要に応じてライフサイクル イベントへの応答に使用します。

## RoleEntryPoint クラスを拡張する

[RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) クラスには、Azure が呼び出すメソッドが含まれて ときに、 **開始**, 、**実行**, 、または **停止** web またはワーカー ロールです。 必要に応じてこれらのメソッドをオーバーライドし、ロールの初期化、ロールのシャットダウン シーケンス、ロールの実行スレッドを管理できます。

**RoleEntryPoint** を拡張した場合、メソッドの次のような動作に注意する必要があります。

-   [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) と [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) メソッドがブール値を返すため、返されることは **false** これらのメソッドからです。

     コードが **false** を返した場合、ロール プロセスは指定したシャットダウン シーケンスを実行せずに突然終了します。 一般的には、**OnStart** メソッドから **false** は返さないようにしてください。

-   **RoleEntryPoint** メソッドのオーバーロード内でキャッチされなかった例外は、未処理の例外として扱われます。

     Azure が発生して例外がライフ サイクル メソッドの内部で発生する場合、 [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) イベント、および、プロセスが終了します。 ロールは、オフラインになった後、Azure によって再開されます。 ハンドルされない例外が発生したときに、 [停止](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) イベントは発生しません、 **OnStop** メソッドは呼び出されません。

ロールが開始しない場合や、ロールが初期化、ビジー状態、停止中の状態で再利用されている場合、コードが再開されるたびに、ライフサイクル イベントの内部で未処理の例外がスローされる場合があります。 ここでは、使用して、 [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) 例外の原因を特定し、それを適切に処理するイベントです。 ロールを戻ることも、 [実行](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) メソッドにロールを再起動します。 展開の状態の詳細については、次を参照してください。 [一般的な問題をロールがリサイクル](https://msdn.microsoft.com/library/azure/gg465402.aspx)します。
> [AZURE.NOTE] 使用している場合、 [Azure Tools for Microsoft Visual Studio](https://msdn.microsoft.com/library/azure/ee405484.aspx) 、アプリケーションを開発するには、ロール プロジェクトのテンプレートを自動的に拡張、 **RoleEntryPoint** 、WebRole.cs ファイルと WorkerRole.cs ファイルで、クラスです。

## OnStart メソッド

**OnStart** メソッドは、ロール インスタンスが Azure によってオンラインに切り替えられたときに呼び出されます。 OnStart コードの実行中、ロール インスタンスは "**ビジー**" とマークされ、外部トラフィックはロード バランサーからこのロールに送信されません。 イベント ハンドラーを実装して、開始などの初期化作業を実行するには、このメソッドをオーバーライドする [Azure 診断](cloud-services-how-to-monitor.md)します。

**OnStart** が **true** を返す場合、インスタンスは正常に初期化されており、Azure は **RoleEntryPoint.Run** メソッドを呼び出します。 **OnStart** が **false** を返す場合、ロールは予定されたシャットダウン シーケンスを実行せず、すぐに終了します。

**OnStart** メソッドをオーバーライドするコード例を次に示します。 このメソッドは、ロール インスタンスが開始されたときに診断モニターを構成して開始し、ストレージ アカウントへのログ データの転送をセットアップします。

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## OnStop メソッド

**OnStop** メソッドは、Azure によってロール インスタンスがオフラインになった後、プロセスが終了する前に呼び出されます。 このメソッドをオーバーライドして、ロール インスタンスが完全にシャットダウンするのに必要なコードを呼び出すことができます。
> [AZURE.IMPORTANT] **OnStop** メソッドで実行されるコードは、ユーザーが開始したシャットダウン以外の理由で呼び出された場合、限られた時間内で処理を完了する必要があります。 この時間が経過するとプロセスは終了するため、**OnStop** メソッドのコードが短時間で実行できるか、最後まで実行されなくても許容できることを確認する必要があります。 **OnStop** メソッドは、**Stopping** イベントが発生した後に呼び出されます。


## Run メソッド

**Run** メソッドをオーバーライドして、ロール インスタンスの実行時間の長いスレッドを実装できます。

**Run** メソッドのオーバーライドは必須ではありません。既定の実装では、スリープ状態を永遠に続けるスレッドが開始されます。 **Run** メソッドをオーバーライドした場合、コードは無期限にブロックされます。 **Run** メソッドから制御が戻った場合、ロールは自動的に適切に再利用されます。言い換えれば、Azure で **Stopping** イベントが発生し、**OnStop** メソッドが呼び出されて、ロールがオフラインになる前にシャットダウン シーケンスを実行できます。


### Web ロール用の ASP.NET ライフサイクル メソッドの実装

Web ロールの初期化とシャットダウン シーケンスを管理するには、**RoleEntryPoint** クラスで提供されるメソッドのほかに、ASP.NET のライフサイクル メソッドも使用できます。 これは既存の ASP.NET アプリケーションを Azure に移植する場合に、互換性の面で役立つことがあります。 ASP.NET のライフサイクル メソッドは、**RoleEntryPoint** メソッドの内部から呼び出されます。  **Application\_Start** メソッドは、 **RoleEntryPoint.OnStart** メソッドが終了します。  **Application\_End** メソッドは前に、 **RoleEntryPoint.OnStop** メソッドが呼び出されます。

## 次のステップ

学習方法 [クラウド サービス パッケージを作成](cloud-services-model-and-package.md)します。




