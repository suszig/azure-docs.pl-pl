---
title: "Dostosowywanie stylów w portalu deweloperów w usłudze Azure API Management | Microsoft Docs"
description: "Dowiedz się, jak zmodyfikować style używane dla dowolnej strony w portalu deweloperów w usłudze Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: vlvinogr
editor: 
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/09/2017
ms.author: antonba
ms.openlocfilehash: 89baf60d0204a1701e93309f09b90bc94c4ca57b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="customize-the-styling-of-the-developer-portal-in-azure-api-management"></a>Dostosowywanie stylu portalu deweloperów w usłudze Azure API Management
Istnieją trzy podstawowe sposoby dostosowywania portalu deweloperów w usłudze Azure API Management:

* [Edycja zawartości stron statycznych i elementów układu strony][modify-content-layout]
* [Aktualizacja stylów używanych dla elementów strony w portalu deweloperów][customize-styles] (opisana w tym przewodniku)
* [Zmodyfikowanie szablonów używanych dla stron wygenerowanych przez portal][portal-templates] (np. dokumentacji interfejsu API, produktów, uwierzytelniania użytkowników itp.)

## <a name="change-headers-styling"> </a>Zmienianie stylu elementów strony

Kolory, czcionki, rozmiary, odstępy i inne elementy stylu związane z dowolną stroną portalu są definiowane przez reguły stylów. 

Edytowanie reguły stylów odbywa się w **portalu deweloperów**, przy użyciu konta administratora. Aby do niego przejść, otwórz witrynę Azure Portal i kliknij pozycję **Portal wydawcy** na pasku narzędzi wystąpienia usługi API Management.

![Publisher portal][api-management-management-console]

Następnie kliknij polecenie **Portal deweloperów** w prawym górnym rogu. 

![Link do portalu deweloperów w portalu wydawcy][api-management-pp-dp-link]

Aby otworzyć pasek narzędzi dostosowywania, umieść wskaźnik myszy na ikonie dostosowywania (lub wybierz ją), a następnie kliknij pozycję „style” na pasku narzędzi.

![Przycisk paska narzędzi do dostosowywania][api-management-customization-toolbar-button]

Istnieją dwa główne sposób edycji reguł stylów — można przeglądać listę wszystkich reguł stylów używanych w dowolnym miejscu (która jest wyświetlana domyślnie) i modyfikować styl zgodnie z potrzebami lub można wybrać pozycję **Wybierz element na stronie**, a następnie kliknąć dowolne miejsce na stronie, aby wyświetlić tylko style odpowiednie dla tego elementu.

![Pasek narzędzi do dostosowywania][api-management-customization-toolbar]

Na potrzeby tego przykładu kliknij opcję **Wybierz element na stronie**.  Teraz umieszczenie kursora myszy nad elementami powoduje ich wyróżnienie, a kliknięcie wyróżnionego elementu pozwala rozpocząć edycję jego stylów. Przenieś wskaźnik myszy na tekst w nagłówku (zazwyczaj jest tam nazwa firmy), a następnie kliknij go. W edytorze stylów pojawi się zestaw nazwanych i podzielonych na kategorie reguł stylów. Każda reguła reprezentuje właściwość stylu wybranego elementu. Na przykład w wybranym powyżej tekście nagłówka rozmiar tekstu to @font-size-h1, a nazwa czcionki z alternatywami to @headings-font-family.

> Jeśli znasz środowisko [bootstrap][bootstrap], zauważysz, że te reguły są w rzeczywistości [zmiennymi LESS][LESS variables] w motywie bootstrap używanym w portalu dla deweloperów.
> 
> 

Zmienimy teraz kolor tekstu nagłówka. Zaznacz zawartość pola **@headings-color** i wpisz **#000000**. Jest to kod szesnastkowy koloru czarnego. Wtedy na końcu pola tekstowego pojawi się wskaźnik koloru w kształcie kwadratu. Po kliknięciu tego wskaźnika można wybrać kolor z selektora kolorów.

![Selektor kolorów][api-management-customization-toolbar-color-picker]

Zmiany można podglądać w czasie rzeczywistym podczas ich wprowadzania, ale są one widoczne tylko dla administratorów. Aby zmiany stały się widoczne dla wszystkich użytkowników, kliknij przycisk **Publikuj** znajdujący się w edytorze stylów i zatwierdź zmiany.

![Menu Publikuj][api-management-customization-toolbar-publish-form]

> Aby zmienić reguły stylów, które są stosowane do innego elementu na stronie, wykonaj tę samą procedurę co dla nagłówka. Kliknij pozycję **Wybierz element na stronie** w edytorze stylów, wybierz interesujący Cię element i zacznij modyfikować wartości reguł stylu wyświetlanych na ekranie.
> 
> 


## <a name="next-steps"> </a>Następne kroki
* Dowiedz się, w jaki sposób dostosować zawartość portalu dla deweloperów przy użyciu [szablonów portalu dla deweloperów](api-management-developer-portal-templates.md).

[Change the styling of the headers]: #change-headers-styling
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-styles/api-management-management-console.png
[api-management-pp-dp-link]: ./media/api-management-customize-styles/api-management-pp-dp-link.png
[api-management-customization-toolbar-button]: ./media/api-management-customize-styles/api-management-customization-toolbar-button.png
[api-management-customization-toolbar]: ./media/api-management-customize-styles/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-styles/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-styles/api-management-customization-toolbar-publish-form.png

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/
