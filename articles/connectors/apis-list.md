<properties
    pageTitle="Lista łączników zarządzanych przez firmę Microsoft do użycia w aplikacjach logiki Microsoft Azure | Microsoft Azure App Service"
    description="Pobierz pełną listę łączników zarządzanych przez firmę Microsoft służących do tworzenia aplikacji logiki w usłudze Azure App Service"
    services="app-service\logic"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/31/2016"
    ms.author="deonhe"/>

# Lista zarządzanych łączników

>[AZURE.NOTE] Ta wersja artykułu ma zastosowanie do aplikacji logiki w wersji schematu 2015-08-01-preview. Aby uzyskać informacje dotyczące wersji schematu 2014-12-01-preview, kliknij link [Lista łączników](../app-service-logic/app-service-logic-connectors-list.md). 

Informacje o cenach oraz listę opcji dostępnych w poszczególnych warstwach usług można znaleźć w temacie [Azure App Service — cennik](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Jeśli chcesz zacząć korzystać z usługi Azure Logic Apps przed założeniem konta platformy Azure, przejdź do strony umożliwiającej [wypróbowanie aplikacji logiki](https://tryappservice.azure.com/?appservice=logic). Możesz od razu utworzyć krótkotrwałą, początkową aplikację logiki w usłudze App Service. Bez kart kredytowych i bez zobowiązań.

Wybierz ikonę, aby dowiedzieć się, jak szybko wykorzystać te łączniki do tworzenia aplikacji wywołujących te usługi. Te łączniki mogą służyć do tworzenia aplikacji logiki, aplikacji PowerApps i przepływów.

|Łączniki||||
|-----------|-----------|-----------|-----------|
|[![API Icon][blobicon]<br/>**Obiekty blob Azure**][azureblobdoc]|[![API Icon][boxicon]<br/>**Box**][boxDoc]|[![API Icon][crmonlineicon]<br/>**CRM Online**][crmonlinedoc]|[![API Icon][dropboxicon]<br/>**Dropbox**][dropboxdoc]|
|[![API Icon][facebookicon]<br/>**Facebook**][facebookdoc]|[![API Icon][ftpicon]<br/>**FTP**][ftpdoc]|[![API Icon][githubicon]<br/>**GitHub**][githubdoc]|[![API Icon][googledriveicon]<br/>**Dysk Google**][googledrivedoc]|
|[![API Icon][mailchimpicon]<br/>**MailChimp**][mailchimpdoc]|[![API Icon][microsofttranslatoricon]<br/>**Translator**][microsofttranslatordoc]|[![API Icon][office365icon]<br/>**Office 365 —**<br/>**Outlook**][office365outlookdoc]|[![API Icon][office365icon]<br/>**Office 365 —**<br/>**użytkownicy**][office365usersdoc]|
|[![API Icon][office365icon]<br/>**Office 365 —**<br/>**wideo**][office365videodoc]|[![API Icon][onedriveicon]<br/>**OneDrive**][onedrivedoc]|[![API Icon][onedriveicon]<br/>**OneDrive<br/>dla Firm**][onedriveforbusinessdoc]|[![API Icon][outlookicon]<br/>**Outlook**][outlookdoc]|
|[![API Icon][projectonlineicon]<br/>**Project<br/>Online**][projectonlinedoc]|[![API Icon][rssicon]<br/>**RSS**][rssdoc]|[![API Icon][salesforceicon]<br/>**Salesforce**][salesforcedoc]|[![API Icon][sendgridicon]<br/>**SendGrid**][sendgriddoc]|
|[![API Icon][servicebusicon]<br/>**Service Bus**][servicebusdoc]|[![API Icon][sftpicon]<br/>**SFTP**][sftpdoc]|[![API Icon][sharepointicon]<br/>**SharePoint**<br/>**Online**][sharepointdoc]|[![API Icon][slackicon]<br/>**Slack**<br/>][slackdoc]|
|[![API Icon][smtpicon]<br/>**SMTP**][smtpdoc]|[![API Icon][sqlicon]<br/>**SQL Azure**][sqldoc]|[![API Icon][trelloicon]<br/>**Trello**][trellodoc]|[![API Icon][twilioicon]<br/>**Twilio**][twiliodoc]|
|[![API Icon][twittericon]<br/>**Twitter**][twitterdoc]|[![API Icon][wunderlisticon]<br/>**Wunderlist**][wunderlistdoc]|[![API Icon][yammericon]<br/>**Yammer**][yammerdoc] | |

> [AZURE.NOTE] Jeśli aplikacje logiki były tworzone przy użyciu schematu 2014-12-01-preview, można zauważyć, że łączniki integracji przedsiębiorstwa, na przykład łączniki dla usługi BizTalk, nie są wymienione powyżej. Wiemy, że są one ważne i pracujemy nad tym, aby udostępnić je użytkownikom już wkrótce. Na razie nie możemy podać dokładnej daty, ale zapewniamy, że ich wprowadzenie dla użytkowników jest jednym z naszych najważniejszych priorytetów. W międzyczasie możesz uzyskiwać dostęp do [interfejsów API usługi BizTalk i interfejsów API w wersji 1 z poziomu usługi Logic Apps](https://blogs.msdn.microsoft.com/logicapps/2016/02/25/accessing-v1-apis-and-biztalk-apis-from-logic-apps/). Dziękujemy za zrozumienie. Śledź aktualności.


### Łączniki mogą być wyzwalaczami
Wiele łączników udostępnia wyzwalacze, które mogą powiadamiać aplikację w przypadku wystąpienia określonych zdarzeń. Na przykład łącznik FTP ma wyzwalacz OnUpdatedFile. Można utworzyć aplikację logiki, aplikację PowerApps lub przepływ, które nasłuchują tego wyzwalacza i wykonują akcję przy każdym uruchomieniu wyzwalacza.

Istnieją dwa typy wyzwalaczy:  

* Wyzwalacze sondowania: te wyzwalacze sondują usługę z określoną częstotliwością w celu wyszukiwania nowych danych. Jeśli nowe dane są dostępne, uruchamiane jest nowe wystąpienie aplikacji z tymi danymi jako danymi wejściowymi. Aby zapobiec wielokrotnemu używaniu tych samych danych, wyzwalacz może czyścić dane odczytane i przekazane do aplikacji.
* Wyzwalacze wypychania: te wyzwalacze nasłuchują danych w punkcie końcowym lub zdarzeń. Następnie uruchamiają nowe wystąpienie aplikacji. Przykładem takiego łącznika jest łącznik Twitter.


### Łączniki mogą być akcjami
Łączniki mogą być również używane jako akcje w aplikacjach. Akcje są przydatne do wyszukiwania danych, które następnie mogą być używane podczas wykonywania aplikacji. Na przykład można wyszukiwać dane klienta w bazie danych SQL podczas przetwarzania zamówienia. Można również zapisywać, aktualizować lub usuwać dane w tabeli docelowej. Te czynności można wykonywać przy użyciu akcji udostępnianych przez łączniki. Akcje są mapowane na operacje, które są zdefiniowane w metadanych struktury Swagger.


[Co nowego](../app-service-logic/app-service-logic-schema-2015-08-01.md)  
[Tworzenie aplikacji logiki](../app-service-logic/app-service-logic-create-a-logic-app.md)  
[Rozpoczynanie pracy z aplikacjami PowerApps](../power-apps/powerapps-get-started-azure-portal.md)  
[Migrowanie istniejących aplikacji logiki do najnowszej wersji schematu](connectors-schema-migration.md) 

<!--Connectors Documentation-->
[azureblobdoc]: ./connectors-create-api-azureblobstorage.md "Umożliwia nawiązywanie połączeń z obiektami blob platformy Azure w celu zarządzania plikami w kontenerze obiektu blob."
[bingsearchDoc]: ./connectors-create-api-bingsearch.md "Umożliwia wyszukiwanie w usłudze Bing stron sieci Web, obrazów, wiadomości i wideo."
[boxDoc]: ./connectors-create-api-box.md "Umożliwia nawiązywanie połączeń z usługą Box oraz przekazywanie, pobieranie i tworzenie list plików, a także wykonywanie innych zadań dotyczących plików."
[crmonlinedoc]: ./connectors-create-api-crmonline.md "Umożliwia nawiązywanie połączeń z programem Dynamics CRM Online i wykorzystywanie danych program CRM Online na więcej sposobów."
[dropboxdoc]: ./connectors-create-api-dropbox.md "Umożliwia nawiązywanie połączeń z usługą Dropbox oraz pobieranie, usuwanie i tworzenie list plików, a także wykonywanie innych zadań dotyczących plików."
[exceldoc]: ./connectors-create-api-excel.md "Umożliwia nawiązywanie połączeń z programem Excel."
[facebookdoc]: ./connectors-create-api-facebook.md "Umożliwia nawiązywanie połączeń z serwisem Facebook, aby publikować na osi czasu, uzyskiwać dane ze strumieniowego źródła danych strony i nie tylko."
[ftpdoc]: ./connectors-create-api-ftp.md "Umożliwia nawiązywanie połączeń z serwerem FTP/FTPS i wykonywanie różnych zadań związanych z protokołem FTP, takich jak przekazywanie, pobieranie, usuwanie plików i inne."
[googledrivedoc]: ./connectors-create-api-googledrive.md "Umożliwia nawiązywanie połączeń z Dyskiem Google i interakcję z danymi."
[microsofttranslatordoc]: ./connectors-create-api-microsofttranslator.md
[office365outlookdoc]: ./connectors-create-api-office365-outlook.md "Łącznik usługi Office 365 umożliwia wysyłanie i odbieranie wiadomości e-mail oraz zarządzanie kalendarzem i kontaktami przy użyciu konta usługi Office 365."
[officeunifieddoc]: ./connectors-create-api-bingsearch.md
[office365usersdoc]: ./connectors-create-api-office365-users.md
[office365videodoc]: ./connectors-create-api-office365-video.md
[onedrivedoc]: ./connectors-create-api-onedrive.md "Umożliwia nawiązywanie połączeń z osobistą usługą Microsoft OneDrive oraz przekazywanie, pobieranie i tworzenie list plików, a także wykonywanie innych zadań."
[onedrivedoc]: ./connectors-create-api-onedriveforbusiness.md "Umożliwia nawiązywanie połączeń z firmową usługą Microsoft OneDrive oraz przekazywanie, pobieranie i tworzenie list plików, a także wykonywanie innych zadań."
[outlookdoc]: ./connectors-create-api-outlook.md "Umożliwia nawiązywanie połączeń ze skrzynką pocztową programu Outlook, uzyskiwanie dostępu do poczty e-mail oraz korzystanie z innych funkcji."
[projectonlinedoc]: ./connectors-create-api-projectonline.md "Umożliwia nawiązywanie połączeń z usługą Microsoft Project Online."
[rssdoc]: ./connectors-create-api-rss.md "Łącznik RSS umożliwia użytkownikom publikowanie i pobieranie elementów strumieniowego źródła danych. Umożliwia również użytkownikom wyzwalanie operacji po opublikowaniu nowego elementu w strumieniowym źródle danych."
[salesforcedoc]: ./connectors-create-api-salesforce.md "Umożliwia nawiązywanie połączeń z kontem aplikacji Salesforce i zarządzanie kontami, potencjalnymi klientami, możliwościami sprzedaży itd."
[sendgriddoc]: ./connectors-create-api-sendgrid.md "Umożliwia nawiązywanie połączeń z usługą Microsoft Project Online."
[servicebusdoc]: ./connectors-create-api-servicebus.md "Umożliwia wysyłanie komunikatów z tematów i kolejek usługi Service Bus oraz odbieranie komunikatów z subskrypcji i kolejek usługi Service Bus."
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "Umożliwia nawiązywanie połączeń z usługą SharePoint Online w celu zarządzania dokumentami i elementami listy."
[slackdoc]: ./connectors-create-api-slack.md "Umożliwia nawiązywanie połączeń z usługą Slack i publikowanie wiadomości w kanałach Slack."
[sftpdoc]: ./connectors-create-api-sftp.md "Umożliwia nawiązywanie połączeń z użyciem protokołu SFTP oraz przekazywanie, pobieranie i usuwanie plików, a także wykonywanie innych operacji."
[githubdoc]: ./connectors-create-api-github.md "Umożliwia nawiązywanie połączeń z witryną GitHub i śledzenie problemów."
[mailchimpdoc]: ./connectors-create-api-mailchimp.md "Umożliwia wysyłanie lepszych wiadomości e-mail."
[smtpdoc]: ./connectors-create-api-smtp.md "Umożliwia nawiązywanie połączeń z serwerem SMTP i wysyłanie wiadomości e-mail z załącznikami."
[sqldoc]: ./connectors-create-api-sqlazure.md "Umożliwia nawiązywanie połączeń z bazą danych SQL Azure. Pozwala tworzyć, aktualizować, pobierać i usuwać wpisy w tabeli bazy danych SQL."
[trellodoc]: ./connectors-create-api-trello.md "Trello to bezpłatna, elastyczna i wizualna metoda organizowania dowolnych rzeczy z dowolnymi osobami."
[twiliodoc]: ./connectors-create-api-twilio.md "Umożliwia nawiązywanie połączeń z usługą Twilio oraz wysyłanie i pobieranie wiadomości, pobieranie dostępnych numerów, zarządzanie przychodzącymi numerami telefonów i nie tylko."
[twitterdoc]: ./connectors-create-api-twitter.md "Umożliwia nawiązywanie połączeń z serwisem Twitter oraz pobieranie osi czasu, publikowanie tweetów i wykonywanie innych działań."
[wunderlistdoc]: ./connectors-create-api-wunderlist.md "Umożliwia synchronizowanie aktywności."
[yammerdoc]: ./connectors-create-api-yammer.md "Umożliwia nawiązywanie połączeń z usługą Yammer w celu publikowania wiadomości i pobierania nowych wiadomości."

<!--Icon references-->
[blobicon]: ./media/apis-list/blobicon.png
[bingsearchicon]: ./media/apis-list/bingsearchicon.png
[boxicon]: ./media/apis-list/boxicon.png
[ftpicon]: ./media/apis-list/ftpicon.png
[githubicon]: ./media/apis-list/githubicon.png
[crmonlineicon]: ./media/apis-list/dynamicscrmicon.png
[dropboxicon]: ./media/apis-list/dropboxicon.png
[excelicon]: ./media/apis-list/excelicon.png
[facebookicon]: ./media/apis-list/facebookicon.png
[googledriveicon]: ./media/apis-list/googledriveicon.png
[mailchimpicon]: ./media/apis-list/mailchimpicon.png
[microsofttranslatoricon]: ./media/apis-list/translatoricon.png
[office365icon]: ./media/apis-list/office365icon.png
[onedriveicon]: ./media/apis-list/onedriveicon.png
[onedriveforbusinessicon]: ./media/apis-list/onedriveforbusinessicon.png
[outlookicon]: ./media/apis-list/outlookicon.png
[projectonlineicon]: ./media/apis-list/projectonlineicon.png
[rssicon]: ./media/apis-list/rssicon.png
[salesforceicon]: ./media/apis-list/salesforceicon.png
[sendgridicon]: ./media/apis-list/sendgridicon.png
[servicebusicon]: ./media/apis-list/servicebusicon.png
[sftpicon]: ./media/apis-list/sftpicon.png
[sharepointicon]: ./media/apis-list/sharepointicon.png
[slackicon]: ./media/apis-list/slackicon.png
[smtpicon]: ./media/apis-list/smtpicon.png
[sqlicon]: ./media/apis-list/sqlicon.png
[trelloicon]: ./media/apis-list/trelloicon.png
[twilioicon]: ./media/apis-list/twilioicon.png
[twittericon]: ./media/apis-list/twittericon.png
[wunderlisticon]: ./media/apis-list/wunderlisticon.png
[yammericon]: ./media/apis-list/yammericon.png



<!--HONumber=Jun16_HO2-->


