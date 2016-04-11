<properties
    pageTitle="PowerShell を使用した Service Fabric アプリケーション管理の自動化 | Microsoft Azure"
    description="PowerShell を使用して、Service Fabric アプリケーションをデプロイ、アップグレード、テスト、および削除します。"
    services="service-fabric"
    documentationCenter=".net"
    authors="rwike77"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-fabric"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="ryanwi"/>

# PowerShell を使用した Service Fabric アプリケーションのデプロイ、アップグレード、テスト、および削除

この記事では、PowerShell を使用して、Azure Service Fabric アプリケーションのデプロイ、アップグレード、削除、およびテストの一般的なタスクを自動化する方法を説明します。  

## 前提条件

アーティクル内のタスク移動する前に、必ず [ランタイム、SDK、およびツールをインストール](service-fabric-get-started.md), もインストールされますが、 **ServiceFabric** と **ServiceFabricTestability** PowerShell モジュールです。 [PowerShell スクリプトの実行を有効にする](service-fabric-get-started.md#enable-powershell-script-execution) と [をインストールして、ローカル クラスターを起動](service-fabric-get-started.md#install-and-start-a-local-cluster) 、記事の例を実行できるようにします。

この例は、記事を使用して、 [**WordCount** サンプル アプリケーション](http://aka.ms/servicefabricsamples) (作業の開始のサンプルにあります)。 サンプル アプリケーションのダウンロードとビルド

この記事で、PowerShell コマンドを実行する前にまず、ローカル Service Fabric クラスターを使用して接続 [**Connect-servicefabriccluster**](https://msdn.microsoft.com/library/azure/mt125938.aspx):

```powershell
Connect-ServiceFabricCluster localhost:19000
```

## タスク: Service Fabric アプリケーションのデプロイ

アプリケーションをビルドし、アプリケーションの種類をパッケージ化したら、ローカル Service Fabric クラスターにアプリケーションをデプロイできます。 まず、Visual Studio でのワード カウント アプリケーションを右クリックしてパッケージを作成 **WordCount** を選択して、ソリューション エクスプ ローラーで **パッケージ**します。  参照してください [アプリケーション Service Fabric のモデル化](service-fabric-application-model.md) については、サービス、アプリケーション マニフェストとパッケージのレイアウト。  デプロイには、アプリケーション パッケージのアップロード、アプリケーションの種類の登録、アプリケーション インスタンスの作成が含まれます。 クラスターに新しいアプリケーションをデプロイするには、このセクションの手順を使用します。

### 手順 1: アプリケーション パッケージのアップロード
アプリケーション パッケージをイメージ ストアにアップロードすると、そのパッケージは内部 Service Fabric コンポーネントがアクセスできる場所に置かれます。  アプリケーション パッケージには、必要なアプリケーション マニフェスト、サービス マニフェスト、およびアプリケーションとサービス インスタンスを作成するコード、構成、データのパッケージが含まれています。   [**Copy-servicefabricapplicationpackage**](https://msdn.microsoft.com/library/azure/mt125905.aspx) コマンドでパッケージをアップロードします。 次に例を示します。

```powershell
Copy-ServiceFabricApplicationPackage C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### 手順 2: アプリケーションの種類の登録
アプリケーション パッケージを登録すると、アプリケーション マニフェストで宣言されたアプリケーションの種類とバージョンを利用できるようになります。 システムは、前の手順でアップロード パッケージを読み取り、パッケージを確認 (実行に相当 [**Test-servicefabricapplicationpackage**](https://msdn.microsoft.com/library/azure/mt125950.aspx) ローカル)、パッケージのコンテンツおよび処理済みのパッケージを内部システムの場所にコピーします。  実行、 [**Register-servicefabricapplicationtype**](https://msdn.microsoft.com/library/azure/mt125958.aspx) コマンドレット。

```powershell
Register-ServiceFabricApplicationType WordCount
```
クラスターに登録されているアプリケーションの種類を表示するには、次のコマンドレットを実行します。

```powershell
Get-ServiceFabricApplicationType
```

### 手順 3: アプリケーション インスタンスの作成
使用して正常に登録されているアプリケーションの種類のバージョンを使用して、アプリケーションをインスタンス化することができます、 [**New-servicefabricapplication**](https://msdn.microsoft.com/library/azure/mt125913.aspx) コマンドです。 各アプリケーションの名前で始まらなければなりません、 **ファブリック:** スキームし、各アプリケーション インスタンスに対して一意であります。 アプリケーションの種類名とアプリケーションの種類のバージョンがで宣言されている、 **ApplicationManifest.xml** ファイルです。 ターゲット アプリケーションの種類のアプリケーション マニフェストで既定のサービスが定義されている場合、それらのサービスも同時に作成されます。

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

 [**Get-servicefabricapplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx) コマンドが正常に作成された、その全体的な状態と共にすべてのアプリケーション インスタンスを一覧表示します。  [**Get-servicefabricservice**](https://msdn.microsoft.com/library/azure/mt125889.aspx) コマンドが特定のアプリケーション インスタンス内で正常に作成されたサービス インスタンスのすべてを一覧表示します。 既定のサービス (指定されている場合) が表示されます。

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## タスク: Service Fabric アプリケーションのアップグレード

以前にデプロイされた Service Fabric アプリケーションをアップグレードすることができます。 このタスクは、「タスク: Service Fabric アプリケーションのデプロイ」でデプロイされた WordCount アプリケーションをアップグレードします。 目を通して [Service Fabric アプリケーションのアップグレード](service-fabric-application-upgrade.md) の詳細。

### 手順 1: アプリケーションの更新

WordCount サービス内のコードに変更を加えます。

サービス コードを更新するには後、は、サービスのバージョン番号をインクリメントする必要があります、 **ServiceManifest.xml** ファイル (内にある、 **PackageRoot** ワード カウント プロジェクトのディレクトリ)。 検索、 **CodePackage** 2.0.0 するサービスのバージョンの変更、マニフェストの要素。  ServiceManifest.xml ファイル内の対応する行は、次のようになります。

```xml
<ServiceManifest Name="WordCountServicePkg" Version="2.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <StatefulServiceType ServiceTypeName="WordCountServiceType" HasPersistedState="true" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="2.0.0">
      ...
```

次に、ApplicationManifest.xml ファイル (WordCount ソリューション下の WordCount アプリケーション プロジェクトにあります) を更新する必要があります。  更新プログラム、 **ServiceManifestRef** のバージョン 2.0.0.0 を使用する要素、 **WordCountServicePkg** プロジェクトです。 更新も、 **ApplicationTypeVersion** 1.0.0.0 から 2.0.0.0 にします。 ApplicationManifest.xml の対応する行は次のようになります。

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="WordCount" ApplicationTypeVersion="2.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
...
<ServiceManifestRef ServiceManifestName="WordCountServicePkg" ServiceManifestVersion="2.0.0" />
```

これらの変更を加えた後は、ファイルを保存し、WordCount プロジェクトをリビルドします。 ワード カウント プロジェクトを右クリックして、更新されたアプリケーションを今すぐパッケージ **パッケージ**します。 デプロイ可能なアプリケーション パッケージが作成されます。 これで、更新されたアプリケーションをデプロイする準備は完了です。

### 手順 2: 更新されたアプリケーション パッケージのコピーと登録

アプリケーションはビルドおよびパッケージ化され、アップグレードの準備は完了しています。 管理者と型を PowerShell ウィンドウを開くかどうか [**Get-servicefabricapplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx), 、ワード カウント アプリケーションの種類のバージョン 1.0.0 が展開されているはずです。  ワード カウント サンプルでは、アプリケーション パッケージが見つかりました: *C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug*します。

次に、更新されたアプリケーション パッケージを Service Fabric イメージ ストア (アプリケーション パッケージが Service Fabric により格納される場所) にコピーします。 パラメーター **ApplicationPackagePathInImageStore** Service Fabric アプリケーションのパッケージが見つかることを通知します。 次のコマンドはアプリケーション パッケージをコピー **WordCountV2** イメージ ストアで。  

```powershell
Copy-ServiceFabricApplicationPackage C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2
```

次の手順が、アプリケーションの新しいバージョンを使用して実行可能な Service Fabric に登録するには、 [**Register-servicefabricapplicationtype**](https://msdn.microsoft.com/library/azure/mt125958.aspx) コマンドレット。

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

このコマンドが成功しなかった場合は、手順 1 で説明したように、サービスをリビルドする必要があります。

### 手順 3: アップグレードの開始
アプリケーションのアップグレードには、さまざまなアップグレード パラメーター、タイムアウト、および正常性条件を適用できます。 目を通して、 [アプリケーション アップグレード パラメーター](service-fabric-application-upgrade-parameters.md) と [アップグレード プロセス](service-fabric-application-upgrade.md) 詳細ドキュメントです。 このチュートリアルでは、サービスの正常性評価の条件は、既定値 (と推奨値) のままにします。 すべてのサービスとインスタンスにする必要があります _正常_ 、アップグレード後にします。  ただし、増加しないように、 **HealthCheckStableDuration** を 60 秒 (そう、サービスは正常な状態は次のアップグレード ドメインにアップグレードを続行する前に、少なくとも 20 秒間)。  設定も、 **UpgradeDomainTimeout** を 1200 秒に、 **UpgradeTimeout** を 3000 秒にします。 最後に、設定、 **UpgradeFailureAction** に **ロールバック**, 、Service Fabric ロールバックする以前のバージョンのアプリケーションのアップグレード中にエラーが発生した場合どの要求します。

使用してアプリケーションのアップグレードを開始できます、 [**Start-servicefabricapplicationupgrade**](https://msdn.microsoft.com/library/azure/mt125975.aspx) コマンドレット。

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

アプリケーション名は、以前にデプロイされた v1.0.0 のアプリケーション名 (fabric:/WordCount) と同じであることに注意してください。 Service Fabric はこの名前を使用して、アップグレードが実行されるアプリケーションを識別します。 タイムアウトを短く設定しすぎている場合、問題を示すタイムアウトのエラー メッセージが表示されることがあります。 参照してください [アプリケーションのアップグレードのトラブルシューティングを行う](service-fabric-application-upgrade-troubleshooting.md), 、かタイムアウトの設定値を大ききます。

使用して、アプリケーション アップグレードの進行状況を監視する [Service Fabric エクスプ ローラー](service-fabric-visualizing-your-cluster.md), 、またはを使用して、 [**Get-servicefabricapplicationupgrade**](https://msdn.microsoft.com/library/azure/mt125988.aspx) コマンドレット。

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

少し後に、 [Get-servicefabricapplicationupgrade](https://msdn.microsoft.com/library/azure/mt125988.aspx) コマンドレットはすべてのアップグレード ドメインがアップグレードされたことを表示する必要があります ("完了") です。

## タスク: Service Fabric アプリケーションのテスト

高品質なサービスのコードを記述するには、開発者は信頼性の低いインフラストラクチャの障害を誘発してサービスの安定性をテストできる必要があります。 Service Fabric を使用すると、開発者は、混乱とフェールオーバーのテスト シナリオを使用して、障害アクションを誘発し、障害のある状態でサービスをテストできます。  目を通して [Testability の概要](service-fabric-testability-overview.md) の詳細。

### 手順 1: 混乱テスト シナリオの実行
混乱のテスト シナリオでは、Service Fabric クラスター全体にわたる障害を生成します。 このシナリオには、一般的に数か月間または数年間から数時間で発生するエラーが圧縮されています。 障害率の高い交互に配置された障害の組み合わせにより、通常は見過ごされるめったに発生しないケースが検出されます。 次の例では、混乱テスト シナリオを60 分間実行します。

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### 手順 2: フェールオーバー テスト シナリオの実行
フェールオーバー テスト シナリオは、クラスター全体ではなく、特定のサービス パーティションを対象とします。また、影響を受けていないその他のサービスは残します。 このシナリオでは、ビジネス ロジックを実行しながら、サービス検証で一連のシミュレートされた障害を反復処理します。 サービス検証の失敗は、さらに調査の必要な問題があることを示します。 フェールオーバー テストは、一度に 1 つの障害のみを誘発します。これに対し、混乱のテストでは可能性のある複数の障害を発生させることができます。 次の例では、fabric:/WordCount/WordCountService サービスに対して、60 秒間フェールオーバー テストを実行します。

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Connect-ServiceFabricCluster

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## タスク: Service Fabric アプリケーションの削除
デプロイされたアプリケーション インスタンスの削除や、クラスターからのプロビジョニングされているアプリケーションの種類の削除、および ImageStore からのアプリケーション パッケージの削除を実行することができます。

### 手順 1: アプリケーション インスタンスの削除
アプリケーションのインスタンスが必要なくなったときに完全に削除できますを使用して、 [**Remove-servicefabricapplication**](https://msdn.microsoft.com/library/azure/mt125914.aspx) コマンドレットです。 また、このコマンドレットでは、アプリケーションに属するすべてのサービスも自動的に削除され、すべてのサービス状態が完全に削除されます。 この操作を元に戻すことはできず、アプリケーションの状態を復元することはできません。

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### 手順 2: アプリケーションの種類の登録解除
アプリケーションの種類の特定のバージョンが不要になったときの登録を解除を使用して、 [**Unregister-servicefabricapplicationtype**](https://msdn.microsoft.com/library/azure/mt125885.aspx) コマンドレットです。 使用していない種類の登録を解除すると、イメージ ストアのアプリケーション パッケージによって使用されるストレージ領域を解放します。 あるアプリケーションの種類でインスタンス化されたアプリケーションがない場合、またはそれを参照している保留中のアプリケーションのアップグレードがない場合、そのアプリケーションの種類を登録解除できます。

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### 手順 3: アプリケーション パッケージの削除
使用して、イメージ ストアから、アプリケーション パッケージを削除できるアプリケーションの種類の登録を解除した後、 [**Remove-servicefabricapplicationpackage**](https://msdn.microsoft.com/library/azure/mt163532.aspx) コマンドレットです。

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## 次のステップ
[Service Fabric アプリケーションのライフサイクル](service-fabric-application-lifecycle.md)

[アプリケーションをデプロイする](service-fabric-deploy-remove-applications.md)

[アプリケーションのアップグレード](service-fabric-application-upgrade.md)

[Azure Service Fabric コマンドレット](https://msdn.microsoft.com/library/azure/mt125965.aspx)

[Azure Service Fabric Testability コマンドレット](https://msdn.microsoft.com/library/azure/mt125844.aspx)

