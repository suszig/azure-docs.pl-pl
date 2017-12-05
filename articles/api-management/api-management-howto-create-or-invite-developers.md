---
title: "Jak zarządzać kontami użytkowników w usłudze Azure API Management | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć lub zaprosić użytkowników w usłudze Azure API Management"
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
ms.openlocfilehash: 6f2fd5e4c1a51fe9d1652c9970bcd8d76b25ab60
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Jak zarządzać kontami użytkowników w usłudze Azure API Management
W usłudze API Management Deweloperzy są użytkownicy interfejsów API, które można udostępnić przy użyciu interfejsu API zarządzania. Ten przewodnik przedstawia sposobu tworzenia i zapraszanie deweloperów do korzystania z interfejsów API i produktów czy należy udostępnić je z wystąpieniem usługi API Management. Informacje na programowe zarządzanie kontami użytkowników, zobacz [jednostki użytkownika](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentacji w [interfejsu API REST zarządzania](https://msdn.microsoft.com/library/azure/dn776326.aspx) odwołania.

## <a name="create-developer"></a>Tworzenie nowych developer
Aby utworzyć nowy developer, kliknij przycisk **portal wydawcy** w portalu Azure usługi Zarządzanie interfejsami API. Spowoduje to przejście do portalu wydawcy usługi API Management. Jeśli jeszcze nie utworzono wystąpienie usługi API Management, zobacz [Utwórz wystąpienie usługi Zarządzanie interfejsami API][Create an API Management service instance].

![Portal wydawcy][api-management-management-console]

Kliknij przycisk **użytkowników** z **zarządzanie interfejsami API** menu po lewej stronie, a następnie kliknij przycisk **dodać użytkownika**.

![Utwórz developer][api-management-create-developer]

Wprowadź **E-mail**, **hasło**, i **nazwa** developer nowy i kliknij **zapisać**.

![Utwórz developer][api-management-add-new-user]

Domyślnie developer nowo utworzonego konta są **Active**wraz ze skojarzonymi z **deweloperzy** grupy.

![Nowe developer][api-management-new-developer]

Konta dewelopera, które znajdują się w **active** stanu można uzyskać dostęp do wszystkich interfejsów API, do którego mają zostać subskrypcji. Aby skojarzyć developer nowo utworzony z dodatkowych grup, zobacz [sposobu kojarzenia grupy z deweloperami][How to associate groups with developers].

## <a name="invite-developer"></a>Zaprosić dewelopera
Aby zaprosić dewelopera, kliknij przycisk **użytkowników** z **zarządzanie interfejsami API** menu po lewej stronie, a następnie kliknij przycisk **zaprosić użytkowników**.

![Zaproś developer][api-management-invite-developer]

Wprowadź nazwę i adres e-mail dewelopera, a następnie kliknij przycisk **zaprosić**.

![Zaproś developer][api-management-invite-developer-window]

Zostanie wyświetlony komunikat potwierdzenia, ale Deweloper nowo zaproszonych nie znajdują się na liście dopiero po ich zaakceptowaniu zaproszenia. 

![Zaproś potwierdzenia][api-management-invite-developer-confirmation]

Gdy Projektant jest zaproszenie, zostanie wysłana wiadomość e-mail do deweloperów. Ten adres e-mail jest generowana z użyciem szablonu i można dostosowywać. Aby uzyskać więcej informacji, zobacz [Konfigurowanie szablonów wiadomości e-mail][Configure email templates].

Po zaakceptowaniu zaproszenia konto staje się aktywny.

## <a name="block-developer"></a> Dezaktywuj lub ponownym uaktywnieniem konta dewelopera
Domyślnie są konta dewelopera nowo utworzone lub zaproszonych **Active**. Aby zdezaktywować konta dewelopera, kliknij przycisk **bloku**. Aby ponownie uaktywnić konto dewelopera zablokowanych, kliknij przycisk **Aktywuj**. Konto dewelopera zablokowanych nie można uzyskać dostępu do portalu dla deweloperów lub zadzwoń wszystkie interfejsy API. Aby usunąć konto użytkownika, kliknij przycisk **usunąć**.

![Blok developer][api-management-new-developer]

## <a name="reset-a-user-password"></a>Resetowanie hasła użytkownika
Aby zresetować hasło do konta użytkownika, kliknij nazwę konta.

![Resetowanie hasła][api-management-view-developer]

Kliknij przycisk **resetowania hasła** wysłać łącze do użytkownika, aby zresetować swoje hasło.

![Resetowanie hasła][api-management-reset-password]

Aby programowo pracować z kontami użytkowników, zobacz [jednostki użytkownika](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentacji w [interfejsu API REST zarządzania](https://msdn.microsoft.com/library/azure/dn776326.aspx) odwołania. Aby zresetować hasło do konta użytkownika na określoną wartość, można użyć [zaktualizowania użytkownika](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) operację i określ odpowiednie hasło.

## <a name="pending-verification"></a>Oczekujące weryfikacji
![Oczekujące weryfikacji][api-management-pending-verification]

## <a name="next-steps"> </a>Następne kroki
Po utworzeniu konta dewelopera, można ją skojarzyć z rolami i subskrybowania produktów i interfejsów API. Aby uzyskać więcej informacji, zobacz [sposobu tworzenia i używania grup][How to create and use groups].

[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png


[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
