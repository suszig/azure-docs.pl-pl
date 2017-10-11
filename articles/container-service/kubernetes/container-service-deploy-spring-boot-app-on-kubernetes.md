---
title: "Wdrażanie aplikacji rozruchu Spring na Kubernetes w usłudze kontenera platformy Azure | Dokumentacja firmy Microsoft"
description: "Ten samouczek przeprowadzi Cię, chociaż kroki, aby wdrożyć aplikację Spring rozruchu w Kubernetes klastra w systemie Microsoft Azure."
services: container-service
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: 7f726436b2d459b8c16abb02e07de099abfd8974
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-container-service"></a>Wdrażanie aplikacji platformy Spring Boot w klastrze Kubernetes w usłudze Azure Container Service

 **[Spring Framework]**  popularnych struktura open source, który pomaga Java deweloperom tworzenie aplikacji interfejsu API, mobilne i sieci web. W tym samouczku używana przykładowej aplikacji utworzony za pomocą [rozruchu Spring], podejście oparte na Konwencji do szybkie rozpoczęcie pracy przy użyciu źródła.

**[Kubernetes]**  i  **[Docker]**  są rozwiązania open source, które ułatwiają deweloperom automatyzację wdrażania, skalowania i zarządzania swoje aplikacje uruchomione kontenery.

Ten samouczek przedstawia Chociaż połączenie tych dwóch popularnych open source technologii do opracowywania i wdrażania aplikacji Spring rozruchu w systemie Microsoft Azure. W szczególności, użyj  *[rozruchu Spring]*  przy projektowaniu aplikacji,  *[Kubernetes]*  wdrożenia kontenera i [ Usługa kontenera platformy Azure (ACS)] do hostowania aplikacji.

### <a name="prerequisites"></a>Wymagania wstępne

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
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   --lub--
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Klonowanie [rozruchu Spring na wprowadzenie Docker] przykładowy projekt do katalogu.
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Zmień katalog projektu zakończone.
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. Używanie programu Maven do tworzenia i uruchamianie przykładowej aplikacji.
   ```
   mvn package spring-boot:run
   ```

1. Testowanie aplikacji sieci web, przechodząc do adresem http://localhost: 8080 lub z następującymi `curl` polecenia:
   ```
   curl http://localhost:8080
   ```

1. Powinien zostać wyświetlony następujący komunikat wyświetlany: **Hello, World Docker**

   ![Przejdź do przykładowej aplikacji lokalnie][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>Tworzenie rejestru kontenera Azure za pomocą wiersza polecenia platformy Azure

1. Otwórz wiersz polecenia.

1. Zaloguj się do konta platformy Azure:
   ```azurecli
   az login
   ```

1. Utwórz grupę zasobów dla zasobów platformy Azure używana w tym samouczku.
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. Utwórz rejestru prywatnej kontenera platformy Azure w grupie zasobów. Samouczek wypychanie przykładową aplikację jako obraz Docker do tego rejestru w kolejnych krokach. Zastąp `wingtiptoysregistry` o unikatowej nazwie dla rejestru.
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a>Wypychanie aplikację do rejestru kontenera

1. Przejdź do katalogu konfiguracji dla tej instalacji Maven (~/.m2/ domyślne lub C:\Users\username\.m2), a następnie otwórz *settings.xml* plik w edytorze tekstów.

1. Pobrać hasło do rejestru kontenera z wiersza polecenia platformy Azure.
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. Dodaj identyfikator rejestru kontenera Azure i hasło na nowy `<server>` kolekcji w *settings.xml* pliku.
`id` i `username` są nazwa rejestru. Użyj `password` wartość z zakresu od poprzedniego polecenia (bez cudzysłowów).

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Przejdź do katalogu projektu zakończone aplikacji Spring rozruchu (na przykład "*C:\SpringBoot\gs-spring-boot-docker\complete*"lub"*/users/robert/SpringBoot/gs-spring-boot-docker/complete* "), a następnie otwórz *pom.xml* plik w edytorze tekstów.

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
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. Przejdź do katalogu projektu zakończone aplikacji Spring rozruchu i uruchom następujące polecenie, aby utworzyć kontener Docker i Wypchnij obrazu w rejestrze:

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  Użytkownik może zostać wyświetlony komunikat o błędzie jest podobny do jednego z następujących przypadku Maven wypchnięcia obrazu na platformie Azure:
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> Jeśli ten błąd, zaloguj się do platformy Azure z poziomu wiersza polecenia Docker.
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Następnie Wypchnij z kontenera:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-the-azure-cli"></a>Tworzenie klastra Kubernetes na ACS za pomocą wiersza polecenia platformy Azure

1. Utwórz klaster Kubernetes usługi kontenera platformy Azure. Poniższe polecenie tworzy *kubernetes* klastra w *wingtiptoys kubernetes* zasobów grupy z *wingtiptoys containerservice* jako nazwę klastra i *wingtiptoys kubernetes* jako DNS prefiksu:
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   To polecenie może zająć trochę czasu, aby zakończyć.

1. Zainstaluj `kubectl` przy użyciu wiersza polecenia platformy Azure. Linux użytkownicy mogą mieć do prefiksu to polecenie z `sudo` ponieważ wdrażania Kubernetes CLI `/usr/local/bin`.
   ```azurecli
   az acs kubernetes install-cli
   ```

1. Pobierz informacje o konfiguracji klastra, klaster można zarządzać za pomocą interfejsu sieci web Kubernetes i `kubectl`. 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a>Wdrażanie obrazu do klastra Kubernetes

W tym samouczku wdraża aplikacje przy użyciu funkcji `kubectl`, umożliwiają następnie eksplorować wdrożenia za pomocą interfejsu sieci web Kubernetes.

### <a name="deploy-with-the-kubernetes-web-interface"></a>Rozmieszczanie za pomocą interfejsu sieci web Kubernetes

1. Otwórz wiersz polecenia.

1. Otwórz konfigurację witryny sieci Web dla klastra Kubernetes w domyślnej przeglądarce:
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. Po otwarciu Kubernetes konfiguracji witryny sieci Web w przeglądarce, kliknij łącze, aby **wdrażania konteneryzowanych aplikacji**:

   ![Kubernetes konfiguracji witryny sieci Web][KB01]

1. Gdy **wdrażania konteneryzowanych aplikacji** zostanie wyświetlona strona, określ następujące opcje:

   a. Wybierz **Określ szczegóły aplikacji poniżej**.

   b. Wprowadź nazwę aplikacji Spring rozruchu dla **Nazwa aplikacji**, na przykład: "*gs-spring — rozruchu — docker*".

   c. Wprowadź identyfikator logowania serwera i kontener obrazu z wcześniej **obrazu kontenera**, na przykład: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".

   d. Wybierz **zewnętrznych** dla **usługi**.

   e. Określ sieci zewnętrznych i wewnętrznych portów w **portu** i **docelowy port** pól tekstowych.

   ![Kubernetes konfiguracji witryny sieci Web][KB02]


1. Kliknij przycisk **Wdróż** celu wdrożenia kontenera.

   ![Wdrażanie kontenera][KB05]

1. Po wdrożeniu aplikacji, zostanie wyświetlona aplikacji rozruchu Spring kategorii **usług**.

   ![Kubernetes usług][KB06]

1. Jeśli klikniesz link do **zewnętrzne punkty końcowe**, zobaczysz rozruchu Spring aplikacji działających na platformie Azure.

   ![Kubernetes usług][KB07]

   ![Przejdź do przykładowej aplikacji na platformie Azure][SB02]


### <a name="deploy-with-kubectl"></a>Wdrażanie z kubectl

1. Otwórz wiersz polecenia.

1. Uruchom z kontenera w klastrze Kubernetes przy użyciu `kubectl run` polecenia. Nadaj nazwę usługi dla aplikacji na Kubernetes i nazwę pełnego obrazu. Na przykład:
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   W tym poleceniu:

   * Nazwa kontenera `gs-spring-boot-docker` określono natychmiast po `run` polecenia

   * `--image` Parametr określa nazwę serwera i obrazu połączonego logowania jako`wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`

1. Udostępnianie klastra Kubernetes zewnętrznie przy użyciu `kubectl expose` polecenia. Określ nazwę usługi, port TCP publicznych umożliwiają dostęp do aplikacji i port docelowy wewnętrznego, które nasłuchuje aplikacja. Na przykład:
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   W tym poleceniu:

   * Nazwa kontenera `gs-spring-boot-docker` określono natychmiast po `expose deployment` polecenia

   * `--type` Parametr określa, że klaster używa modułu równoważenia obciążenia

   * `--port` Parametr określa publicznych port TCP 80. Możesz uzyskać dostępu do aplikacji na tym porcie.

   * `--target-port` Parametr określa wewnętrzny port TCP 8080. Moduł równoważenia obciążenia przekazuje żądania do aplikacji na tym porcie.

1. Po wdrożeniu aplikacji do klastra, zapytania zewnętrzny adres IP, a następnie otwórz go w przeglądarce sieci web:

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Przejdź do przykładowej aplikacji na platformie Azure][SB02]


## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat Spring rozruch przy użyciu na platformie Azure zobacz następujące artykuły:

* [Wdrażanie aplikacji rozruchu Spring w usłudze Azure App Service](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Wdrażanie aplikacji Spring rozruchu w systemie Linux w usłudze kontenera platformy Azure](container-service-deploy-spring-boot-app-on-linux.md)

Aby uzyskać więcej informacji o używaniu platformy Azure z językiem Java, zobacz [Centrum deweloperów języka Java dla platformy Azure] i [Java Tools for Visual Studio Team Services] (Narzędzia języka Java dla usługi Visual Studio Team Services).

Aby uzyskać więcej informacji na temat rozruchu Spring na Docker przykładowy projekt, zobacz [rozruchu Spring na wprowadzenie Docker].

Poniższe linki udostępniają dodatkowe informacje na temat tworzenia aplikacji Spring rozruchu:

* Aby uzyskać więcej informacji dotyczących tworzenia prostej aplikacji Spring rozruchu Zobacz Initializr Spring na https://start.spring.io/.

Poniższe linki udostępniają dodatkowe informacje dotyczące korzystania z usługi Azure Kubernetes:

* [Rozpoczynanie pracy z klastrem Kubernetes w usłudze kontenera](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [Przy użyciu interfejsu użytkownika sieci web Kubernetes z usługi kontenera platformy Azure](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

Więcej informacji na temat przy użyciu interfejsu wiersza polecenia Kubernetes jest dostępna w **kubectl** Podręcznik użytkownika w <https://kubernetes.io/docs/user-guide/kubectl/>.

Kubernetes witryny sieci Web ma kilka artykuł, w którym omówiono w nim przy użyciu obrazów w rejestrach prywatnych:

* [Konfigurowanie usługi konta dla stanowiskami]
* [Przestrzenie nazw]
* [Ściąganie obrazu z rejestru prywatnych]

Dodatkowe przykłady korzystania z platformy Azure, niestandardowe obrazy usługi Docker można znaleźć [dla aplikacji sieci Web platformy Azure w systemie Linux przy użyciu niestandardowego obrazu Docker].

<!-- URL List -->

[Azure interfejsu wiersza polecenia (CLI)]: /cli/azure/overview
[ Usługa kontenera platformy Azure (ACS)]: https://azure.microsoft.com/services/container-service/
[Centrum deweloperów języka Java dla platformy Azure]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[dla aplikacji sieci Web platformy Azure w systemie Linux przy użyciu niestandardowego obrazu Docker]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[bezpłatne konto platformy Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[korzyści dla subskrybentów MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[rozruchu Spring]: http://projects.spring.io/spring-boot/
[rozruchu Spring na wprowadzenie Docker]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Konfigurowanie usługi konta dla stanowiskami]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[Przestrzenie nazw]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[Ściąganie obrazu z rejestru prywatnych]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR04.png

[KB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB01.png
[KB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB02.png
[KB03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB03.png
[KB04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB04.png
[KB05]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB05.png
[KB06]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB06.png
[KB07]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB07.png
