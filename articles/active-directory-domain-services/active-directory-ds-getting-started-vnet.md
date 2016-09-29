<properties
    pageTitle="Usługi domenowe Azure AD: tworzenie lub wybieranie sieci wirtualnej | Microsoft Azure"
    description="Wprowadzenie do usługi Active Directory Domain Services"
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
    ms.date="09/20/2016"
    ms.author="maheshu"/>


# Tworzenie lub wybieranie sieci wirtualnej dla usługi Azure AD Domain Services

## Wytyczne dotyczące wybierania sieci wirtualnej platformy Azure
> [AZURE.NOTE] **Przed rozpoczęciem**: zapoznaj się z tematem [Networking considerations for Azure AD Domain Services](active-directory-ds-networking.md) (Zagadnienia dotyczące sieci w usłudze Azure AD Domain Services).


## Zadanie 2. Tworzenie sieci wirtualnej platformy Azure
Następne zadanie konfiguracji to utworzenie sieci wirtualnej platformy Azure, w której chcesz włączyć Usługi domenowe Azure AD. Jeśli masz już istniejącą sieć wirtualną, której chcesz użyć, możesz pominąć ten krok.

> [AZURE.NOTE] Upewnij się, że sieć wirtualna platformy Azure utworzona lub wybrana do użycia z Usługami domenowymi Azure AD należy do regionu platformy Azure, który jest obsługiwany przez Usługi domenowe Azure AD. Ze strony zawierającej [usługi platformy Azure uporządkowane według regionów](https://azure.microsoft.com/regions/#services/) dowiesz się, w których regionach platformy Azure jest dostępna usługa Azure AD Domain Services.

Zapamiętaj nazwę sieci wirtualnej, aby podczas włączania usługi Azure AD Domain Services w kolejnym kroku konfiguracji wybrać właściwą sieć.

Wykonaj poniższe czynności konfiguracyjne, aby utworzyć sieć wirtualną platformy Azure, w której chcesz włączyć usługę Azure AD Domain Services.

1. Przejdź do **klasycznego portalu Azure** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. W okienku po lewej stronie wybierz węzeł **Sieci**.

3. Kliknij pozycję **NOWE** na pasku zadań u dołu strony.

    ![Węzeł sieci wirtualnych](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. W węźle **Usługi sieciowe** wybierz pozycję **Virtual Network**.

5. Kliknij pozycję **Szybkie tworzenie** w celu utworzenia sieci wirtualnej.

    ![Sieć wirtualna — szybkie tworzenie](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Określ wartość pola **Nazwa** dla sieci wirtualnej. Możesz również skonfigurować wartości **Przestrzeń adresowa** lub **Maksymalna liczba maszyn wirtualnych** dla sieci. Na razie ustawienie serwera DNS można zostawić ustawione na wartość „Brak”. To ustawienie zostanie zaktualizowane po włączeniu Usług domenowych Azure AD.

7. Upewnij się, że z listy rozwijanej **Lokalizacja** wybrano obsługiwany region platformy Azure. Ze strony zawierającej [usługi platformy Azure uporządkowane według regionów](https://azure.microsoft.com/regions/#services/) dowiesz się, w których regionach platformy Azure jest dostępna usługa Azure AD Domain Services.

8. Kliknij przycisk **Utwórz sieć wirtualną**, aby utworzyć sieć wirtualną.

    ![Utwórz sieć wirtualną dla Usług domenowych Azure AD.](./media/active-directory-domain-services-getting-started/create-vnet.png)

<br>

## Zadanie 3. Włączanie Usług domenowych Azure AD
Następne zadanie konfiguracji to [włączenie Usług domenowych Azure AD](active-directory-ds-getting-started-enableaadds.md).



<!--HONumber=Sep16_HO3-->


