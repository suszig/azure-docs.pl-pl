<properties
   pageTitle ="サービス ファブリック クラスターのセキュリティ: クライアントの役割 |Microsoft Azure]
   description =「この記事は、2 つのクライアントの役割と役割に提供されるアクセス許可について説明」します。 
   サービス ="service fabric"
   documentationCenter=".net"
   authors ="mani ramaswamy"
   manager ="coreysa"
   editor=""/>

<tags
   ms.service="service fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/13/2015"
   ms.author="subramar"/>



# ロール ベースのアクセス制御 (Service Fabric クライアント用)

Service Fabric では、Service Fabric クラスターに接続されるクライアントのために、管理者用とユーザー用の 2 つの異なるアクセス コントロールの種類がサポートされています。 アクセス制御を使用すると、クラスター管理者は、ユーザーのグループごとに特定のクラスター操作へのアクセスを制限して、クラスターのセキュリティを強化できます。 管理者は管理機能 (読み取り/書き込み機能を含む) へのフル アクセスを持ち、ユーザーは既定では管理機能への読み取りアクセス (たとえば、クエリなど) と、アプリケーションおよびサービスを解決する機能だけを持ちます。

2 つのクライアント ロール (管理者またはクライアント) は、クラスターの作成時に、それぞれに個別の証明書を設定することによって指定されます。 参照してください [サービス ファブリック クラスターのセキュリティ](service-fabric-cluster-security.md) 詳細については、セキュリティで保護された Service Fabric クラスターを設定する方法です。


## 既定のアクセス制御の設定

管理者のアクセス制御の種類は、すべての FabricClient API へのフル アクセスを持ちます。 Service Fabric クラスターに対して、次のような任意の読み取りおよび書き込み操作を実行できます。

### アプリケーションおよびサービスの操作

* **CreateService**: サービスの作成
* **CreateServiceFromTemplate**: テンプレートからのサービスの作成
* **UpdateService**: サービスの更新
* **DeleteService**: サービスの削除
* **ProvisionApplicationType**: アプリケーションの種類のプロビジョニング
* **CreateApplication**: アプリケーションの作成
* **DeleteApplication**: アプリケーションの削除
* **UpgradeApplication**: アプリケーションのアップグレードの開始または中断
* **UnprovisionApplicationType**: アプリケーションの種類のプロビジョニング解除
* **MoveNextUpgradeDomain**: 明示的なアップグレード ドメインでのアプリケーションのアップグレードの再開
* **ReportUpgradeHealth**: 現在のアップグレードの進行状況でのアプリケーションのアップグレードの再開
* **ReportHealth**: 正常性のレポート
* **PredeployPackageToNode**: デプロイメント前の API
* **CodePackageControl**: コード パッケージの再開
* **RecoverPartition**: パーティションの復旧
* **RecoverPartitions**: 複数のパーティションの復旧
* **RecoverServicePartitions**: サービス パーティションの復旧
* **RecoverSystemPartitions**: システム サービス パーティションの復旧


### クラスターの操作

* **ProvisionFabric**: MSI やクラスター マニフェストのプロビジョニング
* **UpgradeFabric**: クラスターのアップグレードの開始
* **UnprovisionFabric**: MSI やクラスター マニフェストのプロビジョニング解除
* **MoveNextFabricUpgradeDomain**: 明示的なアップグレード ドメインでのクラスターのアップグレードの再開
* **ReportFabricUpgradeHealth**: 現在のアップグレードの進行状況でのクラスターのアップグレードの再開
* **StartInfrastructureTask**: インフラストラクチャのタスクの開始
* **FinishInfrastructureTask** : インフラストラクチャのタスクの終了
* **InvokeInfrastructureCommand**: インフラストラクチャのタスクの管理コマンド
* **ActivateNode**: ノードのアクティブ化
* **DeactivateNode** : ノードの非アクティブ化
* **DeactivateNodesBatch**: 複数のノードの非アクティブ化
* **RemoveNodeDeactivations**: 複数のノードでの非アクティブ化の取り消し
* **GetNodeDeactivationStatus**: 非アクティブ化の状態の確認
* **NodeStateRemoved**: 削除済みノード状態のレポート
* **ReportFault**: 障害のレポート
* **FileContent**: イメージ ストア クライアント ファイルの転送 (クラスターの外部へ)
* **FileDownload**: イメージ ストア クライアント ファイルのダウンロードの開始 (クラスターの外部へ)
* **InternalList**: イメージ ストア クライアント ファイルの一覧操作 (内部)
* **Delete**: イメージ ストア クライアントの削除操作
* **Upload**: イメージ ストア クライアントのアップロード操作
* **NodeControl**: ノードの開始、停止、および再開
* **MoveReplicaControl**: ノードから別のノードへのレプリカの移動

### その他の操作

* **Ping**: クライアントの ping
* **Query**: 許可されるすべてのクエリ
* **NameExists**: 名前付け URI の存在確認



ユーザーのアクセス制御の種類は、既定では以下の操作に限定されます (管理者のアクセス制御では、これらの操作へのアクセスも許可されます)。

* **EnumerateSubnames**: 名前付け URI の列挙
* **EnumerateProperties**: 名前付けプロパティの列挙
* **PropertyReadBatch**: 名前付けプロパティの読み取り操作
* * * GetServiceDescription: 時間の長いポーリングのサービスの通知とサービスの説明を読む
* **ResolveService**: クレーム ベースのサービス解決
* **ResolveNameOwner**: 名前付け URI の所有者の解決
* **ResolvePartition**: システム サービスの解決
* **ServiceNotifications**: イベント ベースのサービスの通知
* **GetUpgradeStatus**: アプリケーションのアップグレード状態のポーリング
* **GetFabricUpgradeStatus**: クラスターのアップグレード状態のポーリング
* **InvokeInfrastructureQuery**: インフラストラクチャ タスクのクエリ
* **List**: イメージ ストア クライアント ファイルのリスト操作
* **ResetPartitionLoad**: failoverUnit の負荷のリセット
* **ToggleVerboseServicePlacementHealthReporting: 詳細なサービス配置正常性レポートの切り替え

## クライアント ロールの既定の設定の変更

クラスター マニフェスト ファイルでは、必要であれば、クライアントに管理者機能を提供できます。 移動して、既定値を変更することができます、 ** ファブリック設定* 中にオプション [クラスターの作成](service-fabric-cluster-creation-via-portal.md) で上記のように指定された設定を提供して、 **名** フィールドと **管理者、ユーザー** 値] フィールドにします。

## 次のステップ

[Service Fabric クラスターのセキュリティ](service-fabric-cluster-security.md)

[Service Fabric クラスターの作成](service-fabric-cluster-creation-via-portal.md)





