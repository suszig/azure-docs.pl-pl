---
title: "Wdrażanie aplikacji Spring rozruch przy użyciu wtyczki Maven Fabric8"
description: "Ten samouczek przedstawia jednak kroki, aby wdrożyć aplikację Spring rozruchu w systemie Microsoft Azure przy użyciu wtyczki Fabric8 Apache Maven."
services: container-service
documentationcenter: java
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 09/15/2017
ms.author: yuwzho;robmcm
ms.custom: 
ms.openlocfilehash: 6ec48caef0eebeacf260b0dbc91b175368faea69
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="deploy-a-spring-boot-app-using-the-fabric8-maven-plugin"></a>Wdrażanie aplikacji Spring rozruch przy użyciu wtyczki Maven Fabric8

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

**[Fabric8]**  to rozwiązanie open source, które jest oparty na  **[Kubernetes]**, co pomaga deweloperom tworzenie aplikacji w kontenerach systemu Linux.

Ten samouczek przedstawia przy użyciu wtyczki Fabric8 przypadku Maven do rozwoju do wdrożenia aplikacji na hoście z systemem Linux w [usługi kontenera platformy Azure (ACS)].

## <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać kroki opisane w tym samouczku, musisz mieć następujące wymagania wstępne:

* Subskrypcja platformy Azure; Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz przeprowadzić aktywację Twojej [korzyści dla subskrybentów MSDN] lub zarejestrować się w celu [bezpłatne konto platformy Azure].
* [Azure interfejsu wiersza polecenia (CLI)].
* Aktualne [Java Developer Kit (JDK)].
* Apache na [Maven] (w wersji 3) Narzędzie kompilacji.
* A [Git] klienta.
* A [Docker] klienta.

> [!NOTE]
>
> Ze względu na wymagania dotyczące wirtualizacji w tym samouczku nie można wykonać kroki opisane w tym artykule na maszynie wirtualnej; należy użyć komputera fizycznego z włączonymi funkcjami wirtualizacji.
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>Utwórz rozruchu Spring na wprowadzenie Docker aplikacji sieci web

W poniższych krokach objaśniono sposób tworzenia aplikacji sieci web Spring rozruchu i testowanie go lokalnie.

1. Otwórz wiersz polecenia i utworzyć katalogu lokalnego do przechowywania aplikacji, przejdź do tego katalogu; na przykład:
   ```shell
   md /home/GenaSoto/SpringBoot
   cd /home/GenaSoto/SpringBoot
   ```
   --lub--
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```

1. Klonowanie [rozruchu Spring na wprowadzenie Docker] przykładowy projekt do katalogu.
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Zmień katalog na ukończone projektu; na przykład:
   ```shell
   cd gs-spring-boot-docker/complete
   ```
   --lub--
   ```shell
   cd gs-spring-boot-docker\complete
   ```

1. Używanie programu Maven do tworzenia i uruchamianie przykładowej aplikacji.
   ```shell
   mvn clean package spring-boot:run
   ```

1. Testowanie aplikacji sieci web, przechodząc do adresem http://localhost: 8080 lub z następującymi `curl` polecenia:
   ```shell
   curl http://localhost:8080
   ```

   Powinny pojawić się **Hello, World Docker** komunikat wyświetlany.

   ![Przejdź do przykładowej aplikacji lokalnie][SB01]


## <a name="install-the-kubernetes-command-line-interface-and-create-an-azure-resource-group-using-the-azure-cli"></a>Instalowanie interfejsu wiersza polecenia Kubernetes i tworzenie grupy zasobów platformy Azure przy użyciu wiersza polecenia platformy Azure

1. Otwórz wiersz polecenia.

1. Wpisz następujące polecenie, aby zalogować się do konta platformy Azure:
   ```azurecli
   az login
   ```
   Postępuj zgodnie z instrukcjami, aby ukończyć proces logowania
   
   Azure CLI spowoduje wyświetlenie listy kont; na przykład:

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "00000000-0000-0000-0000-000000000000",
       "isDefault": false,
       "name": "Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "00000000-0000-0000-0000-000000000000",
       "user": {
         "name": "Gena.Soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```

1. Jeśli nie masz już Kubernetes interfejsu wiersza polecenia (`kubectl`) zainstalowana, można zainstalować przy użyciu wiersza polecenia platformy Azure; na przykład:
   ```azurecli
   az acs kubernetes install-cli
   ```

   > [!NOTE]
   >
   > Linux użytkownicy mogą mieć do prefiksu to polecenie z `sudo` ponieważ wdrażania Kubernetes CLI `/usr/local/bin`.
   >
   > Jeśli masz już `kubectl`) zainstalowany i używana wersja `kubectl` jest za stary, zobaczysz komunikat o błędzie podobny do poniższego przykładu przy próbie wykonać wszystkie czynności opisane w dalszej części tego artykułu:
   >
   > ```
   > error: group map[autoscaling:0x0000000000 batch:0x0000000000 certificates.k8s.io
   > :0x0000000000 extensions:0x0000000000 storage.k8s.io:0x0000000000 apps:0x0000000
   > 000 authentication.k8s.io:0x0000000000 policy:0x0000000000 rbac.authorization.k8
   > s.io:0x0000000000 federation:0x0000000000 authorization.k8s.io:0x0000000000 comp
   > onentconfig:0x0000000000] is already registered
   > ```
   >
   > W takim przypadku należy ponownie zainstalować `kubectl` do zaktualizowania wersji.
   >

1. Utwórz grupę zasobów dla zasobów platformy Azure, które będą używane w tym samouczku; na przykład:
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=westeurope
   ```
   Gdzie:  
      * *wingtiptoys kubernetes* jest unikatową nazwę grupy zasobów  
      * *westeurope* jest odpowiednią lokalizację geograficzną dla aplikacji  

   Azure CLI wyświetli wyniki z Tworzenie grupy zasobów; na przykład:  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes",
     "location": "westeurope",
     "managedBy": null,
     "name": "wingtiptoys-kubernetes",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```


## <a name="create-a-kubernetes-cluster-using-the-azure-cli"></a>Tworzenie klastra Kubernetes przy użyciu wiersza polecenia platformy Azure

1. Tworzenie klastra Kubernetes w nowej grupy zasobów; na przykład:  
   ```azurecli 
   az acs create --orchestrator-type kubernetes --resource-group wingtiptoys-kubernetes --name wingtiptoys-cluster --generate-ssh-keys --dns-prefix=wingtiptoys
   ```
   Gdzie:  
      * *wingtiptoys kubernetes* to nazwa grupy zasobów z wcześniej w tym artykule  
      * *klaster wingtiptoys* jest unikatową nazwę klastra Kubernetes
      * *wingtiptoys* jest nazwą DNS unikatową nazwę aplikacji

   Azure CLI wyświetli wyniki z tworzenia klastra; na przykład:  

   ```json
   {
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.Resources/deployments/azurecli0000000000.00000000000",
     "name": "azurecli0000000000.00000000000",
     "properties": {
       "correlationId": "00000000-0000-0000-0000-000000000000",
       "debugSetting": null,
       "dependencies": [],
       "mode": "Incremental",
       "outputs": {
         "masterFQDN": {
           "type": "String",
           "value": "wingtiptoysmgmt.westeurope.cloudapp.azure.com"
         },
         "sshMaster0": {
           "type": "String",
           "value": "ssh azureuser@wingtiptoysmgmt.westeurope.cloudapp.azure.com -A -p 22"
         }
       },
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
                 "westeurope"
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
       "timestamp": "2017-09-15T01:00:00.000000+00:00"
     },
     "resourceGroup": "wingtiptoys-kubernetes"
   }
   ```

1. Pobieranie poświadczeń dla nowego klastra Kubernetes; na przykład:  
   ```azurecli 
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes --name wingtiptoys-cluster
   ```

1. Sprawdź połączenie przy użyciu następującego polecenia:
   ```shell 
   kubectl get nodes
   ```

   Należy wyświetlić listę węzłów i Stany jak w następującym przykładzie:

   ```shell
   NAME                    STATUS                     AGE       VERSION
   k8s-agent-00000000-0    Ready                      5h        v1.6.6
   k8s-agent-00000000-1    Ready                      5h        v1.6.6
   k8s-agent-00000000-2    Ready                      5h        v1.6.6
   k8s-master-00000000-0   Ready,SchedulingDisabled   5h        v1.6.6
   ```


## <a name="create-a-private-azure-container-registry-using-the-azure-cli"></a>Tworzenie rejestru prywatnej kontenera platformy Azure przy użyciu wiersza polecenia platformy Azure

1. Tworzenie rejestru prywatnej kontenera platformy Azure w grupie zasobów do obsługi obrazu Docker; na przykład:
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes --location westeurope --name wingtiptoysregistry --sku Basic
   ```
   Gdzie:  
      * *wingtiptoys kubernetes* to nazwa grupy zasobów z wcześniej w tym artykule  
      * *wingtiptoysregistry* jest unikatową nazwę prywatnej rejestru
      * *westeurope* jest odpowiednią lokalizację geograficzną dla aplikacji  

   Azure CLI wyświetli wyniki tworzenia z rejestru; na przykład:  

   ```json
   {
     "adminUserEnabled": true,
     "creationDate": "2017-09-15T01:00:00.000000+00:00",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/wingtiptoys-kubernetes/providers/Microsoft.ContainerRegistry/registries/wingtiptoysregistry",
     "location": "westeurope",
     "loginServer": "wingtiptoysregistry.azurecr.io",
     "name": "wingtiptoysregistry",
     "provisioningState": "Succeeded",
     "resourceGroup": "wingtiptoys-kubernetes",
     "sku": {
       "name": "Basic",
       "tier": "Basic"
     },
     "storageAccount": {
       "name": "wingtiptoysregistr000000"
     },
     "tags": {},
     "type": "Microsoft.ContainerRegistry/registries"
   }
   ```

1. Pobrać hasło do rejestru kontenera z wiersza polecenia platformy Azure.
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   Azure CLI wyświetli hasło do rejestru; na przykład:  

   ```json
   {
     "name": "password",
     "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. Przejdź do katalogu konfiguracji dla tej instalacji Maven (~/.m2/ domyślne lub C:\Users\username\.m2), a następnie otwórz *settings.xml* plik w edytorze tekstów.

1. Dodawanie adresu URL rejestru kontenera platformy Azure, nazwę użytkownika i hasło na nowy `<server>` kolekcji w *settings.xml* pliku.
`id` i `username` są nazwa rejestru. Użyj `password` wartość z zakresu od poprzedniego polecenia (bez cudzysłowów).

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry.azurecr.io</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Przejdź do katalogu projektu zakończone aplikacji Spring rozruchu (na przykład "*C:\SpringBoot\gs-spring-boot-docker\complete*"lub"*/home/GenaSoto/SpringBoot/gs-spring-boot-docker/complete* "), a następnie otwórz *pom.xml* plik w edytorze tekstów.

1. Aktualizacja `<properties>` kolekcji w *pom.xml* pliku z wartością serwera logowania do rejestru kontenera platformy Azure.

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Aktualizacja `<plugins>` kolekcji w *pom.xml* pliku, aby `<plugin>` zawiera logowania serwera adresu rejestru nazwę i rejestru kontenera platformy Azure.

   ```xml
   <plugin>
     <groupId>com.spotify</groupId>
     <artifactId>dockerfile-maven-plugin</artifactId>
     <version>1.3.4</version>
     <configuration>
       <repository>${docker.image.prefix}/${project.artifactId}</repository>
       <serverId>${docker.image.prefix}</serverId>
       <registryUrl>https://${docker.image.prefix}</registryUrl>
     </configuration>
   </plugin>
   ```

1. Przejdź do katalogu projektu zakończone aplikacji Spring rozruchu, a następnie uruchom następujące polecenie narzędzia Maven do tworzenia kontenera Docker i push obrazu do rejestru:

   ```shell
   mvn package dockerfile:build -DpushImage
   ```

   Maven wyświetli wyniki kompilacji; na przykład:  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 38.113 s
   [INFO] Finished at: 2017-09-15T02:00:00-07:00
   [INFO] Final Memory: 47M/338M
   [INFO] ----------------------------------------------------
   ```


## <a name="configure-your-spring-boot-app-to-use-the-fabric8-maven-plugin"></a>Skonfiguruj aplikację Spring rozruchu, aby użyć wtyczki Fabric8 Maven

1. Przejdź do katalogu projektu zakończone aplikacji Spring rozruchu, (na przykład: "*C:\SpringBoot\gs-spring-boot-docker\complete*"lub"*/home/GenaSoto/SpringBoot/gs-spring-boot-docker / pełne*"), a następnie otwórz *pom.xml* plik w edytorze tekstów.

1. Aktualizacja `<plugins>` kolekcji w *pom.xml* plik, aby dodać wtyczkę Fabric8 Maven:

   ```xml
   <plugin>
     <groupId>io.fabric8</groupId>
     <artifactId>fabric8-maven-plugin</artifactId>
     <version>3.5.30</version>
     <configuration>
       <ignoreServices>false</ignoreServices>
       <registry>${docker.image.prefix}</registry>
     </configuration>
   </plugin>
   ```

1. Przejdź do katalogu głównego źródła dla aplikacji Spring rozruchu, (na przykład: "*C:\SpringBoot\gs-spring-boot-docker\complete\src\main*"lub"*/home/GenaSoto/SpringBoot/gs-spring-boot-docker / Zakończenie/src/main*") i Utwórz nowy folder o nazwie"*fabric8*".

1. Utwórz trzy yaml programu fragmenty plików w nowym *fabric8* folderu:

   a. Utwórz plik o nazwie **deployment.yml** z następującą zawartość:
      ```yaml
      apiVersion: extensions/v1beta1
      kind: Deployment
      metadata:
        name: ${project.artifactId}
        labels:
          run: gs-spring-boot-docker
      spec:
        replicas: 1
        selector:
          matchLabels:
            run: gs-spring-boot-docker
        strategy:
          rollingUpdate:
            maxSurge: 1
            maxUnavailable: 1
          type: RollingUpdate
        template:
          metadata:
            labels:
              run: gs-spring-boot-docker
          spec:
            containers:
            - image: ${docker.image.prefix}/${project.artifactId}:latest
              name: gs-spring-boot-docker
              imagePullPolicy: Always
              ports:
              - containerPort: 8080
            imagePullSecrets:
              - name: mysecrets
      ```

   b. Utwórz plik o nazwie **secrets.yml** z następującą zawartość:
      ```yaml
      apiVersion: v1
      kind: Secret
      metadata:
        name: mysecrets
        namespace: default
        annotations:
          maven.fabric8.io/dockerServerId:  ${docker.image.prefix}
      type: kubernetes.io/dockercfg
      ```

   c. Utwórz plik o nazwie **service.yml** z następującą zawartość:
      ```yaml
      apiVersion: v1
      kind: Service
      metadata:
        name: ${project.artifactId}
        labels:
          expose: "true"
      spec:
        ports:
        - port: 80
          targetPort: 8080
        type: LoadBalancer
      ```

1. Uruchom następujące polecenie narzędzia Maven, aby utworzyć plik listy zasobów Kubernetes:

   ```shell
   mvn fabric8:resource
   ```

   To polecenie łączy wszystkie pliki yaml programu Kubernetes zasobów w obszarze *src/main/fabric8* folderu do pliku yaml programu, który zawiera listę zasobów Kubernetes, które mogą być stosowane do klastra Kubernetes bezpośrednio lub eksportu do wykresu helm.

   Maven wyświetli wyniki kompilacji; na przykład:  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 16.744 s
   [INFO] Finished at: 2017-09-15T02:35:00-07:00
   [INFO] Final Memory: 30M/290M
   [INFO] ----------------------------------------------------
   ```

1. Uruchom następujące polecenie narzędzia Maven, aby zastosować plik listy zasobów do klastra Kubernetes:

   ```shell
   mvn fabric8:apply
   ```

   Maven wyświetli wyniki kompilacji; na przykład:  

   ```shell
   [INFO] ----------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ----------------------------------------------------
   [INFO] Total time: 14.814 s
   [INFO] Finished at: 2017-09-15T02:41:00-07:00
   [INFO] Final Memory: 35M/288M
   [INFO] ----------------------------------------------------
   ```

1. Po wdrożeniu aplikacji do klastra, zapytania zewnętrzny adres IP przy użyciu `kubectl` aplikacji, np.:
   ```shell
   kubectl get svc -w
   ```

   `kubectl`zostanie wyświetlona wewnętrzne i zewnętrzne adresy IP; na przykład:
   
   ```shell
   NAME                    CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
   kubernetes              10.0.0.1     <none>        443/TCP        19h
   gs-spring-boot-docker   10.0.242.8   13.65.196.3   80:31215/TCP   3m
   ```
   
   Zewnętrzny adres IP można użyć, aby otworzyć aplikację w przeglądarce sieci web.

   ![Przeglądaj zewnętrznie przykładowej aplikacji][SB02]

## <a name="delete-your-kubernetes-cluster"></a>Usuń klaster Kubernetes

Gdy klaster Kubernetes nie jest już potrzebne, można użyć `az group delete` polecenie, aby usunąć grupę zasobów, co spowoduje usunięcie wszystkich jego powiązane zasoby; na przykład:

   ```azurecli
   az group delete --name wingtiptoys-kubernetes --yes --no-wait
   ```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o używaniu aplikacji rozruchu Spring na platformie Azure zobacz następujące artykuły:

* [Wdrażanie aplikacji rozruchu Spring w usłudze Azure App Service](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Wdrażanie aplikacji Spring rozruchu w systemie Linux w usłudze kontenera platformy Azure](container-service-deploy-spring-boot-app-on-linux.md)
* [Wdrażanie aplikacji rozruchu Spring w klastrze Kubernetes w usłudze kontenera platformy Azure](container-service-deploy-spring-boot-app-on-kubernetes.md)

Aby uzyskać więcej informacji o używaniu platformy Azure z językiem Java, zobacz [Centrum deweloperów języka Java dla platformy Azure] i [Java Tools for Visual Studio Team Services] (Narzędzia języka Java dla usługi Visual Studio Team Services).

Aby uzyskać dalsze informacje na temat rozruchu Spring na Docker przykładowy projekt, zobacz [rozruchu Spring na wprowadzenie Docker].

Aby uzyskać pomoc dotyczącą Rozpoczynanie pracy z aplikacjami Spring rozruchu, zobacz **Spring Initializr** w <https://start.spring.io/>.

Aby uzyskać więcej informacji na temat rozpoczynania pracy z tworzenia prostej aplikacji Spring rozruchu, zobacz Initializr Spring na <https://start.spring.io/>.

Dodatkowe przykłady korzystania z platformy Azure, niestandardowe obrazy usługi Docker można znaleźć [dla aplikacji sieci Web platformy Azure w systemie Linux przy użyciu niestandardowego obrazu Docker].

<!-- URL List -->

[Azure interfejsu wiersza polecenia (CLI)]: /cli/azure/overview
[usługi kontenera platformy Azure (ACS)]: https://azure.microsoft.com/services/container-service/
[Centrum deweloperów języka Java dla platformy Azure]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[dla aplikacji sieci Web platformy Azure w systemie Linux przy użyciu niestandardowego obrazu Docker]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Fabric8]: https://fabric8.io/
[bezpłatne konto platformy Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Maven]: http://maven.apache.org/
[korzyści dla subskrybentów MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[rozruchu Spring na wprowadzenie Docker]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-using-fabric8-maven-plugin/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-using-fabric8-maven-plugin/SB02.png
