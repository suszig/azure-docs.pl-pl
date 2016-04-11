<properties 
   pageTitle="Service Bus リレー型メッセージングのチュートリアル | Microsoft Azure"
   description="Service Bus リレー型メッセージングを使用して、Service Bus クライアント アプリケーションとサービスをビルドします。"
   services="service-bus"
   documentationCenter="na"
   authors="sethmanheim"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="service-bus"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/11/2015"
   ms.author="sethm" />

# Service Bus リレー型メッセージングのチュートリアル

このチュートリアルでは、Service Bus の "リレー" 機能を使用して、単純な Service Bus クライアント アプリケーションとサービスを構築する方法について説明します。 Service Bus の「仲介」または非同期メッセージング機能を使用するアプリケーションを構築する方法を説明する対応するチュートリアルについては、 [サービス バスの仲介型メッセージングに関する .NET チュートリアル](https://msdn.microsoft.com/library/hh367512.aspx)します。 Service Bus を使用する類似のチュートリアル [仲介型メッセージング](service-bus-messaging-overview.md/#Brokered-messaging), を参照してください、 [サービス バスの仲介型メッセージングに関する .NET チュートリアル](https://msdn.microsoft.com/library/hh367512.aspx)します。

このチュートリアルを利用すると、Service Bus のクライアント アプリケーションとサービス アプリケーションを作成するために必要な手順を理解できます。 WCF の該当するサービスと同様に、サービスは 1 つ以上のエンドポイントを公開するコンストラクトで、各エンドポイントは 1 つ以上のサービス操作を公開します。 サービスのエンドポイントでは、サービスが見つかるアドレス、クライアントがサービスとやり取りする必要がある情報を含むバインド、サービスがそのクライアントに提供する機能を定義するコントラクトを指定します。 WCF と Service Bus サービスの主な違いは、エンドポイントがコンピューターのローカルではなくクラウドで公開される点です。

このチュートリアルの一連のトピックに従って操作が完了すると、実行中のサービスとサービスの操作を呼び出すことができるクライアントが作成されます。 最初のトピックでは、アカウントを設定する方法について説明します。 次の手順では、コントラクトを使用するサービスを定義する方法、そのサービスを実装する方法、コードでそのサービスを構成する方法について説明します。 また、サービスをホストして実行する方法についても説明します。 作成されるサービスは自己ホスト型であり、クライアントとサービスは同じコンピューターで実行されます。 このサービスは、コードまたは構成ファイルを使用して構成することができます。 詳細については、次を参照してください。 [Service Bus で登録する WCF サービスを構成する](https://msdn.microsoft.com/library/ee173579.aspx) と [サービス サービス バスを構築する](https://msdn.microsoft.com/library/ee173564.aspx)です。

最後の 3 つの手順では、クライアント アプリケーションの作成方法、クライアント アプリケーションの構成方法、ホストの機能にアクセスできるクライアントを作成して使用する方法について説明します。 詳細については、次を参照してください。 [サービス バス クライアント アプリケーションの構築](https://msdn.microsoft.com/library/ee173543.aspx) と [の発見と Service Bus サービスを公開](https://msdn.microsoft.com/library/dd582704.aspx)します。

このセクションのすべてのトピックでは、開発環境として Visual Studio を使用していることを前提とします。

## アカウントにサインアップする

最初の手順では、Service Bus サービス名前空間を作成し、Shared Access Signature (SAS) キーを取得します。 サービス名前空間は、Service Bus によって公開される各アプリケーションのアプリケーション境界を提供します。 サービス名前空間と SAS キーの組み合わせが、アプリケーションへのアクセスを Service Bus が認証するための資格情報になります。

名前空間を作成する」の手順に従います [How To: 作成または Service Bus Service 名前空間を変更する](https://msdn.microsoft.com/library/hh690931.aspx)です。

>[AZURE.NOTE] クライアントとサービスの両方のアプリケーションに対して、同じ名前空間を使用する必要はありません。

1. メイン ウィンドウで、 [Azure クラシック ポータル][], 、前の手順で作成したサービス名前空間の名前をクリックします。

2. をクリックして **構成** 共有アクセス ポリシー、サービス名前空間を既定値を表示します。

3. 主キーを書き留めて、 **RootManageSharedAccessKey** ポリシー、またはクリップボードにコピーします。 この値は、このチュートリアルの後半で使用します。

## Service Bus で使用する WCF サービス コントラクトを定義する

サービス コントラクトは、サービスでサポートされる操作 (メソッドや関数を表す Web サービスの用語) を指定します。 コントラクトを作成するには、C++、C#、または Visual Basic インターフェイスを定義します。 インターフェイスの各メソッドは、特定のサービス操作に対応しています。 各インターフェイス、 [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) 、属性が適用され、各操作が必要、 [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) 属性を適用します。 持つインターフェイスのメソッドの場合、 [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) 属性がない、 [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) 属性は、メソッドは公開されません。 以下の手順では、これらのタスクのコード例を示します。 コントラクトを定義する方法の詳細については、次を参照してください。 [Service Bus の WCF コントラクトの設計](https://msdn.microsoft.com/library/ee173585.aspx)します。 コントラクトとサービスの拡大については、次を参照してください。 [を設計および実装するサービス](https://msdn.microsoft.com/library/ms729746.aspx) WCF ドキュメントです。

### インターフェイスを使用して Service Bus を作成するには

1. プログラムを右クリックして、管理者として Visual Studio を開き、 **開始** メニュー **管理者として実行**します。

1. 新しいコンソール アプリケーション プロジェクトを作成します。 クリックして、 **ファイル** メニューをクリック **新規**, 、] をクリックし、 **プロジェクト**します。  **新しいプロジェクト** ダイアログ ボックスで、をクリックして **Visual c#** (場合 **Visual C# の場合** 見当たらない場合は、下を見て **他の言語**)。 クリックして、 **コンソール アプリケーション** テンプレートと名付けます **EchoService**します。 既定値を使用して **場所**します。 クリックして **OK** プロジェクトを作成します。

1. 参照を追加 `System.ServiceModel.dll` プロジェクト: ソリューション エクスプ ローラーで右クリックし、 **参照** クリックしてプロジェクト フォルダーの下のフォルダー **参照の追加**します。 選択、 **.NET** ] タブで、 **参照の追加** ダイアログおよびまで下にスクロールするを参照してください **System.ServiceModel**します。 クリックして選択し、 **OK**します。

1. ソリューション エクスプローラーで Program.cs ファイルをダブルクリックしてエディターで開きます。

1. System.ServiceModel 名前空間の `using` ステートメントを追加します。

    ```
    using System.ServiceModel;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) 名前空間をプログラムで WCF の基本機能にアクセスすることができます。 Service Bus は、サービス コントラクトの定義に WCF の多くのオブジェクトと属性を使用します。

1. 既定の名前から名前空間の名前を変更する **EchoService** に **Microsoft.ServiceBus.Samples**します。

    >[AZURE.IMPORTANT] このチュートリアルでは、c# の名前空間 Micr**osoft します。ServiceBus.Samples**, 、これは、コントラクトの名前空間の管理手順 6. で、構成ファイルで使用されている種類: WCF クライアントを構成します。 このサンプルをビルドするときに必要な名前空間を指定できますが、それに応じて、アプリケーション構成ファイルでコントラクトとサービスの名前空間を変更しない限り、このチュートリアルは機能しません。 App.config ファイルで指定する名前空間は、C# ファイルで指定した名前空間と同じである必要があります。

1. 直後に、 `Microsoft.ServiceBus.Samples` 名前空間宣言がという名前の新しいインターフェイスを定義、名前空間内で `IEchoContract` を適用し、 `ServiceContractAttribute` 属性の名前空間の値を持つインターフェイスを **http://samples.microsoft.com/ServiceModel/Relay/**します。 この名前空間の値は、コードのスコープ全体で使用する名前空間とは異なります。 代わりに、この名前空間の値は、このコントラクトの一意の識別子として使用されます。 名前空間を明示的に指定すると、既定の名前空間値がコントラクト名に追加されなくなります。

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    publicinterface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] 通常、サービス コントラクトの名前空間には、バージョン情報を含む名前付けスキームが含まれています。 サービス コントラクトの名前空間にバージョン情報を含めると、サービスは、新しいサービス コントラクトを新しい名前空間を使用して定義し、それを新しいエンドポイントで公開することで、主要な変更を分離できます。 この方法では、クライアントは、古いサービス コントラクトを更新する必要なく、使用し続けることができます。 バージョン情報は、日付またはビルド番号で構成できます。 詳細については、次を参照してください。 [サービスのバージョン管理](http://go.microsoft.com/fwlink/?LinkID=180498)します。 このチュートリアルの目的では、サービス コントラクトの名前空間の名前付けスキームにバージョン情報は含まれません。

1. IEchoContract インターフェイス内で、`IEchoContract` コントラクトがインターフェイスで公開している単一操作のメソッドを宣言し、パブリック Service Bus コントラクトの一部として公開するメソッドに `OperationContractAttribute` 属性を適用します。

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. 次に示すように、コントラクトの外部で、`IEchoChannel` から `IClientChannel` インターフェイスにも継承されるチャネルを宣言します。

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    publicinterface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    publicinterface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    チャネルは、ホストとクライアントが相互に情報を渡すときに経由する WCF オブジェクトです。 後で、2 つのアプリケーション間で情報をエコーするチャネルに対するコードを記述します。

1.  **ビルド** ] メニューのをクリックして **ソリューションのビルド** または f6 キーを押して作業の精度を確認するキーを押します。

### 例

次のコードは、Service Bus コントラクトを定義する基本的なインターフェイスを示しています。

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

これでインターフェイスが作成されたので、このインターフェイスを実装することができます。

## Service Bus を使用するために WCF コントラクトを実装するには

Service Bus サービスを作成するには、まずコントラクトを作成する必要があります。コントラクトは、インターフェイスを使用して定義します。 インターフェイスの作成の詳細については、前の手順を参照してください。 次の手順はインターフェイスの実装です。 これには、ユーザー定義の `EchoService` インターフェイスを実装する `IEchoContract` というクラスの作成が含まれます。 インターフェイスを実装したら、App.config 構成ファイルを使用してインターフェイスを構成します。 構成ファイルには、サービス名、コントラクト名、Service Bus との通信に使用するプロトコルの種類など、アプリケーションに必要な情報が含まれています。 以下の手順では、これらのタスクに使用するコード例を示します。 サービス コントラクトを実装する方法に関する一般的な詳細については、次を参照してください。 [サービス コントラクトを実装する](https://msdn.microsoft.com/library/ms733764.aspx) 、Windows Communication Foundation (WCF) ドキュメントです。

1. `IEchoContract` インターフェイスの定義の直後に、`EchoService` という新しいクラスを作成します。 `EchoService` クラスによって、`IEchoContract` インターフェイスが実装されます。 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    他のインターフェイス実装と同様に、別のファイルに指定した定義を実装することができます。 ただし、このチュートリアルでは、インターフェイス定義や `Main` メソッドと同じファイルに実装します。

1. 適用、 [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) サービス名と名前空間を示す属性です。

    ```[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. `EchoService` クラスの `IEchoContract` インターフェイスで定義された `Echo` メソッドを実装します。 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. をクリックして **ビルド**, 、順にクリックして **ソリューションのビルド** 作業の精度を確認します。

### サービス ホストの構成を定義するには

1. 構成ファイルは WCF 構成ファイルとよく似ており、 サービス名、エンドポイント (つまり、クライアントとホストが相互に通信するために Service Bus が公開している場所)、バインド (通信に使用されているプロトコルの種類) が含まれています。 主な違いは、この構成済みのサービス エンドポイントを参照する、 [netTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx), 、.NET Framework の一部ではないです。 [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) サービス バスによって定義されたバインディングの 1 つです。

1.  **ソリューション エクスプ ローラー**, 、現在、次の XML 要素を含む App.config ファイルをクリックします。

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
    </configuration>
    ```

1. `<system.serviceModel>`XML 要素を App.config ファイルに追加します。 これは 1 つ以上のサービスを定義する WCF 要素です。 この例では、サービス名とエンドポイントの定義に使用されます。

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
  
      </system.serviceModel>
    </configuration>
    ```

1. `<system.serviceModel>` タグ内に、`<services>` 要素を追加します。 1 つの構成ファイルで複数の Service Bus アプリケーションを定義できます。 ただし、このチュートリアルで定義するのは 1 つだけです。
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. `<services>` 要素内に、サービスの名前を定義する `<service>` 要素を追加します。

    ```
    <servicename="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. `<service>` 要素内で、エンドポイント コントラクトの場所に加え、エンドポイントのバインドの種類も定義します。

    ```
    <endpointcontract="Microsoft.ServiceBus.Samples.IEchoContract"binding="netTcpRelayBinding"/>
    ```

    エンドポイントは、クライアントがホスト アプリケーションを検索する場所を定義します。 このチュートリアルでは、後でこの手順を使用して、Service Bus を介してホストを完全に公開する URI を作成します。 バインドでは、Service Bus と通信するプロトコルとして TCP を使用することを宣言します。


1. `<services>` 要素の直後に、次のバインド拡張を追加します。
 
    ```
    <extensions>
      <bindingExtensions>
        <addname="netTcpRelayBinding"type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
    ```

1.  **ビルド** ] メニューのをクリックして **ソリューションのビルド** 作業の精度を確認します。

### 例

次のコードは、サービス コントラクトの実装を示します。

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

次のコードは、サービス ホストに関連付けられる App.config ファイルの基本的な形式を示します。

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding" type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## Service Bus に登録する基本的な Web サービスをホストして実行する

この手順では、基本的な Service Bus サービスを実行する方法について説明します。

### Service Bus 資格情報を作成するには

1. Microsoft.ServiceBus.dll への参照をプロジェクトに追加します。 を参照してください [NuGet Service Bus パッケージを使用して](https://msdn.microsoft.com/library/dn741354.aspx)します。

    >[AZURE.NOTE] コマンド ライン コンパイラを使用する場合は、アセンブリにパスを提供することも必要があります。

1. Program.cs で、Microsoft.ServiceBus 名前空間の `using` ステートメントを追加します。

    ```
    using Microsoft.ServiceBus;
    ```

1. `Main()` で、コンソール ウィンドウから読み取った名前空間と SAS キーを格納するための 2 つの変数を作成します。

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    SAS キーは、後で Service Bus プロジェクトにアクセスするために使用されます。 サービス名前空間は、サービスの URI を作成するために、パラメーターとして `CreateServiceUri` に渡されます。

4. 使用して、 [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) オブジェクトを使用する SAS キー資格情報の種類としてを宣言します。 1 つ前の手順で追加したコードの直後に次のコードを追加します。

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### サービスのベース アドレスを作成するには

1. 1 つ前の手順で追加したコードの次に、サービスのベース アドレスの `Uri` インスタンスを作成します。 この URI では、Service Bus スキーム、名前空間、サービス インターフェイスのパスを指定します。

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    "sb" は Service Bus スキームの省略形であり、プロトコルとして TCP を使用していることを示します。 これも以前と表示された構成ファイルと [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) バインドとして指定されました。
    
    このチュートリアルでは、URI は `sb://putServiceNamespaceHere.windows.net/EchoService` です。

### サービス ホストを作成して構成するには

1. 接続モードを `AutoDetect` に設定します。

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    接続モードは、サービスが Service Bus との通信に使用するプロトコル、つまり HTTP または TCP を表します。 既定の設定である `AutoDetect` を使用すると、サービスは、TCP が利用できる場合は TCP 経由で、TCP が利用できない場合は HTTP 経由で、Service Bus に接続しようとします。 これは、サービスがクライアントの通信に指定するプロトコルとは異なることに注意してください。 そのプロトコルは、使用されるバインドによって決まります。 たとえば、サービスを使用できます、 [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) バインディングで、(サービス バスで公開されている) のエンドポイントが HTTP 経由でクライアントと通信することを指定します。 同じサービスを指定できます **ConnectivityMode.AutoDetect** サービスは TCP を介して Service Bus と通信できるようにします。

1. このセクションで先に作成した URI を使用して、サービス ホストを作成します。

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    サービス ホストは、サービスをインスタンス化する WCF オブジェクトです。 ここで、作成するサービスの種類 (`EchoService` 型) とサービスを公開するアドレスをサービス ホストに渡します。

1. Program.cs ファイルの上部にあるへの参照を追加 [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) と [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx)します。

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. `Main()` に戻り、パブリック アクセスを有効にするようにエンドポイントを構成します。

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    この手順では、プロジェクトの Service Bus ATOM フィードを調べることで、アプリケーションが公的に見つかることを Service Bus に通知します。 設定した場合 **DiscoveryType** に **プライベート**, 、クライアントはサービスにアクセスできます。 ただし、Service Bus 名前空間を検索するとき、サービスは表示されません。 代わりに、クライアントは、エンドポイントのパスを事前に認識しておく必要があります。

1. App.config ファイルで定義されているサービス エンドポイントにサービスの資格情報を適用します。

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    前の手順で説明したように、構成ファイルで複数のサービスとエンドポイントを宣言しておくことができます。 宣言した場合、このコードは構成ファイルを走査し、資格情報を適用する必要があるすべてのエンドポイントを検索します。 ただし、このチュートリアルでは、構成ファイルのエンドポイントは 1 つだけです。

### サービス ホストを開くには

1. サービスを開きます。

    ```
    host.Open();
    ```

1. サービスが実行中であることをユーザーに通知し、サービスをシャットダウンする方法を説明します。

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. 完了したら、サービス ホストを閉じます。

    ```
    host.Close();
    ```

1. F6 キーを押して、プロジェクトをビルドします。

### 例

次の例では、チュートリアルの前の手順で説明したサービス コントラクトと実装が含まれています。ここでは、コンソール アプリケーションでサービスをホストします。 次のコードをコンパイルして、EchoService.exe という実行可能ファイルを作成します。

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## サービス コントラクト用の WCF クライアントを作成する

次の手順では、基本的な Service Bus クライアント アプリケーションを作成し、後の手順で実装するサービス コントラクトを定義します。 これらの手順の多くは、コントラクトの定義、App.config ファイルの編集、資格情報を使用した Service Bus への接続など、サービスの作成に使用した手順に似ています。 以下の手順では、これらのタスクに使用するコード例を示します。

1. 次の手順に従って、クライアント用の現在の Visual Studio ソリューションに新しいプロジェクトを作成します。
    1. ソリューション エクスプ ローラーで、サービスを含む同じソリューション内で (プロジェクトではない)、現在のソリューションを右クリックし] をクリックして **追加**します。 クリックして **新しいプロジェクト**します。
    2.  **新しいプロジェクトの追加** ダイアログ ボックスで、] をクリックして **Visual c#** (場合 **Visual C# の場合** 見当たらない場合は、下を見て **他の言語**) を選択、 **コンソール アプリケーション** テンプレートと名付けます **EchoClient**します。
    3. Click **OK**.
<br />

1. ソリューション エクスプ ローラーで Program.cs ファイルをダブルクリック、 **EchoClient** エディターで開きます。

1. 名前空間の名前を、既定の名前である `EchoClient` から `Microsoft.ServiceBus.Samples` に変更します。

1. プロジェクトの System.ServiceModel.dll への参照を追加します。 
    1. 右クリック **参照** 下にある、 **EchoClient** ソリューション エクスプ ローラーでプロジェクトです。 クリックし、 **参照の追加**します。
    2. このチュートリアルの最初の手順で既にこのアセンブリへの参照を追加したのでそれが一覧に、 **最近使用したファイル** ] タブをクリックします。 クリックして **最近使用したファイル**, 選択してから、 **System.ServiceModel.dll** リストからです。 クリックして **OK**します。 表示されない場合 **System.ServiceModel.dll** で、 **最近使用したファイル** ] タブをクリックして、 **参照** ] タブでに移動し、 **C:\Windows\Microsoft.NET\Framework\v3.0\Windows Communication Foundation**します。 そこからアセンブリを選択します。
<br />

1. 追加、 `using` のステートメント、 [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) Program.cs ファイルの名前空間。 

    ```
    using System.ServiceModel;
    ```

1. 前の手順を繰り返して、Microsoft.ServiceBus.dll への参照を追加し、 [Microsoft.ServiceBus](https://msdn.microsoft.com/library/microsoft.servicebus.aspx) をプロジェクトに名前空間。

1. 次の例に示すように、サービス コントラクトの定義を名前空間に追加します。 この定義がで使用される定義と同じことに注意してください、 **サービス** プロジェクトです。 `Microsoft.ServiceBus.Samples` 名前空間の先頭に次のコードを追加する必要があります。

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    publicinterface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    publicinterface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. F6 キーを押して、クライアントをビルドします。

### 例

次のコードは、EchoClient プロジェクトの Program.cs ファイルの現在の状態を示します。

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

[ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## WCF クライアントを構成する

この手順では、このチュートリアルで前に作成したサービスにアクセスする基本的なクライアント アプリケーションの App.config ファイルを作成します。 この App.config ファイルでは、エンドポイントのコントラクト、バインド、および名前を定義します。 以下の手順では、これらのタスクに使用するコード例を示します。

1. ソリューション エクスプ ローラーで、クライアント プロジェクトでダブルクリック **App.config** を開くには、ファイルは、次の XML 要素が含まれています。

    ```
    <?xmlversion="1.0"?>
    <configuration>
      <startup>
        <supportedRuntimeversion="v4.0"sku=".NETFramework,Version=v4.0"/>
      </startup>
    </configuration>
    ```

1. App.config ファイルに `system.serviceModel` の XML 要素を追加します。

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
    
      </system.serviceModel>
    </configuration>
    ```
    
    この要素は、アプリケーションで WCF スタイルのエンドポイントを使用することを宣言します。 既に説明したように、Service Bus アプリケーションの構成の多くは WCF アプリケーションと同じです。主な違いは、構成ファイルが示す場所です。

1. system.serviceModel 要素内に、`<client>` 要素を追加します。

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    この手順により、WCF スタイルのクライアント アプリケーションを定義していることを宣言します。

1. `client` 要素内で、エンドポイントの名前、コントラクト、バインドの種類を定義します。

    ```
    <endpointname="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    この手順では、エンドポイントの名前、サービスで定義されたコントラクト、クライアント アプリケーションが Service Bus との通信に TCP を使用することを定義します。 エンドポイント名は、次の手順でこのエンドポイント構成をサービス URI とリンクするために使用されます。

1. 直後に、 <client> 要素は、次のバインド拡張機能を追加します。
 
    ```
    <extensions>
      <bindingExtensions>
        <addname="netTcpRelayBinding"type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
    ```

1. クリックして **ファイル**, 、し **すべて保存**します。

## 例

次のコードは、Echo クライアントの App.config ファイルを示します。

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding" type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## Service Bus を呼び出すための WCF クライアントを実装する

この手順では、このチュートリアルで作成したサービスにアクセスする基本的なクライアント アプリケーションを実装します。 サービスと同様に、クライアントも、Service Bus にアクセスするために多くの同じ操作を実行します。

1. 接続モードを設定します。

1. ホスト サービスの場所を示す URI を作成します。

1. セキュリティ資格情報を定義します。

1. 資格情報を接続に適用します。

1. 接続を開きます。

1. アプリケーション固有のタスクを実行します。

1. 接続を閉じます。

ただし、1 つの主な相違点ですが、サービスへの呼び出しでは、クライアント アプリケーションが Service Bus に接続するチャネルを使用する **ServiceHost**します。 以下の手順では、これらのタスクに使用するコード例を示します。

### クライアント アプリケーションを実装するには

1. 接続モードを設定 **AutoDetect**します。 クライアント アプリケーションの `Main()` メソッドの中に次のコードを追加します。

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. コンソールから読み取られるサービス名前空間と SAS キーの値を保持する変数を定義します。

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Service Bus プロジェクトでホストの場所を定義する URI を作成します。

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. サービス名前空間のエンドポイントの資格情報オブジェクトを作成します。

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. App.config ファイルに記述されている構成を読み込むチャネル ファクトリを作成します。

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    チャネル ファクトリは、サービスおよびクライアント アプリケーションが通信に使用するチャネルを作成する WCF オブジェクトです。

1. Service Bus 資格情報を適用します。

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. サービスに対するチャネルを作成して開きます。

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. エコーの基本的なユーザー インターフェイスと機能を記述します。

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    このコードではサービスのプロキシとしてチャネル オブジェクトのインスタンスを使用することに注意してください。

1. チャネルを閉じ、ファクトリを終了します。

    ```
    channel.Close();
    channelFactory.Close();
    ```

## クライアント アプリケーションを実行するには

1. F6 キーを押してソリューションをビルドします。 これにより、このチュートリアルの前の手順で作成したクライアント プロジェクトとサービス プロジェクトの両方がビルドされて、それぞれの実行可能ファイルが作成されます。

1. クライアント アプリケーションを実行する前に、サービス アプリケーションが実行されていることを確認します。

    この時点では、EchoService.exe という名前の Echo サービス アプリケーションの実行可能ファイルが、サービス プロジェクト フォルダーの下の \bin\Debug\EchoService.exe (デバッグ構成の場合) または \bin\Release\EchoService.exe (リリース構成の場合) に配置されています。 サービス アプリケーションを起動するには、このファイルをダブルクリックします。

1. コンソール ウィンドウが開き、名前空間の入力を求めるメッセージが表示されます。 このコンソール ウィンドウにサービス名前空間を入力し、Enter キーを押します。

1. 次に、SAS キーの入力を求められます。 SAS キーを入力し、Enter キーを押します。

    コンソール ウィンドウの出力例を次に示します。 ここで示されている値は例での使用のみを目的としています。

    `Your Service Namespace: myNamespace`

    `Your SAS Key: <SAS key value>`

    サービス アプリケーションが開始され、次の例に示すように、リッスンしているアドレスがコンソール ウィンドウに出力されます。

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`

    `Press [Enter] to exit`
    
1. クライアント アプリケーションを実行します。 この時点では、EchoClient.exe という名前の Echo クライアント アプリケーションの実行可能ファイルが、クライアント プロジェクト ディレクトリの下の .\bin\Debug\EchoClient.exe (デバッグ構成の場合) または .\bin\Release\EchoClient.exe (リリース構成の場合) に配置されています。 クライアント アプリケーションを起動するには、このファイルをダブルクリックします。

1. コンソール ウィンドウが開き、前にサービス アプリケーションについて入力したのと同じ情報の入力を求められます。 前の手順に従って、サービス名前空間、発行者名、発行者のシークレットに、クライアント アプリケーションと同じ値を入力します。

1. これらの値を入力すると、クライアントはサービスに対するチャネルを開き、次のコンソール出力例に示すように、なんらかのテキストを入力するよう求めます。

    `Enter text to echo (or [Enter] to exit):` 

    サービス アプリケーションに送信するテキストを入力し、Enter キーを押します。 このテキストは、Echo サービス操作を介してサービスに送信され、次の出力例のように、サービス コンソール ウィンドウに表示されます。

    `Echoing: My sample text`

    クライアント アプリケーションは、`Echo` 操作の戻り値を受け取ります。これは元のテキストで、コンソール ウィンドウに出力されます。 クライアント コンソール ウィンドウの出力例を次に示します。

    `Server echoed: My sample text`

1. この方法で、クライアントからサービスにテキスト メッセージの送信を続けることができます。 終了したら、クライアント コンソール ウィンドウとサービス コンソール ウィンドウで Enter キーを押して両方のアプリケーションを終了します。

## 例

次の例では、クライアント アプリケーションを作成する方法、サービスの操作を呼び出す方法、操作の呼び出しが完了した後でクライアントを終了する方法を示します。

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

クライアントを開始する前に、サービスが実行中であることを確認してください。 

## 次のステップ

このチュートリアルでは、Service Bus の "リレー" 機能を使用して、Service Bus クライアント アプリケーションとサービスを構築する方法を紹介しました。 Service Bus を使用する類似のチュートリアル [仲介型メッセージング](service-bus-messaging-overview.md/#Brokered-messaging), を参照してください、 [サービス バスの仲介型メッセージングに関する .NET チュートリアル](https://msdn.microsoft.com/library/hh367512.aspx)します。

Service Bus の詳細については、次のトピックを参照してください。

- [Service Bus メッセージングの概要](service-bus-messaging-overview.md)
- [Service Bus の基礎](service-bus-fundamentals-hybrid-solutions.md)
- [Service Bus のアーキテクチャ](service-bus-architecture.md)

[Azure classic portal]: http://manage.windowsazure.com
