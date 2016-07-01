<properties 
    pageTitle="Co to jest Logic Apps?" 
    description="Dowiedz się więcej o aplikacjach Logic Apps w usłudze App Service" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="app-service\logic" 
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="04/07/2016"
    ms.author="klam"/>

#Co to jest Logic Apps?

| Krótki przewodnik |
| --------------- |
| [Język definicji usługi Logic Apps](https://msdn.microsoft.com/library/azure/mt643789.aspx) |
| [Dokumentacja dotycząca łączników usługi Logic Apps](../connectors/apis-list.md) |
| [Forum usługi Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) |

Usługa Azure App Service to w pełni zarządzane rozwiązanie typu PaaS (platforma jako usługa) dla deweloperów, które ułatwia tworzenie aplikacji sieci Web, mobilnych i umożliwiających integrację. Usługa Logic Apps to część tego pakietu i umożliwia wszystkim użytkownikom technicznym i deweloperom automatyzację przepływu pracy i wykonywania procesów biznesowych dzięki łatwemu w użyciu projektantowi wizualnemu.

Co najważniejsze, usługę Logic Apps można łączyć z wbudowanymi [zarządzanymi interfejsami API][managedapis], co pomaga w realizowaniu nawet trudnych scenariuszy integracji: 

![Projektant aplikacji przepływu](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Jak wspomniano, dzięki aplikacjom logiki można automatyzować procesy biznesowe. Oto kilka przykładów:  
 
* Automatyczne replikowanie nowych rekordów w bazie danych SQL i wysyłanie poczty do recepcji.   
* Automatyczne wyszukiwanie negatywnych tweetów i wysyłanie ich do kanału Slack.

Wszystkie tego typu scenariusze można skonfigurować przy użyciu projektanta wizualnego i bez konieczności napisania jakiegokolwiek wiersza kodu. Rozpocznij [teraz tworzenie aplikacji logiki][create].

## Dlaczego warto korzystać z usługi Logic Apps?

Usługa Logic Apps umożliwia deweloperom projektowanie przepływów pracy uruchamianych przy użyciu wyzwalaczy i wykonujących serię kroków. Każdy krok wywołuje interfejs API, zapewniając bezpieczną obsługę uwierzytelniania i zastosowanie najlepszych rozwiązań, takich jak punkty kontrolne i wykonywanie trwałe.

Jeśli chcesz zautomatyzować dowolny proces biznesowy (np. wyszukiwanie negatywnych tweetów i publikowanie ich w wewnętrznym kanale Slack lub replikowanie pojawiających się nowych rekordów klientów z bazy SQL do systemu CRM), usługa Logic Apps ułatwia integrowanie różnych źródeł danych z chmury do środowiska lokalnego. Zapoznaj się z naszymi inspirującymi [zarządzanymi interfejsami API][managedapis] i [rozpocznij][create] teraz, aby sprawdzić, jakie masz możliwości. 

Ponadto nasze [zarządzane interfejsy API BizTalk][biztalk] umożliwiają skalowanie do dojrzałych scenariuszy integracji dzięki możliwościom [aparatu reguł][rules], [zarządzania partnerami handlowymi][tpm] i wielu innym.

- **Proste w użyciu narzędzia do projektowania** — projekt aplikacji Logic Apps można w całości przygotować w przeglądarce. Rozpocznij od wyzwalacza — od prostego harmonogramu powiązanego z pojawianiem się nowych tweetów dotyczących o firmy. Następnie zorganizuj dowolną liczbę akcji przy użyciu bogatej galerii łączników.

- **Proste tworzenie usługi typu SaaS** — nawet zadania tworzenia proste do opisania są trudne do zaimplementowania w kodzie. Aplikacje Logic Apps ułatwiają łączenie różnych systemów. Chcesz w oprogramowaniu CRM utworzyć zadanie oparte na aktywności na kontach serwisów Facebook lub Twitter? Chcesz połączyć rozwiązanie marketingowe w chmurze z lokalnym systemem rozliczeniowym? Aplikacje logiki to najszybsze i najbardziej niezawodne rozwiązania umożliwiające zaspokojenie tych potrzeb.

- **Szybkie rozpoczynanie pracy z szablonami** — aby Ci pomóc, przygotowaliśmy [galerię szablonów][templates] umożliwiającą szybkie tworzenie niektórych typowych rozwiązań. Od zaawansowanych rozwiązań BizTalk do prostej łączności z usługami typu SaaS (kilka rozwiązań jest po prostu „do zabawy”) — galeria to najszybszy sposób poznania możliwości usługi Logic Apps.

- **Możliwość rozszerzania** — nie widzisz potrzebnego Ci interfejsu API? Usługa Logic Apps jest przeznaczona do pracy z Aplikacjami interfejsu API. W prosty sposób możesz utworzyć własną aplikację interfejsu API i używać jej jako niestandardowego interfejsu API. Możesz stworzyć aplikację tylko dla siebie lub udostępnić ją i sprzedawać w witrynie Marketplace.

- **Moc prawdziwej integracji** — łatwe rozpoczynanie pracy i rozwój w miarę potrzeb. Usługa Logic Apps może w prosty sposób korzystać z możliwości usługi BizTalk, wiodącego rozwiązania firmy Microsoft służącego specjalistom ds. integracji do tworzenia potrzebnych rozwiązań. Dowiedz się więcej na temat [możliwości usług BizTalk dostępnych w usłudze Logic Apps][biztalk].

## Pojęcia dotyczące Aplikacji logiki

Poniżej przedstawiono niektóre z kluczowych elementów składających się na środowisko pracy Logic Apps. 

- **Przepływ pracy** — usługa Logic Apps oferuje graficzny model procesów biznesowych w postaci serii kroków lub przepływu pracy.
- **Zarządzane interfejsy API** — aplikacje logiki muszą mieć dostęp do danych i usług. Zarządzane interfejsy API są tworzone specjalnie z myślą o ułatwianiu nawiązywania połączeń z danymi oraz pracy z nimi. Zapoznaj się z listą dostępnych obecnie interfejsów API w części dotyczącej [zarządzanych interfejsów API][managedapis].
- **Wyzwalacze** — niektóre zarządzane interfejsy API mogą również działać jako wyzwalacze. Wyzwalacz uruchamia nowe wystąpienie przepływu pracy w przypadku określonego zdarzenia, takiego jak otrzymanie wiadomości e-mail lub zmiana konta usługi Azure Storage.
-  **Akcje** — każdy krok po wyzwalaczu w przepływie pracy jest nazywany akcją. Każda akcja jest przeważnie mapowana na operację w zarządzanych lub niestandardowych aplikacjach interfejsu API.
- **BizTalk** — w przypadku bardziej zaawansowanych scenariuszy integracji usługa Logic Apps oferuje możliwości usługi BizTalk. BizTalk to wiodąca w branży platforma integracji firmy Microsoft. Aplikacje interfejsu API BizTalk umożliwiają łatwe dołączanie sprawdzania poprawności, przekształcania, reguł i innych elementów do przepływów pracy Aplikacji logiki. Dowiedz się więcej z części opisującej, [co to są aplikacje interfejsu API BizTalk][biztalk].

## Wprowadzenie  

 - Aby rozpocząć pracę z usługą Logic Apps, wykonaj kroki opisane w samouczku dotyczącym [tworzenia Aplikacji logiki][create].  
 - [Wyświetl typowe przykłady i scenariusze](app-service-logic-examples-and-scenarios.md)
 - [Dzięki usłudze Logic Apps możesz automatyzować procesy biznesowe](http://channel9.msdn.com/Events/Build/2016/T694) 
 - [Dowiedz się, jak integrować systemy z usługą Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
- Aby uzyskać więcej informacji o platformie usługi Azure App Service, zobacz temat [Azure App Service][appservice].

[biztalk]: app-service-logic-what-are-biztalk-api-apps.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-create-a-trading-partner-agreement.md
[rules]: app-service-logic-use-biztalk-rules.md
[templates]: app-service-logic-use-logic-app-templates.md



<!--HONumber=Jun16_HO2-->


