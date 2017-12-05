---
title: "Debugowanie swoje interfejsy API za pomocą śledzenia niepomyślnych żądań w usłudze Azure API Management | Dokumentacja firmy Microsoft"
description: "Wykonaj kroki w tym samouczku, aby dowiedzieć się, jak przeprowadzać inspekcję czynności w usłudze Azure API Management przetwarzania żądania."
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
ms.openlocfilehash: 7b9bec7927169b9d820c095a7d11705264e7dcfe
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="debug-your-apis-using-request-tracing"></a>Debugowanie swoje interfejsy API za pomocą śledzenia niepomyślnych żądań

W tym samouczku opisano, jak przeprowadzać inspekcję przetwarzania żądania, które zapewniają pomoc podczas debugowania i rozwiązywanie problemów z interfejsu API. 

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wywołanie śledzenia

![Inspektor interfejsu API](media/api-management-howto-api-inspector/api-inspector001.PNG)

## <a name="prerequisites"></a>Wymagania wstępne

+ Ukończenie następujących Szybki Start: [utworzenia wystąpienia usługi Azure API Management](get-started-create-service-instance.md).
+ Ponadto Ukończ samouczek następujących: [Import i opublikować swój pierwszy interfejs API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="trace-a-call"></a>Wywołanie śledzenia

1. Wybierz **interfejsów API**.
2. Kliknij przycisk **API konferencji pokaz** z listy interfejsu API.
3. Wybierz **GetSpeakers** operacji.
4. Przełącz się do **testu** kartę.
5. Upewnij się, że Dołącz nagłówek HTTP o nazwie **Ocp Apim śledzenia** z wartością ustawioną na **true**.
6. Kliknij przycisk **"Wyślij"** do wywoływania interfejsu API. 
7. Poczekaj, aż do ukończenia wywołania. 
8. Przejdź do **śledzenia** karcie **konsoli interfejsu API**. Można kliknąć dowolną z poniższych linków, aby przejść do śledzenia szczegółowe informacje o: **przychodzących**, **zaplecza**, **wychodzących**.

    W **przychodzących** sekcji Zobacz oryginalnego żądania interfejsu API zarządzania otrzymanych od wywołującego i wszystkie zasady zastosowane do żądania tym limit szybkości i zasad nagłówka set dodaliśmy w kroku 2.

    W **zaplecza** sekcji Zobacz żądania interfejsu API zarządzania wysyłane do zaplecza interfejsu API i otrzymania odpowiedzi.
    
    W **wychodzących** sekcji Zobacz wszystkie zasady zastosowane do odpowiedzi przed wysłaniem z powrotem do wywołującego.
 
    > [!TIP]
    > Każdy krok również przedstawia czas, który upłynął od żądania jest odbierany przez interfejs API zarządzania.

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wywołanie śledzenia

Przejdź do następnego samouczek:

> [!div class="nextstepaction"]
> [Użyj poprawki](api-management-get-started-revise-api.md)
