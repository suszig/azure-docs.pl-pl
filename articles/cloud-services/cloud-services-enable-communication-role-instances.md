<properties 
pageTitle="Cloud Services でのロールの通信 | Microsoft Azure" 
description="Cloud Services のロール インスタンスには、外部または他のロール インスタンスとの通信用のエンドポイント (http、https、tcp、udp) を定義できます。" 
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

# Azure におけるロール インスタンスの通信の有効化

クラウド サービスのロールは、内部接続および外部接続で通信します。 外部接続と呼ばれる **入力エンドポイント** 内部接続と呼ばれます **内部エンドポイント**します。 このトピックの説明を変更する方法、 [サービス定義](cloud-services-model-and-package.md#csdef) エンドポイントを作成します。


## 入力エンドポイント
入力エンドポイントは、ポートを外部に公開するときに使用します。 プロトコルの種類とエンドポイントのポートを指定してから、そのエンドポイントの内部ポートと外部ポートの両方に適用します。 エンドポイントの別の内部ポートを指定する場合は、 [localPort](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) 属性です。

入力エンドポイントは、次のプロトコルを使用できます: **http、https、tcp、udp**します。

入力エンドポイントを作成するには、追加、 **InputEndpoint** の子要素、 **エンドポイント** web またはワーカー ロールの要素。

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## インスタンス入力エンドポイント
インスタンス入力エンドポイントは入力エンドポイントに似ていますが、ロード バランサーにポート フォワーディングを使用することで、個々のロール インスタンスごとに、特定の公開ポートをマップできます。 1 つの公開ポートまたはポートの範囲を指定できます。

インスタンス入力エンドポイントでのみ使用できます **tcp** または **udp** プロトコルとして。

インスタンス入力エンドポイントを作成するには、追加、 **InstanceInputEndpoint** の子要素、 **エンドポイント** web またはワーカー ロールの要素。

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## 内部エンドポイント
内部エンドポイントは、インスタンス間の通信で使用できます。 ポートは省略可能なので、省略した場合、動的なポートがエンドポイントに割り当てられます。 ポートの範囲も使用できます。 使用できる内部エンドポイントは、1 ロールあたり 5 つに制限されています。

内部エンドポイントは、次のプロトコルを使用できます: **http、tcp、udp、**です。

内部入力エンドポイントを作成するには、追加、 **InternalEndpoint** の子要素、 **エンドポイント** web またはワーカー ロールの要素。

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

ポートの範囲を使用することもできます。

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## worker ロールとWeb ロールの比較

Worker ロールと Web ロールの両方を使用する場合は、エンドポイントにはわずかな相違点が 1 つあります。 Web ロールが必要に少なくとも 1 つの入力エンドポイントを使用して、 **HTTP** プロトコルです。


```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## .NET SDK を使用してエンドポイントにアクセスする方法
Azure マネージ ライブラリは、実行時に通信できるようロール インスタンスにメソッドを提供します。 ロール インスタンス内で実行するコードから、現在のロール インスタンスに関する情報だけでなく、他のロール インスタンスの存在およびそのエンドポイントに関する情報を取得できます。

> [AZURE.NOTE] クラウド サービスで実行されていると、少なくとも 1 つの内部エンドポイントを定義するロール インスタンスに関する情報のみを取得できます。 別のサービスで実行されているロール インスタンスに関するデータは取得することはできません。

使用することができます、 [インスタンス](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) ロールのインスタンスを取得するプロパティです。 まず使用して、 [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) を使用して現在のロール インスタンスへの参照を返して、 [ロール](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) ロール自体への参照を返すプロパティ。

.NET SDK を介してプログラムによってロール インスタンスに接続する場合、比較的簡単にエンドポイントの情報にアクセスできます。 たとえば、特定のロール環境にすでに接続している場合、次のコードで特定のエンドポイントのポートを取得できます。

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

 **インスタンス** プロパティのコレクションを返します **RoleInstance** オブジェクトです。 このコレクションには常に、現在のインスタンスが含まれています。 ロールによって内部エンドポイントが定義されていない場合、コレクションに現在のインスタンスは含まれますが、その他のインスタンスは含まれません。 コレクション内のロール インスタンスの数は、そのロールの内部エンドポイントが定義されていない場合は常に 1 になります。 ロールによって内部エンドポイントが定義されている場合は、そのインスタンスは実行時に検索可能で、コレクション内のインスタンスの数は、そのロールに対してサービス構成ファイルで指定されたインスタンスの数に一致します。

> [AZURE.NOTE] Azure マネージ ライブラリは他のロール インスタンスの正常性を判断する手段を提供していないが、このような正常性評価自分で実装できる、サービスには、この機能が必要がある場合。 使用する [Azure 診断](https://msdn.microsoft.com/library/azure/gg433048.aspx) ロール インスタンスの実行に関する情報を取得します。

ロール インスタンスの内部エンドポイントのポート番号を確認するのに使用することができます、 [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) エンドポイントの名前と対応する IP を含むディクショナリ オブジェクトのアドレスを返すプロパティとポート。  [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) プロパティは、指定したエンドポイントのポートと IP アドレスを返します。  **PublicIPEndpoint** プロパティは、負荷分散されたエンドポイントのポートを取得します。 IP アドレス部分、 **PublicIPEndpoint** プロパティは使用されません。

ロール インスタンスを反復処理する例を次に示します。

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

これは、サービス定義で公開されるエンドポイントを取得し、接続のリッスンを開始する worker ロールの例です。

> [AZURE.WARNING] このコードは、展開済みサービスでのみ機能します。 Azure コンピューティング エミュレーターで実行する場合は、直接ポート エンドポイントを作成する構成要素をサービス (**InstanceInputEndpoint** 要素) は無視されます。

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;
        
        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;
                
        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);
                
        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## ロール通信を制御するためのネットワーク トラフィック規則
内部エンドポイントを定義した後、作成したエンドポイントに基づいてネットワーク トラフィック規則を追加して、ロール インスタンス間の通信方法を制御できます。 次の図は、ロール通信を制御するためのいくつかの一般的なシナリオを示しています。

![ネットワーク トラフィック規則のシナリオ](./media/cloud-services-enable-communication-role-instances/scenarios.png "Network Traffic Rules Scenarios")

次のコード例は、前の図で示されたロールのロール定義を示しています。 各ロール定義では、少なくとも 1 つの内部エンドポイントが定義されています。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [AZURE.NOTE] ロール間の通信の制限は、固定して、自動的に割り当てられたポートの両方の内部エンドポイントで発生します。

既定では、内部エンドポイントを定義すると、任意のロールから別のロールの内部エンドポイントに、無制限で通信を行うことができます。 通信を制限するには追加、 **NetworkTrafficRules** 要素を **ServiceDefinition** サービス定義ファイル内の要素。

### シナリオ 1
のみからのネットワーク トラフィックを許可する **WebRole1** に **WorkerRole1**します。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### シナリオ 2
ネットワーク トラフィックのみを許可 **WebRole1** に **WorkerRole1** と **WorkerRole2**します。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### シナリオ 3
ネットワーク トラフィックのみを許可 **WebRole1** に **WorkerRole1**, 、および **WorkerRole1** に **WorkerRole2**します。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### シナリオ 4
ネットワーク トラフィックのみを許可 **WebRole1** に **WorkerRole1**, 、**WebRole1** に **WorkerRole2**, 、および **WorkerRole1** に **WorkerRole2**します。

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

上記で使用する要素の XML スキーマ リファレンスをご覧 [ここ](https://msdn.microsoft.com/library/azure/gg557551.aspx)します。

## 次のステップ
詳細については、クラウド サービス [モデル](cloud-services-model-and-package.md)します。
