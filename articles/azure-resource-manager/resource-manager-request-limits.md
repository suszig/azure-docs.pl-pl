---
title: "Limity żądań usługi Azure Resource Manager | Dokumentacja firmy Microsoft"
description: "Informacje dotyczące używania przepustowości z żądaniami usługi Azure Resource Manager po osiągnięciu limitu subskrypcji."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/26/2018
ms.author: tomfitz
ms.openlocfilehash: dc109cdaeade900e239624f408cea2a1f448ae5a
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="throttling-resource-manager-requests"></a>Ograniczanie żądań Resource Manager
Dla każdej subskrypcji i dzierżawy Resource Manager limity żądań do 15 000 na godzinę do odczytu i zapisu żądania 1200 na godzinę. Te limity mają zastosowanie do każdego wystąpienia usługi Azure Resource Manager. Istnieje wiele wystąpień w każdym regionie Azure i wdrożeniu usługi Azure Resource Manager wszystkie regiony platformy Azure.  Tak, w praktyce skutecznie znacznie wyższa niż te limity są limity, jako użytkownik żądań są zazwyczaj obsługiwane przez wiele różnych wystąpień.

Jeśli aplikacji lub skryptu osiągnie tych ograniczeń, należy do ograniczania własnych żądań. W tym artykule przedstawiono sposób określania pozostałych żądań, które mają przed przekroczeniem limitu oraz sposobu odpowiedzi po osiągnięciu limitu.

Po osiągnięciu limitu, otrzymasz kod stanu HTTP **429 zbyt wiele żądań**.

Liczba żądań, które obejmuje subskrypcję lub dzierżawy. Jeśli masz wiele równoczesnych aplikacje wysyłania żądań w ramach subskrypcji, żądania z tych aplikacji są dodawane ze sobą w celu określenia liczby pozostałych żądań.

Subskrypcja zakres żądania są te dotyczą przekazywanie subskrypcji IDENTYFIKATORA, takie jak pobieranie grup zasobów w ramach subskrypcji. Żądania dzierżawy w zakresie nie ma identyfikator subskrypcji, takie jak pobieranie prawidłowych lokalizacji platformy Azure.

## <a name="remaining-requests"></a>Pozostałych żądań
Liczba pozostałych żądań, które można określić, sprawdzając nagłówków odpowiedzi. Każde żądanie zawiera wartości dla wielu żądań zapisu i odczytu pozostałych. W poniższej tabeli opisano nagłówki odpowiedzi, który można sprawdzić dla tych wartości:

| Nagłówek odpowiedzi | Opis |
| --- | --- |
| x-ms-ratelimit-remaining-subscription-reads |Odczytuje subskrypcji zakres pozostałych |
| x-ms-ratelimit-remaining-subscription-writes |Zapisuje zakres subskrypcji pozostałych |
| x-ms-ratelimit-remaining-tenant-reads |Zakres dzierżawy odczytuje pozostałych |
| x-ms-ratelimit-remaining-tenant-writes |Zapisuje zakres dzierżawy pozostałych |
| x-ms-ratelimit-remaining-subscription-resource-requests |Subskrypcja zakresu pozostałych żądań typu zasobu.<br /><br />Wartość tego nagłówka jest zwracany tylko wtedy, jeśli usługa ma zastąpić domyślny limit. Menedżer zasobów dodaje tej wartości, zamiast subskrypcji odczytów i zapisów. |
| x-ms-ratelimit-remaining-subscription-resource-entities-read |Subskrypcja zakresu pozostałych żądania kolekcji dla typu zasobu.<br /><br />Wartość tego nagłówka jest zwracany tylko wtedy, jeśli usługa ma zastąpić domyślny limit. Ta wartość określa liczbę pozostałych żądań kolekcji (listy zasobów). |
| x-ms-ratelimit-remaining-tenant-resource-requests |Zakres dzierżawy pozostałych żądań typu zasobu.<br /><br />Ten nagłówek jest dodawać tylko dla żądań na poziomie dzierżawy, a tylko wtedy, gdy usługa ma zastąpić domyślny limit. Menedżer zasobów dodaje tej wartości, zamiast dzierżawy odczytów i zapisów. |
| x-ms-ratelimit-remaining-tenant-resource-entities-read |Dzierżawy zakresu pozostałych żądania kolekcji dla typu zasobu.<br /><br />Ten nagłówek jest dodawać tylko dla żądań na poziomie dzierżawy, a tylko wtedy, gdy usługa ma zastąpić domyślny limit. |

## <a name="retrieving-the-header-values"></a>Pobieranie wartości nagłówka
Nie różni się od pobierania wartości nagłówka się podczas pobierania tych wartości nagłówka w tym kod lub skrypt. 

Na przykład w **C#**, możesz pobrać wartość nagłówka z **HttpWebResponse** obiektu o nazwie **odpowiedzi** następującym kodem:

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

W **PowerShell**, pobrać wartość nagłówka podczas operacji Invoke WebRequest.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Lub, jeśli chcesz zobaczyć pozostałych żądań do debugowania, możesz podać **-Debug** parametru na Twojej **środowiska PowerShell** polecenia cmdlet.

```powershell
Get-AzureRmResourceGroup -Debug
```

Która zwraca wiele wartości, z uwzględnieniem następujących wartości odpowiedzi:

```powershell
...
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 14999
...
```

W **interfejsu wiersza polecenia Azure**, pobrać wartość nagłówka przy użyciu opcji na pełniejsze.

```azurecli
az group list --verbose --debug
```

Która zwraca wiele wartości, w tym następujący obiekt:

```azurecli
...
silly: returnObject
{
  "statusCode": 200,
  "header": {
    "cache-control": "no-cache",
    "pragma": "no-cache",
    "content-type": "application/json; charset=utf-8",
    "expires": "-1",
    "x-ms-ratelimit-remaining-subscription-reads": "14998",
    ...
```

## <a name="waiting-before-sending-next-request"></a>Oczekiwania przed wysłaniem żądania dalej
Po osiągnięciu limitu żądań, Menedżer zasobów zwraca **429** kod stanu HTTP i **ponownych prób po** wartość nagłówka. **Ponownych prób po** wartość określa liczbę sekund oczekiwania w aplikacji (lub uśpienia) przed wysłaniem następnego żądania. Po wysłaniu żądania, przed upływem wartość ponownych prób, Twoje żądanie nie jest przetwarzane i nowa wartość ponownych prób jest zwracany.

## <a name="next-steps"></a>Kolejne kroki

* Aby uzyskać więcej informacji na temat limitów i przydziały zobacz [subskrypcji platformy Azure i usługi limity, przydziały i ograniczenia](../azure-subscription-service-limits.md).
* Informacje na temat obsługi żądań asynchronicznych REST, zobacz [śledzić operacje asynchroniczne Azure](resource-manager-async-operations.md).
