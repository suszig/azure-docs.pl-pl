<properties
   pageTitle="Reliable Service プログラミング モデルの詳細な使用方法"
   description="サービスの柔軟性を高めるための Service Fabric の Reliable Services プログラミング モデルの詳細な使用方法について説明します。"
   services="Service-Fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/17/2015"
   ms.author="jesseb"/>


# Reliable Services プログラミング モデルの詳細な使用方法

Service Fabric を使用すると、信頼性の高いステートレス サービスとステートフル サービスを簡単に作成および管理できます。 このガイドでは、サービスの制御と柔軟性を高めるための Reliable Services プログラミング モデルの詳細な使用方法について説明します。 このガイドを読む前に理解しておく [Reliable Services プログラミング モデル](service-fabric-reliable-services-introduction.md)します。

## ステートレス サービスの基本クラス

StatelessService 基本クラスでは RunAsync() と CreateServiceInstanceListeners() を提供しており、大半のステートレス サービスにはこれで十分です。 StatelessServiceBase クラスは、StatelessService の基盤となり、追加のサービス ライフ サイクル イベントを公開します。 制御の強化または柔軟性の向上を必要とする場合は、StatelessServiceBase から派生することができます。 開発者向けリファレンス ドキュメントを参照してください [StatelessService](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.services.statelessservice.aspx) と [StatelessServiceBase](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.services.statelessservicebase.aspx) の詳細。

- `OnInitialize(StatelessServiceInitializiationParameters) を無効にします。`
    OnInitialize は、最初に Service Fabric によって呼び出されるメソッドです。 サービス名、パーティション ID、インスタンス ID、およびコード パッケージ情報などのサービス初期化情報が得られます。 ここでは、複雑な処理を行うべきではありません。 時間のかかる初期化は、OnOpenAsync で行う必要があります。

- `タスク OnOpenAsync (IStatelessServicePartition, CancellationToken)`
    OnOpenAsync は、ステートレス サービス インスタンスが使用されるときに呼び出されます。 この時点で、拡張サービス初期化タスクを開始できます。

- `タスク OnCloseAsync(CancellationToken)`
    OnCloseAsync はステートレス サービス インスタンスがシャット ダウンを正常に送信されているときに呼び出されます。 これは、サービスのコードがアップグレードされるか、サービス インスタンスが負荷分散のために移動されるか、または一時的なエラーが検出されたときに行われる可能性があります。 OnCloseAsync を使用して、安全にすべてのリソースを閉じたり、バック グラウンド処理を停止したり、外部の状態の保存を完了したり、既存の接続を終了したりすることができます。

- `OnAbort() を無効にします。`
    OnAbort は、ステートレス サービス インスタンスが強制的にシャット ダウンされているときに呼び出されます。 これは一般に、ノードで永続的なエラーが検出されたときや Service Fabric が内部エラーのために、サービス インスタンスのライフ サイクルを確実に管理できないときに呼び出されます。

## ステートフル サービスの基本クラス

StatefulService 基本クラスは、ほとんどのステートフル サービスに十分であるはずです。 ステートレス サービスと同様に、StatefulServiceBase クラスは StatefulService の基盤となり、追加のサービス ライフ サイクル イベントを公開します。 さらに、カスタムの信頼性の高い状態プロバイダーを提供し、必要に応じてセカンダリで通信リスナーをサポートできます。 制御の強化または柔軟性の向上を必要とする場合は、StatefulServiceBase から派生することができます。 開発者向けリファレンス ドキュメントを参照してください [StatefulService](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.services.statefulservice.aspx) と [StatefulServiceBase](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.services.statefulservicebase.aspx) の詳細。

- `タスク OnChangeRoleAsync (ReplicaRole, CancellationToken)`
    OnChangeRoleAsync は、ステートフル サービスは、ロールの変更などをプライマリまたはセカンダリのときに呼び出されます。 プライマリ レプリカには書き込み状態 (信頼性の高いコレクションの作成と書き込みが許可される) が指定され、セカンダリ レプリカには読み取り状態 (既存の信頼性の高いコレクションからのみ読み取ることができる) が指定されます。 ロールの変更に対応して、セカンダリでの読み取り専用検証、レポート生成、またはデータ マイニングの実行などのバック グラウンド タスクを開始または更新できます。

- `IStateProviderReplica CreateStateProviderReplica()`
    ステートフル サービスには、信頼性の高い状態プロバイダーが予期されます。 StatefulService は、信頼性の高いコレクション (辞書やキューなど) を提供する ReliableStateManager クラスを使用します。 このメソッドを上書きし、ReliableStateManager クラスを構成できます。その場合、ReliableStateManagerConfiguration をそのコンストラクターに渡します。 これにより、カスタムのステート シリアライザーを提供したり、データが失われたときの動作を指定したり、レプリケーター/ステート プロバイダーを構成したりできます。

さらに、StatefulServiceBase は、StatelessServiceBase と同じ 4 つのライフ サイクル イベントを同じセマンティクスとユース ケースで提供します。

- `OnInitialize(StatefulServiceInitializiationParameters) を無効にします。`
- `タスク OnOpenAsync (IStatefulServicePartition, CancellationToken)`
- `タスク OnCloseAsync(CancellationToken)`
- `OnAbort() を無効にします。`

## 次のステップ

Service Fabric に関する詳細なトピックについては、次の記事を参照してください。

- [ステートフル Reliable Services の構成](service-fabric-reliable-services-configuration.md)

- [Service Fabric の正常性の概要](service-fabric-health-introduction.md)

- [システム正常性レポートを使用したトラブルシューティング](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

- [配置の制約の概要](service-fabric-placement-constraint.md)





