---
title: "Użyj Terraform i Utwórz zestaw, za pomocą HCL skalowania maszyny Wirtualnej platformy Azure."
description: "Terraform używany do konfigurowania i wersji skali maszyny wirtualnej platformy Azure ustawić wraz z sieci wirtualnej i zarządzane dołączone dyski."
keywords: "terraform, devops, skalowanie maszyny ustawiony, wirtualnych, sieci, magazynu, modułów"
ms.service: virtual-machines-linux
author: dcaro
ms.author: dcaro
ms.date: 10/04/2017
ms.topic: article
ms.openlocfilehash: 7a4e21d547b3d2b2399f9f68b1babd9f82a421b7
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="use-terraform-to-plan-and-create-a-networked-azure-vm-scale-set-with-managed-storage"></a>Użyj Terraform zaplanować i utworzyć zestaw z magazynów zarządzana sieciowych skalowania maszyny Wirtualnej Azure

W tym artykule używamy [Terraform](https://www.terraform.io/) możesz utworzyć i wdrożyć [scaleset maszyny wirtualnej platformy Azure](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) z dysków zarządzanych za pomocą [Hashicorp konfiguracji języka](https://www.terraform.io/docs/configuration/syntax.html) (HCL).  

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie wdrożenia Terraform
> * Użyj zmiennych i dane wyjściowe Terraform wdrożenia 
> * Tworzenie i wdrażanie infrastruktury sieciowej
> * Tworzenie i wdrażanie zestaw skali maszyny wirtualnej i dołączenie go do sieci
> * Tworzenie i wdrażanie jumpbox nawiązywania połączenia z maszynami wirtualnymi za pośrednictwem SSH

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="before-you-begin"></a>Przed rozpoczęciem

- [Instalowanie Terraform i Konfiguracja dostępu do platformy Azure](/azure/virtual-machines/linux/terraform-install-configure)
- [Utwórz parę kluczy SSH](/azure/virtual-machines/linux/mac-create-ssh-keys) Jeśli nie został wcześniej.

## <a name="create-the-file-structure"></a>Tworzenie struktury plików

Utwórz trzy nowe pliki w pustych katalogów z następujących nazw:

- `variables.tf`Ten plik zawiera wartości zmiennych w szablonie.
- `output.tf`Ten plik zawiera opis ustawień, które będą wyświetlane po wdrożeniu.
- `vmss.tf`Ten kod skali maszyny wirtualnej ustawić infrastruktury.

## <a name="create-the-variables-and-output-definitions"></a>Utwórz zmienne i wyjściowych definicje

W tym kroku należy zdefiniować zmienne dostosować zasoby utworzone przez Terraform.

Edytuj `variables.tf` pliku, skopiuj następujący kod, a następnie zapisać zmiany.

```tf 
variable "location" {
  description = "The location where resources will be created"
  default     = "West US"
}

variable "resource_group_name" {
  description = "The name of the resource group in which the resources will be created"
  default     = "myResourceGroup"
}
```

Edytuj `output.tf` plik i skopiuj następujący kod do udostępnienia w pełni kwalifikowaną nazwę domeny dla maszyn wirtualnych. 

```hcl 
output "vmss_public_ip" {
    value = "${azurerm_public_ip.vmss.fqdn}"
}
```

## <a name="define-the-network-infrastructure-in-a-template"></a>Zdefiniuj w szablonie infrastruktury sieciowej

W tym kroku możesz utworzyć następującymi elementami infrastruktury sieci w nowej grupy zasobów platformy Azure: 

  - Jedną sieć Wirtualną z przestrzeni adresowej 10.0.0.0/16 
  - Jedna podsieć z przestrzenią adresową 10.0.2.0/24
  - Dwa publiczne adresy IP. Używaną przez maszynę wirtualną równoważenia obciążenia zestaw skalowania, używany do nawiązania połączenia SSH jumpbox innych.


Edytuj i skopiuj następujący kod w `vmss.tf` pliku: 

```tf
resource "azurerm_resource_group" "vmss" {
  name     = "${var.resource_group_name}"
  location = "${var.location}"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_virtual_network" "vmss" {
  name                = "vmss-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_subnet" "vmss" {
  name                 = "vmss-subnet"
  resource_group_name  = "${azurerm_resource_group.vmss.name}"
  virtual_network_name = "${azurerm_virtual_network.vmss.name}"
  address_prefix       = "10.0.2.0/24"
}

resource "azurerm_public_ip" "vmss" {
  name                         = "vmss-public-ip"
  location                     = "${var.location}"
  resource_group_name          = "${azurerm_resource_group.vmss.name}"
  public_ip_address_allocation = "static"
  domain_name_label            = "${azurerm_resource_group.vmss.name}"

  tags {
    environment = "codelab"
  }
}

``` 

> [!NOTE]
> Zaleca się oznaczyć tagami zasoby wdrażane na platformie Azure, aby ułatwić ich identyfikację w przyszłości.

## <a name="create-the-network-infrastructure"></a>Tworzenie infrastruktury sieciowej

Inicjowanie środowiska Terraform przez uruchomienie następującego polecenia w katalogu, w którym utworzono `.tf` plików:

```bash
terraform init 
```

Następnie uruchom następujące polecenie, aby wdrożyć infrastrukturę platformy Azure.

```bash
terraform apply
```

Upewnij się, że nazwa FQDN publiczny adres IP odpowiada konfiguracji: ![VMSS terraform nazwy FQDN dla publicznego adresu IP](./media/tf-create-vmss-step4-fqdn.png)


Grupa zasobów musi zawierać następujące zasoby: ![zasobów sieciowych terraform VMSS](./media/tf-create-vmss-step4-rg.png)


## <a name="edit-the-infrastructure-to-add-the-virtual-machine-scale-set"></a>Edytuj infrastruktury, aby dodać zestaw skali maszyny wirtualnej

W tym kroku należy dodać do szablonu następujące zasoby:

- Jeden Azure obciążenia równoważenia i reguły w celu obsługi aplikacji i dołączenie go do publicznego adresu IP skonfigurowane wcześniej.
- Wewnętrznej bazy danych Azure puli adresów i przypisz je do usługi równoważenia obciążenia 
- Port sondy kondycji używany przez aplikację i skonfigurowany w module równoważenia obciążenia 
- Ustaw działo się za usługą równoważenia obciążenia, systemem sieci wirtualnej wdrożonej wcześniej skali maszyny wirtualnej
- [Nginx](http://nginx.org/) w węzłach skalowania maszyny wirtualnej za pomocą niestandardowego rozszerzenia skryptu.

Dodaj następujący kod na końcu `vmss.tf` pliku.

```tf
resource "azurerm_lb" "vmss" {
  name                = "vmss-lb"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  frontend_ip_configuration {
    name                 = "PublicIPAddress"
    public_ip_address_id = "${azurerm_public_ip.vmss.id}"
  }

  tags {
    environment = "codelab"
  }
}

resource "azurerm_lb_backend_address_pool" "bpepool" {
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  loadbalancer_id     = "${azurerm_lb.vmss.id}"
  name                = "BackEndAddressPool"
}

resource "azurerm_lb_probe" "vmss" {
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  loadbalancer_id     = "${azurerm_lb.vmss.id}"
  name                = "ssh-running-probe"
  port                = "${var.application_port}"
}

resource "azurerm_lb_rule" "lbnatrule" {
    resource_group_name            = "${azurerm_resource_group.vmss.name}"
    loadbalancer_id                = "${azurerm_lb.vmss.id}"
    name                           = "http"
    protocol                       = "Tcp"
    frontend_port                  = "${var.application_port}"
    backend_port                   = "${var.application_port}"
    backend_address_pool_id        = "${azurerm_lb_backend_address_pool.bpepool.id}"
    frontend_ip_configuration_name = "PublicIPAddress"
    probe_id                       = "${azurerm_lb_probe.vmss.id}"
}

resource "azurerm_virtual_machine_scale_set" "vmss" {
  name                = "vmscaleset"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  upgrade_policy_mode = "Manual"

  sku {
    name     = "Standard_DS1_v2"
    tier     = "Standard"
    capacity = 2
  }

  storage_profile_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "16.04-LTS"
    version   = "latest"
  }

  storage_profile_os_disk {
    name              = ""
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  storage_profile_data_disk {
    lun          = 0
    caching        = "ReadWrite"
    create_option  = "Empty"
    disk_size_gb   = 10
  }

  os_profile {
    computer_name_prefix = "vmlab"
    admin_username       = "azureuser"
    admin_password       = "Passwword1234"
  }

  os_profile_linux_config {
    disable_password_authentication = true

    ssh_keys {
      path     = "/home/azureuser/.ssh/authorized_keys"
      key_data = "${file("~/.ssh/id_rsa.pub")}"
    }
  }

  network_profile {
    name    = "terraformnetworkprofile"
    primary = true

    ip_configuration {
      name                                   = "IPConfiguration"
      subnet_id                              = "${azurerm_subnet.vmss.id}"
      load_balancer_backend_address_pool_ids = ["${azurerm_lb_backend_address_pool.bpepool.id}"]
    }
  }

  extension { 
    name = "vmssextension"
    publisher = "Microsoft.OSTCExtensions"
    type = "CustomScriptForLinux"
    type_handler_version = "1.2"
    settings = <<SETTINGS
    {
        "commandToExecute": "sudo apt-get -y install nginx"
    }
    SETTINGS
  }

  tags {
    environment = "codelab"
  }
}
```

Dostosowywanie wdrożenia, dodając następujący kod do `variables.tf`:

```tf 
variable "application_port" {
    description = "The port that you want to expose to the external load balancer"
    default     = 80
}

variable "admin_password" {
    description = "Default password for admin"
    default = "Passwwoord11223344"
}
``` 


## <a name="deploy-the-virtual-machine-scale-set-in-azure"></a>Wdrażanie skalowania maszyny wirtualnej ustawić na platformie Azure

Uruchom następujące polecenie, aby wizualizować wdrożenia zestawu skali maszyny wirtualnej:

```bash
terraform plan
```

Dane wyjściowe polecenia powinny wyglądać następująco.
![Terraform Dodaj vmss planu](./media/tf-create-vmss-step6-plan884d3aefd9708a711bc09a66e85eb149c23a3ccff959655ec00418168b2bd481.png)

Następnie można wdrożyć dodatkowe zasoby platformy Azure: 

```bash
terraform apply 
```

Powinna wyglądać zawartości grupy zasobów:

![Grupa zasobów scaleset wirtualna Terraform](./media/tf-create-vmss-step6-apply.png)

Otwórz przeglądarkę i połącz się z nazwą FQDN, który został zwrócony przez polecenie. 

## <a name="add-an-ssh-jumpbox-to-the-existing-network"></a>Dodaj jumpbox SSH do istniejącej sieci 

W tym kroku należy skonfigurować następujące zasoby:
- Interfejs sieciowy jest podłączony do tej samej podsieci co zestaw skali maszyny wirtualnej.
- Maszyny wirtualnej jest połączona z interfejsem sieciowym. Ta jumpbox jest dostępny zdalnie. Po nawiązaniu połączenia można SSH do dowolnego w zestawie skalowania maszyn wirtualnych.



Dodaj następujący kod na końcu `vmss.tf` pliku:

```hcl 
resource "azurerm_public_ip" "jumpbox" {
  name                         = "jumpbox-public-ip"
  location                     = "${var.location}"
  resource_group_name          = "${azurerm_resource_group.vmss.name}"
  public_ip_address_allocation = "static"
  domain_name_label            = "${azurerm_resource_group.vmss.name}-ssh"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_network_interface" "jumpbox" {
  name                = "jumpbox-nic"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  ip_configuration {
    name                          = "IPConfiguration"
    subnet_id                     = "${azurerm_subnet.vmss.id}"
    private_ip_address_allocation = "dynamic"
    public_ip_address_id          = "${azurerm_public_ip.jumpbox.id}"
  }

  tags {
    environment = "codelab"
  }
}

resource "azurerm_virtual_machine" "jumpbox" {
  name                  = "jumpbox"
  location              = "${var.location}"
  resource_group_name   = "${azurerm_resource_group.vmss.name}"
  network_interface_ids = ["${azurerm_network_interface.jumpbox.id}"]
  vm_size               = "Standard_DS1_v2"

  storage_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "16.04-LTS"
    version   = "latest"
  }

  storage_os_disk {
    name              = "jumpbox-osdisk"
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  os_profile {
    computer_name  = "jumpbox"
    admin_username = "azureuser"
    admin_password = "Password1234!"
  }

  os_profile_linux_config {
    disable_password_authentication = true

    ssh_keys {
      path     = "/home/azureuser/.ssh/authorized_keys"
      key_data = "${file("~/.ssh/id_rsa.pub")}"
    }
  }

  tags {
    environment = "codelab"
  }
}
```

Edytuj `outputs.tf` i Dodaj następujący kod, aby wyświetlić nazwę hosta jumpbox po zakończeniu wdrażania:

```
output "jumpbox_public_ip" {
    value = "${azurerm_public_ip.jumpbox.fqdn}"
}
```

## <a name="deploy-the-jumpbox"></a>Wdrażanie jumpbox

Wdróż jumpbox.

```bash
terraform apply 
```

Po zakończeniu wdrażania zawartości grupy zasobów wygląda następująco:

![Grupa zasobów scaleset wirtualna Terraform](./media/tf-create-create-vmss-step8.png)

> [!NOTE]
> Zaloguj się za pomocą hasła jest wyłączone na jumpbox i zestawu skalowania maszyny wirtualnej została wdrożona. Zaloguj się przy użyciu protokołu SSH, aby dostęp do maszyn wirtualnych.

## <a name="clean-up-the-environment"></a>Wyczyść środowisko

Zasoby, utworzonej w tym samouczku zostaną usunięte następujące polecenia:

```bash
terraform destroy
```

Typ `yes` po otrzymaniu monitu, aby potwierdzić usuwanie zasobów. Niszczenie zajmie kilka minut.

## <a name="next-steps"></a>Następne kroki

W tym samouczku wdrożono ustawiony na platformie Azure przy użyciu Terraform skali maszyny wirtualnej. W tym samouczku omówiono:

> [!div class="checklist"]
> * Inicjowanie wdrażania Terraform
> * Użyj zmiennych i dane wyjściowe Terraform wdrożenia 
> * Tworzenie i wdrażanie infrastruktury sieciowej
> * Tworzenie i wdrażanie zestaw skali maszyny wirtualnej i dołącz je do istniejącego środowiska
> * Tworzenie i wdrażanie jumpbox nawiązywania połączenia z maszynami wirtualnymi za pośrednictwem SSH 
