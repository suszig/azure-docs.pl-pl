---
title: "Jak tworzyć i publikować w usłudze Azure API Management produktu"
description: "Dowiedz się, jak tworzyć i publikować w usłudze Azure API Management produktów."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 31de55cb-9384-490b-a2f2-6dfcf83da764
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 73bf4451ba1b71807e22440beecc73a7e8045c5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>Jak tworzyć i publikować w usłudze Azure API Management produktu
W systemie Azure API Management produkt zawiera jeden lub więcej interfejsów API, a także przydział użycia i warunki użytkowania. Po opublikowaniu produktu deweloperzy mogą subskrybować produktu i rozpocząć przy użyciu interfejsów API produktu. Temat zawiera przewodnik dotyczący tworzenia produktu, dodawanie interfejsu API i publikowania dla deweloperów.

## <a name="create-product"></a>Tworzenie produktu
Działania są dodawane i skonfigurowane do interfejsu API w portalu wydawcy. Aby uzyskać dostęp do portalu wydawcy, kliknij przycisk **portal wydawcy** w portalu Azure usługi Zarządzanie interfejsami API.

![Portal wydawcy][api-management-management-console]

> Jeśli jeszcze nie masz utworzonego wystąpienia usługi API Management, zobacz [Tworzenie wystąpienia usługi API Management][Create an API Management service instance] w samouczku [Wprowadzenie do usługi Azure API Management][Get started with Azure API Management].
> 
> 

Polecenie **produktów** w menu po lewej stronie, aby wyświetlić **produktów** , a następnie kliknij przycisk **Dodaj produkt**.

![Produkty][api-management-products]

![Nowego produktu][api-management-add-new-product]

Wprowadź opisową nazwę produktu w **nazwa** opis produktu w **opis** pola.

Produkty w usłudze API Management może być **Otwórz** lub **chronione**. Produkty chronione trzeba subskrybować przed użyciem, a produkty otwarte mogą być używane bez subskrypcji. Sprawdź **wymagają subskrypcji** do tworzenia chronionej produkt, który wymaga subskrypcji. Jest to ustawienie domyślne.

Sprawdź **wymagają zatwierdzenia subskrypcji** Jeśli chcesz, aby administrator, aby przejrzeć i zaakceptować lub odrzucić prób subskrypcję do tego produktu. Jeśli pole jest zaznaczona, prób subskrypcja będzie automatycznie zatwierdzone. Aby uzyskać więcej informacji o subskrypcji, zobacz [wyświetlić subskrybentów produktu][View subscribers to a product].

Aby zezwalać kontom developer do subskrybowania wielokrotnie produktu, sprawdź **zezwalać na wiele subskrypcji** pole wyboru. Jeśli to pole nie jest zaznaczone, każde konto dewelopera można zasubskrybować tylko jeden raz dla produktu.

![Nieograniczone wiele subskrypcji][api-management-unlimited-multiple-subscriptions]

Aby ograniczyć liczbę wiele równoczesnych subskrypcji, sprawdź **Ogranicz liczbę jednoczesnych subskrypcje** pole wyboru, a następnie wprowadź limit subskrypcji. W poniższym przykładzie jednoczesnych subskrypcje są maksymalnie cztery na konto dewelopera.

![Cztery wiele subskrypcji][api-management-four-multiple-subscriptions]

Po skonfigurowaniu wszystkich nowych opcji produktu, kliknij przycisk **zapisać** do tworzenia nowych produktów.

![Produkty][api-management-products-page]

> Domyślnie są nieopublikowane nowych produktów i są widoczne tylko dla **Administratorzy** grupy.
> 
> 

Aby skonfigurować produkt, kliknij nazwę produktu w **produktów** kartę.

## <a name="add-apis"></a>Dodaj interfejsów API do produktu
**Produktów** strona zawiera cztery łącza dla konfiguracji: **Podsumowanie**, **ustawienia**, **widoczność**, i  **Subskrybenci**. **Podsumowanie** jest karta, którym można dodać interfejsów API i opublikować lub Cofnij publikowanie produktu.

![Podsumowanie][api-management-new-product-summary]

Przed opublikowaniem produkt, musisz dodać jeden lub więcej interfejsów API. Aby to zrobić, kliknij przycisk **dodać interfejsu API do produktu**.

![Dodaj interfejsów API][api-management-add-apis-to-product]

Wybierz żądaną interfejsów API i kliknij przycisk **zapisać**.

## <a name="add-description"></a>Dodać opisowe informacje do produktu
**Ustawienia** karta umożliwia zawierają szczegółowe informacje o produkcie, takie jak jego przeznaczenie, zapewnia dostęp do interfejsów API i inne przydatne informacje. Zawartość jest przeznaczona dla deweloperów, spowoduje wywołanie interfejsu API, które mogą być zapisywane w postaci zwykłego tekstu lub kod znaczników HTML.

![Ustawienia produktów][api-management-product-settings]

Sprawdź **wymagają subskrypcji** do tworzenia chronionej produkt, który wymaga subskrypcji można używać, lub wyczyść pole wyboru, aby utworzyć otwieranie produktu, który można wywołać bez subskrypcji.

Wybierz **wymagają zatwierdzenia subskrypcji** Jeśli chcesz ręcznie zatwierdzić wszystkie żądania subskrypcji produktu. Domyślnie wszystkie subskrypcje produktu są przyznawane automatycznie.

Aby zezwalać kontom developer do subskrybowania wielokrotnie produktu, sprawdź **zezwalać na wiele subskrypcji** pole wyboru i opcjonalnie Określ limit. Jeśli to pole nie jest zaznaczone, każde konto dewelopera można zasubskrybować tylko jeden raz dla produktu.

Opcjonalnie wypełnij **warunki użytkowania** pola opisujące warunki użytkowania produktów, którzy użytkownicy muszą zaakceptować, aby korzystać z produktu.

## <a name="publish-product"></a>Publikowania produktu
Aby można było wywołać interfejsów API w produkcie, musi zostać opublikowany produktu. Na **Podsumowanie** produktu, kliknij pozycję **publikowania**, a następnie kliknij przycisk **tak, przed opublikowaniem** o potwierdzenie. Aby wcześniej opublikowanej produktu prywatne, kliknij przycisk **Cofnij publikowanie**.

![Publikowanie produktu][api-management-publish-product]

## <a name="make-visible"></a>Udostępnienie produktu dla deweloperów
**Widoczność** karcie można wybrać role mogą wyświetlić w portalu dla deweloperów i subskrybowanie produktu.

![Widoczność produktu][api-management-product-visiblity]

Aby włączyć lub wyłączyć widoczność produktu dla deweloperów w grupie, sprawdź lub usuń zaznaczenie pola wyboru obok grupy, a następnie kliknij przycisk **zapisać**.

> Aby uzyskać więcej informacji, zobacz [tworzenie i używanie grup do zarządzania konta dewelopera usługi Azure API Management][How to create and use groups to manage developer accounts in Azure API Management].
> 
> 

## <a name="view-subscribers"></a>Wyświetlić subskrybentów produktu
**Subskrybentów** karta zawiera listę deweloperów, którzy subskrybuje produktu. Ustawienia dla wszystkich deweloperów i szczegółowe informacje można wyświetlić, klikając nazwę dewelopera. W tym przykładzie deweloperzy nie masz jeszcze subskrypcję do produktu.

![Deweloperzy][api-management-developer-list]

## <a name="next-steps"> </a>Następne kroki
Po są dodawane odpowiednie interfejsów API i publikowane produktu, deweloperzy mogą subskrybować produktu i rozpocząć do wywoływania interfejsów API. Samouczek przedstawiający tych elementów, a także zaawansowane produktu configuration [jak utworzyć i skonfigurować ustawienia zaawansowane produktu w usłudze Azure API Management][How create and configure advanced product settings in Azure API Management].

Aby uzyskać więcej informacji na temat pracy z produktami zobacz poniższe wideo.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[View subscribers to a product]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[How to create and use groups to manage developer accounts in Azure API Management]: api-management-howto-create-groups.md
[How create and configure advanced product settings in Azure API Management]: api-management-howto-product-with-rules.md 
