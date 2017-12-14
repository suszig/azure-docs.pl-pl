---
title: "Kontenery równoważenia obciążenia w klastrze Azure DC/OS"
description: "Zrównoważenia obciążenia w wielu kontenerów w klastrze usługi kontenera platformy Azure DC/OS."
services: container-service
author: rgardler
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 06/02/2017
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 6f5467d0fbcc577a548f1100ed6e4d380fe38759
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2017
---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a>Kontenery Równoważenie obciążenia klastra usługi kontenera platformy Azure DC/OS

W tym artykule firma Microsoft Poznaj sposobu tworzenia wewnętrznego modułu równoważenia obciążenia w usłudze kontenera platformy Azure DC/OS zarządzane przy użyciu platformy Marathon-LB. Ta konfiguracja pozwala na skalowanie w poziomie aplikacji. Umożliwia także korzystanie z agenta publicznego i prywatnego klastrów dzięki wprowadzania z usługi równoważenia obciążenia na publiczny klastra i kontenerów aplikacji w klastrze prywatnych. W tym samouczku zostały wykonane następujące czynności:

> [!div class="checklist"]
> * Konfigurowanie równoważenia obciążenia platformy Marathon
> * Wdrażanie aplikacji przy użyciu usługi równoważenia obciążenia
> * Konfigurowanie i modułu równoważenia obciążenia Azure

Należy klastra ACS DC/OS, aby wykonać kroki opisane w tym samouczku. W razie potrzeby [w tym przykładzie skrypt](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) można utworzyć.

Dla tego samouczka wymagany jest interfejs wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a>Równoważenie obciążenia — omówienie

Istnieją dwie warstwy równoważenia obciążenia w klastrze usługi kontenera platformy Azure DC/OS: 

**Moduł równoważenia obciążenia Azure** udostępnia publicznych punktów wejścia (te, które uzyskują dostęp do użytkowników końcowych). Warstwa Azure LB jest udostępniany automatycznie przez usługi kontenera platformy Azure i jest domyślnie skonfigurowany tak, aby ujawnić portu 80 i 443 8080.

**Moduł równoważenia obciążenia platformy Marathon (warstwa marathon-lb)** tras ruchu przychodzącego żądania do wystąpień kontenera usługi te żądania. Skalowania kontenerów świadczących naszej usługi sieci web, warstwa marathon-lb jest dynamicznie dostosowuje. Ten moduł równoważenia obciążenia nie jest dostępny domyślnie w usłudze kontenera, ale jest łatwy do zainstalowania.

## <a name="configure-marathon-load-balancer"></a>Konfigurowanie równoważenia obciążenia platformy Marathon

Moduł Marathon Load Balancer dynamicznie zmienia konfigurację na podstawie wdrożonych kontenerów. Istnieje również odporne na utratę kontenera lub agenta — jeśli dzieje się tak, Apache Mesos uruchomieniu kontener w innym miejscu i dostosowuje warstwa marathon-lb.

Uruchom następujące polecenie, aby zainstalować usługę marathon równoważenia obciążenia w klastrze agenta publicznego.

```azurecli-interactive
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a>Wdrożenie aplikacji o zrównoważonym obciążeniu

Teraz po przygotowaniu pakietu marathon-lb możemy wdrożyć kontener aplikacji, dla którego chcemy zrównoważyć obciążenie. 

Najpierw pobierz FQDN publicznie ujawnionych agentów.

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

Następnie utwórz plik o nazwie *hello web.json* i skopiuj następującą zawartość. `HAPROXY_0_VHOST` Etykiety musi zostać zaktualizowany w pełni kwalifikowaną nazwą domeny agentów DC/OS. 

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
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
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}
```

Użyj interfejsu wiersza polecenia DC/OS, aby uruchomić aplikację. Domyślnie wdraża Marathon aplikacji do klastra prywatnych. Oznacza to, że powyższe wdrożenia jest dostępny tylko przez moduł równoważenia obciążenia, która zwykle jest zamierzone zachowanie.

```azurecli-interactive
dcos marathon app add hello-web.json
```

Po wdrożeniu aplikacji, przejdź do nazwy FQDN klastra agent Wyświetl aplikację ze zrównoważonym obciążeniem.

![Obraz aplikacji ze zrównoważonym obciążeniem](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a>Konfigurowanie usługi równoważenia obciążenia Azure

Domyślnie moduł Azure Load Balancer udostępnia porty 80, 8080 i 443. Jeśli używasz jednego z tych trzech portów (tak jak w powyższym przykładzie), nie musisz wykonywać żadnych działań. Powinno być możliwe trafienie nazwy FQDN równoważenia obciążenia agenta i każdym odświeżeniu, użytkownik będzie kliknij jedną z trzech serwerów sieci web w działaniu okrężnym. 

Użycie innego portu, musisz dodać regułę okrężnego oraz sondę modułu równoważenia obciążenia dla portu, który został użyty. Można to zrobić w [interfejsie wiersza polecenia Azure](../../azure-resource-manager/xplat-cli-azure-resource-manager.md), używając poleceń `azure network lb rule create` i `azure network lb probe create`.

## <a name="next-steps"></a>Następne kroki

W tym samouczku poznanie równoważenia obciążenia usługi ACS z zarówno Marathon, jak i Azure usługi równoważenia obciążenia w tym następujące akcje:

> [!div class="checklist"]
> * Konfigurowanie równoważenia obciążenia platformy Marathon
> * Wdrażanie aplikacji przy użyciu usługi równoważenia obciążenia
> * Konfigurowanie i modułu równoważenia obciążenia Azure

Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat integracji magazynu platformy Azure z DC/OS na platformie Azure.

> [!div class="nextstepaction"]
> [Azure instalacji udziału plików w klastrze DC/OS](container-service-dcos-fileshare.md)