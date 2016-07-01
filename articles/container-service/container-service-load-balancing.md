<properties
   pageTitle="Równoważenie obciążenia klastra usługi kontenera platformy Azure | Microsoft Azure"
   description="Równoważenie obciążenia klastra usługi kontenera platformy Azure."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, Micro-services, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/18/2016"
   ms.author="rogardle"/>

# Równoważenie obciążenia klastra usługi kontenera platformy Azure

W tym artykule skonfigurujemy fronton sieci Web, który może być skalowany w celu świadczenia usług związanych z równoważeniem obciążenia na platformie Azure.


## Wymagania wstępne

[Wdróż wystąpienie usługi kontenera platformy Azure](container-service-deployment.md) z typem aranżacji DCOS, [upewnij się, że klient może połączyć się z klastrem](container-service-connect.md) i [AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)].


## Równoważenie obciążenia

W klastrze usługi kontenera znajdują się dwie warstwy równoważenia obciążenia: warstwa Azure LB dla publicznych punktów wejścia (do których trafiają użytkownicy końcowi) i podstawowa warstwa marathon-lb przesyłająca żądania przychodzące do wystąpień kontenera obsługujących żądania. Warstwa marathon-lb jest dynamicznie dostosowywana w przypadku skalowania kontenerów świadczących usługę.

## Marathon LB 

Rozwiązanie Marathon LB będzie dynamicznie zmieniać swoją konfigurację w oparciu o wdrożone kontenery. Jest ono również odporne na utratę kontenera lub agenta. W takim przypadku rozwiązanie Mesos po prostu ponownie uruchomi kontener w innym miejscu i ponownie skonfiguruje warstwę Marathon LB. 

Aby zainstalować warstwę Marathon LB, uruchom następujące polecenie na komputerze klienckim:

```bash
dcos package install marathon-lb 
``` 

Teraz, gdy mamy już pakiet marathon-lb, możemy wdrożyć prosty serwer sieci Web przy użyciu następującej konfiguracji:


```json
{ 
  "id": "web", 
  "container": { 
    "type": "DOCKER", 
    "docker": { 
      "image": "tutum/hello-world", 
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

Części klucza w tym przypadku to: 
  * Ustaw wartość HAProxy_0_VHOST na nazwę FQDN modułu równoważenia obciążenia dla agentów. Ma ona następującą postać: `<acsName>agents.<region>.cloudapp.azure.com`. Jeśli na przykład utworzono klaster usługi kontenera o nazwie `myacs` w regionie `West US`, nazwa FQDN będzie następująca: `myacsagents.westus.cloudapp.azure.com`. Możesz również znaleźć tę wartość, wyszukując moduł równoważenia obciążenia z wyrazem „agent” w nazwie podczas przeszukiwania zasobów w grupie zasobów utworzonej dla usługi kontenera w [portalu Azure](https://portal.azure.com).
  * Ustaw wartość servicePort na port > = 10 000. W ten sposób można zidentyfikować usługę uruchamianą w tym kontenerze. Warstwa marathon-lb używa tej wartości w celu identyfikowania usług do zrównoważenia.
  * Ustaw etykietę HAPROXY_GROUP na wartość „external”.
  * Ustaw opcję hostPort na wartość 0. Ten sposób oznacza, że rozwiązanie marathon będzie arbitralnie przypisywać dostępny port.

Skopiuj dane JSON do pliku o nazwie `hello-web.json` i użyj go do wdrożenia kontenera: 

```bash
dcos marathon app add hello-web.json 
``` 

## Azure LB 

Domyślnie warstwa Azure LB udostępnia porty 80, 8080 i 443. Jeśli używasz jednego z tych trzech portów (tak jak w powyższym przykładzie), nie musisz wykonywać kolejnych czynności: trafienie nazwy FQDN warstwy LB agenta powinno być możliwe, a po każdym odświeżeniu nastąpi trafienie jednego z trzech serwerów sieci Web w ramach działania okrężnego. Jeśli jednak korzystasz z innego portu, musisz dodać regułę działania okrężnego oraz sondę w warstwie Azure LB dla używanego portu. Tę czynność można wykonać z poziomu [interfejsu wiersza polecenia XPLAT platformy Azure](../xplat-cli-azure-resource-manager.md) za pomocą poleceń `azure lb rule create` i `azure lb probe create`.


## Dodatkowe scenariusze

Może wystąpić scenariusz, w którym do udostępniania różnych usług są stosowane różne domeny. Na przykład: 

moja_domena1.com -> Azure LB:80 -> marathon-lb:10001 -> mój_kontener1:33292  
moja_domena2.com -> Azure LB:80 -> marathon-lb:10002 -> mój_kontener2:22321 

W tym celu zapoznaj się z funkcją [hostów wirtualnych](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), która umożliwia kojarzenie domen z określonymi ścieżkami warstwy marathon-lb.

Możesz również udostępniać inne porty i ponownie mapować je na prawidłowe usługi poza warstwą marathon lb. Na przykład:

Azure lb:80 -> marathon-lb:10001 -> mój_konenter:233423  
Azure lb:8080 -> marathon-lb:1002 -> mój_kontener2:33432 
 

## Następne kroki

Zapoznaj się z [tym wpisem w blogu](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), aby uzyskać więcej informacji na temat warstwy Marathon LB.



<!--HONumber=Jun16_HO2-->


