---
title: "Samouczek NoSQL języka C++ dla usługi DocumentDB | Microsoft Docs"
description: "Samouczek NoSQL języka C++, który pokazuje tworzenie bazy danych w języku C++ i aplikacji konsolowej przy użyciu zatwierdzonego dla usługi DocumentDB zestawu SDK języka C++. DocumentDB to skalowana w skali globalnej usługa bazy danych NoSQL."
services: documentdb
documentationcenter: cpp
author: asthana86
manager: jhubbard
editor: 
ms.assetid: b8756b60-8d41-4231-ba4f-6cfcfe3b4bab
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: hero-article
ms.date: 12/25/2016
ms.author: aasthan
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: 78c3da6fd83a6fca0351a90846d10acd82924be3
ms.lasthandoff: 04/18/2017


---
# <a name="nosql-c-tutorial-documentdb-c-console-application"></a>Samouczek NoSQL języka C++: aplikacja konsolowa w języku C++ dla usługi DocumentDB
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js dla MongoDB](documentdb-mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 
 

Witamy w samouczku języka C++ dla zatwierdzonego dla usługi DocumentDB zestawu SDK języka C++! W ramach tego samouczka utworzysz aplikację konsolową, która tworzy zasoby usługi DocumentDB, w tym bazę danych w języku C++ oraz wykonuje względem nich zapytania.

Omówione zostaną następujące czynności:

* Tworzenie konta usługi DocumentDB i nawiązywanie z nim połączenia
* Instalowanie aplikacji
* Tworzenie bazy danych usługi DocumentDB w języku C++
* Tworzenie kolekcji
* Tworzenie dokumentów JSON
* Wykonywanie zapytań względem kolekcji
* Zastępowanie dokumentu
* Usuwanie dokumentu
* Usuwanie bazy danych usługi DocumentDB w języku C++

Nie masz czasu? Nie martw się! Kompletne rozwiązanie jest dostępne w witrynie [GitHub](https://github.com/stalker314314/DocumentDBCpp). Przejdź do sekcji [Pobieranie kompletnego rozwiązania](#GetSolution), aby uzyskać krótkie instrukcje.

Po ukończeniu samouczka języka C++ użyj przycisków głosowania u dołu tej strony, aby przesłać nam swoją opinię. 

Jeśli chcesz, abyśmy skontaktowali się z Tobą bezpośrednio, możesz w komentarzach podać swój adres e-mail lub [skontaktować się z nami tutaj](https://www.research.net/r/8BKRJ3Z). 

Teraz do dzieła!

## <a name="prerequisites-for-the-c-tutorial"></a>Wymagania wstępne dla samouczka języka C++
Upewnij się, że masz:

* Aktywne konto platformy Azure. Jeśli go nie masz, możesz zarejestrować się w celu uzyskania [bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
* Program [Visual Studio](https://www.visualstudio.com/downloads/) z zainstalowanymi składnikami języka C++.

## <a name="step-1-create-a-documentdb-account"></a>Krok 1. Tworzenie konta usługi DocumentDB
Utwórz konto usługi DocumentDB. Jeśli masz już konto, którego chcesz użyć, możesz przejść od razu do kroku [Konfigurowanie aplikacji języka C++](#SetupNode).

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a id="SetupC++"></a>Krok 2. Konfigurowanie aplikacji języka C++
1. Otwórz program Visual Studio, w menu **Plik** kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**. 
2. W oknie **Nowy projekt** w okienku **Zainstalowane** rozwiń węzeł **Visual C++**, kliknij pozycję **Win32**, a następnie kliknij pozycję **Aplikacja konsolowa Win32**. Nadaj projektowi nazwę hellodocumentdb, a następnie kliknij przycisk **OK**. 
   
    ![Zrzut ekranu przedstawiający kreatora nowego projektu](media/documentdb-cpp-get-started/hellodocumentdb.png)
3. Po uruchomieniu Kreatora aplikacji Win32 kliknij przycisk **Zakończ**.
4. Po utworzeniu projektu otwórz Menedżera pakietów NuGet, klikając prawym przyciskiem myszy projekt **hellodocumentdb** w **Eksploratorze rozwiązań**, a następnie klikając pozycję **Zarządzaj pakietami NuGet**. 
   
    ![Zrzut ekranu przedstawiający pozycję Zarządzaj pakietami NuGet w menu projektu](media/documentdb-cpp-get-started/nuget.png)
5. Na karcie **NuGet: hellodocumentdb** kliknij pozycję **Przeglądaj**, a następnie wyszukaj ciąg *documentdbcpp*. W wynikach wybierz pozycję DocumentDbCPP, jak pokazano na poniższym zrzucie ekranu. Ten pakiet instaluje odwołania do zestawu SDK REST języka C++, który jest zależnością dla pakietu DocumentDbCPP.  
   
    ![Zrzut ekranu przedstawiający wyróżniony pakiet DocumentDbCpp](media/documentdb-cpp-get-started/documentdbcpp.png)
   
    Gdy pakiety zostaną dodane do projektu, będziemy gotowi do rozpoczęcia pisania kodu.   

## <a id="Config"></a>Krok 3. Kopiowanie szczegółów połączenia z witryny Azure Portal dla bazy danych DocumentDB
Wywołaj witrynę [Azure Portal](https://portal.azure.com) i przejdź do utworzonego konta bazy danych NoSQL (DocumentDB). W następnym kroku identyfikator URI i klucz podstawowy z witryny Azure Portal będą nam potrzebne do nawiązania połączenia z naszych fragmentów kodu w języku C++. 

![Identyfikator URI usługi DocumentDB i klucze w witrynie Azure Portal](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

## <a id="Connect"></a>Krok 4. Łączenie z kontem usługi DocumentDB
1. Po wierszu `#include "stdafx.h"` dodaj następujące nagłówki i przestrzenie nazw do kodu źródłowego.
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. Następnie dodaj następujący kod do funkcji main oraz zastąp konfigurację konta i klucz podstawowy, aby dopasować ustawienia usługi DocumentDB z kroku 3. 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    Teraz, gdy masz kod do zainicjowania klienta usługi DocumentDB, przyjrzyjmy się pracy z zasobami usługi DocumentDB.

## <a id="CreateDBColl"></a>Krok 5. Tworzenie bazy danych i kolekcji w języku C++
Zanim wykonamy ten krok, zajmijmy się sposobem interakcji bazy danych, kolekcji i dokumentów dla osób, które zaczynają korzystanie z usługi DocumentDB. [Baza danych](documentdb-resources.md#databases) jest kontenerem logicznym magazynu dokumentów podzielonym na kolekcje. [Kolekcja](documentdb-resources.md#collections) jest kontenerem dokumentów JSON i skojarzonej logiki aplikacji JavaScript. Więcej informacji o modelu zasobów hierarchicznych i pojęciach usługi DocumentDB znajduje się w temacie [Pojęcia i hierarchiczny model zasobów bazy danych DocumentDB](documentdb-resources.md).

Aby utworzyć bazę danych i odpowiednią kolekcję, dodaj następujący kod na końcu Twojej funkcji main. Spowoduje to utworzenie bazy danych o nazwie „FamilyRegistry” i kolekcji o nazwie „FamilyCollection” przy użyciu konfiguracji klienta zadeklarowanej w poprzednim kroku.

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <a id="CreateDoc"></a>Krok 6. Tworzenie dokumentu
[Dokumenty](documentdb-resources.md#documents) są zawartością JSON zdefiniowaną przez użytkownika (dowolną). Teraz można wstawić dokument do usługi DocumentDB. Dokument można utworzyć, kopiując następujący kod i wklejając go na końcu funkcji main. 

    try {
      value document_family;
      document_family[L"id"] = value::string(L"AndersenFamily");
      document_family[L"FirstName"] = value::string(L"Thomas");
      document_family[L"LastName"] = value::string(L"Andersen");
      shared_ptr<Document> doc = coll->CreateDocumentAsync(document_family).get();

      document_family[L"id"] = value::string(L"WakefieldFamily");
      document_family[L"FirstName"] = value::string(L"Lucy");
      document_family[L"LastName"] = value::string(L"Wakefield");
      doc = coll->CreateDocumentAsync(document_family).get();
    } catch (ResourceAlreadyExistsException ex) {
      wcout << ex.message();
    }

Podsumowując, ten kod tworzy bazę danych, kolekcję i dokumenty usługi DocumentDB, do których można tworzyć zapytania w Eksploratorze dokumentów w witrynie Azure Portal. 

![Samouczek języka C++ — diagram pokazujący hierarchiczną relację między kontem, bazą danych, kolekcją i dokumentami](media/documentdb-cpp-get-started/documentdbdocs.png)

## <a id="QueryDB"></a>Krok 7. Wykonywanie zapytań względem zasobów usługi DocumentDB
Usługa DocumentDB obsługuje [zaawansowane zapytania](documentdb-sql-query.md) względem dokumentów JSON przechowywanych w każdej kolekcji. Następujący przykładowy kod przedstawia zapytanie wykonywane przy użyciu składni SQL usługi DocumentDB, które możesz uruchomić względem dokumentów utworzonych w poprzednim kroku.

Funkcja przyjmuje jako argumenty unikatowy identyfikator lub identyfikator zasobu dla bazy danych i kolekcji wraz z klientem dokumentu. Dodaj następujący kod przed funkcją main.

    void executesimplequery(const DocumentClient &client,
                            const wstring dbresourceid,
                            const wstring collresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        wstring coll_name = coll->id();
        shared_ptr<DocumentIterator> iter =
            coll->QueryDocumentsAsync(wstring(L"SELECT * FROM " + coll_name)).get();
        wcout << "\n\nQuerying collection:";
        while (iter->HasMore()) {
          shared_ptr<Document> doc = iter->Next();
          wstring doc_name = doc->id();
          wcout << "\n\t" << doc_name << "\n";
          wcout << "\t"
                << "[{\"FirstName\":"
                << doc->payload().at(U("FirstName")).as_string()
                << ",\"LastName\":" << doc->payload().at(U("LastName")).as_string()
                << "}]";
        }
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Replace"></a>Krok 8. Zastępowanie dokumentu
Usługa DocumentDB obsługuje zastępowanie dokumentów JSON, jak pokazano w poniższym kodzie. Dodaj ten kod po funkcji executesimplequery.

    void replacedocument(const DocumentClient &client, const wstring dbresourceid,
                         const wstring collresourceid,
                         const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        value newdoc;
        newdoc[L"id"] = value::string(L"WakefieldFamily");
        newdoc[L"FirstName"] = value::string(L"Lucy");
        newdoc[L"LastName"] = value::string(L"Smith Wakefield");
        coll->ReplaceDocument(docresourceid, newdoc);
      } catch (DocumentDBRuntimeException ex) {
        throw;
      }
    }

## <a id="Delete"></a>Krok 9. Usuwanie dokumentu
Usługa DocumentDB obsługuje usuwanie dokumentów JSON. W tym celu możesz skopiować następujący kod i wkleić go po funkcji replacedocument. 

    void deletedocument(const DocumentClient &client, const wstring dbresourceid,
                        const wstring collresourceid, const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        coll->DeleteDocumentAsync(docresourceid).get();
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="DeleteDB"></a>Krok 10. Usuwanie bazy danych
Usunięcie utworzonej bazy danych usunie bazę danych i wszystkie zasoby podrzędne (kolekcje, dokumenty itd.).

Skopiuj poniższy fragment kodu (funkcja cleanup) i wklej go po funkcji deletedocument, aby usunąć bazę danych i wszystkie zasoby podrzędne.

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Run"></a>Krok 11. Uruchamianie całej aplikacji w języku C++!
Dodaliśmy kod do tworzenia, modyfikowania i usuwania różnych zasobów usługi DocumentDB oraz do wykonywania względem nich zapytań.  Połączmy to teraz ze sobą, dodając wywołania tych różnych funkcji w naszej funkcji main w pliku hellodocumentdb.cpp wraz z pewnymi komunikatami diagnostycznymi.

Możesz to zrobić, zastępując funkcję main Twojej aplikacji poniższym kodem. Nadpisuje to parametry account_configuration_uri i primary_key, które zostały skopiowane do kodu w kroku 3, więc zapisz ten wiersz lub ponownie skopiuj wartości z portalu. 

    int main() {
        try {
            // Start by defining your account's configuration
            DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
            // Create your client
            DocumentClient client(conf);
            // Create a new database
            try {
                shared_ptr<Database> db = client.CreateDatabase(L"FamilyDB");
                wcout << "\nCreating database:\n" << db->id();
                // Create a collection inside database
                shared_ptr<Collection> coll = db->CreateCollection(L"FamilyColl");
                wcout << "\n\nCreating collection:\n" << coll->id();
                value document_family;
                document_family[L"id"] = value::string(L"AndersenFamily");
                document_family[L"FirstName"] = value::string(L"Thomas");
                document_family[L"LastName"] = value::string(L"Andersen");
                shared_ptr<Document> doc =
                    coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                document_family[L"id"] = value::string(L"WakefieldFamily");
                document_family[L"FirstName"] = value::string(L"Lucy");
                document_family[L"LastName"] = value::string(L"Wakefield");
                doc = coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                replacedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nReplaced document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                deletedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nDeleted document:\n" << doc->id();
                deletedb(client, db->resource_id());
                wcout << "\n\nDeleted db:\n" << db->id();
                cin.get();
            }
            catch (ResourceAlreadyExistsException ex) {
                wcout << ex.message();
            }
        }
        catch (DocumentDBRuntimeException ex) {
            wcout << ex.message();
        }
        cin.get();
    }

Teraz powinna być możliwość skompilowania i uruchomienia kodu w programie Visual Studio po naciśnięciu klawisza F5 lub alternatywnie w oknie terminala, lokalizując aplikację, a następnie uruchamiając plik wykonywalny. 

Powinny zostać wyświetlone dane wyjściowe aplikacji rozpoczynania pracy. Dane wyjściowe powinny odpowiadać poniższemu zrzutowi ekranu.

![Dane wyjściowe aplikacji usługi DocumentDB w języku C++](media/documentdb-cpp-get-started/docdbconsole.png)

Gratulacje! Udało Ci się ukończyć samouczek języka C++ i utworzyć swoją pierwszą aplikację konsolową usługi DocumentDB!

## <a id="GetSolution"></a>Pobieranie kompletnego rozwiązania samouczka języka C++
Aby skompilować rozwiązanie GetStarted, które zawiera wszystkie przykłady z tego artykułu, będą potrzebne następujące elementy:

* [Konto usługi DocumentDB][documentdb-create-account].
* Rozwiązanie [GetStarted](https://github.com/stalker314314/DocumentDBCpp) dostępne w witrynie GitHub.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [monitorować konto usługi DocumentDB](documentdb-monitor-accounts.md).
* Uruchom zapytania względem naszego przykładowego zestawu danych na [placu zabaw dla zapytań](https://www.documentdb.com/sql/demo).
* Dowiedz się więcej o modelu programowania w sekcji Dla deweloperów [strony dokumentacji usługi DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md



