<properties
    pageTitle="Usługi domenowe Azure AD: tworzenie lub wybieranie sieci wirtualnej | Microsoft Azure"
    description="Wprowadzenie do Usług domenowych Azure Active Directory (wersja zapoznawcza)"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/25/2016"
    ms.author="maheshu"/>

# Usługi domenowe Azure AD *(wersja zapoznawcza)* — tworzenie lub wybieranie sieci wirtualnej

## Wytyczne dotyczące wybierania sieci wirtualnej platformy Azure
Wybierając sieć wirtualną do użycia z Usługami domenowymi Azure AD, pamiętaj o następujących wytycznych:

- Upewnij się, że wybierana sieć wirtualna jest dostępna w regionie obsługiwanym przez Usługi domenowe Azure AD. Ze strony zawierającej [usługi platformy Azure uporządkowane według regionów](https://azure.microsoft.com/regions/#services/) dowiesz się, w których regionach platformy Azure są dostępne Usługi domenowe Azure AD.

- Jeśli planujesz użyć istniejącej sieci wirtualnej, upewnij się, czy jest to sieć regionalna. Sieci wirtualnych korzystających ze starszego mechanizmu grup koligacji nie można używać z Usługami domenowymi Azure AD. Konieczna będzie [migracja starszych sieci wirtualnych do regionalnych sieci wirtualnych](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

- Jeśli planujesz użyć istniejącej sieci wirtualnej, upewnij się, że nie skonfigurowano dla niej żadnych niestandardowych serwerów DNS. Usługi domenowe Azure AD nie obsługują niestandardowych/własnych serwerów DNS.

- Jeśli planujesz użyć istniejącej sieci wirtualnej, upewnij się, że nie ma w niej dostępnej istniejącej domeny o tej samej nazwie. Na przykład załóżmy, że masz domenę o nazwie „contoso.com” już dostępną w wybranej sieci wirtualnej. Następnie próbujesz włączyć domenę zarządzaną Usług domenowych Azure AD z tą samą nazwą domeny („contoso.com”) w tej sieci wirtualnej. Spowoduje to wystąpienie błędu podczas próby włączenia Usług domenowych Azure AD. Przyczyną błędu będzie konflikt nazw domeny w sieci wirtualnej. W takiej sytuacji musisz użyć innej nazwy podczas konfigurowania domeny zarządzanej Usług domenowych Azure AD. Możesz również anulować aprowizację istniejącej domeny i kontynuować włączanie Usług domenowych Azure AD.

- Wybierz sieć wirtualną, która obecnie hostuje/będzie hostować maszyny wirtualne wymagające dostępu do Usług domenowych Azure AD. Po włączeniu usługi nie będzie można przenieść Usług domenowych do innej sieci wirtualnej.

- Usługi domenowe Azure AD nie są obsługiwane w sieciach wirtualnych utworzonych za pomocą usługi Azure Resource Manager. Możesz [połączyć klasyczną sieć wirtualną z siecią wirtualną opartą na usłudze ARM](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md), aby korzystać z Usług domenowych Azure AD w sieci wirtualnej utworzonej za pomocą usługi Azure Resource Manager.


## Zadanie 2. Tworzenie sieci wirtualnej platformy Azure
Następne zadanie konfiguracji to utworzenie sieci wirtualnej platformy Azure, w której chcesz włączyć Usługi domenowe Azure AD. Jeśli masz już istniejącą sieć wirtualną, której chcesz użyć, możesz pominąć ten krok.

> [AZURE.NOTE] Upewnij się, że sieć wirtualna platformy Azure utworzona lub wybrana do użycia z Usługami domenowymi Azure AD należy do regionu platformy Azure, który jest obsługiwany przez Usługi domenowe Azure AD. Ze strony zawierającej [usługi platformy Azure uporządkowane według regionów](https://azure.microsoft.com/regions/#services/) dowiesz się, w których regionach platformy Azure są dostępne Usługi domenowe Azure AD.

Zapamiętaj nazwę sieci wirtualnej, aby podczas włączania Usług domenowych Azure AD w kolejnym kroku konfiguracji wybrać właściwą sieć.

Wykonaj poniższe czynności konfiguracyjne, aby utworzyć sieć wirtualną platformy Azure, w której chcesz włączyć Usługi domenowe Azure AD.

1. Przejdź do **klasycznego portalu Azure** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. W okienku po lewej stronie wybierz węzeł **Sieci**.

3. Kliknij pozycję **NOWE** na pasku zadań u dołu strony.

    ![Węzeł sieci wirtualnych](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. W węźle **Usługi sieciowe** wybierz pozycję **Virtual Network**.

5. Kliknij pozycję **Szybkie tworzenie** w celu utworzenia sieci wirtualnej.

    ![Sieć wirtualna — szybkie tworzenie](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Określ wartość pola **Nazwa** dla sieci wirtualnej. Możesz również skonfigurować wartości **Przestrzeń adresowa** lub **Maksymalna liczba maszyn wirtualnych** dla sieci. Na razie ustawienie serwera DNS można zostawić ustawione na wartość „Brak”. To ustawienie zostanie zaktualizowane po włączeniu Usług domenowych Azure AD.

7. Upewnij się, że z listy rozwijanej **Lokalizacja** wybrano obsługiwany region platformy Azure. Ze strony zawierającej [usługi platformy Azure uporządkowane według regionów](https://azure.microsoft.com/regions/#services/) dowiesz się, w których regionach platformy Azure są dostępne Usługi domenowe Azure AD. Jest to ważny krok. W przypadku wybrania sieci wirtualnej w regionie Azure, który nie jest obsługiwany przez Usługi domenowe Azure AD, nie będzie można włączyć usługi w tej sieci wirtualnej.

8. Kliknij przycisk **Utwórz sieć wirtualną**, aby utworzyć sieć wirtualną.

    ![Utwórz sieć wirtualną dla Usług domenowych Azure AD.](./media/active-directory-domain-services-getting-started/create-vnet.png)

<br>

## Zadanie 3. Włączanie Usług domenowych Azure AD
Następne zadanie konfiguracji to [włączenie Usług domenowych Azure AD](active-directory-ds-getting-started-enableaadds.md).



<!--HONumber=Jun16_HO2-->


