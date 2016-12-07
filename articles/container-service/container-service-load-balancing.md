---
title: "Kontenery równoważenia obciążenia klastra usługi Azure Container Service | Microsoft Docs"
description: "Równoważenie obciążenia w wielu kontenerach w klastrze usługi Azure Container Service."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Kontenery, mikrousługi, DC/OS, Azure"
ms.assetid: f0ab5645-2636-42de-b23b-4c3a7e3aa8bb
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: cf255856302ad5bdb1f6022d231833610acbcac5


---
# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>Równoważenie obciążenia w klastrze usługi Azure Container Service
W tym artykule pokażemy, jak utworzyć wewnętrzny moduł równoważenia obciążenia w usłudze Azure Container Service zarządzanej przez rozwiązanie DC/OS przy użyciu narzędzia Marathon-LB. Umożliwi to skalowanie aplikacji w poziomie. Ponadto umożliwi to wykorzystanie agentów klastrów publicznych i prywatnych poprzez umieszczenie modułów równoważenia obciążenia w klastrze publicznym i kontenerów aplikacji w klastrze prywatnym.

## <a name="prerequisites"></a>Wymagania wstępne
[Wdróż wystąpienie usługi Azure Container Service](container-service-deployment.md) z typem aranżacji DCOS i [upewnij się, że klient może połączyć się z klastrem](container-service-connect.md). 

## <a name="load-balancing"></a>Równoważenie obciążenia
Istnieją dwie warstwy równoważenia obciążenia w klastrze usługi Container Service, które utworzymy: 

1. Usługa Azure Load Balancer zapewnia publiczne punkty wejścia (z którymi kontaktują się użytkownicy końcowi). Jest ona dostarczana automatycznie przez usługę Azure Container Service i domyślnie skonfigurowana do udostępniania portów 80, 443 i 8080.
2. Usługa Marathon Load Balancer (marathon-lb) kieruje żądania przychodzące do wystąpień kontenera, które obsługują te żądania. Warstwa marathon-lb jest dynamicznie dostosowywana w przypadku skalowania kontenerów świadczących usługę sieci Web. Moduł równoważenia obciążenia nie jest zapewniany domyślnie w usłudze kontenera, ale można go łatwo zainstalować.

## <a name="marathon-load-balancer"></a>Marathon Load Balancer
Moduł Marathon Load Balancer dynamicznie zmienia konfigurację na podstawie wdrożonych kontenerów. Jest on również odporny na utratę kontenera lub agenta. W takim przypadku rozwiązanie Apache Mesos po prostu ponownie uruchomi kontener w innym miejscu, a moduł marathon-lb automatycznie dostosuje się.

Do instalacji modułu Marathon Load Balancer można użyć interfejsu użytkownika w sieci Web lub wiersza polecenia rozwiązania DC/OS.

### <a name="install-marathonlb-using-dcos-web-ui"></a>Instalacja modułu Marathon-LB przy użyciu interfejsu użytkownika w sieci Web rozwiązania DC/OS
1. Kliknij pozycję „Universe” (Wszechświat)
2. Wyszukaj pozycję „Marathon-LB”
3. Kliknij pozycję „Install” (Zainstaluj)

![Instalacja modułu marathon-lb przy użyciu interfejsu użytkownika w sieci Web rozwiązania DC/OS](./media/dcos/marathon-lb-install.png)

### <a name="install-marathonlb-using-the-dcos-cli"></a>Instalacja modułu Marathon-LB przy użyciu interfejsu wiersza polecenia rozwiązania DC/OS
Po zainstalowaniu interfejsu wiersza polecenia rozwiązania DC/OS i upewnieniu się co do możliwości połączenia z klastrem, uruchom następujące polecenie z klienta:

```bash
dcos package install marathon-lb
```

To polecenie automatycznie instaluje moduł równoważenia obciążenia w klastrze agentów publicznych.

## <a name="deploy-a-load-balanced-web-application"></a>Wdrażanie aplikacji sieci Web z równoważeniem obciążenia
Teraz po przygotowaniu pakietu marathon-lb możemy wdrożyć kontener aplikacji, dla którego chcemy zrównoważyć obciążenie. W tym przykładzie wdrożymy prosty serwer sieci Web, używając następującej konfiguracji:

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

* Ustaw wartość `HAProxy_0_VHOST` na nazwę FQDN modułu równoważenia obciążenia dla agentów. Ma ona postać `<acsName>agents.<region>.cloudapp.azure.com`. Jeśli na przykład utworzono klaster usługi Container Service o nazwie `myacs` w regionie `West US`, nazwa FQDN będzie następująca: `myacsagents.westus.cloudapp.azure.com`. Możesz również znaleźć tę wartość, wyszukując moduł równoważenia obciążenia z wyrazem „agent” w nazwie podczas przeszukiwania zasobów w grupie zasobów utworzonej dla usługi Container Service w [witrynie Azure Portal](https://portal.azure.com).
* Ustaw wartość servicePort na port > = 10 000. W ten sposób można zidentyfikować usługę uruchamianą w tym kontenerze. Moduł marathon-lb używa tej wartości w celu identyfikowania usług do zrównoważenia.
* Ustaw etykietę `HAPROXY_GROUP` na wartość „external”.
* Ustaw parametr `hostPort` na wartość 0. Ten sposób oznacza, że rozwiązanie Marathon będzie arbitralnie przypisywać dostępny port.
* Ustaw parametr `instances` na liczbę wystąpień, które chcesz utworzyć. W późniejszym czasie możesz zawsze zmienić tę wartość.

Warto zauważyć, że domyślnie narzędzie Marathon będzie wdrażać rozwiązania do klastra prywatnego. Oznacza to, że powyższe wdrożenie będzie dostępne tylko poprzez moduł równoważenia obciążenia, co jest zazwyczaj pożądanym zachowaniem.

### <a name="deploy-using-the-dcos-web-ui"></a>Wdrażanie przy użyciu interfejsu użytkownika w sieci Web rozwiązania DC/OS
1. Odwiedź stronę Marathon pod adresem http://localhost/marathon (po skonfigurowaniu [tunelu SSH](container-service-connect.md)) i kliknij pozycję `Create Appliction`.
2. W oknie dialogowym `New Application` kliknij pozycję `JSON Mode` w prawym górnym rogu.
3. Wklej powyższy kod JSON do edytora.
4. Kliknij pozycję `Create Appliction`.

### <a name="deploy-using-the-dcos-cli"></a>Wdrażanie przy użyciu interfejsu wiersza polecenia rozwiązania DC/OS
Aby wdrożyć tę aplikację przy użyciu interfejsu wiersza polecenia rozwiązania DC/OS, po prostu skopiuj powyższy kod JSON do pliku o nazwie `hello-web.json` i uruchom polecenie:

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Azure Load Balancer
Domyślnie moduł Azure Load Balancer udostępnia porty 80, 8080 i 443. Jeśli używasz jednego z tych trzech portów (tak jak w powyższym przykładzie), nie musisz wykonywać żadnych działań. Powinieneś być w stanie osiągnąć nazwę FQDN modułu równoważenia obciążenia agenta, a przy każdym odświeżeniu trafisz po kolei na jeden ze swoich trzech serwerów sieci Web. Jeśli jednak korzystasz z innego portu, musisz dodać regułę działania okrężnego oraz sondę w module równoważenia obciążenia dla używanego portu. Można to zrobić w [interfejsie wiersza polecenia Azure](../xplat-cli-azure-resource-manager.md), używając poleceń `azure network lb rule create` i `azure network lb probe create`. Można to zrobić również za pośrednictwem witryny Azure Portal.

## <a name="additional-scenarios"></a>Dodatkowe scenariusze
Może wystąpić scenariusz, w którym do udostępniania różnych usług są stosowane różne domeny. Na przykład:

moja_domena1.com -> Azure LB:80 -> marathon-lb:10001 -> mój_kontener1:33292  
moja_domena2.com -> Azure LB:80 -> marathon-lb:10002 -> mój_kontener2:22321

W tym celu zapoznaj się z funkcją [hostów wirtualnych](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), która umożliwia kojarzenie domen z określonymi ścieżkami warstwy marathon-lb.

Możesz również udostępniać inne porty i ponownie mapować je na prawidłowe usługi poza warstwą marathon-lb. Na przykład:

Azure lb:80 -> marathon-lb:10001 -> mój_konenter:233423  
Azure lb:8080 -> marathon-lb:1002 -> mój_kontener2:33432

## <a name="next-steps"></a>Następne kroki
Więcej informacji na temat warstwy [marathon-lb](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/) można znaleźć w dokumentacji rozwiązania DC/OS.




<!--HONumber=Nov16_HO2-->


