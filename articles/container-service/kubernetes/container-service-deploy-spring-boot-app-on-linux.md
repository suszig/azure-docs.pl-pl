---
title: "Wdrażanie aplikacji sieci Web Spring rozruchu w systemie Linux w usłudze kontenera platformy Azure | Dokumentacja firmy Microsoft"
description: "Ten samouczek przedstawia jednak kroki, aby wdrożyć aplikację rozruchu Spring jako aplikacji sieci web systemu Linux w systemie Microsoft Azure."
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
ms.openlocfilehash: daa0ed81a6b9f20e146698947099a991da42cd6d
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="deploy-a-spring-boot-application-on-linux-in-the-azure-container-service"></a>Wdrażanie aplikacji Spring rozruchu w systemie Linux w usłudze kontenera platformy Azure

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

**[Spring Framework]**  to rozwiązanie open source, które pomaga deweloperom języka Java, tworzenie aplikacji na poziomie przedsiębiorstwa. Jeden z popularnych więcej projektów, które jest wbudowane znajdujący się na platformy jest [rozruchu Spring], zapewniające uproszczone podejście do tworzenia aplikacji Java autonomicznych.

**[Docker]**  jest rozwiązania open source, które pomaga deweloperom automatyzację wdrażania, skalowania i zarządzania ich aplikacji, które są uruchomione w kontenerach.

Ten samouczek przedstawia przy użyciu rozwiązania Docker do opracowywania i wdrażania aplikacji Spring rozruchu na hoście z systemem Linux w [usługi kontenera platformy Azure (ACS)].

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

W poniższych krokach objaśniono kroki, które są wymagane do tworzenia prostej aplikacji sieci web Spring rozruchu i przetestować go lokalnie.

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

1. Klonowanie [rozruchu Spring na wprowadzenie Docker] przykładowy projekt do katalogu utworzonego; na przykład:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Zmień katalog na ukończone projektu; na przykład:
   ```
   cd gs-spring-boot-docker/complete
   ```

1. Kompiluj plik JAR za pomocą programu Maven; na przykład:
   ```
   mvn package
   ```

1. Po utworzeniu aplikacji sieci web, zmień katalog na `target` katalog w którym plik JAR znajduje się uruchomić aplikacji sieci web, na przykład:
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. Testowanie aplikacji sieci web przy użyciu przeglądania lokalnie za pomocą przeglądarki sieci web. Na przykład jeśli masz curl dostępny i skonfigurowany serwer Tomcat do uruchomienia na porcie 80:
   ```
   curl http://localhost
   ```

1. Powinien zostać wyświetlony następujący komunikat wyświetlany: **Hello, World Docker!**

   ![Przejdź do przykładowej aplikacji lokalnie][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Tworzenie rejestru kontenera Azure ma być używana jako prywatnych rejestru Docker

W poniższych krokach objaśniono przy użyciu portalu Azure można utworzyć rejestru kontenera platformy Azure.

> [!NOTE]
>
> Jeśli chcesz użyć wiersza polecenia platformy Azure zamiast portalu Azure, postępuj zgodnie z instrukcjami [tworzenie prywatnych rejestru kontenera Docker przy użyciu programu Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).
>

1. Przejdź do [portalu Azure] i zaloguj się.

   Po zalogowaniu się do swojego konta w portalu Azure, możesz wykonać kroki opisane w [tworzenie prywatnych rejestru kontenera Docker przy użyciu portalu Azure] artykułu, które są paraphrased w poniższych krokach dla sake z praktycznych.

1. Kliknij ikonę menu **+ nowy**, następnie kliknij przycisk **kontenery**, a następnie kliknij przycisk **rejestru kontenera Azure**.
   
   ![Tworzenie nowego rejestru kontenera platformy Azure][AR01]

1. Po stronie informacje o szablonie rejestru kontenera Azure zostanie wyświetlony, kliknij przycisk **Utwórz**. 

   ![Tworzenie nowego rejestru kontenera platformy Azure][AR02]

1. Gdy **Utwórz kontener rejestru** zostanie wyświetlona strona, wprowadź Twojej **nazwa rejestru** i **grupy zasobów**, wybierz **włączyć** dla **Administrator**, a następnie kliknij przycisk **Utwórz**.

   ![Konfigurowanie ustawień rejestru kontenera platformy Azure][AR03]

1. Po utworzeniu kontenera rejestru przejdź do kontenera rejestru w portalu Azure, a następnie kliknij przycisk **klucze dostępu**. Zanotuj nazwę użytkownika i hasło dla następnych kroków.

   ![Klawisze dostępu rejestru kontenera platformy Azure][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a>Skonfiguruj Maven do używania kluczy dostępu rejestru kontenera platformy Azure

1. Przejdź do katalogu konfiguracji dla tej instalacji Maven i Otwórz *settings.xml* plik w edytorze tekstów.

1. Dodaj ustawienia dostępu do rejestru kontenera Azure opisanych w poprzedniej części tego samouczka do `<servers>` kolekcji w *settings.xml* pliku; na przykład:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Przejdź do katalogu projektu zakończone aplikacji Spring rozruchu, (na przykład: "*C:\SpringBoot\gs-spring-boot-docker\complete*"lub"*/users/robert/SpringBoot/gs-spring-boot-docker/complete* "), a następnie otwórz *pom.xml* plik w edytorze tekstów.

1. Aktualizacja `<properties>` kolekcji w *pom.xml* pliku z wartością serwera logowania do rejestru kontenera Azure opisanych w poprzedniej części tego samouczka; na przykład:

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Aktualizacja `<plugins>` kolekcji w *pom.xml* pliku, aby `<plugin>` zawiera logowania serwera adresu rejestru nazwę i rejestru kontenera Azure opisanych w poprzedniej części tego samouczka. Na przykład:

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

1. Przejdź do katalogu projektu zakończone aplikacji Spring rozruchu i uruchom następujące polecenie, aby ponownie skompilować aplikację i push kontenera do rejestru kontenera platformy Azure:

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> Gdy push z kontenera Docker na platformie Azure, otrzymasz komunikat o błędzie podobny do jednego z następujących nawet jeśli pomyślnie utworzono kontener programu Docker:
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> W takim przypadku należy zalogować się do konta platformy Azure z poziomu wiersza polecenia Docker; na przykład:
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Następnie możesz wypchnąć z kontenera w wierszu polecenia; na przykład:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a>Tworzenie aplikacji sieci web w systemie Linux w usłudze Azure App Service przy użyciu obrazu kontenera

1. Przejdź do [portalu Azure] i zaloguj się.

1. Kliknij ikonę menu **+ nowy**, następnie kliknij przycisk **sieci Web i mobilność**, a następnie kliknij przycisk **aplikacji sieci Web w systemie Linux**.
   
   ![Utwórz nową aplikację sieci web w portalu Azure][LX01]

1. Gdy **aplikacji sieci Web w systemie Linux** zostanie wyświetlona strona, wprowadź następujące informacje:

   a. Wprowadź unikatową nazwę **Nazwa aplikacji**, na przykład: "*wingtiptoyslinux*."

   b. Wybierz użytkownika **subskrypcji** z listy rozwijanej.

   c. Wybierz istniejące **grupy zasobów**, lub określ nazwę, aby utworzyć nową grupę zasobów.

   d. Kliknij przycisk **kontenera konfiguracji** i wprowadź następujące informacje:

      * Wybierz **prywatnej rejestru**.

      * **Obraz i opcjonalnie tag**: Określ nazwę kontenera z wcześniej, na przykład: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"

      * **Adres URL serwera**: Określ adres URL rejestru z wcześniej, na przykład: "*https://wingtiptoysregistry.azurecr.io*"

      * **Nazwa użytkownika logowania** i **hasło**: Określ poświadczenia logowania z użytkownika **klucze dostępu** używanego w poprzednich krokach.
   
   e. Po wprowadzeniu wszystkich powyższych informacji, kliknij przycisk **OK**.

   ![Konfiguruj ustawienia aplikacji sieci web][LX02]

1. Kliknij przycisk **Utwórz**.

> [!NOTE]
>
> Azure automatycznie przypisze żądań internetowych do osadzonego serwer Tomcat, który działa na standardowe porty 80 lub 8080. Jednak jeśli skonfigurowano serwer Tomcat osadzonych do uruchamiania na port niestandardowy, należy dodać zmienną środowiskową do aplikacji sieci web, który definiuje port serwera Tomcat osadzonych. Aby to zrobić, wykonaj następujące kroki:
>
> 1. Przejdź do [portalu Azure] i zaloguj się.
> 
> 2. Kliknij ikonę **usługi aplikacji**. (Zobacz element #1 na obrazie poniżej).
>
> 3. Wybierz swoją aplikację internetową z listy. (Element #2 na poniższym obrazie.)
>
> 4. Kliknij przycisk **ustawienia aplikacji**. (Element #3 na poniższym obrazie.)
>
> 5. W **ustawień aplikacji** Dodaj nową zmienną środowiskową o nazwie **portu** i wprowadź numer portu niestandardowego dla wartości. (Element #4 obraz poniżej.)
>
> 6. Kliknij pozycję **Zapisz**. (Element #5 na poniższej ilustracji.)
>
> ![Zapisywanie niestandardowy numer portu w portalu Azure][LX03]
>

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

Aby uzyskać więcej informacji o używaniu aplikacji rozruchu Spring na platformie Azure zobacz następujące artykuły:

* [Wdrażanie aplikacji rozruchu Spring w usłudze Azure App Service](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Wdrażanie aplikacji rozruchu Spring w klastrze Kubernetes w usłudze kontenera platformy Azure](container-service-deploy-spring-boot-app-on-kubernetes.md)

Aby uzyskać więcej informacji o używaniu platformy Azure z językiem Java, zobacz [Centrum deweloperów języka Java dla platformy Azure] i [Java Tools for Visual Studio Team Services] (Narzędzia języka Java dla usługi Visual Studio Team Services).

Aby uzyskać dalsze informacje na temat rozruchu Spring na Docker przykładowy projekt, zobacz [rozruchu Spring na wprowadzenie Docker].

Aby uzyskać pomoc dotyczącą Rozpoczynanie pracy z aplikacjami Spring rozruchu, zobacz **Spring Initializr** na https://start.spring.io/.

Aby uzyskać więcej informacji na temat rozpoczynania pracy z tworzenia prostej aplikacji Spring rozruchu Zobacz Initializr Spring na https://start.spring.io/.

Dodatkowe przykłady korzystania z platformy Azure, niestandardowe obrazy usługi Docker można znaleźć [dla aplikacji sieci Web platformy Azure w systemie Linux przy użyciu niestandardowego obrazu Docker].

<!-- URL List -->

[Azure interfejsu wiersza polecenia (CLI)]: /cli/azure/overview
[usługi kontenera platformy Azure (ACS)]: https://azure.microsoft.com/services/container-service/
[Centrum deweloperów języka Java dla platformy Azure]: https://azure.microsoft.com/develop/java/
[portalu Azure]: https://portal.azure.com/
[tworzenie prywatnych rejestru kontenera Docker przy użyciu portalu Azure]: /azure/container-registry/container-registry-get-started-portal
[dla aplikacji sieci Web platformy Azure w systemie Linux przy użyciu niestandardowego obrazu Docker]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[bezpłatne konto platformy Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[korzyści dla subskrybentów MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[rozruchu Spring]: http://projects.spring.io/spring-boot/
[rozruchu Spring na wprowadzenie Docker]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-linux/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-linux/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-linux/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-linux/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-linux/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-linux/AR04.png

[LX01]: ./media/container-service-deploy-spring-boot-app-on-linux/LX01.png
[LX02]: ./media/container-service-deploy-spring-boot-app-on-linux/LX02.png
[LX03]: ./media/container-service-deploy-spring-boot-app-on-linux/LX03.png
