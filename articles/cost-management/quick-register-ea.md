---
title: "Rejestracja umowy Enterprise Agreement platformy Azure w rozwiązaniu Azure Cost Management | Microsoft Docs"
description: "Zarejestruj się w usłudze Azure Cost Management firmy Cloudyn przy użyciu umowy Enterprise Agreement platformy Azure."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: 41a9df712b07253d9f5f9db8542fb9917592320f
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2017
---
# <a name="register-an-azure-enterprise-agreement-and-view-cost-data"></a>Rejestracja umowy Enterprise Agreement platformy Azure i wyświetlanie danych kosztów

Do rejestrowania w usłudze Azure Cost Management firmy Cloudyn służy umowa Enterprise Agreement platformy Azure. Rejestracja zapewnia dostęp do portalu firmy Cloudyn. W tym przewodniku Szybki start opisano szczegółowo proces rejestracji niezbędny do utworzenia subskrypcji wersji próbnej rozwiązania Cloudyn i zalogowania w portalu Cloudyn. Przedstawiono w nim także, jak od razu rozpocząć wyświetlanie danych kosztów.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

- Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="create-a-trial-registration"></a>Tworzenie rejestracji wersji próbnej

1. W witrynie Azure Portal kliknij pozycję **Zarządzanie kosztami i rozliczenia** na liście usług.
2. W obszarze **Przegląd** kliknij pozycję **Zarządzanie kosztami**  
    ![Strona Zarządzanie kosztami](./media/quick-register-ea/cost-mgt-billing-service.png)
3. Na stronie **Zarządzanie kosztami** kliknij pozycję **Przejdź do zarządzania kosztami**, aby otworzyć stronę rejestracji rozwiązania Cloudyn w nowym oknie.
4. Na stronie rejestracji wersji próbnej rozwiązania Cloudyn wpisz nazwę swojej firmy i wybierz pozycję **Azure Enterprise Enrollment Administrator** (Administrator rejestracji umowy Enterprise Agreement platformy Azure).  
    ![rejestracja wersji próbnej](./media/quick-register-ea/trial-reg.png)
5. Wprowadź klucz interfejsu API rejestracji Enterprise Portal. Jeśli nie masz klucza pod ręką, kliknij link [Enterprise Portal](https://ea.azure.com) i wykonaj następujące kroki:
  1. Zaloguj się do witryny Azure Enterprise i przycisk **Reports** (Raporty), kliknij przycisk **API Access Key** (Klucz dostępu API), a następnie skopiuj klucz podstawowy.  
    ![Klucz API EA](./media/quick-register-ea/ea-key.png)
  3. Wróć do strony rejestracji i wklej swój klucz API.
6. Zaakceptuj warunki użytkowania i zweryfikuj swój klucz. Kliknij przycisk **Next** (Dalej), aby autoryzować rozwiązanie Cloudyn do zbierania danych zasobów platformy Azure. Te dane obejmują dane dotyczące użycia, wydajności, rozliczeń i tagów z Twoich subskrypcji.  
    ![walidacja klucza](./media/quick-register-ea/ea-key-validated.png)
7. W obszarze **Invite other stakeholders** (Zaproś innych uczestników projektu) możesz dodać użytkowników, wpisując ich adresy e-mail. Po zakończeniu kliknij przycisk **Next** (Dalej). Dodanie wszystkich danych dotyczących rozliczeń do rozwiązania Cloudyn potrwa około 2 godzin.
8. Kliknij pozycję **Go to Cloudyn** (Przejdź do rozwiązania Cloudyn), aby otworzyć portal Cloudyn, a następnie na stronie **Cloud Accounts Management** (Zarządzanie kontami w chmurze) powinny zostać wyświetlone dane Twojego zarejestrowanego konta EA.

Aby obejrzeć wideo dotyczące rejestrowania umowy Enterprise Agreement, zobacz [How to Find Your EA Enrollment ID and API Key for use in Azure Cost Management by Cloudyn](https://youtu.be/u_phLs_udig) (Jak wyszukać swój identyfikator rejestracji EA do użytku w rozwiązaniu Azure Cost Management firmy Cloudyn).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start opisano rejestrację w usłudze Cost Management przy użyciu danych umowy Enterprise Agreement platformy Azure. Przedstawiono także logowanie się do portalu Cloudyn i rozpoczęcie wyświetlania danych kosztów. Aby dowiedzieć się więcej na temat usługi Azure Cost Management firmy Cloudyn, przejdź do samouczka usługi Cost Management.

> [!div class="nextstepaction"]
> [Przeglądanie użycia i kosztów](./tutorial-review-usage.md)
