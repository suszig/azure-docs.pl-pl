---
title: "Użyj dzierżawy usługi Office 365 z subskrypcją platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dodać katalog usługi Office 365 (dzierżawcy) do subskrypcji platformy Azure."
services: 
documentationcenter: 
author: JiangChen79
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: cc9c57c6-7bfd-4dea-9027-c75ef3737589
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/13/2017
ms.author: cjiang
ms.openlocfilehash: e6300932d044ec9a4f88eb5bd5977220ed11d513
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="link-an-office-365-tenant-to-an-azure-subscription"></a>Link dzierżawy usługi Office 365 z subskrypcją platformy Azure
Połącz oddzielne subskrypcji platformy Azure i usługi Office 365, umożliwiając dostęp do dzierżawy usługi Office 365 z subskrypcją platformy Azure. Aby połączyć subskrypcji, logowanie do platformy Azure przy użyciu konta administratora usługi Azure, Dodaj katalog, a następnie dodaj usługi Office 365 pracy lub nauki konta do dzierżawy usługi Azure Active Directory.

**Chcesz przenieść swoją subskrypcję platformy Azure do pracy usługi Office 365 lub konta służbowego?** Jeśli konta platformy Azure przy użyciu osobistego konta Microsoft i mają być używane, lub zaloguj się przy użyciu konta usługi Office 365, zdecydowanie zaleca się transfer subskrypcji. Zobacz [Transfer Azure własności subskrypcji na inne konto](billing-subscription-transfer.md). 

**Czy chcesz utworzyć konto platformy Azure przy użyciu usługi Office 365?** Zobacz [tworzenia konta platformy Azure z kontem usługi Office 365](billing-use-existing-office-365-account-azure-subscription.md). 

## <a name="before-you-begin"></a>Przed rozpoczęciem
* Musi mieć poświadczenia administratora usługi subskrypcji platformy Azure. Konta administratora współpracującego nie można wykonać niektóre z opisanych w tym artykule. Aby zmienić administratorem usługi, zobacz [jak dodać lub zmienić role administratora platformy Azure](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).
* Musi mieć poświadczenia administratora globalnego dzierżawy usługi Office 365.
* Adres e-mail administratora usługi nie może być w dzierżawie usługi Office 365.
* Adres e-mail administratora usługi nie musi odpowiadać z administratorami globalnymi dzierżawy usługi Office 365.
* Jeśli używany jest adres e-mail konta Microsoft i konta organizacyjnego, tymczasowo zmienić administratorem usługi subskrypcji platformy Azure, aby korzystać z innego konta Microsoft. Możesz utworzyć konto Microsoft, w [strony Rejestracja konta Microsoft](https://signup.live.com/).

## <a name="link-office-365-tenant-to-azure-subscription"></a>Dzierżawy usługi Office 365 Link do subskrypcji platformy Azure
Aby skojarzyć dzierżawy usługi Office 365 z subskrypcją platformy Azure, wykonaj następujące kroki:

### <a name="step-1-add-office-365-tenant-to-your-azure-subscription"></a>Krok 1: Dodaj dzierżawy usługi Office 365 do subskrypcji platformy Azure

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) przy użyciu poświadczeń administratora usługi.

    ![Zrzut ekranu Azure logowanie](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
2. W okienku po lewej stronie wybierz **usługi ACTIVE DIRECTORY**. Niepowołanymi dzierżawy usługi Office 365. Jeśli zostanie wyświetlony, przejdź do [krok 2: Zmień katalog skojarzony z subskrypcją platformy Azure](#Step2).
   
   ![Wpis zrzut ekranu z usługi Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)
3. Wybierz **nowy** > **katalogu** > **tworzenie NIESTANDARDOWYCH**.
   
    ![Utwórz niestandardowy zrzut ekranu z usługą Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
4. Na **Dodaj katalog** w obszarze **katalogu**, wybierz pozycję **Użyj istniejącego katalogu**. Następnie wybierz **chcę się teraz wylogować**i wybierz **Complete** ![ukończyć ikona](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Zrzut ekranu przedstawiający "Użyj istniejącego katalogu"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
5. Po zarejestrowaniu zalogować się przy użyciu poświadczeń administratora globalnego dzierżawy usługi Office 365.
   
    ![Logowanie administratora globalnego zrzut ekranu usługi Office 365](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
6. Wybierz **kontynuować**.
   
    ![Zrzut ekranu przedstawiający weryfikacji](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
7. Wybierz **Wyloguj się teraz**.
   
    ![Zrzut ekranu przedstawiający wylogowania](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
8. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) przy użyciu poświadczeń administratora usługi.
   
    ![Zrzut ekranu Azure logowanie](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
9. Powinny pojawić się dzierżawą usługi Office 365 na pulpicie nawigacyjnym.
   
    ![Zrzut ekranu przedstawiający pulpit nawigacyjny](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <a name="Step2"></a>Krok 2: Zmień katalog skojarzony z subskrypcją platformy Azure
   
1. Wybierz **ustawienia**.
   
    ![Zrzut ekranu Azure ikona klasycznych ustawień portalu](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
2. Wybierz subskrypcję platformy Azure, a następnie wybierz **Edytuj katalog**.

    ![Zrzut ekranu Azure subskrypcji edycji katalogu](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
3. Wybierz **dalej** ![dalej](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).
   
    ![Zrzut ekranu przedstawiający "Zmień katalog skojarzony"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
4. Przejrzyj konta, których dotyczą. W przypadku wszystkich administratorów wspólnej i [kontroli dostępu opartej na rolach (RBAC)](../active-directory/role-based-access-control-configure.md) użytkownikom dostępu przypisanego do istniejących grup zasobów zostaną usunięte. Ostrzeżenie, które otrzymujesz wymienia tylko usunięcie współadministratorów.
      
    ![Zrzut ekranu pokazujący kont administratora współpracującego do usunięcia.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Zrzut ekranu przedstawia przykład konta użytkownika do usunięcia.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
5. Wybierz **Complete** ![ukończyć ikona](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

### <a name="step-3-add-your-office-365-organizational-accounts-as-admins-to-the-azure-subscription"></a>Krok 3: Dodawanie konta organizacyjnego usługi Office 365 jako administratorzy subskrypcji platformy Azure
   
Aby dodać administratora do subskrypcji platformy Azure, zobacz [Dodawanie lub zmienianie role administratora platformy Azure, zarządzających subskrypcji lub usług](billing-add-change-azure-subscription-administrator.md)

## <a name="need-help-contact-support"></a>Potrzebujesz pomocy? Skontaktuj się z pomocą techniczną.

Jeśli nadal potrzebujesz pomocy, [się z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) uzyskać szybkie rozwiązanie problemu.

