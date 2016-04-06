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

最も頻繁に使用され、推奨されているアップグレード方法は、監視付きローリング アップグレードです。  アップグレードされているアプリケーションの正常性は、正常性ポリシーのセットに基づいて、Service Fabric により監視されます。 アップグレード ドメイン (UD) 内のアプリケーションをアップグレードすると、Service Fabric は、アプリケーションの正常性を評価し、正常性ポリシーに基づいて、次のアップグレード ドメインに進むかアップグレードが失敗かを決定します。 監視付きアプリケーション アップグレードは、マネージ API またはネイティブ API、PowerShell、および REST を使用して実行できます。 Visual Studio を使用してアップグレードを実行する手順については、次を参照してください [アップグレード Visual Studio を使用してアプリケーションを](service-fabric-application-upgrade-tutorial.md)します。

Service Fabric による監視付きローリング アップグレードを使用することにより、アプリケーション管理者は、アプリケーションの正常性を判断するために Service Fabric が使用する正常性評価ポリシーを構成できます。 さらに、管理者は、自動ロールバックなどで正常性評価が失敗した場合に実行されるアクションを構成することもできます。 このセクションでは、PowerShell を使用した SDK サンプルの 1 つの監視付きアップグレードについて説明します。

## 手順 1: ビジュアル オブジェクト サンプルのビルドとデプロイ

追加を github からアプリケーションをダウンロードしてこれらの手順を実行する、 **webgl utils.js** と **gl マトリックス-min.js** プロジェクトへのファイルのサンプルの readme ファイルで説明したようです。 そうしないと、アプリケーションは動作しません。 これらをプロジェクトに追加すると、ビルドし、アプリケーション プロジェクトを右クリックして、アプリケーションを発行 **VisualObjectsApplication** し Service Fabric のメニュー項目で、[発行] コマンドを選択すると、次のようにします。  参照してください [Service Fabric アプリケーションのアップグレード チュートリアル](service-fabric-application-tutorial.md) の詳細。  代わりに、PowerShell を使用して、アプリケーションをデプロイすることができます。

> [AZURE.NOTE] まず、クラスターに接続を使用して 1 つが PowerShell で Service Fabric のコマンドを使用することがあります、 `Connect-ServiceFabricCluster` コマンドレットです。 また、クラスターがローカル コンピューターにセットアップ済みになっている必要があります。 記事を参照してください [Service Fabric 開発環境を設定する](service-fabric-get-started.md)です。

Visual Studio でプロジェクトをビルドしてから、PowerShell コマンドを使用いずれかが **Copy-servicefabricapplicationpackage** を使用して Service Fabric ランタイムにアプリケーションを登録することによって後に、アプリケーション パッケージを ImageStore にコピーする、 **Register-servicefabricapplicationpackage** コマンドレットを使用して、アプリケーションのインスタンスを最後に開始、 **New-servicefabricapplication** コマンドレットです。  これら 3 つの手順は、Visual Studio で [デプロイ] メニュー項目を使用することに似ています。

使用するようになりましたが、 [クラスターおよびアプリケーションを表示する Service Fabric エクスプ ローラー](service-fabric-visualizing-your-cluster.md)します。 アプリケーションにもナビゲートできますに Internet Explorer」と入力して、web サービスの持つ [http://localhost:8081/visualobjects](http://localhost:8081/visualobjects) アドレス バーにします。  画面上を動くフローティング ビジュアル オブジェクトが表示されます。  さらに、使用する **Get-servicefabricapplication** アプリケーション状態を確認します。

## 手順 2: ビジュアル オブジェクト サンプルの更新

手順 1. でデプロイされたバージョンでは、ビジュアル オブジェクトが回転しないことにお気付きになったことでしょう。 アプリケーションをアップグレードして、ビジュアル オブジェクトも回転させてみましょう。

VisualObjects ソリューション内の VisualObjects.ActorService プロジェクトを選択し、StatefulVisualObjectActor.cs ファイルを開きます。 そのファイル内で `MoveObject` メソッドに移動し、`this.State.Move()` をコメント アウトし、`this.State.Move(true)` をコメント解除します。 この変更によって、サービスのアップグレード後、オブジェクトは回転するようになります。

更新する必要もあります、 *ServiceManifest.xml* (PackageRoot の下)、プロジェクトのファイル **VisualObjects.ActorService**します。 更新プログラム、 *CodePackage* およびサービスのバージョン 2.0、および対応する行を *ServiceManifest.xml* ファイルです。
Visual Studio を使用する *マニフェスト ファイルの編集* オプション、マニフェスト ファイルの変更、確認を行うためのソリューションを右クリックした後に [Service Fabric アプリケーションのアップグレード チュートリアル](service-fabric-application-tutorial.md)します。


変更後、マニフェストは次のようになります (太字部分は変更個所を示します)。

```xml
<ServiceManifestName="VisualObjects.ActorService"Version="2.0"xmlns="http://schemas.microsoft.com/2011/01/fabric"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code"Version="2.0">
```

これで、更新する必要があります、 *ApplicationManifest.xml* ファイル (では、 **VisualObjects** プロジェクトの下、 **VisualObjects** ソリューション) のバージョン 2.0 を使用する、 **VisualObjects.ActorService** プロジェクトし、も 1.0.0.0 から 2.0.0.0 にアプリケーションのバージョンを更新します。 これで、対応する行、 *ApplicationManifest.xml* のように、次のようになります。

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"ApplicationTypeName="VisualObjects"ApplicationTypeVersion="2.0.0.0"xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService"ServiceManifestVersion="2.0" />
```


これをクリックして、プロジェクトをビルドだけ **ActorService** プロジェクト、右クリックして Visual Studio でビルドを選択すると (すべて再構築を選択した場合は、同様に、他のプロジェクトのバージョンを更新する必要があります、 *ServiceManifest.xml* し、[、 *ApplicationManifest.xml*, であるため、コードを変更するよう)。 今すぐみましょう、更新されたアプリケーションを右クリックしてパッケージ化する *VisualObjectsApplication*, 、Service Fabric のメニューを選択し、パッケージを選択します。 デプロイ可能なアプリケーション パッケージが作成されます。  これで、更新されたアプリケーションをデプロイする準備は完了です。


## 手順 3: 正常性ポリシーを決定し、アップグレード パラメーター

精通、 [アプリケーション アップグレード パラメーター](service-fabric-application-upgrade-parameters.md) と [アップグレード プロセス](service-fabric-application-upgrade.md) タイムアウトと正常性の条件を適用するのにはさまざまなアップグレード パラメーターを十分に理解、します。 このチュートリアルでは、私たちは、既定にするには、サービス正常性評価条件をそのまま残る (と推奨値) するすべてのサービスとインスタンスにする必要があります _正常_ 、アップグレード後にします。  ただし増やしましょう、 *HealthCheckStableDuration* を 60 秒 (そう、サービスは正常な状態は次のアップグレード ドメインにアップグレードを続行する前に、少なくとも 20 秒間)。  設定しても、 *UpgradeDomainTimeout* を 1200 秒、および *UpgradeTimeout* を 3000 秒です。 最後に、設定しましょう、 *UpgradeFailureAction* ロールバックによりロールバックするよう要求 Service Fabric アプリケーションを以前のバージョンをアップグレード中に問題が発生した場合。 これで、アップグレードを開始 (ステップ 4) する時点で指定されているアップグレード パラメーターは、次のようになります。

FailureAction = Rollback

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000


## 手順 4: アプリケーションのアップグレードの準備

アプリケーションはビルドされ、アップグレードの準備は完了しています。 管理者と型を PowerShell ウィンドウを開くかどうか **Get-servicefabricapplication**, 、アプリケーションの種類 1.0.0.0 であることができるようにする必要があります **VisualObjects** されてに展開されています。  Service Fabric SDK - を圧縮解除した次の相対パス、アプリケーション パッケージが格納されている *samples \services\stateful\visualobjects\visualobjects\obj\x64\debug*します。 そのディレクトリ内に "パッケージ" フォルダーが見つかるはずです。このフォルダーにアプリケーション パッケージが格納されます。 タイムスタンプを確認して、最新のビルドであることを確認します (場合によってはパスを適切に変更する必要もあります)。

次に、更新されたアプリケーション パッケージを Service Fabric ImageStore (アプリケーション パッケージが Service Fabric により保存される場所) にコピーします。 パラメーター *ApplicationPackagePathInImageStore* Service Fabric アプリケーションのパッケージが見つかることを通知します。 (パスを適切に変更する必要があります)、次のコマンドと、"VisualObjects\_V2"に、更新されたアプリケーションを配置おができます。

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


アプリケーション名がで説明した方法に注意してください、 *ApplicationManifest.xml* ファイルです。 Service Fabric はこの名前を使用して、アップグレードが実行されるアプリケーションを識別します。 タイムアウトを短く設定しすぎている場合、問題を示すエラー メッセージが表示されることがあります。 トラブルシューティングのセクションを参照するか、タイムアウトの時間を長くします。

これで、アプリケーションのアップグレードが続行するか監視できます Service Fabric エクスプ ローラーを使用して次の PowerShell コマンドを使用します。 **Get-servicefabricapplicationupgrade fabric:/VisualObjects**します。

数分後に、上記の PowerShell コマンドを使用して、すべてのアップグレード ドメインがアップグレードされた (完了) 状態であることが表示されます。 ブラウザー ウィンドウのビジュアル オブジェクトが回転しているはずです。

練習のために、バージョンをバージョン 2 からバージョン 3 へ、あるいは、バージョン 2 からバージョン 1 に変更および移行してみることもできます (v2 から v1 へのアップグレードも可能です)。 タイムアウトと正常性ポリシーをいろいろ試して、これらに精通してください。 Azure クラスターにデプロイする場合、ローカル クラスターにデプロイするときに使用するパラメーターと使用されるパラメーターが異なる場合があります。タイムアウトを控えめに設定することをお勧めします。


## 次のステップ

[アップグレード、アプリケーションを使用して Visual Studio](service-fabric-application-upgrade-tutorial.md) Visual Studio を使用してアプリケーションのアップグレードについて説明します。

使用して、アプリケーションのアップグレードを制御 [アップグレード パラメーター](service-fabric-application-upgrade-parameters.md)します。

使用する方法を習得し、アプリケーションのアップグレードの互換性を確保 [データのシリアル化](service-fabric-application-upgrade-data-serialization.md)します。

参照することで、アプリケーションをアップグレードするときに高度な機能を使用する方法について [高度なトピック](service-fabric-application-upgrade-advanced.md)します。

」の手順を参照してアプリケーションのアップグレードの一般的な問題が解決 [アプリケーションのアップグレードのトラブルシューティング ](service-fabric-application-upgrade-troubleshooting.md)します。



