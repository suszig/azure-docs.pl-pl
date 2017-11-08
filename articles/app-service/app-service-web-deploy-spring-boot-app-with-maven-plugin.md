---
title: "Wdrażanie aplikacji rozruchu Spring na platformie Azure przy użyciu wtyczki Maven dla aplikacji sieci Web Azure"
description: "Informacje o sposobie wdrożenia aplikacji rozruchu Spring na platformie Azure za pomocą wtyczki Maven dla aplikacji sieci Web platformy Azure."
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
ms.openlocfilehash: f22089349e149d8e71df88d5c173869d2a5abad7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-to-azure"></a>Wdrażanie aplikacji rozruchu Spring na platformie Azure przy użyciu wtyczki Maven dla aplikacji sieci Web Azure

[Maven wtyczki dla aplikacji sieci Web Azure](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) dla [Apache Maven](http://maven.apache.org/) zapewnia bezproblemową integrację usługi Azure App Service w projekty Maven oraz usprawnia proces deweloperom wdrażanie aplikacji sieci web w usłudze Azure App Service.

W tym artykule przedstawiono wdrażanie rozruchu Spring przykładowej aplikacji do usługi aplikacji Azure za pomocą wtyczki Maven dla aplikacji sieci Web platformy Azure.

> [!NOTE]
>
> Dodatek plug-in Maven dla aplikacji sieci Web Azure jest obecnie dostępna jako podgląd. Obecnie jest obsługiwana tylko publikacji FTP, chociaż dodatkowe funkcje są planowane w przyszłości.
>

## <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać kroki opisane w tym samouczku, musisz mieć następujące wymagania wstępne:

* Subskrypcja platformy Azure; Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz przeprowadzić aktywację Twojej [korzyści dla subskrybentów MSDN] lub zarejestrować się w celu [bezpłatne konto platformy Azure].
* [Azure interfejsu wiersza polecenia (CLI)].
* Aktualne [Java Development Kit (JDK)], wersja 1.7 lub nowszej.
* Apache na [Maven] (w wersji 3) Narzędzie kompilacji.
* A [Git] klienta.

## <a name="clone-the-sample-spring-boot-web-app"></a>Klonowanie przykładową aplikację sieci web Spring rozruchu

W tej sekcji sklonować ukończona aplikacja Spring rozruchu i przetestować go lokalnie.

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

1. Klonowanie [Spring wprowadzenie rozruchu] przykładowy projekt do katalogu utworzonego; na przykład:
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
   ```

1. Zmień katalog na ukończone projektu; na przykład:
   ```shell
   cd gs-spring-boot/complete
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

1. Powinien zostać wyświetlony następujący komunikat wyświetlany: **pozdrowienia z Spring rozruchowe!**

## <a name="create-an-azure-service-principal"></a>Tworzenie nazwy głównej usługi platformy Azure

W tej sekcji utworzysz Azure nazwy głównej usługi, który wtyczki Maven używa podczas wdrażania aplikacji sieci web na platformie Azure.

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
   > Po skonfigurowaniu dodatek plug-in Maven, aby wdrożyć aplikację sieci web na platformie Azure, będzie używać wartości z tej odpowiedzi JSON. `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, I `tttttttt` są symbole zastępcze, które są używane w tym przykładzie ułatwiające Mapuj tych wartości do ich odpowiednich elementów, podczas konfigurowania programu Maven `settings.xml` pliku w następnej sekcji.
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a>Skonfiguruj Maven do użycia z nazwy głównej usługi Azure

W tej sekcji możesz Użyj wartości od podmiotu zabezpieczeń usługi Azure można skonfigurować uwierzytelniania, która Maven używa podczas wdrażania aplikacji sieci web na platformie Azure.

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
   `<environment>` | Definiuje środowisko chmury Azure docelowych, które jest `AZURE` w tym przykładzie. (Pełna lista środowisk jest dostępna w [Maven wtyczki dla aplikacji sieci Web Azure] dokumentacji)

1. Zapisz i Zamknij *settings.xml* pliku.

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-to-azure"></a>OPCJONALNIE: Dostosowywanie programu pom.xml przed wdrożeniem aplikacji sieci web na platformie Azure

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
         <appName>maven-web-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>ftp</deploymentType>
         <resources>
            <resource>
               <directory>${project.basedir}/target</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>*.jar</include>
               </includes>
            </resource>
            <resource>
               <directory>${project.basedir}</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>web.config</include>
               </includes>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```

Istnieje kilka wartości, które można modyfikować dla wtyczki Maven i szczegółowy opis każdego z tych elementów jest dostępna w [Maven wtyczki dla aplikacji sieci Web Azure] dokumentacji. Który trwa wspomniano, istnieje kilka wartości, które warto wyróżnianie w tym artykule:

Element | Opis
---|---|---
`<version>` | Określa wersję [Maven wtyczki dla aplikacji sieci Web Azure]. Należy sprawdzić wersji na liście [Maven centralnym repozytorium](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) aby upewnić się, że używasz najnowszej wersji.
`<authentication>` | Określa informacje dotyczące uwierzytelniania dla platformy Azure, który w tym przykładzie zawiera `<serverId>` element, który zawiera `azure-auth`; Maven używa tej wartości do wyszukania nazwy głównej usługi Azure wartości w Twojej Maven *settings.xml* pliku, który został zdefiniowany w wcześniejszej części tego artykułu.
`<resourceGroup>` | Określa, docelowa grupa zasobów, która jest `maven-plugin` w tym przykładzie. Grupa zasobów jest tworzona podczas wdrażania, jeśli jeszcze nie istnieje.
`<appName>` | Określa docelową nazwę dla aplikacji sieci web. W tym przykładzie nazwa docelowa jest `maven-web-app-${maven.build.timestamp}`, gdzie `${maven.build.timestamp}` jest do niej dołączany sufiks w tym przykładzie, aby uniknąć konfliktu. (Sygnatura czasowa jest opcjonalny; można określić dowolny unikatowy ciąg dla nazwy aplikacji).
`<region>` | Określa region docelowy, który w tym przykładzie jest `westus`. (Pełna lista jest w [Maven wtyczki dla aplikacji sieci Web Azure] dokumentacji.)
`<javaVersion>` | Określa wersję środowiska uruchomieniowego języka Java dla aplikacji sieci web. (Pełna lista jest w [Maven wtyczki dla aplikacji sieci Web Azure] dokumentacji.)
`<deploymentType>` | Określa typ wdrożenia dla aplikacji sieci web. Obecnie tylko `ftp` jest obsługiwana, chociaż obsługuje inne typy wdrożenia jest programowanie.
`<resources>` | Określa zasobów i docelowy miejsc docelowych, które Maven używa podczas wdrażania aplikacji sieci web na platformie Azure. W tym przykładzie dwa `<resource>` elementy Określ, czy Maven wdroży plik JAR dla aplikacji sieci web i *web.config* plik z projektu Spring rozruchu.

## <a name="build-and-deploy-your-web-app-to-azure"></a>Tworzenie i wdrażanie aplikacji sieci web na platformie Azure

Po skonfigurowaniu wszystkich ustawień w poprzednich sekcjach tego artykułu, można przystąpić do wdrażania aplikacji sieci web na platformie Azure. Aby to zrobić, wykonaj następujące kroki:

1. Z wiersza polecenia lub okno terminalu, które były wcześniej używane, skompiluj ponownie plik JAR za pomocą programu Maven Jeśli wprowadzono zmiany do *pom.xml* pliku; na przykład:
   ```shell
   mvn clean package
   ```

1. Wdrażanie aplikacji sieci web na platformie Azure przy użyciu narzędzia Maven; na przykład:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven zostanie wdrożona aplikacja sieci web na platformie Azure; Jeśli aplikacja sieci web nie istnieje, zostanie utworzona.

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

* [Maven wtyczki dla aplikacji sieci Web Azure]

* [Logowanie do platformy Azure z interfejsu wiersza polecenia platformy Azure](/azure/xplat-cli-connect)

* [Jak używać wtyczki Maven dla aplikacji sieci Web Azure wdrażania konteneryzowanych aplikacji Spring rozruchu w systemie Azure](containers/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin.md)

* [Tworzenie nazwy głównej usługi platformy Azure z 2.0 interfejsu wiersza polecenia platformy Azure](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Informacje o ustawieniach maven](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure interfejsu wiersza polecenia (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[portalu Azure]: https://portal.azure.com/
[bezpłatne konto platformy Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[korzyści dla subskrybentów MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring wprowadzenie rozruchu]: https://github.com/microsoft/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven wtyczki dla aplikacji sieci Web Azure]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP02.png
