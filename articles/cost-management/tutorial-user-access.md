---
title: "Przypisywanie dostępu w usłudze Azure Cost Management | Microsoft Docs"
description: "Przypisywanie dostępu do danych rozwiązania Cost Management przy użyciu kont użytkowników w celu zdefiniowania poziomów dostępu do jednostek."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 89639e6d9c06edf110dc8432fbefe1fbfd1298c9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="assign-access-to-cost-management-data"></a>Przypisywanie dostępu do danych rozwiązania Cost Management

Prawa dostępu do danych rozwiązania Cost Management są przyznawane za pośrednictwem funkcji zarządzania użytkownikami lub jednostkami. Konta użytkowników platformy Cloudyn określają dostęp do *jednostek* i funkcji administracyjnych. Istnieją dwa typy dostępu: administratora i użytkownika. O ile nie zmodyfikowano ich dla poszczególnych użytkowników, prawa dostępu administratora umożliwiają użytkownikowi nieograniczone korzystanie ze wszystkich funkcji w portalu Cloudyn, w tym z funkcji, takich jak: zarządzanie użytkownikami, zarządzanie listami adresatów oraz dostęp jednostki głównej do danych wszystkich jednostek. Dostęp użytkownika jest przeznaczony dla użytkowników końcowych i umożliwia im wyświetlanie raportów oraz tworzenie raportów przy użyciu dostępu do danych jednostki.

Jednostki są używane do odzwierciedlania struktury hierarchicznej organizacji biznesowej. Identyfikują one działy, oddziały i zespoły Twojej organizacji w rozwiązaniu Cloudyn. Hierarchia jednostek pomaga dokładnie śledzić wydatki dla poszczególnych jednostek.

Zarejestrowanie konta lub umowy dotyczącej platformy Azure spowodowało utworzenie w rozwiązaniu Cloudyn konta z uprawnieniami administratora, więc możesz wykonać wszystkie czynności opisane w tym samouczku. Ten samouczek dotyczy dostępu do danych rozwiązania Cost Management, w tym danych zarządzania użytkownikami i zarządzania jednostkami. Omawiane kwestie:

> [!div class="checklist"]
> * Tworzenie użytkownika z prawami dostępu administratora
> * Tworzenie użytkownika z prawami dostępu użytkownika
> * Tworzenie jednostek



## <a name="create-a-user-with-admin-access"></a>Tworzenie użytkownika z prawami dostępu administratora

Mimo że Ty masz już prawa dostępu administratora, współpracownicy z organizacji mogą również potrzebować takich praw. W portalu Cloudyn kliknij symbol koła zębatego w prawym górnym rogu i wybierz pozycję **Zarządzanie użytkownikami**. Kliknij pozycję **Dodaj nowego użytkownika**, aby dodać nowego użytkownika.

Wprowadź wymagane informacje o użytkowniku. Pole hasła może pozostać puste, dzięki czemu użytkownik będzie mógł ustawić nowe hasło podczas pierwszego logowania. Link z informacjami logowania jest wysyłany do użytkownika pocztą e-mail z platformy Cloudyn po wybraniu pozycji **Powiadom użytkownika pocztą e-mail**. Wybierz uprawnienia, aby zezwolić na zarządzanie użytkownikami, co pozwoli użytkownikowi na tworzenie i modyfikowanie innych użytkowników. Zarządzanie listami adresatów umożliwia użytkownikowi edytowanie list adresatów.

W obszarze **Użytkownik ma dostęp administratora** wybrano jednostkę główną organizacji. Pozostaw zaznaczoną jednostkę główną i zapisz informacje o użytkowniku. Wybranie jednostki głównej pozwoli użytkownikowi na posiadanie uprawnień administratora nie tylko do jednostki głównej w drzewie, ale także do wszystkich jednostek, które znajdują się pod nią.  
  ![dodawanie nowego użytkownika z prawami dostępu administratora](.\media\tutorial-user-access\new-admin-access.png)

## <a name="create-a-user-with-user-access"></a>Tworzenie użytkownika z prawami dostępu użytkownika
Zazwyczaj użytkownicy, którzy potrzebują dostępu do danych rozwiązania Cost Management, takich jak pulpity nawigacyjne i raporty, powinni mieć prawa użytkownika do ich wyświetlania. Utwórz nowego użytkownika z dostępem użytkownika podobnego do utworzonego przy użyciu dostępu administratora, pamiętając o następujących różnicach:

- Usuń zaznaczenie pól **Zezwól na zarządzanie użytkownikami** i **Zezwól na zarządzanie listami adresatów**, a następnie usuń wszystkie pozycje z listy **Użytkownik ma dostęp administratora**.
- Zaznacz jednostki, do których użytkownik musi mieć dostęp, na liście **Użytkownik ma dostęp użytkownika**.
- W razie potrzeby możesz również zezwolić administratorowi na dostęp do określonych jednostek.

![dodawanie nowego użytkownika z prawami dostępu użytkownika](.\media\tutorial-user-access\new-user-access.png)

Aby obejrzeć film wideo z samouczkiem dotyczącym dodawania użytkowników, zobacz [Adding Users to Azure Cost Management by Cloudyn (Dodawanie użytkowników do usługi Azure Cost Management na platformie Cloudyn)](https://youtu.be/Nzn7GLahx30).

## <a name="create-entities"></a>Tworzenie jednostek

W przypadku definiowania hierarchii jednostek kosztów najlepszym rozwiązaniem jest zidentyfikowanie struktury organizacji.

Podczas tworzenia drzewa zastanów się, jak musisz lub chcesz wyświetlać swoje koszty posegregowane według jednostek biznesowych, centrów kosztów, środowisk i działów sprzedaży. Drzewo jednostek w rozwiązaniu Cloudyn jest elastyczne z powodu dziedziczenia jednostek. Poszczególne subskrypcje dla kont w chmurze są połączone z określonymi jednostkami. Dlatego jednostki są wielodostępne. Dostęp poszczególnych użytkowników można przypisywać do tylko do ich segmentu firmy przy użyciu jednostek. Dzięki temu dane są izolowane, nawet w dużych częściach firm, takich jak przedstawicielstwa. Izolacja danych wspomaga także ład.  

Po zarejestrowaniu konta lub umowy związanej z platformą Azure na platformie Cloudyn dane zasobów platformy Azure, w tym dane użycia, wydajności, rozliczeń i tagów, z subskrypcji zostały skopiowane do konta platformy Cloudyn. Drzewo jednostek trzeba jednak utworzyć ręcznie. W przypadku pominięcia rejestracji usługi Azure Resource Manager tylko dane rozliczeń i pewne raporty zasobów są dostępne w portalu Cloudyn.

W portalu Cloudyn kliknij pozycję **Ustawienia** w górnym prawym rogu i wybierz pozycję **Konta w chmurze**. Rozpoczynasz od pojedynczej jednostki (głównej) i tworzysz drzewo jednostek pod tą jednostką główną. Oto przykład hierarchii jednostek, która po ukończeniu drzewa może przypominać wiele organizacji IT:

![drzewo jednostki](.\media\tutorial-user-access\entity-tree.png)

Obok pozycji **Jednostki** kliknij pozycję **Dodaj jednostkę**. Wprowadź informacje o osobie lub dziale do dodania. Wartości pól **Imię i nazwisko** i **Adres e-mail** nie muszą być zgodne z danymi istniejących użytkowników. Jeśli chcesz wyświetlić listę poziomów dostępu, wyszukaj w pomocy zagadnienia dotyczące *dodawania jednostki*.

![dodawanie jednostki](.\media\tutorial-user-access\add-entity.png)

Gdy wszystko będzie gotowe, **zapisz** jednostkę.


Aby obejrzeć film wideo z samouczkiem dotyczącym tworzenia hierarchii jednostek kosztów, zobacz [Creating a Cost Entity Hierarchy in Azure Cost Management by Cloudyn (Tworzenie hierarchii jednostek kosztów w usłudze Azure Cost Management na platformie Cloudyn)](https://youtu.be/dAd9G7u0FmU).

Jeśli jesteś użytkownikiem platformy Azure z umową Enterprise Agreement, obejrzyj film z samouczkiem dotyczącym kojarzenia kont i subskrypcji z jednostkami: [Connecting to Azure Resource Manager with Azure Cost Management by Cloudyn (Łączenie z usługą Azure Resource Manager przy użyciu usługi Azure Cost Management na platformie Cloudyn)](https://youtu.be/oCIwvfBB6kk).

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie użytkownika z prawami dostępu administratora
> * Tworzenie użytkownika z prawami dostępu użytkownika
> * Tworzenie jednostek

Przejdź do następnego samouczka, aby dowiedzieć się, jak prognozować wydatki przy użyciu danych historycznych.

> [!div class="nextstepaction"]
> [Prognozowanie wydatków w przyszłości](tutorial-forecast-spending.md)
