---
title: "Jak używać Spring początkowego rozruchu z interfejsu API Azure rozwiązania Cosmos bazy danych usługi DocumentDB"
description: "Informacje o sposobie konfigurowania aplikacji utworzonych za pomocą inicjatora Spring rozruchu z interfejsu API Azure rozwiązania Cosmos bazy danych usługi DocumentDB."
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: "Rozwiązania Cosmos Spring, Spring początkowego rozruchu, bazy danych"
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/08/2017
ms.author: robmcm;yungez;kevinzha
ms.openlocfilehash: 273cc750857c5e466882060a38ac0f3475811e98
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a>Jak używać Spring początkowego rozruchu z interfejsu API Azure rozwiązania Cosmos bazy danych usługi DocumentDB

## <a name="overview"></a>Omówienie

**[Spring Framework]**  to rozwiązanie open source, które pomaga deweloperom języka Java, tworzenie aplikacji na poziomie przedsiębiorstwa. Jeden z popularnych więcej projektów, które jest wbudowane znajdujący się na platformy jest [rozruchu Spring], zapewniające uproszczone podejście do tworzenia aplikacji Java autonomicznych. Aby pomóc deweloperom Rozpoczynanie pracy z funkcją rozruchu Spring, kilka pakietów rozruchu Spring próbki są dostępne pod adresem <https://github.com/spring-guides/>. Oprócz możliwości wyboru z listy projektów Spring rozruchu podstawowe,  **[Spring Initializr]**  ułatwia deweloperom szybkie wprowadzenie do tworzenia niestandardowych aplikacji rozruchu sprężyny.

Azure DB rozwiązania Cosmos jest usługą globalnie rozproszone bazy danych, dzięki której deweloperzy mogą pracować z danymi przy użyciu różnych standardowych interfejsów API, takich jak usługi DocumentDB, bazy danych MongoDB, wykres i tabela interfejsów API. Początkowy rozruchu Spring firmy Microsoft umożliwia deweloperom korzystanie z aplikacji Spring rozruchu, które łatwo zintegrować z bazy danych rozwiązania Cosmos Azure za pomocą interfejsów API usługi DocumentDB.

W tym artykule przedstawiono tworzenie rozwiązania Cosmos bazy danych platformy Azure przy użyciu portalu Azure, a następnie za pomocą **Spring Initializr** tworzenie aplikacji java niestandardowej, a następnie dodaj funkcje początkowego rozruchu Spring do niestandardowych aplikacji do przechowywania danych i pobierania danych z bazy danych programu Azure rozwiązania Cosmos przy użyciu interfejsu API usługi DocumentDB.

## <a name="prerequisites"></a>Wymagania wstępne

Następujące wymagania wstępne są wymagane w celu wykonaj kroki opisane w tym artykule:

* Subskrypcja platformy Azure; Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz przeprowadzić aktywację Twojej [korzyści dla subskrybentów MSDN] lub zarejestrować się w celu [bezpłatne konto platformy Azure].

* A [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), wersji 1,7 lub nowszej.

* [Apache Maven](http://maven.apache.org/), w wersji 3.0 lub nowszej.

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>Tworzenie bazy danych Azure rozwiązania Cosmos przy użyciu portalu Azure

1. Przejdź do portalu Azure pod adresem <https://portal.azure.com/> i kliknij przycisk **+ nowy**.

   ![Azure Portal][AZ01]

1. Kliknij przycisk **baz danych**, a następnie kliknij przycisk **bazy danych Azure rozwiązania Cosmos**.

   ![Azure Portal][AZ02]

1. Na **bazy danych Azure rozwiązania Cosmos** strony, wprowadź następujące informacje:

   * Wprowadź unikatową **identyfikator**, który będzie używany jako identyfikator URI dla Twojej bazy danych. Na przykład: *wingtiptoysdata.documents.azure.com*.
   * Wybierz **SQL (dokument bazy danych)** dla interfejsu API.
   * Wybierz **subskrypcji** chcesz użyć dla bazy danych.
   * Określ, czy należy utworzyć nową **grupy zasobów** bazy danych, lub wybierz istniejącą grupę zasobów.
   * Określ **lokalizacji** bazy danych.
   
   Po określeniu tych opcji, kliknij przycisk **Utwórz** do utworzenia bazy danych.

   ![Azure Portal][AZ03]

1. Podczas tworzenia bazy danych znajduje się w sieci Azure **pulpitu nawigacyjnego**, jak również w obszarze **wszystkie zasoby** i **bazy danych Azure rozwiązania Cosmos** stron. Możesz kliknąć na żadnym z tych lokalizacjach, aby otworzyć stronę właściwości dla pamięci podręcznej bazy danych.

   ![Azure Portal][AZ04]

1. Po stronie właściwości dla bazy danych jest wyświetlany, kliknij przycisk **klucze dostępu** i skopiuj klucze dostępu i identyfikator URI dla Twojej bazy danych; te wartości zostaną użyte w aplikacji rozruchu sprężyny.

   ![Azure Portal][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Utworzyć prostą aplikację Spring rozruchu z Initializr źródła

1. Przejdź do <https://start.spring.io/>.

1. Określ, czy chcesz wygenerować **Maven** projektu z **Java**, wprowadź **grupy** i **artefaktu** nazwy aplikacji, a następnie kliknij przycisk, aby **Generowanie projektu**.

   ![Opcje Initializr Basic Spring][SI01]

   > [!NOTE]
   >
   > Używa Spring Initializr **grupy** i **artefaktu** nazwy, aby utworzyć nazwę pakietu; na przykład: *com.example.wintiptoys*.
   >

1. Po wyświetleniu monitu można pobrać projektu do ścieżki na komputerze lokalnym.

   ![Pobierz niestandardowy projekt Spring rozruchu][SI02]

1. Po wyodrębnieniu plików w systemie lokalnym, prosty aplikacji rozruchu Spring będzie gotowy do edycji.

   ![Pliki projektu rozruchu Spring niestandardowe][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a>Skonfiguruj aplikację Spring rozruchu, aby użyć początkowego rozruchu Spring Azure

1. Zlokalizuj *pom.xml* pliku w katalogu aplikacji; na przykład:

   `C:\SpringBoot\wingtiptoys\pom.xml`

   — lub —

   `/users/example/home/wingtiptoys/pom.xml`

   ![Zlokalizuj plik pom.xml][PM01]

1. Otwórz *pom.xml* plik w edytorze tekstów i dodaj następujące wiersze do listy `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![Edytowanie pliku pom.xml][PM02]

1. Zapisz i Zamknij *pom.xml* pliku.

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a>Skonfiguruj aplikację Spring rozruchu, aby użyć bazy danych programu Azure rozwiązania Cosmos

1. Zlokalizuj *application.properties* w pliku *zasobów* katalogu aplikacji, np.:

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   — lub —

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![Zlokalizuj plik application.properties][RE01]

1. Otwórz *application.properties* plik w edytorze tekstów i dodaj następujące wiersze do pliku i Zastąp przykładowe wartości odpowiednich właściwości bazy danych:

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![Edytowanie pliku application.properties][RE02]

1. Zapisz i Zamknij *application.properties* pliku.

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>Dodaj przykładowy kod do implementacji funkcji podstawowych bazy danych

W tej sekcji utworzysz dwie klasy Java do przechowywania danych użytkownika, a następnie zmodyfikować klasy głównym aplikacji do tworzenia instancji klasy użytkownika i zapisać go do bazy danych.

### <a name="define-a-basic-class-for-storing-user-data"></a>Definiowanie podstawowe klasy do przechowywania danych użytkownika

1. Utwórz nowy plik o nazwie *User.java* w tym samym katalogu co plik głównej aplikacji Java.

1. Otwórz *User.java* plik w edytorze tekstów i dodaj następujące wiersze do pliku, aby określić klasę ogólnego użytkownika, która przechowuje i pobrać wartości w bazie danych:

   ```java
   package com.example.wingtiptoys;

   public class User {
      private String id;
      private String firstName;
      private String lastName;
 
      public User(String id, String firstName, String lastName) {
         this.id = id;
         this.firstName = firstName;
         this.lastName = lastName;
      }
   
      public String getId() {
         return this.id;
      }

      public void setId(String id) {
         this.id = id;
      }

      public String getFirstName() {
         return firstName;
      }

      public void setFirstName(String firstName) {
         this.firstName = firstName;
      }

      public String getLastName() {
         return lastName;
      }

      public void setLastName(String lastName) {
         this.lastName = lastName;
      }

      @Override
      public String toString() {
         return String.format("User: %s %s", firstName, lastName);
      }
   }
   ```

1. Zapisz i Zamknij *User.java* pliku.

### <a name="define-a-data-repository-interface"></a>Zdefiniuj interfejs repozytorium danych

1. Utwórz nowy plik o nazwie *UserRepository.java* w tym samym katalogu co plik głównej aplikacji Java.

1. Otwórz *UserRepository.java* plik w edytorze tekstów i dodaj następujące wiersze do pliku, aby określić repozytorium interfejs, który rozszerza interfejs domyślny repozytorium usługi DocumentDB:

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. Zapisz i Zamknij *UserRepository.java* pliku.

### <a name="modify-the-main-application-class"></a>Modyfikowanie klasy głównym aplikacji

1. Zlokalizuj plik głównej aplikacji Java w katalogu pakietu aplikacji; na przykład:

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   — lub —

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![Zlokalizuj plik aplikacji Java][JV01]

1. Otwórz plik głównej aplikacji Java w edytorze tekstów i dodaj następujące wiersze do pliku:

   ```java
   package com.example.wingtiptoys;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class WingtiptoysApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;
    
      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysApplication.class, args);
      }

      public void run(String... var1) throws Exception {
         final User testUser = new User("testId", "testFirstName", "testLastName");

         repository.deleteAll();
         repository.save(testUser);

         final User result = repository.findOne(testUser.getId());

         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

1. Zapisz i zamknij plik głównej aplikacji Java.

## <a name="build-and-test-your-app"></a>Tworzenie i testowanie aplikacji

1. Otwórz wiersz polecenia i zmień katalog na folder gdy Twoje *pom.xml* znajduje się plik, na przykład:

   `cd C:\SpringBoot\wingtiptoys`

   — lub —

   `cd /users/example/home/wingtiptoys`

1. Tworzenie aplikacji Spring rozruchu z Maven i uruchamianie na przykład:

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. Aplikacja wyświetli komunikaty środowiska uruchomieniowego i powinien zostać wyświetlony komunikat `User: testFirstName testLastName` wyświetlane, aby wskazać, że wartości zostały pomyślnie przechowywane i pobierane z bazy danych.

   ![Pomyślne dane wyjściowe aplikacji][JV02]

1. OPCJONALNIE: Portalu Azure można użyć do wyświetlania zawartości Twojego rozwiązania Cosmos bazy danych Azure na stronie właściwości dla bazy danych, klikając **Eksploratora dokumentów**, wybierając, a element z listy wyświetlanych do wyświetlania zawartości.

   ![Aby wyświetlić dane przy użyciu Eksploratora dokumentów][JV03]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat korzystania z bazy danych Azure rozwiązania Cosmos i Java zobacz następujące artykuły:

* [Dokumentacja rozwiązania Cosmos Azure DB].

* [Azure rozwiązania Cosmos bazy danych: Tworzenie aplikacji interfejsu API usługi DocumentDB z języka Java i portalu Azure][Build a DocumentDB API app with Java]

Aby uzyskać więcej informacji o używaniu aplikacji rozruchu Spring na platformie Azure zobacz następujące artykuły:

* [Spring rozruchu DocumenDB początkowy dla platformy Azure](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [Wdrażanie aplikacji rozruchu Spring w usłudze Azure App Service](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [Działającej aplikacja rozruchu Spring w klastrze Kubernetes w usłudze kontenera platformy Azure](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Aby uzyskać więcej informacji o używaniu platformy Azure z językiem Java, zobacz [Centrum deweloperów języka Java dla platformy Azure] i [Java Tools for Visual Studio Team Services] (Narzędzia języka Java dla usługi Visual Studio Team Services).

<!-- URL List -->

[Dokumentacja rozwiązania Cosmos Azure DB]: /azure/cosmos-db/
[Centrum deweloperów języka Java dla platformy Azure]: https://azure.microsoft.com/develop/java/
[Build a DocumentDB API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-documentdb-java
[bezpłatne konto platformy Azure]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[korzyści dla subskrybentów MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[rozruchu Spring]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ01.png
[AZ02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ02.png
[AZ03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ03.png
[AZ04]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ04.png
[AZ05]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ05.png

[SI01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI01.png
[SI02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI02.png
[SI03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI03.png

[RE01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE01.png
[RE02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE02.png

[PM01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM01.png
[PM02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM02.png

[JV01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV01.png
[JV02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV02.png
[JV03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV03.png
