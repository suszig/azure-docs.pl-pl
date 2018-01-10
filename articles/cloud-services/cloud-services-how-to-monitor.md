---
title: "Monitorowanie usługi w chmurze platformy Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano obejmuje co monitorowanie usługi w chmurze platformy Azure i jakie niektóre opcje są."
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: adegeo
ms.openlocfilehash: c63a49c65f2d8261caa534308477888c752a89da
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="introduction-to-cloud-service-monitoring"></a>Wprowadzenie do monitorowania usługi w chmurze

Można monitorować kluczowe metryki wydajności dla usługi w chmurze. Minimalna ilość danych zbiera co roli usługi w chmurze: użycia Procesora, wykorzystanie sieci i wykorzystania dysku. Jeśli usługa w chmurze ma `Microsoft.Azure.Diagnostics` rozszerzenia stosowane do roli, tej roli mogą zbierać dodatkowe punktów danych. Ten artykuł zawiera wprowadzenie do diagnostyki Azure dla usługi w chmurze.

Podstawowe monitorowanie danych licznika wydajności z wystąpień roli jest próbkowany i zbierane co 3 minut. To podstawowe dane monitorowania nie są przechowywane na koncie magazynu i bez dodatkowych kosztów skojarzone z nim.

Zaawansowane monitorowanie dodatkowe metryki są pobierane i pobierane częstotliwością wynoszącą 5 minut, godzinę i 12 godzin. Zagregowane dane są przechowywane na koncie magazynu, w tabelach i są przeczyszczane po 10 dni. Skonfigurowano konto magazynu używane przez rolę; można użyć różnych kont magazynu dla różnych ról. Te ustawienia zostaną skonfigurowane z użyciem parametrów połączenia w [csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) i [.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) plików.


## <a name="basic-monitoring"></a>Podstawowe monitorowanie

Zgodnie z wprowadzeniem, usługa w chmurze automatycznie zbiera podstawowe dane monitorowania z hosta maszyny wirtualnej. Te dane obejmują: procent procesora CPU, we/wy sieci i dysku odczytu/zapisu. Zebrane dane monitorowania jest automatycznie wyświetlana strony Przegląd i metryki usługi w chmurze, w portalu Azure. 

Podstawowe monitorowanie nie wymagają konta magazynu. 

![usługi w chmurze podstawowe monitorowanie kafelków](media/cloud-services-how-to-monitor/basic-tiles.png)

## <a name="advanced-monitoring"></a>Zaawansowane monitorowanie

Zaawansowanego monitorowania polega na użyciu rozszerzenia diagnostyki Azure (i opcjonalnie zestaw SDK usługi Application Insights) w roli, którą chcesz monitorować. Rozszerzenie diagnostyki używa pliku konfiguracyjnego (dla każdej roli) o nazwie **diagnostics.wadcfgx** konfigurowania metryki diagnostyki monitorowane. Dane diagnostyczne Azure zbiera rozszerzenia jest przechowywane w konta usługi Azure Storage, który jest skonfigurowany w **.wadcfgx** i w [csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) i [.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) plików. Oznacza to, że istnieje dodatkowy koszt związany z zaawansowanego monitorowania.

Przy tworzeniu każdej roli programu Visual Studio dodaje rozszerzenia diagnostyki Azure do niej. To rozszerzenie zebrać następujące informacje:

* Niestandardowe liczniki wydajności
* Dzienniki aplikacji
* Dzienniki zdarzeń systemu Windows
* Źródło zdarzenia platformy .NET
* Dzienniki usług IIS
* Manifest na podstawie ETW
* Zrzuty awaryjne
* Dzienniki błędów klienta

Podczas tych danych jest agregowana do konta magazynu, portalu nie zapewnia natywnego sposobem dane wykresu. Innej usługi, takie jak usługi Application Insights, można użyć do grupowania i wyświetlić dane.

### <a name="use-application-insights"></a>Użyj usługi Application Insights

Po opublikowaniu usługi w chmurze w programie Visual Studio podano opcję Wyślij dane diagnostyczne do usługi Application Insights. Można utworzyć zasobu usługi Application Insights w tym czasie, lub wysłać dane do istniejącego zasobu. Usługi w chmurze mogą być monitorowane przez usługę Application Insights dostępność, wydajność, błędy i użycia. Niestandardowe wykresy mogą być dodawane do usługi Application Insights, dzięki czemu dane można wyświetlić który ważna najbardziej. Mogą być zbierane dane wystąpienia roli przy użyciu zestawu SDK usługi Application Insights w projekcie usługi w chmurze. Aby uzyskać więcej informacji na temat integracji usługi Application Insights, zobacz [usługi Application Insights z usługami w chmurze](../application-insights/app-insights-cloudservices.md).

Należy pamiętać, że podczas korzystania z usługi Application Insights, aby wyświetlić liczniki wydajności (i inne ustawienia) określono za pośrednictwem rozszerzenia systemu Windows Azure Diagnostics tylko otrzymasz więcej możliwości dzięki integracji zestawu SDK usługi Application Insights do role sieci web i proces roboczy.


## <a name="add-advanced-monitoring"></a>Dodaj zaawansowanego monitorowania

Pierwsza strona, jeśli nie masz **klasycznego** konta magazynu [utworzyć](../storage/common/storage-create-storage-account.md#create-a-storage-account). Upewnij się, że utworzono konto magazynu z **klasycznego modelu wdrożenia** określony.

Następnie przejdź do **konta magazynu (klasyczne)** zasobów. Wybierz **ustawienia** > **klucze dostępu** i skopiuj **parametry połączenia podstawowej** wartość. Ta wartość jest wymagane dla usługi w chmurze. 

Istnieją dwa pliki konfiguracji, należy zmienić Diagnostics zaawansowane włączyć, **ServiceDefinition.csdef** i **pliku ServiceConfiguration.cscfg**. Prawdopodobnie użytkownik ma dwa **.cscfg** pliki jedną o nazwie **ServiceConfiguration.cloud.cscfg** wdrażania platformy Azure i jedną o nazwie **ServiceConfiguration.local.cscfg** używany do wdrożenia lokalnego debugowania. Zmiana obu z nich.

W **ServiceDefinition.csdef** plików, Dodaj nowe ustawienie o nazwie `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` dla każdej roli, która używa zaawansowane diagnostyki. Podczas tworzenia nowego projektu programu Visual Studio dodaje do tej wartości do pliku. W przypadku, gdy jest on niedostępny, można dodać go teraz. 

```xml
<ServiceDefinition name="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRoleWithSBQueue1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
```

Definiuje to nowe ustawienie, które muszą zostać dodane do każdej **pliku ServiceConfiguration.cscfg** pliku. Otwórz i zmień każdego **.cscfg** pliku. Dodaj ustawienie o nazwie `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`. Ustaw wartość na jedną **parametry połączenia podstawowej** konta magazynu classic lub do `UseDevelopmentStorage=true`, jeśli chcesz użyć lokalnego magazynu na komputerze deweloperskim.

```xml
<ServiceConfiguration serviceName="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="WorkerRoleWithSBQueue1">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=mystorage;AccountKey=KWwkdfmskOIS240jnBOeeXVGHT9QgKS4kIQ3wWVKzOYkfjdsjfkjdsaf+sddfwwfw+sdffsdafda/w==" />

<!-- or use the local development machine for storage
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
-->
```

## <a name="next-steps"></a>Kolejne kroki

- [Więcej informacji na temat usługi Application Insights z usługami w chmurze.](../application-insights/app-insights-cloudservices.md)

