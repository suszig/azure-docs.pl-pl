---
title: "Zarejestrowane za pomocą informacje o partnerze dostawcy usług Kryptograficznych z usługą Azure Management koszt | Dokumentacja firmy Microsoft"
description: "Używać Twoich informacji partnera dostawcy usług Kryptograficznych, aby zarejestrować w usłudze Azure kosztów zarządzania przez Cloudyn."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: bcb072a2f2ab8c0e5097fca2c95309464483cb53
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="register-with-the-csp-partner-program-and-view-cost-data"></a>Zarejestruj program partnerski dostawcy usług Kryptograficznych i widoku danych kosztów

Jako serwer partnerski dostawcy usług Kryptograficznych można zarejestrować usługi Azure Management koszt przez Cloudyn. Rejestracja umożliwia dostęp do portalu Cloudyn. Szczegóły tego szybkiego startu procesu rejestracji potrzebne do tworzenia subskrypcji wersji próbnej Cloudyn i zaloguj się do portalu Cloudyn. On również pokazano, jak zacząć od razu wyświetlanie danych kosztów.


>[!NOTE]
>Aby ukończyć rejestrację, musi być administratorem programu partnera z dostępu do interfejsu API Center partnera. Konfiguracja interfejsu API Center partnera jest wymagany do uwierzytelniania i dostępu do danych. Aby uzyskać więcej informacji, zobacz [Połącz z interfejsu API Centrum partnerskiego](https://msdn.microsoft.com/library/partnercenter/mt709136.aspx). Ponadto pośrednie dostawcy usług Kryptograficznych użytkowników tylko przy użyciu Cloudyn podczas ich bezpośredniego sprzedawcą dostawcy usług Kryptograficznych rejestruje Cloudyn w celu zapewnienia dostępu do swoich klientów i subskrypcje.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

- Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="create-a-trial-registration"></a>Tworzenie wersji próbnej rejestracji

1. W portalu Azure kliknij **kosztów zarządzania + rozliczeń** na liście usług.
2. W obszarze **omówienie**, kliknij przycisk **koszt zarządzania**  
    ![Koszt strony zarządzania](./media/quick-register-csp/cost-mgt-billing-service.png)
3. Na **kosztów zarządzania** kliknij przycisk **przejdź do zarządzania koszt** aby otworzyć stronę rejestracji Cloudyn w nowym oknie.
4. Na stronie portalu rejestracji wersji próbnej Cloudyn wpisz nazwę firmy, wybierz **Administrator programu partnera dostawcy usług Kryptograficznych Microsoft**, a następnie kliknij przycisk **dalej**.  
5. Wprowadź **identyfikator aplikacji**, **Identyfikatora Commerce**, **klucz tajny aplikacji**i wybierz **domyślne cennik Planowanie**. Jeśli nie masz informacje przydatne, zaloguj się do portalu Centrum partnerskiego pod adresem [https://partnercenter.microsoft.com](https://partnercenter.microsoft.com) głównej administratora konta, a następnie wykonaj następujące czynności:
  1. Przejdź do **pulpitu nawigacyjnego** > **ustawienia konta** > **Zarządzanie aplikacjami**.
  2. Jeśli uprzednio utworzono aplikację sieci Web, Pomiń ten krok. W przeciwnym razie kliknij przycisk **Dodaj nową aplikację sieci web** w **aplikacji sieci Web** sekcji.
  3. Kopiuj **identyfikator aplikacji** identyfikatora GUID z aplikacji sieci web.
  4. Kopiuj **Identyfikatora Commerce** identyfikatora GUID z aplikacji sieci web.
  5. Wybierz okres ważności klucza jako jeden lub dwa lata, zgodnie z potrzebami. Wybierz **Dodaj klucz** , a następnie skopiuj i Zapisz wartość klucza tajnego.  
    ![Centrum partnerskiego dostawcy usług Kryptograficznych](./media/quick-register-csp/csp-partner-center.png)
  6. Wróć do strony rejestracji i Wklej informacje.  
      ![Poświadczenia konta dostawcy usług Kryptograficznych](./media/quick-register-csp/csp-reg.png)
6. Akceptuję warunki użytkowania, a następnie sprawdź poprawność informacji. Kliknij przycisk **dalej** do autoryzacji Cloudyn zbierać dane zasobów platformy Azure. Zbierane dane obejmują użycia i wydajności, rozliczeń i dane znacznika z subskrypcji.  
7. W obszarze **zapraszanie innych uczestników**, możesz dodać użytkowników, wpisując ich adresów e-mail. Po zakończeniu kliknij przycisk **dalej**. Trwa około dwie godziny dla wszystkich danych rozliczeń do zostaną dodane do Cloudyn.
8. Kliknij przycisk **przejdź do Cloudyn** aby otworzyć Cloudyn portal, a następnie na **Zarządzanie kontami chmury** strony, powinny pojawić się zarejestrowanego dostawcy usług Kryptograficznych informacje o Twoim koncie.

## <a name="configure-indirect-csp-access-in-cloudyn"></a>Skonfiguruj bezpośredni dostęp do dostawcy usług Kryptograficznych w Cloudyn

Domyślnie interfejsu API Center partnera jest dostępny tylko do kierowania dostawców usług kryptograficznych. Jednak dla ich pośrednie dostawcy usług Kryptograficznych klientów lub partnerów za pośrednictwem grupy jednostek w Cloudyn dostępu można skonfigurować bezpośrednie dostawcy usług Kryptograficznych.

Aby włączyć dostęp do pośredniego dostawcy usług Kryptograficznych klientów lub partnerów, postępuj zgodnie z instrukcjami [tworzenia wersji próbnej rejestracji](#create-a-trial-registration) do konfigurowania rejestracji wersji próbnej. Następnie wykonaj następujące kroki do segmentu pośrednich danych dostawcy usług Kryptograficznych za pomocą Cloudyn grupy jednostek. Następnie można przypisać odpowiednie uprawnienia do grupy jednostek.

1. Utwórz grupę jednostki z informacjami o [utworzenie jednostek](tutorial-user-access.md#create-entities).
2. Wykonaj kroki opisane w temacie [przypisywanie subskrypcji do jednostek koszt](https://support.cloudyn.com/hc/en-us/articles/115005139425-Video-Assigning-subscriptions-to-Cost-Entities). Skojarzenie konta pośrednie klienta dostawcy usług Kryptograficznych i ich subskrypcji platformy Azure do wcześniej utworzonej jednostki.
3. Wykonaj kroki opisane w temacie [utworzyć użytkownika mającego uprawnienia administratora](tutorial-user-access.md#create-a-user-with-admin-access) się tworzenie konta użytkownika z uprawnienia administratora. Następnie upewnij się, że konto użytkownika ma dostęp administratora do poszczególne osoby, które zostały utworzone wcześniej dla pośredniego konta.

Pośrednie partnerów dostawcy usług Kryptograficznych Zaloguj się do portalu Cloudyn przy użyciu konta, które zostały utworzone dla nich.


[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Następne kroki

W tym szybkiego startu informacje o dostawcy usług Kryptograficznych jest używane do rejestrowania przy kosztów zarządzania. Również zalogowaniem się do portalu Cloudyn i uruchomić, wyświetlanie danych kosztów. Aby dowiedzieć się więcej na temat zarządzania koszt Azure przez Cloudyn, nadal samouczka kosztów zarządzania.

> [!div class="nextstepaction"]
> [Przejrzyj użycia i kosztów](./tutorial-review-usage.md)
