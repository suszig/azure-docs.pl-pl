<properties
   pageTitle="リレー型メッセージングを使用した Service Bus REST チュートリアル | Microsoft Azure"
   description="REST ベースのインターフェイスを表示する簡易な Service Bus Relay ホスト アプリケーションを構築します。"
   services="service-bus"
   documentationCenter="na"
   authors="sethmanheim"
   manager="timlt"
   editor="" />
<tags
   ms.service="service-bus"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/14/2015"
   ms.author="sethm" />

# Service Bus REST のチュートリアル

このチュートリアルでは、REST ベースのインターフェイスを表示する簡易な Service Bus ホスト アプリケーションを構築する方法について説明します。 REST を使用すると、Web ブラウザーなどの Web クライアントから HTTP 要求を介して Service Bus API にアクセスできるようになります。

このチュートリアルでは、Windows Communication Foundation (WCF) REST プログラミング モデルを使用して、Service Bus に REST サービスを構築します。 詳細については、次を参照してください。 [WCF REST プログラミング モデル](https://msdn.microsoft.com/library/bb412169.aspx) と [を設計および実装するサービス](https://msdn.microsoft.com/library/ms729746.aspx) WCF ドキュメントです。

## 手順 1: Azure アカウントにサインアップする

最初の手順では、サービス名前空間を作成し、Shared Access Signature (SAS) キーを取得します。 名前空間は、Service Bus によって公開される各アプリケーションのアプリケーション境界を提供します。 サービス名前空間が作成された時点で、SAS キーが生成されます。 サービス名前空間と SAS キーの組み合わせが、アプリケーションへのアクセスを Service Bus が認証する資格情報になります。

### サービス名前空間を作成し、SAS キーを取得するには

1. 名前空間の作成、 [Azure クラシック ポータル][], 、手順に従います [How To: 作成または Service Bus Service 名前空間を変更する](https://msdn.microsoft.com/library/hh690931.aspx)です。

2. ポータルのメイン ウィンドウで、前の手順で作成したサービス名前空間の名前をクリックします。

3. クリックして **構成** 名前空間の共有アクセス ポリシーを表示します。

4. 主キーを書き留めて、 **RootManageSharedAccessKey** ポリシー、またはクリップボードにコピーします。 この値は、チュートリアルの後半で使用します。

## 手順 2: Service Bus で使用する REST ベースの WCF サービス コントラクトを定義する

他の Service Bus Service と同様に、REST スタイルのサービスを作成するときは、コントラクトを定義する必要があります。 コントラクトには、ホストがサポートする操作を指定します。 サービス操作は、Web サービス メソッドと考えることができます。 コントラクトを作成するには、C++、C#、または Visual Basic インターフェイスを定義します。 インターフェイスの各メソッドは、特定のサービス操作に対応しています。  [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) 属性は、各インターフェイスに適用する必要があり、 [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) 属性は、各操作に適用する必要があります。 持つインターフェイスのメソッドの場合、 [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) が、 [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), 、メソッドは公開されません。 これらのタスクに使用されるコードの例を手順に従って説明します。

基本の Service Bus コントラクトと REST スタイル コントラクトの主な違いは、プロパティの追加、 [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx)します。 このプロパティを使用すると、インターフェイス内のメソッドを相手側のインターフェイスのメソッドにマップすることができます。 使用して、この場合、 [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) メソッドを HTTP GET にリンクします。 その結果、Service Bus は、インターフェイスに送信されたコマンドをより正確に取得および解釈できるようになります。

### インターフェイスを使用して Service Bus を作成するには

1. 管理者として Visual Studio を開きます: でプログラムを右クリックし、 **開始** ] メニューの [クリックして **管理者として実行**します。

2. 新しいコンソール アプリケーション プロジェクトを作成します。 クリックして、 **ファイル** メニューをクリック **新規**, 選択してから、 **プロジェクト**します。  **新しいプロジェクト** ダイアログ ボックスで、] をクリックして **Visual c#** (場合 **Visual C# の場合** 見当たらない場合は、下を見て **他の言語**) を選択、 **コンソール アプリケーション** テンプレートと名付けます **ImageListener**します。 既定値を使用して **場所**します。 クリックして **OK** プロジェクトを作成します。

3. C# プロジェクトの場合、`Program.cs` ファイルが作成されます。 このクラスには、空の `Main()` メソッドが含まれています。このメソッドは、コンソール アプリケーション プロジェクトを正常にビルドするために必要です。

4. 参照を追加 **System.ServiceModel.dll** をプロジェクトにします。

    a. ソリューション エクスプ ローラーで右クリックし、 **参照** クリックしてプロジェクト フォルダーの下のフォルダー **参照の追加**します。

    b. クリックして、 **.NET** ] タブで、 **参照の追加** ] ダイアログ ボックスとまで下にスクロールするを参照してください **System.ServiceModel**します。 をクリックして選択し、 **OK**します。

5. 参照を追加する前の手順を繰り返して、 **System.ServiceModel.Web.dll** アセンブリ。

6. 追加 `using` ステートメントを **System.ServiceModel**, 、**System.ServiceModel.Channels**, 、**System.ServiceModel.Web**, 、および **System.IO** 名前空間。

    ```c
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) WCF の基本機能にプログラムでアクセスできるようにするための名前空間です。 Service Bus は、サービス コントラクトの定義に WCF の多くのオブジェクトと属性を使用します。 ほとんどの場合、Service Bus Relay アプリケーションにはこの名前空間を使用することになります。 同様に、 [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) Service Bus とクライアントの web ブラウザーで通信を経由するオブジェクトであると、チャネルの定義に役立ちます。 最後に、 [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) web ベースのアプリケーションを作成するための型が含まれています。

7. Visual Studio の既定のプログラムの名前空間の名前を変更 **Microsoft.ServiceBus.Samples**します。

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. 直接名前空間宣言の後にという名前の新しいインターフェイス定義 **IImageContract** を適用し、 **ServiceContractAttribute** 属性の値を持つインターフェイスを `http://samples.microsoft.com/ServiceModel/Relay/`します。 この名前空間の値は、コードのスコープ全体で使用する名前空間とは異なります。 この名前空間の値は、このコントラクトの一意の識別子として使用されます。値にはバージョン情報が含まれています。 詳細については、次を参照してください。 [サービスのバージョン管理](http://go.microsoft.com/fwlink/?LinkID=180498)します。 名前空間を明示的に指定すると、既定の名前空間値がコントラクト名に追加されなくなります。

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. `IImageContract` インターフェイス内で、`IImageContract` コントラクトがインターフェイスで公開している単一操作のメソッドを宣言し、パブリック Service Bus コントラクトの一部として公開するメソッドに `OperationContractAttribute` 属性を適用します。

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. 次に、 **OperationContract** 属性を適用、 **WebGet** 属性です。

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    HTTP GET 要求をルーティングする Service Bus これにより、 **GetImage**, の戻り値に変換し、 **GetImage** を HTTP GETRESPONSE 応答します。 このチュートリアルの後半で、Web ブラウザーを使用してこのメソッドにアクセスし、ブラウザーに画像を表示します。

11. `IImageContract` 定義の直後に、`IImageContract` インターフェイスと `IClientChannel` インターフェイスの両方から継承するチャネルを宣言します。

    ```
    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    チャネルは、サービスとクライアントが相互に情報を渡すときに経由する WCF オブジェクトです。 後述の手順でホスト アプリケーションでチャネルを作成します。 ブラウザーから HTTP GET 要求を渡すため、このチャネルを使用して Service Bus、 **GetImage** 実装します。 Service Bus は、実行するチャネルを使用するもの **GetImage** 値を返し、クライアントのブラウザーに対する HTTP GETRESPONSE に変換します。

12.  **ビルド** ] メニューのをクリックして **ソリューションのビルド** をこれまで作業の精度を確認します。

### 例

次のコードは、Service Bus コントラクトを定義する基本的なインターフェイスを示しています。

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## 手順 3: Service Bus を使用する REST ベースの WCF サービス コントラクトを実装する

REST スタイルの Service Bus Service を作成するには、まずコントラクトを作成する必要があります。コントラクトは、インターフェイスを使用して定義します。 次の手順はインターフェイスの実装です。 これではという名前のクラスを作成する **ImageService** ユーザー定義を実装する **IImageContract** インターフェイスです。 コントラクトを実装したら、App.config ファイルを使用してインターフェイスを構成します。 構成ファイルには、サービス名、コントラクト名、Service Bus との通信に使用するプロトコルの種類など、アプリケーションに必要な情報が含まれています。 以下の手順では、これらのタスクに使用するコード例を示します。

これまでの手順と同様に、REST スタイルのコントラクトと基本の Service Bus コントラクトの実装にはほとんど違いがありません。

### REST スタイルの Service Bus コントラクトを実装するには

1. という新しいクラスを作成 **ImageService** の定義の直後に、 **IImageContract** インターフェイスです。  **ImageService** クラスが実装する、 **IImageContract** インターフェイスです。

    ```
    class ImageService : IImageContract
    {
    }
    ```
    他のインターフェイス実装と同様に、別のファイルに指定した定義を実装することができます。 ただし、このチュートリアルでは、インターフェイス定義や `Main()` メソッドと同じファイルで実装します。

2. 適用、 [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) 属性を **IImageService** クラス、クラスが WCF コントラクトの実装であることを示します。

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    既に説明したように、この名前空間は従来の名前空間ではありません。 この名前空間は、コントラクトを特定する WCF アーキテクチャの一部です。 詳細については、次を参照してください。、 [データ コントラクト名](https://msdn.microsoft.com/library/ms731045.aspx) WCF ドキュメントのトピックです。

3. .jpg 画像をプロジェクトに追加します。  

    これは、サービスが受信側ブラウザーに表示する画像です。 プロジェクトを右クリックし、クリックして **追加**します。 クリックして **既存項目の**です。 使用して、 **既存項目の追加** ] ダイアログ ボックスで、適切な .jpg を参照してクリックし、 **追加**します。

    ファイルを追加するときに、以下のことを確認 **ファイルをすべて** ] の横にドロップダウン リストで選択されて、 **ファイル名:** フィールドです。 以降、このチュートリアルでは、画像名が "image.jpg" という前提で説明します。 別のファイルを使用する場合は、画像のファイル名を変更するか、ファイル名に合わせてコードを変更します。

4. 実行中のサービスの検出で、イメージ ファイルがで処理できるようにする **ソリューション エクスプ ローラー** イメージ ファイルを右クリックします。  **プロパティ** ペインで、設定 **出力ディレクトリにコピー** に **新しい場合はコピー**します。

5. 参照を追加、 **System.Drawing.dll**, 、**system.runtime.serialization.dll から参照できる**, 、および **Microsoft.ServiceBus.dll** アセンブリをプロジェクトにも関連付けられている次のコードを追加 `using` ステートメントです。  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6.  **ImageService** クラスにビットマップを読み込み、クライアント ブラウザーに送信する準備をする次のコンス トラクターを追加します。

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. 直接前のコードの直後後に次のコードを追加 **GetImage** メソッドで、 **ImageService** 、画像を含む HTTP メッセージを返すためにします。

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    この実装を使用して **MemoryStream** 画像を取得し、ブラウザーにストリーミングする準備をします。 ゼロ位置からストリーミングを開始し、ストリーム コンテンツを jpeg 形式と宣言し、情報をストリーミングします。

8.  **ビルド** ] メニューのをクリックして **ソリューションのビルド**します。

### Service Bus で Web サービスを実行するための構成を定義するには

1. 右クリックし、 **ImageListener** プロジェクトです。 クリックして **追加**, 、し **[新しい項目の**です。

2.  **ソリューション エクスプ ローラー**, をダブルクリックして **App.config**, が現在、次の XML 要素が含まれています。

    ```
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
    </configuration>
    ```

    この構成ファイルは WCF 構成ファイルと似ており、サービス名、エンドポイント (つまり、Service Bus がクライアントおよびホストと相互に通信するために公開している場所)、バインディング (通信に使用されているプロトコルの種類) が含まれています。 主な違いは、構成されたサービス エンドポイントを参照する、 [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) バインディングで、.NET Framework の一部ではありません。 Service Bus アプリケーションを構成する方法の詳細については、次を参照してください。 [Service Bus で登録する WCF サービスを構成する](https://msdn.microsoft.com/library/ee173579.aspx)です。


3. `<system.serviceModel>`XML 要素を App.config ファイルに追加します。 これは 1 つ以上のサービスを定義する WCF 要素です。 この要素は、サービス名とエンドポイントの定義に使用されます。

    ```
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <system.serviceModel>

        </system.serviceModel>

    </configuration>
    ```

4. `system.serviceModel`要素内に、次の内容の `<bindings>` 要素を追加します。 ここでアプリケーションに使用するバインドを定義します。 複数のバインドを定義できますが、このチュートリアルで定義するバインドは 1 つのみです。

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    この手順は、Service Bus を定義 [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) バインディング **relayClientAuthenticationType** に設定 **None**します。 この設定は、このバインドを使用するエンドポイントは、クライアントの資格情報を必要としないことを示します。

5. `<bindings>` 要素の後に、`<services>` 要素を追加します。 バインドと同様に、1 つの構成ファイルで複数のサービスを定義できます。 ただし、このチュートリアルで定義するサービスは 1 つのみです。

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    この操作により、以前に定義された既定値を使用するサービスを **webHttpRelayBinding**します。 既定値も使用 **sbTokenProvider**, 、次の手順で定義されています。

6. 後に、 `<services>` 要素を作成、 `<behaviors>` "sas_key"次のコンテンツの要素を *共有アクセス署名* (SAS) キーから取得した、 [Azure クラシック ポータル][] 手順 1. でします。

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

7.  **ビルド** ] メニューのをクリックして **ソリューションのビルド** ソリューション全体をビルドします。

### 例

次のコードを使用して Service Bus で実行されている REST ベースのサービスのコントラクトとサービスの実装を示しています、 **WebHttpRelayBinding** バインドします。

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

次の例は、サービスに関連付けられている App.config ファイルです。

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <bindings>
      <!-- Application Binding -->
      <webHttpRelayBinding>
        <binding name="default">
          <!-- Turn off client authentication so that client does not need to present credential through browser or fiddler -->
          <security relayClientAuthenticationType="None" />
        </binding>
      </webHttpRelayBinding>
    </bindings>

    <services>
      <!-- Application Service -->
      <service name="Microsoft.ServiceBus.Samples.ImageService"
               behaviorConfiguration="default">
        <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
      </service>
    </services>

    <behaviors>
      <endpointBehaviors>
        <behavior name="sbTokenProvider">
          <transportClientEndpointBehavior>
            <tokenProvider>
              <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
            </tokenProvider>
          <transportClientEndpointBehavior>
        </behavior>
      </endpointBehaviors>
      <serviceBehaviors>
        <behavior name="default">
          <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
        </behavior>
      </serviceBehaviors>
    </behaviors>

  </system.serviceModel>
</configuration>
```

## 手順 4: Service Bus を使用する REST ベースの WCF サービスをホストする

この手順では、Service Bus でコンソール アプリケーションを使用して、Web サービスを実行する方法について説明します。 この手順で作成するコードの詳細については、手順に従って例を使用して説明します。

### サービスのベース アドレスを作成するには

1. `Main()`関数の宣言で、Service Bus プロジェクトの名前空間を格納する変数を作成します。

    ```
    string serviceNamespace = "InsertServiceNamespaceHere";
    ```
    Service Bus は、名前空間の名前を使用して一意の URI を作成します。

2. 名前空間に基づくサービスのベース アドレスの `Uri` インスタンスを作成します。

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### Web サービス ホストを作成および構成するには

- このセクションで作成した URI アドレスを使用して、Web サービス ホストを作成します。

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    サービス ホストは、ホスト アプリケーションをインスタンス化する WCF オブジェクトです。 この例では、作成するホストの種類 (、 **ImageService**) とホスト アプリケーションを公開するアドレス。

### Web サービス ホストを実行するには

1. サービスを開きます。

    ```
    host.Open();
    ```
    サービスが実行されます。

2. サービスが実行中であることと、サービスの停止方法を示すメッセージが表示されます。

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. 完了したら、サービス ホストを閉じます。

    ```c
    host.Close();
    ```

## 例

次の例では、チュートリアルの前の手順で説明したサービス コントラクトと実装が含まれています。ここでは、コンソール アプリケーションでサービスをホストします。 次のコードをコンパイルして、ImageListener.exe という実行可能ファイルを作成します。

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### コードのコンパイル

ソリューションをビルドしたら、次の手順でアプリケーションを実行します。

1. コマンド プロンプトからサービス (ImageListener\bin\Debug\ImageListener.exe) を実行します。

2. コマンド プロンプトのアドレスをコピーし、ブラウザーに貼り付けて画像を確認します。

## 次のステップ

ここでは、Service Bus Relay サービスを使用するアプリケーションを構築しました。リレー型メッセージングの詳細については、次の記事を参照してください。

- [Azure Service Bus アーキテクチャの概要](service-bus-fundamentals-hybrid-solutions.md#relays)

- [サービス バス リレー サービスの使用方法](service-bus-dotnet-how-to-use-relay.md)

[Azure classic portal]: http://manage.windowsazure.com
