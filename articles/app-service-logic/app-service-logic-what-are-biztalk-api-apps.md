<properties 
    pageTitle="Co to są łączniki i aplikacje API Apps w BizTalk" 
    description="Dowiedz się więcej na temat aplikacji API Apps, łączników i aplikacji interfejsu API BizTalk" 
    services="app-service\logic" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="04/20/2016" 
    ms.author="mandia"/>

# Co to są łączniki i aplikacje API Apps w BizTalk

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


Usługa Azure App Service została stworzona z myślą o możliwościach rozszerzania i zapewniania typowej łączności za pośrednictwem usługi API Apps. *Łącznik* to typ aplikacji interfejsu API, którego działanie koncentruje się na łączności. Łączniki — tak samo jak każda inna Aplikacja interfejsu API — są używane z poziomu usług Web Apps, Mobile Apps i Logic Apps. Łączniki ułatwiają nawiązywanie połączeń z istniejącymi usługami, wspomagają zarządzanie uwierzytelnianiem, a także oferują możliwości monitorowania, analizy i inne.

Każdy deweloper może tworzyć własne aplikacje API Apps i wdrażać je prywatnie. W przyszłości deweloperzy będą mogli udostępniać i sprzedawać własne niestandardowe aplikacje API Apps na rynku. 

![API Apps w witrynie Marketplace](./media/app-service-logic-what-are-biztalk-api-apps/Marketplace.png)

Aby deweloperzy mogli szybciej tworzyć rozwiązania za pomocą usługi Azure App Service, zespół ds. platformy Azure dodał w witrynie Marketplace wiele łączników umożliwiających realizację wielu typowych scenariuszy. Ponadto w celu rozszerzenia zasięgu usługi App Service o złożone i zaawansowane scenariusze integracji można skorzystać z możliwości warstwy Premium i usługi BizTalk.

W usłudze Azure App Service są dostępne różne „warstwy” usług. Wszystkie warstwy obejmują wszystkie łączniki i aplikacje API Apps wraz z ich pełną funkcjonalnością.  

Warstwy usług oraz ich zawartość zostały opisane w artykule [App Service  — cennik](https://azure.microsoft.com/pricing/details/app-service/). W poniższych sekcjach opisano różne kategorie łączników i aplikacji API Apps w BizTalk.


## Łączniki hybrydowe 
Łączniki hybrydowe jeszcze bardziej rozszerzają zasięg usług App Service w przedsiębiorstwie dzięki łączności z rozwiązaniami [SAP](app-service-logic-connector-sap.md), [Oracle](app-service-logic-connector-oracle.md), [DB2](app-service-logic-connector-db2.md), [Informix](app-service-logic-connector-informix.md) i WebSphere MQ. 

## Usługi EAI i EDI
Tworzenie aplikacji biznesowych o krytycznym znaczeniu wymaga czegoś więcej niż tylko łączność. Stworzone w oparciu o wiodącą w branży platformę integracyjną firmy Microsoft — BizTalk Server — aplikacje API Apps w BizTalk udostępniają zaawansowane funkcje integracji, które można z łatwością dodać do aplikacji Web Apps, Mobile Apps lub Logic Apps. Niektóre z możliwości integracji to [weryfikacja](app-service-logic-xml-validator.md), [wyodrębnianie](app-service-logic-xpath-extract.md), [przekształcanie](app-service-logic-transform-xml-documents.md), [kodery](app-service-logic-connector-jsonencoder.md), [zarządzanie partnerami handlowymi](app-service-logic-connector-tpm.md) i obsługa formatów EDI, takich jak [X12](app-service-logic-connector-x12.md), [EDIFACT](app-service-logic-connector-edifact.md) i [AS2](app-service-logic-connector-as2.md).

Dodatkowe zasoby: [Business-to-business connectors and API apps](app-service-logic-b2b-connectors.md) (Aplikacje interfejsu API i łączniki B2B)  
[Create a B2B process (Tworzenie procesu B2B)](app-service-logic-create-a-b2b-process.md)  
[Tworzenie umowy z partnerem handlowym](app-service-logic-create-a-trading-partner-agreement.md)  
[Track your B2B messages (Śledzenie wiadomości B2B)](app-service-logic-track-b2b-messages.md)  


## Reguły
Reguły biznesowe to zasady i decyzje, które kontrolują procesy biznesowe. Zazwyczaj reguły są dynamiczne i zmieniają się wraz z upływem czasu z różnych powodów, takich jak plany biznesowe, regulacje prawne i wiele innych. Informacje w artykule [BizTalk Rules in App Services](app-service-logic-use-biztalk-rules.md) (Reguły BizTalk w usługach App Service) ułatwiają oddzielenie tych zasad od kodu aplikacji, dzięki czemu proces wprowadzania zmian jest prostszy i szybszy.

## Lista łączników i Aplikacji interfejsu API
Artykuł [Connectors and API Apps List](app-service-logic-connectors-list.md) (Lista łączników i aplikacji API Apps) zawiera pełną listę łączników i Aplikacji interfejsu API z każdej kategorii, w tym łączników standardowych, BizTalk EAI, łączników Premium itd.
 



<!--HONumber=Jun16_HO2-->


