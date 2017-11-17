---
title: "Utwórz i zarejestruj łączniki SOAP - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Konfigurowanie protokołu SOAP łączniki do użycia w aplikacjach logiki platformy Azure"
author: divyaswarnkar
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
ms.author: LADocs; divswa
ms.openlocfilehash: 0323b0f7ee03dce209d5a71c6711988a34ba7633
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="create-and-register-soap-connectors-in-azure-logic-apps"></a>Utwórz i zarejestruj łączniki SOAP w aplikacjach logiki platformy Azure

Integracja usług SOAP w swoich przepływach pracy aplikacji logiki, można utworzyć i zarejestrować niestandardowego łącznika protokołu dynamicznej konfiguracji hosta (SOAP, Simple Object Access Protocol) przy użyciu sieci Web Services Description Language (WSDL) opisujący usługi protokołu SOAP. Łączniki SOAP działają podobnie wbudowane łączniki, dzięki czemu można używać ich w taki sam sposób jak inne łączniki w aplikacjach logiki.


## <a name="prerequisites"></a>Wymagania wstępne

Aby zarejestrować Twojego łącznika SOAP, potrzebne są następujące elementy:

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, można uruchomić z poziomu [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/). W przeciwnym razie zarejestrować się w celu [subskrypcji płatność za rzeczywiste użycie](https://azure.microsoft.com/pricing/purchase-options/).

* Dowolny element w tym miejscu:
  * Adres URL do języka WSDL, który definiuje usługi SOAP i interfejsy API
  * Plik WSDL, który definiuje usługi SOAP i interfejsy API

  W tym samouczku, korzystając z naszym przykładzie [usługi SOAP zamówień](http://fazioapisoap.azurewebsites.net/FazioService.svc?singleWsdl).

* Opcjonalnie: Obrazu do użycia jako ikona łącznik niestandardowy


## <a name="1-create-your-connector"></a>1. Utwórz łącznik programu

1. W portalu Azure, w menu głównym Azure wybierz **nowy**. W polu wyszukiwania wprowadź "łącznik aplikacje logiki" jako filtr, a następnie naciśnij klawisz Enter.

   ![Nowe Wyszukaj "łącznik aplikacje logiki"](./media/logic-apps-soap-connector-create-register/create-logic-apps-connector.png)

2. Na liście wyników wybierz **łącznik aplikacji logiki** > **Utwórz**.

   ![Tworzenie łącznika Logic Apps](./media/logic-apps-soap-connector-create-register/choose-logic-apps-connector.png)

3. Podaj szczegóły rejestrowaniu Twojego łącznika zgodnie z opisem w tabeli. Gdy wszystko będzie gotowe, wybierz pozycję **Przypnij do pulpitu nawigacyjnego** > **Utwórz**.

   ![Szczegóły łącznika niestandardowych aplikacji logiki](./media/logic-apps-soap-connector-create-register/logic-apps-soap-connector-details.png)

   | Właściwość | Sugerowana wartość | Opis | 
   | -------- | --------------- | ----------- | 
   | **Nazwa** | *Nazwa protokołu SOAP łącznika* | Podaj nazwę dla Twojego łącznika. | 
   | **Subskrypcja** | *Nazwa w przypadku subskrypcji platformy Azure* | Wybierz subskrypcję platformy Azure. | 
   | **Grupa zasobów** | *Azure-resource grupy name* | Utwórz lub wybierz grupę Azure służący do organizowania zasobów platformy Azure. | 
   | **Lokalizacja** | *region wdrożenia* | Wybierz region wdrożenia dla Twojego łącznika. | 
   |||| 

   Po Azure wdraża z łącznika, automatycznie otwiera menu łącznik aplikacji logiki. 
   W przeciwnym razie wybierz łącznik programu soap z pulpitu nawigacyjnego platformy Azure.

## <a name="2-define-your-connector"></a>2. Zdefiniuj Twojego łącznika

Teraz określić plik WSDL lub adres URL do tworzenia łącznika programu, uwierzytelniania, który korzysta z łącznika, i akcji i wyzwalacze, które zapewnia Twojego łącznika soap


### <a name="2a-specify-the-wsdl-file-or-url-for-your-connector"></a>2a. Określ plik WSDL lub adres URL dla Twojego łącznika

1. W menu z łącznika, jeśli jeszcze nie jest wybrana, wybierz **łącznik aplikacji logiki**. Na pasku narzędzi wybierz **Edytuj**.

   ![Edytuj łącznik niestandardowy](./media/logic-apps-soap-connector-create-register/edit-soap-connector.png)

2. Wybierz **ogólne** tak, aby uzyskać szczegółowe informacje w poniższych tabelach zapewniają do tworzenia, zabezpieczanie i definiowanie akcje i Wyzwalacze dla Twojego łącznika protokołu SOAP.

   1. Dla **łączniki niestandardowe**, wybierz pozycję **SOAP** dla Twojego **punkt końcowy interfejsu API** , możesz podać pliku WSDL, który opisuje interfejs API.

      ![Określ plik WSDL dla interfejsu API](./media/logic-apps-soap-connector-create-register/provide-wsdl-file.png)

      | Opcja | Format |Opis | 
      | ------ | ------ | ----------- | 
      | **Przekaż WSDL z pliku** | *Plik WSDL* | Przejdź do lokalizacji pliku WSDL, a następnie wybierz tego pliku. | 
      | **Przekaż WSDL z adresu URL** | http://*ścieżki do wsdl pliku* | Podaj adres URL pliku WSDL usługi. | 
      | **SOAP resztą** |   | Przekształć interfejsów API w usłudze SOAP w interfejsów API REST. | 
      |||| 

   2. Aby uzyskać **ogólne informacje**, Przekaż ikonę dla Twojego łącznika. 
   Zazwyczaj **opis**, **hosta**, i **podstawowego adresu URL** pola zostaną automatycznie wypełnione z pliku WSDL. 
   Ale jeśli nie, Dodaj te informacje, zgodnie z opisem w tabeli i wybierz polecenie **Kontynuuj**. 

      ![Szczegóły łącznika](./media/logic-apps-soap-connector-create-register/add-general-details.png)

      | Opcja lub ustawienia | Format | Opis | 
      | ----------------- | ------ | ----------- | 
      | **Przekaż ikonę** | *PNG-or-jpg-File-under-1-MB* | Ikona, który reprezentuje łącznik programu <p>Kolor: Najlepiej białe logo względem koloru tła. <p>Wymiary: Logo ~ 160 pikseli wewnątrz kwadratu 230 pikseli | 
      | **Kolor tła ikon** | *Ikona marki — kolor — szesnastkowy — kod* | <p>Kolor tła z ikonę, która odpowiada kolor tła w pliku ikony. <p>Format: szesnastkową. Na przykład #007ee5 reprezentuje kolor niebieski. | 
      | **Opis** | *Opis łącznika* | Podaj krótki opis Twojego łącznika. | 
      | **Host** | *Łącznik hosta* | Podaj domeny hosta usługi protokołu SOAP. | 
      | **Podstawowy adres URL** | *Łącznik podstawowego adresu URL* | Podaj podstawowy adres URL usługi protokołu SOAP. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. Opis uwierzytelniania, który korzysta z łącznika

1. Teraz wybierz **zabezpieczeń** , możesz przejrzeć lub opisano uwierzytelniania, który korzysta z łącznika. Uwierzytelnianie zapewnia odpowiednio przepływu tożsamości użytkowników między usługą i wszystkich klientów.

   Domyślnie z łącznika w **typ uwierzytelniania** ustawiono **bez uwierzytelniania**.
   
   ![Typ uwierzytelniania](./media/logic-apps-soap-connector-create-register/security-authentication-options.png)

   Aby zmienić typ uwierzytelniania, wybierz **Edytuj**. Możesz wybrać **uwierzytelnianie podstawowe**. Aby korzystanie z etykiet parametru wartości inne niż domyślne, należy zaktualizować je w obszarze **Etykieta parametru**.

   ![Uwierzytelnianie podstawowe](./media/logic-apps-soap-connector-create-register/security.png)

   
2. Aby zapisać Twoje łącznika po wprowadzeniu informacji o zabezpieczeniach, w górnej części strony, wybierz polecenie **zaktualizować łącznika**, a następnie wybierz **Kontynuuj**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Przeglądanie, zaktualizować lub zdefiniuj akcje i Wyzwalacze dla Twojego łącznika

1. Teraz wybierz **definicji** , można przejrzeć, edytować lub zdefiniuj nowe akcje i wyzwalacze, które użytkownicy mogą dodawać do ich przepływów pracy.

   Akcje i wyzwalacze są oparte na operacje zdefiniowane w pliku WSDL, które automatycznie wypełnić **definicji** obejmują żądania i odpowiedzi wartości i strony. Tak Jeśli wymagane operacje już występować w tym miejscu, można przejść do następnego kroku w procesie rejestracji bez wprowadzania zmian na tej stronie.

   ![Definicja łącznika](./media/logic-apps-soap-connector-create-register/definition.png)

2. Opcjonalnie, jeśli chcesz edytować istniejące działania i jest wyzwalane, lub dodać nowe, [wykonaj te czynności](logic-apps-custom-connector-register.md#add-action-or-trigger).


## <a name="3-finish-creating-your-connector"></a>3. Zakończ tworzenie łącznika programu

Gdy wszystko jest gotowe, wybierz pozycję **łącznika aktualizacji** aby umożliwić wdrażanie Twojego łącznika. 

Gratulacje! Teraz podczas tworzenia aplikacji logiki można znaleźć z łącznika w Projektancie aplikacji logiki i Dodaj ten łącznik do aplikacji logiki.

![W Projektancie aplikacji logiki znajdź łącznik programu](./media/logic-apps-soap-connector-create-register/soap-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Udostępnianie Twojego łącznika innym użytkownikom Logic Apps

Zarejestrowane, ale Niepoświadczone łączników niestandardowych działają podobnie łączniki zarządzany przez firmę Microsoft, ale są widoczne i dostępne *tylko* autora łącznika i użytkowników, którzy mają tej samej dzierżawy usługi Azure Active Directory i subskrypcji platformy Azure w przypadku aplikacji logiki w regionie, w których te aplikacje są wdrożone. Mimo że udostępnianie jest opcjonalny, konieczne może być scenariuszy, w którym chcesz udostępnić innym użytkownikom łączników. 

> [!IMPORTANT]
> Jeśli łącznik zostanie udostępniona, inne rozpoczęcie zależą od tego łącznika. 
> ***Usunięcie łącznika programu powoduje usunięcie wszystkich połączeń z tego łącznika.***
 
Aby udostępnić użytkownikom zewnętrznym poza te granice, na przykład wszyscy użytkownicy aplikacji logiki, łącznika programu [przesłać Twojego łącznika do certyfikacji Microsoft](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>Często zadawane pytania

**Pytanie:** jest SOAP łącznik ogólnie dostępna (GA)? </br>
**A:** SOAP łącznik jest w **Podgląd**, i nie zostało GA usługi.

**Pytanie:** istnieją żadnych ograniczeń i znane problemy dotyczące łącznika SOAP? </br>
**Odpowiedź:** tak, zobacz [ograniczenia łącznika SOAP i znane problemy](../api-management/api-management-api-import-restrictions.md#wsdl).

**Pytanie:** istnieją żadnych limitów dla łączników niestandardowych? </br>
**Odpowiedź:** tak, zobacz [łącznik niestandardowy ogranicza tutaj](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Uzyskaj pomoc techniczną

* Obsługa tworzenia i dołączania lub do żądania funkcji, które nie są dostępne w kreatorze Rejestracja, skontaktuj się z [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com). Firma Microsoft monitoruje tego konta na problemów i pytania deweloperów i kieruje je do odpowiednich zespołu.

* Aby zadać lub odpowiedzi na pytania, lub zobacz, co robią innych użytkowników usługi Azure Logic Apps, odwiedź stronę [forum usługi Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Aby pomóc w udoskonalaniu Logic Apps, Zagłosuj lub Prześlij pomysłów na [witrynę opinii użytkowników Logic Apps](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Następne kroki

* [Opcjonalnie: Certyfikować Twojego łącznika](../logic-apps/custom-connector-submit-certification.md)
* [Łącznik niestandardowy — często zadawane pytania](../logic-apps/custom-connector-faq.md)