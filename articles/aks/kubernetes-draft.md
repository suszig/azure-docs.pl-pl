---
title: "Projekt za pomocą AKS i rejestru kontenera platformy Azure"
description: "Projekt za pomocą AKS i rejestru kontenera platformy Azure"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: df5614d8a708b49ee1368c4d7983f45d29920fd8
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="use-draft-with-azure-container-service-aks"></a>Projekt za pomocą usługi kontenera platformy Azure (AKS)

Projekt jest narzędziem open source, które pomaga pakietu i uruchamianie kodu w klastrze Kubernetes. Projekt jest przeznaczona dla rozwoju cyklu iteracji; ponieważ kod jest obecnie opracowywane, ale przed zatwierdzeniem do systemu kontroli wersji. Z wersji próbnej można szybko wdrożyć ponownie aplikację Kubernetes wystąpienia zmian kodu. Aby uzyskać więcej informacji na temat projektu, zobacz [projekt dokumentacji w witrynie Github](https://github.com/Azure/draft/tree/master/docs).

Szczegóły tego dokumentu z klastrem Kubernetes na AKS przy użyciu wersji roboczej.

## <a name="prerequisites"></a>Wymagania wstępne

W krokach szczegółowo opisanych w tym dokumencie założono, że klaster usługi AKS został utworzony i że zostało nawiązane połączenie kubectl z klastrem. Jeśli potrzebujesz tych elementów, zobacz [szybkiego startu AKS](./kubernetes-walkthrough.md).

Należy również prywatnej rejestru Docker w rejestrze kontenera platformy Azure (ACR). Aby uzyskać instrukcje na temat wdrażania wystąpienia ACR, zobacz [Szybki Start Azure kontenera rejestru](../container-registry/container-registry-get-started-azure-cli.md).

## <a name="install-helm"></a>Zainstaluj Helm

Helm interfejsu wiersza polecenia jest klient, który działa w systemie deweloperskim i umożliwia uruchamianie, zatrzymywanie i zarządzania aplikacjami z wykresami Helm.

Aby zainstalować Helm interfejsu wiersza polecenia na komputerze Mac, należy użyć `brew`. Aby uzyskać dodatkowe opcje instalacji, zobacz [instalowanie Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md).

```console
brew install kubernetes-helm
```

Dane wyjściowe:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.6.2: 50 files, 132.4MB
```

## <a name="install-draft"></a>Zainstaluj wersję roboczą

Projekt interfejsu wiersza polecenia jest klient, który działa w systemie deweloperskim i pozwala na quicky wdrożenie w klastrze Kubernetes kodu.

Aby zainstalować projektu interfejsu wiersza polecenia przy użyciu Mac `brew`. Aby uzyskać dodatkowe opcje instalacji Zobacz, [zainstalować wersję roboczą przewodnik](https://github.com/Azure/draft/blob/master/docs/install.md).

```console
brew install draft
```

Dane wyjściowe:

```
==> Installing draft from azure/draft
==> Downloading https://azuredraft.blob.core.windows.net/draft/draft-v0.7.0-darwin-amd64.tar.gz
Already downloaded: /Users/neilpeterson/Library/Caches/Homebrew/draft-0.7.0.tar.gz
==> /usr/local/Cellar/draft/0.7.0/bin/draft init --client-only
🍺  /usr/local/Cellar/draft/0.7.0: 6 files, 61.2MB, built in 1 second
```

## <a name="configure-draft"></a>Konfigurowanie projektu

Podczas konfigurowania wersji roboczej, należy określić rejestru kontenera. W tym przykładzie jest używany rejestru kontenera platformy Azure.

Uruchom następujące polecenie, aby pobrać nazwy i nazwy serwera logowania wystąpienia ACR. Zaktualizuj polecenia o nazwie grupę zasobów, zawierającą wystąpienia ACR.

```console
az acr list --resource-group <resource group> --query "[].{Name:name,LoginServer:loginServer}" --output table
```

Również jest potrzebne hasło wystąpienia ACR.

Uruchom następujące polecenie, aby zwrócić hasła ACR. Zaktualizuj polecenia o nazwie wystąpienia ACR.

```console
az acr credential show --name <acr name> --query "passwords[0].value" --output table
```

Inicjowanie projektu z `draft init` polecenia.

```console
draft init
```

W trakcie tego procesu zostanie wyświetlony monit o poświadczenia rejestru kontenera. Korzystając z rejestru kontenera platformy Azure, adres URL rejestru jest nazwa ACR logowania serwera, nazwa użytkownika to nazwa wystąpienia ACR i hasło jest hasło ACR.

```console
1. Enter your Docker registry URL (e.g. docker.io/myuser, quay.io/myuser, myregistry.azurecr.io): <ACR Login Server>
2. Enter your username: <ACR Name>
3. Enter your password: <ACR Password>
```

Po zakończeniu projektu jest skonfigurowany w klastrze Kubernetes i jest gotowe do użycia.

```
Draft has been installed into your Kubernetes Cluster.
Happy Sailing!
```

## <a name="run-an-application"></a>Uruchamianie aplikacji

Repozytorium projekt obejmuje kilka przykładowe aplikacje, które mogą służyć do pokaz projektu. Utwórz kopię sklonowanego repozytorium.

```console
git clone https://github.com/Azure/draft
```

Przejdź do katalogu przykłady Java.

```console
cd draft/examples/java/
```

Użyj `draft create` polecenie, aby rozpocząć proces. To polecenie tworzy artefaktów, które są używane do uruchamiania aplikacji w klastrze Kubernetes. Elementy te obejmują plik Dockerfile, wykres Helm i `draft.toml` pliku, który jest plikiem konfiguracji projektu.

```console
draft create
```

Dane wyjściowe:

```
--> Draft detected the primary language as Java with 92.205567% certainty.
--> Ready to sail
```

Aby uruchomić aplikację w klastrze Kubernetes, należy użyć `draft up` polecenia. To polecenie operacji przekazywania plików kodu i konfiguracji aplikacji do klastra Kubernetes. Następnie uruchamia plik Dockerfile, aby utworzyć obraz kontenera, wypychanie obrazu do rejestru kontenera i uruchamia finally wykres Helm, aby uruchomić aplikację.

```console
draft up
```

Dane wyjściowe:

```
Draft Up Started: 'open-jaguar'
open-jaguar: Building Docker Image: SUCCESS ⚓  (28.0342s)
open-jaguar: Pushing Docker Image: SUCCESS ⚓  (7.0647s)
open-jaguar: Releasing Application: SUCCESS ⚓  (4.5056s)
open-jaguar: Build ID: 01BW3VVNZYQ5NQ8V1QSDGNVD0S
```

## <a name="test-the-application"></a>Testowanie aplikacji

Aby przetestować aplikację, należy użyć `draft connect` polecenia. To polecenie proxy połączenie z pod Kubernetes, dzięki czemu bezpiecznego połączenia lokalnego. Po zakończeniu aplikacji są dostępne na podany adres URL.

W niektórych przypadkach może potrwać kilka minut, aż do pobrania obrazu kontenera i aplikację do uruchomienia. Jeśli wystąpi błąd podczas uzyskiwania dostępu do aplikacji, ponów próbę połączenia.

```console
draft connect
```

Dane wyjściowe:

```
Connecting to your app...SUCCESS...Connect to your app on localhost:46143
Starting log streaming...
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
== Spark has ignited ...
>> Listening on 0.0.0.0:4567
```

Po zakończeniu badania użycia aplikacji `Control+C` zatrzymania połączenia serwera proxy.

## <a name="expose-application"></a>Udostępnianie aplikacji

Podczas testowania aplikacji w Kubernetes, można udostępnić aplikacje w Internecie. Można to zrobić przy użyciu usługi Kubernetes z typem [usługi równoważenia obciążenia](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer) lub [kontrolera wejściowych](https://kubernetes.io/docs/concepts/services-networking/ingress/). Szczegóły tego dokumentu przy użyciu usługi Kubernetes.


Najpierw projekt pakietu musi zostać zaktualizowany, aby określić, że usługa z typem `LoadBalancer` powinien zostać utworzony. Aby to zrobić, typ usługi, w aktualizacji `values.yaml` pliku.

```console
vi chart/java/values.yaml
```

Zlokalizuj `service.type` właściwości i zaktualizuj wartość z `ClusterIP` do `LoadBalancer`.

```yaml
replicaCount: 2
image:
  repository: openjdk
  tag: 8-jdk-alpine
  pullPolicy: IfNotPresent
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
resources:
  limits:
    cpu: 100m
    memory: 128Mi
  requests:
    cpu: 100m
    memory: 128Mi
  ```

Uruchom `draft up` ponowne uruchomienie aplikacji.

```console
draft up
```

Może upłynąć kilka minut, aż usługi powrócić do publicznego adresu IP. Aby monitorować postęp użyj `kubectl get service` z czujki.

```console
kubectl get service -w
```

Początkowo *IP zewnętrznego* dla usługi pojawia się jako `pending`.

```
deadly-squid-java   10.0.141.72   <pending>     80:32150/TCP   14m
```

Gdy adres IP zewnętrznego zmienił się z `pending` do `IP address`, użyj `Control+C` można zatrzymać procesu czujki kubectl.

```
deadly-squid-java   10.0.141.72   52.175.224.118   80:32150/TCP   17m
```

Aby wyświetlić aplikację, przejdź do zewnętrznego adresu IP.

```console
curl 52.175.224.118
```

Dane wyjściowe:

```
Hello World, I'm Java
```

## <a name="iterate-on-the-application"></a>Iterowanie w aplikacji

Projekt został skonfigurowany, a aplikacja jest uruchomiona w Kubernetes, są ustawiane dla kodu iteracji. Kod zawsze chcesz przetestować zaktualizowane, uruchom `draft up` polecenie, aby zaktualizować działającej aplikacji.

Na przykład aktualizacji aplikacji Java hello world.

```console
vi src/main/java/helloworld/Hello.java
```

Aktualizowanie tekstu Hello World.

```java
package helloworld;

import static spark.Spark.*;

public class Hello {
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java - Draft Rocks!");
    }
}
```

Uruchom `draft up` polecenie, aby ponownie wdrożyć aplikację.

```console
draft up
```

Dane wyjściowe

```
Draft Up Started: 'deadly-squid'
deadly-squid: Building Docker Image: SUCCESS ⚓  (18.0813s)
deadly-squid: Pushing Docker Image: SUCCESS ⚓  (7.9394s)
deadly-squid: Releasing Application: SUCCESS ⚓  (6.5005s)
deadly-squid: Build ID: 01BWK8C8X922F5C0HCQ8FT12RR
```

Na koniec Wyświetl aplikację, aby zobaczyć aktualizacje.

```console
curl 52.175.224.118
```

Dane wyjściowe:

```
Hello World, I'm Java - Draft Rocks!
```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o korzystaniu z wersji próbnej zobacz dokumentację projekt w witrynie GitHub.

> [!div class="nextstepaction"]
> [Dokumentacja projektu](https://github.com/Azure/draft/tree/master/docs)