---
title: "Jak tworzyć i publikować w usłudze Azure API Management produktu"
description: "Dowiedz się, jak tworzyć i publikować w usłudze Azure API Management produktów."
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
ms.openlocfilehash: e6b11145506780f9a08799c4c9daf55ba17b366d
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/22/2017
---
# <a name="create-and-publish-a-product"></a>Tworzenie i publikowanie produktu  

W systemie Azure API Management produkt zawiera jeden lub więcej interfejsów API, a także przydział użycia i warunki użytkowania. Po opublikowaniu produktu deweloperzy mogą subskrybować produktu i rozpocząć przy użyciu interfejsów API produktu.  

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie i publikowanie produktu
> * Dodawanie interfejsu API do produktu

![Dodano produktu](media/api-management-howto-add-products/added-product.png)

## <a name="prerequisites"></a>Wymagania wstępne

+ Ukończenie następujących Szybki Start: [utworzenia wystąpienia usługi Azure API Management](get-started-create-service-instance.md).
+ Ponadto Ukończ samouczek następujących: [Import i opublikować swój pierwszy interfejs API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-and-publish-a-product"></a>Tworzenie i publikowanie produktu

1. Polecenie **produktów** w menu po lewej stronie, aby wyświetlić **produktów** strony.
2. Kliknij przycisk **+ produktu**.

    ![Dodano produktu](media/api-management-howto-add-products/add-product.png)

    Po dodaniu produkt, należy podać następujące informacje: 

    |Name (Nazwa)|Opis|
    |---|---|
    |Nazwa wyświetlana|Nazwa jako użytkownik ma do pokazania w **portalu dla deweloperów**.|
    |Name (Nazwa)|Opisową nazwę produktu.|
    |Opis|**Opis** pole umożliwia zawierają szczegółowe informacje o produkcie, takie jak jego przeznaczenie, zapewnia dostęp do interfejsów API i inne przydatne informacje.|
    |Stan|Naciśnij klawisz **opublikowano** Jeśli chcesz opublikować produktu. Aby można było wywołać interfejsów API w produkcie, musi zostać opublikowany produktu. Domyślnie są nieopublikowane nowych produktów i są widoczne tylko dla **Administratorzy** grupy.|
    |Wymaga zatwierdzenia|Sprawdź **wymagają zatwierdzenia subskrypcji** Jeśli chcesz, aby administrator, aby przejrzeć i zaakceptować lub odrzucić prób subskrypcję do tego produktu. Jeśli pole jest zaznaczona, prób subskrypcji są automatycznie zatwierdzone. |
    |Limit liczby subskrypcji|Aby ograniczyć liczbę wiele równoczesnych subskrypcji, wprowadź limit subskrypcji. |
    |Postanowienia prawne|Mogą obejmować warunki użytkowania produktów, którzy użytkownicy muszą zaakceptować, aby korzystać z produktu.|
    |Interfejsy API|Produkty są skojarzenia jeden lub więcej interfejsów API. Można zawierają wiele interfejsów API i ich oferty dla deweloperów za pośrednictwem portalu dla deweloperów. <br/> Podczas tworzenia produktu można dodać istniejącego interfejsu API. Interfejs API można dodać do niego później, albo z produktów **ustawienia** strony lub podczas tworzenia interfejsu API.|<br/>Deweloperzy muszą najpierw subskrybować produktu, aby uzyskać dostęp do interfejsu API. Gdy subskrybujesz one, otrzymują klucz subskrypcji, który ułatwia jakiegokolwiek interfejsu API w tym produkcie.<br/> Jeśli utworzono wystąpienie APIM jesteś administratorem już, aby zasubskrybować każdego produktu domyślnie.|

3. Kliknij przycisk **Utwórz** do tworzenia nowych produktów.

### <a name="add-more-configurations"></a>Dodaj więcej konfiguracji

Można kontynuować konfigurowanie produktu po zapisaniu go, wybierając **ustawienia** kartę. 

Wyświetlić/dodać subskrybentów produktu z **subskrypcje** kartę.

Ustawienie widoczności produktu dla deweloperów lub gościa z **kontrola dostępu** kartę.

## <a name="add-apis"></a>Dodaj interfejsów API do produktu

Produkty są skojarzenia jeden lub więcej interfejsów API. Można zawierają wiele interfejsów API i ich oferty dla deweloperów za pośrednictwem portalu dla deweloperów. Podczas tworzenia produktu można dodać istniejącego interfejsu API. Interfejs API można dodać do niego później, albo z produktów **ustawienia** strony lub podczas tworzenia interfejsu API.

Deweloperzy muszą najpierw subskrybować produktu, aby uzyskać dostęp do interfejsu API. Gdy subskrybujesz one, otrzymują klucz subskrypcji, który ułatwia jakiegokolwiek interfejsu API w tym produkcie. Jeśli utworzono wystąpienie APIM jesteś administratorem już, aby zasubskrybować każdego produktu domyślnie.

### <a name="add-an-api-to-an-existing-product"></a>Dodaj interfejs API do istniejącego produktu

1. Wybierz produkt.
2. Wybierz kartę interfejsów API.
3. Kliknij przycisk **i interfejs API**.
4. Wybierz interfejs API, a następnie kliknij przycisk **Utwórz**.

## <a name="video"></a>Połączenia wideo

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

## <a name="next-steps"></a>Kolejne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie i publikowanie produktu
> * Dodawanie interfejsu API do produktu

Przejdź do następnego samouczek:

> [!div class="nextstepaction"]
> [Utwórz puste interfejsu API i mock odpowiedzi interfejsu API](mock-api-responses.md)
