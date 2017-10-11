---
title: "Użyj MongoChef dla rozwiązania Cosmos Azure DB | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak korzystać z bazy danych Azure rozwiązania Cosmos MongoChef: interfejsu API dla konta bazy danych MongoDB"
keywords: mongochef
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: anhoh
ms.openlocfilehash: 54c9799bd646b827f602e2ea2f9a15a4fc853f00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Użyj MongoChef z bazy danych Azure rozwiązania Cosmos: interfejsu API dla konta bazy danych MongoDB

Aby nawiązać połączenie bazy danych Azure rozwiązania Cosmos: interfejsu API dla konta bazy danych MongoDB, musi:

* Pobierz i zainstaluj [MongoChef](http://3t.io/mongochef)
* Z bazy danych rozwiązania Cosmos Azure ma: interfejsu API dla konta bazy danych MongoDB [ciąg połączenia](connect-mongodb-account.md) informacji

## <a name="create-the-connection-in-mongochef"></a>Utwórz połączenie w MongoChef
Aby dodać bazy danych programu Azure rozwiązania Cosmos: interfejsu API dla konta bazy danych MongoDB MongoChef Menedżera połączeń, wykonaj następujące kroki.

1. Pobieranie Twojej bazy danych rozwiązania Cosmos Azure: interfejs API dla bazy danych MongoDB informacje o połączeniu z instrukcjami [tutaj](connect-mongodb-account.md).

    ![Zrzut ekranu przedstawiający blok ciągu połączenia](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Kliknij przycisk **Connect** aby otworzyć Menedżera połączeń, a następnie przycisk **nowego połączenia**

    ![Zrzut ekranu przedstawiający MongoChef Menedżera połączeń](./media/mongodb-mongochef/ConnectionManager.png)
3. W **nowe połączenie** okna na **serwera** wprowadź hosta (FQDN) Azure DB rozwiązania Cosmos: interfejs API dla konta bazy danych MongoDB i numer portu.

    ![Zrzut ekranu przedstawiający kartę MongoChef połączenie Menedżera serwera](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. W **nowe połączenie** okna na **uwierzytelniania** , wybierz pozycję tryb uwierzytelniania **Standard (CR bazy danych MONGODB lub SCARM-SHA-1)** , a następnie wprowadź nazwę użytkownika i hasło.  Zaakceptuj domyślną db uwierzytelniania (Administrator) lub podać własne wartości.

    ![Zrzut ekranu: karta Uwierzytelnianie Menedżera połączeń MongoChef](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. W **nowe połączenie** okna na **SSL** karcie wyboru **protokołu SSL używany nawiązać** pole wyboru i **akceptować certyfikaty SSL z podpisem własnym serwera**  przycisk radiowy.

    ![Zrzut ekranu: karta SSL Menedżera połączeń MongoChef](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Kliknij przycisk **Testuj połączenie** przycisk, aby sprawdzić informacje o połączeniu, kliknij przycisk **OK** powrócić do okna nowe połączenie, a następnie kliknij przycisk **zapisać**.

    ![Zrzut ekranu przedstawiający okno MongoChef testu połączenia](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-to-create-a-database-collection-and-documents"></a>Użyj MongoChef, aby utworzyć bazę danych, kolekcji i dokumentów
Aby utworzyć bazę danych, kolekcji i dokumentów za pomocą MongoChef, wykonaj następujące kroki.

1. W **Menedżera połączeń**, zaznacz połączenie i kliknij **Connect**.

    ![Zrzut ekranu przedstawiający MongoChef Menedżera połączeń](./media/mongodb-mongochef/ConnectToAccount.png)
2. Kliknij prawym przyciskiem myszy hosta, a następnie wybierz pozycję **dodawania bazy danych**.  Podaj nazwę bazy danych, a następnie kliknij przycisk **OK**.

    ![Zrzut ekranu opcji MongoChef dodawania bazy danych](./media/mongodb-mongochef/AddDatabase1.png)
3. Kliknij prawym przyciskiem myszy kliknij bazę danych i wybierz polecenie **Dodaj kolekcji**.  Podaj nazwę kolekcji, a następnie kliknij przycisk **Utwórz**.

    ![Zrzut ekranu opcji MongoChef Dodaj kolekcji](./media/mongodb-mongochef/AddCollection.png)
4. Kliknij przycisk **kolekcji** menu elementu, następnie kliknij przycisk **Dodawanie dokumentu**.

    ![Zrzut ekranu przedstawiający element menu MongoChef Dodawanie dokumentu](./media/mongodb-mongochef/AddDocument1.png)
5. W oknie dialogowym Dodawanie dokumentu, wklej następujący, a następnie kliknij przycisk **Dodawanie dokumentu**.

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
               { "firstName": "Thomas" },
               { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }
6. Dodaj teraz o następującej zawartości innego dokumentu.

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                 "givenName": "Jesse",
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            {
                "familyName": "Miller",
                 "givenName": "Lisa",
                 "gender": "female",
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }
7. Wykonaj przykładowe zapytanie. Na przykład wyszukaj o nazwisko "Andersen" i zwróć pola stanu i elementów nadrzędnych.

    ![Zrzut ekranu przedstawiający Mongo Chef wyników zapytania](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Następne kroki
* Zapoznaj się z rozwiązania Cosmos Azure DB: Interfejs API, bazy danych mongodb [przykłady](mongodb-samples.md).
