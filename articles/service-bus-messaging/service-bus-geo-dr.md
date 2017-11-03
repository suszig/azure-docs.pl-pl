---
title: "Azure awarii usługi Service Bus geograficznie | Dokumentacja firmy Microsoft"
description: "Jak używać regionów geograficznych do trybu failover i odzyskiwania po awarii w Azure Service Bus"
services: service-bus-messaging
documentationcenter: 
author: christianwolf42
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: sethm
ms.openlocfilehash: 49f2992245d694f85b7b1f1c34339f1445c9d699
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="azure-service-bus-geo-disaster-recovery-preview"></a>Azure Service Bus geograficznie-odzyskiwaniem (wersja zapoznawcza)

Przestój regionalnych centrach danych jest krytyczne do przetwarzania danych w dalszym ciągu działać w innym regionie lub w centrum danych. W efekcie *odzyskiwania po awarii z magazynu geograficznie* i *— replikacja geograficzna* są ważne funkcje dla każdego przedsiębiorstwa. Usługa Azure Service Bus obsługuje odzyskiwania po awarii geograficznie i replikacja geograficzna, na poziomie przestrzeni nazw. 

Podgląd odzyskiwania po awarii geograficzna jest obecnie dostępna tylko w dwóch regionach (**północno-środkowe stany** i **południowo-środkowe stany)**.

## <a name="outages-and-disasters"></a>Awarie i awarii

[Najlepszych rozwiązań dotyczących izolacji aplikacji przed awariami usługi Service Bus i awarii](service-bus-outages-disasters.md) artykułu rozróżnia "awarie" i "awarii", które należy zwrócić uwagę. *Awarii* jest tymczasowej niedostępności usługi Azure Service Bus i może mieć wpływ na niektóre składniki usługi, takie jak magazynie obsługi komunikatów, lub nawet całe centrum danych. Jednak po problem został rozwiązany, usługi Service Bus znowu dostępne. Zazwyczaj awarii nie powoduje utraty wiadomości lub innych danych. Przykładem takich awarii może być awarii zasilania w centrum danych.

A *po awarii* jest zdefiniowany jako stały lub długoterminowego utraty usługi Service Bus [jednostki skalowania](service-bus-architecture.md#service-bus-scale-units) lub datacenter. Centrum danych może nie może stać się dostępne ponownie lub może nie działać przez kilka godzin lub dni. Przykładami takich awarii są fire, zalewania lub trzęsienie ziemi. Po awarii, które staje się stałe może spowodować utratę niektórych komunikatów lub innych danych. Jednak w większości przypadków nie powinny istnieć bez utraty danych i komunikaty mogą zostać odzyskane po centrum danych kopii zapasowych.

Funkcja odzyskiwania po awarii geograficznie Azure Service Bus jest rozwiązanie odzyskiwania po awarii. Pojęcia i przepływ pracy opisany w tym artykule należy zastosować scenariuszy awarii, a nie przejściowego lub tymczasowych przestojów.  

## <a name="basic-concepts-and-terms"></a>Podstawowych pojęć i terminów

Funkcja odzyskiwania po awarii implementuje odzyskiwania po awarii metadanych i opiera się na podstawowym i nazw odzyskiwania po awarii dodatkowej. Należy pamiętać, że funkcja odzyskiwania po awarii geograficznie dostępne dla [przestrzeniach nazw warstwy Premium](service-bus-premium-messaging.md) tylko. Nie trzeba wprowadzić zmiany w ciągu połączenia, ponieważ połączenie jest realizowane za pomocą aliasu.

Poniższe terminy są używane w tym artykule:

-  *Alias*: parametry połączenia głównego.

-  *Przestrzeń nazw podstawowe i pomocnicze*: w tym artykule opisano przestrzenie nazw, które odpowiadają alias. Podstawową jest "aktywny" i odbiera komunikaty, pomocniczy jest "pasywny" i nie odbiera wiadomości. Metadanych między nimi jest w synchronizacji, więc zarówno bezproblemowo zaakceptować wiadomości bez żadnych zmian kodu aplikacji.

-  *Metadane*: Z reprezentacji obiektów w Azure Service Bus. Aktualnie obsługiwany jest tylko metadane.

-  *Tryb failover*: proces aktywacji dodatkowej przestrzeni nazw. Ściągać komunikaty z wcześniej głównej przestrzeni nazw, gdy staną się dostępne ponownie, a następnie usuń przestrzeń nazw. Aby utworzyć innego trybu failover, należy dodać nowej dodatkowej przestrzeni nazw do powiązań. Ponowne użycie poprzedniego głównej przestrzeni nazw, po przejściu w tryb failover, należy najpierw usunąć wszystkich istniejących jednostek z przestrzeni nazw. Upewnij się, że wszystkie komunikaty przedtem.

## <a name="failover-workflow"></a>Przepływ pracy trybu failover

Poniższa sekcja zawiera omówienie cały proces konfigurowania początkowej trybu failover i jak przejść z tego punktu.

![1][]

Należy najpierw Konfigurowanie podstawowego i pomocniczego przestrzeni nazw, a następnie utworzyć parowania. Połączenie umożliwia alias, który służy do połączenia. Ponieważ używasz aliasu, nie trzeba zmienić parametry połączenia. Tylko nowych przestrzeni nazw można dodać do Twojej pracy awaryjnej parowania. Na koniec należy dodać logikę wyzwalacza (na przykład niektóre logiki biznesowej, które wykrywa, czy przestrzeń nazw nie jest dostępny i inicjuje przejście w tryb failover). Można sprawdzić za pomocą dostępności przestrzeni nazw [przeglądania wiadomości](message-browsing.md) możliwości usługi Service Bus.

Po skonfigurowaniu zarówno monitorowania i odzyskiwania po awarii można przyjrzeć się proces trybu failover. Jeśli wyzwalacz inicjuje trybu failover, lub ręcznie zainicjować trybu failover, wymagane są dwa kroki:

1. W przypadku awarii innego chcesz mieć możliwość pracy awaryjnej w ponownie. W związku z tym ustawienie drugi pasywnym przestrzeni nazw i zaktualizuj parowanie. 
2. Ściągać komunikaty z wcześniej głównej przestrzeni nazw, po udostępnieniu nowej przestrzeni nazw. Po wykonaniu tej ponownego użycia lub Usuń stare głównej przestrzeni nazw.

![2][]

## <a name="set-up-disaster-recovery"></a>Konfigurowanie odzyskiwania po awarii

Ta sekcja zawiera opis sposobu tworzenia własnego kodu odzyskiwania Geo usługi Service Bus — po awarii. Aby to zrobić, należy dwie przestrzenie nazw w niezależnie od lokalizacji; na przykład Południowe Stany i północno-środkowe Stany. W poniższym przykładzie użyto programu Visual Studio 2017 r.

1.  Utwórz nową **App(.Net Framework) konsoli** projekt w programie Visual Studio i nadaj mu nazwę, na przykład **SBGeoDR**.

2.  Zainstaluj następujące pakiety NuGet:
    1.  Microsoft.IdentityModel.Clients.ActiveDirectory
    2.  Microsoft.Azure.Management.ServiceBus

3. Upewnij się, że wersja używanego pakietu Newtonsoft.Json NuGet jest 10.0.3.

3.  Dodaj następujące `using` instrukcje w kodzie:

    ```csharp
    using System.Threading;
    using Microsoft.Azure.Management.ServiceBus;
    using Microsoft.Azure.Management.ServiceBus.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

4. Modyfikowanie użytkownika `main()` metody w celu dodania dwóch przestrzeniach nazw warstwy premium:

    ```csharp
    // 1. Create primary namespace (optional).

    var namespaceParams = new SBNamespace()
    {
        Location = "South Central US",
        Sku = new SBSku()
        {
            Name = SkuName.Premium,
            Capacity = 1
        }

    };

    var namespace1 = client.Namespaces.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, namespaceParams);

    // 2. Create secondary namespace (optional if you already have an empty namespace available).

    var namespaceParams2 = new SBNamespace()
    {
        Location = "North Central US",
        Sku = new SBSku()
        {
            Name = SkuName.Premium,
            Capacity = 1
        }

    };

    // If you re-run this program while namespaces are still paired this operation will fail with a bad request.
    // This is because we block all updates on secondary namespaces once it is paired.

    var namespace2 = client.Namespaces.CreateOrUpdate(resourceGroupName, geoDRSecondaryNS, namespaceParams2);
    ```

5. Włącz połączenie między dwoma obszarami nazw i uzyskać alias, który później użyć do nawiązania połączenia obiekty:

    ```csharp
    // 3. Pair the namespaces to enable DR.

    ArmDisasterRecovery drStatus = client.DisasterRecoveryConfigs.CreateOrUpdate(
        resourceGroupName,
        geoDRPrimaryNS,
        alias,
        new ArmDisasterRecovery { PartnerNamespace = geoDRSecondaryNS });

    // A similar loop can be used to check if other operations (Failover, BreakPairing, Delete) 
    // mentioned below have been successful.
    while (drStatus.ProvisioningState != ProvisioningStateDR.Succeeded)
    {
        Console.WriteLine("Waiting for DR to be set up. Current state: " +
        drStatus.ProvisioningState);
        drStatus = client.DisasterRecoveryConfigs.Get(
        resourceGroupName,
        geoDRPrimaryNS,
        alias);

        Thread.CurrentThread.Join(TimeSpan.FromSeconds(30));
    }
    ```

Pomyślnie skonfigurowano dwie pary nazw. Teraz można utworzyć jednostki, aby przyjrzeć się synchronizacja metadanych. Jeśli chcesz wykonać trybu failover, natychmiast po pozwolić trochę czasu zsynchronizować metadane. Można dodać czas uśpienia krótkie, na przykład:

```csharp
client.Topics.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, "myTopic", new SBTopic());
client.Subscriptions.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, "myTopic", "myTopic-Sub1", new SBSubscription());

// sleeping to allow metadata to sync across primary and secondary
Thread.Sleep(1000 * 60);
```

W tym momencie można dodawać jednostek w portalu lub za pośrednictwem usługi Azure Resource Manager i zobacz, jak zsynchronizować. Jeśli plan jest ręcznie do trybu failover, należy utworzyć aplikację, która monitoruje głównej przestrzeni nazw i inicjuje trybu failover, jeśli stanie się niedostępny. 

## <a name="initiate-a-failover"></a>Zainicjuj tryb failover

Poniższy kod przedstawia sposób Zainicjuj tryb failover:

```csharp
// Note that this failover operation is always run against the secondary namespace 
// (because primary might be down at time of failover).

client.DisasterRecoveryConfigs.FailOver(resourceGroupName, geoDRSecondaryNS, alias);
```

Po wyzwolić tryb failover, Dodawanie nowej przestrzeni nazw pasywnego i ponownie nawiąż połączenie. Kod, aby utworzyć nowe połączenie, jest wyświetlany w poprzedniej sekcji. Ponadto należy usunąć wiadomości z starego głównej przestrzeni nazw, po zakończeniu pracy awaryjnej. Przykłady tego, jak odbierać komunikaty z kolejki, zobacz [Rozpoczynanie pracy z kolejkami](service-bus-dotnet-get-started-with-queues.md).

## <a name="how-to-disable-geo-disaster-recovery"></a>Jak wyłączyć odzyskiwanie po awarii Geo

Poniższy kod przedstawia sposób wyłączania sparowania przestrzeni nazw:

```csharp
client.DisasterRecoveryConfigs.BreakPairing(resourceGroupName, geoDRPrimaryNS, alias);
```

Poniższy kod usuwa alias, który został utworzony:

```csharp
// Delete the DR config (alias).
// Note that this operation must run against the namespace to which the alias is currently pointing.
// If you break the pairing and want to delete the namespaces afterwards, you must delete the alias first.

client.DisasterRecoveryConfigs.Delete(resourceGroupName, geoDRPrimaryNS, alias);
```

## <a name="steps-after-a-failover-failback"></a>Czynności po przejściu w tryb failover (powrót po awarii)

Po przejściu w tryb failover wykonaj następujące dwa kroki:

1.  Tworzenie nowego pasywnym dodatkowej przestrzeni nazw. Kod jest wyświetlany w poprzedniej sekcji.
2.  Usuń pozostałe wiadomości z kolejki.

## <a name="alias-connection-string-and-test-code"></a>Alias połączenia ciągu i testowania kodu

Jeśli chcesz przetestować proces trybu failover, można zapisać przykładowej aplikacji, który wypycha wiadomości do głównej przy użyciu aliasu przestrzeni nazw. Aby to zrobić, upewnij się, uzyskanie alias parametry połączenia z aktywnej przestrzeni nazw. Przy użyciu bieżącej wersji zapoznawczej nie istnieje żadne inne interfejsu bezpośrednio uzyskać ciąg połączenia. Poniższy przykładowy kod łączy przed i po pracy awaryjnej:

```csharp
var accessKeys = client.Namespaces.ListKeys(resourceGroupName, geoDRPrimaryNS, "RootManageSharedAccessKey");
var aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
var aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;

if(aliasPrimaryConnectionString == null)
{
    accessKeys = client.Namespaces.ListKeys(resourceGroupName, geoDRSecondaryNS, "RootManageSharedAccessKey");
    aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
    aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;
}
```

## <a name="next-steps"></a>Następne kroki

- Zobacz odzyskiwania po awarii geograficznie [tutaj dokumentacji interfejsu API REST](/rest/api/servicebus/disasterrecoveryconfigs).
- Uruchom odzyskiwanie po awarii geograficznie [w witrynie GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2).
- Zobacz odzyskiwania po awarii geograficznie [przykład, który wysyła wiadomości do aliasu](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1).

Aby dowiedzieć się więcej na temat komunikatów usługi Service Bus, zobacz następujące artykuły:

* [Podstawy usługi Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Kolejki, tematy i subskrypcje usługi Service Bus](service-bus-queues-topics-subscriptions.md)
* [Wprowadzenie do kolejek usługi Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak używać tematów i subskrypcji usługi Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [Interfejs API REST](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geo1.png
[2]: ./media/service-bus-geo-dr/geo2.png
