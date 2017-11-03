---
title: "Usługa Azure Service Bus zawiesić jednostek obsługi komunikatów | Dokumentacja firmy Microsoft"
description: "Wstrzymywanie i ponowne uaktywnianie jednostki komunikatów usługi Azure Service Bus."
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2017
ms.author: sethm
ms.openlocfilehash: 09aee1ba9ec3ce72732cb1f60c9a840ffc4beb2f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Wstrzymywanie i ponownie uaktywnić jednostek obsługi komunikatów (wyłączone)

Kolejki, tematy i subskrypcje może być tymczasowo wstrzymane. Zawieszenie umieszcza jednostki w stanie wyłączenia, w którym wszystkie wiadomości są zachowywane w magazynie. Jednak wiadomości nie może być usunięte lub dodane, a operacje odpowiednich protokołu yield błędy.

Wstrzymywanie jednostki jest zazwyczaj wykonywane pilnych powodów. Jednym ze scenariuszy o wdrożeniu odbiornik uszkodzony pobierającej komunikatów kolejki, niepowodzenia przetwarzania, jeszcze niepoprawnie zakończeniu wiadomości i usuwa je. Jeśli to zachowanie jest zdiagnozowaniu, kolejki można wyłączyć dla odbiera dopóki poprawiony kod nie zostanie wdrożony i dodatkowo można zapobiec utraty danych spowodowanej nieprawidłowy kod.

Zawieszenie lub ponownej aktywacji mogą być wykonywane przez użytkownika lub przez system. System zawiesza tylko jednostek poważne przyczyn administracyjnych na przykład naciśnięcie subskrypcji limit wydatków. Wyłączone systemu jednostek nie można ponownie uaktywnić przez użytkownika, ale zostaną przywrócone, gdy został rozwiązany przyczynę zawieszenia.

W portalu **właściwości** sekcji odpowiednich jednostek umożliwia zmianę stanu; Poniższy zrzut ekranu przedstawia Przełącz kolejki:

![][1]

Portalu zezwala tylko na całkowite wyłączenie kolejek. Można również wyłączyć wysyłania i odbierania operacji oddzielnie za pomocą usługi Service Bus [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) interfejsu API w .NET Framework SDK lub za pomocą szablonu usługi Azure Resource Manager za pomocą wiersza polecenia platformy Azure lub programu Azure PowerShell.

## <a name="suspension-states"></a>Stany zawieszenia

Dostępne są następujące stany, które można ustawić dla kolejki:

-   **Aktywne**: kolejki jest aktywny.
-   **Wyłączone**: kolejki jest wstrzymana.
-   **SendDisabled**: kolejki częściowo jest wstrzymana z receive jest dozwolone.
-   **ReceiveDisabled**: kolejki częściowo jest wstrzymana z send jest dozwolone.

Dla subskrypcji i tematy, tylko **Active** i **wyłączone** można ustawić.

[Wymiary modułów EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) wyliczenie definiuje również zestawu przejściowego stanów, które można ustawić tylko przez system. W poniższym przykładzie przedstawiono polecenia programu PowerShell, aby wyłączyć kolejki. Polecenie ponownej aktywacji jest równoważne ustawienie `Status` do **Active**.

```powershell
$q = Get-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o komunikatów usługi Service Bus, zobacz następujące tematy:

* [Podstawy usługi Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Kolejki, tematy i subskrypcje usługi Service Bus](service-bus-queues-topics-subscriptions.md)
* [Wprowadzenie do kolejek usługi Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak używać tematów i subskrypcji usługi Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/queue-disable.png

