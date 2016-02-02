<properties
   pageTitle="Service Fabric 高信頼アクターの概要 | Microsoft Azure"
   description="Service Fabric 高信頼アクターのプログラミング モデルの概要"
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2015"
   ms.author="vturecek"/>


# Service Fabric 高信頼アクターの概要

高信頼アクター API はによって提供される 2 つの高度なフレームワークの 1 つ [Service Fabric](service-fabric-technical-overview.md), 、と共に、 [Reliable Services API](service-fabric-reliable-services-introduction.md)します。

高信頼アクター API は、アクター パターンに基づいて、Service Fabric によって実現する拡張性と信頼性を利用しながら、コードを簡素化する非同期のシングル スレッド プログラミング モデルを提供します。

## アクター

アクターは、独立したシングル スレッド コンポーネントであり、状態と動作の両方をカプセル化します。 このアクターは、.NET オブジェクトに似ているため、自然なプログラミング モデルを提供します。 .NET オブジェクトが .NET 型のインスタンスであるように、アクターはすべてアクター型のインスタンスです。 たとえば、電卓の機能を実装するアクター型がある場合や、クラスター全体のさまざまなノードで分散されるその型のアクターが多数存在する場合があります。 このようなアクターはそれぞれ、アクター ID で一意に識別されます。

## アクター インターフェイスの定義と実装

アクターは、要求応答パターンを使用して非同期メッセージを渡すことによって、他のアクターを含む、システムの残りの部分と対話します。 これらの対話は、非同期メソッドとしてインターフェイスで定義されます。 たとえば、電卓の機能を実装するアクター型のインターフェイスは、次のように定義される場合があります。

```csharp
public interface ICalculatorActor : IActor
{
    Task<double> AddAsync(double valueOne, double valueTwo);
    Task<double> SubtractAsync(double valueOne, double valueTwo);
}
```

アクター型では、このインターフェイスを次のように実装できます。

```csharp
public class CalculatorActor : StatelessActor, ICalculatorActor
{
    public Task<double> AddAsync(double valueOne, double valueTwo)
    {
        return Task.FromResult(valueOne + valueTwo);
    }

    public Task<double> SubtractAsync(double valueOne, double valueTwo)
    {
        return Task.FromResult(valueOne - valueTwo);
    }
}
```

メソッド呼び出しとその応答が、最終的にはクラスター全体でネットワーク要求になるため、引数と返されるタスクの結果の型は、プラットフォームがシリアル化できる必要があります。 具体的には、必要がある [データ コントラクト シリアル化可能](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)します。
> [AZURE.TIP] Fabric アクター ランタイムは、いくつか出力 [アクター メソッドに関するイベントとパフォーマンス カウンター](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters)します。 これらは、診断やパフォーマンスの監視に役立ちます。

アクター インターフェイス メソッドに関連する次の規則は特筆に価する機能です。
- アクター インターフェイス メソッドはオーバー ロードすることはできません。
- アクター インターフェイス メソッドを持つことはできません、ref、または省略可能なパラメーターです。

## アクターの通信

### アクター プロキシ

アクター クライアント API は、アクター インスタンスとアクター クライアント間の通信を提供します。 アクターと通信するために、クライアントは、アクター インターフェイスを実装するアクター プロキシ オブジェクトを作成します。 クライアントは、プロキシ オブジェクトでメソッドを呼び出すことによって、アクターと対話します。 アクター間の通信だけでなく、クライアントとアクター間の通信でもアクター プロキシを使用できます。 ここでも電卓を例とします。電卓アクターのクライアント コードは以下のように記述できます。

```csharp
ActorId actorId = ActorId.NewId();
string applicationName = "fabric:/CalculatorActorApp";
ICalculatorActor calculatorActor = ActorProxy.Create<ICalculatorActor>(actorId, applicationName);
double result = calculatorActor.AddAsync(2, 3).Result;
```

アクター プロキシ オブジェクトの作成には、アクター ID とアプリケーション名という 2 つの情報が使用されていることに注意してください。 アクター ID は、アプリケーション名を識別中に、アクターを一意に識別する識別子、 [Service Fabric アプリケーション](service-fabric-reliable-actors-platform.md#service-fabric-application-model-concepts-for-actors) で、アクターが配置されています。

### アクターの有効期間

Service Fabric アクターは仮想アクターです。つまり、その有効期間は、メモリ内表現に関連付けられていません。 したがって、明示的に作成したり、破棄したりする必要はありません。 アクター ランタイムは、そのアクターの要求の初回受信時に、自動的にアクターをアクティブ化します。 アクターが一定の時間使用されていない場合、アクター ランタイムは、アクターが存在するという情報を維持しながら、メモリ内オブジェクトをガベージ コレクトします。このアクターは、必要に応じて後で再アクティブ化できます。 詳細については、次を参照してください。 [アクターのライフ サイクルとガベージ コレクション](service-fabric-reliable-actors-lifecycle.md)します。

### 場所の透過性と自動フェールオーバー

Service Fabric アクターは、高可用性と信頼性を実現するために、クラスター全体にアクターを分散し、障害が発生したノードのアクターを、正常に稼働しているノードに必要に応じて自動的に移行します。  `ActorProxy` ID を使用してアクターを検索するために必要な解決を実行してクライアント側でクラス [パーティション](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) との通信チャネルを開きます。  `ActorProxy` 通信エラーやフェールオーバーの場合も再試行します。 これにより障害が発生してもメッセージは確実に配信されます。また、アクター実装によって、同じクライアントから重複するメッセージを取得できることも意味します。

## 同時実行

### ターンに基づくアクセス

アクター ランタイムは、アクター メソッドにアクセスするためのターンに基づくモデルを提供します。 これは、アクター コード内で随時アクティブ化できるスレッドが 1 つだけであることを意味します。

ターンは、その他のアクターまたはクライアントからの要求に応じたアクター メソッドの実行の完了またはの完全な実行、 [タイマーとアラーム](service-fabric-reliable-actors-timers-reminders.md) コールバックします。 これらのメソッドとコールバックが非同期でも、アクター ランタイムはこれらをインターリーブしません。ターンは、新しいターンが許可される前に完全に完了する必要があります。 つまり、現在実行しているアクター メソッドまたはタイマーとアラームのコールバックは、メソッドまたはコールバックの新しい呼び出しが許可される前に完全に完了する必要があります。 実行がメソッドまたはコールバックから返され、メソッドまたはコールバックによって返されたタスクが完了した場合は、そのメソッドまたはコールバックは完了したと見なされます。 ターンごとの同時実行は、メソッド、タイマーおよびコールバックが異なる場合でも優先されることに注意してください。

アクター ランタイムは、ターンの開始時にアクターごとのロックを取得し、ターンの終了時にロックを解除することで、ターンごとの同時実行を強制します。 そのため、ターンごとの同時実行は、アクター全体ではなく、アクターごとに強制されます。 アクターのメソッドおよびタイマーとアラームのコールバックは、別のアクターの代わりに同時に実行できます。

上記の概念を以下の例で示します。 2 つの非同期メソッド (つまり、*Method1* と *Method2*)、タイマーおよびアラームを実装するアクター型があるとします。 以下の図は、このアクター型に属する 2 つのアクター (*ActorId1* と *ActorId2*) の代わりにこれらのメソッドとコールバックを実行する場合のタイムラインの例を示しています。

![][1]

上記の図では次の規則に従っています。

- 各垂直線は、特定のアクターの代わりにメソッドまたはコールバックを実行する場合の論理フローを示しています。
- 各垂直線上にマークされているイベントは発生順になっており、古いイベントの下に新しいイベントが続きます。
- タイムラインでは、異なるアクターに対応する異なる色が使用されています。
- 強調表示は、アクターごとのロックがメソッドまたはコールバックの代わりに保持される期間を示すために使用されています。

上記の図では次の点に注意する必要があります。

- クライアント要求 *xyz789* に応じて *ActorId2* の代わりに *Method1* を実行しているときに、別のクライアント要求 *abc123* が到着する場合があります。その場合も、*ActorId2* が *Method1* を実行する必要があります。 ただし、*Method1* の後者の実行は、前者の実行が完了するまでは開始されません。 同様に、*Method1* がクライアント要求 *xyz789* に応じて実行されている間に、*ActorId2* によって登録されたアラームが開始されます。 アラームのコールバックが実行されるのは、*Method1* の両方の実行が完了した場合のみです。 これはすべてターンごとの同時実行が *ActorId2* に対して強制されるためです。
- 同様に、ターンごとの同時実行は *ActorId1* に対しても強制されます。図に示されているように、*ActorId1* の代わりに *Method1*、*Method2* およびタイマーのコールバックが順次実行されます。
- *ActorId1* の代理としての *Method1* の実行は、*ActorId2* の代理として実行と重複しています。 これは、ターンごとの同時実行が、アクター全体ではなくアクター内でのみ強制されるためです。
- メソッドやコールバックの実行の一部で、 `タスク` によって返されるメソッドやコールバック メソッドが返してから完了します。 その他、 `タスク` は既に完了して、メソッドやコールバックが返すまでです。 いずれの場合も、アクターごとのロックが解除されるどちらの後の動作、つまりメソッドやコールバックが返す後にのみ、 `タスク` が完了するとします。

### 再入

アクター ランタイムでは、既定で再入が許可されます。 つまり、アクター A のアクター メソッドが アクター B に対してメソッドを呼び出してから、アクター B がアクター A に対して別のメソッドを呼び出す場合、メソッドは同じ論理呼び出しチェーン コンテキストの一部であるため実行が許可されます。 すべてのタイマーとアラームの呼び出しは新しい論理呼び出しコンテキストで始まります。 参照してください [再入](service-fabric-reliable-actors-reentrancy.md) 詳細についてです。

### 同時実行の保証の範囲

アクター ランタイムは、これらのメソッドの呼び出しを制御する状況でこのような同時実行を保証します。 たとえば、クライアント要求の受信に対する応答として行われるメソッド呼び出しおよびタイマーとアラームのコールバックに対して、このような保証を提供します。 ただし、アクター コードがアクター ランタイムによって提供されるメカニズム以外でこれらのメソッドを直接呼び出す場合、ランタイムは同時実行を保証できません。 たとえば、メソッドが、アクター メソッドによって返されるタスクに関連付けられていない一部のタスクのコンテキストで呼び出される場合、またはアクターが独自に作成するスレッドから呼び出される場合、ランタイムは同時実行を保証することはできません。 そのため、バック グラウンド操作を実行するアクター使用する必要があります [アクターのタイマーまたはアクターのアラーム](service-fabric-reliable-actors-timers-reminders.md) 、ターンごとの同時実行を尊重します。
> [AZURE.TIP] Fabric アクター ランタイムは、いくつか出力 [同時実行制御に関連するイベントとパフォーマンス カウンター](service-fabric-reliable-actors-diagnostics.md#concurrency-events-and-performance-counters)します。 これらは、診断やパフォーマンスの監視に役立ちます。

## アクターの状態管理

Fabric アクターでは、ステートレスまたはステートフルなアクターを作成することができます。

### ステートレス アクター

派生したステートレス アクター、 `StatelessActor` 基本クラス、アクター ランタイムによって管理されている任意の状態はありません。 アクターのメンバー変数は、他の .NET 型と同じように、そのメモリ内の有効期間にわたって保持されます。 ただし、アクターはアイドル状態がしばらく続くとガベージ コレクトされるため、その状態は失われます。 同様に、フェールオーバーによって状態が失われる場合もあります。フェールオーバーは、アップグレード、リソース バランシング操作、またはアクター プロセスやアクター プロセスをホストするノードの障害によって発生します。

ステートレス アクターの例を次に示します。

```csharp
class HelloActor : StatelessActor, IHello
{
    public Task<string> SayHello(string greeting)
    {
        return Task.FromResult("You said: '" + greeting + "', I say: Hello Actors!");
    }
}
```

### ステートフル アクター

ステートフル アクターは、ガベージ コレクションとフェールオーバー全体で保持する必要がある状態を持つことです。派生する、 `StatefulActor < TState >`, ここで、 `TState` 保持する必要がある状態の種類です。状態を使用してアクター メソッドでアクセスできる `状態` 基本クラスのプロパティです。

状態にアクセスするステートフル アクターの例を次に示します。

```csharp
class VoicemailBoxActor : StatefulActor<VoicemailBox>, IVoicemailBoxActor
{
    public Task<List<Voicemail>> GetMessagesAsync()
    {
        return Task.FromResult(State.MessageList);
    }
    ...
}
```

アクターの状態はディスクに保持され、クラスター内の複数のノードにレプリケートされます。これにより、ガベージ コレクションおよびフェールオーバー全体で保持されます。 つまりなどのメソッドの引数と戻り値をアクター状態の型でなければなりません  [データ コントラクト シリアル化可能](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)します。
> [AZURE.NOTE] 参照してください、 [シリアル化に関する信頼性の高いアクター メモ](service-fabric-reliable-actors-notes-on-actor-type-serialization.md) インターフェイスおよびアクター状態の型の定義方法のモードの詳細については、資料です。  

#### アクター状態プロバイダー

状態の格納と取得はアクター状態プロバイダーによって提供されます。 状態プロバイダーは、状態プロバイダーの特定の属性を使用して、アクターごとまたはアセンブリ内のすべてのアクターに構成できます。 アクターがアクティブ化されている場合、その状態はメモリに読み込まれます。 アクター メソッドが完了すると、変更済みの状態は、状態プロバイダーに対してメソッドを呼び出すことで、アクター ランタイムによって自動的に保存されます。 保存中にエラーが発生した場合、アクター ランタイムは、新しいアクター インスタンスを作成し、最後の一貫性のある状態を状態プロバイダーから読み込みます。

既定では、ステートフル アクターはキー値ストアのアクター状態プロバイダーを使用します。この状態プロバイダーは、Service Fabric プラットフォームで提供される分散キー値ストアに構築されます。 詳細についてを参照してください、トピック「 [状態プロバイダーの選択](service-fabric-reliable-actors-platform.md#actor-state-provider-choices)します。
> [AZURE.TIP] アクター ランタイムは、いくつか出力 [アクター状態管理に関連するイベントとパフォーマンス カウンター](service-fabric-reliable-actors-diagnostics.md#actor-state-management-events-and-performance-counters)します。 これらは、診断やパフォーマンスの監視に役立ちます。

#### 読み取り専用メソッド

既定では、アクター ランタイムは、アクター メソッド呼び出し、タイマーのコールバック、またはアラームのコールバックの完了時にアクター状態を保存します。 状態の保存が完了するまでは、他のアクターの呼び出しは許可されません。

アクター メソッドで状態が変更されない場合があります。その場合、状態の保存にさらに時間がかかり、システムの全体的なスループットに影響する可能性があります。 これを回避するために、読み取り専用として、状態を変更しないメソッドとタイマーのコールバックをマークすることができます。

次の例は、アクター メソッドを使用して読み取り専用としてマークする方法を示しています。 `Readonly` 属性です。

```csharp
public interface IVoicemailBoxActor : IActor
{
    [Readonly]
    Task<List<Voicemail>> GetMessagesAsync();
}
```

タイマーのコールバックをマークすることができます、 `Readonly` 同様の方法で属性です。 アラームの場合で読み取り専用フラグを渡す引数として使用する、 `RegisterReminder` アラームを登録するために呼び出されるメソッド。

## 次のステップ

### 概念

[アクターのライフ サイクルとガベージ コレクション](service-fabric-reliable-actors-lifecycle.md)

[アクターのタイマーとアラーム](service-fabric-reliable-actors-timers-reminders.md)

[アクターのイベント](service-fabric-reliable-actors-events.md)

[アクターの再入](service-fabric-reliable-actors-reentrancy.md)

[Fabric アクターが Service Fabric プラットフォームを使用する方法](service-fabric-reliable-actors-platform.md)

[KVSActorStateProvider アクターの構成](service-fabric-reliable-actors-KVSActorstateprovider-configuration.md)

[アクターの診断とパフォーマンスの監視](service-fabric-reliable-actors-diagnostics.md)



[1]: ./media/service-fabric-reliable-actors-introduction/concurrency.png 

