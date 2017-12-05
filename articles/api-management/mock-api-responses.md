---
title: Mock odpowiedzi interfejsu API w portalu Azure | Dokumentacja firmy Microsoft
description: "Ten samouczek przedstawia sposób użycia interfejsu API zarządzania (APIM) można ustawić zasady na interfejs API, tak aby zwracało mocked odpowiedzi. Tej metody endables deweloperom kontynuować proces wdrażania i testowania wewnętrznej bazy danych w przypadku wystąpienia interfejsu API zarządzania nie jest dostępne do wysyłania odpowiedzi prawdziwe."
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
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: e485071b026c52eb23532639546ad475fc92cde3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="mock-api-responses"></a>Zasymulować odpowiedzi interfejsu API

Interfejsy API wewnętrznej bazy danych można zaimportować do interfejsu API APIM lub utworzone i zarządzane ręcznie. Kroki opisane w tym samouczku pokazano, jak APIM umożliwia tworzenie pustego interfejsu API i zarządzać nim ręcznie. Samouczek pokazuje, jak ustawić zasady na interfejs API, tak aby zwracało mocked odpowiedzi. Ta metoda umożliwia deweloperom kontynuować wdrażania i testowania wystąpienia APIM, nawet jeśli wewnętrznej bazy danych nie jest dostępne do wysyłania odpowiedzi prawdziwe. Możliwość makiety odpowiedzi może być przydatne w wielu scenariuszach:

+ Kiedy najpierw zaprojektowano fasad interfejsu API i implementacji wewnętrznej bazy danych jest dostarczany później. Lub wewnętrznej bazy danych jest opracowywanych równolegle.
+ Gdy wewnętrznej bazy danych jest tymczasowo operacyjne lub nie można skalować.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie nowego testu API 
> * Dodaj operację do badania interfejsu API
> * Włącz mocking odpowiedzi
> * Testowanie mocked interfejsu API

![Operacja mocked odpowiedzi](./media/mock-api-responses/mock-api-responses01.png)

## <a name="prerequisites"></a>Wymagania wstępne

Ukończenie następujących Szybki Start: [utworzenia wystąpienia usługi Azure API Management](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-a-test-api"></a>Tworzenie nowego testu API 

Kroki opisane w tej sekcji pokazano, jak utworzyć pusty interfejsu API z nie wewnętrznej bazy danych. On również pokazano, jak dodać operację do interfejsu API. Wywołanie operacji po wykonaniu kroków w tej sekcji powoduje błąd. Po ukończeniu czynności opisane w sekcji "Włącz mocking odpowiedzi", zostanie wyświetlone żadne błędy.

1. Wybierz **interfejsów API** zgodnie z **zarządzanie interfejsami API**.
2. Wybierz z menu po lewej stronie **+ Dodaj interfejsu API**.
3. Wybierz **puste API** z listy.
4. Wprowadź "*interfejsu API Test*" dla **Nazwa wyświetlana**.
5. Wprowadź "*nieograniczone*" dla **produkty**.
6. Wybierz pozycję **Utwórz**.

## <a name="add-an-operation-to-the-test-api"></a>Dodaj operację do badania interfejsu API

1. Wybierz interfejs API utworzony w poprzednim kroku.
2. Kliknij przycisk **+ Dodaj operację**.

    ![Operacja mocked odpowiedzi](./media/mock-api-responses/mock-api-responses02.png)

    |Ustawienie|Wartość|Opis|
    |---|---|---|
    |**Adres URL** (Zlecenie HTTP)|POBIERZ|Można w jednym z wstępnie zdefiniowanych zleceń HTTP.|
    |**ADRES URL** |*/ Test*|Ścieżka adresu URL dla interfejsu API. |
    |**Nazwa wyświetlana**|*Wywołanie testu*|Nazwa, która jest wyświetlana w **portalu dla deweloperów**.|
    |**Opis**||Podaj opis działania, który służy do zapewnienia dokumentacji dla deweloperów przy użyciu tego interfejsu API w **portalu dla deweloperów**.|
    |**Zapytanie** kartę||Można dodać parametry zapytania. Oprócz zapewnienia nazwę i opis, musisz podać wartości, które mogą być przypisane do tego parametru. Jedna z wartości może być oznaczony jako domyślny (opcjonalnie).|
    |**Żądanie** kartę||Można określić typy zawartości żądania, przykłady i schematów. |
    |**Odpowiedź** kartę|Czynności, które należy wykonać w tej tabeli.|Zdefiniuj kody stanów odpowiedzi, typy zawartości, przykłady i schematy.|

3. Wybierz **odpowiedzi** kartę znajdujący się pod adresem URL, nazwę wyświetlaną i opis pola.
4. Kliknij przycisk **+ Dodaj odpowiedzi**.
5. Wybierz **200 OK** z listy.
6. W obszarze **reprezentacje** nagłówek po prawej stronie, wybierz **+ Dodaj reprezentacja**.
7. Wprowadź "*application/json*" w polu wyszukiwania i wybierz **application/json** typ zawartości.
8. W **próbki** tekst Wprowadź "*{"sampleField":"test"}*".
9. Wybierz pozycję **Zapisz**.

## <a name="enable-response-mocking"></a>Włącz mocking odpowiedzi

1. Wybierz utworzony w kroku "Tworzenie testu API" interfejsu API.
2. Wybierz operacji testu, który został dodany.
2. W oknie po prawej stronie kliknij **projekt** kartę.
3. W **przetwarzania przychodzącego** okna, kliknij ikonę ołówka.
4. W **Mocking** wybierz opcję **odpowiedzi statyczne** dla **Mocking zachowanie**.
5. W **zarządzanie interfejsami API zwraca następującą odpowiedź:** polu tekstowym **200 OK application/json**. To pole wyboru oznacza, że Twój interfejs API powinien zwrócić przykładowe odpowiedzi, który zdefiniowano w poprzedniej sekcji.
6. Wybierz pozycję **Zapisz**.

## <a name="test-the-mocked-api"></a>Testowanie mocked interfejsu API

1. Wybierz utworzony w kroku "Tworzenie testu API" interfejsu API.
2. Otwórz **testu** kartę.
3. Upewnij się, **Test wywołania** interfejsu API jest zaznaczone.

    > [!TIP]
    > Żółty pasek tekstem **włączono Mocking** wskazuje, że odpowiedzi zwrócony z interfejsu API Management, wysyła mocking zasad i nie odpowiedzi rzeczywiste wewnętrznej bazy danych.

3. Wybierz **wysyłania** aby wywołać testu.
4. **Odpowiedzi HTTP** wyświetla dane JSON podane jako próbka w pierwszej części samouczka.

## <a name="video"></a>Połączenia wideo

> [!VIDEO https://www.youtube.com/embed/i9PjUAvw7DQ]
> 
> 

## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie nowego testu API
> * Dodaj operację do badania interfejsu API
> * Włącz mocking odpowiedzi
> * Testowanie mocked interfejsu API

Przejdź do następnego samouczek:

> [!div class="nextstepaction"]
> [Przekształcanie i chronić opublikowanych interfejsu API](transform-api.md)
