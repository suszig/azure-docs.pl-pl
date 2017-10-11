---
title: "Tworzenie interfejsów API w usłudze Azure API Management"
description: "Dowiedz się, jak utworzyć i skonfigurować interfejsów API w usłudze Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 14c20da4-f29f-4b28-bec7-3d4c50b734da
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: ab08256fbc3caca05bf23a12016ad2acf4fc7412
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-apis-in-azure-api-management"></a>Tworzenie interfejsów API w usłudze Azure API Management
Interfejs API w usłudze API Management reprezentuje zestaw operacji, które może być wywoływany przez aplikacje klienckie. Nowe interfejsy API są tworzone w portalu wydawcy, a następnie są dodawane żądanej operacji. Po dodaniu działania, interfejsu API jest dodawany do produktu i mogą być publikowane. Po opublikowaniu interfejsu API można zasubskrybować i używane przez programistów.

Ten przewodnik przedstawia pierwszym krokiem w procesie: jak tworzyć i konfigurować nowy interfejs API w usłudze API Management. Aby uzyskać więcej informacji na operacje dodawania i publikowania produktu, zobacz [sposób dodawania działań do interfejsu API] [ How to add operations to an API] i [jak tworzyć i publikować produktu][How to create and publish a product].

## <a name="create-new-api"></a>Utwórz nowy interfejs API
Interfejsy API są tworzone i skonfigurować w portalu wydawcy. Aby uzyskać dostęp do portalu wydawcy, kliknij przycisk **portal wydawcy** w portalu Azure usługi Zarządzanie interfejsami API.

![Portal wydawcy][api-management-management-console]

> Jeśli jeszcze nie masz utworzonego wystąpienia usługi API Management, zobacz [Tworzenie wystąpienia usługi API Management][Create an API Management service instance] w samouczku [Wprowadzenie do usługi Azure API Management][Get started with Azure API Management].
> 
> 

Kliknij przycisk **interfejsów API** z **zarządzanie interfejsami API** menu po lewej stronie, a następnie kliknij przycisk **dodać interfejsu API**.

![Tworzenie interfejsu API][api-management-create-api]

Użyj **dodać nowy interfejs API** okno, aby skonfigurować nowy interfejs API.

![Dodawanie nowego interfejsu API][api-management-add-new-api]

Następujące pola są używane do konfigurowania nowego interfejsu API.

* **Nazwa interfejsu API sieci Web** to unikatowa i opisowa nazwa interfejsu API. Jest on wyświetlany w portalach deweloperów i wydawcy.
* **Adres URL usługi sieci Web** odwołuje się do usługi HTTP implementacja interfejsu API. Zarządzanie interfejsami API przekazuje żądania do tego adresu.
* **Sufiks adresu URL interfejsu API sieci Web** jest dołączany do podstawowego adresu URL dla interfejsu API usługi zarządzania. Element podstawowy adres URL jest wspólne dla wszystkich interfejsów API hostowanych przez wystąpienie usługi Zarządzanie interfejsami API. Zarządzanie interfejsami API odróżnia interfejsy API według ich sufiks i w związku z tym sufiks muszą być unikatowe dla każdego interfejsu API dla danego wydawcy.
* **Schemat adresu URL interfejsu API sieci Web** określa protokoły mogą być używane do dostępu do interfejsu API. HTTPs jest określony, domyślnie.
* Opcjonalnie można dodać ten nowy interfejs API do produktu, kliknij przycisk **produktów (opcjonalnie)** listy rozwijanej i wybierz produkt. Ten krok można powtarzać wielokrotnie dodać interfejsu API do wielu produktów.

Po skonfigurowaniu odpowiednie wartości, kliknij przycisk **zapisać**. Po utworzeniu nowego interfejsu API, stronie podsumowania dla interfejsu API jest wyświetlana w portalu wydawcy.

![Podsumowanie interfejsu API][api-management-api-summary]

## <a name="configure-api-settings"></a>Ustawień skonfiguruj interfejsu API
Można użyć **ustawienia** kartę, aby sprawdzić i Edytuj konfigurację interfejsu API. **Nazwa interfejsu API sieci Web**, **adres URL usługi sieci Web**, i **sufiks adresu URL interfejsu API sieci Web** początkowo są ustawiane podczas interfejsu API jest tworzony i mogą być modyfikowane w tym miejscu. **Opis** zapewnia opcjonalny opis, a **schemat adresu URL interfejsu API sieci Web** określa protokoły mogą być używane do dostępu do interfejsu API.

![Ustawienia interfejsu API][api-management-api-settings]

Aby skonfigurować uwierzytelnianie bramy dla implementacji interfejsu API usługi wewnętrznej bazy danych, wybierz **zabezpieczeń** kartę. **Przy użyciu poświadczeń** listy rozwijanej może służyć do konfigurowania **podstawowe HTTP** lub **certyfikaty klienta** uwierzytelniania. Aby użyć uwierzytelniania podstawowego HTTP, wystarczy wprowadzić odpowiednie poświadczenia. Uzyskać przy użyciu uwierzytelniania certyfikatu klienta, zobacz [zabezpieczania usług zaplecza za pomocą klienta uwierzytelniania certyfikatów w usłudze Azure API Management][How to secure back-end services using client certificate authentication in Azure API Management].

**Zabezpieczeń** kartę można również skonfigurować **autoryzacji użytkownika** przy użyciu protokołu OAuth 2.0. Aby uzyskać więcej informacji, zobacz [sposób autoryzowania konta dewelopera przy użyciu protokołu OAuth 2.0 w usłudze Azure API Management][How to authorize developer accounts using OAuth 2.0 in Azure API Management].

![Ustawień uwierzytelniania podstawowego][api-management-api-settings-credentials]

Kliknij przycisk **zapisać** zapisać jakiekolwiek zmiany ustawień interfejsu API.

## <a name="next-steps"> </a>Następne kroki
Po utworzeniu interfejsu API i skonfigurowane ustawienia następne kroki jest dodać operacje interfejsu API, Dodaj interfejs API produktu, a następnie opublikować go, aby była ona dostępna dla deweloperów. Aby uzyskać więcej informacji zobacz następujące artykuły.

* [Jak dodać operacje do interfejsu API][How to add operations to an API]
* [Jak utworzyć i opublikować produktu][How to create and publish a product]

[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How to secure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How to authorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
