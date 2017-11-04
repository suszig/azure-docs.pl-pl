---
title: "Zapisywanie wyszukiwań i numeru pin zasobów danych w usłudze Azure Data Catalog | Dokumentacja firmy Microsoft"
description: "Artykule wyróżnianie możliwości usługi Azure Data Catalog dla źródeł danych i zasobów danych do późniejszego użycia."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6bd00a81-820d-4b7c-91fa-ab09e575474c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 11/01/2017
ms.author: maroche
ms.openlocfilehash: ec28a9873b6a1d60ee3c13a18a0c68a24e8a067c
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a>Zapisz wyszukiwanie i zasobów danych numeru pin w wykazie danych Azure
## <a name="introduction"></a>Wprowadzenie
Azure Data Catalog oferuje możliwości dla odnajdywanie źródła danych. Można szybko wyszukiwanie i filtrowanie katalogu odnajdywania źródeł danych i zrozumienie ich przeznaczenie, dzięki czemu łatwiej znaleźć odpowiednie dane od rodzaju zadania.

Ale co zrobić, jeśli należy regularnie pracować z tymi samymi danymi? I co zrobić, jeśli użytkownicy będą mieli regularnie współtworzenia swoją wiedzę do tych samych źródeł danych w katalogu? W takich przypadkach wielokrotnie wydania tej samej operacji wyszukiwania może być mało wydajne. Jest to, gdzie zapisanego wyszukiwania i zasobów danych przypięte może pomóc.

## <a name="saved-searches"></a>Zapisane wyszukiwania
Zapisane wyszukiwanie w wykazie danych jest wielokrotnego użytku, definicji wyszukiwania dla poszczególnych użytkowników. Można zdefiniować wyszukiwania, w tym terminy wyszukiwania, znaczników i inne filtry i zapisz go. Można ponownie uruchomić zapisane wyszukiwanie definicji później, aby zwrócić żadnych zasobów danych, które pasują do jej kryteriów wyszukiwania.

### <a name="create-a-saved-search"></a>Utwórz zapisanego wyszukiwania
Aby utworzyć zapisane wyszukiwanie, wykonaj następujące czynności:
1. W portalu Azure Data Catalog w **bieżące wyszukiwanie** okna, kliknij przycisk **zapisać**. 

    ![Bieżące łącze Zapisz ustawienia wyszukiwania](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. Wprowadź kryteria wyszukiwania, które chcesz ponownie użyć, a następnie kliknij przycisk **zapisać**.

    ![Nazwa wyszukiwania zapisać bieżące ustawienia wyszukiwania](./media/data-catalog-how-to-save-pin/02-name.png)

3. Po wyświetleniu monitu wprowadź nazwę dla zapisanego wyszukiwania. Wybierz nazwę opisową i opisujący zasobów danych, które zostaną zwrócone przez wyszukiwanie.

### <a name="manage-saved-searches"></a>Zarządzanie zapisanych wyszukiwań
Po zapisaniu jednego lub więcej wyszukiwań **zapisane wyszukiwania** opcja jest wyświetlana poniżej **bieżące wyszukiwanie** pole. Po rozwinięciu listy są wyświetlane wszystkie zapisane wyszukiwania.

 ![Lista zapisanych wyszukiwań](./media/data-catalog-how-to-save-pin/03-list.png)

Wykonaj jedną z następujących czynności:

* Aby wykonać wyszukiwanie, wybierz zapisanego kryterium wyszukiwania z listy.

* Aby wyświetlić listę opcji zarządzania dla zapisanego wyszukiwania, kliknij strzałkę w dół obok nazwy wyszukiwania.

    ![Opcje zarządzania zapisanych wyszukiwań](./media/data-catalog-how-to-save-pin/04-managing.png)

* Aby wprowadzić nową nazwę dla zapisanego wyszukiwania, wybierz **zmienić**. Definicja wyszukiwania nie ulega zmianie.

* Aby usunąć zapisanego wyszukiwania z listy, wybierz **usunąć**, a następnie Potwierdź usunięcie.

* Aby oznaczyć zapisane wyszukiwanie jako wyszukiwania domyślne, wybierz **Zapisz jako domyślne**. Jeśli wyszukiwaniu "pusty" na stronie głównej usługi Azure Data Catalog, wyszukiwanie domyślny jest wykonywana. Ponadto wyszukiwania, który jest oznaczony jako domyślne wyszukiwanie jest wyświetlany w górnej części **zapisane wyszukiwania** listy.

### <a name="organizational-saved-searches"></a>Organizacyjnej zapisanych wyszukiwań
Wszystkich użytkowników w organizacji można zapisać wyszukiwanie na własny użytek. Administratorzy katalogu danych można także zapisać wyszukiwanie wszystkich użytkowników w organizacji. Gdy Administratorzy, Zapisz wyszukiwanie, są one dostępne z **udziału w firmie** opcji. Ta opcja udostępnia zapisanego wyszukiwania dla wszystkich użytkowników w organizacji.

 ![Organizacyjnej zapisanych wyszukiwań](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a>Zasoby danych przypięte
Z zapisane wyszukiwania można zapisać i ponownie użyć definicji wyszukiwania. Zasoby danych, które są zwracane przez operacji wyszukiwania może zmieniać się w czasie, gdy zawartość zmiany katalogu. Po przypięciu zasobów danych, można zidentyfikować jawnie określonych danych zasoby, aby ułatwić im dostęp bez konieczności korzystania z wyszukiwania.

Przypinanie zasobu danych jest prosta. Aby dodać zasobów danych, do listy przypiętych, po prostu kliknij **numeru pin** ikony. Ikona jest wyświetlana w rogu zasobów kafelka w widoku tile i w lewej kolumnie w widoku listy w portalu wykazu danych Azure.

![Ikonę pinezki zasobów danych](./media/data-catalog-how-to-save-pin/05-pinning.png)

Cofnięcie unieruchomienia zasobu danych jest równie proste. Po prostu kliknij **odpiąć** ikonę, aby zmienić ustawienie dla wybranego elementu zawartości.

![Ikona odpiąć zasobów danych](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="the-my-assets-section"></a>W sekcji Moje zasoby
Strona główna portalu wykazu danych zawiera **Moje zasoby** sekcja, która zawiera zasoby, przydatne do bieżącego użytkownika. Ta sekcja zawiera oba zasoby przypięty i zapisane wyszukiwania.

![W sekcji Moje zasoby na stronie głównej](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Podsumowanie
Wykaz danych Azure oferuje możliwości, które ułatwiają odnajdywanie źródeł danych, które są potrzebne, innych członków organizacji może poświęcać mniej czasu wyszukiwania danych i więcej czasu korzystania z niego. Zapisane wyszukiwania i przypięty dane, które zasoby kompilacji na tych podstawowych możliwości, aby użytkownicy mogą łatwo zidentyfikować źródeł danych, które działają w systemie wielokrotnie.
