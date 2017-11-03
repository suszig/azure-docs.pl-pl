---
title: "Samouczek usługi kontenera platformy Azure — Zarządzanie DC/OS | Dokumentacja firmy Microsoft"
description: "Samouczek usługi kontenera platformy Azure — Zarządzanie DC/OS"
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
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: e93f782c26c32f97749e817ec59ee3c2ecb7e119
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-container-service-tutorial---manage-dcos"></a>Samouczek usługi kontenera platformy Azure — Zarządzanie DC/OS

DC/OS przewiduje uruchamianie nowoczesnych i konteneryzowanych aplikacji rozproszonej platformy. Z usługi kontenera platformy Azure inicjowania obsługi klastra DC/OS gotowy produkcji jest proste i szybkie. Szczegóły tego szybki start, podstawowe kroki potrzebne do wdrożenia klastra DC/OS i wykonywania podstawowych obciążenia.

> [!div class="checklist"]
> * Tworzenie klastra usługi ACS DC/OS
> * Łączenie z klastrem
> * Instalowanie interfejsu wiersza polecenia DC/OS
> * Wdrażanie aplikacji do klastra
> * Skalowanie aplikacji w klastrze
> * Skalowanie węzłów klastra DC/OS
> * Podstawowe możliwości zarządzania DC/OS
> * Usuń klaster DC/OS

Dla tego samouczka wymagany jest interfejs wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-dcos-cluster"></a>Tworzenie klastra DC/OS

Najpierw utwórz nową grupę zasobów o [Tworzenie grupy az](/cli/azure/group#create) polecenia. Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *westeurope*.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Następnie Utwórz klaster DC/OS z [az acs utworzyć](/cli/azure/acs#create) polecenia.

Poniższy przykład tworzy klaster DC/OS o nazwie *myDCOSCluster* i tworzy kluczy SSH, jeśli jeszcze nie istnieje. Aby użyć określonego zestawu kluczy, użyj opcji `--ssh-key-value`.  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

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

## <a name="install-dcos-cli"></a>Instalowanie interfejsu wiersza polecenia DC/OS

Instalowanie przy użyciu interfejsu wiersza polecenia DC/OS [az dcos acs install-cli](/azure/acs/dcos#install-cli) polecenia. Jeśli używasz Azure CloudShell, interfejsu wiersza polecenia DC/OS jest już zainstalowana. Jeśli używasz interfejsu wiersza polecenia Azure macOS lub Linux, może być konieczne uruchomienie polecenia z sudo.

```azurecli
az acs dcos install-cli
```

Interfejsu wiersza polecenia można było korzystać z klastrem, musi być skonfigurowana do używania tunelu SSH. Aby to zrobić, uruchom następujące polecenie, dostosowywania port, jeśli to konieczne.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Uruchamianie aplikacji

Domyślnie planowania mechanizm dla klastra usługi ACS DC/OS jest Marathon. Platformy Marathon umożliwia uruchamianie aplikacji i zarządzanie stanem aplikacji w klastrze DC/OS. Aby zaplanować aplikacji za pośrednictwem platformy Marathon, Utwórz plik o nazwie **marathon app.json**i skopiuj do niego następującą zawartość. 

```json
{
  "id": "demo-app-private",
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
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
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

Gdy **zadania** zmienia wartość kolumny z *0/1* do *1/1*, wdrażanie aplikacji zostało zakończone.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a>Skalowanie aplikacji platformy Marathon

W poprzednim przykładzie została utworzona aplikacja pojedynczego wystąpienia. Aby zaktualizować tego wdrożenia, dzięki czemu są dostępne trzy wystąpienia aplikacji, otwarcie **marathon app.json** pliku, a następnie zaktualizuj właściwości wystąpienia 3.

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 3,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

Aktualizowanie aplikacji przy użyciu `dcos marathon app update` polecenia.

```azurecli
dcos marathon app update demo-app-private < marathon-app.json
```

Aby wyświetlić stan wdrożenia dla aplikacji, uruchom następujące polecenie.

```azurecli
dcos marathon app list
```

Gdy **zadania** zmienia wartość kolumny z *1/3* do *3/1*, wdrażanie aplikacji zostało zakończone.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a>Uruchamianie aplikacji dostępny z Internetu

Klaster ACS DC/OS składa się z dwóch zestawów węzła, jeden publiczny, który jest dostępny w Internecie i prywatnego, który nie jest dostępny w Internecie. Domyślny zestaw jest prywatny węzły użytego w ostatnim przykładzie.

Aby aplikacja była dostępna w Internecie, należy je wdrożyć na zestaw węzłów publicznych. Aby to zrobić, należy podać `acceptedResourceRoles` obiekt wartość `slave_public`.

Utwórz plik o nazwie **nginx public.json** i skopiuj do niego następującą zawartość.

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
dcos marathon app add nginx-public.json
```

Pobierz publiczny adres IP agentów publicznych klastra DC/OS.

```azurecli 
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Przeglądanie pod tym adresem zwraca domyślnej witryny NGINX.

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a>Skalowanie klastra DC/OS

W poprzednich przykładach aplikacja została przeskalować do wielu wystąpień. Infrastruktura DC/OS można skalować w taki sposób, aby zapewnić bardziej lub mniej wydajności obliczeniowej. Jest to zrobić za pomocą [skalować acs az]() polecenia. 

Aby wyświetlić bieżące liczba agentów DC/OS, użyj [Pokaż acs az](/cli/azure/acs#show) polecenia.

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

Aby zwiększyć liczbę 5, należy użyć [skalować acs az](/cli/azure/acs#scale) polecenia. 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a>Usuń klaster DC/OS

Gdy nie są już potrzebne, można użyć [usunięcie grupy az](/cli/azure/group#delete) polecenia, aby usunąć grupę zasobów, klaster DC/OS i wszystkie powiązane zasoby.

```azurecli 
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku uzyskanych o podstawowe zadania zarządzania DC/OS, m.in. 

> [!div class="checklist"]
> * Tworzenie klastra usługi ACS DC/OS
> * Łączenie z klastrem
> * Instalowanie interfejsu wiersza polecenia DC/OS
> * Wdrażanie aplikacji do klastra
> * Skalowanie aplikacji w klastrze
> * Skalowanie węzłów klastra DC/OS
> * Usuń klaster DC/OS

Przejdź do następnego samouczek, aby dowiedzieć się więcej na temat aplikacji w DC/OS na platformie Azure równoważenia obciążenia. 

> [!div class="nextstepaction"]
> [Równoważenie obciążenia aplikacji](container-service-load-balancing.md)