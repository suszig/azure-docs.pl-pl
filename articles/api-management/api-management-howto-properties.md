---
title: "Sposób użycia właściwości w ramach zasad usługi Azure API Management"
description: "Dowiedz się, jak używać właściwości w ramach zasad usługi Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 6f39b00f-cf6e-4cef-9bf2-1f89202c0bc0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 517f434c8f7fabc1402fb938d5ff5c733b86f2fd
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Sposób użycia właściwości w ramach zasad usługi Azure API Management
Zarządzanie interfejsami API zasady są zaawansowanych możliwości systemu, który umożliwia wydawcy, aby zmienić zachowanie interfejsu API za pomocą konfiguracji. Zasady to zbiór instrukcji, które są wykonywane sekwencyjnie podczas żądania lub odpowiedzi interfejsu API. Deklaracji zasad może być skonstruowany przy użyciu wartości tekstowe literału, wyrażenie zasad i właściwości. 

Każde wystąpienie usługi Zarządzanie interfejsami API ma zbiór właściwości pary klucz wartość, które są globalne do wystąpienia usługi. Te właściwości mogą służyć do zarządzania stałych wartości ciągów we wszystkich Konfiguracja interfejsu API i zasady. Każda właściwość ma następujące atrybuty.

| Atrybut | Typ | Opis |
| --- | --- | --- |
| Nazwa |Ciąg |Nazwa właściwości. Może zawierać tylko litery, cyfry, okres, łączniki i znaki podkreślenia. |
| Wartość |Ciąg |Wartość właściwości. Nie może być pusta ani zawierać tylko odstępu. |
| Wpis tajny |Wartość logiczna |Określa, czy wartość jest klucz tajny i powinny być szyfrowane, czy nie. |
| Tagi |Tablica ciągów |Opcjonalnie tagi, które udostępniane może służyć do filtrowania listy właściwości. |

Właściwości są skonfigurowane w portalu wydawcy na **właściwości** kartę. W poniższym przykładzie trzy właściwości są skonfigurowane.

![Właściwości][api-management-properties]

Wartości właściwości mogą zawierać ciągi literału i [wyrażenie zasad](https://msdn.microsoft.com/library/azure/dn910913.aspx). W poniższej tabeli przedstawiono poprzednie trzy przykładowe właściwości i ich atrybutów. Wartość `ExpressionProperty` jest wyrażenie zasad, która zwraca ciąg zawierający bieżącą datę i godzinę. Właściwość `ContosoHeaderValue` jest oznaczona jako klucz tajny, więc jego wartość nie jest wyświetlana.

| Nazwa | Wartość | Wpis tajny | Tagi |
| --- | --- | --- | --- |
| ContosoHeader |trackingId |False |Contoso |
| ContosoHeaderValue |•••••••••••••••••••••• |True |Contoso |
| ExpressionProperty |@(DateTime.Now.ToString()) |False | |

## <a name="to-use-a-property"></a>Aby użyć właściwości
Aby użyć właściwości w zasadach, umieść nazwą właściwości wewnątrz podwójne pary nawiasów klamrowych, takich jak `{{ContosoHeader}}`, jak pokazano w poniższym przykładzie.

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

W tym przykładzie `ContosoHeader` jest używana jako nazwa nagłówka w `set-header` zasad, a `ContosoHeaderValue` jest używany jako wartość nagłówka. Gdy ta zasada jest oceniane podczas żądania lub odpowiedzi na bramie usługi API Management `{{ContosoHeader}}` i `{{ContosoHeaderValue}}` są zamieniane na ich wartości odpowiednich właściwości.

Właściwości mogą być używane jako atrybut pełną lub wartości elementów, jak pokazano w poprzednim przykładzie, ale również mogą zostać wstawione do albo połączone z częścią wyrażenia literału tekstu, jak pokazano w poniższym przykładzie:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Właściwości może również zawierać wyrażenie zasad. W poniższym przykładzie `ExpressionProperty` jest używany.

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

W przypadku oceny tych zasad `{{ExpressionProperty}}` zastępuje z wartością: `@(DateTime.Now.ToString())`. Ponieważ wartość jest wyrażenie zasad, wyrażenie jest obliczane i zasady kontynuuje jego wykonywania.

Ten limit można sprawdzić w portalu dla deweloperów, wywołując operację, która ma zasady z właściwości w zakresie. W poniższym przykładzie, operacja jest wywoływana z dwóch poprzedni przykład `set-header` zasad przy użyciu właściwości. Należy pamiętać, że odpowiedź zawiera dwa Nagłówki niestandardowe, które zostały skonfigurowane przy użyciu zasad z właściwościami.

![Portal dla deweloperów][api-management-send-results]

Jeśli przyjrzymy się [inspektora interfejsu API śledzenia](api-management-howto-api-inspector.md) dla wywołaniu, które obejmuje dwie zasady poprzedniej próbki z właściwościami, można wyświetlić dwa `set-header` zasady z wartościami właściwości dodaje oraz ocena wyrażenie zasad dla właściwości, który zawiera wyrażenie zasad.

![Interfejs API inspektora śledzenia][api-management-api-inspector-trace]

Należy pamiętać, że podczas wartości właściwości mogą zawierać wyrażenia zasad, wartości właściwości nie może zawierać inne właściwości. Jeśli tekst zawierający odwołania do właściwości jest używana do wartości właściwości, takie jak `Property value text {{MyProperty}}`, odwołania do tej właściwości nie można zamienić i zostaną zawarte w ramach wartości właściwości.

## <a name="to-create-a-property"></a>Aby utworzyć właściwość
Aby utworzyć właściwość, kliknij przycisk **Dodaj właściwość** na **właściwości** kartę.

![Dodaj właściwość][api-management-properties-add-property-menu]

**Nazwa** i **wartość** są wymaganymi wartościami. Jeśli wartość tej właściwości jest klucz tajny, sprawdź **jest klucz tajny** wyboru. Wprowadź co najmniej jeden opcjonalny znaczniki, aby ułatwić organizowanie właściwości, a następnie kliknij przycisk **zapisać**.

![Dodaj właściwość][api-management-properties-add-property]

Po zapisaniu nowej właściwości **wyszukiwania właściwości** pole tekstowe jest wypełniane przy użyciu nazwy nowej właściwości i nową właściwość jest wyświetlana. Aby wyświetlić wszystkie właściwości, wyczyść **wyszukiwania właściwości** pole tekstowe i naciśnij klawisz enter.

![Właściwości][api-management-properties-property-saved]

Aby uzyskać informacje dotyczące tworzenia właściwości przy użyciu interfejsu API REST, zobacz [utworzyć właściwość przy użyciu interfejsu API REST](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Aby edytować właściwość
Aby edytować właściwości, kliknij przycisk **Edytuj** obok właściwości do edycji.

![Edytowanie właściwości][api-management-properties-edit]

Wprowadź żądane zmiany, a następnie kliknij przycisk **zapisać**. Jeśli zmienisz nazwę właściwości, wszystkie zasady, które odwołują się do tej właściwości są automatycznie aktualizowane do użycia nowej nazwy.

![Edytowanie właściwości][api-management-properties-edit-property]

Informacje dotyczące edytowania właściwości przy użyciu interfejsu API REST, zobacz [Edytuj właściwości przy użyciu interfejsu API REST](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Aby usunąć właściwość
Aby usunąć właściwość, kliknij przycisk **usunąć** obok właściwości do usunięcia.

![Usuń właściwość][api-management-properties-delete]

Kliknij przycisk **tak, usuń go** o potwierdzenie.

![Potwierdzenie usunięcia][api-management-delete-confirm]

> [!IMPORTANT]
> Jeśli właściwość odwołuje się do niego żadnych zasad, nie można pomyślnie usunąć przed usunięciem właściwości z wszystkich zasad, które go używają.
> 
> 

Aby uzyskać informacje na temat usuwania właściwości przy użyciu interfejsu API REST, zobacz [usunąć właściwości przy użyciu interfejsu API REST](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Do wyszukiwania i filtrowania właściwości
**Właściwości** karta zawiera wyszukiwania i filtrowania funkcje ułatwiające zarządzanie właściwości. Aby filtrować listę właściwości według nazwy właściwości, wpisz wyszukiwany termin w **wyszukiwania właściwości** pola tekstowego. Aby wyświetlić wszystkie właściwości, wyczyść **wyszukiwania właściwości** pole tekstowe i naciśnij klawisz enter.

![Wyszukiwanie][api-management-properties-search]

Aby filtrować listę właściwości, według wartości tagów, wprowadź jeden lub więcej tagów do **Filtruj według znaczników** pola tekstowego. Aby wyświetlić wszystkie właściwości, wyczyść **Filtruj według znaczników** pole tekstowe i naciśnij klawisz enter.

![Filtr][api-management-properties-filter]

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o pracy z zasadami
  * [Zasady w usłudze API Management](api-management-howto-policies.md)
  * [Dokumentacja zasad](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [Wyrażenia zasad](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Obejrzyj film wideo
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Use-Properties-in-Policies/player]
> 
> 

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

