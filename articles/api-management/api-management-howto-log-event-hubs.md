---
title: "Jak mają być rejestrowane zdarzenia do usługi Azure Event Hubs w usłudze Azure API Management | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie rejestrowania zdarzeń do usługi Azure Event Hubs w usłudze Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 066f151aa96b3a57c86515411ba05a982c10aa5f
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Jak mają być rejestrowane zdarzenia do usługi Azure Event Hubs w usłudze Azure API Management
Azure Event Hubs to wysoce skalowalna usługa transferu danych przychodzących, która może obsługiwać miliony zdarzeń na sekundę, dzięki czemu możliwe jest przetwarzanie i analizowanie olbrzymich ilości danych wytworzonych przez podłączone urządzenia i aplikacje. Usługa Event Hubs działa jako "drzwi wejściowe" dla potoku zdarzeń, a po pobraniu danych do Centrum zdarzeń, można je przekształcać i przechowywane za pomocą dowolnego dostawcy analiz w czasie rzeczywistym lub kart przetwarzania wsadowego i magazynowania. Usługa Event Hubs oddziela wytwarzanie strumienia zdarzeń od użycia tych zdarzeń, dzięki czemu odbiorcy zdarzeń mogą uzyskiwać dostęp do zdarzeń zgodnie z własnym harmonogramem.

W tym artykule jest dodatek do [zintegrować zarządzanie interfejsami API Azure z usługą Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) wideo i opisuje sposób rejestrowania zdarzeń interfejsu API zarządzania przy użyciu usługi Azure Event Hubs.

## <a name="create-an-azure-event-hub"></a>Tworzenie Centrum zdarzeń platformy Azure
Aby utworzyć nowe Centrum zdarzeń, zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) i kliknij przycisk **nowy**->**usługi aplikacji**->**usługi Service Bus**->**Centrum zdarzeń**->**szybkie tworzenie**. Wprowadź nazwę Centrum zdarzeń, region, wybierz subskrypcję, a następnie wybierz obszar nazw. Jeśli nie utworzono jeszcze przestrzeni nazw można go utworzyć, wpisując nazwę w **Namespace** pola tekstowego. Po skonfigurowaniu wszystkich właściwości, kliknij przycisk **Utwórz nowe Centrum zdarzeń** do tworzenia Centrum zdarzeń.

![Tworzenie Centrum zdarzeń][create-event-hub]

Następnie przejdź do **Konfiguruj** karcie nowe Centrum zdarzeń i utworzyć dwa **udostępnionych zasady dostępu**. Nazwa pierwszego **wysyłanie** i nadaj mu **wysyłania** uprawnienia.

![Wysyłanie zasad][sending-policy]

Nazwa drugi **odbieranie**, nadaj **nasłuchiwania** uprawnienia, a następnie kliknij przycisk **zapisać**.

![Odebranie zasad][receiving-policy]

Wszystkie zasady dostępu współdzielonego umożliwia aplikacji wysyłanie i odbieranie zdarzeń z Centrum zdarzeń. Aby uzyskać dostęp do parametrów połączenia dla tych zasad, przejdź do **pulpitu nawigacyjnego** Centrum zdarzeń i kliknij na karcie **informacje o połączeniu**.

![Parametry połączenia][event-hub-dashboard]

**Wysyłanie** ciąg połączenia jest używany podczas rejestrowania zdarzeń i **odbieranie** ciąg połączenia jest używany podczas pobierania zdarzeń z Centrum zdarzeń.

![Parametry połączenia][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Utworzyć zarządzanie interfejsami API rejestratora
Teraz, gdy masz Centrum zdarzeń, następnym krokiem jest skonfigurowanie [rejestratora](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) w zarządzania infrastrukturą interfejsu API usługi tak, aby móc zarejestrować zdarzenia do Centrum zdarzeń.

Zarządzanie interfejsami API rejestratorów są skonfigurowane przy użyciu [interfejsu API REST zarządzania interfejsu API](http://aka.ms/smapi). Przed rozpoczęciem korzystania z interfejsu API REST po raz pierwszy, przejrzyj [wymagania wstępne](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) i upewnij się, że masz [włączony dostęp do interfejsu API REST](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).

Aby utworzyć rejestrator, należy przy użyciu następującego szablonu adresu URL żądania HTTP PUT.

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview`

* Zastąp `{your service}` nazwą wystąpienia usługi Zarządzanie interfejsami API.
* Zastąp `{new logger name}` z żądaną nazwą dla Twojego nowego rejestratora. Ta nazwa będzie odwoływać, podczas konfigurowania [dziennika do Centrum eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) zasad

Dodaj następujące nagłówki na żądanie.

* Content-Type: application/json
* Autoryzacji: SharedAccessSignature 58...
  * Aby uzyskać instrukcje dotyczące generowania `SharedAccessSignature` zobacz [Azure API Management REST API uwierzytelniania](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).

Określ treści żądania, korzystając z poniższego szablonu.

```json
{
  "loggertype" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of the Event Hub from the Azure Classic Portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* `loggertype`należy wybrać opcję `AzureEventHub`.
* `description`zapewnia opcjonalny opis rejestratora i w razie potrzeby może być ciągiem o zerowej długości.
* `credentials`zawiera `name` i `connectionString` Azure Centrum zdarzeń.

Po dokonaniu żądania, jeśli rejestrator jest tworzony kod stanu `201 Created` jest zwracany.

> [!NOTE]
> Pozostałe możliwe kody powrotu i ich przyczyny, zobacz [utworzyć Rejestrator](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT). Aby zobaczyć, jak wykonywać inne operacje, takie jak listy, update i delete, zobacz [rejestratora](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) dokumentacji jednostki.
>
>

## <a name="configure-log-to-eventhubs-policies"></a>Konfigurowanie zasad dziennika do eventhubs
Po Twoje rejestratora jest skonfigurowany w usłudze API Management, można skonfigurować zasad dziennika do eventhubs żądanego zdarzenia logowania. Zasada dziennika do eventhubs może służyć w sekcji zasady ruchu przychodzącego lub sekcji zasady ruchu wychodzącego.

Aby skonfigurować zasady, zaloguj się do [portalu Azure](https://portal.azure.com), przejdź do usługi API Management i kliknij przycisk **portal wydawcy** dostęp do portalu wydawcy.

![Portal wydawcy][publisher-portal]

Kliknij przycisk **zasady** zarządzanie interfejsami API menu po lewej stronie, wybierz żądany produktu i interfejsu API, a następnie kliknij przycisk **Dodaj zasady**. W tym przykładzie Trwa dodawanie zasad, aby **Echo API** w **nieograniczone** produktu.

![Dodawanie zasad][add-policy]

Umieść kursor w `inbound` sekcji zasad i kliknij przycisk **dziennika do Centrum EventHub** zasad, aby wstawić `log-to-eventhub` szablon zasad w instrukcji.

![Edytor zasad][event-hub-policy]

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```

Zastąp `logger-id` nazwą rejestratora zarządzanie interfejsami API skonfigurowana w poprzednim kroku.

Można użyć dowolne wyrażenie zwracające ciąg jako wartość `log-to-eventhub` elementu. W tym przykładzie jest rejestrowane ciąg zawierający daty i godziny, nazwę usługi, identyfikator żądania, żądania adresu ip oraz nazwy operacji.

Kliknij przycisk **zapisać** Aby zapisać konfigurację zaktualizowane zasady. Natychmiast po ich zapisaniu zasady są aktywne, a zdarzenia są rejestrowane w wyznaczonym Centrum zdarzeń.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej na temat usługi Azure Event Hubs
  * [Wprowadzenie do usługi Azure Event Hubs](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Odbieranie komunikatów za pomocą klasy EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Przewodnik programowania w usłudze Event Hubs](../event-hubs/event-hubs-programming-guide.md)
* Dowiedz się więcej na temat integracji usługi API Management i usługi Event Hubs
  * [Odwołanie do jednostki rejestratora](https://docs.microsoft.com/rest/api/apimanagement/loggers)
  * [informacje o zasadach dziennika do Centrum eventhub](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Monitoruj swoje interfejsy API z usługi Azure API Management, usługa Event Hubs i Runscope](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Obejrzyj przewodnik wideo
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Integrate-Azure-API-Management-with-Event-Hubs/player]
>
>

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
