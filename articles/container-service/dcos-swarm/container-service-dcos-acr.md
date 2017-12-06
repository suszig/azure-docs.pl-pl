---
title: "Przy użyciu ACR z klastrem Azure DC/OS"
description: "Rejestru kontenera Azure za pomocą klastra DC/OS usługi kontenera platformy Azure"
services: container-service
author: julienstroheker
manager: dcaro
ms.service: container-service
ms.topic: tutorial
ms.date: 03/23/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: 4a3213c28f24e9d1dfc309c6d34771ccc062dae4
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="use-acr-with-a-dcos-cluster-to-deploy-your-application"></a>Użyj ACR z klastrem DC/OS, aby wdrożyć aplikację

W tym artykule firma Microsoft Poznaj sposób użycia rejestru kontenera platformy Azure z klastrem DC/OS. Przy użyciu ACR umożliwia prywatnie przechowywania obrazów i zarządzanie nimi kontenera. Ten samouczek obejmuje następujące zadania:

> [!div class="checklist"]
> * Wdrażanie rejestru kontenera platformy Azure (w razie potrzeby)
> * Skonfiguruj uwierzytelnianie ACR w klastrze DC/OS
> * Przekazać obraz w rejestrze kontenera platformy Azure
> * Uruchom obrazu kontenera z rejestru kontenera platformy Azure

Należy klastra ACS DC/OS, aby wykonać kroki opisane w tym samouczku. W razie potrzeby [w tym przykładzie skrypt](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) można utworzyć.

Dla tego samouczka wymagany jest interfejs wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-azure-container-registry"></a>Wdrażanie rejestru kontenera platformy Azure

W razie potrzeby utwórz rejestru kontenera Azure [az acr utworzyć](/cli/azure/acr#create) polecenia. 

Poniższy przykład tworzy rejestru z losowo wygenerować nazwę. Rejestr jest również konfigurowany za pomocą konta administratora przy użyciu `--admin-enabled` argumentu.

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry$RANDOM --sku Basic
```

Po utworzeniu rejestru interfejsu wiersza polecenia Azure generuje danych podobny do następującego. Zwróć uwagę na `name` i `loginServer`, są one używane w kolejnych krokach.

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T03:40:56.511597+00:00",
  "id": "/subscriptions/f2799821-a08a-434e-9128-454ec4348b10/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry23489",
  "location": "eastus",
  "loginServer": "mycontainerregistry23489.azurecr.io",
  "name": "myContainerRegistry23489",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "mycontainerregistr034017"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Pobierz kontener przy użyciu poświadczeń rejestru [Pokaż poświadczeń acr az](/cli/azure/acr/credential) polecenia. SUBSTITUTE `--name` z elementem zauważyć w ostatnim kroku. Zwróć uwagę jednego hasła jest potrzebna w kolejnym kroku.

```azurecli-interactive
az acr credential show --name myContainerRegistry23489
```

Aby uzyskać więcej informacji na rejestru kontenera platformy Azure, zobacz [wprowadzenie do prywatnego rejestrów kontenera Docker](../../container-registry/container-registry-intro.md). 

## <a name="manage-acr-authentication"></a>Zarządzanie ACR uwierzytelniania

Konwencjonalne sposobem obrazu wypychania i ściągania z rejestru prywatnej jest najpierw uwierzytelnić się w rejestrze. Aby to zrobić, należy użyć `docker login` na klienta, który wymaga dostępu do rejestru prywatnych. Ponieważ klaster DC/OS może zawierać wiele węzłów, które mają być uwierzytelniane z ACR, warto zautomatyzować ten proces w każdym węźle. 

### <a name="create-shared-storage"></a>Utwórz magazyn udostępniony

Ten proces wykorzystuje na udział plików na platformę Azure, który został zainstalowany w każdym węźle w klastrze. Jeśli Magazyn udostępniony nie mają już skonfigurowany, zobacz [udział plików w klastrze DC/OS](container-service-dcos-fileshare.md).

### <a name="configure-acr-authentication"></a>Skonfiguruj uwierzytelnianie ACR

Najpierw uzyskać nazwę FQDN wzorca DC/OS i zapisze go w zmiennej.

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

Utwórz połączenie SSH z wzorcem (lub pierwszego serwera głównego) klastra systemu DC/OS. Zaktualizuj nazwę użytkownika, jeśli wartości innych niż domyślne był używany podczas tworzenia klastra.

```azurecli-interactive
ssh azureuser@$FQDN
```

Uruchom następujące polecenie, aby zalogować się do rejestru kontenera platformy Azure. Zastąp `--username` o nazwie rejestru kontenera i `--password` jeden z dostarczonego hasła. Zastąp ostatni argument *mycontainerregistry.azurecr.io* w tym przykładzie nazwą loginServer rejestru kontenera. 

To polecenie zapisuje wartości uwierzytelniania lokalnie w obszarze `~/.docker` ścieżki.

```azurecli-interactive
docker -H tcp://localhost:2375 login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry.azurecr.io
```

Tworzenie pliku skompresowanego, który zawiera wartości uwierzytelniania rejestru kontenera.

```azurecli-interactive
tar czf docker.tar.gz .docker
```

Skopiuj ten plik do magazynu udostępnionego klastra. Ten krok sprawia, że plik jest dostępny we wszystkich węzłach klastra DC/OS.

```azurecli-interactive
cp docker.tar.gz /mnt/share/dcosshare
```

## <a name="upload-image-to-acr"></a>Przekaż obraz do awaryjnego

Teraz z komputerze deweloperskim lub inne systemy z Docker zainstalowane, tworzenie obrazu i przekaż go do rejestru kontenera platformy Azure.

Utwórz kontener na podstawie obrazu Ubuntu.

```azurecli-interactive
docker run ubuntu --name base-image
```

Teraz przechwytywania kontenera do nowego obrazu. Nazwa obrazu musi zawierać `loginServer` registrywith kontenera w formacie nazwa `loginServer/imageName`.

```azurecli-interactive
docker -H tcp://localhost:2375 commit base-image mycontainerregistry30678.azurecr.io/dcos-demo
````

Zaloguj się do rejestru kontenera platformy Azure. Zamień nazwę nazwę loginServer — nazwa użytkownika o nazwie rejestru kontenera i--hasła za pomocą jednego z podanych haseł.

```azurecli-interactive
docker login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry2675.azurecr.io
```

Na koniec Przekaż obraz w rejestrze ACR. W tym przykładzie przesyła obraz o nazwie dcos demonstracyjnej.

```azurecli-interactive
docker push mycontainerregistry30678.azurecr.io/dcos-demo
```

## <a name="run-an-image-from-acr"></a>Uruchom obrazu z ACR

Aby użyć obrazu z rejestru ACR, Utwórz plik nazwy *acrDemo.json* i skopiuj następujący tekst do niego. Zamień na nazwę obrazu nazwa loginServer rejestru kontenera i nazwy obrazu, na przykład `loginServer/imageName`. Zwróć uwagę na `uris` właściwości. Ta właściwość zawiera lokalizację pliku uwierzytelniania rejestru kontenera, w tym przypadku jest udział plików Azure, który jest zainstalowany na każdym węźle w klastrze DC/OS.

```json
{
  "id": "myapp",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "mycontainerregistry30678.azurecr.io/dcos-demo",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "uris":  [
       "file:///mnt/share/dcosshare/docker.tar.gz"
   ]
}
```

Wdrożenie aplikacji z poziomu interfejsu wiersza polecenia DC / ° c.

```azurecli-interactive
dcos marathon app add acrDemo.json
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostały skonfiguruj DC/OS, aby użyć rejestru kontenera Azure, łącznie z następujących zadań:

> [!div class="checklist"]
> * Wdrażanie rejestru kontenera platformy Azure (w razie potrzeby)
> * Skonfiguruj uwierzytelnianie ACR w klastrze DC/OS
> * Przekazać obraz w rejestrze kontenera platformy Azure
> * Uruchom obrazu kontenera z rejestru kontenera platformy Azure
