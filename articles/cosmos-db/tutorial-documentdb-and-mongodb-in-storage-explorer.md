---
title: "Zarządzanie Azure rozwiązania Cosmos bazy danych w Eksploratorze usługi Storage platformy Azure"
description: "Dowiedz się, jak zarządzać Azure DB rozwiązania Cosmos w Eksploratorze usługi Storage platformy Azure."
Keywords: Azure Cosmos DB, Azure Storage Explorer, DocumentDB, MongoDB, DocumentDB
services: cosmos-db
documentationcenter: 
author: Jiaj-Li
manager: omafnan
editor: 
tags: Azure Cosmos DB
ms.assetid: 
ms.service: cosmos-db
ms.custom: Azure Cosmos DB active
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: Jiaj-Li
ms.openlocfilehash: fc7d9494ddce127cb402d756f5fb03f21c5d3e8c
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="manage-azure-cosmos-db-in-azure-storage-explorer-preview"></a>Zarządzanie Azure rozwiązania Cosmos bazy danych w Eksploratorze usługi Azure Storage (wersja zapoznawcza)

Przy użyciu bazy danych Azure rozwiązania Cosmos w Eksploratorze usługi Storage Azure umożliwia użytkownikom zarządzanie jednostki bazy danych Azure rozwiązania Cosmos, manipulować danymi, zaktualizuj procedur składowanych i wyzwalaczy oraz inne jednostki Azure, takich jak magazynu obiektów blob i kolejek. Teraz służy tego samego narzędzia do zarządzania z różnymi jednostkami Azure w jednym miejscu. W tej chwili Eksploratora usługi Storage Azure obsługuje konta bazy danych MongoDB i SQL (DocumentDB).

W tym artykule możesz dowiedzieć się, jak zarządzać bazy danych rozwiązania Cosmos Azure za pomocą Eksploratora usługi Storage.


## <a name="prerequisites"></a>Wymagania wstępne

Konto bazy danych rozwiązania Cosmos Azure SQL (DocumentDB) lub baza danych MongoDB. Jeśli nie masz konta, możesz utworzyć jedną w portalu Azure, zgodnie z opisem w [bazy danych Azure rozwiązania Cosmos: tworzenie aplikacji sieci web interfejsu API usługi DocumentDB z usług .NET i portalu Azure](create-documentdb-dotnet.md).

## <a name="installation"></a>Instalacja

Zainstaluj najnowsze bitów Eksploratora usługi Storage Azure tutaj: [Eksploratora usługi Storage Azure](https://azure.microsoft.com/features/storage-explorer/), teraz obsługujemy wersji systemu Windows, Linux i komputerów MAC.

## <a name="connect-to-an-azure-subscription"></a>Łączenie się z subskrypcją platformy Azure

1. Po zainstalowaniu **Eksploratora usługi Storage Azure**, kliknij przycisk **wtyczki** ikony po lewej stronie, jak pokazano na poniższej ilustracji.
       
   ![Podłącz ikony](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/plug-in-icon.png)
 
2. Wybierz **Dodaj konto Azure**, a następnie kliknij przycisk **logowania**.

   ![Łączenie się z subskrypcją platformy Azure](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connect-to-azure-subscription.png)

2. W **logowania do usługi Azure** wybierz pozycję **Zaloguj**, a następnie wprowadź poświadczenia platformy Azure.

    ![Logowanie](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/sign-in.png)

3. Wybierz subskrypcję z listy, a następnie kliknij przycisk **Zastosuj**.

    ![Składanie wniosku o przyjęcie do programu](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/apply-subscription.png)

    W okienku Eksplorator aktualizacji i wyświetla konta w wybranej subskrypcji.

    ![Lista kont](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/account-list.png)

    Pomyślnie nawiązano połączenie z **konta bazy danych Azure rozwiązania Cosmos** do subskrypcji platformy Azure.

## <a name="connect-to-azure-cosmos-db-by-using-a-connection-string"></a>Łączenie do bazy danych rozwiązania Cosmos Azure przy użyciu parametrów połączenia

Alternatywny sposób łączenia z bazą danych Azure rozwiązania Cosmos jest używane są parametry połączenia. Nawiązywanie połączenia przy użyciu parametrów połączenia, wykonaj następujące kroki.

1. Znajdź **lokalnej i dołączonego** w drzewie po lewej stronie kliknij prawym przyciskiem myszy **kont DB rozwiązania Cosmos Azure**, wybierz **Połącz z bazy danych Azure rozwiązania Cosmos...**

    ![Łączenie do bazy danych Azure rozwiązania Cosmos przez ciąg połączenia](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connect-to-db-by-connection-string.png)

2. Wybierz odpowiedni **domyślne środowisko** dla danego typu konta albo **DocumentDB** lub **bazy danych MongoDB**, Wklej w Twojej **parametry połączenia**, a następnie kliknij przycisk **OK** do łączenia z konta bazy danych Azure rozwiązania Cosmos. Aby uzyskać informacje na podczas pobierania ciągu połączenia, zobacz [pobrać ciągu połączenia](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string).

    ![Parametry połączenia](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/connection-string.png)

## <a name="azure-cosmos-db-resource-management"></a>Zarządzanie zasobami w usłudze Azure DB rozwiązania Cosmos

Konto bazy danych rozwiązania Cosmos Azure można zarządzać za pomocą ćwiczeń następujące operacje:
* Otwórz konto w portalu Azure
* Dodaj zasób do listy szybki dostęp
* Wyszukiwanie i Odśwież zasobów
* Tworzenie i usuwanie baz danych
* Tworzenie i usuwanie kolekcji
* Tworzenia, edytowania, usuwania i filtrowanie dokumentów
* Zarządzanie procedur składowanych, wyzwalaczy i funkcji zdefiniowanych przez użytkownika

### <a name="quick-access-tasks"></a>Zadania szybki dostęp

Klikając prawym przyciskiem myszy subskrypcję w okienku Eksplorator, można wykonywać wiele zadań szybkich akcji:

* Kliknij prawym przyciskiem myszy konto bazy danych rozwiązania Cosmos Azure lub bazą danych, możesz wybrać **Otwórz w portalu** i zarządzać zasobem w przeglądarce w portalu Azure.

     ![Otwórz w portalu](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/open-in-portal.png)

* Możesz także dodać konto bazy danych Azure rozwiązania Cosmos, bazy danych, kolekcji **szybki dostęp**.
* **Wyszukiwanie w tym miejscu** umożliwia wyszukiwanie słów kluczowych w ramach wybranej ścieżki.

    ![Wyszukiwanie w tym miejscu](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/search-from-here.png) 

### <a name="database-and-collection-management"></a>Zarządzanie bazą danych i kolekcji
#### <a name="create-a-database"></a>Tworzenie bazy danych 
Kliknij prawym przyciskiem myszy konto bazy danych rozwiązania Cosmos Azure, wybierz **Create Database**, wprowadzania nazwy bazy danych, a następnie naciśnij klawisz **Enter** do wykonania.

![Tworzenie bazy danych](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-database.png) 

#### <a name="delete-a-database"></a>Usuwanie bazy danych
Kliknij prawym przyciskiem myszy bazę danych, kliknij przycisk **usunąć bazy danych**i kliknij przycisk **tak** w oknie podręcznym. Węzeł bazy danych zostanie usunięta, a konto bazy danych rozwiązania Cosmos Azure były odświeżane automatycznie.

![Usuń database1](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-database1.png)  

![Usuń database2](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-database2.png) 

#### <a name="create-a-collection"></a>Tworzenie kolekcji
Kliknij prawym przyciskiem myszy bazę danych, wybierz **Utwórz kolekcję**, a następnie podaj następujące informacje, takie jak **identyfikator kolekcji**, **pojemności**itp. Kliknij przycisk **OK**, aby zakończyć. Aby uzyskać informacje na temat ustawień klucza partycji, zobacz [projektu dla partycjonowania](partition-data.md#designing-for-partitioning).

Użycie klucza partycji podczas tworzenia kolekcji, po zakończeniu tworzenia na kolekcji nie można zmienić wartość klucza partycji.

![Utwórz collection1](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-collection.png)

![Utwórz collection2](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/create-collection2.png) 

#### <a name="delete-a-collection"></a>Usuwanie kolekcji
Kliknij prawym przyciskiem myszy kolekcję, kliknij przycisk **Usuń kolekcję**, a następnie kliknij przycisk **tak** w oknie podręcznym. 

Węzeł kolekcji zostanie usunięta, a bazy danych były odświeżane automatycznie.  

![Usuń kolekcję](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/delete-collection.png) 

### <a name="document-management"></a>Zarządzanie dokumentami

#### <a name="create-and-modify-documents"></a>Tworzenie i modyfikowanie dokumentów
Aby utworzyć nowy dokument, otwórz **dokumenty** w oknie po lewej stronie kliknij **nowy dokument**, Edytuj zawartość, w okienku po prawej stronie, a następnie kliknij przycisk **zapisać**. Możesz również zaktualizować istniejący dokument, a następnie kliknij przycisk **zapisać**. Zmiany mogą odrzucone przez kliknięcie przycisku **odrzucić**.

![Dokument](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/document.png)

#### <a name="delete-a-document"></a>Usuwanie dokumentu
Kliknij przycisk **usunąć** przycisk, aby usunąć wybrany dokument.
#### <a name="query-for-documents"></a>Zapytanie dla dokumentów
Edytuj filtr dokumentu, wprowadzając [zapytania SQL](documentdb-sql-query.md) , a następnie kliknij przycisk **Zastosuj**.

![Filtr](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/filter.png)

### <a name="manage-stored-procedures-triggers-and-udfs"></a>Zarządzanie procedur składowanych, wyzwalaczy i funkcji UDF
* Można utworzyć procedury składowanej, w drzewie po lewej stronie, kliknij prawym przyciskiem myszy **procedury składowanej**, wybierz **Utwórz procedurę składowaną**, wprowadź nazwę w lewej, wpisz skrypty procedury składowanej w prawym okienku, a następnie Kliknij przycisk **Utwórz**. 
* Można również edytować istniejące procedury składowanej dwukrotnie, wprowadzania aktualizacji, a następnie klikając polecenie **aktualizacji** Aby zapisać, lub kliknij przycisk **odrzucić** by anulować zmianę.

![Procedura składowana](./media/tutorial-documentdb-and-mongodb-in-storage-explorer/stored-procedure.png)

* Operacje dla **wyzwalaczy** i **UDF** są podobne do reguł **procedur składowanych**.

## <a name="next-steps"></a>Następne kroki

* Obejrzyj następujące wideo, aby zobaczyć, jak używać bazy danych Azure rozwiązania Cosmos w Eksploratorze usługi Azure Storage: [używać rozwiązania Cosmos bazy danych Azure w Eksploratorze usługi Storage Azure](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
* Dowiedz się więcej o Eksploratora usługi Storage i połącz jedną usługę w [wprowadzenie do Eksploratora usługi Storage (wersja zapoznawcza)](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

