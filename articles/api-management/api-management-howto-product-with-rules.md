---
title: "Ochrona interfejsu API za pomocą usługi Azure API Management | Microsoft Docs"
description: "Dowiedz się, jak chronić interfejs API za pomocą zasad przydziałów i dławienia (ograniczania liczby wywołań)."
services: api-management
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: 450dc368-d005-401d-ae64-3e1a2229b12f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 300b1d762a61c810dbffde5aaacd8a85f12c9fca
ms.contentlocale: pl-pl
ms.lasthandoff: 09/14/2017

---
# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Ochrona interfejsu API za pomocą ograniczania liczby wywołań przy użyciu usługi Azure API Management
W tym przewodniku przedstawiono, jak łatwo można dodawać ochronę do interfejsu API zaplecza dzięki konfigurowaniu zasad ograniczania liczby wywołań i przydziałów za pomocą usługi Azure API Management.

W tym samouczku utworzysz produkt interfejsu API o nazwie „Bezpłatna wersja próbna”, który umożliwia deweloperom wykonywanie do 10 wywołań na minutę i maksymalnie 200 wywołań na tydzień tego interfejsu API przy użyciu zasad [Ograniczanie liczby wywołań na subskrypcję](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) i [Ustawianie przydziału użycia na subskrypcję](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota). Następnie opublikujesz interfejs API i przetestujesz zasadę ograniczania liczby wywołań.

Bardziej zaawansowane scenariusze ograniczania żądań używające zasad [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) i [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) są opisane w artykule [Advanced request throttling with Azure API Management](api-management-sample-flexible-throttling.md) (Zaawansowane ograniczanie żądań za pomocą usługi Azure API Management).

## <a name="create-product"> </a>Aby utworzyć produkt
W tym kroku utworzysz produkt Bezpłatna wersja próbna, który nie wymaga zatwierdzania subskrypcji.

> [!NOTE]
> Jeśli już masz skonfigurowany produkt i chcesz używać go w ramach tego samouczka, możesz przejść od razu do tematu [Konfigurowanie zasad ograniczania liczby wywołań oraz przydziałów][Configure call rate limit and quota policies] i wykonać kroki samouczka od tamtego miejsca przy użyciu swojego produktu zamiast produktu Bezpłatna wersja próbna.
> 
> 

Na początku kliknij opcję **Portal wydawcy** w klasycznej witrynie Azure Portal dla usługi API Management.

![Portal wydawcy][api-management-management-console]

> Jeśli jeszcze nie utworzono wystąpienia usługi API Management, zobacz [Tworzenie wystąpienia usługi API Management][Create an API Management service instance] w samouczku [Zarządzanie pierwszym interfejsem API w usłudze Azure API Management][Manage your first API in Azure API Management].
> 
> 

Kliknij opcję **Produkty** w menu **API Management** po lewej stronie, aby wyświetlić stronę **Produkty**.

![Dodawanie produktu][api-management-add-product]

Kliknij przycisk **Dodaj produkt**, aby wyświetlić okno dialogowe **Dodawanie nowego produktu**.

![Dodawanie nowego produktu][api-management-new-product-window]

W polu **Tytuł** wpisz **Bezpłatna wersja próbna**.

W polu **Opis** wpisz następujący tekst: **Subskrybenci będą mogli uruchamiać 10 wywołań na minutę, ale nie więcej niż 200 wywołań na tydzień, po czym nastąpi odmowa dostępu.**

Produkty w usłudze API Management mogą być chronione lub otwarte. Produkty chronione muszą być subskrybowane przed użyciem. Produkty otwarte mogą być używane bez subskrypcji. Upewnij się, że opcja **Wymagaj subskrypcji** jest zaznaczona, aby utworzyć produkt chroniony, który wymaga subskrypcji. Jest to ustawienie domyślne.

Jeśli chcesz, aby administrator przeglądał i akceptował lub odrzucał próby subskrypcji tego produktu, wybierz opcję **Wymagaj zatwierdzenia subskrypcji**. Jeśli to pole wyboru nie jest zaznaczone, próby subskrypcji będą zatwierdzane automatycznie. W tym przykładzie subskrypcje są zatwierdzane automatycznie, więc nie zaznaczaj tego pola.

Aby umożliwić kontom deweloperów wielokrotne subskrybowanie nowego produktu, zaznacz pole wyboru **Zezwalaj na wiele jednoczesnych subskrypcji**. Ten samouczek nie wykorzystuje wielu jednoczesnych subskrypcji, więc pozostaw to pole niezaznaczone.

Po wprowadzeniu wszystkich wartości kliknij przycisk **Zapisz**, aby utworzyć produkt.

![Dodany produkt][api-management-product-added]

Domyślnie nowe produkty są widoczne dla użytkowników w grupie **Administratorzy**. Zamierzamy dodać grupę **Deweloperzy**. Kliknij produkt **Bezpłatna wersja próbna**, a następnie kliknij kartę **Widoczność**.

> W usłudze API Management grupy służą do zarządzania widocznością produktów dla deweloperów. Widoczność produktów jest przydzielana według grup, a deweloperzy mogą wyświetlać i subskrybować produkty, które są widoczne dla grup, do których należą. Aby uzyskać więcej informacji, zobacz [How to create and use groups in Azure API Management][How to create and use groups in Azure API Management] (Jak utworzyć grupy w usłudze Azure API Management i używać ich).
> 
> 

![Dodawanie grupy deweloperów][api-management-add-developers-group]

Zaznacz pole wyboru **Deweloperzy**, a następnie kliknij przycisk **Zapisz**.

## <a name="add-api"> </a>Aby dodać interfejs API do produktu
W tym kroku samouczka dodamy interfejs Echo API do nowego produktu Bezpłatna wersja próbna.

> Każde wystąpienie usługi API Management ma wstępnie skonfigurowany interfejs Echo API, którego można używać do eksperymentów oraz poznawania usługi API Management. Aby uzyskać więcej informacji, zobacz [Zarządzanie pierwszym interfejsem API w usłudze Azure API Management][Manage your first API in Azure API Management].
> 
> 

Kliknij przycisk **Produkty** z menu **API Management** po lewej stronie, a następnie kliknij produkt **Bezpłatna wersja próbna**, aby go skonfigurować.

![Konfigurowanie produktu][api-management-configure-product]

Kliknij przycisk **Dodaj interfejs API do produktu**.

![Dodawanie interfejsu API do produktu.][api-management-add-api]

Wybierz interfejs **Echo API**, a następnie kliknij przycisk **Zapisz**.

![Dodawanie interfejsu Echo API][api-management-add-echo-api]

## <a name="policies"> </a>Aby skonfigurować zasady ograniczania liczby wywołań oraz przydziałów
Ograniczenia liczby wywołań i przydziały są konfigurowane w edytorze zasad. Dwiema zasadami, które dodamy w tym samouczku, są zasady [Ograniczanie liczby wywołań na subskrypcję](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) i [Ustawianie przydziału użycia na subskrypcję](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota). Te zasady muszą być stosowane w zakresie produktu.

Kliknij opcję **Zasady** w menu **API Management** po lewej stronie. Na liście **Produkty** kliknij produkt **Bezpłatna wersja próbna**.

![Zasady produktu][api-management-product-policy]

Kliknij przycisk **Dodaj zasadę**, aby zaimportować szablon zasad i rozpocząć tworzenie zasad ograniczania liczby wywołań i przydziałów.

![Dodawanie zasad][api-management-add-policy]

Zasady ograniczania liczby wywołań i przydziałów są zasadami ruchu przychodzącego, więc umieść kursor w elemencie inbound.

![Edytor zasad][api-management-policy-editor-inbound]

Przewiń listę zasad i znajdź wpis zasad **Ograniczanie liczby wywołań na subskrypcję**.

![Instrukcje zasad][api-management-limit-policies]

Po umieszczeniu kursora w elemencie zasady **inbound**, kliknij strzałkę obok opcji **Ograniczanie liczby wywołań na subskrypcję**, aby wstawić ten szablon zasady.

```xml
<rate-limit calls="number" renewal-period="seconds">
<api name="name" calls="number">
<operation name="name" calls="number" />
</api>
</rate-limit>
```

Jak widać we fragmencie kodu, zasady umożliwiają ustawienie limitów dla interfejsów API i operacji produktu. W tym samouczku nie będziemy korzystać z tej możliwości, więc usuń elementy **api** i **operation** elementu **rate-limit**, aby pozostał tylko zewnętrzny element **rate-limit**, jak pokazano w poniższym przykładzie.

```xml
<rate-limit calls="number" renewal-period="seconds">
</rate-limit>
```

W produkcie Bezpłatna wersja próbna maksymalna liczba wywołań to 10 na minutę, więc wpisz **10** jako wartość atrybutu **calls** oraz i **60** jako wartość atrybutu **renewal-period**.

```xml
<rate-limit calls="10" renewal-period="60">
</rate-limit>
```

Aby skonfigurować zasady **Ustawianie przydziału użycia na subskrypcję**, umieść kursor bezpośrednio pod nowo dodanym elementem **rate-limit** w elemencie **inbound**, a następnie zlokalizuj i kliknij strzałkę po lewej stronie zasad **Ustawianie przydziału użycia na subskrypcję**.

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
<api name="name" calls="number" bandwidth="kilobytes">
<operation name="name" calls="number" bandwidth="kilobytes" />
</api>
</quota>
```

Podobnie jak zasady **Ograniczanie liczby wywołań na subskrypcję**, zasady **Ustawianie przydziału użycia na subskrypcję** umożliwia ustawienie limitów dla interfejsów API i operacji w produkcie. W tym samouczku nie będziemy korzystać z tej możliwości, więc usuń elementy **api** i **operation** z elementu **quota**, jak pokazano w poniższym przykładzie.

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
</quota>
```

Przydziały mogą opierać się na liczbie wywołań na interwał, przepustowości lub obu tych warunkach. W tym samouczku nie będziemy ograniczać żądań na podstawie przepustowości, więc usuń atrybut **bandwidth**.

```xml
<quota calls="number" renewal-period="seconds">
</quota>
```

W produkcie Bezpłatna wersja próbna przydział wynosi 200 wywołań na tydzień. Podaj **200** jako wartość atrybutu **calls**, a następnie podaj **604800** jako wartość atrybutu **renewal-period**.

```xml
<quota calls="200" renewal-period="604800">
</quota>
```

> Interwały zasad są określane w sekundach. Aby obliczyć interwał dla tygodnia, należy pomnożyć liczbę dni (7) przez liczbę godzin w ciągu dnia (24) przez liczbę minut w godzinie (60) przez liczbę sekund w ciągu minuty (60): 7 * 24 * 60 * 60 = 604800.
> 
> 

Po zakończeniu konfigurowania zasady powinny być zgodne z poniższym przykładem.

```xml
<policies>
    <inbound>
        <rate-limit calls="10" renewal-period="60">
        </rate-limit>
        <quota calls="200" renewal-period="604800">
        </quota>
        <base />

</inbound>
<outbound>

    <base />

    </outbound>
</policies>
```

Po skonfigurowaniu żądanych zasad kliknij przycisk **Zapisz**.

![Zapisywanie zasad][api-management-policy-save]

## <a name="publish-product"> </a> Aby opublikować produkt
Teraz, gdy interfejsy API zostały dodane, a zasady skonfigurowane, trzeba opublikować produkt, aby mógł być używany przez deweloperów. Kliknij przycisk **Produkty** z menu **API Management** po lewej stronie, a następnie kliknij produkt **Bezpłatna wersja próbna**, aby go skonfigurować.

![Konfigurowanie produktu][api-management-configure-product]

Kliknij przycisk **Publikuj**, a następnie kliknij przycisk **Tak, opublikuj**, aby potwierdzić.

![Publikowanie produktu][api-management-publish-product]

## <a name="subscribe-account"> </a>Aby zasubskrybować produkt dla konta dewelopera
Teraz, gdy produkt jest publikowany, jest on dostępny do subskrybowania i używania przez deweloperów.

> Administratorzy wystąpienia usługi API Management automatycznie mają subskrypcję każdego produktu. W tym kroku samouczka zasubskrybujemy produkt Bezpłatna wersja próbna dla jednego z kont deweloperów, którzy nie są administratorami. Jeśli konto dewelopera należy do roli Administratorzy, możesz wykonać ten krok, chociaż masz już subskrypcję.
> 
> 

Kliknij przycisk **Użytkownicy** w menu **API Management** po lewej stronie, a następnie kliknij nazwę Twojego konta dewelopera. W tym przykładzie użyto konta dewelopera **Clayton Gragg**.

![Konfigurowanie konta dewelopera][api-management-configure-developer]

Kliknij przycisk **Dodaj subskrypcję**.

![Dodawanie subskrypcji][api-management-add-subscription-menu]

Wybierz produkt **Bezpłatna wersja próbna**, a następnie kliknij przycisk **Subskrybuj**.

![Dodawanie subskrypcji][api-management-add-subscription]

> [!NOTE]
> W tym samouczku nie włączono wielu jednoczesnych subskrypcji dla produktu Bezpłatna wersja próbna. Gdyby były włączone, pojawiłby się monit o podanie nazwy subskrypcji, jak pokazano w poniższym przykładzie.
> 
> 

![Dodawanie subskrypcji][api-management-add-subscription-multiple]

Po kliknięciu przycisku **Subskrybuj** produkt jest wyświetlany na liście **Subskrypcja** danego użytkownika.

![Dodana subskrypcja][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Aby wywołać operację i przetestować ograniczanie liczby wywołań
Teraz, gdy produkt Bezpłatna wersja próbna jest skonfigurowany i opublikowany, możemy wywołać pewne operacje i przetestować zasadę ograniczania liczby wywołań.
Kliknij opcję **Portal dla deweloperów** w górnym menu po prawej stronie, aby przejść do portalu dla deweloperów.

![Portal dla deweloperów][api-management-developer-portal-menu]

Kliknij opcję **Interfejsy API** w górnym menu, a następnie wybierz interfejs **Echo API**.

![Portal dla deweloperów][api-management-developer-portal-api-menu]

Kliknij operację **GET Resource** (pobieranie zasobu), a następnie kliknij przycisk **Wypróbuj**.

![Otwarta konsola][api-management-open-console]

Zachowaj domyślne wartości parametrów, a następnie wybierz klucz subskrypcji dla produktu Bezpłatna wersja próbna.

![Klucz subskrypcji][api-management-select-key]

> [!NOTE]
> Jeśli masz wiele subskrypcji, koniecznie wybierz klucz dla produktu **Bezpłatna wersja próbna**. W przeciwnym razie zasady skonfigurowane w poprzednich krokach nie będą obowiązywać.
> 
> 

Kliknij przycisk **Wyślij**, a następnie zobacz odpowiedź. Zauważ, że **Stan odpowiedzi** to **200 OK**.

![Wyniki operacji][api-management-http-get-results]

Kliknij przycisk **Wyślij** szybciej niż pozwala na to zasada ograniczenia liczby wywołań do 10 na minutę. Po przekroczeniu zasady ograniczania liczby wywołań zwracany jest stan **429 Too Many Requests** (429 Zbyt wiele żądań).

![Wyniki operacji][api-management-http-get-429]

**Zawartość odpowiedzi** wskazuje pozostały czas, zanim ponowna próba zakończy się pomyślnie.

Jeśli obowiązuje zasada ograniczania liczby wywołań do 10 na minutę, kolejne wywołania będą kończyć się niepowodzeniem, aż upłynie 60 sekund od pierwszego z 10 pomyślnych wywołań produktu przed przekroczeniem ograniczenia. W tym przykładzie pozostały interwał to 54 sekundy.

## <a name="next-steps"> </a>Następne kroki
* Obejrzyj następujący film z prezentacją ustawiania ograniczeń liczby wywołań i przydziałów.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Manage your first API in Azure API Management]: api-management-get-started.md
[How to create and use groups in Azure API Management]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Configure call rate limit and quota policies]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota

