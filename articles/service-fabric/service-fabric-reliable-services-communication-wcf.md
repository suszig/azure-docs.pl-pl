<properties
   pageTitle="Reliable Services の WCF 通信スタック | Microsoft Azure"
   description="Service Fabric の組み込み WCF 通信スタックは、Reliable Services にクライアント サービス WCF 通信を提供します。"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="11/17/2015"
   ms.author="bharatn@microsoft.com"/>


# Reliable Services の WCF ベースの通信スタック

Reliable Services フレームワークにより、サービスの作成者はサービスに使用する通信スタックを決定できます。 使用して、任意の通信スタックをプラグインできる、 `ICommunicationListener` から返される、 [CreateServiceReplicaListeners または CreateServiceInstanceListeners](service-fabric-reliable-service-communication.md) メソッドです。 フレームワークでは、WCF ベースの通信を使用したいと考えるサービス作成者に、WCF ベースの通信スタックの実装を提供します。

## WCF 通信リスナー

WCF 固有の実装 `ICommunicationListener` によって提供される、 `Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener` クラスです。

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    // TODO: If your service needs to handle user requests, return a list of ServiceReplicaListeners here.
    return new[] { new ServiceReplicaListener(parameters =>
        new WcfCommunicationListener(typeof(ICalculator), this)
        {
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // which identifies the endpoint that the wcf servicehost should listen on.
            //
            EndpointResourceName = "ServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            Binding = this.CreateListenBinding()
        }
    )};
}
```

## WCF の通信スタック用のクライアントの作成

WCF を使用するサービスと通信するクライアントを作成するには、フレームワークは、 `WcfClientCommunicationFactory`, の WCF 固有の実装である [`ClientCommunicationFactoryBase`] (サービスの fabric の信頼性の高い-サービス-communication.md)。

```csharp

public WcfCommunicationClientFactory(
    ServicePartitionResolver servicePartitionResolver = null,
    Binding binding = null,
    object callback = null,
    IList<IExceptionHandler> exceptionHandlers = null,
    IEnumerable<Type> doNotRetryExceptionTypes = null)
```

WCF 通信チャネルはからアクセスできる、 `WcfCommunicationClient` によって作成された、 `WcfCommunicationClientFactory`

```csharp

public class WcfCommunicationClient<TChannel> : ICommunicationClient where TChannel : class
{
    public TChannel Channel { get; }
    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

クライアント コードで使用できる、 `WcfCommunicationClientFactory` と共に、 `ServicePartitionClient` をサービス エンドポイントを特定し、サービスと通信を行います。

```csharp

//
// Create a service resolver for resolving the endpoints of the calculator service.
//
ServicePartitionResolver serviceResolver = new ServicePartitionResolver(() => new FabricClient());

//
// Create the binding.
//
NetTcpBinding binding = CreateClientConnectionBinding();

var clientFactory = new WcfCommunicationClientFactory<ICalculator>(
    serviceResolver,// ServicePartitionResolver
    binding,        // Client binding
    null,           // Callback object
    null);          // donot retry Exception types


//
// Create a client for communicating with the calc service which has been created with
// Singleton partition scheme.
//
var calculatorServicePartitionClient = new ServicePartitionClient<WcfCommunicationClient<ICalculator>>(
    clientFactory,
    ServiceName);

//
// Call the service to perform the operation.
//
var result = calculatorServicePartitionClient.InvokeWithRetryAsync(
    client => client.Channel.AddAsync(2, 3)).Result;
```

## 次のステップ

* [信頼性の高いサービスのリモート処理でのリモート プロシージャ コール](service-fabric-reliable-services-communication-remoting.md)

* [Reliable Services で OWIN による web API](service-fabric-reliable-services-communication-webapi.md)





