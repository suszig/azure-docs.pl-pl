<properties
    pageTitle="Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu szablonu Azure | Microsoft Azure"
    description="Tworzenie bezpiecznej maszyny wirtualnej systemu Linux na platformie Azure przy użyciu szablonu usługi Azure Resource Manager."
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
    ms.date="04/27/2016"
    ms.author="v-livech"/>

# Tworzenie zabezpieczonej maszyny wirtualnej systemu Linux przy użyciu szablonu Azure

Aby utworzyć maszynę wirtualną systemu Linux na podstawie szablonu, potrzebujesz [interfejsu wiersza polecenia platformy Azure](../xplat-cli-install.md) w trybie menedżera zasobów (`azure config mode arm`).

## Szybkie polecenia — podsumowanie

```bash
chrisl@fedora$ azure group create -n <exampleRGname> -l <exampleAzureRegion> [--template-uri <URL> | --template-file <path> | <template-file> -e <parameters.json file>]
```

## Szczegółowy przewodnik

Szablony umożliwiają tworzenie maszyn wirtualnych na platformie Azure przy użyciu ustawień, które można dostosować podczas uruchomienia, np. nazwy użytkowników i nazwy hostów. W tym artykule skupimy się na uruchomieniu maszyny wirtualnej systemu Ubuntu przy użyciu szablonu platformy Azure, który tworzy sieciową grupę zabezpieczeń z otwartym tylko jednym portem (22 dla protokołu SSH). W celu zalogowania konieczne jest posiadanie kluczy SSH.

Szablony usługi Azure Resource Manager są plikami JSON, których można użyć do prostych, jednorazowych zadań, takich jak uruchomienie maszyny wirtualnej systemu Ubuntu (jak opisano to w tym artykule), lub do utworzenia złożonych konfiguracji platformy Azure dla całych środowisk, np. podczas wdrażania środowisk testowych, programistycznych lub produkcyjnych z sieci do systemu operacyjnego w celu wdrożenia stosu aplikacji.

## Tworzenie maszyny wirtualnej systemu Linux

Poniższy przykład kodu pokazuje sposób wywoływania polecenia `azure group create` w celu jednoczesnego utworzenia grupy zasobów i wdrożenia maszyny wirtualnej systemu Linux zabezpieczonej przez protokół SSH za pomocą [szablonu usługi Azure Resource Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Należy pamiętać, że w tym przykładzie trzeba użyć nazw, które będą unikatowe w danym środowisku. Ten przykład wykorzystuje `quicksecuretemplate` jako nazwę grupy zasobów, `securelinux` jako nazwę maszyny wirtualnej i `quicksecurelinux` jako nazwę domeny podrzędnej.

```bash
chrisl@fedora$ azure group create -n quicksecuretemplate -l eastus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
info:    Executing command group create
+ Getting resource group quicksecuretemplate
+ Creating resource group quicksecuretemplate
info:    Created resource group quicksecuretemplate
info:    Supply values for the following parameters
adminUserName: ops
sshKeyData: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDDRZ/XB8p8uXMqgI8EoN3dWQw... user@contoso.com
dnsLabelPrefix: quicksecurelinux
vmName: securelinux
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<guid>/resourceGroups/quicksecuretemplate
data:    Name:                quicksecuretemplate
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Możesz utworzyć nową grupę zasobów i wdrożyć maszynę wirtualną przy użyciu parametru `--template-uri` lub pobrać bądź utworzyć szablon lokalnie i przekazać go przy użyciu parametru `--template-file` ze ścieżką do pliku szablonu jako argumentem. Interfejs wiersza polecenia platformy Azure monituje o parametry wymagane przez szablon.

## Następne kroki

Po utworzeniu maszyny wirtualnej systemu Linux za pomocą szablonów możesz zobaczyć, jakie inne struktury aplikacji są dostępne do wykorzystania przy użyciu szablonów. Przeszukaj [galerię szablonów](https://azure.microsoft.com/documentation/templates/), aby odkryć kolejne platformy aplikacji do wdrożenia.



<!--HONumber=Jun16_HO2-->


