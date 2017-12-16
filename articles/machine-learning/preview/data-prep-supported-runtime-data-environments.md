---
title: "Obsługiwane kombinacje środowisk wykonywania i danych do usługi Azure Machine Learning danych przygotowania | Dokumentacja firmy Microsoft"
description: "Ten dokument zawiera pełną listę obsługiwanych kombinacje różnych środowisk uruchomieniowych i źródeł danych dla usługi Azure Machine Learning danych przygotowania"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 93acec3bada30364b6e3338e037b442d7e6cd309
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/15/2017
---
# <a name="supported-matrix-for-this-release"></a>Macierz obsługiwane w tej wersji 
Po kodzie ładuje dane przy użyciu źródeł danych Learning maszyny Azure lub Azure Machine Learning danych przygotowania, pobierania albo Pandas lub dataframe Spark, następujących kombinacji eksperymentu obliczeniowe środowisk i lokalizacje danych są obsługiwane:

|     |Lokalne pliki  |Azure Blob Storage  |Baza danych programu SQL Server ***  |
|---------|---------|---------|---------|---------|
|Lokalne Python    |     Obsługiwane    |Nieobsługiwane         | Nieobsługiwane        |         |
|Python docker (maszyny Wirtualnej systemu Linux)     |Obsługiwane w plikach projektu tylko *         | Nieobsługiwane        |        Nieobsługiwane |         |
|PySpark docker (maszyny Wirtualnej systemu Linux)     |Obsługiwane w plikach projektu tylko *     |Obsługiwane         | Obsługiwane**        |         |
|Python maszyny wirtualnej nauki danych Azure     |Obsługiwane w plikach projektu tylko *         |Nieobsługiwane         |Nieobsługiwane         |         |
|PySPark maszyny wirtualnej nauki danych Azure     | Obsługiwane w plikach projektu tylko *        |Nieobsługiwane         |Nieobsługiwane         |         |
|Azure HDInsight PySpark     | Nieobsługiwane        |Obsługiwane         |Obsługiwane**         |         |
|Azure HDInsight języka Python     | Nieobsługiwane        | Nieobsługiwane        | Nieobsługiwane        |         |

Azure Data Lake Store nie jest obecnie obsługiwana dla dowolnego obiektu docelowego obliczeń.

* Jeśli są używane lokalne ścieżki do plików, pliki w projekcie są kopiowane do środowiska obliczeniowego, a następnie przeczytaj istnieje. Nie są kopiowane pliki poza projektem, a ścieżki nie zostanie rozwiązany w środowisku obliczeń. Należy rozważyć użycie podstawienia źródła danych, dzięki czemu kod może użyć lokalnego pliku podczas uruchamiania lokalnego. Następnie przełącz się do obiektu blob magazynu Azure do innej konfiguracji uruchamiania. Również służy Obsługa próbkowania w źródłach danych do zarządzania działa na dużej ilości danych tylko w niektórych konfiguracjach wykonywania.

** Używa sterownik programu SQL Server JDBC Maven 6.2.1. Należy się upewnić, że ten pakiet (lub jeden zgodny) znajduje się w pliku spark_dependencies.yml dla środowiska obliczeniowego.

Obsługa usługi Azure SQL Database lub SQL Server pod warunkiem, że bazy danych jest osiągalna z poziomu środowiska obliczeniowego. 
