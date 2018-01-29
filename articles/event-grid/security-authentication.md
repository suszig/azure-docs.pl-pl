---
title: "Azure siatki zdarzeń zabezpieczeń i uwierzytelniania"
description: "Opisuje Azure zdarzeń siatki i jego pojęcia."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 09/18/2017
ms.author: babanisa
ms.openlocfilehash: 510e578f3ebeb6ad7a4d81249cdfd7ce1d3684ad
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="event-grid-security-and-authentication"></a>Zdarzenie siatki zabezpieczeń i uwierzytelniania 

Azure siatki zdarzeń ma trzy typy uwierzytelniania:

* Subskrypcja zdarzeń
* Publikowanie zdarzenia
* Dostarczania zdarzeń elementu WebHook

## <a name="webhook-event-delivery"></a>Dostarczania zdarzeń elementu WebHook

Element Webhook ma jeden wiele sposobów, aby odbierać zdarzenia w czasie rzeczywistym z siatki zdarzeń platformy Azure. Za każdym razem, gdy jest gotowe do dostarczenia nowego zdarzenia, elementu Webhook siatki zdarzeń wysyła żądanie HTTP do skonfigurowanego punktu końcowego HTTP ze zdarzeniem w treści.

Po zarejestrowaniu własny punkt końcowy elementu WebHook siatki zdarzeń wysyła możesz żądania POST z kodem poprawności w celu potwierdzenia własność punktu końcowego. Twoja aplikacja powinna odpowiadać za wstecz wyświetlania kodu walidacji. Siatka zdarzeń nie dostarcza zdarzeń do elementu WebHook punktów końcowych, które nie przeszły sprawdzanie poprawności.

### <a name="validation-details"></a>Szczegóły weryfikacji

* W momencie aktualizacja tworzenia subskrypcji zdarzeń siatki zdarzeń zapisuje zdarzenie "SubscriptionValidationEvent" do docelowego punktu końcowego.
* Zdarzenie zawiera wartość nagłówka "SubscriptionValidation Æg zdarzeń typu:".
* Treści zdarzenia ma ten sam schemat jako inne zdarzenia, zdarzenia siatki.
* Dane zdarzenia zawiera właściwość "validationCode" z ciągiem losowo wygenerowany. Na przykład "validationCode: acb13...".

Przykład SubscriptionValidationEvent przedstawiono w poniższym przykładzie:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2017-08-06T22:09:30.740323Z"
}]
```

Aby zweryfikować własność punktu końcowego, odsyłania kodu walidacji we właściwości validationResponse, jak pokazano w poniższym przykładzie:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Na koniec jest należy pamiętać, że siatki zdarzeń Azure obsługuje tylko punktów końcowych HTTPS elementu webhook.

## <a name="event-subscription"></a>Subskrypcja zdarzeń

Aby subskrybować zdarzenia, musisz mieć **Microsoft.EventGrid/EventSubscriptions/Write** uprawnienia wymaganego zasobu. To uprawnienie jest konieczne, ponieważ pisania nową subskrypcję w zakresie zasobów. Wymagany zasób różni się w zależności od tego, czy są subskrybowanie tematu system lub niestandardowego tematu. Oba typy są opisane w tej sekcji.

### <a name="system-topics-azure-service-publishers"></a>Tematy systemu (usługa Azure wydawców)

Tematy systemu potrzebne uprawnienia do zapisu w nowej subskrypcji zdarzeń w zakresie zasobów publikowania zdarzenia. Format zasobu jest:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Na przykład, aby subskrybować zdarzenia na konto magazynu o nazwie **myacct**, musisz mieć uprawnienia Microsoft.EventGrid/EventSubscriptions/Write na:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Niestandardowe — tematy

Tematy niestandardowe należy uprawnienia do zapisu w nowej subskrypcji zdarzeń w zakresie tematu zdarzeń siatki. Format zasobu jest:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Na przykład, aby zasubskrybować niestandardowego tematu o nazwie **mytopic**, musisz mieć uprawnienia Microsoft.EventGrid/EventSubscriptions/Write na:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="topic-publishing"></a>Publikowanie w temacie

Tematy za pomocą dostępu sygnatury dostępu Współdzielonego lub uwierzytelniania opartego na kluczu. Firma Microsoft zaleca SAS, ale uwierzytelniania opartego na kluczu udostępnia prosty programowania i jest zgodny z wielu istniejących wydawców elementu webhook. 

Wartość uwierzytelniania możesz uwzględnić w nagłówku HTTP. Sygnatury dostępu Współdzielonego, można użyć **Æg sas token** wartości nagłówka. Dla uwierzytelniania opartego na kluczu, użyj **Æg sas klucza** wartości nagłówka.

### <a name="key-authentication"></a>Uwierzytelnianie za pomocą klucza

Uwierzytelnianie za pomocą klucza jest to najprostsza forma uwierzytelniania. Użyj następującego formatu:`aeg-sas-key: <your key>`

Na przykład można przekazać klucza z:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>Tokeny sygnatur dostępu współdzielonego

Tokeny sygnatury dostępu Współdzielonego dla zdarzeń siatki obejmują zasobu czas wygaśnięcia i sygnaturę. Format tokenu sygnatury dostępu Współdzielonego: `r={resource}&e={expiration}&s={signature}`.

Zasób jest ścieżką tematu, do którego są wysyłane zdarzenia. Na przykład ścieżka prawidłowego zasobu to:`https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

Podpis jest generowanie z klucza.

Na przykład prawidłowy **Æg sas token** wartość to:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

Poniższy przykład tworzy token sygnatury dostępu Współdzielonego do użytku z siatki zdarzeń:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    var culture = CultureInfo.CreateSpecificCulture("en-US");
    var encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString(culture));

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a>Kontrola dostępu administracyjnego

Azure siatki zdarzeń umożliwia kontrolę dostępu do różnych użytkowników w różnych operacji zarządzania, takich jak listy zdarzeń subskrypcji, tworzenie nowych i generowania kluczy. Siatka zdarzeń korzysta z platformy Azure na podstawie ról dostępu Sprawdź (RBAC).

### <a name="operation-types"></a>Typy operacji

Siatka zdarzeń platformy Azure obsługuje następujące akcje:

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

Ostatnie trzy operacje zwracać potencjalnie poufne informacje, które pobiera przefiltrowane z normalnych operacji odczytu. Jest najlepszym rozwiązaniem, można ograniczyć dostęp do tych operacji. Role niestandardowe można tworzyć przy użyciu [programu Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [Azure interfejsu wiersza polecenia (CLI)](../active-directory/role-based-access-control-manage-access-azure-cli.md)i [interfejsu API REST](../active-directory/role-based-access-control-manage-access-rest.md).

### <a name="enforcing-role-based-access-check-rbac"></a>Wymuszanie roli na podstawie sprawdzanie dostępu (RBAC)

Aby wymusić RBAC dla różnych użytkowników, wykonaj następujące kroki:

#### <a name="create-a-custom-role-definition-file-json"></a>Utwórz plik definicji niestandardowej roli zabezpieczeń (JSON)

Poniżej przedstawiono przykładowe definicje ról siatki zdarzeń, umożliwiające użytkownikom do wykonywania różnych zestawów działań.

**EventGridReadOnlyRole.json**: Zezwalaj tylko na operacje tylko do odczytu.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.json**: Zezwalaj na akcje po ograniczone, ale nie zezwalaj akcje usuwania.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.json**: umożliwia wszystkie akcje siatki zdarzeń.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

#### <a name="create-and-assign-custom-role-with-azure-cli"></a>Utwórz i przypisz niestandardowej roli zabezpieczeń z wiersza polecenia platformy Azure

Aby utworzyć niestandardową rolę, należy użyć:

```azurecli
az role definition create --role-definition @<file path>
```

Aby przypisać rolę do użytkownika, należy użyć:

```azurecli
az role assignment create --assignee <user name> --role "<name of role>"
```

## <a name="next-steps"></a>Kolejne kroki

* Aby obejrzeć wprowadzenie do siatki zdarzeń, zobacz [o siatki zdarzeń](overview.md)
