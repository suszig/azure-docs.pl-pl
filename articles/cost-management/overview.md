---
title: "Omówienie usługi Azure koszt zarządzania przez Cloudyn | Dokumentacja firmy Microsoft"
description: "Azure Cost Management firmy Cloudyn to rozwiązanie do zarządzania kosztami wielu chmur, które ułatwia korzystanie z platformy Azure i innych zasobów w chmurze."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: overview
ms.service: cost-management
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 969340080bfe2b04704367c2225895728773119e
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2017
---
# <a name="what-is-azure-cost-management"></a>Co to jest Azure kosztów zarządzania?

Azure kosztów zarządzania Cloudyn umożliwia śledzenie użycia chmury i wydatków zasobów platformy Azure i innych dostawców chmury, w tym usług AWS i Google. Łatwe do zrozumienia pulpitu nawigacyjnego raporty uzyskać pomoc dotyczącą alokacji kosztów i showbacks/obciążeń zwrotnych również. Kosztów zarządzania ułatwia, optymalizację wydatków na technologie, określając niedostatecznie zasobów, które można następnie zarządzać i dostosować chmury.

Aby obejrzeć wprowadzenie wideo, zobacz [wprowadzenie do usługi Azure Management koszt przez Cloudyn](https://youtu.be/NWIRny6Wpsk).

## <a name="monitor-usage-and-spending"></a>Monitorowanie użycia i wydatków

Monitorowanie użycie i wydatków jest krytycznie ważne w przypadku infrastruktury chmury, ponieważ organizacje płacić za zasoby, które wykorzystują one wraz z upływem czasu. Nieoczekiwany koszt nadwyżki szybko może wystąpić, gdy użycie przekroczy próg umowy. Kilka ważnych czynników może utrudnić ad hoc monitorowania. Najpierw projekcji kosztów na podstawie użycia średni przyjęto założenie użycia spójność w danym okresie rozliczeniowym. Po drugie kosztów zbliża się lub przekracza budżetu, ważne jest uzyskiwanie powiadomień z wyprzedzeniem, aby dostosować wydatki. I dostawcom usług w chmurze nie może oferować projekcji koszt porównanie wartości progowe lub raporty okresach porównania.

Raporty ułatwiają monitorowanie, wydatków do analizowania i śledzić użycie w chmurze, koszty i trendów. Zgłasza przy użyciu w miarę upływu czasu, można wykryć anomalii, które różnią się od normalnego trendów. Wydajność w danym wdrożeniu chmury są widoczne w raportach optymalizacji. Można także zauważyć wydajność w raportach analizy kosztów.

![Raport koszt w czasie](media\overview\cost-over-time-rpt.png)


## <a name="manage-costs"></a>Zarządzanie kosztami

Dane historyczne ułatwiają zarządzanie kosztami podczas analizowania wraz z upływem czasu, aby zidentyfikować trendy użycia i koszty. Trendy są następnie używane do Prognozowanie wydatków w przyszłości. Koszt zarządzania obejmuje również przydatne szacowany koszt raportów.

Alokacja kosztu zarządza koszty analizy kosztów na podstawie Twojej znakowania zasad. Tagi na niestandardowe konta, zasobów i jednostek służą do zaktualizowania alokacji kosztów. Menedżer kategorii organizuje tagów w celu uzyskania dodatkowych ładu. I użyj alokacji kosztów dla ogólnej analizy kosztów/opłaty zwrotne do wyświetlenia wykorzystania zasobów i koszty wpłynąć na zachowania dotyczące lub nałożyć dzierżawy klientów.

Kontrola dostępu ułatwia zarządzanie kosztami przez zapewnienie, że użytkownicy i zespoły dostępu tylko kosztów zarządzania dane, które są potrzebne. Umożliwia struktury jednostek, zarządzanie użytkownikami i zaplanowane raporty z listy adresatów przypisywanie dostępu.

Alerty pomaga zarządzać kosztami przez powiadamianie użytkownika automatycznie nietypowe wydatków lub przekroczenia wydatków. Alerty mogą również wyświetlać powiadomienia inni uczestnicy projektu automatycznie dla anomalii wydatków i ryzyka przekroczenie wydatków. Różne raporty obsługuje alerty oparte na budżetu, a jego kosztem progów. Jednak alertów nie są obecnie obsługiwane dla dostawcy usług Kryptograficznych partnera kont lub subskrypcji.

## <a name="improve-efficiency"></a>Poprawa wydajności

Można ustalić optymalne użycie maszyny Wirtualnej i zidentyfikować bezczynnych maszyn wirtualnych lub usuń bezczynnych maszyn wirtualnych i odłączyć dysków o kosztów zarządzania. Korzystając z informacji w optymalizacji rozmiaru i raporty nieefektywne podejście, można utworzyć plan rozmiar w dół lub usunąć bezczynności maszyny wirtualnej. Jednak optymalizacji raporty nie są obecnie obsługiwane dla dostawcy usług Kryptograficznych partnera kont lub subskrypcji.

![Zalecenia dotyczące zmiany rozmiaru](.\media\overview\sizing.png)

Jeśli zainicjowano obsługę administracyjną usług AWS zastrzeżone wystąpień, można zwiększenie wykorzystanie zastrzeżone wystąpienia przy użyciu funkcji raportów optymalizacji, gdzie można wyświetlić zalecenia dotyczące kupowania, modyfikowania zastrzeżenia nieużywane i plan obsługi.

## <a name="next-steps"></a>Następne kroki

Teraz, kiedy znasz koszt zarządzania, następnym krokiem jest Zarejestruj środowiska chmury i rozpocząć eksplorowanie danych.

- [Zarejestruj poszczególnych subskrypcji platformy Azure](quick-register-azure-sub.md)
