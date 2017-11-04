---
title: "Zarejestruj subskrypcji platformy Azure z usługą Azure Management koszt | Dokumentacja firmy Microsoft"
description: "Użyj subskrypcji platformy Azure do rejestracji w usłudze Azure kosztów zarządzania przez Cloudyn."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: 4fed181a004ba6a5ea35dcbe1366e7d48342c96d
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2017
---
# <a name="register-an-individual-azure-subscription-and-view-cost-data"></a>Zarejestruj poszczególnych subskrypcji platformy Azure i wyświetlanie kosztów danych

Subskrypcji platformy Azure jest używany do rejestrowania usługi Azure Management koszt przez Cloudyn. Rejestracja umożliwia dostęp do portalu Cloudyn. Szczegóły tego szybkiego startu procesu rejestracji potrzebne do tworzenia subskrypcji wersji próbnej Cloudyn i zaloguj się do portalu Cloudyn. On również pokazano, jak zacząć od razu wyświetlanie danych kosztów.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

- Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="create-a-trial-registration"></a>Tworzenie wersji próbnej rejestracji

1. W portalu Azure kliknij **kosztów zarządzania + rozliczeń** na liście usług.
2. W obszarze **omówienie**, kliknij przycisk **koszt zarządzania**  
    ![Koszt strony zarządzania](./media/quick-register-azure-sub/cost-mgt-billing-service.png)
3. Na **kosztów zarządzania** kliknij przycisk **przejdź do zarządzania koszt** aby otworzyć stronę rejestracji Cloudyn w nowym oknie.
4. Na stronie portalu rejestracji wersji próbnej Cloudyn wpisz nazwę firmy, a następnie wybierz **poszczególnych właściciel subskrypcji Azure** , a następnie kliknij przycisk **dalej**. Nazwa konta i identyfikator dzierżawcy jest automatycznie dodawany do formularza.  
    ![Wersja próbna rejestracji](./media/quick-register-azure-sub/trial-reg-ind.png)
5. Wybierz użytkownika **oferują Identyfikatora — nazwa** skojarzonych z Twoją subskrypcją. Jeśli masz pewności, co to jest identyfikator szybkości dla Twojej subskrypcji, można wyświetlić rachunku Azure i wyszukaj **identyfikator oferty**.
6. Akceptuję warunki użytkowania, a następnie zweryfikować informacje, a następnie kliknij przycisk **dalej**.
7. W **zebrać dodatkowe dane** kliknij przycisk **dalej** do autoryzacji Cloudyn zbierać dane zasobów platformy Azure. Zbierane dane obejmują użycia i wydajności, rozliczeń i dane znacznika z subskrypcji.  
    ![zbieranie dodatkowych danych](./media/quick-register-azure-sub/gather-additional.png)
8. Przeglądarkę przejście na stronę logowania dla Cloudyn. Zaloguj się przy użyciu poświadczeń subskrypcji platformy Azure.
9. Kliknij przycisk **przejdź do Cloudyn** aby otworzyć Cloudyn portal, a następnie na **Zarządzanie kontami** strony, powinny pojawić się dane konta subskrypcji platformy Azure.  
    ![Zarządzanie kontami](./media/quick-register-azure-sub/accounts-mgt.png)

Aby obejrzeć film samouczek dotyczący rejestrowania subskrypcji platformy Azure, zobacz [znajdowanie do użycia w Azure kosztów zarządzania przez Cloudyn Twojego katalogu identyfikator GUID i identyfikator szybkość](https://youtu.be/PaRjnyaNGMI).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Następne kroki

W tym szybkiego startu informacji o subskrypcji platformy Azure jest używane do rejestrowania przy kosztów zarządzania. Również zalogowaniem się do portalu Cloudyn i uruchomić, wyświetlanie danych kosztów. Aby dowiedzieć się więcej na temat zarządzania koszt Azure przez Cloudyn, nadal samouczka kosztów zarządzania.

> [!div class="nextstepaction"]
> [Przejrzyj użycia i kosztów](./tutorial-review-usage.md)
