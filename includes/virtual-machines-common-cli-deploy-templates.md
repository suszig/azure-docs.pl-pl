
* [Szybkie tworzenie maszyny wirtualnej na platformie Azure](#quick-create-a-vm-in-azure)
* [Wdrażanie maszyny wirtualnej na platformie Azure na podstawie szablonu](#deploy-a-vm-in-azure-from-a-template)
* [Tworzenie maszyny wirtualnej za pomocą obrazu niestandardowego](#create-a-custom-vm-image)
* [Wdrażanie maszyny wirtualnej korzystającej z sieci wirtualnej i modułu równoważenia obciążenia](#deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer)
* [Usuwanie grupy zasobów](#remove-a-resource-group)
* [Wyświetlanie dziennika dla wdrożenia grupy zasobów](#show-the-log-for-a-resource-group-deployment)
* [Wyświetlanie informacji o maszynie wirtualnej](#display-information-about-a-virtual-machine)
* [Łączenie z maszyną wirtualną opartą na systemie Linux](#log-on-to-a-linux-based-virtual-machine)
* [Zatrzymywanie maszyny wirtualnej](#stop-a-virtual-machine)
* [Uruchamianie maszyny wirtualnej](#start-a-virtual-machine)
* [Dołączanie dysku danych](#attach-a-data-disk)

## <a name="getting-ready"></a>Przygotowanie
Aby używać interfejsu wiersza polecenia platformy Azure z grupami zasobów Azure, musisz mieć właściwą wersję interfejsu wiersza polecenia platformy Azure i konto platformy Azure. Jeśli nie masz interfejsu wiersza polecenia platformy Azure, [zainstaluj go](../articles/xplat-cli-install.md).

### <a name="update-your-azure-cli-version-to-090-or-later"></a>Aktualizowanie interfejsu wiersza polecenia platformy Azure do wersji 0.9.0 lub nowszej
Wpisz `azure --version`, aby sprawdzić, czy masz już zainstalowaną wersję 0.9.0 lub nowszą.

```azurecli
azure --version
0.9.0 (node: 0.10.25)
```

Jeśli nie masz wersji 0.9.0 ani nowszej, musisz zaktualizować posiadaną wersję przy użyciu jednego z natywnych instalatorów lub za pomocą narzędzia **npm**, wpisując `npm update -g azure-cli`.

Możesz także uruchomić interfejs wiersza polecenia platformy Azure przy użyciu następującego [obrazu platformy Docker](https://registry.hub.docker.com/u/microsoft/azure-cli/). Z poziomu hosta platformy Docker uruchom następujące polecenie:

```bash
docker run -it microsoft/azure-cli
```

### <a name="set-your-azure-account-and-subscription"></a>Ustawianie konta i subskrypcji platformy Azure
Jeśli nie masz jeszcze subskrypcji platformy Azure, ale masz subskrypcję MSDN, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). lub zarejestrować się w celu skorzystania z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

Teraz [interaktywnie zaloguj się do konta platformy Azure](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login), wpisując `azure login` i postępując zgodnie z monitami wyświetlanymi w środowisku logowania interakcyjnego do konta platformy Azure. 

> [!NOTE]
> Jeśli masz identyfikator służbowy i wiesz, że nie włączono uwierzytelniania dwuskładnikowego, możesz **również** użyć instrukcji `azure login -u` wraz z identyfikatorem służbowym, aby zalogować się do *bez* sesji interakcyjnej. Jeśli nie masz identyfikatora służbowego, możesz [utworzyć identyfikator służbowy z poziomu osobistego konta Microsoft](../articles/virtual-machines/virtual-machines-windows-create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), aby logować się w taki sam sposób.
>
>

Twoje konto może mieć więcej niż jedną subskrypcję. Możesz wpisać polecenie `azure account list`, aby wyświetlić listę swoich subskrypcji, która może wyglądać następująco:

```azurecli
azure account list
info:    Executing command account list
data:    Name                              Id                                    Tenant Id                            Current
data:    --------------------------------  ------------------------------------  ------------------------------------  -------
data:    Contoso Admin                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  true
data:    Fabrikam dev                      xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Fabrikam test                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Contoso production                xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
```

Aby ustawić bieżącą subskrypcję platformy Azure, wpisz poniższy kod. Użyj nazwy subskrypcji lub identyfikatora, który zawiera zasoby przeznaczone do zarządzania.

```azurecli
azure account set <subscription name or ID> true
```

### <a name="switch-to-the-azure-cli-resource-group-mode"></a>Przełączanie do trybu grupy zasobów interfejsu wiersza polecenia platformy Azure
Domyślnie interfejs wiersza polecenia platformy Azure jest uruchamiany w trybie zarządzania usługami (tryb **asm**). Wpisz poniższe polecenie, aby przełączyć do trybu grupy zasobów.

```azurecli
azure config mode arm
```

## <a name="understanding-azure-resource-templates-and-resource-groups"></a>Opis szablonów zasobów i grup zasobów platformy Azure
Większość aplikacji jest tworzona z połączenia różnych typów zasobów (na przykład jednej lub kilku maszyn wirtualnych i kont magazynu, bazy danych SQL, sieci wirtualnej lub sieci dostarczania zawartości). W domyślnym interfejsie API zarządzania usługami platformy Azure i klasycznej witrynie Azure Portal te elementy były przedstawiane przy użyciu podejścia „każda usługa oddzielnie”. Takie podejście wymaga wdrażania poszczególnych usług i zarządzania nimi (lub znalezienia narzędzia, które to robi) indywidualnie, a nie jako pojedynczą jednostką logiczną wdrożenia.

*Szablony usługi Azure Resource Manager* umożliwiają jednak wdrażanie tych różnych zasobów i zarządzanie nimi jako jedną jednostką logiczną wdrożenia w sposób deklaratywny. Zamiast instruować platformę Azure odnośnie wdrażanych zasobów w kolejnych poleceniach, możesz opisać całe wdrożenie (wszystkie zasoby oraz skojarzone parametry konfiguracji i wdrożenia) w pliku JSON i poinformować platformę Azure, że te zasoby mają zostać wdrożone jako jedna grupa.

Następnie możesz zarządzać całym cyklem życia zasobów grupy przy użyciu poleceń zarządzania zasobami interfejsu wiersza polecenia platformy Azure:

* zatrzymać, uruchomić lub usunąć wszystkie zasoby w grupie jednocześnie;
* zastosować reguły kontroli dostępu opartej na rolach (RBAC) w celu zablokowania dotyczących ich uprawnień zabezpieczeń;
* przeprowadzać inspekcje operacji;
* oznaczać zasoby za pomocą dodatkowych metadanych w celu ułatwienia śledzenia.

Znacznie więcej informacji na temat grup zasobów platformy Azure i oferowanych przez nie funkcji zawiera temat [Omówienie usługi Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md). Jeśli interesuje Cię tworzenie szablonów, zobacz [Tworzenie szablonów usługi Azure Resource Manager](../articles/resource-group-authoring-templates.md).

## <a name="a-idquick-create-a-vm-in-azureatask-quick-create-a-vm-in-azure"></a><a id="quick-create-a-vm-in-azure"></a>Zadanie: Szybkie tworzenie maszyny wirtualnej na platformie Azure
Czasami wiesz, którego obrazu potrzebujesz, i natychmiast chcesz utworzyć maszynę wirtualną z tego obrazu, niespecjalnie przejmując się infrastrukturą — być może musisz coś przetestować na czystej maszynie wirtualnej. To sytuacja, w której należy użyć polecenia `azure vm quick-create` i przekazać argumenty niezbędne do utworzenia maszyny wirtualnej i jej infrastruktury.

Najpierw utwórz grupę zasobów.

```azurecli
azure group create coreos-quick westus
info:    Executing command group create
+ Getting resource group coreos-quick
+ Creating resource group coreos-quick
info:    Created resource group coreos-quick
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick
data:    Name:                coreos-quick
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Następnie potrzebny będzie obraz. Aby znaleźć obraz za pomocą interfejsu wiersza polecenia platformy Azure, zobacz [Navigating and selecting Azure virtual machine images with PowerShell and the Azure CLI](../articles/virtual-machines/virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Przechodzenie do obrazów maszyn wirtualnych platformy Azure i wybieranie ich przy użyciu programu PowerShell i interfejsu wiersza polecenia). Na potrzeby tego artykułu uwzględniono krótką listę popularnych obrazów. W przypadku tego szybkiego tworzenia używany będzie obraz stabilny systemu CoreOS.

> [!NOTE]
> Dla opcji ComputeImageVersion można także po prostu podać wartość „latest” (najnowsze) jako parametr w języku szablonu i w interfejsie wiersza polecenia platformy Azure. Pozwoli to zawsze używać najnowszej wersji obrazu z zastosowanymi wszystkimi poprawkami bez konieczności modyfikowania skryptów i szablonów. Jest to pokazane poniżej.
>
>

| PublisherName | Oferta | SKU | Wersja |
|:--- |:--- |:--- |:--- |
| OpenLogic |CentOS |7 |7.0.201503 |
| OpenLogic |CentOS |7.1 |7.1.201504 |
| CoreOS |CoreOS |Beta |647.0.0 |
| CoreOS |CoreOS |Stable |633.1.0 |
| MicrosoftDynamicsNAV |DynamicsNAV |2015 |8.0.40459 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2013 |1.0.0 |
| msopentech |Oracle-Database-12c-Weblogic-Server-12c |Standardowa (Standard) |1.0.0 |
| msopentech |Oracle-Database-12c-Weblogic-Server-12c |Enterprise |1.0.0 |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-DW |12.0.2430 |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-OLTP |12.0.2430 |
| Canonical |UbuntuServer |12.04.5-LTS |12.04.201504230 |
| Canonical |UbuntuServer |14.04.2-LTS |14.04.201503090 |
| MicrosoftWindowsServer |WindowsServer |2012-Datacenter |3.0.201503 |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |4.0.201503 |
| MicrosoftWindowsServer |WindowsServer |Windows-Server-Technical-Preview |5.0.201504 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |1.0.141204 |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |4.3.4665 |

Aby utworzyć maszynę wirtualną, wprowadź polecenie `azure vm quick-create` i przygotuj się na monity. Powinno to wyglądać następująco:

```azurecli
azure vm quick-create
info:    Executing command vm quick-create
Resource group name: coreos-quick
Virtual machine name: coreos
Location name: westus
Operating system Type [Windows, Linux]: linux
ImageURN (format: "publisherName:offer:skus:version"): coreos:coreos:stable:latest
User name: ops
Password: *********
Confirm password: *********
+ Looking up the VM "coreos"
info:    Using the VM Size "Standard_A1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Retrieving storage accounts
info:    Could not find any storage accounts in the region "westus", trying to create new one
+ Creating storage account "cli9fd3fce49e9a9b3d14302" in "westus"
+ Looking up the storage account cli9fd3fce49e9a9b3d14302
+ Looking up the NIC "coreo-westu-1430261891570-nic"
info:    An nic with given name "coreo-westu-1430261891570-nic" not found, creating a new one
+ Looking up the virtual network "coreo-westu-1430261891570-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "coreo-westu-1430261891570-vnet" [address prefix: "10.0.0.0/16"] with subnet "coreo-westu-1430261891570-sne+" [address prefix: "10.0.1.0/24"]
+ Looking up the virtual network "coreo-westu-1430261891570-vnet"
+ Looking up the subnet "coreo-westu-1430261891570-snet" under the virtual network "coreo-westu-1430261891570-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "coreo-westu-1430261891570-pip"
info:    PublicIP with given name "coreo-westu-1430261891570-pip" not found, creating a new one
+ Creating public ip "coreo-westu-1430261891570-pip"
+ Looking up the public ip "coreo-westu-1430261891570-pip"
+ Creating NIC "coreo-westu-1430261891570-nic"
+ Looking up the NIC "coreo-westu-1430261891570-nic"
+ Creating VM "coreos"
+ Looking up the VM "coreos"
+ Looking up the NIC "coreo-westu-1430261891570-nic"
+ Looking up the public ip "coreo-westu-1430261891570-pip"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Compute/virtualMachines/coreos
data:    ProvisioningState               :Succeeded
data:    Name                            :coreos
data:    Location                        :westus
data:    FQDN                            :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :coreos
data:        Offer                       :coreos
data:        Sku                         :stable
data:        Version                     :633.1.0
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :cli9fd3fce49e9a9b3d-os-1430261892283
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli9fd3fce49e9a9b3d14302.blob.core.windows.net/vhds/cli9fd3fce49e9a9b3d-os-1430261892283.vhd
data:
data:    OS Profile:
data:      Computer Name                 :coreos
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :false
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Network/networkInterfaces/coreo-westu-1430261891570-nic
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-30-72-E3
data:          Provisioning State        :Succeeded
data:          Name                      :coreo-westu-1430261891570-nic
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.1.4
data:            Public IP address       :104.40.24.124
data:            FQDN                    :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
info:    vm quick-create command OK
```

I możesz iść dalej z nową maszyną wirtualną.

## <a name="a-iddeploy-a-vm-in-azure-from-a-templateatask-deploy-a-vm-in-azure-from-a-template"></a><a id="deploy-a-vm-in-azure-from-a-template"></a>Zadanie: Wdrażanie maszyny wirtualnej na platformie Azure na podstawie szablonu
Instrukcje zawarte w tych sekcjach umożliwiają wdrożenie nowej maszyny wirtualnej platformy Azure na podstawie szablonu przy użyciu interfejsu wiersza polecenia platformy Azure. Ten szablon umożliwia utworzenie jednej maszyny wirtualnej w nowej sieci wirtualnej z jedną podsiecią i, w odróżnieniu od `azure vm quick-create`, umożliwia dokładne opisanie tego, czego oczekujesz, i powtórzenie go bez błędów. Oto, co ten szablon tworzy:

![](./media/virtual-machines-common-cli-deploy-templates/new-vm.png)

### <a name="step-1-examine-the-json-file-for-the-template-parameters"></a>Krok 1. Sprawdzanie pliku JSON dla parametrów szablonu
Poniżej przedstawiono zawartość pliku JSON dla szablonu. (Szablon znajduje się również w witrynie [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json)).

Szablony są elastyczne, dzięki czemu projektant może udostępnić w szablonie bardzo wiele parametrów lub zaoferować tylko niewielki ich zestaw, tworząc mniej elastyczny szablon. Aby zebrać informacje potrzebne do przekazania szablonu w postaci parametrów, otwórz plik szablonu (w tym temacie wymieniono wbudowany szablon, poniżej) i sprawdź wartości **parametrów**.

W tym przypadku poniższy szablon poprosi o następujące informacje:

* Unikalna nazwa konta magazynu.
* Nazwa użytkownika administratora maszyny wirtualnej.
* Hasło.
* Nazwa domeny do użycia dla świata zewnętrznego.
* Numer wersji systemu Ubuntu Server (ale będzie akceptować tylko jeden z listy).

Więcej informacji na temat [wymagań dotyczących nazwy użytkownika i hasła](../articles/virtual-machines/virtual-machines-linux-faq.md#what-are-the-username-requirements-when-creating-a-vm).

Po podjęciu decyzji dotyczących tych wartości możesz utworzyć grupę i wdrożyć ten szablon w swojej subskrypcji platformy Azure.

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "newStorageAccountName": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for the storage account where the virtual machine's disks will be placed."
    }
    },
    "adminUsername": {
    "type": "string",
    "metadata": {
        "description": "User name for the virtual machine."
    }
    },
    "adminPassword": {
    "type": "securestring",
    "metadata": {
        "description": "Password for the virtual machine."
    }
    },
    "dnsNameForPublicIP": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for the public IP used to access the virtual machine."
    }
    },
    "ubuntuOSVersion": {
    "type": "string",
    "defaultValue": "14.04.2-LTS",
    "allowedValues": [
        "12.04.5-LTS",
        "14.04.2-LTS",
        "15.04"
    ],
    "metadata": {
        "description": "The Ubuntu version for the VM. This will pick a fully patched image of this given Ubuntu version. Allowed values: 12.04.5-LTS, 14.04.2-LTS, 15.04."
    }
    }
},
"variables": {
    "location": "West US",
    "imagePublisher": "Canonical",
    "imageOffer": "UbuntuServer",
    "OSDiskName": "osdiskforlinuxsimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyUbuntuVM",
    "vmSize": "Standard_D1",
    "virtualNetworkName": "MyVNET",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
},
"resources": [
    {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('newStorageAccountName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[variables('location')]",
    "properties": {
        "accountType": "[variables('storageAccountType')]"
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[variables('publicIPAddressName')]",
    "location": "[variables('location')]",
    "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
        "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
        }
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
    "location": "[variables('location')]",
    "properties": {
        "addressSpace": {
        "addressPrefixes": [
            "[variables('addressPrefix')]"
        ]
        },
        "subnets": [
        {
            "name": "[variables('subnetName')]",
            "properties": {
            "addressPrefix": "[variables('subnetPrefix')]"
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('nicName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
    ],
    "properties": {
        "ipConfigurations": [
        {
            "name": "ipconfig1",
            "properties": {
            "privateIPAllocationMethod": "Dynamic",
            "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
            },
            "subnet": {
                "id": "[variables('subnetRef')]"
            }
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', parameters('newStorageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
        "computername": "[variables('vmName')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
        "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('ubuntuOSVersion')]",
            "version": "latest"
        },
        "osDisk": {
            "name": "osdisk",
            "vhd": {
            "uri": "[concat('http://',parameters('newStorageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
        }
        },
        "networkProfile": {
        "networkInterfaces": [
            {
            "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
        ]
        }
    }
    }
]
}
```

### <a name="step-2-create-the-virtual-machine-by-using-the-template"></a>Krok 2. Tworzenie maszyny wirtualnej przy użyciu szablonu
Kiedy wartości parametrów są gotowe, musisz utworzyć grupę zasobów dla wdrożenia szablonu, a następnie wdrożyć szablon.

Aby utworzyć grupę zasobów, wpisz polecenie `azure group create <group name> <location>` z żądaną nazwą grupy oraz lokalizacją centrum danych, w której chcesz wykonać wdrożenie. To dzieje się szybko:

```azurecli
azure group create myResourceGroup westus
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Teraz aby utworzyć wdrożenie, wywołaj funkcję `azure group deployment create` i przekaż:

* Plik szablonu (jeśli powyższy szablon JSON został zapisany w pliku lokalnym).
* Identyfikator URI szablonu (jeśli chcesz wskazać plik z witryny GitHub lub innego adresu sieci Web).
* Grupa zasobów, w której chcesz wykonać wdrożenie.
* Opcjonalna nazwa wdrożenia.

Zostanie wyświetlony monit o podanie wartości parametrów w sekcji parametrów pliku JSON. Po określeniu wszystkich wartości parametrów rozpocznie się wdrożenie.

Oto przykład:

```azurecli
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json myResourceGroup firstDeployment
info:    Executing command group deployment create
info:    Supply values for the following parameters
newStorageAccountName: storageaccount
adminUsername: ops
adminPassword: password
dnsNameForPublicIP: newdomainname
```

Otrzymasz informacje następującego typu:

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "firstDeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment to complete
data:    DeploymentName     : firstDeployment
data:    ResourceGroupName  : myResourceGroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T07:53:55.1828878Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  -------------
data:    newStorageAccountName  String        storageaccount
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameForPublicIP     String        newdomainname
data:    ubuntuOSVersion        String        14.10
info:    group deployment create command OK
```


## <a name="a-idcreate-a-custom-vm-imageatask-create-a-custom-vm-image"></a><a id="create-a-custom-vm-image"></a>Zadanie: Tworzenie niestandardowej maszyny wirtualnej
Podstawowe użycia szablonów przedstawiono powyżej, a więc teraz możemy użyć podobnych instrukcji w celu utworzenia niestandardowej maszyny wirtualnej z określonego pliku VHD na platformie Azure przy użyciu szablonu za pośrednictwem interfejsu wiersza polecenia platformy Azure. Różnica tutaj polega na tym, że ten szablon tworzy jedną maszynę wirtualną z określonego wirtualnego dysku twardego (VHD).

### <a name="step-1-examine-the-json-file-for-the-template"></a>Krok 1. Sprawdzanie pliku JSON dla szablonu
Poniżej przedstawiono zawartość pliku JSON dla szablonu użytego jako przykład w tej sekcji. (Szablon znajduje się również w witrynie [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json)).

Ponownie musisz znaleźć wartości, które chcesz wprowadzić dla parametrów nieposiadających wartości domyślnych. Po uruchomieniu polecenia `azure group deployment create` interfejs wiersza polecenia platformy Azure wyświetli monit o wprowadzenie tych wartości.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters" : {
        "userImageStorageAccountName": {
            "type": "string",
            "defaultValue" : "userImageStorageAccountName"
        },
        "userImageStorageContainerName" : {
            "type" : "string",
            "defaultValue" : "userImageStorageContainerName"
        },
        "userImageVhdName" : {
            "type" : "string",
            "defaultValue" : "userImageVhdName"
        },
        "dnsNameForPublicIP" : {
            "type" : "string",
            "defaultValue": "uniqueDnsNameForPublicIP"
        },
        "adminUserName": {
            "type": "string"
        },
        "adminPassword": {
            "type": "securestring"
        },
        "osType" : {
            "type" : "string",
            "allowedValues" : [
                "windows",
                "linux"
            ]
        },
        "subscriptionId":{
            "type" : "string"
        },
        "location": {
            "type": "String",
            "defaultValue" : "West US"
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A2"
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue" : "myPublicIP"
        },
        "vmName": {
            "type": "string",
            "defaultValue" : "myVM"
        },
        "virtualNetworkName":{
            "type" : "string",
            "defaultValue" : "myVNET"
        },
        "nicName":{
            "type" : "string",
            "defaultValue":"myNIC"
        }
    },
    "variables": {
        "addressPrefix":"10.0.0.0/16",
        "subnet1Name": "Subnet-1",
        "subnet2Name": "Subnet-2",
        "subnet1Prefix" : "10.0.0.0/24",
        "subnet2Prefix" : "10.0.1.0/24",
        "publicIPAddressType" : "Dynamic",
        "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
        "subnet1Ref" : "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
        "userImageName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('userImageVhdName'))]",
        "osDiskVhdName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
    },
    "resources": [
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[parameters('publicIPAddressName')]",
        "location": "[parameters('location')]",
        "properties": {
            "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
            }
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/virtualNetworks",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "properties": {
        "addressSpace": {
            "addressPrefixes": [
            "[variables('addressPrefix')]"
            ]
        },
        "subnets": [
            {
            "name": "[variables('subnet1Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet1Prefix')]"
            }
            },
            {
            "name": "[variables('subnet2Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet2Prefix')]"
            }
            }
        ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[parameters('nicName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
            "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
        ],
        "properties": {
            "ipConfigurations": [
            {
                "name": "ipconfig1",
                "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                        "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                    },
                    "subnet": {
                        "id": "[variables('subnet1Ref')]"
                    }
                }
            }
            ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[parameters('vmName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
        ],
        "properties": {
            "hardwareProfile": {
                "vmSize": "[parameters('vmSize')]"
            },
            "osProfile": {
                "computername": "[parameters('vmName')]",
                "adminUsername": "[parameters('adminUsername')]",
                "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
                "osDisk" : {
                    "name" : "[concat(parameters('vmName'),'-osDisk')]",
                    "osType" : "[parameters('osType')]",
                    "caching" : "ReadWrite",
                    "image" : {
                        "uri" : "[variables('userImageName')]"
                    },
                    "vhd" : {
                        "uri" : "[variables('osDiskVhdName')]"
                    }
                }
            },
            "networkProfile": {
                "networkInterfaces" : [
                {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                }
                ]
            }
        }
    }
    ]
}
```

### <a name="step-2-obtain-the-vhd"></a>Krok 2. Uzyskiwanie dysku VHD
Oczywiście niezbędny będzie plik VHD. Możesz użyć dysku znajdującego się już na platformie Azure lub przekazać inny.

W przypadku maszyny wirtualnej z systemem Windows zobacz [Tworzenie wirtualnego dysku twardego systemu Windows Server i przekazywanie go na platformę Azure](../articles/virtual-machines/virtual-machines-windows-classic-createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

W przypadku maszyny wirtualnej z systemem Linux zobacz [Tworzenie i przekazywanie wirtualnego dysku twardego zawierającego system operacyjny Linux](../articles/virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="step-3-create-the-virtual-machine-by-using-the-template"></a>Krok 3: Tworzenie maszyny wirtualnej przy użyciu szablonu
Teraz możesz przystąpić do tworzenia nowej maszyny wirtualnej na podstawie pliku VHD. Utwórz grupę, w której ma zostać wykonane wdrożenie, używając polecenia `azure group create <location>`:

```azurecli
azure group create myResourceGroupUser eastus
info:    Executing command group create
+ Getting resource group myResourceGroupUser
+ Creating resource group myResourceGroupUser
info:    Created resource group myResourceGroupUser
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupUser
data:    Name:                myResourceGroupUser
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Następnie utwórz wdrożenie za pomocą opcji `--template-uri`, aby wywołać bezpośrednio w szablonie (ewentualnie możesz użyć opcji `--template-file`, aby użyć pliku zapisanego lokalnie). Należy zauważyć, że ponieważ szablon ma określone wartości domyślne, użytkownik jest monitowany tylko o kilka elementów. W przypadku wdrożenia szablonu w różnych miejscach może się okazać, że występują pewne konflikty nazewnictwa z wartościami domyślnymi (zwłaszcza z tworzoną nazwą DNS).

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json \
> myResourceGroup \
> customVhdDeployment
info:    Executing command group deployment create
info:    Supply values for the following parameters
adminUserName: ops
adminPassword: password
osType: linux
subscriptionId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

Dane wyjściowe wyglądają podobnie do poniższych:

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "customVhdDeployment"
+ Registering providers
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment to complete
error:   Deployment provisioning state was not successful
data:    DeploymentName     : customVhdDeployment
data:    ResourceGroupName  : myResourceGroupUser
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T14:55:48.0963829Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                           Type          Value
data:    -----------------------------  ------------  ------------------------------------
data:    userImageStorageAccountName    String        userImageStorageAccountName
data:    userImageStorageContainerName  String        userImageStorageContainerName
data:    userImageVhdName               String        userImageVhdName
data:    dnsNameForPublicIP             String        uniqueDnsNameForPublicIP
data:    adminUserName                  String        ops
data:    adminPassword                  SecureString  undefined
data:    osType                         String        linux
data:    subscriptionId                 String        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
data:    location                       String        West US
data:    vmSize                         String        Standard_A2
data:    publicIPAddressName            String        myPublicIP
data:    vmName                         String        myVM
data:    virtualNetworkName             String        myVNET
data:    nicName                        String        myNIC
info:    group deployment create command OK
```

## <a name="a-iddeploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balanceratask-deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a><a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Zadanie: Wdrażanie aplikacji z wieloma maszynami wirtualnymi, korzystającej z sieci wirtualnej i modułu równoważenia obciążenia
Ten szablon umożliwia utworzenie dwóch maszyn wirtualnych w ramach modułu równoważenia obciążenia i skonfigurowanie reguły równoważenia obciążenia na porcie 80. Ten szablon wdraża również konto magazynu, sieć wirtualną, publiczny adres IP, zestaw dostępności i interfejsy sieciowe.

![](./media/virtual-machines-common-cli-deploy-templates/multivmextlb.png)

Wykonaj poniższe kroki, aby wdrożyć aplikację z wieloma maszynami wirtualnymi, która korzysta z sieci wirtualnej i modułu równoważenia obciążenia, używając szablonu usługi Resource Manager z repozytorium szablonów witryny GitHub za pomocą poleceń programu PowerShell Azure.

### <a name="step-1-examine-the-json-file-for-the-template"></a>Krok 1. Sprawdzanie pliku JSON dla szablonu
Poniżej przedstawiono zawartość pliku JSON dla szablonu. Najnowsza wersja znajduje się [w repozytorium GitHub dla szablonów](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json). W tym temacie używany jest przełącznik `--template-uri` do wywołania szablonu, ale możesz także użyć przełącznika `--template-file` w celu przekazania lokalnej wersji.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of resources"
            }
        },
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "Name of storage account"
            }
        },
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "Admin user name"
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Admin password"
            }
        },
        "dnsNameforLBIP": {
            "type": "string",
            "metadata": {
                "description": "DNS for load balancer IP"
            }
        },
        "backendPort": {
            "type": "int",
            "defaultValue": 3389,
            "metadata": {
                "description": "Back-end port"
            }
        },
        "vmNamePrefix": {
            "type": "string",
            "defaultValue": "myVM",
            "metadata": {
                "description": "Prefix to use for VM names"
            }
        },
        "vmSourceImageName": {
            "type": "string",
            "defaultValue": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201412.01-en.us-127GB.vhd"
        },
        "lbName": {
            "type": "string",
            "defaultValue": "myLB",
            "metadata": {
                "description": "Load balancer name"
            }
        },
        "nicNamePrefix": {
            "type": "string",
            "defaultValue": "nic",
            "metadata": {
                "description": "Network interface name prefix"
            }
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue": "myPublicIP",
            "metadata": {
                "description": "Public IP name"
            }
        },
        "vnetName": {
            "type": "string",
            "defaultValue": "myVNET",
            "metadata": {
                "description": "Virtual network name"
            }
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A1",
            "metadata": {
                "description": "Size of the VM"
            }
        }
    },
    "variables": {
        "storageAccountType": "Standard_LRS",
        "vmStorageAccountContainerName": "vhds",
        "availabilitySetName": "myAvSet",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "Subnet-1",
        "subnetPrefix": "10.0.0.0/24",
        "publicIPAddressType": "Dynamic",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
        "numberOfInstances": 2,
        "nicId1": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 0))]",
        "nicId2": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 1))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LBFE')]",
        "backEndIPConfigID1": "[concat(variables('nicId1'),'/ipConfigurations/ipconfig1')]",
        "backEndIPConfigID2": "[concat(variables('nicId2'),'/ipConfigurations/ipconfig1')]",
        "sourceImageName": "[concat('/', subscription().subscriptionId,'/services/images/',parameters('vmSourceImageName'))]",
        "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/LBBE')]",
        "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": { }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('dnsNameforLBIP')]"
                }
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('vnetName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnetName')]",
                        "properties": {
                            "addressPrefix": "[variables('subnetPrefix')]"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/networkInterfaces",
            "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
            "location": "[parameters('location')]",
            "copy": {
                "name": "nicLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "subnet": {
                                "id": "[variables('subnetRef')]"
                            }
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/backendAddressPools/LBBE')]"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/inboundNatRule/RDP-VM', copyindex())]"
                            }
                        ]


                    }
                ]

            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "name": "[parameters('lbName')]",
            "type": "Microsoft.Network/loadBalancers",
            "location": "[parameters('location')]",
            "dependsOn": [
                "nicLoop",
                "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
            ],
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "LBFE",
                        "properties": {
                            "publicIPAddress": {
                                "id": "[variables('publicIPAddressID')]"
                            }
                        }
                    }
                ],
                "backendAddressPools": [
                    {
                        "name": "LBBE"

                    }
                ],
                "inboundNatRules": [
                    {
                        "name": "RDP-VM1",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50001,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    },
                    {
                        "name": "RDP-VM2",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50002,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "LBRule",
                        "properties": {
                            "frontendIPConfiguration": {
                                "id": "[variables('frontEndIPConfigID')]"
                            },
                            "backendAddressPool": {
                                "id": "[variables('lbPoolID')]"
                            },
                            "protocol": "tcp",
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 5,
                            "probe": {
                                "id": "[variables('lbProbeID')]"
                            }
                        }
                    }
                ],
                "probes": [
                    {
                        "name": "tcpProbe",
                        "properties": {
                            "protocol": "tcp",
                            "port": 80,
                            "intervalInSeconds": "5",
                            "numberOfProbes": "2"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
            "copy": {
                "name": "virtualMachineLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
                "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
                "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
            ],
            "properties": {
                "availabilitySet": {
                    "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
                },
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computername": "[concat(parameters('vmNamePrefix'), copyIndex())]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "sourceImage": {
                        "id": "[variables('sourceImageName')]"
                    },
                    "destinationVhdsContainer": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/')]"
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="step-2-create-the-deployment-by-using-the-template"></a>Krok 2. Tworzenie wdrożenia przy użyciu szablonu
Utwórz grupę zasobów szablonu przy użyciu polecenia `azure group create <location>`. Następnie utwórz wdrożenie do tej grupy zasobów za pomocą polecenia `azure group deployment create`, przekazując grupę wdrożenia, nazwę wdrożenia i odpowiadając na monity o parametry w szablonie, które nie mają wartości domyślnych.

```azurecli
azure group create lbgroup westus
info:    Executing command group create
+ Getting resource group lbgroup
+ Creating resource group lbgroup
info:    Created resource group lbgroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/lbgroup
data:    Name:                lbgroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Teraz użyj polecenia `azure group deployment create` i opcji `--template-uri`, aby wdrożyć szablon. Miej przygotowane wartości parametrów do wprowadzenia po wyświetleniu monitu, jak pokazano poniżej.

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json \
> lbgroup \
> newdeployment
info:    Executing command group deployment create
info:    Supply values for the following parameters
location: westus
newStorageAccountName: storagename
adminUsername: ops
adminPassword: password
dnsNameforLBIP: lbdomainname
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "newdeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.compute
info:    Registering provider microsoft.network
+ Waiting for deployment to complete
data:    DeploymentName     : newdeployment
data:    ResourceGroupName  : lbgroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T20:58:40.1678876Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  ----------------------
data:    location               String        westus
data:    newStorageAccountName  String        storagename
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameforLBIP         String        lbdomainname
data:    backendPort            Int           3389
data:    vmNamePrefix           String        myVM
data:    imagePublisher         String        MicrosoftWindowsServer
data:    imageOffer             String        WindowsServer
data:    imageSKU               String        2012-R2-Datacenter
data:    lbName                 String        myLB
data:    nicNamePrefix          String        nic
data:    publicIPAddressName    String        myPublicIP
data:    vnetName               String        myVNET
data:    vmSize                 String        Standard_A1
info:    group deployment create command OK
```

Należy zauważyć, że ten szablon wdraża obraz systemu Windows Server, który jednak łatwo może zostać zastąpiony przez dowolny obraz systemu Linux. Czy chcesz utworzyć klaster Docker z wieloma menedżerami Swarm? [Możesz to zrobić](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).

## <a name="a-idremove-a-resource-groupatask-remove-a-resource-group"></a><a id="remove-a-resource-group"></a>Zadanie: Usuwanie grupy zasobów
Pamiętaj, że możesz wdrożyć ponownie do grupy zasobów, ale jeśli praca z nią została zakończona, możesz usunąć ją za pomocą polecenia `azure group delete <group name>`.

```azurecli
azure group delete myResourceGroup
info:    Executing command group delete
Delete resource group myResourceGroup? [y/n] y
+ Deleting resource group myResourceGroup
info:    group delete command OK
```

## <a name="a-idshow-the-log-for-a-resource-group-deploymentatask-show-the-log-for-a-resource-group-deployment"></a><a id="show-the-log-for-a-resource-group-deployment"></a>Zadanie: Wyświetlanie dziennika dla wdrożenia grupy zasobów
To zadanie jest typowe podczas tworzenia lub korzystania z szablonów. Do wywołania wyświetlania dzienników wdrożenia dla grupy służy polecenie `azure group log show <groupname>`, które wyświetla część informacji przydatną dla zrozumienia, dlaczego coś się stało lub się nie stało. (Aby uzyskać więcej informacji na temat rozwiązywania problemów dotyczących wdrożeń oraz inne informacje o problemach, zobacz [Troubleshoot common Azure deployment errors with Azure Resource Manager](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md) (Rozwiązywanie typowych problemów dotyczących wdrażania za pomocą usługi Azure Resource Manager).

W celu określenia konkretnych błędów możesz korzystać z narzędzi, takich jak na przykład narzędzie **jq**, aby badać problemy bardziej precyzyjnie, na przykład zidentyfikować poszczególne błędy, które wymagają naprawienia. W poniższym przykładzie użyto narzędzia **jq** do przeanalizowania dziennika wdrożenia dla **lbgroup** w poszukiwaniu błędów.

```azurecli
azure group log show lbgroup -l --json | jq '.[] | select(.status.value == "Failed") | .properties'
```
Możesz szybko znaleźć przyczynę problemu, naprawić błąd i ponowić próbę. W poniższym przypadku szablon tworzył dwie maszyny wirtualne jednocześnie, co spowodowało blokadę pliku VHD. (Po zmodyfikowaniu szablonu wdrożenie szybko się powiodło).

```json
{
    "statusCode": "Conflict",
    "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"The resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"AcquireDiskLeaseFailed\",\"message\":\"Failed to acquire lease while creating disk 'osdisk' using blob with URI http://storage.blob.core.windows.net/vhds/osdisk.vhd.\"}]}}"
}
```

## <a name="a-iddisplay-information-about-a-virtual-machineatask-display-information-about-a-virtual-machine"></a><a id="display-information-about-a-virtual-machine"></a>Zadanie: Wyświetlanie informacji o maszynie wirtualnej
Informacje o określonych maszynach wirtualnych w grupie zasobów można wyświetlić za pomocą polecenia `azure vm show <groupname> <vmname>`. Jeśli w grupie jest więcej niż jedna maszyna wirtualna, może być konieczne wcześniejsze wyświetlenie listy maszyn wirtualnych w grupie za pomocą polecenia `azure vm list <groupname>`.

```azurecli
azure vm list zoo
info:    Executing command vm list
+ Getting virtual machines
data:    Name   ProvisioningState  Location  Size
data:    -----  -----------------  --------  -----------
data:    myVM0  Succeeded          westus    Standard_A1
data:    myVM1  Failed             westus    Standard_A1
```

Następnie wyszukaj maszynę myVM1:

```azurecli
azure vm show zoo myVM1
info:    Executing command vm show
+ Looking up the VM "myVM1"
+ Looking up the NIC "nic1"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Failed
data:    Name                            :myVM1
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :MicrosoftWindowsServer
data:        Offer                       :WindowsServer
data:        Sku                         :2012-R2-Datacenter
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Windows
data:        Name                        :osdisk
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :http://zoostorageralph.blob.core.windows.net/vhds/osdisk.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Windows Configuration:
data:        Provision VM Agent          :true
data:        Enable automatic updates    :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Network/networkInterfaces/nic1
data:          Primary                   :false
data:          Provisioning State        :Succeeded
data:          Name                      :nic1
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.0.5
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/availabilitySets/MYAVSET
info:    vm show command OK
```

> [!NOTE]
> Jeśli chcesz programowo przechowywać dane wyjściowe poleceń konsoli i manipulować nimi, możesz użyć narzędzia analizy JSON, na przykład **[jq](https://github.com/stedolan/jq)** lub **[jsawk](https://github.com/micha/jsawk)**, lub bibliotek języka odpowiednich dla zadania.
>
>

## <a name="a-idlog-on-to-a-linux-based-virtual-machineatask-log-on-to-a-linux-based-virtual-machine"></a><a id="log-on-to-a-linux-based-virtual-machine"></a>Zadanie: Logowanie na maszynie wirtualnej z systemem Linux
Zazwyczaj maszyny z systemem Linux nawiązują połączenie za pośrednictwem protokołu SSH. Aby uzyskać więcej informacji, zobacz temat dotyczący [korzystania z protokołu SSH systemu Linux na platformie Azure](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="a-idstop-a-virtual-machineatask-stop-a-vm"></a><a id="stop-a-virtual-machine"></a>Zadanie: Zatrzymywanie maszyny wirtualnej
Uruchom następujące polecenie:

```azurecli
azure vm stop <group name> <virtual machine name>
```

> [!IMPORTANT]
> Użyj tego parametru, aby zachować wirtualny adres IP (VIP) sieci wirtualnej w przypadku, gdy jest to ostatnia maszyna wirtualna w tej sieci wirtualnej. <br><br> Jeśli używasz parametru `StayProvisioned`, opłata nadal będzie naliczana za maszynę wirtualną.
>
>

## <a name="a-idstart-a-virtual-machineatask-start-a-vm"></a><a id="start-a-virtual-machine"></a>Zadanie: Uruchamianie maszyny wirtualnej
Uruchom następujące polecenie:

```azurecli
azure vm start <group name> <virtual machine name>
```

## <a name="a-idattach-a-data-diskatask-attach-a-data-disk"></a><a id="attach-a-data-disk"></a>Zadanie: Dołączanie dysku danych
Musisz także zdecydować, czy dołączyć nowy dysk, czy taki, który zawiera dane. Dla nowego dysku polecenie tworzy plik VHD i dołącza go w tym samym poleceniu.

Aby dołączyć nowy dysk, uruchom następujące polecenie:

```azurecli
    azure vm disk attach-new <resource-group> <vm-name> <size-in-gb>
```

Aby dołączyć istniejący dysk danych, uruchom następujące polecenie:

```azurecli
azure vm disk attach <resource-group> <vm-name> [vhd-url]
```

Następnie należy zainstalować dysk, tak jak zwykle w systemie Linux.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać znacznie więcej przykładów użycia interfejsu wiersza polecenia platformy Azure z trybem **arm**, zobacz [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md) (Używanie interfejsu wiersza polecenia platformy Azure na komputerach Mac i komputerach z systemem Linux oraz Windows z usługą Azure Resource Manager). Aby dowiedzieć się więcej na temat zasobów platformy Azure i powiązanych koncepcji, zobacz [Omówienie usługi Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md).

Aby uzyskać dodatkowe szablony, których możesz użyć, zobacz [Szablony szybkiego startu platformy Azure](https://azure.microsoft.com/documentation/templates/) i [Application frameworks using templates](../articles/virtual-machines/virtual-machines-linux-app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Platformy aplikacji korzystające z szablonów).


<!--HONumber=Jan17_HO4-->


