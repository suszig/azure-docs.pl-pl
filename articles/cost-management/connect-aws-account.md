---
title: "Konta usług Amazon Web Services połączyć z usługą Azure koszt Management | Dokumentacja firmy Microsoft"
description: "Połącz konta usług Amazon Web Services do wyświetlania kosztów i użycia danych w raportach dotyczących zarządzania kosztów."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 02/08/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: 4a0280420132aad9f1e0b17d5998ec225bb0eaa1
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="connect-an-amazon-web-services-account"></a>Połącz konta usług Amazon Web Services

Dostępne są dwie opcje do nawiązania połączenia Azure koszt zarządzania kontem usługi Amazon Web Services (AWS). Można połączyć z rolą IAM lub na koncie użytkownika IAM tylko do odczytu. Rola IAM jest zalecane, ponieważ pozwala na delegowany dostęp z uprawnieniami zdefiniowanych zaufane jednostki. Rola IAM nie wymaga udziału długoterminowe klucze dostępu. Po nawiązaniu połączenia konta usług AWS koszt zarządzania, kosztów i użycia danych jest dostępne w raportach kosztów zarządzania. Ten dokument przeprowadzi Cię przez obie opcje.

Aby uzyskać więcej informacji o tożsamości usług AWS IAM, zobacz [tożsamości (użytkowników, grup i ról)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html).

## <a name="aws-role-based-access"></a>Dostęp do usług AWS opartej na rolach

Poniższe sekcje przeprowadzenie tworzenie roli IAM tylko do odczytu do zapewnienia dostępu do zarządzania kosztów.

### <a name="get-your-cost-management-account-external-id"></a>Pobierz identyfikator zewnętrznego konta kosztów zarządzania

Pierwszym krokiem jest uzyskanie hasła unikatowe połączenia z portalu Azure kosztów zarządzania. Jest on używany w AWS jako **Identyfikatora zewnętrznego**.

1. Otwórz Cloudyn portal z portalu Azure lub przejdź do [https://azure.cloudyn.com](https://azure.cloudyn.com) i zaloguj się.
2. Kliknij przycisk **ustawienia** (koło zębate ikonę), a następnie wybierz **kont chmury**.
3. Zarządzanie kontami wybierz **kont usług AWS** a następnie kliknij pozycję **Dodaj nowy +**.
4. W **Dodaj konto usług AWS** okno dialogowe, kopiowania **Identyfikatora zewnętrznego** i Zapisz tę wartość dla roli usług AWS kroki tworzenia w następnej sekcji. Na poniższej ilustracji jest identyfikator przykład _Cloudyn_. Inny identyfikator.  
    ![Identyfikator zewnętrznego](./media/connect-aws-account/external-id.png)

### <a name="add-aws-read-only-role-based-access"></a>Dodawanie usług AWS dostęp tylko do odczytu opartej na rolach

1. Zaloguj się do konsoli usług AWS pod adresem https://console.aws.amazon.com/iam/home i wybierz **ról**.
2. Kliknij przycisk **Utwórz rolę** , a następnie wybierz **AWS innego konta**.
3. Wklej tożsamość `432263259397` w **Identyfikatora** pola. Ten identyfikator konta jest kontem modułu zbierającego dane koszt zarządzania, którego musisz użyć.
4. Obok pozycji **opcje**, wybierz pozycję **wymagają Identyfikatora zewnętrznego** Wklej wartość, która wcześniej skopiowany w **Identyfikatora zewnętrznego** pola, a następnie kliknij przycisk **dalej: Uprawnienia**.  
    ![Tworzenie roli](./media/connect-aws-account/create-role01.png)
5. W obszarze **Dołącz zasady uprawnień**w **typ zasad** filtru pole wyszukiwania, wpisz `ReadOnlyAccess`, wybierz pozycję **ReadOnlyAccess**, następnie kliknij przycisk **dalej: Przegląd**.  
    ![Dostęp tylko do odczytu](./media/connect-aws-account/readonlyaccess.png)
6. Na stronie Przegląd Sprawdź wybrane opcje są poprawne i wpisz **nazwy roli**. Na przykład *Mgt-Azure-koszt*. Wprowadź **opis roli**. Na przykład _przypisania roli dla usługi Azure Management koszt_, następnie kliknij przycisk **Utwórz rolę**.
7. W **ról** , kliknij utworzoną rolę, a następnie skopiuj **ARN roli** wartości na stronie podsumowania. Użyj wartości ARN roli później podczas rejestrowania konfigurację w Azure kosztów zarządzania.  
    ![Rola ARN](./media/connect-aws-account/role-arn.png)

### <a name="configure-aws-iam-role-access-in-cost-management"></a>Skonfigurowanie dostępu roli usług AWS IAM w koszt zarządzania

1. Otwórz Cloudyn portal z portalu Azure lub przejdź do https://azure.cloudyn.com/ i logowania.
2. Kliknij przycisk **ustawienia** (koło zębate ikonę), a następnie wybierz **kont chmury**.
3. Zarządzanie kontami wybierz **kont usług AWS** a następnie kliknij pozycję **Dodaj nowy +**.
4. W **nazwa konta**, wpisz nazwę konta.
5. Obok pozycji **typu dostępu**, wybierz pozycję **roli IAM**.
6. W **ARN roli** pól, wklej skopiowane wcześniej wartość, a następnie kliknij przycisk **zapisać**.  
    ![Stan konta usług AWS](./media/connect-aws-account/aws-account-status01.png)

Konta usług AWS pojawią się na liście kont. **Identyfikator właściciela** wymienione jest zgodna z wartością ARN roli. Twoje **stan konta** powinien mieć zielony znacznik wyboru.

Kosztów zarządzania rozpoczyna zbieranie danych i wypełniania raportów. Jednak niektóre raporty optymalizacji mogą wymagać dane z kilku dni przed wygenerowaniem są dokładne zalecenia.

## <a name="aws-user-based-access"></a>Dostęp do usług AWS oparta na użytkowniku

Poniższe sekcje przeprowadzenie Tworzenie użytkownika tylko do odczytu w celu zapewnienia dostępu do zarządzania koszt.

### <a name="add-aws-read-only-user-based-access"></a>Dodawanie usług AWS dostęp tylko do odczytu oparta na użytkowniku

1. Zaloguj się do konsoli usług AWS pod adresem https://console.aws.amazon.com/iam/home i wybierz **użytkowników**.
2. Kliknij przycisk **dodać użytkownika**.
3. W **nazwy użytkownika** wpisz nazwę użytkownika.
4. Dla **dostęp typu**, wybierz pozycję **dostęp programistyczny** i kliknij przycisk **dalej: uprawnienia**.  
    ![Dodaj użytkownika](./media/connect-aws-account/add-user01.png)
5. Uprawnienia, wybierz **dołączyć istniejących zasad bezpośrednio**.
6. W obszarze **Dołącz zasady uprawnień**w **typ zasad** filtru pole wyszukiwania, wpisz `ReadOnlyAccess`, wybierz pozycję **ReadOnlyAccess**, a następnie kliknij przycisk **dalej : Przejrzyj**.  
    ![Ustawianie uprawnień dla użytkownika](./media/connect-aws-account/set-permission-for-user.png)
7. Na stronie przeglądu, sprawdź wybrane opcje są poprawne, a następnie kliknij przycisk **tworzenia użytkownika**.
8. Na stronie pełny dostęp do klucza identyfikator i klucz tajny klucz dostępu do są wyświetlane. Te informacje umożliwiają konfigurowanie rejestracji w kosztów zarządzania.
9. Kliknij przycisk **Pobierz CSV** i Zapisz plik credentials.csv do bezpiecznej lokalizacji.  
    ![Pobierz poświadczenia](./media/connect-aws-account/download-csv.png)


### <a name="configure-aws-iam-user-based-access-in-cost-management"></a>Konfigurowanie dostępu oparte na użytkowniku usług AWS IAM w koszt zarządzania

1. Otwórz Cloudyn portal z portalu Azure lub przejdź do https://azure.cloudyn.com/ i logowania.
2. Kliknij przycisk **ustawienia** (koło zębate ikonę), a następnie wybierz **kont chmury**.
3. Zarządzanie kontami wybierz **kont usług AWS** a następnie kliknij pozycję **Dodaj nowy +**.
4. Aby uzyskać **nazwa konta**, wpisz nazwę konta.
5. Obok pozycji **typu dostępu**, wybierz pozycję **użytkownika IAM**.
6. W **klucz dostępu**, Wklej **dostępu Identyfikatora klucza** wartości z pliku credentials.csv.
7. W **klucz tajny**, Wklej **klucza tajnego dostępu** wartości z pliku credentials.csv, a następnie kliknij przycisk **zapisać**.  
    ![Stan konta użytkownika usług AWS](./media/connect-aws-account/aws-user-account-status.png)

Konta usług AWS pojawią się na liście kont. Twoje **stan konta** powinien mieć zielony znacznik wyboru.

Kosztów zarządzania rozpoczyna zbieranie danych i wypełniania raportów. Jednak niektóre raporty optymalizacji mogą wymagać dane z kilku dni przed wygenerowaniem są dokładne zalecenia.

## <a name="next-steps"></a>Kolejne kroki

- Aby dowiedzieć się więcej na temat usługi Azure koszt zarządzania, w dalszym ciągu [Przejrzyj użycia i koszty](tutorial-review-usage.md) samouczka kosztów zarządzania.
