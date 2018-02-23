---
title: "Szybki start — tworzenie rejestru prywatnego platformy Docker na platformie Azure przy użyciu programu PowerShell"
description: "Szybko naucz się tworzyć rejestr prywatny platformy Docker przy użyciu programu PowerShell."
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 80b5055dee35cd6efe62ee949c05aef386a3ba14
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/13/2018
---
# <a name="create-an-azure-container-registry-using-powershell"></a>Tworzenie usługi Azure Container Registry przy użyciu programu PowerShell

Usługa Azure Container Registry to zarządzana usługa rejestru kontenerów platformy Docker używana do przechowywania prywatnych obrazów kontenerów Docker. W tym przewodniku znajdują się szczegółowe informacje dotyczące tworzenia wystąpienia usługi Azure Container Registry przy użyciu programu PowerShell.

Dla tego przewodnika Szybki start jest wymagany moduł Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie `Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).

Musisz mieć również zainstalowane lokalnie środowisko Docker. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac][docker-mac], [Windows][docker-windows] lub [Linux][docker-linux].

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

Utwórz wystąpienie usługi ACR za pomocą polecenia [New-AzureRMContainerRegistry](/powershell/module/containerregistry/New-AzureRMContainerRegistry).

Nazwa rejestru musi być unikatowa w obrębie platformy Azure i może zawierać od 5 do 50 znaków alfanumerycznych. W poniższym przykładzie użyto nazwy *myContainerRegistry007*. Zaktualizuj ją do unikatowej wartości.

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-acr"></a>Zaloguj się do usługi ACR

Przed wypychaniem i ściąganiem obrazów kontenerów musisz zalogować się do wystąpienia usługi ACR. Najpierw użyj polecenia [Get-AzureRmContainerRegistryCredential](/powershell/module/containerregistry/get-azurermcontainerregistrycredential), aby uzyskać poświadczenia administratora dla wystąpienia usługi ACR.

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Następnie użyj polecenia [docker login][docker-login], aby zalogować się do wystąpienia usługi ACR.

```bash
docker login $registry.LoginServer -u $creds.Username -p $creds.Password
```

Po ukończeniu polecenie zwraca ciąg `Login Succeeded`. Może także zostać wyświetlone ostrzeżenie o zabezpieczeniach zalecające użycie parametru `--password-stdin`. Użycie tego parametru wykracza poza zakres tego artykułu, jednak zalecamy zastosowanie tego najlepszego rozwiązania. Zobacz polecenie [docker login][docker-login], aby uzyskać więcej informacji.

## <a name="push-image-to-acr"></a>Wypychanie obrazu do usługi ACR

Aby wypchnąć obraz do usługi Azure Container Registry, najpierw musisz go mieć. W razie potrzeby uruchom następujące polecenie, aby ściągnąć wcześniej utworzony obraz z usługi Docker Hub.

```bash
docker pull microsoft/aci-helloworld
```

Obraz musi być otagowany nazwą serwera logowania usługi ACR. Uruchom polecenie [Get-AzureRmContainerRegistry](/powershell/module/containerregistry/Get-AzureRmContainerRegistry), aby zwrócić nazwę serwera logowania wystąpienia usługi ACR.

```powershell
Get-AzureRmContainerRegistry | Select Loginserver
```

Aby dodać tag do obrazu, użyj polecenia [docker tag][docker-tag]. Zastąp element *acrLoginServer* nazwą serwera logowania wystąpienia usługi ACR.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Na koniec użyj polecenia [docker push][docker-push], aby wypchnąć obrazy do wystąpienia usługi ACR. Zastąp element *acrLoginServer* nazwą serwera logowania wystąpienia usługi ACR.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy grupa zasobów, wystąpienie usługi ACR i wszystkie obrazy kontenerów nie będą już potrzebne, możesz je usunąć za pomocą polecenia [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku Szybki start utworzono usługę Azure Container Registry za pomocą interfejsu wiersza polecenia platformy Azure. Jeśli chcesz używać usługi Azure Container Registry z usługą Azure Container Instances, przejdź do samouczka dotyczącego usługi Azure Container Instances.

> [!div class="nextstepaction"]
> [Samouczek dotyczący usługi Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/