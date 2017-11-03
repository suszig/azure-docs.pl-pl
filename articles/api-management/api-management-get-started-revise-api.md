---
title: "Użyj wersji, aby zaktualizować interfejs API usługi Azure API Management | Dokumentacja firmy Microsoft"
description: "Wykonaj kroki tego samouczka, aby dowiedzieć się, jak wprowadzać zmiany nierozdzielające przy użyciu zmian w usłudze API Management."
services: api-management
documentationcenter: 
author: mattfarm
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/18/2017
ms.author: apimpm
ms.openlocfilehash: 0d67166a16ae4d640080ad83e7625e594b0dc246
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="make-non-breaking-changes-safely-using-revisions"></a>Wprowadź zmiany nierozdzielające bezpiecznie za pomocą poprawki
Ten przewodnik opisuje sposób zmian do interfejsu API bezpiecznie i przekazać zmiany deweloperów.

## <a name="prerequisites"></a>Wymagania wstępne
Do ukończenia tego samouczka, należy utworzyć interfejsu API usługi zarządzania i mieć istniejącego interfejsu API można zmienić (zamiast interfejsu API konferencyjnej).

## <a name="about-revisions"></a>O poprawki
Gdy interfejs API jest gotowa do użytku i uruchamia do użycia przez deweloperów, zazwyczaj należy zachować ostrożność w wprowadzania zmian do tego interfejsu API — w celu zakłócać wywołań interfejsu API. Jest również przydatne może być umożliwienie deweloperom wiedzieć o wprowadzone zmiany. Firma Microsoft można to zrobić za pomocą usługi Azure API Management **poprawki**.

## <a name="walkthrough"></a>Przewodnik
W tym przewodniku możemy dodać nową wersję, operacja dodania do niego, a następnie stać się bieżącym — tworzenie wpis dziennika zmian w takim przypadku tej poprawki.

## <a name="add-a-new-revision"></a>Dodawanie nowej wersji
1. Przejdź do **interfejsów API** strony w ramach usługi Zarządzanie interfejsami API w portalu Azure.
2. Wybierz **API konferencji** interfejsu API wybierz z listy, następnie **poprawki** kartę z menu w górnej części strony.
3. Wybierz **+ Dodaj poprawki**

    > [!TIP]
    > Można również wybrać **dodać poprawki** w menu kontekstowym (**...** ) w interfejsie API.
![Poprawki menu u góry ekranu](media/api-management-getstarted-revise-api/TopMenu.PNG)

4. Podaj opis dla nowego poprawek, ułatwiające należy pamiętać, co będzie służyć do.
5. Wybierz **tworzenie**
6. Został utworzony z nowej wersji.

    > [!NOTE]
    > Oryginalny API pozostaje w **Revision 1**. Jest to poprawki, które użytkownicy będą w dalszym ciągu wywołana, dopóki nie wybierzesz należy do innej wersji bieżącej.

## <a name="make-non-breaking-changes-to-your-revision"></a>Zmiany nierozdzielające poprawek
1. Wybierz **projekt** kartę w górnej części ekranu.
2. Zwróć uwagę, że **selektora poprawki** (bezpośrednio nad karty Projekt) zawiera Twojej bieżącej wersji **2 poprawki**.

    > [!TIP]
    > Umożliwia przełączanie między poprawki, które mają być używane w selektorze poprawki.

3. Wybierz **+ Dodaj operację**.
4. Ustaw nowe operacji jako **POST**, nazw i nazwę wyświetlaną operacji jako **opinii**
5. **Zapisz** nowej operacji.
6. Teraz wprowadzono zmianę **2 poprawki**. Użyj **selektora poprawki** górnej części strony, aby wrócić do **Revision 1**.
7. Należy zauważyć, że nie ma nowych operacji **Revision 1**. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Wprowadź bieżący poprawek i Dodaj wpis dziennika zmian
1. Wybierz **poprawki** kartę z menu w górnej części strony.
![Menu poprawki na ekranie poprawki.](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)
2. Otwórz menu kontekstowe (**...** ) dla **wersji 2**.
3. Wybierz **stać się bieżącym**.
![Należy bieżącej wersji, a następnie wysyłać do dziennika zmian](media/api-management-getstarted-revise-api/MakeCurrent.PNG)
4. Wybierz **Post na dziennik zmian publicznego dla tego interfejsu API**
5. Wprowadź opis wyświetlany deweloperów, na przykład zmiany **"Dodać nową operację opinii."**
6. **Poprawka 2** teraz jest aktualny.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Przeglądaj, aby zobaczyć zmiany i dziennika zmian portalu dla deweloperów
1. Wybierz **portalu dla deweloperów** z górnego menu.
2. Wybierz **interfejsów API**, a następnie wybierz **API konferencji**.
3. Zwróć uwagę, nowej **opinii** operacji jest teraz dostępna.
4. Wybierz **historii zmian interfejsu API** widoczna nazwa interfejsu API.
5. Należy zauważyć, że wpis dziennika zmian pojawia się na tej liście.
![Dziennik zmian w portalu dla deweloperów](media/api-management-getstarted-revise-api/ChangeLogDevPortal.PNG)

## <a name="next-steps"></a>Następne kroki
[Publikowanie wersji interfejsu API z usługi Azure API Management](#api-management-getstarted-publish-versions.md)