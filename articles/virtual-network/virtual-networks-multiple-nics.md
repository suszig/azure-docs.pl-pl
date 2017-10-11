---
title: "Tworzenie maszyny Wirtualnej (klasyczne) z wieloma kartami sieciowymi przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć i skonfigurować maszyny wirtualne z wieloma kartami sieciowymi przy użyciu programu PowerShell."
services: virtual-network, virtual-machines
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a1a3952c-2dcc-4977-bd7a-52d623c1fb07
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 68ccc1cac22e593b099729fe68c6bee63df44d9b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-classic-with-multiple-nics"></a>Tworzenie maszyny Wirtualnej (klasyczne) z wieloma kartami sieciowymi
Można tworzyć maszyn wirtualnych (VM) na platformie Azure i dołączyć wiele interfejsów sieciowych (NIC) do wszystkich maszyn wirtualnych. Wiele kart sieciowych są wymagane w przypadku wielu sieci wirtualnych urządzeń, takich jak dostarczania aplikacji i rozwiązań Optymalizacja sieci WAN. Wiele kart sieciowych zapewniają również izolacji ruchu między kart sieciowych.

![Obsługa wielu kart interfejsu Sieciowego dla maszyny Wirtualnej](./media/virtual-networks-multiple-nics/IC757773.png)

Na rysunku przedstawiono Maszynę wirtualną z trzema kartami sieciowymi, każdy jest połączony z innej podsieci.

> [!IMPORTANT]
> Platforma Azure oferuje dwa różne modele wdrażania związane z tworzeniem zasobów i pracą z nimi: [model wdrażania przy użyciu usługi Azure Resource Manager i model klasyczny](../resource-manager-deployment-model.md). Ten artykuł dotyczy klasycznego modelu wdrożenia. Firma Microsoft zaleca, aby większości nowych wdrożeń korzystać Menedżera zasobów.

* VIP połączonych z Internetem (wdrożenia klasyczne) jest obsługiwane tylko dla karty sieciowej "domyślny". Istnieje tylko jeden adresów VIP na adres IP domyślnej karty sieciowej.
* W tym czasie wystąpienia poziomu publicznego adresu IP (LPIP) adresy (wdrożenia klasyczne) nie są obsługiwane dla wielu maszyn wirtualnych kart Sieciowych.
* Kolejność kart sieciowych wewnątrz maszyny wirtualnej będzie losowa i może również ulec zmianie między aktualizacjami infrastruktury platformy Azure. Jednak adresów IP i odpowiednie ethernet MAC adresy będą pozostają takie same. Załóżmy na przykład **Eth1** ma adres IP 10.1.0.100 i adres MAC 00-0D-3A-B0-39-0D; po aktualizacji infrastruktury platformy Azure i ponowne uruchomienie komputera, można zmienić na **Eth2**, ale adresów IP i MAC parowanie jest taka sama. Jeśli ponowne uruchomienie jest inicjowane przez klienta, kolejność kart interfejsu Sieciowego jest taka sama.
* Adres dla każdej karty Sieciowej na każdej maszynie Wirtualnej musi znajdować się w podsieci, wiele kart sieciowych na jednej maszynie Wirtualnej każdego można przypisać adresy znajdujące się w tej samej podsieci.
* Rozmiar maszyny Wirtualnej określa liczbę kart Sieciowych, który można utworzyć maszyny wirtualnej. Odwołanie [systemu Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) i [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artykuły, aby określić, jak wiele kart Sieciowych każdego rozmiaru maszyny Wirtualnej obsługuje rozmiary maszyny Wirtualnej. 

## <a name="network-security-groups-nsgs"></a>Grupy zabezpieczeń sieci (NSG)
W ramach wdrożenia usługi Resource Manager dowolnej karty interfejsu Sieciowego na maszynie Wirtualnej może być skojarzony z sieciowej grupy zabezpieczeń (NSG), w tym wszystkie karty sieciowe na maszynie Wirtualnej, który ma wiele kart sieciowych włączone. Jeśli karta sieciowa zostanie przypisany adres znajdujący się w podsieci, w którym jest skojarzona z grupy NSG podsieci, następnie zasady w tej podsieci NSG również zastosowanie do tej karty sieciowej. Oprócz kojarzenia podsieci z grup NSG, można również skojarzyć karty Sieciowej z grupy NSG.

Jeśli podsieć jest skojarzony z grupy NSG, a karta sieciowa w tej podsieci indywidualnie powiązanego z grupy NSG, skojarzone reguły NSG są stosowane w **przepływ kolejności** zgodnie z kierunek ruchu przekazywany do lub z karty Sieciowej:

* **Ruch przychodzący** których obiektem docelowym jest karta sieciowa w pytanie najpierw przechodzi przez podsieci, wyzwolenie reguły NSG podsieci, przed przekazaniem do karty Sieciowej, a następnie wyzwolenie reguły NSG karty Sieciowej.
* **Ruch wychodzący** źródłem jest zagrożona kart przepływa pierwszy na wyjściu z karty Sieciowej, wyzwolenie reguły NSG karty Sieciowej, przed przechodzącej przez podsieci, a następnie wyzwolenie reguły NSG podsieci.

Dowiedz się więcej o [grup zabezpieczeń sieci](virtual-networks-nsg.md) i sposób stosowania oparte na skojarzenia z podsieci, maszyn wirtualnych i karty sieciowe.

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a>Konfigurowanie wielu kart Sieciowych maszyny Wirtualnej w ramach wdrożenia klasycznego
Poniższe instrukcje umożliwia tworzenie wielu kart Sieciowych maszyny Wirtualnej zawierający 3 kart sieciowych: domyślna karta sieciowa a dwie dodatkowe karty sieciowe. Kroki konfiguracji spowoduje utworzenie maszyny Wirtualnej, który zostanie skonfigurowany zgodnie z poniższym fragment pliku konfiguracji usługi:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over the remainder section …
    </VirtualNetworkSite>


Przed próbą uruchomienia poleceń środowiska PowerShell w tym przykładzie należy się następujące wymagania wstępne.

* Subskrypcja platformy Azure.
* Skonfigurowanej sieci wirtualnej. Zobacz [omówienie sieci wirtualnych](virtual-networks-overview.md) uzyskać więcej informacji o sieci wirtualnych.
* Najnowszą wersję programu Azure PowerShell pobrane i zainstalowane. Zobacz artykuł [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).

Aby utworzyć Maszynę wirtualną z wieloma kartami sieciowymi, wykonaj następujące kroki, wprowadzając każde polecenie w ramach jednej sesji programu PowerShell:

1. Wybierz obraz maszyny Wirtualnej z galerii obrazu maszyny Wirtualnej platformy Azure. Należy pamiętać, że obrazy często zmieniana i są dostępne w poszczególnych regionach. Obraz określony w poniższym przykładzie może zmienić lub może nie może być w danym regionie, dlatego należy określić obraz, które są potrzebne.

    ```powershell
    $image = Get-AzureVMImage `
    -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"
    ```

2. Tworzenie konfiguracji maszyny Wirtualnej.

    ```powershell
    $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
    -Image $image.ImageName –AvailabilitySetName "MyAVSet"
    ```

3. Utwórz identyfikator logowania administratora domyślnego.

    ```powershell
    Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
    -Password "<YourAdminPassword>"
    ```

4. Dodawanie dodatkowych kart sieciowych do konfiguracji maszyny Wirtualnej.

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
    -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
    Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
    -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm
    ```

5. Podaj podsieć lub adres IP dla domyślnej karty sieciowej.

    ```powershell
    Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
    Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm
    ```

6. Utwórz maszynę Wirtualną w sieci wirtualnej.

    ```powershell
    New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm
    ```

    > [!NOTE]
    > Sieć wirtualna określone w tym miejscu musi już istnieć (określone w wymaganiach wstępnych). W poniższym przykładzie określa sieć wirtualną o nazwie **MultiNIC-VNet**.
    >

## <a name="limitations"></a>Ograniczenia
Następujące ograniczenia mają zastosowanie w przypadku korzystania z wielu kart sieciowych:

* Maszyny wirtualne z wieloma kartami sieciowymi muszą być tworzone w sieci wirtualnej platformy Azure (sieci wirtualne). Nie można skonfigurować sieci wirtualnej nie maszyn wirtualnych z wieloma kartami sieciowymi.
* Wszystkie maszyny wirtualne w zestawie dostępności muszą używać wielu kart sieciowych lub jednej karty sieciowej. Nie może być wiele maszyn wirtualnych kart Sieciowych i jednej karty Sieciowej maszyn wirtualnych w zestawie dostępności. Te same zasady mają zastosowanie dla maszyn wirtualnych w usłudze w chmurze. Dla wielu kart interfejsu Sieciowego maszyn wirtualnych ich nie muszą mieć taką samą liczbę kart sieciowych, tak długo, jak długo każdy z nich ma co najmniej dwóch.
* Nie można skonfigurować maszynę Wirtualną z jednej karty Sieciowej z wielu kart sieciowych (i na odwrót) po wdrożeniu, bez usunięcia i ponownego utworzenia go.

## <a name="secondary-nics-access-to-other-subnets"></a>Pomocniczy dostęp kart sieciowych do innych podsieci
Domyślnie dodatkowej karty sieciowe nie zostanie skonfigurowany dla bramy domyślnej z powodu którego będzie ograniczony przepływ ruchu na dodatkowej karty sieciowe muszą mieścić się w tej samej podsieci. Jeśli użytkownicy chcesz włączyć dodatkowej kart sieciowych do komunikowania się poza własnej podsieci, muszą dodać wpis w tabeli routingu, aby skonfigurować bramę, zgodnie z poniższym opisem.

> [!NOTE]
> Maszyny wirtualne utworzone przed lipca 2015 może mieć brama domyślna jest skonfigurowana dla wszystkich kart sieciowych. Brama domyślna dla dodatkowej kart sieciowych nie zostaną usunięte, dopóki te maszyny wirtualne są ponownie uruchamiane. W systemach operacyjnych, w których jest używany model routingu słabe hosta, takich jak Linux Jeśli ruch przychodzące i wychodzące używane różne karty sieciowe mogą być dzielone łączności z Internetem.
> 

### <a name="configure-windows-vms"></a>Konfigurowanie maszyn wirtualnych systemu Windows
Załóżmy, że ma maszyny Wirtualnej systemu Windows z dwiema kartami sieciowymi w następujący sposób:

* Podstawowy adres IP karty Sieciowej: 192.168.1.4
* Pomocniczego adresu IP karty Sieciowej: 192.168.2.5

Tabeli tras IPv4 dla tej maszyny Wirtualnej będzie wyglądać następująco:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Zwróć uwagę, że trasa domyślna (0.0.0.0) jest dostępna tylko dla podstawowego karty sieciowej. Nie można uzyskać dostęp do zasobów spoza podsieci dodatkowej karty sieciowej, jak pokazano poniżej:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

Aby dodać trasy domyślnej na pomocniczej karcie interfejsu Sieciowego, wykonaj następujące czynności:

1. W wierszu polecenia uruchom poniższe polecenie, aby zidentyfikować numer indeksu dodatkowej karty sieciowej:
   
        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================
2. Zwróć uwagę, drugi wpis w tabeli z indeksem 27 (w tym przykładzie).
3. W wierszu polecenia Uruchom **dodać trasy** polecenia, jak pokazano poniżej. W tym przykładzie 192.168.2.1 jest określany jako brama domyślna dodatkowej karty sieciowej:
   
        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27
4. Aby przetestować połączenie, przejdź wstecz do wiersza polecenia, a następnie spróbuj wysyłania poleceń ping do różnych podsieci z pomocniczego karty Sieciowej jako pokazano int eh w poniższym przykładzie:
   
        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5
   
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
5. Możesz również sprawdzić tabeli routingu do sprawdzenia nowo dodanego trasy, jak pokazano poniżej:
   
        C:\Users\Administrator>route print
   
        ...
   
        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Konfigurowanie maszyn wirtualnych systemu Linux
Dla maszyn wirtualnych systemu Linux ponieważ domyślnie korzysta z hosta słabe routingu, zaleca się czy dodatkowej karty sieciowe są ograniczone do przepływów ruchu sieciowego tylko w obrębie tej samej podsieci. Jednak w przypadku niektórych scenariuszy wymaga łączności spoza podsieci, użytkowników należy włączyć na podstawie zasad routingu, aby upewnić się, że ruch przychodzące i wychodzące używa tej samej karty sieciowej.

## <a name="next-steps"></a>Następne kroki
* Wdrażanie [MultiNIC maszyn wirtualnych, w przypadku aplikacji warstwy 2 w ramach wdrożenia usługi Resource Manager](virtual-network-deploy-multinic-arm-template.md).
* Wdrażanie [MultiNIC maszyn wirtualnych, w przypadku aplikacji warstwy 2 w ramach wdrożenia klasycznego](virtual-network-deploy-multinic-classic-ps.md).

