---
title: "Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal | Microsoft Docs"
description: "Poznaj sposoby tworzenia konta usługi Data Lake Analytics przy użyciu witryny Azure Portal, tworzenia zadania usługi Data Lake Analytics przy użyciu języka U-SQL oraz przesyłania zadania. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 7f23ae904461e754b3871b33ca40c2ff4fcb99f0
ms.lasthandoff: 03/21/2017


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-portal"></a>Samouczek: rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Poznaj sposoby tworzenia kont usługi Azure Data Lake Analytics przy użyciu witryny Azure Portal, definiowania zadań w języku [U-SQL](data-lake-analytics-u-sql-get-started.md) oraz przesyłania zadań do usługi Data Lake Analytics. Więcej informacji na temat usługi Data Lake Analytics można znaleźć w artykule [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md).

W ramach tego samouczka utworzysz zadanie, które odczytuje zawartość pliku z wartościami rozdzielanymi tabulatorami (TSV) i konwertuje je do pliku z wartościami rozdzielanymi przecinkami (CSV). Aby wykonać kroki opisane w tym samouczku, korzystając z innych obsługiwanych narzędzi, kliknij odpowiednią kartę w górnej części tej sekcji. Po pomyślnym utworzeniu pierwszego zadania możesz zacząć tworzyć bardziej złożone przekształcenia danych w języku U-SQL.

## <a name="prerequisites"></a>Wymagania wstępne
Przed przystąpieniem do wykonywania kroków opisanych w tym samouczku musisz mieć poniższe:

* **Subskrypcja platformy Azure**. Zobacz artykuł [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-data-lake-analytics-account"></a>Tworzenie konta usługi Data Lake Analytics
Aby można było uruchomić jakiekolwiek zadanie, musisz mieć konto usługi Data Lake Analytics.

Każde konto usługi Data Lake Analytics jest w relacji zależności z kontem usługi Azure Data Lake Store.  To konto jest określane jako domyślne konto usługi Data Lake Store.  Konto usługi Data Lake Store można utworzyć wcześniej lub podczas tworzenia konta usługi Data Lake Analytics. Podczas pracy z tym samouczkiem utworzysz konto usługi Data Lake Store z kontem usługi Data Lake Analytics.

**Tworzenie konta usługi Data Lake Analytics**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij przycisk **Nowy**, kliknij pozycję **Zbieranie danych i analiza**, a następnie kliknij pozycję **Data Lake Analytics**.
3. Wpisz lub wybierz poniższe wartości:

    ![Blok portalu usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

   * **Nazwa**: nazwa konta usługi Data Lake Analytics (dozwolone są tylko małe litery i cyfry).
   * **Subskrypcja**: wybierz subskrypcję platformy Azure używaną na potrzeby konta usługi Analytics.
   * **Grupa zasobów**. Wybierz istniejącą grupę zasobów platformy Azure lub utwórz nową. Usługa Azure Resource Manager umożliwia pracę z zasobami w aplikacji jako grupą. Aby uzyskać więcej informacji, zobacz temat [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
   * **Lokalizacja**. Wybierz centrum danych platformy Azure na potrzeby konta usługi Data Lake Analytics.
   * **Data Lake Store**: kliknij przycisk *Skonfiguruj wymagane ustawienia*. Postępuj zgodnie z instrukcjami, aby utworzyć nowe konto usługi Data Lake Store lub wybierz konto istniejące. Każde konto usługi Data Lake Analytics ma zależne konto usługi Data Lake Store. Konto usługi Data Lake Analytics i zależne konto usługi Data Lake Store muszą znajdować się w tym samym centrum danych Azure.
4. Wybieranie warstwy cenowej  
5. Kliknij przycisk **Utwórz**. Ponownie zostanie wyświetlony ekran główny portalu, na którym pojawi się nowy kafelek z etykietą „Deploying Azure Data Lake Analytics” (Wdrażanie usługi Azure Data Lake Analytics). Tworzenie konta usługi Data Lake Analytics potrwa kilka minut. Utworzone konto jest wyświetlane w nowym bloku otwartego portalu.

Po utworzeniu konta usługi Data Lake Analytics można dodać dodatkowe konta usługi Data Lake Store i konta usługi Azure Storage. Aby uzyskać instrukcje, zobacz temat [Zarządzanie źródłami danych konta usługi Data Lake Analytics](data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

## <a name="prepare-source-data"></a>Przygotowanie danych źródłowych
W ramach tego samouczka przetworzymy dzienniki wyszukiwania.  Dziennik wyszukiwania może być przechowywany w usłudze Data Lake Store lub w usłudze Azure Blob Storage.

Witryna Azure Portal udostępnia interfejs użytkownika umożliwiający skopiowanie przykładowych plików danych na domyślne konto usługi Data Lake Store. Pliki te obejmują również dziennik wyszukiwania.

**Kopiowanie przykładowych plików danych**

1. Z witryny [Azure Portal](https://portal.azure.com) otwórz swoje konto usługi Data Lake Analytics.  Zobacz [Zarządzanie kontami usługi Data Lake Analytics](data-lake-analytics-get-started-portal.md#create-data-lake-analytics-account), aby utworzyć i otworzyć konto w portalu.
2. Rozwiń węzeł **Podstawowe elementy**, a następnie kliknij pozycję **Eksploruj przykładowe skrypty**. Spowoduje to otwarcie kolejnego bloku o nazwie **Przykładowe skrypty**.

    ![Przykładowy skrypt portalu usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-sample-scripts.png)
3. Kliknij pozycję **Brak danych przykładowych**, aby skopiować przykładowe pliki danych. Po zakończeniu w portalu zostanie wyświetlony komunikat **Dane przykładowe zostały pomyślnie zaktualizowane**.
4. W bloku konta usługi Data Lake Analytics kliknij przycisk **Eksplorator danych** w górnej części.

    ![Przycisk eksploratora danych usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-data-explorer-button.png)

    Zostaną otwarte dwa bloki. Jeden z nich to blok **Eksplorator danych**, a drugi to blok domyślnego konta usługi Data Lake Store.
5. W bloku domyślnego konta usługi Data Lake Store kliknij pozycję **Przykłady**, aby rozwinąć folder, a następnie kliknij pozycję **Dane**, aby rozwinąć folder. Powinny zostać wyświetlone następujące pliki i foldery.

   * AmbulanceData/
   * AdsLog.tsv
   * SearchLog.tsv
   * version.txt
   * WebLog.log

     Na potrzeby tego samouczka zostanie użyty plik SearchLog.tsv.

W praktyce należy zaprogramować aplikacje do zapisywania danych na połączonych kontach magazynu lub przekazać dane. Aby uzyskać informacje na temat przekazywania plików, zobacz temat [Przekazywanie danych do usługi Data Lake Store](data-lake-analytics-manage-use-portal.md#upload-data-to-adls) lub [Przekazywanie danych do usługi Blob Storage](data-lake-analytics-manage-use-portal.md#upload-data-to-wasb).

## <a name="create-and-submit-data-lake-analytics-jobs"></a>Tworzenie i przesyłanie zadań usługi Data Lake Analytics
Po przygotowaniu danych źródłowych możesz rozpocząć tworzenie skryptu U-SQL.  

**Aby przesłać zadanie**

1. W bloku konta usługi Data Lake Analytics w portalu kliknij pozycję **Nowe zadanie**.

    ![Przycisk nowego zadania usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job-button.png)

    Jeśli blok nie jest widoczny, zobacz temat [Otwieranie konta usługi Data Lake Analytics w portalu](data-lake-analytics-manage-use-portal.md#access-adla-account).
2. Wprowadź nazwę w polu **Nazwa zadania** oraz poniższy skrypt U-SQL:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();

        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    ![Tworzenie zadań U-SQL w usłudze Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job.png)

    Ten skrypt U-SQL odczytuje źródłowy plik danych przy użyciu ekstraktora **Extractors.Tsv()**, a następnie tworzy plik csv przy użyciu ekstraktora **Outputters.Csv()**.

    Nie należy modyfikować tych dwóch ścieżek, jeśli plik źródłowy nie został skopiowany do innej lokalizacji.  Jeśli folder wyjściowy nie istnieje, usługa Data Lake Analytics go utworzy.  W tym przypadku użyto prostych ścieżek względnych.  

    Użycie ścieżek względnych jest łatwiejsze w przypadku plików przechowywanych na domyślnych kontach usługi Data Lake. Można także użyć ścieżek bezwzględnych.  Na przykład:

        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL w usłudze Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md) i [Dokumentację języka U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

1. Kliknij przycisk **Prześlij zadanie** u górnej części.   
2. Poczekaj, aż stan zadania zmieni się na **Zakończono pomyślnie**. Zauważ, że wykonanie zadania trwało około jednej minuty.

    Jeśli zadanie zakończyło się niepowodzeniem, zobacz temat [Monitorowanie zadań usługi Data Lake Analytics i rozwiązywanie problemów](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).
3. W dolnej części bloku kliknij kartę **Dane wyjściowe**, a następnie kliknij pozycję **SearchLog-from-Data-Lake.csv**. Można wyświetlić podgląd pliku wyjściowego, pobrać go, zmienić jego nazwę i go usunąć.

    ![Właściwości pliku wyjściowego zadania usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-output-file-properties.png)

## <a name="see-also"></a>Zobacz też
* Aby uzyskać informacje na temat bardziej złożonego zapytania, zobacz temat [Analizowanie dzienników witryn sieci Web przy użyciu usługi Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Aby rozpocząć tworzenie aplikacji w języku U-SQL, zobacz artykuł [Tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL w usłudze Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informacje o zadaniach zarządzania znajdziesz w artykule [Zarządzanie usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal](data-lake-analytics-manage-use-portal.md).
* Aby zapoznać się z omówieniem usługi Data Lake Analytics, zobacz [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md).
* Aby wyświetlić ten samouczek przy użyciu innych narzędzi, kliknij odpowiedni selektor karty w górnej części strony.
* Aby rejestrować informacje diagnostyczne, zobacz [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md) (Dostęp do dzienników diagnostycznych usługi Azure Data Lake Analytics)

