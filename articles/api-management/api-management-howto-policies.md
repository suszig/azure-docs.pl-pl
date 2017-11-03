---
title: "Zasady w usłudze Azure API Management | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie tworzenia, edytowania i konfigurowania zasad w usłudze API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 537e5caf-708b-430e-a83f-72b70af28aa9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: ef4cb447430a613dd519d96dd7732a9349ebba39
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2017
---
# <a name="policies-in-azure-api-management"></a>Zasady w usłudze Azure API Management
W usłudze Azure API Management zasady są zaawansowanych możliwości systemu, który umożliwia wydawcy, aby zmienić zachowanie interfejsu API za pomocą konfiguracji. Zasady są zbiór instrukcji, które są wykonywane sekwencyjnie na żądanie lub odpowiedź interfejsu API. Popularne instrukcje obejmują Konwersja formatu z pliku XML do formatu JSON i Wywołaj szybkość ograniczenie, aby ograniczyć ilość przychodzących od dewelopera. Wiele zasad są dostępne poza pole.

Zobacz [informacje o zasadach] [ Policy Reference] pełną listę deklaracji zasad i ich ustawienia.

Zasady są stosowane wewnątrz bramy, która znajduje się pomiędzy konsumenta interfejsu API i zarządzanego interfejsu API. Brama odbiera wszystkie żądania i zwykle przekazuje je niezmieniony do podstawowej interfejsu API. Jednak zasady można zastosować zmiany do żądania przychodzące i wychodzące odpowiedzi.

Wyrażenia zasad mogą służyć jako wartości atrybutów lub wartości tekstowe w dowolnej z zasad usługi API Management, o ile w zasadach nie określono inaczej. Niektóre zasady, takie jak [sterowania przepływem] [ Control flow] i [zmiennej zestaw] [ Set variable] zasady są oparte na wyrażeniach zasad. Aby uzyskać więcej informacji, zobacz [zaawansowane zasady] [ Advanced policies] i [wyrażenie zasad][Policy expressions].

## <a name="scopes"></a>Sposobu konfigurowania zasad
Zasady można skonfigurować globalnie lub w zakresie [produktu][Product], [interfejsu API] [ API] lub [operacji] [Operation]. Aby skonfigurować zasadę, przejdź do edytora zasad w portalu wydawcy.

![Menu zasad][policies-menu]

Edytor zasad składa się z trzech głównych sekcji: zakres zasad (u góry), definicji zasad, gdy zasady są edytowane (lewych) i instrukcje listy (po prawej):

![Edytor zasad][policies-editor]

Aby rozpocząć konfigurowanie zasad, musisz najpierw wybrać zakres, w którym mają dotyczyć zasady. Na poniższym zrzucie ekranu **Starter** produkt jest zaznaczony. Należy pamiętać, że kwadratowy symbol obok nazwy zasad wskazuje, że zasady jest już stosowane na tym poziomie.

![Zakres][policies-scope]

Ponieważ zasady zostały już zastosowane, konfiguracji jest wyświetlany w definicji widoku.

![Konfigurowanie][policies-configure]

Zasady zostaną wyświetlone tylko do odczytu na początku. Aby edytować kliknij definicji **Konfiguruj zasady** akcji.

![Edytuj][policies-edit]

Definicja zasad jest proste dokument XML, który opisuje sekwencji instrukcji dla ruchu przychodzącego i wychodzącego. Plik XML można edytować bezpośrednio w oknie definicji. Lista instrukcji znajduje się po prawej stronie i instrukcje mające zastosowanie do bieżącego zakresu są włączone i wyróżniony; jak dowodzą **Limit szybkości wywołać** instrukcji na zrzucie ekranu powyżej.

Kliknięcie instrukcji włączone spowoduje dodanie odpowiednich XML w lokalizacji kursora w definicji widoku. 

> [!NOTE]
> Zasady, które chcesz dodać nie jest włączona, upewnij się, że jesteś w niewłaściwym zakresie dla tej zasady. Każda instrukcja zasad jest przeznaczony do użytku w określonych zakresach i sekcje zasad. Aby zapoznać się z sekcji zasad i zakresy dla zasady, sprawdź **użycia** sekcji dla tej zasady w [informacje o zasadach][Policy Reference].
> 
> 

Pełna lista deklaracji zasad i ich ustawienia są dostępne w [informacje o zasadach][Policy Reference].

Na przykład, aby dodać nowe oświadczenie ograniczanie żądań przychodzących do określonych adresów IP, umieść kursor wewnątrz treści `inbound` — element XML i kliknij przycisk **wywołującego Ogranicz adresów IP** instrukcji.

![Zasady ograniczeń][policies-restrict]

Spowoduje to dodanie fragment kodu XML dotyczący `inbound` element, który zawiera wskazówki dotyczące sposobu konfigurowania instrukcji.

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

Ogranicza liczby żądań przychodzących i zaakceptować tylko te z adresu IP 1.2.3.4 zmodyfikować kod XML w następujący sposób:

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

![Zapisz][policies-save]

Po zakończeniu konfigurowania instrukcje zasad, kliknij przycisk **zapisać** i zmiany będą przekazywane do bramy usługi API Management natychmiast.

## <a name="sections"></a>Opis zasad konfiguracji
Zasady zostaną serię instrukcji, które są wykonywane w kolejności na żądania i odpowiedzi. Konfiguracja jest odpowiednio podzielone na `inbound`, `backend`, `outbound`, i `on-error` sekcjach przedstawiono, jak pokazano w poniższej konfiguracji.

```xml
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is forwarded to 
         the backend service go here -->
  </backend>
  <outbound>
    <!-- statements to be applied to the response go here -->
  </outbound>
  <on-error>
    <!-- statements to be applied if there is an error condition go here -->
  </on-error>
</policies> 
```

Jeśli występuje błąd podczas przetwarzania żądania, wszystkie pozostałe kroki `inbound`, `backend`, lub `outbound` sekcje są pomijane i wykonywania przechodzi do instrukcji w `on-error` sekcji. Przez umieszczenie deklaracji zasad w `on-error` możesz przejrzeć kod błędu przy użyciu sekcji `context.LastError` właściwość, sprawdzić i dostosować za pomocą odpowiedzi błędu `set-body` zasad i skonfigurować, co się stanie w przypadku wystąpienia błędu. Brak kody błędów dla wbudowanych kroków i błędy, które mogą wystąpić podczas przetwarzania zasady. Aby uzyskać więcej informacji, zobacz [obsługi błędów w zasad interfejsu API zarządzania](https://msdn.microsoft.com/library/azure/mt629506.aspx).

Ponieważ zasad można określić na różnych poziomach (globalne, produktu, interfejsu api i operacji) konfiguracji umożliwia określenie kolejności, w którym instrukcji definicji zasad wykonywania względem zasad nadrzędnej. 

Zakresy zasad są oceniane w następującej kolejności.

1. Globalne
2. Zakres produktu
3. Zakres interfejsu API
4. Operacja zakresu

Instrukcje w nich są oceniane według położenia `base` elementu, jeśli jest obecny. Globalne zasady nie ma nadrzędnego zasad i za pomocą `<base>` element w nim nie ma wpływu.

Na przykład jeśli masz zasady na poziomie globalnym i zasady skonfigurowane dla interfejsu API, następnie każdorazowe użycie tego konkretnego interfejsu API obie zasady zostaną zastosowane. Zarządzanie interfejsami API umożliwia deterministyczne kolejność deklaracji zasad połączonych za pośrednictwem elementu podstawowego. 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

W definicji zasad przykład powyżej `cross-domain` instrukcji jest wykonywany przed następować wszystkie wyższej zasady, które z kolei, `find-and-replace` zasad. 

Aby wyświetlić zasady w bieżącym zakresie w edytorze zasad, kliknij **ponownie Oblicz skutecznych zasad dla wybranego zakresu**.

## <a name="next-steps"></a>Następne kroki
Zapoznaj się z następującego wideo na wyrażeniach zasad.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
