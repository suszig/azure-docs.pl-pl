---
title: "Monitor klastra Azure DC/OS - operacje zarządzania | Dokumentacja firmy Microsoft"
description: "Monitorowanie klastra usługi kontenera platformy Azure DC/OS z programu Microsoft Operations Management Suite."
services: container-service
documentationcenter: 
author: keikhara
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: 9b8f96b34b53982c469273a3df9751ceb7930d60
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-operations-management-suite"></a>Monitor klastra usługi kontenera platformy Azure DC/OS w usłudze Operations Management Suite

Pakiet Microsoft Operations Management Suite (OMS) to oparte na chmurze rozwiązanie firmy Microsoft do zarządzania systemami IT, które ułatwia zarządzanie infrastrukturą lokalną i chmurową oraz jej ochronę. Kontener rozwiązanie to rozwiązanie w OMS analizy dzienników, które ułatwia przeglądanie spisu kontenera, wydajności i dzienniki w jednej lokalizacji. Można inspekcji, rozwiązywanie problemów z kontenerów, wyświetlając dzienniki w centralnej lokalizacji oraz znaleźć zakłócenia, wykorzystywanie nadmiarowe kontenera na hoście.

![](media/container-service-monitoring-oms/image1.png)

Aby uzyskać więcej informacji o rozwiązaniu kontenera, zapoznaj się [analizy dzienników rozwiązania kontenera](../../log-analytics/log-analytics-containers.md).

## <a name="setting-up-oms-from-the-dcos-universe"></a>Konfigurowanie OMS z universe DC/OS


W tym artykule założono, że skonfigurowano DC/OS i wdrożeniu aplikacji kontenera sieci web proste w klastrze.

### <a name="pre-requisite"></a>Wymagania wstępne
- [Subskrypcja usługi Microsoft Azure](https://azure.microsoft.com/free/) — można uzyskać tę bezpłatnie.  
- Instalator obszar roboczy Microsoft OMS — zobacz "Krok 3" poniżej
- [Interfejs wiersza polecenia DC/OS](https://dcos.io/docs/1.8/usage/cli/install/) zainstalowane.

1. Na pulpicie nawigacyjnym DC/OS kliknij Universe i wyszukaj "OMS", jak pokazano poniżej.

![](media/container-service-monitoring-oms/image2.png)

2. Kliknij pozycję **Zainstaluj**. Zobaczysz punktu obecności się z informacjami o wersji OMS i **zainstaluj pakiet** lub **instalacji zaawansowanym** przycisku. Po kliknięciu **zaawansowane instalacji**, która prowadzi do **OMS określonej konfiguracji właściwości** strony.

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. W tym miejscu, użytkownik zostanie poproszony o podanie `wsid` (identyfikator obszaru roboczego OMS) i `wskey` (OMS klucz podstawowy identyfikator obszaru roboczego). Uzyskanie zarówno `wsid` i `wskey` musisz utworzyć konto OMS w <https://mms.microsoft.com>. Wykonaj kroki, aby utworzyć konto. Po zakończeniu tworzenia konta, musisz uzyskać Twoje `wsid` i `wskey` , klikając **ustawienia**, następnie **połączonych źródeł**, a następnie **serwerówzsystememLinux**, jak pokazano poniżej.

 ![](media/container-service-monitoring-oms/image5.png)

4. Wybierz numer możesz OMS wystąpienia mają, a następnie kliknij przycisk "Przejrzyj i zainstaluj". Zwykle ma mieć liczbę wystąpień OMS równa liczbie maszyny Wirtualnej w klastrze agenta. Agent pakietu OMS dla systemu Linux jest instaluje poszczególnych kontenerów na każdej maszynie Wirtualnej, które chcą zebrać informacje dotyczące monitorowania i rejestrowania informacji.

## <a name="setting-up-a-simple-oms-dashboard"></a>Skonfigurowanie prostego pulpit nawigacyjny OMS

Po zainstalowaniu Agent pakietu OMS dla systemu Linux na maszynach wirtualnych, następnym krokiem jest ustanowienie OMS pulpitu nawigacyjnego. Istnieją dwa sposoby, w tym celu: portalu OMS lub portalu Azure.

### <a name="oms-portal"></a>Portalu OMS 

Zaloguj się do portalu OMS (<https://mms.microsoft.com>) i przejdź do **galerii rozwiązań**.

![](media/container-service-monitoring-oms/image6.png)

Po przejściu do **galerii rozwiązań**, wybierz pozycję **kontenery**.

![](media/container-service-monitoring-oms/image7.png)

Po wybraniu rozwiązania kontenera, zobaczysz Kafelek na stronie pulpitu nawigacyjnego przeglądu OMS. Po danych pozyskiwane kontenera jest indeksowana, zostanie wyświetlona kafelka wypełnione informacjami na kafelkach widoku rozwiązania.

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a>Azure Portal 

Zaloguj się do portalu Azure w <https://portal.microsoft.com/>. Przejdź do **Marketplace**, wybierz pozycję **monitorowanie i zarządzanie** i kliknij przycisk **Zobacz wszystkie**. Następnie wpisz `containers` wyszukiwania. Zostanie wyświetlone "kontenery" w wynikach wyszukiwania. Wybierz **kontenery** i kliknij przycisk **Utwórz**.

![](media/container-service-monitoring-oms/image9.png)

Po kliknięciu **Utwórz**, jego wyświetli monit dla obszaru roboczego. Wybierz obszar roboczy lub jeśli nie istnieje, Utwórz nowy obszar roboczy.

![](media/container-service-monitoring-oms/image10.PNG)

Po wybraniu obszaru roboczego kliknij **Utwórz**.

![](media/container-service-monitoring-oms/image11.png)

Aby uzyskać więcej informacji o rozwiązaniu kontenera OMS, zapoznaj się [analizy dzienników rozwiązania kontenera](../../log-analytics/log-analytics-containers.md).

### <a name="how-to-scale-oms-agent-with-acs-dcos"></a>Jak skalować Agent pakietu OMS z ACS DC/OS 

W przypadku, gdy potrzebne do zainstalowania agenta pakietu OMS jest zbyt mała liczba rzeczywista węzłów lub skalowaniu VMSS, dodając więcej maszyny Wirtualnej, możesz to zrobić przez skalowanie `msoms` usługi.

Można przejść do Marathon lub na karcie usług interfejsu użytkownika DC/OS i skalowanie w górę liczba węzłów.

![](media/container-service-monitoring-oms/image12.PNG)

To spowoduje wdrożenie do innych węzłów, które nie zostały jeszcze wdrożone agent pakietu OMS.

## <a name="uninstall-ms-oms"></a>Odinstaluj MS OMS

Aby odinstalować MS OMS, wprowadź następujące polecenie:

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a>Daj nam znać!
Co to działa? Czego brakuje? Co należy to być przydatne dla Ciebie? Daj nam znać w <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.

## <a name="next-steps"></a>Następne kroki

 Teraz, kiedy OMS zostały skonfigurowane do monitorowania kontenerów,[Zobacz Pulpit nawigacyjny kontenera](../../log-analytics/log-analytics-containers.md).
