---
title: "Jak używać wtyczki Maven dla aplikacji sieci Web platformy Azure można wdrożyć aplikacji Spring rozruchu w rejestrze kontenera platformy Azure w usłudze Azure App Service"
description: "W tym samouczku opisano jednak kroki, aby wdrożyć aplikację Spring rozruchu w rejestrze kontenera Azure Azure do usługi Azure App Service przy użyciu wtyczki Maven."
services: 
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: b087003b3a1e236e4a306678904107b8bf99395e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-in-azure-container-registry-to-azure-app-service"></a>Jak używać wtyczki Maven dla aplikacji sieci Web platformy Azure można wdrożyć aplikacji Spring rozruchu w rejestrze kontenera platformy Azure w usłudze Azure App Service

**[Spring Framework]**  popularnych struktura open source, który pomaga Java deweloperom tworzenie aplikacji interfejsu API, mobilne i sieci web. W tym samouczku używana przykładowej aplikacji utworzony za pomocą [rozruchu Spring], podejście oparte na Konwencji do szybkie rozpoczęcie pracy przy użyciu źródła.

W tym artykule przedstawiono sposób wdrażania przykładowej aplikacji Spring rozruchu w rejestrze kontenera Azure i następnie wdrażanie aplikacji w usłudze Azure App Service za pomocą wtyczki Maven dla aplikacji sieci Web platformy Azure.

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
* A [Docker] klienta.

> [!NOTE]
>
> Ze względu na wymagania dotyczące wirtualizacji w tym samouczku nie można wykonać kroki opisane w tym artykule na maszynie wirtualnej; należy użyć komputera fizycznego z włączonymi funkcjami wirtualizacji.
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a>Klonowanie próbki Spring rozruchu w aplikacji sieci web Docker

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

1. Klonowanie [rozruchu Spring na wprowadzenie Docker] przykładowy projekt do katalogu utworzonego; na przykład:
   ```shell
   git clone -b private-registry https://github.com/Microsoft/gs-spring-boot-docker
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

   ![Przejdź do przykładowej aplikacji lokalnie][SB01]

## <a name="create-an-azure-service-principal"></a>Tworzenie nazwy głównej usługi platformy Azure

W tej sekcji utworzysz Azure nazwy głównej usługi używającej wtyczki Maven podczas wdrażania z kontenera na platformie Azure.

1. Otwórz wiersz polecenia.

1. Zaloguj się do konta platformy Azure przy użyciu wiersza polecenia platformy Azure:
   ```azurecli
   az login
   ```
   Postępuj zgodnie z instrukcjami, aby ukończyć proces logowania.

1. Tworzenie nazwy głównej usługi platformy Azure:
   ```azurecli
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

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>Tworzenie rejestru kontenera Azure za pomocą wiersza polecenia platformy Azure

1. Otwórz wiersz polecenia.

1. Zaloguj się do konta platformy Azure:
   ```azurecli
   az login
   ```

1. Utwórz grupę zasobów dla zasobów platformy Azure, które będą używane w tym artykule:
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   Zastąp `wingtiptoysresources` w tym przykładzie z unikatową nazwę grupy zasobów.

1. Utwórz rejestru prywatnej kontenera platformy Azure w grupie zasobów dla aplikacji Spring rozruchu: 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   Zastąp `wingtiptoysregistry` w tym przykładzie z unikatową nazwę rejestru kontenera.

1. Pobieranie hasła dla kontenera rejestru:
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   Azure będzie odpowiadać przy użyciu hasła; na przykład:
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-to-your-maven-settings"></a>Dodaj rejestru kontenera platformy Azure i nazwę główną usługi Azure w ustawieniach Maven

1. Otwieranie programu Maven `settings.xml` plik w edytorze tekstów; ten plik może być w ścieżce, takie jak następujące przykłady:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Dodawanie ustawień dostępu do rejestru kontenera platformy Azure z poprzedniej sekcji tego artykułu, aby `<servers>` kolekcji w *settings.xml* pliku; na przykład:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   Gdzie:
   Element | Opis
   ---|---|---
   `<id>` | Zawiera nazwę rejestru prywatnej kontenera platformy Azure.
   `<username>` | Zawiera nazwę rejestru prywatnej kontenera platformy Azure.
   `<password>` | Zawiera hasło, które są pobierane w poprzedniej sekcji tego artykułu.

1. Dodaj ustawienia podmiotu zabezpieczeń usługi Azure z wcześniejszej sekcji tego artykułu, aby `<servers>` kolekcji w *settings.xml* pliku; na przykład:

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

## <a name="build-your-docker-container-image-and-push-it-to-your-azure-container-registry"></a>Tworzenie obrazu kontenera programu Docker i wypchnąć go do rejestru kontenera platformy Azure

1. Przejdź do katalogu projektu zakończone aplikacji Spring rozruchu, (np. "*C:\SpringBoot\gs-spring-boot-docker\complete*"lub"*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), a następnie otwórz *pom.xml* plik tekstowy Edytor.

1. Aktualizacja `<properties>` kolekcji w *pom.xml* pliku z wartością serwera logowania do rejestru kontenera Azure opisanych w poprzedniej części tego samouczka; na przykład:

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   Gdzie:
   Element | Opis
   ---|---|---
   `<azure.containerRegistry>` | Określa nazwę rejestru prywatnej kontenera platformy Azure.
   `<docker.image.prefix>` | Określa adres URL rejestru prywatnej kontenera platformy Azure, w którym jest uzyskiwany w wyniku dołączania ". azurecr.io" do nazwy rejestru Kontener prywatny.

1. Sprawdź, czy `<plugin>` dla wtyczki Docker w Twojej *pom.xml* plik zawiera prawidłowe właściwości logowania adres i rejestru nazwy serwera z poprzedniego kroku, w tym samouczku. Na przykład:

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   Gdzie:
   Element | Opis
   ---|---|---
   `<serverId>` | Określa właściwość, która zawiera nazwę rejestru prywatnej kontenera platformy Azure.
   `<registryUrl>` | Określa właściwość, która zawiera adres URL rejestru prywatnej kontenera platformy Azure.

1. Przejdź do katalogu projektu zakończone aplikacji Spring rozruchu i uruchom następujące polecenie, aby ponownie skompilować aplikację i push kontenera do rejestru kontenera platformy Azure:

   ```
   mvn package docker:build -DpushImage 
   ```

1. OPCJONALNIE: Przejdź do [portalu Azure] i sprawdź, czy jest obraz kontenera Docker o nazwie **gs-spring — rozruchu — docker** w rejestrze kontenera.

   ![Sprawdź kontenera w portalu Azure][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-to-azure"></a>Dostosowywanie programu pom.xml, a następnie tworzenie i wdrażanie z kontenera na platformie Azure

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
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
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

Istnieje kilka wartości, które można modyfikować dla wtyczki Maven i szczegółowy opis każdego z tych elementów jest dostępna w [Maven wtyczki dla aplikacji sieci Web Azure] dokumentacji. Który trwa wspomniano, istnieje kilka wartości, które warto wyróżnianie w tym artykule:

Element | Opis
---|---|---
`<version>` | Określa wersję [Maven wtyczki dla aplikacji sieci Web Azure]. Należy sprawdzić wersji na liście [Maven centralnym repozytorium](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) aby upewnić się, że używasz najnowszej wersji.
`<authentication>` | Określa informacje dotyczące uwierzytelniania dla platformy Azure, który w tym przykładzie zawiera `<serverId>` element, który zawiera `azure-auth`; Maven używa tej wartości do wyszukania nazwy głównej usługi Azure wartości w Twojej Maven *settings.xml* pliku, który został zdefiniowany w wcześniejszej części tego artykułu.
`<resourceGroup>` | Określa, docelowa grupa zasobów, która jest `wingtiptoysresources` w tym przykładzie. Nazwa grupy zasobów zostanie utworzony podczas wdrażania, jeśli jeszcze nie istnieje.
`<appName>` | Określa docelową nazwę dla aplikacji sieci web. W tym przykładzie nazwa docelowa jest `maven-linux-app-${maven.build.timestamp}`, gdzie `${maven.build.timestamp}` jest do niej dołączany sufiks w tym przykładzie, aby uniknąć konfliktu. (Sygnatura czasowa jest opcjonalny; można określić dowolny unikatowy ciąg dla nazwy aplikacji).
`<region>` | Określa region docelowy, który w tym przykładzie jest `westus`. (Pełna lista jest w [Maven wtyczki dla aplikacji sieci Web Azure] dokumentacji.)
`<containerSettings>` | Określa właściwości, które zawierają nazwę i adres URL z kontenera.
`<appSettings>` | Określa unikatowy ustawienia Maven do użycia podczas wdrażania aplikacji sieci web na platformie Azure. W tym przykładzie `<property>` element zawiera pary nazwa/wartość elementów podrzędnych, które określić port dla aplikacji.

> [!NOTE]
>
> Ustawienia, aby zmienić numer portu w tym przykładzie jest konieczne tylko w przypadku, gdy zmieniasz portu domyślnego.
>

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
> ```
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

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat różne technologie omówione w tym artykule zobacz następujące artykuły:

* [Maven wtyczki dla aplikacji sieci Web Azure]

* [Logowanie do platformy Azure z interfejsu wiersza polecenia platformy Azure](/azure/xplat-cli-connect)

* [Tworzenie nazwy głównej usługi platformy Azure z 2.0 interfejsu wiersza polecenia platformy Azure](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Informacje o ustawieniach maven](https://maven.apache.org/settings.html)

* [Dodatek docker przypadku Maven]

<!-- URL List -->

[Azure interfejsu wiersza polecenia (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[portalu Azure]: https://portal.azure.com/
[Maven wtyczki dla aplikacji sieci Web Azure]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: tutorial-custom-docker-image.md
[Docker]: https://www.docker.com/
[Dodatek docker przypadku Maven]: https://github.com/spotify/docker-maven-plugin
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

[SB01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP02.png
