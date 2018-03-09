---
title: "Tworzenie zarządzanego maszyny Wirtualnej platformy Azure z wirtualnego dysku twardego uogólnionego lokalnymi | Dokumentacja firmy Microsoft"
description: "Przekazać uogólniony wirtualny dysk twardy na platformie Azure, aby go użyć do utworzenia nowych maszyn wirtualnych, w modelu wdrażania usługi Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: cynthn
ms.openlocfilehash: 2e78ecf6bd281bd5d30f59413789eb1e6fc7b5bc
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>Przekazywanie uogólniony wirtualny dysk twardy i umożliwia tworzenie nowych maszyn wirtualnych na platformie Azure

Ten temat przeprowadzi Cię przez przekazywanie wirtualnego dysku twardego uogólnionego maszyny wirtualnej na platformie Azure, tworzenie obrazu na podstawie wirtualnego dysku twardego i utworzyć nową maszynę Wirtualną z tego obrazu przy użyciu programu PowerShell. Możesz przekazać dysku VHD wyeksportowane z narzędzia wirtualizacji lokalnej lub innej chmury. Przy użyciu [dysków zarządzanych](managed-disks-overview.md) dla nowej maszyny Wirtualnej upraszcza zarządzanie maszyny Wirtualnej i zapewnia większą dostępność, gdy maszyna wirtualna jest umieszczona w zestawie dostępności. 

Jeśli chcesz użyć przykładowego skryptu, zobacz [przykładowy skrypt do przekazania dysku VHD na platformie Azure i utworzyć nową maszynę Wirtualną](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)

## <a name="before-you-begin"></a>Przed rozpoczęciem

- Przed przekazaniem jakiegokolwiek dysku VHD na platformę Azure, należy wykonać [Przygotowywanie wirtualnego dysku twardego Windows lub VHDX do przekazania do platformy Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- Przegląd [planowanie migracji do zarządzanych dysków](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) przed rozpoczęciem migracji do [dysków zarządzanych](managed-disks-overview.md).
- Upewnij się, że masz najnowszą wersję modułu programu AzureRM.Compute PowerShell. Uruchom następujące polecenie, aby go zainstalować.

    ```powershell
    Install-Module AzureRM.Compute -RequiredVersion 2.6.0
    ```
    Aby uzyskać więcej informacji, zobacz [przechowywanie wersji programu Azure PowerShell](/powershell/azure/overview).


## <a name="generalize-the-windows-vm-using-sysprep"></a>Maszyny Wirtualnej systemu Windows za pomocą programu Sysprep do uogólnienia

Program Sysprep usuwa wszystkie informacje osobiste konto, między innymi i przygotowuje komputer do użycia jako obraz. Aby uzyskać więcej informacji o narzędziu Sysprep, zobacz [sposobu użycia programu Sysprep: wprowadzenie](http://technet.microsoft.com/library/bb457073.aspx).

Upewnij się, że ról serwera uruchomionych na komputerze są obsługiwane przez program Sysprep. Aby uzyskać więcej informacji, zobacz [Obsługa programu Sysprep dla ról serwera](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Jeśli korzystasz z programu Sysprep przed przekazaniem dysk VHD do platformy Azure po raz pierwszy, upewnij się, masz [przygotować maszyny Wirtualnej](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) przed uruchomieniem programu Sysprep. 
> 
> 

1. Zaloguj się do maszyny wirtualnej systemu Windows.
2. Otwórz okno Wiersz polecenia jako administrator. Zmień katalog na **%windir%\system32\sysprep**, a następnie uruchom `sysprep.exe`.
3. W **narzędzie przygotowania systemu** okno dialogowe, wybierz opcję **wprowadź systemu Out-of-Box Experience (OOBE)**i upewnij się, że **Generalize** pole wyboru jest zaznaczone.
4. W **opcje zamykania**, wybierz pozycję **zamknięcia**.
5. Kliknij przycisk **OK**.
   
    ![Uruchom program Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. Po zakończeniu działania programu Sysprep, zamyka maszyny wirtualnej. Nie uruchamiaj ponownie maszyny Wirtualnej.



## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.
Jeśli nie masz jeszcze programu PowerShell w wersji 1.4 lub nowszy zainstalowany, przeczytaj [jak instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).

1. Otwórz program Azure PowerShell i zaloguj się do konta platformy Azure. Otwiera okno podręczne wprowadzenie poświadczeń konta platformy Azure.
   
    ```powershell
    Login-AzureRmAccount
    ```
2. Pobierz identyfikatory subskrypcji dla dostępnych subskrypcji.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Ustaw poprawną subskrypcję za pomocą identyfikatora subskrypcji. Zastąp  *<subscriptionID>*  o identyfikatorze poprawną subskrypcję.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>Uzyskaj konto magazynu
Potrzebujesz konta magazynu na platformie Azure do przechowywania załadowanego obrazu maszyny Wirtualnej. Możesz użyć istniejącego konta magazynu lub Utwórz nową. 

Jeśli wirtualny dysk twardy będzie używany do tworzenia zarządzanego dysku dla maszyny Wirtualnej, Lokalizacja konta magazynu musi być w tej samej lokalizacji, w którym zostanie utworzenie maszyny Wirtualnej.

Aby wyświetlić konta dostępny magazyn, wpisz:

```powershell
Get-AzureRmStorageAccount
```

Jeśli chcesz użyć istniejącego konta magazynu, przejdź do [przekazać obraz maszyny Wirtualnej](#upload-the-vm-vhd-to-your-storage-account) sekcji.

Jeśli musisz utworzyć konto magazynu, wykonaj następujące kroki:

1. Potrzebna jest nazwa grupy zasobów, w którym ma zostać utworzony na koncie magazynu. Aby dowiedzieć się, wszystkie grupy zasobów, które są w ramach subskrypcji, wpisz:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Aby utworzyć grupę zasobów o nazwie **myResourceGroup** w **wschodnie stany USA** regionu, wpisz:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "East US"
    ```

2. Utwórz konto magazynu o nazwie **mojekontomagazynu** w tej grupie zasobów za pomocą [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) polecenia cmdlet:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "East US"`
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
    Prawidłowe wartości - SkuName to:
   
   * **Standard_LRS** — magazyn lokalnie nadmiarowy. 
   * **Standard_ZRS** -strefy nadmiarowego magazynu.
   * **Standard_GRS** — z magazynu geograficznie nadmiarowego magazynu. 
   * **Standard_RAGRS** -dostęp do odczytu z magazynu geograficznie nadmiarowego magazynu. 
   * **Premium_LRS** — magazyn lokalnie nadmiarowy Premium. 

## <a name="upload-the-vhd-to-your-storage-account"></a>Przekazanie dysku VHD do konta magazynu

Użyj [AzureRmVhd Dodaj](https://msdn.microsoft.com/library/mt603554.aspx) polecenia cmdlet w celu przekazania dysku VHD do kontenera na koncie magazynu. W tym przykładzie powoduje przekazanie pliku *myVHD.vhd* z *"dyski twarde C:\Users\Public\Documents\Virtual\"*  na konto magazynu o nazwie *mojekontomagazynu* w *myResourceGroup* grupy zasobów. Plik zostaną umieszczone w kontenerze o nazwie *mojkontener* i Nowa nazwa pliku będzie *myUploadedVHD.vhd*.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


W przypadku powodzenia można uzyskać odpowiedzi, która wygląda podobnie do poniższego:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

W zależności od połączenia sieciowego i rozmiar pliku VHD to polecenie może zająć trochę czasu, aby zakończyć

Zapisz **docelowy identyfikator URI** ścieżkę do użycia w przyszłości, jeśli zamierzasz utworzyć dysków zarządzanych lub nowej maszyny Wirtualnej przy użyciu przekazywanego wirtualnego dysku twardego.

### <a name="other-options-for-uploading-a-vhd"></a>Inne opcje przekazywanie wirtualnego dysku twardego
 
 
Możesz również przekazywać dysku VHD do konta magazynu przy użyciu jednej z następujących czynności:

- [Narzędzie AzCopy](http://aka.ms/downloadazcopy)
- [Obiektu Blob magazynu Azure kopiowania interfejsu API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Obiekty BLOB magazynu Azure Explorer przekazywania](https://azurestorageexplorer.codeplex.com/)
- [Dokumentacja interfejsu API REST usługi Import/Eksport magazynu](https://msdn.microsoft.com/library/dn529096.aspx)
-   Zaleca się za pomocą usługi Import/eksport, jeśli szacowany czas przekazywania jest dłuższa niż 7 dni. Można użyć [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) oszacowanie godzinę z jednostki rozmiaru i transferu danych. 
    Narzędzie importu/eksportu może służyć do skopiowania do konta magazynu w warstwie standardowa. Należy skopiować z magazynu w warstwie standardowa do konta magazynu premium za pomocą narzędzia, takiego jak narzędzie AzCopy.

> [!IMPORTANT]
> Jeśli używasz narzędzia AzCopy przekazywania dysk VHD na platformę Azure, upewnij się, ustawiono [/BlobType:page](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy#blobtypeblock--page--append) przed uruchomieniem Przekaż skryptu. Jeśli obiektem docelowym jest obiektu blob, a ta opcja nie jest określona, domyślnie AzCopy tworzy blokowych obiektów blob.
> 
> 



## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>Tworzenie zarządzanego obrazu z przekazanego wirtualnego dysku twardego 

Tworzenie obrazu zarządzanych za pomocą programu uogólniony wirtualny dysk twardy systemu operacyjnego. Zastąp wartości odpowiednimi informacjami.


1.  Najpierw należy ustawić wspólne parametry:

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    $vmSize = "Standard_DS1_v2"
    $location = "East US" 
    $imageName = "yourImageName"
    ```

4.  Utworzyć obraz przy użyciu programu uogólniony wirtualny dysk twardy systemu operacyjnego.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $urlOfUploadedImageVhd
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

## <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej
Utwórz sieć wirtualną i podsieć [sieci wirtualnej](../../virtual-network/virtual-networks-overview.md).

1. Utwórz podsieć. W tym przykładzie tworzy podsieć o nazwie *mySubnet* z prefiksem adresu o *10.0.0.0/24*.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Utwórz sieć wirtualną. W tym przykładzie tworzy sieć wirtualną o nazwie *myVnet* z prefiksem adresu o *10.0.0.0/16*.  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Tworzenie publicznego adresu IP adres i interfejsu sieciowego

Aby umożliwić komunikację z maszyną wirtualną w sieci wirtualnej, potrzebujesz [publicznego adresu IP](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) i interfejsu sieciowego.

1. Utwórz publiczny adres IP. W tym przykładzie jest tworzony publiczny adres IP o nazwie *myPip*. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Utwórz kartę sieciową. W tym przykładzie jest tworzony karty Sieciowej o nazwie **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Tworzenie grupy zabezpieczeń sieci i reguły protokołu RDP

Aby móc zalogować się do maszyny Wirtualnej za pomocą protokołu RDP, musisz mieć reguły zabezpieczeń sieci (NSG), umożliwiająca dostęp RDP do portu 3389. 

W tym przykładzie tworzy grupy NSG o nazwie *myNsg* zawierający regułę o nazwie *myRdpRule* za pośrednictwem portu 3389 która zezwala na ruch RDP. Aby uzyskać więcej informacji na temat grup NSG, zobacz [Otwieranie portów dla maszyny Wirtualnej na platformie Azure przy użyciu programu PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Utwórz zmienną dla sieci wirtualnej

Utwórz zmienną dla ukończonych sieci wirtualnej. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-the-credentials-for-the-vm"></a>Uzyskiwanie poświadczeń dla maszyny Wirtualnej

Następujące polecenie cmdlet zostanie otwarte okno gdzie będą wprowadź nową nazwę użytkownika i hasło do użycia jako konto administratora lokalnego na zdalny dostęp do maszyny Wirtualnej. 

```powershell
$cred = Get-Credential
```

## <a name="add-the-vm-name-and-size-to-the-vm-configuration"></a>Dodaj nazwę maszyny Wirtualnej i rozmiar do konfiguracji maszyny Wirtualnej.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a>Ustaw obraz maszyny Wirtualnej jako źródło obrazu dla nowej maszyny Wirtualnej

Ustaw obraz źródłowy przy użyciu Identyfikatora zarządzanego obrazu maszyny Wirtualnej.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-the-os-configuration-and-add-the-nic"></a>Ustawienia konfiguracji systemu operacyjnego i Dodaj kartę sieciową.

Wprowadź typ magazynu (PremiumLRS lub StandardLRS) i rozmiar dysku systemu operacyjnego. W tym przykładzie typ konta w *PremiumLRS*, rozmiar dysku do *128 GB* i buforowanie dysku, aby *ReadWrite*.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-the-vm"></a>Tworzenie maszyny wirtualnej

Tworzenie nowej maszyny Wirtualnej za pomocą konfiguracji przechowywanej w **$vm** zmiennej.

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-the-vm-was-created"></a>Sprawdź, czy maszyna wirtualna została utworzona
Po zakończeniu powinien zostać wyświetlony nowo utworzony maszyny Wirtualnej w ramach [portalu Azure](https://portal.azure.com) w obszarze **Przeglądaj** > **maszyn wirtualnych**, lub za pomocą następujących poleceń programu PowerShell:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Kolejne kroki

Aby zalogować się do nowej maszyny wirtualnej, przejdź do maszyny Wirtualnej w [portal](https://portal.azure.com), kliknij przycisk **Connect**i Otwórz plik RDP pulpitu zdalnego. Korzystać z poświadczeń konta oryginalnego maszyny wirtualnej, aby zalogować się do nowej maszyny wirtualnej. Aby uzyskać więcej informacji, zobacz [jak połączenia i zaloguj się do maszyny wirtualnej platformy Azure systemem Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

