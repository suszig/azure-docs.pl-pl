---
title: "Narzędzia wprowadzanie danych maszyny wirtualnej nauki danych - Azure | Dokumentacja firmy Microsoft"
description: "Narzędzia wprowadzanie danych maszyny wirtualnej nauki danych"
keywords: "narzędzia do analizy danych, maszyny wirtualnej analizy danych, narzędzia do analizy danych, nauki danych systemu linux"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: 564c06c5017a77431b7d6fed7b43c47141b12252
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="data-science-virtual-machine-data-ingestion-tools"></a>Narzędzia wprowadzanie danych maszyny wirtualnej nauki danych

Pierwszy techniczne czynności w nauce danych lub w projekcie AI jest zidentyfikowanie zestawy danych do użycia i dostosować je do środowiska usługi analytics. Maszyna wirtualna nauki danych (DSVM) zawiera narzędzia i biblioteki, aby przenieść dane z różnych źródeł w magazynie danych analitycznych, lokalnie na DSVM lub na platformie danych w chmurze lub lokalnie. 

Poniżej przedstawiono niektóre narzędzia przepływu danych, które firma Microsoft umieściła na DSVM. 

## <a name="adlcopy"></a>AdlCopy

|    |           |
| ------------- | ------------- |
| Co to jest?   | Narzędzie można skopiować danych z obiektów blob magazynu Azure do usługi Azure Data Lake Store. Można także skopiować dane między dwa konta usługi Azure Data Lake Store.      |
| DSVM obsługiwane wersje      | Windows      |
| Typowe zastosowania      | Importowanie wielu obiektów blob magazynu Azure do usługi Azure Data Lake Store.      |
|  Jak używać / uruchom go?    |   Otwórz wiersz polecenia, wpisz `adlcopy` Aby uzyskać pomoc.    |
| Łącza do próbek      | [Przy użyciu AdlCopy] https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob)      |
| Pokrewne narzędzia na DSVM      | Narzędzia AzCopy, wiersza polecenia platformy Azure     |

## <a name="azure-command-line"></a>Wiersza polecenia platformy Azure

|    |           |
| ------------- | ------------- |
| Co to jest?   | Narzędzie do zarządzania dla platformy Azure. Zawiera także zleceń polecenie, aby przenieść dane z usługi Azure data platform, takich jak obiekty BLOB magazynu Azure, Azure Data Lake Storage     |
| DSVM obsługiwane wersje      | Windows, Linux     |
| Typowe zastosowania      | Importowanie i eksportowanie danych do i z magazynu Azure, Azure Data Lake Store      |
|  Jak używać / uruchom go?    |   Otwórz wiersz polecenia, wpisz `az` Aby uzyskać pomoc.    |
| Łącza do próbek      | [Korzystanie z interfejsu wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/?viee-cli-latest)     |
| Pokrewne narzędzia na DSVM      | Narzędzia AzCopy, AdlCopy      |


## <a name="azcopy"></a>Narzędzie AzCopy

|    |           |
| ------------- | ------------- |
| Co to jest?   | Narzędzie można skopiować danych do i z lokalnych plików, obiektów blob magazynu Azure, plików i tabele.      |
| DSVM obsługiwane wersje      | Windows      |
| Typowe zastosowania      | Kopiowanie plików do magazynu obiektów blob, kopiowania obiektów blob między kontami.      |
|  Jak używać / uruchom go?    |   Otwórz wiersz polecenia, wpisz `azcopy` Aby uzyskać pomoc.    |
| Łącza do próbek      | [Narzędzie AzCopy w systemie Windows](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy)      |
| Pokrewne narzędzia na DSVM      | AdlCopy     |


## <a name="azure-cosmos-db-documentdb-api-data-migration-tool"></a>Azure rozwiązania Cosmos bazy danych: Narzędzia migracji danych interfejsu API usługi DocumentDB

|    |           |
| ------------- | ------------- |
| Co to jest?   | Narzędzie do importowania danych z różnych źródeł, takich jak pliki w formacie JSON, CSV plików SQL, bazy danych MongoDB, Magazyn tabel Azure, Amazon DynamoDB i interfejsu API Azure rozwiązania Cosmos bazy danych usługi DocumentDB kolekcje do bazy danych rozwiązania Cosmos Azure lub usługi Azure DocumentDB.      |
| DSVM obsługiwane wersje      | Windows      |
| Typowe zastosowania      | Importowanie plików z maszyny Wirtualnej do CosmosDB, importowanie danych z magazynu tabel platformy Azure do CosmosDB lub importowanie danych z bazy danych programu SQL Server do CosmosDB.     |
|  Jak używać / uruchom go?    |   Aby użyć wiersza polecenia wersji, otwórz wiersz polecenia, wpisz `dt`. Aby użyć narzędzia graficznego interfejsu użytkownika, otwórz wiersz polecenia, wpisz `dtui`.    |
| Łącza do próbek      | [Importowanie CosmosDB danych](https://docs.microsoft.com/en-us/azure/cosmos-db/import-data)      |
| Pokrewne narzędzia na DSVM      | Narzędzia AzCopy, AdlCopy      |


## <a name="bcp"></a>bcp

|    |           |
| ------------- | ------------- |
| Co to jest?   | Narzędzie SQL Server, aby skopiować danych między programami SQL Server i plik danych.      |
| DSVM obsługiwane wersje      | Windows      |
| Typowe zastosowania      | Importowanie pliku CSV do tabeli programu SQL Server, eksportowanie tabeli programu SQL Server do pliku.      |
|  Jak używać / uruchom go?    |   Otwórz wiersz polecenia, wpisz `bcp` Aby uzyskać pomoc.    |
| Łącza do próbek      | [Narzędzie do kopiowania zbiorczego](https://docs.microsoft.com/en-us/sql/tools/bcp-utility)      |
| Pokrewne narzędzia na DSVM      | SQL Server, narzędzia sqlcmd      |


## <a name="microsoft-data-management-gateway"></a>Brama zarządzania danymi firmy Microsoft

|    |           |
| ------------- | ------------- |
| Co to jest?   | Narzędzie nawiązać lokalnych źródeł danych do usługi do użycia w chmurze.      |
| DSVM obsługiwane wersje      | Windows      |
| Typowe zastosowania      | Łączenie maszyny Wirtualnej z lokalnego źródła danych.      |
|  Jak używać / uruchom go?    |   Uruchom "Brama zarządzania danymi firmy Microsoft" z Start Menu.    |
| Łącza do próbek      | [Brama zarządzania danymi](https://msdn.microsoft.com/library/dn879362.aspx)      |
| Pokrewne narzędzia na DSVM      | Narzędzia AzCopy, AdlCopy, bcp    |
