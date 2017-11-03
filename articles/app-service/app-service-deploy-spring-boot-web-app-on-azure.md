---
title: "Wdrażanie aplikacji rozruchu Spring w usłudze Azure App Service | Dokumentacja firmy Microsoft"
description: "Ten samouczek przeprowadzi deweloperzy przez kroki wdrażania aplikacji sieci web Spring rozruchu wprowadzenie do usługi Azure App Service."
services: app-service\web
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
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.openlocfilehash: 8776142d5452bf5057990702c89aa1a541382ffc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-a-spring-boot-application-to-the-azure-app-service"></a>Wdrażanie aplikacji platformy Spring Boot w usłudze Azure App Service

**[Spring Framework]**  to rozwiązanie open source, które pomaga deweloperom języka Java, tworzenie aplikacji na poziomie przedsiębiorstwa i jest jeden z popularnych więcej projektów, które jest oparty na danej platformie [ Powierzchni rozruchu], zapewniające uproszczone podejście do tworzenia aplikacji Java autonomicznych.

W tym samouczku opisano jednak tworzenia przykładowej aplikacji sieci web Spring wprowadzenie rozruchu i wdrażania go do [usłudze Azure App Service].

### <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać kroki opisane w tym samouczku, należy dysponować następującymi elementami:

* Subskrypcja platformy Azure; Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz przeprowadzić aktywację Twojej [korzyści dla subskrybentów MSDN] lub zarejestrować się w celu [bezpłatne konto platformy Azure].
* Aktualne [Java Developer Kit (JDK)].
* Apache na [Maven] (w wersji 3) Narzędzie kompilacji.
* A [Git] klienta.

## <a name="create-the-spring-boot-getting-started-web-app"></a>Tworzenie aplikacji sieci web Spring wprowadzenie rozruchu

Poniższe kroki przeprowadzi Cię przez kolejne kroki, które są wymagane do tworzenia prostej aplikacji sieci web Spring rozruchu i przetestować go lokalnie.

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

1. Klonowanie [Spring wprowadzenie rozruchu] przykładowy projekt do katalogu, który został właśnie utworzony; na przykład:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. Zmień katalog na ukończone projektu; na przykład:
   ```
   cd gs-spring-boot
   cd complete
   ```

1. Kompiluj plik JAR za pomocą programu Maven; na przykład:
   ```
   mvn package
   ```

1. Po utworzeniu aplikacji sieci web, zmień katalog na plik JAR i uruchomić aplikację sieci web; na przykład:
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. Testowanie aplikacji sieci web, przechodząc do adresem http://localhost: 8080 przy użyciu przeglądarki sieci web, lub użyj składni, jak w następującym przykładzie, jeśli masz curl dostępne:
   ```
   curl http://localhost:8080
   ```

1. Powinien zostać wyświetlony następujący komunikat wyświetlany: **pozdrowienia z Spring rozruchowe!**

   ![Przejdź do przykładowej aplikacji][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a>Tworzenie aplikacji sieci web platformy Azure do użycia z językiem Java

Poniższe kroki przeprowadzi Cię przez kolejne kroki do tworzenia aplikacji sieci Web platformy Azure, skonfiguruj wymagane ustawienia dla języka Java i skonfigurować poświadczenia FTP.

1. Przejdź do [portalu Azure] i zaloguj się.

1. Gdy użytkownik zalogował się na koncie w portalu Azure, kliknij ikonę menu **usługi aplikacji**:
   
   ![Azure Portal][AZ01]

1. Gdy **usługi aplikacji** strona jest wyświetlana, kliknij przycisk **+ Dodaj** do utworzenia nowej usługi aplikacji.

   ![Tworzenie usługi App Service][AZ02]

1. Gdy zostanie wyświetlona lista szablonów aplikacji sieci web, kliknij łącze dla podstawowej aplikacji sieci Web firmy Microsoft.

   ![Szablony aplikacji sieci Web][AZ03]

1. Po stronie informacje o szablonu aplikacji sieci Web zostanie wyświetlony, kliknij przycisk **Utwórz**.

   ![Tworzenie aplikacji sieci Web][AZ04]

1. Podaj unikatową nazwę aplikacji sieci web i określ dodatkowe ustawienia, a następnie **Utwórz**.

   ![Tworzenie ustawień aplikacji sieci Web][AZ05]

1. Po utworzeniu aplikacji sieci web, kliknij ikonę menu **usługi aplikacji**, a następnie kliknij przycisk z nowo utworzonych aplikacji sieci web:

   ![Lista aplikacji sieci Web][AZ06]

1. Po wyświetleniu aplikacji sieci web, określ wersję języka Java przy użyciu następujących kroków:

   a. Kliknij przycisk **ustawienia aplikacji** elementu menu.

   b. Wybierz **Java 8** dla wersji języka Java.

   c. Wybierz **najnowszych** dla wersji pomocniczej Java.

   d. Wybierz **najnowsze 8.5 Tomcat** kontenera sieci web. (Ten kontener nie będzie faktycznie używane; Azure użyje kontenera aplikację Spring rozruchu).

   e. Kliknij pozycję **Zapisz**.

   ![Ustawienia aplikacji][AZ07]

1. Określ poświadczenia wdrażania FTP przy użyciu następujących kroków:

   a. Kliknij przycisk **poświadczenia wdrażania** elementu menu.

   b. Określ nazwę użytkownika i hasło.

   c. Kliknij pozycję **Zapisz**.

   ![Określ poświadczenia wdrożenia][AZ08]

1. Pobierz informacje o połączeniu FTP przy użyciu następujących kroków:

   a. Kliknij przycisk **poświadczenia wdrażania** elementu menu.

   b. Kopiuj pełną FTP nazwę użytkownika i adres URL i zapisywać je w następnej sekcji tego samouczka.

   ![Adresu URL usługi FTP i poświadczenia][AZ09]

## <a name="deploy-your-spring-boot-web-app-to-azure"></a>Wdrażanie aplikacji sieci web Spring rozruchowego na platformie Azure

Poniższe kroki objaśniają proces wdrażania aplikacji sieci web Spring rozruchowego na platformie Azure.

1. Otwórz Edytor tekstu, takiego jak Notatnik systemu Windows i wklej poniższy tekst do nowego dokumentu, a następnie zapisz plik jako *web.config*:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. Po zapisaniu *web.config* plików do systemu, nawiązać połączenia z aplikacji sieci web za pośrednictwem protokołu FTP przy użyciu adresu URL, nazwę użytkownika i hasło z poprzedniej sekcji tego samouczka. Na przykład:
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. Zmień katalog zdalny folder główny aplikacji sieci web (czyli w */lokacji/wwwroot*), następnie skopiuj plik JAR aplikację Spring rozruchu i *web.config* z wcześniej. Na przykład:
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. Po wdrożeniu sieci JAR i *web.config* pliki do aplikacji sieci web, musisz ponownie uruchomić aplikacji sieci web przy użyciu portalu Azure:

   ![][AZ10]

1. Testowanie aplikacji sieci web, przechodząc do adresu URL aplikacji sieci web w przeglądarce sieci web, lub użyj składni, jak w następującym przykładzie, jeśli masz curl dostępne:
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. Powinien zostać wyświetlony następujący komunikat wyświetlany: **pozdrowienia z Spring rozruchowe!**

   ![Przejdź do przykładowej aplikacji][SB02]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o używaniu aplikacji rozruchu Spring na platformie Azure zobacz następujące artykuły:

* [Wdrażanie aplikacji Spring rozruchu w systemie Linux w usłudze kontenera platformy Azure](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md)

* [Wdrażanie aplikacji rozruchu Spring w klastrze Kubernetes w usłudze kontenera platformy Azure](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-kubernetes.md)

Aby uzyskać więcej informacji o używaniu platformy Azure z językiem Java, zobacz [Centrum deweloperów języka Java dla platformy Azure] i [Java Tools for Visual Studio Team Services] (Narzędzia języka Java dla usługi Visual Studio Team Services).

Aby uzyskać dodatkowe informacje o depoying aplikacje sieci web na platformie Azure za pomocą protokołu FTP, zobacz [wdrażanie aplikacji w usłudze Azure App Service przy użyciu FTP/S].

Dodatkowe szczegóły dotyczące rozruchu Spring przykładowy projekt, zobacz [Spring wprowadzenie rozruchu].

Aby uzyskać pomoc dotyczącą Rozpoczynanie pracy z aplikacjami Spring rozruchu, zobacz **Spring Initializr** na https://start.spring.io/.

Aby uzyskać więcej informacji o konfigurowaniu dodatkowych ustawień dla aplikacji sieci web, zobacz [Konfigurowanie aplikacji sieci web w usłudze Azure App Service].

<!-- URL List -->

[usłudze Azure App Service]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[Centrum deweloperów języka Java dla platformy Azure]: https://azure.microsoft.com/develop/java/
[portalu Azure]: https://portal.azure.com/
[Konfigurowanie aplikacji sieci web w usłudze Azure App Service]: /azure/app-service/web-sites-configure
[wdrażanie aplikacji w usłudze Azure App Service przy użyciu FTP/S]: https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp
[bezpłatne konto platformy Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[korzyści dla subskrybentów MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[ Powierzchni rozruchu]: http://projects.spring.io/spring-boot/
[Spring wprowadzenie rozruchu]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB01.png
[SB02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB02.png

[AZ01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ01.png
[AZ02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ02.png
[AZ03]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ03.png
[AZ04]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ04.png
[AZ05]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ05.png
[AZ06]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ06.png
[AZ07]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ07.png
[AZ08]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ08.png
[AZ09]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ09.png
[AZ10]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ10.png
