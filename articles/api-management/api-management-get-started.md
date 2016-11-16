---
title: "Zarządzanie pierwszym interfejsem API w usłudze Azure API Management | Microsoft Docs"
description: "Naucz się tworzyć interfejsy API, dodawać operacje i zacznij korzystać z usługi API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 51b7df8b-1c43-43c6-90c9-0aa24f48206b
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 10/25/2016
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 587c7346bcb8e6549febd3904c8d0a9e46cbc50a


---
# <a name="manage-your-first-api-in-azure-api-management"></a>Zarządzanie pierwszym interfejsem API w usłudze Azure API Management
## <a name="overview"> </a>Omówienie
W tym przewodniku przedstawiono, jak szybko rozpocząć korzystanie z usługi Azure API Management i utworzyć pierwsze wywołanie interfejsu API.

## <a name="concepts"> </a>Co to jest usługa Azure API Management?
Usługa Azure API Management umożliwia użycie dowolnego oprogramowania zaplecza i uruchomienie opartego na nim, w pełni funkcjonalnego programu interfejsu API.

Typowe scenariusze obejmują:

* **Zabezpieczanie infrastruktury mobilnej** przez uzyskanie dostępu bramowego z kluczami interfejsu API, zapobieganie atakom DOS przy użyciu ograniczania przepływności lub korzystanie z zaawansowanych zasad zabezpieczeń, takich jak sprawdzanie poprawności tokenu JWT.
* **Włączanie ekosystemów partnerów niezależnych dostawców oprogramowania** dzięki oferowaniu szybkiego dołączania partnerów za pośrednictwem portalu dla deweloperów i tworzeniu fasady interfejsu API w celu oddzielenia od wewnętrznych implementacji, które nie są dojrzałe do użycia przez partnera.
* **Uruchamianie wewnętrznego programu interfejsu API** dzięki oferowaniu organizacji scentralizowanej lokalizacji w celu komunikowania dostępności oraz ostatnich zmian wprowadzonych do interfejsów API, uzyskania dostępu bramowego za pomocą kont organizacyjnych, a wszystko to na podstawie zabezpieczonego kanału między bramą interfejsu API a zapleczem.

System składa się z następujących składników:

* **Brama interfejsu API** to punkt końcowy, który ma następujące zastosowania:
  
  * Przyjmowanie wywołań API i kierowanie ich do zaplecza.
  * Weryfikowanie kluczy interfejsu API, tokenów JWT, certyfikatów i innych poświadczeń.
  * Wymuszanie przydziałów użycia i ograniczeń liczby wywołań.
  * Przekształcanie interfejsu API na bieżąco, bez modyfikacji kodu.
  * Buforowanie odpowiedzi zaplecza w skonfigurowanym miejscu.
  * Rejestrowanie w dzienniku metadanych wywołań w celu analizy.
* **Portal wydawcy** to interfejs administracyjny, w którym konfiguruje się program interfejsu API. Jego zastosowania to:
  
  * Definiowanie lub importowanie schematu interfejsu API.
  * Tworzenie pakietów interfejsów API do produktów.
  * Konfigurowanie zasad, takich jak przydziały lub przekształcenia w interfejsach API.
  * Uzyskiwanie szczegółowych informacji analitycznych.
  * Zarządzanie użytkownikami.
* **Portal dla deweloperów** służy jako główna witryna internetowa dla deweloperów, która umożliwia im:
  
  * Czytanie dokumentacji interfejsu API.
  * Wypróbowanie interfejsu API za pośrednictwem interakcyjnej konsoli.
  * Tworzenie konta i subskrybowanie, aby uzyskać klucze interfejsu API.
  * Zyskanie dostępu do analiz własnego użycia.

## <a name="create-service-instance"> </a>Tworzenie wystąpienia usługi API Management
> [!NOTE]
> Do ukończenia tego samouczka jest potrzebne konto platformy Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatna wersja próbna platformy Azure][Bezpłatna wersja próbna platformy Azure].
> 
> 

Pierwszym krokiem podczas pracy z usługą API Management jest utworzenie wystąpienia usługi. Zaloguj się do witryny [Azure Portal][Azure Portal], a następnie kliknij kolejno pozycje **Nowy**, **Sieć Web i mobilność** i **API Management**.

![Nowe wystąpienie usługi API Management][api-management-create-instance-menu]

W polu **Nazwa** podaj unikatową nazwę domeny podrzędnej do użycia jako adres URL usługi.

W polach **Subskrypcja**, **Grupa zasobów** i **Lokalizacja** wybierz wartości odpowiadające wystąpieniu usługi.

Wprowadź wartość **Contoso Ltd.** w polu **Nazwa organizacji**, a następnie wprowadź adres e-mail w polu **Adres e-mail administratora**.

> [!NOTE]
> Ten adres e-mail służy do wysyłania powiadomienia z systemu usługi API Management. Aby uzyskać więcej informacji, zobacz [How to configure notifications and email templates in Azure API Management][How to configure notifications and email templates in Azure API Management] (Jak konfigurować powiadomienia i szablony wiadomości e-mail w usłudze Azure API Management).
> 
> 

![Nowa usługa API Management][api-management-create-instance-step1]

Wystąpienia usługi API Management są dostępne w trzech warstwach: Deweloper, Standardowej i Premium.

> [!NOTE]
> Warstwa Deweloper służy do rozwoju, testowania i pilotażu programów interfejsu API, gdzie wysoka dostępność nie jest wymagana. W przypadku warstw Standardowej i Premium można skalować liczbę zarezerwowanych jednostek do obsługi większego ruchu. Warstwy Standardowa i Premium zapewniają usłudze API Management najlepszą moc obliczeniową i wydajność. W tym samouczku można korzystać z dowolnej warstwy. Aby uzyskać więcej informacji na temat warstw usługi API Management, zobacz [API Management — cennik][API Management — cennik].
> 
> 

Kliknij przycisk **Utwórz**, aby rozpocząć aprowizowanie wystąpienia usługi.

![Nowa usługa API Management][api-management-instance-created]

Następnym krokiem po utworzeniu wystąpienia usługi jest tworzenie lub importowanie interfejsu API.

## <a name="create-api"> </a>Importowanie interfejsu API
Interfejs API składa się z zestawu operacji, które mogą być wywoływane z poziomu aplikacji klienckiej. Operacje interfejsu API są przekierowywane do istniejących usług sieci Web.

Interfejsy API mogą być tworzone ręcznie (wraz z dodawaniem operacji) lub importowane. W tym samouczku zaimportujemy interfejs API dla przykładowej usługi sieci Web kalkulatora dostarczanej przez firmę Microsoft i hostowanej na platformie Azure.

> [!NOTE]
> Aby uzyskać wskazówki na temat tworzenia interfejsu API i ręcznego dodawania operacji, zobacz artykuły [How to create APIs](api-management-howto-create-apis.md) (Tworzenie interfejsów API) i [How to add operations to an API](api-management-howto-add-operations.md) (Dodawanie operacji do interfejsu API).
> 
> 

Interfejsy API są konfigurowane w portalu wydawcy. Aby przejść do niego, kliknij pozycję **Portal wydawcy** na pasku narzędzi usługi.

![Portal wydawcy][api-management-management-console]

Aby zaimportować interfejs API kalkulatora, kliknij opcję **Interfejsy API** w menu **API Management** po lewej stronie, a następnie kliknij przycisk **Importuj interfejs API**.

![Przycisk do importowania interfejsu API][api-management-import-api]

Wykonaj poniższe kroki, aby skonfigurować interfejs API kalkulatora:

1. Kliknij opcję **Z adresu URL**, wprowadź **http://calcapi.cloudapp.net/calcapi.json** w polu tekstowym **Adres URL dokumentu specyfikacji**, a następnie kliknij przycisk opcji **Swagger**.
2. Wpisz **calc** w polu tekstowym **Sufiks adresu URL interfejsu API sieci Web**.
3. Kliknij pole **Produkty (opcjonalne)** i wybierz produkt **Starter**.
4. Kliknij przycisk **Zapisz**, aby zaimportować interfejs API.

![Dodawanie nowego interfejsu API][api-management-import-new-api]

> [!NOTE]
> Usługa **API Management** obecnie obsługuje importowanie obu wersji dokumentu Swagger (1.2 i 2.0). Zwróć uwagę, że chociaż w [specyfikacji Swagger 2.0](http://swagger.io/specification) zadeklarowano, że właściwości `host`, `basePath`, i `schemes` są opcjonalne, dokument Swagger 2.0 **MUSI** zawierać te właściwości. W przeciwnym razie nie zostanie zaimportowany. 
> 
> 

Po zaimportowaniu interfejsu API w portalu wydawcy zostanie wyświetlona strona podsumowania dotycząca interfejsu API.

![Podsumowanie interfejsu API][api-management-imported-api-summary]

Sekcja interfejsu API ma kilka kart. Karta **Podsumowanie** wyświetla podstawowe metryki i informacje o interfejsie API. Karta [Ustawienia](api-management-howto-create-apis.md#configure-api-settings) służy do wyświetlania i edytowania konfiguracji dotyczącej interfejsu API. Karta [Operacje](api-management-howto-add-operations.md) służy do zarządzania operacjami interfejsu API. Karta **Zabezpieczenia** służy do konfigurowania uwierzytelniania bramy dla serwera zaplecza przy użyciu uwierzytelniania podstawowego lub [wzajemnego uwierzytelniania za pomocą certyfikatu](api-management-howto-mutual-certificates.md), a także konfigurowania [uwierzytelniania użytkownika przy użyciu protokołu OAuth 2.0](api-management-howto-oauth2.md).  Karta **Problemy** służy do wyświetlania problemów zgłoszonych przez deweloperów, którzy korzystają z Twoich interfejsów API. Karta **Produkty** służy do konfigurowania produktów, które zawierają ten interfejs API.

Domyślnie każde wystąpienie usługi API Management zawiera dwa produkty przykładowe:

* **Starter (początkowy)**
* **Unlimited (nieograniczony)**

W tym samouczku interfejs API o nazwie Basic Calculator został dodany do produktu Starter podczas importowania interfejsu API.

Przed wywołaniem interfejsu API deweloperzy muszą najpierw subskrybować produkt umożliwiający im dostęp. Deweloperzy mogą subskrybować produkty w portalu dla deweloperów, a administratorzy mogą subskrybować dla deweloperów produkty w portalu wydawcy. Jesteś administratorem od momentu utworzenia wystąpienia usługi API Management w poprzednich krokach samouczka, więc domyślnie już masz subskrybowany każdy produkt.

## <a name="call-operation"> </a>Wywoływanie operacji z portalu dla deweloperów
Operacje mogą być wywoływane bezpośrednio z portalu dla deweloperów, który zapewnia wygodny sposób wyświetlania i testowania operacji interfejsu API. W tym kroku samouczka wywołasz operację **Add two integers** (Dodawanie dwóch liczb całkowitych) interfejsu API Basic Calculator. Kliknij opcję **Portal dla deweloperów** w menu po prawej stronie u góry portalu wydawcy.

![Portal dla deweloperów][api-management-developer-portal-menu]

Kliknij opcję **Interfejsy API** w górnym menu, a następnie kliknij pozycję **Basic Calculator**, aby wyświetlić dostępne operacje.

![Portal dla deweloperów][api-management-developer-portal-calc-api]

Należy pamiętać, że przykładowe opisy i parametry, które zostały zaimportowane wraz interfejsem API i operacjami, stanowią dokumentację dla deweloperów, którzy będą korzystali z tej operacji. Te opisy można również dodawać podczas ręcznego dodawania operacji.

Aby wywołać operację **Add two integers** (Dodawanie dwóch liczb całkowitych), kliknij przycisk **Wypróbuj**.

![Wypróbuj][api-management-developer-portal-calc-api-console]

Możesz wprowadzić inne wartości parametrów lub zachować ustawienia domyślne, a następnie kliknąć opcję **Wyślij**.

![HTTP Get][api-management-invoke-get]

Po wywołaniu operacji portal dla deweloperów wyświetla **stan odpowiedzi**, **nagłówki odpowiedzi** oraz wszelką **zawartość odpowiedzi**.

![Odpowiedź][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Wyświetlanie analiz
Aby wyświetlić analizy dotyczące interfejsu API Basic Calculator, przełącz się ponownie do portalu wydawcy, wybierając opcję **Zarządzaj** w menu po prawej stronie u góry portalu dla deweloperów.

![Zarządzaj][api-management-manage-menu]

Widok domyślny dla portalu wydawcy to **pulpit nawigacyjny**, który zawiera przegląd wystąpienia usługi API Management.

![Pulpit nawigacyjny][api-management-dashboard]

Umieść kursor myszy nad wykresem dla interfejsu API **Basic Calculator**, aby zobaczyć konkretne metryki dotyczące użycia interfejsu API w danym okresie.

> [!NOTE]
> Jeśli nie widzisz żadnych linii na wykresie, powróć do portalu dla deweloperów i wykonaj kilka wywołań interfejsu API, poczekaj chwilę, a następnie wróć do pulpitu nawigacyjnego.
> 
> 

Kliknij przycisk **Wyświetl szczegóły**, aby wyświetlić stronę podsumowania dla interfejsu API, tym większą wersję wyświetlanych metryk.

![Analiza][api-management-mouse-over]

![Podsumowanie][api-management-api-summary-metrics]

Szczegółowe metryki i raporty uzyskasz po kliknięciu opcji **Analiza** w menu **API Management** lewej stronie.

![Omówienie][api-management-analytics-overview]

Sekcja **Analiza** zawiera cztery karty:

* **Rzut oka** zapewnia ogólne metryki użycia i kondycji, a także listy najważniejszych deweloperów, produktów, interfejsów API i operacji.
* **Użycie** zapewnia głębszy wgląd w wywołania interfejsu API i przepustowość, w tym reprezentację geograficzną.
* **Kondycja** koncentruje się na kodach stanu, współczynnikach pomyślnego użycia pamięci podręcznej, czasach odpowiedzi oraz czasach odpowiedzi interfejsu API i usług.
* **Działanie** udostępnia raporty zawierające szczegółowe informacje o określonym działaniu według dewelopera, produktu, interfejsu API i operacji.

## <a name="next-steps"> </a>Następne kroki
* Dowiedz się, w jaki sposób [chronić interfejs API przy użyciu ograniczeń liczby wywołań](api-management-howto-product-with-rules.md).

[Bezpłatna wersja próbna platformy Azure]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Tworzenie wystąpienia usługi API Management]: #create-service-instance
[Tworzenie interfejsu API]: #create-api
[Dodawanie operacji]: #add-operation
[Dodawanie nowego interfejsu API do produktu]: #add-api-to-product
[Subskrypcja produktu, który zawiera interfejs API]: #subscribe
[Wywoływanie operacji z portalu dla deweloperów]: #call-operation
[Wyświetlanie analiz]: #view-analytics
[Następne kroki]: #next-steps


[Zarządzanie kontami deweloperów w usłudze Azure API Management]: api-management-howto-create-or-invite-developers.md
[Konfigurowanie ustawień interfejsu API]: api-management-howto-create-apis.md#configure-api-settings
[How to configure notifications and email templates in Azure API Management]: api-management-howto-configure-notifications.md
[Odpowiedzi]: api-management-howto-add-operations.md#responses
[Jak utworzyć i opublikować produkt]: api-management-howto-add-products.md
[API Management — cennik]: http://azure.microsoft.com/pricing/details/api-management/

[Azure Portal]: https://portal.azure.com/

[usługa API Management — konsola zarządzania]: ./media/api-management-get-started/api-management-management-console.png
[usługa API Management — menu tworzenia wystąpień]: ./media/api-management-get-started/api-management-create-instance-menu.png
[usługa API Management — tworzenie wystąpienia (krok 1)]: ./media/api-management-get-started/api-management-create-instance-step1.png
[usługa API Management — tworzenie wystąpienia (krok 2)]: ./media/api-management-get-started/api-management-create-instance-step2.png
[usługa API Management — wystąpienie utworzone]: ./media/api-management-get-started/api-management-instance-created.png
[usługa API Management — importowanie interfejsu API]: ./media/api-management-get-started/api-management-import-api.png
[usługa API Management — importowanie nowego interfejsu API]: ./media/api-management-get-started/api-management-import-new-api.png
[usługa API Management — podsumowanie zaimportowanego interfejsu API]: ./media/api-management-get-started/api-management-imported-api-summary.png
[usługa API Management — operacje obliczania]: ./media/api-management-get-started/api-management-calc-operations.png
[usługa API Management — tworzenie listy produktów]: ./media/api-management-get-started/api-management-list-products.png
[usługa API Management — dodawanie interfejsu API do produktu]: ./media/api-management-get-started/api-management-add-api-to-product.png
[usługa API Management — dodawanie interfejsu Echo API do produktu]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[usługa API Management — interfejs API dodany do produktu]: ./media/api-management-get-started/api-management-api-added-to-product.png
[usługa API Management — deweloperzy]: ./media/api-management-get-started/api-management-developers.png
[usługa API Management — dodawanie subskrypcji]: ./media/api-management-get-started/api-management-add-subscription.png
[usługa API Management — okno dodawania subskrypcji]: ./media/api-management-get-started/api-management-add-subscription-window.png
[usługa API Management — dodano subskrypcję]: ./media/api-management-get-started/api-management-subscription-added.png
[usługa API Management — menu portalu dla deweloperów]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[usługa API Management — interfejs API obliczania portalu dla deweloperów]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[usługa API Management — konsola interfejsu API obliczania portalu dla deweloperów]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[usługa API Management — wywoływanie polecenia „get”]: ./media/api-management-get-started/api-management-invoke-get.png
[usługa API Management — odpowiedź na wywołanie polecenia „get”]: ./media/api-management-get-started/api-management-invoke-get-response.png
[usługa API Management — menu zarządzania]: ./media/api-management-get-started/api-management-manage-menu.png
[usługa API Management — pulpit nawigacyjny]: ./media/api-management-get-started/api-management-dashboard.png

[usługa API Management — dodawanie odpowiedzi]: ./media/api-management-get-started/api-management-add-response.png
[usługa API Management — okno dodawania odpowiedzi]: ./media/api-management-get-started/api-management-add-response-window.png
[usługa API Management — klucz dla deweloperów]: ./media/api-management-get-started/api-management-developer-key.png
[usługa API Management — umieszczenie wskaźnika myszy]: ./media/api-management-get-started/api-management-mouse-over.png
[usługa API Management — metryki podsumowania interfejsu API]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[usługa API Management — omówienie analizy]: ./media/api-management-get-started/api-management-analytics-overview.png
[usługa API Management — użycie analizy]: ./media/api-management-get-started/api-management-analytics-usage.png
[usługa API Management — ]: ./media/api-management-get-started/api-management-.png
[usługa API Management — ]: ./media/api-management-get-started/api-management-.png



<!--HONumber=Nov16_HO2-->


