---
title: "Używanie narzędzia Draft z usługami Azure Container Service i Azure Container Registry | Microsoft Docs"
description: "Utwórz klaster ACS Kubernetes i rejestr Azure Container Registry, aby utworzyć swoją pierwszą aplikację na platformie Azure z użyciem narzędzia Draft."
services: container-service
documentationcenter: 
author: squillace
manager: gamonroy
editor: 
tags: draft, helm, acs, azure-container-service
keywords: Docker, Containers, microservices, Kubernetes, Draft, Azure
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: rasquill
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: dc3ae52b1ec6717c7e19a160e3e7ea5d211f1f5f
ms.contentlocale: pl-pl
ms.lasthandoff: 06/28/2017



---

<a id="use-draft-with-azure-container-service-and-azure-container-registry-to-build-and-deploy-an-application-to-kubernetes" class="xliff"></a>

# Użycie narzędzia Draft z usługami Azure Container Service i Azure Container Registry w celu utworzenia i wdrożenia aplikacji w rozwiązaniu Kubernetes

[Draft](https://aka.ms/draft) to nowe narzędzie open-source, które ułatwia programowanie aplikacji bazujących na kontenerach i wdrażanie ich w klastrach Kubernetes bez bliższej znajomości rozwiązań Docker i Kubernetes (a nawet bez ich instalacji). Narzędzia takie jak Draft pozwalają Tobie i Twoim zespołom skupiać się na tworzeniu aplikacji z użyciem rozwiązania Kubernetes bez poświęcania większej uwagi infrastrukturze.

Narzędzia Draft można użyć z dowolnym rejestrem obrazów Docker i dowolnym klastrem Kubernetes, w tym lokalnym. Ten samouczek pokazuje, jak używać usługi ACS z rozwiązaniem Kubernetes, oraz usług ACR i Azure DNS do tworzenia dynamicznego potoku deweloperskiego ciągłej integracji/ciągłego dostarczania za pomocą narzędzia Draft.


<a id="create-an-azure-container-registry" class="xliff"></a>

## Tworzenie rejestru Azure Container Registry
Możesz z łatwością [utworzyć nowy rejestr Azure Container Registry](../container-registry/container-registry-get-started-azure-cli.md), przy czym kroki są następujące:

1. Utwórz grupę zasobów Azure, aby zarządzać rejestrem ACR i klastrem Kubernetes w usłudze ACS.
      ```azurecli
      az group create --name draft --location eastus
      ```

2. Utwórz rejestr obrazów ACR za pomocą polecenia [az acr create](/cli/azure/acr#create)
      ```azurecli
      az acr create -g draft -n draftacs --sku Basic --admin-enabled true -l eastus
      ```


<a id="create-an-azure-container-service-with-kubernetes" class="xliff"></a>

## Tworzenie usługi Azure Container Service za pomocą rozwiązania Kubernetes

Teraz można już użyć polecenia [az acs create](/cli/azure/acs#create) do utworzenia klastra ACS za pomocą rozwiązania Kubernetes jako wartości `--orchestrator-type`.
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes
```

> [!NOTE]
> Ponieważ rozwiązanie Kubernetes nie jest domyślnym typem koordynatora, pamiętaj o użyciu przełącznika `--orchestrator-type kubernetes`.

Dane wyjściowe po pomyślnym wykonaniu polecenia wyglądają podobnie do następujących.

```json
waiting for AAD role to propagate.done
{
  "id": "/subscriptions/<guid>/resourceGroups/draft/providers/Microsoft.Resources/deployments/azurecli14904.93snip09",
  "name": "azurecli1496227204.9323909",
  "properties": {
    "correlationId": "<guid>",
    "debugSetting": null,
    "dependencies": [],
    "mode": "Incremental",
    "outputs": null,
    "parameters": {
      "clientSecret": {
        "type": "SecureString"
      }
    },
    "parametersLink": null,
    "providers": [
      {
        "id": null,
        "namespace": "Microsoft.ContainerService",
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              "westus"
            ],
            "properties": null,
            "resourceType": "containerServices"
          }
        ]
      }
    ],
    "provisioningState": "Succeeded",
    "template": null,
    "templateLink": null,
    "timestamp": "2017-05-31T10:46:29.434095+00:00"
  },
  "resourceGroup": "draft"
}
```

Po utworzeniu klastra można zaimportować poświadczenia za pomocą polecenia [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials). Zostanie utworzony lokalny plik konfiguracji klastra, którego narzędzia Helm i Draft wymagają do wykonania pracy.

<a id="install-and-configure-draft" class="xliff"></a>

## Instalowanie i konfigurowanie narzędzia Draft
Instrukcje instalacji narzędzia Draft można znaleźć w [repozytorium narzędzia Draft](https://github.com/Azure/draft/blob/master/docs/install.md). Są one stosunkowo proste, ale wymagają pewnych czynności konfiguracyjnych, ponieważ narzędzie to zależy od narzędzia [Helm](https://aka.ms/helm) w zakresie utworzenia i wdrożenia planu Helm w klastrze Kubernetes.

1. [Pobierz i zainstaluj narzędzie Helm](https://aka.ms/helm#install).
2. Użyj narzędzia Helm, aby wyszukać i zainstalować program `stable/traefik` oraz kontroler danych przychodzących w celu umożliwienia obsługi żądań przychodzących dla Twoich kompilacji.
    ```bash
    $ helm search traefik
    NAME            VERSION DESCRIPTION
    stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

    $ helm install stable/traefik --name ingress
    ```
    Teraz ustaw wyrażenie kontrolne na kontrolerze `ingress`, aby przechwycić wartość zewnętrznego adresu IP po jego wdrożeniu. Ten adres IP zostanie [zamapowany na domenę wdrażania](#wire-up-deployment-domain) w następnej sekcji.

    ```bash
    kubectl get svc -w
    NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
    ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
    kubernetes                    10.0.0.1       <none>          443/TCP                      7h
    ```

    W tym przypadku zewnętrzny adres IP dla domeny wdrażania to `13.64.108.240`. Teraz możesz zamapować swoją domenę na ten adres IP.

<a id="wire-up-deployment-domain" class="xliff"></a>

## Podłączanie domeny wdrażania

Narzędzie Draft tworzy wydanie dla każdego tworzonego planu Helm, czyli każdej aplikacji, nad którą pracujesz. Każde z nich otrzymuje wygenerowaną nazwę używaną przez narzędzie Draft jako _poddomena_ w kontrolowanej przez Ciebie głównej _domenie wdrażania_. (W tym przykładzie jako domeny wdrażania używamy domeny `squillace.io`). Aby włączyć takie zachowanie poddomeny, musisz utworzyć rekord A dla wartości `'*'` we wpisach usługi DNS dla swojej domeny wdrażania, aby każda wygenerowana poddomena była kierowana do kontrolera danych przychodzących rozwiązania Kubernetes.

Twój dostawca domeny ma swój własny sposób przypisywania serwerów DNS. Aby [wydelegować serwery nazw Twojej domeny do usługi Azure DNS](../dns/dns-delegate-domain-azure-dns.md), musisz wykonać następujące kroki:

1. Utwórz grupę zasobów dla swojej strefy.
    ```azurecli
    az group create --name squillace.io --location eastus
    {
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io",
      "location": "eastus",
      "managedBy": null,
      "name": "zones",
      "properties": {
        "provisioningState": "Succeeded"
      },
      "tags": null
    }
    ```

2. Utwórz strefę DNS dla swojej domeny.
Użyj polecenia [az network dns zone create](/cli/azure/network/dns/zone#create), aby uzyskać serwery nazw w celu delegowania kontroli DNS do usługi Azure DNS dla Twojej domeny.
    ```azurecli
    az network dns zone create --resource-group squillace.io --name squillace.io
    {
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/zones/providers/Microsoft.Network/dnszones/squillace.io",
      "location": "global",
      "maxNumberOfRecordSets": 5000,
      "name": "squillace.io",
      "nameServers": [
        "ns1-09.azure-dns.com.",
        "ns2-09.azure-dns.net.",
        "ns3-09.azure-dns.org.",
        "ns4-09.azure-dns.info."
      ],
      "numberOfRecordSets": 2,
      "resourceGroup": "squillace.io",
      "tags": {},
      "type": "Microsoft.Network/dnszones"
    }
    ```
3. Dodaj uzyskane serwery DNS do dostawcy domeny dla Twojej domeny wdrażania, co pozwoli używać usługi Azure DNS do dowolnego zmieniania sposobu wskazywania Twojej domeny.
4. Utwórz wpis zestawu rekordów A na potrzeby mapowania swojej domeny wdrażania na adres IP `ingress` z kroku 2 w poprzedniej sekcji.
    ```azurecli
    az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*' -g squillace.io -z squillace.io
    ```
Dane wyjściowe wyglądają podobnie do tych:
    ```json
    {
      "arecords": [
        {
          "ipv4Address": "13.64.108.240"
        }
      ],
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io/providers/Microsoft.Network/dnszones/squillace.io/A/*",
      "metadata": null,
      "name": "*",
      "resourceGroup": "squillace.io",
      "ttl": 3600,
      "type": "Microsoft.Network/dnszones/A"
    }
    ```

5. Skonfiguruj narzędzie Draft do korzystania z Twojego rejestru i tworzenia poddomen dla każdego planu Helm, które to narzędzie utworzy. Aby skonfigurować narzędzie Draft, potrzebne są:
  - nazwa rejestru Azure Container Registry (w tym przykładzie: `draft`);
  - klucz rejestru lub hasło z polecenia `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`;
  - domena główna wdrażania, która została skonfigurowana w celu zamapowania zewnętrznego adresu IP Kubernetes dla danych przychodzących (tutaj: `squillace.io`).

  Wywołaj polecenie `draft init`. Proces konfiguracji zapyta o wartości wymienione powyżej. Gdy proces uruchomisz po raz pierwszy, będzie on wyglądać podobnie do następującego.
    ```
    draft init
    Creating pack ruby...
    Creating pack node...
    Creating pack gradle...
    Creating pack maven...
    Creating pack php...
    Creating pack python...
    Creating pack dotnetcore...
    Creating pack golang...
    $DRAFT_HOME has been configured at /Users/ralphsquillace/.draft.

    In order to install Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io, quay.io, myregistry.azurecr.io): draft.azurecr.io
    2. Enter your username: draft
    3. Enter your password:
    4. Enter your org where Draft will push images [draft]: draft
    5. Enter your top-level domain for ingress (e.g. draft.example.com): squillace.io
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
    ```

Teraz możesz wdrożyć aplikację.


<a id="build-and-deploy-an-application" class="xliff"></a>

## Kompilowanie i wdrażanie aplikacji

W repozytorium narzędzia Draft znajduje się [sześć prostych, przykładowych aplikacji](https://github.com/Azure/draft/tree/master/examples). Sklonuj repozytorium i posłużmy się [przykładem w języku Python](https://github.com/Azure/draft/tree/master/examples/python). Przejdź do katalogu examples/Python i wpisz `draft create`, aby skompilować aplikację. Powinno to wyglądać podobnie jak na poniższym przykładzie.
```bash
$ draft create
--> Python app detected
--> Ready to sail
```

Dane wyjściowe obejmują plik Docker i plan Helm. W celu skompilowania i wdrożenia wystarczy wpisać polecenie `draft up`. Dane wyjściowe są obszerne, ale zaczynają się tak jak w poniższym przykładzie.
```bash
$ draft up
--> Building Dockerfile
Step 1 : FROM python:onbuild
onbuild: Pulling from library/python
10a267c67f42: Pulling fs layer
fb5937da9414: Pulling fs layer
9021b2326a1e: Pulling fs layer
dbed9b09434e: Pulling fs layer
ea8a37f15161: Pulling fs layer
<snip>
```

W przypadku powodzenia kończą się podobnie jak w poniższym przykładzie.
```bash
ab68189731eb: Pushed
53c0ab0341bee12d01be3d3c192fbd63562af7f1: digest: sha256:bb0450ec37acf67ed461c1512ef21f58a500ff9326ce3ec623ce1e4427df9765 size: 2841
--> Deploying to Kubernetes
--> Status: DEPLOYED
--> Notes:

  http://gangly-bronco.squillace.io to access your application

Watching local files for changes...
```

Bez względu na nazwę planu możesz teraz wydać polecenie `curl http://gangly-bronco.squillace.io` i uzyskać odpowiedź: `Hello World!`.

<a id="next-steps" class="xliff"></a>

## Następne kroki

Teraz, gdy już masz klaster ACS Kubernetes, możesz przyjrzeć się sposobie korzystania z usługi [Azure Container Registry](../container-registry/container-registry-intro.md), aby utworzyć więcej innych wdrożeń tego scenariusza. Na przykład możesz utworzyć zestaw rekordów usługi DNS dla domeny draft._basedomain.toplevel_, który steruje działaniem na poziomie głębszej poddomeny dla specyficznych wdrożeń usługi ACS.







