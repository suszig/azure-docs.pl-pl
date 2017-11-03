---
title: "Zarządzanie Azure Data Lake Analytics przy użyciu interfejsu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie zarządzania kont usługi Data Lake Analytics, źródła danych, zadaniami i użytkownikami przy użyciu wiersza polecenia platformy Azure"
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: f90bada3572c0ed40b07d76ec02c1b499bbd1428
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Zarządzanie Azure Data Lake Analytics przy użyciu interfejsu wiersza polecenia platformy Azure (CLI)
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Dowiedz się, jak zarządzać kontami usługi Azure Data Lake Analytics, źródeł danych, użytkowników i zadań przy użyciu wiersza polecenia platformy Azure. Aby wyświetlić tematy dotyczące zarządzania przy użyciu innych narzędzi, kliknij łącze powyżej.


**Wymagania wstępne**

Przed przystąpieniem do wykonania kroków opisanych w tym samouczku należy dysponować następującymi elementami:

* **Subskrypcja platformy Azure**. Zobacz artykuł [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Interfejs wiersza polecenia platformy Azure**. Zobacz temat [Instalowanie i konfigurowanie interfejsu wiersza polecenia platformy Azure](../cli-install-nodejs.md).
  * Aby ukończyć ten przewodnik, pobierz i zainstaluj **wersję wstępną** [narzędzi interfejsu wiersza polecenia platformy Azure](https://github.com/MicrosoftBigData/AzureDataLake/releases).
* **Uwierzytelnianie** przy użyciu następującego polecenia:
  
        azure login
    Więcej informacji na temat uwierzytelniania za pomocą konta służbowego lub szkolnego znajdziesz w temacie [Połączenie z subskrypcją platformy Azure z poziomu interfejsu wiersza polecenia platformy Azure](../xplat-cli-connect.md).
* **Przełączenie w tryb usługi Azure Resource Manager** przy użyciu następującego polecenia:
  
        azure config mode arm

**Aby wyświetlić listę poleceń usługi Data Lake Store i usługi Data Lake Analytics:**

    azure datalake store
    azure datalake analytics

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-accounts"></a>Zarządzanie kontami
Przed uruchomieniem zadania usługi Data Lake Analytics, musisz mieć konto usługi Data Lake Analytics. W przeciwieństwie do usługi Azure HDInsight nie płatności dla konta usługi Analytics zadania nie jest uruchomiona.  Płacisz tylko za czas, gdy jest uruchomione zadanie.  Aby uzyskać więcej informacji, zobacz [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md).  

### <a name="create-accounts"></a>Tworzenie kont
      azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"


### <a name="update-accounts"></a>Aktualizowanie kont
Polecenie aktualizuje właściwości istniejącego konta Data Lake Analytics

    azure datalake analytics account set "<Data Lake Analytics Account Name>"


### <a name="list-accounts"></a>Wyświetlanie listy kont
Lista Data Lake Analytics kont 

    azure datalake analytics account list

Lista Data Lake Analytics kont w ramach określonej grupy zasobów

    azure datalake analytics account list -g "<Azure Resource Group Name>"

Uzyskiwanie szczegółowych informacji z określonego konta usługi Data Lake Analytics

    azure datalake analytics account show -g "<Azure Resource Group Name>" -n "<Data Lake Analytics Account Name>"

### <a name="delete-data-lake-analytics-accounts"></a>Usuwanie konta usługi Data Lake Analytics
      azure datalake analytics account delete "<Data Lake Analytics Account Name>"


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Zarządzaj źródłami danych konta
Data Lake Analytics obsługuje obecnie następujących źródeł danych:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage](../storage/common/storage-introduction.md)

Podczas tworzenia konta usługi Analytics należy wyznaczyć konta usługi Azure Data Lake Storage jako domyślne konto magazynu. Domyślne konto magazynu ADL służy do przechowywania metadanych i zadanie inspekcji z dziennikami zadań. Po utworzeniu konta usługi Analytics można dodać dodatkowe konta usługi Data Lake Storage i/lub konta usługi Magazyn Azure. 

### <a name="find-the-default-adl-storage-account"></a>Znajdź ADL domyślne konto magazynu
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

Wartość jest wyświetlana w obszarze właściwości: datalakeStoreAccount:name.

### <a name="add-additional-azure-blob-storage-accounts"></a>Dodawanie dodatkowych kont magazynu obiektów Blob platformy Azure
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -b "<Azure Blob Storage Account Short Name>" -k "<Azure Storage Account Key>"

> [!NOTE]
> Są obsługiwane tylko obiektu Blob magazynu krótkiej nazwy.  Nie używaj nazwy FQDN, na przykład "myblob.blob.core.windows.net".
> 
> 

### <a name="add-additional-data-lake-store-accounts"></a>Dodawanie dodatkowych kont usługi Data Lake Store
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -l "<Data Lake Store Account Name>" [-d]

[-d] jest opcjonalnym przełącznikiem, aby wskazać, czy Data Lake dodawany jest domyślne konto usługi Data Lake. 

### <a name="update-existing-data-source"></a>Aktualizowanie istniejącego źródła danych
Aby ustawić istniejącego konta usługi Data Lake Store domyślne:

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -l "<Azure Data Lake Store Account Name>" -d

Aby zaktualizować istniejący klucz konta magazynu obiektów Blob:

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -b "<Blob Storage Account Name>" -k "<New Blob Storage Account Key>"

### <a name="list-data-sources"></a>Lista źródeł danych:
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

![Data Lake Analytics listy źródła danych](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a>Usuń źródła danych:
Aby usunąć konto usługi Data Lake Store:

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Azure Data Lake Store Account Name>"

Aby usunąć konto magazynu obiektów Blob:

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Blob Storage Account Name>"

## <a name="manage-jobs"></a>Zarządzanie zadaniami
Aby można było utworzyć zadanie, musisz mieć konto usługi Data Lake Analytics.  Aby uzyskać więcej informacji, zobacz [kont zarządzania usługi Data Lake Analytics](#manage-accounts).

### <a name="list-jobs"></a>Lista zadań
      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"

![Data Lake Analytics listy źródła danych](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a>Uzyskiwanie szczegółów zadania
      azure datalake analytics job show -n "<Data Lake Analytics Account Name>" -j "<Job ID>"

### <a name="submit-jobs"></a>Przesyłanie zadań
> [!NOTE]
> Domyślnie priorytet zadania wynosi 1000, a domyślny stopień równoległości zadania jest 1.
> 
> 

    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"

### <a name="cancel-jobs"></a>Anulowanie zadań
Aby znaleźć identyfikator zadania, a następnie użyć Anuluj, aby anulować zadanie, należy użyć polecenia listy.

      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"
      azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job ID>"

## <a name="manage-catalog"></a>Zarządzanie katalogiem
Katalogu U-SQL jest używana do struktury danych i kod, aby mogły być udostępniane przez skrypty U-SQL. Katalog umożliwia najwyższym możliwym poziomie wydajności z danymi w usłudze Azure Data Lake. Więcej informacji znajduje się w temacie [Używanie katalogu U-SQL](data-lake-analytics-use-u-sql-catalog.md).

### <a name="list-catalog-items"></a>Lista elementów katalogu
    #List databases
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t database

    #List tables
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t table

Typy obejmują bazy danych, schematów, zestawu, zewnętrzne źródło danych, tabeli, funkcji zwracającej tabelę lub statystyk tabeli.

<!-- ################################ -->
<!-- ################################ -->
## <a name="use-arm-groups"></a>Użyj grup ARM
Aplikacje składają się zazwyczaj z wielu składników, na przykład aplikacji sieci Web, bazy danych, serwera bazy danych, magazynu i usług firm zewnętrznych. Usługa Azure Resource Manager (ARM) umożliwia pracę z zasobami w aplikacji w postaci grupy określanej jako grupa zasobów platformy Azure. Można wdrożyć, zaktualizować, monitorować lub usunąć wszystkie zasoby aplikacji w ramach jednej, skoordynowanej operacji. Wdrażanie wykonuje się przy użyciu szablonu, którego można następnie używać w różnych środowiskach (testowanie, etap przejściowy i produkcja). Można również uprościć rozliczenia w organizacji, wyświetlając zestawienia kosztów całej grupy. Aby uzyskać więcej informacji, zobacz temat [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). 

Usługa Data Lake Analytics może obejmować następujące składniki:

* Usługa Azure Data Lake Analytics konta
* Wymagane domyślne konto usługi Azure Data Lake Storage
* Dodatkowe usługi Azure Data Lake kont magazynu
* Dodatkowe konta usługi Azure Storage

Możesz utworzyć te składniki w ramach jednej grupy ARM, aby ułatwić zarządzanie.

![Usługa Azure Data Lake Analytics konta i magazynu](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Konto usługi Data Lake Analytics i kont magazynu zależnego muszą znajdować się w tym samym centrum danych Azure.
Jednak grupy ARM może znajdować się w centrum danych.  

## <a name="see-also"></a>Zobacz też
* [Omówienie usługi Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Rozpoczynanie pracy z usługą Data Lake Analytics przy użyciu portalu Azure](data-lake-analytics-get-started-portal.md)
* [Zarządzanie usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal](data-lake-analytics-manage-use-portal.md)
* [Monitorowanie zadań usługi Azure Data Lake Analytics i rozwiązywanie problemów przy użyciu witryny Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

