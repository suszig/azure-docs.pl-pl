---
title: "Umożliwia zmianę nierozdzielające bezpiecznie w usłudze Azure API Management poprawki | Dokumentacja firmy Microsoft"
description: "Wykonaj kroki tego samouczka, aby dowiedzieć się, jak wprowadzać zmiany nierozdzielające przy użyciu zmian w usłudze API Management."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 50d7ac17faebb34f1a1f9a3259aa0196950391d9
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="use-revisions-to-make-non-breaking-changes-safely"></a>Do wprowadzania zmian nierozdzielające bezpiecznie używać poprawki
Gdy interfejs API jest gotowa do użytku i uruchamia do użycia przez deweloperów, zazwyczaj należy zachować ostrożność w wprowadzania zmian do tego interfejsu API i w tym samym czasie, aby nie przerywać wywołań interfejsu API. Jest również przydatne może być umożliwienie deweloperom wiedzieć o wprowadzone zmiany. Firma Microsoft można to zrobić za pomocą usługi Azure API Management **poprawki**. Aby uzyskać więcej informacji, zobacz [wersje & poprawki](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/) i [przechowywanie wersji interfejsu API usługi Azure API Management](https://blogs.msdn.microsoft.com/apimanagement/2017/09/13/api-versioning-with-azure-api-management/).

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dodawanie nowej wersji
> * Zmiany nierozdzielające poprawek
> * Wprowadź bieżący poprawek i Dodaj wpis dziennika zmian
> * Przeglądaj, aby zobaczyć zmiany i dziennika zmian portalu dla deweloperów

![Dziennik zmian w portalu dla deweloperów](media/api-management-getstarted-revise-api/azure_portal.PNG)

## <a name="prerequisites"></a>Wymagania wstępne

+ Ukończenie następujących Szybki Start: [utworzenia wystąpienia usługi Azure API Management](get-started-create-service-instance.md).
+ Ponadto Ukończ samouczek następujących: [Import i opublikować swój pierwszy interfejs API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-a-new-revision"></a>Dodawanie nowej wersji

1. Wybierz **interfejsów API** strony.
2. Wybierz **API konferencji** z listy interfejsu API (lub innego interfejsu API, do której chcesz dodać poprawek).
3. Kliknij przycisk **poprawki** kartę z menu w górnej części strony.
4. Wybierz **+ Dodaj poprawki**

    > [!TIP]
    > Można również wybrać **dodać poprawki** w menu kontekstowym (**...** ) interfejsu API.
    
    ![Poprawki menu u góry ekranu](media/api-management-getstarted-revise-api/TopMenu.PNG)

5. Podaj opis dla nowego poprawek, ułatwiające należy pamiętać, co będzie służyć do.
6. Wybierz **tworzenie**
7. Został utworzony z nowej wersji.

    > [!NOTE]
    > Oryginalny interfejs API pozostaje w **Revision 1**. Jest to poprawki, użytkownicy nadal wywołać, dopóki nie wybierzesz należy do innej wersji bieżącej.

## <a name="make-non-breaking-changes-to-your-revision"></a>Zmiany nierozdzielające poprawek

1. Wybierz **API konferencji** z listy interfejsu API.
2. Wybierz **projekt** kartę w górnej części ekranu.
3. Zwróć uwagę, że **selektora poprawki** (bezpośrednio nad karty Projekt) zawiera Twojej bieżącej wersji **2 poprawki**.

    > [!TIP]
    > Umożliwia przełączanie między poprawki, które mają być używane w selektorze poprawki.

4. Wybierz **+ Dodaj operację**.
5. Ustaw nowe operacji jako **POST**, nazw i nazwę wyświetlaną operacji jako **testu**
6. **Zapisz** nowej operacji.
7. Teraz wprowadzono zmianę **2 poprawki**. Użyj **selektora poprawki** górnej części strony, aby wrócić do **Revision 1**.
8. Należy zauważyć, że nie ma nowych operacji **Revision 1**. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Wprowadź bieżący poprawek i Dodaj wpis dziennika zmian
1. Wybierz **poprawki** kartę z menu w górnej części strony.

    ![Menu poprawki na ekranie poprawki.](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)
1. Otwórz menu kontekstowe (**...** ) dla **wersji 2**.
2. Wybierz **stać się bieżącym**. Sprawdź **Post do publicznego zmienić dziennika dla tego interfejsu API**, aby uwagi dotyczące tej zmiany.
3. Wybierz **Post na dziennik zmian publicznego dla tego interfejsu API**
4. Wprowadź opis wyświetlany deweloperów, na przykład zmiany **testowania poprawki. Dodano nową "test" operację.**
5. **Poprawka 2** teraz jest aktualny.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Przeglądaj, aby zobaczyć zmiany i dziennika zmian portalu dla deweloperów
1. W portalu Azure wybierz **interfejsów API**
2. Wybierz **portalu dla deweloperów** z górnego menu.
3. Wybierz **interfejsów API**, a następnie wybierz **API konferencji**.
4. Zwróć uwagę, nowej **test** operacji jest teraz dostępna.
5. Wybierz **historii zmian interfejsu API** widoczna nazwa interfejsu API.
6. Należy zauważyć, że wpis dziennika zmian pojawia się na tej liście.

    ![Portal dla deweloperów](media/api-management-getstarted-revise-api/developer_portal.PNG)

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dodawanie nowej wersji
> * Zmiany nierozdzielające poprawek
> * Wprowadź bieżący poprawek i Dodaj wpis dziennika zmian
> * Przeglądaj, aby zobaczyć zmiany i dziennika zmian portalu dla deweloperów

Przejdź do następnego samouczek:

> [!div class="nextstepaction"]
> [Publikowanie wielu wersji interfejsu API](api-management-get-started-publish-versions.md)