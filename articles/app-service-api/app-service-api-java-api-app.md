---
title: "Tworzenie i wdrażanie aplikacji interfejsu API języka Java w usłudze Azure App Service"
description: "Dowiedz się, jak utworzyć pakiet aplikacji interfejsu API języka Java i wdrożyć go w usłudze Azure App Service."
services: app-service\api
documentationcenter: java
author: rmcmurray
manager: erikre
editor: tdykstra
ms.assetid: 8d21ba5f-fc57-4269-bc8f-2fcab936ec22
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: get-started-article
ms.date: 04/25/2017
ms.author: rachelap;robmcm
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: e38c540071cb49b0177e79178566d72ecb5f8886
ms.contentlocale: pl-pl
ms.lasthandoff: 08/21/2017

---
# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Tworzenie i wdrażanie aplikacji interfejsu API języka Java w usłudze Azure App Service
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

W tym samouczku pokazano, jak utworzyć aplikację języka Java i wdrożyć ją w funkcji Azure App Service API Apps za pomocą systemu [Git]. Instrukcje podane w tym samouczku można wykonać w dowolnym systemie operacyjnym, w którym można uruchomić oprogramowanie Java. Kod w tym samouczku został utworzony za pomocą narzędzia [Maven]. Projekt [Jax-RS], który służy do tworzenia usługi RESTful, jest generowany na podstawie specyfikacji metadanych programu [Swagger] przy użyciu [Edytora programu Swagger].

## <a name="prerequisites"></a>Wymagania wstępne
1. [Java Developer's Kit 8] \(lub nowszy)
2. [Maven] zainstalowane na komputerze deweloperskim
3. [Git] zainstalowany na komputerze deweloperskim
4. Wersja płatna lub [bezpłatna wersja próbna] subskrypcji platformy [Microsoft Azure]
5. Aplikacja testowa HTTP, taka jak [Postman]

## <a name="scaffold-the-api-using-swaggerio"></a>Tworzenie szkieletu interfejsu API przy użyciu narzędzia Swagger.IO
Za pomocą edytora online swagger.io można wprowadzić kod JSON lub YAML programu Swagger reprezentujący strukturę interfejsu API. Po zaprojektowaniu powierzchni interfejsu API można wyeksportować kod dla różnych platform i struktur. W następnej sekcji kod z utworzonym szkieletem zostanie zmodyfikowany w celu dodania funkcji testowych. 

Ten pokaz rozpocznie się od przedstawienia treści kodu JSON programu Swagger, która zostanie wklejona do edytora swagger.io. Przy użyciu tego edytora zostanie wygenerowany kod korzystający z projektu JAX-RS w celu uzyskania dostępu do punktu końcowego interfejsu API REST. Następnie kod z utworzonym szkieletem zostanie zmodyfikowany w celu zwrócenia danych testowych, co stanowi symulację interfejsu API REST korzystającego z mechanizmu stanu trwałego danych.  

1. Skopiuj poniższy kod JSON programu Swagger do Schowka:
   
        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }
2. Przejdź do [Edytora Online programu Swagger]. W wyświetlonym edytorze kliknij element menu **File -> Paste JSON (Plik -> Wklej dane JSON)**.
   
    ![Element menu Paste JSON (Wklej dane JSON)][paste-json]
3. Wklej skopiowane wcześniej dane JSON programu Swagger interfejsu API listy kontaktów. 
   
    ![Wklejanie kodu JSON do programu Swagger][pasted-swagger]
4. Przejrzyj zawartość stron z dokumentacją oraz podsumowanie interfejsu API wyświetlone w edytorze. 
   
    ![Wyświetlanie dokumentów wygenerowanych przez program Swagger][view-swagger-generated-docs]
5. Wybierz polecenie **Generate Server -> JAX-RS (Generuj serwer -> JAX-RS)** w menu, aby utworzyć szkielet kodu po stronie serwera, który zostanie później zmodyfikowany w celu dodania implementacji testowej. 
   
    ![Element menu umożliwiający generowanie kodu][generate-code-menu-item]
   
    Po wygenerowaniu kodu zostanie udostępniony plik ZIP do pobrania. Zawiera on szkielet kodu utworzonego przez generator kodu programu Swagger oraz wszystkie skojarzone skrypty kompilacji. Rozpakuj całą zawartość biblioteki do katalogu na deweloperskiej stacji roboczej. 

## <a name="edit-the-code-to-add-api-implementation"></a>Edytowanie kodu w celu dodania implementacji interfejsu API
W tej sekcji implementacja po stronie serwera zawarta w kodzie wygenerowanym przez program Swagger zostanie zastąpiona niestandardowym kodem. Nowy kod zwróci element ArrayList z jednostkami kontaktowymi do klienta wywołującego. 

1. Otwórz plik modelu *Contact.java* znajdujący się w folderze *src/gen/java/io/swagger/model* za pomocą programu [Visual Studio Code] lub edytora tekstu, którego używasz. 
   
    ![Otwieranie pliku modelu kontaktów][open-contact-model-file]
2. Dodaj następującego konstruktora do klasy **Contact**. 
   
        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }
3. Otwórz plik implementacji usługi *ContactsApiServiceImpl.java* znajdujący się w folderze *src/main/java/io/swagger/api/impl* za pomocą programu [Visual Studio Code] lub edytora tekstu, którego używasz.
   
    ![Otwieranie pliku kodu usługi kontaktów][open-contact-service-code-file]
4. Zastąp kod w pliku nowym kodem, aby dodać implementację testową w kodzie usługi. 
   
        package io.swagger.api.impl;
   
        import io.swagger.api.*;
        
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
               
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;
   
        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
   
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
   
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
   
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
   
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }
5. Otwórz wiersz polecenia i zmień katalog na folder główny aplikacji.
6. Uruchom następujące polecenie narzędzia Maven, aby skompilować kod i uruchomić go za pomocą lokalnego serwera aplikacji Jetty. 
   
        mvn package jetty:run
7. W oknie wiersza polecenia pojawi się informacja o uruchomieniu kodu na serwerze Jetty na porcie 8080. 
   
    ![Otwieranie pliku kodu usługi kontaktów][run-jetty-war]
8. Za pomocą aplikacji [Postman] wyślij żądanie do metody interfejsu API służącej do pobrania wszystkich kontaktów i dostępnej pod adresem http://localhost:8080/api/contacts.
   
    ![Wywołanie interfejsu API kontaktów][calling-contacts-api]
9. Za pomocą aplikacji [Postman] wyślij żądanie do metody interfejsu API służącej do pobrania określonego kontaktu i dostępnej pod adresem http://localhost:8080/api/contacts/2.
   
    ![Wywołanie interfejsu API kontaktów][calling-specific-contact-api]
10. Na koniec utwórz plik WAR języka Java (Web ARchive — archiwum sieci Web), uruchamiając następujące polecenie narzędzia Maven za pomocą konsoli. 
    
         mvn package war:war
11. Utworzony plik WAR zostanie umieszczony w folderze **target**. Przejdź do folderu **target** i zmień nazwę pliku WAR na **ROOT.war**. (Wielkość liter jest istotna).
    
          rename swagger-jaxrs-server-1.0.0.war ROOT.war
12. Następnie uruchom następujące polecenia z poziomu folderu głównego aplikacji, aby utworzyć folder **deploy** używany do wdrożenia pliku WAR na platformie Azure. 
    
          mkdir deploy
          mkdir deploy\webapps
          copy target\ROOT.war deploy\webapps
          cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>Publikowanie danych wyjściowych w usłudze Azure App Service
W tej sekcji dowiesz się, jak utworzyć nową aplikację interfejsu API przy użyciu portalu Azure, przygotować ją do hostowania aplikacji języka Java i wdrożyć nowo utworzony plik WAR w usłudze Azure App Service w celu uruchomienia nowej aplikacji interfejsu API. 

1. Utwórz nową aplikację interfejsu API w [Azure Portal], klikając elementy menu **Nowy -> Sieć Web i mobilność -> Aplikacja interfejsu API**, wprowadzając informacje dotyczące aplikacji, a następnie klikając pozycję **Utwórz**.
   
    ![Tworzenie nowej aplikacji interfejsu API][create-api-app]
2. Po utworzeniu aplikacji interfejsu API otwórz blok **Ustawienia** dla tej aplikacji, a następnie kliknij element menu **Ustawienia aplikacji**. Za pomocą dostępnych opcji wybierz najnowsze wersje oprogramowania Java, wybierz najnowszą wersję serwera Tomcat w menu **Kontener sieci Web**, a następnie kliknij pozycję **Zapisz**.
   
    ![Konfigurowanie oprogramowania Java w bloku Aplikacja interfejsu API][set-up-java]
3. Kliknij element **Poświadczenia wdrożenia** w menu ustawień i podaj nazwę użytkownika i hasło, których chcesz używać do publikowania plików w aplikacji interfejsu API. 
   
    ![Konfigurowanie poświadczeń wdrożenia][deployment-credentials]
4. Kliknij element **Źródło wdrożenia** w menu ustawień. W wyświetlonym obszarze kliknij przycisk **Wybierz źródło**, wybierz opcję **Lokalne repozytorium Git**, a następnie kliknij przycisk **OK**. Spowoduje to utworzenie repozytorium Git działającego na platformie Azure, które jest skojarzone z aplikacją interfejsu API. Za każdym razem, gdy zatwierdzisz kod w *głównej* gałęzi repozytorium Git, zostanie on opublikowany w uruchomionym wystąpieniu aplikacji interfejsu API. 
   
    ![Konfigurowanie nowego lokalnego repozytorium Git][select-git-repo]
5. Skopiuj adres URL nowego repozytorium Git do Schowka. Zapisz ten adres, ponieważ będzie on potrzebny później. 
   
    ![Konfigurowanie nowego repozytorium Git dla aplikacji][copy-git-repo-url]
6. Za pomocą narzędzia Git wypchnij plik WAR do repozytorium online. W tym celu przejdź do utworzonego wcześniej folderu **deploy**, aby móc łatwo zatwierdzić kod w repozytorium uruchomionym w usłudze App Service. W oknie konsoli przejdź do folderu, w którym znajduje się folder webapps, a następnie wydaj następujące polecenia narzędzia Git, aby uruchomić proces i szybko włączyć wdrożenie. 
   
         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master
   
    Po wysłaniu żądania **push** pojawi się monit o wpisanie hasła utworzonego w ramach konfigurowania poświadczeń wdrożenia. Po wprowadzeniu poświadczeń w portalu pojawi się informacja o wdrożeniu aktualizacji.
7. Jeśli ponownie użyjesz aplikacji Postman, aby sprawdzić działanie nowo wdrożonej aplikacji interfejsu API uruchomionej w usłudze Azure App Service, zobaczysz, że jest ono spójne i że aplikacja zwraca dane kontaktowe zgodnie z oczekiwaniami, korzystając z prostych zmian kodu w pliku języka Java, którego szkielet utworzono za pomocą narzędzia Swagger.io. 
   
    ![Używanie interfejsu API REST kontaktów języka Java uruchomionego na platformie Azure][postman-calling-azure-contacts]

## <a name="next-steps"></a>Następne kroki
W tym artykule udało się rozpocząć pracę z plikiem JSON programu Swagger i kodem języka Java z utworzonym szkieletem uzyskanym za pomocą edytora Swagger.io. W efekcie wprowadzenia prostych zmian i zastosowania procesu wdrażania narzędzia Git utworzono funkcjonalną aplikację interfejsu API napisaną w języku Java. W następnym samouczku przedstawiono sposób [korzystania z aplikacji interfejsu API z poziomu klientów JavaScript przy użyciu specyfikacji CORS][App Service API CORS]. W kolejnych samouczkach z tej serii opisano implementowanie uwierzytelniania i autoryzacji.

Aby poszerzyć wiedzę przyswojoną w tym przykładzie, możesz zapoznać się z zagadnieniami dotyczącymi utrwalania obiektów blob JSON za pomocą [Storage SDK for Java]. Możesz również użyć [Document DB Java SDK], aby zapisać dane kontaktowe w bazie danych dokumentów na platformie Azure. 

<a name="see-also"></a>

## <a name="see-also"></a>Zobacz też
Aby uzyskać więcej informacji o używaniu platformy Azure z językiem Java, odwiedź stronę [Azure dla deweloperów języka Java](/java/azure).

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure Portal]: https://portal.azure.com/
[Document DB Java SDK]: ../documentdb/documentdb-java-application.md
[bezpłatna wersja próbna]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Azure Java Developer Center]: /develop/java/
[Java Developer's Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax-RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Edytora Online programu Swagger]: http://editor2.swagger.io/
[Postman]: https://www.getpostman.com/
[Storage SDK for Java]:../storage/blobs/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Edytora programu Swagger]: http://editor.swagger.io/
[Visual Studio Code]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png

