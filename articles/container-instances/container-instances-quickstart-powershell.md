---
title: "Szybki start — Tworzenie pierwszego kontenera usługi Azure Container Instances za pomocą programu PowerShell"
description: "Rozpocznij pracę z usługą Azure Container Instances, tworząc wystąpienie kontenera systemu Windows przy użyciu programu PowerShell."
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: f24619c9ff2667bb96827b1f17bf9ff04b42eaff
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Tworzenie pierwszego kontenera w usłudze Azure Container Instances

Usługa Azure Container Instances ułatwia tworzenie kontenerów Docker na platformie Azure oraz zarządzanie nimi bez konieczności inicjowania obsługi maszyn wirtualnych czy adoptowania usługi wyższego poziomu.

Podczas pracy z tym przewodnikiem Szybki start utworzysz kontener systemu Windows na platformie Azure i uwidocznisz go w Internecie przy użyciu publicznego adresu IP. Ta operacja jest wykonywana za pomocą jednego polecenia. W ciągu zaledwie kilku minut działająca aplikacja będzie widoczna w przeglądarce:

![Widziana w przeglądarce aplikacja wdrożona za pomocą usługi Azure Container Instances][qs-powershell-01]

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Jeśli postanowisz zainstalować program PowerShell i używać go lokalnie, ten samouczek wymaga modułu Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie `Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure.

## <a name="create-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów platformy Azure za pomocą polecenia [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi.

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Tworzenie kontenera

Kontener można utworzyć, podając nazwę, obraz usługi Docker i grupę zasobów platformy Azure w poleceniu cmdlet [New-AzureRmContainerGroup][New-AzureRmContainerGroup]. Opcjonalnie można ujawnić kontener w Internecie za pomocą publicznego adresu IP. W tym przypadku użyjemy kontenera z systemem Windows Nano Server, na którym działają usługi Internet Information Services (IIS).

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -IpAddressType Public
```

W ciągu kilku sekund otrzymasz odpowiedź na żądanie. Początkowo kontener ma stan **Tworzenie**, ale powinien zostać uruchomiony w ciągu jednej lub dwóch minut. Stan możesz sprawdzić za pomocą polecenia cmdlet [Get-AzureRmContainerGroupp][Get-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

Stan aprowizowania i adres IP kontenera będą widoczne w danych wyjściowych polecenia cmdlet:

```
ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/12345678-1234-1234-1234-12345678abcd/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            :
IpAddress                : 40.71.248.73
Ports                    : {80}
OsType                   : Windows
Volumes                  :
```

Gdy właściwość **ProvisioningState** kontenera będzie miała wartość `Succeeded`, możesz uzyskiwać do niego dostęp w przeglądarce przy użyciu podanego adresu IP.

![Widziana w przeglądarce usługa IIS wdrożona za pomocą usługi Azure Container Instances][qs-powershell-01]

## <a name="delete-the-container"></a>Usuwanie kontenera

Po zakończeniu pracy z kontenerem możesz go usunąć przy użyciu polecenia cmdlet [Remove-AzureRmContainerGroup][Remove-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start uruchomiliśmy wstępnie skompilowany kontener systemu Windows w usłudze Azure Container Instances. Jeśli chcesz spróbować samodzielnie skompilować kontener i wdrożyć go w usłudze Azure Container Instances za pomocą usługi Azure Container Registry, przejdź do samouczka dotyczącego usługi Azure Container Instances.

> [!div class="nextstepaction"]
> [Samouczek dotyczący usługi Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png