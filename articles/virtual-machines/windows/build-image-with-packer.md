---
title: "Tworzenie obrazów maszyn wirtualnych Windows Azure z pakujący | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać pakujący do tworzenia obrazów maszyn wirtualnych systemu Windows na platformie Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/18/2017
ms.author: iainfou
ms.openlocfilehash: 7fc4f8091ea43df83805a3c406a8cd61c311d6fd
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-use-packer-to-create-windows-virtual-machine-images-in-azure"></a>Jak używać pakujący do tworzenia obrazów systemu Windows maszyny wirtualnej na platformie Azure
Każda maszyna wirtualna (VM) na platformie Azure jest tworzony z obrazu, który definiuje dystrybucji systemu Windows i wersji systemu operacyjnego. Obrazy mogą obejmować wstępnie zainstalowane aplikacje i konfiguracje. Portalu Azure Marketplace zawiera wiele obrazów pierwszy i innych firm dla najbardziej typowych systemu operacyjnego i środowisk aplikacji albo można utworzyć własne niestandardowe obrazy dostosowane do potrzeb użytkownika. W tym artykule szczegółowo przedstawiają, jak korzystać z narzędzia typu open source [pakujący](https://www.packer.io/) do definiowania i tworzenie niestandardowych obrazów na platformie Azure.


## <a name="create-azure-resource-group"></a>Tworzenie grupy zasobów platformy Azure
Podczas procesu tworzenia pakujący tworzy tymczasowy zasobów Azure zbudował źródłowej maszyny Wirtualnej. Aby przechwycić tego źródła do użycia jako obraz maszyny Wirtualnej, należy zdefiniować grupę zasobów. Dane wyjściowe z procesu tworzenia pakujący znajduje się w tej grupie zasobów.

Utwórz nową grupę zasobów o [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroup* w *eastus* lokalizacji:

```powershell
$rgName = "myResourceGroup"
$location = "East US"
New-AzureRmResourceGroup -Name $rgName -Location $location
```

## <a name="create-azure-credentials"></a>Utwórz poświadczenia platformy Azure
Pakujący jest uwierzytelniany w usłudze Azure przy użyciu nazwy głównej usługi. Podmiot zabezpieczeń usługi Azure jest tożsamość zabezpieczeń korzystających z aplikacji, usługami i automatyzacja takich narzędzi jak pakujący. Kontrolowanie i definiowanie uprawnień określające, jakie operacje nazwy głównej usługi można wykonać na platformie Azure.

Utwórz usługę podmiotu zabezpieczeń z [AzureRmADServicePrincipal nowy](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) i przypisz uprawnienia do nazwy głównej usługi utworzyć i zarządzanie zasobami za pomocą [AzureRmRoleAssignment nowy](/powershell/module/azurerm.resources/new-azurermroleassignment):

```powershell
$sp = New-AzureRmADServicePrincipal -DisplayName "Azure Packer" `
    -Password (ConvertTo-SecureString "P@ssw0rd!" -AsPlainText -Force)
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Aby uwierzytelniać na platformie Azure, również należy uzyskać Azure identyfikatorów dzierżawcy i subskrypcji z [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription):

```powershell
$sub = Get-AzureRmSubscription
$sub.TenantId
$sub.SubscriptionId
```

Korzystając z tych dwóch identyfikatorów w następnym kroku.


## <a name="define-packer-template"></a>Zdefiniuj szablon pakujący
Aby tworzyć obrazy, należy utworzyć szablon w formacie JSON. W szablonie należy zdefiniować producentów i provisioners wykonujące procesu tworzenia rzeczywistych. Ma pakujący [administracyjnej dla platformy Azure](https://www.packer.io/docs/builders/azure.html) który można zdefiniować zasobów platformy Azure, takich jak poświadczenia główne usługi utworzony w poprzednim kroku.

Utwórz plik o nazwie *windows.json* i wklej następującą zawartość. Wprowadź własne wartości dla następujących elementów:

| Parametr                           | Gdzie można uzyskać |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | Identyfikator podmiotu zabezpieczeń usługi widoku z`$sp.applicationId` |
| *client_secret*                     | Hasło określone w`$securePassword` |
| *tenant_id*                         | Dane wyjściowe z `$sub.TenantId` polecenia |
| *IDENTYFIKATOR_SUBSKRYPCJI*                   | Dane wyjściowe z `$sub.SubscriptionId` polecenia |
| *Identyfikator obiektu*                         | Identyfikator obiekt główny usługi widoku z`$sp.Id` |
| *managed_image_resource_group_name* | Nazwa grupy zasobów, utworzonego w pierwszym kroku |
| *managed_image_name*                | Nazwa obrazu dysków zarządzanych w tworzonym |

```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "0831b578-8ab6-40b9-a581-9a880a94aab1",
    "client_secret": "P@ssw0rd!",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "object_id": "a7dfb070-0d5b-47ac-b9a5-cf214fff0ae2",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Windows",
    "image_publisher": "MicrosoftWindowsServer",
    "image_offer": "WindowsServer",
    "image_sku": "2016-Datacenter",

    "communicator": "winrm",
    "winrm_use_ssl": "true",
    "winrm_insecure": "true",
    "winrm_timeout": "3m",
    "winrm_username": "packer",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "type": "powershell",
    "inline": [
      "Add-WindowsFeature Web-Server",
      "if( Test-Path $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml ){ rm $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml -Force}",
      "& $Env:SystemRoot\\System32\\Sysprep\\Sysprep.exe /oobe /generalize /shutdown /quiet"
    ]
  }]
}
```

Ten szablon tworzy Maszynę wirtualną systemu Windows Server 2016, instaluje usługi IIS, a następnie stanowi uogólnienie maszyny Wirtualnej przy użyciu narzędzia Sysprep.


## <a name="build-packer-image"></a>Utwórz obraz pakujący
Jeśli nie masz jeszcze pakujący zainstalowany na komputerze lokalnym [postępuj zgodnie z instrukcjami instalacji pakujący](https://www.packer.io/docs/install/index.html).

Utworzyć obraz, określając pakujący Twojego pliku szablonu w następujący sposób:

```bash
./packer build windows.json
```

Przykład danych wyjściowych z poprzedniego polecenia jest następujący:

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> task : Image deployment
==> azure-arm:  ->> dept : Engineering
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the certificate’s URL ...
==> azure-arm:  -> Key Vault Name        : ‘pkrkvpq0mthtbtt’
==> azure-arm:  -> Key Vault Secret Name : ‘packerKeyVaultSecret’
==> azure-arm:  -> Certificate URL       : ‘https://pkrkvpq0mthtbtt.vault.azure.net/secrets/packerKeyVaultSecret/8c7bd823e4fa44e1abb747636128adbb'
==> azure-arm: Setting the certificate’s URL ...
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.55.35’
==> azure-arm: Waiting for WinRM to become available...
==> azure-arm: Connected to WinRM!
==> azure-arm: Provisioning with Powershell...
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-powershell-provisioner902510110
    azure-arm: #< CLIXML
    azure-arm:
    azure-arm: Success Restart Needed Exit Code      Feature Result
    azure-arm: ------- -------------- ---------      --------------
    azure-arm: True    No             Success        {Common HTTP Features, Default Document, D...
    azure-arm: <Objs Version=“1.1.0.1” xmlns=“http://schemas.microsoft.com/powershell/2004/04"><Obj S=“progress” RefId=“0"><TN RefId=“0”><T>System.Management.Automation.PSCustomObject</T><T>System.Object</T></TN><MS><I64 N=“SourceId”>1</I64><PR N=“Record”><AV>Preparing modules for first use.</AV><AI>0</AI><Nil /><PI>-1</PI><PC>-1</PC><T>Completed</T><SR>-1</SR><SD> </SD></PR></MS></Obj></Objs>
==> azure-arm: Querying the machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-pq0mthtbtt/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Compute Name              : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm: Deleting the temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. The artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```

Trwa kilka minut, aż pakujący do tworzenia maszyny Wirtualnej, uruchom provisioners i wyczyścić wdrożenia.


## <a name="create-vm-from-azure-image"></a>Tworzenie maszyny Wirtualnej z obrazu platformy Azure
Można teraz utworzyć Maszynę wirtualną z obrazu z [AzureRmVM nowy](/powershell/module/azurerm.compute/new-azurermvm). Najpierw należy ustawić administrator nazwy użytkownika i hasła dla maszyny Wirtualnej z [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential).

```powershell
$cred = Get-Credential
```

Poniższy przykład tworzy Maszynę wirtualną o nazwie *myVM* z *myPackerImage*.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $rgName `
    -Location $location `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName $rgName `
    -Location $location `
    -AllocationMethod "Static" `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleWWW  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $rgName `
    -Location $location `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName $rgName `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Define the image created by Packer
$image = Get-AzureRMImage -ImageName myPackerImage -ResourceGroupName $rgName

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -Id $image.Id | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig
```

Trwa kilka minut, aby utworzyć maszynę Wirtualną z obrazu pakujący.


## <a name="test-vm-and-iis"></a>Test maszyny Wirtualnej i IIS
Publiczny adres IP maszyny wirtualnej z [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Poniższy przykład uzyskuje adres IP dla *myPublicIP* utworzony wcześniej:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName $rgName `
    -Name "myPublicIP" | select "IpAddress"
```

Następnie można wprowadzić publicznego adresu IP w przeglądarce sieci web.

![Domyślna witryna usług IIS](./media/build-image-with-packer/iis.png) 


## <a name="next-steps"></a>Następne kroki
W tym przykładzie użyto pakujący do utworzenia obrazu maszyny Wirtualnej z już zainstalowanymi usługami IIS. Można tego obrazu maszyny Wirtualnej będą widoczne obok istniejących przepływów pracy wdrażania, takie jak wdrożyć aplikację na maszyny wirtualne utworzone na podstawie obrazu z usługi Team Services, Ansible, Chef lub Puppet.

Dla szablonów pakujący dodatkowe przykładowe dla innych dystrybucjach systemu Windows, temacie [tego repozytorium GitHub](https://github.com/hashicorp/packer/tree/master/examples/azure).