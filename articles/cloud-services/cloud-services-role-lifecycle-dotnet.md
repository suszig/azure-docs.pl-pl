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

拡張する場合に **RoleEntryPoint**, 、メソッドの次の動作を認識する必要があります。

-    [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) と [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) メソッドがブール値を返すため、返されることは **false** これらのメソッドからです。

     コードを返した場合 **false**, 、役割の処理が突然、終了、シャット ダウン シーケンスを実行することがなくインプレース必要があります。 一般に、返さないようにしてください **false** から、 **OnStart** メソッドです。
     
-   いずれかのオーバー ロード内で例外がキャッチされていない、 **RoleEntryPoint** メソッドは、未処理の例外として扱われます。

     Azure が発生して例外がライフ サイクル メソッドの内部で発生する場合、 [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) イベント、および、プロセスが終了します。 ロールは、オフラインになった後、Azure によって再開されます。 ハンドルされない例外が発生したときに、 [停止](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) イベントは発生しません、 **OnStop** メソッドは呼び出されません。

ロールが開始しない場合や、ロールが初期化、ビジー状態、停止中の状態で再利用されている場合、コードが再開されるたびに、ライフサイクル イベントの内部で未処理の例外がスローされる場合があります。 ここでは、使用して、 [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) 例外の原因を特定し、それを適切に処理するイベントです。 ロールを戻ることも、 [実行](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) メソッドにロールを再起動します。 展開の状態の詳細については、次を参照してください。 [一般的な問題をロールがリサイクル](https://msdn.microsoft.com/library/azure/gg465402.aspx)します。

> [AZURE.NOTE] 使用している場合、 [Azure Tools for Microsoft Visual Studio](https://msdn.microsoft.com/library/azure/ee405484.aspx) 、アプリケーションを開発するには、ロール プロジェクトのテンプレートを自動的に拡張、 **RoleEntryPoint** 、WebRole.cs ファイルと WorkerRole.cs ファイルで、クラスです。

## OnStart メソッド

 **OnStart** Azure によってロール インスタンスがオンラインになると、メソッドが呼び出されます。 ロール インスタンスとしてマークされている OnStart コードの実行中に **ビジー** 外部トラフィックは送信されませんにロード バランサーによってとします。 イベント ハンドラーを実装して、開始などの初期化作業を実行するには、このメソッドをオーバーライドする [Azure 診断](cloud-services-how-to-monitor.md)します。

場合 **OnStart** 返します **true**, 、インスタンスが正常に初期化されており、Azure は、 **RoleEntryPoint.Run** メソッドです。 場合 **OnStart** 返します **false**, 、ロールは、計画されたシャット ダウン シーケンスを実行することがなく、すぐに終了します。

次のコード例は、オーバーライドする方法を示します、 **OnStart** メソッドです。 このメソッドは、ロール インスタンスが開始されたときに診断モニターを構成して開始し、ストレージ アカウントへのログ データの転送をセットアップします。

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

 **OnStop** ロール インスタンスがオフラインになった後、プロセスが終了する前に、Azure によってメソッドが呼び出されます。 このメソッドをオーバーライドして、ロール インスタンスが完全にシャットダウンするのに必要なコードを呼び出すことができます。

> [AZURE.IMPORTANT] 実行されるコード、 **OnStop** メソッドにはユーザーによって開始されたシャット ダウン以外の理由で呼び出された場合に終了する期間を限定します。 この時間が経過すると、プロセスが終了、ようにする必要があります内のコードは、 **OnStop** メソッドが迅速に実行できるまたはまで実行を許容します。  **OnStop** メソッドは、 **停止** イベントが発生します。


## Run メソッド

オーバーライドすることができます、 **実行** 、ロール インスタンスの実行時間の長いスレッドを実装するメソッドです。

オーバーライドする、 **実行** メソッドは必要ありません。 既定の実装が永遠にスリープ状態のスレッドを開始します。 オーバーライドした場合、 **実行** メソッド、コードは、無期限にブロックされます。 場合、 **実行** 、ロールが自動的に適切にリサイクルされる; つまり、Azure を発生させるメソッドが返される、 **を停止する** イベントと呼び出し、 **OnStop** メソッドをロールがオフライン前に、シャット ダウン シーケンスを実行することがあります。


### Web ロール用の ASP.NET ライフサイクル メソッドの実装

提供されるだけでなく、ASP.NET のライフ サイクル メソッドを使用する、 **RoleEntryPoint** クラス、web ロールの初期化とシャット ダウン シーケンスを管理します。 これは既存の ASP.NET アプリケーションを Azure に移植する場合に、互換性の面で役立つことがあります。 ASP.NET のライフ サイクル メソッドは、内から呼び出される、 **RoleEntryPoint** メソッドです。  **Application\_Start** メソッドは、 **RoleEntryPoint.OnStart** メソッドが終了します。  **Application\_End** メソッドは前に、 **RoleEntryPoint.OnStop** メソッドが呼び出されます。

## 次のステップ
学習方法 [クラウド サービス パッケージを作成](cloud-services-model-and-package.md)します。
