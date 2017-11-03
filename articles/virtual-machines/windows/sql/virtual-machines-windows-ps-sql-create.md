---
title: Tworzenie maszyny wirtualnej programu SQL Server w programie Azure PowerShell (Resource Manager) | Dokumentacja firmy Microsoft
description: "Zawiera kroki i skryptów programu PowerShell do tworzenia maszyny Wirtualnej platformy Azure z obrazów Galeria maszyny wirtualnej programu SQL Server."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/29/2017
ms.author: jroth
ms.openlocfilehash: 33c306258b6be40f2c5cbc016e3c84e36bf61e0d
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Aprowizowanie maszyny wirtualnej programu SQL Server przy użyciu programu Azure PowerShell (Resource Manager)
> [!div class="op_single_selector"]
> * [Portal](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
>
>

## <a name="overview"></a>Omówienie
Ten samouczek pokazuje, jak utworzyć jednej maszyny wirtualnej platformy Azure za pomocą funkcji **usługi Azure Resource Manager** modelu wdrażania przy użyciu poleceń cmdlet programu Azure PowerShell. W tym samouczku utworzymy jednej maszyny wirtualnej za pomocą pojedynczego dysku z obrazu w galerii programu SQL. Zostanie utworzony nowy dostawców dla magazynu, sieci i zasoby obliczeniowe, które będą używane przez maszynę wirtualną. Jeśli masz istniejące dostawców dla każdego z tych zasobów, można użyć tych dostawców.

Jeśli potrzebujesz klasycznego wersją tego tematu, zobacz [Aprowizowanie maszyny wirtualnej programu SQL Server przy użyciu usługi Azure PowerShell klasycznego](../classic/ps-sql-create.md).

## <a name="prerequisites"></a>Wymagania wstępne
W tym samouczku potrzebne są:

* Konto platformy Azure i subskrypcji przed jego uruchomieniem. Jeśli nie masz, zaloguj się do [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
* [Program Azure PowerShell)](/powershell/azure/overview), minimalna wersja 1.4.0 lub nowszy (w tym samouczku napisane przy użyciu wersji 1.5.0).
  * Aby pobrać swoją wersję, wpisz **flagą Azure Get-Module - ListAvailable**.

## <a name="configure-your-subscription"></a>Skonfiguruj subskrypcję
Otwórz program Windows PowerShell i ustanowić dostępu do konta platformy Azure, uruchamiając następujące polecenie cmdlet. Zostaną wyświetlone ze znakiem na ekranie, aby wprowadzić swoje poświadczenia. Użyj tego samego adres e-mail i hasło, którego używasz do logowania do portalu Azure.

```PowerShell
Add-AzureRmAccount
```

Po pomyślnym zalogowaniu niektóre informacje są wyświetlane na ekranie, która zawiera nazwę subskrypcji i identyfikator subskrypcji domyślne. Jest to subskrypcji, w którym zostaną utworzone zasoby w tym samouczku, chyba że zostanie zmienione do innej subskrypcji. Jeśli masz wiele subskrypcji, uruchom następujące polecenie cmdlet, aby powrócić do listy wszystkich subskrypcji:

```PowerShell
Get-AzureRmSubscription
```
Aby zmienić do innej subskrypcji, uruchom następujące polecenie z docelowym Nazwa subskrypcji.

```PowerShell
Select-AzureRmSubscription -SubscriptionName YourTargetSubscriptionName
```

## <a name="define-image-variables"></a>Zdefiniuj zmienne obrazu
Aby uprościć użyteczność i zrozumienia ukończone skryptu z tego samouczka, Rozpoczniemy, definiując liczba zmiennych. Zmiany wartości parametrów, ale Uważaj nazewnictwa ograniczenia związane z długości nazwy i znaków specjalnych, modyfikując podanych wartości.

### <a name="location-and-resource-group"></a>Lokalizacji i grupy zasobów
Dwie zmienne umożliwiają definiowanie obszaru danych i grupy zasobów, do której zostanie utworzona innych zasobów dla maszyny wirtualnej.

Zmodyfikuj stosownie do potrzeb, a następnie wykonaj następujące polecenia cmdlet można zainicjować zmienne.

```PowerShell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm1"
```

### <a name="storage-properties"></a>Magazyn właściwości
Użyj poniższych zmiennych, aby określić konto magazynu i typu magazynu do użycia przez maszynę wirtualną.

Zmodyfikuj zgodnie z potrzebami, a następnie wykonaj następujące polecenie cmdlet, aby zainicjować zmienne. Należy pamiętać, że w tym przykładzie używamy [magazyn w warstwie Premium](../premium-storage.md), który jest zalecane w przypadku obciążeń produkcyjnych. Aby uzyskać więcej informacji na ten wskazówki i inne zalecenia, zobacz [wydajności najlepsze rozwiązania dotyczące programu SQL Server w usłudze Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

```PowerShell
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"
```

### <a name="network-properties"></a>Właściwości sieci
Użyj poniższych zmiennych do definiowania interfejsu sieciowego, Metoda alokacji TCP/IP, nazwa sieci wirtualnej, nazwę podsieci wirtualnej, zakres adresów IP dla sieci wirtualnej, zakres adresów IP podsieci i etykieta nazwy domeny publicznego do użytku przez sieć maszyny wirtualnej.

Zmodyfikuj zgodnie z potrzebami, a następnie wykonaj następujące polecenie cmdlet, aby zainicjować zmienne.

```PowerShell
$InterfaceName = $ResourceGroupName + "ServerInterface"
$TCPIPAllocationMethod = "Dynamic"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$DomainName = "sqlvm1"
```

### <a name="virtual-machine-properties"></a>Właściwości maszyny wirtualnej
Użyj poniższych zmiennych, aby zdefiniować nazwę maszyny wirtualnej, nazwę komputera, rozmiar maszyny wirtualnej i nazwa dysku systemu operacyjnego dla maszyny wirtualnej.

Zmodyfikuj zgodnie z potrzebami, a następnie wykonaj następujące polecenie cmdlet, aby zainicjować zmienne.

```PowerShell
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"
```

### <a name="image-properties"></a>Właściwości obrazu
Użyj poniższych zmiennych do definiowania obrazu do użycia dla maszyny wirtualnej. W tym przykładzie jest używany obraz wersji programu SQL Server 2016 Developer. Developer edition za darmo jest licencjonowany do badania i rozwój i ponoszenie kosztów uruchamiania maszyny Wirtualnej.

Zmodyfikuj zgodnie z potrzebami, a następnie wykonaj następujące polecenie cmdlet, aby zainicjować zmienne.

```PowerShell
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2016-WS2016"
$Sku = "SQLDEV"
$Version = "latest"
```

Należy pamiętać, że można uzyskać pełną listę ofert obrazu programu SQL Server za pomocą polecenia Get-AzureRmVMImageOffer:

```PowerShell
Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'
```

Aby zobaczyć dostępne oferty, za pomocą polecenia Get-AzureRmVMImageSku SKU. Polecenie zawiera wszystkie dostępne dla jednostki SKU **SQL2016SP1 WS2016** oferty.

```PowerShell
Get-AzureRmVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer 'SQL2016SP1-WS2016' | Select Skus
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Z modelu wdrażania usługi Resource Manager pierwszy obiekt tworzona jest grupa zasobów. Używamy [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) , aby utworzyć grupy zasobów platformy Azure i jej zasobach z nazwy grupy zasobów i lokalizacji zdefiniowanej przez zmiennych, które zostały wcześniej zainicjowane.

Wykonaj następujące polecenie cmdlet można utworzyć nowej grupy zasobów.

```PowerShell
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>Tworzenie konta magazynu
Maszyna wirtualna wymaga zasobów magazynu dla dysku systemu operacyjnego i plików danych i dziennika programu SQL Server. Dla uproszczenia utworzymy jednego dysku dla obu. Możesz dołączyć dodatkowe dyski później za pomocą [dysku Azure Dodaj](/powershell/module/azure/add-azuredisk) polecenia cmdlet, aby umieścić danych programu SQL Server i dziennik pliki na dedykowanych dysków. Używamy [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) , aby utworzyć magazynu standardowego konta w nowej grupy zasobów oraz nazwy konta magazynu, nazwa jednostki Sku magazynu i lokalizacja zdefiniowane przy użyciu zmiennych wcześniej zainicjowane.

Wykonaj następujące polecenie cmdlet, aby utworzyć nowe konto magazynu.

```PowerShell
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location
```

## <a name="create-network-resources"></a>Utwórz zasoby sieciowe
Maszyna wirtualna wymaga liczba zasobów sieciowych do obsługi łączności sieciowej.

* Każda maszyna wirtualna wymaga sieci wirtualnej.
* Sieć wirtualna musi mieć co najmniej jedną podsieć zdefiniowane.
* Interfejs sieciowy musi być zdefiniowany publiczny lub prywatny adres IP.

### <a name="create-a-virtual-network-subnet-configuration"></a>Tworzenie konfiguracji podsieci sieci wirtualnej
Firma Microsoft będzie Rozpocznij od utworzenia konfiguracji podsieci dla naszych sieci wirtualnej. W naszym samouczku utworzymy podsieci domyślne przy użyciu [AzureRmVirtualNetworkSubnetConfig nowy](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) polecenia cmdlet. Utworzymy naszych konfiguracji podsieci sieci wirtualnej z prefiksem nazwy i adresu podsieci zdefiniowane przy użyciu zmiennych, które zostały wcześniej zainicjowane.

> [!NOTE]
> Można zdefiniować dodatkowe właściwości konfiguracji podsieci sieci wirtualnej za pomocą tego polecenia cmdlet, ale która wykracza poza zakres tego samouczka.

Wykonaj następujące polecenie cmdlet, aby utworzyć konfigurację podsieci wirtualnej.

```PowerShell
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej
Następnie zostanie utworzony przy użyciu naszego sieci wirtualnej [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) polecenia cmdlet. Nasze sieci wirtualnej zostanie utworzony w tej nowej grupy zasobów, z nazwy, lokalizacji i adres prefiks zdefiniowany za pomocą zmiennych, które zostały wcześniej zainicjowane, a za pomocą konfiguracji podsieci, który został zdefiniowany w poprzednim kroku.

Wykonaj następujące polecenie cmdlet, aby utworzyć sieć wirtualną.

```PowerShell
$VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>Utwórz publiczny adres IP
Teraz, gdy mamy naszych sieci wirtualnej zdefiniowane należy skonfigurować adres IP dla połączenia z maszyną wirtualną. W tym samouczku utworzymy publiczny adres IP za pomocą dynamicznego adresu IP, adresy obsługuje łączność z Internetem. Używamy [AzureRmPublicIpAddress nowy](/powershell/module/azurerm.network/new-azurermpublicipaddress) , aby utworzyć publiczny adres IP adres w prevously utworzoną grupę zasobów i nazwy, lokalizacji, Metoda alokacji i etykieta nazwy domeny DNS zdefiniowane przy użyciu zmiennych tego wcześniej zainicjowana.

> [!NOTE]
> Można zdefiniować dodatkowe właściwości publiczny adres IP za pomocą tego polecenia cmdlet, ale która wykracza poza zakres tego samouczka początkowej. Można również utworzyć za pomocą statycznego adresu prywatny adres lub adres, ale jest to poza zakres tego samouczka.

Wykonaj następujące polecenie cmdlet, aby utworzyć publicznego adresu IP.

```PowerShell
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-interface"></a>Tworzenie interfejsu sieciowego
Firma Microsoft są teraz gotowe do tworzenia interfejsu sieciowego, który będzie używany przez naszych maszyny wirtualnej. Używamy [AzureRmNetworkInterface nowy](/powershell/module/azurerm.network/new-azurermnetworkinterface) wcześniej zdefiniowane, aby utworzyć naszych interfejsu sieciowego w wcześniej utworzoną grupę zasobów i nazwy, lokalizacji, podsieci i publicznego adresu IP.

Wykonaj następujące polecenie cmdlet, aby utworzyć interfejsu sieciowego.

```PowerShell
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id
```

## <a name="configure-a-vm-object"></a>Konfigurowanie obiektu maszyny Wirtualnej
Teraz, gdy mamy magazynu i zasobów sieciowych zdefiniowanych możemy przystąpić do definiowania zasoby obliczeniowe dla maszyny wirtualnej. W naszym samouczku będziemy Określ rozmiar maszyny wirtualnej i różne właściwości systemu operacyjnego, określ interfejsu sieciowego, który utworzono wcześniej, zdefiniuj magazynu obiektów blob, a następnie określ dysk systemu operacyjnego.

### <a name="create-the-vm-object"></a>Tworzenie obiektu maszyny Wirtualnej
Firma Microsoft będzie uruchomić określania rozmiaru maszyny wirtualnej. W tym samouczku będziemy określania DS13. Używamy [AzureRmVMConfig nowy](/powershell/module/azurerm.compute/new-azurermvmconfig) , aby utworzyć obiekt można skonfigurować maszyny wirtualnej o nazwie i rozmiarze zdefiniowane przy użyciu zmiennych, które zostały wcześniej zainicjowane.

Wykonaj następujące polecenie cmdlet, aby utworzyć obiekt maszyny wirtualnej.

```PowerShell
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Utwórz obiekt poświadczeń, aby pomieścić nazwę i hasło dla poświadczeń administratora lokalnego
Zanim firma Microsoft można ustawić właściwości systemu operacyjnego dla maszyny wirtualnej, musimy Podaj poświadczenia dla konta administratora lokalnego jako bezpieczny ciąg. W tym celu użyjemy [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) polecenia cmdlet.

Wykonaj następujące polecenie cmdlet i, w oknie żądanie poświadczeń programu Windows PowerShell, wpisz nazwę i hasło dla konta administratora lokalnego na maszynie wirtualnej systemu Windows.

```PowerShell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Ustaw właściwości systemu operacyjnego dla maszyny wirtualnej
Teraz możemy przystąpić do ustawiania właściwości systemu operacyjnego maszyny wirtualnej. Używamy [AzureRmVMOperatingSystem zestaw](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) polecenia cmdlet, aby ustawić typ systemu operacyjnego jako systemu Windows, wymagają [agenta maszyny wirtualnej](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) do zainstalowania, określ, czy polecenie cmdlet umożliwia automatyczne aktualizacje i Ustaw nazwę maszyny wirtualnej, nazwę komputera i poświadczenia, korzystając ze zmiennych, które zostały wcześniej zainicjowane.

Wykonaj następujące polecenie cmdlet można ustawić właściwości systemu operacyjnego dla maszyny wirtualnej.

```PowerShell
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Dodaj interfejs sieciowy do maszyny wirtualnej
Następnie dodamy interfejsu sieciowego, który utworzono wcześniej do maszyny wirtualnej. Używamy [AzureRmVMNetworkInterface Dodaj](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) polecenia cmdlet, aby dodać interfejs sieci przy użyciu zdefiniowanego wcześniej zmiennej interfejs sieci.

Wykonaj następujące polecenie cmdlet, aby ustawić interfejsu sieciowego dla maszyny wirtualnej.

```PowerShell
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Ustaw lokalizację magazynu obiektów blob dysku używanego przez maszynę wirtualną
Następnie możemy ustawi lokalizacji magazynu obiektów blob dysku używanego przez maszynę wirtualną, korzystając ze zmiennych, które wcześniej zdefiniowane.

Wykonaj następujące polecenie cmdlet, aby ustawić lokalizacji magazynu obiektów blob.

```PowerShell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Ustaw właściwości dysku dla maszyny wirtualnej systemu operacyjnego
Następnie możemy ustawi systemu operacyjnego właściwości dysku dla maszyny wirtualnej. Używamy [AzureRmVMOSDisk zestaw](/powershell/module/azurerm.compute/set-azurermvmosdisk) polecenia cmdlet, aby określić, że system operacyjny dla maszyny wirtualnej będzie pochodzić z obrazu, aby ustawić buforowanie, aby odczytać tylko (ponieważ program SQL Server jest instalowany na tym samym dysku) i definiowanie wirtualnego Nazwa komputera i zdefiniowane przy użyciu zmiennych, które wcześniej zdefiniowanego dysku systemu operacyjnego.

Wykonaj następujące polecenie cmdlet można ustawić właściwości dysku dla maszyny wirtualnej systemu operacyjnego.

```PowerShell
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Określ obraz platformy dla maszyny wirtualnej
Nasze ostatni krok konfiguracji jest Określ obrazu platformy naszych maszyny wirtualnej. W naszym samouczku używamy najnowsze obrazu programu SQL Server 2016 CTP. Używamy [AzureRmVMSourceImage zestaw](/powershell/module/azurerm.compute/set-azurermvmsourceimage) polecenia cmdlet, aby użyć tego obrazu, zgodnie z definicją w zmiennych, które wcześniej zdefiniowany.

Wykonaj następujące polecenie cmdlet, aby określić obrazu platformy dla maszyny wirtualnej.

```PowerShell
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>Tworzenie maszyny Wirtualnej SQL
Po zakończeniu kroków konfiguracji, można przystąpić do tworzenia maszyny wirtualnej. Używamy [AzureRmVM nowy](/powershell/module/azurerm.compute/new-azurermvm) polecenia cmdlet, aby utworzyć maszynę wirtualną, korzystając ze zmiennych, które możemy zdefiniowane.

Wykonaj następujące polecenie cmdlet, aby utworzyć maszynę wirtualną.

```PowerShell
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

Tworzenie maszyny wirtualnej. Należy zauważyć, że dla diagnostyki rozruchu utworzeniu konta magazynu w warstwie standardowa ponieważ podanego konta magazynu dla dysku maszyny wirtualnej to konto magazynu premium.

Możesz teraz przeglądać tej maszyny w portalu Azure, aby wyświetlić [publicznego adresu IP i nazwy FQDN](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="example-script"></a>Przykładowy skrypt
Poniższy skrypt zawiera pełną skrypt programu PowerShell na potrzeby tego samouczka. Przyjęto założenie, że masz już ustawienia subskrypcji platformy Azure do użycia z **Add-AzureRmAccount** i **Select-AzureRmSubscription** poleceń.

```PowerShell
# Variables

## Global
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm1"

## Storage
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"

## Network
$InterfaceName = $ResourceGroupName + "ServerInterface"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$TCPIPAllocationMethod = "Dynamic"
$DomainName = "sqlvm1"

##Compute
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"

##Image
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2016-WS2016"
$Sku = "Enterprise"
$Version = "latest"

# Resource Group
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Storage
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

# Network
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
$VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

# Compute
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

# Image
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## Create the VM in Azure
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

## <a name="next-steps"></a>Następne kroki
Po utworzeniu maszyny wirtualnej, można przystąpić do połączenia z maszyną wirtualną za pomocą protokołu RDP i ustawienia łączności. Aby uzyskać więcej informacji, zobacz [Connect do programu SQL Server maszynę wirtualną na platformie Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).
