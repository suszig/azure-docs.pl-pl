<properties
   pageTitle="Równoważenie obciążenia klastra usługi kontenera platformy Azure | Microsoft Azure"
   description="Równoważenie obciążenia klastra usługi kontenera platformy Azure."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Kontenery, mikrousługi, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>

# Równoważenie obciążenia klastra usługi kontenera platformy Azure

W tym artykule skonfigurujemy fronton sieci web w usłudze Azure Container Service zarządzanej przez rozwiązanie DC/OS. Skonfigurujemy także usługę Marathon-LB, aby umożliwić skalowanie aplikacji w górę.

## Wymagania wstępne

[Wdróż wystąpienie usługi Azure Container Service](container-service-deployment.md) z typem aranżacji DCOS i [upewnij się, że klient może połączyć się z klastrem](container-service-connect.md). 

## Równoważenie obciążenia

Istnieją dwie warstwy równoważenia obciążenia w klastrze usługi Container Service, które utworzymy: 

  1. Usługa Azure Load Balancer zapewnia publiczne punkty wejścia (z którymi kontaktują się użytkownicy końcowi). Jest ona dostarczana automatycznie przez usługę Azure Container Service i domyślnie skonfigurowana do udostępniania portów 80, 443 i 8080.
  2. Usługa Marathon Load Balancer (marathon-lb) kieruje żądania przychodzące do wystąpień kontenera, które obsługują te żądania. Warstwa marathon-lb jest dynamicznie dostosowywana w przypadku skalowania kontenerów świadczących usługę sieci Web. Moduł równoważenia obciążenia nie jest zapewniany domyślnie w usłudze kontenera, ale można go łatwo zainstalować.

## Marathon Load Balancer

Moduł Marathon Load Balancer dynamicznie zmienia konfigurację na podstawie wdrożonych kontenerów. Jest on również odporny na utratę kontenera lub agenta. W takim przypadku rozwiązanie Apache Mesos po prostu ponownie uruchomi kontener w innym miejscu, a moduł marathon-lb automatycznie dostosuje się.

Do instalacji modułu Marathon Load Balancer można użyć interfejsu użytkownika w sieci Web lub wiersza polecenia rozwiązania DC/OS.

### Instalacja modułu Marathon-LB przy użyciu interfejsu użytkownika w sieci Web rozwiązania DC/OS

  1. Kliknij pozycję „Universe” (Wszechświat)
  2. Wyszukaj pozycję „Marathon-LB”
  3. Kliknij pozycję „Install” (Zainstaluj)

![Instalacja modułu marathon-lb przy użyciu interfejsu użytkownika w sieci Web rozwiązania DC/OS](./media/dcos/marathon-lb-install.png)

### Instalacja modułu Marathon-LB przy użyciu interfejsu wiersza polecenia rozwiązania DC/OS

Po zainstalowaniu interfejsu wiersza polecenia rozwiązania DC/OS i upewnieniu się co do możliwości połączenia z klastrem, uruchom następujące polecenie z klienta:

```bash
dcos package install marathon-lb
```

## Wdrażanie aplikacji sieci Web z równoważeniem obciążenia

Teraz, gdy mamy już pakiet marathon-lb, możemy wdrożyć prosty serwer sieci Web przy użyciu następującej konfiguracji:

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

### Wdrażanie przy użyciu interfejsu użytkownika w sieci Web rozwiązania DC/OS

  1. Odwiedź stronę rozwiązania Marathon pod adresem http://localhost/marathon (po skonfigurowaniu [tunelu SSH](container-service-connect.md) i kliknij pozycję `Create Appliction`
  2. W oknie dialogowym `New Application` kliknij pozycję `JSON Mode` w prawym górnym rogu
  3. Wklej powyższy kod JSON do edytora
  4. Kliknij pozycję `Create Appliction`

### Wdrażanie przy użyciu interfejsu wiersza polecenia rozwiązania DC/OS

Aby wdrożyć tę aplikację przy użyciu interfejsu wiersza polecenia rozwiązania DC/OS, po prostu skopiuj powyższy kod JSON do pliku o nazwie `hello-web.json` i uruchom polecenie:

```bash
dcos marathon app add hello-web.json
```

## Azure Load Balancer

Domyślnie moduł Azure Load Balancer udostępnia porty 80, 8080 i 443. Jeśli używasz jednego z tych trzech portów (tak jak w powyższym przykładzie), nie musisz wykonywać żadnych działań. Powinieneś być w stanie osiągnąć nazwę FQDN modułu równoważenia obciążenia agenta, a przy każdym odświeżeniu trafisz po kolei na jeden ze swoich trzech serwerów sieci Web. Jeśli jednak korzystasz z innego portu, musisz dodać regułę działania okrężnego oraz sondę w module równoważenia obciążenia dla używanego portu. Można to zrobić w [interfejsie wiersza polecenia Azure](../xplat-cli-azure-resource-manager.md), używając poleceń `azure lb rule create` i `azure lb probe create`. Można to zrobić również za pośrednictwem witryny Azure Portal.


## Dodatkowe scenariusze

Może wystąpić scenariusz, w którym do udostępniania różnych usług są stosowane różne domeny. Na przykład:

moja_domena1.com -> Azure LB:80 -> marathon-lb:10001 -> mój_kontener1:33292  
moja_domena2.com -> Azure LB:80 -> marathon-lb:10002 -> mój_kontener2:22321

W tym celu zapoznaj się z funkcją [hostów wirtualnych](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), która umożliwia kojarzenie domen z określonymi ścieżkami warstwy marathon-lb.

Możesz również udostępniać inne porty i ponownie mapować je na prawidłowe usługi poza warstwą marathon-lb. Na przykład:

Azure lb:80 -> marathon-lb:10001 -> mój_konenter:233423  
Azure lb:8080 -> marathon-lb:1002 -> mój_kontener2:33432


## Następne kroki

Więcej informacji na temat warstwy [marathon-lb](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/) można znaleźć w dokumentacji rozwiązania DC/OS.



<!--HONumber=sep16_HO1-->


