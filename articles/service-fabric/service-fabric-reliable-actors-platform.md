---
title: "Reliable Actors w sieci szkieletowej usług | Dokumentacja firmy Microsoft"
description: "Opisuje sposób Reliable Actors są warstwowy na niezawodne usługi i korzystać z funkcji platformy sieć szkieletowa usług."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: vturecek
ms.openlocfilehash: 43b3f758fe7017c0ec949ba6e28b76438cf1bc13
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a>Jak elementy Reliable Actors korzystają z platformy usługi Service Fabric
W tym artykule opisano, jak Reliable Actors działają na platformie Azure Service Fabric. Wywołuje Reliable Actors uruchomienia w ramach obsługiwanej w implementacji usługi stanowej niezawodnej *usługi aktora*. Usługa aktora zawiera składniki niezbędne do zarządzania cyklem życia i komunikat podczas wysyłania do sieci złośliwych użytkowników:

* Środowisko uruchomieniowe aktora zarządza cykl życia, wyrzucanie elementów bezużytecznych i wymusza jednowątkowe dostęp.
* Odbiornik komunikacji zdalnej usługi aktora akceptuje wywołań złośliwych użytkowników dostępu zdalnego i wysyła je do dyspozytora rozesłać do wystąpienia odpowiednie aktora.
* Dostawca stanu aktora opakowuje dostawców stanu (na przykład dostawcy stanu niezawodnej kolekcje) i udostępnia adapter dla zarządzania stanem aktora.

Te składniki tworzą razem framework niezawodnego aktora.

## <a name="service-layering"></a>Tworzenie warstw usługi
Ponieważ sama usługa aktora jest niezawodnej usługi, wszystkie [model aplikacji](service-fabric-application-model.md), cykl życia, [pakowania](service-fabric-package-apps.md), [wdrożenia](service-fabric-deploy-remove-applications.md), uaktualnianie i skalowanie pojęcia niezawodne usługi Zastosuj ten sam sposób, aby usługi aktora.

![Tworzenie warstw usługi aktora][1]

Na powyższym diagramie przedstawiono związek między struktur aplikacji usługi Service Fabric i kod użytkownika. Niebieski elementy reprezentują usługi niezawodnej struktury aplikacji, orange reprezentuje framework niezawodnego aktora i zielony reprezentuje kod użytkownika.

W usługach niezawodnej usługi dziedziczy `StatefulService` klasy. Ta klasa jest pochodną `StatefulServiceBase` (lub `StatelessService` usług bezstanowych). W Reliable Actors możesz użyć usługi aktora. Usługa aktora jest różne implementacje `StatefulServiceBase` klasy, która implementuje wzorca aktora, gdzie uruchomić sieci złośliwych użytkowników. Ponieważ sama usługa aktora jest po prostu implementacją `StatefulServiceBase`, można napisać własne usługi, która pochodzi z `ActorService` i implementuje funkcje na poziomie usługi tak samo jak w przypadku dziedziczenia `StatefulService`, takich jak:

* Usługa tworzenia kopii zapasowej i przywracania.
* Udostępnione funkcji dla wszystkich podmiotów, na przykład wyłącznika.
* Zdalne wywoływanie procedur w samej usługi aktora i na każdym poszczególnych aktora.

> [!NOTE]
> Stanowe usług nie są obecnie obsługiwane w języku Java/Linux.

### <a name="using-the-actor-service"></a>Przy użyciu usługi aktora
Wystąpienia aktora mają dostęp do usługi aktora, w którym jest uruchomiony. Za pośrednictwem usługi aktora wystąpień aktora programowo może uzyskać kontekstu usługi. Kontekst usługi zawiera identyfikator partycji, nazwę usługi, nazwa aplikacji i inne informacje specyficzne dla platformy Service Fabric:

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```
```Java
CompletableFuture<?> MyActorMethod()
{
    UUID partitionId = this.getActorService().getServiceContext().getPartitionId();
    String serviceTypeName = this.getActorService().getServiceContext().getServiceTypeName();
    URI serviceInstanceName = this.getActorService().getServiceContext().getServiceName();
    String applicationInstanceName = this.getActorService().getServiceContext().getCodePackageActivationContext().getApplicationName();
}
```


Podobnie jak wszystkie niezawodnej usługi usługi aktora musi być zarejestrowany w środowisku uruchomieniowym usługi sieć szkieletowa typ usługi. Dla usługi aktora do uruchomienia wystąpień z aktorem danego typu aktora musi być zarejestrowany w usłudze aktora. Metoda rejestracji `ActorRuntime` wykonuje tę pracę dla aktorów. Ogólnie rzecz biorąc można zarejestrować tylko danego typu aktora i usługi aktora z ustawieniami domyślnymi niejawnie będą używane:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>().GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```

Alternatywnie można użyć wyrażenia lambda udostępniane przez metodę rejestracji do utworzenia usługi aktora samodzielnie. Można następnie skonfigurować usługi aktora i utworzenia jawnie swoich wystąpień aktora, w którym można wstrzyknięcia zależności do Twojej aktora za pośrednictwem jej konstruktora:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new ActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
static class Program
{
    private static void Main()
    {
      ActorRuntime.registerActorAsync(
              MyActor.class,
              (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
              timeout);

        Thread.sleep(Long.MAX_VALUE);
    }
}
```

### <a name="actor-service-methods"></a>Metody usługi aktora
Implementuje usługi aktora `IActorService` (C#) lub `ActorService` (języka Java), który z kolei implementuje `IService` (C#) lub `Service` (Java). Jest to interfejs używany przez niezawodnych usług zdalnych, co pozwala na metody usług zdalnych wywołań procedur. Zawiera metody poziomu usług, które może zostać wywołana zdalnie za pomocą usług zdalnych.

#### <a name="enumerating-actors"></a>Wyliczanie złośliwych użytkowników
Usługa aktora umożliwia klientowi wyliczyć metadane dotyczące osób, które obsługuje usługę. Ponieważ usługa aktora jest partycjonowany usługi stanowej, wyliczenie jest wykonywane dla każdej partycji. Ponieważ każda partycja może zawierać wiele osób, wyliczenia jest zwracana jako zestaw wyników stronicowania. Strony są zwracane przez zapoznaniem wszystkich stron. Poniższy przykład przedstawia sposób tworzenia listy wszystkich podmiotów active w jednej partycji usługi aktora:

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```

#### <a name="deleting-actors"></a>Usuwanie złośliwych użytkowników
Usługa aktora udostępnia również funkcję usuwania złośliwych użytkowników:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

Aby uzyskać więcej informacji na temat usuwania złośliwych użytkowników i ich stan, zobacz [dokumentacji cyklu życia aktora](service-fabric-reliable-actors-lifecycle.md).

### <a name="custom-actor-service"></a>Usługa aktora niestandardowych
Za pomocą aktora lambda rejestracji, możesz zarejestrować własnej usługi aktora niestandardowej, która pochodzi z `ActorService` (C#) i `FabricActorService` (Java). W tej usłudze aktora niestandardowych można zaimplementować własnej funkcji poziomu usług pisząc klasy usługi, która dziedziczy `ActorService` (C#) lub `FabricActorService` (Java). Usługi aktora niestandardowych dziedziczy wszystkie funkcje środowiska uruchomieniowego aktora z `ActorService` (C#) lub `FabricActorService` (Java) i może służyć do wykonania metody usługi.

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```
```Java
class MyActorService extends FabricActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, BiFunction<FabricActorService, ActorId, ActorBase> newActor)
    {
         super(context, typeInfo, newActor);
    }
}
```

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new MyActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
        Thread.sleep(Long.MAX_VALUE);
    }
}
```

#### <a name="implementing-actor-backup-and-restore"></a>Implementowanie aktora kopii zapasowej i przywracania
 W poniższym przykładzie usługi aktora niestandardowych opisuje metodę kopii zapasowych danych aktora dzięki wykorzystaniu odbiornika usługi zdalne znajduje się już w `ActorService`:

```csharp
public interface IMyActorService : IService
{
    Task BackupActorsAsync();
}

class MyActorService : ActorService, IMyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }

    public Task BackupActorsAsync()
    {
        return this.BackupAsync(new BackupDescription(PerformBackupAsync));
    }

    private async Task<bool> PerformBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store the contents of backupInfo.Directory
           return true;
        }
        finally
        {
           Directory.Delete(backupInfo.Directory, recursive: true);
        }
    }
}
```
```Java
public interface MyActorService extends Service
{
    CompletableFuture<?> backupActorsAsync();
}

class MyActorServiceImpl extends ActorService implements MyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<FabricActorService, ActorId, ActorBase> newActor)
    {
       super(context, typeInfo, newActor);
    }

    public CompletableFuture backupActorsAsync()
    {
        return this.backupAsync(new BackupDescription((backupInfo, cancellationToken) -> performBackupAsync(backupInfo, cancellationToken)));
    }

    private CompletableFuture<Boolean> performBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store the contents of backupInfo.Directory
           return true;
        }
        finally
        {
           deleteDirectory(backupInfo.Directory)
        }
    }

    void deleteDirectory(File file) {
        File[] contents = file.listFiles();
        if (contents != null) {
            for (File f : contents) {
               deleteDirectory(f);
             }
        }
        file.delete();
    }
}
```


W tym przykładzie `IMyActorService` jest umowa komunikacji zdalnej, która implementuje `IService` (C#) i `Service` (języka Java), a następnie jest implementowany przez `MyActorService`. Dodanie tego kontraktu usługi zdalne metod na `IMyActorService` są teraz dostępne do klienta przez utworzenie zdalnym obiektem pośredniczącym za pośrednictwem `ActorServiceProxy`:

```csharp
IMyActorService myActorServiceProxy = ActorServiceProxy.Create<IMyActorService>(
    new Uri("fabric:/MyApp/MyService"), ActorId.CreateRandom());

await myActorServiceProxy.BackupActorsAsync();
```
```Java
MyActorService myActorServiceProxy = ActorServiceProxy.create(MyActorService.class,
    new URI("fabric:/MyApp/MyService"), actorId);

myActorServiceProxy.backupActorsAsync();
```

## <a name="application-model"></a>Model aplikacji
Usługi aktora są niezawodne usługi, więc model aplikacji jest taka sama. Jednak narzędzi kompilacji framework aktora Generowanie niektórych plików modelu aplikacji za Ciebie.

### <a name="service-manifest"></a>Manifest usługi
Narzędzia kompilacji framework aktora automatycznie generować zawartość pliku ServiceManifest.xml usługi aktora. Ten plik zawiera:

* Typ usługi aktora. Nazwa typu jest generowany na podstawie nazwy projektu z aktora. Na podstawie trwałości atrybutu na Twoje aktora, HasPersistedState zostanie również ustawiona flaga odpowiednio.
* Pakiet kodu.
* Pakiet konfiguracji.
* Zasoby i punktów końcowych.

### <a name="application-manifest"></a>Manifest aplikacji
Narzędzia kompilacji framework aktora automatyczne tworzenie definicji domyślnej usługi dla usługi aktora. Narzędzia kompilacji wypełniania właściwości usługi domyślne:

* Liczba zestawu replik jest określana przez atrybut trwałości na Twoje aktora. Zawsze, gdy atrybut trwałości na Twoje aktora została zmieniona, liczba zestawu replik w definicji domyślnej usługi zostanie zresetowany odpowiednio.
* Schemat partycji i zakres są ustawione na jednolitych Int64 z pełnego zakresu klucza Int64.

## <a name="service-fabric-partition-concepts-for-actors"></a>Pojęcia dotyczące partycji usługi sieć szkieletowa osób
Usługi aktora są podzielone na partycje usługi stanowej. Każda partycja usługi aktora zawiera zestaw złośliwych użytkowników. Partycji usługi są automatycznie dystrybuowane za pośrednictwem wiele węzłów w sieci szkieletowej usług. W związku z tym rozpowszechnianych wystąpień aktora.

![Aktora partycjonowania i dystrybucji][5]

Niezawodne usługi mogą być tworzone z różnych partycji, systemów i zakresami kluczy partycji. Usługa aktora wykorzystuje schemat partycjonowania Int64 z pełnego zakresu klucza Int64 do mapowania złośliwych użytkowników do partycji.

### <a name="actor-id"></a>Identyfikator aktora
Każdy aktora utworzony w usłudze ma unikatowy identyfikator skojarzony z nim reprezentowany przez `ActorId` klasy. `ActorId`to wartość Identyfikatora nieprzezroczyste, która może służyć do dystrybucji uniform podmiotów między partycji usługi przez Generowanie losowe identyfikatory:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Każdy `ActorId` jest wartość skrótu do postaci liczby Int64. Jest to, dlaczego usługa aktora musi używać schemat partycjonowania Int64 z pełnego zakresu klucza Int64. Jednak można używać niestandardowej wartości Identyfikatora dla `ActorID`, w tym identyfikatory GUID/UUID, ciągi i Int64s.

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

Podczas korzystania z identyfikatorów GUID/UUID i ciągi, wartości są wartość skrótu do postaci liczby Int64. Jednak gdy możesz jest jawnie dostarczanie Int64 do `ActorId`, Int64 przypisze bezpośrednio do partycji bez dalszego wyznaczania wartości skrótu. Można użyć tej metody do partycji, których podmioty są umieszczane w kontroli.

## <a name="actor-using-remoting-v2-stack"></a>Przy użyciu usług zdalnych V2 stosu aktora
Przy użyciu 2,8 pakietu nuget użytkownicy mogą teraz używać usług zdalnych V2 stosu, w którym jest więcej wydajności i udostępnia funkcje, takie jak niestandardowej serializacji. V2 usług zdalnych nie jest zgodny z istniejącego stosu komunikacji zdalnej (wywołania teraz go jako stosu komunikacji zdalnej V1).

Następujące zmiany muszą korzystać z usług zdalnych stosu V2.
 1. Dodaj następujący atrybut zestawu w interfejsach aktora.
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListener = RemotingListener.V2Listener,RemotingClient = RemotingClient.V2Client)]
   ```

 2. Projekty kompilacji i uaktualniania ActorService aktora klienta i aby rozpocząć korzystanie z stosu V2.

### <a name="actor-service-upgrade-to-remoting-v2-stack-without-impacting-service-availability"></a>Aktora usługi uaktualnienie do komunikacji zdalnej V2 stosu bez wpływu na dostępność usługi.
Ta zmiana będzie uaktualnienie krok 2. Wykonaj kroki opisane w tej samej sekwencji wymienione.

1.  Dodaj następujący atrybut zestawu w interfejsach aktora. Ten atrybut zostanie uruchomione dwa odbiorniki dla ActorService, V1 (istniejące) i odbiornika V2. Uaktualnij ActorService z tą zmianą.

  ```csharp
  [assembly:FabricTransportActorRemotingProvider(RemotingListener = RemotingListener.CompatListener,RemotingClient = RemotingClient.V2Client)]
  ```

2. Uaktualnij ActorClients po zakończeniu uaktualniania powyżej.
Ten krok zapewnia, że serwer Proxy aktora używa stosu V2 komunikacji zdalnej.

3. Ten krok jest opcjonalny. Zmień atrybut powyżej, aby usunąć odbiornik V1.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListener = RemotingListener.V2Listener,RemotingClient = RemotingClient.V2Client)]
    ```

## <a name="next-steps"></a>Następne kroki
* [Zarządzanie stanem aktora](service-fabric-reliable-actors-state-management.md)
* [Aktora cykl życia i odzyskiwanie pamięci](service-fabric-reliable-actors-lifecycle.md)
* [Dokumentacji interfejsu API złośliwych użytkowników](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [.NET przykładowy kod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java przykładowy kod](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
