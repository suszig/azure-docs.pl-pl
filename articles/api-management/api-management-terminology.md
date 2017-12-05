---
title: Azure API Management terminologia | Dokumentacja firmy Microsoft
description: "Ten artykuł zawiera definicje terminów, które są specyficzne dla interfejsu API zarządzania."
services: api-management
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/11/2017
ms.author: apimpm
ms.openlocfilehash: 9391b65a5aade4c050ca964354bfea2d3a2338d8
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="terminology"></a>Terminologia

Ten artykuł zawiera definicje terminów, specyficzne zarządzania interfejsu API (APIM).

## <a name="term-definitions"></a>Definicje terminów

* **Interfejs API zaplecza** — usługa HTTP, która implementuje interfejs API i operacjach. 
* **API frontonu**/**APIM API** — APIM interfejsu API nie obsługuje interfejsów API, tworzy fasad dla interfejsu API zakończyć swoje interfejsy API w celu dostosowania fasad zgodnie z potrzebami, nie ingerując z tyłu. Aby uzyskać więcej informacji, zobacz [Import i opublikować interfejs API](import-and-publish.md).
* **Produkt APIM** -produkt zawiera jeden lub więcej interfejsów API, a także przydział użycia i warunki użytkowania. Można zawierają wiele interfejsów API i ich oferty dla deweloperów za pośrednictwem portalu dla deweloperów. Aby uzyskać więcej informacji, zobacz [tworzenie i publikowanie produktu](api-management-howto-add-products.md).
* **Operacja interfejsu API APIM** — każdy APIM interfejsu API reprezentuje zestaw działań dostępnych dla deweloperów. Każdy interfejs API APIM zawiera odwołanie do usługi zaplecza, który implementuje interfejs API i jego operacji mapy do operacji zaimplementowanych przez usługę zaplecza. Aby uzyskać więcej informacji, zobacz [odpowiedzi Mock interfejsu API](mock-api-responses.md).
* **Wersja** — czasami chcesz opublikować nowych lub innego interfejsu API funkcji niektórych użytkowników, podczas gdy inne korzystać z interfejsu API, który jest obecnie obsługiwane dla nich. Aby uzyskać więcej informacji, zobacz [publikowanie wielu wersji interfejsu API](api-management-get-started-publish-versions.md).
* **Poprawki** — interfejs API jest gotowe do go i rozpoczyna się do użycia przez deweloperów, należy zwykle należy zachować ostrożność w wprowadzania zmian do tego interfejsu API i w tym samym czasie, aby nie przerywać wywołań interfejsu API. Jest również przydatne może być umożliwienie deweloperom wiedzieć o wprowadzone zmiany. Aby uzyskać więcej informacji, zobacz [Użyj poprawki](api-management-get-started-revise-api.md).
* **Portalu dla deweloperów** -klientów (deweloperzy) należy używać portalu dla deweloperów dostępu do sieci interfejsów API. Można dostosować portalu dla deweloperów. Aby uzyskać więcej informacji, zobacz [Dostosowywanie portalu dla deweloperów](api-management-customize-styles.md).

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Tworzenie wystąpienia](get-started-create-service-instance.md)

