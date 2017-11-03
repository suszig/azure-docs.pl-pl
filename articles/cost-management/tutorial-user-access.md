---
title: "Przypisywanie dostępu w Azure kosztów zarządzania | Dokumentacja firmy Microsoft"
description: "Przypisywanie dostępu do danych zarządzania z kontami użytkowników, którzy zdefiniować poziomy dostępu do jednostek."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: a42f3b51bf6d888d0d5602887ed317c6164391ef
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2017
---
# <a name="assign-access-to-cost-management-data"></a>Przypisywanie dostępu do danych zarządzania koszt

Dostęp do danych zarządzania koszt są dostarczane przez użytkownika lub zarządzania jednostki. Określ konta użytkowników Cloudyn dostęp do *jednostek* i funkcje administracyjne. Dwa typy dostępu: administratora i użytkownika. Jeśli zmodyfikowano dla poszczególnych użytkowników, dostępu z uprawnieniami administratora umożliwia użytkownika bez ograniczeń stosowania wszystkich funkcji w portalu Cloudyn łącznie: Zarządzanie użytkownikami, zarządzania listy adresatów i jednostki głównego dostępu do wszystkich danych jednostki. Dostęp użytkownika jest przeznaczony dla użytkowników końcowych do wyświetlania raportów i tworzenie raportów przy użyciu dostępu, że muszą oni danych jednostki.

Jednostki są używane do uwzględnienia strukturę hierarchiczną organizacji biznesowych. Identyfikują one działów, działów i zespoły w Cloudyn w Twojej organizacji. Hierarchia jednostek pomaga precyzyjnie monitorować wydatków przez jednostki.

Po zarejestrowaniu z umową Azure lub konta konta z uprawnieniami administratora został utworzony w Cloudyn, więc można wykonać wszystkie czynności w tym samouczku. Ten samouczek obejmuje dostęp do danych koszt zarządzania, m.in. Zarządzanie użytkownikami i zarządzanie jednostki. Omawiane kwestie:

> [!div class="checklist"]
> * Utworzyć użytkownika mającego uprawnienia administratora
> * Utwórz użytkownika z dostępem użytkowników
> * Utwórz jednostki



## <a name="create-a-user-with-admin-access"></a>Utworzyć użytkownika mającego uprawnienia administratora

Mimo że masz już dostępu z uprawnieniami administratora, współpracowników w Twojej organizacji może również musi być dostępu z uprawnieniami administratora. W portalu Cloudyn kliknij koło zębate symbol w górnym prawym rogu i wybierz pozycję **Zarządzanie użytkownikami**. Kliknij przycisk **Dodaj nowego użytkownika** można dodać nowego użytkownika.

Wprowadź wymagane informacje o użytkowniku. Pole hasła może pozostać puste, dzięki czemu użytkownik może ustawić nowe hasło pierwszego znaku w. Łącze o informacje logowania są wysyłane do użytkownika za pośrednictwem poczty e-mail z Cloudyn po wybraniu **użytkownika powiadamiania pocztą e-mail**. Wybierz uprawnienia, aby umożliwić zarządzanie użytkownikami, aby użytkownik można tworzyć i modyfikować innych użytkowników. Wyświetla listę Zarządzanie adresatami umożliwia użytkownikowi edytowanie listy adresatów.

W obszarze **użytkownik ma dostęp administratora**, wybrano jednostki głównego Twojej organizacji. Pozostaw zaznaczone głównego, a następnie zapisz informacje o użytkowniku. Wybranie jednostki głównego umożliwia użytkownikowi uprawnień administratora nie tylko do obiektu głównego drzewa, ale także do wszystkich jednostek, które znajdują się poniżej.  
  ![Dodaj nowego użytkownika z uprawnienia administratora](.\media\tutorial-user-access\new-admin-access.png)

## <a name="create-a-user-with-user-access"></a>Utwórz użytkownika z dostępem użytkowników
Typowy użytkowników, którzy potrzebują dostępu do danych zarządzania kosztów, takich jak pulpity nawigacyjne i raporty powinny mieć dostęp użytkownika do ich wyświetlania. Utwórz nowego użytkownika z dostępem użytkowników podobny do tego, który został utworzony przy użyciu dostępu administratora, z następującymi różnicami:

- Wyczyść **umożliwia zarządzanie użytkownikami**, **Zezwalaj odbiorcy wymieniono zarządzania**i Wyczyść wszystko w **użytkownik ma dostęp administratora** listy.
- Zaznacz obiekty, że użytkownik musi mieć dostęp do w **użytkownik ma dostęp użytkownika** listy.
- Możesz również zezwolić administratorem w celu uzyskania dostępu do konkretnych obiektów, zgodnie z potrzebami.

![Dodaj nowego użytkownika z dostępem użytkowników](.\media\tutorial-user-access\new-user-access.png)

Aby obejrzeć film samouczka temat dodawania użytkowników, zobacz [Dodawanie użytkowników do usługi Azure Management koszt przez Cloudyn](https://youtu.be/Nzn7GLahx30).

## <a name="create-entities"></a>Utwórz jednostki

Podczas definiowania hierarchii kosztów, najlepszym rozwiązaniem jest określenie struktury organizacji.

Podczas tworzenia drzewa, należy rozważyć sposób mają lub chcesz wyświetlać swoje koszty rozdzielonych według jednostek biznesowych, kosztów centrów, środowisk i działów sprzedaży. Drzewo jednostki w Cloudyn jest elastyczny dziedziczenia jednostki. Poszczególne subskrypcje dla kont chmurze są połączone z konkretnych obiektów. Tak jednostki są wielodostępnej. Dostęp do określonych użytkowników można przypisać do ich segment firmy przy użyciu jednostek. W ten sposób przechowuje dane samodzielnie, nawet w różnych części dużych firm, takich jak podmioty zależne. I ułatwia zarządzanie izolacji danych.  

Po zarejestrowaniu z umową Azure lub konto z Cloudyn dane zasobów platformy Azure, łącznie z użycia i wydajności, rozliczeń i dane znacznika z subskrypcji został skopiowany do swojego konta Cloudyn. Jednak należy ręcznie utworzyć swoje drzewo jednostki. Jeśli została pominięta rejestracji usługi Azure Resource Manager, następnie tylko danych dotyczących rozliczeń i kilka raportów zasobów są dostępne w portalu Cloudyn.

W portalu Cloudyn kliknij **ustawienia** w górnym prawym rogu i wybierz pozycję **kont chmury**. Rozpocznij pojedynczej jednostki (root) i kompilacji z drzewa jednostki w katalogu głównym. Oto przykład hierarchii jednostek, który może wyglądać organizacje IT po zakończeniu drzewa:

![drzewo jednostki](.\media\tutorial-user-access\entity-tree.png)

Obok pozycji **jednostek**, kliknij przycisk **Dodaj jednostki**. Wprowadź informacje o osobie lub działu, które chcesz dodać. **Imię i nazwisko** i **E-mail** pola, aby nie musi odpowiadać istniejących użytkowników. Jeśli chcesz wyświetlić listę poziomy dostępu, wyszukiwanie pomocy dotyczącej *Dodawanie jednostki*.

![Dodawanie jednostki](.\media\tutorial-user-access\add-entity.png)

Gdy wszystko będzie gotowe, **zapisać** jednostki.


Aby obejrzeć film samouczka tworzenie hierarchii jednostek koszt informacje, zobacz [tworzenie hierarchii jednostek kosztów w Azure koszt zarządzania, Cloudyn](https://youtu.be/dAd9G7u0FmU).

Jeśli jesteś użytkownikiem Azure Enterprise Agreement, obejrzyj film samouczka o sposobie kojarzenia konta i subskrypcji do jednostek na [łączenie usługi Azure Resource Manager z usługą Azure Management koszt przez Cloudyn](https://youtu.be/oCIwvfBB6kk).

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utworzyć użytkownika mającego uprawnienia administratora
> * Utwórz użytkownika z dostępem użytkowników
> * Utwórz jednostki

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak i prognozowanie wydatków przy użyciu danych historycznych.

> [!div class="nextstepaction"]
> [Prognozowanie wydatków w przyszłości](tutorial-forecast-spending.md)
