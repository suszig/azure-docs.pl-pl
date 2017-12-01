---
title: "Ciągłe kompilacji i integracji aplikacji usługi Azure Service Fabric Linux przy użyciu Wpięć | Dokumentacja firmy Microsoft"
description: "Ciągłe kompilacji i integracji aplikacji usługi sieci szkieletowej w systemie Linux przy użyciu Wpięć"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/27/2017
ms.author: saysa
ms.openlocfilehash: 89b356c3959b7cb63a746805d60535e07f0d6898
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="use-jenkins-to-build-and-deploy-your-linux-applications"></a>Użyj Wpięć do tworzenia i wdrażania aplikacji systemu Linux
Jenkins to popularne narzędzie służące do przeprowadzania ciągłej integracji i ciągłego wdrażania aplikacji. Poniżej przedstawiono sposób kompilowania i wdrażania aplikacji usługi Azure Service Fabric przy użyciu narzędzia Jenkins.

## <a name="general-prerequisites"></a>Ogólne wymagania wstępne
- Lokalnie zainstalowane narzędzie Git. Odpowiednią wersję narzędzia Git dla używanego systemu operacyjnego można zainstalować ze [strony pobierania narzędzia Git](https://git-scm.com/downloads). Jeśli jesteś nowym użytkownikiem narzędzia Git, możesz się dowiedzieć o nim więcej z [jego dokumentacji](https://git-scm.com/docs).
- Przygotowana wtyczka narzędzia Jenkins dla usługi Service Fabric. Możesz ją pobrać z [witryny pobierania usługi Service Fabric](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi). Jeśli używasz rename przeglądarki edge rozszerzenia pobrany plik z rozszerzeniem .zip do .hpi.

## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Konfigurowanie narzędzia Jenkins w ramach klastra usługi Service Fabric

Narzędzie Jenkins możesz skonfigurować wewnątrz klastra usługi Service Fabric lub poza nim. Poniższe sekcje pokazują, jak je skonfigurować w klastrze podczas korzystania z konta magazynu platformy Azure można zapisać stanu wystąpienia kontenera.

### <a name="prerequisites"></a>Wymagania wstępne
1. Przygotuj klaster systemu Linux usługi Service Fabric. W klastrach usługi Service Fabric utworzonych z poziomu witryny Azure Portal jest już zainstalowana platforma Docker. Jeśli klaster jest uruchamiany lokalnie, za pomocą polecenia ``docker info`` sprawdź, czy platforma Docker jest zainstalowana. Jeśli nie jest zainstalowana, zainstaluj ją za pomocą następujących poleceń:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ``` 

   > [!NOTE]
   > Upewnij się, że 8081 port jest określony jako punktu końcowego niestandardowych w klastrze.
   >

2. Klonowanie aplikacji, korzystając z następujących czynności:
  ```sh
  git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
  cd service-fabric-java-getting-started/Services/JenkinsDocker/
  ```

3. Utrwalanie stanu kontenera Wpięć w udziale plików:
  * Tworzenie konta magazynu Azure w **tego samego regionu** co klaster o nazwie, takich jak ``sfjenkinsstorage1``.
  * Utwórz **udziału plików** w magazynie konta o nazwie takich jak ``sfjenkins``.
  * Polecenie **Connect** dla udziału plików i zanotuj wartości on wyświetla w obszarze **łączących się z systemem Linux**, wartość powinna wyglądać podobnie do poniższego:
  ```sh
  sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
  ```

  > [!NOTE]
  > Do instalacji cifs udziałów musisz z pakietem witryny cifs zainstalowanych w węzłach klastra.       
  >

4. Zaktualizuj symbole zastępcze w ```setupentrypoint.sh``` skryptu szczegóły usługi azure storage w kroku 3.
```sh
vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
```
  * Zastąp ``[REMOTE_FILE_SHARE_LOCATION]`` z wartością ``//sfjenkinsstorage1.file.core.windows.net/sfjenkins`` z danych wyjściowych Połącz w kroku 3.
  * Zastąp ``[FILE_SHARE_CONNECT_OPTIONS_STRING]`` z wartością ``vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777`` z kroku 3.

5. **Tylko bezpieczny klastra:** w celu skonfigurowania wdrożenia aplikacji w klastrze bezpiecznego z Wpięć, certyfikat musi być dostępny w kontenerze Wpięć. W systemie Linux klastrów certificates(PEM) są po prostu kopiowane z określonego przez X509StoreName na kontenerze magazynu. W ApplicationManifest w obszarze ContainerHostPolicies dodać to odwołanie certyfikatu i zaktualizuj wartość odcisku palca. Wartość musi być czy z certyfikatem przez siebie znajdującej się na węźle.
  ```xml
  <CertificateRef Name="MyCert" X509FindValue="[Thumbprint]"/>
  ```
  > [!NOTE]
  > Wartość musi być taka sama jak certyfikat, który jest używany do nawiązania bezpiecznego klastra. 
  >

6. Połącz się z klastrem i instalowanie aplikacji do kontenera.

  **Zabezpieczenia klastra**
  ```sh
  sfctl cluster select --endpoint https://PublicIPorFQDN:19080  --pem [Pem] --no-verify # cluster connect command
  bash Scripts/install.sh
  ```

  **Niezabezpieczone klastra**
  ```sh
  sfctl cluster select --endpoint http://PublicIPorFQDN:19080 # cluster connect command
  bash Scripts/install.sh
  ```

  To spowoduje zainstalowanie kontenera narzędzia Jenkins w klastrze i umożliwi monitorowanie za pomocą narzędzia Service Fabric Explorer.

    > [!NOTE]
    > Może upłynąć kilka minut dla obrazu Wpięć do pobrania w klastrze.
    >

### <a name="steps"></a>Kroki
1. W przeglądarce przejdź na adres ``http://PublicIPorFQDN:8081``. Tam jest dostępna ścieżka do początkowego hasła administratora wymaganego do zalogowania się. 
2. Spójrz na Service Fabric Explorer ustalenie, na który węzeł kontenera Wpięć jest uruchomiony. Secure Shell (SSH), logowanie do tego węzła.
```sh
ssh user@PublicIPorFQDN -p [port]
``` 
3. Uzyskaj identyfikator wystąpienia kontenera za pomocą polecenia ``docker ps -a``.
4. Zaloguj się do kontenera przy użyciu protokołu SSH (Secure Shell) i wklej ścieżkę wyświetloną w portalu narzędzia Jenkins. Jeśli na przykład w portalu jest wyświetlana ścieżka `PATH_TO_INITIAL_ADMIN_PASSWORD`, uruchom następujące polecenia:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  ```
  ```sh
  cat PATH_TO_INITIAL_ADMIN_PASSWORD # This displays the pasword value
  ```
5. Na stronie uruchomić pobierania Wpięć wybierz wybierz wtyczki do zainstalowania opcji, zaznacz **Brak** pole wyboru, a następnie kliknij przycisk Zainstaluj.
6. Utwórz użytkownika, lub zaznacz, aby kontynuować konta administratora.

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
  1. Ściągnij obraz kontenera narzędzia Jenkins usługi Service Fabric: ``docker pull rapatchi/jenkins:v9``
  2. Uruchom obraz kontenera: ``docker run -itd -p 8080:8080 rapatchi/jenkins:v9``
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
W tym miejscu możesz przekazać wtyczkę. Wybierz **wybierz plik**, a następnie wybierz **serviceFabric.hpi** pliku, który został pobrany w obszarze wymagania wstępne lub pobrać [tutaj](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi). Po wybraniu polecenia **Upload** (Przekaż) narzędzie Jenkins automatycznie zainstaluje wtyczkę. Zezwól na ponowne uruchomienie komputera, jeśli zostanie wyświetlony odpowiedni monit.

## <a name="create-and-configure-a-jenkins-job"></a>Tworzenie i konfigurowanie zadania narzędzia Jenkins

1. Utwórz **nowy element** z poziomu pulpitu nawigacyjnego.
2. Wprowadź nazwę elementu (na przykład **MyJob**). Wybierz pozycję **free-style project** (projekt w stylu dowolnym) i kliknij przycisk **OK**.
3. Przejdź na stronę zadania i kliknij pozycję **Configure** (Konfiguruj).

   a. W sekcji Ogólne, zaznacz pole wyboru **projektu GitHub**, i określ adres URL projektu usługi GitHub. Pod tym adresem URL jest hostowana aplikacja Java usługi Service Fabric, którą chcesz zintegrować z przepływem ciągłej integracji/ciągłego wdrażania narzędzia Jenkins (na przykład ``https://github.com/sayantancs/SFJenkins``).

   b. W obszarze **Source Code Management** (Zarządzanie kodem źródłowym) wybierz pozycję **Git**. Określ adres URL repozytorium hostującego aplikację Java usługi Service Fabric, którą chcesz zintegrować z przepływem ciągłej integracji/ciągłego wdrażania narzędzia Jenkins (na przykład ``https://github.com/sayantancs/SFJenkins.git``). W tym miejscu możesz również określić gałąź do skompilowania (na przykład **/master**).
4. Skonfiguruj usługę *GitHub* (która hostuje repozytorium), aby mogła komunikować się z narzędziem Jenkins. Wykonaj następujące czynności:

   a. Przejdź do strony repozytorium serwisu GitHub. Przejdź do pozycji **Settings** > **Integrations and Services** (Ustawienia > Integracje i usługi).

   b. Wybierz pozycję **Add Service** (Dodaj usługę), wpisz ciąg **Jenkins** i wybierz pozycję **Jenkins-Github plugin** (Wtyczka Jenkins-Github).

   c. Wprowadź adres URL elementu webhook narzędzia Jenkins (domyślnie adres URL powinien być następujący: ``http://<PublicIPorFQDN>:8081/github-webhook/``). Kliknij pozycję **add/update service** (dodaj/aktualizuj usługę).

   d. Do wystąpienia narzędzia Jenkins zostanie wysłane zdarzenie testowe. W usłudze GitHub powinien zostać wyświetlony zielony znacznik wyboru obok elementu webhook i projekt zostanie skompilowany.

   e. W sekcji **Build Triggers** (Wyzwalacze kompilacji) wybierz odpowiednią opcję kompilacji. W tym przykładzie kompilacja ma być wyzwalana po każdej operacji wypychania do repozytorium. Dlatego wybierz pozycję **GitHub hook trigger for GITScm polling** (Wyzwalacz punktu zaczepienia GitHub na potrzeby sondowania GITScm). Wcześniej ta opcja miała nazwę **Build when a change is pushed to GitHub** (Kompiluj po wypchnięciu zmiany do usługi GitHub).

   f. **Tworzenie aplikacji Java sekcji:** w obszarze **kompilacji sekcji**, z listy rozwijanej **kroku kompilacji Dodaj**, wybierz opcję **wywołanie skryptu narzędzia Gradle**. W elemencie widget wchodzącej, otwórz menu Zaawansowane, określ ścieżkę do **głównego kompilacji skryptu** aplikacji. Umożliwia ona przejmuje build.gradle z określoną ścieżkę i odpowiednio działa. W przypadku utworzenia projektu o nazwie ``MyActor`` (przy użyciu wtyczki środowiska Eclipse lub generatora Yeoman) główny skrypt kompilacji powinien zawierać wartość ``${WORKSPACE}/MyActor``. Na poniższym zrzucie ekranu przedstawiono przykład:

    ![Akcja kompilacji narzędzia Jenkins w usłudze Service Fabric][build-step]

   g. **Tworzenie sekcji dla aplikacji .net Core:** w obszarze **kompilacji sekcji**, z listy rozwijanej **kroku kompilacji Dodaj**, wybierz opcję **wykonywania powłoki**. W wyświetlonym oknie polecenia katalogu najpierw musi zostać zmienione na ścieżkę, w którym znajduje się plik build.sh. Po zmianie katalogu build.sh skrypt może działać i zostanie utworzona aplikacja.

      ```sh
      cd /var/jenkins_home/workspace/[Job Name]/[Path to build.sh]  # change directory to location of build.sh file
      ./build.sh
      ```

    Poniżej znajduje się exmaple poleceń, które są używane do tworzenia [usługi licznika](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started/tree/master/Services/CounterService) Przykładowa nazwa zadania Wpięć CounterServiceApplication.

    ![Akcja kompilacji narzędzia Jenkins w usłudze Service Fabric][build-step-dotnet]
  
   h. Z listy rozwijanej **Post-Build Actions** (Akcje po kompilacji) wybierz pozycję **Deploy Service Fabric Project** (Wdróż projekt usługi Service Fabric). W tym miejscu należy podać szczegóły dotyczące klastra, w ramach którego zostanie wdrożona skompilowana aplikacja usługi Service Fabric narzędzia Jenkins. Ścieżkę do certyfikatu można znaleźć przez wyświetlanie wartość echo zmiennej środowiskowej Certificates_JenkinsOnSF_Code_MyCert_PEM od w kontenerze. Ta ścieżka może służyć do pól certyfikatu klienta i klucz klienta.

      ```sh
      echo $Certificates_JenkinsOnSF_Code_MyCert_PEM
      ```
   
    Możesz także podać dodatkowe szczegóły dotyczące aplikacji służące do jej wdrożenia. Na poniższym zrzucie ekranu przedstawiono przykład:

    ![Akcja kompilacji narzędzia Jenkins w usłudze Service Fabric][post-build-step]

      > [!NOTE]
      > Określany w tym miejscu klaster może być tym samym klastrem, który hostuje aplikację kontenera narzędzia Jenkins, jeśli usługa Service Fabric jest używana do wdrożenia obrazu kontenera narzędzia Jenkins.
      >

## <a name="next-steps"></a>Następne kroki
Usługa GitHub i narzędzie Jenkins są teraz skonfigurowane. Rozważ dokonanie jakiejś przykładowej zmiany w Twoim projekcie ``MyActor`` w przykładzie repozytorium, https://github.com/sayantancs/SFJenkins. Wypchnij zmiany do zdalnej gałęzi ``master`` (lub dowolnej innej gałęzi skonfigurowanej do pracy). Spowoduje to wyzwolenie skonfigurowanego zadania narzędzia Jenkins ``MyJob``. Pobierze ono zmiany z usługi GitHub, skompiluje je i wdroży aplikację w punkcie końcowym klastra określonym w akcjach wykonywanych po kompilacji.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step.png
  [build-step-dotnet]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step-dotnet.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-step.png

