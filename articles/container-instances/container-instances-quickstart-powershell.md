---
title: "Szybki Start — tworzenie Twojego pierwszego kontenera wystąpień kontenera platformy Azure przy użyciu programu PowerShell"
description: "Wprowadzenie do wystąpień kontenera Azure przez utworzenie wystąpienia kontenera systemu Windows przy użyciu programu PowerShell."
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: ca10274fc6a23d7f5e7436dbaf72a6e7a918f275
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Tworzenie pierwszego kontenera w usłudze Azure Container Instances

Wystąpień kontenera Azure ułatwia tworzenie i zarządzanie nimi kontenery Docker na platformie Azure, bez konieczności umieszczanie maszyn wirtualnych lub wdrożyć usługę wyższego poziomu usługi.

Tego przewodnika Szybki Start służy do tworzenia kontenera systemu Windows na platformie Azure i uwidacznia go do Internetu za pomocą publicznego adresu IP. Ta operacja jest wykonywana za pomocą jednego polecenia. W ciągu kilku minut można wyświetlić działającej aplikacji przeglądarki:

![Widziana w przeglądarce aplikacja wdrożona za pomocą usługi Azure Container Instances][qs-powershell-01]

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Jeśli postanowisz zainstalować program PowerShell i używać go lokalnie, ten samouczek wymaga modułu Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie `Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure.

## <a name="create-resource-group"></a>Tworzenie grupy zasobów

Tworzenie grupy zasobów platformy Azure z [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi.

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Tworzenie kontenera

Można utworzyć kontener, podając nazwę obrazu Docker i grupy zasobów platformy Azure do [AzureRmContainerGroup nowy] [ New-AzureRmContainerGroup] polecenia cmdlet. Opcjonalnie można ujawnić kontener w Internecie za pomocą publicznego adresu IP. W tym przypadku użyjemy kontener Windows Nano Server systemem Internet Information Services (IIS).

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -IpAddressType Public
```

W ciągu kilku sekund można uzyskać odpowiedzi na żądanie. Początkowo kontenera jest w **tworzenie** stanu, ale powinna być uruchamiana w minutę lub dwie. Możesz sprawdzić stan, za pomocą [Get-AzureRmContainerGroup] [ Get-AzureRmContainerGroup] polecenia cmdlet:

 ```azurepowershell-interactive
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

Stan inicjowania obsługi administracyjnej i adres IP kontenera są wyświetlane w danych wyjściowych polecenia cmdlet:

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

Raz kontenera **ProvisioningState** przenosi `Succeeded`, przejdziesz go w przeglądarce przy użyciu adresu IP, pod warunkiem.

![Usługi IIS wdrażane za pomocą wystąpień kontenera platformy Azure w przeglądarce][qs-powershell-01]

## <a name="delete-the-container"></a>Usuwanie kontenera

Po zakończeniu z kontenerem, możesz je usunąć za pomocą [AzureRmContainerGroup Usuń] [ Remove-AzureRmContainerGroup] polecenia cmdlet:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Następne kroki

W tym Szybki Start rozpoczęty kontener Windows wbudowanych w wystąpień kontenera platformy Azure. Jeśli chcesz spróbuj samodzielnie tworzenia kontenera i wdrażania go do wystąpień kontenera Azure za pomocą rejestru kontenera platformy Azure, przejdź do samouczka wystąpień kontenera platformy Azure.

> [!div class="nextstepaction"]
> [Samouczek wystąpień kontenera platformy Azure](./container-instances-tutorial-prepare-app.md)

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png