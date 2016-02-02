<properties
   pageTitle="アプリケーション用の Web フロントエンドの作成 | Microsoft Azure"
   description="ASP.NET 5 Web API プロジェクトと ServiceProxy によるサービス間通信を使用して、Web に Service Fabric アプリケーションを公開します。"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/21/2015"
   ms.author="seanmck"/>



# アプリケーション用の Web サービス フロントエンドの構築

既定では、Service Fabric サービスは Web にパブリック インターフェイスを提供しません。 HTTP クライアントにアプリケーションの機能を公開するには、エントリ ポイントとして機能する Web プロジェクトを作成し、そこから個々のサービスと通信する必要があります。

このチュートリアルでは、ステートフル サービス プロジェクト テンプレートに基づく Reliable Service が既に含まれるアプリケーションに ASP.NET 5 Web API フロントエンドを追加する方法を説明します。 されていない場合は、読み進めることを検討してください [Visual Studio で、最初のアプリケーションを作成する](service-fabric-create-your-first-application-in-visual-studio.md) このチュートリアルを開始する前にします。


## アプリケーションへの ASP.NET 5 サービスの追加

ASP.NET 5 は軽量のクロスプラットフォーム Web 開発フレームワークであり、最新の Web UI と Web API を作成できます。 ASP.NET Web API プロジェクトを既存のアプリケーションに追加してみましょう。

1. ソリューション エクスプローラーで、アプリケーション プロジェクトの **[サービス]** を右クリックして、**[ファブリック サービスの追加]** を選択します。

    ![既存アプリケーションへの新しいサービスの追加][vs-add-new-service]

2. [新しいサービス] ダイアログ ボックスで、**ASP.NET 5** を選択し、名前を付けます。

    ![新しいサービス ダイアログでの ASP.NET Web サービスの選択][vs-new-service-dialog]

3. 次のダイアログ ボックスでは、一連の ASP.NET 5 プロジェクト テンプレートが提供されます。 Service Fabric アプリケーションの外部で ASP.NET 5 プロジェクトを作成した場合、これらは同じテンプレートであることに注意してください。 このチュートリアルでは **Web API** を選択しますが、完全な Web アプリケーションの構築にも同じ概念を適用できます。

    ![ASP.NET プロジェクトの種類の選択][vs-new-aspnet-project-dialog]

    作成した Web API プロジェクトでは、2 つのサービスがアプリケーションに含まれます。 アプリケーションの作成を続けながら、まったく同じ方法でさらにサービスを追加し、個別にバージョン管理およびアップグレードできます。


## アプリケーションの実行

手順を理解してもらうため、新しいアプリケーションをデプロイし、ASP.NET 5 Web API テンプレートによって提供される既定の動作を見てみます。

1. Visual Studio で F5 キーを押してアプリをデバッグします。

2. 展開が完了したら、Visual Studio は http://localhost:33003 (ポート番号がランダムに割り当てられているし、コンピューターに異なる場合があります) のような ASP.NET Web API サービスのルートにブラウザーを起動します。 ASP.NET 5 Web API テンプレートではルートの既定の動作が提供されないので、ブラウザーでエラーが発生します。

3. 追加 `/api/値` ブラウザー内の場所にします。 これを呼び出す、 `取得` Web API テンプレートと戻り値、既定の応答で ValuesController のメソッドが、テンプレートによって提供される JSON を含む 2 つの文字列の配列。

    ![ASP.NET 5 Web API テンプレートから返される既定値][browser-aspnet-template-values]

    チュートリアルの最後では、これらの既定値をステートフル サービスからの最新のカウンター値で置き換えます。


## サービスへの接続

Service Fabric は、Reliable Services との通信方法において完全な柔軟性を提供します。 1 つのアプリケーションに、TCP でアクセス可能なサービス、HTTP REST API でアクセス可能なサービス、Web ソケットでアクセス可能なサービスが含まれることがあります。 背景に使用できるオプションとのトレードオフについては、次を参照してください。 [サービスと通信する](service-fabric-connect-and-communicate-with-services.md)します。 このチュートリアルではを次の単純な手法のいずれかを使用して、 `ServiceProxy`/`ServiceRemotingListener` SDK で提供されるクラスです。

`ServiceProxy` (リモート プロシージャ呼び出しまたは RPC でモデル化された) のアプローチとして、公開キーに対応するサービス コントラクトし、そのインターフェイスを使用して、サービスと対話するためのプロキシ クラスを生成するためのインターフェイスを定義します。


### インターフェイスの作成

最初に、ステートフル サービスとそのクライアントの間のコントラクトとして機能する、ASP.NET 5 プロジェクトを含むインターフェイスを作成します。

1. ソリューション エクスプローラーでソリューションを右クリックし、**[追加]、[新しいプロジェクト]** の順に選択します。

2. 左側のナビゲーション ウィンドウで Visual C# のエントリを選択し、**[クラス ライブラリ]** テンプレートを選択します。 .NET Framework のバージョンが 4.5.1 に設定されていることを確認します。

    ![ステートフル サービス用のインターフェイス プロジェクトの作成][vs-add-class-library-project]

3. インターフェイスで使用できるようにするために `ServiceProxy`, 、Service Fabric の NuGet パッケージの 1 つに含まれている IService インターフェイスから派生する必要があります。 パッケージを追加するには、新しいクラス ライブラリ プロジェクトを右クリックして、**[NuGet パッケージの管理]** を選択します。

4. **[プレリリースを含める]** チェック ボックスがオンになっていることを確認した後、**Microsoft.ServiceFabric.Services** パッケージを検索してインストールします。

    ![Services NuGet パッケージの追加][vs-services-nuget-package]

5. クラス ライブラリでは、1 つのメソッドを使用して、インターフェイスを作成 `GetCountAsync`, 、および IService からインターフェイスを拡張します。

    ```c#
    namespace MyStatefulService.Interfaces
    {
        using Microsoft.ServiceFabric.Services.Remoting;

        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```


### ステートフル サービスでのインターフェイスの実装

インターフェイスを定義したので、次にステートフル サービスでそれを実装する必要があります。

1. ステートフル サービスで、インターフェイスを含むクラス ライブラリ プロジェクトへの参照を追加します。

    ![ステートフル サービスのクラス ライブラリ プロジェクトへの参照の追加][vs-add-class-library-reference]

2. 継承されるクラスを探し `StatefulService`, など `MyStatefulService`, を実装するように拡張し、 `ICounter` インターフェイスです。

    ```c#
    public class MyStatefulService : StatefulService, ICounter
    {        
          // ...
    }
    ```

3. ここで定義されている 1 つのメソッドを実装、 `ICounter` インターフェイス、 `GetCountAsync`します。

    ```c#
    public async Task<long> GetCountAsync()
    {
      var myDictionary =
        await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```


### ServiceRemotingListener を使用したステートフル サービスの公開

`ICounter` 実装されるインターフェイスは、最後に、他のサービスから呼び出し可能であるステートフルなサービスを有効にする方法は、通信チャネルを開きます。 Service Fabric ステートフル サービスと呼ばれる上書き可能なメソッドに提供 `CreateServiceReplicaListeners` を有効にする、サービスに必要な通信の種類に基づいて 1 つまたは複数の通信リスナーを指定することができます。
>[AZURE.NOTE] ステートレスなサービスへの通信チャネルを開くための同等のメソッドが呼び出されます `CreateServiceInstanceListeners`します。

ここでは、 `ServiceRemotingListener`, を使用してクライアントからは RPC エンドポイントを呼び出し可能な作成を `ServiceProxy`します。

```c#
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (initParams) =>
                new ServiceRemotingListener<ICounter>(initParams, this))
    };
}
```


### ServiceProxy を使用したサービスとの対話

ステートフル サービスが他のサービスからトラフィックを受信できるようになったので、残っているのは、ASP.NET Web サービスからそれと通信するためのコードを追加することだけです。

1. ASP.NET プロジェクトを含むクラス ライブラリへの参照を追加、 `ICounter` インターフェイスです。

2. 前にクラス ライブラリ プロジェクトで行ったのと同様に、ASP.NET プロジェクトに Microsoft.ServiceFabric.Services パッケージを追加します。 これにより、 `ServiceProxy` クラスです。

3. Controllers フォルダーで開き、 `ValuesController` クラスです。 なお、 `取得` メソッドは、現在だけ"value1"と"value2"は、ブラウザーで既に見た内容と一致するのでは、ハードコーディングされた文字列配列を返します。 この実装を次のコードに置き換えます。

    ```c#
    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(0, new Uri("fabric:/MyApp/MyStatefulService"));

        long count = await counter.GetCountAsync();

        return new string[] { count.ToString() };
    }
    ```

    コードの最初の行が重要です。 ステートフル サービスへの ICounter プロキシを作成するには、パーティション ID とサービス名の 2 つの情報を提供する必要があります。

    パーティション分割を使用すると、顧客 ID や郵便番号などの定義したキーに基づいて異なるバケットにステートを分割することにより、ステートフル サービスを拡張できます。 この単純なアプリケーションではステートフル サービスはパーティションを 1 つしか持たないので、キーは重要ではありません。どのようなキーを提供しても同じパーティションになります。 サービスのパーティション分割の詳細については、次を参照してください。 [Service Fabric の信頼性の高いサービスのパーティション方法](service-fabric-concepts-partitioning)します。

    サービス名は、フォームのファブリックの URI:/< $application_name >/< サービス名 >。

    これら 2 つの情報により、Service Fabric は要求送信先のコンピューターを一意に識別できます。  `ServiceProxy` 、ステートフル サービス パーティションをホストしているコンピューターは失敗し、別のコンピューターを置き換えることを昇格させる必要がある場合はシームレスに処理します。 この抽象化により、他のサービスを処理するクライアント コードの作成が大幅に簡略化されます。

    単に呼び出しますプロキシを取得したら、 `GetCountAsync` メソッドし、その結果を返します。

4. 再び F5 キーを押して、変更したアプリケーションを実行します。 前と同じように、Visual Studio はブラウザーを自動的に起動して Web プロジェクトのルートを表示します。 "api/values" パスを追加すると、返される現在のカウンター値がわかります。

    ![ブラウザーに表示されたステートフル カウンター値][browser-aspnet-counter-value]

    定期的にブラウザーを更新して、カウンターの値が更新されるのを確認します。


## アクターについて

このチュートリアルではステートフル サービスと通信する Web フロントエンドの追加に注目しましたが、非常によく似たモデルに従ってアクターと対話できます。 実際にはもう少し簡単です。

アクター プロジェクトを作成すると、Visual Studio によってインターフェイス プロジェクトが自動的に生成されます。 そのインターフェイスを使用して Web プロジェクトでアクター プロキシを生成し、アクターと通信できます。 何も確立することに相当する必要はありませんので、通信チャネルを自動的に提供する `ServiceRemotingListener` このチュートリアルでは、ステートフル サービスの場合とします。

## ローカル クラスターでの Web サービスの実行

一般に、ローカル クラスターは 5 ノードの構成を 1 つのコンピューターにまとめただけなので、ローカル クラスターにデプロイした複数コンピューター クラスターにまったく同じ Service Fabric アプリケーションをデプロイでき、意図したとおりに動作することを信頼できます。

ただし、Web サービスに関しては 1 つの重要な違いがあります。 クラスターが Azure でのようにロード バランサーの背後に配置されている場合、ロード バランサーはコンピューター間にトラフィックをラウンドロビンするだけなので、Web サービスをすべてのコンピューターにデプロイする必要があります。 設定してそのため、 `InstanceCount` サービス-1 の特殊な値にします。 一方、ローカルで実行するときは、実行するサービスのインスタンスを 1 つだけにする必要があります。そうしないと、同じパスとポートでリッスンしている複数のプロセスが競合します。 そのため、ローカル デプロイメントの場合は Web サービスのインスタンス数を 1 に設定する必要があります。

別の環境に別の値を構成する方法については、次を参照してください。 [アプリケーションのパラメーターを複数の環境を管理する](service-fabric-manage-multiple-environment-app-configuration.md)します。

## 次のステップ

- [クラウドに移行するアプリケーションを配置する Azure でクラスターを作成します。](service-fabric-cluster-creation-via-portal.md)
- [サービスとの通信の詳細についてください。](service-fabric-connect-and-communicate-with-services.md)
- [ステートフル サービスのパーティション分割の詳細についてください。](service-fabric-concepts-partitioning.md)




[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png 
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png 
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png 
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png 
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png 
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png 
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png 
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png 

