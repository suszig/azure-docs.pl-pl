---
title: "Jak połączyć subskrypcji platformy Azure do usługi Azure AD B2C | Dokumentacja firmy Microsoft"
description: "Przewodnik krok po kroku, aby włączyć rozliczeń dla dzierżawy usługi Azure AD B2C do subskrypcji platformy Azure."
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2016
ms.author: joroja
ms.openlocfilehash: 5b9955b2af7f20a79981315fa33a0eb5380a5465
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="linking-an-azure-subscription-to-an-azure-b2c-tenant-to-pay-for-usage-charges"></a>Łączenie subskrypcji platformy Azure do dzierżawy usługi Azure B2C, aby zapłacić za opłaty za użycie

Opłaty dotyczących ciągłego użytkowania dla usługi Azure Active Directory B2C (lub usługi Azure AD B2C) są rozliczane z subskrypcją platformy Azure. Konieczne jest jawnie łącze dzierżawy usługi Azure AD B2C do subskrypcji platformy Azure po utworzeniu dzierżawy B2C, sam administratorowi dzierżawy.  To łącze jest to osiągane przez utworzenie usługi Azure AD "Dzierżawy B2C" zasobu w docelowej subskrypcji platformy Azure. Wiele dzierżaw B2C można połączyć z jedną subskrypcją platformy Azure oraz innych zasobów platformy Azure (na przykład maszyny wirtualne, Magazyn danych, LogicApps)


> [!IMPORTANT]
> Najnowsze informacje dotyczące użycia rozliczeniach i cenach usługi B2C jest na następującej stronie: [cennik usługi Azure AD B2C](
https://azure.microsoft.com/pricing/details/active-directory-b2c/)

## <a name="step-1---create-an-azure-ad-b2c-tenant"></a>Krok 1 — Tworzenie dzierżawy usługi Azure AD B2C
Tworzenie dzierżawy usługi B2C musi najpierw zostać zakończona. Jeśli utworzono już urządzenie docelowe dzierżawy B2C, Pomiń ten krok. [Wprowadzenie do usługi Azure AD B2C](active-directory-b2c-get-started.md)

## <a name="step-2---open-azure-portal-in-the-azure-ad-tenant-that-shows-your-azure-subscription"></a>Krok 2 — Otwieranie portalu Azure w dzierżawy Azure AD, pokazujący subskrypcji platformy Azure
Przejdź do witryny [Azure Portal](https://portal.azure.com). Przełącz się do dzierżawy Azure AD, który przedstawiono subskrypcji platformy Azure, które chcesz użyć. Tej dzierżawy usługi Azure AD jest inna niż dzierżawy B2C. W portalu Azure kliknij nazwę konta w prawym górnym rogu pulpitu nawigacyjnego, aby wybrać dzierżawy Azure AD. Subskrypcji platformy Azure jest potrzebna, aby kontynuować. [Uzyskiwanie subskrypcji platformy Azure](https://account.windowsazure.com/signup?showCatalog=True)

![Przełączanie do dzierżawy usługi Azure AD](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="step-3---create-a-b2c-tenant-resource-in-azure-marketplace"></a>Krok 3 — Tworzenie zasobu dzierżawy B2C w portalu Azure Marketplace
Otwórz Marketplace, klikając ikonę Marketplace lub wybierając zielonego "+" w lewym górnym rogu pulpitu nawigacyjnego.  Wyszukaj i wybierz Azure Active Directory B2C. Wybierz opcję Utwórz.

![Wybierz witryny Marketplace](./media/active-directory-b2c-how-to-enable-billing/marketplace.png)

![Funkcje B2C wyszukiwania](./media/active-directory-b2c-how-to-enable-billing/searchb2c.png)

Zasobów usługi Azure AD B2C Tworzenie okna dialogowego obejmuje następujące parametry:

1. Dzierżawy usługi Azure AD B2C — z listy rozwijanej wybierz dzierżawy usługi Azure AD B2C.  Pokaż tylko kwalifikujących się dzierżaw usługi Azure AD B2C.  Kwalifikujące się dzierżawcy usługi B2C spełnia te warunki: jesteś administratorem globalnym dzierżawy B2C i dzierżawy B2C nie jest obecnie skojarzony z subskrypcją platformy Azure

2. Nazwa platformy Azure AD B2C zasobów - wyświetlana jest wartość do dopasowania nazwę domeny dzierżawy usługi B2C

3. Subskrypcja — aktywną subskrypcją platformy Azure, w którym są uprawnienia administratora lub administratora wspólnej.  Wiele dzierżaw usługi Azure AD B2C można dodać jedną subskrypcją platformy Azure

4. Lokalizacja grupy zasobów i grupy zasobów — to artefaktu pomaga organizować wielu zasobów platformy Azure.  Ten wybór nie ma wpływu na lokalizację dzierżawy B2C, wydajność lub rozliczeń stanu

5. Przypnij do pulpitu nawigacyjnego najprostszym dostępu do informacji dotyczących rozliczeń dzierżawy B2C i ustawieniami dzierżawy B2C ![utworzyć zasobu usługi B2C](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="step-4---manage-your-b2c-tenant-resources-optional"></a>Krok 4 — zarządzanie zasobami dzierżawy B2C (opcjonalnie)
Po zakończeniu wdrażania nowego zasobu "Dzierżawy B2C" jest tworzona w docelowej grupie zasobów i związane z subskrypcją platformy Azure.  Nowy zasób typu, który dodaje "Dzierżawy B2C" powinna zostać wyświetlona równolegle z innymi zasobami platformy Azure.

![Utwórz zasób B2C](./media/active-directory-b2c-how-to-enable-billing/b2cresourcedashboard.png)

Klikając zasobów dzierżawy B2C, jest możliwość
- Kliknij nazwę subskrypcji do przeglądania informacji dotyczących rozliczeń. Zobacz dotyczącymi rozliczeń i użycia.
- Kliknij ustawienia usługi Azure AD B2C, aby otworzyć nową kartę przeglądarki bezpośrednio w dzierżawcy usługi B2C blok ustawień
- Prześlij żądanie pomocy technicznej
- Przenoszenie zasobu dzierżawy B2C do innej subskrypcji platformy Azure lub innej grupie zasobów.  Zmiany tego wyboru subskrypcji Azure, której odbiera opłaty za użycie.

![Ustawienia zasobu B2C](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a>Znane problemy
- Usuwanie dzierżawy B2C. Jeśli dzierżawy B2C jest tworzony, usunięty i utworzony ponownie z tą samą nazwą domeny, również Usuń i ponownie utwórz zasób "Konsolidację" z tą samą nazwą domeny.  Ten zasób "Konsolidację" w obszarze "Wszystkie zasoby" można znaleźć w dzierżawie subskrypcji za pośrednictwem portalu Azure.
- Własnym narzuconego ograniczenia lokalizacji regionalnych zasobów.  W rzadkich przypadkach użytkownik może nawiązać regionalnych ograniczenia dotyczące tworzenia zasobów platformy Azure.  To ograniczenie może uniemożliwić tworzenie połączenia między subskrypcją platformy Azure i dzierżawy B2C. Aby uniknąć, należy złagodzenie tego ograniczenia.

## <a name="next-steps"></a>Następne kroki
Po zakończeniu tych kroków dla każdego dzierżawcy usługi B2C, subskrypcji platformy Azure są rozliczane zgodnie z szczegóły bezpośrednie Azure lub umowy Enterprise Agreement.
- Przejrzyj użycie i rozliczenia w wybranej subskrypcji Azure
- Przeglądanie szczegółowe dane użycia każdego dnia raportów za pomocą [użycia interfejsu API raportowania](active-directory-b2c-reference-usage-reporting-api.md)
