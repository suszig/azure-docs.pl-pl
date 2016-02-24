<properties
   pageTitle="Azure Service Fabric Reliable Services の構成の概要 | Microsoft Azure"
   description="Azure Service Fabric のステートフル Reliable Services を構成する方法について説明します。"
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/28/2015"
   ms.author="sumukhs"/>

# ステートフル Reliable Services の構成
ステートフル Reliable Service の既定の構成は、構成パッケージ (Config) か、サービス実装 (コード) を通じて変更できます。

+ **Config** -アプリケーション内の各サービスの"Config"フォルダーの下の Visual Studio パッケージ ルートで生成された"Settings.xml"ファイルを変更することで、構成パッケージを使用して構成を実行します。
+ **コード**   -を StatefulService.CreateReliableStateManager をオーバーライドし、適切なオプション セットを持つ ReliableStateManagerConfiguration オブジェクトを使用する ReliableStateManager を作成するコードを使用して構成を実行します。

既定では、Service Fabric ランタイムは "Settings.xml" ファイルで事前定義済みのセクション名を検索し、基になるランタイム コンポーネントの作成中に構成値を使用します。

> [AZURE.NOTE]  **いない** コードを使用して、サービスを構成する予定がない場合は、Visual Studio のソリューションで生成される"Settings.xml"ファイルでは、以下の構成のセクション名を削除します。
構成パッケージまたはセクションの名前を変更するには、ReliableStateManager の構成時にコードを変更する必要があります。


## レプリケーターのセキュリティ構成
レプリケーション時に使用される通信チャネルをセキュリティ保護する場合は、レプリケーターのセキュリティ構成を使用します。 これは、サービスは互いのレプリケーション トラフィックを表示できないため、高可用性データもセキュリティ保護されることを意味します。
既定では、空のセキュリティ構成セクションでレプリケーション セキュリティは有効になりません。

### 既定のセクション名
ReplicatorSecurityConfig

> [AZURE.NOTE] このセクションの名前を変更するには、このサービスを使用する ReliableStateManager を作成するときに、ReliableStateManagerConfiguration コンス トラクターに replicatorSecuritySectionName パラメータを上書きします。


## レプリケーター構成
状態をローカルにレプリケートして永続化することでステートフル Reliable Service の状態プロバイダーの状態の信頼性を高める役割を持つレプリケーターを構成する場合に、レプリケーター構成を使用します。
既定の構成は Visual Studio テンプレートによって生成され、これで十分なはずです。 このセクションでは、レプリケーターのチューニングに使用できる追加の構成について説明します。

### 既定のセクション名
ReplicatorConfig

> [AZURE.NOTE] このセクションの名前を変更するには、このサービスを使用する ReliableStateManager を作成するときに、ReliableStateManagerConfiguration コンス トラクターに replicatorSettingsSectionName パラメータを上書きします。


### 構成名
|名前|単位|既定値|解説|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|秒|0.05|操作を受信してからプライマリに受信確認を返すまで、セカンダリでレプリケーターが待機する期間です。 この期間内で処理された操作に対して送信される他の受信確認は、1 つの応答として送信されます。|
|ReplicatorEndpoint|該当なし|該当なし - RequiredParameter|プライマリとセカンダリのレプリケーターがレプリカ セットの他のレプリケーターと通信するために使用する IP アドレスとポートです。 これは、サービス マニフェストの TCP リソース エンドポイントを参照する必要があります。 参照してください [サービス マニフェストのリソース](service-fabric-service-manifest-resources.md) 詳細サービス マニフェストにエンドポイント リソースを定義する方法です。 |
|MaxPrimaryReplicationQueueSize|操作数|8192|プライマリ キューの操作の最大数です。 プライマリ レプリケーターがすべてのセカンダリ レプリケーターから受信確認を受信した後に、操作は解放されます。この値は 64 より大きく、2 のべき乗である必要があります。|
|MaxSecondaryReplicationQueueSize|操作数|16384|セカンダリ キューの操作の最大数です。 操作は、永続性によってその状態の高可用性が実現されてから解放されます。 この値は 64 より大きく、2 のべき乗である必要があります。|
|CheckpointThresholdInMB|MB|200|その後で状態がチェックポイントされるログ ファイル領域の量。|
|MaxRecordSizeInKB|KB|1024|レプリケーターがログに書き込むことができるレコードの最大サイズです。 この値は 4 の倍数で 16 より大きい必要があります。|
|OptimizeLogForLowerDiskUsage|ブール値|true|true の場合、NTFS スパース ファイルを使用してレプリカの専用ログ ファイルが作成されるように、ログが構成されます。 これにより、ファイルで使用される実際のディスク領域が削減されます。 false の場合、最適な書き込みパフォーマンスを提供する固定の割り当てがファイルに作成されます。|
|MaxRecordSizeInKB|KB|1024|レプリケーターがログに書き込むことができるレコードの最大サイズです。 この値は 4 の倍数で 16 より大きい必要があります。|
|SharedLogId|guid|""|このレプリカで使用される共有ログ ファイルの識別に使用する一意の guid を指定します。 一般にサービスはこの設定を使用しないはずですが、SharedLogId を指定した場合は、SharedLogPath も指定する必要があります。|
|SharedLogPath|完全修飾パス名|""|このレプリカの共有ログ ファイルが作成される完全修飾パスを指定します。 一般にサービスはこの設定を使用しないはずですが、SharedLogPath を指定した場合は、SharedLogId も指定する必要があります。|


## コードによるサンプル構成
```csharp
protected override IReliableStateManager CreateReliableStateManager()
{
    return new ReliableStateManager(
        new ReliableStateManagerConfiguration(
            new ReliableStateManagerReplicatorSettings
            {
                RetryInterval = TimeSpan.FromSeconds(3)
            }));
}
```


## サンプル構成ファイル
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```


## 解説
BatchAcknowledgementInterval は、レプリケーションの待機時間を制御します。 値が '0' の場合、待機時間は最短になりますが、スループットに影響します (送信および処理が必要な受信確認メッセージが増え、それぞれに含まれる受信確認が少なくなります)。
BatchAcknowledgementInterval の値が大きいほど、全体的なレプリケーションのスループットが高くなり、操作の待機時間が長くなります。 これは、トランザクションのコミットの待機時間に直結します。

CheckpointThresholdInMB の値により、レプリケーターがレプリカの専用ログ ファイルの状態情報の保存に使用できるディスク領域の量を制御します。 この値を既定値より大きくすると、ログで使用できる操作履歴が増えるために部分的な状態転送が発生するので、新しいレプリカがセットに追加されるときの再構成時間が短くなりますが、クラッシュ後のレプリカの復旧時間は長くなる可能性があります。

OptimizeForLowerDiskUsage の設定では、ログ ファイルの領域が「過剰プロビジョニング」されるよう、アクティブなレプリカは、非アクティブなレプリカは少ないディスクで使用される中に、そのログ ファイルでの状態の詳細を保存できます。
領域です。 状態情報 OptimizeForLowerDiskUsage を false に設定して、ディスク領域の不足に書き込まれますが、これによりより多くのレプリカが原因で発生するよりも、ノードでホストするには
ログ ファイルをより迅速にします。

MaxRecordSizeInKB は、レプリケーターがログ ファイルに書き込むことのできるレコードの最大サイズを定義します。 ほとんどすべてのケースでは既定の 1,024 KB レコード サイズが最適なかどうか、サービスは、
この値を大きく必要がありますし、状態情報の一部である大規模なデータ アイテムを引き起こしています。 Maxrecordsizeinkb 小さいレコードとして 1024年より小さくするメリットはほとんどがあります。
小さいレコードに必要な領域のみを使用します。 これを変更する必要があるのは、まれなケースだけであると予想されます。

SharedLogId と SharedLogPath の設定は、常に一緒に使用して、サービスがノードの既定の共有ログとは別の共有ログを使用できるようにします。 最適な効率を高めるため、多くのサービスとして
可能なは、同じ共有ログを指定する必要があります。 共有ログ ファイルは、ヘッドの移動の競合が減るように、共有ログ ファイル専用に使用されるディスクに配置する必要があります。 これを変更する必要があるのは、まれなケースだけであると予想されます。

