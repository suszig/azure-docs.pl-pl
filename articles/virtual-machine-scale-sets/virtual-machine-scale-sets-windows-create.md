<properties
    pageTitle="Tworzenie zestawu skalowania maszyn wirtualnych przy użyciu programu PowerShell | Microsoft Azure"
    description="Tworzenie zestawu skalowania maszyn wirtualnych przy użyciu programu PowerShell"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="davidmu"/>


# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Tworzenie zestawu skalowania maszyn wirtualnych systemu Windows przy użyciu programu Azure PowerShell

Te kroki są stosowane w przypadku metody uzupełniania podczas tworzenia zestawu skalowania maszyn wirtualnych Azure. Zobacz [Omówienie zestawów skalowania maszyn wirtualnych](virtual-machine-scale-sets-overview.md), aby dowiedzieć się więcej o zestawach skalowania.

Wykonanie kroków opisanych w tym artykule powinno zająć około 30 minut.

## <a name="step-1:-install-azure-powershell"></a>Krok 1. Instalowanie programu Azure PowerShell

Zobacz [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Jak zainstalować i skonfigurować program Azure PowerShell), aby uzyskać informacje na temat instalowania najnowszej wersji programu Azure PowerShell, wybierania subskrypcji i logowania się do konta.

## <a name="step-2:-create-resources"></a>Krok 2: Tworzenie zasobów

Utwórz zasoby wymagane dla nowego zestawu skalowania.

### <a name="resource-group"></a>Grupa zasobów

Zestaw skalowania maszyn wirtualnych musi być zawarty w grupie zasobów.

1. Pobierz listę dostępnych lokalizacji i usług, które są obsługiwane:

        Get-AzureLocation | Sort Name | Select Name, AvailableServices

    Powinny zostać wyświetlone informacje podobne do następujących:

        Name                AvailableServices
        ----                -----------------
        Australia East      {Compute, Storage, PersistentVMRole, HighMemory}
        Australia Southeast {Compute, Storage, PersistentVMRole, HighMemory}
        Brazil South        {Compute, Storage, PersistentVMRole, HighMemory}
        Central India       {Compute, Storage, PersistentVMRole, HighMemory}
        Central US          {Compute, Storage, PersistentVMRole, HighMemory}
        East Asia           {Compute, Storage, PersistentVMRole, HighMemory}
        East US             {Compute, Storage, PersistentVMRole, HighMemory}
        East US 2           {Compute, Storage, PersistentVMRole, HighMemory}
        Japan East          {Compute, Storage, PersistentVMRole, HighMemory}
        Japan West          {Compute, Storage, PersistentVMRole, HighMemory}
        North Central US    {Compute, Storage, PersistentVMRole, HighMemory}
        North Europe        {Compute, Storage, PersistentVMRole, HighMemory}
        South Central US    {Compute, Storage, PersistentVMRole, HighMemory}
        South India         {Compute, Storage, PersistentVMRole, HighMemory}
        Southeast Asia      {Compute, Storage, PersistentVMRole, HighMemory}
        West Europe         {Compute, Storage, PersistentVMRole, HighMemory}
        West India          {Compute, Storage, PersistentVMRole, HighMemory}
        West US             {Compute, Storage, PersistentVMRole, HighMemory}

2. Wybierz najbardziej odpowiednią dla siebie lokalizację, zastąp wartość **$locName** nazwą lokalizacji, a następnie utwórz zmienną:

        $locName = "location name from the list, such as Central US"

3. Zastąp wartość **$rgName** nazwą, której chcesz używać dla nowej grupy zasobów, a następnie utwórz zmienną: 

        $rgName = "resource group name"
        
4. Utwórz grupę zasobów:
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Powinny zostać wyświetlone informacje podobne do następujących:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Konto magazynu

Konto magazynu jest używane przez maszynę wirtualną do przechowywania dysku systemu operacyjnego i danych diagnostycznych używanych do skalowania. Jeśli to możliwe, najlepiej jest mieć konto magazynu dla każdej maszyny wirtualnej utworzonej w zestawie skalowania. Jeśli nie jest to możliwe, należy planować nie więcej niż 20 maszyn wirtualnych na jedno konto magazynu. W przykładzie w tym artykule przedstawiono trzy konta magazynu tworzone dla trzech maszyn wirtualnych.

1. Zastąp wartość **$saName** nazwą konta magazynu. Sprawdź unikatowość nazwy. 

        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName

    Jeśli odpowiedzią jest **True** (Prawda), zaproponowana nazwa jest unikatowa.

3. Zastąp wartość **$saType** typem konta magazynu, a następnie utwórz zmienną:  

        $saType = "storage account type"
        
    Możliwe wartości to: Standard_LRS, Standard_GRS, Standard_RAGRS lub Premium_LRS.
        
4. Utwórz konto:
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Powinny zostać wyświetlone informacje podobne do następujących:

        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext

5. Powtórz kroki od 1 do 4, aby utworzyć trzy konta magazynu, na przykład myst1, myst2 i myst3.

### <a name="virtual-network"></a>Sieć wirtualna

Sieć wirtualna jest wymagana dla maszyn wirtualnych w zestawie skalowania.

1. Zastąp wartość **$subnetName** nazwą, której chcesz używać dla podsieci w sieci wirtualnej, a następnie utwórz zmienną: 

        $subnetName = "subnet name"
        
2. Utwórz konfigurację podsieci:
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
    Prefiks adresu może być inny w sieci wirtualnej.

3. Zastąp wartość **$netName** nazwą, której chcesz używać dla sieci wirtualnej, a następnie utwórz zmienną: 

        $netName = "virtual network name"
        
4. Utwórz sieć wirtualną:
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="public-ip-address"></a>Publiczny adres IP

Przed utworzeniem interfejsu sieciowego należy utworzyć publiczny adres IP.

1. Zastąp wartość **$domName** etykietą nazwy domeny, która ma być używana z tym publicznym adresem IP, a następnie utwórz zmienną:  

        $domName = "domain name label"
        
    Etykieta może zawierać tylko litery, cyfry i łączniki, a ostatni znak musi być literą lub cyfrą.
    
2. Sprawdź, czy nazwa jest unikatowa:
    
        Test-AzureRmDnsAvailability -DomainQualifiedName $domName -Location $locName

    Jeśli odpowiedzią jest **True** (Prawda), zaproponowana nazwa jest unikatowa.

3. Zastąp wartość **$pipName** nazwą, której chcesz używać dla sieci publicznego adresu IP, a następnie utwórz zmienną: 

        $pipName = "public ip address name"
        
4. Utwórz publiczny adres IP:
    
        $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic -DomainNameLabel $domName

### <a name="network-interface"></a>Interfejs sieciowy

Teraz, gdy masz publiczny adres IP, możesz utworzyć interfejs sieciowy.

1. Zastąp wartość **$nicName** nazwą, której chcesz używać dla interfejsu sieciowego, a następnie utwórz zmienną: 

        $nicName = "network interface name"
        
2. Utwórz interfejs sieciowy:
    
        $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id

### <a name="configuration-of-the-scale-set"></a>Konfiguracja zestawu skalowania

Masz wszystkie zasoby potrzebne do konfiguracji zestawu skalowania, więc utwórz go.  

1. Zastąp wartość **$ipName** nazwą, której chcesz używać dla konfiguracji IP, a następnie utwórz zmienną: 

        $ipName = "IP configuration name"
        
2. Utwórz konfigurację adresu IP:

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Zastąp wartość **$vmssConfig** nazwą, której chcesz używać dla konfiguracji zestawu skalowania, a następnie utwórz zmienną:   

        $vmssConfig = "Scale set configuration name"
        
3. Utwórz konfigurację dla zestawu skalowania:

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    Ten przykład przedstawia zestaw skalowania tworzony z trzema maszynami wirtualnymi. Zobacz [Omówienie zestawów skalowania maszyn wirtualnych](virtual-machine-scale-sets-overview.md), aby uzyskać więcej informacji o pojemności zestawów skalowania. Ten krok obejmuje również ustawienie rozmiaru (nazywanego SkuName, nazwą jednostki SKU) maszyn wirtualnych w zestawie. Aby znaleźć rozmiar spełniający wymagania, zobacz [Rozmiary maszyn wirtualnych](../virtual-machines/virtual-machines-windows-sizes.md).
    
4. Dodaj konfigurację interfejsu sieciowego do konfiguracji zestawu skalowania:
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Powinny zostać wyświetlone informacje podobne do następujących:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Profil systemu operacyjnego

1. Zastąp wartość **$computerName** prefiksem nazwy komputera, którego chcesz użyć, a następnie utwórz zmienną: 

        $computerName = "computer name prefix"
        
2. Zastąp wartość **$adminName** nazwą konta administratora na maszynach wirtualnych, a następnie utwórz zmienną:

        $adminName = "administrator account name"
        
3. Zastąp wartość **$adminPassword** hasłem konta, a następnie utwórz zmienną:

        $adminPassword = "password for administrator accounts"
        
4. Utwórz profil systemu operacyjnego

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Profil magazynu

1. Zastąp wartość **$storageProfile** nazwą, której chcesz używać dla profilu magazynu, a następnie utwórz zmienną:  

        $storageProfile = "storage profile name"
        
2. Utwórz zmienne, które definiują obraz do użycia:  
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Aby znaleźć informacje o innych używanych obrazach, zobacz temat [Navigate and select Azure virtual machine images with Windows PowerShell and the Azure CLI](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md) (Nawigacja i wybieranie obrazów maszyn wirtualnych platformy Azure za pomocą programu Windows PowerShell i interfejsu wiersza polecenia platformy Azure).
        
3. Zastąp wartość **$vhdContainers** listą zawierającą ścieżki, w których są przechowywane wirtualne dyski twarde, na przykład „https://mystorage.blob.core.windows.net/vhds”, a następnie utwórz zmienną:
       
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
        
4. Utwórz profil magazynu:

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Zestaw skalowania maszyn wirtualnych

Na koniec możesz utworzyć zestaw skalowania.

1. Zastąp wartość **$vmssName** nazwą zestawu skalowania maszyn wirtualnych, a następnie utwórz zmienną:

        $vmssName = "scale set name"
        
2. Tworzenie zestawu skalowania:

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Powinny zostać wyświetlone informacje podobne do następującego przykładu, który pokazuje pomyślne wdrożenie:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3:-explore-resources"></a>Krok 3: Przeglądanie zasobów

Użyj tych zasobów, aby eksplorować zestaw skalowania maszyn wirtualnych, który został utworzony:

- Azure Portal — za pośrednictwem witryny portalu dostępna jest ograniczona ilość informacji.
- [Eksplorator zasobów Azure](https://resources.azure.com/) — to narzędzie jest najlepsze do eksplorowania bieżącego stanu zestawu skalowania.
- Azure PowerShell — użyj tego polecenia, aby uzyskać informacje:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        
        Or 
        
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        

## <a name="next-steps"></a>Następne kroki

- Zarządzaj nowo utworzonym zestawem skalowania, korzystając z informacji zawartych w temacie [Manage virtual machines in a Virtual Machine Scale Set](virtual-machine-scale-sets-windows-manage.md) (Zarządzanie maszynami wirtualnymi w zestawie skalowania maszyn wirtualnych)
- Rozważ skonfigurowanie automatycznego skalowania dla zestawu skalowania przy użyciu informacji podanych w temacie [Automatic scaling and virtual machine scale sets](virtual-machine-scale-sets-autoscale-overview.md) (Automatyczne skalowanie i zestawy skalowania maszyn wirtualnych)
- Więcej informacji o skalowaniu w pionie zawiera artykuł [Vertical autoscale with Virtual Machine Scale sets](virtual-machine-scale-sets-vertical-scale-reprovision.md) (Automatyczne skalowanie w pionie za pomocą zestawów skalowania maszyn wirtualnych)



<!--HONumber=Oct16_HO3-->


