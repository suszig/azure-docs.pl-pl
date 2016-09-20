<properties
   pageTitle="Zarządzanie kontenerem usługi kontenera platformy Azure przy użyciu interfejsu API REST | Microsoft Azure"
   description="Wdrażanie kontenerów do klastra Mesos usługi kontenera platformy Azure przy użyciu interfejsu API REST platformy Marathon."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontenery, mikrousługi, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/16/2016"
   ms.author="nepeters"/>

# Zarządzenie kontenerem przy użyciu interfejsu API REST

Platforma DC/OS dostarcza środowisko wdrażania i skalowania obciążeń klastrowanych, zapewniając jednocześnie abstrakcyjność sprzętu bazowego. Ponad systemem DC/OS istnieje platforma, która zarządza planowaniem i wykonywaniem obciążeń obliczeniowych.

Platformy są dostępne dla wielu popularnych zadań. W tym dokumencie opisano, jak tworzyć i skalować wdrożenia kontenerów przy użyciu platformy Marathon. Przed przystąpieniem do pracy nad tymi przykładami będziesz potrzebować klastra DC/OS skonfigurowanego w usłudze kontenera platformy Azure. Potrzebna będzie także zdalna łączność z tym klastrem. Aby uzyskać więcej informacji na temat tych elementów, zobacz następujące artykuły:

- [Wdrażanie klastra usługi kontenera platformy Azure](container-service-deployment.md)
- [Łączenie z klastrem usługi kontenera platformy Azure](container-service-connect.md)

Po nawiązaniu połączeniu z klastrem usługi kontenera platformy Azure masz dostęp do platformy DC/OS i powiązanych interfejsów API REST pod adresem http://localhost:local-port. W przykładach przedstawionych w tym dokumencie założono, że tunelowanie korzysta z portu 80. Na przykład dostęp do punktu końcowego platformy Marathon można uzyskać pod adresem `http://localhost/marathon/v2/`. Aby uzyskać więcej informacji o różnych interfejsach API, zobacz dokumentację Mesosphere dotyczącą [interfejsu API platformy Marathon](https://mesosphere.github.io/marathon/docs/rest-api.html) i [interfejsu API programu Chronos](https://mesos.github.io/chronos/docs/api.html) oraz dokumentację Apache dotyczącą [interfejsu API aplikacji Mesos Scheduler](http://mesos.apache.org/documentation/latest/scheduler-http-api/).

## Gromadzenie informacji z platform DC/OS i Marathon

Przed wdrożeniem kontenerów do klastra DC/OS zbierz określone informacje o klastrze DC/OS, takie jak nazwy i bieżący stan agentów DC/OS. W tym celu wykonaj zapytanie w punkcie końcowym `master/slaves` interfejsu API REST platformy DC/OS. Jeśli operacja zostanie wykonana pomyślnie, wyświetlona zostanie lista agentów DC/OS i szereg właściwości każdego z nich.

```bash
curl http://localhost/mesos/master/slaves
```

Teraz użyj punktu końcowego `/apps` platformy Marathon, aby sprawdzić bieżące wdrożenia aplikacji w klastrze DC/OS. Jeśli jest to nowy klaster, pojawi się pusta tablica aplikacji.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## Wdrażanie kontenera w formacie programu Docker

Kontenery w formacie programu Docker można wdrażać za pośrednictwem platformy Marathon przy użyciu pliku JSON, który opisuje zamierzone wdrożenie. W poniższym przykładzie zostanie wdrożony kontener Nginx, który powiąże port 80 agenta DC/OS z portem 80 kontenera. Warto zauważyć, że właściwość „acceptedResourceRoles” jest ustawiona na wartość „slave_public”. Spowoduje to wdrożenie kontenera w agencie w zestawie skali agenta publicznego.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

W celu wdrożenia kontenera w formacie programu Docker utwórz własny plik JSON lub użyj przykładu dostępnego w [wersji demonstracyjnej usługi kontenera platformy Azure](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Zapisz go w dostępnej lokalizacji. Następnie w celu wdrożenia kontenera uruchom następujące polecenie. Określ nazwę pliku JSON.

```
# deploy container

curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Dane wyjściowe będą mieć postać podobną do następującej:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Teraz po wykonaniu zapytania dotyczącego aplikacji na platformie Marathon nowa aplikacja pojawi się w danych wyjściowych.

```
curl localhost/marathon/v2/apps
```

## Skalowanie kontenerów

Interfejs API platformy Marathon umożliwia także skalowanie w poziomie oraz skalowanie na zewnątrz wdrożeń aplikacji. W poprzednim przykładzie wdrożono jedno wystąpienie aplikacji. Wykonamy teraz skalowanie w poziomie, aby uzyskać trzy wystąpienia aplikacji. W tym celu utwórz plik JSON zawierający następujący tekst JSON i zapisz go w dostępnej lokalizacji.

```json
{ "instances": 3 }
```

Uruchom poniższe polecenie, aby skalować aplikację w poziomie.

>[AZURE.NOTE] Identyfikator URI będzie mieć postać http://localhost/marathon/v2/apps/ z dołączonym identyfikatorem aplikacji do skalowania. W przypadku użycia przedstawionej tutaj przykładowej aplikacji Nginx identyfikator URI będzie mieć postać http://localhost/marathon/v2/apps/nginx.

```json
# scale container

curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Na koniec wykonaj zapytanie dotyczące aplikacji w punkcie końcowym platformy Marathon. Widoczne będą trzy kontenery Nginx.

```
curl localhost/marathon/v2/apps
```

## Użycie programu PowerShell w tym ćwiczeniu: interakcja interfejsu API REST platformy Marathon z programem PowerShell

Te same akcje można wykonać za pomocą poleceń programu PowerShell w systemie Windows.

Aby zebrać informacje dotyczące klastra DC/OS, takie jak nazwy i stan agenta, uruchom następujące polecenie.

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Kontenery w formacie programu Docker można wdrażać za pośrednictwem platformy Marathon przy użyciu pliku JSON, który opisuje zamierzone wdrożenie. W poniższym przykładzie zostanie wdrożony kontener Nginx, który powiąże port 80 agenta DC/OS z portem 80 kontenera.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Utwórz własny plik JSON lub użyj przykładu dostępnego w [wersji demonstracyjnej usługi kontenera platformy Azure](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Zapisz go w dostępnej lokalizacji. Następnie w celu wdrożenia kontenera uruchom następujące polecenie. Określ nazwę pliku JSON.

```powershell
# deploy container

Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Interfejs API platformy Marathon umożliwia także skalowanie w poziomie oraz skalowanie na zewnątrz wdrożeń aplikacji. W poprzednim przykładzie wdrożono jedno wystąpienie aplikacji. Wykonamy teraz skalowanie w poziomie, aby uzyskać trzy wystąpienia aplikacji. W tym celu utwórz plik JSON zawierający następujący tekst JSON i zapisz go w dostępnej lokalizacji.

```json
{ "instances": 3 }
```

Uruchom poniższe polecenie, aby skalować aplikację w poziomie.

> [AZURE.NOTE] Identyfikator URI będzie mieć postać http://localhost/marathon/v2/apps/ z dołączonym identyfikatorem aplikacji do skalowania. W przypadku użycia przedstawionej tutaj przykładowej aplikacji Nginx, identyfikator URI będzie mieć postać http://localhost/marathon/v2/apps/nginx.

```powershell
# scale container

Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## Następne kroki

- [Dowiedz się więcej o punktach końcowych HTTP platformy Mesos]( http://mesos.apache.org/documentation/latest/endpoints/).
- [Dowiedz się więcej o interfejsie API REST platformy Marathon]( https://mesosphere.github.io/marathon/docs/rest-api.html).



<!--HONumber=sep16_HO1-->


