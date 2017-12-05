---
title: "Publikowanie wersji interfejsu API przy użyciu usługi Azure API Management | Dokumentacja firmy Microsoft"
description: "Wykonaj kroki tego samouczka, aby dowiedzieć się, jak opublikować wiele wersji w usłudze API Management."
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
ms.openlocfilehash: d63bdd3110f5c5db3e7bfec424644fdbc8d8d90c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="publish-multiple-versions-of-your-api"></a>Publikowanie wielu wersji interfejsu API 

Istnieją w czasie gdy jest niemożliwe do ma wszystkie obiekty wywołujące do używania interfejsu API dokładnie takiej samej wersji. Czasami chcesz opublikować nowe lub inne funkcje interfejsu API w przypadku niektórych użytkowników, podczas gdy inne korzystać z interfejsu API, który jest obecnie obsługiwane dla nich. Podczas wywoływania konieczne jest uaktualnienie do nowszej wersji, chcą mieć możliwość zrobić to przy użyciu łatwy do zrozumienia podejście.  Firma Microsoft może to zrobić za pomocą **wersji** w usłudze Azure API Management. Aby uzyskać więcej informacji, zobacz [wersje & poprawki](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/).

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dodaj nową wersję istniejącego interfejsu API
> * Wybieranie wersji schematu
> * Dodaj wersję produktu
> * Przeglądaj, aby wyświetlić wersję portalu dla deweloperów

![Wersja wyświetlane w portalu dla deweloperów](media/api-management-getstarted-publish-versions/azure_portal.PNG)

## <a name="prerequisites"></a>Wymagania wstępne

+ Ukończenie następujących Szybki Start: [utworzenia wystąpienia usługi Azure API Management](get-started-create-service-instance.md).
+ Ponadto Ukończ samouczek następujących: [Import i opublikować swój pierwszy interfejs API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-a-new-version"></a>Dodaj nową wersję

![Menu kontekstowe interfejsu API — Dodaj wersji](media/api-management-getstarted-publish-versions/AddVersionMenu.png)

1. Wybierz **API konferencji** z listy interfejsu API.
2. Wybierz menu kontekstowe (**...** ) obok niej.
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

6. Teraz możesz edytować i skonfigurować **v1** jako interfejs API, który różni się **oryginalnego**. Zmiany w jednej wersji nie wpływają na inny.

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

    ![Menu kontekstowe interfejsu API — Dodaj wersji](media/api-management-getstarted-publish-versions/developer_portal.png)

## <a name="next-steps"></a>Następne kroki

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dodaj nową wersję istniejącego interfejsu API
> * Wybieranie wersji schematu 
> * Dodaj wersję produktu
> * Przeglądaj, aby wyświetlić wersję portalu dla deweloperów

Przejdź do następnego samouczek:

> [!div class="nextstepaction"]
> [Uaktualnienie i skalowanie](upgrade-and-scale.md)