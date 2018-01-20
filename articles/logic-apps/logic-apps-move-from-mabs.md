---
title: "Przenoszenie aplikacji z usługi BizTalk Services do usługi Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Przenieś lub migrację MABS usługi BizTalk Azure Logic Apps"
services: logic-apps
documentationcenter: 
author: jonfancey
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: ladocs; jonfan; mandia
ms.openlocfilehash: 01c5376ac5ba9125eede9deb5ee0a7a006a91bb4
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="move-from-biztalk-services-to-logic-apps"></a>Przenoszenie z usługi BizTalk Services do aplikacji logiki

Trwa wycofywanie Microsoft Azure BizTalk usług (MABS). Aby przenieść rozwiązań integracji MABS do usługi Azure Logic Apps, należy użyć w tym temacie. 

## <a name="overview"></a>Przegląd

Usługi BizTalk Services składa się z dwóch usług podrzędne:

1.  Połączeń hybrydowych usług Microsoft BizTalk
2.  Usługi EAI i EDI integracji na podstawie Mostek

Jeśli potrzebujesz przenieść połączeń hybrydowych było możliwe, następnie [połączeń hybrydowych usługi aplikacji Azure](../app-service/app-service-hybrid-connections.md) opisano zmiany i funkcjonalności tej usługi. Azure połączeń hybrydowych zastępuje połączeń hybrydowych usługi BizTalk. Połączenia hybrydowe platformy Azure jest dostępna w usłudze Azure App Service i jest oferowana w portalu Azure. Połączenia hybrydowe platformy Azure udostępnia również nowe Menedżera połączeń hybrydowych do zarządzania istniejących połączeń hybrydowych usługi BizTalk Services i nowych połączeń hybrydowych utworzone w portalu. Azure App Service połączeń hybrydowych było możliwe jest ogólnie dostępna (GA).

Usługi EAI i EDI na podstawie Mostek integracji Logic Apps jest zastąpienie. Logic Apps zapewnia te same możliwości jak usługi BizTalk Services i inne. Logic Apps zapewnia przepływ pracy oparty na użycie w skali chmury oraz funkcji aranżacji, które umożliwiają szybkie i łatwe tworzenie złożonych integracji rozwiązań za pomocą przeglądarki lub narzędzia w programie Visual Studio.

Poniższa tabela zawiera mapowania funkcji usługi BizTalk Services do aplikacji logiki.

| BizTalk Services   | Logic Apps            | Przeznaczenie                  |
| ------------------ | --------------------- | ---------------------------- |
| Łącznik          | Łącznik             | Wysyłanie i odbieranie danych   |
| Mostek             | Aplikacja logiki             | Procesor potoku           |
| Sprawdź poprawność etap     | Sprawdzanie poprawności kodu XML akcji      | Sprawdzanie poprawności względem schematu dokumentu XML             |
| Dodawanie etapu       | Tokeny danych      | Podwyższ poziom właściwości do wiadomości lub decyzje dotyczące routingu             |
| Przekształć etap    | Przekształć akcji      | Konwertowanie wiadomości XML z jednego formatu na inny             |
| Dekodowanie etap       | Płaski akcji dekodowanie pliku      | Konwertowanie pliku prostego na XML             |
| Kodowanie etap       |  Płaski akcji kodowanie pliku      | Konwertowanie z pliku XML do pliku prostego             |
| Inspektor wiadomości       |  Środowisko Azure Functions lub aplikacje interfejsu API      | Uruchamianie niestandardowego kodu w Twojej integracji             |
| Akcja trasy      |  Warunek lub przełącznika      | Rozsyłanie wiadomości do jednej z określonych łączników             |

Istnieje wiele różnych typów artefaktów w usługi BizTalk Services.

## <a name="connectors"></a>Łączniki
Łączniki w usługi BizTalk Services umożliwiają mostków do wysyłania i odbierania danych, w tym dwukierunkowe mostki włączone interakcje oparte na protokole HTTP żądanie/odpowiedź. W aplikacjach logiki tego samego terminologii jest używany. Łączniki w aplikacjach logiki pełnią taką samą funkcję, a również ponad 140, który może łączyć się szerokiego zakresu technologii i usług, zarówno lokalnie za pomocą bramy danych lokalnych (zastępując Usługa karty BizTalk używane przez usługi BizTalk Services), i w chmurze usługi SaaS i PaaS, takie jak OneDrive, usługi Office 365, Dynamics CRM i wiele innych.

Źródeł w usługi BizTalk Services są ograniczone do subskrypcji FTP, SFTP i kolejką usługi Service Bus lub temat.

![](media/logic-apps-move-from-mabs/sources.png)

Każdy mostek ma punkt końcowy HTTP domyślnie, który jest skonfigurowany z adresem środowiska uruchomieniowego i właściwości adres względny mostka. Aby osiągnąć taki sam jak Logic Apps, użyj [żądań i odpowiedzi](../connectors/connectors-native-reqres.md) akcje.

## <a name="xml-processing-and-bridges"></a>Przetwarzanie kodu XML i mostków
Mostek w usługi BizTalk Services jest odpowiednikiem potoku przetwarzania. Mostek może zająć danych otrzymywanych z łącznika, i wykonaj niektóre pracować z danymi, a następnie wyślij je do innego systemu. Logic Apps nie takie same dzięki obsłudze tej samej wzorce interakcji z potoku jako usługi BizTalk Services, a także wiele innych wzorcach integracji. [Mostek żądanie-odpowiedź XML](https://msdn.microsoft.com/library/azure/hh689781.aspx) w usługi BizTalk Services nosi nazwę potoku VETER składające się z etapów, które można:

* V sprawdzania poprawności
* (E) Enrich
* (T) transform
* (E) Enrich
* (R) trasy

Jak pokazano na poniższej ilustracji, przetwarzania są dzielone na żądania i odpowiedzi oraz zapewnia kontrolę nad żądania i odpowiedzi ścieżek oddzielnie (na przykład przy użyciu różnych mapy dla każdego):

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Ponadto XML jednokierunkowe Mostek dodaje dekodowania i kodowanie na początku i końca przetwarzania i przekazujące Mostek zawiera jeden etap Enrich.

### <a name="message-processing-and-decodingencoding"></a>Przetwarzania komunikatu i dekodowania kodowania
W usługi BizTalk Services odbierać wiadomości XML o różnych typach i określić zgodnego schematu odebranej wiadomości. To jest wykonywane w **typów komunikatów** etap potoku przetwarzania receive. Następnie na etapie dekodowania używa typu wykryto komunikat zdekodować przy użyciu podanego schematu. Jeśli schemat jest schematu pliku prostego, konwertuje przychodzące pliku prostego na format XML. 

Logic Apps oferuje podobne możliwości. Odbieranie pliku prostego przez wiele różnych protokołów przy użyciu wyzwalaczy różnych łącznika (systemu plików, FTP, HTTP i tak dalej) i używać [płaskiej dekodowanie pliku](../logic-apps/logic-apps-enterprise-integration-flatfile.md) akcji, aby przekonwertować przychodzących danych XML. Można przenieść z istniejących schematów pliku prostego bezpośrednio do aplikacji logiki, bez konieczności wprowadzania jakichkolwiek zmian, a następnie przekaż schematy do swojego konta integracji.

### <a name="validation"></a>Walidacja
Po przekonwertowaniu przychodzących danych XML (lub jeśli XML został odebrany format komunikatu), sprawdzanie poprawności uruchamia ustalenie, czy komunikat odpowiada schematu XSD. Aby to zrobić w aplikacji logiki, użyj [sprawdzanie poprawności kodu XML](../logic-apps/logic-apps-enterprise-integration-xml-validation.md) akcji. Można ponownie użyć tego samego schematów z usługi BizTalk Services bez wprowadzania żadnych zmian.

### <a name="transform-messages"></a>Przekształć wiadomości
W usługi BizTalk Services etapu przekształcenia konwertuje jeden format komunikatu opartych na języku XML do innego. Jest to realizowane przez stosowanie mapy, przy użyciu mapowania na podstawie TRFM. W aplikacjach logiki proces jest podobny. Akcja przekształcenia wykonuje mapy z Twojego konta integracji. Główna różnica polega na tym, że mapy w aplikacjach logiki są w formacie XSLT. XSLT obejmuje możliwość ponownego użycia istniejącej XSLT już istnieje, w tym mapy utworzone dla serwera BizTalk, zawierające functoids. 

### <a name="routing-rules"></a>Reguły routingu
Usługi BizTalk Services sprawia, że decyzje dotyczące routingu na które punkt końcowy/łącznika w celu wysyłania wiadomości przychodzących/danych. Możliwe przy użyciu opcji filtr routingu jest możliwość wybierz jedną z kilku wstępnie skonfigurowane punkty końcowe:

![](media/logic-apps-move-from-mabs/route-filter.png)

Logic Apps oferuje bardziej zaawansowanych możliwości logiki z [warunku](../logic-apps/logic-apps-use-logic-app-features.md) i [przełącznika](../logic-apps/logic-apps-switch-case.md), włączanie przepływu sterowania zaawansowane i routing. Konwertowanie filtrów routingu w usługi BizTalk Services uzyskuje się za pomocą **warunku** *Jeśli* dostępne są tylko dwie opcje. Jeśli istnieje więcej niż dwa, użyj **przełącznika**.

### <a name="enrich"></a>Dodawanie
Enrich etapem usługi BizTalk Services przetwarzania dodaje właściwości do kontekst komunikat skojarzony z odebrane dane. Na przykład podwyższenie poziomu właściwości do użycia na potrzeby routingu (opisanych poniżej) z wyszukiwania w bazie danych lub wyodrębniając wartość za pomocą wyrażenia XPath. Logic Apps umożliwia dostęp do wszystkie dane kontekstowe dane wyjściowe z poprzedzających akcje, ułatwiając prostego do replikowania takie samo zachowanie. Na przykład za pomocą `Get Row` akcji połączenia SQL, zwróć dane z bazy danych programu SQL Server i używania danych w ramach akcji decyzji dla routingu. Podobnie, właściwości przychodzące usługi Service Bus wiadomości w kolejce przez wyzwalacz są adresowanego, a także XPath przy użyciu wyrażenia języka definicji przepływu pracy xpath.

### <a name="use-custom-code"></a>Użyj niestandardowego kodu
Usługi BizTalk Services zapewnia możliwość [uruchomienia niestandardowego kodu](https://msdn.microsoft.com/library/azure/dn232389.aspx) przekazany w własne zestawy. Ten sposób jest implementowany przez [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector.aspx) interfejsu. Na każdym z etapów mostka obejmuje dwie właściwości (na inspektora wprowadź i na inspektora zakończenia) zapewniające typ architektury .net, utworzony, który implementuje ten interfejs. Kod niestandardowy pozwala na bardziej złożonych przetwarzania danych, a także ponownie wykorzystać istniejący kod w zestawach, wykonujących wspólnej logiki biznesowej. 

Logic Apps oferuje dwa podstawowe sposoby wykonania kodu niestandardowego: usługi Azure Functions i aplikacji API Apps. Azure Functions można utworzyć i wywoływać z aplikacji logiki. Zobacz [Dodaj i wykonywania kodu niestandardowego dla usługi logic apps za pomocą usługi Azure Functions](../logic-apps/logic-apps-azure-functions.md). Aplikacje interfejsu API, część usługi Azure App Service umożliwia tworzenie własnych wyzwalacze i akcje. Dowiedz się więcej o [Tworzenie niestandardowego interfejsu API do użycia z usługą Logic Apps](../logic-apps/logic-apps-create-api-app.md). 

Jeśli masz kod niestandardowy w assmeblies, który można wywołać z usługi BizTalk Services należy Przenieś ten kod do usługi Azure Functions lub tworzenie niestandardowych interfejsów API z aplikacji interfejsu API. w zależności od jest implementacja. Na przykład jeśli masz kod, który opakowuje innej usługi, że aplikacje logiki nie ma łącznika, następnie utwórz aplikację interfejsu API i używać akcje, które zapewnia aplikacji interfejsu API w aplikacjach logiki. Jeśli masz funkcje pomocnicze lub bibliotek usługi Azure Functions wówczas prawdopodobnie najlepszego dopasowania.

### <a name="edi-processing-and-trading-partner-management"></a>EDI przetwarzania i Zarządzanie partnerami handlowymi
Usługi BizTalk Services zawiera przetwarzanie EDI i B2B z obsługą AS2 (zastosowania instrukcji 2), X12 i EDIFACT. Podobnie jak Logic Apps. W usługach BizTalk Twojej Tworzenie mostków EDI Utwórz/i oraz zarządzanie nimi handlowym partnerów umów w dedykowanym portalu zarządzania i śledzenia.

W aplikacjach logiki, ta funkcja jest uwzględniona w [pakiet integracyjny dla przedsiębiorstw](../logic-apps/logic-apps-enterprise-integration-overview.md). Składa się z konta integracji i B2B akcje EDI i B2B przetwarzania. [Konta integracji](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) służy do tworzenia i zarządzania nimi [partnerami handlowymi](../logic-apps/logic-apps-enterprise-integration-partners.md) i [umowy](../logic-apps/logic-apps-enterprise-integration-agreements.md). Po utworzeniu konta integracji można skojarzyć jedną lub więcej aplikacji logiki do konta. Jeden raz, można użyć akcji B2B uzyskiwać dostęp do handlowym partnera informacji w aplikacjach logiki. Dostępne są następujące akcje:

* Kodowanie AS2
* Dekodowanie AS2
* Kodowanie X12
* Dekodowanie X12
* Kodowanie EDIFACT
* Dekodowanie EDIFACT

W przeciwieństwie do usługi BizTalk Services te akcje są całkowicie niezależna od protokołów transportu. Dlatego podczas tworzenia aplikacji logiki, uzyskuje się większą elastyczność łączników, których używasz do wysyłania i odbierania danych. Na przykład możliwe do odbierania X12 pliki jako załączniki poczty e-mail, a następnie proces tych plików w aplikacji logiki. 

## <a name="manage-and-monitor"></a>Monitorowanie i zarządzanie nim
A także zarządzania partnerami handlowymi, portalu dedykowanego dla usługi BizTalk Services pod warunkiem możliwości śledzenia do monitorowania i rozwiązywania problemów. 

Logic Apps oferuje bardziej zaawansowane funkcje monitorowania i funkcji monitorowania w [portalu Azure](../logic-apps/logic-apps-monitor-your-logic-apps.md)oraz z [rozwiązanie B2B usługi Operations Management Suite](../logic-apps/logic-apps-monitor-b2b-message.md); w tym aplikacji mobilnej dla śledzeniu na rzeczy podczas pracy w ruchu.

## <a name="high-availability"></a>Wysoka dostępność
Aby zapewniania wysokiej dostępności (HA) w usługach BizTalk, należy użyć więcej niż jedno wystąpienie w danym regionie udostępnianie obciążenie przetwarzania. Dzięki aplikacjom logiki wysokiej dostępności w regionie jest wbudowana i pochodzi bez ponoszenia dodatkowych kosztów. Do odzyskiwania po awarii poza regionem do przetwarzania B2B usługi BizTalk proces tworzenia kopii zapasowej i przywracania jest wymagana. W aplikacjach logiki, cross-region aktywny/pasywny [możliwości DR](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md) podano; synchronizacji danych B2B co pozwala na kontach integracji w różnych regionach dla ciągłość prowadzenia działalności biznesowej.

## <a name="next"></a>Następne kroki
* [Dowiedzieć się, co to są aplikacje logiki](logic-apps-overview.md)
* [Utworzyć swoją pierwszą aplikację logiki](quickstart-create-first-logic-app-workflow.md) lub szybko rozpocząć pracę przy użyciu [wstępnie utworzonego szablonu](logic-apps-create-logic-apps-from-templates.md)  
* [Wyświetlić wszystkie dostępne łączniki](../connectors/apis-list.md), których można używać w aplikacji logiki
