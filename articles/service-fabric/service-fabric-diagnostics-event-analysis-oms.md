---
title: "Analiza zdarzeń sieci szkieletowej usług Azure z usługą OMS | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat wizualizacji i analizowania zdarzeń, monitorowania i diagnostyki klastrów sieci szkieletowej usług Azure przy użyciu pakietu OMS."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: f0cefab15a115719ea9c378546a7e6004bd06187
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="event-analysis-and-visualization-with-oms"></a>Analiza zdarzeń i wizualizacji z OMS

Operations Management Suite (OMS) to zbiór usług zarządzania, które pomagają w monitorowania i diagnostyki dla aplikacji i usług hostowanych w chmurze. Aby uzyskać bardziej szczegółowy przegląd OMS, jakie oferuje, przeczytaj [co to jest OMS?](../operations-management-suite/operations-management-suite-overview.md)

## <a name="log-analytics-and-the-oms-workspace"></a>Analiza dzienników i obszar roboczy OMS

Analiza dzienników zbiera dane z zarządzanych zasobów, włącznie z tabeli magazynu systemu Azure lub agenta i przechowuje ją w centralnym repozytorium. Dane można następnie używana do analizy, alerty i wizualizacji lub dodatkowe eksportowanie. Analiza dzienników obsługuje zdarzeń, danych wydajności lub innych danych niestandardowych.

Po skonfigurowaniu OMS mają dostęp do określonego *obszarem roboczym pakietu OMS*, z którym danych można wykonać zapytania lub wizualizowane w pulpitach nawigacyjnych.

Po odebraniu danych przez analizy dzienników, OMS ma kilka *rozwiązań do zarządzania* , które są opakowaniach jednostkowych rozwiązań do przychodzących danych, dostosować, tak aby kilka scenariuszy monitorowania. Obejmują one *usługi sieć szkieletowa Analytics* rozwiązania i *kontenery* rozwiązania, które są najbardziej odpowiednie dwóch te Diagnostyka i monitorowanie w przypadku używania klastrów sieci szkieletowej usług. Istnieje kilka innych oraz które warto eksploracji i OMS umożliwia również tworzenie rozwiązań niestandardowych, które można uzyskać więcej informacji [tutaj](../operations-management-suite/operations-management-suite-solutions.md). W tym samym obszarze roboczym pakietu OMS, obok analizy dzienników można skonfigurować każdego rozwiązania, które chcesz użyć dla klastra. Obszary robocze umożliwiają dla niestandardowych pulpitów nawigacyjnych i wizualizacja danych i modyfikacji danych, które chcesz gromadzenie i przetwarzanie i analizowanie danych.

## <a name="setting-up-an-oms-workspace-with-the-service-fabric-analytics-solution"></a>Konfigurowanie obszar roboczy OMS z rozwiązania analizy sieci szkieletowej usług
Zaleca się, że obejmują rozwiązania usługi sieci szkieletowej w obszarze roboczym pakietu OMS - zawiera pulpit nawigacyjny, który zawiera różne kanały dzienników przychodzące z poziomu platformy i aplikacji oraz zapewnia możliwość określonych dzienników w sieci szkieletowej usług zapytania. W tym miejscu jest stosunkowo proste rozwiązania sieci szkieletowej usługi wygląd, z jednej aplikacji wdrożonych w klastrze:

![Rozwiązanie rz OMS](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

Istnieją dwa sposoby udostępniania i konfigurowania obszarem roboczym pakietu OMS za pomocą szablonu usługi Resource Manager lub bezpośrednio z portalu Azure Marketplace. Użyj pierwszej podczas wdrażania klastra, a drugie Jeśli masz już wdrożone w diagnostyce klastra włączone.

### <a name="deploying-oms-using-a-resource-management-template"></a>Wdrażanie pakietu OMS przy użyciu szablonu zarządzanie zasobami

W przypadku wdrażania klastra przy użyciu szablonu usługi Resource Manager, szablon można również utworzyć nowy obszar roboczy OMS Dodaj rozwiązania sieci szkieletowej usług do niego i skonfiguruj go odczytać danych z tabel do przechowywania odpowiednie.

>[!NOTE]
>Aby to zrobić, musi być aktywne w kolejności dla tabel usługi Azure storage istnieć OMS diagnostyki / Log Analytics dostęp do informacji z.

[W tym miejscu](https://azure.microsoft.com/resources/templates/service-fabric-oms/) jest przykładowy szablon, który można używać i modyfikować zgodnie z harmonogramem wymaganie, który przeprowadza powyżej akcje. W przypadku, że ma więcej Opcjonalność, istnieje kilka więcej szablonów, które zapewniają różne opcje w zależności od tego, gdzie w procesie może być konfigurowania obszarem roboczym pakietu OMS - znajduje się w temacie [szablonów usługi Service Fabric i OMS](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

### <a name="deploying-oms-using-through-azure-marketplace"></a>Wdrażanie pakietu OMS przy użyciu za pośrednictwem portalu Azure Marketplace

Jeśli wolisz Dodaj obszar roboczy OMS po wdrożeniu klastra, przejdź do portalu Azure Marketplace i poszukaj *"Analytics sieci szkieletowej usług"*.

![OMS rz Analytics w portalu Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

* Polecenie **tworzenie**
* W oknie tworzenia usługi sieć szkieletowa Analytics kliknij **wybierz obszar roboczy** dla *obszarem roboczym pakietu OMS* pola, a następnie **Utwórz nowy obszar roboczy**. Wypełnij odpowiednie wpisy — jedynym wymaganiem jest subskrypcji dla klastra sieci szkieletowej usług i obszarem roboczym pakietu OMS musi być taka sama. Po sprawdzeniu poprawności wpisy obszar roboczy OMS zostanie uruchomione w celu wdrożenia. Ten trwa tylko kilka minut.
* Gdy skończysz, kliknij przycisk **Utwórz** ponownie w dolnej części okna Tworzenie usługi Analytics sieci szkieletowej. Upewnij się, że nowy obszar roboczy zostaną wyświetlone w obszarze *obszarem roboczym pakietu OMS*. Spowoduje to dodanie rozwiązania do obszaru roboczego, który został utworzony.


Chociaż spowoduje to dodanie rozwiązania do obszaru roboczego, obszar roboczy nadal musi być podłączony do danych diagnostycznych z klastra. Przejdź do utworzenia rozwiązania analizy sieci szkieletowej usług w grupy zasobów. Powinny pojawić się *ServiceFabric (\<nameOfOMSWorkspace\>)*.

* Kliknij rozwiązanie, aby przejść do strony Przegląd, z którym można zmienić ustawienia rozwiązania, ustawienia obszaru roboczego i przejdź do portalu OMS.
* W menu nawigacji po lewej stronie kliknij **dzienników kont magazynu**w obszarze *źródeł danych obszaru roboczego*.

    ![Rozwiązanie usługi sieć szkieletowa Analytics w portalu](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics-portal.png)

* Na *dzienniki konta magazynu* kliknij przycisk **Dodaj** u góry, aby dodać dzienników z klastra do obszaru roboczego.
* Kliknij przycisk do **konta magazynu** można dodać odpowiednie konta tworzone w klastrze. Jeśli zostanie użyta domyślna nazwa, nosi nazwę konta magazynu *sfdg\<resourceGroupName\>*. Można również to potwierdzić, sprawdzając szablonu usługi Azure Resource Manager używane do wdrażania klastra, sprawdzając wartość używaną do `applicationDiagnosticsStorageAccountName`. Może być również konieczne przewiń w dół i kliknij przycisk **załadować więcej** Jeśli nazwa konta nie jest wyświetlany. Kliknij nazwę konta magazynu prawo podczas jest wyświetlane aby go wybrać.
* Następnie należy określić *— typ danych*, powinny być **zdarzenia sieci szkieletowej usług**.
* *Źródła* automatycznie powinien być ustawiony na *WADServiceFabric\*EventTable*.
* Kliknij przycisk **OK** nawiązać obszaru roboczego dzienników z klastra.

    ![Dodaj dzienniki konta magazynu z usługą OMS](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

* Konto powinna zostać wyświetlona jako część programu *dzienniki konta magazynu* w źródłach danych obszaru roboczego.

Z tym zostało dodane rozwiązania analizy sieci szkieletowej usług w obszarze roboczym analizy dzienników OMS, który jest teraz prawidłowo podłączone do sieci klastra platformy oraz tabeli dziennika aplikacji. Możesz dodać dodatkowe źródła do obszaru roboczego w taki sam sposób.

## <a name="using-the-oms-agent"></a>Za pomocą agenta pakietu OMS

Jest zalecane, aby użyć EventFlow i WAD jako rozwiązania agregacji, ponieważ pozwalają one na bardziej podejściu Diagnostyka i monitorowanie. Na przykład jeśli chcesz zmienić Twoje dane wyjściowe z EventFlow wymaga nie zmieniono Twojego rzeczywiste Instrumentacji tylko prostą modyfikację do pliku konfiguracji. Jeśli, można jednak podjąć decyzję o inwestycji w przy użyciu pakietu OMS i chcesz kontynuować z użyciem jej do analizy zdarzeń (musi być jedynym Użyj platformy, ale raczej jego będzie co najmniej jednej z platform), firma Microsoft zaleca zapoznanie się ustawienie [OMS ag Enterprise](../log-analytics/log-analytics-windows-agents.md). Należy też używać agent pakietu OMS podczas wdrażania kontenerów do klastra, zgodnie z opisem poniżej.

Proces w ten sposób jest stosunkowo łatwa, ponieważ należy dodać agenta jako skalowania maszyny wirtualnej ustawić rozszerzenia do szablonu usługi Resource Manager zapewnienie, że jest zainstalowany na każdym z węzłów. Przykładowy szablon Menedżera zasobów, która wdraża obszar roboczy OMS z rozwiązaniem sieci szkieletowej usług (jak powyżej) i dodanie agenta do węzły znajdują się w przypadku klastrów [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) lub [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

Zalety tego są następujące:

* Bardziej rozbudowane dane po stronie liczników i metryki wydajności
* Łatwa w konfigurowaniu metryki zbieranych z klastra i bez konieczności aktualizacji konfiguracji sieci klastra. Zmiany ustawień agenta może odbywać się w portalu OMS i do dopasowania ustawień konfiguracji niezbędnych do automatycznego uruchomienia agenta. Aby skonfigurować agenta pakietu OMS do pobrania konkretnych licznikach wydajności, przejdź do obszaru roboczego **strona główna > Ustawienia > danych > liczników wydajności systemu Windows** i wybierz dane, które chcesz wyświetlić zebrane
* Dane zostaną wyświetlone szybciej niż musi być przechowywany przed odbierany przez OMS / Log Analytics
* Monitorowanie kontenery jest znacznie łatwiejsze, ponieważ jego mogą odbierać dzienniki docker (stdout, stderr) oraz statystyka (metryki wydajności na poziomach kontenera i węzła)

Główne w tym miejscu to, ponieważ agent zostanie wdrożony w klastrze z wszystkich aplikacji, może wystąpić pewne wpływ na wydajność aplikacji w klastrze.

## <a name="monitoring-containers"></a>Kontenery monitorowania

Podczas wdrażania kontenerów do klastra usługi sieć szkieletowa, zaleca się, że klaster został skonfigurowany z agentem pakietu OMS i czy rozwiązania kontenery został dodany do obszaru roboczego OMS, aby włączyć monitorowanie i diagnostyki. Oto, jak wygląda rozwiązania kontenery w obszarze roboczym:

![Pulpit nawigacyjny podstawowe OMS](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Agent umożliwia zbieranie kilka dzienników specyficzne dla kontenera, które można wykonać zapytania w OMS, lub używany do wizualizowanego wskaźników. Typy dziennika, które są zbierane są:

* ContainerInventory: zawiera informacje o lokalizacji kontenera, nazwy i obrazów
* ContainerImageInventory: informacje o wdrożonej obrazów, w tym identyfikatory lub rozmiary
* ContainerLog: dzienniki błędu, dzienniki docker (stdout itp.) i innych pozycji
* ContainerServiceLog: docker demon poleceń, które zostały uruchomione
* O wydajności: liczniki wydajności w tym kontenerze procesora cpu, pamięć, ruch sieciowy na dysku We/Wy i metryki niestandardowe z maszyn hosta

W tym artykule opisano kroki wymagane do skonfigurowania kontenera monitorowania dla klastra. Aby dowiedzieć się więcej o rozwiązaniu kontenery w OMS, zapoznaj się ich [dokumentacji](../log-analytics/log-analytics-containers.md).

Aby skonfigurować rozwiązanie kontenera, w obszarze roboczym, upewnij się, że masz agent pakietu OMS wdrożone w węzłach klastra, wykonując kroki wymienione powyżej. Gdy klaster jest gotowy, należy wdrożyć kontener do niego. Przy tym pamiętać, że kontener jest wdrażany do klastra, po raz pierwszy go może zająć kilka minut pobranie obrazu w zależności od rozmiaru.

W portalu Azure Marketplace, wyszukaj *rozwiązanie monitorowanie kontenera* i Utwórz **rozwiązanie monitorowanie kontenera** wynik, który powinna pochodzić, w obszarze monitorowanie i zarządzanie kategorii.

![Dodawanie rozwiązania kontenerów](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

W kroku tworzenia żądania obszarem roboczym pakietu OMS. Wybierz jedną, która została utworzona przy użyciu wdrażania powyżej. Ten krok dodaje rozwiązania kontenery w obszarze roboczym pakietu OMS i jest automatyczne wykrywany przez agenta pakietu OMS wdrożone przez szablon. Agent rozpocznie zbieranie danych o procesach kontenery w klastrze i mniej niż 15 minut, lub tak, powinien zostać wyświetlony światła się za pomocą danych, tak jak obraz powyżej pulpitu nawigacyjnego rozwiązania.


## <a name="next-steps"></a>Następne kroki

Zapoznaj się z następujących narzędzi OMS i opcji, aby dostosować obszar roboczy do potrzeb:

* W przypadku klastrów lokalnymi OMS oferuje bramy (do przodu serwer Proxy HTTP), która może służyć do wysyłania danych z usługą OMS. Dowiedz się więcej o tym, że w [łączenia komputerów bez dostępu do Internetu z usługą OMS przy użyciu bramy OMS](../log-analytics/log-analytics-oms-gateway.md)
* Skonfiguruj OMS, aby skonfigurować [automatycznego alerty](../log-analytics/log-analytics-alerts.md) do pomocy w wykrywaniu i Diagnostyka
* Pobierz zapoznaniu się z [wyszukiwania i badania dziennika](../log-analytics/log-analytics-log-searches.md) funkcje dostępne w ramach analizy dzienników