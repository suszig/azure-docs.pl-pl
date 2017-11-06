---
title: "Zarządzanie stanem Reliable Actors | Dokumentacja firmy Microsoft"
description: "Opisuje sposób Reliable Actors stanu jest zarządzany, utrwalenia i replikacji wysokiej dostępności."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: fae68c9fb40951e3f7a6fce67d75872cecfc52bd
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="reliable-actors-state-management"></a>Zarządzanie stanem podmiotów niezawodnej
Reliable Actors są jednowątkowych obiektów hermetyzacji zarówno logiki i stanu. Ponieważ podmioty są uruchamiane na niezawodne usługi, ich niezawodnie zachowują stan, przy użyciu tego samego stanu trwałego i mechanizmów replikacji, które używa niezawodne usługi. W ten sposób złośliwych użytkowników nie utracili ich stanu po awariach po ponownej aktywacji po wyrzucanie elementów bezużytecznych lub po przeniesieniu ich między węzłami w klastrze równoważenia zasobów lub uaktualnień.

## <a name="state-persistence-and-replication"></a>Trwałość stanu i replikacji
Uwzględniane są wszystkie Reliable Actors *stateful* ponieważ każde wystąpienie aktora jest mapowany na unikatowy identyfikator. Oznacza to, że ten sam identyfikator aktora powtarzane wywołania są kierowane do tego samego wystąpienia aktora. W systemie bezstanowych natomiast wywołań klienta nie ma gwarancji być kierowane do tego samego serwera zawsze. Z tego powodu usługi aktora są zawsze usług stanowych.

Mimo że podmioty są traktowane jako wartość, która oznacza to, że ich musi niezawodne przechowywanie stanu. Złośliwych użytkowników można wybrać poziom trwałości stanu i replikacji na podstawie ich danych wymagania dotyczące magazynu:

* **Stan utrwalony**: stan jest zachowywany na dysku i jest replikowany do replik 3 lub więcej. Jest to najbardziej niezawodna opcji magazynu stanu gdzie stan może się powtarzać, za pośrednictwem awarii całego klastra.
* **Stan volatile**: stan jest replikowana do replik 3 lub więcej i tylko przechowywany w pamięci. Zapewnia odporność względem awarii węzła i niepowodzenie aktora oraz podczas uaktualniania i równoważenia zasobów. Jednak stan nie jest zachowywany na dysku. Jeśli więc wszystkie repliki zostaną utracone na raz, stan utracone również.
* **Nie stanu utrwalonego**: stan nie jest replikowany lub zapisany na dysku. Ten poziom jest osób, które nie wymagają po prostu do zarządzania stanem niezawodnie.

Każdy poziom trwałości jest po prostu inną *dostawcy stanu* i *replikacji* konfiguracji usługi. Określa, czy stan jest zapisywany na dysku jest zależna od dostawcy stanu — składnik w niezawodnej usługi, która przechowuje stan. Zależy od liczby replikami usługa jest wdrażana z replikacji. Podobnie jak w przypadku niezawodne usługi dostawcy stanu i liczby replik można łatwo skonfigurować ręcznie. Framework aktora udostępnia atrybut, gdy używany na aktora, automatycznie wybiera domyślny dostawca stanu i automatycznie generuje ustawienia liczby replik osiągnięcie jednego z tych trzech ustawień trwałości. Atrybut StatePersistence nie jest dziedziczone przez klasy pochodnej, podać jego poziom StatePersistence każdego typu aktora.

### <a name="persisted-state"></a>Stan utrwalony
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl  extends FabricActor implements MyActor
{
}
```  
To ustawienie powoduje użycie dostawcy stanu, która przechowuje dane na dysku i automatycznie ustawia liczbę repliki usługi do 3.

### <a name="volatile-state"></a>Stan nietrwałe
```csharp
[StatePersistence(StatePersistence.Volatile)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Volatile)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
To ustawienie korzysta z dostawcy stanu w pamięci wyłącznie- i ustawia liczby replik 3.

### <a name="no-persisted-state"></a>Nie stanu utrwalonego
```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.None)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
To ustawienie korzysta z dostawcy stanu w pamięci — tylko i ustawia liczbę replik równą 1.

### <a name="defaults-and-generated-settings"></a>Wygenerowany ustawienia i wartości domyślnych
Jeśli używasz `StatePersistence` atrybutu, dostawcy stanu jest automatycznie wybrana w czasie wykonywania podczas uruchamiania usługi aktora. Liczba replik, jednak jest ustawiony w czasie kompilacji za pomocą narzędzi Visual Studio aktora kompilacji. Narzędzia kompilacji automatycznego generowania *usługi domyślnej* usługi aktora w pliku ApplicationManifest.xml. Parametry są tworzone dla **rozmiar zestawu replik min** i **rozmiar zestawu replik docelowej**.

Te parametry można zmienić ręcznie. Ale zawsze `StatePersistence` zmiany atrybutu, parametry zostaną ustawione na wartości domyślne rozmiar zestawu replik dla wybranego `StatePersistence` atrybutu zastąpienie poprzedniej wartości. Innymi słowy, są ustawiane w pliku ServiceManifest.xml wartości *tylko* zastąpione w czasie kompilacji zmiana `StatePersistence` wartość atrybutu.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="MyActorService_PartitionCount" DefaultValue="10" />
      <Parameter Name="MyActorService_MinReplicaSetSize" DefaultValue="3" />
      <Parameter Name="MyActorService_TargetReplicaSetSize" DefaultValue="3" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyActorPkg" ServiceManifestVersion="1.0.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MyActorService" GeneratedIdRef="77d965dc-85fb-488c-bd06-c6c1fe29d593|Persisted">
         <StatefulService ServiceTypeName="MyActorServiceType" TargetReplicaSetSize="[MyActorService_TargetReplicaSetSize]" MinReplicaSetSize="[MyActorService_MinReplicaSetSize]">
            <UniformInt64Partition PartitionCount="[MyActorService_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
         </StatefulService>
      </Service>
   </DefaultServices>
</ApplicationManifest>
```

## <a name="state-manager"></a>Menedżer stanu
Każde wystąpienie aktora ma własną Menedżer stanu: struktura typu słownika danych, która w niezawodny sposób przechowuje par klucz/wartość. Menedżer stanów jest otokę dostawcy stanu. Można go użyć do przechowywania danych, niezależnie od tego, które jest używane ustawienie trwałości. Nie ma żadnej gwarancji, że można zmienić uruchomionej usługi aktora z ustawienie volatile (w pamięci —) stanie tylko do ustawienia stanu utrwalonego za pomocą uaktualnienia stopniowego przy zachowaniu danych. Istnieje możliwość zmiany liczby replik dla uruchomionej usługi.

Menedżer stanu klucze muszą być ciągami. Wartości są ogólne i mogą być dowolnego typu, łącznie z niestandardowych typów. Wartości przechowywane w Menedżerze stanu musi być kontraktu danych, które można serializować, ponieważ mogą być przesyłane przez sieć do innych węzłów podczas replikacji i mogą być zapisywane na dysku, w zależności od ustawienia trwałości stanu aktora.

Menedżer stanu przedstawia stan podobne do tych znaleziony w słowniku niezawodnej zarządzania typowe metody słownika.

### <a name="accessing-state"></a>Uzyskiwanie dostępu do stanu
Stan jest możliwy za pośrednictwem Menedżera stanu według klucza. Metody menedżera stanu są wszystkie asynchronicznego, ponieważ może wymagać We/Wy dysku, gdy podmiotów utrwalonych stanu. Po pierwszym dostępie obiekty stanu są buforowane w pamięci. Powtórz uzyskiwanie dostępu do operacji dostępu do obiektów bezpośrednio z pamięci i zwracany synchronicznie, bez ponoszenia We/Wy dysku lub asynchroniczne przełączania kontekstu. Obiekt stanu zostanie usunięty z pamięci podręcznej w następujących przypadkach:

* Metoda aktora zgłasza nieobsługiwany wyjątek, po jego pobiera obiekt przez menedżera stanu.
* Uaktywnieniu aktora, po dezaktywowany lub po awarii.
* Dostawca stanu strony stanu na dysku. To zachowanie zależy od implementacji dostawcy stanu. Domyślny dostawca stanu dla `Persisted` ma to zachowanie.

Stan można pobrać za pomocą standardowego *uzyskać* operacja, która zgłasza `KeyNotFoundException`(C#) lub `NoSuchElementException`(Java), jeśli wpis nie istnieje dla klucza:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<int> GetCountAsync()
    {
        return this.StateManager.GetStateAsync<int>("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().getStateAsync("MyState");
    }
}
```

Stan można również pobrać za pomocą *TryGet* metodę, która nie zgłasza, jeśli wpis nie istnieje dla klucza:

```csharp
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task<int> GetCountAsync()
    {
        ConditionalValue<int> result = await this.StateManager.TryGetStateAsync<int>("MyState");
        if (result.HasValue)
        {
            return result.Value;
        }

        return 0;
    }
}
```
```Java
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().<Integer>tryGetStateAsync("MyState").thenApply(result -> {
            if (result.hasValue()) {
                return result.getValue();
            } else {
                return 0;
            });
    }
}
```

### <a name="saving-state"></a>Zapisywanie stanu
Metody pobierania stanu Menedżera zwraca odwołanie do obiektu w pamięci lokalnej. Modyfikowanie tego obiektu w pamięci lokalnej wyłącznie nie powoduje jego zapisania trwałym. Gdy obiekt jest pobierane z Menedżera stanu i modyfikować, należy ponownie wstawić do przez menedżera stanu można zapisać trwałym.

Stan można wstawić za pomocą bezwarunkowe *ustawić*, który jest odpowiednikiem `dictionary["key"] = value` składni:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task SetCountAsync(int value)
    {
        return this.StateManager.SetStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture setCountAsync(int value)
    {
        return this.stateManager().setStateAsync("MyState", value);
    }
}
```

Stan można dodać za pomocą *Dodaj* metody. Ta metoda zgłasza `InvalidOperationException`(C#) lub `IllegalStateException`(Java) próbuje dodać klucz już istnieje.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task AddCountAsync(int value)
    {
        return this.StateManager.AddStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().addOrUpdateStateAsync("MyState", value, (key, old_value) -> old_value + value);
    }
}
```

Stan można również dodać za pomocą *TryAdd* metody. Ta metoda nie zgłasza, gdy próbuje dodać klucz już istnieje.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task AddCountAsync(int value)
    {
        bool result = await this.StateManager.TryAddStateAsync<int>("MyState", value);

        if (result)
        {
            // Added successfully!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().tryAddStateAsync("MyState", value).thenApply((result)->{
            if(result)
            {
                // Added successfully!
            }
        });
    }
}
```

Na końcu metody aktora Menedżer stanu automatycznie zapisuje wartości, które zostały dodane lub zmodyfikowane przez operacji insert lub update. "Zapisz" może zawierać przechowywanie na dysku i replikacji, w zależności od ustawienia używane. Wartości, które nie zostały zmodyfikowane nie są zachowywane lub replikowane. Jeśli wartości nie zostały zmodyfikowane, Zapisz działania nie działają. Jeśli zapiszesz kończy się niepowodzeniem, zostanie odrzucony stanu modyfikacji i załadowaniu oryginalnego stanu.

Można także zapisać stan ręcznie przez wywołanie metody `SaveStateAsync` metody na podstawie aktora:

```csharp
async Task IMyActor.SetCountAsync(int count)
{
    await this.StateManager.AddOrUpdateStateAsync("count", count, (key, value) => count > value ? count : value);

    await this.SaveStateAsync();
}
```
```Java
interface MyActor {
    CompletableFuture setCountAsync(int count)
    {
        this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value).thenApply();

        this.stateManager().saveStateAsync().thenApply();
    }
}
```

### <a name="removing-state"></a>Usuwanie stanu
Możesz usunąć stan trwale z Menedżer stanu aktora wywołując *Usuń* metody. Ta metoda zgłasza `KeyNotFoundException`(C#) lub `NoSuchElementException`(Java) podczas próby usunięcia klucz, który nie istnieje.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task RemoveCountAsync()
    {
        return this.StateManager.RemoveStateAsync("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().removeStateAsync("MyState");
    }
}
```

Można również usunąć stan trwale przy użyciu *TryRemove* metody. Ta metoda nie zgłasza podczas próby usunięcia klucza, który nie istnieje.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task RemoveCountAsync()
    {
        bool result = await this.StateManager.TryRemoveStateAsync("MyState");

        if (result)
        {
            // State removed!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().tryRemoveStateAsync("MyState").thenApply((result)->{
            if(result)
            {
                // State removed!
            }
        });
    }
}
```

## <a name="next-steps"></a>Następne kroki

Stan, który jest przechowywany w Reliable Actors musi być serializowany przed zapisany na dysku i replikowany wysokiej dostępności. Dowiedz się więcej o [serializacji typu aktora](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

Następnie Dowiedz się więcej o [aktora Diagnostyka i monitorowanie wydajności](service-fabric-reliable-actors-diagnostics.md).
