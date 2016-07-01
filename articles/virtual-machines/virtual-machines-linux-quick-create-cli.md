<properties
   pageTitle="Tworzenie maszyny wirtualnej z systemem Linux na platformie Azure przy użyciu interfejsu wiersza polecenia | Microsoft Azure"
   description="Tworzenie maszyny wirtualnej z systemem Linux na platformie Azure przy użyciu interfejsu wiersza polecenia."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="05/03/2016"
   ms.author="v-livech"/>


# Tworzenie maszyny wirtualnej z systemem Linux na platformie Azure przy użyciu interfejsu wiersza polecenia

W tym artykule przedstawiono szybki sposób wdrożenia maszyny wirtualnej z systemem Linux na platformie Azure przy użyciu polecenia `azure vm quick-create` interfejsu wiersza polecenia platformy Azure. Polecenie `quick-create` wdraża maszynę wirtualną z podstawową infrastrukturą wokół niej. Możesz jej użyć do bardzo szybkiego wdrożenia prototypu lub przetestowania konceptu (możesz traktować tę metodę jako najszybszy sposób uzyskania powłoki bash systemu Linux).  Konieczne jest posiadanie konta na platformie Azure ([uzyskaj bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/)) oraz zalogowanie się w [interfejsie wiersza polecenia platformy Azure](../xplat-cli-install.md) (`azure login`) działającym w trybie menedżera zasobów (`azure config mode arm`).  Szybkie wdrożenie maszyny wirtualnej z systemem Linux jest możliwe również przy użyciu [Portalu Azure](virtual-machines-linux-quick-create-portal.md).

## Szybkie polecenia — podsumowanie

Jedno polecenie umożliwiające wdrożenie maszyny wirtualnej z systemem CoreOS i dołączenie klucza SSH:

```bash
azure vm quick-create -M ~/.ssh/azure_id_rsa.pub -Q CoreOS
```

## Wdrażanie maszyny wirtualnej z systemem Linux

Korzystając z tego samego polecenia, które podano powyżej, poniżej przedstawiono każdy monit oraz spodziewane dane wyjściowe, ale z użyciem aliasu RHEL w celu utworzenia maszyny wirtualnej z systemem RedHat Enteprise Linux 7.2.  

## Korzystanie z aliasu ImageURN

Polecenie `quick-create` interfejsu wiersza polecenia platformy Azure zawiera aliasy mapowane na najczęściej używane dystrybucje systemu operacyjnego. W poniższej tabeli wymieniono aliasy dystrybucji (stan dla interfejsu wiersza polecenia platformy Azure w wersji 0.10).  Wszystkie wdrożenia przy użyciu polecenia `quick-create` domyślnie wykorzystują maszyny wirtualne z magazynem SSD, które oferują wysoką wydajność.

| Alias     | Wydawca | Oferta        | SKU         | Wersja |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | najnowsza  |
| CoreOS    | CoreOS    | CoreOS       | Stable      | najnowsza  |
| Debian    | credativ  | Debian       | 8           | najnowsza  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | najnowsza  |
| RHEL      | Redhat    | RHEL         | 7.2         | najnowsza  |
| SLES      | SLES      | SLES         | 12-SP1      | najnowsza  |
| UbuntuLTS | Canonical | UbuntuServer | 14.04.4-LTS | najnowsza  |



Dla opcji **ImageURN** (`-Q`) użyjemy wartości `RHEL` w celu wdrożenia maszyny wirtualnej z systemem RedHat Enterprise Linux 7.2. Te 7 aliasów reprezentuje niewielką część systemów operacyjnych dostępnych na platformie Azure. Możesz znaleźć więcej obrazów w portalu Marketplace, [wyszukując obraz](virtual-machines-linux-cli-ps-findimage.md) lub [przekazać własny obraz niestandardowy](virtual-machines-linux-create-upload-generic.md).

W poniższych krokach związanych z poleceniem zastąp wartości w monitach wartościami z własnego środowiska. Korzystamy z wartości przykładowych.  

Postępuj zgodnie z poleceniami i wprowadź własne nazwy.

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q RHEL
```

Dane wyjściowe powinny wyglądać podobnie do następującego bloku.

```bash
info:    Executing command vm quick-create
Resource group name: rhel-quick
Virtual machine name: rhel
Location name: westus
Operating system Type [Windows, Linux]: linux
User name: ops
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "rhel"
info:    Verifying the public key SSH file: /Users/ops/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli1630678171193501687
info:    Could not find the storage account "cli1630678171193501687", trying to create new one
+ Creating storage account "cli1630678171193501687" in "westus"
+ Looking up the storage account cli1630678171193501687
+ Looking up the NIC "rhel-westu-1630678171-nic"
info:    An nic with given name "rhel-westu-1630678171-nic" not found, creating a new one
+ Looking up the virtual network "rhel-westu-1630678171-vnet"
info:    Preparing to create new virtual network and subnet
+ Creating a new virtual network "rhel-westu-1630678171-vnet" [address prefix: "10.0.0.0/16"] with subnet "rhel-westu-1630678171-snet" [address prefix: "10.0.1.0/24"]
+ Looking up the virtual network "rhel-westu-1630678171-vnet"
+ Looking up the subnet "rhel-westu-1630678171-snet" under the virtual network "rhel-westu-1630678171-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "rhel-westu-1630678171-pip"
info:    PublicIP with given name "rhel-westu-1630678171-pip" not found, creating a new one
+ Creating public ip "rhel-westu-1630678171-pip"
+ Looking up the public ip "rhel-westu-1630678171-pip"
+ Creating NIC "rhel-westu-1630678171-nic"
+ Looking up the NIC "rhel-westu-1630678171-nic"
+ Looking up the storage account clisto909893658rhel
+ Creating VM "rhel"
+ Looking up the VM "rhel"
+ Looking up the NIC "rhel-westu-1630678171-nic"
+ Looking up the public ip "rhel-westu-1630678171-pip"
data:    Id                              :/subscriptions/<guid>/resourceGroups/rhel-quick/providers/Microsoft.Compute/virtualMachines/rhel
data:    ProvisioningState               :Succeeded
data:    Name                            :rhel
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :RedHat
data:        Offer                       :RHEL
data:        Sku                         :7.2
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic5abbc145c0242c1-os-1462425492101
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli1630678171193501687.blob.core.windows.net/vhds/clic5abbc145c0242c1-os-1462425492101.vhd
data:
data:    OS Profile:
data:      Computer Name                 :rhel
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-32-0F-DD
data:          Provisioning State        :Succeeded
data:          Name                      :rhel-westu-1630678171-nic
data:          Location                  :westus
data:            Public IP address       :104.42.236.196
data:            FQDN                    :rhel-westu-1630678171-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto909893658rhel.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

Teraz możesz nawiązać połączenie z maszyną wirtualną, korzystając z protokołu SSH oraz domyślnego portu SSH (22) i w pełni kwalifikowanej nazwy domeny (FQDN, fully qualified domain name) wyświetlonej w powyższych danych wyjściowych. Możesz również użyć podanego adresu IP.

```bash
ssh ops@rhel-westu-1630678171-pip.westus.cloudapp.azure.com
```
Proces logowania powinien wyglądać następująco:

```bash
The authenticity of host 'rhel-westu-1630678171-pip.westus.cloudapp.azure.com (104.42.236.196)' can't be established.
RSA key fingerprint is 0e:81:c4:36:2d:eb:3c:5a:dc:7e:65:8a:3f:3e:b0:cb.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'rhel-westu-1630678171-pip.westus.cloudapp.azure.com,104.42.236.196' (RSA) to the list of known hosts.
[ops@rhel ~]$ ls -a
.  ..  .bash_logout  .bash_profile  .bashrc  .cache  .config  .ssh
```

## Następne kroki

Polecenie `azure vm quick-create` jest szybkim sposobem wdrożenia maszyny wirtualnej, dzięki któremu możesz zalogować się do powłoki bash i rozpocząć pracę. Jednak użycie polecenia `vm quick-create` nie zapewnia dodatkowych korzyści związanych ze złożonym środowiskiem.  Aby wdrożyć maszynę wirtualną z systemem Linux dostosowaną do Twojej infrastruktury, możesz skorzystać z dowolnego z poniższych artykułów.

- [Use an Azure resource manager template to create a specific deployment (Tworzenie konkretnego wdrożenia za pomocą szablonu usługi Azure Resource Manager)](virtual-machines-linux-cli-deploy-templates.md)
- [Create your own custom environment for a Linux VM using Azure CLI commands directly](virtual-machines-linux-create-cli-complete.md) (Tworzenie niestandardowego środowiska dla maszyny wirtualnej z systemem Linux poprzez bezpośrednie użycie poleceń interfejsu wiersza polecenia platformy Azure)
- [Create a SSH Secured Linux VM on Azure using Templates (Tworzenie maszyny wirtualnej z systemem Linux zabezpieczonej przez protokół SSH na platformie Azure przy użyciu szablonów)](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

Te artykuły ułatwiają rozpoczęcie pracy związanej z tworzeniem infrastruktury Azure oraz dowolnej liczby narzędzi do wdrażania, konfiguracji i aranżacji infrastruktury (własnej lub typu open-source).



<!--HONumber=Jun16_HO2-->


