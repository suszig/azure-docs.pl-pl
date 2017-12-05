---
title: "Dostosować styl strony w portalu dla deweloperów usługi Azure API Management | Dokumentacja firmy Microsoft"
description: "Wykonaj kroki tego przewodnika Szybki Start, aby dostosować styl elementów w portalu dla deweloperów usługi Azure API Management."
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
ms.openlocfilehash: f427663ba1c437785c8c521925d9f733c45cb40d
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="customize-the-style-of-the-developer-portal-pages"></a>Dostosować styl strony portalu deweloperów

Istnieją trzy sposoby najczęściej używane do dostosowywania portalu dla deweloperów w usłudze Azure API Management:
 
* [Edytuj zawartość strony statyczne i elementy układ strony](api-management-modify-content-layout.md)
* Aktualizuj style używany do elementów strony w portalu dla deweloperów (co omówiono w tym przewodniku)
* [Modyfikowanie szablonów używany dla stron wygenerowanych przez portal](api-management-developer-portal-templates.md) (na przykład dokumentacja interfejsu API, produktów, uwierzytelnianie użytkowników)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dostosowywanie stylu elementów na stronach **Developer** portalu
> * Wyświetl zmiany

![Dostosowywanie stylów](./media/modify-developer-portal-style/developer_portal.png)

## <a name="prerequisites"></a>Wymagania wstępne

+ Ukończenie następujących Szybki Start: [utworzenia wystąpienia usługi Azure API Management](get-started-create-service-instance.md).
+ Ponadto Ukończ samouczek następujących: [Import i opublikować swój pierwszy interfejs API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="customize-the-developer-portal"></a>Dostosowywanie portalu dla deweloperów

1. Wybierz **omówienie**.
2. Kliknij przycisk **portalu dla deweloperów** przycisk na wierzchu z **omówienie** okna. Alternatywnie możesz kliknąć **portalu dla deweloperów adresu URL** łącza.
3. Po lewej stronie górnej części ekranu widoczna ikona składającej się z dwóch pędzli. Umieść kursor nad ikonę, aby otworzyć menu dostosowywania portalu.

    ![Dostosowywanie stylów](./media/modify-developer-portal-style/modify-developer-portal-style01.png)
4. Wybierz **style** z menu, aby otworzyć okienko Dostosowywanie stylów.

    Wszystkie elementy, które można dostosować za pomocą **style** wyświetlane na stronie
5. Wprowadź "nagłówki color" w **zmieniać wartości zmiennych, aby dostosować wygląd portalu deweloperów:** pola.

     **@headings-color**  Element jest wyświetlany na stronie. Ta zmienna Określa kolor tekstu.

    ![Dostosowywanie stylów](./media/modify-developer-portal-style/modify-developer-portal-style02.png)
    
6. Kliknij pole  **@headings-color**  zmiennej. 
    
    Otwiera próbnika kolorów listy rozwijanej.
7. Wybierz nowy kolor z selektorami kolorów listy rozwijanej.

    > [!TIP]
    > W czasie rzeczywistym Podgląd jest dostępny dla wszystkich zmian. W górnej części okienka dostosowywania wyświetlany jest wskaźnik postępu. Po kilku sekundach tekst nagłówka zmiany w kolorze nowo wybrane.

8. Wybierz **publikowania** z lewym dolnym w menu dostosowywania okienka.
9. Wybierz **opublikować dostosowania** publicznie udostępniać zmiany.

## <a name="view-your-change"></a>Wyświetl zmiany

1. Przejdź do portalu dla deweloperów.
2. Widać wprowadzone zmiany.

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dostosowywanie stylu elementów na stronach **Developer** portalu
> * Wyświetl zmiany

> [!div class="nextstepaction"]
> [Dostosowywanie portalu dla deweloperów usługi Azure API Management za pomocą szablonów](api-management-developer-portal-templates.md)