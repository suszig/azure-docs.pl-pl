---
title: Tworzenie sieci wirtualnej platformy Azure z wieloma podsieciami | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak utworzyć sieć wirtualną z wieloma podsieciami na platformie Azure."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: f82a95ec9543b2d53ef28bf7f15315e23cf4893a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-network-with-multiple-subnets"></a>Tworzenie sieci wirtualnej z wieloma podsieciami

W tym samouczku jak utworzyć podstawowy sieci wirtualnej platformy Azure z oddzielne podsieci publiczne i prywatne. Zasoby w sieci wirtualne mogą komunikować się ze sobą oraz z zasobami w innych sieciach, połączony z siecią wirtualną. W tym samym lub różnych podsieci w sieci wirtualnej można utworzyć zasobów platformy Azure, takich jak maszyny wirtualne, środowiska usługi aplikacji, zestawy skalowania maszyny wirtualnej, Azure HDInsight i usługi w chmurze. Tworzenie zasobów w różnych podsieciach pozwala do filtrowania ruchu sieciowego i z podsieciami niezależnie z [sieciowej grupy zabezpieczeń](virtual-networks-create-nsg-arm-pportal.md), a w [kierować ruchem między podsieciami](virtual-network-create-udr-arm-ps.md) za pośrednictwem sieci Urządzenie wirtualne, takie jak zapory, jeśli chcesz. 

W następujących sekcjach opisano kroki, które należy wykonać, aby utworzyć sieć wirtualną przy użyciu [portalu Azure](#portal), interfejsu wiersza polecenia platformy Azure ([interfejsu wiersza polecenia Azure](#azure-cli)), [programu Azure PowerShell](#powershell)i [szablonu usługi Azure Resource Manager](#resource-manager-template). Wynik jest taki sam, niezależnie od tego, które narzędzie służy do tworzenia sieci wirtualnej. Kliknij łącze narzędzia, aby przejść do tej części samouczka. Dowiedz się więcej na temat wszystkich [sieci wirtualnej](virtual-network-manage-network.md) i [podsieci](virtual-network-manage-subnet.md) ustawienia.

Ten artykuł zawiera kroki, aby utworzyć sieć wirtualną przy użyciu modelu wdrażania Menedżera zasobów, który jest modelem wdrożenia, które firma Microsoft zaleca używanie podczas tworzenia nowej sieci wirtualnej. Jeśli musisz utworzyć sieć wirtualną (klasyczne), zobacz [utworzyć sieć wirtualną (klasyczne)](create-virtual-network-classic.md). Jeśli nie masz doświadczenia w obsłudze modele wdrażania platformy Azure, zobacz [modele wdrażania zrozumieć Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="portal"></a>Azure portal

1. W przeglądarce sieci Web, przejdź do [portalu Azure](https://portal.azure.com). Zaloguj się za pomocą programu [konta Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Jeśli nie masz konta platformy Azure, możesz skorzystać z [bezpłatnej wersji próbnej](https://azure.microsoft.com/offers/ms-azr-0044p).
2. W portalu kliknij **+ nowy** > **sieci** > **sieci wirtualnej**.
3. Na **Utwórz sieć wirtualną** bloku, wprowadź następujące wartości, a następnie kliknij przycisk **Utwórz**:

    |Ustawienie|Wartość|
    |---|---|
    |Nazwa|myVnet|
    |Przestrzeń adresowa|10.0.0.0/16|
    |Nazwa podsieci|Publiczne|
    |Zakres adresów podsieci|10.0.0.0/24|
    |Grupa zasobów|Pozostaw **Utwórz nowy** zaznaczone, a następnie wprowadź **myResourceGroup**.|
    |Subskrypcji i lokalizacji|Wybierz subskrypcji i lokalizacji.

    Jeśli jesteś nowym użytkownikiem usługi Azure, Dowiedz się więcej o [grup zasobów](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [subskrypcje](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), i [lokalizacje](https://azure.microsoft.com/regions) (zwaną także *regionów*).
4. W portalu można utworzyć tylko jedną podsieć, podczas tworzenia sieci wirtualnej. W tym samouczku utworzysz drugiej podsieci po utworzeniu sieci wirtualnej. Później można utworzyć Internet dostępnych zasobów w **publicznego** podsieci. Można także utworzyć zasoby, które nie są dostępne z Internetu w **prywatnej** podsieci. Aby utworzyć drugą podsieć w **wyszukiwania zasobów** polu w górnej części strony, wprowadź **myVnet**. W wynikach wyszukiwania kliknij **myVnet**. Jeśli masz wiele sieci wirtualnych o tej samej nazwie w ramach subskrypcji, Sprawdź grupy zasobów, które znajdują się w każdej sieci wirtualnej. Upewnij się, że możesz kliknąć pozycję **myVnet** Wyszukaj wynik, który ma grupy zasobów **myResourceGroup**.
5. Na **myVnet** bloku, w obszarze **ustawienia**, kliknij przycisk **podsieci**.
6. Na **myVnet - podsieci** bloku, kliknij przycisk **+ podsieci**.
7. Na **Dodaj podsieć** bloku dla **nazwa**, wprowadź **prywatnej**. Aby uzyskać **zakres adresów**, wprowadź **10.0.1.0/24**.  Kliknij przycisk **OK**.
8. Na **myVnet - podsieci** bloku, przejrzyj podsieci. Widać **publicznego** i **prywatnej** podsieci, które zostały utworzone.
9. **Opcjonalnie:** wykonać dodatkowe samouczki kategorii [następne kroki](#next-steps) do filtrowania ruchu sieciowego i z każdej podsieci z grupami zabezpieczeń sieci, można kierować ruchem między podsieciami przez urządzenie wirtualne sieci , lub aby połączyć sieć wirtualną do innych sieci wirtualnych lub sieciami lokalnymi.
10. **Opcjonalnie:** usunąć zasoby, które można utworzyć w tym samouczku, wykonując kroki opisane w [zasoby zostaną usunięte](#delete-portal).

## <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

Polecenia interfejsu wiersza polecenia platformy Azure są takie same, czy można wykonywać polecenia w systemie Windows, Linux lub macOS. Jednak ma różnic skryptów powłoki systemu operacyjnego. Skrypt w poniższych krokach wykonuje powłoki Bash. 

1. [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję interfejsu wiersza polecenia Azure zainstalowane. Aby uzyskać pomoc dotyczącą poleceń interfejsu wiersza polecenia, wpisz `az <command> --help`. Zamiast instalowania interfejsu wiersza polecenia i jego wymagania wstępne, można użyć powłoki chmury Azure. Usługa Azure Cloud Shell jest bezpłatną powłoką Bash, którą można uruchamiać bezpośrednio w witrynie Azure Portal. Powłoka chmury ma Azure CLI wstępnie zainstalowane i skonfigurowane do użycia z Twoim kontem. Użycia powłoki chmury, kliknij polecenie Shell chmury (**> _**) u góry [portal](https://portal.azure.com) lub po prostu kliknij *wypróbuj* przycisk w kolejnych krokach. 
2. Jeśli uruchomiony lokalnie interfejsu wiersza polecenia, zaloguj się do platformy Azure z `az login` polecenia. Jeśli przy użyciu powłoki chmury, użytkownik jest już zalogowany.
3. Przejrzyj poniższy skrypt i jego komentarzami. W przeglądarce Skopiuj skrypt i wklej go do sesji interfejsu wiersza polecenia:

    ```azurecli-interactive
    #!/bin/bash
    
    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus
    
    # Create a virtual network with one subnet named Public.
    az network vnet create \
      --name myVnet \
      --resource-group myResourceGroup \
      --subnet-name Public
    
    # Create an additional subnet named Private in the virtual network.
    az network vnet subnet create \
      --name Private \
      --address-prefix 10.0.1.0/24 \
      --vnet-name myVnet \
      --resource-group myResourceGroup
    ```
    
4. Po zakończeniu skrypt zostanie uruchomiony, przejrzyj podsieci sieci wirtualnej. Skopiuj poniższe polecenie, a następnie wklej go do sesji interfejsu wiersza polecenia:

    ```azurecli
    az network vnet subnet list --resource-group myResourceGroup --vnet-name myVnet --output table
    ```

5. **Opcjonalnie:** wykonać dodatkowe samouczki kategorii [następne kroki](#next-steps) do filtrowania ruchu sieciowego i z każdej podsieci z grupami zabezpieczeń sieci, można kierować ruchem między podsieciami przez urządzenie wirtualne sieci , lub aby połączyć sieć wirtualną do innych sieci wirtualnych lub sieciami lokalnymi.
6. **Opcjonalne**: usunięcie zasobów, utworzonych w tym samouczku, wykonując kroki opisane w [zasoby zostaną usunięte](#delete-cli).

## <a name="powershell"></a>PowerShell

1. Zainstaluj najnowszą wersję modułu [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) programu PowerShell. Jeśli jesteś nowym użytkownikiem programu Azure PowerShell, zobacz temat [Azure PowerShell overview (Omówienie programu Azure PowerShell)](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. W sesji programu PowerShell, zaloguj się do platformy Azure z Twojej [konta Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) przy użyciu `login-azurermaccount` polecenia.

3. Przejrzyj poniższy skrypt i jego komentarzami. W przeglądarce Skopiuj skrypt i wklej go do sesji programu PowerShell:

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name myResourceGroup `
      -Location eastus
    
    # Create the public and private subnets.
    $Subnet1 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Public `
      -AddressPrefix 10.0.0.0/24
    $Subnet2 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Private `
      -AddressPrefix 10.0.1.0/24
    
    # Create a virtual network.
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Location eastus `
      -Name myVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet1,$Subnet2
    ```

4. Aby wyświetlić podsieci sieci wirtualnej, skopiuj poniższe polecenie, a następnie wklej go do sesji programu PowerShell:

    ```powershell
    $Vnet.subnets | Format-Table Name, AddressPrefix
    ```

5. **Opcjonalnie:** wykonać dodatkowe samouczki kategorii [następne kroki](#next-steps) do filtrowania ruchu sieciowego i z każdej podsieci z grupami zabezpieczeń sieci, można kierować ruchem między podsieciami przez urządzenie wirtualne sieci , lub aby połączyć sieć wirtualną do innych sieci wirtualnych lub sieciami lokalnymi.
6. **Opcjonalne**: usunięcie zasobów, utworzonych w tym samouczku, wykonując kroki opisane w [zasoby zostaną usunięte](#delete-powershell).

## <a name="resource-manager-template"></a>Szablon usługi Resource Manager

Sieć wirtualną można wdrożyć za pomocą szablonu usługi Azure Resource Manager. Aby dowiedzieć się więcej na temat szablonów, zobacz [co to jest Menedżer zasobów](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#template-deployment). Aby uzyskać dostęp do szablonu i zapoznaj się z jego parametrów, zobacz [utworzyć sieć wirtualną z dwiema podsieciami](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) szablonu. Szablonu można wdrożyć za pomocą [portal](#template-portal), [interfejsu wiersza polecenia Azure](#template-cli), lub [PowerShell](#template-powershell).

Opcjonalne kroki po wdrożeniu szablonu:

1. Zakończenie dodatkowe samouczki kategorii [następne kroki](#next-steps) do filtrowania ruchu sieciowego do i z każdej podsieci z grupami zabezpieczeń sieci, można kierować ruchem między podsieciami przez urządzenie wirtualne sieci, lub nawiązać wirtualnego sieć do innych sieci wirtualnych lub sieciami lokalnymi.
2. Usuwanie zasobów, utworzonych w tym samouczku, wykonując kroki opisane w dowolnym podsekcjach [zasoby zostaną usunięte](#delete).

### <a name="template-portal"></a>Azure portal

1. W przeglądarce otwórz [strony szablonu](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets).
2. Kliknij przycisk **wdrażanie na platformie Azure** przycisku. Jeśli jeszcze nie jest zalogowany na platformie Azure, zaloguj się na ekranie logowania do portalu Azure, który pojawia się.
3. Zaloguj się do portalu przy użyciu programu [konta Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Jeśli nie masz konta platformy Azure, możesz skorzystać z [bezpłatnej wersji próbnej](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Wprowadź następujące wartości parametrów:

    |Parametr|Wartość|
    |---|---|
    |Subskrypcja|Wybierz subskrypcję|
    |Grupa zasobów|myResourceGroup|
    |Lokalizacja|Wybierz lokalizację|
    |Nazwa sieci wirtualnej|myVnet|
    |Prefiks adresu sieci wirtualnej|10.0.0.0/16|
    |Subnet1Prefix|10.0.0.0/24|
    |Subnet1Name|Publiczne|
    |Subnet2Prefix|10.0.1.0/24|
    |Subnet2Name|Prywatne|

5. Akceptuję warunki i postanowienia, a następnie kliknij przycisk **zakupu** do wdrożenia sieci wirtualnej.

### <a name="template-cli"></a>Interfejs wiersza polecenia platformy Azure

1. [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję interfejsu wiersza polecenia Azure zainstalowane. Aby uzyskać pomoc dotyczącą poleceń interfejsu wiersza polecenia, wpisz `az <command> --help`. Zamiast instalowania interfejsu wiersza polecenia i jego wymagania wstępne, można użyć powłoki chmury Azure. Usługa Azure Cloud Shell jest bezpłatną powłoką Bash, którą można uruchamiać bezpośrednio w witrynie Azure Portal. Powłoka chmury ma Azure CLI wstępnie zainstalowane i skonfigurowane do użycia z Twoim kontem. Użycia powłoki chmury, kliknij polecenie Shell chmury **> _** przycisk w górnej części [portal](https://portal.azure.com), lub kliknij **wypróbuj** przycisk w kolejnych krokach. 
2. Jeśli uruchomiony lokalnie interfejsu wiersza polecenia, zaloguj się do platformy Azure z `az login` polecenia. Jeśli przy użyciu powłoki chmury, użytkownik jest już zalogowany.
3. Aby utworzyć grupę zasobów dla sieci wirtualnej, skopiuj poniższe polecenie i wklej go do sesji interfejsu wiersza polecenia:

    ```azurecli-interactive
    az group create --name myResourceGroup --location eastus
    ```
    
4. Szablonu można wdrożyć przy użyciu jednej z następujących opcji Parametry:
    - **Domyślne wartości parametrów**. Wprowadź następujące polecenie:
    
        ```azurecli-interactive
        az group deployment create --resource-group myResourceGroup --name VnetTutorial --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json`
        ```
    - **Wartości parametrów niestandardowych**. Pobierz i zmodyfikować szablon, przed przystąpieniem do wdrażania szablonu. Możesz również wdrożyć szablon przy użyciu parametrów wiersza polecenia lub wdrożyć szablon przy użyciu pliku oddzielne parametrów. Aby pobrać pliki szablonu i parametrów, kliknij przycisk **Przejdź w serwisie GitHub** znajdującego się na [utworzyć sieć wirtualną z dwiema podsieciami](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) strony szablonu. W usłudze GitHub kliknij **azuredeploy.parameters.json** lub **azuredeploy.json** pliku. Następnie kliknij przycisk **Raw** przycisk, aby wyświetlić plik. W przeglądarce skopiuj zawartość pliku. Zapisz zawartość do pliku na komputerze. Można zmodyfikować wartości parametrów w szablonie, lub wdrożyć szablon przy użyciu pliku oddzielne parametrów.  

    Aby dowiedzieć się więcej na temat sposobu wdrażania szablonów za pomocą tych metod, wpisz `az group deployment create --help`.

### <a name="template-powershell"></a>Środowiska PowerShell

1. Zainstaluj najnowszą wersję modułu [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) programu PowerShell. Jeśli jesteś nowym użytkownikiem programu Azure PowerShell, zobacz temat [Azure PowerShell overview (Omówienie programu Azure PowerShell)](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. W sesji programu PowerShell, aby się zalogować użytkownika [konta Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account), wprowadź `login-azurermaccount`.
3. Aby utworzyć grupę zasobów dla sieci wirtualnej, wprowadź następujące polecenie:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
    ```
    
4. Szablonu można wdrożyć przy użyciu jednej z następujących opcji Parametry:
    - **Domyślne wartości parametrów**. Wprowadź następujące polecenie:
    
        ```powershell
        New-AzureRmResourceGroupDeployment -Name VnetTutorial -ResourceGroupName myResourceGroup -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json
        ```
        
    - **Wartości parametrów niestandardowych**. Pobierz i zmodyfikować szablon przed jego wdrożeniem. Możesz również wdrożyć szablon przy użyciu parametrów wiersza polecenia lub wdrożyć szablon przy użyciu pliku oddzielne parametrów. Aby pobrać pliki szablonu i parametrów, kliknij przycisk **Przejdź w serwisie GitHub** znajdującego się na [utworzyć sieć wirtualną z dwiema podsieciami](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) strony szablonu. W usłudze GitHub kliknij **azuredeploy.parameters.json** lub **azuredeploy.json** pliku. Następnie kliknij przycisk **Raw** przycisk, aby wyświetlić plik. W przeglądarce skopiuj zawartość pliku. Zapisz zawartość do pliku na komputerze. Można zmodyfikować wartości parametrów w szablonie, lub wdrożyć szablon przy użyciu pliku oddzielne parametrów.  

    Aby dowiedzieć się więcej na temat sposobu wdrażania szablonów za pomocą tych metod, wpisz `Get-Help New-AzureRmResourceGroupDeployment`. 

## <a name="delete"></a>Usuwanie zasobów

Po zakończeniu tego samouczka można usunąć zasoby, które zostały utworzone, tak aby nie wiąże się z opłatami użycia. Usunięcie grupy zasobów powoduje usunięcie wszystkich zasobów, które znajdują się w grupie zasobów.

### <a name="delete-portal"></a>Azure portal

1. W polu wyszukiwania portalu wprowadź **myResourceGroup**. W wynikach wyszukiwania kliknij **myResourceGroup**.
2. Na **myResourceGroup** bloku, kliknij przycisk **usunąć** ikony.
3. Aby potwierdzić decyzję, w **typu nazwa grupy zasobów** wprowadź **myResourceGroup**, a następnie kliknij przycisk **usunąć**.

### <a name="delete-cli"></a>Interfejs wiersza polecenia platformy Azure

W sesji interfejsu wiersza polecenia wprowadź następujące polecenie:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>Środowiska PowerShell

W sesji programu PowerShell wpisz następujące polecenie:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się więcej o wszystkich ustawieniach podsieci i sieci wirtualnej, zobacz [Zarządzanie sieciami wirtualnymi](virtual-network-manage-network.md#view-vnet) i [Zarządzanie podsieci sieci wirtualnej](virtual-network-manage-subnet.md#create-subnet). Istnieją różne opcje dotyczące używania sieci wirtualnych i podsieci w środowisku produkcyjnym, aby spełnić różne wymagania.
- Filtrowanie ruchu przychodzącego i wychodzącego podsieci, tworząc i stosując [sieciowej grupy zabezpieczeń](virtual-networks-nsg.md) do podsieci.
- Kierować ruchem między podsieciami przez urządzenie wirtualne sieci, tworząc [trasy zdefiniowane przez użytkownika](virtual-network-create-udr-arm-ps.md) i zastosować trasy do każdej podsieci.
- Utwórz [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) lub [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) maszyny wirtualnej w ramach istniejącej sieci wirtualnej.
- Połącz dwie sieci wirtualne, tworząc [sieci wirtualnej komunikacji równorzędnej](virtual-network-peering-overview.md) między sieciami wirtualnymi.
- Połączyć sieć wirtualną z siecią lokalną przy użyciu [bramy sieci VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) lub [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) obwodu.
