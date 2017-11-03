---
title: "Maszyny Wirtualnej z wielu IP adresów 1.0 interfejsu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można przypisać wiele adresów IP do maszyny wirtualnej z wykorzystaniem 1.0 interfejsu wiersza polecenia platformy Azure | Menedżer zasobów."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
ms.openlocfilehash: 9f085dfa1fe4db36d58cb976bb550a46bf241ac7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-azure-cli-10"></a>Przypisz wielu adresów IP do maszyn wirtualnych przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 1.0

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

W tym artykule opisano sposób tworzenia maszyny wirtualnej (VM) za pośrednictwem modelu wdrażania usługi Azure Resource Manager przy użyciu 1.0 interfejsu wiersza polecenia platformy Azure. Nie można przypisać wiele adresów IP do zasobów została utworzona za pośrednictwem klasycznego modelu wdrażania. Aby dowiedzieć się więcej na temat modeli wdrażania platformy Azure, przeczytaj [zrozumieć modele wdrażania](../resource-manager-deployment-model.md) artykułu.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Utwórz maszynę Wirtualną z wielu adresów IP

Można wykonać tego zadania przy użyciu programu Azure CLI 1.0 (w tym artykule) lub [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md). Czynności, które wykonują wyjaśniono, jak utworzyć przykładowy maszyny Wirtualnej z wielu adresów IP, zgodnie z opisem w scenariuszu. Zmień nazwy zmiennych i typy adresów IP zgodnie z wymaganiami implementacji.

1. Instalowanie i konfigurowanie 1.0 interfejsu wiersza polecenia Azure, wykonując kroki opisane w [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artykułu i zaloguj się do platformy Azure konta `azure-login` polecenia.

2. Utwórz grupę zasobów:
    
    ```azurecli
    RgName=myResourceGroup
    Location=westcentralus
    azure group create --name $RgName --location $Location
    ```
3. Utwórz sieć wirtualną:

    ```azurecli
    azure network vnet create --resource-group $RgName --location $Location --name myVNet \
    --address-prefixes 10.0.0.0/16
    ```
4. Utwórz podsieć w sieci wirtualnej:

    ```azurecli
    azure network vnet subnet create --name mySubnet --resource-group $RgName --vnet-name myVNet \
    --address-prefix 10.0.0.0/24
    ```
    
5. Utwórz konto magazynu dla maszyny Wirtualnej. Przed uruchomieniem następujące polecenie, Zastąp *mojekontomagazynu* o unikatowej nazwie. Nazwa musi być unikatowa w obrębie platformy Azure:

    ```azurecli
    az storage account create --resource-group $RgName --location $Location --name mystorageaccount \
    --kind Storage --sku Standard_LRS
    ```

6. Utwórz konfiguracje protokołu IP karty Sieciowej i przypisz konfiguracje adresów IP do karty sieciowej. Można dodać, usunąć lub zmienić konfigurację, w razie potrzeby. W scenariuszu opisano następujące konfiguracje:

    **Polecenie IPConfig-1**

    Wprowadź poleceń, które należy wykonać, aby utworzyć:

    - Publiczny zasobu adresu IP za pomocą statycznego publicznego adresu IP
    - Karta sieciowa, przypisywanie publicznego adresu IP i statycznego prywatnego adresu IP do niego.
    
    Zastąp *mypublicdns* o nazwie, która jest unikatowa w lokalizacji platformy Azure.

      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP1 \
      --domain-name-label mypublicdns --allocation-method Static
        
      azure network nic create --resource-group $RgName --location $Location --name myNic1 \
      --private-ip-address 10.0.0.4 --subnet-name mySubnet --subnet-vnet-name myVNet \
      --subnet-name mySubnet --public-ip-name myPublicIP1
      ```

      > [!NOTE]
      > Publiczne adresy IP ma nominalnego opłatą. Aby dowiedzieć się więcej o cenach adresu IP, przeczytaj [cennik adres IP](https://azure.microsoft.com/pricing/details/ip-addresses) strony. Istnieje ograniczona liczba publicznych adresów IP, których można użyć w ramach subskrypcji. Aby uzyskać więcej informacji o limitach, przeczytaj artykuł dotyczący [limitów platformy Azure](../azure-subscription-service-limits.md#networking-limits).

    **Polecenie IPConfig-2**

     Wprowadź następujące polecenia, aby utworzyć nowy zasób publiczny adres IP i nową konfigurację IP ze statycznego publicznego adresu IP i statycznego prywatnego adresu IP:
    
      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP2
      --domain-name-label mypublicdns2 --allocation-method Static

      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --name IPConfig-2
      --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
      ```

    **Polecenie IPConfig 3**

    Wprowadź następujące polecenia, aby utworzyć konfigurację protokołu IP za pomocą statycznego prywatnego adresu IP i nie publiczny adres IP:

      ```azurecli
      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --private-ip-address 10.0.0.6 \
      --name IPConfig-3
      ```

    >[!NOTE] 
    >Chociaż ten artykuł przypisuje wszystkie konfiguracje adresów IP do jednej karty Sieciowej, można także przypisać wielu konfiguracji adresów IP do dowolnej karty interfejsu Sieciowego na maszynie wirtualnej. Aby dowiedzieć się, jak utworzyć Maszynę wirtualną z wieloma kartami sieciowymi, odczytać tworzenia maszyny Wirtualnej z wielu kart sieciowych artykułu.

7. Tworzenie maszyny wirtualnej z systemem Linux 

    ```azurecli
    az vm create --resource-group $RgName --name myVM1 --location $Location --nics myNic1 \
    --image UbuntuLTS --ssh-key-value ~/.ssh/id_rsa.pub --admin-username azureuser
    ```

    Aby zmienić rozmiar maszyny Wirtualnej standardowego DS2 v2, na przykład po prostu dodaj następującą właściwość `--vm-size Standard_DS3_v2` do `azure vm create` polecenia w kroku 6.

8. Wprowadź następujące polecenie, aby wyświetlić karty Sieciowej i skojarzone konfiguracje adresów IP:

    ```azurecli
    azure network nic show --resource-group $RgName --name myNic1
    ```
9. Dodaj prywatnych adresów IP do systemu operacyjnego maszyny Wirtualnej, wykonując kroki odpowiednie dla systemu operacyjnego w [adresów IP Dodaj do systemu operacyjnego maszyny Wirtualnej](#os-config) sekcji tego artykułu.

## <a name="add"></a>Dodaj adresy IP do maszyny Wirtualnej

Dodatkowe prywatne i publiczne adresy IP można dodać do istniejącej karty NIC, wykonując kroki, które należy wykonać. Przykłady opracowano na [scenariusza](#Scenario) opisane w tym artykule.

1. Otwieranie wiersza polecenia platformy Azure i wykonaj pozostałe kroki w tej sekcji w ramach jednej sesji interfejsu wiersza polecenia. Jeśli nie masz jeszcze interfejsu wiersza polecenia Azure zainstalowany i skonfigurowany, wykonaj kroki [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artykułu i zaloguj się do konta platformy Azure.

2. Wykonaj kroki jednego z następujących sekcji, w zależności od wymagań:

    - **Dodaj prywatnego adresu IP**
    
        Aby dodać prywatnego adresu IP do karty Sieciowej, należy utworzyć konfigurację protokołu IP przy użyciu poniższego polecenia. Statyczny adres musi być nieużywany adres podsieci.

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 \
        --private-ip-address 10.0.0.7 --name IPConfig-4
        ```
        Tworzenie konfiguracji tyle, ile potrzebujesz, przy użyciu nazwy konfiguracji unikatowy i prywatnych adresów IP (w przypadku konfiguracji statycznych adresów IP).

    - **Dodaj publiczny adres IP**
    
        Publiczny adres IP jest dodawany przez kojarzenie go z nową konfigurację adresu IP lub istniejącej konfiguracji adresu IP. Wykonaj kroki opisane w sekcji, które należy wykonać, ile potrzebujesz.

        > [!NOTE]
        > Publiczne adresy IP ma nominalnego opłatą. Aby dowiedzieć się więcej o cenach adresu IP, przeczytaj [cennik adres IP](https://azure.microsoft.com/pricing/details/ip-addresses) strony. Istnieje ograniczona liczba publicznych adresów IP, których można użyć w ramach subskrypcji. Aby uzyskać więcej informacji o limitach, przeczytaj artykuł dotyczący [limitów platformy Azure](../azure-subscription-service-limits.md#networking-limits).
        >

        **Skojarz zasób nowej konfiguracji adresu IP**
    
        Po dodaniu publicznego adresu IP w nowej konfiguracji adresu IP, musisz również dodać prywatnego adresu IP, ponieważ wszystkie konfiguracje adresów IP muszą mieć prywatnego adresu IP. Możesz dodać istniejący zasób publiczny adres IP lub Utwórz nową. Aby utworzyć nową, wprowadź następujące polecenie:

        ```azurecli
        azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 \
        --domain-name-label mypublicdns3
        ```

        Aby utworzyć nową konfigurację adresu IP za pomocą statycznego prywatnego adresu IP oraz skojarzonych z nimi *myPublicIP3* publicznego adresu IP adresów zasobów, wprowadź następujące polecenie:

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 \
        --private-ip-address 10.0.0.8 --public-ip-name myPublicIP3
        ```

        **Skojarz zasób do istniejącej konfiguracji adresu IP**

        Zasób publicznego adresu IP może być skojarzony tylko konfiguracją protokołu IP, który nie ma jeszcze skojarzone. Można określić, czy konfiguracja IP ma skojarzone publicznego adresu IP, wprowadzając następujące polecenie:

        ```azurecli
        azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
        ```

        Wyszukaj wiersz podobny do przedstawionego poniżej 3 IPConfig w dane wyjściowe:

        ```         
        Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet
        ```
          
        Ponieważ **publicznego adresu IP** kolumny dla *IpConfig 3* jest puste, żaden z zasobów publiczny adres IP jest obecnie z nią skojarzona. Dodaj istniejący zasób publicznego adresu IP do IpConfig 3 lub wprowadź następujące polecenie, aby go utworzyć:

        ```azurecli
        azure network public-ip create --resource-group  myResourceGroup --location westcentralus \
        --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
        ```

        Wprowadź następujące polecenie, aby skojarzyć zasób publicznego adresu IP do istniejącej konfiguracji IP o nazwie *IPConfig 3*:
        ```azurecli
        azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 \
        --public-ip-name myPublicIP3
        ```

3. Wyświetl prywatnych adresów IP i zasoby adresów publicznych adresów IP przypisanych do karty Sieciowej, wprowadzając następujące polecenie:

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

      Zwrócony wynik jest podobny do następującego:
      ```
      Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        
      default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
      IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
      IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet  myPublicIP3
      ```
4. Dodaj prywatnych adresów IP dodany do karty Sieciowej do maszyny Wirtualnej systemu operacyjnego, zgodnie z instrukcjami w [adresów IP Dodaj do systemu operacyjnego maszyny Wirtualnej](#os-config) sekcji tego artykułu. Nie dodawaj publiczne adresy IP do systemu operacyjnego.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
