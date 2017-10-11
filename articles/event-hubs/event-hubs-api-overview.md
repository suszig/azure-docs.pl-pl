---
title: "Omówienie usługi Azure API centra zdarzeń | Dokumentacja firmy Microsoft"
description: "Przegląd informacji o dostępnych interfejsach API centra zdarzeń platformy Azure"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3f221a0c-182d-4e39-9f3d-3a3c16c5c6ed
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 40cd76e1aacb68d6051cae4a3c90a8970f5449f0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="available-event-hubs-apis"></a>Interfejsy API centra zdarzeń dostępne

## <a name="runtime-apis"></a>Interfejsy API środowiska wykonawczego

Poniżej znajduje się opis wszystkich aktualnie dostępnych klientów środowiska uruchomieniowego usługi Azure Event Hubs. Chociaż niektóre z tych bibliotek zawiera również funkcje zarządzania ograniczone, dostępne są także [biblioteki określonej](#management-apis) przeznaczone do obsługi operacji zarządzania. Podstawowe tych bibliotek koncentruje się na wysyłanie i odbieranie komunikatów z Centrum zdarzeń.

Zobacz [dodatkowe informacje](#additional-information) więcej szczegółów na bieżący stan wszystkich bibliotek środowiska uruchomieniowego.

| Język/Platform | Pakiet klienta | EventProcessorHost pakietu | Repozytorium |
| --- | --- | --- | --- |
| .NET standard | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) | [GitHub](https://github.com/azure/azure-event-hubs-dotnet) |
| .NET framework | [NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) | Nie dotyczy |
| Java | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) | [GitHub](https://github.com/Azure/azure-event-hubs-java) |
| Węzeł | [NPM](https://www.npmjs.com/package/azure-event-hubs) | Nie dotyczy | [GitHub](https://github.com/Azure/azure-event-hubs-node) |
| C | Nie dotyczy | Nie dotyczy | [GitHub](https://github.com/Azure/azure-event-hubs-c) |

### <a name="additional-information"></a>Dodatkowe informacje

#### <a name="net"></a>.NET
Ekosystemu platformy .NET ma wiele środowisk uruchomieniowych, więc istnieje wiele bibliotek .NET dla usługi Event Hubs. Biblioteki standardowe .NET mogą być uruchamiane przy użyciu platformy .NET Core lub .NET Framework, gdy biblioteka programu .NET Framework może być uruchamiany tylko w środowisku .NET Framework. Aby uzyskać więcej informacji dotyczących platformy .NET Framework, zobacz [framework w wersji](https://docs.microsoft.com/dotnet/articles/standard/frameworks#framework-versions).

#### <a name="node"></a>Węzeł

Biblioteka języka Node.js jest obecnie w wersji zapoznawczej i są obsługiwane jako projekt po stronie przez pracowników firmy Microsoft i zewnętrznych współpracowników. Wszystkie udziały wraz z kodem źródłowym Zapraszamy i zostanie zweryfikowana.

## <a name="management-apis"></a>Interfejsy API Management

Poniżej znajduje się lista wszystkich obecnie zarządzania określone biblioteki. Żadna z tych bibliotek zawierają operacji środowiska uruchomieniowego i są wyłącznie w celu zarządzania jednostek usługi Event Hubs.

| Język/Platform | Pakiet zarządzania | Repozytorium |
| --- | --- | --- | --- |
| .NET standard | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [GitHub](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a>Następne kroki
Następujące linki pozwalają dowiedzieć się więcej na temat usługi Event Hubs:

* [Omówienie usługi Event Hubs](event-hubs-what-is-event-hubs.md)
* [Tworzenie centrum zdarzeń](event-hubs-create.md)
* [Event Hubs — często zadawane pytania](event-hubs-faq.md)