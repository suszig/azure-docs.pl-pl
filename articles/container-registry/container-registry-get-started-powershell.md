---
title: "Szybki Start — tworzenie prywatnych rejestru Docker na platformie Azure przy użyciu programu PowerShell"
description: "Dowiedz się szybko utworzyć prywatnego rejestru kontenera Docker przy użyciu programu PowerShell."
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: tysonn
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/08/2017
ms.author: nepeters
ms.openlocfilehash: b58b10e644e934cc38a6e0512ba7642ab8bf27c4
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/14/2017
---
# <a name="create-an-azure-container-registry-using-powershell"></a>Tworzenie rejestru kontenera Azure za pomocą programu PowerShell

Usługa Azure Container Registry to zarządzana usługa rejestru kontenerów platformy Docker używana do przechowywania prywatnych obrazów kontenerów Docker. Szczegóły tego przewodnika tworzenia wystąpienia rejestru kontenera Azure za pomocą programu PowerShell.

Ta opcja szybkiego startu wymaga programu Azure PowerShell w wersji modułu 3,6 lub nowszej. Uruchom polecenie `Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).

Musi mieć również Docker zainstalowane lokalnie. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) lub [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia `Login-AzureRmAccount` i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów platformy Azure za pomocą polecenia [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container-registry"></a>Tworzenie rejestru kontenerów

Tworzenie wystąpienia ACR przy użyciu [AzureRMContainerRegistry nowy](/powershell/module/containerregistry/New-AzureRMContainerRegistry) polecenia.

Nazwa rejestru **muszą być unikatowe**. W poniższym przykładzie *myContainerRegistry007* jest używany. Zaktualizuj to unikatowe wartości.

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-acr"></a>Zaloguj się do awaryjnego

Przed wypychaniem i ściąganiem obrazów kontenerów musisz zalogować się do wystąpienia usługi ACR. Najpierw użyj [Get AzureRmContainerRegistryCredential](/powershell/module/containerregistry/get-azurermcontainerregistrycredential) polecenie, aby uzyskać poświadczenia administratora dla tego wystąpienia ACR.

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Następnie użyj [logowania docker](https://docs.docker.com/engine/reference/commandline/login/) polecenie, aby zalogować się do wystąpienia ACR.

```bash
docker login $registry.LoginServer -u $creds.Username -p $creds.Password
```

Polecenie zwraca komunikat „Logowanie pomyślne” po ukończeniu.

## <a name="push-image-to-acr"></a>Obraz wypychania do awaryjnego

Aby wypchnąć obrazu do rejestru kontenera platformy Azure, najpierw musi mieć obraz. W razie potrzeby, uruchom następujące polecenie, aby ściąganie wstępnie utworzony obraz z Centrum Docker.

```bash
docker pull microsoft/aci-helloworld
```

Obraz musi być oznaczane nazwa ACR logowania serwera. Uruchom [Get-AzureRmContainerRegistry](/powershell/module/containerregistry/Get-AzureRmContainerRegistry) polecenia, aby zwrócić nazwę logowania serwera wystąpienia ACR.

```powershell
Get-AzureRmContainerRegistry | Select Loginserver
```

Tag przy użyciu obrazu [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) polecenia. Zastąp *acrLoginServer* z nazwą serwera logowania wystąpienia ACR.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Na koniec użyj [wypychania docker](https://docs.docker.com/engine/reference/commandline/push/) do dystrybuowania obrazów do wystąpienia ACR. Zastąp *acrLoginServer* z nazwą serwera logowania wystąpienia ACR.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, można użyć [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) polecenie, aby usunąć grupę zasobów, wystąpienie ACR i wszystkie obrazy kontenera.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

Ta opcja szybkiego startu została utworzona rejestru kontenera platformy Azure z wiersza polecenia platformy Azure. Jeśli chcesz rejestru kontenera Azure za pomocą wystąpień kontenera platformy Azure, przejdź do samouczka wystąpień kontenera platformy Azure.

> [!div class="nextstepaction"]
> [Samouczek wystąpień kontenera platformy Azure](../container-instances/container-instances-tutorial-prepare-app.md)