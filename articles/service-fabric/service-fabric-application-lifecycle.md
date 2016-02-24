<properties
   pageTitle="Service Fabric のアプリケーション ライフサイクル | Microsoft Azure"
   description="Service Fabric アプリケーションの開発、デプロイ、テスト、アップグレード、保守、および削除について説明します。"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>


<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/15/2015"
   ms.author="ryanwi; mani-ramaswamy"/>


# Service Fabric アプリケーションのライフサイクル
通常は、その他のプラットフォームと同様に、Azure Service Fabric のアプリケーションは、デザイン、開発、テスト、デプロイ、アップグレード、保守、削除のフェーズを通過します。 Service Fabric は、開発からデプロイ、日常的な管理、保守、最終的な使用停止に至るまで、クラウド アプリケーションの完全なアプリケーション ライフサイクルに対して高度なサポートを提供します。 そのサービス モデルにより、アプリケーションのライフサイクルで個別に関与するさまざまな役割が有効になります。 この記事では、API の概要と、Service Fabric アプリケーション ライフサイクルのフェーズでさまざまなロールがその API をどのように使用するかを示します。

## サービス モデルのロール
サービス モデルのロールは、次のとおりです。

- **サービスの開発者**: 再利用して同じ種類または異なる種類の複数のアプリケーションで使用できるモジュールと汎用サービスを開発します。 たとえば、チケット発行のアプリケーション (ヘルプ デスク) または電子商取引アプリケーション (ショッピング カート) を作成するために Queue サービスを使用できます。

- **アプリケーション開発者**: 特定の要件またはシナリオを満たすためにサービスのコレクションを統合してアプリケーションを作成します。 たとえば、電子商取引 Web サイトには、"JSON ステートレス フロントエンド サービス"、"オークション ステートフル サービス"、"ステートフル キュー サービス" を統合して、オークション ソリューションが構築されることがあります。

- **アプリケーション管理者**: アプリケーション構成 (テンプレートの構成パラメーターの入力)、デプロイメント (使用可能なリソースへのマッピング)、およびサービスの品質上の決定を行います。 たとえば、アプリケーション管理者は、アプリケーションの言語ロケール (米国には英語、日本には日本語) を決定します。 別のデプロイされたアプリケーションには、さまざまな設定を含めることができます。

- **演算子**: アプリケーションの構成に基づくアプリケーションと、アプリケーション管理者が指定されている要件をデプロイします。 たとえば、オペレーターはアプリケーションをプロビジョニングしてデプロイし、Azure で実行されていることを確認します。 オペレーターは、アプリケーションの正常性とパフォーマンスの情報を監視し、必要に応じて、物理インフラストラクチャを管理します。


## 開発
1. A *サービスの開発者* さまざまな種類を使用してサービスの開発、 [高信頼アクター](service-fabric-reliable-actors-introduction.md) または [信頼性の高いサービス](../service-fabric/service-fabric-reliable-services-introduction.md) プログラミング モデルです。
2. A *サービスの開発者* 1 つまたは複数のコード、構成、およびデータ パッケージで構成されるサービス マニフェスト ファイルで開発したサービス型を宣言して記述します。
3.  *アプリケーション開発者* 異なる種類のサービスを使用してアプリケーションをビルドします。
4.  *アプリケーション開発者* 構成サービスのサービス マニフェストを参照し、適切に上書きして、構成サービスの構成とデプロイメントの設定を別のパラメーター化を宣言して、アプリケーション マニフェストでアプリケーションの種類を記述します。

参照してください [高信頼アクターを使ってみる](service-fabric-reliable-actors-get-started.md) と [信頼性の高いサービスを使ってみる](service-fabric-reliable-services-quick-start.md) 例についてです。

## デプロイ
1.  *アプリケーション管理者* に特定のアプリケーションの適切なパラメーターを指定して、Service Fabric クラスターに展開するアプリケーションの種類には、 **ApplicationType** アプリケーション マニフェスト内の要素。

2.  *演算子* を使用して、クラスターのイメージ ストアにアプリケーション パッケージをアップロード、 [**CopyApplicationPackage** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) または [**Copy-servicefabricapplicationpackage** コマンドレット](https://msdn.microsoft.com/library/azure/mt125905.aspx)します。 アプリケーション パッケージには、アプリケーション マニフェストとサービス パッケージのコレクションが含まれています。 Service Fabric は、Azure Blob ストアまたは Service Fabric のシステム サービスが対象となる、イメージ ストアに格納されているアプリケーション パッケージからアプリケーションをデプロイします。

3.  *演算子* からアップロードされたアプリケーション パッケージを使用して、対象のクラスターでアプリケーションの種類をプロビジョニング、 [**ProvisionApplicationAsync** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), 、  [**Register-servicefabricapplicationtype** コマンドレット](https://msdn.microsoft.com/library/azure/mt125958.aspx), 、または [**作成アプリケーション** REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

3. アプリケーションをプロビジョニングした後、 *演算子* によって指定されたパラメーターでアプリケーションを起動、 *アプリケーション管理者* を使用して、 [**CreateApplicationAsync** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync.aspx), 、 [**New-servicefabricapplication** コマンドレット](https://msdn.microsoft.com/library/azure/mt125913.aspx), 、または [**アプリケーションの作成** REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

4. アプリケーションを展開後、 *演算子* を使用して、 [**CreateServiceAsync** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.createserviceasync.aspx), 、 [**New-servicefabricservice** コマンドレット](https://msdn.microsoft.com/library/azure/mt125874.aspx), 、または [**アプリケーションの作成** REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx) 利用可能なサービスの種類に基づいて、アプリケーションの新しいサービス インスタンスを作成します。

5. これで、Service Fabric のクラスターで、アプリケーションが実行されます。

参照してください [アプリケーションを配置](service-fabric-deploy-remove-applications.md) 例です。

## テスト
1. ローカル開発クラスターまたはテスト クラスターにデプロイした後、 *サービスの開発者* を使用して、組み込みのフェールオーバー テスト シナリオを実行、 [**FailoverTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenarioparameters.aspx) と [**FailoverTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenario.aspx) クラス、または [**Invoke-servicefabricfailovertestscenario** コマンドレット](https://msdn.microsoft.com/library/azure/mt125935.aspx)します。 フェールオーバー テスト シナリオで、重要な切り替えとフェールオーバーを通して指定したサービスを実行して、引き続き利用可能で動作していることを確認します。

2.  *サービスの開発者* 組み込みの chaos テスト シナリオを使用して、実行し、 [**ChaosTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenarioparameters.aspx) と [**ChaosTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenario.aspx) クラス、または [**Invoke-servicefabricchaostestscenario** コマンドレット](https://msdn.microsoft.com/library/azure/mt126036.aspx)します。 chaos テスト シナリオは、複数のノード、コード パッケージ、レプリカのエラーをクラスターにランダムに誘導します。

参照してください [Testability シナリオ](service-fabric-testability-scenarios.md) 例です。

## アップグレード
1. A *サービスの開発者* 構成サービスをインスタンス化されたアプリケーションの更新やバグを修正し、サービス マニフェストの新しいバージョンを提供します。

2.  *アプリケーション開発者* をオーバーライドして、一貫性のあるサービスの構成と展開の設定をパラメーター化し、アプリケーション マニフェストの新しいバージョンを提供します。 アプリケーション開発者は、アプリケーションに新しいバージョンのサービス マニフェストを組み込み、更新されたアプリケーション パッケージで新しいバージョンのアプリケーションの種類を提供します。

3.  *アプリケーション管理者* 、適切なパラメーターを更新することで、ターゲット アプリケーションにアプリケーションの種類の新しいバージョンが組み込まれています。

4.  *演算子* 、イメージ ストアを使用してクラスターに更新されたアプリケーション パッケージをアップロード、 [**CopyApplicationPackage** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) または [**Copy-servicefabricapplicationpackage** コマンドレット](https://msdn.microsoft.com/library/azure/mt125905.aspx)します。 アプリケーション パッケージには、アプリケーション マニフェストとサービス パッケージのコレクションが含まれています。

5.  *演算子* を使用してターゲット クラスター内のアプリケーションの新しいバージョンをプロビジョニング、 [**ProvisionApplicationAsync** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), 、 [**Register-servicefabricapplicationtype** コマンドレット](https://msdn.microsoft.com/library/azure/mt125958.aspx), 、または [**アプリケーションのプロビジョニング** REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

6.  *演算子* 対象のアプリケーションを使用して新しいバージョンにアップグレード、 [**UpgradeApplicationAsync** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.upgradeapplicationasync.aspx), 、 [**Start-servicefabricapplicationupgrade** コマンドレット](https://msdn.microsoft.com/library/azure/mt125975.aspx), 、または [**アプリケーションごとの種類もアップグレード** REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

7.  *演算子* を使用してアップグレードの進行状況を確認、 [**GetApplicationUpgradeProgressAsync** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.getapplicationupgradeprogressasync.aspx), 、 [**Get-servicefabricapplicationupgrade** コマンドレット](https://msdn.microsoft.com/library/azure/mt125988.aspx), 、または [**アプリケーション アップグレードの進行状況表示** REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

8. 必要に応じて、 *演算子* が変更され、現在のアプリケーションのパラメーターを再適用を使用したアップグレード、 [**UpdateApplicationUpgradeAsync** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.updateapplicationupgradeasync.aspx), 、 [**Update-servicefabricapplicationupgrade** コマンドレット](https://msdn.microsoft.com/library/azure/mt126030.aspx), 、または [**Update Application Upgrade** REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

9. 必要に応じて、 *演算子* 、現在のアプリケーションのロールバックを使用したアップグレード、 [**RollbackApplicationUpgradeAsync** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.rollbackapplicationupgradeasync.aspx), 、 [**Start-servicefabricapplicationrollback** コマンドレット](https://msdn.microsoft.com/library/azure/mt125833.aspx), 、または [**Rollback Application Upgrade** REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

10. Service Fabric はその構成サービスのいずれかの可用性を失うことなく、クラスター内で実行されている対象アプリケーションをアップグレードします。

参照してください、 [アプリケーション アップグレードのチュートリアル](service-fabric-application-upgrade-tutorial.md) 例です。

## 管理
1. オペレーティング システムのアップグレードと修正プログラムに対しては、Service Fabric はクラスター内で実行しているすべてのアプリケーションの可用性を保証する Azure インフラストラクチャとつなぐインターフェイスです。

2. Service Fabric プラットフォームへのアップグレードと修正プログラムに対しては、クラスターで実行されているアプリケーションの可用性が失われることなく、Service Fabric 自体でアップグレードされます。

3.  *アプリケーション管理者* 追加または過去の容量使用率のデータと予測される将来の要求を分析した後、クラスター ノードの削除を承認します。

4.  *演算子* を追加し指定したノードを削除、 *アプリケーション管理者*します。

5. 新しいノードをクラスターに追加する、または既存のノードをクラスターから削除するとき、Service Fabric は最適なパフォーマンスを実現するために、クラスター内のすべてのノード間でアプリケーションを実行している負荷を自動的に分散します。

## 削除
1.  *演算子* を使用して、アプリケーション全体を削除せず、クラスター内の実行中のサービスの特定のインスタンスを削除することができます、 [**DeleteServiceAsync** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync.aspx), 、 [**Remove-servicefabricservice** コマンドレット](https://msdn.microsoft.com/library/azure/mt126033.aspx), 、または [**サービスの削除** REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。  

2.  *演算子* アプリケーション インスタンスとそのすべてのサービスを使用してを削除することも、 [**DeleteApplicationAsync** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync.aspx), 、 [**Remove-servicefabricapplication** コマンドレット](https://msdn.microsoft.com/library/azure/mt125914.aspx), 、または [**Delete Application** REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

3. アプリケーションとサービスが停止されると、 *演算子* アプリケーションの種類を使用して、準備を解除できる、 [**UnprovisionApplicationAsync** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync.aspx), 、  [**Unregister-servicefabricapplicationtype** コマンドレット](https://msdn.microsoft.com/library/azure/mt125885.aspx), 、または [**アプリケーションをプロビジョニング解除** REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。 プロビジョニングを解除されたアプリケーションの種類の場合、ImageStore からアプリケーション パッケージが削除されません。 アプリケーション パッケージを手動で削除する必要があります。

4.  *演算子* を使用して、ImageStore からアプリケーション パッケージを削除、 [**RemoveApplicationPackage** メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage.aspx) または [**Remove-servicefabricapplicationpackage** コマンドレット](https://msdn.microsoft.com/library/azure/mt163532.aspx)します。

参照してください [アプリケーションを配置](service-fabric-deploy-remove-applications.md) 例です。

## 次のステップ

Service Fabric アプリケーションとサービスの開発、テスト、管理に関する詳細については、以下を参照してください。

- [Reliable Actor](service-fabric-reliable-actors-introduction.md)
- [Reliable Service](../service-fabric/service-fabric-reliable-services-introduction.md)
- [アプリケーションをデプロイする](service-fabric-deploy-remove-applications.md)
- [アプリケーションのアップグレード](service-fabric-application-upgrade.md)
- [Testability の概要](service-fabric-testability-overview.md)
- [REST ベース アプリケーション ライフサイクルのサンプル](service-fabric-rest-based-application-lifecycle-sample.md)

