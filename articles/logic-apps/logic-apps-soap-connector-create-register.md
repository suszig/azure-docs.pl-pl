---
title: "Utwórz i zarejestruj łączniki SOAP - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Konfigurowanie łączników protokołu SOAP do użycia w usłudze Azure Logic Apps"
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
ms.date: 10/24/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 031762e5639fc52e0b0a6a5bf8d12db25da25e12
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/03/2018
---
# <a name="create-and-register-soap-connectors-in-azure-logic-apps"></a>Utwórz i zarejestruj łączniki SOAP w aplikacjach logiki platformy Azure

Aby zintegrować usługi SOAP w swoich przepływach pracy aplikacji logiki, można utworzyć i zarejestrować łącznik niestandardowy protokołu Simple Object Access Protocol (SOAP) przy użyciu języka Web Services Description Language (WSDL) opisującego usługę protokołu SOAP. Łączniki SOAP działają podobnie jak wstępnie skonfigurowane łączniki, dzięki czemu można używać ich tak samo jak innych łączników w aplikacjach logiki.


## <a name="prerequisites"></a>Wymagania wstępne

Do zarejestrowania łącznika protokołu SOAP potrzebne są następujące elementy:

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, możesz rozpocząć pracę z [bezpłatnym kontem platformy Azure](https://azure.microsoft.com/free/). W przeciwnym razie zarejestruj się, aby [skorzystać z subskrypcji z płatnością zgodnie z rzeczywistym użyciem](https://azure.microsoft.com/pricing/purchase-options/).

* Dowolny spośród następujących elementów:
  * Adres URL do środowiska języka WSDL, który definiuje usługę protokołu SOAP i interfejsy API
  * Plik języka WSDL, który definiuje usługę protokołu SOAP i interfejsy API

  W tym samouczku można użyć naszego przykładu [usługi protokołu SOAP do obsługi zamówień](http://fazioapisoap.azurewebsites.net/FazioService.svc?singleWsdl).

* Opcjonalnie: obraz do użycia jako ikona łącznika niestandardowego


## <a name="1-create-your-connector"></a>1. Tworzenie łącznika

1. W witrynie Azure Portal w głównym menu platformy Azure wybierz pozycję **Nowy**. W polu wyszukiwania wpisz „logic apps connector” jako filtr i naciśnij klawisz Enter.

   ![Nowy, wyszukiwanie ciągu „logic apps connector”](./media/logic-apps-soap-connector-create-register/create-logic-apps-connector.png)

2. Z listy wyników wybierz pozycję **Logic Apps Connector** > **Utwórz**.

   ![Tworzenie łącznika usługi Logic Apps](./media/logic-apps-soap-connector-create-register/choose-logic-apps-connector.png)

3. Podaj szczegóły potrzebne do zarejestrowania łącznika, jak opisano w tabeli. Gdy skończysz, wybierz pozycję **Przypnij do pulpitu nawigacyjnego** > **Utwórz**.

   ![Szczegóły łącznika niestandardowego usługi Logic Apps](./media/logic-apps-soap-connector-create-register/logic-apps-soap-connector-details.png)

   | Właściwość | Sugerowana wartość | Opis | 
   | -------- | --------------- | ----------- | 
   | **Nazwa** | *nazwa-łącznika-soap* | Podaj nazwę łącznika. | 
   | **Subskrypcja** | *nazwa-subskrypcji-platformy-Azure* | Wybierz swoją subskrypcję platformy Azure. | 
   | **Grupa zasobów** | *nazwa-grupy-zasobów-platformy-Azure* | Utwórz lub wybierz grupę platformy Azure na potrzeby organizowania zasobów platformy Azure. | 
   | **Lokalizacja** | *region-wdrożenia* | Wybierz region wdrożenia łącznika. | 
   |||| 

   Po wdrożeniu łącznika na platformie Azure zostanie automatycznie otwarte menu łącznika aplikacji logiki. 
   Jeśli tak się nie stanie, wybierz łącznik protokołu SOAP na pulpicie platformy Azure.

## <a name="2-define-your-connector"></a>2. Definiowanie łącznika

Teraz określ plik WSDL lub adres URL na potrzeby tworzenia łącznika, uwierzytelniania używanego przez łącznik oraz akcji i wyzwalaczy udostępnianych przez łącznik protokołu SOAP.


### <a name="2a-specify-the-wsdl-file-or-url-for-your-connector"></a>2a. Określanie pliku WSDL lub adres URL dla łącznika

1. Z menu łącznika wybierz pozycję **Łącznik usługi Logic Apps**, jeśli nie została jeszcze wybrana. Na pasku narzędzi wybierz pozycję **Edytuj**.

   ![Edytuj łącznik niestandardowy](./media/logic-apps-soap-connector-create-register/edit-soap-connector.png)

2. Wybierz pozycję **Ogólne**, aby w poniższych tabelach podać szczegółowe informacje na potrzeby tworzenia, zabezpieczania i definiowania akcji oraz wyzwalacza dla łącznika protokołu SOAP.

   1. W obszarze **Łączniki niestandardowe** wybierz pozycję **SOAP** dla pozycji **Punkt końcowy interfejsu API**, aby umożliwić udostępnienie pliku WSDL opisującego interfejs API.

      ![Określanie pliku WSDL dla interfejsu API](./media/logic-apps-soap-connector-create-register/provide-wsdl-file.png)

      | Opcja | Format |Opis | 
      | ------ | ------ | ----------- | 
      | **Przekaż dane WSDL z pliku** | *plik-WSDL* | Przejdź do lokalizacji pliku WSDL, a następnie wybierz ten plik. | 
      | **Przekaż dane WSDL z adresu URL** | http://*ścieżka-do-pliku-wsdl* | Podaj adres URL pliku WSDL usługi. | 
      | **Konwersja z interfejsu SOAP na interfejs REST** |   | Przekształć interfejsy API w usłudze SOAP na interfejsy API REST. | 
      |||| 

   2. W obszarze **Ogólne informacje** przekaż ikonę łącznika. 
   Pola **Opis**, **Host** i **Podstawowy adres URL** są przeważnie automatycznie wypełniane na podstawie pliku WSDL. 
   W przeciwnym razie dodaj te informacje zgodnie z opisem w tabeli i wybierz pozycję **Kontynuuj**. 

      ![Szczegóły łącznika](./media/logic-apps-soap-connector-create-register/add-general-details.png)

      | Opcja lub ustawienie | Format | Opis | 
      | ----------------- | ------ | ----------- | 
      | **Przekaż ikonę** | *plik-png-lub-jpeg-mniejszy-niż-1-MB* | Ikona reprezentująca łącznik <p>Kolor: najlepiej białe logo na kolorowym tle. <p>Wymiary: logo ~160 pikseli wewnątrz kwadratu 230 pikseli | 
      | **Kolor tła ikony** | *kod-szesnastkowy-koloru-marki-ikony* | <p>Kolor tła za ikoną, który odpowiada kolorowi tła w pliku ikony. <p>Format: szesnastkowy. Na przykład kod #007ee5 reprezentuje kolor niebieski. | 
      | **Opis** | *opis-łącznika* | Podaj krótki opis łącznika. | 
      | **Host** | *host-łącznika* | Podaj domenę hosta usługi protokołu SOAP. | 
      | **Podstawowy adres URL** | *łącznik-podstawowego-adresu-URL* | Podaj podstawowy adres URL usługi protokołu SOAP. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. Podawanie opisu uwierzytelniania używanego przez łącznik

1. Teraz wybierz pozycję **Zabezpieczenia**, aby przejrzeć lub opisać uwierzytelnianie używane przez łącznik. Dzięki uwierzytelnianiu masz pewność, że przepływ tożsamości użytkowników między usługą i dowolnymi klientami jest prawidłowy.

   Domyślnie w obszarze **Typ uwierzytelniania** jest ustawiana wartość **Bez uwierzytelniania**.
   
   ![Typ uwierzytelniania](./media/logic-apps-soap-connector-create-register/security-authentication-options.png)

   Aby zmienić typ uwierzytelniania, wybierz pozycję **Edytuj**. Możesz wybrać pozycję **Uwierzytelnianie podstawowe**. Aby korzystać z etykiet parametrów innych niż wartości domyślne, zaktualizuj je w obszarze **Etykieta parametru**.

   ![Uwierzytelnianie podstawowe](./media/logic-apps-soap-connector-create-register/security.png)

   
2. Aby zapisać łącznik po wprowadzeniu informacji o zabezpieczeniach, w górnej części strony wybierz pozycję **Zaktualizuj łącznik**, a następnie wybierz pozycję **Kontynuuj**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Przeglądanie, aktualizowanie lub definiowanie akcji i wyzwalaczy łącznika

1. Teraz wybierz pozycję **Definicja**, aby przeglądać, edytować lub definiować nowe akcje i wyzwalacze, które użytkownicy mogą dodawać do swoich przepływów pracy.

   Akcje i wyzwalacze są oparte na operacjach zdefiniowanych w pliku WSDL, które automatycznie wypełniają stronę **Definicja** i obejmują wartości żądania i odpowiedzi. Dlatego jeśli wymagane operacje są już widoczne w tym miejscu, możesz przejść do następnego kroku w procesie rejestracji bez wprowadzania zmian na tej stronie.

   ![Definicja łącznika](./media/logic-apps-soap-connector-create-register/definition.png)

2. Opcjonalnie, jeśli chcesz edytować istniejące działania i jest wyzwalane, lub dodać nowe, [wykonaj te czynności](logic-apps-custom-connector-register.md#add-action-or-trigger).


## <a name="3-finish-creating-your-connector"></a>3. Kończenie tworzenia łącznika

Po zakończeniu wybierz pozycję **Zaktualizuj łącznik**, aby umożliwić wdrożenie łącznika. 

Gratulacje! Teraz w przypadku tworzenia aplikacji logiki możesz znaleźć łącznik w Projektancie aplikacji usługi Logic Apps i dodać go do aplikacji logiki.

![Wyszukiwanie łącznika w Projektancie aplikacji usługi Logic Apps](./media/logic-apps-soap-connector-create-register/soap-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Udostępnianie łącznika innym użytkownikom usługi Logic Apps

Zarejestrowane, ale niecertyfikowane łączniki działają tak jak łączniki zarządzane przez firmę Microsoft, ale są widoczne i dostępne *tylko* dla autora łącznika oraz użytkowników, którzy mają tę samą dzierżawę usługi Azure Active Directory i subskrypcję platformy Azure dla aplikacji logiki w regionie, w których te aplikacje są wdrażane. Udostępnianie jest opcjonalne, ale w niektórych scenariuszach udostępnienie łączników innym użytkownikom może być potrzebne. 

> [!IMPORTANT]
> Jeśli udostępnisz łącznik, praca innych osób może zacząć od niego zależeć. 
> ***Usunięcie łącznika powoduje usunięcie wszystkich połączeń z tym łącznikiem.***
 
Aby udostępnić użytkownikom zewnętrznym poza te granice, na przykład wszyscy użytkownicy aplikacji logiki, łącznika programu [przesłać Twojego łącznika do certyfikacji Microsoft](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>Często zadawane pytania

**Pyt.:** Czy łącznik protokołu SOAP jest ogólnie dostępny? </br>
**Odp.:** Łącznik protokołu SOAP jest dostępny w **wersji zapoznawczej** i nie jest jeszcze usługą ogólnodostępną.

**Pyt.:** Czy istnieją ograniczenia i znane problemy dotyczące łącznika protokołu SOAP? </br>
**Odp.:** Tak, zobacz [SOAP connector restrictions and known issues (Ograniczenia i znane problemy dotyczące łącznika protokołu SOAP)](../api-management/api-management-api-import-restrictions.md#wsdl).

**Pyt.:** Czy istnieją limity łączników niestandardowych? </br>
**Odp.:** Tak, zobacz [Custom connector limits (Limity łączników niestandardowych)](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej

* Aby uzyskać pomoc techniczną dotyczącą tworzenia i dołączania lub żądania funkcji, które nie są dostępne w kreatorze rejestracji, skontaktuj się z nami pod adresem [condevhelp@microsoft.com](mailto:condevhelp@microsoft.com). Firma Microsoft monitoruje to konto pod kątem problemów i pytań deweloperów, a następnie kieruje je do odpowiednich zespołów.

* Aby zadawać pytania, odpowiadać na nie lub patrzeć, co robią inni użytkownicy usługi Azure Logic Apps, odwiedź [forum usługi Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Aby pomóc w ulepszaniu usługi Logic Apps, przesyłaj pomysły lub głosuj na nie w [witrynie opinii użytkowników usługi Logic Apps](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Kolejne kroki

* [Opcjonalnie: Certyfikować Twojego łącznika](../logic-apps/custom-connector-submit-certification.md)
* [Łącznik niestandardowy — często zadawane pytania](../logic-apps/custom-connector-faq.md)