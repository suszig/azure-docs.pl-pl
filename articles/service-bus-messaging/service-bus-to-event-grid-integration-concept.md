---
title: "Omówienie integracji usług Azure Service Bus i Event Grid | Microsoft Docs"
description: "Opis integracji wiadomości usługi Service Bus z usługą Event Grid"
services: service-bus-messaging
documentationcenter: .net
author: ChristianWolf42
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 02/15/2018
ms.author: chwolf
ms.openlocfilehash: 17f6c107ea1adfd4463ff4cc205fe11d111acb84
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="azure-service-bus-to-azure-event-grid-integration-overview"></a>Omówienie integracji usług Azure Service Bus i Event Grid

W usłudze Azure Service Bus uruchomiono nową opcję integracji z usługą Azure Event Grid. Kluczowy scenariusz, który umożliwia ta funkcja, jest następujący: subskrypcje lub kolejki usługi Service Bus z małą liczbą wiadomości nie muszą mieć odbiornika przez cały czas przeprowadzającego sondowanie w poszukiwaniu komunikatów. Usługa Service Bus teraz może emitować zdarzenia do usługi Azure Event Grid, gdy przy braku odbiorników kolejka lub subskrypcja zawiera wiadomości. Można tworzyć subskrypcje usługi Azure Event Grid powiązane z przestrzeniami nazw usługi Service Bus, słuchać tych zdarzeń i reagować na nie przez uruchomienie odbiornika. W przypadku tej funkcji usługa Service Bus może być używana w reaktywnych modelach programowania.

Do włączenia tej funkcji potrzebne są następujące elementy:

* Przestrzeń nazw usługi Azure Service Bus w warstwie Premium z co najmniej jedną kolejką lub tematem usługi Service Bus i co najmniej jedną subskrypcją.
* Dostęp współautora do przestrzeni nazw usługi Azure Service Bus.
* Ponadto potrzebujesz subskrypcji usługi Azure Event Grid dla przestrzeni nazw usługi Service Bus. Ta subskrypcja otrzymuje z usługi Azure Event Grid powiadomienia o wiadomościach do pobrania. Typowymi subskrybentami mogą być aplikacje Logic Apps, funkcje Azure Functions lub element webhook kontaktujący się z aplikacją internetową, który następnie przetwarza wiadomości. 

![19][]

### <a name="verify-that-you-have-contributor-access"></a>Weryfikowanie dostępu współautora

Przejdź do przestrzeni nazw usługi Service Bus i wybierz opcję „Kontrola dostępu (IAM)”, jak pokazano poniżej:

![1][]

### <a name="events-and-event-schemas"></a>Zdarzenia i schematów zdarzeń

Obecnie usługa Azure Service Bus wysyła zdarzenia w przypadku dwóch scenariuszy.

* [ActiveMessagesWithNoListenersAvailable](#active-messages-available-event)
* [DeadletterMessagesAvailable](#dead-lettered-messages-available-event)

Ponadto używa ona standardowych zabezpieczeń usługi Azure Event Grid i [mechanizmów uwierzytelniania](https://docs.microsoft.com/en-us/azure/event-grid/security-authentication).

Aby uzyskać dalsze szczegółowe informacje na temat schematów zdarzeń usługi Event Grid, kliknij [ten](https://docs.microsoft.com/en-us/azure/event-grid/event-schema) link.

#### <a name="active-messages-available-event"></a>Zdarzenie Dostępne aktywne wiadomości

To zdarzenie jest generowane, jeśli masz aktywne wiadomości w kolejce lub subskrypcji i nie ma nasłuchujących odbiorników.

Schemat tego zdarzenia jest następujący:

```JSON
{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}
```

#### <a name="dead-lettered-messages-available-event"></a>Zdarzenie Dostępnie wiadomości utracone

Odbierasz co najmniej jedno zdarzenie na kolejkę utraconych wiadomości, która zawiera wiadomości, ale nie ma aktywnych odbiorników.

Schemat tego zdarzenia jest następujący:

```JSON
[{
     "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="how-often-and-how-many-events-are-emitted"></a>Jak często i ile zdarzeń jest emitowanych?

Jeśli masz wiele kolejek i tematów/subskrypcji w przestrzeni nazw, odbierasz co najmniej jedno zdarzenie na kolejkę i jedno na subskrypcję. Zdarzenia są emitowane natychmiast, jeśli w jednostce usługi Service Bus nie ma wiadomości i nadejdzie nowa wiadomość lub co dwie minuty, chyba że usługa Azure Service Bus wykryje aktywny odbiornik. Przeglądanie wiadomości nie przerywa zdarzeń.

Domyślnie usługa Azure Service Bus emituje zdarzenia dla wszystkich jednostek w przestrzeni nazw. Jeśli chcesz odbierać zdarzenia tylko dla konkretnych jednostek, zapoznaj się z kolejną sekcją dotyczącą filtrowania.

### <a name="filtering-limiting-from-where-you-get-events"></a>Filtrowanie ograniczające miejsce odbierania zdarzeń

Jeśli na przykład chcesz odbierać zdarzenia tylko dla jednej kolejki lub jednej subskrypcji w przestrzeni nazw, możesz użyć filtrów „Zaczyna się od” i „Kończy się na” udostępnianych przez usługę Azure Event Grid. W niektórych interfejsach filtry są nazywane „Prefiks” i „Sufiks”. Jeśli chcesz odbierać zdarzenia dla wielu, ale nie wszystkich, kolejek i subskrypcji, możesz utworzyć wiele różnych subskrypcji usługi Azure Event Grid i określić filtr dla każdej z nich.

## <a name="how-to-create-azure-event-grid-subscriptions-for-service-bus-namespaces"></a>Jak utworzyć subskrypcje usługi Azure Event Grid dla przestrzeni nazw usługi Service Bus

Istnieją trzy różne sposoby tworzenia subskrypcji usługi Azure Event Grid dla przestrzeni nazw usługi Service Bus.

* [Witryna Azure Portal](#portal-instructions)
* [Interfejs wiersza polecenia platformy Azure](#azure-cli-instructions)
* [Program PowerShell](#powershell-instructions)

## <a name="portal-instructions"></a>Instrukcje dotyczące portalu

Aby utworzyć nową subskrypcję usługi Azure Event Grid, przejdź do przestrzeni nazw w witrynie Azure Portal i wybierz blok Event Grid. Kliknij pozycję „+ Subskrypcja zdarzenia”. Poniżej pokazano przestrzeń nazw, która ma już kilka subskrypcji usługi Event Grid.

![20][]

Poniższy zrzut ekranu pokazuje przykład subskrybowania funkcji platformy Azure lub elementu webhook bez specyficznego filtrowania:

![21][]

## <a name="azure-cli-instructions"></a>Instrukcje dotyczące interfejsu wiersza polecenia platformy Azure

Najpierw upewnij się, że masz co zainstalowany interfejs wiersza polecenia co najmniej w wersji 2.0. Instalator jest dostępny do pobrania w tym miejscu. Następnie naciśnij klawisze „Windows + X” i otwórz nową konsolę programu PowerShell z uprawnieniami administratora. Możesz również pobrać powłokę poleceń w witrynie Azure Portal.

Wykonaj następujący kod:

```PowerShell
Az login

Aa account set -s “THE SUBSCRIPTION YOU WANT TO USE”

$namespaceid=(az resource show --namespace Microsoft.ServiceBus --resource-type namespaces --name “<yourNamespace>“--resource-group “<Your Resource Group Name>” --query id --output tsv)

az eventgrid event-subscription create --resource-id $namespaceid --name “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” --endpoint “<your_function_url>” --subject-ends-with “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

## <a name="powershell-instructions"></a>Instrukcje dotyczące programu PowerShell

Upewnij się, że masz zainstalowany program Azure PowerShell. Znajdziesz go tutaj. Następnie naciśnij klawisze „Windows + X” i otwórz nową konsolę programu PowerShell z uprawnieniami administratora. Możesz również pobrać powłokę poleceń w witrynie Azure Portal.

```PowerShell
Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionName "<YOUR SUBSCRIPTION NAME>"

# This might be installed already
Install-Module AzureRM.ServiceBus

$NSID = (Get-AzureRmServiceBusNamespace -ResourceGroupName "<YOUR RESOURCE GROUP NAME>" -Na
mespaceName "<YOUR NAMESPACE NAME>").Id 

New-AzureRmEVentGridSubscription -EventSubscriptionName “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” -ResourceId $NSID -Endpoint "<YOUR FUNCTION URL>” -SubjectEndsWith “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

W tym miejscu możesz eksplorować inne opcje instalacji lub [przetestować przepływające zdarzenia](#test-that-events-are-flowing).

## <a name="next-steps"></a>Następne kroki

* [Przykłady](service-bus-to-event-grid-integration-example.md) usług Service Bus i Event Grid.
* Dowiedz się więcej na temat usługi [Azure Event Grid](https://docs.microsoft.com/en-us/azure/azure-functions/).
* Dowiedz się więcej na temat usługi [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/).
* Dowiedz się więcej na temat usługi [Azure Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/).
* Dowiedz się więcej na temat usługi [Azure Service Bus](https://docs.microsoft.com/en-us/azure/azure-functions/).

[1]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgrid1.png
[19]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgriddiagram.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png