---
title: "Importuj aplikację interfejsu API jako interfejs API w portalu Azure | Dokumentacja firmy Microsoft"
description: "Ten samouczek przedstawia sposób użycia interfejsu API zarządzania (APIM) do zaimportowania aplikacji interfejsu API jako interfejs API."
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
ms.openlocfilehash: d0e1aa6763d96b5a84bbc5fba7dcae690c051eb3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="import-an-api-app-as-an-api"></a>Importuj z aplikacji interfejsu API jako interfejs API

W tym artykule pokazano, jak zaimportować aplikację interfejsu API jako interfejs API. Artykuł opisuje również sposób testowania APIM interfejsu API.

W tym artykule dowiesz się, jak:

> [!div class="checklist"]
> * Importuj z aplikacji interfejsu API jako interfejs API
> * Testowanie interfejsu API w portalu Azure
> * Testowanie interfejsu API w portalu dla deweloperów

## <a name="prerequisites"></a>Wymagania wstępne

+ Ukończenie następujących Szybki Start: [utworzenia wystąpienia usługi Azure API Management](get-started-create-service-instance.md)
+ Upewnij się, że istnieje aplikacji interfejsu API w ramach subskrypcji. Aby uzyskać więcej informacji zobacz [aplikacji usługi dokumentację] [https://docs.microsoft.com/azure/app-service/]

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importu i opublikuj interfejs API zaplecza

1. Wybierz **interfejsów API** zgodnie z **zarządzanie interfejsami API**.
2. Wybierz **aplikacji interfejsu API** z **dodać nowy interfejs API** listy.

    ! (Aplikacji interfejsu API) [. / media/import-api-app-as-api/api-app.png]
3. Naciśnij klawisz **Przeglądaj** Aby wyświetlić listę aplikacji interfejsu API w ramach subskrypcji.
4. Wybierz aplikację. APIM znajduje swagger skojarzone z wybraną aplikację, pobiera go i importuje go. 

    W przypadku APIM nie może znaleźć struktury swagger, opisuje interfejs API jako interfejs API "przekazywanie". 
5. Dodaj sufiks adresu URL interfejsu API. Sufiks to nazwa, która określa tego interfejsu API w tym wystąpieniu APIM. Musi być unikatowa w tym wystąpieniu APIM.
6. Opublikuj interfejsu API interfejsu API można skojarzyć z produktem. W takim przypadku "*nieograniczone*" produkt jest używany.  Jeśli chcesz dla interfejsu API do opublikowania i być dostępne dla deweloperów, należy ją dodać do produktu. Można to zrobić podczas tworzenia interfejsu API lub ustaw ją później.

    Produkty są skojarzenia jeden lub więcej interfejsów API. Można zawierają wiele interfejsów API i ich oferty dla deweloperów za pośrednictwem portalu dla deweloperów. Deweloperzy muszą najpierw subskrybować produktu, aby uzyskać dostęp do interfejsu API. Gdy subskrybujesz one, otrzymują klucz subskrypcji, który ułatwia jakiegokolwiek interfejsu API w tym produkcie. Jeśli utworzono wystąpienie APIM jesteś administratorem już, aby zasubskrybować każdego produktu domyślnie.

    Domyślnie każde wystąpienie usługi API Management zawiera dwa produkty przykładowe:

    * **Starter (początkowy)**
    * **Unlimited (nieograniczony)**   
7. Wybierz pozycję **Utwórz**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Testowanie nowy interfejs API APIM w portalu Azure

Operacje można wywołać bezpośrednio z portalu Azure oferują wygodny sposób, aby wyświetlić i przetestować operacje interfejsu API.  

1. Wybierz interfejs API utworzony w poprzednim kroku.
2. Naciśnij klawisz **testu** kartę.
3. Wybierz niektórych operacji.

    Zostaje wyświetlona strona pól parametrów zapytania i nagłówków. Jeden z nagłówków jest "Ocp-Apim-subskrypcji — klucz", dla subskrypcji klucza produktu, który jest skojarzony z tym interfejsie API. Jeśli utworzono wystąpienie APIM są przeznaczone dla administratorów, więc klucz jest wypełniane automatycznie. 
1. Naciśnij klawisz **wysyłania**.

    Zaplecze odpowiada **200 OK** i niektórych danych.

## <a name="call-operation"> </a>Wywoływanie operacji z portalu dla deweloperów

Ponadto można wywołać operacji **portalu dla deweloperów** do testowania interfejsów API. 

1. Wybierz utworzony w interfejsu API "Import i opublikuj interfejs API zaplecza" krok.
2. Naciśnij klawisz **portalu dla deweloperów**.

    Otwiera witrynę "Portalu dla deweloperów".
3. Wybierz **interfejsu API** utworzony.
4. Kliknij operację, którą chcesz przetestować.
5. Naciśnij klawisz **wypróbuj**.
6. Naciśnij klawisz **wysyłania**.
    
    Po wywołaniu operacji portal dla deweloperów wyświetla **stan odpowiedzi**, **nagłówki odpowiedzi** oraz wszelką **zawartość odpowiedzi**.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Przekształcanie i chronić opublikowanych interfejsu API](transform-api.md)