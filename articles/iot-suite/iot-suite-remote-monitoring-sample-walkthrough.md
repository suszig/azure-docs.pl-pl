---
title: "Architektura zdalnego rozwiązanie monitorowania - Azure | Dokumentacja firmy Microsoft"
description: "Przewodnik architektury zdalnego wstępnie skonfigurowane rozwiązanie monitorowania."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 6a90eb0eae6855cd0fbede65a024b5ff6490cce0
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# <a name="remote-monitoring-preconfigured-solution-architecture"></a>Zdalne monitorowanie wstępnie architektury rozwiązania

Pakiet IoT zdalne monitorowanie [wstępnie skonfigurowane rozwiązanie](iot-suite-what-are-preconfigured-solutions.md) implementuje end-to-end rozwiązanie monitorowania dla wielu komputerów w lokalizacjach zdalnych. Rozwiązanie to łączy najważniejsze usługi platformy Azure, aby umożliwić ogólną implementację scenariusza biznesowego. Za pomocą rozwiązania jako punkt początkowy dla własnego implementacji i [dostosować](iot-suite-remote-monitoring-customize.md) go zgodnie z wymaganiami firmy.

Ten artykuł przeprowadzi Cię przez niektóre kluczowe elementy rozwiązania do monitorowania zdalnego, aby umożliwić Ci zrozumienie jego sposobu działania. Ta wiedza ułatwi Ci:

* Rozwiązywanie problemów w rozwiązaniu.
* Planowanie sposobu dostosowywania rozwiązania, aby spełniało Twoje wymagania.
* Projektowanie własnego rozwiązania IoT korzystającego z usług Azure.

## <a name="logical-architecture"></a>Architektura logiczna

Poniższy diagram przedstawia logiczne składniki zdalnego monitorowania wstępnie skonfigurowane rozwiązanie umieszczenia na [architektury IoT](iot-suite-what-is-azure-iot.md):

![Architektura logiczna](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="why-microservices"></a>Dlaczego mikrousług?

Powstał architektury chmury, ponieważ firma Microsoft wydała pierwszy wstępnie skonfigurowanych rozwiązań. [Mikrousług](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) pojawiło się jako rozwiązaniem sprawdzonych osiągnąć skalowalność i elastyczność bez ograniczania programowanie szybkości. Kilka usług firmy Microsoft za pomocą tego wzorca architektury wewnętrznie dużą niezawodność i skalowalność wyników. Zaktualizowano wstępnie skonfigurowanych rozwiązań umieścić te learnings w praktyce również mogli korzystać z nich.

> [!TIP]
> Aby dowiedzieć się więcej na temat architektury mikrousług, zobacz [Architektura aplikacji .NET](https://www.microsoft.com/net/learn/architecture) i [Mikrousługi: rewolucja w aplikacjach wspierana przez chmurę](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="device-connectivity"></a>Łączność urządzeń

Rozwiązanie zawiera następujące składniki w części łączność urządzeń architektura logiczna:

### <a name="simulated-devices"></a>Symulowane urządzenia

Rozwiązanie zawiera mikrousługi, który umożliwia zarządzanie pulą symulowanego urządzenia do testowania przepływu end-to-end w rozwiązaniu. Symulowane urządzenie:

* Generowanie telemetrii urządzenia do chmury.
* Odpowiadanie na wywołania metody chmury do urządzenia z Centrum IoT.

Mikrousługi zawiera punkt końcowy RESTful umożliwiające tworzenie, uruchamianie i zatrzymywanie symulacji. Każdy symulacji składa się z wirtualnych urządzeń różnych typów, które wysyłania danych telemetrycznych i reagowanie na wywołania metody.

Można udostępnić symulowanego urządzenia z poziomu pulpitu nawigacyjnego w portalu rozwiązania.

### <a name="physical-devices"></a>Fizyczne urządzenia

Fizyczne urządzenia można podłączyć do rozwiązania. Można zaimplementować zachowanie symulowanego urządzenia za pomocą urządzenia Azure IoT zestawów SDK.

Można udostępnić fizyczne urządzenia z poziomu pulpitu nawigacyjnego w portalu rozwiązania.

### <a name="iot-hub-and-the-iot-manager-microservice"></a>Centrum IoT i mikrousługi Menedżera IoT

[Centrum IoT](../iot-hub/index.md) wysyła strumień danych wysyłanych z urządzenia do chmury i udostępnia go do `telemetry-agent` mikrousługi.

Usługa IoT Hub w rozwiązaniu wykonuje ponadto następujące czynności:

* Przechowuje rejestr tożsamości, który przechowuje identyfikatorów i klucze uwierzytelniania ze wszystkimi urządzeniami może łączyć się z portalu. Za pomocą rejestru tożsamości możesz włączać i wyłączać urządzenia.
* Wywołuje metody na Twoich urządzeniach w imieniu portalu rozwiązania.
* Obsługuje bliźniacze reprezentacje wszystkich zarejestrowanych urządzeń. W bliźniaczej reprezentacji urządzenia są przechowywane wartości właściwości zgłaszanych przez urządzenie. W bliźniaczej reprezentacji urządzenia są także przechowywane żądane właściwości ustawione w portalu rozwiązania, które mają zostać pobrane przez urządzanie podczas nawiązywania następnego połączenia.
* Planuje zadania ustawiania właściwości dla wielu urządzeń lub wywoływania metod na wielu urządzeniach.

Rozwiązanie zawiera `iot-manager` mikrousługi do obsługi interakcji z Centrum IoT, takich jak:

* Tworzenie i zarządzanie urządzeniami IoT.
* Zarządzanie twins urządzenia.
* Wywoływanie metody na urządzeniach.
* Zarządzanie poświadczeniami IoT.

Ta usługa działa również Centrum IoT zapytania, aby pobrać urządzeń należących do grup zdefiniowanych przez użytkownika.

Mikrousługi zapewnia RESTful punkt końcowy do zarządzania urządzeniami i twins urządzenia, wywołania metod i uruchamianie zapytań Centrum IoT.

## <a name="data-processing-and-analytics"></a>Przetwarzanie danych i analiza

Rozwiązanie zawiera następujące składniki przetwarzania danych i analiza część architektura logiczna:

### <a name="device-telemetry"></a>Telemetrii urządzenia

Rozwiązanie zawiera dwa mikrousług do obsługi telemetrii urządzenia.

[Agenta telemetrii](https://github.com/Azure/telemetry-agent-dotnet) mikrousługi:

* Przechowuje dane telemetryczne w bazie danych rozwiązania Cosmos.
* Analizuje strumienia danych telemetrycznych z urządzeń.
* Generuje alarmy zgodnie ze zdefiniowanymi regułami.

Alarmy są przechowywane w bazie danych rozwiązania Cosmos.

`telemetry-agent` Mikrousługi umożliwia portalu rozwiązania do odczytu telemetrii wysyłane z urządzenia. Portal rozwiązania używa także tej usługi:

* Definiowanie reguł monitorowania, takie jak progi, które mogą powodować alarmów
* Pobieranie listy ostatnich alarmów.

Punkt końcowy RESTful udostępniane przez to mikrousługi umożliwia zarządzanie telemetrii, reguł i alarmy.

### <a name="storage"></a>Magazyn

[Adaptera magazynu](https://github.com/Azure/pcs-storage-adapter-dotnet) mikrousługi jest kartą przed usługę głównego magazynu używane do wstępnie skonfigurowane rozwiązanie. Zapewnia prosty zbieranie i przechowywanie kluczy i wartości.

Standardowe wdrożenie wstępnie skonfigurowanego rozwiązania używa DB rozwiązania Cosmos jako jej usługi magazynu głównego.

Bazy danych DB rozwiązania Cosmos przechowuje dane w wstępnie skonfigurowanych rozwiązań. **Adaptera magazynu** mikrousługi działa jako adaptera dla innych mikrousług w rozwiązaniu dostęp do magazynu usług.

## <a name="presentation"></a>Prezentacji

Rozwiązanie zawiera następujące składniki w prezentacji części architektura logiczna:

[Interfejs użytkownika sieci web jest aplikacji React Javascript](https://github.com/Azure/pcs-remote-monitoring-webui). Aplikacja:

* Używa tylko Javascript React i uruchamia całkowicie w przeglądarce.
* Jest stylem CSS.
* Współdziała z publicznego mikrousług połączonej za pośrednictwem wywołania AJAX.

Interfejs użytkownika Wyświetla wszystkie funkcje wstępnie skonfigurowane rozwiązanie i współdziała z innymi usługami, takich jak:

* [Uwierzytelniania](https://github.com/Azure/pcs-auth-dotnet) mikrousługi do ochrony danych użytkownika.
* [Menedżera Centrum iothub](https://github.com/Azure/iothub-manager-dotnet) mikrousługi, aby wyświetlić listę i zarządzać urządzeniami IoT.

[Ui-config](https://github.com/Azure/pcs-config-dotnet) mikrousługi umożliwia przechowywanie i pobieranie ustawień konfiguracji za pomocą interfejsu użytkownika.

## <a name="next-steps"></a>Następne kroki

Aby zapoznać się z dokumentacji źródła kodu i deweloperów, z jednym Uruchom dwa główne repozytoriów GitHub:

* [Wstępnie skonfigurowane rozwiązanie do monitorowania zdalnego z IoT Azure (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/).
* [Wstępnie skonfigurowane rozwiązanie do monitorowania zdalnego za pomocą usługi Azure IoT (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java).
* [Wstępnie skonfigurowane rozwiązanie dla zdalnego monitorowania architektury)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Architecture).

Aby uzyskać więcej informacji pojęciach dotyczących zdalnego wstępnie skonfigurowane rozwiązanie monitorowania, zobacz [dostosować wstępnie skonfigurowane rozwiązanie](iot-suite-remote-monitoring-customize.md).
