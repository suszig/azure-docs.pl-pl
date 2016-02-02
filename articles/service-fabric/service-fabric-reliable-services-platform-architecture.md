<properties
   pageTitle="Reliable Service のアーキテクチャ | Microsoft Azure"
   description="ステートフルおよびステートレスなサービスの reliable service アーキテクチャの概要について説明します。"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/03/2015"
   ms.author="alanwar"/>


# ステートフルとステートレスの Reliable Service のアーキテクチャ

ステートフルまたはステートレスな Service Fabric の信頼性の高いサービスがあります。 いずれの種類であっても、サービスはこの記事で説明されている特定のアーキテクチャで実行されます。
参照してください、 [reliable service の概要](service-fabric-reliable-services-introduction.md) ステートフルおよびステートレスなサービスの違いの詳細についてです。

## ステートフル Reliable Service

### ステートフル サービスのアーキテクチャ

![アーキテクチャの図](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### ステートフル Reliable Service

ステートフル Reliable Service は、StatefulService または StatefulServiceBase のいずれかのクラスから派生します。 Service Fabric によって提供されるこれらの基本クラスの両方と
さまざまなレベルのサポートや、ステートフルなサービス インターフェイスの Service Fabric を使用して、Service Fabric クラスター内でサービスとして参加するための抽象化を提供します。
StatefulService は StatefulServiceBase から派生します。つまり、StatefulServiceBase によってサービスの柔軟性が高まりますが、Service Fabric の内部を深く理解する必要があります。
参照してください、 [reliable service の概要](service-fabric-reliable-services-introduction.md) と [使用率が高度な信頼性の高いサービス](service-fabric-reliable-services-advanced-usage.md) 仕様の詳細については
StatefulService と StatefulServiceBase クラスを使用してサービスを作成します。

いずれの基本クラスでも、サービス実装の有効期間と役割が管理されます。 サービスの実装を行うには作業がある場合、サービスの実装はいずれかの基本クラスの仮想メソッドをオーバーライド可能性があります。
サービス実装のライフ サイクルでは、その時点で、通信リスナー オブジェクトを作成する必要がある場合、またはです。 サービスの実装
通信リスナー オブジェクトは、上の図に、通信リスナーの公開 ICommunicationListener はそれ自体を実装することがあります。
サービスの実装は、Service Fabric によって実装される通信リスナーを使用する、Service Fabric によって実装されます。

ステートフル Reliable Serviceでは、Reliable State Manager を使用して Reliable Collection が利用されます。 Reliable collection は、サービスに高可用性はローカル データ構造体です。
つまり、常にサービスのフェールオーバーに関係なく使用できます。 Reliable Collection は、Reliable State Provider によって実装されます。
Reliable collection の詳細については、を参照してください [Reliable Collection の概要](service-fabric-reliable-services-reliable-collections.md)

### Reliable State Manager および State Provider

Reliable state manager は、信頼性の高い状態プロバイダーを管理し、作成、削除、列挙、および信頼性の高い状態プロバイダーは、あることを確認する機能を持つオブジェクト
永続化された、高可用性にします。 信頼性の高い状態プロバイダーのインスタンスが永続化させて高可用性のデータ構造体のインスタンスをなどを表す、
ディクショナリやキューです。 Reliable State Provider はそれぞれ、Reliable State Provider と対話するためにステートフル サービスによって使用されるインターフェイスを公開します。 たとえば、次のように入力します。
IReliableDictionary が使用される IReliableQueue が使用されますが、信頼性の高いディクショナリと連携する信頼性の高いキューとやり取りします。 すべての Reliable State Provider は IReliableState インターフェイスを実装します。

信頼性の高い状態マネージャーには、ステートフル サービスからへのアクセスを許可する IReliableStateManager という名前のインターフェイスがあります。 Reliable State Provider とのインターフェイスが IReliableStateManager を介して返されます。

Reliable State Manager はプラグイン アーキテクチャを使用し、新しい種類の Reliable Collection を動的にプラグインできます。

高パフォーマンスのバージョン付き差分ストアを実装すると、Reliable Rictionary および Reliable Queue が作成されます。

### トランザクション レプリケーター

状態を確保するため、トランザクション レプリケーターのコンポーネントは、reliable state manager および reliable collection 内部の状態である、サービス
すべてのレプリカに整合性がログにも永続化は、サービスとその状態を実行します。 Reliable State Manager は、プライベート メカニズムを介してトランザクション レプリケーターとインターフェースをとります。

トランザクション レプリケーターは、ネットワーク プロトコルを使用してサービス インスタンスの他のレプリカと状態について通信するため、すべてのレプリカに最新の状態情報が行き渡ります。

トランザクション レプリケーターはログを使用して状態情報を永続化するため、プロセスまたはノードがクラッシュしても状態情報は失われません。 ログとインターフェースをとるにはプライベート メカニズムを使用します。

### ログ

ログ コンポーネントは高パフォーマンスの永続的なストアを提供しており、このストアは、回転盤またはソリッド ステート ディスクに合わせて最適化できるほか、ディスク領域が最も効率的に使用されるよう最適化することもできます。 デザイン
ログは永続的なストレージ (つまり、ハード_ディスク)
低待機時間と高スループットのノードにローカルではない永続的なストレージと比較して許可するように、ステートフルなサービスを実行しているノードにローカルであります。

ログ コンポーネントでは、 2 種類のログ ファイルが使用されます。 ノード全体で共有されるログ ファイルは、そのログ ファイル専用のディスクに格納する必要があります。 このファイルは、Service Fabric ノードに配置されます。
作業ディレクトリです。 サービスの各レプリカにも専用ログ ファイルがあり、サービスの作業ディレクトリ内に配置されます。 共有ログは、状態情報の移行の領域
専用ログ ファイルは、その最終的な送信先が永続化されます。 状態情報をまず共有ログ ファイルに書き込まれに部分的にデステージ専用ログこの設計では
バック グラウンドでファイルです。 このようにして共有ログへの書き込みの待機時間は最小化され、スループットは最大化されるため、サービスの処理時間がより短くなります。

ただし OptimizeForLocalSSD 設定を使用してソリッド ステート ディスクを最適化するため、ログ コンポーネントを構成すると、状態情報を書き込む専用に直接
ログ ファイル、共有ログ ファイルが無効にします。 ソリッド ステート ディスクではヘッドの移動の競合による遅延は発生しないため、専用ログ ファイルに直接書き込むことでペナルティは発生しません。

ログ コンポーネントが、OptimizeLogForLowerDiskUsage を使用してディスクの使用領域を最小限に抑えるように最適化されている場合、専用ログ ファイルが NTFS スパース ファイルとして作成されます。 ログ ファイルは通常ため
常に完全にいっぱいでない状態の情報のスパース ファイルの使用によりより多くのレプリカに使用可能なディスク領域の過度なプロビジョニングします。 ログ ファイルの領域をこの方法で構成されていない場合
事前に割り当てられ、ログ コンポーネントは、最高のパフォーマンスでファイルを直接書き込むことができます。

このログは、最小限のユーザー モード インターフェイスであるにもかかわらず、カーネル モード ドライバーとして書き込まれます。 ログをカーネル モード ドライバーとして実行して、できるすべてのサービスに最高のパフォーマンス
これを使用します。

ログの構成の詳細については、次を参照してください。 [ステートフル reliable services の構成](service-fabric-reliable-services-configuration.md)します。

## ステートレス Reliable Service

### ステートレス サービスのアーキテクチャ

![アーキテクチャの図](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### ステートレス Reliable Service

ステートレス サービスの実装は、StatelessService クラスまたは StatelessServiceBase クラスから派生しますが、StatelessService クラスよりも StatelessServiceBase クラスを使用したほうがより柔軟に作業できます。
いずれの基本クラスでも、サービスの有効期間と役割が管理されます。 サービスに作業を行う場合は、サービスの実装はいずれかの基本クラスの仮想メソッドをオーバーライド可能性があります。
サービスのライフ サイクルでは、その時点で、通信リスナー オブジェクトを作成する必要がある場合、またはです。 サービスが独自の通信を実装することがあります。
そのサービスの実装としての Service Fabric によって、上記の図で、通信リスナーの公開 ICommunicationListener が実装されているリスナー オブジェクトを使用して、
通信リスナーは Service Fabric によって実装されます。

参照してください、 [reliable service の概要](service-fabric-reliable-services-introduction.md) と [使用率が高度な信頼性の高いサービス](service-fabric-reliable-services-advanced-usage.md) 仕様の詳細については
StatelessService クラスおよび StatelessServiceBase クラスを使用してサービスを作成します。


## 次のステップ

Service Fabric の詳細については、次の項目を参照してください。

[Reliable Service の概要](service-fabric-reliable-services-introduction.md)

[クイック スタート](service-fabric-reliable-services-quick-start.md)

[Reliable Collection の概要](service-fabric-reliable-services-reliable-collections.md)

[信頼性の高いサービスには、使用率が高度な](service-fabric-reliable-services-advanced-usage.md)

[Reliable Service の構成](service-fabric-reliable-services-configuration.md)






