---
title: "Certyfikowanie łączników niestandardowych - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Udostępnia łączniki do wszystkich użytkowników usługi Azure Logic Apps, Flow firmy Microsoft i PowerApps firmy Microsoft"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 9bcb138c181df0c12548cc5dc721542fd9c9ba68
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="submit-your-connectors-for-microsoft-certification"></a>Przedstawia łączniki do certyfikacji firmy Microsoft

Aby łączników niestandardowych publicznie dostępny dla wszystkich użytkowników w aplikacjach logiki platformy Azure, Flow firmy Microsoft i Microsoft PowerApps przesłać Twojego łącznika do firmy Microsoft do przeglądu, weryfikacji i zatwierdzania publikowania. 

## <a name="certification-criteria"></a>Kryteria certyfikacji

| Możliwości | Szczegóły | Wymaganym lub zalecanym |
|------------|---------|-------------------------|
| Aplikacja oprogramowania jako a — usługa (SaaS) | Spełnia scenariusza użytkownika, który mieści się również z aplikacji logiki, przepływu i rozwiązaniu PowerApps. | Wymagane |
| Typ uwierzytelniania | Interfejs API musi obsługiwać OAuth2 klucz interfejsu API i uwierzytelnianie podstawowe. | Wymagane | 
| Pomoc techniczna | Należy podać skontaktuj się z pomocą techniczną, aby klienci mogli uzyskać pomoc. | Wymagane | 
| Dostępność i przestoje | Aplikacja ma co najmniej 99,9% czasu pracy. | Zalecane | 
|||| 

Ponadto jeśli nie jesteś partnerem firmy Microsoft lub niezależnego dostawcy oprogramowania (ISV) i chcesz przeprowadzić certyfikację i publicznie wersji łącznika, musi albo własnej usługi podstawowej lub występuje jawna praw do korzystania z interfejsu API.

## <a name="validation-phases"></a>Fazy weryfikacji

Firma Microsoft używa tych fazy weryfikacji za skorzystanie z łącznika:

| Faza sprawdzania poprawności | Opis | 
| ----- | ----------- |
| Funkcjonalność | Firma Microsoft testuje Twojego łącznika z aplikacji logiki, przepływu i rozwiązanie PowerApps błędów: <p>-Nieprawidłowy OpenAPI (Swagger) lub JSON błędów, które pojawiają się w sekcji definicji w Kreatorze łącznika niestandardowego <p>— Błędy sprawdzania poprawności środowisko uruchomieniowe i schemat znajdujące się w sekcji testu z Kreatora łącznika niestandardowego | 
| Zawartość | Microsoft sprawdza, czy Twoje łącznik używa przyjaznej nazwy i opisy dla każdej jednostki. Upewnij się, że każdej operacji, parametr wejściowy i odpowiedzi atrybutu w Swagger Twojego łącznika ma te elementy: <p>- [Podsumowanie](../logic-apps/custom-connector-openapi-extensions.md#summary) <br>- [Opis elementu](../logic-apps/custom-connector-openapi-extensions.md#description) </br>- [Widoczność informacji](../logic-apps/custom-connector-openapi-extensions.md#visibility) | 
||| 

## <a name="nominate-and-submit-your-connector-to-microsoft-for-certification"></a>Wyznaczyć i przesłać Twojego łącznika do firmy Microsoft do certyfikacji

Aby zastosować do certyfikacji, wykonaj następujące kroki:

1. **Wyznaczyć**

   1. [Przedstawia Twojej nazwy](https://go.microsoft.com/fwlink/?linkid=848754).

      1. W dolnej części strony, wybierz **skontaktuj się z nami**.
      2. Wypełnij formularz, a następnie wybierz **pytania dotyczące programu łącznika niezależnego dostawcy oprogramowania i wspólnej marketing**.
      3. Wybierz **przesłać**.

   2. Zaloguj się wzajemne umową o nieujawnianiu informacji inne niż i umowy z partnerem, który pojawi się. 

      Firma Microsoft wymaga tych podpisanej umowy przed kontynuowaniem. 
      Firma Microsoft może następnie sprawdź, czy łącznika użytkownika spełnia kryteria certyfikacji. 
   
   3. Jeśli Twoje łącznika zostanie zatwierdzone, firma Microsoft poinformowała instrukcje dotyczące dołączania.
    
2. **Przegląd**

   1. Wysyłać tych informacji do kontaktu wyznaczenie do przeglądu:

      * Plik OpenAPI, który opisuje interfejs API
      * Plik ikony (.png lub .jpg), który reprezentuje łącznik programu 
      
        Ikona programu powinien mieć logo ~ 160 pikseli wewnątrz kwadratu 230 pikseli. 
        Białe logo na tle kolorowe jest preferowana.
      
      * Kolor brand tej ikony w formacie szesnastkowym, która powinna być zgodna kolorowe tła w pliku ikony

      * Konto testów sprawdzania poprawności
      * Skontaktuj się z pomocą techniczną

   2. Jeśli potrzebujemy więcej informacji, firma Microsoft jest skontaktuje się z Tobą.

3. **Publikowanie**

    Po możemy zweryfikować funkcjonalność i zawartości z łącznika, firma Microsoft we wszystkich produktach i regiony etap łącznika do wdrożenia.
    
    Domyślnie wszystkie łączniki są publikowane jako łączników "premium". 
    Jeśli utworzono aplikację w usłudze Azure można stosować do wyświetlania Twojego łącznika jako "standardowy" łącznik, który jest dostępny dla wszystkich użytkowników z planami Office 365 Enterprise. 
    Aby uzyskać więcej informacji poproś kontakt nazwy.

## <a name="next-steps"></a>Następne kroki

* [Łącznik niestandardowy — często zadawane pytania](../logic-apps/custom-connector-faq.md)
* [Łącznik niestandardowy — omówienie](../logic-apps/custom-connector-overview.md)