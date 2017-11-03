---
title: "Jak używać wtyczki Maven dla aplikacji sieci Web dla kontenerów wdrażania konteneryzowanych aplikacji Spring rozruchu w systemie Azure"
description: "Dowiedz się, jak używać wtyczki Maven dla aplikacji sieci Web dla kontenerów do wdrożenia aplikacji rozruchu Spring na platformie Azure."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 0329aa9b88c7542ab3235a104a0652cd217ff872
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="how-to-use-the-maven-plugin-for-web-app-for-containers-to-deploy-a-containerized-spring-boot-app-to-azure"></a>Jak używać wtyczki Maven dla aplikacji sieci Web dla kontenerów wdrażania konteneryzowanych aplikacji Spring rozruchu w systemie Azure

[Maven wtyczki dla aplikacji sieci Web dla kontenerów](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) dla [Apache Maven](http://maven.apache.org/) zapewnia bezproblemową integrację usługi Azure App Service w projekty Maven oraz usprawnia proces deweloperom wdrażanie aplikacji sieci web do Usługa aplikacji Azure.

W tym artykule przedstawiono wdrażanie przykładowej aplikacji Spring rozruchu w kontenerze Docker do aplikacji sieci Web dla kontenerów, za pomocą wtyczki Maven dla aplikacji sieci Web dla kontenerów.

> [!NOTE]
>
> Dodatek plug-in Maven dla aplikacji sieci Web dla kontenerów jest obecnie dostępna jako podgląd. Obecnie jest obsługiwana tylko publikacji FTP, chociaż dodatkowe funkcje są planowane w przyszłości.
>

## <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać kroki opisane w tym samouczku, musisz mieć następujące wymagania wstępne:

* Subskrypcja platformy Azure; Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz przeprowadzić aktywację Twojej [korzyści dla subskrybentów MSDN] lub zarejestrować się w celu [bezpłatne konto platformy Azure].
* [Azure interfejsu wiersza polecenia (CLI)].
* Aktualne [Java Development Kit (JDK)], wersja 1.7 lub nowszej.
* Apache na [Maven] (w wersji 3) Narzędzie kompilacji.
* A [Git] klienta.
* A [Docker] klienta.

> [!NOTE]
>
> Ze względu na wymagania dotyczące wirtualizacji w tym samouczku nie można wykonać kroki opisane w tym artykule na maszynie wirtualnej; należy użyć komputera fizycznego z włączonymi funkcjami wirtualizacji.
>

## <a name="clone-the-sample-spring-boot-application"></a>Klonowanie przykładowej aplikacji Spring rozruchu

W tej sekcji sklonować konteneryzowanych aplikacji rozruchu Spring i przetestować go lokalnie.

1. Otwórz wiersz polecenia lub okno terminalu i utworzyć katalogu lokalnego do przechowywania aplikacji Spring rozruchu, przejdź do tego katalogu; na przykład:
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   --lub--
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Klonowanie [Spring rozruch z Docker] przykładowy projekt do katalogu, który utworzono; na przykład:
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
   ```

1. Zmień katalog na ukończone projektu; na przykład:
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. Kompiluj plik JAR za pomocą programu Maven; na przykład:
   ```shell
   mvn clean package
   ```

1. Po utworzeniu aplikacji sieci web, należy uruchomić aplikację sieci web za pomocą programu Maven; na przykład:
   ```shell
   mvn spring-boot:run
   ```

1. Testowanie aplikacji sieci web przy użyciu przeglądania lokalnie za pomocą przeglądarki sieci web. Na przykład można użyć następującego polecenia Jeśli masz curl dostępne:
   ```shell
   curl http://localhost:8080
   ```

1. Powinien zostać wyświetlony następujący komunikat wyświetlany: **Hello, World Docker**

## <a name="create-an-azure-service-principal"></a>Tworzenie nazwy głównej usługi platformy Azure

W tej sekcji utworzysz Azure nazwy głównej usługi używającej wtyczki Maven podczas wdrażania z kontenera na platformie Azure.

1. Otwórz wiersz polecenia.

1. Zaloguj się do konta platformy Azure przy użyciu wiersza polecenia platformy Azure:
   ```shell
   az login
   ```
   Postępuj zgodnie z instrukcjami, aby ukończyć proces logowania.

1. Tworzenie nazwy głównej usługi platformy Azure:
   ```shell
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   Gdzie `uuuuuuuu` jest nazwą użytkownika i `pppppppp` jest hasło dla nazwy głównej usługi.

1. Azure odpowiada JSON, który podobnego do następującego:
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > Po skonfigurowaniu dodatek plug-in Maven, aby wdrożyć z kontenera na platformie Azure, będzie używać wartości z tej odpowiedzi JSON. `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, I `tttttttt` są symbole zastępcze, które są używane w tym przykładzie ułatwiające Mapuj tych wartości do ich odpowiednich elementów, podczas konfigurowania programu Maven `settings.xml` pliku w następnej sekcji.
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a>Skonfiguruj Maven do użycia z nazwy głównej usługi Azure

W tej sekcji możesz Użyj wartości od podmiotu zabezpieczeń usługi Azure można skonfigurować uwierzytelniania, które Maven będzie używane podczas wdrażania z kontenera na platformie Azure.

1. Otwieranie programu Maven `settings.xml` plik w edytorze tekstów; ten plik może być w ścieżce, takie jak następujące przykłady:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Dodaj ustawienia głównych usług Azure opisanych w poprzedniej części tego samouczka do `<servers>` kolekcji w *settings.xml* pliku; na przykład:

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   Gdzie:
   Element | Opis
   ---|---|---
   `<id>` | Określa unikatową nazwę Maven używanym w celu wyszukania ustawień zabezpieczeń podczas wdrażania aplikacji sieci web na platformie Azure.
   `<client>` | Zawiera `appId` wartość z Twojej nazwy głównej usługi.
   `<tenant>` | Zawiera `tenant` wartość z Twojej nazwy głównej usługi.
   `<key>` | Zawiera `password` wartość z Twojej nazwy głównej usługi.
   `<environment>` | Definiuje środowisko chmury Azure docelowych, które jest `AZURE` w tym przykładzie. (Pełna lista środowisk jest dostępna w [Maven wtyczki dla aplikacji sieci Web dla kontenerów] dokumentacji)

1. Zapisz i Zamknij *settings.xml* pliku.

## <a name="optional-deploy-your-local-docker-file-to-docker-hub"></a>OPCJONALNIE: Lokalny plik Docker wdrożyć do Centrum Docker

Jeśli masz konto Docker można kompilacji programu Docker kontener obrazu lokalnie i wypchnąć go do Centrum Docker. Aby to zrobić, wykonaj następujące kroki.

1. Otwórz `pom.xml` pliku rozruchowego Spring aplikacji w edytorze tekstów.

1. Zlokalizuj `<imageName>` elementem podrzędnym `<containerSettings>` elementu.

1. Aktualizacja `${docker.image.prefix}` wartości z Docker nazwa konta:
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. Wybierz jedną z następujących metod wdrażania:

   * Tworzenie obrazu kontenera lokalnie z Maven, a następnie użyj Docker do dystrybuowania z kontenera do Centrum Docker:
      ```shell
      mvn clean package docker:build
      docker push
      ```

   * Jeśli masz [wtyczki Docker przypadku Maven] zainstalowana, można automatycznie utworzyć i z kontenera obrazu do Centrum Docker przy użyciu `-DpushImage` parametru:
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-to-azure"></a>OPCJONALNIE: Dostosowywanie Twojej pom.xml przed wdrożeniem z kontenera na platformie Azure

Otwórz `pom.xml` pliku aplikacji Spring rozruchu w edytorze tekstu, a następnie zlokalizuj `<plugin>` elementu `azure-webapp-maven-plugin`. Ten element powinien wyglądać następująco:

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>maven-plugin</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

Istnieje kilka wartości, które można modyfikować dla wtyczki Maven i szczegółowy opis każdego z tych elementów jest dostępna w [Maven wtyczki dla aplikacji sieci Web dla kontenerów] dokumentacji. Który trwa wspomniano, istnieje kilka wartości, które warto wyróżnianie w tym artykule:

Element | Opis
---|---|---
`<version>` | Określa wersję [Maven wtyczki dla aplikacji sieci Web dla kontenerów]. Należy sprawdzić wersji na liście [Maven centralnym repozytorium](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) aby upewnić się, że używasz najnowszej wersji.
`<authentication>` | Określa informacje dotyczące uwierzytelniania dla platformy Azure, który w tym przykładzie zawiera `<serverId>` element, który zawiera `azure-auth`; Maven używa tej wartości do wyszukania nazwy głównej usługi Azure wartości w Twojej Maven *settings.xml* pliku, który został zdefiniowany w wcześniejszej części tego artykułu.
`<resourceGroup>` | Określa, docelowa grupa zasobów, która jest `maven-plugin` w tym przykładzie. Nazwa grupy zasobów zostanie utworzony podczas wdrażania, jeśli jeszcze nie istnieje.
`<appName>` | Określa docelową nazwę dla aplikacji sieci web. W tym przykładzie nazwa docelowa jest `maven-linux-app-${maven.build.timestamp}`, gdzie `${maven.build.timestamp}` jest do niej dołączany sufiks w tym przykładzie, aby uniknąć konfliktu. (Sygnatura czasowa jest opcjonalny; można określić dowolny unikatowy ciąg dla nazwy aplikacji).
`<region>` | Określa region docelowy, który w tym przykładzie jest `westus`. (Pełna lista jest w [Maven wtyczki dla aplikacji sieci Web dla kontenerów] dokumentacji.)
`<appSettings>` | Określa unikatowy ustawienia Maven do użycia podczas wdrażania aplikacji sieci web na platformie Azure. W tym przykładzie `<property>` element zawiera pary nazwa/wartość elementów podrzędnych, które określić port dla aplikacji.

> [!NOTE]
>
> Ustawienia, aby zmienić numer portu w tym przykładzie jest konieczne tylko w przypadku, gdy zmieniasz portu domyślnego.
>

## <a name="build-and-deploy-your-container-to-azure"></a>Tworzenie i wdrażanie z kontenera na platformie Azure

Po skonfigurowaniu wszystkich ustawień w poprzednich sekcjach tego artykułu, można przystąpić do wdrażania z kontenera na platformie Azure. Aby to zrobić, wykonaj następujące kroki:

1. Z wiersza polecenia lub okno terminalu, które były wcześniej używane, skompiluj ponownie plik JAR za pomocą programu Maven Jeśli wprowadzono zmiany do *pom.xml* pliku; na przykład:
   ```shell
   mvn clean package
   ```

1. Wdrażanie aplikacji sieci web na platformie Azure przy użyciu narzędzia Maven; na przykład:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven zostanie wdrożona aplikacja sieci web na platformie Azure; Jeśli aplikacja sieci web nie istnieje, zostanie utworzona.

> [!NOTE]
>
> Jeśli region, który jest określony w `<region>` elementu z *pom.xml* plik nie ma wystarczającej liczby serwerów, które są dostępne po uruchomieniu wdrożenia, można napotkać błąd podobny do poniższego przykładu:
>
> ```bash
> [INFO] Start deploying to Web App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed to execute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> W takim przypadku można określić innego regionu i uruchom ponownie polecenie narzędzia Maven, aby wdrożyć aplikację.
>
>

Po wdrożeniu sieci web będzie można zarządzać nim za pomocą [portalu Azure].

* Aplikacja sieci web zostaną wyświetlone w **usługi aplikacji**:

   ![Aplikacja sieci Web wyświetlane w portalu Azure usługi aplikacji][AP01]

* I adres URL aplikacji sieci web zostaną wyświetlone w **omówienie** dla aplikacji sieci web:

   ![Określanie adresu URL dla aplikacji sieci web][AP02]

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat różne technologie omówione w tym artykule zobacz następujące artykuły:

* [Maven wtyczki dla aplikacji sieci Web dla kontenerów]

* [Logowanie do platformy Azure z interfejsu wiersza polecenia platformy Azure](/azure/xplat-cli-connect)

* [Jak używać wtyczki Maven dla aplikacji sieci Web dla kontenerów, można wdrożyć aplikacji Spring rozruchu w usłudze Azure App Service w systemie Linux](../app-service-web-deploy-spring-boot-app-with-maven-plugin.md)

* [Tworzenie nazwy głównej usługi platformy Azure z 2.0 interfejsu wiersza polecenia platformy Azure](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Informacje o ustawieniach maven](https://maven.apache.org/settings.html)

* [wtyczki Docker przypadku Maven]

<!-- URL List -->

[Azure interfejsu wiersza polecenia (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[portalu Azure]: https://portal.azure.com/
[Docker]: https://www.docker.com/
[wtyczki Docker przypadku Maven]: https://github.com/spotify/docker-maven-plugin
[bezpłatne konto platformy Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[korzyści dla subskrybentów MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Maven wtyczki dla aplikacji sieci Web dla kontenerów]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP02.png
