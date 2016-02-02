<properties
   pageTitle="Service Fabric の高信頼アクター | Microsoft Azure"
   description="Service Fabric プラットフォームの機能を高信頼アクターが使用するしくみについて説明します。アクター開発者の視点から見た概念も取り上げます。"
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
   ms.date="11/13/2015"
   ms.author="abhisram"/>


# 高信頼アクターの Service Fabric プラットフォームの使用方法

アクターは、Azure Service Fabric アプリケーション モデルを使用して、アプリケーションのライフサイクルを管理します。 Service fabric すべてアクター型がマップされている [サービスの種類を](service-fabric-application-model.md#describe-a-service)します。 アクター コードは [パッケージ](service-fabric-application-model.md#package-an-application) Service Fabric アプリケーションとして、 [展開](service-fabric-deploy-remove-applications.md#deploy-an-application) クラスターにします。

## アクターのアプリケーション モデルの概念の例

アクター プロジェクトの例を見てみましょう [Visual Studio を使って作成された](service-fabric-reliable-actors-get-started.md), 、上記の概念の一部を紹介します。

ソリューションが Visual Studio で作成されると、アプリケーション マニフェスト、サービス マニフェスト、Settings.xml 構成ファイルが、アクターのサービスのプロジェクトに含まれます。 これを示したものが下記のスクリーンショットです。

![Visual Studio で作成されたプロジェクト][1]

アクター サービスのプロジェクトに含まれているアプリケーション マニフェストを確認して、アプリケーションの種類およびアクターがパッケージ化されているアプリケーションのバージョンを参照できます。 この例として、アプリケーション マニフェストの次のスニペットが挙げられます。

~~~
< ApplicationManifest xmlns:xsd ="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi ="http://www.w3.org/2001/XMLSchema-instance"
                     ApplicationTypeName ="VoiceMailBoxApplication"
                     ApplicationTypeVersion =「1.0.0.0」
                     xmlns ="http://schemas.microsoft.com/2011/01/fabric">
~~~

アクターのサービスのプロジェクトに含まれているサービス マニフェストを確認して、アクターの型をマップするサービスの種類を参照できます。 この例として、サービス マニフェストの次のスニペットが挙げられます。

~~~
<StatefulServiceType ServiceTypeName="VoiceMailBoxActorServiceType" HasPersistedState="true">
~~~

Visual Studio でアプリケーション パッケージが作成されると、[ビルド出力] ウィンドウのログが、アプリケーション パッケージの場所を示します。 次に例を示します。

    -------- Package started: Project: VoiceMailBoxApplication, Configuration: Debug x64 ------
    VoiceMailBoxApplication -> C:\samples\Samples\Actors\VS2015\VoiceMailBox\VoiceMailBoxApplication\pkg\Debug

上記の場所の一覧の一部を次に示します (簡潔にするため、すべての一覧は省略します)。

    C:\samples\Samples\Actors\VS2015\VoiceMailBox\VoiceMailBoxApplication\pkg\Debug>tree /f
    Folder PATH listing
    Volume serial number is 303F-6F91
    C:.
    │   ApplicationManifest.xml
    │
    ├───VoiceMailBoxActorServicePkg
    │   │   ServiceManifest.xml
    │   │
    │   ├───Code
    │   │   │   Microsoft.ServiceFabric.Actors.dll
    │   │   │       :
    │   │   │   Microsoft.ServiceFabric.Services.dll
    │   │   │   ServiceFabricServiceModel.dll
    │   │   │   System.Fabric.Common.Internal.dll
    │   │   │   System.Fabric.Common.Internal.Strings.dll
    │   │   │   VoiceMailBox.exe
    │   │   │   VoiceMailBox.exe.config
    │   │   │   VoiceMailBox.Interfaces.dll
    │   │   │
    │   │   └───en-us
    │   │           System.Fabric.Common.Internal.Strings.resources.dll
    │   │
    │   └───Config
    │           Settings.xml
    │
    └───VoicemailBoxWebServicePkg
        │   ServiceManifest.xml
        │
        └───Code
            │   Microsoft.Owin.dll
            │       :
            │   Microsoft.ServiceFabric.Services.dll
            │       :
            │   System.Fabric.Common.Internal.dll
            │   System.Fabric.Common.Internal.Strings.dll
            │       :
            │   VoiceMailBox.Interfaces.dll
            │   VoicemailBoxWebService.exe
            │   VoicemailBoxWebService.exe.config
            │
            └───en-us
                    System.Fabric.Common.Internal.Strings.resources.dll

上記の一覧は、アプリケーション パッケージのサービス パッケージ内のコード パッケージに含まれている、VoicemailBox アクターを実装するアセンブリを示します。

アプリケーションの後続の管理 (アップグレードや最終的な削除など) は、Service Fabric のアプリケーション管理メカニズムからも実行できます。 詳細については、トピックを参照して、上、 [アプリケーション モデル](service-fabric-application-model.md), 、[アプリケーションの展開と削除](service-fabric-deploy-remove-applications.md), 、および [アプリケーションのアップグレード](service-fabric-application-upgrade.md)します。

## アクターのサービスの拡張性

クラスターの管理者は、クラスター内の各サービスの種類の 1 つ以上のアクターにサービスを作成できます。 各アクターのサービスはそれぞれ、1 つまたは複数のパーティション (その他の Service Fabric サービスに似ています) を持つことができます。 1 つのサービスの種類に複数のサービスを作成する (アクターの型にマップする) 機能や、サービスに複数のパーティションを作成する機能により、アクターのアプリケーションを拡張できるようになります。 記事を参照してください [スケーラビリティ](service-fabric-concepts-scalability.md) の詳細。
> [AZURE.NOTE] ステートレス アクター サービスが必要な [インスタンス](service-fabric-availability-services.md#availability-of-service-fabric-stateless-services) 数が 1 です。 1 つのパーティション内に、複数インスタンスのステートレス アクター サービスを持つことはできません。 そのため、ステートレス アクター サービスのスケーラビリティのためにインスタンス数を増やすことはできません。 説明されているスケーラビリティ オプションを使用する必要があります、 [スケーラビリティに関する記事](service-fabric-concepts-scalability.md)します。

## アクターの Service Fabric のパーティションの概念

アクターの ID は、アクターのサービスのパーティションにマップされます。 そのアクター ID がマップされるパーティション内で、アクターが作成されます。 アクターが作成されると、アクター ランタイムによって書き込まれます、 [EventSource イベント](service-fabric-reliable-actors-diagnostics.md#eventsource-events) 、アクターが作成されるパーティションを示すことです。 下記はこのことを示すイベントの例をアクター ID が、 `-5349766044453424161` パーティション内で作成された `b6afef61-be9a-4492-8358-8f473e5d2487` サービスの `ファブリック:/VoicemailBoxAdvancedApplication/VoicemailBoxActorService`, ]、[application `ファブリック:/VoicemailBoxAdvancedApplication`します。

    {
      "Timestamp": "2015-04-26T10:12:20.2485941-07:00",
      "ProviderName": "Microsoft-ServiceFabric-Actors",
      "Id": 5,
      "Message": "Actor activated. Actor type: Microsoft.Azure.Service.Fabric.Samples.VoicemailBox.VoiceMailBoxActor, actor ID: -5349766044453424161, stateful: True, replica/instance ID: 130,745,418,574,851,853, partition ID: 0583c745-1bed-43b2-9545-29d7e3448156.",
      "EventName": "ActorActivated",
      "Payload": {
        "actorType": "Microsoft.Azure.Service.Fabric.Samples.VoicemailBox.VoiceMailBoxActor",
        "actorId": "-5349766044453424161",
        "isStateful": "True",
        "replicaOrInstanceId": "130906628008120392",
        "partitionId": "b6afef61-be9a-4492-8358-8f473e5d2487",
        "serviceName": "fabric:/VoicemailBoxAdvancedApplication/VoicemailBoxActorService",
        "applicationName": "fabric:/VoicemailBoxAdvancedApplication",
      }
    }

別のアクター ID が `-4952641569324299627` 別のパーティション内で作成された (`5405d449-2da6-4d9a-ad75-0ec7d65d1a2a`) 下記のイベントで示されているように、同じサービスのです。

    {
      "Timestamp": "2015-04-26T15:06:56.93882-07:00",
      "ProviderName": "Microsoft-ServiceFabric-Actors",
      "Id": 5,
      "Message": "Actor activated. Actor type: Microsoft.Azure.Service.Fabric.Samples.VoicemailBox.VoiceMailBoxActor, actor ID: -4952641569324299627, stateful: True, replica/instance ID: 130,745,418,574,851,853, partition ID: c146fe53-16d7-4d96-bac6-ef54613808ff.",
      "EventName": "ActorActivated",
      "Payload": {
        "actorType": "Microsoft.Azure.Service.Fabric.Samples.VoicemailBox.VoiceMailBoxActor",
        "actorId": "-4952641569324299627",
        "isStateful": "True",
        "replicaOrInstanceId": "130745418574851853",
        "partitionId": "5405d449-2da6-4d9a-ad75-0ec7d65d1a2a",
        "serviceName": "fabric:/VoicemailBoxAdvancedApplication/VoicemailBoxActorService",
        "applicationName": "fabric:/VoicemailBoxAdvancedApplication",
      }
    }

> [AZURE.NOTE] 簡潔にするため、上記のイベントの一部のフィールドを省略しています。

パーティション ID はパーティションに関するその他の情報を取得するために使用できます。 たとえば、 [Service Fabric エクスプ ローラー](service-fabric-visualizing-your-cluster.md) およびサービスとアプリケーションが所属するパーティションに関する情報を表示するツールを使用できます。 次のスクリーン ショットはパーティションに関する情報が表示 `5405d449-2da6-4d9a-ad75-0ec7d65d1a2a`, 、アクター ID が含まれている `-4952641569324299627` 上記の例です。

![Service Fabric エクスプ ローラー内のパーティションに関する情報][3]

アクターには、パーティション ID、サービス名、アプリケーション名、およびその他の Service Fabric プラットフォーム固有の情報を使用してプログラムを取得できます、 `Host.ActivationContext` と `Host.StatelessServiceInitialization` または `Host.StatefulServiceInitializationParameters` アクター型から派生している基本クラスのメンバーです。 次のコード スニペットで例を示します。

```csharp
public void ActorMessage(StatefulActorBase actor, string message, params object[] args)
{
    if (this.IsEnabled())
    {
        string finalMessage = string.Format(message, args);
        ActorMessage(
            actor.GetType().ToString(),
            actor.Id.ToString(),
            actor.ActorService.ServiceInitializationParameters.CodePackageActivationContext.ApplicationTypeName,
            actor.ActorService.ServiceInitializationParameters.CodePackageActivationContext.ApplicationName,
            actor.ActorService.ServiceInitializationParameters.ServiceTypeName,
            actor.ActorService.ServiceInitializationParameters.ServiceName.ToString(),
            actor.ActorService.ServiceInitializationParameters.PartitionId,
            actor.ActorService.ServiceInitializationParameters.ReplicaId,
            FabricRuntime.GetNodeContext().NodeName,
            finalMessage);
    }
}
```

### ステートレスなアクターの Service Fabric のパーティションの概念

ステートレスなアクターは、Service Fabric のステートレス サービスのパーティション内に作成されます。 アクター ID によって、アクターが作成されるパーティションが決定されます。
[インスタンス](service-fabric-availability-services.md#availability-of-service-fabric-stateless-services) 数は、ステートレス アクター サービスは 1 である必要があります。 インスタンス数は 1 以外の値に変更できません。 そのため、アクターはパーティション内の 1 つのサービス インスタンス内に作成されます。
> [AZURE.TIP] Fabric アクター ランタイムは、いくつか出力 [ステートレス アクター インスタンスに関連するイベント](service-fabric-reliable-actors-diagnostics.md#events-related-to-stateless-actor-instances)します。 これらは、診断やパフォーマンスの監視に役立ちます。

ステートレスなアクターが作成されると、アクター ランタイムは、書き込み、 [EventSource イベント](service-fabric-reliable-actors-diagnostics.md#eventsource-events) 、アクターが作成されるパーティションとインスタンスを示します。 この例を次に示します。 示してアクター ID が、 `abc` インスタンス内で作成された `130745709600495974` パーティションの `8c828833-ccf1-4e21-b99d-03b14d4face3`, 、サービスの `ファブリック:/HelloWorldApplication/HelloWorldActorService`, ]、[application `ファブリック:/HelloWorldApplication`します。

    {
      "Timestamp": "2015-04-26T18:17:46.1453113-07:00",
      "ProviderName": "Microsoft-ServiceFabric-Actors",
      "Id": 5,
      "Message": "Actor activated. Actor type: HelloWorld.HelloWorld, actor ID: abc, stateful: False, replica/instance ID: 130,745,709,600,495,974, partition ID: 8c828833-ccf1-4e21-b99d-03b14d4face3.",
      "EventName": "ActorActivated",
      "Payload": {
        "actorType": "HelloWorld.HelloWorld",
        "actorId": "abc",
        "isStateful": "False",
        "replicaOrInstanceId": "130745709600495974",
        "partitionId": "8c828833-ccf1-4e21-b99d-03b14d4face3",
        "serviceName": "fabric:/HelloWorldApplication/HelloWorldActorService",
        "applicationName": "fabric:/HelloWorldApplication",
      }
    }

> [AZURE.NOTE] 簡潔にするため、上記のイベントの一部のフィールドを省略しています。

### ステートフル アクターの Service Fabric パーティションの概念

ステートフル アクターは、Service Fabric ステートフル サービスのパーティション内に作成されます。 アクター ID によって、アクターが作成されるパーティションが決定されます。 サービスの各パーティションは、1 つまたは複数を設定できます [レプリカ](service-fabric-availability-services.md#availability-of-service-fabric-stateful-services) は、クラスター内の別のノードに配置されます。 複数のレプリカを持つことで、アクターの状態に信頼性が得られます。 Azure リソース マネージャーは、クラスター内にある障害とアップグレード ドメインに基づいて配置を最適化します。 同じノードに、同じパーティションの 2 つのレプリカが配置されることはありません。 アクターは、そのアクター ID がマップされるパーティションのプライマリ レプリカに常に作成されます。
> [AZURE.TIP] Fabric アクター ランタイムは、いくつか出力 [ステートフル アクター レプリカに関連するイベント](service-fabric-reliable-actors-diagnostics.md#events-related-to-stateful-actor-replicas)します。 これらは、診断やパフォーマンスの監視に役立ちます。

ことを思い出してください、 [VoiceMailBoxActor の例を前に示した](#service-fabric-partition-concepts-for-actors), 、アクター ID が `-4952641569324299627` パーティション内で作成された `5405d449-2da6-4d9a-ad75-0ec7d65d1a2a`します。 その例の EventSource イベントがレプリカに、アクターが作成されたことを指定しても `130745418574851853` そのパーティションのです。 アクターが作成された時点で、これはそのパーティションのプライマリ レプリカでした。 次の Service Fabric エクスプ ローラーのスクリーン ショットはこれを表しています。

![Service Fabric エクスプ ローラーのプライマリ レプリカ][4]

## アクターの状態プロバイダーの選択

既定のアクター状態プロバイダーの一部が、アクター ランタイムに含まれています。 アクターのサービスに適切な状態プロバイダーを選択するには、状態プロバイダーが基になる Service Fabric プラットフォーム機能をどのように使用してアクターの状態の可用性を高めるかを理解する必要があります。

既定では、ステートフル アクターは、キー値ストアからアクター状態プロバイダーを使用します。 この状態プロバイダーは、Service Fabric プラットフォームで提供される分散キー値ストアに構築されます。 状態が永続的にプライマリをホストしているノードのローカル ディスクに保存 [レプリカ](service-fabric-availability-services.md#availability-of-service-fabric-stateful-services)します。 また、この状態は、セカンダリ レプリカをホストしているノードのローカル ディスクにも複製され、永続的に保存されます。 レプリカのクォーラムがローカル ディスクに状態をコミットした場合にのみ、状態の保存が完了します。 キー値ストアに、障害の状況などの不整合を検出し、これを自動的に修正する高度な機能が備わりました。

アクターのランタイムも含まれています。 `VolatileActorStateProvider`します。 この状態プロバイダーは、状態をレプリカ (状態のコピー) にレプリケートしますが、状態は各レプリカのメモリ内に残ります。 1 つのレプリカがダウンして復帰した場合、その状態は他のレプリカから再構築されます。 ただし、すべてのレプリカが同時にダウンした場合、状態のデータは失われます。 そのため、この状態プロバイダーは、複数のレプリカで障害があってもデータを保護し、アップグレードなどの計画されたフェールオーバーを切り抜けることができるアプリケーションに適しています。 すべてのレプリカが失われた場合は、Service Fabric の外部のメカニズムを使用してデータを再作成する必要があります。 追加することで、揮発性のアクター状態プロバイダーを使用する、ステートフル アクターを構成する、 `VolatileActorStateProvider` 属性をアクターのクラスまたはアセンブリ レベル属性。

次のコード スニペットを使用する明示的な状態プロバイダーの属性を持たないアセンブリのすべてのアクターを変更する方法を示しています。 `VolatileActorStateProvider`します。

```csharp
[assembly:Microsoft.ServiceFabric.Actors.VolatileActorStateProvider]
```

次のコード スニペットは、特定のアクター型の場合、状態プロバイダーを変更する方法を示しています。 `VoicemailBox` をここでは、 `VolatileActorStateProvider`します。

```csharp
[VolatileActorStateProvider]
public class VoicemailBoxActor : StatefulActor<VoicemailBox>, IVoicemailBoxActor
{
    public Task<List<Voicemail>> GetMessagesAsync()
    {
        return Task.FromResult(State.MessageList);
    }
    ...
}
```

状態プロバイダーを変更するには、アクターのサービスを再作成する必要があることに注意してください。 状態プロバイダーはアプリケーションのアップグレードの一部として変更できません。



[1]: ./media/service-fabric-reliable-actors-platform/manifests-in-vs-solution.png 
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png 
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png 
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png 

