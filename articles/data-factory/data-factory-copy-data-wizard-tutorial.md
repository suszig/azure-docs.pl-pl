---
title: "Samouczek: Tworzenie potoku przy użyciu Kreatora kopiowania | Microsoft Docs"
description: "Ten samouczek zawiera instrukcje dotyczące tworzenia potoku usługi Azure Data Factory za pomocą działania kopiowania przy użyciu Kreatora kopiowania obsługiwanego w usłudze Data Factory"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b87afb8e-53b7-4e1b-905b-0343dd096198
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/06/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 3205077236dd44253b3fa36d6eace36fb307871e
ms.openlocfilehash: 11754bbe534638d8321f509d7d82e025c667176c


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-data-factory-copy-wizard"></a>Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu Kreatora kopiowania usługi Fabryka danych
> [!div class="op_single_selector"]
> * [Przegląd i wymagania wstępne](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Kreator kopiowania](data-factory-copy-data-wizard-tutorial.md)
> * [Witryna Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Program Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [Program PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Szablon usługi Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [Interfejs API REST](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [Interfejs API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

**Kreator kopiowania** usługi Azure Data Factory umożliwia łatwe i szybkie utworzenie potoku wdrażającego scenariusz wprowadzania/przenoszenia danych . Dlatego zalecamy użycie kreatora jako pierwszego kroku procesu tworzenia przykładowego potoku dla scenariusza przenoszenia danych. W tym samouczku pokazano, jak utworzyć fabrykę danych Azure Data Factory, uruchomić Kreatora kopiowania oraz wykonać procedurę umożliwiającą podanie szczegółów scenariusza wprowadzania/przenoszenia danych. Po wykonaniu kroków kreatora zostanie automatycznie utworzony potok z działaniem kopiowania umożliwiający kopiowanie danych z magazynu Azure Blob Storage do bazy danych Azure SQL Database. Szczegółowe informacje dotyczące działania kopiowania znajdują się w artykule [Data Movement Activities](data-factory-data-movement-activities.md) (Działania przenoszenia danych). 

> [!IMPORTANT]
> Zapoznaj się z artykułem [Omówienie samouczka i wymagania wstępne](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md), aby uzyskać omówienie samouczka i wykonać kroki **wymagań wstępnych** przed wykonaniem instrukcji w tym samouczku.
> 
> 

## <a name="create-data-factory"></a>Tworzenie fabryki danych
W tym kroku opisano tworzenie fabryki danych Azure o nazwie **ADFTutorialDataFactory** przy użyciu witryny Azure Portal.

1. Po zalogowaniu się w witrynie [Azure Portal](https://portal.azure.com) kliknij pozycję **+NOWY** w lewym górnym rogu, kliknij pozycję **Zbieranie danych i analiza** i kliknij pozycję **Fabryka danych**. 
   
   ![Nowy->Fabryka danych](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)
2. W bloku **Nowa fabryka danych**:
   
   1. Wprowadź wartość **ADFTutorialDataFactory** dla opcji **Nazwa**.
       Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli wystąpi błąd: **Nazwa fabryki danych „ADFTutorialDataFactory” jest niedostępna**, zmień nazwę fabryki danych (np. TwojaNazwaADFTutorialDataFactory) i spróbuj utworzyć ją ponownie. Artykuł [Data Factory — Naming Rules](data-factory-naming-rules.md) (Fabryka danych — zasady nazewnictwa) zawiera zasady nazewnictwa artefaktów usługi Fabryka danych.  
      
       ![Nazwa fabryki danych jest niedostępna](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)
      
      > [!NOTE]
      > W przyszłości nazwa fabryki danych może zostać zarejestrowana jako nazwa DNS, a wówczas stanie się widoczna publicznie.
      > 
      > 
   2. Wybierz swoją **subskrypcję** platformy Azure.
   3. Wykonaj jedną z następujących czynności dotyczącą grupy zasobów: 
      
      - Wybierz pozycję **Użyj istniejącej**, aby wybrać istniejącą grupę zasobów.
      - Wybierz pozycję **Utwórz nowy**, aby wprowadzić nazwę grupy zasobów.
         
          W niektórych krokach w tym samouczku zakłada się, że nazwa grupy zasobów to **ADFTutorialResourceGroup**. Informacje na temat grup zasobów znajdują się w artykule [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Używanie grup zasobów do zarządzania zasobami platformy Azure).
   4. Wybierz **lokalizację** fabryki danych.
   5. Zaznacz pole wyboru **Przypnij do pulpitu nawigacyjnego** u dołu bloku.  
   6. Kliknij przycisk **Utwórz**.
      
       ![Blok Nowa fabryka danych](media/data-factory-copy-data-wizard-tutorial/new-data-factory-blade.png)            
3. Po zakończeniu tworzenia zostanie wyświetlony blok **Fabryka danych**, jak pokazano na poniższej ilustracji:
   
   ![Strona główna fabryki danych](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## <a name="launch-copy-wizard"></a>Uruchamianie Kreatora kopiowania
1. Na stronie głównej usługi Fabryka danych kliknij kafelek **Kopiowanie danych**, aby uruchomić **Kreatora kopiowania**. 
   
   > [!NOTE]
   > Jeśli widać, że przeglądarka sieci Web jest zablokowana podczas wyświetlania komunikatu „Autoryzowanie...”, wyłącz ustawienie **Blokuj pliki cookie i dane witryn innych firm** lub pozostaw je włączone i utwórz wyjątek dla witryny **login.microsoftonline.com**, a następnie spróbuj ponownie uruchomić kreatora.
   > 
   > 
2. Na stronie **Właściwości**:
   
   1. Wprowadź wartość **CopyFromBlobToAzureSql** w polu **Nazwa zadania**
   2. Wprowadź **opis** (opcjonalnie).
   3. Zmień ustawienia **Data/godzina rozpoczęcia** i **Data/godzina zakończenia** tak, aby data zakończenia przypadała w bieżącym dniu, a data rozpoczęcia — pięć dni przed bieżącym dniem.  
   4. Kliknij przycisk **Dalej**.  
      
      ![Narzędzie kopiowania — strona Właściwości](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png) 
3. Na stronie **Magazyn danych źródłowych** kliknij kafelek **Azure Blob Storage**. Ta strona służy do określania magazynu danych źródłowych do zadania kopiowania. Możesz użyć istniejącej połączonej usługi magazynu danych lub określić nowy magazyn danych. Aby użyć istniejącej połączonej usługi, kliknij pozycję **Z ISTNIEJĄCYCH POŁĄCZONYCH USŁUG** i wybierz odpowiednią połączoną usługę. 
   
    ![Narzędzie kopiowania — strona magazynu danych źródłowych](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
4. Na stronie **Określanie konta usługi Azure Blob Storage**:
   
   1. Wprowadź wartość **AzureStorageLinkedService** w polu **Nazwa połączonej usługi**.
   2. Upewnij się, że wybrano opcję **Z subskrypcji Azure** dla ustawienia **Metoda wyboru konta**.
   3. Wybierz swoją **subskrypcję** platformy Azure.  
   4. Wybierz **konto usługi Azure Storage** z listy kont usługi Azure Storage dostępnych w ramach wybranej subskrypcji. Można również wprowadzić ustawienia konta magazynu ręcznie, wybierając opcję **Wprowadź ręcznie** dla ustawienia **Metoda wyboru konta**, a następnie klikając przycisk **Dalej**. 
      
      ![Narzędzie kopiowania — określanie konta usługi Azure Blob Storage](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
5. Na stronie **Wybieranie pliku lub folderu wejściowego**:
   
   1. Przejdź do folderu **adftutorial**.
   2. Zaznacz plik **emp.txt** i kliknij przycisk **Wybierz**.
   3. Kliknij przycisk **Dalej**. 
      
      ![Narzędzie kopiowania — wybieranie pliku lub folderu wejściowego](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
6. Na stronie **Wybieranie pliku lub folderu wejściowego** kliknij przycisk **Dalej**. Nie wybieraj opcji **Kopia binarna**. 
   
    ![Narzędzie kopiowania — wybieranie pliku lub folderu wejściowego](./media/data-factory-copy-data-wizard-tutorial/chose-input-file-folder.png) 
7. Na stronie **Ustawienia formatu pliku** są wyświetlane ograniczniki i schemat wykrywany automatycznie przez kreatora w ramach analizy pliku. Możesz też ręcznie wprowadzić ograniczniki kreatora kopiowania, aby zatrzymać automatyczne wykrywanie lub przesłonić ustawienia. Po przejrzeniu ograniczników i wyświetleniu podglądu danych kliknij przycisk **Dalej**. 
   
    ![Narzędzie kopiowania — ustawienia formatu pliku](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)  
8. Na stronie docelowego magazynu danych wybierz pozycję **Baza danych SQL Azure** i kliknij przycisk **Dalej**.
   
    ![Narzędzie kopiowania — wybieranie magazynu docelowego](./media/data-factory-copy-data-wizard-tutorial/choose-destination-store.png)
9. Na stronie **Określanie bazy danych SQL Azure**:
   
   1. Wprowadź wartość **AzureSqlLinkedService** w polu **Nazwa połączenia**.
   2. Upewnij się, że wybrano opcję **Z subskrypcji Azure** dla ustawienia **Metoda wyboru serwera/bazy danych**.
   3. Wybierz swoją **subskrypcję** platformy Azure.  
   4. Wybierz opcje **Nazwa serwera** i **Baza danych**.
   5. Wypełnij pola **Nazwa użytkownika** i **Hasło**.
   6. Kliknij przycisk **Dalej**.  
      
      ![Narzędzie kopiowania — określanie bazy danych Azure SQL Database](./media/data-factory-copy-data-wizard-tutorial/specify-azure-sql-database.png)
10. Na stronie **Mapowanie tabeli** wybierz pozycję **emp** z listy rozwijanej w polu **Miejsce docelowe**, kliknij **strzałkę w dół** (opcjonalnie), aby wyświetlić schemat i podgląd danych.
    
     ![Narzędzie kopiowania — mapowanie tabeli](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png) 
11. Na stronie **Mapowanie schematu** kliknij przycisk **Dalej**.
    
    ![Narzędzie kopiowania — mapowanie schematu](./media/data-factory-copy-data-wizard-tutorial/schema-mapping-page.png)
12. Na stronie **Ustawienia wydajności** kliknij przycisk **Dalej**. 
    
    ![Narzędzie kopiowania — ustawienia wydajności](./media/data-factory-copy-data-wizard-tutorial/performance-settings.png)
13. Przejrzyj informacje na stronie **Podsumowanie** i kliknij przycisk **Zakończ**. Kreator utworzy dwie połączone usługi, dwa zestawy danych (wejściowy i wyjściowy) i jeden potok w fabryce danych (z której został uruchomiony Kreator kopiowania). 
    
    ![Narzędzie kopiowania — ustawienia wydajności](./media/data-factory-copy-data-wizard-tutorial/summary-page.png)

## <a name="launch-monitor-and-manage-application"></a>Uruchamianie aplikacji do monitorowania i zarządzania
1. Na stronie **Wdrożenie** kliknij link: `Click here to monitor copy pipeline`.
   
   ![Narzędzie kopiowania — wdrażanie zakończyło się pomyślnie](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)  
2. Instrukcje dotyczące monitorowania utworzonego potoku znajdują się w artykule [Monitor and manage pipeline using Monitoring App](data-factory-monitor-manage-app.md) (Monitorowanie potoków i zarządzanie nimi za pomocą aplikacji do monitorowania). Kliknij ikonę **Odśwież** na liście **OKNA DZIAŁANIA**, aby zobaczyć wycinek. 
   
   ![Aplikacja do monitorowania](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png) 
   
   > [!NOTE]
   > Kliknij przycisk **Odśwież** na liście **OKNA DZIAŁANIA** u dołu, aby wyświetlić najnowszy stan. Nie jest on automatycznie odświeżany. 
   > 
   > 

## <a name="see-also"></a>Zobacz też
| Temat | Opis |
|:--- |:--- |
| [Działania przenoszenia danych](data-factory-data-movement-activities.md) |Ten artykuł zawiera szczegółowe informacje dotyczące działania kopiowania używanego w tym samouczku. |
| [Planowanie i wykonywanie](data-factory-scheduling-and-execution.md) |W tym artykule wyjaśniono aspekty planowania i wykonywania modelu aplikacji usługi Fabryka danych Azure. |
| [Potoki](data-factory-create-pipelines.md) |Ten artykuł ułatwia zapoznanie się z potokami i działaniami w usłudze Azure Data Factory oraz ze sposobem konstruowania za ich pomocą przepływów pracy typu end-to-end opartych na danych na potrzeby scenariusza lub firmy. |
| [Zestawy danych](data-factory-create-datasets.md) |Ten artykuł ułatwia zapoznanie się z zestawami danych w usłudze Azure Data Factory. |
| [Monitorowanie potoków i zarządzanie nimi za pomocą aplikacji do monitorowania](data-factory-monitor-manage-app.md) |Ten artykuł zawiera instrukcje dotyczące monitorowania i debugowania potoków oraz zarządzania nimi przy użyciu aplikacji do monitorowania i zarządzania. |




<!--HONumber=Dec16_HO1-->


