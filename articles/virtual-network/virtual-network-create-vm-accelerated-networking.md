---
title: "Utwórz maszynę wirtualną platformy Azure za pomocą sieci przyspieszony | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć maszynę wirtualną za pomocą przyspieszony sieci."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06b77ce5b6f15e3dae4a7d4bad76def949774678
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-virtual-machine-with-accelerated-networking"></a>Utwórz maszynę wirtualną za pomocą przyspieszony sieci

Z tego samouczka dowiesz sposób tworzenia maszyn wirtualnych Azure (VM) za pomocą przyspieszony sieci. Przyspieszone sieci jest GA dla systemu Windows i w publicznej wersji zapoznawczej dla określonych dystrybucje systemu Linux. Przyspieszone sieci umożliwia wirtualizację We/Wy z jednym elementem głównym (SR-IOV) do maszyny Wirtualnej, znacznie poprawia wydajność sieci. Ta ścieżka wysokiej wydajności pomija hosta z ścieżki danych, zmniejszając czas oczekiwania, zakłócenia i użycie procesora CPU do użycia z najbardziej wymagających obciążeń sieci na obsługiwanych typów maszyny Wirtualnej. Na poniższej ilustracji przedstawiono komunikację między dwóch maszyn wirtualnych (VM) z włączonymi i wyłączonymi przyspieszonego sieci:

![Porównanie](./media/virtual-network-create-vm-accelerated-networking/image1.png)

Bez przyspieszonego sieci hosta i przełącznik wirtualny musi przejść przez cały ruch sieciowy i maszyny Wirtualnej. Przełącznik wirtualny udostępnia wszystkie egzekwowanie zasad, takich jak grupy zabezpieczeń sieci, list kontroli dostępu, izolacji i innych usług z wirtualizacją sieci dla ruchu sieciowego. Aby dowiedzieć się więcej na temat przełączników wirtualnych, przeczytaj [wirtualizacji sieci funkcji Hyper-V i przełącznik wirtualny](https://technet.microsoft.com/library/jj945275.aspx) artykułu.

Z przyspieszonego w sieci, ruch sieciowy dociera do maszyny Wirtualnej interfejsu sieciowego (NIC), a następnie jest przekazywany do maszyny Wirtualnej. Wszystkie zasady sieciowe, których dotyczy przełącznika wirtualnego bez przyspieszonego sieci Odciążone i zastosować sprzętu. Stosowanie zasad w sprzęcie umożliwia kartę Sieciową do przesyłania dalej ruchu sieciowego bezpośrednio do maszyny Wirtualnej, pomijanie hosta i przełącznik wirtualny, przy zachowaniu zasady zastosowaniu na hoście.

Korzyści wynikające z przyspieszonego sieci dotyczą tylko maszynę Wirtualną, która jest włączone. Aby uzyskać najlepsze wyniki to idealne rozwiązanie w celu włączenia tej funkcji na co najmniej dwie maszyny wirtualne podłączone do tej samej sieci wirtualnej platformy Azure (VNet). Podczas komunikacji między sieciami wirtualnymi lub łączącego lokalnymi, ta funkcja ma minimalny wpływ na ogólną opóźnienia.

> [!WARNING]
> Publicznej wersji zapoznawczej tego systemu Linux nie może mieć taki sam poziom dostępności i niezawodności, zgodnie z funkcji, które są zwykle dostępności wersji. Funkcja nie jest obsługiwane, mogą mieć ograniczone możliwości i mogą nie być dostępne we wszystkich lokalizacjach Azure. Najbardziej aktualne powiadomień o dostępności i stan tej funkcji należy sprawdzić stronę aktualizacji sieci wirtualnej platformy Azure.

## <a name="benefits"></a>Korzyści
* **Zmniejszyć opóźnienia / wyższy pakietów na sekundę (pps):** usunięcie przełącznika wirtualnego z ścieżki danych usuwa czasu, jaki poświęcają pakietów na hoście dla przetwarzanie zasad i zwiększa liczbę pakietów, które mogą być przetwarzane w ramach maszyny Wirtualnej.
* **Zmniejszona zakłócenia:** przełącznik wirtualny przetwarzania zależy od ilości zasad, które musi zostać zastosowana i obciążenia procesora CPU, który wykonuje przetwarzanie. Odciążanie wymuszanie zasad na sprzęcie usuwa tego zmienności dostarczania pakietów bezpośrednio do maszyny Wirtualnej, usunięcie hosta do komunikacji maszyny Wirtualnej i wszystkich oprogramowania przerwań i przełączenia kontekstu.
* **Zmniejszyć użycie procesora CPU:** pomijanie przełącznika wirtualnego na hoście prowadzi do mniej użycie procesora CPU do przetwarzania ruchu sieciowego.

## <a name="Limitations"></a>Ograniczenia
Podczas przy użyciu tej możliwości istnieją następujące ograniczenia:

* **Tworzenie interfejsu sieci:** akcelerowanego sieci można włączyć tylko dla nowych kart sieciowych. Nie można włączyć dla istniejącej karty sieciowej.
* **Tworzenie maszyny Wirtualnej:** A kart interfejsu Sieciowego z włączoną obsługą przyspieszonego sieci może zostać dołączona tyko do maszyny Wirtualnej po utworzeniu maszyny Wirtualnej. Nie można dołączyć karty Sieciowej do istniejącej maszyny Wirtualnej.
* **Regiony:** maszyn wirtualnych systemu Windows z przyspieszonego w sieci jest oferowanych w regionach najbardziej platformy Azure. Maszyn wirtualnych systemu Linux z przyspieszonego sieci są dostępne w wielu regionach. Rozwija regionów, w których ta funkcja jest dostępna w. Zobacz blog Azure aktualizacje dla sieci wirtualnej poniżej najnowsze informacje.   
* **Obsługiwane systemy operacyjne:** systemu Windows: Microsoft Windows Server 2012 R2 Datacenter i Windows Server 2016. Linux: Ubuntu Server 16.04 LTS i 4.4.0-77 jądra lub nowszą, SLES 12 z dodatkiem SP2, RHEL 7.3 i CentOS 7.3 (opublikowanej przez "Nieautoryzowanego Wave oprogramowanie").
* **Rozmiar maszyny Wirtualnej:** ogólnego przeznaczenia i rozmiary obliczeniowe są zoptymalizowane pod kątem wystąpienia z co najmniej osiem rdzeni. Aby uzyskać więcej informacji, zobacz [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) i [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artykuły rozmiary maszyny Wirtualnej. Zbiór rozmiarów maszyn wirtualnych obsługiwanych w wystąpieniu rozwinie w przyszłości.
* **Tylko wdrożenia za pośrednictwem usługi Azure Resource Manager (ARM):** przyspieszony sieci nie jest dostępny do wdrożenia za pomocą funkcji ASM/frontonu REDDOG.

Zmiany tych ograniczeń są ogłaszane za pośrednictwem [sieci wirtualnych Azure aktualizuje](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview/) strony.

## <a name="create-a-windows-vm"></a>Tworzenie maszyny wirtualnej z systemem Windows
Możesz użyć portalu Azure lub Azure [PowerShell](#windows-powershell) do utworzenia maszyny Wirtualnej.

### <a name="windows-portal"></a>Portal

1. Za pomocą przeglądarki internetowej, otwórz Azure [portal](https://portal.azure.com) i zaloguj się przy użyciu platformy Azure [konta](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Jeśli nie masz już konto, możesz zarejestrować się w celu [bezpłatnej wersji próbnej](https://azure.microsoft.com/offers/ms-azr-0044p).
2. W portalu kliknij **+ nowy** > **obliczeniowe** > **systemu Windows Server Datacenter 2016**. 
3. W **systemu Windows Server Datacenter 2016** wyświetlonym bloku, pozostaw *Resource Manager* wybranego w obszarze **wybierz model wdrożenia**i kliknij przycisk **Utwórz** .
4. W **podstawy** bloku, zostanie wyświetlone, wprowadź następujące wartości, pozostaw pozostałe opcje domyślne lub wybierz lub wprowadź własne wartości i kliknij przycisk **OK** przycisk:

    |Ustawienie|Wartość|
    |---|---|
    |Nazwa|MyVm|
    |Grupa zasobów|Pozostaw **Utwórz nowy** zaznaczone, a następnie wprowadź *MyResourceGroup*|
    |Lokalizacja|Zachodnie stany USA 2|

    Jeśli jesteś nowym użytkownikiem usługi Azure, Dowiedz się więcej o [grup zasobów](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [subskrypcje](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), i [lokalizacje](https://azure.microsoft.com/regions) (które są również nazywane regionów).
5. W **wybierz rozmiar** bloku, zostanie wyświetlone, wprowadź *8* w **minimalna liczba rdzeni** polu, a następnie kliknij przycisk **Wyświetl wszystkie**.
6. Kliknij przycisk **DS4_V2 standardowe**, lub dowolny obsługiwany maszyny Wirtualnej, kliknij przycisk **wybierz** przycisku.
7. W **ustawienia** bloku, zostanie wyświetlone, pozostaw wszystkie ustawienia jako — jest, z wyjątkiem kliknij **włączone** w obszarze **przyspieszony sieci**, następnie kliknij przycisk **OK** przycisku. **Uwaga:** , jeśli w poprzednich krokach wybrane wartości dla rozmiaru maszyny Wirtualnej, system operacyjny lub lokalizacji, które nie są wymienione w [ograniczenia](#Limitations) sekcji tego artykułu **przyspieszony sieci** nie jest widoczny.
8. W **Podsumowanie** bloku, który jest wyświetlany, kliknij przycisk **OK** przycisku. Azure rozpoczyna tworzenie maszyny Wirtualnej. Tworzenie maszyny Wirtualnej trwa kilka minut.
9. Aby zainstalować sterownik sieciowy przyspieszonego dla systemu Windows, wykonaj kroki [Konfigurowanie systemu Windows](#configure-windows) sekcji tego artykułu.

### <a name="windows-powershell"></a>Środowiska PowerShell
1. Zainstaluj najnowszą wersję programu Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modułu. Jeśli jesteś nowym użytkownikiem programu Azure PowerShell, przeczytaj [Omówienie programu Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) artykułu.
2. Uruchom sesję programu PowerShell, klikając przycisk Start systemu Windows, wpisując **powershell**, klikając **programu PowerShell** w wynikach wyszukiwania.
3. W oknie programu PowerShell, wprowadź `login-azurermaccount` polecenie, aby zalogować się przy użyciu platformy Azure [konta](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Jeśli nie masz już konto, możesz zarejestrować się w celu [bezpłatnej wersji próbnej](https://azure.microsoft.com/offers/ms-azr-0044p).
4. W przeglądarce Skopiuj poniższy skrypt:
    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Windows `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id 

    # Create the virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    #
    ```
5. W oknie programu PowerShell kliknij prawym przyciskiem myszy w celu Wklej skrypt i uruchom jej wykonanie. Zostanie wyświetlony monit o nazwę użytkownika i hasło. Te poświadczenia umożliwiają Zaloguj się do maszyny Wirtualnej, gdy nawiązuje z nim połączenie w następnym kroku. Jeśli skrypt zakończy się niepowodzeniem, a wartości w skrypcie zostały zmienione przed jego wykonaniem, potwierdź wartości używanych dla rozmiaru maszyny Wirtualnej, system operacyjny i lokalizacja znajduje się w [ograniczenia](#Limitations) sekcji tego artykułu.
6. Aby zainstalować sterownik sieciowy przyspieszonego dla systemu Windows, wykonaj kroki [Konfigurowanie systemu Windows](#configure-windows) sekcji tego artykułu.

### <a name="configure-windows"></a>Konfigurowanie systemu Windows
Po utworzeniu maszyny Wirtualnej na platformie Azure, należy zainstalować sterownik sieciowy przyspieszonego dla systemu Windows. Przed wykonaniem poniższych kroków, należy utworzyć Maszynę wirtualną systemu Windows z przyspieszonego w sieci za pomocą [portal](#windows-portal) lub [PowerShell](#windows-powershell) kroków w tym artykule. 

1. Za pomocą przeglądarki internetowej, otwórz Azure [portal](https://portal.azure.com) i zaloguj się przy użyciu platformy Azure [konta](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Jeśli nie masz już konto, możesz zarejestrować się w celu [bezpłatnej wersji próbnej](https://azure.microsoft.com/offers/ms-azr-0044p).
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *MyVm*. Gdy **MyVm** pojawia się w wynikach wyszukiwania kliknij ją.
3. W **MyVm** bloku, który jest wyświetlany, kliknij przycisk **Connect** przycisk w lewym górnym rogu bloku. **Uwaga:** Jeśli **tworzenie** jest widoczna w obszarze **Connect** przycisku Azure nie została jeszcze ukończona tworzenia maszyny Wirtualnej. Kliknij przycisk **Connect** tylko wtedy, gdy nie są już wyświetlane **tworzenie** w obszarze **Connect** przycisku.
4. Zezwalaj na przeglądarkę, aby pobrać **MyVm.rdp** pliku.  Po pobraniu, kliknij plik, aby go otworzyć. 
5. Kliknij przycisk **Connect** przycisk **Podłączanie pulpitu zdalnego** wyświetlonym, informujący, że nie można zidentyfikować wydawcy połączenia zdalnego.
6. W **zabezpieczenia systemu Windows** okno zostanie wyświetlone, kliknij przycisk **więcej opcji**, następnie kliknij przycisk **korzystała z innego konta**. Wprowadź nazwę użytkownika i hasła podanego w kroku 4, a następnie kliknij przycisk **OK** przycisku.
7. Kliknij przycisk **tak** przycisk w polu Podłączanie pulpitu zdalnego, który pojawi się powiadomienie, że nie można zweryfikować tożsamości komputera zdalnego.
8. Kliknij prawym przyciskiem myszy przycisk Start systemu Windows, a następnie kliknij przycisk **Menedżera urządzeń**. Rozwiń węzeł **karty sieciowe** węzła. Upewnij się, że **Mellanox ConnectX 3 wirtualnej funkcja Ethernet karty** pojawia się, jak pokazano na poniższej ilustracji:
   
    ![Menedżer urządzeń](./media/virtual-network-create-vm-accelerated-networking/image2.png)

9. Przyspieszone sieci jest teraz włączony dla maszyny Wirtualnej.

## <a name="create-a-linux-vm"></a>Tworzenie maszyny wirtualnej z systemem Linux
Możesz użyć portalu Azure lub Azure [PowerShell](#linux-powershell) do utworzenia Ubuntu lub SLES maszyny Wirtualnej. RHEL i maszyny wirtualnej CentOS istnieje inny przepływ pracy.  Zapoznaj się z poniższymi instrukcjami.

### <a name="linux-portal"></a>Portal
1. Zarejestruj przyspieszonego sieci dla systemu Linux w wersji zapoznawczej, wykonując kroki 1-5 [utworzyć Maszynę wirtualną systemu Linux - PowerShell](#linux-powershell) sekcji tego artykułu.  Nie można zarejestrować w portalu w wersji zapoznawczej.
2. Wykonać kroki 1 – 8 w [utworzyć Maszynę wirtualną systemu Windows — portal](#windows-portal) sekcji tego artykułu. W kroku 2 kliknij **Ubuntu Server 16.04 LTS** zamiast **systemu Windows Server Datacenter 2016**. W tym samouczku Wybierz hasło, a nie kluczem SSH, chociaż wdrożeń produkcyjnych, możesz użyć dowolnej. Jeśli **przyspieszony sieci** nie pojawia się po ukończeniu kroku 7 [utworzyć Maszynę wirtualną systemu Windows — portal](#windows-portal) sekcji tego artykułu, prawdopodobnie dla jednego z następujących powodów:
    - Użytkownik nie zostały jeszcze zarejestrowane dla wersji zapoznawczej. Upewnij się, że stan użytkownika rejestracji to **zarejestrowanej**, zgodnie z objaśnieniem w kroku 4 [utworzyć Maszynę wirtualną systemu Linux - Powershell](#linux-powershell) sekcji tego artykułu. **Uwaga:** Jeśli użytkownik brał udział w sieci akcelerowanego podglądu maszyn wirtualnych systemu Windows (nie jest już konieczne Zarejestruj, aby użyć akcelerowanego sieci dla systemu Windows maszyny wirtualne), nie są zostanie automatycznie zarejestrowany dla akcelerowanego sieci dla maszyn wirtualnych systemu Linux w wersji zapoznawczej. Należy zarejestrować sieciach akcelerowanego dla maszyn wirtualnych systemu Linux w wersji zapoznawczej do udziału w niej.
    - Nie wybrano rozmiar maszyny Wirtualnej, system operacyjny lub lokalizacji na liście [ograniczenia](#limitations) sekcji tego artykułu.
3. Aby zainstalować sterownik sieciowy przyspieszonego dla systemu Linux, wykonaj kroki [skonfigurować Linux](#configure-linux) sekcji tego artykułu.

### <a name="linux-powershell"></a>Środowiska PowerShell

>[!WARNING]
>Jeśli Tworzenie maszyn wirtualnych systemu Linux z przyspieszonego sieci w ramach subskrypcji, a następnie spróbuj utworzyć Maszynę wirtualną systemu Windows z przyspieszonego sieci w tej samej subskrypcji, tworzenie maszyny Wirtualnej systemu Windows może zakończyć się niepowodzeniem. Tej wersji zapoznawczej zaleca się testowanie systemu Linux i maszyn wirtualnych systemu Windows z przyspieszonego sieci w oddzielnych subskrypcji.
>

1. Zainstaluj najnowszą wersję programu Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modułu. Jeśli jesteś nowym użytkownikiem programu Azure PowerShell, przeczytaj [Omówienie programu Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) artykułu.
2. Uruchom sesję programu PowerShell, klikając przycisk Start systemu Windows, wpisując **powershell**, klikając **programu PowerShell** w wynikach wyszukiwania.
3. W oknie programu PowerShell, wprowadź `login-azurermaccount` polecenie, aby zalogować się przy użyciu platformy Azure [konta](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Jeśli nie masz już konto, możesz zarejestrować się w celu [bezpłatnej wersji próbnej](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Rejestr przyspieszonego sieci platformy Azure w wersji zapoznawczej, wykonując następujące czynności:
    - Wyślij wiadomość e-mail do [ axnpreview@microsoft.com ](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) identyfikator subskrypcji platformy Azure i zamierzonego użycia. Poczekaj, aż wiadomość e-mail z potwierdzeniem od firmy Microsoft dotyczące włączania subskrypcji.
    - Wprowadź następujące polecenie, aby potwierdzić, że są rejestrowane w wersji zapoznawczej:
    
        ```powershell
        Get-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingForLinux -ProviderNamespace Microsoft.Network
        ```

        Nie wykonuj kroku 5 do **zarejestrowanej** po wprowadzeniu poprzednie polecenie jest wyświetlana w danych wyjściowych. Dane wyjściowe musi wyglądać następujące dane wyjściowe przed kontynuowaniem:
    
        ```powershell
        FeatureName                        ProviderName      RegistrationState
        -----------                        ------------      -----------------
        AllowAcceleratedNetworkingForLinux Microsoft.Network Registered
        ```
        
      >[!NOTE]
      >Jeśli użytkownik brał udział w sieci akcelerowanego podglądu maszyn wirtualnych systemu Windows (nie jest już konieczne Zarejestruj, aby użyć akcelerowanego sieci dla maszyn wirtualnych systemu Windows), możesz nie zostały automatycznie zarejestrowane akcelerowanego sieci dla maszyn wirtualnych systemu Linux w wersji preview. Należy zarejestrować sieciach akcelerowanego dla maszyn wirtualnych systemu Linux w wersji zapoznawczej do udziału w niej.
      >
5. W przeglądarce Skopiuj poniższy skrypt, zastępując Ubuntu lub SLES zgodnie z potrzebami.  Ponownie Redhat i CentOS ma inny przepływ pracy przedstawione poniżej:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Create a new Storage account and define the new VM’s OSDisk name and its URI
    # Must end with ".vhd" extension
    $OSDiskName = "MyOsDiskName.vhd"
    # Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.
    $OSDiskSAName = "thestorageaccountname"  
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $RgName -Name $OSDiskSAName -Type "Standard_GRS" -Location $Location
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName
 
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Linux `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName Canonical `
      -Offer UbuntuServer `
      -Skus 16.04-LTS `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk -Name $OSDiskName `
      -VhdUri $OSDiskUri `
      -CreateOption FromImage 

    # Create the virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    ```
    
6. W oknie programu PowerShell kliknij prawym przyciskiem myszy w celu Wklej skrypt i uruchom jej wykonanie. Zostanie wyświetlony monit o nazwę użytkownika i hasło. Te poświadczenia umożliwiają Zaloguj się do maszyny Wirtualnej, gdy nawiązuje z nim połączenie w następnym kroku. Jeśli skrypt zakończy się niepowodzeniem, upewnij się, że:
    - Są zarejestrowane dla wersji zapoznawczej, zgodnie z objaśnieniem w kroku 4
    - Jeśli zmieniono rozmiar, typ systemu operacyjnego lub wartości lokalizacji w skrypcie maszyny Wirtualnej przed wykonaniem go, że wartości są wymienione w [ograniczenia](#Limitations) sekcji tego artykułu.
7. Aby zainstalować sterownik sieciowy przyspieszonego dla systemu Linux, wykonaj kroki [skonfigurować Linux](#configure-linux) sekcji tego artykułu.

### <a name="configure-linux"></a>Konfigurowanie systemu Linux

Po utworzeniu maszyny Wirtualnej na platformie Azure, należy zainstalować sterownik sieciowy przyspieszonego dla systemu Linux. Przed wykonaniem poniższych kroków, należy utworzyć Maszynę wirtualną systemu Linux z przyspieszonego w sieci za pomocą [portal](#linux-portal) lub [PowerShell](#linux-powershell) kroków w tym artykule. 

1. Za pomocą przeglądarki internetowej, otwórz Azure [portal](https://portal.azure.com) i zaloguj się przy użyciu platformy Azure [konta](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Jeśli nie masz już konto, możesz zarejestrować się w celu [bezpłatnej wersji próbnej](https://azure.microsoft.com/offers/ms-azr-0044p).
2. W górnej części portalu, z prawej strony *wyszukiwania zasobów* kliknij przycisk **> _** ikonę, aby uruchomić powłoki Bash chmury (wersja zapoznawcza). Przedstawia informacje o okienku powłoki Bash chmury pojawia się w dolnej części portalu i po kilku sekundach  **username@Azure:~ $** wiersza. Chociaż można SSH z maszyną wirtualną z komputera, a nie z powłoki chmury instrukcje w tym samouczku założono, że używasz powłoki chmury.
3. W górnej części portalu, w polu zawierający tekst *wyszukiwania zasobów*, typ *MyVm*. Gdy **MyVm** pojawia się w wynikach wyszukiwania kliknij ją.
4. W **MyVm** bloku, który jest wyświetlany, kliknij przycisk **Connect** przycisk w lewym górnym rogu bloku. **Uwaga:** Jeśli **tworzenie** jest widoczna w obszarze **Connect** przycisku Azure nie została jeszcze ukończona tworzenia maszyny Wirtualnej. Kliknij przycisk **Connect** tylko wtedy, gdy nie są już wyświetlane **tworzenie** w obszarze **Connect** przycisku.
5. Azure zostanie otwarte okno informujący o wprowadzenie `ssh adminuser@<ipaddress>`. Wprowadź polecenie w chmurze powłoki (lub kopiowania go z pola, które znajdowały się w kroku 4 i wklej go w chmurze powłoki), naciśnij klawisz Enter.
6. Wprowadź **tak** pytanie pytania, możesz Jeśli chcesz kontynuować połączenie, naciśnij klawisz Enter.
7. Wprowadź hasło, które zostały wprowadzone podczas tworzenia maszyny Wirtualnej. Po pomyślnym zalogowaniu do maszyny Wirtualnej, zostanie wyświetlony adminuser@MyVm:~ $ wiersza. Teraz logujesz się do maszyny Wirtualnej za pośrednictwem chmury sesję powłoki. **Uwaga:** limit czasu sesji powłoki w chmurze po 10 minutach braku aktywności.

W tym momencie instrukcje różnić w zależności od używanego dystrybucji. 

#### <a name="ubuntusles"></a>Ubuntu/SLES

1. W wierszu polecenia wprowadź `uname -r` i upewnij się, wersja:

    * Ubuntu jest "4.4.0-77-generic," lub nowszej
    * SLES jest "4.4.59-92.20-default" lub nowszej

2. Utwórz rentowność między standardowe vNIC sieci i szybszej sieci vNIC przez uruchomienie poleceń, które należy wykonać. Ruch sieciowy używa wyższej wydajności przyspieszonego sieci wirtualnej karty sieciowej, gdy ustalenie zapewnia ruchu sieciowego nie zostało przerwane przez niektóre zmiany konfiguracji.
          
     ```bash
     wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/configure_hv_sriov.sh
     chmod +x ./configure_hv_sriov.sh
     sudo ./configure_hv_sriov.sh
     ```
3. Po uruchomieniu skryptu, maszyna wirtualna zostanie ponownie uruchomiona po wstrzymać 60 sekund.
4. Po ponownym uruchomieniu maszyny Wirtualnej należy połączyć się z nim, wykonując kroki 5-7.
5. Uruchom `ifconfig` polecenie i Potwierdź, że została znaleziona bond0 i interfejs jest wyświetlany jako zapasowe. 
 
 >[!NOTE]
      >Aplikacje przy użyciu przyspieszonego sieci muszą komunikować się za pośrednictwem *bond0* interfejs nie *eth0*.  Nazwa interfejsu może się zmienić przed przyspieszonego sieci osiągnie ogólnej dostępności.

#### <a name="rhelcentos"></a>RHEL/CentOS

Tworzenie Red Hat Enterprise Linux lub wirtualna 7.3 CentOS wymaga kilka dodatkowych czynności, aby załadować sterowników wymaganych dla funkcji SR-IOV i sterownik funkcji wirtualnej funkcji (Wirtualnej) dla karty sieciowej. Pierwszą fazę instrukcji przygotowuje obraz, który można utworzyć jeden lub więcej maszyn wirtualnych, które są wstępnie załadowane sterowniki.

##### <a name="phase-one-prepare-a-red-hat-enterprise-linux-or-centos-73-base-image"></a>Pierwszej fazy: przygotowanie Red Hat Enterprise Linux lub CentOS 7.3 obrazu podstawowego. 

1.  Udostępnianie nie - funkcji SR-IOV CentOS 7.3 Maszynę wirtualną na platformie Azure

2.  Instalowania usług LIS 4.2.2:
    
    ```bash
    wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
    tar -xvf lis-rpms-4.2.2-2.tar.gz
    cd LISISO && sudo ./install.sh
    ```

3.  Pobierz pliki konfiguracji
    
    ```bash
    cd /etc/udev/rules.d/  
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/60-hyperv-vf-name.rules 
    cd /usr/sbin/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/hv_vf_name 
    sudo chmod +x hv_vf_name
    cd /etc/sysconfig/network-scripts/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/ifcfg-vf1   
    ```

4.  Anulowanie zastrzeżenia tej maszyny Wirtualnej

    ```bash
    sudo waagent -deprovision+user 
    ```

5.  Z portalu Azure Zatrzymaj tę maszynę Wirtualną; i przejdź do maszyny Wirtualnej "dyskami", identyfikator URI dysku VHD OSDisk przechwytywania. Ten identyfikator URI zawiera nazwę dysku VHD obrazu podstawowego i jego konta magazynu. 
 
##### <a name="phase-two-provision-new-vms-on-azure"></a>Dwie fazy: obsługi administracyjnej nowych maszyn wirtualnych na platformie Azure

1.  Zainicjuj obsługę nowych maszyn wirtualnych na podstawie New-AzureRMVMConfig przy użyciu obrazu podstawowego przechwycone w fazie jedną, z AcceleratedNetworking włączona na karcie vNIC wirtualnego dysku twardego:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
     -Name $RgName `
     -Location $Location

    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
     -Name MySubnet `
     -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
     -ResourceGroupName $RgName `
     -Location $Location `
     -Name MyVnet `
     -AddressPrefix 10.0.0.0/16 `
     -Subnet $Subnet
    
    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
     -Name MyPublicIp `
     -ResourceGroupName $RgName `
     -Location $Location `
     -AllocationMethod Static
    
    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
     -Name MyNic `
     -ResourceGroupName $RgName `
     -Location $Location `
     -SubnetId $Vnet.Subnets[0].Id `
     -PublicIpAddressId $Pip.Id `
     -EnableAcceleratedNetworking
    
    # Specify the base image's VHD URI (from phase one step 5). 
    # Note: The storage account of this base image vhd should have "Storage service encryption" disabled
    # See more from here: https://docs.microsoft.com/azure/storage/storage-service-encryption
    # This is just an example URI, you will need to replace this when running this script
    $sourceUri="https://myexamplesa.blob.core.windows.net/vhds/CentOS73-Base-Test120170629111341.vhd" 

    # Specify a URI for the location from which the new image binary large object (BLOB) is copied to start the virtual machine. 
    # Must end with ".vhd" extension
    $OsDiskName = "MyOsDiskName.vhd" 
    $destOsDiskUri = "https://myexamplesa.blob.core.windows.net/vhds/" + $OsDiskName
    
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential
    
    # Create a custom virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
     -VMName MyVM -VMSize Standard_DS4_v2 | `
    Set-AzureRmVMOperatingSystem `
     -Linux `
     -ComputerName myVM `
     -Credential $Cred | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk `
     -Name $OsDiskName `
     -SourceImageUri $sourceUri `
     -VhdUri $destOsDiskUri `
     -CreateOption FromImage `
     -Linux
    
    # Create the virtual machine.    
    New-AzureRmVM `
     -ResourceGroupName $RgName `
     -Location $Location `
     -VM $VmConfig
    ```

2.  Po rozruchu maszyn wirtualnych, urządzenia funkcji Wirtualnej przez "lspci" i Sprawdź wpis Mellanox. Na przykład możemy powinien zostać wyświetlony ten element w danych wyjściowych lspci:
    
    ```
    0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
    ```
    
3.  Uruchom skrypt łączenia przez:

    ```bash
    sudo bondvf.sh
    ```

4.  Ponowny rozruch nowych maszyn wirtualnych:

    ```bash
    sudo reboot
    ```

Maszyna wirtualna powinna zaczynać się znakiem bond0 skonfigurowany i ścieżkę przyspieszony sieci włączone.  Uruchom `ifconfig` o potwierdzenie.
