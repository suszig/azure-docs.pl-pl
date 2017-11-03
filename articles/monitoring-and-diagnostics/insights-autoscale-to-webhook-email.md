---
title: "Użyj akcji skalowania automatycznego do wysyłania wiadomości e-mail i elementu webhook powiadomień o alertach. | Microsoft Docs"
description: "Zobacz temat jak korzystać z akcji skalowania automatycznego połączeń telefonicznych z adresu URL sieci web lub wysyłania powiadomień e-mail w monitorze Azure. "
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: eb9a4c98-0894-488c-8ee8-5df0065d094f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: ancav
ms.openlocfilehash: 16caf14028494800e9259f0296c292b606d0210a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Umożliwia wysyłanie poczty e-mail i elementu webhook powiadomień o alertach w monitorze Azure akcji skalowania automatycznego
W tym artykule opisano, jak skonfigurowaniu wyzwalaczy tak, aby można było wywołać adresu URL sieci web określonego lub wysyłania wiadomości e-mail w oparciu akcji skalowania automatycznego na platformie Azure.  

## <a name="webhooks"></a>elementów webhook
Elementów Webhook pozwalają kierować Azure powiadomień o alertach do innych systemów przetwarzania końcowego lub niestandardowych powiadomień. Na przykład routingu alertu do usług, które może obsłużyć przychodzącego żądania sieci web wysyłania SMS, dziennik błędów, powiadomi zespołu za pomocą rozmowy i wiadomości usługi itp. Identyfikator URI elementu webhook musi być prawidłowy punkt końcowy HTTP lub HTTPS.

## <a name="email"></a>Adres e-mail
Może być wysyłana wiadomość e-mail do dowolnego adresu e-mail. Administratorzy i współadministratorzy subskrypcji, w którym jest uruchomiona reguła również zostanie powiadomiony.

## <a name="cloud-services-and-web-apps"></a>Usługi w chmurze i aplikacje sieci Web
Użytkownik może zdecydować się na z portalu Azure dla farmy serwerów (aplikacje sieci Web) i usług w chmurze.

* Wybierz **skalowanie przez** metryki.

![Skalowanie przez](./media/insights-autoscale-to-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a>Zestawy skalowania maszyny wirtualnej
W przypadku nowszych maszyn wirtualnych utworzonych za pomocą Menedżera zasobów (zestawy skalowania maszyny wirtualnej) można skonfigurować to przy użyciu interfejsu API REST, szablony usługi Resource Manager, programu PowerShell i interfejsu wiersza polecenia. Interfejs portalu nie jest jeszcze dostępna.
Korzystając z interfejsu API REST lub Menedżera zasobów szablonu, obejmują elementu powiadomienia o następujących opcji.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
| Pole | Obowiązkowe? | Opis |
| --- | --- | --- |
| Operacja |tak |Wartość musi być "Skala" |
| sendToSubscriptionAdministrator |tak |Wartość musi być "prawda" lub "false" |
| sendToSubscriptionCoAdministrators |tak |Wartość musi być "prawda" lub "false" |
| customEmails |tak |wartość może być null [] lub tablicy ciągów wiadomości e-mail |
| elementów webhook |tak |wartość może być zerowy lub nieprawidłowy identyfikator Uri |
| serviceUri |tak |Nieprawidłowy identyfikator Uri protokołu https |
| properties |tak |Wartość musi być pusty {} lub może zawierać pary klucz wartość |

## <a name="authentication-in-webhooks"></a>Uwierzytelnianie w elementów webhook
Elementu webhook uwierzytelniania z użyciem uwierzytelniania opartego na tokenie, w którym zapisać elementu webhook identyfikatora URI identyfikatorem token jako parametr zapytania. Na przykład https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue

## <a name="autoscale-notification-webhook-payload-schema"></a>Funkcja automatycznego skalowania powiadomień elementu webhook ładunku schematu
Podczas generowania powiadomień skalowania automatycznego następujące metadane jest uwzględnione w ładunku elementu webhook:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


| Pole | Obowiązkowe? | Opis |
| --- | --- | --- |
| status |tak |Stan wskazujący, że wygenerowano akcji skalowania automatycznego |
| Operacja |tak |Zwiększanie wystąpień będzie "Skaluj w poziomie" i w przypadku spadku wystąpień będzie "W skali" |
| Kontekst |tak |Kontekst akcji skalowania automatycznego |
| sygnatura czasowa |tak |Sygnatura czasowa wyzwolenia akcji skalowania automatycznego |
| id |Tak |Identyfikator Menedżera zasobów w ustawieniu skalowania automatycznego |
| name |Tak |Nazwa ustawienia skalowania automatycznego |
| Szczegóły |Tak |Opis akcji, która miała usługi skalowania automatycznego i zmianę liczby wystąpień |
| subscriptionId |Tak |Identyfikator zasobu docelowego, na którym wykonywane jest skalowanie subskrypcji |
| Grupy zasobów o nazwie |Tak |Nazwa grupy zasobów zasobu docelowego, na którym wykonywane jest skalowanie |
| resourceName |Tak |Nazwa zasobu docelowego, na którym wykonywane jest skalowanie |
| Typ zasobu |Tak |Trzy obsługiwane wartości: "microsoft.classiccompute/domainnames/slots/roles" - role usługi w chmurze, "microsoft.compute/virtualmachinescalesets" - zestawy skalowania maszyny wirtualnej i "Microsoft.Web/serverfarms" - aplikacji sieci Web |
| resourceId |Tak |Identyfikator zasobu docelowego, na którym wykonywane jest skalowanie usługi Resource Manager |
| portalLink |Tak |Azure portalu łącze na stronie Podsumowanie zasób docelowy |
| oldCapacity |Tak |Bieżąca (stare) liczbę wystąpień podczas skalowania automatycznego trwało akcji skalowania |
| newCapacity |Tak |Nowe liczba wystąpień skalowania automatycznego skalowania zasobu |
| Właściwości |Nie |Opcjonalny. Zbiór < klucz, wartość > pary (na przykład słownika < String, String >). Pole właściwości jest opcjonalne. W niestandardowego interfejsu użytkownika lub przepływu pracy na podstawie aplikacji logiki można wprowadzić klucze i wartości, które mogą zostać przekazane za pomocą ładunku. Alternatywny sposób, aby przekazywać właściwości niestandardowe wychodzących wywołanie elementu webhook jest użycie elementu webhook identyfikatora URI się (jako parametry kwerendy) |
