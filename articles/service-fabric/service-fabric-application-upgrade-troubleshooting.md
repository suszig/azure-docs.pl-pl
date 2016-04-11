<properties
   pageTitle="アプリケーションのアップグレードのトラブルシューティング | Microsoft Azure"
   description="この記事では、Service Fabric アプリケーションのアップグレードに関する一般的な問題とその解決方法のいくつかについて説明します。"
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
   ms.date="08/11/2015"
   ms.author="subramar"/>

# アプリケーションのアップグレードのトラブルシューティング

この記事では、Service Fabric アプリケーションのアップグレードに関する一般的な問題とその解決方法のいくつかについて説明します。

## アプリケーションのアップグレードで障害が発生した場合のトラブルシューティング

アップグレードに失敗したときの出力、 **Get-servicefabricapplicationupgrade** コマンドにはは、エラーのデバッグの追加情報が含まれます。 この情報を次の目的に使用できます。

1. 失敗の種類を識別する
2. 失敗の理由を識別する
3. 詳細な調査のために、障害が発生したコンポーネントを分離する

Service Fabric がかどうかに関係なくエラーを検出するとすぐに、この情報が表示される、 **FailureAction** をロールバックするか、アップグレードを中断します。

### 失敗の種類を識別する

出力で **Get-servicefabricapplicationupgrade**, 、**FailureTimestampUtc** Service Fabric によってアップグレードの失敗が検出されました (UTC) でタイムスタンプを識別し、 **FailureAction** がトリガーされました。  **FailureReason** 障害の潜在的な 3 つの大まかな原因のいずれかを指定します。

1. UpgradeDomainTimeout - は、特定のアップグレード ドメインの完了に時間があることを示しますと **UpgradeDomainTimeout** 有効期限が切れています。
2. OverallUpgradeTimeout - アップグレード全体が完了するには長すぎることを示しますと **UpgradeTimeout** 有効期限が切れています。
3. Healthcheck - アップグレード ドメインをアップグレードした後、アプリケーションこと、指定された正常性ポリシーに従って正常でないと **HealthCheckRetryTimeout** 有効期限が切れています。

これらのエントリは、アップグレードが失敗し、ロールバックを開始する場合にのみ出力に表示されます。 エラーの種類に応じて、詳細な情報が表示されます。

### アップグレードのタイムアウトを調査する

アップグレードのタイムアウト エラーは、サービスの可用性の問題に起因することが最も一般的です。 次に示す出力は、サービスのレプリカまたはインスタンスのアップグレードが新しいコードのバージョンで起動に失敗する一般的な例です。  **UpgradeDomainProgressAtFailure** フィールドは、エラーの発生時に保留中のアップグレード作業のスナップショットをキャプチャします。

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
~~~

この例では、表示されていますアップグレード ドメインでアップグレードが失敗したこと *MYUD1* と 2 つのパーティション (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* と *4b43f4d8-b26b-424e-9307-7a7a62e79750*) がスタックし、プライマリ レプリカを行うことはできません (*WaitForPrimaryPlacement*) 対象のノードで *Node1* と *Node4*します。  **Get-servicefabricnode** これら 2 つのノードがアップグレード ドメイン内にあることを確認するコマンドを使用できる *MYUD1*します。  *UpgradePhase* という *PostUpgradeSafetyCheck*, 、アップグレード ドメイン内のすべてのノードがアップグレードを完了した後、これらの安全性チェックが発生していることを意味します。 これらすべての情報を組み合わせて、潜在的な問題と新しいバージョンのアプリケーション コードを示します。 最も一般的な問題は、プライマリ コード パスのオープンまたは昇格でのサービスのエラーです。

 *UpgradePhase* の *PreUpgradeSafetyCheck* 実際には、アップグレードを実行する前に、アップグレード ドメインの準備で問題があったことを意味します。 この場合の最も一般的な問題は、プライマリ コード パスのクローズまたは降格でのサービス エラーです。

現在 **UpgradeState** は *RollingBackCompleted*, 元のアップグレードをロールバックを実行する必要がありますので、 **FailureAction**, 、自動的にロールバック障害発生時にアップグレードします。 元のアップグレードを手動で行われたかどうか **FailureAction**, 、アップグレードは代わりにライブを許可するように一時停止状態で、デバッグ アプリケーションのします。

### 正常性チェックの障害を調査する

正常性チェックの障害は、アップグレード ドメインのすべてのノードがアップグレードを終了し、すべての安全性チェックを渡した後で、さまざまなその他の問題によりトリガーされる可能性があります。 次に示す出力は、正常性チェックの失敗により、アップグレードに障害が発生する一般的な例です。  **UnhealthyEvaluations** フィールドはすべてのスナップショットをキャプチャ時、アップグレードの失敗が、ユーザー指定のチェック状態が失敗している [正常性ポリシー](service-fabric-health-introduction.md)します。

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
~~~

正常性チェックの障害を調査してまず Service Fabric の正常性モデルの理解が必要ですが、このような詳細な知識がなくてわかります 2 つのサービスが正常である: *ファブリック:/DemoApp/Svc3* と *ファブリック:/DemoApp/Svc2* と共にエラーの正常性レポート (この場合は"InjectedFault") です。 この例では 2 を 4 つのサービスが異常な 0% の異常な場合の既定のターゲットの下 (*MaxPercentUnhealthyServices*)。

アップグレードが失敗した場合を指定して中断された、 **FailureAction** 手動アップグレードを開始するときのようにで調査を行えるライブ システム障害の状態を追加のアクションを実行する前に必要な場合です。

### 中断されたアップグレードから回復する

ロールバックを **FailureAction**, 、回復に必要なアップグレードがない自動的にロールバックされるためが発生しているがあります。 手動の **FailureAction**, 、いくつかの回復オプションがあります。

1. ロールバックを手動でトリガーする
2. アップグレードの残りの項目を手動で続行する
3. 監視対象のアップグレードを再開する

 **Start-servicefabricapplicationrollback** コマンドは、アプリケーションのロールバックを開始するいつでも使用できます。 コマンドが正常に返されると、ロールバック要求が、システムに登録されてすぐに開始されます。

 **Resume-servicefabricapplicationupgrade** コマンドは、手動でアップグレードの残りの続行を使用することができます、一度に 1 つのアップグレード ドメイン。 このモードでは、安全性チェックのみがシステムによって実行され、正常性チェックは実行されません。 このコマンドは、必ず際に使用される、 *UpgradeState* 表示 *RollingForwardPending*, 、つまり現在のアップグレード ドメインがアップグレードを完了したが、次のアップグレード ドメインがまだ開始 (保留中)。

 **Update-servicefabricapplicationupgrade** 両方の安全性の監視対象のアップグレードを再開するコマンドを使用することし、正常性チェックが実行されます。

~~~
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
~~~

アップグレードは最後に中断したアップグレード ドメインから続行し、以前と同じアップグレード パラメーターと正常性ポリシーを使用します。 必要に応じて、上記の出力に示すアップグレード パラメーターと正常性ポリシーを、アップグレードを再開するときと同じコマンドで変更できます。 この例では、アップグレードが、その他すべてのパラメーターは変更されないまま監視対象モードで再開されたため、以前と同じパラメーターと正常性ポリシーを使用しています。

## 詳細なトラブルシューティング

### Service Fabric が指定された正常性ポリシーに従っていない

考えられる原因 1:

Service Fabric は、正常性評価のためのエンティティ (レプリカ、パーティション、サービスなど) の実際の数値にすべてのパーセンテージを変換し、常に全体のエンティティ数を最も近い値に切り上げします。 などの場合最大 _MaxPercentUnhealthyReplicasPerPartition_ が 21% で、[Service Fabric アプリケーションを最大 2 つのレプリカが 5 個のレプリカ (つまり、 `Math.Ceiling (5\*0.21)`) が、パーティションの正常性を評価するときに、正常です。 正常性ポリシーは、この点を考慮して設定してください。

考えられる原因 2:

正常性ポリシーは、特定のサービス インスタンスではなく、サービスの合計のパーセンテージにより指定されます。 たとえば、アップグレードの前に、アプリケーションに A、B、C、および D の 4 つのサービス インスタンスがあり、サービス D が異常だが、アプリケーションに大きく影響がないとします。 アップグレード中に既知の問題のあるサービス D を無視し、パラメーターを設定する *MaxPercentUnhealthyServices* 25% が A、B、および C のみと仮定した場合に正常である必要があります。 ただし、アップグレード中、C が異常になり、D が正常になる可能性があります この場合でも、アップグレードは正常に完了します。これは、サービスの 25% のみが異常であり、D ではなく C により予期しないエラーが発生する可能性があるためです。この状態では、D は A、B、および C とは異なるサービス型としてモデル化する必要があります。正常性ポリシーはサービス型ごとに指定できるため、これにより様々な異常の割合のしきい値を、アプリケーションの役割に基づいて別のサービスに割り当てることができます。

### アプリケーションのアップグレードの正常性ポリシーを指定していないが、未指定のタイムアウトによりアップグレードが失敗する

正常性ポリシーでから取得されます。 アップグレード要求に指定されてのいない場合、 *ApplicationManifest.xml* 、現在のアプリケーションのバージョン (たとえば、アプリケーション X を v1 から v2、アプリケーション X の v1 に指定されたアプリケーションの正常性ポリシーにアップグレードを使用する場合)。 別の正常性ポリシーをアップグレードに使用するには、アプリケーションのアップグレードの API 呼び出しの一部としてポリシーを指定する必要があります。 アップグレードの実行中、API 呼び出しの一部として指定されたポリシーのみが適用されることに注意してください。 指定されたポリシーがアップグレードを完了すると、 *ApplicationManifest.xml* 使用されます。

### 不適切なタイムアウトが指定されています。

ユーザーの場合、タイムアウトが設定に一貫性がないなどのことかということがあります、 *UpgradeTimeout* より小さい *UpgradeDomainTimeout*します。 このような場合は、エラーが返されます。 これが発生するその他の場合は、場合 *UpgradeDomainTimeout* の合計より小さい *HealthCheckWaitDuration* と *HealthCheckRetryTimeout* または *UpgradeDomainTimeout* の合計より小さい *HealthCheckWaitDuration* と *HealthCheckStableDuration*します。

### アップグレードの時間がかかりすぎる

アップグレードを完了するのにかかる時間は、指定されたさまざまな正常性チェックとタイムアウトにより異なります。これらは、アプリケーションのアップグレード (パッケージのコピー、デプロイ、安定化を含む) にかかる時間により異なります。 タイムアウトが短すぎるとアップグレードが失敗する可能性が高くなるため、タイムアウトは長めにして開始することをお勧めします。

タイムアウトが、アップグレードの時間にどのように影響するのか、以下で簡単に確認できます。

アップグレード ドメインを完了できません。 アップグレードよりも高速 *HealthCheckWaitDuration* + *HealthCheckStableDuration*します。

アップグレードの失敗が発生することはできませんよりも高速 *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*します。

アップグレード ドメインのアップグレードの時間によって制限されます *UpgradeDomainTimeout*します。  場合 *HealthCheckRetryTimeout* と *HealthCheckStableDuration* が両方とも 0 以外であり、アプリケーションの正常性をやり取りで、アップグレードにタイムアウトが最終的に *UpgradeDomainTimeout*します。 *UpgradeDomainTimeout* カウント ダウン アップグレード ドメインは、現在のアップグレードを開始するを開始します。

## 次のステップ

[Visual Studio による Service Fabric アプリケーションのアップグレード](service-fabric-application-upgrade.md)

[PowerShell を使用した Service Fabric アプリケーションのアップグレード](service-fabric-application-upgrade-powershell.md)

[アップグレード パラメーター](service-fabric-application-upgrade-parameters.md)

[手動アップグレードと差分のパッケージを使用したアップグレード](service-fabric-application-upgrade-advanced.md)

[データのシリアル化](service-fabric-application-upgrade-data-serialization.md)
 


