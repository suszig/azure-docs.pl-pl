---
title: Przykłady integracji usług Azure Service Bus i Event Grid | Microsoft Docs
description: Przykłady integracji wiadomości usługi Service Bus z usługą Event Grid
services: service-bus-messaging
documentationcenter: .net
author: ChristianWolf42
manager: timlt
editor: ''
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 02/15/2018
ms.author: chwolf
ms.openlocfilehash: 3819a274696762861fbe76a9684b8495f1724f6a
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="azure-service-bus-to-azure-event-grid-examples"></a>Przykłady łączenia usługi Azure Service Bus z usługą Azure Event Grid

W tym dokumencie przedstawiamy sposób konfigurowania funkcji platformy Azure i aplikacji logiki odbierającej wiadomości w oparciu o proces odbierania zdarzenia z usługi Event Grid. W przykładzie założono, że używany jest temat usługi Service Bus z dwiema subskrypcjami oraz że subskrypcja usługi Event Grid została utworzona tak, aby zdarzenia były wysyłane tylko dla jednej subskrypcji usługi Service Bus. Następnie wiadomości są wysyłane do tematu usługi Service Bus i sprawdzasz, czy zdarzenie jest generowane dla tej subskrypcji usługi Service Bus. Funkcja lub aplikacja logiki odbiera wiadomości z subskrypcji usługi Service Bus i kończy proces.

* Najpierw sprawdź, czy wszystkie [wymagania wstępne](#prerequisites) zostały spełnione.
* Utwórz [prostą testową funkcję platformy Azure](#test-function-setup) w celu debugowania i wyświetlenia początkowego przepływu zdarzeń z usługi Event Grid.  **Ten krok należy wykonać bez względu na to, czy wykonywany będzie krok 3, czy 4.**
* Utwórz [funkcję platformy Azure do odbierania i przetwarzania wiadomości usługi Service Bus](#receive-messages-using-azure-function) na podstawie zdarzeń usługi Event Grid.
* Skorzystaj z aplikacji [Logic Apps](#receive-messages-using-azure-logic-app).

## <a name="prerequisites"></a>Wymagania wstępne

### <a name="service-bus-namespace"></a>Przestrzeń nazw usługi Service Bus

Utwórz przestrzeń nazw usługi Service Bus Premium. Utwórz temat usługi Service Bus z dwiema subskrypcjami.

### <a name="code-to-send-message-to-the-service-bus-topic"></a>Kod do wysłania wiadomości do tematu usługi Service Bus

Wiadomość można wysłać do tematu usługi Service Bus za pomocą dowolnej metody. Można również użyć poniższego przykładu. W przykładowym kodzie założono, że używasz programu Visual Studio 2017 r.

Sklonuj [to repozytorium serwisu GitHub](https://github.com/Azure/azure-service-bus/).

Przejdź do następującego folderu:

\samples\DotNet\Microsoft.ServiceBus.Messaging\ServiceBusEventGridIntegration i otwórz plik: SBEventGridIntegration.sln.

Następnie przejdź do projektu MessageSender i otwórz plik Program.cs.

![8][]

Wypełnij obszary nazwy tematu i parametrów połączenia, a następnie wykonaj aplikację konsolową:

```CSharp
const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
const string TopicName = "YOUR TOPIC NAME";
```

## <a name="test-function-setup"></a>Testowanie konfiguracji funkcji

Przed rozpoczęciem pracy z kompleksowym scenariuszem warto przetestować co najmniej niewielką funkcję do debugowania i sprawdzić zdarzenia w przepływie.

W portalu utwórz nową aplikację funkcji platformy Azure. Kliknij ten [link](https://docs.microsoft.com/en-us/azure/azure-functions/), aby zapoznać się z podstawami usługi Azure Functions.

W nowo utworzonej funkcji kliknij mały znak plus, aby dodać funkcję wyzwalacza http:

![2][]

![3][]

Następnie skopiuj następujący kod do funkcji:

```CSharp
#r "Newtonsoft.Json"
using System.Net;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    // parse query parameter
    var content = req.Content;

    string jsonContent = await content.ReadAsStringAsync(); 
    log.Info($"Received Event with payload: {jsonContent}");

IEnumerable<string> headerValues;
if (req.Headers.TryGetValues("Aeg-Event-Type", out headerValues))
{
var validationHeaderValue = headerValues.FirstOrDefault();
if(validationHeaderValue == "SubscriptionValidation")
{
var events = JsonConvert.DeserializeObject<GridEvent[]>(jsonContent);
     var code = events[0].Data["validationCode"];
     return req.CreateResponse(HttpStatusCode.OK,
     new { validationResponse = code });
}
}

    return jsonContent == null
    ? req.CreateResponse(HttpStatusCode.BadRequest, "Pass a name on the query string or in the request body")
    : req.CreateResponse(HttpStatusCode.OK, "Hello " + jsonContent);
}

public class GridEvent
{
    public string Id { get; set; }
    public string EventType { get; set; }
    public string Subject { get; set; }
    public DateTime EventTime { get; set; }
    public Dictionary<string, string> Data { get; set; }
    public string Topic { get; set; }
}
```

Kliknij pozycję Zapisz i uruchom.

## <a name="connect-function-and-namespace-via-event-grid"></a>Łączenie funkcji i przestrzeni nazw za pośrednictwem usługi Event Grid

Następny krok to powiązanie funkcji i przestrzeni nazw usługi Service Bus. W tym przykładzie użyj witryny Azure Portal. Zobacz stronę [pojęć](service-bus-to-event-grid-integration-concept.md), aby zrozumieć, jak można osiągnąć ten sam cel za pomocą programu PowerShell lub interfejsu wiersza polecenia platformy Azure.

Aby utworzyć nową subskrypcję usługi Azure Event Grid, przejdź do przestrzeni nazw w witrynie Azure Portal i wybierz blok Event Grid. Kliknij pozycję „+ Subskrypcja zdarzeń”.

Poniższy zrzut ekranu pokazuje przestrzeń nazw, która ma już kilka subskrypcji usługi Event Grid.

![20][]

Poniższy zrzut ekranu pokazuje, jak subskrybować element webhook lub funkcję platformy Azure bez specyficznego filtrowania. Pamiętaj, aby dodać filtr dotyczący subskrypcji usługi Service Bus jako „Filtr sufiksu”:

![21][]

Wyślij wiadomość do tematu usługi Service Bus, jak wspomniano w sekcji wymagań wstępnych, i sprawdź przepływ zdarzeń za pomocą funkcji monitorowania funkcji platformy Azure.

![9][]

### <a name="receive-messages-using-azure-function"></a>Odbieranie wiadomości przy użyciu funkcji platformy Azure

W poprzedniej sekcji przeprowadzono prosty scenariusz debugowania i testowania oraz sprawdzono, czy zdarzenia znajdują się w przepływie. W tej części dokumentacji przyjrzysz się procesowi odbierania i przetwarzania wiadomości po odebraniu zdarzenia.

Funkcja platformy Azure jest dodawana w taki sposób, ponieważ funkcje usługi Service Bus działają w ramach usługi Azure Functions — nie obsługują jeszcze natywnie nowej integracji usługi Event Grid.

W rozwiązaniu programu Visual Studio otwartym w wymaganiach wstępnych wybierz plik ReceiveMessagesOnEvent.cs. Wprowadź parametry połączenia w kodzie:

![10][]

```Csharp
const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
```

Następnie przejdź do witryny Azure Portal i pobierz profil publikowania dla funkcji platformy Azure utworzony wcześniej w kroku [2. Testowanie konfiguracji funkcji](#2-test-function-setup).

![11][]

Następnie w programie Visual Studio kliknij prawym przyciskiem myszy element SBEventGridIntegration i wybierz pozycję Publikuj. Użyj pobranego wcześniej profilu publikowania, wybierz opcję Importuj profil, a następnie kliknij pozycję Publikuj.

![12][]

Po opublikowaniu nowej funkcji platformy Azure utwórz nową subskrypcję usługi Azure Event Grid wskazującą na nową funkcję platformy Azure. Upewnij się, że poprawnie stosujesz filtr „Kończy się na”, którego wartością powinna być nazwa subskrypcji usługi Service Bus.

Wyślij wiadomość do utworzonego wcześniej tematu usługi Azure Service Bus i sprawdź w dzienniku funkcji platformy Azure w portalu, czy zdarzenia przepływają i wiadomości są odbierane.

![12-1][]

### <a name="receive-messages-using-azure-logic-app"></a>Odbieranie wiadomości przy użyciu aplikacji logiki platformy Azure

Poniższe instrukcje przedstawiają sposób łączenia aplikacji logiki Azure z usługami Azure Service Bus i Azure Event Grid:

Najpierw utwórz nową aplikację logiki w witrynie Azure Portal i wybierz siatkę Event Grid jako akcję początkową.

![13][]

Widok początkowy w projektancie aplikacji logiki powinien wyglądać jak poniższy zrzut ekranu. Element „Nazwa zasobu” zastąp nazwą własnej przestrzeni nazw. Pamiętaj również o rozwinięciu opcji zaawansowanych i dodaniu filtra sufiksu dla swojej subskrypcji:

![14][]

Następnie dodaj akcję odbierania, aby otrzymywać informacje z subskrypcji tematu. Ostateczna akcja powinna wyglądać jak na poniższym zrzucie ekranu.

![15][]

Następnie dodaj zdarzenie ukończenia, które powinno wyglądać następująco.

![16][]

Zapisz aplikację logiki i wyślij wiadomość do tematu usługi Service Bus, jak wspomniano w wymaganiach wstępnych. Obserwuj wykonywanie aplikacji logiki. Jeśli klikniesz pozycję „Omówienie”, a następnie „Historia uruchomień”, możesz uzyskać dalsze dane dotyczące wykonywania.

![17][]

![18][]

## <a name="next-steps"></a>Następne kroki

* Dowiedz się więcej na temat usługi [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/).
* Dowiedz się więcej na temat usługi [Azure Functions](https://docs.microsoft.com/azure/azure-functions/).
* Dowiedz się więcej na temat usługi [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/).
* Dowiedz się więcej na temat usługi [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

[2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid2.png
[3]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid3.png
[7]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid7.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[10]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid10.png
[11]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid11.png
[12]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12.png
[12-1]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-1.png
[12-2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-2.png
[13]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid13.png
[14]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid14.png
[15]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid15.png
[16]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid16.png
[17]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid17.png
[18]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid18.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png
