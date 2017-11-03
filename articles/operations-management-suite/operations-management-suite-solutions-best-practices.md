---
title: "Najlepsze praktyki rozwiązania OMSManagement | Dokumentacja firmy Microsoft"
description: 
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: b3d07ad3164609a5628c0d9805de55a32870ab94
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="best-practices-for-creating-management-solutions-in-operations-management-suite-oms-preview"></a>Najlepsze rozwiązania dotyczące tworzenia rozwiązań do zarządzania w Operations Management Suite (OMS) (wersja zapoznawcza)
> [!NOTE]
> To jest wstępna dokumentacji do tworzenia rozwiązań do zarządzania w OMS, które są obecnie w wersji zapoznawczej. Żadnego schematu opisanych poniżej może ulec zmianie.  

W tym artykule przedstawiono najlepsze rozwiązania dotyczące [Tworzenie pliku rozwiązania zarządzania](operations-management-suite-solutions-solution-file.md) w Operations Management Suite (OMS).  Te informacje zostaną zaktualizowane określonych dodatkowych najlepszych rozwiązań.

## <a name="data-sources"></a>Źródła danych
- Źródła danych może być [skonfigurowany przy użyciu szablonu usługi Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), ale nie powinny znajdować się w pliku rozwiązania.  Przyczyną jest to, że Konfigurowanie źródeł danych nie jest obecnie idempotentności, co oznacza, że rozwiązania można zastąpić istniejącą konfigurację w obszarze roboczym użytkownika.<br><br>Na przykład rozwiązania może wymagać ostrzeżeń i błędów zdarzenia w dzienniku zdarzeń aplikacji.  Jeśli określisz to jako źródło danych w rozwiązaniu, istnieje ryzyko, usuwanie informacje o zdarzeniach, jeśli użytkownik ma to skonfigurowanych w ich obszaru roboczego.  Jeśli dołączysz wszystkie zdarzenia, następnie użytkownik może zbierać nadmiernego informacje o zdarzeniach w obszarze roboczym użytkownika.

- Jeśli rozwiązanie wymaga danych z jednego ze źródeł danych standardowych, następnie należy zdefiniować to jako warunek wstępny.  Stan w dokumentacji klienta należy skonfigurować źródło danych na ich własnych.  
- Dodaj [Weryfikacja przepływu danych](../log-analytics/log-analytics-view-designer-tiles.md) komunikat do widoków w rozwiązaniu aby poinstruować użytkowników do źródeł danych, które należy skonfigurować dla wymaganych danych mają być zbierane.  Ten komunikat jest wyświetlany na kafelku widoku, gdy nie można odnaleźć wymaganych danych.


## <a name="runbooks"></a>Elementy Runbook
- Dodaj [Harmonogram automatyzacji](../automation/automation-schedules.md) dla każdego elementu runbook w rozwiązaniu musi być uruchamiane zgodnie z harmonogramem.
- Obejmują [modułu IngestionAPI](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) w rozwiązaniu do użycia przez zapisywanie danych w repozytorium analizy dzienników elementów runbook.  Konfigurowanie rozwiązania pod kątem [odwołania](operations-management-suite-solutions-solution-file.md#solution-resource) tego zasobu, tak że pozostaje po rozwiązaniu.  Dzięki temu wielu rozwiązań w celu udostępniania modułu.
- Użyj [zmienne automatyzacji](../automation/automation-schedules.md) podać wartości do rozwiązania, które użytkownicy można zmienić później.  Nawet jeśli rozwiązanie jest skonfigurowany do przechowywania zmiennej, nadal można zmienić jej wartości.

## <a name="views"></a>Widoki
- Wszystkie rozwiązania powinny obejmować jeden widok, która jest wyświetlana w portalu użytkownika.  Widok może zawierać wielu [części wizualizacji](../log-analytics/log-analytics-view-designer-parts.md) w celu zilustrowania różne zestawy danych.
- Dodaj [Weryfikacja przepływu danych](../log-analytics/log-analytics-view-designer-tiles.md) komunikat do widoków w rozwiązaniu aby poinstruować użytkowników do źródeł danych, które należy skonfigurować dla wymaganych danych mają być zbierane.
- Konfigurowanie rozwiązania pod kątem [zawiera](operations-management-suite-solutions-solution-file.md#solution-resource) widok, tak że jego usunięte, jeśli rozwiązanie zostanie usunięta.

## <a name="alerts"></a>Alerty
- Zdefiniuj listy adresatów jako parametru w pliku rozwiązania, użytkownik może zdefiniować je przy instalacji rozwiązania.
- Konfigurowanie rozwiązania pod kątem [odwołania](operations-management-suite-solutions-solution-file.md#solution-resource) reguły alertu, że użytkownik może zmienić ich konfiguracji.  Warto wprowadzić zmiany, takie jak modyfikowanie listy adresatów, zmianę wartości progowej alertu lub wyłączania reguły alertów. 


## <a name="next-steps"></a>Następne kroki
* Zapoznaj się z artykułem podstawowego procesu [projektowania i konfigurowania rozwiązania do zarządzania](operations-management-suite-solutions-creating.md).
* Dowiedz się, jak [Utwórz plik rozwiązania](operations-management-suite-solutions-solution-file.md).
* [Dodaj zapisanych wyszukiwań i alerty](operations-management-suite-solutions-resources-searches-alerts.md) do rozwiązania do zarządzania.
* [Dodawanie widoków](operations-management-suite-solutions-resources-views.md) do rozwiązania do zarządzania.
* [Dodaj element runbook usługi Automatyzacja i innych zasobów](operations-management-suite-solutions-resources-automation.md) do rozwiązania do zarządzania.

