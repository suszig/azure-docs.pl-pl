---
title: "Dane transformacji skrypt programu PowerShell w chmurze przy użyciu fabryki danych | Dokumentacja firmy Microsoft"
description: "Ten skrypt programu PowerShell przekształcenia danych w chmurze, uruchamiając Spark program w klastrze usługi Azure HDInsight Spark."
services: data-factory
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: spelluru
ms.openlocfilehash: 195b7276346827479fbbe10dfaaaa9ed1d754967
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="powershell-script---transform-data-in-cloud-using-azure-data-factory"></a>Skrypt programu PowerShell - przekształcania danych w chmurze przy użyciu fabryki danych Azure

Ten przykładowy skrypt programu PowerShell tworzy potok, który przekształca danych w chmurze, uruchamiając Spark program w klastrze usługi Azure HDInsight Spark. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Wymagania wstępne
* **Konto usługi Azure Storage**. Utwórz python, skrypt i pliku wejściowego i przekazać je do magazynu Azure. Dane wyjściowe programu platformy Spark są przechowywane na tym koncie magazynu. Klaster platformy Spark na żądanie używa tego samego konta magazynu, jako swojego podstawowego magazynu.  

### <a name="upload-python-script-to-your-blob-storage-account"></a>Przekazywanie skryptu w języku Python do konta usługi Blob Storage
1. Utwórz plik w języku Python o nazwie **WordCount_Spark.py** i następującej zawartości: 

    ```python
    import sys
    from operator import add
    
    from pyspark.sql import SparkSession
    
    def main():
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
            
        lines = spark.read.text("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/inputfiles/minecraftstory.txt").rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' ')) \
            .map(lambda x: (x, 1)) \
            .reduceByKey(add)
        counts.saveAsTextFile("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/outputfiles/wordcount")
        
        spark.stop()
    
    if __name__ == "__main__":
        main()
    ```
2. Zastąp wartość **&lt;storageAccountName&gt;** nazwą swojego konta usługi Azure Storage. Następnie zapisz plik. 
3. W usłudze Azure Blob Storage utwórz kontener o nazwie **adftutorial**, jeśli nie istnieje. 
4. Utwórz folder o nazwie **spark**.
5. Utwórz podfolder o nazwie **script** w folderze **spark**. 
6. Przekaż plik **WordCount_Spark.py** do podfolderu **script**. 


### <a name="upload-the-input-file"></a>Przekazywanie pliku wejściowego
1. Utwórz plik o nazwie **minecraftstory.txt** zawierający tekst. Program platformy Spark zlicza liczbę słów w tym tekście. 
2. Utwórz podfolder o nazwie `inputfiles` w `spark` folderu kontenera obiektów blob. 
3. Przekaż `minecraftstory.txt` do podfolderu `inputfiles`. 

## <a name="sample-script"></a>Przykładowy skrypt
> [!IMPORTANT]
> Ten skrypt tworzy pliki w formacie JSON, które definiują jednostek fabryki danych (połączonej usługi, zestawu danych i potoku) na dysku twardym w folderze c:\.

[!code-powershell[main](../../../powershell_scripts/data-factory/transform-data-using-spark/transform-data-using-spark.ps1 "Transform data using Spark")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia

Po uruchomieniu przykładowy skrypt służy następujące polecenie Usuń grupę zasobów i wszystkie zasoby skojarzone z nią:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```
Aby usunąć fabryki danych z grupy zasobów, uruchom następujące polecenie: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń:

| Polecenie | Uwagi |
|---|---|
| [Nowe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Zestaw AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactoryv2) | Tworzenie fabryki danych. |
| [Zestaw AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactoryv2linkedservice) | Tworzy połączonej usługi w fabryce danych. Połączona usługa łączy w magazynie danych lub obliczeniowych do fabryki danych. |
| [Zestaw AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactorv2ypipeline) | Tworzy potok w fabryce danych. Potok zawiera jeden lub więcej działań, które wykonuje określonej operacji. W tym potoku działania spark przekształcenia danych, uruchamiając program w klastrze usługi Azure HDInsight Spark. |
| [Wywołanie AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactoryv2pipelinerun) | Tworzy uruchomienia procesu. Innymi słowy jest uruchamiana potoku. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | Pobiera szczegóły uruchomienie działania (działanie Uruchom) w potoku. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |
|||

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących programu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](https://docs.microsoft.com/powershell/).

Dodatkowe przykłady skryptów PowerShell fabryki danych Azure można znaleźć w [przykłady środowiska PowerShell fabryki danych Azure](../samples-powershell.md).