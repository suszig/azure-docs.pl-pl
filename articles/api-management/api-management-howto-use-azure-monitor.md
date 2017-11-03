---
title: "Monitorowanie zarządzanie interfejsami API Azure Monitor | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie monitorowania usługi Zarządzanie interfejsami API Azure za pomocą monitora Azure."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 717e033aa4bbd4dd8ebcc727c3f551aee81770dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-api-management-with-azure-monitor"></a>Monitor interfejsu API zarządzania z Monitorem systemu Azure
Azure Monitor jest usługą platformy Azure, który udostępnia jedno źródło do monitorowania wszystkich zasobów na platformie Azure. Z monitorem Azure można zwizualizować, zapytania, trasy, archiwum i podejmuj działania na metryki i dzienników pochodzących z zasobów platformy Azure, takich jak zarządzanie interfejsami API. 

Poniższe wideo pokazuje, jak monitorować zarządzanie interfejsami API Azure monitora. Aby uzyskać więcej informacji na temat Monitora Azure, zobacz [Rozpoczynanie pracy z monitorem Azure]. 


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>
 
## <a name="metrics"></a>Metryki
Zarządzanie interfejsami API obecnie emituje pięć metryki i planujemy dodać więcej w przyszłości. Te metryki są emitowane co minutę, umożliwiając niemal w czasie rzeczywistym wgląd w stan i kondycję swoje interfejsy API. Poniżej znajduje się podsumowanie metryki:
* Całkowita liczba żądań bramy: liczba interfejsu API żądań w okresie. 
* Liczba pomyślnych żądań bramy: liczba żądań interfejsu API, które otrzymały pomyślne kody odpowiedzi HTTP, w tym 304 307 i mniejszego niż 301 (na przykład 200). 
* Żądań zakończonych niepowodzeniem bramy: liczba żądań interfejsu API, które odebrano błędną kody odpowiedzi HTTP, w tym 400 i niczego większych niż 500.
* Nieautoryzowanych żądań bramy: liczba żądań interfejsu API, które otrzymały kody odpowiedzi HTTP 401, 403 i 429 w tym. 
* Inne żądania bramy: liczba żądań interfejsu API, które otrzymały kody odpowiedzi HTTP, które nie należą do żadnej z powyższych kategorii (na przykład 418).

Dostępne metryki w usłudze API Management lub metryki dostęp do wszystkich zasobów platformy Azure w monitorze Azure. Aby wyświetlić metryki w usłudze API Management:
1. Otwórz Azure portal.
2. Przejdź do usługi API Management.
3. Kliknij przycisk **metryki**.

![Metryki bloku][metrics-blade]

Aby uzyskać więcej informacji o sposobie używania metryki, zobacz [omówienie metryki].

## <a name="activity-logs"></a>Dzienniki aktywności
Dzienniki aktywności zapewniają wgląd w operacje wykonywane na usługi Zarządzanie interfejsami API. Został on wcześniej znana jako "dzienników inspekcji" lub "operacyjne dzienniki". Korzystając z Dzienniki aktywności, można określić ", co, która i kiedy" dla żadnego zapisu (PUT, POST, DELETE) podejmowaną w odniesieniu do usług interfejsu API zarządzania. 

> [!NOTE]
> Dzienniki aktywności nie obejmują operacji odczytu (GET) lub operacje wykonywane w klasycznym portalu wydawcy lub przy użyciu oryginalnego interfejsów API zarządzania.

Dostęp do dzienników aktywności w usłudze API Management lub uzyskać dostępu do dzienników wszystkich zasobów platformy Azure w monitorze Azure. Aby wyświetlić aktywności logowania w usłudze API Management:
1. Otwórz Azure portal.
2. Przejdź do usługi API Management.
3. Kliknij przycisk **dziennik aktywności**.

![Blok Dzienniki aktywności][activity-logs-blade]

Aby uzyskać więcej informacji o sposobie używania metryki, zobacz [omówienie Dzienniki aktywności].

## <a name="alerts"></a>Alerty
Można skonfigurować do odbierania alertów w oparciu metryki i działania dzienników. Azure Monitor umożliwia skonfigurowanie alert o konieczności wyzwala, wykonaj następujące czynności:

* Wyślij wiadomość e-mail z powiadomieniem
* Wywołanie elementu webhook
* Wywołanie aplikacji logiki platformy Azure

Reguły alertów można skonfigurować w usłudze API Management lub w monitorze Azure. Aby skonfigurować je w usłudze API Management: 
1. Otwórz Azure portal.
2. Przejdź do usługi API Management.
3. Kliknij przycisk **reguły alertów**.

![Blok reguły alertów][alert-rules-blade]

Aby uzyskać więcej informacji o korzystaniu z alertami, zobacz [Przegląd alertów].

## <a name="diagnostic-logs"></a>Dzienniki diagnostyczne
Dzienniki diagnostyczne zawierają zaawansowane informacje o operacje i błędy, które są ważne w przypadku inspekcji, a także rozwiązywania problemów. Dzienniki diagnostyczne różnią się od Dzienniki aktywności. Dzienniki aktywności wgląd w operacje wykonywane na zasobów platformy Azure. Dzienniki diagnostyczne zapewniają wgląd w operacje, w zasobie wykonanie samego.

Zarządzanie interfejsami API zawiera obecnie diagnostyki dzienniki (umieścić w zadaniu wsadowym co godzinę) o poszczególnych interfejsu API żądania z każdego wpisu o następującej strukturze:

```
{
    "Tenant": "",
      "DeploymentName": "",
      "time": "",
      "resourceId": "",
      "category": "GatewayLogs",
      "operationName": "Microsoft.ApiManagement/GatewayLogs",
      "durationMs": ,
      "Level": ,
      "properties": "{
          "ApiId": "",
          "OperationId": "",
          "ProductId": "",
          "SubscriptionId": "",
          "Method": "",
          "Url": "",
          "RequestSize": ,
          "ServiceTime": "",
          "BackendMethod": "",
          "BackendUrl": "",
          "BackendResponseCode": ,
          "ResponseCode": ,
          "ResponseSize": ,
          "Cache": "",
          "UserId"
      }"
 }
```

Można uzyskać dostępu do dzienników diagnostycznych w usłudze API Management lub uzyskać dostępu do dzienników wszystkich zasobów platformy Azure w monitorze Azure. Aby wyświetlić dzienniki diagnostyczne w usłudze API Management:
1. Otwórz Azure portal.
2. Przejdź do usługi API Management.
3. Kliknij przycisk **dziennik diagnostyczny**.

![Blok dzienników diagnostycznych][diagnostic-logs-blade]

Aby uzyskać więcej informacji o sposobie używania metryki, zobacz [Przegląd dzienników diagnostycznych].

## <a name="next-step"></a>Następny krok

* [Rozpoczynanie pracy z monitorem Azure]
* [omówienie metryki]
* [omówienie Dzienniki aktywności]
* [Przegląd dzienników diagnostycznych]
* [Przegląd alertów]

[Rozpoczynanie pracy z monitorem Azure]: ../monitoring-and-diagnostics/monitoring-get-started.md
[omówienie metryki]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md
[omówienie Dzienniki aktywności]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[Przegląd dzienników diagnostycznych]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[Przegląd alertów]: ../monitoring-and-diagnostics/insights-alerts-portal.md



[metrics-blade]: ./media/api-management-azure-monitor/api-management-metrics-blade.png
[activity-logs-blade]: ./media/api-management-azure-monitor/api-management-activity-logs-blade.png
[alert-rules-blade]: ./media/api-management-azure-monitor/api-management-alert-rules-blade.png
[diagnostic-logs-blade]: ./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png
