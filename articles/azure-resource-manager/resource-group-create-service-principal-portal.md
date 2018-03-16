---
title: "Utwórz tożsamość aplikacji usługi Azure w portalu | Dokumentacja firmy Microsoft"
description: "Opisuje sposób tworzenia nowej aplikacji usługi Azure Active Directory i nazwy głównej usługi, który może służyć z kontroli dostępu opartej na rolach w usłudze Azure Resource Manager do zarządzania dostępem do zasobów."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/12/2018
ms.author: tomfitz
ms.openlocfilehash: c2b8498b2d32e2c3c7ed5dca3295ae6a98fa2676
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Aby utworzyć aplikację usługi Azure Active Directory i nazwy głównej usługi, który ma dostęp do zasobów za pomocą portalu

Jeśli masz kod, który wymaga dostępu lub modyfikacji zasobów, należy skonfigurować aplikację Azure Active Directory (AD). Wymagane uprawnienia są przypisywane do aplikacji usługi AD. Ta metoda jest preferowana, na którym uruchomiono aplikację z poświadczeniami użytkownika, ponieważ można przypisać uprawnienia do tożsamości aplikacji, które są inne niż własnych uprawnień. Zazwyczaj te uprawnienia są ograniczone do dokładnie co aplikacja powinna wykonać.

W tym artykule przedstawiono sposób wykonywania tych kroków za pośrednictwem portalu. Głównie aplikacji pojedynczej dzierżawy, gdzie aplikacja jest przeznaczona do uruchamiania tylko jednej organizacji. Zwykle użyć pojedynczej dzierżawy aplikacji dla aplikacji — biznesowych, które uruchamiane w danej organizacji.

> [!IMPORTANT]
> Zamiast tworzenia nazwy głównej usługi, należy wziąć pod uwagę przy użyciu usługi Azure AD zarządzane tożsamości usługi dla tożsamości użytkownika aplikacji. Azure AD MSI jest w publicznej wersji zapoznawczej usługi Azure Active Directory, które upraszcza tworzenie tożsamości dla kodu. Jeśli kod jest uruchomiony na to usługa, która obsługuje program Azure AD MSI i uzyskuje dostęp do zasobów, które obsługują uwierzytelnianie usługi Azure Active Directory, Azure AD MSI jest lepszym rozwiązaniem dla Ciebie. Aby dowiedzieć się więcej na temat usługi Azure AD MSI, łącznie z usług, które obecnie obsługuje, zobacz [zarządzane tożsamość usługi Azure zasobów](../active-directory/managed-service-identity/overview.md).

## <a name="required-permissions"></a>Wymagane uprawnienia

Aby ukończyć w tym artykule, należy wystarczających uprawnień do rejestrowania aplikacji w dzierżawie usługi Azure AD i przypisywanie aplikacji do roli w ramach subskrypcji platformy Azure. Upewnij się, że masz odpowiednie uprawnienia do wykonania tych kroków.

### <a name="check-azure-active-directory-permissions"></a>Sprawdź uprawnienia usługi Azure Active Directory

1. Wybierz **usługi Azure Active Directory**.

   ![Wybierz usługę azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. W usłudze Azure Active Directory, zaznacz **ustawienia użytkownika**.

   ![Wybierz ustawienia użytkownika](./media/resource-group-create-service-principal-portal/select-user-settings.png)

1. Sprawdź **rejestracji aplikacji** ustawienie. Jeśli ustawiono **tak**, użytkownicy niebędący administratorami można zarejestrować aplikacji usługi AD. To ustawienie oznacza, że każdy użytkownik w dzierżawie usługi Azure AD można zarejestrować aplikacji. Możesz przejść do [uprawnienia subskrypcji Azure Sprawdź](#check-azure-subscription-permissions).

   ![Wyświetlanie rejestracji aplikacji](./media/resource-group-create-service-principal-portal/view-app-registrations.png)

1. Jeśli ustawiono rejestracji aplikacji, ustawianie **nr**tylko administrator użytkownicy będą mogli zarejestrować aplikacji. Sprawdź, czy Twoje konto administratora dla dzierżawy usługi Azure AD. Wybierz **omówienie** i przyjrzyj się informacje o użytkowniku. Jeśli Twoje konto jest przypisany do roli użytkownika, ale ustawienia rejestracji aplikacji (z poprzedniego kroku) jest ograniczona do administratorów, skontaktuj się z administratorem, aby albo przypisanie do roli administratora lub aby użytkownicy mogli zarejestrować aplikacji.

   ![Znajdź użytkownika](./media/resource-group-create-service-principal-portal/view-user-info.png)

### <a name="check-azure-subscription-permissions"></a>Sprawdź uprawnienia subskrypcji platformy Azure

W Twojej subskrypcji platformy Azure, jego konto musi mieć `Microsoft.Authorization/*/Write` dostępu przypisany do roli aplikacji usługi AD. Ta akcja jest przyznawane za pośrednictwem [właściciela](../active-directory/role-based-access-built-in-roles.md#owner) roli lub [Administrator dostępu użytkowników](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) roli. Jeśli Twoje konto jest przypisany do **współautora** roli, nie masz odpowiedniego uprawnienia. Błąd podczas próby przypisać nazwę główną usługi do roli.

Aby sprawdzić uprawnienia subskrypcji:

1. Wybierz konto w prawym górnym rogu i wybierz **Moje uprawnienia**.

   ![Wybierz uprawnienia użytkownika](./media/resource-group-create-service-principal-portal/select-my-permissions.png)

1. Z listy rozwijanej wybierz subskrypcję. Wybierz **kliknij tutaj, aby wyświetlić pełny dostęp do szczegółowych informacji dla tej subskrypcji**.

   ![Znajdź użytkownika](./media/resource-group-create-service-principal-portal/view-details.png)

1. Wyświetl przypisane role i określić, czy masz odpowiednie uprawnienia, aby przypisać aplikację usługi AD do roli. Jeśli nie, poproś administratora subskrypcji możesz dodać do roli Administrator dostępu użytkowników. Na poniższej ilustracji użytkownik jest przypisany do właściciela roli, co oznacza, że użytkownik ma odpowiednie uprawnienia.

   ![Pokaż uprawnienia](./media/resource-group-create-service-principal-portal/view-user-role.png)

## <a name="create-an-azure-active-directory-application"></a>Tworzenie aplikacji usługi Azure Active Directory

1. Zaloguj się do konta platformy Azure za pośrednictwem [portalu Azure](https://portal.azure.com).
1. Wybierz **usługi Azure Active Directory**.

   ![Wybierz usługę azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Wybierz **rejestracji aplikacji**.

   ![Wybierz rejestracji aplikacji](./media/resource-group-create-service-principal-portal/select-app-registrations.png)

1. Wybierz **nowej rejestracji aplikacji**.

   ![Dodaj aplikację](./media/resource-group-create-service-principal-portal/select-add-app.png)

1. Podaj nazwę i adres URL dla aplikacji. Wybierz **aplikacji sieci Web / interfejs API** dla typu aplikacji, w którym chcesz utworzyć. Nie można utworzyć poświadczenia dla [aplikacji natywnej](../active-directory/active-directory-application-proxy-native-client.md); w związku z tym, że typ nie działa w przypadku zautomatyzowanych aplikacji. Po ustawieniu wartości, wybierz **Utwórz**.

   ![Nazwa aplikacji](./media/resource-group-create-service-principal-portal/create-app.png)

Utworzono aplikację.

## <a name="get-application-id-and-authentication-key"></a>Pobierz klucz aplikacji identyfikator i uwierzytelniania

Podczas logowania programowo, potrzebny jest identyfikator dla aplikacji i klucz uwierzytelniania. Aby uzyskać te wartości, wykonaj następujące kroki:

1. Z **rejestracji aplikacji** w usłudze Azure Active Directory, wybierz aplikację.

   ![Wybierz aplikację](./media/resource-group-create-service-principal-portal/select-app.png)

1. Kopiuj **identyfikator aplikacji** i zapisze go w kodzie aplikacji. Niektóre [przykładowe aplikacje](#log-in-as-the-application) odwoływać się do tej wartości jako identyfikator klienta.

   ![Identyfikator klienta](./media/resource-group-create-service-principal-portal/copy-app-id.png)

1. Aby wygenerować klucz uwierzytelniania, wybierz **ustawienia**.

   ![Wybierz ustawienia](./media/resource-group-create-service-principal-portal/select-settings.png)

1. Aby wygenerować klucz uwierzytelniania, wybierz **klucze**.

   ![Wybierz klucze](./media/resource-group-create-service-principal-portal/select-keys.png)

1. Podaj opis klucza i czas trwania dla klucza. Po zakończeniu wybierz **zapisać**.

   ![Zapisz klucz](./media/resource-group-create-service-principal-portal/save-key.png)

   Po zapisaniu klucza, wyświetlana jest wartość klucza. Skopiuj tę wartość, ponieważ nie można pobrać klucza później. Musisz podać wartość tego klucza z Identyfikatorem aplikacji do logowania jako aplikacja. Przechowywanie wartości klucza, gdzie aplikacja je pobrać.

   ![zapisano klucza](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a>Uzyskanie Identyfikatora dzierżawy

Podczas logowania programowo, musisz przekazać identyfikator dzierżawcy do żądania uwierzytelniania.

1. Wybierz **usługi Azure Active Directory**.

   ![Wybierz usługę azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Uzyskanie Identyfikatora dzierżawy, wybierz **właściwości** dla dzierżawy usługi Azure AD.

   ![Wybierz właściwości usługi Azure AD](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

1. Kopiuj **identyfikator katalogu**. Ta wartość jest swojego identyfikatora dzierżawcy.

   ![Identyfikator dzierżawy](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-to-role"></a>Przypisywanie aplikacji do roli

Aby uzyskać dostęp do zasobów w ramach subskrypcji, należy przypisać aplikacji do roli. Zdecyduj, które roli reprezentuje odpowiednich uprawnień dla aplikacji. Aby dowiedzieć się więcej o dostępnych ról, zobacz [RBAC: Built in Roles](../active-directory/role-based-access-built-in-roles.md).

Na poziomie subskrypcji, grupy zasobów lub zasobów można ustawić zakresu. Uprawnienia są dziedziczone na niższe poziomy zakresu. Na przykład dodawanie aplikacji do roli czytnik dla grupy zasobów oznacza, że mogą odczytywać, grupy zasobów i wszystkie zasoby, które zawiera.

1. Przejdź na poziom zakresu, który chcesz przypisać aplikację do. Na przykład, aby przypisać rolę w zakresie subskrypcji, wybierz **subskrypcje**. Zamiast tego możesz określić, grupy zasobów lub zasobu.

   ![Wybierz subskrypcję](./media/resource-group-create-service-principal-portal/select-subscription.png)

1. Umożliwia określonej subskrypcji (grupy zasobów lub zasobów), aby przypisać tę aplikację.

   ![Wybierz subskrypcję do przypisania](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

1. Wybierz **(IAM) kontroli dostępu**.

   ![Wybierz dostępu](./media/resource-group-create-service-principal-portal/select-access-control.png)

1. Wybierz pozycję **Dodaj**.

   ![Wybierz opcję Dodaj](./media/resource-group-create-service-principal-portal/select-add.png)

1. Wybierz rolę, którą chcesz przypisać do aplikacji. Poniższy obraz przedstawia **czytnika** roli.

   ![Wybierz rolę](./media/resource-group-create-service-principal-portal/select-role.png)

1. Wyszukaj aplikację i zaznacz go.

   ![Wyszukiwanie aplikacji](./media/resource-group-create-service-principal-portal/search-app.png)

1. Wybierz **zapisać** na zakończenie przypisanie roli. Zostanie wyświetlona aplikacja w listy użytkowników przypisanych do roli dla tego zakresu.

## <a name="next-steps"></a>Kolejne kroki
* Aby skonfigurować aplikację wielu dzierżawców, zobacz [przewodnik dewelopera do autoryzacji przy użyciu interfejsu API Menedżera zasobów Azure](resource-manager-api-authentication.md).
* Aby dowiedzieć się więcej na temat określania zasad zabezpieczeń, zobacz [kontroli dostępu opartej na roli Azure](../active-directory/role-based-access-control-configure.md).  
* Aby uzyskać listę dostępnych akcji, które można udzielić lub odmówić dla użytkowników, zobacz [operacji dostawcy zasobów usługi Azure Resource Manager](../active-directory/role-based-access-control-resource-provider-operations.md).
