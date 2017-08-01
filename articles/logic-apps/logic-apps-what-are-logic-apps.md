---
title: "Łączenie aplikacji i integrowanie danych z przepływami pracy — Azure Logic Apps | Microsoft Docs"
description: "Możesz tworzyć przepływy pracy i automatyzować procesy przez łączenie aplikacji i integrowanie danych z usługą Azure Logic Apps."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 07765c05-72a6-4169-a8ab-f6420bfbaf07
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/23/2017
ms.author: klam
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 64af585f81d39daaa5373d7cf080404ee5f1b037
ms.contentlocale: pl-pl
ms.lasthandoff: 06/28/2017


---
# <a name="what-are-logic-apps"></a>Co to jest Logic Apps?
Usługa Logic Apps umożliwia uproszczenie i wdrażanie skalowalnych integracji i przepływów pracy w chmurze. Udostępnia ona wizualnego projektanta pozwalającego na modelowanie i automatyzację procesu w postaci serii kroków znanych jako przepływ pracy.  Istnieje [wiele łączników](../connectors/apis-list.md) dla rozwiązań w chmurze i lokalnych, które umożliwiają szybką integrację usług i protokołów.  Aplikacja logiki rozpoczyna się od wyzwalacza (na przykład „Gdy konto zostanie dodane do oprogramowania Dynamics CRM”), po uruchomieniu którego można użyć wielu połączeń akcji, konwersji i logiki warunkowej.

Zalety korzystania z aplikacji logiki są następujące:  

* Oszczędność czasu dzięki możliwości tworzenia złożonych procesów przy użyciu łatwych do zrozumienia narzędzi do projektowania
* Bezproblemowe implementowanie wzorców i przepływów pracy, których implementacja w kodzie mogłaby być trudna
* Szybkie rozpoczynanie pracy przy użyciu szablonów
* Dostosowywanie aplikacji logiki przy użyciu własnych interfejsów API, własnego kodu i własnych akcji
* Łączenie i synchronizowanie różnych systemów w środowiskach lokalnych i w chmurze
* Tworzenie na podstawie usług BizTalk Server, API Management, Azure Functions i Azure Service Bus z najwyższej klasy obsługą integracji

Usługa Logic Apps to w pełni zarządzane rozwiązanie typu iPaaS (platforma integracji jako usługa), dzięki którym deweloperzy nie muszą się martwić o zapewnienie hostingu, skalowalność, dostępność i zarządzanie.  Usługa Logic Apps umożliwia automatyczne skalowanie w górę w celu odpowiedzi na rosnące potrzeby.

![Projektant aplikacji przepływu](media/logic-apps-what-are-logic-apps/LogicAppCapture2.png)

Jak wspomniano, dzięki usłudze Logic Apps można automatyzować procesy biznesowe. Oto kilka przykładów:  

* Przenoszenie plików przekazanych na serwer FTP do usługi Azure Storage
* Przetwarzanie i kierowanie zamówień w systemach lokalnych i systemach w chmurze
* Monitorowanie wszystkich tweetów dotyczących określonego tematu, analiza opinii oraz tworzenie alarmów i zadań dla elementów wymagających wykonania czynności.

Wszystkie tego typu scenariusze można skonfigurować przy użyciu projektanta wizualnego i bez konieczności napisania jakiegokolwiek wiersza kodu. Rozpocznij [teraz tworzenie aplikacji logiki][create].  Napisana aplikacja logiki może być [szybko wdrażana i ponownie konfigurowana](../logic-apps/logic-apps-create-deploy-template.md) w wielu środowiskach i regionach.

## <a name="why-logic-apps"></a>Dlaczego warto korzystać z usługi Logic Apps?
Usługa Logic Apps zapewnia szybkość i skalowalność w obszarze integracji w przedsiębiorstwie.  Łatwy w użyciu projektant, dostępne różne wyzwalacze i akcje, a także zaawansowane narzędzia do zarządzania ułatwiają centralizację interfejsów API bardziej niż kiedykolwiek wcześniej.  Usługa Logic Apps umożliwia firmom wdrażającym cyfryzację łączenie starszych systemów z najnowszymi.

Ponadto dzięki naszemu [kontu integracji w przedsiębiorstwie ][biztalk] można przeprowadzać skalowanie do zaawansowanej integracji przy użyciu między innymi [przetwarzania komunikatów XML][xml] i [zarządzania partnerami handlowymi][tpm].

* **Proste w użyciu narzędzia do projektowania** — aplikacje logiki można w całości przygotować w przeglądarce lub przy użyciu narzędzi programu Visual Studio. Rozpocznij od wyzwalacza — od prostego harmonogramu po utworzenie problemu w serwisie GitHub. Następnie zorganizuj dowolną liczbę akcji przy użyciu bogatej galerii łączników.
* **Proste łączenie interfejsów API** — nawet zadania tworzenia proste do opisania są trudne do zaimplementowania w kodzie. Usługa Logic Apps ułatwia łączenie różnych systemów. Chcesz połączyć rozwiązanie marketingowe w chmurze z lokalnym systemem rozliczeniowym? Chcesz scentralizować przesyłanie komunikatów przez interfejsy API i systemy przy użyciu usługi Enterprise Service Bus? Aplikacje logiki to najszybsze i najbardziej niezawodne rozwiązania umożliwiające zaspokojenie tych potrzeb.
* **Szybkie rozpoczynanie pracy z szablonami** — aby Ci pomóc, przygotowaliśmy [galerię szablonów][templates] umożliwiającą szybkie tworzenie niektórych typowych rozwiązań. Od zaawansowanych rozwiązań B2B do prostej łączności z usługami typu SaaS (kilka rozwiązań jest po prostu „do zabawy”) — galeria to najszybszy sposób rozpoczęcia korzystania z usługi Logic Apps.
* **Możliwość rozszerzania** — nie widzisz potrzebnego Ci łącznika? Usługa Logic Apps jest przeznaczona do pracy z własnymi interfejsami API i własnym kodem. Można w prosty sposób utworzyć własną aplikację API używaną z własnym łącznikiem albo utworzyć wywołanie [funkcji platformy Azure](https://functions.azure.com) w celu wykonania fragmentów kodów na żądanie. 
* **Moc prawdziwej integracji** — łatwe rozpoczynanie pracy i rozwój w miarę potrzeb. Usługa Logic Apps może w prosty sposób korzystać z możliwości usługi BizTalk, wiodącego rozwiązania firmy Microsoft służącego specjalistom ds. integracji do tworzenia potrzebnych rozwiązań. Dowiedz się więcej o [pakiecie integracyjnym dla przedsiębiorstw](../logic-apps/logic-apps-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Pojęcia dotyczące Aplikacji logiki
Poniżej przedstawiono niektóre z kluczowych elementów składających się na środowisko pracy Logic Apps. 

* **Przepływ pracy** — usługa Logic Apps oferuje graficzny model procesów biznesowych w postaci serii kroków lub przepływu pracy.
* **Zarządzane łączniki** — aplikacje logiki muszą mieć dostęp do danych i usług. Zarządzane łączniki są tworzone specjalnie z myślą o ułatwianiu nawiązywania połączeń z danymi oraz pracy z nimi. Zobacz listę dostępnych łączników w [artykule na temat zarządzanych łączników][managedapis].
* **Wyzwalacze** — niektóre zarządzane łączniki mogą również działać jako wyzwalacze. Wyzwalacz uruchamia nowe wystąpienie przepływu pracy w przypadku określonego zdarzenia, takiego jak otrzymanie wiadomości e-mail lub zmiana konta usługi Azure Storage.
* **Akcje** — każdy krok po wyzwalaczu w przepływie pracy jest nazywany akcją. Każda akcja jest przeważnie mapowana na operację w zarządzanych łącznikach lub niestandardowych aplikacjach interfejsu API.
* **Pakiet integracyjny dla przedsiębiorstw** — w przypadku bardziej zaawansowanych scenariuszy integracji usługa Logic Apps oferuje możliwości usługi BizTalk. BizTalk to wiodąca w branży platforma integracji firmy Microsoft. Łączniki pakietu integracyjnego dla przedsiębiorstw umożliwiają łatwe dołączanie sprawdzania poprawności, przekształcania i innych elementów do przepływów pracy usługi Logic Apps.

## <a name="getting-started"></a>Wprowadzenie
* Aby rozpocząć pracę z usługą Logic Apps, wykonaj kroki opisane w samouczku dotyczącym [tworzenia aplikacji logiki][create].  
* [Wyświetlanie typowych przykładów i scenariuszy](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Dzięki usłudze Logic Apps możesz automatyzować procesy biznesowe](http://channel9.msdn.com/Events/Build/2016/T694) 
* [Dowiedz się, jak integrować systemy z usługą Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: logic-apps-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: logic-apps-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: logic-apps-enterprise-integration-accounts.md
[xml]: logic-apps-enterprise-integration-b2b.md
[templates]: logic-apps-use-logic-app-templates.md

