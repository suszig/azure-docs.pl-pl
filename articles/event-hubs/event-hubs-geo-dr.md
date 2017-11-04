---
title: "Odzyskiwania po awarii na geograficznie w usłudze Azure Event Hubs | Dokumentacja firmy Microsoft"
description: "Jak używać regionów geograficznych do trybu failover i odzyskiwania po awarii w Azure Event Hubs"
services: event-hubs
documentationcenter: 
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: sethm
ms.openlocfilehash: 94c2782b3166fbc65ae755291a82a2a14556b96f
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/14/2017
---
# <a name="azure-event-hubs-geo-disaster-recovery-preview"></a>Odzyskiwanie po awarii geograficznie w usłudze Azure Event Hubs (wersja zapoznawcza)

Przestój regionalnych centrach danych jest krytyczne do przetwarzania danych w dalszym ciągu działać w innym regionie lub w centrum danych. W efekcie *odzyskiwania po awarii z magazynu geograficznie* i *— replikacja geograficzna* są ważne funkcje dla każdego przedsiębiorstwa. Usługa Azure Event Hubs obsługuje odzyskiwanie po awarii geograficznie i replikacja geograficzna, na poziomie przestrzeni nazw. 

Funkcja odzyskiwania po awarii geo usługi Azure Event hubs jest rozwiązanie odzyskiwania po awarii. Pojęcia i przepływ pracy opisany w tym artykule należy zastosować scenariuszy awarii, a nie przejściowego lub tymczasowych przestojów.

Szczegółowe omówienie odzyskiwania po awarii na platformie Microsoft Azure, zobacz [w tym artykule](/azure/architecture/resiliency/disaster-recovery-azure-applications). 

## <a name="terminology"></a>Terminologia

**Parowanie**: głównej przestrzeni nazw jest określany jako *active* i odbiera komunikaty. Przestrzeń nazw pracy awaryjnej jest *pasywnym* i nie odbiera wiadomości. Metadanych między nimi jest w synchronizacji, więc zarówno bezproblemowo zaakceptować wiadomości bez żadnych zmian kodu aplikacji. Ustanawianie konfiguracji odzyskiwania po awarii między region aktywnych i pasywnych region jest nazywany *parowanie* regionów.

**Alias**: Nazwa konfiguracji odzyskiwania po awarii, który można skonfigurować. Alias zawiera jeden ciąg połączenia stabilna pełni kwalifikowanej domeny nazwę (FQDN). Aplikacje Użyj tych parametrów połączenia alias nawiązać połączenia z przestrzenią nazw.

**Metadane**: odwołuje się do nazwy Centrum zdarzeń, grup odbiorców, partycji, jednostek przepływności, jednostki i właściwości, które są skojarzone z przestrzenią nazw.

## <a name="enable-geo-disaster-recovery"></a>Włącz odzyskiwanie po awarii geo

Można włączyć odzyskiwanie po awarii z magazynu geograficznie centra zdarzeń w krokach 3: 

1. Utwórz geograficznie parowanie, który tworzy alias parametry połączenia i udostępnia metadane na żywo replikacji. 
2. Aktualizowanie istniejących parametrów połączenia klienta do aliasu utworzonego w kroku 1.
3. Inicjuj tryb failover: parowanie geograficznie zostało zerwane i wskazuje alias do dodatkowej przestrzeni nazw jako jego nowej głównej przestrzeni nazw.

Na poniższej ilustracji przedstawiono tego przepływu pracy:

![Parowanie geograficznie przepływu][1] 

### <a name="step-1-create-a-geo-pairing"></a>Krok 1: tworzenie parowanie geo

Aby utworzyć połączenie między dwóch regionach, należy głównej przestrzeni nazw i dodatkowej przestrzeni nazw. Następnie można utworzyć aliasu tworzą pary geo. Po przestrzenie nazw są skojarzone z aliasem, metadane okresowo są replikowane w obu tych przestrzeni nazw. 

Poniższy kod przedstawia, jak to zrobić:

```csharp
ArmDisasterRecovery adr = await client.DisasterRecoveryConfigs.CreateOrUpdateAsync(
                                    config.PrimaryResourceGroupName,
                                    config.PrimaryNamespace,
                                    config.Alias,
                                    new ArmDisasterRecovery(){ PartnerNamespace = config.SecondaryNamespace});
```

### <a name="step-2-update-existing-client-connection-strings"></a>Krok 2: aktualizowanie istniejących parametrów połączenia klienta

Po zakończeniu parowanie z magazynu geograficznie należy zaktualizować parametry połączenia, które wskazują głównej przestrzeni nazw wskaż alias parametry połączenia. Uzyskaj parametry połączenia, jak pokazano w poniższym przykładzie:

```csharp
var accessKeys = await client.Namespaces.ListKeysAsync(config.PrimaryResourceGroupName,
                                                       config.PrimaryNamespace, "RootManageSharedAccessKey");
var aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
var aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;
```

### <a name="step-3-initiate-a-failover"></a>Krok 3: Zainicjuj tryb failover

W przypadku awarii lub jeśli zdecydujesz się Inicjuj tryb failover do dodatkowej przestrzeni nazw, metadane i dane start otrzymywanych dodatkowej przestrzeni nazw. Ponieważ aplikacje, użyj parametrów połączenia aliasu, żadne dalsze akcje nie jest wymagana, zgodnie z ich automatyczne uruchamianie odczytywanie z oraz zapisywanie do usługi event hubs w dodatkowej przestrzeni nazw. 

Poniższy kod przedstawia sposób włączania trybu failover:

```csharp
await client.DisasterRecoveryConfigs.FailOverAsync(config.SecondaryResourceGroupName,
                                                   config.SecondaryNamespace, config.Alias);
```

Po zakończeniu pracy awaryjnej i potrzebne dane występujące w głównej przestrzeni nazw, aby wyodrębnić dane należy użyć ciągu jawne połączenie do usługi event hubs w głównej przestrzeni nazw.

### <a name="other-operations-optional"></a>Inne operacje (opcjonalnie)

Można również Podziel parowanie geograficznie lub usunąć aliasu, jak pokazano w poniższym kodzie. Należy pamiętać, że można usunąć aliasu parametry połączenia, musisz przerwać geo paring:

```csharp
// Break pairing
await client.DisasterRecoveryConfigs.BreakPairingAsync(config.PrimaryResourceGroupName,
                                                       config.PrimaryNamespace, config.Alias);

// Delete alias connection string
// Before the alias is deleted, pairing must be broken
await client.DisasterRecoveryConfigs.DeleteAsync(config.PrimaryResourceGroupName,
                                                 config.PrimaryNamespace, config.Alias);
```

## <a name="considerations-for-public-preview"></a>Zagadnienia dotyczące publicznej wersji zapoznawczej

Należy uwzględnić następujące kwestie dotyczące tej wersji:

1. Możliwość odzyskiwania po awarii geograficzna jest dostępna tylko w regionach północno-środkowe Stany i południowo-środkowe Stany. 
2. Funkcja ta jest obsługiwana tylko w przypadku nowo utworzonego przestrzeni nazw.
3. W wersji zapoznawczej tylko metadane replikacja jest włączona. Rzeczywiste dane nie są replikowane.
4. Przy użyciu wersji zapoznawczej nie istnieje bezpłatna włączenie funkcji. Jednak serwera podstawowego i pomocniczego przestrzeni nazw będą naliczane opłaty jednostek zarezerwowaną przepływnością.

## <a name="next-steps"></a>Następne kroki

* [w witrynie GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) przeprowadzi Cię przez prosty przepływ pracy, który tworzy parowanie geograficznie i inicjuje trybu failover do scenariusza odzyskiwania po awarii.
* [Dokumentacji interfejsu API REST](/rest/api/eventhub/disasterrecoveryconfigs) opisuje interfejsy API do wykonywania konfiguracji odzyskiwania po awarii Geo.

Aby uzyskać więcej informacji na temat usługi Event Hubs, skorzystaj z następujących linków:

* Rozpocznij pracę od [samouczka dotyczącego usługi Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [Event Hubs — często zadawane pytania](event-hubs-faq.md)
* [Przykładowe aplikacje korzystające z usługi Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/eh-geodr1.png

