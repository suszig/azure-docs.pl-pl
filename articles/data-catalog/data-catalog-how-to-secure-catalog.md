---
title: "Jak zabezpieczyć dostęp do usługi Azure Data Catalog | Dokumentacja firmy Microsoft"
description: "W tym artykule wyjaśniono, jak zabezpieczyć dane katalogu i jego zasobów danych."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: 89346113c6231442beb1147c4c4fea524d03d909
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-secure-access-to-data-catalog-and-data-assets"></a>Jak zabezpieczyć dostęp do katalogu danych i zasobów danych
> [!IMPORTANT]
> Ta funkcja jest dostępna tylko w wersji standard usługi Azure Data Catalog.

Wykaz danych Azure służy do określania, kto ma dostęp do katalogu danych i jakie operacje (rejestrowanie, dodawanie adnotacji, przejąć na własność) mogą dotyczyć metadanych w katalogu. 

## <a name="catalog-users-and-permissions"></a>Katalogu użytkowników i uprawnień
Aby podać użytkownik lub Grupa dostępu do wykazu danych i ustawić uprawnienia:

1. Na [strony głównej wykazu danych](http://www.azuredatacatalog.com), kliknij przycisk **ustawienia** na pasku narzędzi.

    ![Data catalog — ustawienia](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. Na stronie ustawień, rozwiń węzeł **użytkownicy wykazu** sekcji.
    ![W katalogu użytkowników — Dodaj](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)
3. Kliknij pozycję **Add** (Dodaj).
4. Wprowadź w pełni kwalifikowaną **nazwy użytkownika** lub nazwa **grupy zabezpieczeń** w usłudze Azure Active Directory (AAD) skojarzone z katalogiem. Użyj przecinka (",") jako separatora, dodając więcej niż jednego użytkownika lub grupę.
    ![Użytkownicy wykazu - użytkowników lub grup](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)
5. Naciśnij klawisz **ENTER** lub **kartę** poza pola tekstowego. 
6.  Upewnij się, że wszystkie uprawnienia (**Adnotuj**, **zarejestrować**, i **Przejmij na własność**) są domyślnie przypisane do tych użytkowników lub grup. Oznacza to, że użytkownik lub grupa może [zarejestrować zasobów danych]( data-catalog-how-to-register.md), [adnotacji zasobów danych]( data-catalog-how-to-annotate.md), i [przejąć na własność zasobów danych]( data-catalog-how-to-manage.md). 
    ![Użytkownicy wykazu - domyślnych uprawnień](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)
7.  Aby dać użytkownikowi lub grupie dostęp tylko do odczytu do katalogu, wyczyść **adnotacji** opcji dla tego użytkownika lub grupy. Można to zrobić, użytkownik lub grupa nie adnotacji zasobów danych w katalogu, ale mogą je wyświetlać. 
8.  Aby uniemożliwić użytkownikowi lub grupie zasobów danych rejestrowania, wyczyść **zarejestrować** opcji dla tego użytkownika lub grupy.
9.  Aby zablokować użytkownika z przejmowania własności zasobów danych, wyczyść **przejąć na własność** opcji dla tego użytkownika lub grupy. 
10. Aby usunąć użytkownika/grupy użytkowników z katalogu, kliknij przycisk **x**  /grupy użytkowników w dolnej części listy. 
    ![Użytkownicy w katalogu — Usuń użytkownika](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)

    > [!IMPORTANT]
    > Firma Microsoft zaleca, aby dodać grupy zabezpieczeń, aby bezpośrednio katalogu użytkowników, zamiast dodawania użytkowników i przypisać uprawnienia. Następnie należy dodać użytkowników do grup zabezpieczeń, które odpowiada ich ról oraz ich wymagany dostęp do katalogu.

## <a name="special-considerations"></a>Uwagi

- Uprawnienia przypisane do grupy zabezpieczeń są dodatku. Użytkownik jest w dwóch grupach. Jedna grupa ma adnotacji uprawnienia i nie mają adnotacje do innej grupy uprawnień. Następnie użytkownik ma adnotacji uprawnienia. 
- Uprawnienia jawnie przypisane do użytkownika musi zostać zastąpiona uprawnienia przypisane do grup, do których należy użytkownik. W poprzednim przykładzie powiedzieć jawnie dodawania użytkownika do katalogu użytkowników i czy nie przypisuj uprawnienia funkcji dodawania adnotacji. Użytkownik nie adnotacji zasobów danych, nawet jeśli użytkownik jest członkiem grupy, która ma adnotacji uprawnienia.

## <a name="next-steps"></a>Kolejne kroki
- [Rozpoczynanie pracy z usługą Azure Data Catalog](data-catalog-get-started.md)

