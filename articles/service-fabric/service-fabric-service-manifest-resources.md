<properties
   pageTitle="Service Fabric サービス エンドポイントの指定 | Microsoft Azure"
   description="HTTPS エンドポイントのセットアップなど、サービス マニフェストにエンドポイント リソースを記述する方法"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/26/2015"
   ms.author="sumukhs"/>


# サービス マニフェストでのリソースの指定

## 概要

サービス マニフェストを使用すると、コンパイルしたコードを変更することなく、サービスで使用するリソースを宣言/変更できます。 Service Fabric は、サービスで使用するエンドポイント リソースの構成をサポートします。 サービス マニフェストで指定したリソースへのアクセスは、SecurityGroup を使用してアプリケーション マニフェスト内で制御できます。 リソースを宣言すると、宣言したリソースをデプロイメント時に変更できるため、サービスに新しい構成メカニズムを導入する必要がありません。

## Endpoints

サービス マニフェストにエンドポイント リソースが定義されている場合、明示的なポートが指定されていない場合、Service Fabric は、予約済みのアプリケーション ポートの範囲からポートを割り当てます (例: 以下のエンドポイント *ServiceEndpoint1* を参照してください)。 さらに、サービスでリソースの特定のポートを要求することもできます。 別のクラスター ノードで実行しているサービスのレプリカに異なるポート番号を割り当てることができます。一方、同じノードで実行している同じサービスのレプリカは、同じポートを共有します。 このようなポートは、レプリケーションやクライアント要求のリッスンなどのさまざまな目的のために、サービスのレプリカで使用できます。

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

参照してください [ステートフル Reliable Services の構成](../Service-Fabric/service-fabric-reliable-services-configuration.md) を読み取るファイル (settings.xml) の詳細について、構成パッケージの設定からエンドポイントを参照します。

## 例: サービスの HTTP エンドポイントを指定する

次のサービス マニフェストは、< リソース > 要素の 1 つの TCP エンドポイント リソースと 2 つの HTTP エンドポイント リソースを定義します。

HTTP エンドポイントは Service Fabric によって自動的に ACL に登録されます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## 例: サービスの HTTPS エンドポイントを指定する

HTTPS プロトコルはサーバー認証を提供し、また、クライアント/サーバー間通信の暗号化に使用されます。 Service Fabric サービスでこれを有効にするには、サービスを定義するときに、エンドポイント *ServiceEndpoint3* に関する前述の説明のように、サービス マニフェストの *[リソース] -> [エンドポイント] -> [エンドポイント]* セクションでプロトコルを指定します。
>[AZURE.NOTE] アプリケーションのアップグレード中はサービスのプロトコルを変更することはできません。これは、重大な変更であるためです。 


以下に、HTTPS の設定に必要な ApplicationManifest の例を示します (証明書の捺印を指定するために必要になります)。 EndpointRef は、HTTPS プロトコルを設定する ServiceManifest の EndpointResource への参照です。 1 つ以上の Endpointcertificates を追加することができます。

```
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```





