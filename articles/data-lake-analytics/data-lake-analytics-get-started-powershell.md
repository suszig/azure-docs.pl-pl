---
title: "Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu programu Azure PowerShell | Microsoft Docs"
description: "Utwórz konto usługi Data Lake Analytics przy użyciu programu Azure PowerShell oraz utwórz zadanie usługi Data Lake Analytics przy użyciu języka U-SQL i prześlij je. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 8a4e901e-9656-4a60-90d0-d78ff2f00656
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 6985dff332928d704f30e167c3bddb62bcc6cac1
ms.contentlocale: pl-pl
ms.lasthandoff: 05/09/2017


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Samouczek: wprowadzenie do pracy z usługą Azure Data Lake Analytics przy użyciu programu Azure PowerShell
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Dowiedz się, jak za pomocą programu Azure PowerShell tworzyć konta usługi Azure Data Lake Analytics, a następnie przesyłać i uruchamiać zadania U-SQL. Więcej informacji na temat usługi Data Lake Analytics można znaleźć w artykule [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne
Przed przystąpieniem do wykonywania kroków opisanych w tym samouczku musisz mieć poniższe:

* **Subskrypcja platformy Azure**. Zobacz artykuł [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Stacja robocza z programem Azure PowerShell**. Zobacz artykuł [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).

## <a name="preparing-for-the-tutorial"></a>Przygotowanie do samouczka
Aby utworzyć konto usługi Data Lake Analytics, najpierw musisz zdefiniować następujące elementy:

* **Grupa zasobów Azure**: konto usługi Data Lake Analytics należy utworzyć w grupie zasobów Azure.
* **Nazwa konta usługi Data Lake Analytics**: może zawierać tylko małe litery i cyfry.
* **Lokalizacja**: centrum danych Azure, które obsługuje usługę Data Lake Analytics.
* **Domyślne konto usługi Data Lake Store**: każde konto usługi Data Lake Analytics ma domyślne konto usługi Data Lake Store. Te konta muszą znajdować się w tej samej lokalizacji.

We fragmentach kodu programu PowerShell w ramach tego samouczka do przechowywania tych informacji są używane następujące zmienne

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="create-a-data-lake-analytics-account"></a>Tworzenie konta Data Lake Analytics

Jeśli nie masz jeszcze grupy zasobów, której możesz użyć, utwórz ją. 

```
New-AzureRmResourceGroup -Name  $rg -Location $location
```

Każde konto usługi Data Lake Analytics wymaga domyślnego konta usługi Data Lake Store używanego do przechowywania dzienników. Możesz użyć istniejącego konta lub utworzyć nowe. 

```
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

Gdy grupa zasobów i konto magazynu usługi Data Lake Store będą dostępne, utwórz konto usługi Data Lake Analytics.

```
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

## <a name="get-information-about-a-data-lake-analytics-account"></a>Uzyskiwanie informacji o koncie usługi Data Lake Analytics

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a>Przesyłanie zadania U-SQL

Utwórz plik tekstowy z następującym skryptem U-SQL.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

Prześlij skrypt.

```
Submit-AdlJob -AccountName $adla –ScriptPath "d:\test.usql"Submit
```

## <a name="monitor-u-sql-jobs"></a>Monitorowanie zadań U-SQL

Wyświetl listę wszystkich zadań w ramach konta. Dane wyjściowe obejmują obecnie uruchomione oraz niedawno ukończone zadania.

```
Get-AdlJob -Account $adla
```

Uzyskaj stan określonego zadania.

```
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

Zamiast wielokrotnie wykonywać polecenie Get-AdlAnalyticsJob do momentu zakończenia zadania, możesz użyć polecenia cmdlet Wait-AdlJob.

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

Po zakończeniu zadania sprawdź, czy plik wyjściowy istnieje, wyświetlając listę plików w folderze.

```
Get-AdlStoreChildItem -Account $adls -Path "/"
```

Sprawdź, czy plik istnieje.

```
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

## <a name="uploading-and-downloading-files"></a>Przekazywanie i pobieranie plików

Pobierz dane wyjściowe skryptu U-SQL.

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv"  -Destination "D:\data.csv"
```


Przekaż plik, który ma być używany jako dane wejściowe skryptu U-SQL.

```
Import-AdlStoreItem -AccountName $adls -Path "D:\data.tsv" -Destination "/data_copy.csv" 
```

## <a name="see-also"></a>Zobacz też
* Aby wyświetlić ten samouczek przy użyciu innych narzędzi, kliknij odpowiedni selektor karty w górnej części strony.
* Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL w usłudze Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informacje o zadaniach zarządzania znajdziesz w artykule [Zarządzanie usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal](data-lake-analytics-manage-use-portal.md).

