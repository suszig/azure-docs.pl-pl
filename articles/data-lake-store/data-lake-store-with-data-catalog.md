---
title: "Zarejestruj dane z usługi Data Lake Store w usłudze Azure Data Catalog | Dokumentacja firmy Microsoft"
description: "Zarejestruj dane z usługi Data Lake Store w wykazie danych Azure"
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3294d91e-a723-41b5-9eca-ace0ee408a4b
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 90e29a9ab522031db8feb934a218d4b671561417
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Zarejestruj dane z usługi Data Lake Store w wykazie danych Azure
W tym artykule dowiesz się, Integrowanie usługi Azure Data Lake Store w wykazie danych Azure, aby dane integrując z wykazu danych wykrywania urządzeń w organizacji. Aby uzyskać więcej informacji na skatalogowania danych, zobacz [Azure Data Catalog](../data-catalog/data-catalog-what-is-data-catalog.md). Aby poznać scenariusze, w których można używać usługi Data Catalog, zobacz [typowych scenariuszy Azure Data Catalog](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Wymagania wstępne
Przed przystąpieniem do wykonania kroków opisanych w tym samouczku należy dysponować następującymi elementami:

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Włączenie subskrypcji platformy Azure** dla publicznej wersji zapoznawczej Data Lake Store. Zobacz [instrukcje](data-lake-store-get-started-portal.md).
* **Konto usługi Azure Data Lake Store**. Postępuj zgodnie z instrukcjami w temacie [Rozpoczynanie pracy z usługą Azure Data Lake Store za pomocą witryny Azure Portal](data-lake-store-get-started-portal.md). W tym samouczku, możemy utworzyć konto usługi Data Lake Store o nazwie **datacatalogstore**.

    Po utworzeniu konta, Przekaż przykładowe zestaw danych do niego. W tym samouczku, Daj nam przekazać pliki CSV w obszarze **AmbulanceData** folderu w [repozytorium Git programu Azure Data Lake](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Można użyć różnych klientów, takie jak [Eksploratora usługi Storage Azure](http://storageexplorer.com/), aby przekazać dane do kontenera obiektów blob.
* **Wykaz danych Azure**. Twoja organizacja musi mieć już Azure Data Catalog utworzony dla Twojej organizacji. Tylko jeden katalog jest dozwolona dla każdej organizacji.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Register Data Lake Store jako źródło dla usługi Data Catalog

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. Przejdź do `https://azure.microsoft.com/services/data-catalog`i kliknij przycisk **wprowadzenie**.
2. Zaloguj się do portalu wykazu danych Azure, a następnie kliknij przycisk **publikowania danych**.

    ![Zarejestruj źródło danych](./media/data-lake-store-with-data-catalog/register-data-source.png "rejestracji źródła danych")
3. Na następnej stronie kliknij **uruchamianie aplikacji**. Będzie to pobieranie pliku manifestu aplikacji na komputerze. Kliknij dwukrotnie plik manifestu, można uruchomić aplikacji.
4. Na stronie powitalnej kliknij **Zaloguj**i wprowadź swoje poświadczenia.

    ![Ekran powitalny](./media/data-lake-store-with-data-catalog/welcome.screen.png "ekranu powitalnego")
5. Na stronie wybierz stronę źródła danych, wybierz **usługi Azure Data Lake**, a następnie kliknij przycisk **dalej**.

    ![Wybierz źródło danych](./media/data-lake-store-with-data-catalog/select-source.png "wybierz źródło danych")
6. Na następnej stronie podaj nazwę konta usługi Data Lake Store, która ma zostać zarejestrowany w wykazie danych. Inne opcje Pozostaw domyślne, a następnie kliknij przycisk **Connect**.

    ![Połącz ze źródłem danych](./media/data-lake-store-with-data-catalog/connect-to-source.png "połączenia ze źródłem danych")
7. Następnej strony można podzielić na następujące segmenty.

    a. **Hierarchii serwerów** pole reprezentuje struktury folderów konta usługi Data Lake Store. **$Root** reprezentuje element główny konta usługi Data Lake Store i **AmbulanceData** reprezentuje folder utworzony w folderze głównym na koncie usługi Data Lake Store.

    b. **Dostępne obiekty** pole zawiera listę plików i folderów w obszarze **AmbulanceData** folderu.

    c. **Obiekty do zarejestrowanych pole** zawiera listę plików i folderów, które chcesz zarejestrować w usłudze Azure Data Catalog.

    ![Wyświetlanie struktury danych](./media/data-lake-store-with-data-catalog/view-data-structure.png "wyświetlanie struktury danych")
8. W tym samouczku należy zarejestrować wszystkie pliki w katalogu. Do tego, kliknij przycisk (![przenieść obiekty](./media/data-lake-store-with-data-catalog/move-objects.png "przenieść obiekty")), aby przenieść wszystkie pliki do **obiekty do zarejestrowania** pole.

    Dane zostaną zarejestrowane w wykazie danych organizacji, dlatego jest podejście zalecane, aby dodać niektóre metadane, w którym później można szybko znaleźć dane. Na przykład można dodać adres e-mail właściciela danych (na przykład taką, która przekazuje dane), lub dodać tag, aby zidentyfikować dane. Przechwytywanie ekranu poniżej zawiera tag dodamy do danych.

    ![Wyświetlanie struktury danych](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "wyświetlanie struktury danych")

    Kliknij przycisk **zarejestrować**.
9. Następujące przechwytywania ekranu oznacza, że dane zostanie pomyślnie zarejestrowana w wykazie danych.

    ![Zakończono rejestrację](./media/data-lake-store-with-data-catalog/registration-complete.png "wyświetlanie struktury danych")
10. Kliknij przycisk **Wyświetl Portal** wróć do portalu wykazu danych i sprawdź, czy ma teraz dostęp do zarejestrowanych danych, z portalu. Aby wyszukiwać dane, można użyć tagu używanych podczas rejestrowania danych.

     ![Wyszukiwanie danych w katalogu](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "wyszukiwanie danych w katalogu")
11. Można teraz wykonywać operacje, takie jak dodawanie adnotacji i dokumentacji do danych. Aby uzyskać więcej informacji zobacz następujące łącza.

    * [Adnotuj źródła danych w katalogu danych](../data-catalog/data-catalog-how-to-annotate.md)
    * [Źródła danych dokumentów w wykazie danych](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Zobacz też
* [Adnotuj źródła danych w katalogu danych](../data-catalog/data-catalog-how-to-annotate.md)
* [Źródła danych dokumentów w wykazie danych](../data-catalog/data-catalog-how-to-documentation.md)
* [Integracja z innymi usługami Azure Data Lake Store](data-lake-store-integrate-with-other-services.md)
