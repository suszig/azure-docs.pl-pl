---
title: "Przy użyciu pakietu OMS dziennika analizy alertu REST API"
description: "Dziennik analizy alertu interfejsu API REST umożliwia tworzenie i Zarządzanie alertami w analizy dzienników, która jest częścią z Operations Management Suite (OMS).  Ten artykuł zawiera szczegółowe informacje o interfejsie API i kilka przykładów do wykonywania różnych operacji."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 628ad256-7181-4a0d-9e68-4ed60c0f3f04
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5ce72ffef4394bf3bbe39fa420c4fcaa965ae35c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a>Tworzenie i zarządzanie nimi reguły alertów w analizy dzienników z interfejsu API REST
Dziennik analizy alertu interfejsu API REST umożliwia tworzenie i Zarządzanie alertami w Operations Management Suite (OMS).  Ten artykuł zawiera szczegółowe informacje o interfejsie API i kilka przykładów do wykonywania różnych operacji.

Interfejsu API REST Search analizy dziennika jest RESTful i jest dostępny za pośrednictwem interfejsu REST API usługi Azure Resource Manager. W tym dokumencie można znaleźć przykłady którym dostęp do interfejsu API z wiersza polecenia programu PowerShell przy użyciu [ARMClient](https://github.com/projectkudu/ARMClient), narzędzie wiersza polecenia typu open source, który upraszcza wywoływanie interfejsu API Azure Resource Manager. Korzystanie z programu PowerShell i ARMClient jest jedną z wielu opcji, aby uzyskać dostęp do interfejsu API Search analizy dziennika. Te narzędzia mogą korzystać z RESTful interfejs API Menedżera zasobów Azure do wykonywania wywołań do obszarów roboczych OMS i wykonywania poleceń wyszukiwania w nich. Interfejs API dane wyjściowe obejmują wyniki wyszukiwania do użytkownika w formacie JSON, co umożliwia używanie wyniki wyszukiwania na wiele różnych sposobów programowo.

## <a name="prerequisites"></a>Wymagania wstępne
Obecnie usługa alertów można tworzyć tylko z zapisanej operacji wyszukiwania w analizy dzienników.  Można to sprawdzić [interfejsu API REST wyszukiwania dziennika](log-analytics-log-search-api.md) Aby uzyskać więcej informacji.

## <a name="schedules"></a>Harmonogramy
Zapisane wyszukiwanie może mieć co najmniej jeden harmonogram. Harmonogram Określa, jak często wyszukiwania jest uruchamiania i przedział czasu, w którym określono kryteria.
Harmonogramy mają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--- |:--- |
| Interwał |Jak często jest wykonywane wyszukiwania. (W minutach). |
| QueryTimeSpan |Przedział czasu, w którym jest oceniana kryteria. Musi być równa lub większa od wartości interwału. (W minutach). |
| Wersja |Używana wersja interfejsu API.  Obecnie ta powinna zawsze być ustawiona na 1. |

Rozważmy na przykład kwerendy zdarzeń z interwałem 15 minut i zakres czasu 30 minut. W takim przypadku zapytanie zostanie uruchomione co 15 minut, a alert będzie uruchomiony, jeśli kryteria nadal rozpoznać na wartość true w zakres 30 minut.

### <a name="retrieving-schedules"></a>Trwa pobieranie harmonogramów
Pobierz wszystkie harmonogramy dla zapisanego wyszukiwania za pomocą metody Get.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Pobierz konkretny harmonogram dla zapisanego wyszukiwania za pomocą metody Get o identyfikatorze harmonogramu.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Poniżej znajduje się przykładowa odpowiedź dla harmonogramu.

```json
{
    "value": [{
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
            "Interval": 15,
            "QueryTimeSpan": 15
        }
    }]
}
```

### <a name="creating-a-schedule"></a>Tworzenie harmonogramu
Za pomocą metody Put identyfikator unikatowy harmonogram do utworzenia nowego harmonogramu.  Należy zauważyć, że dwa harmonogramy nie może sam identyfikator nawet, jeśli są one powiązane z różnymi zapisane wyszukiwania.  Po utworzeniu harmonogramu w konsoli OMS identyfikator GUID jest tworzona dla identyfikator harmonogramu.

> [!NOTE]
> Nazwa dla wszystkich zapisanych wyszukiwań, harmonogramów i akcje utworzone przy użyciu interfejsu API analizy dziennika musi być pisane małymi literami.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Edytowanie harmonogramu
Za pomocą metody Put istniejący identyfikator harmonogramu dla tego samego zapisanego wyszukiwania można zmodyfikować tego harmonogramu.  Treść żądania musi zawierać element etag harmonogramu.

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Usuwanie harmonogramów
Za pomocą metody Delete identyfikator harmonogramu. Aby usunąć harmonogram.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Akcje
Harmonogram może mieć wiele akcji. Akcja może określić jeden lub więcej procesów do wykonywania takich jak wysyłanie wiadomości e-mail lub uruchamianie elementu runbook lub mogą określić próg określający, jeśli wyniki wyszukiwania zgodny pewne kryteria.  Niektóre akcje definiują zarówno tak, aby procesy są wykonywane po spełnieniu wartość progową.

Wszystkie działania mają właściwości w poniższej tabeli.  Różne typy alertów mają różne dodatkowe właściwości, które są opisane poniżej.

| Właściwość | Opis |
|:--- |:--- |
| Typ |Typ akcji.  Obecnie możliwe wartości to Alert i elementu Webhook. |
| Nazwa |Nazwa wyświetlana alertu. |
| Wersja |Używana wersja interfejsu API.  Obecnie ta powinna zawsze być ustawiona na 1. |

### <a name="retrieving-actions"></a>Pobieranie akcji
Pobierz wszystkie akcje dla harmonogramu za pomocą metody Get.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Za pomocą metody Get identyfikator akcji można pobrać określonej akcji dla harmonogramu.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Tworzenie lub edytowanie akcji
Za pomocą metody Put identyfikator akcji, która jest unikatowa w harmonogramie, aby utworzyć nową akcję.  Po utworzeniu akcji w konsoli OMS identyfikator GUID jest identyfikatora akcji.

> [!NOTE]
> Nazwa dla wszystkich zapisanych wyszukiwań, harmonogramów i akcje utworzone przy użyciu interfejsu API analizy dziennika musi być pisane małymi literami.

Za pomocą metody Put istniejącego Identyfikatora akcji dla tego samego zapisanego wyszukiwania można zmodyfikować tego harmonogramu.  Treść żądania musi zawierać element etag harmonogramu.

Format żądania do tworzenia nowej akcji zależy od typu akcji, te przykłady znajdują się w poniższych sekcjach.

### <a name="deleting-actions"></a>Usuwanie akcji
Za pomocą metody Delete identyfikator akcji można usunąć akcji.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Akcje alertu
Harmonogram ma jeden i tylko jeden Alert akcji.  Akcje alertu występuje jeden lub kilka z sekcji w poniższej tabeli.  Każdy jest opisany bardziej szczegółowo poniżej.

| Sekcja | Opis |
|:--- |:--- |
| Próg |Kryteria po uruchomieniu akcji. |
| EmailNotification |Wysyłanie poczty do wielu adresatów. |
| Korygowania |Uruchom element runbook automatyzacji Azure, aby spróbować rozwiązać problem zidentyfikowane. |

#### <a name="thresholds"></a>Progi
Akcja alertu powinna mieć jeden i tylko jeden próg.  Jeśli wyniki zapisanego kryterium wyszukiwania zgodne próg w akcji skojarzonej z tym wyszukiwania, są uruchamiane żadnych innych procesów w tej akcji.  Akcja może również zawierać tylko wartości progowej, aby mogą być używane z akcjami innych typów, które nie zawierają wartości progowe.

Progi mają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--- |:--- |
| Operator |Operator porównania wartości progowej. <br> gt = większe niż <br> lt = mniej niż |
| Wartość |Wartość progu. |

Rozważmy na przykład kwerendy interwał 15 minut, Timespan 30 minut, a próg większym niż 10. W takim przypadku zapytanie zostanie uruchomione co 15 minut, a alert będzie uruchomiony, jeśli zwróceniem 10 zdarzeń, które zostały utworzone w danym okresie 30 minut.

Poniżej znajduje się przykładowa odpowiedź dla akcji z tylko wartości progowej.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Za pomocą metody Put identyfikator unikatowy akcji do utworzenia nowej akcji próg dla harmonogramu.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Za pomocą metody Put istniejącego Identyfikatora akcji można zmodyfikować akcję próg dla harmonogramu.  Treść żądania musi zawierać element etag akcji.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>Powiadomienia e-mail
Powiadomienia e-mail wysyłanie poczty do co najmniej jednego adresata.  Obejmują one właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--- |:--- |
| Adresaci |Lista adresów e-mail. |
| Temat |Temat wiadomości e-mail. |
| Załącznika |Załączniki nie są obecnie obsługiwane, dlatego to zawsze będzie mieć wartość "None". |

Poniżej znajduje się przykładowa odpowiedź dla akcji powiadomienia e-mail o progu.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Za pomocą metody Put identyfikator unikatowy akcji można utworzyć nowej wiadomości e-mail akcji dla harmonogramu.  Poniższy przykład tworzy wiadomość e-mail z powiadomieniem o progu, więc poczty jest wysyłany, gdy wyniki wyszukiwania przekracza wartość progową.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

Aby zmodyfikować działanie poczty e-mail dla harmonogramu, należy użyć metody Put z istniejącego Identyfikatora akcji.  Treść żądania musi zawierać element etag akcji.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a>Akcji korygowania
Korygowaniami uruchomienia elementu runbook w automatyzacji Azure, które próbuje naprawić problem identyfikowana na podstawie alertu.  Należy utworzyć elementu webhook dla elementu runbook, używany w Akcja korygowania i następnie określ identyfikator URI we właściwości WebhookUri.  Podczas tworzenia tej akcji, używając konsoli OMS nowego elementu webhook jest tworzona automatycznie dla elementu runbook.

Korygowaniami obejmują właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--- |:--- |
| RunbookName |Nazwa elementu runbook. To musi być zgodna opublikowanego elementu runbook w ramach konta automatyzacji skonfigurowane w rozwiązaniu automatyzacji w obszarze roboczym pakietu OMS. |
| WebhookUri |Identyfikator URI elementu webhook. |
| Wygaśnięcia |Data wygaśnięcia i godzina elementu webhook.  Jeśli elementu webhook nie ma wygaśnięcia, może to być wszystkie prawidłową datą przyszłą. |

Poniżej znajduje się przykładowa odpowiedź dla akcji korygowania o progu.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Za pomocą metody Put identyfikator unikatowy akcji do utworzenia nowej akcji korygowania harmonogramu.  Poniższy przykład tworzy korygowania o progu, aby uruchomić elementu runbook, gdy wyniki wyszukiwania przekracza wartość progową.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Za pomocą metody Put istniejącego Identyfikatora akcji można zmodyfikować akcji korygowania, harmonogramu.  Treść żądania musi zawierać element etag akcji.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Przykład
Poniżej znajduje się pełny przykład można utworzyć nowy alert e-mail.  Spowoduje to utworzenie nowego harmonogramu wraz z akcji zawierający próg i poczty e-mail.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Akcje elementu Webhook
Akcje elementu Webhook uruchomienia procesu podczas wywoływania adresu URL i opcjonalnie podając ładunku do wysłania.  Są one podobne do akcji korygowania, z wyjątkiem są przeznaczone dla elementów webhook, które może wywołać procesy inne niż elementy runbook automatyzacji Azure.  Zawierają także dodatkowe opcja podania ładunku mają zostać dostarczone do zdalnego procesu.

Akcje elementu Webhook nie mają tego progu, ale zamiast tego powinni zostać dodani do harmonogram, który ma akcję alertu o progu.  Można dodawać wiele akcji elementu Webhook, które zostaną wszystkie uruchomione po spełnieniu wartość progową.

Akcje elementu Webhook obejmują właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--- |:--- |
| WebhookUri |Temat wiadomości e-mail. |
| CustomPayload |Niestandardowy ładunek do wysłania do elementu webhook.  Format będzie zależeć od tego, czego oczekuje elementu webhook. |

Poniżej znajduje się przykładowa odpowiedź dla elementu webhook akcji i skojarzonych alertach o progu.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Utwórz lub Edytuj akcji elementu webhook
Za pomocą metody Put identyfikator unikatowy akcji do utworzenia nowej akcji elementu webhook dla harmonogramu.  Poniższy przykład tworzy akcji elementu Webhook i alertów o progu, dzięki czemu elementu webhook będą wyzwalane, gdy wyniki wyszukiwania przekracza wartość progową.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Za pomocą metody Put istniejącego Identyfikatora akcji można zmodyfikować akcję elementu webhook dla harmonogramu.  Treść żądania musi zawierać element etag akcji.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Następne kroki
* Użyj [interfejsu API REST do wyszukiwania dziennika](log-analytics-log-search-api.md) w analizy dzienników.

