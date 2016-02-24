
<properties
   pageTitle="Service Fabric クラスターのセキュリティ: クライアント ロール | Microsoft Azure"
   description="この記事では、2 つのクライアント ロールと、ロールに提供されるアクセス許可について説明します。" 
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="coreysa"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/13/2015"
   ms.author="subramar"/>



# ロール ベースのアクセス制御 (Service Fabric クライアント用)

Service Fabric では、Service Fabric クラスターに接続されるクライアントのために、管理者用とユーザー用の 2 つの異なるアクセス コントロールの種類がサポートされています。 アクセス制御を使用すると、クラスター管理者は、ユーザーのグループごとに特定のクラスター操作へのアクセスを制限して、クラスターのセキュリティを強化できます。  管理者は管理機能 (読み取り/書き込み機能を含む) へのフル アクセスを持ち、ユーザーは既定では管理機能への読み取りアクセス (たとえば、クエリなど) と、アプリケーションおよびサービスを解決する機能だけを持ちます。

2 つのクライアント ロール (管理者またはクライアント) は、クラスターの作成時に、それぞれに個別の証明書を設定することによって指定されます。 参照してください [サービス ファブリック クラスターのセキュリティ](service-fabric-cluster-security.md) 詳細については、セキュリティで保護された Service Fabric クラスターを設定する方法です。


## 既定のアクセス制御の設定


管理者のアクセス制御の種類は、すべての FabricClient API へのフル アクセスを持ちます。 Service Fabric クラスターに対して、次のような任意の読み取りおよび書き込み操作を実行できます。

### アプリケーションおよびサービスの操作 
* **CreateService** : サービスの作成                          
* **CreateServiceFromTemplate** : テンプレートからサービスの作成                             
* **UpdateService** : 更新プログラムを提供                           
* **DeleteService** : サービスの削除                          
* **ProvisionApplicationType** : アプリケーションの種類のプロビジョニング                          
* **CreateApplication** : アプリケーションの作成                          
* **DeleteApplication** : アプリケーションの削除                          
* **UpgradeApplication** : 開始したり、アプリケーションのアップグレードの操作を中断します。                            
* **UnprovisionApplicationType** : アプリケーションの種類のプロビジョニング解除                          
* **MoveNextUpgradeDomain** : 明示的なアップグレード ドメインでアプリケーションのアップグレードを再開します。                             
* **ReportUpgradeHealth** : 現在のアップグレードの進行状況をアプリケーションのアップグレードを再開します。                             
* **ReportHealth** : 正常性の報告                           
* **PredeployPackageToNode** : 展開前の api                            
* **CodePackageControl** : コード パッケージの再開                             
* **RecoverPartition** : パーティションの回復                             
* **RecoverPartitions** : パーティションの回復                             
* **RecoverServicePartitions** : サービス パーティションを回復します。                          
* **RecoverSystemPartitions** : システム サービスのパーティションを回復します。                            


### クラスターの操作
* **ProvisionFabric** : MSI やクラスター マニフェスト プロビジョニング                            
* **UpgradeFabric** : クラスターのアップグレードを開始                             
* **UnprovisionFabric** : MSI やクラスター マニフェスト プロビジョニング解除                            
* **MoveNextFabricUpgradeDomain** : 明示的にドメインをアップグレードすると、クラスターのアップグレードを再開します。                          
* **ReportFabricUpgradeHealth** : 現在のアップグレードの進行状況、クラスターのアップグレードを再開します。                           
* **StartInfrastructureTask** : インフラストラクチャのタスクを開始                           
* **FinishInfrastructureTask** : インフラストラクチャのタスクの完了                             
* **InvokeInfrastructureCommand** : インフラストラクチャ タスク管理コマンド                             
* **ActivateNode** : ノードをアクティブ化                          
* **DeactivateNode** : ノードの非アクティブ化                          
* **DeactivateNodesBatch** : 複数のノードを非アクティブ化                            
* **RemoveNodeDeactivations** : 複数のノードで元に戻す非アクティブ化                            
* **GetNodeDeactivationStatus** : 非アクティブ化の状態を確認します。                          
* **NodeStateRemoved** : 削除されたノードの状態を報告                           
* **ReportFault** : エラーの報告                             
* **FileContent** : イメージ ストア クライアントのファイル転送 (クラスターの外部)                          
* **FileDownload** : イメージ ストア クライアント ファイルのダウンロード開始 (クラスターの外部)                          
* **InternalList** : イメージ ストア クライアント ファイルの一覧操作 (内部)                          
* **削除** : ストア クライアントの削除操作の画像                          
* **アップロード** : ストア クライアント アップロード操作の画像                          
* **NodeControl** : 開始、停止、;、ノードを再起動します。                            
* **MoveReplicaControl** : 1 つのノードから別のレプリカを移動                           

### その他の操作
* **Ping** : クライアントの ping 応答                           
* **クエリ** : すべてのクエリを許可
* **NameExists** : URI の存在の確認の名前を付ける                          



ユーザーのアクセス制御の種類は、既定では以下の操作に限定されます (管理者のアクセス制御では、これらの操作へのアクセスも許可されます)。

* **EnumerateSubnames** : URI 列挙型の名前を付ける                            
* **EnumerateProperties** : プロパティの列挙型の名前を付ける                             
* **PropertyReadBatch** : 読み取り操作のプロパティの名前を付ける                           
* * * GetServiceDescription: 時間の長いポーリングのサービスの通知とサービスの説明を読む                          
* **ResolveService** : 準拠しているベースのサービスの解決                           
* **ResolveNameOwner** : 名前付けの URI の解決中の所有者                             
* **ResolvePartition** : システム サービスを解決します。                          
* **ServiceNotifications** : イベント ベースのサービスの通知                          
* **GetUpgradeStatus** : アプリケーションのアップグレード ステータスをポーリング                             
* **GetFabricUpgradeStatus** : クラスターのアップグレード ステータスをポーリング                           
* **InvokeInfrastructureQuery** : インフラストラクチャのタスクのクエリを実行します。                             
* **リスト** : イメージ ストア クライアント ファイルの一覧表示操作                             
* **ResetPartitionLoad** : を failoverUnit のリセットの読み込み                            
* **ToggleVerboseServicePlacementHealthReporting: 詳細なサービス配置正常性レポートの切り替え                           

## クライアント ロールの既定の設定の変更

クラスター マニフェスト ファイルでは、必要であれば、クライアントに管理者機能を提供できます。 移動して、既定値を変更することができます、**ファブリック設定* 中にオプション [クラスターの作成](service-fabric-cluster-creation-via-portal.md) で上記のように指定された設定を提供して、 **名前** フィールドと **管理者、ユーザー** 値] フィールドにします。 

## 次のステップ

[Service Fabric クラスターのセキュリティ](service-fabric-cluster-security.md)

[Service Fabric クラスターの作成](service-fabric-cluster-creation-via-portal.md)

