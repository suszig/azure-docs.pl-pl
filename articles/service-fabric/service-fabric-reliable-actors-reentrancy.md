<properties
   pageTitle="高信頼アクターの再入"
   description="Service Fabric 高信頼アクターの再入の概要"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/14/2015"
   ms.author="amanbha"/>



# 高信頼アクターの再入

Fabric アクターは、既定で、論理呼び出しコンテキスト ベースの再入を使用できます。 これにより、アクターは、同じ呼び出しコンテキストのチェーンにある場合は再入可能になります。 たとえば、アクター A がアクター C にメッセージを送信するアクター B にメッセージを送信する場合です。アクター C がアクター A を呼び出す場合は、メッセージ処理の一環として、メッセージが許可されるように再入可能です。 処理が完了するまで、アクター A では異なる呼び出しコンテキストの一部である他のメッセージがブロックされます。

コンテキスト ベースの再入を使用すると、アクターを修飾することによって無効できます論理呼び出しを許可しないようにするアクター クラス `ReentrantAttribute(ReentrancyMode.Disallowed)`します。

```csharp
public enum ReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```

次のコードは再入モードを設定するアクター クラスを `ReentrancyMode.Disallowed`します。 この場合、アクターを送信する場合、再入メッセージを別のアクター型の例外 `FabricException` がスローされます。

```csharp
[Reentrant(ReentrancyMode.Disallowed)]
class VoicemailBoxActor : StatefulActor<VoicemailBox>, IVoicemailBoxActor
{
    ...
}
```





