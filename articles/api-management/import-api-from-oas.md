---
title: "Importuj specyfikacji OpenAPI przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zaimportować specyfikacji OpenAPI z interfejsu API zarządzania."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: f0c77c6e959ca99698b3ea704756a6abf36147f3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="import-an-openapi-specification"></a>Specyfikacja OpenAPI importu

W tym artykule pokazano, jak zaimportować interfejs API zaplecza "OpenAPI specyfikacji" znajdującej się na http://conferenceapi.azurewebsites.net?format=json. Ten interfejs API zaplecza jest obsługiwane przez firmę Microsoft i hostowanej na platformie Azure. Artykuł opisuje również sposób testowania APIM interfejsu API.

W tym artykule dowiesz się, jak:

> [!div class="checklist"]
> * Importuj interfejs API zaplecza "Specyfikacja OpenAPI"
> * Testowanie interfejsu API w portalu Azure
> * Testowanie interfejsu API w portalu dla deweloperów

## <a name="prerequisites"></a>Wymagania wstępne

Ukończenie następujących Szybki Start: [utworzenia wystąpienia usługi Azure API Management](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importu i opublikuj interfejs API zaplecza

1. Wybierz **interfejsów API** zgodnie z **zarządzanie interfejsami API**.
2. Wybierz **specyfikacji OpenAPI** z **dodać nowy interfejs API** listy.
    ![Specyfikacja OpenAPI](./media/import-api-from-oas/oas-api.png)
3. Wprowadź odpowiednie ustawienia. Podczas tworzenia, można ustawić wartości interfejsu API. Alternatywnie można ustawić niektóre z nich później, przechodząc do **ustawienia** kartę. <br/> Jeśli naciśniesz **kartę** niektóre lub wszystkie pola uzyskać wypełnione z użyciem informacji od określonej usługi zaplecza.

    ![Tworzenie interfejsu API](./media/api-management-get-started/create-api.png)

    |Ustawienie|Wartość|Opis|
    |---|---|---|
    |**Specyfikacja OpenAPI**|http://conferenceapi.azurewebsites.NET?Format=JSON|Odwołuje się do usługi implementacja interfejsu API. Zarządzanie interfejsami API przekazuje żądania do tego adresu.|
    |**Nazwa wyświetlana**|*Wersja demonstracyjna konferencji API*|Jeśli po wprowadzeniu adresu URL usługi naciśnij klawisz tab, APIM wypełni to pole, w zależności w formacie json. <br/>Ta nazwa jest wyświetlana w portalu dla deweloperów.|
    |**Nazwa**|*Wersja demonstracyjna konferencji api*|Zapewnia unikatową nazwę dla interfejsu API. <br/>Jeśli po wprowadzeniu adresu URL usługi naciśnij klawisz tab, APIM wypełni to pole, w zależności w formacie json.|
    |**Opis**|Wprowadź opcjonalny opis interfejsu API.|Jeśli po wprowadzeniu adresu URL usługi naciśnij klawisz tab, APIM wypełni to pole, w zależności w formacie json.|
    |**Sufiks adresu URL interfejsu API**|*Konferencja*|Sufiks jest dołączany do podstawowego adresu URL dla interfejsu API usługi zarządzania. Zarządzanie interfejsami API odróżnia interfejsy API według ich sufiks i w związku z tym sufiks muszą być unikatowe dla każdego interfejsu API dla danego wydawcy.|
    |**Schemat adresu URL**|*PROTOKÓŁ HTTPS*|Określa, które protokoły umożliwia dostęp do interfejsu API. |
    |**Produkty**|*Unlimited (nieograniczony)*| Opublikuj interfejsu API interfejsu API można skojarzyć z produktem. Opcjonalnie można dodać ten nowy interfejs API do produktu, wpisz nazwę produktu. Ten krok można powtarzać wielokrotnie dodać interfejsu API do wielu produktów.<br/>Produkty są skojarzenia jeden lub więcej interfejsów API. Można zawierają wiele interfejsów API i ich oferty dla deweloperów za pośrednictwem portalu dla deweloperów. Deweloperzy muszą najpierw subskrybować produktu, aby uzyskać dostęp do interfejsu API. Gdy subskrybujesz one, otrzymują klucz subskrypcji, który ułatwia jakiegokolwiek interfejsu API w tym produkcie. Jeśli utworzono wystąpienie APIM jesteś administratorem już, aby zasubskrybować każdego produktu domyślnie.<br/> Domyślnie każde wystąpienie interfejsu API zarządzania jest dostarczany z dwóch produktów próbki: **Starter** i **nieograniczone**. |

4. Wybierz pozycję **Utwórz**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Testowanie nowy interfejs API APIM w portalu Azure

Operacje można wywołać bezpośrednio z portalu Azure oferują wygodny sposób, aby wyświetlić i przetestować operacje interfejsu API.  

1. Wybierz interfejs API utworzony w poprzednim kroku.
2. Naciśnij klawisz **testu** kartę.

    ![Testuj interfejs API](./media/api-management-get-started/test-api.png)
1. Polecenie **GetSpeakers**.

    Na stronie są wyświetlane pola parametrów zapytania, ale w takim przypadku firma Microsoft nie ma żadnych. Strona wyświetla również pól w nagłówkach. Jeden z nagłówków jest "Ocp-Apim-subskrypcji — klucz", dla subskrypcji klucza produktu, który jest skojarzony z tym interfejsie API. Jeśli utworzono wystąpienie APIM są przeznaczone dla administratorów, więc klucz jest wypełniane automatycznie. 
4. Naciśnij klawisz **wysyłania**.

    Zaplecze odpowiada **200 OK** i niektórych danych.

## <a name="call-operation"></a>Wywołać operację z portalu dla deweloperów

Ponadto można wywołać operacji **portalu dla deweloperów** do testowania interfejsów API. 

1. Wybierz utworzony w interfejsu API "Import i opublikuj interfejs API zaplecza" krok.
2. Naciśnij klawisz **portalu dla deweloperów**.

    ![Testowanie w portalu dla deweloperów](./media/api-management-get-started/developer-portal.png)

    Otwiera witrynę "Portalu dla deweloperów".
3. Wybierz **interfejsu API**.
4. Wybierz **demonstracją konferencji interfejsu API**.
5. Kliknij przycisk **GetSpeakers**.
    
    Na stronie są wyświetlane pola parametrów zapytania, ale w takim przypadku firma Microsoft nie ma żadnych. Strona wyświetla również pól w nagłówkach. Jeden z nagłówków jest "Ocp-Apim-subskrypcji — klucz", dla subskrypcji klucza produktu, który jest skojarzony z tym interfejsie API. Jeśli utworzono wystąpienie APIM są przeznaczone dla administratorów, więc klucz jest wypełniane automatycznie.
6. Naciśnij klawisz **wypróbuj**.
7. Naciśnij klawisz **wysyłania**.
    
    Po wywołaniu operacji portal dla deweloperów wyświetla **stan odpowiedzi**, **nagłówki odpowiedzi** oraz wszelką **zawartość odpowiedzi**.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Przekształcanie i chronić opublikowanych interfejsu API](transform-api.md)
