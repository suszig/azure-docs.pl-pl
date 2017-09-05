---
title: 'Azure Active Directory Domain Services: Tworzenie lub wybieranie sieci wirtualnej | Microsoft Docs'
description: "Włączanie usług Azure Active Directory Domain Services przy użyciu klasycznej witryny Azure Portal"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 13ab1608-e3d8-40de-9f7b-9b5b42199af4
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: maheshu
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: c0d3f90c0f15fbb7aad00fe24c2946738e24ffd8
ms.contentlocale: pl-pl
ms.lasthandoff: 08/29/2017

---
# <a name="create-or-select-a-virtual-network-for-azure-active-directory-domain-services"></a>Tworzenie lub wybieranie sieci wirtualnej dla usługi Azure Active Directory Domain Services

> [!IMPORTANT]
> Przedstawione w tym artykule środowisko klasycznej witryny Azure Portal, które umożliwia włączanie usług Azure AD Domain Services, zostanie wkrótce usunięte. W przypadku nowych wdrożeń [**zamiast niego użyj nowego środowiska witryny Azure Portal (w wersji zapoznawczej)**](active-directory-ds-getting-started.md).
>

## <a name="before-you-begin"></a>Przed rozpoczęciem
Zapoznaj się z tematem [Networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md) (Zagadnienia dotyczące sieci w usłudze Azure Active Directory Domain Services).

## <a name="task-2-create-an-azure-virtual-network"></a>Zadanie 2. Tworzenie sieci wirtualnej platformy Azure
Kolejnym zadaniem konfiguracji jest utworzenie sieci wirtualnej platformy Azure oraz podsieci w tej sieci. Usługi Azure Active Directory Domain Services w tej podsieci możesz włączyć w sieci wirtualnej. Jeśli masz istniejącą sieć wirtualną, której chcesz użyć, możesz pominąć ten krok.

> [!NOTE]
> Upewnij się, że sieć wirtualna platformy Azure utworzona lub wybrana do użycia z usługami Azure Active Directory Domain Services należy do regionu świadczenia usługi Azure, który jest obsługiwany przez usługi Azure Active Directory Domain Services. Zobacz [Usługi platformy Azure uporządkowane według regionów](https://azure.microsoft.com/regions/#services/), aby wyświetlić regiony platformy Azure, w których usługi Azure Active Directory Domain Services są dostępne.
>
>Zapamiętaj nazwę sieci wirtualnej, aby podczas włączania usług Azure Active Directory Domain Services w kolejnym kroku konfiguracji wybrać właściwą sieć.


Aby utworzyć sieć wirtualną platformy Azure, w której chcesz włączyć usługi Azure Active Directory Domain Services, wykonaj następujące instrukcje dotyczące konfiguracji:

1. Przejdź do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. W lewym okienku wybierz opcję **Sieci**.

    ![Węzeł sieci](./media/active-directory-domain-services-getting-started/networks-node.png)  
    Zostanie otwarte okno **Sieci wirtualne**.
3. Na pasku zadań u dołu okna kliknij opcję **Nowy**.

    ![Okno Sieci wirtualne](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. Kliknij opcję **Usługi sieciowe**, a następnie wybierz pozycję **Sieć wirtualna**.

    ![Sieć wirtualna — szybkie tworzenie](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
5. Kliknij pozycję **Szybkie tworzenie** w celu utworzenia sieci wirtualnej.

6. Określ nazwę sieci wirtualnej w polu **Nazwa** i rozważ wykonanie następujących czynności:
    * Możesz skonfigurować wartości **Przestrzeń adresowa** lub **Maksymalna liczba maszyn wirtualnych** dla sieci.
    * Na razie dla ustawienia **Serwer DNS** można zostawić wartość **Brak**. Ustawienia możesz zaktualizować po włączeniu usług Azure Active Directory Domain Services.
7. Na liście rozwijanej **Lokalizacja** wybierz obsługiwany region świadczenia usługi Azure.  
    Zobacz [Usługi platformy Azure uporządkowane według regionów](https://azure.microsoft.com/regions/#services/), aby upewnić się co do regionów świadczenia usługi Azure, w których usługa Azure Active Directory Domain Services jest dostępna.
8. Kliknij przycisk **Utwórz sieć wirtualną**, aby utworzyć sieć wirtualną.

    ![Tworzenie sieci wirtualnej dla usługi Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. Po utworzeniu sieci wirtualnej wybierz nazwę sieci wirtualnej, a następnie kliknij kartę **Konfiguracja**.

    ![Tworzenie podsieci](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. W części **Przestrzenie adresowe sieci wirtualnych** kliknij pozycję **Dodaj podsieć**, a następnie określ podsieć o nazwie **AaddsSubnet**.

    ![Tworzenie podsieci dla usługi Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

11. Kliknij przycisk **Zapisz**, aby utworzyć podsieć.


## <a name="next-step"></a>Następny krok
[Zadanie 3. Włączanie usług Azure Active Directory Domain Services](active-directory-ds-getting-started-enableaadds.md)

