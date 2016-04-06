<properties
   pageTitle="Service Fabric アプリケーション トレース ストアとして ElasticSearch を使用する | Microsoft Azure"
   description="Service Fabric アプリケーションで ElasticSearch と Kibana を使用してアプリケーション トレース (ログ) の格納、インデックス作成、および検索を行う方法について説明します。"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/02/2015"
   ms.author="karolz@microsoft.com"/>

# Service Fabric アプリケーション トレース ストアとして ElasticSearch を使用する
## はじめに
この記事で説明する方法 [Service Fabric](http://azure.microsoft.com/documentation/services/service-fabric/) アプリケーションを使用して **ElasticSearch** と **Kibana** アプリケーション トレースの記憶域、インデックス作成および検索をします。 [ElasticSearch](https://www.elastic.co/guide/index.html) はオープン ソース、分散、および拡張性の高いリアルタイム検索および分析するエンジンと、このタスクに適していて、Microsoft Azure で実行されている Windows または Linux の仮想マシンにインストールすることができます。 ElasticSearch は非常に効率よく処理できる *構造化された* などのテクノロジを使用して生成されたトレース **Event Tracing for Windows (ETW)**します。

ETW は、Service Fabric ランタイムが診断情報 (トレース) をソースにするために使用します。Service Fabric アプリケーションも診断情報をソースにする場合に推奨される方法です。 この方法ではランタイムで提供されたトレースとアプリケーションから提供されたトレースを関連付けることができるので、トラブルシューティングが簡単になります。 Visual Studio での Service Fabric プロジェクト テンプレートにはロギング API が含まれます (.NET に基づいて **EventSource** クラス) 既定で ETW トレースを出力します。 ETW を使用して Service Fabric アプリケーションのトレースの概要を参照してください [今回](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)します。

ElasticSearch に出現するトレースの場合、リアル タイムで Service Fabric クラスター ノードでキャプチャし、ElasticSearch エンドポイントに送信する必要があります。 トレースのキャプチャには、主に 2 つのオプションがあります。

+ **プロセスでトレースのキャプチャ**  
アプリケーション、またはプロセスを正確には、サービスは、トレース ストア (ElasticSearch) から診断データが送信を担当します。

+ **プロセス外のトレースのキャプチャ**  
別のエージェントがサービスのプロセスからトレースをキャプチャし、トレースのストアに送信します。

以降、この記事では、Azure で ElasticSearch を設定する方法、2 つのキャプチャ オプションの長所と短所、ElasticSearch にデータを送信するファブリック サービスを構成する方法について説明します。


## Azure で ElasticSearch を設定する
Azure で ElasticSearch サービスを設定する最も簡単な方法は [**Azure ARM テンプレート**](../resource-group-overview.md)します。 包括的な [ElasticSearch のクイック スタートの ARM テンプレート](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) は Azure のクイック スタート テンプレート リポジトリから入手できます。 このテンプレートでは、スケール ユニット (ノードのグループ) に別のストレージ アカウントを使用するので、異なる構成で、接続されているデータ ディスク数が異なる別のクライアントとサーバーをプロビジョニングできます。

この記事と呼ばれる別のテンプレートを使用して **ES マルチノード** から、 [Microsoft Patterns & Practices ELK ブランチ](https://github.com/mspnp/semantic-logging/tree/elk/)します。 このテンプレートは使い方が簡単で、既定で HTTP 基本認証で保護たれ ElasticSearch クラスターを簡単に作成できます。 続行する前にダウンロードしてください、 [Microsoft P & P"elk"リポジトリ](https://github.com/mspnp/semantic-logging/tree/elk/) (には、リポジトリを複製または ZIP ファイルをダウンロードするか) のコンピューターに GitHub からです。 ES-MultiNode テンプレートは、同じ名前のフォルダーに格納されています。
>[AZURE.NOTE] ES マルチノード テンプレートと関連付けられているスクリプトは、現在 ElasticSearch 1.7 リリースをサポートします。 今後、ElasticSearch 2.0 のサポートが追加される予定です。

### コンピューターで ElasticSearch のインストール スクリプトを実行する準備を整える
ES-MultiNode テンプレートを使用するには、提供されている `CreateElasticSearchCluster`という PowerShell スクリプトを使用するのが最も簡単な方法です。 このスクリプトを使用するには、Azure PowerShell モジュールと、openssl というツールをインストールする必要があります。 openssl は、SSH キーを作成するために必要です。SSH キーは、ElasticSearch クラスターをリモート管理するために使用できます。

注: `CreateElasticSearchCluster` スクリプトは、Windows コンピューターから ES-MultiNode テンプレートを簡単に使用できるように設計されています。 Windows 以外のコンピューターでもこのテンプレートを使用できますが、そのシナリオについては、この記事では説明しません。

1. これは既にインストールしていない場合、は、インストール [**Azure PowerSell モジュール**](http://go.microsoft.com/fwlink/p/?linkid=320376)します。 メッセージが表示されたら、[実行] をクリックしてインストールします。
>[AZURE.NOTE] Azure PowerShell には、Azure PowerShell 1.0 リリースでの大きな変更は中です。 現在、CreateElasticSearchCluster は、Azure PowerShell 0.9.8 で動作するように設計されており、Azure PowerShell 1.0 プレビューをサポートしていません。 今後、Azure PowerShell 1.0 互換のスクリプトが提供される予定です。

2.  **Openssl** のディストリビューションにツールが含まれています [**Git for Windows**](http://www.git-scm.com/downloads)します。 ない場合は、インストール [Git for Windows](http://www.git-scm.com/downloads) ようになりました (既定のインストール オプションは、[ok] をされます)。

3. Git がインストール済みで、システム パスに含まれていない場合は、Microsoft Azure PowerShell ウィンドウを開き、次のコマンドを実行します。

    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```

    置き換える、 `<Git installation folder>` 既定では、コンピューターに Git の場所で *"C:\Program Files\Git"*します。 最初のパスの先頭にセミコロン (;) がある点に注意してください。

4. Azure にログオンしていることを確認 (を介して [*Add-azureaccount*](https://msdn.microsoft.com/library/azure/dn790372.aspx) コマンドレット) と、ElasticSearch クラスターの作成に使用するサブスクリプションを選択した ([*Select-azuresubscription*](https://msdn.microsoft.com/library/azure/dn790367.aspx))。

5. まだの場合は、現在のディレクトリを ES-MultiNode フォルダーに変更します。

### CreateElasticSearchCluster スクリプトの実行
スクリプトを実行する前に、`azuredeploy-parameters.json` ファイルを開き、スクリプト パラメーターの値を確認するか指定します。 次のパラメーターがあります。

|パラメーター名           |説明|
|-----------------------  |--------------------------|
|dnsNameForLoadBalancerIP |ElasticSearch クラスターのパブリックに参照できる DNS 名を作成するために使用される名前です (指定した名前に Azure リージョン ドメインを付加して作成します)。 たとえば、このパラメーター値が "myBigCluster" で選択した Azure リージョンが米国西部の場合、クラスターの結果の DNS 名は、myBigCluster.westus.cloudapp.azure.com になります。 <br /><br />この名前は、データ ノードの名前など、ElasticSearch クラスターに関連付けられている多くの成果物の名前のルートとしても使用されます。|
|storageAccountPrefix    |ElasticSearch クラスター用に作成されるストレージ アカウントのプレフィックス。 <br /><br /> テンプレートの現在のバージョンが 1 つの共有記憶域アカウントを使用しますが、今後変更される可能性があります。|
|adminUsername           |ElasticSearch クラスターを管理する管理者アカウントの名前です (対応する SSH キーが自動的に生成されます)。|
|dataNodeCount           |ElasticSearch クラスター内のノード数。 最新バージョンのスクリプトは、データとクエリ ノードを区別していません。すべてのノードが両方の役割を担います。|
|dataDiskSize            |各データ ノードに割り当てられるデータ ディスクのサイズ (GB 単位)。 各ノードは、ElasticSearch サービス専用の 4 個のデータ ディスクを受け取ります。|
|region                  |ElasticSearch クラスターがある Azure リージョンの名前。|
|esClusterName           |ElasticSearch クラスターの内部名。 <br /><br />この値は、既定値は現在サポートされていません ES マルチノード テンプレートで同じ仮想ネットワークで 1 つ以上の ElasticSearch クラスターを実行する予定がない場合に変更する必要があります。|
|esUserName esPassword  |(HTTP 基本認証に従って) ES クラスターへのアクセス権を持つように構成されるユーザーの資格情報。|

これで、スクリプトを実行する準備が整いました。 次のコマンドを実行します。
```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name>
```
ここで `<es-group-name>` すべてのクラスター リソースを格納する Azure リソース グループの名前を指定します。

>[AZURE.NOTE] Azure へのログオンを忘れていたテスト AzureResourceGroup コマンドレットからは NullReferenceException を取得する場合 (`Add-AzureAccount`)。

スクリプトを実行してエラーが発生し、エラーの原因が誤ったテンプレート パラメーター値であると判断した場合は、パラメーター ファイルを修正し、別のリソース グループ名でスクリプトを再実行します。 `-RemoveExistingResourceGroup`パラメーターをスクリプトの呼び出しに追加すると、同じリソース グループ名を再利用して、古いリソース グループを消去することもできます。

### CreateElasticSearchCluster スクリプトの実行結果
`CreateElasticSearchCluster`スクリプトを実行すると、次のメイン アーティファクトが作成されます。 わかりやすくするために、`dnsNameForLoadBalancerIP` パラメーターの値に 'myBigCluster' を使用し、クラスターを作成したリージョンが米国西部だとします。

|アーティファクト|名前、場所、備考|
|----------------------------------|----------------------------------|
|リモート管理用の SSH キー |myBigCluster.key ファイル (CreateElasticSearchCluster が実行されたディレクトリ内)。 <br /><br />これは、クラスター内のデータ ノードを管理ノードと (管理ノード) を介して接続するために使用するキーです。|
|管理者ノード                        |myBigCluster admin.westus.cloudapp.azure.com <br /><br />これは、専用の VM リモート ElasticSearch クラスター管理では、外部の SSH 接続を許可する 1 つだけです。 すべての ElasticSearch クラスター ノードと同じ仮想ネットワークで実行されますが、ElasticSearch サービスは実行されません。|
|データ ノード                        |myBigCluster1. myBigCluster*N* <br /><br />ElasticSearch および Kibana サービスを実行しているデータ ノードです。 SSH で各ノードに接続できますが、管理者ノードを経由する必要があります。|
|ElasticSearch クラスター             |http://myBigCluster.westus.cloudapp.azure.com/es/ <br /><br />ElasticSearch クラスター (/es サフィックスに注意してください) のプライマリ エンドポイントは、上記のです。 基本 HTTP 認証で保護されます (資格情報は、ES-MultiNode テンプレートの esUserName/esPassword パラメーターで指定されました)。 クラスターもインストールされているヘッド プラグイン (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) の基本的なクラスター管理します。|
|Kibana サービス                    |http://myBigCluster.westus.cloudapp.azure.com <br /><br />Kibana サービスが作成された ElasticSearch クラスターからデータを表示する設定します。同じによって保護されている認証の資格情報をクラスター自体です。|

## プロセス内とプロセス外のトレース キャプチャ
「はじめに」では、診断データを収集する基本的な方法として、プロセス内とプロセス外の方法について触れました。 それぞれに長所と短所があります。

利点、 **プロセスでトレースをキャプチャ** が含まれます。

1. *構成とデプロイメントが容易*

    * 診断データ コレクションの構成は、アプリケーション構成のごく一部に過ぎないので、アプリケーションのその他の部分と常に "同期を保つ" ことは簡単です。

    * アプリケーションごと、またはサービスごとの構成を簡単に実現できます。

    * 通常、プロセス外のトレース キャプチャの場合、診断エージェントの別のデプロイメントと構成が必要です。そのため、管理者の作業とエラーの原因が増えます。 多くの場合、特定のエージェント テクノロジは、1 つの仮想マシン (ノード) につき 1 インスタンスのエージェントのみを許容しています。つまり、診断構成のコレクションの構成は、そのノードで実行されているすべてのアプリケーションとサービスで共有されます。

2. *柔軟性*

    * 対象データ ストレージ システムをサポートするクライアント ライブラリがある場合、必要なときにいつでもアプリケーションからデータを送信できます。 必要に応じて、新しいシンクを追加できます。

    * 複雑なキャプチャ、フィルター処理、データ集計ルールを実装できます。

    * 多くの場合、プロセス外のトレース キャプチャは、エージェントがサポートするデータ シンクによって制限されます。 一部のエージェントは拡張可能です。

3. *内部アプリケーション データおよびコンテキストへのアクセス*

    * アプリケーション/サービス プロセス内で実行される診断サブシステムは、コンテキスト情報を使用してトレースを簡単に強化できます。

    * プロセス外のアプローチでは、Event Tracing for Windows (ETW) などの何らかのプロセス内通信メカニズムを使用してデータをエージェントに送信する必要があります。 その結果、制限が増える可能性があります。

利点、 **プロセス外のトレースをキャプチャ** が含まれます。

1. *アプリケーションを監視し、クラッシュ ダンプを収集できる*

    * アプリケーションの起動に失敗するか、アプリケーションがクラッシュした場合、プロセス内のトレース キャプチャに成功しないことがあります。 独立したエージェントは、重要なトラブルシューティング情報をキャプチャするのに消費される可能性を持ちます。<br /><br />

2. *成熟度、堅牢性、実証済みのパフォーマンス*

    * プラットフォーム ベンダーが開発したエージェント (Microsoft Azure Diagnostics エージェントなど) は厳格なテストを受け、実際の環境で鍛えられています。

    * プロセス内のトレース キャプチャでは、アプリケーション プロセスから診断データを送信する処理がアプリケーションのメイン タスクを邪魔したり、タイミングやパフォーマンスの問題を起こしたりしないように気を付ける必要があります。 独立して実行されるエージェントは、このような問題を起こす可能性が低いので、通常は特にシステムへの影響を軽減するために設計されます。

当然ながら、両方のアプローチを組み合わせて活用することもできます。実際、それが多くのアプリケーションに最適なソリューションです。

この記事では、使用、 **Microsoft.Diagnostic.Listeners ライブラリ** と ElasticSearch クラスターへの Service Fabric アプリケーションからデータを送信するキャプチャ プロセスでトレースします。

## Listeners ライブラリを使用して診断データを ElasticSearch を送信する
Microsoft.Diagnostic.Listeners ライブラリは、Party Cluster サンプル Fabric アプリケーションの一部です。 使用方法:

1. ダウンロード [パーティ製クラスター サンプル](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) GitHub からです。

2. Party Cluster サンプル ディレクトリの Microsoft.Diagnostics.Listeners および Microsoft.Diagnostics.Listeners.Fabric プロジェクト (フォルダー全体) を、ElasticSearch にデータを送信する予定のアプリケーションのソリューション フォルダーにコピーします。

3. 対象のソリューションを開き、ソリューション エクスプローラーでソリューション ノードを右クリックし、[既存プロジェクトの追加] を選択します。 Microsoft.Diagnostics.Listeners プロジェクトをソリューションに追加します。 Microsoft.Diagnostics.Listeners.Fabric プロジェクトでも同じ手順を繰り返します。

4. サービス プロジェクトから 2 つの追加したプロジェクトに対して、プロジェクト参照を追加します (ElasticSearch にデータを送信する予定の各サービスは、Microsoft.Diagnostics.EventListeners と Microsoft.Diagnostics.EventListeners.Fabric を参照する必要があります)。

    ![Microsoft.Diagnostics.EventListeners および Microsoft.Diagnostics.EventListeners.Fabric ライブラリのプロジェクト参照][1]

### Service Fabric と Microsoft.Diagnostics.Tracing NuGet パッケージの 2015 年 11 月プレビュー
Service Fabric のターゲットの 2015 年 11 月プレビューで構築されたアプリケーション **.NET Framework 4.5.1** これはプレビュー リリースの時点で、Azure でサポートされている .NET Framework の最新のバージョンであるためです。 残念ながら、このバージョンの .NET Framework には、Microsoft.Diagnostics.Listeners ライブラリに必要な EventListener API の一部が含まれていません。 Microsoft.Diagnostics.Listeners ライブラリを使用するすべてのプロジェクトがいずれかのによって提供される EventSource の代替実装を使用する必要があります EventSource (Fabric アプリケーションで Api のログ記録の基になるコンポーネント) と EventListener 密結合であるため **Microsoft.Diagnostics.Tracing NuGet パッケージ**, 、Microsoft によって作成されます。 このパッケージは、この .NET Framework に含まれる EventSource と完全に下位互換性があります。そのため、参照する名前空間を変更する以外にコードの変更は必要ありません。

EventSource クラスの Microsoft.Diagnostics.Tracing 実装を使用する場合、データを ElasticSearch に送信する必要があるサービス プロジェクトごとに、次の手順を実行します。

1. サービス プロジェクトを右クリックし、[NuGet パッケージの管理] を選択します

2. (まだ選択していない場合は) nuget.org package ソースに切り替え、"Microsoft.Diagnostics.Tracing" を検索します

3. `Microsoft.Diagnostics.Tracing.EventSource`パッケージ (とその依存関係) をインストールします

4. サービス プロジェクトで ServiceEventSource.cs または ActorEventSource.cs ファイルを開き、ファイルの先頭にある `using System.Diagnostics.Tracing` ディレクティブを `using Microsoft.Diagnostics.Tracing` ディレクティブに置き換えます。

次の手順は必要ありません回 **.NET Framework 4.6** は Microsoft Azure でサポートされています。

### ElasticSearch リスナーのインストールと構成
診断データを ElasticSearch に送信するために必要な最後の手順は、`ElasticSearchListener` のインスタンスを作成し、ElasticSearch 接続データで構成することです。 リスナーは、サービス プロジェクトに定義されている EventSource クラスで発生したすべてのイベントを自動的にキャプチャします。 サービスの有効期間中はリスナーが有効である必要があるので、サービス初期化コードで作成することをお勧めします。 ステートレス サービスの初期化コードに必要な変更を加えた後の例を次に示します (追加部分のコメントには、先頭に `****` が付いています)。

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    public class Program
    {
        public static void Main(string[] args)
        {
            try
            {
                using (FabricRuntime fabricRuntime = FabricRuntime.Create())
                {

                    // **** Instantiate ElasticSearchListener
                    var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                    ElasticSearchListener esListener = null;
                    if (configProvider.HasConfiguration)
                    {
                        esListener = new ElasticSearchListener(configProvider);
                    }

                    // This is the name of the ServiceType that is registered with FabricRuntime.
                    // This name must match the name defined in the ServiceManifest. If you change
                    // this name, please change the name of the ServiceType in the ServiceManifest.
                    fabricRuntime.RegisterServiceType("Stateless1Type", typeof(Stateless1));

                    ServiceEventSource.Current.ServiceTypeRegistered(
                        Process.GetCurrentProcess().Id,
                        typeof(Stateless1).Name);

                    Thread.Sleep(Timeout.Infinite);

                    // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                    GC.KeepAlive(esListener);

                }
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

ElasticSearch 接続データは、サービス構成ファイル (PackageRoot\Config\Settings.xml) の別のセクションに指定する必要があります。 セクション名は、`FabricConfigurationProvider` コンストラクターに渡される値に対応する必要があります。次に例を示します。

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
値 `serviceUri`, 、`userName` と `password` ElasticSearch クラスター エンドポイント アドレス、ElasticSearch ユーザー名とパスワードに、それぞれ対応します。 `indexNamePrefix` ElasticSearch のインデックスをプレフィックスします。Microsoft.Diagnostics.Listeners ライブラリでは、日常的にそのデータに対する新しいインデックスを作成します。

### 確認
以上で完了です。 これで、サービスを実行するたびに、構成に指定した ElasticSearch サービスにトレースが送信されるようになります。 これを確認するには開始 Kibana UI (この例では、ページのアドレスになり、http://myBigCluster.westus.cloudapp.azure.com/) でターゲット ElasticSearch インスタンスに関連付けられているチェック、そのインデックス用の選択した名前のプレフィックスを持つ、 `ElasticSearchListener` インスタンスが、実際に作成され、データに指定されています。

![PartyCluster アプリケーション イベントを表示する Kibana][2]

## 次のステップ
- [Service Fabric サービスの診断と監視](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png


