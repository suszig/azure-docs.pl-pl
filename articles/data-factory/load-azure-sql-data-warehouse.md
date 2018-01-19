---
title: "Ładowanie danych do usługi Azure SQL Data Warehouse przy użyciu fabryki danych Azure | Dokumentacja firmy Microsoft"
description: "Użyj fabryki danych Azure, aby skopiować dane do magazynu danych SQL Azure"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: 36e24da50386d1abc441e2beb09f570a9612a346
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="load-data-into-azure-sql-data-warehouse-using-azure-data-factory"></a>Ładowanie danych do usługi Azure SQL Data Warehouse przy użyciu fabryki danych Azure

[Usługa Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) oparte na chmurze, skalowalnych w poziomie bazy danych jest w stanie przetwarzanie bardzo dużych woluminów danych relacyjnych i nierelacyjnych.  Zbudowany w oparciu o architekturę masowego przetwarzania równoległego (MPP), usługa SQL Data Warehouse jest zoptymalizowana pod kątem obciążeń magazynu danych przedsiębiorstwa.  Zapewnia elastyczność chmury elastycznie skalować magazyn i zasobów obliczeniowych niezależnie.

Wprowadzenie do korzystania z usługi Azure SQL Data Warehouse jest teraz łatwiejsze niż kiedykolwiek przy użyciu **fabryki danych Azure**.  Fabryka danych Azure to usługa integracji pełni zarządzanych danych oparte na chmurze, używany do wypełniania SQL Data Warehouse przy użyciu danych z istniejącego systemu i zapisywanie cenny czas podczas oceny usługi SQL Data Warehouse i tworzenia rozwiązań analizy. Poniżej przedstawiono najważniejsze zalety ładowania danych do usługi Azure SQL Data Warehouse przy użyciu fabryki danych Azure:

* **Można skonfigurować**: Kreator intuicyjne krok 5 z bez skryptu wymagane.
* **Obsługa magazynu danych sformatowanego**: wbudowaną obsługę bogaty zestaw lokalnymi i magazyny danych oparte na chmurze, zobacz szczegółową listę w [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats) tabeli.
* **Bezpieczne i zgodne**: dane są przesyłane za pośrednictwem protokołu HTTPS lub ExpressRoute i zapewnia obecności usługi globalne dane nigdy nie przekracza granic geograficznych
* **Bezkonkurencyjne wydajności przy użyciu programu PolyBase**: przy użyciu programu Polybase jest najbardziej wydajnym sposobem przenoszenia danych do usługi Azure SQL Data Warehouse. Funkcja tymczasowych obiektów blob, można osiągnąć szybkości wysokie obciążenie ze wszystkich typów magazynów danych oprócz magazynu obiektów Blob platformy Azure i usługi Data Lake Store, który aparat Polybase obsługuje domyślnie. Dowiedz się więcej szczegółów z [wydajności działania kopiowania](copy-activity-performance.md).

W tym artykule przedstawiono sposób użycia narzędzia kopii fabryki danych do **ładowanie danych z bazy danych SQL Azure do usługi Azure SQL Data Warehouse**. Można wykonać podobne kroki można skopiować danych z innych typów magazynów danych.

> [!NOTE]
>  Aby uzyskać ogólne informacje o możliwościach fabryki danych podczas kopiowania danych do/z usługi Azure SQL Data Warehouse, zobacz [kopiowanie danych do i z usługi Azure SQL Data Warehouse przy użyciu fabryki danych Azure](connector-azure-sql-data-warehouse.md) artykułu.
>
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [działanie kopiowania w wersji 1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Wymagania wstępne

* **Subskrypcja platformy Azure**. Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.
* **Magazyn Azure SQL Data Warehouse**. Ten magazyn danych służy do przechowywania danych skopiowanych z bazy SQL Database. Jeśli nie masz magazynu Azure SQL Data Warehouse, utwórz go, wykonując czynności przedstawione w artykule [Create an Azure SQL Warehouse (Tworzenie magazynu Azure SQL Data Warehouse)](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md).
* **Usługa Azure SQL Database**. W tym samouczku kopiuje dane z bazy danych SQL Azure z firmy Adventure Works LT przykładowe dane, można utworzyć jeden następujące [tworzenie bazy danych Azure SQL](../sql-database/sql-database-get-started-portal.md) artykułu. 
* **Konto usługi Azure Storage**. Magazyn Azure jest używany jako **przemieszczania** obiektów blob w operacji kopiowania zbiorczego. Jeśli nie masz konta usługi Azure Storage, utwórz je, wykonując czynności przedstawione w artykule [Tworzenie konta magazynu](../storage/common/storage-create-storage-account.md#create-a-storage-account).

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych

1. Kliknij przycisk **NOWY** w lewym menu, kliknij pozycję **Dane + analiza**, a następnie kliknij przycisk **Data Factory**. 
   
   ![Nowy-> Fabryka danych](./media/load-azure-sql-data-warehouse/new-azure-data-factory-menu.png)
2. W **nowa fabryka danych** Podaj wartości, jak pokazano na poniższym zrzucie ekranu:
      
     ![Nowa strona fabryki danych](./media/load-azure-sql-data-warehouse/new-azure-data-factory.png)
 
   * **Nazwa.** Wprowadź unikatową nazwę globalne dla fabryki danych. Jeśli wystąpi poniższy błąd, zmień nazwę fabryki danych (np. twojanazwaADFTutorialDataFactory) i spróbuj utworzyć ją ponownie. Zobacz [fabryki danych - reguły nazewnictwa](naming-rules.md) artykułu dla reguł nazewnictwa artefakty fabryki danych.
  
            `Data factory name "LoadSQLDWDemo" is not available`

    * **Subskrypcja.** Wybierz **subskrypcję** Azure, w której chcesz utworzyć fabrykę danych. 
    * **Grupy zasobów.** Wybierz istniejącą grupę zasobów z listy rozwijanej lub wybierz **Utwórz nowy** opcję i wprowadź nazwę grupy zasobów. Informacje na temat grup zasobów znajdują się w artykule [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Używanie grup zasobów do zarządzania zasobami platformy Azure).  
    * **Wersja.** Wybierz **V2 (wersja zapoznawcza)**.
    * **Lokalizacja.** Wybierz lokalizację dla fabryki danych. Tylko obsługiwane lokalizacje są wyświetlane na liście rozwijanej. Magazyny danych (magazyn Azure, baza danych SQL Azure itp.) używany przez fabryki danych może być w innych lokalizacjach lub regionach. 

3. Kliknij przycisk **Utwórz**.
4. Po utworzeniu pełnej, przejdź do pozycji z fabryką danych i zostanie wyświetlony **fabryki danych** strony, jak pokazano w obrazie. Kliknij przycisk **autora & Monitor** Kafelek, aby uruchomić aplikację integracji danych w osobnej karcie.
   
   ![Strona główna fabryki danych](./media/load-azure-sql-data-warehouse/data-factory-home-page.png)

## <a name="load-data-into-azure-sql-data-warehouse"></a>Ładowanie danych do usługi Azure SQL Data Warehouse

1. W strony wprowadzenie, kliknij przycisk **kopiowania danych** Kafelek, aby uruchomić narzędzie Kopia danych. 

   ![Kopiowanie danych narzędzie kafelka](./media/load-azure-sql-data-warehouse/copy-data-tool-tile.png)
2. W **właściwości** strony narzędzia kopii danych, określ **CopyFromSQLToSQLDW** dla **Nazwa zadania**i kliknij przycisk **dalej**. 

    ![Kopiowanie danych strony właściwości narzędzia](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)
3. W **magazynu danych źródła** wybierz pozycję **bazy danych SQL Azure**i kliknij przycisk **dalej**.

    ![Strona magazynu danych źródła](./media/load-azure-sql-data-warehouse/specify-source.png)
4. W **Określ bazę danych Azure SQL** wykonaj następujące czynności: 
    1. Wybierz serwer Azure SQL w celu **nazwy serwera**.
    2. Wybierz bazy danych Azure SQL, aby **Nazwa bazy danych**.
    3. Określ nazwę użytkownika dla **nazwy użytkownika**.
    4. Określ hasło użytkownika, **hasło**.
    5. Kliknij przycisk **Dalej**. 

        ![Określ bazy danych Azure SQL](./media/load-azure-sql-data-warehouse/specify-source-connection.png)
5. W **wybierz tabele, z których chcesz skopiować dane, lub użyć niestandardowej kwerendy** strony, filtrów tabel, określając **SalesLT** w polu wejściowym, następnie sprawdź **(Zaznacz wszystko)** pole wyboru, aby wybrać wszystkie tabele, a następnie kliknij przycisk **dalej**. 

    ![Wybierz wejściowy plik lub folder](./media/load-azure-sql-data-warehouse/select-source-tables.png)

6. W **magazyn danych docelowy** wybierz pozycję **magazyn danych SQL Azure**i kliknij przycisk **dalej**. 

    ![Docelowa strona magazynu danych](./media/load-azure-sql-data-warehouse/specify-sink.png)
7. W **określić magazyn danych SQL Azure** wykonaj następujące czynności: 

    1. Wybierz serwer Azure SQL w celu **nazwy serwera**.
    2. Wybierz magazyn danych SQL Azure do **Nazwa bazy danych**.
    3. Określ nazwę użytkownika dla **nazwy użytkownika**.
    4. Określ hasło użytkownika, **hasło**.
    5. Kliknij przycisk **Dalej**. 

        ![Określ magazyn danych Azure SQL](./media/load-azure-sql-data-warehouse/specify-sink-connection.png)
8. W **mapowania tabeli** strony Przejrzyj, a następnie kliknij przycisk **dalej**. Mapowania tabeli inteligentnego pojawia się, że mapy źródła do tabel docelowych, na podstawie tabeli nazw. Jeśli tabela nie istnieje w docelowym, domyślnie fabryki danych Azure tworzy jeden o takiej samej nazwie. Można również mapować do istniejącej tabeli. 

    > [!NOTE]
    > Automatycznego tworzenia tabel dla ujścia magazynu danych SQL ma zastosowanie, gdy serwer SQL lub bazy danych SQL Azure jest źródłem. Po skopiowaniu danych z innego źródła magazynu danych potrzebne do wstępnie tworzenia schematu w ujściu usługi Azure SQL Data Warehouse przed kopię danych.

    ![Strona mapowania tabeli](./media/load-azure-sql-data-warehouse/specify-table-mapping.png)

9. W **mapowanie schematu** strony Przejrzyj, a następnie kliknij przycisk **dalej**. Mapowanie inteligentnego jest oparta na nazwę kolumny. Jeśli chcesz umożliwić fabryki danych automatycznego tworzenia tabel, konwersja typu danych właściwe może się tak zdarzyć w razie potrzeby, aby naprawić niezgodności między magazynami źródłowym i docelowym. W przypadku konwersji typu nieobsługiwanych danych między kolumną źródłowym i docelowym, zostanie wyświetlony komunikat o błędzie równolegle z tej tabeli.

    ![Strona mapowanie schematu](./media/load-azure-sql-data-warehouse/specify-schema-mapping.png)

11. W **ustawienia** wybierz magazyn Azure w **nazwy konta magazynu** listy rozwijanej. Służy do przemieszczania danych przed załadowaniem do usługi SQL Data Warehouse przy użyciu programu PolyBase. Po zakończeniu kopiowania Tymczasowe dane w magazynie zostaną automatycznie wyczyszczone. 

    W obszarze "Ustawienia zaawansowane" Usuń zaznaczenie opcji "Użyj domyślnej typu".

    ![Ustawienia strony](./media/load-azure-sql-data-warehouse/copy-settings.png)
12. W **Podsumowanie** strony, przejrzyj ustawienia i kliknij przycisk **dalej**.

    ![Strona podsumowania](./media/load-azure-sql-data-warehouse/summary-page.png)
13. W **stronę wdrożenia**, kliknij przycisk **monitora** do monitorowania procesu (zadania).

    ![Strona wdrożenia](./media/load-azure-sql-data-warehouse/deployment-page.png)
14. Zwróć uwagę, że **Monitor** wybierana jest karta po lewej stronie. Zobacz łącza, aby wyświetlić szczegóły uruchomienia działania i ponowne uruchomienie procesu w **akcje** kolumny. 

    ![Uruchamia Monitor potoku](./media/load-azure-sql-data-warehouse/monitor-pipeline-run.png)
15. Aby wyświetlić skojarzone z potoku Uruchom uruchomień działania, kliknij przycisk **odbywa się działanie widoku** łącze w **akcje** kolumny. 10 działania kopiowania w potoku, w każdym kopiuje jedna tabela danych. Aby powrócić do potoku uruchamia widok, kliknij przycisk **potoki** łącze u góry. Kliknij przycisk **Odśwież** Aby odświeżyć listę. 

    ![Monitor uruchomień działania](./media/load-azure-sql-data-warehouse/monitor-activity-run.png)

16. Dodatkowo można monitorować szczegóły wykonywania każde działanie kopiowania, klikając **szczegóły** łącze w obszarze **akcje** w działaniu widok monitorowania. Przedstawia on informacje o tym ilość danych skopiowana ze źródła do zbiornika, przepływności, kroki przechodzi przez o odpowiedni czas trwania i użyć konfiguracji.

    ![Monitorowanie aktywności szczegóły uruchomienia](./media/load-azure-sql-data-warehouse/monitor-activity-run-details.png)

## <a name="next-steps"></a>Kolejne kroki

Przejdź do następujących artykuł, aby dowiedzieć się więcej na temat obsługi usługi Azure SQL Data Warehouse: 

> [!div class="nextstepaction"]
>[Łącznik usługi Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md)