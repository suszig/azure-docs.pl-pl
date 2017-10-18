---
title: "Łączniki dla usługi Azure Logic Apps | Microsoft Docs"
description: "Wybieraj spośród wszystkich dostępnych łączników zarządzanych przez firmę Microsoft, aby budować i tworzyć aplikacje logiki"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: f1f1fd50-b7f9-4d13-824a-39678619aa7a
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: mandia; ladocs
ms.openlocfilehash: c14ac7592efabfec8668d7437463e2d8771ee072
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="connectors-list"></a>Lista łączników
> [!TIP]
> [Pełna lista od A do Z](#az) (znajdująca się w tym temacie) zawiera wszystkie dostępne łączniki, których można użyć w usłudze Logic Apps. [Szczegóły łącznika](/connectors/) to lista wszystkich wyzwalaczy i akcji zdefiniowanych w strukturze Swagger, a także wszystkich ograniczeń dla każdego łącznika.

Łączniki są integralną częścią procesu tworzenia aplikacji logiki. Za pomocą tych łączników można w naprawdę rozszerzyć aplikacje lokalne i w chmurze, aby wykonywały różne działania na tworzonych oraz już istniejących danych. Łączniki są dostępne w następujących kategoriach: 

* **Łączniki standardowe**: są automatycznie udostępniane i dołączane podczas korzystania z aplikacji logiki. Przykłady: Service Bus, Power BI, Oracle Database, OneDrive i wiele innych.

* **Łączniki konta integracji**: są udostępniane po zakupieniu konta integracji. Za pomocą tych łączników można przekształcać i sprawdzać poprawność kodu XML, przetwarzać komunikaty przesyłane między firmami zgodnie ze specyfikacjami AS2 / X12 / EDIFACT oraz kodować i dekodować pliki proste. Jeśli pracujesz z produktem BizTalk Server, te łączniki umożliwią rozszerzenie przepływów pracy BizTalk na platformę Azure.  

    Produkt BizTalk Server ma także [adapter usługi Logic Apps](https://msdn.microsoft.com/library/mt787163.aspx) umożliwiający zarówno odbieranie z aplikacji logicznej, jak i wysyłanie do niej.

* **Łączniki dla przedsiębiorstw**: zawierają MQ i SAP. Są dostępne za dodatkową opłatą. 

Aby uzyskać szczegółowe informacje dotyczące kosztów, zobacz [Logic Apps — cennik](https://azure.microsoft.com/pricing/details/logic-apps/) i [Model cen](../logic-apps/logic-apps-pricing.md). 

## <a name="popular-connectors"></a>Popularne łączniki
Za pomocą tych łączników tysiące aplikacji w milionach wykonań pomyślnie przetwarzają dane i informacje. W poniższej tabeli wymieniono najbardziej popularne i niektóre z ulubionych łączników naszych użytkowników:

| |  |  |  |
| --- | --- | --- | --- |
| [![Ikona interfejsu API][AzureBlobStorageicon]<br/>**Azure Blob<br/>Storage**][AzureBlobStoragedoc] | Za pomocą tego łącznika możesz zautomatyzować dowolne zadania konta magazynu. Obsługuje operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD). | [![Ikona interfejsu API][Azure-Functionsicon]<br/>**Azure Functions**][azure-functionsdoc] | Umożliwia tworzenie funkcji, które uruchamiają niestandardowe fragmenty języka C# lub node.js, a następnie użycie tych funkcji w aplikacjach logiki.  |
| [![Ikona interfejsu API][Dynamics-365icon]<br/>**Dynamics 365<br/>CRM Online**][Dynamics-365doc] | To jest jeden z najpopularniejszych łączników. Zawiera wyzwalacze oraz akcje ułatwiające automatyzowanie przepływów pracy z klientami i nie tylko. | [![Ikona interfejsu API][Event-Hubs-icon]<br/>**Event Hubs**][event-hubs-doc] | Publikuj zdarzenia i korzystaj z nich w centrum zdarzeń. Na przykład można pobierać dane wyjściowe z aplikacji logiki przy użyciu usługi Event Hubs, a następnie wysyłać je do dostawcy analiz w czasie rzeczywistym. |
| [![Ikona interfejsu API][FTPicon]<br/>**FTP**][FTPdoc] | Jeśli serwer FTP jest dostępny z Internetu, można zautomatyzować przepływy pracy do pracy z plikami i folderami. <br/><br/>Protokół SFTP jest również dostępny za pomocą łącznika SFTP. | [![Ikona interfejsu API][HTTPicon]<br/>**HTTP**][httpdoc] | Używaj aplikacji logiki do komunikowania się z dowolnym punktem końcowym za pośrednictwem protokołu HTTP. |
| [![Ikona interfejsu API][Office-365-Outlookicon]<br/>**Office 365<br/>Outlook**][office365-outlookdoc] | Wiele wyzwalaczy oraz mnóstwo akcji umożliwiających korzystanie z poczty e-mail i zdarzeń usługi Office 365 w ramach przepływów pracy. <br/><br/>Ten łącznik zawiera akcję *wiadomość e-mail zatwierdzenia* umożliwiającą zatwierdzanie żądań urlopów, raportów wydatków i tak dalej. <br/><br/>Obsługa użytkowników usługi Office 365 jest również dostępna za pośrednictwem łącznika Office 365 Users.| [![Ikona interfejsu API][HTTP-Requesticon]<br/>**Żądanie/odpowiedź**][HTTP-Requestdoc] | Ten łącznik udostępnia adres URL HTTPS. Kiedy aplikacja logiki odbiera żądanie tego adresu URL, uruchamia się aplikacja logiki. |
| [![Ikona interfejsu API][Salesforceicon]<br/>**Salesforce**][salesforcedoc] | Łatwo loguj się przy użyciu konta usług Salesforce, aby uzyskać dostęp do obiektów, takich jak potencjalni klienci i inne. |  [![Ikona interfejsu API][Service-Busicon]<br/>**Service Bus**][Service-Busdoc] | Najpopularniejszy łącznik dla aplikacji logiki. Zawiera wyzwalacze i akcje umożliwiające asynchroniczne przesyłanie komunikatów oraz publikowanie/subskrypcję w kolejkach, subskrypcje i tematy. |
|  [![Ikona interfejsu API][SharePointicon]<br/>**SharePoint<br/>Online**][SharePointdoc] | Jeśli korzystasz z programu SharePoint i możesz uzyskać korzyści z automatyzacji, zalecamy przyjrzenie się temu łącznikowi. Można go używać z lokalnym programem SharePoint i usługą SharePoint Online. | [![Ikona interfejsu API][SQL-Servericon]<br/>**SQL Server**][SQL-Serverdoc] | Jeden z najczęściej używanych łączników. Umożliwia nawiązywanie połączeń z lokalnym programem SQL Server i usługą Azure SQL Database. | 
| [![Ikona interfejsu API][Twittericon]<br/>**Twitter**][Twitterdoc] | Pozwala łatwo logować się za pomocą konta w serwisie Twitter i umożliwia uruchamianie przepływu pracy po opublikowaniu nowego tweeta. Następnie można zapisywać te tweety w bazie danych SQL lub na liście programu SharePoint. | | | 

## <a name="integration-account-connectors"></a>Łączniki konta integracji 

Pakiet integracyjny dla przedsiębiorstw (EIP) zawiera łączniki, które są dobrze znane w społeczności programu BizTalk Server. Po zakupie [konta integracji](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) uzyskasz także następujące łączniki: 

|  |  |  |  |
| --- | --- | --- | --- |
| [![Ikona interfejsu API][as2icon]<br/>**Dekodowanie</br> AS2**][as2decode] | [![Ikona interfejsu API][as2icon]<br/>**Kodowanie</br> AS2**][as2encode] | [![Ikona interfejsu API][x12icon]<br/>**Dekodowanie</br> EDIFACT**][EDIFACTdecode] | [![Ikona interfejsu API][x12icon]<br/>**Kodowanie</br> EDIFACT**][EDIFACTencode] |
[![Ikona interfejsu API][flatfileicon]<br/>**Kodowanie</br> pliku prostego**][flatfiledoc] | [![Ikona interfejsu API][flatfiledecodeicon]<br/>**Dekodowanie</br> pliku prostego**][flatfiledecodedoc] | [![Ikona interfejsu API][integrationaccounticon]<br/>**Konto<br/>integracji**][integrationaccountdoc] | [![Ikona interfejsu API][xmltransformicon]<br/>**Przekształcenie<br/>XML**][xmltransformdoc] |
| [![Ikona interfejsu API][x12icon]<br/>**Dekodowanie</br> X12**][x12decode] | [![Ikona interfejsu API][x12icon]<br/>**Kodowanie</br> X12**][x12encode] | [![Ikona interfejsu API][xmlvalidateicon]<br/>**Walidacja <br/>XML**][xmlvalidatedoc] | |

## <a name="enterprise-connectors"></a>Łączniki dla przedsiębiorstw

Nawiązuj połączenie z aplikacjami dla przedsiębiorstw w obrębie aplikacji logiki.

|  |  |
| --- | --- |
|[![Ikona interfejsu API][MQicon]<br/>**MQ**][mqdoc]|[![Ikona interfejsu API][SAPicon]<br/>**SAP**][sapconnector]|


## <a name="az"></a>Pełna lista od A do Z

[Szczegóły łącznika](/connectors/) to lista wszystkich wyzwalaczy i akcji zdefiniowanych w strukturze Swagger, a także wszystkich ograniczeń dla każdego łącznika.

| | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| [**1**](#1) | [**A**](#a) | [**B**](#b) | [**C**](#c) | [**D**](#d) | [**E**](#e) | [**F**](#f) | [**G**](#g) | [**H**](#h) | [**I**](#i) | [**J**](#j) | [**L**](#l) | [**M**](#m) |
| [**N**](#n) | [**O**](#o) | [**P**](#p) | [**R**](#r) | [**S**](#s) | [**T**](#t) | [**U**](#u) | [**V**](#v) | [**W**](#w) | [**X**](#x) | [**Y**](#y) | [**Z**](#z) | | 

| | |
|---|---|
|<a name="1"></a>10to8 — planowanie terminów<br/><br/><a name="a"></a>Act!<br/>Adobe Creative Cloud<br/>appFigures<br/>[AS2][as2doc]<br/>Asana<br/>Azure Active Directory (AD)<br/>Usługa Azure API Management<br/>Azure App Services<br/>Azure Application<br/>Azure Automation<br/>[Azure Blob Storage][azureblobstoragedoc]<br/>Azure Data Lake<br/>Azure DocumentDB (Cosmos DB)<br/>[Azure Functions][azure-functionsdoc]<br/>[Azure Logic Apps][nested-logic-appdoc]<br/>AzureML<br/>Azure Queues<br/>Azure Resource Manager<br/>[Azure SQL Database][sql-serverdoc]<br/><br/><a name="b"></a>Basecamp 2<br/>Basecamp 3<br/>Batch<br/>Benchmark Email<br/>Wyszukiwanie Bing<br/>Bitbucket<br/>Bitly<br/>Platforma BizTalk Server<br/>Blogger<br/>Box<br/>Buffer<br/><br/><a name="c"></a>Calendly<br/>Campfire<br/>Capsule CRM<br/>Chatter<br/>Cognito Forms<br/>Interfejs API przetwarzania obrazów usług Cognitive Services<br/>Interfejs API rozpoznawania twarzy usług Cognitive Services<br/>Cognitive Services LUIS<br/>Analiza tekstu usług Cognitive Services<br/>Common Data Service<br/>Konwersja zawartości<br/>Control-Terminate<br/>[Niestandardowe interfejsy API / Web Apps][api/web-appdoc]<br/><br/><a name="d"></a>Operacje na danych<br/>[DB2][db2doc]<br/>Disqus<br/>DocuSign<br/>Do Until<br/>Dropbox<br/>[Dynamics 365 CRM Online][Dynamics-365doc]<br/>Dynamics 365 for Financials<br/>Dynamics 365 for Operations<br/>Dynamics NAV<br/><br/><a name="e"></a>Easy Redmine<br/>EDIFACT<br/>[Event Hubs][event-hubs-doc]<br/>Eventbrite<br/><br/><a name="f"></a>Facebook<br/>[File System][filesystemdoc]<br/>[Flat File][flatfiledoc]<br/>FreshBooks<br/>Freshdesk<br/>Freshservice<br/>[FTP][ftpdoc]<br/><br/><a name="g"></a>GitHub<br/>Gmail<br/>Kalendarz Google<br/>Kontakty Google<br/>Dysk Google<br/>Arkusze Google<br/>Zadania Google<br/>GoToMeeting<br/>GoToTraining<br/>GoToWebinar<br/><br/><a name="h"></a>Harvest<br/>HelloSign<br/>HipChat<br/>[HTTP][httpdoc]<br/>[HTTP + Swagger][http-swaggerdoc]<br/>[HTTP Webhook][webhookdoc]<br/><br/><a name="i"></a>[Informix][informixdoc]<br/>Infusionsoft<br/>Inoreader<br/>Insightly<br/>Instagram<br/>Instapaper<br/>Konto integracji<br/>Intercom | <a name="j"></a>JotForm<br/>JIRA<br/><br/><a name="l"></a>LeanKit<br/>LiveChat<br/><br/><a name="m"></a>MailChimp<br/>Mandrill<br/>Medium<br/>Microsoft Forms<br/>Microsoft Teams<br/>Microsoft Translator<br/>[MQ][mqdoc]<br/>MSN Weather<br/>Muhimbi PDF<br/>MySQL<br/><br/><a name="n"></a>Nexmo<br/><br/><a name="o"></a>[Office 365 Outlook][office365-outlookdoc]<br/>Office 365 Users<br/>Office 365 Video<br/>OneDrive<br/>OneDrive dla Firm<br/>OneNote (dla firm)<br/>[Baza danych Oracle][oracle-db-doc]<br/>Outlook Customer Manager<br/>Outlook Tasks<br/>Outlook.com<br/><br/><a name="p"></a>PagerDuty<br/>Parserr<br/>Paylocity<br/>Pinterest<br/>Pipedrive<br/>Pivotal Tracker<br/>Planner<br/>PostgreSQL<br/>Power BI<br/>Project Online<br/><br/><a name="r"></a>Redmine<br/>[Request / Response][http-requestdoc]<br/>RSS<br/><br/><a name="s"></a>[Salesforce][salesforcedoc]<br/>[SAP Application Server][sapconnector]<br/>[SAP Message Server][sapconnector]<br/>[Schedule][recurrencedoc]<br/>Zakres<br/>SendGrid<br/>Wysyłanie komunikatów do partii<br/>[Service Bus][service-busdoc]<br/>SFTP<br/>[SharePoint Online][sharepointdoc]<br/>[SharePoint Server][sharepointserver]<br/>Slack<br/>Smartsheet<br/>SMTP<br/>SparkPost<br/>[SQL Server][sql-serverdoc]<br/>Stripe<br/>SurveyMonkey<br/>Switch Case<br/><br/><a name="t"></a>Teamwork Projects<br/>Teradata<br/>Todoist<br/>Toodledo<br/>[Przekształcanie kodu XML][xmltransformdoc]<br/>Trello<br/>Twilio<br/>[Twitter][twitterdoc]<br/>Typeform<br/><br/><a name="u"></a>UserVoice<br/><br/><a name="v"></a>Variables<br/>Vimeo<br/>Visual Studio Team Services<br/><br/><a name="w"></a>WebMerge<br/>WordPress<br/>Wunderlist<br/><br/><a name="x"></a>[X12][x12doc]<br/>[Walidacja kodu XML][xmlvalidatedoc]<br/><br/><a name="y"></a>Yammer<br/>YouTube<br/><br/><a name="z"></a>Zendesk |

> [!TIP]
> Aby zacząć korzystać z usługi Azure Logic Apps przed założeniem konta platformy Azure, przejdź do strony umożliwiającej [wypróbowanie usługi Logic Apps](https://tryappservice.azure.com/?appservice=logic). Możesz od razu utworzyć krótkotrwałą wersję początkową aplikacji logiki. Bez kart kredytowych i bez zobowiązań.

## <a name="connectors-as-triggers-and-actions"></a>Łączniki jako wyzwalacze i akcje

**Wyzwalacz** rozpoczyna lub uruchamia wystąpienie aplikacji logiki. Niektóre łączniki udostępniają wyzwalacze, które powiadamiają aplikację w przypadku wystąpienia określonych zdarzeń. Na przykład łącznik FTP ma wyzwalacz `OnUpdatedFile` uruchamiający aplikację logiki po zaktualizowaniu pliku. 

Aplikacje logiki mają następujące typy wyzwalaczy:  

* *Wyzwalacze sondowania*: te wyzwalacze sondują usługę z określoną częstotliwością w celu wyszukiwania nowych danych. 

    Jeśli nowe dane są dostępne, uruchamiane jest nowe wystąpienie aplikacji logiki z tymi danymi jako danymi wejściowymi. 

* *Wyzwalacze wypychania*: te wyzwalacze nasłuchują danych w punkcie końcowym lub zdarzeń, a następnie uruchamiają nowe wystąpienie aplikacji logiki.

* *Wyzwalacz cykliczny*: ten wyzwalacz tworzy wystąpienie aplikacji logiki zgodnie z ustalonym harmonogramem.

Łączniki udostępniają również **akcje**, których można używać w przepływie pracy. Na przykład aplikacja logiki może wyszukiwać dane, a następnie używać ich w późniejszym czasie. Można na przykład wyszukać dane klienta w bazie danych programu SQL, a następnie użyć tych danych w celu utworzenia przepływu pracy. 

> [!TIP]
> Aby uzyskać więcej szczegółów na temat wyzwalaczy i akcji, zobacz [Przegląd łączników](connectors-overview.md). 


## <a name="message-manipulation-actions"></a>Akcje manipulowania komunikatami

Aplikacje logiki zawierają wbudowane akcje, które mogą zmieniać dane ładunku i umożliwiają manipulowanie nimi. Wbudowany łącznik **Operacje na danych** zawiera następujące akcje: 

| | |
|---|---|
| **Redaguj** | Buduj lub generuj wartości bądź obiekty do późniejszego użycia lub do użycia podczas budowania przepływu pracy. Można na przykład utworzyć obiekt JSON z wartościami z wielu kroków lub obliczyć stałą do późniejszego odwołania w czasie działania aplikacji logiki. |
| **Utwórz tabelę CSV**<br/>**Utwórz tabelę HTML** | Przekształć tablicowy zestaw wyników w tabelę CSV lub HTML. Na przykład dodaj akcję CRM „Wyświetl rekordy” i filtr dla rekordów dodanych dzisiaj. Następnie wyślij wyniki w postaci tabeli HTML w wiadomości e-mail. |
| **Filtruj tablicę** (zapytanie) | Filtruj zestaw wyników, aby pozostały w nim tylko interesujące Cię wpisy. Na przykład wyszukaj wszystkie tweety zawierające tag `#Azure`, a następnie „przefiltruj” zwrócone tweety, aby pozostały tylko te, które spełniają warunek `Tweeted_by_followers > 50`. |
| **Dołącz** | Dołącz tablicę według ogranicznika. Na przykład operacja wykrywania kluczowych fraz zwraca tablicę kluczowych fraz. Można je „dołączyć” za pomocą znaku `,` lub innego ogranicznika. Na przykład zamiast fraz `["Some", "Phrase"]` zostanie zwrócona fraza `"Some, Phrase"`. |
| **Przeanalizuj dane JSON** | Przeanalizuj wartości z obiektu JSON i uzyskaj do nich dostęp w projektancie. Na przykład jeśli usługa Azure Function zwraca ładunek JSON, można go przeanalizować, aby móc później uzyskać dostęp do właściwości JSON w innym kroku. Ta akcja sprawdza również, czy dane JSON są zgodne z określonym schematem w czasie wykonywania. | 
| **Wybierz** | Wybierz niektóre właściwości tablicy do dalszego przetwarzania. Jeśli korzystasz z akcji „Wyświetl rekordy” względem bazy danych SQL i w wyniku działania tej akcji zostanie zwróconych 15 kolumn, wybierz tylko niektóre z nich do dalszego przetwarzania. Dane wyjściowe są prezentowane w postaci tablicy, która zawiera tylko wybrane właściwości. |

## <a name="custom-connectors-and-azure-certification"></a>Niestandardowe łączniki i certyfikaty platformy Azure 

Aby wywoływać interfejsy API, na których jest uruchomiony niestandardowy kod lub które nie są dostępne jako łączniki, można rozszerzyć platformę usługi Logic Apps, [tworząc aplikacje interfejsu API oparte na protokole REST jako łączniki niestandardowe](../logic-apps/logic-apps-create-api-app.md). 

Jeśli chcesz, aby Twoje niestandardowe aplikacje interfejsu API były publiczne i dostępne do użytku na platformie Azure, prześlij swoje nominacje do [programu certyfikatów platformy Microsoft Azure](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/).

## <a name="get-help"></a>Uzyskiwanie pomocy

Aby zadawać pytania, odpowiadać na nie i patrzeć, co robią inni użytkownicy usługi Azure Logic Apps, przejdź do [forum usługi Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Aby pomóc w ulepszaniu usługi Azure Logic Apps, przesyłaj pomysły lub głosuj na nie w [witrynie opinii użytkowników aplikacji logiki](http://aka.ms/logicapps-wish).

Czy pominęliśmy jakiś temat dotyczący łączników albo jakieś informacje, które uważasz za istotne? Jeśli tak, pomóż nam, uzupełniając istniejące tematy lub pisząc swój własny temat. Nasza dokumentacja jest typu open source i jest hostowana w usłudze GitHub. Zacznij, przechodząc do naszego [repozytorium GitHub](https://github.com/Microsoft/azure-docs). 

## <a name="next-steps"></a>Następne kroki
* [Tworzenie pierwszej aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md)
* [Tworzenie niestandardowych interfejsów API dla aplikacji logiki](../logic-apps/logic-apps-create-api-app.md)
* [Monitorowanie aplikacji logiki](../logic-apps/logic-apps-monitor-your-logic-apps.md)

<!--Connectors Documentation-->

[api/web-appdoc]: ../logic-apps/logic-apps-custom-hosted-api.md "Integracja aplikacji logiki z usługą App Service API Apps"
[azureblobstoragedoc]: ./connectors-create-api-azureblobstorage.md "Zarządzanie plikami w kontenerze obiektów blob za pomocą łącznika usługi Azure Blob Storage"
[azure-functionsdoc]: ../logic-apps/logic-apps-azure-functions.md "Integracja aplikacji logiki z usługą Azure Functions"
[db2doc]: ./connectors-create-api-db2.md "Łączenie z bazą danych IBM DB2 w chmurze lub lokalnie. Aktualizowanie wiersza, pobieranie tabeli oraz inne funkcje"
[Dynamics-365doc]: ./connectors-create-api-crmonline.md "Łączenie z usługą Dynamics CRM Online w celu korzystania z danych usługi CRM Online"
[event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Łączenie z usługą Azure Event Hubs. Odbieranie i wysyłanie zdarzeń między aplikacjami logiki i usługą Event Hubs"
[filesystemdoc]: ../logic-apps/logic-apps-using-file-connector.md "Łączenie z lokalnym systemem plików"
[ftpdoc]: ./connectors-create-api-ftp.md "Łączenie z serwerem FTP/FTPS i wykonywanie zadań związanych z protokołem FTP, takich jak np. przekazywanie, pobieranie i usuwanie plików"
[httpdoc]: ./connectors-native-http.md "Nawiązywanie połączeń HTTP za pomocą łącznika HTTP"
[http-requestdoc]: ./connectors-native-reqres.md "Dodawanie akcji dla żądań i odpowiedzi HTTP w aplikacjach logiki"
[http-swaggerdoc]: ./connectors-native-http-swagger.md "Nawiązywanie połączeń HTTP za pomocą łącznika protokołu HTTP + Swagger"
[informixdoc]: ./connectors-create-api-informix.md "Łączenie z usługą Informix w chmurze lub lokalnie. Odczytywanie wiersza, wyświetlanie listy tabel i inne funkcje"
[nested-logic-appdoc]: ../logic-apps/logic-apps-http-endpoint.md "Integracja aplikacji logiki z zagnieżdżonymi przepływami pracy"
[office365-outlookdoc]: ./connectors-create-api-office365-outlook.md "Łączenie z kontem usługi Office 365. Wysyłanie i odbieranie wiadomości e-mail, zarządzanie kalendarzem i kontaktami oraz inne funkcje"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Łączenie z bazą danych Oracle w celu m.in. dodawania, wstawiania i usuwania wierszy"
[mqdoc]: ./connectors-create-api-mq.md "Łączenie z programem MQ zainstalowanym lokalnie lub na platformie Azure i wysyłanie oraz odbieranie komunikatów"
[recurrencedoc]:  ./connectors-native-recurrence.md "Wyzwalanie akcji cyklicznych dla aplikacji logiki"
[salesforcedoc]: ./connectors-create-api-salesforce.md "Łączenie z kontem usługi Salesforce. Zarządzanie kontami, potencjalnymi klientami i możliwościami sprzedaży oraz inne funkcje"
[sapconnector]: ../logic-apps/logic-apps-using-sap-connector.md "Łączenie z lokalnym systemem SAP"
[service-busdoc]: ./connectors-create-api-servicebus.md "Wysyłanie komunikatów z tematów i kolejek usługi Service Bus oraz odbieranie komunikatów z subskrypcji i kolejek usługi Service Bus"
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "Łączenie z usługą SharePoint Online. Zarządzanie dokumentami i elementami list oraz inne funkcje"
[sharepointserver]: ./connectors-create-api-sharepointserver.md "Łączenie z programem SharePoint na serwerze lokalnym. Zarządzanie dokumentami i elementami list oraz inne funkcje"
[sql-serverdoc]: ./connectors-create-api-sqlazure.md "Łączenie z usługą Azure SQL Database lub programem SQL Server. Tworzenie, aktualizowanie, pobieranie i usuwanie wpisów w tabeli bazy danych SQL."
[twitterdoc]: ./connectors-create-api-twitter.md "Łączenie z serwisem Twitter. Pobieranie osi czasu, publikowanie tweetów i inne funkcje"
[webhookdoc]: ./connectors-native-webhook.md "Dodawanie akcji elementu Webhook i wyzwalaczy do aplikacji logiki"

[as2doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Więcej informacji na temat integracji dla przedsiębiorstw — AS2."
[x12doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Więcej informacji na temat integracji dla przedsiębiorstw — X12."
[flatfiledoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Więcej informacji na temat integracji dla przedsiębiorstw — plik prosty."
[flatfiledecodedoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Więcej informacji na temat integracji dla przedsiębiorstw — plik prosty."
[xmlvalidatedoc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Więcej informacji na temat integracji dla przedsiębiorstw — walidacja XML."
[xmltransformdoc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Więcej informacji na temat integracji dla przedsiębiorstw — transformacje."
[as2decode]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Więcej informacji na temat integracji dla przedsiębiorstw — dekodowanie komunikatów AS2."
[as2encode]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Więcej informacji na temat integracji dla przedsiębiorstw — kodowanie komunikatów AS2."
[X12decode]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Więcej informacji na temat integracji dla przedsiębiorstw — dekodowanie komunikatów X12."
[X12encode]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Więcej informacji na temat integracji dla przedsiębiorstw — kodowanie komunikatów X12."
[EDIFACTdecode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Więcej informacji na temat integracji dla przedsiębiorstw — dekodowanie komunikatów EDIFACT."
[EDIFACTencode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Więcej informacji na temat integracji dla przedsiębiorstw — kodowanie komunikatów EDIFACT."
[integrationaccountdoc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Wyszukiwanie schematów, map partnerów i innych elementów na koncie integracji"


[boxDoc]: ./connectors-create-api-box.md "Łączenie z usługą Box. Przekazywanie, pobieranie, usuwanie i wyświetlanie listy plików oraz inne funkcje"
[delaydoc]: ./connectors-native-delay.md "Wykonywanie akcji z opóźnieniem"
[dropboxdoc]: ./connectors-create-api-dropbox.md "Łączenie z usługą Dropbox. Przekazywanie, pobieranie, usuwanie i wyświetlanie listy plików oraz inne funkcje"
[facebookdoc]: ./connectors-create-api-facebook.md "Łączenie z serwisem Facebook. Publikowanie na osi czasu, uzyskiwanie kanału informacyjnego strony i inne funkcje"
[githubdoc]: ./connectors-create-api-github.md "Łączenie z witryną GitHub i śledzenie problemów"
[google-drivedoc]: ./connectors-create-api-googledrive.md "Łączenie z Dyskiem Google i korzystanie z danych"
[google-sheetsdoc]: ./connectors-create-api-googlesheet.md "Łączenie z Arkuszami Google i modyfikowanie danych"
[google-tasksdoc]: ./connectors-create-api-googletasks.md "Łączenie z Zadaniami Google i zarządzanie zadaniami"
[google-calendardoc]: ./connectors-create-api-googlecalendar.md "Nawiązuje połączenie z usługą Kalendarz Google i może zarządzać kalendarzem."
[http-responsedoc]: ./connectors-native-reqres.md "Dodawanie akcji dla żądań i odpowiedzi HTTP w aplikacjach logiki"
[instagramdoc]: ./connectors-create-api-instagram.md "Łączenie z usługą Instagram. Wyzwalanie zdarzeń lub reagowanie na nie"
[mailchimpdoc]: ./connectors-create-api-mailchimp.md "Łączenie z kontem MailChimp. Automatyzowanie obsługi wiadomości e-mail i zarządzanie nimi"
[mandrilldoc]: ./connectors-create-api-mandrill.md "Łączenie z usługą Mandrill w celu komunikowania się"
[microsoft-translatordoc]: ./connectors-create-api-microsofttranslator.md "Łączenie z usługą Microsoft Translator. Tłumaczenie tekstu, wykrywanie języków i inne funkcje" 
[office365-usersdoc]: ./connectors-create-api-office365-users.md 
[office365-videodoc]: ./connectors-create-api-office365-video.md "Pobieranie informacji o filmach wideo, list i kanałów wideo oraz adresów URL odtwarzania dla filmów wideo usługi Office 365"
[onedrivedoc]: ./connectors-create-api-onedrive.md "Łączenie z osobistą usługą Microsoft OneDrive. Przekazywanie, usuwanie i wyświetlanie listy plików oraz inne funkcje"
[onedrive-for-businessdoc]: ./connectors-create-api-onedriveforbusiness.md "Łączenie z firmową usługą Microsoft OneDrive. Przekazywanie, usuwanie i wyświetlanie listy plików oraz inne funkcje"
[outlook.comdoc]: ./connectors-create-api-outlook.md "Łączenie ze skrzynką pocztową programu Outlook. Zarządzanie pocztą e-mail, kalendarzami i kontaktami oraz inne funkcje"
[project-onlinedoc]: ./connectors-create-api-projectonline.md "Łączenie z usługą Microsoft Project Online. Zarządzanie projektami, zadaniami i zasobami oraz inne funkcje"
[querydoc]: ./connectors-native-query.md "Wybieranie i filtrowanie tablic za pomocą akcji zapytania"
[rssdoc]: ./connectors-create-api-rss.md "Publikowanie i pobieranie elementów kanału informacyjnego, wyzwalanie operacji po opublikowaniu nowego elementu w kanale informacyjnym RSS."
[sendgriddoc]: ./connectors-create-api-sendgrid.md "Łączenie z usługą SendGrid. Wysyłanie wiadomości e-mail i zarządzanie listami adresatów"
[sftpdoc]: ./connectors-create-api-sftp.md "Łączenie z kontem SFTP. Przekazywanie, pobieranie i usuwanie plików oraz inne funkcje"
[slackdoc]: ./connectors-create-api-slack.md "Łączenie z usługą Slack i publikowanie wiadomości w kanałach Slack"
[smtpdoc]: ./connectors-create-api-smtp.md "Łączenie z serwerem SMTP i wysyłanie wiadomości e-mail z załącznikami"
[sparkpostdoc]: ./connectors-create-api-sparkpost.md "Łączenie z usługą SparkPost w celu komunikowania się"
[trellodoc]: ./connectors-create-api-trello.md "Łączenie z usługą Trello. Zarządzanie projektami i organizowanie dowolnych rzeczy z dowolnymi osobami"
[twiliodoc]: ./connectors-create-api-twilio.md "Łączenie z usługą Twilio. Wysyłanie i pobieranie wiadomości, pobieranie dostępnych numerów, zarządzanie przychodzącymi numerami telefonów i inne funkcje"
[wunderlistdoc]: ./connectors-create-api-wunderlist.md "Łączenie z usługą Wunderlist. Zarządzanie zadaniami i listami zadań do wykonania, synchronizowanie danych oraz inne funkcje"
[yammerdoc]: ./connectors-create-api-yammer.md "Łączenie z usługą Yammer. Publikowanie wiadomości, pobieranie nowych wiadomości i inne funkcje"
[youtubedoc]: ./connectors-create-api-youtube.md "Łączenie z serwisem YouTube. Zarządzanie filmami wideo i kanałami"


<!--Icon references-->
[appFiguresicon]: ./media/apis-list/appfigures.png
[Asanaicon]: ./media/apis-list/asana.png
[Azure-Automation-icon]: ./media/apis-list/azure-automation.png
[AzureBlobStorageicon]: ./media/apis-list/azureblob.png
[Azure-Data-Lake-icon]: ./media/apis-list/azure-data-lake.png
[Azure-DocumentDBicon]: ./media/apis-list/azure-documentdb.png
[Azure-MLicon]: ./media/apis-list/azureml.png
[Azure-Resource-Manager-icon]: ./media/apis-list/azure-resource-manager.png
[Azure-Queues-icon]: ./media/apis-list/azure-queues.png
[Basecamp-3icon]: ./media/apis-list/basecamp.png
[Bitbucket-icon]: ./media/apis-list/bitbucket.png
[Bitlyicon]: ./media/apis-list/bitly.png
[BizTalk-Servericon]: ./media/apis-list/biztalk.png
[Bloggericon]: ./media/apis-list/blogger.png
[Boxicon]: ./media/apis-list/box.png
[Campfireicon]: ./media/apis-list/campfire.png
[Cognitive-Services-Text-Analyticsicon]: ./media/apis-list/cognitiveservicestextanalytics.png
[DB2icon]: ./media/apis-list/db2.png
[Dropboxicon]: ./media/apis-list/dropbox.png
[Dynamics-365icon]: ./media/apis-list/dynamicscrmonline.png
[Dynamics-365-for-Financialsicon]: ./media/apis-list/madeira.png
[Dynamics-365-for-Operationsicon]: ./media/apis-list/dynamicsax.png
[Easy-Redmineicon]: ./media/apis-list/easyredmine.png
[Event-Hubs-icon]: ./media/apis-list/eventhubs.png
[Facebookicon]: ./media/apis-list/facebook.png
[FTPicon]: ./media/apis-list/ftp.png
[GitHubicon]: ./media/apis-list/github.png
[Google-Calendaricon]: ./media/apis-list/googlecalendar.png
[Google-Driveicon]: ./media/apis-list/googledrive.png
[Google-Sheetsicon]: ./media/apis-list/googlesheet.png
[Google-Tasksicon]: ./media/apis-list/googletasks.png
[HideKeyicon]: ./media/apis-list/hidekey.png
[HipChaticon]: ./media/apis-list/hipchat.png
[Informixicon]: ./media/apis-list/informix.png
[Insightlyicon]: ./media/apis-list/insightly.png
[Instagramicon]: ./media/apis-list/instagram.png
[Instapapericon]: ./media/apis-list/instapaper.png
[JIRAicon]: ./media/apis-list/jira.png
[MailChimpicon]: ./media/apis-list/mailchimp.png
[Mandrillicon]: ./media/apis-list/mandrill.png
[Microsoft-Translatoricon]: ./media/apis-list/microsofttranslator.png
[MQicon]: ./media/apis-list/mq.png
[Office-365-Outlookicon]: ./media/apis-list/office365.png
[Office-365-Usersicon]: ./media/apis-list/office365users.png
[Office-365-Videoicon]: ./media/apis-list/office365video.png
[OneDriveicon]: ./media/apis-list/onedrive.png
[OneDrive-for-Businessicon]: ./media/apis-list/onedriveforbusiness.png
[Oracle-DB-icon]: ./media/apis-list/oracle-db.png
[Outlook.comicon]: ./media/apis-list/outlook.png
[PagerDutyicon]: ./media/apis-list/pagerduty.png
[Pinteresticon]: ./media/apis-list/pinterest.png
[Project-Onlineicon]: ./media/apis-list/projectonline.png
[Redmineicon]: ./media/apis-list/redmine.png
[RSSicon]: ./media/apis-list/rss.png
[Common-Data-Serviceicon]: ./media/apis-list/runtimeservice.png
[Salesforceicon]: ./media/apis-list/salesforce.png
[SAPicon]: ./media/apis-list/sap.png
[SendGridicon]: ./media/apis-list/sendgrid.png
[Service-Busicon]: ./media/apis-list/servicebus.png
[SFTPicon]: ./media/apis-list/sftp.png
[SharePointicon]: ./media/apis-list/sharepointonline.png
[Slackicon]: ./media/apis-list/slack.png
[Smartsheeticon]: ./media/apis-list/smartsheet.png
[SMTPicon]: ./media/apis-list/smtp.png
[SparkPosticon]: ./media/apis-list/sparkpost.png
[SQL-Servericon]: ./media/apis-list/sql.png
[Todoisticon]: ./media/apis-list/todoist.png
[Trelloicon]: ./media/apis-list/trello.png
[Twilioicon]: ./media/apis-list/twilio.png
[Twittericon]: ./media/apis-list/twitter.png
[Vimeoicon]: ./media/apis-list/vimeo.png
[Visual-Studio-Team-Servicesicon]: ./media/apis-list/visualstudioteamservices.png
[WordPressicon]: ./media/apis-list/wordpress.png
[Wunderlisticon]: ./media/apis-list/wunderlist.png
[Yammericon]: ./media/apis-list/yammer.png
[YouTubeicon]: ./media/apis-list/youtube.png

<!-- Primitive Icons -->
[API/Web-Appicon]: ./media/apis-list/api.png
[Azure-Functionsicon]: ./media/apis-list/function.png
[Delayicon]: ./media/apis-list/delay.png
[FileSystemIcon]: ./media/apis-list/filesystem.png
[HTTPicon]: ./media/apis-list/http.png
[HTTP-Requesticon]: ./media/apis-list/request.png
[HTTP-Responseicon]: ./media/apis-list/response.png
[HTTP-Swaggericon]: ./media/apis-list/http_swagger.png
[Nested-Logic-Appicon]: ./media/apis-list/workflow.png
[Recurrenceicon]: ./media/apis-list/recurrence.png
[Queryicon]: ./media/apis-list/query.png
[Webhookicon]: ./media/apis-list/webhook.png

<!-- EIP Icons -->
[as2icon]: ./media/apis-list/as2.png
[x12icon]: ./media/apis-list/x12new.png
[flatfileicon]: ./media/apis-list/flatfileencoding.png
[flatfiledecodeicon]: ./media/apis-list/flatfiledecoding.png
[xmlvalidateicon]: ./media/apis-list/xmlvalidation.png
[xmltransformicon]: ./media/apis-list/xsltransform.png
[integrationaccounticon]: ./media/apis-list/integrationaccount.png
