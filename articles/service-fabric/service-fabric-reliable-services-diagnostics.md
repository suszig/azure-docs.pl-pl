<properties
   pageTitle="ステートフル Reliable Services の診断"
   description="ステートフル Reliable Services の診断機能"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/03/2015"
   ms.author="alanwar"/>


# ステートフル Reliable Services の診断機能

ステートフル Reliable Services の StatefulServiceBase クラスが所定の [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) イベント 
ランタイムがどのように動作洞察を提供、およびトラブルシューティングに役立つをサービスのデバッグに使用することができます。

## EventSource イベント

ステートフル Reliable Services の StatefulServiceBase クラスの EventSource 名は、"Microsoft-ServiceFabric-Services" です。 このイベント ソースのイベントに表示します 
[診断イベント](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) ウィンドウとサービス 
される [Visual Studio でデバッグ](service-fabric-debugging-your-application.md)します。

収集または EventSource イベントの表示に役立つツールとテクノロジの例としては [PerfView](http://www.microsoft.com/download/details.aspx?id=28567),、 
[Microsoft Azure 診断](../cloud-services-dotnet-diagnostics.md) および 
[Microsoft TraceEvent ライブラリ](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent)します。

## イベント

| イベント名| イベント ID| レベル| イベントの説明|
|----------|--------|-----|-----------------|
| StatefulRunAsyncInvocation| 1| 情報| サービスの RunAsync タスクが開始されたるときに出力されます|
| StatefulRunAsyncCancellation| 2| 情報| サービスの RunAsync タスクが取り消されたときに出力されます|
| StatefulRunAsyncCompletion| 3| 情報| サービスの RunAsync タスクが完了したときに出力されます|
| StatefulRunAsyncSlowCancellation| 4| 警告| サービスの RunAsync タスクが取り消しの実行に時間がかかりすぎたときに出力されます|
| StatefulRunAsyncFailure| 5| エラー| サービスの RunAsync タスクが例外をスローしたときに出力されます|

## イベントの解釈

StatefulRunAsyncInvocation、StatefulRunAsyncCompletion、および StatefulRunAsyncCancellation イベントは、サービスを記述する場合のタイミングと同様に、サービスのライフ サイクルを理解すると便利ですが、 
サービスが開始、取り消され、完了します。 これはサービスの問題をデバッグする場合やサービスのライフ サイクルを理解する場合に役立つことがあります。

サービスの作成者は、細心の注意を払う必要があります。 
StatefulRunAsyncSlowCancellation および StatefulRunAsyncFailure イベントに、サービスの問題を示すとします。

StatefulRunAsyncFailure が出力されるときに
サービスの RunAsync() タスクでは、例外をスローします。 一般に、スローされる例外は、サービスのエラーやバグを示します。 さらに、例外のサービスが失敗すると、されために移動します。 
高価な操作が実行されると、サービスが移動中に、受信要求を遅延させる別のノードです。 サービスの作成者は例外の原因を特定し、可能であればそれを軽減する必要があります。

StatefulRunAsyncSlowCancellation は、RunAsync タスクの取り消し要求に 4 秒より長くかかるたびに出力されます。 影響を与えると、サービスは、取り消しの完了に時間がかかる、
別のノードですぐに再起動するサービスの機能し、サービスの全体的な可用性に影響を与える可能性があります。





