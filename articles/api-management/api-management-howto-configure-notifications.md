---
title: "Konfigurowanie powiadomień i szablonów w usłudze Azure API Management wiadomości e-mail | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować powiadomienia i szablonów w usłudze Azure API Management wiadomości e-mail."
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
ms.openlocfilehash: ec560bbab3caf4cde090ed3c9a47ccc0afcb2492
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>How to configure notifications and email templates in Azure API Management
Zarządzanie interfejsami API pozwala, aby skonfigurować powiadomienia dla określonych zdarzeń i konfigurowania szablonów wiadomości e-mail, które są używane do komunikacji z Administratorzy i deweloperzy wystąpienia interfejsu API zarządzania. W tym temacie pokazano, jak skonfigurować powiadomienia o zdarzeniach dostępny i zawiera omówienie konfigurowania szablonów wiadomości e-mail używany dla tych zdarzeń.

## <a name="publisher-notifications"></a>Skonfigurować powiadomienia o wydawcy
Kliknij, aby skonfigurować powiadomienia **portal wydawcy** w portalu Azure usługi Zarządzanie interfejsami API. Spowoduje to przejście do portalu wydawcy usługi API Management.

![Portal wydawcy][api-management-management-console]

> [!NOTE] 
> Jeśli jeszcze nie utworzono wystąpienie usługi API Management, zobacz [Utwórz wystąpienie usługi Zarządzanie interfejsami API][Create an API Management service instance].

Kliknij przycisk **powiadomienia** z **zarządzanie interfejsami API** menu po lewej stronie, aby wyświetlić dostępnych powiadomień.

![Wydawcy powiadomień][api-management-publisher-notifications]

Poniższa lista zdarzeń można skonfigurować dla powiadomienia.

* **Żądania subskrypcji (wymaganie zatwierdzenia)** -adresatów wiadomości e-mail określonego i użytkowników będzie otrzymywał powiadomienia e-mail o żądaniach subskrypcji dla produktów interfejsu API, wymaganie zatwierdzenia.
* **Nowe subskrypcje** -adresatów wiadomości e-mail określonego i użytkowników będzie otrzymywał powiadomienia e-mail o nowe subskrypcje produktu interfejsu API.
* **Żądania galerii aplikacji** -adresatów wiadomości e-mail określonego i użytkowników będzie otrzymywał powiadomienia e-mail, gdy nowe aplikacje są przesyłane do galerii aplikacji.
* **UDW** -adresatów wiadomości e-mail określonego i użytkownicy otrzymają wiadomość e-mail kopie wszystkich wiadomości e-mail wysyłanych do deweloperów.
* **Nowy problem lub komentarz** — adresatów wiadomości e-mail określonego i użytkownicy będą otrzymywać powiadomienia e-mail, gdy nowy problem lub komentarz jest przesyłany w portalu dla deweloperów.
* **Komunikat zamknięcia konta** -adresatów wiadomości e-mail określony i użytkownicy będą otrzymywać powiadomienia e-mail, po zamknięciu konta.
* **Limit przydziału subskrypcji o** -następujących adresatów wiadomości e-mail i użytkowników będzie otrzymywał powiadomienia e-mail, gdy użycie subskrypcji pobiera możliwości wykorzystania przydziału użycia.

Dla każdego zdarzenia można określić adresatów wiadomości e-mail w polu tekstowym adres e-mail lub użytkownicy mogą wybrać z listy.

Aby określić adresy e-mail, aby otrzymywać powiadomienia, wprowadź je w polu tekstowym adres e-mail. Jeśli masz wiele adresów e-mail, oddzielając je przecinkami.

![Adresatów powiadomień][api-management-email-addresses]

Aby określić należy powiadomić użytkowników, kliknij przycisk **Dodaj adresata**, zaznacz pole obok użytkowników, aby otrzymywać powiadomienia, a następnie kliknij przycisk **OK**.

> [!NOTE] 
> Tylko administratorzy są wyświetlane na liście.


Po skonfigurowaniu odbiorców powiadomień, kliknij przycisk **zapisać** dotyczyć odbiorców powiadomień zaktualizowane.

> [!NOTE] 
> Jeśli oddalisz się od **wydawcy powiadomień** kartę portalu wydawcy generuje alert, jeśli istnieją niezapisane zmiany.


## <a name="email-templates"></a>Konfigurowanie szablonów wiadomości e-mail
Zarządzanie interfejsami API udostępnia szablony wiadomości e-mail dla wiadomości e-mail, które są wysyłane w ramach zarządzania i korzystania z usługi. Znajdują się następujące szablony wiadomości e-mail.

* Zatwierdzone przesyłanie galerii aplikacji
* Litera farewell Developer
* Limit przydziału Developer zbliża się powiadomień
* Zaproś użytkownika
* Nowy komentarz dodany do problemu
* Nowy problem odebranych
* Nowa subskrypcja aktywowany
* Potwierdzenie odnowić subskrypcję
* Odrzuci to żądanie subskrypcji
* Odebrano żądanie subskrypcji

Te szablony mogą być modyfikowane pożądane.

Kliknij, aby wyświetlić i skonfigurować szablony wiadomości e-mail dla swojego wystąpienia usługi API Management **powiadomienia** z **zarządzanie interfejsami API** menu po lewej stronie, a następnie wybierz **szablony wiadomości E-mail**kartę.

![Szablony wiadomości e-mail][api-management-email-templates]

Aby wyświetlić lub zmodyfikować szablon, wybierz go z **szablony** listy rozwijanej.

![Lista szablonów wiadomości e-mail][api-management-email-templates-list]

Każdy szablon wiadomości e-mail ma podmiotu w postaci zwykłego tekstu i definicji treści w formacie HTML. Każdy element można dostosować pożądane.

![Edytor szablonu wiadomości e-mail][api-management-email-template]

**Parametry** lista zawiera listę parametrów, służący wstawione do tematu lub treści, zostanie zastąpiony wyznaczonej wartości, po wysłaniu wiadomości e-mail. Aby wstawić parametru, umieść kursor, który mają parametr, aby przejść, a następnie kliknij przycisk strzałki w lewo nazwę parametru.

Kliknij przycisk **Podgląd** lub **wysłać teście** aby zobaczyć, jak wiadomości e-mail będą wyglądać lub Wyślij testową wiadomość e-mail.

> [!NOTE] 
> Parametry nie są zastępowane rzeczywistymi wartościami podczas przeglądania lub wysyłanie testu.

Aby zapisać zmiany szablonu wiadomości e-mail, kliknij polecenie **zapisać**, lub Anuluj zmiany kliknij **anulować**.
 

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
