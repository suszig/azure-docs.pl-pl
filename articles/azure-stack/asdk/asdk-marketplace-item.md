---
title: Dodawanie elementu stosu Azure marketplace z platformy Azure | Dokumentacja firmy Microsoft
description: "Opisuje sposób dodawania obrazu maszyny wirtualnej bazującej na platformie Azure w systemie Windows Server w portalu Azure Marketplace stosu."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 25f179f825526e20377c0e94ccb38a788cadd898
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-add-an-azure-stack-marketplace-item-from-azure"></a>Samouczek: Dodawanie elementu stosu Azure marketplace z platformy Azure

Jako Operator stosu Azure w pierwszej kolejności należy wykonać, aby użytkownicy mogli wdrażać maszyny wirtualnej (VM) jest dodanie obrazu maszyny Wirtualnej do stosu Azure marketplace. Domyślnie, nic nie jest opublikowana w stosie Azure marketplace, ale można pobrać elementów z [wyselekcjonowanych listy elementów witrynę Azure marketplace](.\.\azure-stack-marketplace-azure-items.md) które zostały przetestowane na stosie Azure. Użyj tej opcji, jeśli są używane w przypadku połączonych i wystąpienia stosu Azure zostały zarejestrowane przy użyciu platformy Azure.

W tym samouczku możesz dodać obraz maszyny Wirtualnej systemu Windows Server 2016 z poziomu portalu Azure marketplace do stosu Azure marketplace.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dodaj element marketplace stos Azure maszyny Wirtualnej serwera systemu Windows
> * Sprawdź, czy obraz maszyny Wirtualnej jest dostępny 
> * Testowanie obrazu maszyny Wirtualnej

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

- Zainstaluj [modułów programu Azure PowerShell platformy Azure zgodnego stosu](asdk-post-deploy.md#install-azure-stack-powershell)
- Pobierz [narzędzia Azure stosu](asdk-post-deploy.md#download-the-azure-stack-tools)
- [Zarejestruj ASDK](asdk-register.md) z subskrypcją platformy Azure

## <a name="add-a-windows-server-vm-image"></a>Dodaj obraz maszyny Wirtualnej systemu Windows Server
Możesz dodać obraz systemu Windows Server 2016 do stosu Azure marketplace, pobierając go z poziomu portalu Azure marketplace. Użyj tej opcji, jeśli są używane w przypadku połączonych i masz już [zarejestrowany wystąpienia stosu Azure](asdk-register.md) z platformy Azure.

1. Zaloguj się do [portalu administracyjnego ASDK](https://adminportal.local.azurestack.external).

2. Wybierz **więcej usług** > **zarządzania Marketplace** > **Dodaj z platformy Azure**. 

   ![Dodaj z platformy Azure](media/asdk-marketplace-item/azs-marketplace.png)

3. Znajdowanie lub wyszukiwanie **systemu Windows Server 2016 Datacenter** obrazu.

4. Wybierz **systemu Windows Server Datacenter 2016** obrazu, a następnie wybierz **Pobierz**.

   ![Pobierz obraz z platformy Azure](media/asdk-marketplace-item/azure-marketplace-ws2016.png)


> [!NOTE]
> Trwa około godzinę do pobrania obrazu maszyny Wirtualnej i opublikować ją w stosie Azure marketplace. 

Po zakończeniu pobierania obrazu zostanie opublikowana ani dostępna w obszarze **zarządzania Marketplace**. Obraz jest również dostępny w obszarze **obliczeniowe** do utworzenia nowej maszyny wirtualnej.

## <a name="verify-the-marketplace-item-is-available"></a>Sprawdź, czy element marketplace jest dostępny
Wykonaj te kroki, aby Sprawdź, czy nowy obraz maszyny Wirtualnej jest dostępna w witrynie marketplace stosu Azure.

1. Zaloguj się do [portalu administracyjnego ASDK](https://adminportal.local.azurestack.external).

2. Wybierz **więcej usług** > **zarządzania Marketplace**. 

3. Sprawdź, czy obraz maszyny Wirtualnej systemu Windows Server 2016 centrum danych został pomyślnie dodany.

   ![Pobrany obraz z platformy Azure](media/asdk-marketplace-item/azs-marketplace-ws2016.png)

## <a name="test-the-vm-image"></a>Testowanie obrazu maszyny Wirtualnej
Operator stosu Azure, można użyć [portalu administratora](https://adminportal.local.azurestack.external) do utworzenia testu maszyny Wirtualnej, aby sprawdzić poprawność obraz został udostępniony pomyślnie. 

1. Zaloguj się do [portalu administracyjnego ASDK](https://adminportal.local.azurestack.external).

2. Kliknij przycisk **nowe** > **obliczeniowe** > **systemu Windows Server 2016 Datacenter** > **utworzyć**.  
 ![Tworzenie maszyny Wirtualnej z obrazu witryny marketplace](media/asdk-marketplace-item/new-compute.png)

3. W **podstawy** bloku, wprowadź następujące informacje, a następnie kliknij przycisk **OK**:
  - **Nazwa**: test-vm-1
  - **Nazwa użytkownika**: AdminTestUser
  - **Hasło**: AzS TestVM01
  - **Subskrypcja**: Zaakceptuj domyślny dostawca subskrypcji
  - **Grupa zasobów**: test-vm-zarządcy zasobów
  - **Lokalizacja**: lokalne

4. W **wybierz rozmiar** bloku, kliknij przycisk **A1 standardowe**, a następnie kliknij przycisk **wybierz**.  

5. W **ustawienia** bloku, zaakceptuj ustawienia domyślne i kliknij przycisk **OK**

6. W bloku **Podsumowanie** kliknij przycisk **OK**, aby utworzyć maszynę wirtualną.  
> [!NOTE]
> Wdrożenie maszyny wirtualnej może zająć kilka minut.

7. Aby wyświetlić do nowej maszyny Wirtualnej, kliknij przycisk **maszyn wirtualnych**, następnie wyszukaj **test-vm-1** i kliknij jego nazwę.
    ![Pierwszy obraz maszyny Wirtualnej testu wyświetlany](media/asdk-marketplace-item/first-test-vm.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Po pomyślnie zalogowany do maszyny Wirtualnej i zweryfikować, że obraz test działa prawidłowo, należy usunąć grupy zasobów testu. Spowoduje to zwolnienie ograniczone zasoby dostępne do instalacji ASDK jednym węzłem.

Gdy nie są już potrzebne, Usuń grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby wykonaj następujące czynności:

1. Zaloguj się do [portalu administracyjnego ASDK](https://adminportal.local.azurestack.external).
2. Kliknij przycisk **grup zasobów** > **test-vm-zarządcy zasobów** > **Usuń grupę zasobów**.
3. Typ **test-vm-zarządcy zasobów** Nazwa grupy zasobów, a następnie kliknij przycisk **usunąć**.

## <a name="next-steps"></a>Kolejne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dodaj element marketplace stos Azure maszyny Wirtualnej serwera systemu Windows
> * Sprawdź, czy obraz maszyny Wirtualnej jest dostępny 
> * Testowanie obrazu maszyny Wirtualnej

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak utworzyć ofertę stosu Azure i plan.

> [!div class="nextstepaction"]
> [Oferty usług Azure IaaS stosu](asdk-offer-services.md)