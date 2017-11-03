---
title: "Azure kontenera usługi Szybki Start — wdrażanie klastra DC/OS | Dokumentacja firmy Microsoft"
description: "Azure kontenera usługi Szybki Start — wdrażanie klastra DC/OS"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontenery, mikrousługi, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 8070d224fe6281e61f67483d4f1dd905a2ab99eb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-a-dcos-cluster"></a>Wdrażanie klastra DC/OS

DC/OS przewiduje uruchamianie nowoczesnych i konteneryzowanych aplikacji rozproszonej platformy. Z usługi kontenera platformy Azure inicjowania obsługi klastra DC/OS gotowy produkcji jest proste i szybkie. Szczegóły tego szybki start podstawowe kroki potrzebne do wdrożenia DC/OS klastra i uruchom podstawowe obciążenie.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

Dla tego samouczka wymagany jest interfejs wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [az login](/cli/azure/#login) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-dcos-cluster"></a>Tworzenie klastra DC/OS

Tworzenie klastra DC/OS z [az acs utworzyć](/cli/azure/acs#create) polecenia.

Poniższy przykład tworzy klaster DC/OS o nazwie *myDCOSCluster* i tworzy kluczy SSH, jeśli jeszcze nie istnieje. Aby użyć określonego zestawu kluczy, użyj opcji `--ssh-key-value`.  

```azurecli
az acs create --orchestrator-type dcos --resource-group myResourceGroup --name myDCOSCluster --generate-ssh-keys
```

W niektórych przypadkach, np. ograniczonej wersji próbnej, subskrypcja platformy Azure ma ograniczony dostęp do zasobów platformy Azure. Jeśli wdrożenie nie powiedzie się z powodu ograniczonej liczby dostępnych rdzeni, zmniejsz domyślną liczbę agentów, dodając `--agent-count 1` do polecenia [az acs create](/cli/azure/acs#create). 

Po kilku minutach polecenie zakończeniu i zwraca informacje o wdrożeniu.

## <a name="connect-to-dcos-cluster"></a>Łączenie z klastrem DC/OS

Po utworzeniu klastra DC/OS, może być dostęp za pośrednictwem tunelu SSH. Uruchom następujące polecenie, aby zwrócić publiczny adres IP wzorca DC/OS. Ten adres IP jest przechowywana w zmiennej i używane w następnym kroku.

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

Do tworzenia tunelu SSH, uruchom następujące polecenie, a następnie postępuj zgodnie z wyświetlanymi instrukcjami. Jeśli port 80 jest już w użyciu, polecenie nie powiedzie się. Aktualizacji portu tunelowane do jednego nie korzystać, takich jak `85:localhost:80`. 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

Można przetestować tunelu SSH przechodząc do `http://localhost`. Jeśli port innych czy 80 został użyty, Dostosuj lokalizacji do dopasowania. 

W przypadku tunelowania SSH została pomyślnie utworzona, jest zwracana portalu DC/OS.

![DCOS INTERFEJSU UŻYTKOWNIKA](./media/container-service-dcos-quickstart/dcos-ui.png)

## <a name="install-dcos-cli"></a>Instalowanie interfejsu wiersza polecenia DC/OS

Interfejs wiersza polecenia DC/OS jest używany do zarządzania klastrem DC/OS z wiersza polecenia. Instalowanie przy użyciu interfejsu wiersza polecenia DC/OS [az dcos acs install-cli](/azure/acs/dcos#install-cli) polecenia. Jeśli używasz Azure CloudShell, interfejsu wiersza polecenia DC/OS jest już zainstalowana. 

Jeśli używasz interfejsu wiersza polecenia Azure macOS lub Linux, może być konieczne uruchomienie polecenia z sudo.

```azurecli
az acs dcos install-cli
```

Interfejsu wiersza polecenia można było korzystać z klastrem, musi być skonfigurowana do używania tunelu SSH. Aby to zrobić, uruchom następujące polecenie, dostosowywania port, jeśli to konieczne.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Uruchamianie aplikacji

Domyślnie planowania mechanizm dla klastra usługi ACS DC/OS jest Marathon. Platformy Marathon umożliwia uruchamianie aplikacji i zarządzanie stanem aplikacji w klastrze DC/OS. Aby zaplanować aplikacji za pośrednictwem platformy Marathon, Utwórz plik o nazwie *marathon app.json*i skopiuj do niego następującą zawartość. 

```json
{
  "id": "demo-app",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  },
  "acceptedResourceRoles": [
    "slave_public"
  ]
}
```

Uruchom następujące polecenie, aby zaplanować aplikacji do uruchamiania w klastrze DC/OS.

```azurecli
dcos marathon app add marathon-app.json
```

Aby wyświetlić stan wdrożenia dla aplikacji, uruchom następujące polecenie.

```azurecli
dcos marathon app list
```

Gdy **oczekiwania** zmienia wartość kolumny z *True* do *False*, wdrażanie aplikacji zostało zakończone.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/1    ---       ---      False      DOCKER   None
```

Pobierz publiczny adres IP agentów klastra DC/OS.

```azurecli
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Przeglądanie pod tym adresem zwraca domyślnej witryny NGINX.

![NGINX](./media/container-service-dcos-quickstart/nginx.png)

## <a name="delete-dcos-cluster"></a>Usuń klaster DC/OS

Gdy nie są już potrzebne, można użyć [usunięcie grupy az](/cli/azure/group#delete) polecenia, aby usunąć grupę zasobów, klaster DC/OS i wszystkie powiązane zasoby.

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Następne kroki

W tym szybki start wdrożeniu klastra DC/OS i zostało uruchomione proste kontenera Docker w klastrze. Aby dowiedzieć się więcej na temat usługi kontenera platformy Azure, nadal samouczki ACS.

> [!div class="nextstepaction"]
> [Zarządzanie klastrem ACS DC/OS](container-service-dcos-manage-tutorial.md)