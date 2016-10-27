<properties
    pageTitle="Lista dostępnych łączników i aplikacji API Apps | Usługa Microsoft Azure App Service"
    description="Dowiedz się więcej na temat łączników i aplikacji API Apps w usłudze Azure App Service"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>



# Lista łączników i aplikacji API Apps do użycia w aplikacjach Logic Apps
>[AZURE.NOTE] Ta wersja artykułu ma zastosowanie do aplikacji logiki w wersji schematu 2014-12-01-preview. Informacje o ogólnie dostępnej wersji usługi Logic Apps zawiera [Nowa lista łączników](../connectors/apis-list.md).

Dowiedz się więcej na temat wszystkich dostępnych łączników i aplikacji API Apps utworzonych przez firmę Microsoft do użycia w usłudze Logic Apps.

Informacje o cenach oraz listę opcji dostępnych w poszczególnych warstwach usług można znaleźć w temacie [Azure App Service — ceny](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Aby zacząć korzystać z usługi Logic Apps przed założeniem konta platformy Azure, przejdź do strony umożliwiającej [wypróbowanie aplikacji logiki](https://tryappservice.azure.com/?appservice=logic). Możesz od razu utworzyć krótkotrwałą wersję początkową aplikacji logiki w usłudze App Service. Bez kart kredytowych i bez zobowiązań.

## Łączniki podstawowe
W poniższej tabeli wymieniono wszystkie dostępne łączniki i aplikacje API Apps utworzone przez firmę Microsoft, które są dostępne jako łączniki podstawowe:

Nazwa | Opis
--- | ---
[Translator Bing](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Umożliwia tłumaczenie tekstu na inny język przy użyciu usługi Bing.
[HTTP](app-service-logic-connector-http.md) | Odbiornik HTTP otwiera punkt końcowy, który działa jako serwer HTTP, i nasłuchuje przychodzących żądań HTTP lub HTTPS. Akcja HTTP nie wymaga Aplikacji interfejsu API i jest obsługiwana w sposób natywny w usłudze Logic Apps.
[Slack](app-service-logic-connector-slack.md) | Umożliwia nawiązanie połączenia z usługą Slack i publikowanie wiadomości w kanałach Slack.


## Łączniki integracji dla przedsiębiorstw
W poniższej tabeli wymieniono wszystkie dostępne łączniki i aplikacje API Apps utworzone przez firmę Microsoft, które są dostępne jako łączniki integracji dla przedsiębiorstw:

Nazwa  | Opis
------------- | -------------
[BizTalk Rules](app-service-logic-use-biztalk-rules.md) | Reguły BizTalk służą do definiowania i kontrolowania logiki biznesowej w organizacji. Zasady biznesowe można aktualizować bez konieczności ponownego kompilowania lub ponownego wdrażania skojarzonych aplikacji.
[BizTalk XPath Extractor](app-service-logic-xpath-extract.md) | Wyszukuje i wyodrębnia dane z zawartości XML w oparciu o wybrany element XPath.
[Łącznik DB2](app-service-logic-connector-db2.md) | Umożliwia nawiązanie połączenia z bazą danych IBM DB2 lokalnie i na maszynie wirtualnej platformy Azure z systemem operacyjnym Windows. Może mapować operacje interfejsu API sieci Web i interfejsu API OData do poleceń języka Informix Structured Query Language. <br/><br/>Brak wyzwalaczy. Akcje obejmują wybieranie tabeli, wstawianie, aktualizowanie, usuwanie i instrukcje niestandardowe.<br/><br/>Ten łącznik obejmuje także klienta firmy Microsoft dla architektury DRDA, który służy do nawiązywania połączenia z serwerem Informix w sieci TCP/IP.
[Plik](app-service-logic-connector-file.md) | Za pomocą tego łącznika można nawiązywać połączenie z lokalnym systemem plików lub siecią oraz wykonywać inne zadania związane z plikami, takie jak przekazywanie, usuwanie, tworzenie list plików i inne.
[Informix](app-service-logic-connector-informix.md) | Umożliwia nawiązanie połączenia z bazą danych IBM Informix lokalnie i na maszynie wirtualnej platformy Azure z systemem operacyjnym Windows. Może mapować operacje interfejsu API sieci Web i interfejsu API OData do poleceń języka Informix Structured Query Language.<br/><br/>Brak wyzwalaczy. Akcje obejmują wybieranie tabeli, wstawianie, aktualizowanie, usuwanie i instrukcje niestandardowe.<br/><br/>Jeśli łącznik jest używany lokalnie, można korzystać z sieci VPN lub rozwiązania Azure ExpressRoute. Ten łącznik obejmuje także klienta firmy Microsoft dla architektury DRDA, który służy do nawiązywania połączenia z serwerem Informix w sieci TCP/IP.
[Microsoft SQL Server](app-service-logic-connector-sql.md) | Umożliwia nawiązanie połączenia z lokalnym serwerem SQL Server lub bazą danych Azure SQL Database. Pozwala tworzyć, aktualizować, pobierać i usuwać pozycje w tabeli bazy danych SQL.
MQ | Umożliwia nawiązanie połączenia z serwerem IBM WebSphere MQ w wersji 8 lokalnie i na maszynie wirtualnej platformy Azure z systemem operacyjnym Windows. Jeśli łącznik jest używany lokalnie, można korzystać z sieci VPN lub rozwiązania Azure ExpressRoute. Łącznik obejmuje także klienta firmy Microsoft dla programu MQ.<br/><br/>Brak wyzwalaczy. Brak akcji.<br/><br/>**Uwaga:** obecnie nie można używać tego łącznika z usługą Logic Apps.

## Łączniki jako wyzwalacze
Kilka łączników udostępnia wyzwalacze dla usługi Logic Apps. Istnieją dwa typy tych wyzwalaczy:

1. Wyzwalacze sondowania: te wyzwalacze sondują usługę z określoną częstotliwością w celu wyszukiwania nowych danych. Jeśli nowe dane są dostępne, uruchamiane jest nowe wystąpienie aplikacji logiki z tymi danymi jako danymi wejściowymi. Aby zapobiec wielokrotnemu używaniu tych samych danych, wyzwalacz może czyścić dane odczytane i przekazane do aplikacji logiki. Przykłady takich łączników obejmują Plik, SQL i Azure Storage.
2. Wyzwalacze wypychania: te wyzwalacze nasłuchują danych w punkcie końcowym lub zdarzeń. Następnie uruchamiają nowe wystąpienie aplikacji logiki. Przykłady takich łączników obejmują Odbiornik HTTP i Twitter.

## Łączniki jako akcje
Łączniki mogą być również używane jako akcje w aplikacjach logiki. Akcje są przydatne w przypadku wyszukiwania w aplikacji logiki danych, które następnie są używane podczas wykonywania. Można na przykład wyszukiwać dane w bazie danych SQL w celu uzyskania dodatkowych informacji o kliencie podczas przetwarzania zamówienia. Można również zapisywać, aktualizować lub usuwać dane w lokalizacji docelowej. Te czynności można wykonywać przy użyciu akcji udostępnianych przez łączniki. Akcje są mapowane na operacje w aplikacjach API Apps (zgodnie z definicją w metadanych struktury Swagger).

## Tworzenie własnych łączników i aplikacji API Apps
[Materiały referencyjne dotyczące łączników i aplikacji API Apps](http://aka.ms/appservicesconnectorreference)  
[Wyzwalacze aplikacji interfejsu API w usłudze Azure App Service](../app-service-api/app-service-api-dotnet-triggers.md)  
[Materiały referencyjne dotyczące aplikacji logiki](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## Więcej informacji na temat łączników i aplikacji API Apps
[Co to są łączniki i aplikacje API Apps w BizTalk](app-service-logic-what-are-biztalk-api-apps.md)  
[Korzystanie z Menedżera połączeń hybrydowych w usłudze Azure App Service](app-service-logic-hybrid-connection-manager.md)  
[Zarządzanie wbudowanymi aplikacjami API Apps i łącznikami oraz ich monitorowanie](app-service-logic-monitor-your-connectors.md)



<!--HONumber=Oct16_HO3-->


