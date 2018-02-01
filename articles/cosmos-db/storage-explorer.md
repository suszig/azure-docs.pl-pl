---
title: "Zarządzanie usługą Azure Cosmos DB w Eksploratorze usługi Azure Storage"
description: "Dowiedz się, jak zarządzać usługą Azure Cosmos DB w Eksploratorze usługi Azure Storage."
Keywords: Azure Cosmos DB, Azure Storage Explorer, MongoDB
services: cosmos-db
documentationcenter: 
author: jejiang
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
ms.author: Jejiang
ms.openlocfilehash: baa7eee614159f9c6af493aff74b27773471f7d7
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/23/2018
---
# <a name="manage-azure-cosmos-db-in-azure-storage-explorer-preview"></a>Zarządzanie usługą Azure Cosmos DB w Eksploratorze usługi Azure Storage (wersja zapoznawcza)

Korzystanie z usługi Azure Cosmos DB w Eksploratorze usługi Azure Storage pozwala użytkownikom zarządzać jednostkami usługi Azure Cosmos DB, wykonywać operacje na danych oraz aktualizować procedury składowane i wyzwalacze, a także inne jednostki platformy Azure, takie jak obiekty blob i kolejki usługi Storage. Za pomocą jednego narzędzia można teraz centralnie zarządzać różnymi jednostkami platformy Azure. Aktualnie Eksplorator usługi Azure Storage obsługuje konta SQL <!--and MongoDB-->. Eksplorator usługi Azure Storage nie współdziała z lokalnym emulatorem usługi Azure Cosmos DB. 

Ten artykuł zawiera informacje dotyczące zarządzania usługą Azure Cosmos DB za pomocą Eksploratora usługi Storage.


## <a name="prerequisites"></a>Wymagania wstępne

Konto usługi Azure Cosmos DB dla interfejsu API SQL <!--or MongoDB API-->. Jeśli nie masz konta, możesz je utworzyć w witrynie Azure Portal, wykonując instrukcje podane w temacie [Azure Cosmos DB: Tworzenie aplikacji internetowej interfejsu API SQL za pomocą platformy .NET i witryny Azure Portal](create-sql-api-dotnet.md).

## <a name="installation"></a>Instalacja

Zainstaluj najnowszą wersję Eksploratora usługi Azure Storage. Program [Eksplorator usługi Azure Storage](https://azure.microsoft.com/features/storage-explorer/) jest dostępny w wersji dla systemu Windows i Linux oraz dla komputerów MAC.

## <a name="connect-to-an-azure-subscription"></a>Łączenie się z subskrypcją platformy Azure

1. Po zainstalowaniu **Eksploratora usługi Azure Storage** kliknij ikonę **wtyczki** widoczną po lewej stronie, jak pokazano na poniższej ilustracji.
       
   ![Ikona wtyczki](./media/storage-explorer/plug-in-icon.png)
 
2. Wybierz pozycję **Dodaj konto platformy Azure**, a następnie kliknij pozycję **Zaloguj się**.

   ![Łączenie się z subskrypcją platformy Azure](./media/storage-explorer/connect-to-azure-subscription.png)

2. W oknie dialogowym **Logowanie do platformy Azure** wybierz pozycję **Zaloguj**, a następnie wprowadź poświadczenia platformy Azure.

    ![Logowanie](./media/storage-explorer/sign-in.png)

3. Wybierz subskrypcję z listy, a następnie kliknij przycisk **Zastosuj**.

    ![Zastosuj](./media/storage-explorer/apply-subscription.png)

    Zawartość okienka Eksploratora zostanie zaktualizowana i pojawią się konta w wybranej subskrypcji.

    ![Lista kont](./media/storage-explorer/account-list.png)

    Wykonanie powyższych czynności pozwala nawiązać połączenie z **kontem usługi Azure Cosmos DB** i subskrypcją platformy Azure.

## <a name="connect-to-azure-cosmos-db-by-using-a-connection-string"></a>Łączenie się z usługą Azure Cosmos DB przy użyciu parametrów połączenia

Alternatywna metoda połączenia się z usługą Azure Cosmos DB polega na użyciu parametrów połączenia. Aby nawiązać połączenie przy użyciu parametrów połączenia, wykonaj następujące kroki.

1. Znajdź pozycję **Lokalne i dołączone** w drzewie po lewej stronie, kliknij prawym przyciskiem myszy pozycję **Konta usługi Azure Cosmos DB** i wybierz pozycję **Połącz z usługą Azure Cosmos DB**.

    ![Łączenie się z usługą Azure Cosmos DB przy użyciu parametrów połączenia](./media/storage-explorer/connect-to-db-by-connection-string.png)

2. W polu **Środowisko domyślne** wybierz odpowiednie środowisko dla swojego typu konta (<!--either-->**DocumentDB**<!--or **MongoDB**-->), w polu **Parametry połączenia** wklej parametry połączenia, a następnie kliknij przycisk **OK** w celu połączenia się z kontem usługi Azure Cosmos DB. Aby uzyskać informacje dotyczące pobierania parametrów połączenia, zobacz [Get the connection string (Pobieranie parametrów połączenia)](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string).

    ![Parametry połączenia](./media/storage-explorer/connection-string.png)

## <a name="azure-cosmos-db-resource-management"></a>Zarządzanie zasobami usługi Azure Cosmos DB

W ramach zarządzania kontem usługi Azure Cosmos DB można wykonywać następujące operacje:
* Otwieranie konta w witrynie Azure Portal
* Dodawanie zasobu do listy szybkiego dostępu
* Wyszukiwanie i odświeżanie zasobów
* Tworzenie i usuwanie baz danych
* Tworzenie i usuwanie kolekcji
* Tworzenie, edytowanie, usuwanie i filtrowanie dokumentów
* Zarządzanie procedurami składowanymi, wyzwalaczami i funkcjami zdefiniowanymi przez użytkownika

### <a name="quick-access-tasks"></a>Zadania szybkiego dostępu

Po kliknięciu prawym przyciskiem myszy subskrypcji w okienku Eksploratora pojawią się polecenia, które pozwalają wykonywać wiele szybkich działań:

* Po kliknięciu prawym przyciskiem myszy konta lub bazy danych usługi Azure Cosmos DB można wybrać polecenie **Otwórz w portalu**, aby zarządzać zasobem w przeglądarce w witrynie Azure Portal.

     ![Otwórz w portalu](./media/storage-explorer/open-in-portal.png)

* Do paska **Szybki dostęp** można także dodać konto, bazę danych lub kolekcję usługi Azure Cosmos DB.
* Opcja **Wyszukaj od tego miejsca** umożliwia wyszukiwanie słów kluczowych w obrębie wybranej ścieżki.

    ![Wyszukaj od tego miejsca](./media/storage-explorer/search-from-here.png) 

### <a name="database-and-collection-management"></a>Zarządzanie bazą danych i kolekcją
#### <a name="create-a-database"></a>Tworzenie bazy danych 
Kliknij prawym przyciskiem myszy konto usługi Azure Cosmos DB, wybierz polecenie **Utwórz bazę danych**, wprowadź nazwę bazy danych, a następnie naciśnij klawisz **Enter**, aby zakończyć procedurę.

![Tworzenie bazy danych](./media/storage-explorer/create-database.png) 

#### <a name="delete-a-database"></a>Usuwanie bazy danych
Kliknij prawym przyciskiem myszy bazę danych, kliknij polecenie **Usuń bazę danych** i kliknij przycisk **Tak** w oknie podręcznym. Węzeł bazy danych zostanie usunięty, a konto usługi Azure Cosmos DB zostanie odświeżone automatycznie.

![Usuwanie bazy danych 1](./media/storage-explorer/delete-database1.png)  

![Usuwanie bazy danych 2](./media/storage-explorer/delete-database2.png) 

#### <a name="create-a-collection"></a>Tworzenie kolekcji
Kliknij prawym przyciskiem myszy bazę danych, wybierz polecenie **Utwórz kolekcję**, a następnie podaj odpowiednie informacje w polach, takich jak **Identyfikator kolekcji**, **Pojemność magazynu** itd. Kliknij przycisk **OK**, aby zakończyć. Aby uzyskać informacje na temat ustawień klucza partycji, zobacz [Design for partitioning (Projektowanie pod kątem partycjonowania)](partition-data.md#designing-for-partitioning).

Jeśli podczas tworzenia kolekcji jest używany klucz partycji, to po jej utworzeniu nie można zmienić wartości klucza partycji.

![Tworzenie kolekcji 1](./media/storage-explorer/create-collection.png)

![Tworzenie kolekcji 2](./media/storage-explorer/create-collection2.png) 

#### <a name="delete-a-collection"></a>Usuwanie kolekcji
Kliknij prawym przyciskiem myszy kolekcję, kliknij polecenie **Usuń kolekcję**, a następnie kliknij przycisk **Tak** w oknie podręcznym. 

Węzeł kolekcji zostanie usunięty, a baza danych zostanie odświeżona automatycznie.  

![Usuwanie kolekcji](./media/storage-explorer/delete-collection.png) 

### <a name="document-management"></a>Zarządzanie dokumentami

#### <a name="create-and-modify-documents"></a>Tworzenie i modyfikowanie dokumentów
Aby utworzyć nowy dokument, otwórz węzeł **Dokumenty** w lewym oknie, kliknij pozycję **Nowy dokument**, zmodyfikuj zawartość dokumentu w prawym okienku, a następnie kliknij przycisk **Zapisz**. Możesz również zaktualizować istniejący dokument, a następnie kliknąć przycisk **Zapisz**. Aby odrzucić zmiany, kliknij przycisk **Odrzuć**.

![Dokument](./media/storage-explorer/document.png)

#### <a name="delete-a-document"></a>Usuwanie dokumentu
Aby usunąć wybrany dokument, kliknij przycisk **Usuń**.
#### <a name="query-for-documents"></a>Wykonywanie zapytań dotyczących dokumentów
Aby zmodyfikować filtr dokumentu, wprowadź [zapytanie SQL](sql-api-sql-query.md), a następnie kliknij przycisk **Zastosuj**.

![Filtr](./media/storage-explorer/filter.png)

### <a name="manage-stored-procedures-triggers-and-udfs"></a>Zarządzanie procedurami składowanymi, wyzwalaczami i funkcjami definiowanymi przez użytkownika (UDF)
* Aby utworzyć procedurę składowaną, w drzewie po lewej stronie kliknij prawym przyciskiem myszy pozycję **Procedura składowana**, wybierz polecenie **Utwórz procedurę składowaną**, wprowadź nazwę po lewej stronie, wpisz skrypty procedury składowanej w prawym okienku, a następnie kliknij przycisk **Utwórz**. 
* Możesz również edytować istniejące procedury składowane. W tym celu kliknij dwukrotnie procedurę, wprowadź zmiany, a następnie kliknij pozycję **Aktualizuj**, aby zapisać zmiany, lub pozycję **Odrzuć**, aby je anulować.

![Procedura składowana](./media/storage-explorer/stored-procedure.png)

* Operacje dotyczące **wyzwalaczy** i **funkcji definiowanych przez użytkownika** są podobne do operacji dla **procedur składowanych**.

## <a name="next-steps"></a>Następne kroki

* Obejrzyj ten film wideo, aby dowiedzieć się, jak używać usługi Azure Cosmos DB w Eksploratorze usługi Azure Storage: [Use Azure Cosmos DB in Azure Storage Explorer (Używanie usługi Azure Cosmos DB w Eksploratorze usługi Azure Storage)](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
* Aby dowiedzieć się więcej o Eksploratorze usługi Storage i sposobach łączenia się z dodatkowymi usługami, zobacz [Wprowadzenie do programu Storage Explorer (wersja zapoznawcza)](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

