---
title: "Dodawanie lub zmienianie ról subskrypcji Azure administratora | Dokumentacja firmy Microsoft"
description: "Opisuje, jak dodać lub zmienić administratora współpracującego Azure, Administrator usługi i konto administratora"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/19/2017
ms.author: genli
ms.openlocfilehash: 178d7fbc17464f7c9a4bc891453d0999e4ba62c3
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2017
---
# <a name="add-or-change-azure-administrator-roles-that-manage-the-subscription-or-services"></a>Dodać lub zmienić role administratora platformy Azure, zarządzających subskrypcji lub usług

Możesz zmienić administratora platformy Azure, który zarządza subskrypcji platformy Azure ani nie zarządza nimi usług Azure używane w ramach subskrypcji. Do wyświetlania informacji dotyczących rozliczeń platformy Azure i zarządzać subskrypcjami, należy zalogować się do Centrum konta jako administratora konta. 

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-admin-for-a-subscription-in-azure-portal"></a>Dodawanie administratora właściciela RBAC dla subskrypcji w portalu Azure 

Aby dodać osobę administratora dla subskrypcji w portalu Azure, zalecamy nadanie im [RBAC](../active-directory/role-based-access-control-configure.md) rolę właściciela. Rola właściciela można zarządzać zasobów w subskrypcji przypisany i nie ma uprawnień dostępu do innych subskrypcji. Właściciele dodać za pomocą [portalu Azure](https://portal.azure.com) nie może zarządzać zasobem w [klasycznego portalu Azure](https://manage.windowsazure.com).

1. Zaloguj się do [subskrypcje wyświetlić w portalu Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Wybierz subskrypcję, która ma administratora, aby uzyskać dostęp.
1. Wybierz **(IAM) kontroli dostępu** w menu.
1. Wybierz **dodać** > **roli** > **właściciela**. Wpisz adres e-mail użytkownika, aby dodać jako właściciela, wybierz użytkownika, a następnie wybierz **zapisać**.

    ![Zrzut ekranu pokazujący rolę właściciela wybrane](./media/billing-add-change-azure-subscription-administrator/add-role.png)

## <a name="add-or-change-co-administrator"></a>Dodawanie lub zmienianie współadministratorem

Jako współadministrator można dodawać tylko jego właściciel. Inni użytkownicy z ról, takich jak współautora i czytnika nie można dodać jako współadministratorów.

1. Jeśli nie jest jeszcze dodać osobę jako właściciela, postępując zgodnie z instrukcjami z powyżej.
2. **Kliknij prawym przyciskiem myszy** użytkownika właściciela zostanie dodany, a następnie wybierz **Dodaj jako współadministrator**. Jeśli nie widzisz **Dodaj jako współadministrator** opcję świeże strony lub spróbuj użyć innej przeglądarki internetowej. 

     ![Zrzut ekranu, który dodaje współadministratorem](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    >[!TIP]
    >Należy dodać konto "Właściciela" jako współadministrator, jeśli użytkownik chce zarządzać usług platformy Azure w [klasycznego portalu Azure](https://manage.windowsazure.com/).

    Aby usunąć uprawnienia administratora współpracującego **kliknij prawym przyciskiem myszy** użytkownika "Współadministratorem", a następnie wybierz **usunąć administratora współpracującego**.

    ![Zrzut ekranu, która usuwa współadministratorem](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Zmiany administratora usługi dla subskrypcji platformy Azure

Tylko Administrator konta może zmienić administratora usługi dla subskrypcji. Domyślnie po utworzeniu konta, administratora usługi jest taki sam jak konto administratora.

1. Upewnij się, że dany scenariusz jest obsługiwany przez sprawdzanie [limity dla administratorów usługi zmiana](#limits).
1. Zaloguj się do [Centrum konta](https://account.windowsazure.com/subscriptions) jako administratora konta.
1. Wybierz subskrypcję.
1. Po prawej stronie wybierz **Edytuj szczegóły subskrypcji**.

    ![Zrzut ekranu przedstawiający przycisk Edytuj subskrypcji w Centrum konta](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. W **administratora usługi** wprowadź adres e-mail nowego administratora usługi.

    ![Zrzut ekranu przedstawiający okno Zmień adres e-mail administratora usługi](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Ograniczenia dotyczące zmiana Administratorzy usługi

* Każda subskrypcja jest skojarzona z katalogu usługi Azure AD. Aby znaleźć subskrypcja jest skojarzona z katalogu, przejdź do [klasycznego portalu Azure](https://manage.windowsazure.com/), wybierz pozycję **ustawienia** > **subskrypcje**. Sprawdź identyfikator subskrypcji można znaleźć katalogu.
* Jeśli logujesz się przy użyciu konta służbowego, można dodać inne konta w organizacji jako administratora usługi. Na przykład abby@contoso.com można dodać bob@contoso.com jako administratora usługi, ale nie można dodać john@notcontoso.com chyba że john@notcontoso.com ma obecności w katalogu contoso.com. Użytkownicy zalogowany za pomocą konta służbowego można kontynuować dodawanie użytkowników Account Microsoft jako administratora usługi.

  | Metoda logowania | Dodaj Account Microsoft użytkownika jako administratora systemu? | Dodaj konto służbowe w tej samej organizacji jako SA? | Dodaj konto służbowe w innej organizacji jako SA? |
  | --- | --- | --- | --- |
  |  Konto Microsoft |Tak |Nie |Nie |
  |  Konto służbowe |Tak |Tak |Nie |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Zmienić administratora konta subskrypcji platformy Azure

Aby zmienić administratora konta subskrypcji, zobacz [przeniesienia własności subskrypcji platformy Azure na inne konto](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Nie masz pewności, który jest administratorem konta?** Wykonaj następujące kroki:

1. Zaloguj się do [subskrypcje wyświetlić w portalu Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Wybierz subskrypcję, którą chcesz sprawdzić, a następnie sprawdź w obszarze **ustawienia**.
1. Wybierz **właściwości**. Administratora konta subskrypcji jest wyświetlany w **administrator konta** pole.  

## <a name="types-of-azure-admin-accounts"></a>Typy kont administratora platformy Azure

 Konto administratora, Administrator usługi i współadministrator są trzy rodzaje ról administratorów w Microsoft Azure. W poniższej tabeli opisano różnice między tych trzech ról administracyjnych.

| Rola administratora | Limit | Opis |
| --- | --- | --- |
| Konto administratora (AA) |1 na konto platformy Azure |To jest osobą, która utworzył konto na lub zakupiono subskrypcji platformy Azure i uprawnień dostępu [Centrum konta](https://account.azure.com/Subscriptions) i wykonywać różne zadania zarządzania. Obejmują one, można tworzyć subskrypcje, anulowanie subskrypcji, zmienić rozliczeń dla subskrypcji oraz zmienić administratora usługi. |
| Administrator usługi (SA) |1 na subskrypcji platformy Azure |Ta rola jest autoryzowany do zarządzania usługami w [portalu Azure](https://portal.azure.com). Domyślnie na nową subskrypcję konto administratora jest również administratorem usługi. |
| Współadministrator (CA) w [klasycznego portalu Azure](https://manage.windowsazure.com) |200 na subskrypcję |Ta rola ma takie same uprawnienia dostępu jak administrator usługi, ale nie może zmieniać skojarzenia subskrypcji do katalogów platformy Azure. |

Azure opartej na roli Active Directory kontroli dostępu (RBAC) umożliwia użytkownikom można dodać do wielu ról. Aby uzyskać więcej informacji, zobacz [kontroli dostępu opartej na roli Azure Active Directory](../active-directory/role-based-access-control-configure.md).


## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Dowiedz się więcej na temat kontroli dostępu do zasobów i usługi Active Directory

* Aby dowiedzieć się więcej na temat sposobu jest kontrolowany dostęp do zasobów w systemie Microsoft Azure, zobacz [opis dostęp do zasobów na platformie Azure](../active-directory/active-directory-understanding-resource-access.md).
* Aby uzyskać więcej informacji o usłudze Azure Active Directory, zobacz [jak subskrypcje platformy Azure są kojarzone z usługi Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) i [przypisywanie ról administratorów w usłudze Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Potrzebujesz pomocy? Skontaktuj się z pomocą techniczną.

Jeśli nadal potrzebujesz pomocy, [się z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) uzyskać szybkie rozwiązanie problemu.
