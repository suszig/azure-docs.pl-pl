---
title: "Ładowanie danych do usługi Azure Data Lake Store przy użyciu fabryki danych Azure | Dokumentacja firmy Microsoft"
description: "Kopiowanie danych do usługi Azure Data Lake Store za pomocą fabryki danych Azure"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: a684f8bcdefc8e3338fae59e56863dfc37c23f1f
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2018
---
# <a name="load-data-into-azure-data-lake-store-using-azure-data-factory"></a>Ładowanie danych do usługi Azure Data Lake Store przy użyciu fabryki danych Azure

[Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) jest całego przedsiębiorstwa hiperskali repozytorium na potrzeby obciążeń analizy dużych danych big data. Usługa Azure Data Lake umożliwia przechwytywanie danych dowolnego typu, o dowolnym rozmiarze i szybkości wprowadzania oraz przechowywanie ich w jednym miejscu na potrzeby analiz operacyjnych i poznawczych.

Fabryka danych Azure to usługa integracji pełni zarządzanych danych oparte na chmurze, używany do wypełniania lake przy użyciu danych z istniejącego systemu i zapisywanie cenny czas podczas tworzenia rozwiązań analizy. Poniżej przedstawiono najważniejsze zalety ładowania danych do usługi Azure Data Lake Store przy użyciu fabryki danych Azure:

* **Można skonfigurować**: Kreator intuicyjne krok 5 z bez skryptu wymagane.
* **Obsługa magazynu danych sformatowanego**: wbudowaną obsługę bogaty zestaw lokalnymi i magazyny danych oparte na chmurze, zobacz szczegółową listę w [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats) tabeli.
* **Bezpieczne i zgodne**: dane są przesyłane za pośrednictwem protokołu HTTPS lub ExpressRoute i zapewnia obecności usługi globalne dane nigdy nie przekracza granic geograficznych
* **Wysoka wydajność**: maksymalnie 1 GB/s danych ładowania szybkości do usługi Azure Data Lake Store. Dowiedz się więcej szczegółów z [wydajności działania kopiowania](copy-activity-performance.md).

W tym artykule przedstawiono sposób użycia narzędzia kopii fabryki danych do **ładowanie danych z usługi Amazon S3 do usługi Azure Data Lake Store**. Można wykonać podobne kroki można skopiować danych z innych typów magazynów danych.

> [!NOTE]
>  Aby uzyskać ogólne informacje o możliwościach fabryki danych podczas kopiowania danych do/z usługi Azure Data Lake Store, zobacz [kopiowanie danych do i z usługi Azure Data Lake Store przy użyciu fabryki danych Azure](connector-azure-data-lake-store.md) artykułu.
>
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [działanie kopiowania w wersji 1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Wymagania wstępne

* **Subskrypcja platformy Azure**. Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.
* **Azure Data Lake Store**. Jeśli nie masz konta usługi Data Lake Store, zobacz [Utwórz konto usługi Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account) artykułu kroki go utworzyć.
* **Amazon S3**. W tym artykule pokazano, jak skopiować dane z usługi Amazon S3. Wykonując kroki podobne, można użyć innych magazynów danych.

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych

1. Kliknij przycisk **NOWY** w lewym menu, kliknij pozycję **Dane + analiza**, a następnie kliknij przycisk **Data Factory**. 
   
   ![Nowy-> Fabryka danych](./media/load-data-into-azure-data-lake-store/new-azure-data-factory-menu.png)
2. W **nowa fabryka danych** Podaj wartości, jak pokazano na poniższym zrzucie ekranu: 
      
     ![Nowa strona fabryki danych](./media/load-data-into-azure-data-lake-store//new-azure-data-factory.png)
 
   * **Nazwa.** Wprowadź unikatową nazwę globalne dla fabryki danych. Jeśli wystąpi poniższy błąd, zmień nazwę fabryki danych (np. twojanazwaADFTutorialDataFactory) i spróbuj utworzyć ją ponownie. Zobacz [fabryki danych - reguły nazewnictwa](naming-rules.md) artykułu dla reguł nazewnictwa artefakty fabryki danych.
  
            `Data factory name "LoadADLSDemo" is not available`

    * **Subskrypcja.** Wybierz **subskrypcję** Azure, w której chcesz utworzyć fabrykę danych. 
    * **Grupy zasobów.** Wybierz istniejącą grupę zasobów z listy rozwijanej lub wybierz **Utwórz nowy** opcję i wprowadź nazwę grupy zasobów. Informacje na temat grup zasobów znajdują się w artykule [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Używanie grup zasobów do zarządzania zasobami platformy Azure).  
    * **Wersja.** Wybierz **V2 (wersja zapoznawcza)**.
    * **Lokalizacja.** Wybierz lokalizację dla fabryki danych. Tylko obsługiwane lokalizacje są wyświetlane na liście rozwijanej. Magazyny danych (Azure Data Lake Store, usługi Azure Storage, baza danych SQL Azure itp.) używany przez fabryki danych może być w innych lokalizacjach lub regionach.

3. Kliknij przycisk **Utwórz**.
4. Po utworzeniu pełnej, przejdź do pozycji z fabryką danych i zostanie wyświetlony **fabryki danych** strony, jak pokazano w obrazie. Kliknij przycisk **autora & Monitor** Kafelek, aby uruchomić aplikację integracji danych w osobnej karcie. 
   
   ![Strona główna fabryki danych](./media/load-data-into-azure-data-lake-store/data-factory-home-page.png)

## <a name="load-data-into-azure-data-lake-store"></a>Ładowanie danych do usługi Azure Data Lake Store

1. W strony wprowadzenie, kliknij przycisk **kopiowania danych** Kafelek, aby uruchomić narzędzie Kopia danych. 

   ![Kopiowanie danych narzędzie kafelka](./media/load-data-into-azure-data-lake-store/copy-data-tool-tile.png)
2. W **właściwości** strony narzędzia kopii danych, określ **CopyFromAmazonS3ToADLS** dla **Nazwa zadania**i kliknij przycisk **dalej**. 

    ![Kopiowanie danych strony właściwości narzędzia](./media/load-data-into-azure-data-lake-store/copy-data-tool-properties-page.png)
3. W **magazynu danych źródła** wybierz pozycję **Amazon S3**i kliknij przycisk **dalej**.

    ![Strona magazynu danych źródła](./media/load-data-into-azure-data-lake-store/source-data-store-page.png)
4. W **połączenia Określ Amazon S3** wykonaj następujące czynności: 
    1. Określ **uzyskać dostęp do Identyfikatora klucza**.
    2. Określ **klucza tajnego dostępu**.
    3. Kliknij przycisk **Dalej**. 

        ![Określ konto Amazon S3](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account.png)
5. W **wybierz wejściowy plik lub folder** , przejdź do folderu pliku chcesz kopiować, wybierz i kliknij przycisk **wybierz**, następnie kliknij przycisk **dalej**. 

    ![Wybierz wejściowy plik lub folder](./media/load-data-into-azure-data-lake-store/choose-input-folder.png)

6. W **magazyn danych docelowy** wybierz pozycję **Azure Data Lake Store**i kliknij przycisk **dalej**. 

    ![Docelowa strona magazynu danych](./media/load-data-into-azure-data-lake-store/destination-data-storage-page.png)

7. Na tej stronie wybierz zachowanie kopii, sprawdzając **kopiowania plików rekursywnie** i **binarne kopiowania** (kopiowanie plików jako — jest) opcje. Kliknij przycisk **Dalej**.

    ![Określ folder wyjściowy](./media/load-data-into-azure-data-lake-store/specify-binary-copy.png)

8. W **połączenia Określ Data Lake Store** wykonaj następujące czynności: 

    1. Wybierz usługi Data Lake Store dla **nazwa konta usługi Data Lake Store**.
    2. Określ usługę podmiotu zabezpieczeń informacje w tym **dzierżawy**, **usługi identyfikator podmiotu zabezpieczeń**, i **klucz główny usługi**.
    3. Kliknij przycisk **Dalej**. 

    > [!IMPORTANT]
    > W tym przewodniku, należy użyć **nazwy głównej usługi** do uwierzytelniania usługi data lake store. Postępuj zgodnie z instrukcjami [tutaj](connector-azure-data-lake-store.md#using-service-principal-authentication) i upewnij się, że można przydzielić usługi głównej odpowiednie uprawnienia w usłudze Azure Data Lake Store.

    ![Określ konto usługi Azure Data Lake Store](./media/load-data-into-azure-data-lake-store/specify-adls.png)

9. W **wybierz dane wyjściowe pliku lub folderu** określ **copyfroms3**, następnie kliknij przycisk **dalej**. 

    ![Określ folder wyjściowy](./media/load-data-into-azure-data-lake-store/specify-adls-path.png)


10. W **ustawienia** kliknij przycisk **dalej**. 

    ![Ustawienia strony](./media/load-data-into-azure-data-lake-store/copy-settings.png)
11. W **Podsumowanie** strony, przejrzyj ustawienia i kliknij przycisk **dalej**.

    ![Strona podsumowania](./media/load-data-into-azure-data-lake-store/copy-summary.png)
12. W **stronę wdrożenia**, kliknij przycisk **monitora** do monitorowania procesu (zadania).

    ![Strona wdrożenia](./media/load-data-into-azure-data-lake-store/deployment-page.png)
13. Zwróć uwagę, że **Monitor** wybierana jest karta po lewej stronie. Zobacz łącza, aby wyświetlić szczegóły uruchomienia działania i ponowne uruchomienie procesu w **akcje** kolumny. 

    ![Uruchamia Monitor potoku](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)
14. Aby wyświetlić skojarzone z potoku Uruchom uruchomień działania, kliknij przycisk **odbywa się działanie widoku** łącze w **akcje** kolumny. Potoku, jest tylko jedno działanie (działanie kopiowania), tak aby widoczne tylko jeden wpis. Aby powrócić do potoku uruchamia widok, kliknij przycisk **potoki** łącze u góry. Kliknij przycisk **Odśwież** Aby odświeżyć listę. 

    ![Monitor uruchomień działania](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

15. Upewnij się, że dane są kopiowane do usługi Azure Data Lake Store. 

    ![Sprawdź dane wyjściowe usługi Data Lake Store](./media/load-data-into-azure-data-lake-store/adls-copy-result.png)

## <a name="next-steps"></a>Kolejne kroki

Przejdź do następujących artykuł, aby dowiedzieć się więcej na temat obsługi usługi Azure Data Lake Store: 

> [!div class="nextstepaction"]
>[Łącznik usługi Azure Data Lake Store](connector-azure-data-lake-store.md)