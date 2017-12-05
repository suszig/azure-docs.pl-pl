---
title: "Zarządzanie kontami dewelopera przy użyciu grup w usłudze Azure API Management | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zarządzać kontami dewelopera przy użyciu grup w usłudze Azure API Management"
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: 1587243bcd5f2b9af98b8b529c152ba49ef676be
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Tworzenie i używanie grup do zarządzania konta dewelopera usługi Azure API Management
W usłudze API Management grupy służą do zarządzania widocznością produktów dla deweloperów. Produkty są najpierw stają się widoczne dla grup, a następnie deweloperów w tych grupach można wyświetlać i subskrybować produktów, które są skojarzone z grupami. 

Usługa API Management ma następujące niezmienne grupy systemowe.

* **Administratorzy** — do tej grupy należą administratorzy subskrypcji platformy Azure. Administratorzy zarządzają wystąpieniami usługi API Management, tworząc interfejsy API, operacje i produkty, które są używane przez deweloperów.
* **Deweloperzy** — do tej grupy należą uwierzytelnieni użytkownicy portalu dla deweloperów. Deweloperzy to klienci, którzy tworzą aplikacje przy użyciu interfejsów API. Deweloperzy otrzymują dostęp do portalu dla deweloperów i tworzą aplikacje, które wywołują operacje interfejsów API.
* **Goście** — do tej grupy należą nieuwierzytelnieni użytkownicy portalu dla deweloperów, tacy jak potencjalni klienci odwiedzający portal dla deweloperów wystąpienia usługi API Management. Mogą mieć przyznany pewien zakres dostępu tylko do odczytu, czyli np. możliwość wyświetlania interfejsów API, ale nie ich wywoływania.

Oprócz tych grup systemu, Administratorzy mogą tworzyć niestandardowe grupy lub [korzystać z zewnętrznej grupy w skojarzonych dzierżaw usługi Azure Active Directory][leverage external groups in associated Azure Active Directory tenants]. Niestandardowe i zewnętrzne grupy mogą być używane razem z grupami systemowymi, zapewniając deweloperom widoczność produktów interfejsu API i dostęp do nich. Można na przykład utworzyć jedną grupę niestandardową dla deweloperów powiązanych z konkretną organizacją partnera i zezwolić im na dostęp do interfejsów API z produktów zawierających tylko odpowiednie interfejsy API. Użytkownik może należeć do więcej niż jednej grupy.

W tym przewodniku pokazano, jak Administratorzy wystąpienia interfejsu API zarządzania mogą dodawać nowe grupy i skojarzyć je z produktami i deweloperów.

> [!NOTE]
> Oprócz tworzenia grup i zarządzanie nimi w portalu wydawcy, można tworzyć i zarządzać przy użyciu interfejsu API REST API zarządzania grupami [grupy](https://msdn.microsoft.com/library/azure/dn776329.aspx) jednostki.
> 
> 

## <a name="create-group"></a>Utwórz grupę
Aby utworzyć nową grupę, kliknij przycisk **portal wydawcy** w portalu Azure usługi Zarządzanie interfejsami API. Spowoduje to przejście do portalu wydawcy usługi API Management.

![Portal wydawcy][api-management-management-console]

> Jeśli jeszcze nie utworzono wystąpienie usługi API Management, zobacz [Utwórz wystąpienie usługi Zarządzanie interfejsami API][Create an API Management service instance].
> 
> 

Kliknij przycisk **grup** z **zarządzanie interfejsami API** menu po lewej stronie, a następnie kliknij przycisk **Dodaj grupę**.

![Dodaj nową grupę][api-management-add-group]

Wprowadź unikatową nazwę grupy i opcjonalny opis, a następnie kliknij przycisk **zapisać**.

![Dodaj nową grupę][api-management-add-group-window]

Nowa grupa zostanie wyświetlona na karcie grupy. Aby edytować **nazwa** lub **opis** grupy, kliknij nazwę grupy na liście. Aby usunąć grupę, kliknij przycisk **usunąć**.

![Grupy dodane][api-management-new-group]

Teraz, gdy grupa nie zostanie utworzona, można ją skojarzyć z produktami i deweloperów.

## <a name="associate-group-product"></a>Skojarzyć grupy z produktem
Aby skojarzyć grupy z produktem, kliknij przycisk **produktów** z **zarządzanie interfejsami API** menu po lewej stronie, a następnie kliknij nazwę żądanego produktu.

![Ustaw widoczność][api-management-add-group-to-product]

Wybierz **widoczność** kartę, aby dodawać i usuwać grupy i wyświetlić bieżących grup dla produktu. Aby dodać lub usunąć grupy, zaznacz lub usuń zaznaczenie pól wyboru dla żądanej grupy i kliknij przycisk **zapisać**.

![Ustaw widoczność][api-management-add-group-to-product-visibility]

> [!NOTE]
> Aby dodać grupy usługi Azure Active Directory, zobacz [sposób autoryzowania konta dewelopera przy użyciu usługi Azure Active Directory w usłudze Azure API Management](api-management-howto-aad.md).
> 
> Aby skonfigurować grupy z **widoczność** produktu, kliknij pozycję **Zarządzaj grupami**.
> 
> 

Jeśli produkt jest skojarzona z grupą, deweloperzy w tej grupie można wyświetlać i subskrybować produktu.

## <a name="associate-group-developer"></a>Kojarzyć grup z deweloperami
Aby skojarzyć grupy z deweloperami, kliknij przycisk **użytkowników** z **zarządzanie interfejsami API** menu po lewej stronie, a następnie zaznacz pole obok deweloperzy chcesz skojarzyć z grupą.

![Dodawanie projektanta do grupy][api-management-add-group-to-developer]

Po żądaną Deweloperzy są zaznaczone, kliknij odpowiednią grupę w **dodać do grupy** listy rozwijanej. Deweloperzy mogą zostać usunięte z grup za pomocą **Usuń z grupy** listy rozwijanej. 

![Deweloperzy][api-management-add-group-to-developer-saved]

Po dodaniu skojarzenia między projektanta i grupy, możesz je wyświetlić w **użytkowników** kartę.

## <a name="next-steps"> </a>Następne kroki
* Po dodaniu do grupy Deweloperzy mogą wyświetlać i subskrybować produktów skojarzonych z tej grupy. Aby uzyskać więcej informacji, zobacz [jak tworzyć i publikować w usłudze Azure API Management produktu][How create and publish a product in Azure API Management],
* Oprócz tworzenia grup i zarządzanie nimi w portalu wydawcy, można tworzyć i zarządzać przy użyciu interfejsu API REST API zarządzania grupami [grupy](https://msdn.microsoft.com/library/azure/dn776329.aspx) jednostki.

[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
