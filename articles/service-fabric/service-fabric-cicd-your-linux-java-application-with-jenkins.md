---
title: "Ciągła kompilacja i integrowanie aplikacji Java usługi Service Fabric w systemie Linux przy użyciu narzędzia Jenkins | Microsoft Docs"
description: "Ciągła kompilacja i integrowanie aplikacji Java w systemie Linux przy użyciu narzędzia Jenkins"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 335f41e2e4a40e64e87382ea338673de3ab79c27
ms.lasthandoff: 03/14/2017


---
# <a name="build-and-deploy-your-linux-java-application-using-jenkins"></a>Kompiluj i wdrażaj aplikację Java systemu Linux przy użyciu narzędzia Jenkins
Jenkins to popularne narzędzie służące do przeprowadzania ciągłej integracji i ciągłego wdrażania. W tym dokumencie przedstawiono, w jaki sposób można kompilować i wdrażać aplikację usługi Service Fabric za pomocą narzędzia Jenkins.

## <a name="general-prerequisites"></a>Ogólne wymagania wstępne
1. Należy mieć lokalnie zainstalowane narzędzie git. Odpowiednią wersję narzędzia Git można zainstalować z [tej witryny](https://git-scm.com/downloads) w zależności od używanego systemu operacyjnego. Jeśli dopiero zaczynasz korzystać z narzędzia git, w celu zapoznania się z nim możesz wykonać kroki opisane w [dokumentacji](https://git-scm.com/docs).
2. Miej pod ręką wtyczkę narzędzia Jenkins dla usługi Service Fabric. Pobierz dodatek Jenkins usługi Service Fabric [stąd](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi).

## <a name="setting-up-jenkins-inside-a-service-fabric-cluster"></a>Konfigurowanie narzędzia Jenkins w ramach klastra usługi Service Fabric

### <a name="prerequisites"></a>Wymagania wstępne
1. Przygotuj klaster systemu Linux usługi Service Fabric. W klastrach usługi Service Fabric utworzonych z poziomu witryny Azure Portal jest już zainstalowana platforma Docker. Jeśli korzystasz z klastra lokalnego, sprawdź za pomocą polecenia ``docker info``, czy platforma Docker jest zainstalowana. Jeśli nie jest, zainstaluj ją odpowiednio przy użyciu następujących poleceń:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```
2. Wdróż aplikację kontenera usługi Service Fabric w klastrze, wykonując następujące kroki:

  ```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git -b JenkinsDocker
cd service-fabric-java-getting-started/Services/JenkinsDocker/
azure servicefabric cluster connect http://PublicIPorFQDN:19080   # Azure CLI cluster connect command
bash Scripts/install.sh
```
To spowoduje zainstalowanie kontenera narzędzia Jenkins w klastrze i umożliwia monitorowanie za pomocą eksploratora usługi Service Fabric.

### <a name="steps"></a>Kroki
1. Przejdź do adresu URL ``http://PublicIPorFQDN:8081`` z poziomu przeglądarki. Tam jest dostępna ścieżka do początkowego hasła administratora wymaganego do zalogowania się. Możesz kontynuować używanie narzędzia Jenkins jako administrator lub utworzyć i zmienić użytkownika po zalogowaniu się przy użyciu początkowego konta administratora.

  > [!NOTE]
  > Podczas tworzenia klastra upewnij się, że port 8081 jest określony jako port punktu końcowego aplikacji
  >

2. Pobierz identyfikator wystąpienia kontenera za pomocą polecenia ``docker ps -a``.
3. Zaloguj się do kontenera przy użyciu protokołu SSH i wklej ścieżkę wyświetloną w portalu narzędzia Jenkins. Na przykład jeśli w portalu wyświetlana jest ścieżka `PATH_TO_INITIAL_ADMIN_PASSWORD`, możesz wykonać następujące polecenia:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. Skonfiguruj usługę GitHub do pracy z narzędziem Jenkins za pomocą kroków wymienionych pod [tym linkiem](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
    * Użyj instrukcji udostępnionych przez usługę GitHub, aby wygenerować klucz SSH i dodać go do konta usługi GitHub, które hostuje Twoje repozytorium.
    * Uruchom polecenia wymienione pod powyższym linkiem w powłoce platformy Docker narzędzia Jenkins (a nie na hoście)
    * Aby zalogować się do powłoki narzędzia Jenkins z poziomu hosta, użyj następującego polecenia:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="setting-up-jenkins-outside-a-service-fabric-cluster"></a>Konfigurowanie narzędzia Jenkins poza klastrem usługi Service Fabric

### <a name="prerequisites"></a>Wymagania wstępne
Wymagane jest zainstalowanie platformy Docker. Poniższe polecenia umożliwiają zainstalowanie platformy Docker z terminalu:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

Teraz, po uruchomieniu polecenia ``docker info`` w terminalu, powinny zostać wyświetlone dane wyjściowe z informacją, że usługa platformy Docker jest uruchomiona.

### <a name="steps"></a>Kroki
  1. Ściągnij obraz kontenera narzędzia Jenkins usługi Service Fabric: ``docker pull servicefabric-microsoft.azurecr.io/jenkins:v1``
  2. Uruchom obraz kontenera: ``docker run -itd -p 8080:8080 servicefabric-microsoft.azurecr.io/jenkins:v1``
  3. Pobierz identyfikator wystąpienia obrazu kontenera. Listę wszystkich kontenerów platformy Docker można wyświetlić za pomocą polecenia ``docker ps –a``
  4. Zaloguj się do portalu narzędzia Jenkins, wykonując następujące kroki:

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Jeśli identyfikator kontenera to 2d24a73b5964, użyj wartości 2d24.
    * To hasło będzie wymagane do zalogowania się do pulpitu nawigacyjnego narzędzia Jenkins z poziomu portalu, którego adres to ``http://<HOST-IP>:8080``
    * Po pierwszym zalogowaniu się możesz utworzyć własne konto użytkownika i używać go w przyszłości. Możesz także nadal korzystać z konta administratora. Po utworzeniu użytkownika należy kontynuować, korzystając z niego.
  5. Skonfiguruj usługę GitHub do pracy z narzędziem Jenkins za pomocą kroków wymienionych pod [tym linkiem](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
      * Użyj instrukcji udostępnionych w serwisie GitHub, aby wygenerować klucz SSH i dodać klucz SSH do konta serwisu GitHub, które będzie hostować repozytorium.
      * Uruchom polecenia wymienione pod powyższym linkiem w powłoce platformy Docker narzędzia Jenkins (a nie na hoście)
      * Aby zalogować się do powłoki narzędzia Jenkins z poziomu hosta, użyj następujących poleceń:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

    > [!NOTE]
    > Upewnij się, że klaster lub maszyna, gdzie będzie hostowany obraz kontenera narzędzia Jenkins ma publiczny adres IP, aby powiadomienia z serwisu GitHub były odbierane przez wystąpienie narzędzia Jenkins.
    >

## <a name="install-the-service-fabric-jenkins-plugin-from-portal"></a>Instalowanie wtyczki narzędzia Jenkins dla usługi Service Fabric z poziomu portalu

1. Przejdź do strony ``http://PublicIPorFQDN:8081``
2. Na pulpicie nawigacyjnym narzędzia Jenkins wybierz pozycję **Manage Jenkins** -> **Manage Plugins** -> **Advanced** (Zarządzaj narzędziem Jenkins > Zarządzaj wtyczkami > Zaawansowane).
W tym miejscu możesz przekazać wtyczkę. Wybierz opcję **Choose file** (Wybierz plik), a następnie wybierz plik serviceFabric.hpi, który został pobrany w ramach wymagań wstępnych. Po wybraniu polecenia przekazania narzędzie Jenkins automatycznie zainstaluje wtyczkę. Zezwól na ponowne uruchomienie komputera, jeśli zostanie wyświetlony odpowiedni monit.

## <a name="creating-and-configuring-a-jenkins-job"></a>Tworzenie i konfigurowanie zadania narzędzia Jenkins

1. Tworzenie **nowego elementu** z pulpitu nawigacyjnego
2. Podaj nazwę nowego elementu, na przykład **MojeZadanie**, wybierz projekt w stylu dowolnym i kliknij przycisk OK
3. Następnie przejdź na stronę zadania i kliknij pozycję **Configure** - (Konfiguruj)
  * W sekcji General (Ogólne) w obszarze **Github project** (Projekt usługi GitHub) określ adres URL projektu usługi GitHub hostującej aplikację Java usługi Service Fabric, którą chcesz zintegrować z przepływem ciągłej integracji/ciągłego wdrażania narzędzia Jenkins (na przykład ``https://github.com/sayantancs/SFJenkins``).
  * W obszarze **Source Code Management** (Zarządzanie kodem źródłowym) wybierz pozycję **Git**. Określ adres URL repozytorium hostującego aplikację Java usługi Service Fabric, którą chcesz zintegrować z przepływem ciągłej integracji/ciągłego wdrażania narzędzia Jenkins (na przykład ``https://github.com/sayantancs/SFJenkins.git``). W tym miejscu możesz również określić gałąź do skompilowania, na przykład ***/master**.
4. Skonfiguruj usługę *GitHub* (która hostuje repozytorium), aby mogła komunikować się z narzędziem Jenkins, wykonując następujące kroki —
  1. Przejdź do strony repozytorium serwisu GitHub. Przejdź do pozycji **Settings** -> **Integrations and Services** (Ustawienia > Integracje i usługi).
  2. Wybierz pozycję **Add Service** (Dodaj usługę), wpisz ciąg „Jenkins”, wybierz pozycję **Jenkins-Github plugin** (Wtyczka Jenkins-Github).
  3. Wprowadź adres URL elementu webhook narzędzia Jenkins (domyślnie adres URL powinien być następujący: ``http://<PublicIPorFQDN>:8081/github-webhook/``). Kliknij pozycję dodania/aktualizacji usługi.
  4. Do wystąpienia narzędzia Jenkins zostanie wysłane zdarzenie testowe. W serwisie GitHub powinien zostać wyświetlony zielony znacznik wyboru obok elementu webhook i projekt zostanie skompilowany.
  5. W sekcji **Build Triggers** (Wyzwalacze kompilacji) wybierz żądaną opcję kompilacji. Dla tego przypadku użycia planujemy wyzwolenie kompilacji po każdym wypchnięciu do repozytorium, więc odpowiednią opcją będzie **GitHub hook trigger for GITScm polling** (Wyzwalacz punktu zaczepienia GitHub na potrzeby sondowania GITScm). Wcześniej było to „Build when a change is pushed to GitHub” (Kompiluj po wypchnięciu zmiany do usługi GitHub).
  6. W **sekcji Build (Kompilacja)** z listy rozwijanej **Add build step** (Dodaj krok kompilacji) wybierz opcję **Invoke Gradle Script** (Wywołaj skrypt Gradle). W wyświetlonym widżecie określ ścieżkę do lokalizacji **Root build script** (Główny skrypt kompilacji) dla aplikacji. Spowoduje to wybranie elementu build.gradle z określonej ścieżki i wykonanie odpowiedniego działania. W przypadku utworzenia projektu o nazwie ``MyActor`` (przy użyciu wtyczki środowiska Eclipse lub generatora Yeoman) główny skrypt kompilacji powinien zawierać element ``${WORKSPACE}/MyActor``. Przykładowo ta sekcja wygląda mniej więcej w następujący sposób:

    ![Akcja kompilacji narzędzia Jenkins w usłudze Service Fabric][build-step]
  7. W obszarze **Post-Build Actions** (Akcje po kompilacji) wybierz pozycję **Deploy Service Fabric Project** (Wdróż projekt usługi Service Fabric). W tym miejscu należy podać szczegóły dotyczące klastra, w ramach którego zostanie wdrożona skompilowana aplikacja usługi Service Fabric narzędzia Jenkins oraz dodatkowe szczegóły dotyczące aplikacji służące do jej wdrożenia. Poniższy zrzut ekranu może służyć jako źródło informacji:

    ![Akcja kompilacji narzędzia Jenkins w usłudze Service Fabric][post-build-step]

 > [!NOTE]
 > Określany w tym miejscu klaster może być tym samym klastrem, który hostuje aplikację kontenera narzędzia Jenkins, jeśli usługa Service Fabric jest używana do wdrożenia obrazu kontenera narzędzia Jenkins.
 >

### <a name="end-to-end-scenario"></a>Kompleksowy scenariusz
W tym momencie usługa GitHub oraz narzędzie Jenkins są skonfigurowane oraz jest możliwe wprowadzenie pewnych przykładowych zmian w projekcie ``MyActor`` w repozytorium (na przykład https://github.com/sayantancs/SFJenkins) i wypchnięcie ich do zdalnej gałęzi ``master`` lub dowolnej innej gałęzi, która została skonfigurowana do użycia. Spowoduje to wyzwolenie skonfigurowanego zadania narzędzia Jenkins ``MyJob``. W jego wyniku będzie miało miejsce pobranie zmian z serwisu GitHub, skompilowanie ich i wdrożenie aplikacji w punkcie końcowym klastra określonym w akcjach wykonywanych po kompilacji.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png

