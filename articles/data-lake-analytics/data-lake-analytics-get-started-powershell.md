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
ms.openlocfilehash: 832a859e70e382eb2eeb41560d1b880f7b87de53
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Wprowadzenie do pracy z usługą Azure Data Lake Analytics przy użyciu programu Azure PowerShell
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Dowiedz się, jak za pomocą programu Azure PowerShell tworzyć konta usługi Azure Data Lake Analytics, a następnie przesyłać i uruchamiać zadania U-SQL. Więcej informacji na temat usługi Data Lake Analytics można znaleźć w artykule [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne

Przed przystąpieniem do wykonywania kroków opisanych w tym samouczku musisz mieć poniższe:

* **Konto usługi Azure Data Lake Analytics**. Zobacz [Wprowadzenie do usługi Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal).
* **Stacja robocza z programem Azure PowerShell**. Zobacz artykuł [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

W tym samouczku założono, że użytkownik wie już, jak korzystać z programu Azure PowerShell. W szczególności musi wiedzieć, jak zalogować się do platformy Azure. Jeśli potrzebujesz dodatkowych informacji zobacz temat [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

Aby zalogować się przy użyciu nazwy subskrypcji:

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

Podczas logowania zamiast nazwy subskrypcji można także użyć identyfikatora subskrypcji:

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

Jeśli to się powiedzie, dane wyjściowe tego polecenia mają mniej więcej taką formę:

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

## <a name="preparing-for-the-tutorial"></a>Przygotowanie do samouczka

We fragmentach kodu programu PowerShell w ramach tego samouczka do przechowywania tych informacji są używane następujące zmienne:

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="get-information-about-a-data-lake-analytics-account"></a>Uzyskiwanie informacji o koncie usługi Data Lake Analytics

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a>Przesyłanie zadania U-SQL

Utwórz zmienną środowiska PowerShell do przechowywania skryptu U-SQL.

```
$script = @"
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

"@
```

Prześlij skrypt.

```
$job = Submit-AdlJob -Account $adla -Name "My Job" –Script $script
```

Możesz również zapisać skrypt jako plik i przesłać go za pomocą następującego polecenia:

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -Account $adla -Name "My Job" –ScriptPath $filename
```


Uzyskaj stan określonego zadania. Używaj tego polecenia cmdlet, dopóki zadanie nie zostanie wykonane.

```
$job = Get-AdlJob -Account $adla -JobId $job.JobId
```

Zamiast wielokrotnie wykonywać polecenie Get-AdlAnalyticsJob do momentu zakończenia zadania, możesz użyć polecenia cmdlet Wait-AdlJob.

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

Pobierz plik wyjściowy.

```
Export-AdlStoreItem -Account $adls -Path "/data.csv" -Destination "C:\data.csv"
```

## <a name="see-also"></a>Zobacz też
* Aby wyświetlić ten samouczek przy użyciu innych narzędzi, kliknij odpowiedni selektor karty w górnej części strony.
* Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL w usłudze Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informacje o zadaniach zarządzania znajdziesz w artykule [Zarządzanie usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal](data-lake-analytics-manage-use-portal.md).
