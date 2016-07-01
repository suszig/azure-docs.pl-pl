<properties
    pageTitle="Tworzenie maszyny wirtualnej systemu Linux przy użyciu szablonu platformy Azure | Microsoft Azure"
    description="Tworzenie maszyny wirtualnej systemu Linux na platformie Azure przy użyciu szablonu usługi Azure Resource Manager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="04/29/2016"
    ms.author="v-livech"/>

# Tworzenie maszyny wirtualnej systemu Linux przy użyciu szablonu platformy Azure

W tym artykule przedstawiono szybki sposób wdrożenia maszyny wirtualnej systemu Linux na platformie Azure przy użyciu szablonu Azure.  Konieczne jest posiadanie konta na platformie Azure ([pobierz bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/)) oraz uruchomionego (`azure login`) [interfejsu wiersza polecenia platformy Azure](../xplat-cli-install.md) w trybie menedżera zasobów (`azure config mode arm`).  Szybkie wdrożenie maszyny wirtualnej systemu Linux jest możliwe również przy użyciu [Portalu Azure](virtual-machines-linux-quick-create-portal.md) lub [interfejsu wiersza polecenia platformy Azure](virtual-machines-linux-quick-create-cli.md).


## Szybkie polecenie

W poniższych przykładach poleceń zastąp wartości między znakami &lt; i &gt; wartościami z własnego środowiska.

```bash
azure group create \
-n quicksecuretemplate \
-l eastus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## Szczegółowy przewodnik

Szablony umożliwiają tworzenie maszyn wirtualnych na platformie Azure przy użyciu ustawień, które można dostosować podczas uruchomienia, np. nazwy użytkowników i nazwy hostów. W tym artykule skupimy się na uruchomieniu maszyny wirtualnej systemu Ubuntu przy użyciu szablonu platformy Azure, który tworzy sieciową grupę zabezpieczeń z otwartym tylko jednym portem (22 dla protokołu SSH). W celu zalogowania konieczne jest posiadanie kluczy SSH.

Szablony usługi Azure Resource Manager są plikami JSON, których można użyć do prostych, jednorazowych zadań, takich jak uruchomienie maszyny wirtualnej systemu Ubuntu (jak opisano to w tym artykule), lub do utworzenia złożonych konfiguracji platformy Azure dla całych środowisk, np. podczas wdrażania środowisk testowych, programistycznych lub produkcyjnych z sieci do systemu operacyjnego w celu wdrożenia stosu aplikacji.

## Tworzenie maszyny wirtualnej systemu Linux

Poniższy przykład kodu pokazuje sposób wywoływania polecenia `azure group create` w celu jednoczesnego utworzenia grupy zasobów i wdrożenia maszyny wirtualnej systemu Linux zabezpieczonej przez protokół SSH za pomocą [szablonu usługi Azure Resource Manager](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Należy pamiętać, że w tym przykładzie trzeba użyć nazw, które będą unikatowe w danym środowisku. Ten przykład wykorzystuje `quicksecuretemplate` jako nazwę grupy zasobów, `securelinux` jako nazwę maszyny wirtualnej i `quicksecurelinux` jako nazwę domeny podrzędnej.

```bash
azure group create \
-n quicksecuretemplate \
-l eastus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Dane wyjściowe

```bash
info:    Executing command group create
+ Getting resource group quicksecuretemplate
+ Creating resource group quicksecuretemplate
info:    Created resource group quicksecuretemplate
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== vlivech@azure
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/quicksecuretemplate
data:    Name:                quicksecuretemplate
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Możesz utworzyć nową grupę zasobów i wdrożyć maszynę wirtualną przy użyciu parametru `--template-uri` lub pobrać bądź utworzyć szablon lokalnie i przekazać go przy użyciu parametru `--template-file` ze ścieżką do pliku szablonu jako argumentem. Interfejs wiersza polecenia platformy Azure monituje o parametry wymagane przez szablon.

## Następne kroki

Po utworzeniu maszyn wirtualnych systemu Linux za pomocą szablonów możesz zobaczyć, jakie inne platformy aplikacji można wdrożyć przy użyciu szablonów. Przeszukaj [galerię szablonów](https://azure.microsoft.com/documentation/templates/), aby odkryć kolejne platformy aplikacji do wdrożenia.



<!--HONumber=Jun16_HO2-->


