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

アーティクル内のタスク移動する前に、必ず [ランタイム、SDK、およびツールをインストール](service-fabric-get-started.md), もインストールされますが、 **ServiceFabric** と **ServiceFabricTestability** PowerShell モジュールです。 [を有効にする PowerShell スクリプトの実行](service-fabric-get-started.md#enable-powershell-script-execution) と [をインストールして、ローカル クラスターを起動](service-fabric-get-started.md#install-and-start-a-local-cluster) 、記事の例を実行できるようにします。

この例は、記事を使用して、 [* * * * WordCount サンプル アプリケーション](http://aka.ms/servicefabricsamples) (作業の開始のサンプルにあります)。 サンプル アプリケーションのダウンロードとビルド

この記事で、PowerShell コマンドを実行する前にまず、ローカル Service Fabric クラスターを使用して接続 [* * の接続-* * ServiceFabricCluster](https://msdn.microsoft.com/library/azure/mt125938.aspx):

```powershell
Connect-ServiceFabricCluster localhost:19000
```

## タスク: Service Fabric アプリケーションのデプロイ

アプリケーションをビルドし、アプリケーションの種類をパッケージ化したら、ローカル Service Fabric クラスターにアプリケーションをデプロイできます。 最初に、Visual Studio で WordCount アプリケーションをパッケージ化します。ソリューション エクスプローラーで **WordCount** を右クリックし、**[パッケージ]** を選択します。 参照してください [アプリケーション Service Fabric のモデル化](service-fabric-application-model.md) については、サービス、アプリケーション マニフェストとパッケージのレイアウト。 デプロイには、アプリケーション パッケージのアップロード、アプリケーションの種類の登録、アプリケーション インスタンスの作成が含まれます。 クラスターに新しいアプリケーションをデプロイするには、このセクションの手順を使用します。

### 手順 1: アプリケーション パッケージのアップロード

アプリケーション パッケージをイメージ ストアにアップロードすると、そのパッケージは内部 Service Fabric コンポーネントがアクセスできる場所に置かれます。 アプリケーション パッケージには、必要なアプリケーション マニフェスト、サービス マニフェスト、およびアプリケーションとサービス インスタンスを作成するコード、構成、データのパッケージが含まれています。  [* * コピー-* * ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/azure/mt125905.aspx) コマンドでパッケージをアップロードします。 次に例を示します。

```powershell
Copy-ServiceFabricApplicationPackage C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### 手順 2: アプリケーションの種類の登録

アプリケーション パッケージを登録すると、アプリケーション マニフェストで宣言されたアプリケーションの種類とバージョンを利用できるようになります。 システムは、前の手順でアップロード パッケージを読み取り、パッケージを確認 (実行に相当 [* * テスト-ServiceFabricApplicationPackage * *](https://msdn.microsoft.com/library/azure/mt125950.aspx) ローカル)、パッケージのコンテンツおよび処理済みのパッケージを内部システムの場所にコピーします。 実行、 [* * 登録-ServiceFabricApplicationType * *](https://msdn.microsoft.com/library/azure/mt125958.aspx) コマンドレット。

```powershell
Register-ServiceFabricApplicationType WordCount
```
クラスターに登録されているアプリケーションの種類を表示するには、次のコマンドレットを実行します。

```powershell
Get-ServiceFabricApplicationType
```

### 手順 3: アプリケーション インスタンスの作成

使用して正常に登録されているアプリケーションの種類のバージョンを使用して、アプリケーションをインスタンス化することができます、 [* * New-* * ServiceFabricApplication](https://msdn.microsoft.com/library/azure/mt125913.aspx) コマンドです。 各アプリケーションの名前は、**fabric:** スキームで開始され、各アプリケーション インスタンスに対して一意でなければなりません。 アプリケーションの種類の名前とアプリケーションの種類のバージョンは、**ApplicationManifest.xml** ファイルで宣言されます。 ターゲット アプリケーションの種類のアプリケーション マニフェストで既定のサービスが定義されている場合、それらのサービスも同時に作成されます。

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

[* * Get-ServiceFabricApplication * *](https://msdn.microsoft.com/library/azure/mt163515.aspx) コマンドが正常に作成された、その全体的な状態と共にすべてのアプリケーション インスタンスを一覧表示します。  [* * Get-ServiceFabricService * *](https://msdn.microsoft.com/library/azure/mt125889.aspx) コマンドが特定のアプリケーション インスタンス内で正常に作成されたサービス インスタンスのすべてを一覧表示します。 既定のサービス (指定されている場合) が表示されます。

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## タスク: Service Fabric アプリケーションのアップグレード

以前にデプロイされた Service Fabric アプリケーションをアップグレードすることができます。 このタスクは、「タスク: Service Fabric アプリケーションのデプロイ」でデプロイされた WordCount アプリケーションをアップグレードします。 目を通して [Service Fabric アプリケーションのアップグレード](service-fabric-application-upgrade.md) の詳細。

### 手順 1: アプリケーションの更新

WordCount サービス内のコードに変更を加えます。

サービスのコードを更新した後、**ServiceManifest.xml** ファイル (WordCount プロジェクトの **PackageRoot** ディレクトリにあります) で、サービスのバージョン番号をインクリメントする必要があります。 マニフェストの **CodePackage** 要素を検索し、サービスのバージョンを 2.0.0 に変更します。 ServiceManifest.xml ファイル内の対応する行は、次のようになります。

```xml
<ServiceManifest Name="WordCountServicePkg" Version="2.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <StatefulServiceType ServiceTypeName="WordCountServiceType" HasPersistedState="true" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="2.0.0">
      ...
```

次に、ApplicationManifest.xml ファイル (WordCount ソリューション下の WordCount アプリケーション プロジェクトにあります) を更新する必要があります。 **ServiceManifestRef** 要素を更新して、バージョン 2.0.0.0 の **WordCountServicePkg** プロジェクトを使用します。 **ApplicationTypeVersion** も 2.0.0.0 から 1.0.0.0 に更新します。 ApplicationManifest.xml の対応する行は次のようになります。

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="WordCount" ApplicationTypeVersion="2.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
...
<ServiceManifestRef ServiceManifestName="WordCountServicePkg" ServiceManifestVersion="2.0.0" />
```

これらの変更を加えた後は、ファイルを保存し、WordCount プロジェクトをリビルドします。 WordCount プロジェクトを右クリックして、**[パッケージ]** を選択し、更新されたアプリケーションをパッケージ化します。 デプロイ可能なアプリケーション パッケージが作成されます。 これで、更新されたアプリケーションをデプロイする準備は完了です。

### 手順 2: 更新されたアプリケーション パッケージのコピーと登録

アプリケーションはビルドおよびパッケージ化され、アップグレードの準備は完了しています。 管理者と型を PowerShell ウィンドウを開くかどうか [* * Get-ServiceFabricApplication * *](https://msdn.microsoft.com/library/azure/mt163515.aspx), 、ワード カウント アプリケーションの種類のバージョン 1.0.0 が展開されているはずです。 WordCount サンプルの場合、アプリケーション パッケージは次の場所にあります: *C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug*

次に、更新されたアプリケーション パッケージを Service Fabric イメージ ストア (アプリケーション パッケージが Service Fabric により格納される場所) にコピーします。 **ApplicationPackagePathInImageStore** パラメーターから、Service Fabric にアプリケーション パッケージが見つかる場所が通知されます。 次のコマンドは、アプリケーション パッケージを、イメージ ストアの **WordCountV2** にコピーします。

```powershell
Copy-ServiceFabricApplicationPackage C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2
```

次の手順が、アプリケーションの新しいバージョンを使用して実行可能な Service Fabric に登録するには、 [* * 登録-ServiceFabricApplicationType * *](https://msdn.microsoft.com/library/azure/mt125958.aspx) コマンドレット。

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

このコマンドが成功しなかった場合は、手順 1 で説明したように、サービスをリビルドする必要があります。

### 手順 3: アップグレードの開始

アプリケーションのアップグレードには、さまざまなアップグレード パラメーター、タイムアウト、および正常性条件を適用できます。 目を通して、 [アプリケーション アップグレード パラメーター](service-fabric-application-upgrade-parameters.md) と [アップグレード プロセス](service-fabric-application-upgrade.md) 詳細ドキュメントです。 このチュートリアルでは、サービスの正常性評価の条件は、既定値 (と推奨値) のままにします。 すべてのサービスとインスタンスは、アップグレード後に_正常_である必要があります。 ただし、**HealthCheckStableDuration** を 60 秒間に増やす必要があります (そうすると、次のアップグレード ドメインにアップグレードが進む前に、少なくとも 20 秒間、サービスが正常な状態ができます)。 また、**UpgradeDomainTimeout** を 1200 秒に、**UpgradeTimeout** を 3000 秒に設定します。 最後に、**UpgradeFailureAction** を **[ロールバック]** に設定します。この設定を行うと、アップグレード中に問題が発生した場合、Service Fabric はアプリケーションを以前のバージョンにロールバックするよう要求します。

使用してアプリケーションのアップグレードを開始できます、 [* * スタート-* * ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/azure/mt125975.aspx) コマンドレット。

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

アプリケーション名は、以前にデプロイされた v1.0.0 のアプリケーション名 (fabric:/WordCount) と同じであることに注意してください。 Service Fabric はこの名前を使用して、アップグレードが実行されるアプリケーションを識別します。 タイムアウトを短く設定しすぎている場合、問題を示すタイムアウトのエラー メッセージが表示されることがあります。 参照してください [アプリケーションのアップグレードのトラブルシューティングを行う](service-fabric-application-upgrade-troubleshooting.md), 、かタイムアウトの設定値を大ききます。

使用して、アプリケーション アップグレードの進行状況を監視する [Service Fabric エクスプ ローラー](service-fabric-visualizing-your-cluster.md), 、またはを使用して、 [* * Get-ServiceFabricApplicationUpgrade * *](https://msdn.microsoft.com/library/azure/mt125988.aspx) コマンドレット。

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

少し後に、 [Get-servicefabricapplicationupgrade](https://msdn.microsoft.com/library/azure/mt125988.aspx) コマンドレットはすべてのアップグレード ドメインがアップグレードされたことを表示する必要があります ("完了") です。

## タスク: Service Fabric アプリケーションのテスト

高品質なサービスのコードを記述するには、開発者は信頼性の低いインフラストラクチャの障害を誘発してサービスの安定性をテストできる必要があります。 Service Fabric を使用すると、開発者は、混乱とフェールオーバーのテスト シナリオを使用して、障害アクションを誘発し、障害のある状態でサービスをテストできます。 目を通して [Testability の概要](service-fabric-testability-overview.md) の詳細。

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

アプリケーションのインスタンスが必要なくなったときに完全に削除できますを使用して、 [* * 削除-* * ServiceFabricApplication](https://msdn.microsoft.com/library/azure/mt125914.aspx) コマンドレットです。 また、このコマンドレットでは、アプリケーションに属するすべてのサービスも自動的に削除され、すべてのサービス状態が完全に削除されます。 この操作を元に戻すことはできず、アプリケーションの状態を復元することはできません。

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### 手順 2: アプリケーションの種類の登録解除

アプリケーションの種類の特定のバージョンが不要になったときの登録を解除を使用して、 [* * 登録解除-* * ServiceFabricApplicationType](https://msdn.microsoft.com/library/azure/mt125885.aspx) コマンドレットです。 使用していない種類の登録を解除すると、イメージ ストアのアプリケーション パッケージによって使用されるストレージ領域を解放します。 あるアプリケーションの種類でインスタンス化されたアプリケーションがない場合、またはそれを参照している保留中のアプリケーションのアップグレードがない場合、そのアプリケーションの種類を登録解除できます。

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### 手順 3: アプリケーション パッケージの削除

使用して、イメージ ストアから、アプリケーション パッケージを削除できるアプリケーションの種類の登録を解除した後、 [* * 削除-* * ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/azure/mt163532.aspx) コマンドレットです。

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## 次のステップ

[Service Fabric アプリケーションのライフ サイクル](service-fabric-application-lifecycle.md)

[アプリケーションを配置します。](service-fabric-deploy-remove-applications.md)

[アプリケーションのアップグレード](service-fabric-application-upgrade.md)

[Azure Service Fabric のコマンドレット](https://msdn.microsoft.com/library/azure/mt125965.aspx)

[Azure Service Fabric の testability コマンドレット](https://msdn.microsoft.com/library/azure/mt125844.aspx)





