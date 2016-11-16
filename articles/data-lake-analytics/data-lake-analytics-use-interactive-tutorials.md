---
title: "Poznawanie usługi Data Lake Analytics i języka U-SQL przy użyciu interaktywnych samouczków witryny Azure Portal | Microsoft Docs"
description: "Szybki start — poznawanie usługi Data Lake Analytics i języka U-SQL. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 31399d47-e937-4c43-ab0a-6aea8875378d
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/16/2016
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: c2a9bd7f78afc77f72236d0204f39798f0388362


---
# <a name="use-azure-data-lake-analytics-interactive-tutorials"></a>Korzystanie z interaktywnych samouczków usługi Azure Data Lake Analytics
Witryna Azure Portal oferuje interaktywny samouczek ułatwiający rozpoczęcie pracy z usługą Data Lake Analytics. W tym artykule przedstawiono sposób wykonywania kroków samouczka w celu analizowania dzienników witryn sieci Web.

> [!NOTE]
> Jeśli chcesz wykonać kroki tego samouczka przy użyciu programu Visual Studio, zobacz temat [Analizowanie dzienników witryn sieci Web przy użyciu usługi Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
> Planowane jest dodanie do portalu kolejnych interaktywnych samouczków.
> 
> 

Aby znaleźć więcej samouczków, zobacz tematy:

* [Rozpoczynanie pracy z usługą Data Lake Analytics przy użyciu portalu Azure](data-lake-analytics-get-started-portal.md)
* [Rozpoczynanie pracy z usługą Data Lake Analytics przy użyciu programu Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Rozpoczynanie pracy z usługą Data Lake Analytics przy użyciu zestawu SDK programu .NET](data-lake-analytics-get-started-net-sdk.md)
* [Tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) 

**Wymagania wstępne**

Przed przystąpieniem do wykonania kroków opisanych w tym samouczku należy dysponować następującymi elementami:

* **Konto usługi Data Lake Analytics**.  Zobacz temat [Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal](data-lake-analytics-get-started-portal.md).

## <a name="create-data-lake-analytics-account"></a>Tworzenie konta usługi Data Lake Analytics
Aby można było uruchomić jakiekolwiek zadanie, musisz mieć konto usługi Data Lake Analytics.

Każde konto usługi Data Lake Analytics jest w relacji zależności z kontem usługi [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).  To konto jest określane jako domyślne konto usługi Data Lake Store.  Konto usługi Data Lake Store można utworzyć wcześniej lub podczas tworzenia konta usługi Data Lake Analytics. Podczas pracy z tym samouczkiem utworzysz konto usługi Data Lake Store z kontem usługi Analytics.

**Aby utworzyć konto usługi Data Lake Analytics**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute).
2. Kliknij pozycję **Microsoft Azure** w lewym górnym rogu, aby otworzyć tablicę startową.
3. Kliknij kafelek **Marketplace**.  
4. Wpisz ciąg **Azure Data Lake Analytics** w polu wyszukiwania w bloku **Wszystko**, a następnie naciśnij klawisz **ENTER**. Na liście zostanie wyświetlona pozycja **Azure Data Lake Analytics**.
5. Kliknij pozycję **Azure Data Lake Analytics** na liście.
6. Kliknij pozycję **Utwórz** w dolnej części bloku.
7. Wpisz lub wybierz wartości poniższych pozycji:
   
    ![Blok portalu usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)
   
   * **Nazwa**: nazwa konta usługi Analytics.
   * **Data Lake — magazyn**: każde konto usługi Data Lake Analytics ma zależne konto usługi Data Lake Store. Konto usługi Data Lake Analytics i zależne konto usługi Data Lake Store muszą znajdować się w tym samym centrum danych Azure. Postępuj zgodnie z instrukcjami, aby utworzyć nowe konto usługi Data Lake Store lub wybierz konto istniejące.
   * **Subskrypcja**: wybierz subskrypcję platformy Azure używaną na potrzeby konta usługi Analytics.
   * **Grupa zasobów**. Wybierz istniejącą grupę zasobów platformy Azure lub utwórz nową. Aplikacje składają się zazwyczaj z wielu składników, na przykład aplikacji sieci Web, bazy danych, serwera bazy danych, magazynu i usług firm zewnętrznych. Usługa Azure Resource Manager (ARM) umożliwia pracę z zasobami w aplikacji w postaci grupy określanej jako grupa zasobów platformy Azure. Można wdrożyć, zaktualizować, monitorować lub usunąć wszystkie zasoby aplikacji w ramach jednej, skoordynowanej operacji. Wdrażanie wykonuje się przy użyciu szablonu, którego można następnie używać w różnych środowiskach (testowanie, etap przejściowy i produkcja). Można również uprościć rozliczenia w organizacji, wyświetlając zestawienia kosztów całej grupy. Aby uzyskać więcej informacji, zobacz temat [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). 
   * **Lokalizacja**. Wybierz centrum danych platformy Azure na potrzeby konta usługi Data Lake Analytics. 
8. Wybierz pozycję **Przypnij do tablicy startowej**. Jest to wymagane w celu wykonania kroków tego samouczka.
9. Kliknij przycisk **Utwórz**. Spowoduje to przejście do tablicy startowej portalu. Do strony głównej zostanie dodany nowy kafelek z etykietą „Deploying Azure Data Lake Analytics” (Wdrażanie usługi Azure Data Lake Analytics). Utworzenie konta usługi Data Lake Analytics może potrwać kilka minut. Utworzone konto jest wyświetlane w nowym bloku otwartego portalu.
   
    ![Blok portalu usługi Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

## <a name="run-website-log-analysis-interactive-tutorial"></a>Uruchamianie interaktywnego samouczka analizy dzienników witryn sieci Web
**Aby otworzyć interaktywny samouczek analizy dzienników witryn sieci Web**

1. W portalu w menu znajdującym się po lewej stronie kliknij pozycję **Microsoft Azure**, aby otworzyć tablicę startową.
2. Kliknij kafelek połączony z kontem usługi Data Lake Analytics.
3. Kliknij pozycję **Explore interactive tutorials** (Eksploruj interaktywne samouczki) na pasku **Essentials** (Podstawowe elementy).
   
    ![Interaktywne samouczki usługi Data Lake Analytics](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)
4. Jeśli zostanie wyświetlone pomarańczowe ostrzeżenie „Samples not set up, click ...” (Nie skonfigurowano przykładów, kliknij...), kliknij pozycję **Copy Sample Data** (Kopiuj przykładowe dane), aby skopiować przykładowe dane do domyślnego konta Data Lake Store. Do uruchomienia interaktywnego samouczka potrzebne są dane.
5. W bloku **Interactive Tutorials** (Interaktywne samouczki) kliknij pozycję **Website Log Analytics** (Analiza dziennika witryny sieci Web). Samouczek zostanie otwarty w nowym bloku portalu.
6. Kliknij pozycję **1 Introduction** (1 Wprowadzenie) i postępuj zgodnie z instrukcjami.

## <a name="see-also"></a>Zobacz też
* [Omówienie usługi Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Rozpoczynanie pracy z usługą Data Lake Analytics przy użyciu portalu Azure](data-lake-analytics-get-started-portal.md)
* [Rozpoczynanie pracy z usługą Data Lake Analytics przy użyciu programu Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Analizowanie dzienników witryn sieci Web przy użyciu usługi Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md)




<!--HONumber=Nov16_HO2-->


