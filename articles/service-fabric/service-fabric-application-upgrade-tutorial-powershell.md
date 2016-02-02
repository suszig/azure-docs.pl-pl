<properties
   pageTitle="PowerShell を使用した Service Fabric アプリケーションのアップグレード | Microsoft Azure"
   description="この記事では、PowerShell による Service Fabric アプリケーションのデプロイ、コードの変更、アップグレードのロールアウトを段階的に説明します。"
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
   ms.date="07/17/2015"
   ms.author="subramar"/>




# PowerShell を使用した Service Fabric アプリケーションのアップグレード

最も頻繁に使用され、推奨されているアップグレード方法は、監視付きローリング アップグレードです。 アップグレードされているアプリケーションの正常性は、正常性ポリシーのセットに基づいて、Service Fabric により監視されます。 アップグレード ドメイン (UD) 内のアプリケーションをアップグレードすると、Service Fabric は、アプリケーションの正常性を評価し、正常性ポリシーに基づいて、次のアップグレード ドメインに進むかアップグレードが失敗かを決定します。 監視付きアプリケーション アップグレードは、マネージ API またはネイティブ API、PowerShell、および REST を使用して実行できます。 Visual Studio を使用してアップグレードを実行する手順については、次を参照してください [アップグレード Visual Studio を使用してアプリケーションを](service-fabric-application-upgrade-tutorial.md)します。

Service Fabric による監視付きローリング アップグレードを使用することにより、アプリケーション管理者は、アプリケーションの正常性を判断するために Service Fabric が使用する正常性評価ポリシーを構成できます。 さらに、管理者は、自動ロールバックなどで正常性評価が失敗した場合に実行されるアクションを構成することもできます。 このセクションでは、PowerShell を使用した SDK サンプルの 1 つの監視付きアップグレードについて説明します。

## 手順 1: ビジュアル オブジェクト サンプルのビルドとデプロイ

この手順を行うには、Github からアプリケーションをダウンロードし、サンプルの readme ファイルで説明されているように、**webgl-utils.js** および **gl-matrix-min.js** ファイルをプロジェクトに追加します。 そうしないと、アプリケーションは動作しません。 これらのファイルをプロジェクトに追加した後、アプリケーション プロジェクト **[VisualObjectsApplication]** を右クリックし、次の図のように [Service Fabric] メニュー項目の発行コマンドを選択して、アプリケーションをビルドおよび発行します。 参照してください [Service Fabric アプリケーションのアップグレード チュートリアル](service-fabric-application-tutorial.md) の詳細。 代わりに、PowerShell を使用して、アプリケーションをデプロイすることができます。
> [AZURE.NOTE] まず、クラスターに接続を使用して 1 つが PowerShell で Service Fabric のコマンドを使用することがあります、 `Connect-servicefabriccluster` コマンドレットです。 また、クラスターがローカル コンピューターにセットアップ済みになっている必要があります。 記事を参照してください [Service Fabric 開発環境を設定する](service-fabric-get-started.md)します。

Visual Studio でプロジェクトをビルドした後、PowerShell コマンドの **Copy-ServiceFabricApplicationPackage** を使用してアプリケーション パッケージを ImageStore にコピーし、次に、**Register-ServiceFabricApplicationPackage** コマンドレットを使用して、アプリケーションを Service Fabric ランタイムに登録して、最後に **New-ServiceFabricApplication** コマンドレットを使用して、アプリケーションのインスタンスを開始できます。 これら 3 つの手順は、Visual Studio で [デプロイ] メニュー項目を使用することに似ています。

使用するようになりましたが、 [クラスターおよびアプリケーションを表示する Service Fabric エクスプ ローラー](service-fabric-visualizing-your-cluster.md)します。 アプリケーションにもナビゲートできますに Internet Explorer」と入力して、web サービスの持つ [場所/visualobjects](http://localhost:8081/visualobjects) アドレス バーにします。 画面上を動くフローティング ビジュアル オブジェクトが表示されます。 さらに、**Get-ServiceFabricApplication** を使用すると、アプリケーションの状態を確認できます。

## 手順 2: ビジュアル オブジェクト サンプルの更新

手順 1. でデプロイされたバージョンでは、ビジュアル オブジェクトが回転しないことにお気付きになったことでしょう。 アプリケーションをアップグレードして、ビジュアル オブジェクトも回転させてみましょう。

VisualObjects ソリューション内の VisualObjects.ActorService プロジェクトを選択し、StatefulVisualObjectActor.cs ファイルを開きます。 そのファイル内で、メソッドに移動 `MoveObject`, 、コメント アウト `これです。State.Move()` をコメント解除します `これです。State.Move(true)`します。 この変更によって、サービスのアップグレード後、オブジェクトは回転するようになります。

プロジェクト **VisualObjects.ActorService** の *ServiceManifest.xml* ファイル (PackageRoot の下) を更新する必要もあります。 *CodePackage* およびサービスのバージョンを 2.0 に更新し、*ServiceManifest.xml* ファイルの対応する行を更新します。
Visual Studio を使用する *マニフェスト ファイルの編集* オプション、マニフェスト ファイルの変更、確認を行うためのソリューションを右クリックした後に [Service Fabric アプリケーションのアップグレード チュートリアル](service-fabric-application-tutorial.md)します。


変更後、マニフェストは次のようになります (太字部分は変更個所を示します)。

```xml
<ServiceManifestName="VisualObjects.ActorService"Version="2.0"xmlns="http://schemas.microsoft.com/2011/01/fabric"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code"Version="2.0">
```

次に、*ApplicationManifest.xml* ファイル (**VisualObjects** ソリューションの **VisualObjects** プロジェクトの下に見つかります) を更新して、**VisualObjects.ActorService** プロジェクトのバージョン 2.0 が使用できるようにし、さらに、アプリケーションのバージョンも 1.0.0.0 から 2.0.0.0 に更新することが必要です。 これで、*ApplicationManifest.xml* ファイルの対応する行は以下のようになっているはずです。

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"ApplicationTypeName="VisualObjects"ApplicationTypeVersion="2.0.0.0"xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService"ServiceManifestVersion="2.0" />
```


次に、Visual Studio で **ActorService** プロジェクトを選択し、右クリックしてビルドを選択することによりプロジェクトをビルドします ([すべてリビルド] を選択した場合、コードが変更されることになる可能性があり、その場合は *ServiceManifest.xml* ファイルと *ApplicationManifest.xml* ファイルのその他のプロジェクトのバージョンも更新する必要があります)。 *VisualObjectsApplication* を右クリックし、[Service Fabric のメニュー]、[パッケージ] の順に選択して、更新されたアプリケーションをパッケージ化しましょう。 デプロイ可能なアプリケーション パッケージが作成されます。 これで、更新されたアプリケーションをデプロイする準備は完了です。


## 手順 3: 正常性ポリシーを決定し、アップグレード パラメーター

精通、 [アプリケーション アップグレード パラメーター](service-fabric-application-upgrade-parameters.md) と [アップグレード プロセス](service-fabric-application-upgrade.md) タイムアウトと正常性の条件を適用するのにはさまざまなアップグレード パラメーターを十分に理解、します。 このチュートリアルでは、サービスの正常性評価条件を既定 (推奨値) のままにします。これには、すべてのサービスとインスタンスが、アップグレード後_正常_である必要があります。 ただし、*HealthCheckStableDuration* を 60 秒間に増やしましょう (そうすると、次のアップグレード ドメインにアップグレードが進む前に、少なくとも 20 秒間、サービスが正常な状態ができます)。 また、*UpgradeDomainTimeout* を 1200 秒に、*UpgradeTimeout* を 3000 秒に設定しましょう。 最後に、*UpgradeFailureAction* を [ロールバック] に設定しましょう。これで、アップグレード中に問題が発生した場合、Service Fabric はアプリケーションを以前のバージョンにロールバックするよう要求されます。 これで、アップグレードを開始 (ステップ 4) する時点で指定されているアップグレード パラメーターは、次のようになります。

FailureAction = Rollback

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000


## 手順 4: アプリケーションのアップグレードの準備

アプリケーションはビルドされ、アップグレードの準備は完了しています。 PowerShell ウィンドウを管理者として開き、「**Get-ServiceFabricApplication**」と入力すると、デプロイされているのが **VisualObjects** のアプリケーションの種類 1.0.0.0 であることが表示されます。 アプリケーション パッケージは、Service Fabric SDK を圧縮解除した次の相対パスの下に格納されています - *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*。 そのディレクトリ内に "パッケージ" フォルダーが見つかるはずです。このフォルダーにアプリケーション パッケージが格納されます。 タイムスタンプを確認して、最新のビルドであることを確認します (場合によってはパスを適切に変更する必要もあります)。

次に、更新されたアプリケーション パッケージを Service Fabric ImageStore (アプリケーション パッケージが Service Fabric により保存される場所) にコピーします。 *ApplicationPackagePathInImageStore* パラメーターから、Service Fabric にアプリケーション パッケージが見つかる場所が通知されます。 (パスを適切に変更する必要があります)、次のコマンドと、"VisualObjects\_V2"に、更新されたアプリケーションを配置おができます。

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

次の手順では、Service Fabric を使用してこのアプリケーションを登録します。これには、次のコマンドを使用します。

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

上記のコマンドが成功しなかった場合、すべてのサービスのリビルドが必要な可能性があります。 手順 2 で説明したとおり、WebService のバージョンの更新が必要な場合もあります。

## 手順 5: アプリケーションのアップグレード開始

アプリケーションのアップグレードを開始する準備がすべて完了しました。開始するには次のコマンドを使用します。

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


アプリケーション名は *ApplicationManifest.xml* ファイルに記述されているとおりにするよう注意してください。 Service Fabric はこの名前を使用して、アップグレードが実行されるアプリケーションを識別します。 タイムアウトを短く設定しすぎている場合、問題を示すエラー メッセージが表示されることがあります。 トラブルシューティングのセクションを参照するか、タイムアウトの時間を長くします。

アプリケーションのアップグレード処理の進行状況は、Service Fabric エクスプローラーを使用するか、次の PowerShell コマンドを使用して監視できます: **Get-ServiceFabricApplicationUpgrade fabric:/VisualObjects**。

数分後に、上記の PowerShell コマンドを使用して、すべてのアップグレード ドメインがアップグレードされた (完了) 状態であることが表示されます。 ブラウザー ウィンドウのビジュアル オブジェクトが回転しているはずです。

練習のために、バージョンをバージョン 2 からバージョン 3 へ、あるいは、バージョン 2 からバージョン 1 に変更および移行してみることもできます (v2 から v1 へのアップグレードも可能です)。 タイムアウトと正常性ポリシーをいろいろ試して、これらに精通してください。 Azure クラスターにデプロイする場合、ローカル クラスターにデプロイするときに使用するパラメーターと使用されるパラメーターが異なる場合があります。タイムアウトを控えめに設定することをお勧めします。


## 次のステップ

[アップグレード アプリケーションを使用して Visual Studio](service-fabric-application-upgrade-tutorial.md) Visual Studio を使用してアプリケーションのアップグレードについて説明します。

使用して、アプリケーションのアップグレードを制御 [アップグレード パラメーター](service-fabric-application-upgrade-parameters.md)します。

使用する方法を習得し、アプリケーションのアップグレードの互換性を確保 [データのシリアル化](service-fabric-application-upgrade-data-serialization.md)します。

参照することで、アプリケーションをアップグレードするときに高度な機能を使用する方法について [高度なトピック](service-fabric-application-upgrade-advanced.md)します。

」の手順を参照してアプリケーションのアップグレードの一般的な問題が解決 [アプリケーションのアップグレードのトラブルシューティング ](service-fabric-application-upgrade-troubleshooting.md)します。






