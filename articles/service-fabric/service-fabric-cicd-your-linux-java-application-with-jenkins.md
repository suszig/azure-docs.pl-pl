---
title: "Ciągła kompilacja i ciągłe integrowanie aplikacji Java usługi Azure Service Fabric w systemie Linux przy użyciu narzędzia Jenkins | Microsoft Docs"
description: "Ciągła kompilacja i ciągłe integrowanie aplikacji Java w systemie Linux przy użyciu narzędzia Jenkins"
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
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 71e3d130f22515d22dc7f486f3dede936b874049
ms.lasthandoff: 03/25/2017


---
# <a name="use-jenkins-to-build-and-deploy-your-linux-java-application"></a>Kompiluj i wdrażaj aplikację Java w systemie Linux przy użyciu narzędzia Jenkins
Jenkins to popularne narzędzie służące do przeprowadzania ciągłej integracji i ciągłego wdrażania aplikacji. Poniżej przedstawiono sposób kompilowania i wdrażania aplikacji usługi Azure Service Fabric przy użyciu narzędzia Jenkins.

## <a name="general-prerequisites"></a>Ogólne wymagania wstępne
- Lokalnie zainstalowane narzędzie Git. Odpowiednią wersję narzędzia Git dla używanego systemu operacyjnego można zainstalować ze [strony pobierania narzędzia Git](https://git-scm.com/downloads). Jeśli jesteś nowym użytkownikiem narzędzia Git, możesz się dowiedzieć o nim więcej z [jego dokumentacji](https://git-scm.com/docs).
- Przygotowana wtyczka narzędzia Jenkins dla usługi Service Fabric. Możesz ją pobrać z [witryny pobierania usługi Service Fabric](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi).

## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Konfigurowanie narzędzia Jenkins w ramach klastra usługi Service Fabric

Narzędzie Jenkins możesz skonfigurować wewnątrz klastra usługi Service Fabric lub poza nim. W poniższych sekcjach opisano sposób konfiguracji narzędzia wewnątrz klastra.

### <a name="prerequisites"></a>Wymagania wstępne
1. Przygotuj klaster systemu Linux usługi Service Fabric. W klastrach usługi Service Fabric utworzonych z poziomu witryny Azure Portal jest już zainstalowana platforma Docker. Jeśli klaster jest uruchamiany lokalnie, za pomocą polecenia ``docker info`` sprawdź, czy platforma Docker jest zainstalowana. Jeśli nie jest zainstalowana, zainstaluj ją za pomocą następujących poleceń:

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
To spowoduje zainstalowanie kontenera narzędzia Jenkins w klastrze i umożliwi monitorowanie za pomocą narzędzia Service Fabric Explorer.

### <a name="steps"></a>Kroki
1. W przeglądarce przejdź na adres ``http://PublicIPorFQDN:8081``. Tam jest dostępna ścieżka do początkowego hasła administratora wymaganego do zalogowania się. Narzędzia Jenkins możesz dalej używać jako administrator. Możesz też utworzyć i zmienić użytkownika po zalogowaniu się przy użyciu początkowego konta administratora.

   > [!NOTE]
   > Podczas tworzenia klastra upewnij się, że port 8081 jest określony jako port punktu końcowego aplikacji.
   >

2. Uzyskaj identyfikator wystąpienia kontenera za pomocą polecenia ``docker ps -a``.
3. Zaloguj się do kontenera przy użyciu protokołu SSH (Secure Shell) i wklej ścieżkę wyświetloną w portalu narzędzia Jenkins. Jeśli na przykład w portalu jest wyświetlana ścieżka `PATH_TO_INITIAL_ADMIN_PASSWORD`, uruchom następujące polecenia:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. Skonfiguruj usługę GitHub do pracy z narzędziem Jenkins, wykonując kroki opisane w temacie [Generating a new SSH key and adding it to the SSH agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) (Generowanie nowego klucza SSH i dodawanie go do agenta SSH).
    * Korzystając z instrukcji udostępnionych przez usługę GitHub, wygeneruj klucz SSH i dodaj go do konta usługi GitHub, które hostuje Twoje repozytorium.
    * Uruchom polecenia wymienione pod powyższym linkiem w powłoce platformy Docker narzędzia Jenkins (a nie na hoście).
    * Aby zalogować się do powłoki narzędzia Jenkins z poziomu hosta, użyj następującego polecenia:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Konfigurowanie narzędzia Jenkins poza klastrem usługi Service Fabric

Narzędzie Jenkins możesz skonfigurować wewnątrz klastra usługi Service Fabric lub poza nim. W poniższych sekcjach opisano sposób konfiguracji narzędzia poza klastrem.

### <a name="prerequisites"></a>Wymagania wstępne
Wymagane jest zainstalowanie platformy Docker. Poniższe polecenia umożliwiają zainstalowanie platformy Docker z terminalu:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

Teraz, po uruchomieniu polecenia ``docker info`` w terminalu, powinny zostać wyświetlone dane wyjściowe z informacją, że usługa platformy Docker jest uruchomiona.

### <a name="steps"></a>Kroki
  1. Ściągnij obraz kontenera narzędzia Jenkins usługi Service Fabric: ``docker pull raunakpandya/jenkins:v1``
  2. Uruchom obraz kontenera: ``docker run -itd -p 8080:8080 raunakpandya/jenkins:v1``
  3. Uzyskaj identyfikator wystąpienia obrazu kontenera. Listę wszystkich kontenerów platformy Docker można wyświetlić za pomocą polecenia ``docker ps –a``
  4. Zaloguj się do portalu narzędzia Jenkins, wykonując następujące kroki:

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Jeśli identyfikator kontenera to 2d24a73b5964, użyj wartości 2d24.
    * To hasło będzie wymagane do zalogowania się do pulpitu nawigacyjnego narzędzia Jenkins z poziomu portalu, którego adres to ``http://<HOST-IP>:8080``
    * Po pierwszym zalogowaniu się możesz utworzyć własne konto użytkownika i używać go w przyszłości. Możesz także nadal korzystać z konta administratora. Po utworzeniu użytkownika należy kontynuować, korzystając z niego.
  5. Skonfiguruj usługę GitHub do pracy z narzędziem Jenkins, wykonując kroki opisane w temacie [Generating a new SSH key and adding it to the SSH agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) (Generowanie nowego klucza SSH i dodawanie go do agenta SSH).
        * Korzystając z instrukcji udostępnionych przez usługę GitHub, wygeneruj klucz SSH i dodaj go do konta usługi GitHub, które hostuje repozytorium.
        * Uruchom polecenia wymienione pod powyższym linkiem w powłoce platformy Docker narzędzia Jenkins (a nie na hoście).
        * Aby zalogować się do powłoki narzędzia Jenkins z poziomu hosta, użyj następujących poleceń:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Upewnij się, że klaster lub maszyna hostujące obraz kontenera narzędzia Jenkins mają publiczny adres IP. Dzięki temu wystąpienie narzędzia Jenkins może otrzymywać powiadomienia z usługi GitHub.

## <a name="install-the-service-fabric-jenkins-plug-in-from-the-portal"></a>Instalowanie wtyczki narzędzia Jenkins dla usługi Service Fabric z poziomu portalu

1. Przejdź do strony ``http://PublicIPorFQDN:8081``
2. Na pulpicie nawigacyjnym narzędzia Jenkins wybierz pozycję **Manage Jenkins** > **Manage Plugins** > **Advanced** (Zarządzaj narzędziem Jenkins > Zarządzaj wtyczkami > Zaawansowane).
W tym miejscu możesz przekazać wtyczkę. Wybierz pozycję **Choose file** (Wybierz plik), a następnie wybierz plik **serviceFabric.hpi**, który został pobrany w ramach wymagań wstępnych. Po wybraniu polecenia **Upload** (Przekaż) narzędzie Jenkins automatycznie zainstaluje wtyczkę. Zezwól na ponowne uruchomienie komputera, jeśli zostanie wyświetlony odpowiedni monit.

## <a name="create-and-configure-a-jenkins-job"></a>Tworzenie i konfigurowanie zadania narzędzia Jenkins

1. Utwórz **nowy element** z poziomu pulpitu nawigacyjnego.
2. Wprowadź nazwę elementu (na przykład **MyJob**). Wybierz pozycję **free-style project** (projekt w stylu dowolnym) i kliknij przycisk **OK**.
3. Przejdź na stronę zadania i kliknij pozycję **Configure** (Konfiguruj).

   a. W sekcji ogólnej, w obszarze **GitHub project** (Projekt GitHub) określ adres URL projektu GitHub. Pod tym adresem URL jest hostowana aplikacja Java usługi Service Fabric, którą chcesz zintegrować z przepływem ciągłej integracji/ciągłego wdrażania narzędzia Jenkins (na przykład ``https://github.com/sayantancs/SFJenkins``).

   b. W obszarze **Source Code Management** (Zarządzanie kodem źródłowym) wybierz pozycję **Git**. Określ adres URL repozytorium hostującego aplikację Java usługi Service Fabric, którą chcesz zintegrować z przepływem ciągłej integracji/ciągłego wdrażania narzędzia Jenkins (na przykład ``https://github.com/sayantancs/SFJenkins.git``). W tym miejscu możesz również określić gałąź do skompilowania (na przykład **/master**).
4. Skonfiguruj usługę *GitHub* (która hostuje repozytorium), aby mogła komunikować się z narzędziem Jenkins. Wykonaj następujące czynności:

   a. Przejdź do strony repozytorium serwisu GitHub. Przejdź do pozycji **Settings** > **Integrations and Services** (Ustawienia > Integracje i usługi).

   b. Wybierz pozycję **Add Service** (Dodaj usługę), wpisz ciąg **Jenkins** i wybierz pozycję **Jenkins-Github plugin** (Wtyczka Jenkins-Github).

   c. Wprowadź adres URL elementu webhook narzędzia Jenkins (domyślnie adres URL powinien być następujący: ``http://<PublicIPorFQDN>:8081/github-webhook/``). Kliknij pozycję **add/update service** (dodaj/aktualizuj usługę).

   d. Do wystąpienia narzędzia Jenkins zostanie wysłane zdarzenie testowe. W usłudze GitHub powinien zostać wyświetlony zielony znacznik wyboru obok elementu webhook i projekt zostanie skompilowany.

   e. W sekcji **Build Triggers** (Wyzwalacze kompilacji) wybierz odpowiednią opcję kompilacji. W tym przykładzie kompilacja ma być wyzwalana po każdej operacji wypychania do repozytorium. Dlatego wybierz pozycję **GitHub hook trigger for GITScm polling** (Wyzwalacz punktu zaczepienia GitHub na potrzeby sondowania GITScm). Wcześniej ta opcja miała nazwę **Build when a change is pushed to GitHub** (Kompiluj po wypchnięciu zmiany do usługi GitHub).

   f. W sekcji **Build** (Kompilacja) z listy rozwijanej **Add build step** (Dodaj krok kompilacji) wybierz opcję **Invoke Gradle Script** (Wywołaj skrypt Gradle). W wyświetlonym widżecie określ ścieżkę do lokalizacji **Root build script** (Główny skrypt kompilacji) dla aplikacji. Spowoduje to wybranie elementu build.gradle z określonej ścieżki i wykonanie odpowiedniego działania. W przypadku utworzenia projektu o nazwie ``MyActor`` (przy użyciu wtyczki środowiska Eclipse lub generatora Yeoman) główny skrypt kompilacji powinien zawierać wartość ``${WORKSPACE}/MyActor``. Na poniższym zrzucie ekranu przedstawiono przykład:

    ![Akcja kompilacji narzędzia Jenkins w usłudze Service Fabric][build-step]

   g. Z listy rozwijanej **Post-Build Actions** (Akcje po kompilacji) wybierz pozycję **Deploy Service Fabric Project** (Wdróż projekt usługi Service Fabric). W tym miejscu należy podać szczegóły dotyczące klastra, w ramach którego zostanie wdrożona skompilowana aplikacja usługi Service Fabric narzędzia Jenkins. Możesz także podać dodatkowe szczegóły dotyczące aplikacji służące do jej wdrożenia. Na poniższym zrzucie ekranu przedstawiono przykład:

    ![Akcja kompilacji narzędzia Jenkins w usłudze Service Fabric][post-build-step]

   > [!NOTE]
   > Określany w tym miejscu klaster może być tym samym klastrem, który hostuje aplikację kontenera narzędzia Jenkins, jeśli usługa Service Fabric jest używana do wdrożenia obrazu kontenera narzędzia Jenkins.
   >

## <a name="next-steps"></a>Następne kroki
Usługa GitHub i narzędzie Jenkins są teraz skonfigurowane. Rozważ dokonanie jakiejś przykładowej zmiany w Twoim projekcie ``MyActor`` w przykładzie repozytorium, https://github.com/sayantancs/SFJenkins. Wypchnij zmiany do zdalnej gałęzi ``master`` (lub dowolnej innej gałęzi skonfigurowanej do pracy). Spowoduje to wyzwolenie skonfigurowanego zadania narzędzia Jenkins ``MyJob``. Pobierze ono zmiany z usługi GitHub, skompiluje je i wdroży aplikację w punkcie końcowym klastra określonym w akcjach wykonywanych po kompilacji.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png

