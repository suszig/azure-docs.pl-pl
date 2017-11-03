---
title: "Jak skonfigurować aplikację Spring rozruchu inicjatora, aby użyć pamięci podręcznej Redis"
description: "Informacje o sposobie konfigurowania aplikacji utworzone za pomocą Spring Initializr Spring rozruchowy do użycia pamięci podręcznej Redis Azure."
services: redis-cache
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: "Pamięć podręczna Redis Spring, Spring początkowego rozruchu,"
ms.assetid: 
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: java
ms.topic: article
ms.date: 08/31/2017
ms.author: robmcm;zhijzhao;yidon
ms.openlocfilehash: 7a6ec549654d00975494bac8594a6777af5ec415
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-a-spring-boot-initializer-app-to-use-redis-cache"></a>Jak skonfigurować aplikację Spring rozruchu inicjatora, aby użyć pamięci podręcznej Redis

## <a name="overview"></a>Omówienie

**[Spring Framework]**  to rozwiązanie open source, które pomaga deweloperom języka Java, tworzenie aplikacji na poziomie przedsiębiorstwa. Jeden z popularnych więcej projektów, które jest wbudowane znajdujący się na platformy jest [rozruchu Spring], zapewniające uproszczone podejście do tworzenia aplikacji Java autonomicznych. Aby pomóc deweloperom Rozpoczynanie pracy z funkcją rozruchu Spring, kilka pakietów rozruchu Spring próbki są dostępne pod adresem <https://github.com/spring-guides/>. Oprócz możliwości wyboru z listy projektów Spring rozruchu podstawowe,  **[Spring Initializr]**  ułatwia deweloperom szybkie wprowadzenie do tworzenia niestandardowych aplikacji rozruchu sprężyny.

W tym artykule przedstawiono tworzenie pamięci podręcznej Redis przy użyciu portalu Azure, następnie za pomocą **Spring Initializr** można utworzyć niestandardową aplikację, a następnie utworzenie aplikacji sieci web Java, która przechowuje i pobiera dane przy użyciu z pamięci podręcznej Redis pamięć podręczna.

## <a name="prerequisites"></a>Wymagania wstępne

Następujące wymagania wstępne są wymagane w celu wykonaj kroki opisane w tym artykule:

* Subskrypcja platformy Azure; Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz przeprowadzić aktywację Twojej [korzyści dla subskrybentów MSDN] lub zarejestrować się w celu [bezpłatne konto platformy Azure].

* A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), wersji 1,7 lub nowszej.

* [Apache Maven](http://maven.apache.org/), w wersji 3.0 lub nowszej.

## <a name="create-a-redis-cache-on-azure"></a>Tworzenie pamięci podręcznej Redis na platformie Azure

1. Przejdź do portalu Azure pod adresem <https://portal.azure.com/> i kliknij na element **+ nowy**.

   ![Azure Portal][AZ01]

1. Kliknij przycisk **bazy danych**, a następnie kliknij przycisk **pamięci podręcznej Redis**.

   ![Azure Portal][AZ02]

1. Na **nowa pamięć podręczna Redis** Podaj następujące informacje:

   * Wprowadź **nazwy DNS** dla pamięci podręcznej.
   * Określ użytkownika **subskrypcji**, **grupy zasobów**, **lokalizacji**, i **warstwa cenowa**.
   * W tym samouczku, wybierz **odblokować port 6379**.

   > [!NOTE]
   >
   > Można użyć protokołu SSL z pamięci podręcznej Redis, ale musisz użyć innego klienta pamięci podręcznej Redis, takich jak Jedis. Aby uzyskać więcej informacji, zobacz [jak pamięć podręczna Redis Azure za pomocą języka Java][Redis Cache with Java].
   >

   Po określeniu tych opcji, kliknij przycisk **Utwórz** do utworzenia pamięci podręcznej.

   ![Azure Portal][AZ03]

1. Po zakończeniu pamięć podręczną, pojawi się one dostępne w sieci Azure **pulpitu nawigacyjnego**, jak również w obszarze **wszystkie zasoby**, i **pamięci podręczne Redis** stron. Możesz kliknąć na pamięć podręczną na żadnym z tych lokalizacjach, aby otworzyć stronę właściwości dla pamięci podręcznej.

   ![Azure Portal][AZ04]

1. Po stronie zawierającej listę właściwości dla pamięci podręcznej jest wyświetlany, kliknij przycisk **klucze dostępu** i kopiowanie kluczy dostępu do pamięci podręcznej.

   ![Azure Portal][AZ05]

## <a name="create-a-custom-application-using-the-spring-initializr"></a>Tworzenie niestandardowych aplikacji przy użyciu Spring Initializr

1. Przejdź do <https://start.spring.io/>.

1. Określ, czy chcesz wygenerować **Maven** projektu z **Java**, wprowadź **grupy** i **Aritifact** nazwy aplikacji, a następnie kliknij łącze, aby **przełączyć się do wersji pełnej** z Initializr sprężyny.

   ![Opcje Initializr Basic Spring][SI01]

   > [!NOTE]
   >
   > Użyje Spring Initializr **grupy** i **Aritifact** nazwy, aby utworzyć nazwę pakietu; na przykład: *com.contoso.myazuredemo*.
   >

1. Przewiń w dół do **Web** sekcji i pole wyboru dla **sieci Web**, przewiń w dół do **NoSQL** sekcji i pole wyboru dla **Redis**, następnie przewiń w dół strony i kliknij przycisk, aby **Generowanie projektu**.

   ![Opcje Initializr Spring pełny][SI02]

1. Po wyświetleniu monitu można pobrać projektu do ścieżki na komputerze lokalnym.

   ![Pobierz niestandardowy projekt Spring rozruchu][SI03]

1. Po wyodrębnieniu plików w systemie lokalnym, niestandardową aplikację Spring rozruchu będzie gotowy do edycji.

   ![Pliki projektu rozruchu Spring niestandardowe][SI04]

## <a name="configure-your-custom-spring-boot-to-use-your-redis-cache"></a>Konfigurowanie niestandardowych rozruchowych Spring do użycia pamięci podręcznej Redis

1. Zlokalizuj *application.properties* w pliku *zasobów* katalogu aplikacji, lub utworzyć plik, jeśli jeszcze nie istnieje.

   ![Zlokalizuj plik application.properties][RE01]

1. Otwórz *application.properties* plik w edytorze tekstów i dodaj następujące wiersze do pliku i Zastąp przykładowe wartości odpowiednich właściwości z pamięci podręcznej:

   ```yaml
   # Specify the DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify the port for your Redis cache.
   spring.redis.port=6379

   # Specify the access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![Edytowanie pliku application.properties][RE02]

   > [!NOTE]
   >
   > Jeśli były używane różne klienta pamięci podręcznej Redis, takich jak Jedis, która włącza protokół SSL, należy określić port 6380 w Twojej *application.properties* pliku. Aby uzyskać więcej informacji, zobacz [jak pamięć podręczna Redis Azure za pomocą języka Java][Redis Cache with Java].
   >

1. Zapisz i Zamknij *application.properties* pliku.

1. Utwórz folder o nazwie *kontrolera* w folderze źródłowym pakietu; na przykład:

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   — lub —

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. Utwórz nowy plik o nazwie *HelloController.java* w *kontrolera* folderu. Otwórz plik w edytorze tekstów i Dodaj do niej następujący kod:

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.data.redis.core.StringRedisTemplate;
   import org.springframework.data.redis.core.ValueOperations;

   @RestController
   public class HelloController {
   
      @Autowired
      private StringRedisTemplate template;

      @RequestMapping("/")
      // Define the Hello World controller.
      public String hello() {
      
         ValueOperations<String, String> ops = this.template.opsForValue();

         // Add a Hello World string to your cache.
         String key = "greeting";
         if (!this.template.hasKey(key)) {
             ops.set(key, "Hello World!");
         }

         // Return the string from your cache.
         return ops.get(key);
      }
   }
   ```
   
   Gdzie należy zastąpić `com.contoso.myazuredemo` z nazwy pakietu dla projektu.

1. Zapisz i Zamknij *HelloController.java* pliku.

1. Tworzenie aplikacji Spring rozruchu z Maven i uruchamianie na przykład:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Testowanie aplikacji sieci web, przechodząc do adresem http://localhost: 8080 przy użyciu przeglądarki sieci web, lub użyj składni, jak w następującym przykładzie, jeśli masz curl dostępne:

   ```shell
   curl http://localhost:8080
   ```

   Powinny pojawić się "Witaj świecie!" komunikat z Twojej kontrolera próbki wyświetlany, który dynamicznie pobierania z pamięci podręcznej Redis.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o używaniu aplikacji rozruchu Spring na platformie Azure zobacz następujące artykuły:

* [Wdrażanie aplikacji rozruchu Spring w usłudze Azure App Service](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [Działającej aplikacja rozruchu Spring w klastrze Kubernetes w usłudze kontenera platformy Azure](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Aby uzyskać więcej informacji o używaniu platformy Azure z językiem Java, zobacz [Centrum deweloperów języka Java dla platformy Azure] i [Java Tools for Visual Studio Team Services] (Narzędzia języka Java dla usługi Visual Studio Team Services).

Uruchomiony przy użyciu pamięci podręcznej Redis z językiem Java na platformie Azure, można znaleźć więcej informacji na temat uzyskiwania [jak pamięć podręczna Redis Azure za pomocą języka Java][Redis Cache with Java].

<!-- URL List -->

[Centrum deweloperów języka Java dla platformy Azure]: https://azure.microsoft.com/develop/java/
[bezpłatne konto platformy Azure]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[korzyści dla subskrybentów MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[rozruchu Spring]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Redis Cache with Java]: cache-java-get-started.md

<!-- IMG List -->

[AZ01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ01.png
[AZ02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ02.png
[AZ03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ03.png
[AZ04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ04.png
[AZ05]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ05.png

[SI01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI01.png
[SI02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI02.png
[SI03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI03.png
[SI04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI04.png

[RE01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE01.png
[RE02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE02.png
