---
title: "Kopiowanie danych przy użyciu narzędzia do kopiowania danych platformy Azure | Microsoft Docs"
description: "Utwórz fabrykę Azure Data Factory, a następnie za pomocą narzędzia do kopiowania danych skopiuj dane z folderu w magazynie Azure Blob Storage do innego folderu."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/16/2018
ms.author: jingwang
ms.openlocfilehash: c17e738e3db18503f7cdda24a5f01ceb78e4e6a3
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="use-copy-data-tool-to-copy-data"></a>Kopiowanie danych przy użyciu narzędzia do kopiowania danych 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Wersja 2 — wersja zapoznawcza](quickstart-create-data-factory-copy-data-tool.md)

W tym przewodniku Szybki start użyjesz witryny Azure Portal do utworzenia fabryki danych. Następnie za pomocą narzędzia do kopiowania danych utworzysz potok służący do kopiowania danych z folderu w magazynie Azure Blob Storage do innego folderu. 

> [!NOTE]
> Jeśli jesteś nowym użytkownikiem usługi Azure Data Factory, przed wykonaniem kroków zawartych w tym przewodniku Szybki start zobacz [Wprowadzenie do usługi Azure Data Factory](data-factory-introduction.md). 
>
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz artykuł z [wprowadzeniem do usługi Data Factory w wersji 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych

1. Kliknij przycisk **Nowy** w lewym menu, kliknij pozycję **Dane + analiza**, a następnie kliknij pozycję **Data Factory**. 
   
   ![Nowy-> Fabryka danych](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory-menu.png)
2. Na stronie **Nowa fabryka danych** wprowadź wartość **ADFTutorialDataFactory** w polu **Nazwa**. 
      
     ![Strona Nowa fabryka danych](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory.png)
 
   Nazwa fabryki danych platformy Azure musi być **globalnie unikatowa**. Jeśli dla pola nazwy wystąpi poniższy błąd, zmień nazwę fabryki danych (np. twojanazwaADFTutorialDataFactory), a następnie ponów próbę utworzenia. Artykuł [Data Factory — Naming Rules (Usługa Data Factory — reguły nazewnictwa)](naming-rules.md) zawiera reguły nazewnictwa artefaktów usługi Data Factory.
  
     ![Nazwa nie jest dostępna — błąd](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. Wybierz **subskrypcję** Azure, w której chcesz utworzyć fabrykę danych. 
4. Dla opcji **Grupa zasobów** wykonaj jedną z następujących czynności:
     
      - Wybierz pozycję **Użyj istniejącej**, a następnie wybierz istniejącą grupę zasobów z listy rozwijanej. 
      - Wybierz pozycję **Utwórz nową**, a następnie wprowadź nazwę grupy zasobów.   
         
      Informacje na temat grup zasobów znajdują się w artykule [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Używanie grup zasobów do zarządzania zasobami platformy Azure).  
4. Wybierz wartość **V2 (wersja zapoznawcza)** dla **wersji**.
5. Na liście **lokalizacja** wybierz lokalizację fabryki danych. Na liście rozwijanej są wyświetlane tylko obsługiwane lokalizacje. Magazyny danych (Azure Storage, Azure SQL Database itp.) i jednostki obliczeniowe (HDInsight itp.) używane przez fabrykę danych mogą mieścić się w innych lokalizacjach/regionach.
6. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**.     
7. Kliknij przycisk **Utwórz**.
8. Na pulpicie nawigacyjnym jest widoczny następujący kafelek ze stanem: **Wdrażanie fabryki danych**. 

    ![kafelek Wdrażanie fabryki danych](media/quickstart-create-data-factory-copy-data-tool/deploying-data-factory.png)
9. Po zakończeniu tworzenia zostanie wyświetlona strona **Fabryka danych**, jak pokazano na poniższej ilustracji.
   
   ![Strona główna fabryki danych](./media/quickstart-create-data-factory-copy-data-tool/data-factory-home-page.png)
10. Kliknij kafelek **Tworzenie i monitorowanie**, aby w osobnej karcie uruchomić interfejs użytkownika usługi Azure Data Factory. 

## <a name="launch-copy-data-tool"></a>Uruchamianie narzędzia do kopiowania danych

1. Na stronie wprowadzenia kliknij kafelek **Kopiuj dane**, aby uruchomić narzędzie do kopiowania danych. 

   ![Kafelek narzędzia do kopiowania danych](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-tile.png)
2. Na stronie **Właściwości** narzędzia do kopiowania danych kliknij przycisk **Dalej**. Na tej stronie możesz określić nazwę potoku i jego opis. 

    ![Narzędzie do kopiowania danych — strona Właściwości](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-properties-page.png)
3. Na stronie **Magazyn danych źródłowych** wybierz pozycję **Azure Blob Storage**, a następnie kliknij przycisk **Dalej**.

    ![Strona Źródłowy magazyn danych](./media/quickstart-create-data-factory-copy-data-tool/source-data-store-page.png)
4. Na stronie **Określanie konta usługi Azure Blob Storage** wybierz **nazwę konta magazynu** z listy rozwijanej, a następnie kliknij przycisk Dalej. 

    ![Określanie konta magazynu obiektów blob](./media/quickstart-create-data-factory-copy-data-tool/specify-blob-storage-account.png)
5. Na stronie **Wybieranie pliku lub folderu wejściowego** wykonaj następujące czynności:

    1. Przejdź do folderu **adftutorial/input**. 
    2. Wybierz plik **emp.txt**.
    3. Kliknij pozycję **Wybierz**. Możesz dwukrotnie kliknąć plik **emp.txt**, aby pominąć ten krok. 
    4. Kliknij przycisk **Dalej**. 

    ![Wybieranie pliku lub folderu wejściowego](./media/quickstart-create-data-factory-copy-data-tool/choose-input-file-folder.png)
6. Na stronie **Ustawienia formatu pliku** zwróć uwagę, że narzędzie automatycznie wykrywa ograniczniki kolumn i wierszy, a następnie kliknij przycisk **Dalej**. Na tej stronie możesz także wyświetlić podgląd danych i wyświetlić schemat danych wejściowych. 

    ![Strona Ustawienia formatu pliku](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-page.png)
7. Na stronie **Docelowy magazyn danych** wybierz pozycję **Azure Blob Storage**, a następnie kliknij przycisk **Dalej**. 

    ![Strona Docelowy magazyn danych](./media/quickstart-create-data-factory-copy-data-tool/destination-data-store-page.png)    
8. Na stronie **Określanie konta usługi Azure Blob Storage** wybierz konto usługi Azure Blob Storage, a następnie kliknij przycisk **Dalej**. 

    ![Strona Określanie magazynu danych ujścia](./media/quickstart-create-data-factory-copy-data-tool/specify-sink-blob-storage-account.png)
9. Na stronie **Wybieranie pliku lub folderu wyjściowego** wykonaj następujące czynności: 

    1. Wprowadź wartość **adftutorial/output** jako **ścieżkę do folderu**.
    2. Wprowadź wartość **emp.txt** jako **nazwę pliku**. 
    3. Kliknij przycisk **Dalej**. 

    ![Wybieranie pliku lub folderu wyjściowego](./media/quickstart-create-data-factory-copy-data-tool/choose-output-file-folder.png) 
10. Na stronie **Ustawienia formatu pliku** kliknij przycisk **Dalej**. 

    ![Strona Ustawienia formatu pliku](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-output-page.png)
11. Na stronie **Ustawienia** kliknij przycisk **Dalej**. 

    ![Strona Ustawienia zaawansowane](./media/quickstart-create-data-factory-copy-data-tool/advanced-settings-page.png)
12. Sprawdź wszystkie ustawienia na stronie **Podsumowanie**, a następnie kliknij przycisk Dalej. 

    ![Strona podsumowania](./media/quickstart-create-data-factory-copy-data-tool/summary-page.png)
13. Na stronie **Zakończono wdrożenie** kliknij pozycję **Monitoruj**, aby monitorować utworzony potok. 

    ![Strona Wdrażanie](./media/quickstart-create-data-factory-copy-data-tool/deployment-page.png)
14. Aplikacja zostanie przełączona na kartę **Monitorowanie**. Na tej stronie wyświetlany jest stan potoku. Kliknij pozycję **Odśwież**, aby odświeżyć listę. 
    
    ![Strona Monitorowanie uruchomień potoku](./media/quickstart-create-data-factory-copy-data-tool/monitor-pipeline-runs-page.png)
15. Kliknij link **Wyświetl uruchomienia działania** w kolumnie Akcje. Potok ma tylko jedno działanie typu **Kopiowanie**. 

    ![Strona Uruchomienia działania](./media/quickstart-create-data-factory-copy-data-tool/activity-runs.png)
16. Aby wyświetlić szczegółowe informacje na temat operacji kopiowania, kliknij link **Szczegóły** (obraz okularów) w kolumnie **Działania**. Aby uzyskać więcej informacji o właściwościach, zobacz [Omówienie działania kopiowania](copy-activity-overview.md). 

    ![Szczegóły operacji kopiowania](./media/quickstart-create-data-factory-copy-data-tool/copy-operation-details.png)
17. Sprawdź, czy plik **emp.txt** został utworzony w folderze **output** w kontenerze **adftutorial**. Jeśli folder wyjściowy nie istnieje, usługa Data Factory automatycznie go utworzy. 
18. Przejdź do karty **Edycja**, aby móc edytować połączone usługi, zestawy danych i potoki. Aby dowiedzieć się więcej o edytowaniu tych elementów w interfejsie użytkownika usługi Data Factory, zobacz [Tworzenie fabryki danych za pomocą witryny Azure Portal](quickstart-create-data-factory-portal.md).

    ![Karta Edycja](./media/quickstart-create-data-factory-copy-data-tool/edit-tab.png)

## <a name="next-steps"></a>Następne kroki
Potok w tym przykładzie kopiuje dane z jednej lokalizacji do innej lokalizacji w usłudze Azure Blob Storage. Zapoznaj się z [samouczkami](tutorial-copy-data-portal.md), aby dowiedzieć się więcej o korzystaniu z usługi Data Factory w dalszych scenariuszach. 