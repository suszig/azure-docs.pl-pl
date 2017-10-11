---
title: "Importowanie interfejsu API usługi Azure API Management | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zaimportować interfejsu API i jego operacji do usługi Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 40398b0a-ac2c-43f0-89e1-07e4abbf502f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: c851b88fc1067e65044266d07775717c028e75d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Jak zaimportować definicję interfejsu API z operacjami w usłudze Azure API Management
W usłudze API Management można tworzyć nowych interfejsów API i operacje ręcznie dodawać lub interfejsu API, które mogą być importowane wraz z operacji w jednym kroku.

Interfejsy API i ich operacje mogą być importowane przy użyciu następujących formatów.

* WADL
* Swagger

W tym przewodniku pokazano, jak tworzyć nowy interfejs API i zaimportować jego operacji w jednym kroku. Informacje dotyczące ręcznego tworzenia interfejsu API i dodawanie działań, zobacz [Tworzenie interfejsów API] [ How to create APIs] i [sposób dodawania działań do interfejsu API][How to add operations to an API].

## <a name="import-api"> </a>Importowanie interfejsu API
Interfejsy API są tworzone i skonfigurować w portalu wydawcy. Aby uzyskać dostęp do portalu wydawcy, kliknij przycisk **portal wydawcy** w portalu Azure usługi Zarządzanie interfejsami API. Jeśli jeszcze nie masz utworzonego wystąpienia usługi API Management, zobacz [Tworzenie wystąpienia usługi API Management][Create an API Management service instance] w samouczku [Wprowadzenie do usługi Azure API Management][Get started with Azure API Management].

![Portal wydawcy][api-management-management-console]

Kliknij przycisk **interfejsów API** z **zarządzanie interfejsami API** menu po lewej stronie, a następnie kliknij przycisk **zaimportować interfejsu API**.

![Interfejs API importu][api-management-import-apis]

**Import API** okno ma trzy karty, które odpowiadają trzy sposoby zapewnienia specyfikacja interfejsu API.

* **Ze Schowka** umożliwia wklejanie specyfikacja interfejsu API w polu tekstowym wyznaczonych.
* **Z pliku** umożliwia Wyszukaj i wybierz plik, który zawiera specyfikację interfejsu API.
* **Z adresu URL** pozwala podać adres URL w specyfikacji interfejsu API.

![Format importu interfejsu API][api-management-import-api-clipboard]

Po podaniu specyfikacja interfejsu API, użyj przycisków radiowych po prawej stronie, aby wskazać specyfikacji formatu. Obsługiwane są następujące formaty.

* WADL
* Swagger

Następnie wprowadź **sufiks adresu URL interfejsu API sieci Web**. To jest dołączany do podstawowego adresu URL dla interfejsu API usługi zarządzania. Element podstawowy adres URL jest wspólne dla wszystkich interfejsów API hostowanych na każde wystąpienie usługi Zarządzanie interfejsami API. Zarządzanie interfejsami API odróżnia interfejsy API według ich sufiks i w związku z tym sufiks muszą być unikatowe dla każdego interfejsu API w określonym wystąpieniu usługi interfejsu API zarządzania.

Po wprowadzeniu wszystkich wartości kliknij **zapisać** do tworzenia interfejsu API i skojarzone operacji. 

> [!NOTE]
> Samouczek importowania podstawowe Kalkulator interfejsu API w formacie struktury Swagger, zobacz [Zarządzanie pierwszy interfejs API usługi Azure API Management](api-management-get-started.md).
> 
> 

## <a name="export-api"></a> Wyeksportować interfejsu API
Oprócz importowania nowych interfejsów API, możesz wyeksportować definicje swoje interfejsy API z portalu wydawcy. Aby to zrobić, kliknij przycisk **wyeksportować interfejsu API** z **karta Podsumowanie** z Twojej **interfejsu API**.

![Eksportuj interfejsu API][api-management-export-api]

Interfejsy API można wyeksportować za pomocą WADL lub struktury Swagger. Zaznacz pożądany format, kliknij przycisk **zapisać**i wybierz lokalizację, w której chcesz zapisać plik.

![Format eksportu interfejsu API][api-management-export-api-format]

## <a name="next-steps"> </a>Następne kroki
Po utworzeniu interfejsu API i operacje zaimportowane, możesz przejrzeć i skonfigurować dodatkowe ustawienia, Dodaj interfejs API produktu, a następnie opublikować go, aby była ona dostępna dla deweloperów. Aby uzyskać więcej informacji zobacz następujące przewodniki.

* [Jak skonfigurować ustawienia interfejsu API][How to configure API settings]
* [Jak utworzyć i opublikować produktu][How to create and publish a product]

[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to create APIs]: api-management-howto-create-apis.md
[How to configure API settings]: api-management-howto-create-apis.md#configure-api-settings
