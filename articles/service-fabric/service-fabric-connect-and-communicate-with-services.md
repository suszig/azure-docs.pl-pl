<properties
   pageTitle="Microsoft Azure Service Fabric サービスと通信する方法"
   description="この記事では、Service Fabric アプリケーションのサービスに接続して通信する方法について説明します。"
   services="service-fabric"
   documentationCenter=".net"
   authors="kunaldsingh"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/21/2015"
   ms.author="kunalds"/>


# サービスとの通信
マイクロサービスの世界では、全体的なソリューションはそれぞれが特殊なタスクを実行する多くの各種サービスで構成されます。 これらのマイクロサービスは、相互に通信してエンド ツー エンドのワークフローを有効にします。 また、サービスに接続して通信するクライアント アプリケーションもあります。 このドキュメントでは、Service Fabric で作成するサービスとの通信のセットアップが、Service Fabric によっていかに容易になるかを説明します。

## 主要な概念
以下は、サービスへの通信をセットアップする際に注意する必要がある主要な概念の一部です。
### 通信はクライアント サーバーとして表される
Service Fabric 通信 API は、同じクラスター内で動作する 2 つのサービスの場合でも、クライアント サービス型の対話を表します。 クライアント (つまり別のサービス) から接続されるターゲット サービスは、サーバーとして機能し、受信要求をリッスンします。 クライアント (クラスター内の別のサービスに過ぎない場合がある) は、サーバーに接続して呼び出しを行います。
### サービスの移動
分散システムでは、実行するサービス インスタンスが、時間の経過と共に 1 台のコンピューターから別のコンピューターに移動することがあります。その理由はさまざまで、たとえばリソース分散のための負荷のメトリックを使用して構成する場合や、アップグレード、フェールオーバー、スケール アウト、その他のさまざまな状況が原因になります。 この影響は、サービス インスタンスのエンドポイント アドレスが変更されることです。サービス インスタンスとの通信をセットアップするには、次のループを実行する必要があります。 これらの詳細は、Service Fabric によって提供される通信 API を使用すると、抽象化されます。

* **解決するには**: Service Fabric 内のすべてのサービス インスタンスの一意の Uri はなどがある"fabric:/myapplication/myservice"と時間の経過と共にこれらの Uri は変わりません。 各サービス インスタンスはエンドポイントも公開しますが、これはサービス インスタンスが移動されるときに変更される可能性があります。 これは、URL は不変でも IP アドレスは変わることがある Websites と似ています。 World Wide Web の DNS が Web サイトの URL を IP アドレスに解決するのと同様に、Service Fabric プラットフォームも、URI をエンドポイントに解決するシステム サービスを持っています。 この手順には、サービス インスタンスの URI をエンドポイントに解決することが含まれます。

* **接続**: サービスの URI がエンドポイント アドレスに解決される、次の手順はそのサービスとの接続を試行します。 サービスの移動でエンドポイントのアドレスが変わっていた場合、この接続は失敗する可能性があります。サービス移動の原因としては、コンピューター エラーや負荷分散があります。

* **再試行**: かどうか、先行する解決し、接続の手順は、接続試行の失敗を再試行する必要があるし、接続が成功するまでこのサイクルが繰り返されます。

## 使用する通信 API を決定する
Service Fabric の一部として通信 API にいくつかの異なるオプションが提供されています。最適なオプションの決定は、プログラミング モデル、通信フレームワーク、およびサービスが作成されているプログラミング言語に応じて異なります。
### 高信頼アクターの通信
高信頼アクター API を使用して作成されたサービスでは、すべての通信の詳細は抽象化されており、通信は ActorProxy へのメソッド呼び出しとして発生するため、ここで読むのをやめて結構です。

### Reliable Services の通信オプション
Reliable Services API を使用してサービスが作成された場合、異なるオプションがいくつかがあります。 通信プロトコルの選択により、使用する Service Fabric の通信 API が決まります。

* **特定の通信プロトコルを使用する必要がなく、短期間で稼働する何か**: 特定の通信フレームワークを選択していない場合は、理想的なオプションを使用する、 [既定のスタック](service-fabric-reliable-services-communication-remoting.md) 、アクター通信モデルと同様のモデルの利用できます。 これは、サービスの通信を開始する最も簡単ですばやい方法です。 これは、厳密に型指定された RPC 通信を提供し、ほとんどの通信の詳細を抽象化するため、コードでリモート サービスを呼び出すと、ローカル オブジェクトのインスタンスでメソッドを呼び出しているように見えます。 これらのクラスは、解決、接続、再試行、およびエラー処理を抽象化しながら、通信チャネルをセットアップして、メソッド呼び出しに基づく通信モデルを可能にします。 このため、`ServiceCommunicationListener` クラスはサーバー側で使用され、`ServiceProxy` クラスは通信のクライアント側で使用されます。

* **HTTP**: Service Fabric 通信 Api を解決中に定義する通信メカニズムを使用する HTTP ベースの通信を提供する柔軟性を活用するには、接続、および再試行のロジックは、抽象化したままです。 たとえば Web API を使用して活用し通信メカニズムを指定することができます、 [`ICommunicationClient` と `ServicePartitionClient` クラス](service-fabric-reliable-services-communication.md) 通信をセットアップします。
* **WCF**: 通信フレームワークとして WCF を使用する既存のコードがあるかどうかは、クライアントのサーバー側では、およびクラスを使用し WcfCommunicationClient でを使用することができます。 詳細について参照してください [今回](service-fabric-reliable-services-communication-wcf.md)します。

* **カスタム プロトコルを含むその他の通信フレームワーク**: Service Fabric 通信 Api を検出し、接続のすべての作業を抽象化したまま、通信スタックをプラグインできるが、カスタムの通信プロトコルを含むその他の通信フレームワークの使用について計画しているかどうか。 参照してください [今回](service-fabric-reliable-services-communication.md) 詳細です。

### 異なるプログラミング言語で記述されたサービスの間の通信
ServiceFabric のすべての通信 API は、現在 C# でのみ使用可能であるため、Java、Node.JS など、その他のプログラミング言語で記述されたサービスがある場合は、独自の通信メカニズムを作成する必要があります。

## 次のステップ
* [Reliable Services フレームワークによって提供される既定の通信スタック ](service-fabric-reliable-services-communication-remoting.md)
* [Reliable Services 通信モデル](service-fabric-reliable-services-communication.md)
* [OWIN 自己ホストによる Microsoft Azure Service Fabric Web API の概要](service-fabric-reliable-services-communication-webapi.md)
* [Reliable Services の WCF ベースの通信スタック](service-fabric-reliable-services-communication-wcf.md)

