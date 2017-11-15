---
title: "Utwórz ruchu sieciowego trasy do trasy zdefiniowane przez użytkownika za pośrednictwem sieci urządzenie wirtualne - wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak tworzona jest trasa zdefiniowana przez użytkownika, aby zastąpić domyślny Azure routingu przez routingu ruchu sieciowego przez urządzenie wirtualne sieci."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/16/2017
ms.author: jdial
ms.openlocfilehash: adfcf53f9fca0efafb538edfd65b95313dcf1559
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-user-defined-route---azure-cli"></a>Tworzenie trasy zdefiniowane przez użytkownika - wiersza polecenia platformy Azure

W tym samouczku, Dowiedz się, jak utworzyć trasy zdefiniowane przez użytkownika, aby kierować ruchem między dwiema [sieci wirtualnej](virtual-networks-overview.md) podsieci przez urządzenie wirtualne sieci. Urządzenie wirtualne sieci jest maszyny wirtualnej, która uruchamia aplikację sieciową, takie jak zapory. Aby dowiedzieć się więcej na temat wstępnie skonfigurowanej sieci wirtualnych urządzeń, które można wdrożyć w sieci wirtualnej platformy Azure, zobacz [portalu Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

Po utworzeniu podsieci w sieci wirtualnej Azure tworzy domyślne [tras systemowych](virtual-networks-udr-overview.md#system-routes) umożliwiających zasoby we wszystkich podsieciach do komunikowania się ze sobą, jak pokazano na poniższej ilustracji:

![Trasy domyślne](./media/create-user-defined-route/default-routes.png)

W tym samouczku utworzysz sieć wirtualną z publiczne, prywatne i strefą DMZ podsieci, jak pokazano na rysunku poniżej. Zwykle serwery sieci web mógł zostać wdrożony do publicznego podsieci i aplikacji lub serwer bazy danych może być wdrożona w sieci prywatnej. Utwórz maszynę wirtualną na działanie jako urządzenie wirtualne sieci w podsieci DMZ i opcjonalnie, utwórz maszynę wirtualną w każdej podsieci, które komunikują się za pośrednictwem sieci urządzenia wirtualnego. Cały ruch między podsieciami publiczne i prywatne jest kierowany przez urządzenia, jak pokazano na poniższej ilustracji:

![Trasy definiowane przez użytkownika](./media/create-user-defined-route/user-defined-routes.png)

Ten artykuł zawiera kroki umożliwiające utworzenie trasy zdefiniowanej przez użytkownika za pośrednictwem modelu wdrażania usługi Resource Manager, który jest modelem wdrożenia, które firma Microsoft zaleca używanie podczas tworzenia tras zdefiniowanych przez użytkownika. Jeśli konieczne jest utworzenie trasy zdefiniowanej przez użytkownika (klasyczne), zobacz [utworzenie trasy zdefiniowanej przez użytkownika (klasyczne)](virtual-network-create-udr-classic-cli.md). Jeśli nie masz doświadczenia w obsłudze modele wdrażania platformy Azure, zobacz [modele wdrażania zrozumieć Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Aby dowiedzieć się więcej na temat trasy zdefiniowane przez użytkownika, zobacz [trasy zdefiniowane przez użytkownika — omówienie](virtual-networks-udr-overview.md#user-defined).

## <a name="create-routes-and-network-virtual-appliance"></a>Tworzenie trasy i urządzenie wirtualne sieci

Polecenia interfejsu wiersza polecenia platformy Azure są takie same, czy można wykonywać polecenia w systemie Windows, Linux lub macOS. Jednak ma różnic skryptów powłoki systemu operacyjnego. Wykonywanie skryptów w poniższych krokach w wymagają instalacji i uruchamiania poleceń powłoki Bash wiersza polecenia platformy Azure. Można [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json) na komputerze, lub po prostu kliknij **wypróbuj** przycisk w żadnym skryptów do wykonywania skryptów w powłoce chmury Azure.
 
1. **Wymagań wstępnych**: tworzenie sieci wirtualnej z dwoma podsieciami, wykonując kroki opisane w [utworzyć sieć wirtualną](#create-a-virtual-network).
2. Jeśli uruchomiona wiersza polecenia platformy Azure z komputera, zaloguj się do Azure z `az login` polecenia. Jeśli przy użyciu powłoki chmury, automatycznie zalogowano.
3. Ustaw zmienne kilka używanych w całym do pozostałych kroków:

    ```azurecli-interactive
    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    ```

4. Utwórz *DMZ* podsieci w sieci wirtualnej utworzone w sekcji wymagań wstępnych:

    ```azurecli-interactive
    az network vnet subnet create \
      --name DMZ \
      --address-prefix 10.0.2.0/24 \
      --vnet-name myVnet \
      --resource-group $rgName
    ```

5. Utwórz maszynę wirtualną NVA. Przypisz statyczne publiczne i prywatne adresy IP do interfejsu sieciowego, który tworzy interfejsu wiersza polecenia. Statyczne adresy nie zmieniają przez cały okres istnienia maszyny wirtualnej. Analizę NVA można maszynę wirtualną z systemem operacyjnym Linux lub Windows. Aby utworzyć maszynę wirtualną, skopiuj skrypt dla obu systemów operacyjnych i wklej go do interfejsu wiersza polecenia. Jeśli tworzenie maszyny Wirtualnej systemu Windows, wklej skrypt do edytora tekstu, zmień wartość atrybutu *AdminPassword* zmiennej, a następnie wklej zmieniony tekst do interfejs wiersza polecenia.

    **Linux**

    ```azurecli-interactive
    az vm create \
      --resource-group $rgName \
      --name myVm-Nva \
      --image UbuntuLTS \
      --private-ip-address 10.0.2.4 \
      --public-ip-address myPublicIp-myVm-Nva \
      --public-ip-address-allocation static \
      --subnet DMZ \
      --vnet-name myVnet \
      --admin-username azureuser \
      --generate-ssh-keys
    ```

    **Windows**

    ```azurecli-interactive
    AdminPassword=ChangeToYourPassword
    az vm create \
      --resource-group $rgName \
      --name myVm-Nva \
      --image win2016datacenter \
      --private-ip-address 10.0.2.4 \
      --public-ip-address myPublicIp-myVm-Nva \
      --public-ip-address-allocation static \
      --subnet DMZ \
      --vnet-name myVnet \
      --admin-username azureuser \
      --admin-password $AdminPassword      
    ```

6. Włącz przesyłanie dalej IP dla interfejsu sieciowego NVA. Włączenie IP przesyłania dalej dla interfejsu sieciowego powoduje Azure nie sprawdzić źródłowego i docelowego adresu IP. Jeśli nie włączysz to ustawienie, ruch kierowany do adresu IP innego niż karty interfejsu Sieciowego, który odbiera, zostało przerwane przez platformę Azure.

    ```azurecli-interactive
    az network nic update \
      --name myVm-NvaVMNic \
      --resource-group $rgName \
      --ip-forwarding true
    ```

7. Tworzy tabelę tras dla *publicznego* podsieci.

    ```azurecli-interactive
    az network route-table create \
      --name myRouteTable-Public \
      --resource-group $rgName
    ```
    
8. Domyślnie program Azure tras ruchu między wszystkich podsieci w sieci wirtualnej. Utwórz trasę, aby zmienić domyślne Azure routingu, tak aby ruch z publicznego podsieci prywatne podsieci jest kierowany przez NVA, zamiast bezpośrednio do podsieci prywatnego.

    ```azurecli-interactive    
    az network route-table route create \
      --name ToPrivateSubnet \
      --resource-group $rgName \
      --route-table-name myRouteTable-Public \
      --address-prefix 10.0.1.0/24 \
      --next-hop-type VirtualAppliance \
      --next-hop-ip-address 10.0.2.4
    ```

9. Skojarz *publicznego myRouteTable* tabeli tras do *publicznego* podsieci. Kojarzenie tabelę tras z podsiecią powoduje, że Azure, aby rozsyłać cały ruch wychodzący z podsieci zgodnie z tras w tabeli tras. Tabela tras może być skojarzona z zero lub wiele podsieci, podsieci, mogą mieć zero lub jedną tabelę tras skojarzony.

    ```azurecli-interactive
    az network vnet subnet update \
      --name Public \
      --vnet-name myVnet \
      --resource-group $rgName \
      --route-table myRouteTable-Public
    ```

10. Utwórz tabelę tras dla *prywatnej* podsieci.

    ```azurecli-interactive
    az network route-table create \
      --name myRouteTable-Private \
      --resource-group $rgName
    ```
      
11. Utwórz ruch trasy do trasy, z *prywatnej* podsieci do *publicznego* podsieci za pośrednictwem NVA maszyny wirtualnej.

    ```azurecli-interactive
    az network route-table route create \
      --name ToPublicSubnet \
      --resource-group $rgName \
      --route-table-name myRouteTable-Private \
      --address-prefix 10.0.0.0/24 \
      --next-hop-type VirtualAppliance \
      --next-hop-ip-address 10.0.2.4
    ```

12. Skojarz tabeli tras do *prywatnej* podsieci.

    ```azurecli-interactive
    az network vnet subnet update \
      --name Private \
      --vnet-name myVnet \
      --resource-group $rgName \
      --route-table myRouteTable-Private
    ```
    
13. **Opcjonalnie:** Utwórz maszynę wirtualną w podsieciach publicznej i prywatnej, a następnie sprawdź, czy komunikacji między maszynami wirtualnymi jest kierowany przez urządzenie wirtualne sieci, wykonując kroki opisane w [zweryfikowaćroutingu](#validate-routing).
14. **Opcjonalne**: Aby usunąć zasoby, które możesz utworzyć w tym samouczku, wykonaj kroki [zasoby zostaną usunięte](#delete-resources).

## <a name="validate-routing"></a>Sprawdź poprawność routingu

1. Jeśli nie jest jeszcze, wykonaj kroki [tworzenia tras i urządzenie wirtualne sieci](#create-routes-and-network-virtual-appliance).
2. Kliknij przycisk **wypróbuj** przycisk w polu poniżej, które otwiera powłokę chmury Azure. W przypadku wyświetlenia monitu zaloguj się do platformy Azure przy użyciu Twojej [konta Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Jeśli nie masz konta platformy Azure, możesz skorzystać z [bezpłatnej wersji próbnej](https://azure.microsoft.com/offers/ms-azr-0044p). Powłoka chmury Azure jest powłoki bash wolnego z interfejsu wiersza polecenia platformy Azure preinstalowany. 

    Następujące skrypty utworzyć dwie maszyny wirtualne w *publicznego* podsieci i jeden w *prywatnej* podsieci. Skrypty również włączyć funkcję przekazywania pakietów IP dla interfejsu sieciowego w ramach systemu operacyjnego NVA, aby włączyć system operacyjny do routowania ruchu za pośrednictwem interfejsu sieciowego. Zwykle produkcji NVA sprawdza ruch przed przesłaniem go, ale w tym samouczku proste NVA tylko kieruje ruch na bez sprawdzania go. 

    Kliknij przycisk **kopiowania** przycisk **Linux** lub **Windows** skrypty, które należy wykonać i Wklej zawartość skryptu w edytorze tekstów. Zmień hasło *adminPassword* zmiennej, a następnie wklej skrypt powłokę chmury Azure. Uruchom skrypt dla systemu operacyjnego wybranego podczas tworzenia urządzenia wirtualnego sieci w kroku 5 [tworzenia tras i urządzenie wirtualne sieci](#create-routes-and-network-virtual-appliance). 

    **Linux**

    ```azurecli-interactive
    #!/bin/bash

    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    adminPassword=ChangeToYourPassword
    
    # Create a virtual machine in the Public subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Public \
      --image UbuntuLTS \
      --vnet-name myVnet \
      --subnet Public \
      --public-ip-address myPublicIp-Public \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Create a virtual machine in the Private subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Private \
      --image UbuntuLTS \
      --vnet-name myVnet \
      --subnet Private \
      --public-ip-address myPublicIp-Private \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Enable IP forwarding for the network interface in the NVA virtual machine's operating system.    
    az vm extension set \
      --resource-group $rgName \
      --vm-name myVm-Nva \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
    ```

    **Windows**

    ```azurecli-interactive

    #!/bin/bash
    #Set variables used in the script.
    rgName="myResourceGroup"
    location="eastus"
    adminPassword=ChangeToYourPassword
    
    # Create a virtual machine in the Public subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Public \
      --image win2016datacenter \
      --vnet-name myVnet \
      --subnet Public \
      --public-ip-address myPublicIp-Public \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Allow pings through the Windows Firewall.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Public \
      --resource-group $rgName \
      --settings '{"commandToExecute":"netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}'

    # Create a virtual machine in the Private subnet.
    az vm create \
      --resource-group $rgName \
      --name myVm-Private \
      --image win2016datacenter \
      --vnet-name myVnet \
      --subnet Private \
      --public-ip-address myPublicIp-Private \
      --admin-username azureuser \
      --admin-password $adminPassword

    # Allow pings through the Windows Firewall.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Private \
      --resource-group $rgName \
      --settings '{"commandToExecute":"netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}'

    # Enable IP forwarding for the network interface in the NVA virtual machine's operating system.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Nva \
      --resource-group $rgName \
      --settings '{"commandToExecute":"powershell.exe Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1"}'

    # Restart the NVA virtual machine.
    az vm extension set \
      --publisher Microsoft.Compute \
      --version 1.9 \
      --name CustomScriptExtension \
      --vm-name myVm-Nva \
      --resource-group $rgName \
      --settings '{"commandToExecute":"powershell.exe Restart-Computer -ComputerName myVm-Nva -Force"}'
    ```

3. Sprawdź poprawność komunikacji między maszynami wirtualnymi w podsieci publicznej i prywatnej. 

    - Otwórz [SSH](../virtual-machines/linux/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-vm) (Linux) lub [pulpitu zdalnego](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-vm) połączenia (system Windows) do publicznego adresu IP *myVm publicznego* maszyny wirtualnej.
    - W wierszu polecenia na *publicznego myVm* maszyny wirtualnej, wprowadź `ping myVm-Private`. Odpowiedzi jest wyświetlany, ponieważ analizę NVA kieruje ruchem i zapobieganie publicznemu dostępowi do podsieci prywatnego.
    - Z *myVm publicznego* maszyny wirtualnej, uruchom śledzenie trasy między maszynami wirtualnymi w podsieci publiczne i prywatne. Wprowadź odpowiednie polecenie poniżej, w zależności od systemu operacyjnego zainstalowanych na maszynach wirtualnych w podsieciach publicznej i prywatnej:
        - **Windows**: W wierszu polecenia Uruchom `tracert myvm-private` polecenia.
        - **Ubuntu**: Uruchom `tracepath myvm-private` polecenia.
      Ruch przechodzi przez 10.0.2.4 (NVA) przed osiągnięciem 10.0.1.4 (maszynę wirtualną w podsieci prywatne). 
    - Wykonać poprzednie kroki, łącząc się z *prywatnego myVm* maszyny wirtualnej i polecenie ping *myVm publicznego* maszyny wirtualnej. Śledzenie trasy zawiera komunikacji określanego 10.0.2.4 przed osiągnięciem 10.0.0.4 (maszynę wirtualną w publicznych podsieci).
      
      > [!NOTE]
      > Poprzednie kroki umożliwiają potwierdzenie routingu między Azure prywatnych adresów IP. Jeśli chcesz przekazać lub serwer proxy, ruch do publicznego adresu IP adresów przez urządzenie wirtualne sieci:
      > - Urządzenie musi zapewnić translatora adresów sieciowych lub możliwości serwera proxy. Jeśli translatora adresów sieciowych, urządzenie musi tłumaczenie źródła adresów IP, adres do jego własnej i przesyłania dalej tego żądania do publicznego adresu IP. Czy urządzenie ma adres sieciowy translacji adresu źródłowego lub używany jako serwer proxy, Azure tłumaczy urządzenie wirtualne sieci prywatnego adresu IP do publicznego adresu IP. Aby uzyskać więcej informacji o różnych metodach Azure używa do przekształcania prywatnych adresów IP na publiczne adresy IP, zobacz [opis połączenia wychodzące](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
      > - Dodatkowe trasy w tabeli tras, takie jak prefiks: wartość 0.0.0.0/0, typ następnego przeskoku VirtualAppliance i następnego przeskoku adresów IP 10.0.2.4 (w poprzednim przykładowy skrypt).
      >
    - **Opcjonalnie**: Użyj następnego przeskoku możliwości obserwatora sieciowego Azure można sprawdzić poprawności następnego przeskoku między dwiema maszynami wirtualnymi w obrębie platformy Azure. Przed użyciem obserwatora sieciowego, należy najpierw [utworzenia wystąpienia obserwatora sieciowego Azure](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json) dla regionu, którego chcesz użyć w. W tym samouczku jest używany nam wschodnie regionu. Po aktywowaniu wystąpienia obserwatora sieciowego, dla regionu, wprowadź następujące polecenie, aby zobaczyć informacje o następnego przeskoku między maszynami wirtualnymi w podsieci publicznej i prywatnej:
     
        ```azurecli-interactive
        az network watcher show-next-hop --resource-group myResourceGroup --vm myVm-Public --source-ip 10.0.0.4 --dest-ip 10.0.1.4
        ```

       Zwraca dane wyjściowe *10.0.2.4* jako **adres IP następnego przeskoku** i *VirtualAppliance* jako **Typ następnego przeskoku**.

> [!NOTE]
> W celu zilustrowania koncepcji, w tym samouczku, publicznych adresów IP przypisanych do maszyn wirtualnych w publicznej i prywatnej podsieci i dostępu do portu całej sieci jest włączona w systemie Azure dla obu maszyn wirtualnych. Podczas tworzenia maszyn wirtualnych w środowisku produkcyjnym, publicznych adresów IP nie może przypisać do nich i mogą filtrować ruch sieciowy do podsieci prywatne, wdrażając urządzenie wirtualne sieci przed nim lub przypisywanie sieciowej grupy zabezpieczeń do podsieci, Interfejs sieciowy, lub obie. Aby dowiedzieć się więcej na temat grup zabezpieczeń sieci, zobacz [sieciowej grupy zabezpieczeń](virtual-networks-nsg.md).

## <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej

Ten samouczek wymaga istniejącej sieci wirtualnej z dwoma podsieciami. Kliknij przycisk **wypróbuj** przycisk w polu poniżej, aby szybko utworzyć sieć wirtualną. Kliknięcie przycisku **wypróbuj** przycisk otwiera [powłoki chmury Azure](../cloud-shell/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Chociaż powłoki chmury programu PowerShell lub powłoki Bash, jest uruchamiany w tej sekcji, powłoki Bash służy do tworzenia sieci wirtualnej. Powłoki Bash ma interfejsu wiersza polecenia platformy Azure zainstalowany. W przypadku wyświetlenia monitu przez powłokę chmury, zaloguj się do platformy Azure przy użyciu Twojej [konta Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Jeśli nie masz konta platformy Azure, możesz skorzystać z [bezpłatnej wersji próbnej](https://azure.microsoft.com/offers/ms-azr-0044p). Aby utworzyć sieć wirtualna używana w tym samouczku, kliknij przycisk **kopiowania** przycisku w polu poniżej, a następnie wkleić skrypt powłoki chmury Azure:

```azurecli-interactive
#!/bin/bash

#Set variables used in the script.
rgName="myResourceGroup"
location="eastus"

# Create a resource group.
az group create \
  --name $rgName \
  --location $location

# Create a virtual network with one subnet named Public.
az network vnet create \
  --name myVnet \
  --resource-group $rgName \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24

# Create an additional subnet named Private in the virtual network.
az network vnet subnet create \
  --name Private \
  --address-prefix 10.0.1.0/24 \
  --vnet-name myVnet \
  --resource-group $rgName
```

Aby dowiedzieć się więcej na temat sposobu korzystania z portalu, programu PowerShell lub szablonu usługi Azure Resource Manager można utworzyć sieci wirtualnej, zobacz [utworzyć sieć wirtualną](virtual-networks-create-vnet-arm-pportal.md).

## <a name="delete-resources"></a>Usuwanie zasobów

Po zakończeniu tego samouczka można usunąć zasoby, które zostały utworzone, tak aby nie wiąże się z opłatami użycia. Usunięcie grupy zasobów powoduje usunięcie wszystkich zasobów, które znajdują się w grupie zasobów. W sesji interfejsu wiersza polecenia wprowadź następujące polecenie:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Następne kroki

- Utwórz [urządzenie wirtualne wysokiej dostępności sieci](/azure/architecture/reference-architectures/dmz/nva-ha?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Urządzenie wirtualne sieci często mają wiele interfejsów sieciowych i adresów IP przypisanych do nich. Dowiedz się, jak [dodać interfejsów sieciowych do istniejącej maszyny wirtualnej](virtual-network-network-interface-vm.md#vm-add-nic) i [dodać adresy IP do istniejącego interfejsu sieciowego](virtual-network-network-interface-addresses.md#add-ip-addresses). Chociaż wszystkich rozmiarów maszyn wirtualnych może mieć co najmniej dwa interfejsy sieci dołączone, każdego rozmiaru maszyny wirtualnej obsługuje maksymalna liczba interfejsów sieciowych. Aby dowiedzieć się, jak wiele interfejsów sieciowych każdego wirtualnego komputera obsługuje rozmiaru, zobacz [Windows](../virtual-machines/windows/sizes.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json) i [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) rozmiarów maszyn wirtualnych. 
- Utworzenie trasy zdefiniowanej przez użytkownika do wymuszanie tunelowania ruchu lokalnej za pośrednictwem [połączenia sieci VPN lokacja lokacja](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
