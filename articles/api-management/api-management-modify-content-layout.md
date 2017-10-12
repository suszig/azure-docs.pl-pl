---
title: "Modyfikowanie zawartości stron w portalu deweloperów w usłudze Azure API Management | Microsoft Docs"
description: "Dowiedz się, jak edytować zawartość stron w portalu deweloperów w usłudze Azure API Management."
services: api-management
documentationcenter: 
author: antonba
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
ms.openlocfilehash: 708c803c36c182ed90e04731b12d4ade00ae7ffb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="modify-the-content-and-layout-of-pages-on-the-developer-portal-in-azure-api-management"></a>Modyfikowanie zawartości i układu stron w portalu deweloperów w usłudze Azure API Management
Istnieją trzy podstawowe sposoby dostosowywania portalu deweloperów w usłudze Azure API Management:

* [Edycja zawartości stron statycznych i elementów układu strony][modify-content-layout] (opisana w tym przewodniku)
* [Aktualizacja stylów używanych dla elementów strony w portalu deweloperów][customize-styles]
* [Zmodyfikowanie szablonów używanych dla stron wygenerowanych przez portal][portal-templates] (np. dokumentacji interfejsu API, produktów, uwierzytelniania użytkowników itp.)

## <a name="page-structure"> </a>Struktura stron portalu deweloperów

Portal deweloperów jest oparty na systemie zarządzania zawartością. Układ każdej strony jest tworzony przez zestaw małych elementów nazywanych widżetami:

![Struktura stron portalu deweloperów][api-management-customization-widget-structure]

Wszystkie widżety można edytować. 
* Podstawowa zawartość poszczególnych stron znajduje się w widżecie „Zawartość”. Edycja strony polega na edytowaniu zawartości tego widżetu.
* Wszystkie elementy układu strony są zawarte w pozostałych widżetach. Zmiany wprowadzone w tych widżetach zostaną zastosowane do wszystkich stron. Będą one nazywane „widżetami układu”.

W ramach codziennej edycji stron zwykle modyfikowany jest tylko widżet Zawartość mający różną zawartość w przypadku poszczególnych stron.

## <a name="modify-layout-widget"> </a>Modyfikowanie zawartości widżetu układu

Zawartość portalu dla deweloperów jest modyfikowana przy użyciu portalu wydawcy dostępnego za pośrednictwem klasycznej witryny Azure Portal. Aby przejść do niego, kliknij pozycję **Portal wydawcy** na pasku narzędzi wystąpienia usługi API Management.

![Publisher portal][api-management-management-console]

Aby edytować zawartość tego widżetu, kliknij pozycję **Widżety** w menu **Portal deweloperów** po lewej stronie. W ramach tego przykładu zmodyfikujemy zawartość widżetu Nagłówek. Wybierz z listy widżet **Nagłówek**.

![Widżety — nagłówek][api-management-widgets-header]

Zawartość nagłówka można edytować w polu **Treść**. Zmień tekst tak, jak chcesz, a następnie kliknij przycisk **Zapisz** u dołu strony.

Teraz nowy nagłówek powinien być widoczny na każdej stronie portalu dla deweloperów.

> Aby otworzyć portal dla deweloperów w portalu wydawcy, kliknij przycisk **Portal dla deweloperów** na górnym pasku.
> 
> 

## <a name="edit-page-contents"> </a>Edytowanie zawartości strony

Aby wyświetlić listę wszystkich istniejących stron zawartości, kliknij pozycję **Zawartość** w menu **Portal dla deweloperów** w portalu wydawcy.

![Zarządzanie zawartością][api-management-customization-manage-content]

Kliknij stronę **Zapraszamy**, aby edytować zawartość strony głównej portalu dla deweloperów. Wprowadź żądane zmiany, w razie potrzeby wyświetl ich podgląd, a następnie kliknij opcję **Publikuj teraz**, aby stały się widoczne dla wszystkich użytkowników.

> Strona główna używa specjalnego układu, który umożliwia wyświetlanie transparentu u góry. Transparentu nie można edytować w sekcji **Zawartość**. Aby edytować ten transparent, kliknij opcję **Widżety** w menu **Portal dla deweloperów**, wybierz opcję **Strona główna** z listy rozwijanej **Bieżąca warstwa**, a następnie otwórz element **Transparent** w obszarze **Wyróżniona sekcja**. Zawartość tego widżetu można edytować podobnie jak dowolnej innej strony.
> 
> 

## <a name="next-steps"> </a>Następne kroki
* [Aktualizacja stylów używanych dla elementów strony w portalu deweloperów][customize-styles]
* [Zmodyfikowanie szablonów używanych dla stron wygenerowanych przez portal][portal-templates] (np. dokumentacji interfejsu API, produktów, uwierzytelniania użytkowników itp.)

[Structure of developer portal pages]: #page-structure
[Modifying the contents of a layout widget]: #modify-layout-widget
[Edit the contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customization-widget-structure]: ./media/api-management-modify-content-layout/portal-widget-structure.png
[api-management-management-console]: ./media/api-management-modify-content-layout/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-modify-content-layout/api-management-widgets-header.png
[api-management-customization-manage-content]: ./media/api-management-modify-content-layout/api-management-customization-manage-content.png
