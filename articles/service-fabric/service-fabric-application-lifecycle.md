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

通常は、その他のプラットフォームと同様に、Azure Service Fabric のアプリケーションは、デザイン、開発、テスト、デプロイメント、アップグレード、保守、削除のフェーズを通過します。 Service Fabric は、開発からデプロイメント、日常的な管理、保守、最終的な使用停止に至るまで、クラウド アプリケーションの完全なアプリケーション ライフサイクルに対して高度なサポートを提供します。 そのサービス モデルにより、アプリケーションのライフサイクルで個別に関与するさまざまな役割が有効になります。 この記事では、API の概要と、Service Fabric アプリケーション ライフサイクルのフェーズでさまざまなロールがその API をどのように使用するかを示します。

## サービス モデルのロール

サービス モデルのロールは、次のとおりです。

- **サービス開発者**: 同じ種類や別の種類の複数のアプリケーションで再利用、使用できるモジュールと汎用サービスを開発します。 たとえば、チケット発行のアプリケーション (ヘルプ デスク) または電子商取引アプリケーション (ショッピング カート) を作成するために Queue サービスを使用できます。

- **アプリケーション開発者**: 特定の要件またはシナリオを満たすために、サービスのコレクションを統合してアプリケーションを作成します。 たとえば、電子商取引 Web サイトには、"JSON ステートレス フロントエンド サービス"、"オークション ステートフル サービス"、"ステートフル キュー サービス" を統合して、オークション ソリューションが構築されることがあります。

- **アプリケーション管理者**: アプリケーションの構成 (テンプレートの構成パラメーターを入力)、デプロイメント (使用可能なリソースへのマッピング)、サービスの品質を決定します。 たとえば、アプリケーション管理者は、アプリケーションの言語ロケール (米国には英語、日本には日本語) を決定します。 別のデプロイされたアプリケーションには、さまざまな設定を含めることができます。

- **オペレーター**: アプリケーションの構成に基づくアプリケーションと、アプリケーション管理者が指定する要件をデプロイします。 たとえば、オペレーターはアプリケーションをプロビジョニングしてデプロイし、Azure で実行されていることを確認します。 オペレーターは、アプリケーションの正常性とパフォーマンスの情報を監視し、必要に応じて、物理インフラストラクチャを管理します。


## 開発

1. A *サービスの開発者* さまざまな種類を使用してサービスの開発、 [高信頼アクター](service-fabric-reliable-actors-introduction.md) または [信頼性の高いサービス](../service-fabric/service-fabric-reliable-services-introduction.md) プログラミング モデルです。
2. *サービス開発者* は、1 つ以上のコード、構成、データのパッケージで構成されるサービス マニフェスト ファイルで、開発したサービスの種類について宣言によって記述します。
3. *アプリケーション開発者*は、異なる種類のサービスを使用するアプリケーションを構築します。
4. *アプリケーション開発者*は、構成サービスのサービス マニフェストを参照し、異なる構成と構成サービスのデプロイメント設定を適切にオーバーライドしてパラメーター化し、アプリケーション マニフェストでアプリケーションの種類を宣言によって記述します。

参照してください [高信頼アクターを使ってみる](service-fabric-reliable-actors-get-started.md) と [信頼性の高いサービスを使ってみる](service-fabric-reliable-services-quick-start.md) 例についてです。

## デプロイ

1. *アプリケーション管理者*は、アプリケーション マニフェストで **ApplicationType** 要素を指定して、Service Fabric クラスターにデプロイされるように、特定のアプリケーションに合わせてアプリケーションの種類を調整します。

2. *演算子* を使用して、クラスターのイメージ ストアにアプリケーション パッケージをアップロード、 [* * * * CopyApplicationPackage メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) または [* * コピー-ServiceFabricApplicationPackage * * コマンドレット](https://msdn.microsoft.com/library/azure/mt125905.aspx)します。 アプリケーション パッケージには、アプリケーション マニフェストとサービス パッケージのコレクションが含まれています。 Service Fabric は、Azure Blob ストアまたは Service Fabric のシステム サービスが対象となる、イメージ ストアに格納されているアプリケーション パッケージからアプリケーションをデプロイします。

3. *演算子* からアップロードされたアプリケーション パッケージを使用して、対象のクラスターでアプリケーションの種類をプロビジョニング、 [* * * * ProvisionApplicationAsync メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), 、  [* * 登録-ServiceFabricApplicationType * * コマンドレット](https://msdn.microsoft.com/library/azure/mt125958.aspx), 、または [* * Create * アプリケーション * REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

3. アプリケーションをプロビジョニングした後、 *演算子* によって指定されたパラメーターでアプリケーションを起動、 *アプリケーション管理者* を使用して、 [* * * * CreateApplicationAsync メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync.aspx), 、 [* * 新規-ServiceFabricApplication * * コマンドレット](https://msdn.microsoft.com/library/azure/mt125913.aspx), 、または [* * Create * アプリケーション * REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

4. アプリケーションを展開すると後、 *演算子* を使用して、 [* * CreateServiceAsync * * メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.createserviceasync.aspx), 、 [* * 新規-ServiceFabricService * * コマンドレット](https://msdn.microsoft.com/library/azure/mt125874.aspx), 、または [* * Create * アプリケーション * REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx) 利用可能なサービスの種類に基づいて、アプリケーションの新しいサービス インスタンスを作成します。

5. これで、Service Fabric のクラスターで、アプリケーションが実行されます。

参照してください [アプリケーションを配置](service-fabric-deploy-remove-applications.md) 例です。

## テスト

1. ローカル開発クラスターまたはテスト クラスターにデプロイした後、 *サービスの開発者* を使用して、組み込みのフェールオーバー テスト シナリオを実行、 [* * FailoverTestScenarioParameters * *](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenarioparameters.aspx) と [* * FailoverTestScenario * *](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenario.aspx) クラス、または [* * Invoke-ServiceFabricFailoverTestScenario * * コマンドレット](https://msdn.microsoft.com/library/azure/mt125935.aspx)します。 フェールオーバー テスト シナリオで、重要な切り替えとフェールオーバーを通して指定したサービスを実行して、引き続き利用可能で動作していることを確認します。

2. *サービスの開発者* 組み込みの chaos テスト シナリオを使用して、実行し、 [* * ChaosTestScenarioParameters * *](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenarioparameters.aspx) と [* * ChaosTestScenario * *](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenario.aspx) クラス、または [* * Invoke-ServiceFabricChaosTestScenario * * コマンドレット](https://msdn.microsoft.com/library/azure/mt126036.aspx)します。 chaos テスト シナリオは、複数のノード、コード パッケージ、レプリカのエラーをクラスターにランダムに誘導します。

参照してください [Testability シナリオ](service-fabric-testability-scenarios.md) 例です。

## アップグレード

1. *サービス開発者*は、インスタンス化されたアプリケーションの構成サービスの更新、バグの修正を行い、サービス マニフェストの新しいバージョンを提供します。

2. *アプリケーション開発者*は、一貫性のあるサービスの構成とデプロイメント設定をオーバーライドしてパラメーター化し、新しいバージョンのアプリケーション マニフェストを提供します。 アプリケーション開発者は、アプリケーションに新しいバージョンのサービス マニフェストを組み込み、更新されたアプリケーション パッケージで新しいバージョンのアプリケーションの種類を提供します。

3. *アプリケーション管理者*は、適切なパラメーターを更新して、対象のアプリケーションに新しいバージョンのアプリケーションの種類を組み込みます。

4. *演算子* 、イメージ ストアを使用してクラスターに更新されたアプリケーション パッケージをアップロード、 [* * * * CopyApplicationPackage メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) または [* * コピー-ServiceFabricApplicationPackage * * コマンドレット](https://msdn.microsoft.com/library/azure/mt125905.aspx)します。 アプリケーション パッケージには、アプリケーション マニフェストとサービス パッケージのコレクションが含まれています。

5. *演算子* を使用してターゲット クラスター内のアプリケーションの新しいバージョンをプロビジョニング、 [* * * * ProvisionApplicationAsync メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), 、 [* * 登録-ServiceFabricApplicationType * * コマンドレット](https://msdn.microsoft.com/library/azure/mt125958.aspx), 、または [* * アプリケーション * * REST 操作をプロビジョニング](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

6. *演算子* 対象のアプリケーションを使用して新しいバージョンにアップグレード、 [* * * * UpgradeApplicationAsync メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.upgradeapplicationasync.aspx), 、 [* * 開始-ServiceFabricApplicationUpgrade * * コマンドレット](https://msdn.microsoft.com/library/azure/mt125975.aspx), 、または [* * アプリケーションごとの種類 * * REST 操作をアップグレード](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

7. *演算子* を使用してアップグレードの進行状況を確認、 [* * * * GetApplicationUpgradeProgressAsync メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.getapplicationupgradeprogressasync.aspx), 、 [* * 取得-ServiceFabricApplicationUpgrade * * コマンドレット](https://msdn.microsoft.com/library/azure/mt125988.aspx), 、または [* * Get Application Upgrade 進行中 * * REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

8. 必要に応じて、 *演算子* が変更され、現在のアプリケーションのパラメーターを再適用を使用したアップグレード、 [* * UpdateApplicationUpgradeAsync * * メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.updateapplicationupgradeasync.aspx), 、 [* * 更新-ServiceFabricApplicationUpgrade * * コマンドレット](https://msdn.microsoft.com/library/azure/mt126030.aspx), 、または [* * Update Application アップグレード * * REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

9. 必要に応じて、 *演算子* 、現在のアプリケーションのロールバックを使用したアップグレード、 [* * RollbackApplicationUpgradeAsync * * メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.rollbackapplicationupgradeasync.aspx), 、 [* * スタート-ServiceFabricApplicationRollback * * コマンドレット](https://msdn.microsoft.com/library/azure/mt125833.aspx), 、または [* * Rollback Application アップグレード * * REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

10. Service Fabric はその構成サービスのいずれかの可用性を失うことなく、クラスター内で実行されている対象アプリケーションをアップグレードします。

参照してください、 [アプリケーション アップグレードのチュートリアル](service-fabric-application-upgrade-tutorial.md) 例です。

## 管理

1. オペレーティング システムのアップグレードと修正プログラムに対しては、Service Fabric はクラスター内で実行しているすべてのアプリケーションの可用性を保証する Azure インフラストラクチャとつなぐインターフェイスです。

2. Service Fabric プラットフォームへのアップグレードと修正プログラムに対しては、クラスターで実行されているアプリケーションの可用性が失われることなく、Service Fabric 自体でアップグレードされます。

3. *アプリケーション管理者*は、過去の容量使用率のデータと予測される将来の要求を分析した後、ノードの追加やクラスターからのノードの削除を承認します。

4. *オペレーター*は、*アプリケーション管理者*が指定したノードを追加、削除します。

5. 新しいノードをクラスターに追加する、または既存のノードをクラスターから削除するとき、Service Fabric は最適なパフォーマンスを実現するために、クラスター内のすべてのノード間でアプリケーションを実行している負荷を自動的に分散します。

## 削除

1. *演算子* を使用して、アプリケーション全体を削除せず、クラスター内の実行中のサービスの特定のインスタンスを削除することができます、 [* * * * DeleteServiceAsync メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync.aspx), 、 [* * 削除-ServiceFabricService * * コマンドレット](https://msdn.microsoft.com/library/azure/mt126033.aspx), 、または [* * Delete * * Service REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

2. *演算子* アプリケーション インスタンスとそのすべてのサービスを使用してを削除することも、 [* * * * DeleteApplicationAsync メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync.aspx), 、 [* * 削除-ServiceFabricApplication * * コマンドレット](https://msdn.microsoft.com/library/azure/mt125914.aspx), 、または [* * Delete * * Application REST 操作](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。

3. アプリケーションとサービスが停止されると、 *演算子* アプリケーションの種類を使用して、プロビジョニング解除できます、 [* * * * UnprovisionApplicationAsync メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync.aspx), 、  [* * 登録解除-ServiceFabricApplicationType * * コマンドレット](https://msdn.microsoft.com/library/azure/mt125885.aspx), 、または [* * アプリケーション * * REST 操作の準備を解除](https://msdn.microsoft.com/library/azure/dn707692.aspx)します。 プロビジョニングを解除されたアプリケーションの種類の場合、ImageStore からアプリケーション パッケージが削除されません。 アプリケーション パッケージを手動で削除する必要があります。

4. *演算子* を使用して、ImageStore からアプリケーション パッケージを削除、 [* * * * RemoveApplicationPackage メソッド](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage.aspx) または [* * 削除-ServiceFabricApplicationPackage * * コマンドレット](https://msdn.microsoft.com/library/azure/mt163532.aspx)します。

参照してください [アプリケーションを配置](service-fabric-deploy-remove-applications.md) 例です。

## 次のステップ

Service Fabric アプリケーションとサービスの開発、テスト、管理に関する詳細については、以下を参照してください。

- [高信頼アクター](service-fabric-reliable-actors-introduction.md)
- [信頼性の高いサービス](../service-fabric/service-fabric-reliable-services-introduction.md)
- [アプリケーションを配置します。](service-fabric-deploy-remove-applications.md)
- [アプリケーションのアップグレード](service-fabric-application-upgrade.md)
- [Testability の概要](service-fabric-testability-overview.md)
- [REST ベースのアプリケーション ライフ サイクルのサンプル](service-fabric-rest-based-application-lifecycle-sample.md)





