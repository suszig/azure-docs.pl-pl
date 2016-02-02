<properties
   pageTitle="高信頼アクターのタイマーとアラーム"
   description="Service Fabric 高信頼アクターのタイマーとアラームの概要"
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2015"
   ms.author="amanbha"/>



# アクターのタイマー

アクターのタイマーは、アクターのランタイムによって提供されるターンごとの同時実行の保証をコールバック メソッドが考慮するように、.NET タイマーの単純なラッパーを提供します。

アクターが使用できる、 `RegisterTimer` と `UnregisterTimer` メソッドをその基本クラスを登録し、タイマーの登録を解除します。 次の例はタイマー API の使用を示します。 API は、.NET タイマーによく似ています。 タイマーの期限が次の例で、 `MoveObject` メソッドがアクター ランタイムによって呼び出され、ターンごとの同時実行、つまり他のアクターのメソッドまたはタイマーとアラームのコールバックがない進行中のこのコールバックまでの実行が完了を尊重することが保証されます。

```csharp
class VisualObjectActor : StatefulActor<VisualObject>, IVisualObject
{
    private IActorTimer _updateTimer;

    protected override Task OnActivateAsync()
    {
        ...

        _updateTimer = RegisterTimer(
            MoveObject,                     // callback method
            state,                          // state to pass to callback method
            TimeSpan.FromMilliseconds(15),  // amount of time to delay before callback is invoked
            TimeSpan.FromMilliseconds(15)); // time interval between invocation of the callback method

        return base.OnActivateAsync();
    }

    protected override Task OnDeactivateAsync()
    {
        if (_updateTimer != null)
        {
            UnregisterTimer(_updateTimer);
        }

        return base.OnDeactivateAsync();
    }

    private Task MoveObject(object state)
    {
        ...
        return Task.FromResult(true);
    }
}
```

タイマーの次の期間は、コールバックが実行を完了した後に開始します。 これは、コールバックを実行している間はタイマーが停止し、コールバックが完了したときにタイマーが開始することを意味します。

アクターのランタイムは、アクターが上記の例のようにステートフルなアクターの場合、コールバックが完了したときのアクターの状態を保存します。 状態の保存中にエラーが発生した場合、そのアクターのオブジェクトは非アクティブ化し、新しいインスタンスがアクティブ化されます。 アクターの状態を変更しないコールバック メソッドとして登録できます、読み取り専用のタイマーのコールバック タイマーのコールバックで読み取り専用の属性を指定することによって、セクションで説明したよう [読み取り専用メソッド](service-fabric-reliable-actors-introduction.md#readonly-methods)します。

ガベージ コレクションの一部としてアクターが非アクティブ化され、その後にタイマーのコールバックが呼び出されないと、すべてのタイマーが停止します。 また、アクターのランタイムは、非アクティブ化の前に実行されていたタイマーについての情報は保持しません。 その後、再アクティブ化したときに必要なタイマーを登録するかどうかはアクターによって異なります。 詳細についてを参照してください [アクターのガベージ コレクション](service-fabric-reliable-actors-lifecycle.md)します。

## アクターのアラーム

アラームは、指定した時間にアクターに永続的なコールバックをトリガーするメカニズムです。 機能はタイマーに似ていますが、タイマーとは異なり、アラームはアクターによって明示的に登録が解除されるまでのすべての状況下でトリガーされます。 具体的には、アラームはアクターの無効化およびフェールオーバーによりトリガーされます。これは、アクターのランタイムがアクターのアラームに関する情報を永続化するためです。

アラームは、ステートフル アクターのみでサポートされます。 ステートレス アクターは、アラームを使用できません。 アクターの状態プロバイダーは、アクターによって登録されているアラームに関する情報を保存する役割を担います。

アクターを呼び出してアラームを登録する、 `RegisterReminder` 次の例のように、基本クラスで提供されるメソッドです。

```csharp
string task = "Pay cell phone bill";
int amountInDollars = 100;
Task<IActorReminder> reminderRegistration = RegisterReminder(
                                                task,
                                                BitConverter.GetBytes(amountInDollars),
                                                TimeSpan.FromDays(3),
                                                TimeSpan.FromDays(1),
                                                ActorReminderAttributes.None);
```

上記の例で `「携帯電話料金を支払う」` はアラーム名であり、アクターがアラームを一意に識別に使用する文字列です。 `BitConverter.GetBytes(amountInDollars)` アラームに関連付けられているコンテキストします。 これはアクターに戻りますアラームのコールバックに渡す引数としてつまり `IRemindable.ReceiveReminderAsync`します。

アラームを使用するアクターを実装する必要があります `IRemindable` インターフェイスを次の例で示すようにします。

```csharp
public class ToDoListActor : StatefulActor<ToDoList>, IToDoListActor, IRemindable
{
    public Task ReceiveReminderAsync(string reminderName, byte[] context, TimeSpan dueTime, TimeSpan period)
    {
        if (reminderName.Equals("Pay cell phone bill"))
        {
            int amountToPay = BitConverter.ToInt32(context, 0);
            System.Console.WriteLine("Please pay your cell phone bill of ${0}!", amountToPay);
        }
        return Task.FromResult(true);
    }
}
```

Fabric アクター ランタイムが呼び出すアラームがトリガーされると、 `ReceiveReminderAsync` アクターのメソッドです。 アクターは、複数のアラームを登録でき、 `ReceiveReminderAsync` メソッドには、いつでもこれらの確認メッセージのいずれかがトリガーが呼び出されます。 アクターに渡されるアラーム名を使用できます、 `ReceiveReminderAsync` どのアラームがトリガーされたを確認するメソッドです。

ランタイムは、アクターを保存するアクター状態のときに、 `ReceiveReminderAsync` 呼び出しが完了します。 状態の保存中にエラーが発生した場合、そのアクターのオブジェクトは非アクティブ化し、新しいインスタンスがアクティブ化されます。 状態がアラームのコールバックの完了時に保存する必要がないかを指定する、 `ActorReminderAttributes.ReadOnly` フラグを設定できる、 `属性` パラメーターと、 `RegisterReminder` アラームを作成するメソッドが呼び出されます。

アラームの登録を解除する、 `UnregisterReminder` 次の例のように、メソッドを呼び出す必要があります。

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminder(reminder);
```

上のように、 `UnregisterReminder` メソッドは、 `IActorReminder` インターフェイスです。 アクターの基本クラスがサポートする、 `GetReminder` を取得するために使用するメソッド、 `IActorReminder` アラーム名で渡すことによって、インターフェイスです。 これは、アクターが永続化する必要がないので便利、 `IActorReminder` から返されたインターフェイス、 `RegisterReminder` メソッドの呼び出しです。





