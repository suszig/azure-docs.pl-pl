---
title: "Publikowanie wersji interfejsu API przy użyciu usługi Azure API Management | Dokumentacja firmy Microsoft"
description: "Wykonaj kroki tego samouczka, aby dowiedzieć się, jak opublikować wiele wersji w usłudze API Management."
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
ms.openlocfilehash: 7c355e2feb5ebe5971d8391b326422a1abec1497
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="publish-multiple-versions-of-your-api-in-a-predictable-way"></a>Publikowanie wielu wersji interfejsu API w sposób przewidywalne
W tym samouczku opisano, jak skonfigurować wersji interfejsu API, a następnie wybrać sposób są one nazywane przez deweloperów interfejsu API.

## <a name="prerequisites"></a>Wymagania wstępne
Do ukończenia tego samouczka, należy utworzyć interfejsu API usługi zarządzania i mieć istniejącego interfejsu API można zmienić (zamiast interfejsu API konferencyjnej).

## <a name="about-versions"></a>Informacje dotyczące wersji
Czasami jest niemożliwe do ma wszystkie obiekty wywołujące do interfejsu API używać tej samej wersji. Czasami chcesz opublikować nowe lub inne funkcje interfejsu API w przypadku niektórych użytkowników, podczas gdy inne korzystać z interfejsu API, który jest obecnie obsługiwane dla nich. Podczas wywoływania konieczne jest uaktualnienie do nowszej wersji, chcą mieć możliwość zrobić to przy użyciu łatwy do zrozumienia podejście.  Firma Microsoft może to zrobić za pomocą **wersji** w usłudze Azure API Management.

## <a name="walkthrough"></a>Przewodnik
W tym przewodniku dodamy nową wersję istniejącego interfejsu API, wybierając identyfikator wersji schematu i wersji.

## <a name="add-a-new-version"></a>Dodaj nową wersję
![Menu kontekstowe interfejsu API — Dodaj wersji](media/api-management-getstarted-publish-versions/AddVersionMenu.png)
1. Przejdź do **interfejsów API** strony w ramach usługi Zarządzanie interfejsami API w portalu Azure.
2. Wybierz **API konferencji** z listy interfejsu API, a następnie wybierz menu kontekstowe (**...** ) obok niej.
3. Wybierz **+ Dodaj wersję**.

    > [!TIP]
    > Po utworzeniu nowego interfejsu API — można również włączyć wersji wybierz **wersji tego interfejsu API?** na **dodać interfejsu API** ekranu.

## <a name="choose-a-versioning-scheme"></a>Wybierz system kontroli wersji
Zarządzanie interfejsami API Azure można wybrać sposób Zezwól wywołań określić, która wersja interfejsu API one. Można to zrobić, wybierając **schemat przechowywania wersji**. Ten program może być albo **ciągu kwerendy, nagłówek lub ścieżka**. W tym przykładzie używamy ścieżki.
![Dodaj wersji ekranu](media/api-management-getstarted-publish-versions/AddVersion.PNG)
1. Pozostaw **ścieżki** wybrany jako sieci **schemat przechowywania wersji**.
2. Dodaj **v1** jako sieci **identyfikator wersji**.

    > [!TIP]
    > W przypadku wybrania **nagłówka** lub **ciągu zapytania** schemat przechowywania wersji, należy podać wartość dodatkowe — nazwa nagłówka lub parametr ciągu zapytania.

3. Podaj opis, jeśli chcesz.
4. Wybierz **Utwórz** do skonfigurowania nowej wersji.
5. Underneath **Big API konferencji** na liście interfejsu API, pojawi się dwóch odrębnych interfejsów API - **oryginalnego**, i **v1**.
![Wersje kategorii interfejsu API w portalu Azure](media/api-management-getstarted-publish-versions/VersionList.PNG)

    > [!Note]
    > Jeśli dodasz wersji bez numerów wersji interfejsu API, zawsze utworzymy **oryginalnego** - Ci odpowiadać na domyślny adres URL. Daje to pewność, że wszystkie istniejące elementy wywołujące nie są dzielone przez proces dodawania wersji. Jeśli tworzysz nowy interfejs API z wersjami włączone na początku nie zostanie utworzony oryginalny.

6. Teraz możesz edytować i skonfigurować **v1** jako interfejs API, który jest całkowicie osobnej do **oryginalnego**. Zmiany w jednej wersji nie wpływają na inny.

## <a name="add-the-version-to-a-product"></a>Dodaj wersję produktu
Dla wywołań zobaczyć nowej wersji, musi zostać dodany do **produktu** (produkty nie są dziedziczone z nadrzędnej wersji).

1. Wybierz **produktów** na stronie usługi zarządzania.
2. Wybierz **nieograniczone**.
3. Wybierz **interfejsów API**.
4. Wybierz pozycję **Dodaj**.
5. Wybierz **konferencji interfejsu API w wersji v1**.
6. Wróć do strony usługi zarządzania i wybierz **interfejsów API**.

## <a name="browse-the-developer-portal-to-see-the-version"></a>Przeglądaj, aby wyświetlić wersję portalu dla deweloperów
1. Wybierz **portalu dla deweloperów** z górnego menu.
2. Wybierz **interfejsów API**, zwróć uwagę, że **API konferencji** pokazuje **oryginalnego** i **v1** wersji.
3. Wybierz **v1**.
4. Powiadomienie **żądanie adresu URL** pierwszej operacji na liście. Pokazuje, że ścieżka URL interfejsu API zawiera **v1**.
![Wersja wyświetlane w portalu dla deweloperów](media/api-management-getstarted-publish-versions/VersionDevPortal.PNG)
