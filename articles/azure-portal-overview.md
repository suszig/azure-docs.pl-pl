---
title: "Omówienie witryny Azure Portal | Microsoft Docs"
description: "Dowiedz się, jak korzystać z witryny Microsoft Azure Portal."
services: 
documentationcenter: 
author: davidwrede
manager: erikre
editor: jimbe
ms.assetid: 53cb9df1-c96a-4f4e-b022-18336cd3d697
ms.service: azure-portal
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 12/16/2015
ms.author: dwrede
ms.openlocfilehash: 45decfdec01b7086d1f9d18b31cf01cec1adb34d
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="microsoft-azure-portal-overview"></a>Przegląd witryny Microsoft Azure Portal
Microsoft Azure Portal to centralne miejsce, w którym możliwe jest aprowizowanie zasobów platformy Azure i zarządzanie nimi.  Dzięki temu samouczkowi zapoznasz się z portalem i dowiesz się, jak korzystać z niektórych z następujących kluczowych możliwości:

* **Kompleksowy portal Marketplace** umożliwiający przeglądanie tysięcy elementów od firmy Microsoft i innych dostawców, które mogą zostać zakupione i/lub których możliwa jest aprowizacja.
* **Ujednolicone i skalowalne środowisko przeglądania**, które ułatwia znajdowanie interesujących zasobów i wykonywanie różnych operacji związanych z zarządzaniem.
* **Spójne strony zarządzania** (lub bloki) umożliwiające zarządzanie szerokim wachlarzem usług platformy Azure w spójny sposób, który polega na udostępnianiu ustawień, akcji, informacji dotyczących rozliczeń, monitorowania kondycji i danych dotyczących zużycia oraz wielu innych elementów.
* **Własne środowisko** umożliwiające tworzenie dostosowanego ekranu startowego, na którym zawsze po zalogowaniu się będą wyświetlane żądane informacje.  Można również dostosowywać dowolne bloki zarządzania, które zawierają kafelki.
  
  ![Orientacja interfejsu użytkownika witryny Azure Portal][UIOrientation]

## <a name="before-you-get-started"></a>Przed rozpoczęciem
Konieczne będzie posiadanie ważnej subskrypcji platformy Azure, aby móc skorzystać z tego samouczka.  Jeśli nie masz ważnej subskrypcji, [utwórz konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/) już dzisiaj.  Po utworzeniu subskrypcji dostęp do portalu można uzyskać pod adresem <https://portal.azure.com>.

## <a name="how-to-create-a-resource"></a>Tworzenie zasobu
Platforma Azure zawiera portal Marketplace z tysiącami elementów, które można utworzyć w jednym miejscu.  Załóżmy, że chcesz utworzyć nową maszynę wirtualną systemu Windows Server 2012.  Nowe centrum jest punktem wejścia do nadzorowanego zestawu polecanych kategorii z portalu Marketplace.  Każda kategoria ma niewielki zestaw polecanych elementów wraz z linkiem do pełnego portalu Marketplace, w którym są wyświetlane wszystkie kategorie i jest możliwe wyszukiwanie. Aby utworzyć tę nową maszynę wirtualną systemu Windows Server 2012, wykonaj następujące czynności:  

1. System Windows Server 2012 jest polecany, więc możesz wybrać go z kategorii Obliczenia.  
2. Wypełnij niektóre podstawowe dane wejściowe formularza.
3. Kliknij pozycję Utwórz. Aprowizacja maszyny wirtualnej rozpocznie się natychmiast.

Po utworzeniu zasobu w centrum powiadomień zostanie wygenerowany alert oraz zostanie otwarty blok zarządzania (możesz zawsze przejść do zasobów później).

![Kategorie portalu][PortalCategories]

## <a name="how-to-find-your-resources"></a>Znajdowanie zasobów
Często używane zasoby zawsze można przypiąć do tablicy startowej, ale konieczne może być przejście do zasobu, który nie jest często używany.  Za pomocą wyświetlonego poniżej centrum przeglądania można uzyskać dostęp do wszystkich zasobów.  Możliwe jest filtrowanie według subskrypcji, wybieranie i zmienianie rozmiarów kolumn oraz przechodzenie do bloków zarządzania. W tym celu wystarczy kliknąć odpowiednie elementy.

![Centrum przeglądania][BrowseHub]

## <a name="how-to-manage-and-delegate-access-to-a-resource"></a>Zarządzanie dostępem do zasobu i delegowanie go
Z poziomu tego bloku można nawiązać połączenie z maszyną wirtualną za pomocą pulpitu zdalnego, monitorować kluczowe metryki wydajności, kontrolować dostęp do tej maszyny wirtualnej przy użyciu dostępu opartego na rolach (RBAC), konfigurować maszynę wirtualną i wykonywać inne istotne zadania zarządzania.  Delegowanie dostępu na podstawie roli ma kluczowe znaczenie dla zarządzania w odpowiedniej skali.  Aby uzyskać więcej informacji, kliknij [tutaj](active-directory/role-based-access-control-configure.md). Aby delegować dostęp do zasobu, należy wykonać następujące czynności:

1. Przejdź do zasobu.
2. Kliknij pozycję Wszystkie ustawienia w sekcji Podstawowe elementy.
3. Kliknij pozycję Użytkownicy na liście ustawień.
4. Kliknij pozycję Dodaj na pasku poleceń.
5. Wybierz użytkownika i rolę.

![Zarządzanie zasobem][ManageResource]

## <a name="how-to-get-help"></a>Uzyskiwanie pomocy
Jeśli masz jakikolwiek problem, zawsze Ci pomożemy.  Portal ma stronę pomocy i obsługi technicznej, która może pomóc w znalezieniu odpowiedniego rozwiązania.  W zależności od Twojego [planu pomocy technicznej](https://azure.microsoft.com/support/plans/) możesz również tworzyć bilety pomocy technicznej bezpośrednio w portalu.  Po utworzeniu biletu pomocy technicznej możesz zarządzać jego cyklem życia z poziomu portalu. Aby wyświetlić stronę pomocy i obsługi technicznej, przejdź do obszaru Przeglądaj -> Pomoc i obsługa techniczna.  

![Pomoc i obsługa techniczna][HelpSupport]

## <a name="summary"></a>Podsumowanie
Informacje zawarte w tym samouczku ułatwiają wykonywanie następujących czynności:

* Zakładanie konta, uzyskiwanie subskrypcji i przeglądanie portalu.
* Używanie interfejsu użytkownika portalu oraz tworzenie i przeglądanie zasobów.
* Tworzenie zasobów i ich przeglądanie.
* Poznanie struktury bloków zarządzania i sposobów spójnego zarządzania różnymi typami zasobów.
* Dostosowywanie portalu w taki sposób, aby interesujące informacje były wyświetlane w jego najbardziej eksponowanym miejscu.
* Sterowanie dostępem do zasobów przy użyciu dostępu opartego na rolach (RBAC).
* Uzyskiwanie pomocy i obsługi technicznej.

Witryna Microsoft Azure Portal znacząco ułatwia tworzenie aplikacji w chmurze i zarządzanie nimi.  Zapoznaj się z [blogiem zarządzania](https://azure.microsoft.com/blog/topics/management/), aby być na bieżąco, ponieważ stale [oczekujemy na opinie](https://feedback.azure.com/forums/223579-azure-preview-portal/) i wprowadzamy ulepszenia.  [Blog ScottGu ](http://weblogs.asp.net/scottgu) to inne doskonałe miejsce, aby uzyskać informacje na temat wszystkich aktualizacji platformy Azure.

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png
