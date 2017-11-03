---
title: "Przykłady interfejsu wiersza polecenia platformy Azure dla platformy Azure rozwiązania Cosmos DB | Dokumentacja firmy Microsoft"
description: "Przykładów dla interfejsu wiersza polecenia platformy Azure — tworzenie i zarządzanie kontami, baz danych, kontenerów, regiony i zapory bazy danych Azure rozwiązania Cosmos."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 06/07/2017
ms.author: mimig
ms.openlocfilehash: 189034b049e776a3b929930be937ec60bc1db241
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Przykładów dla interfejsu wiersza polecenia platformy Azure dla bazy danych Azure rozwiązania Cosmos

Poniższa tabela zawiera linki do przykładowe skrypty wiersza polecenia platformy Azure dla bazy danych Azure rozwiązania Cosmos. Odwołanie do strony dla wszystkich poleceń interfejsu wiersza polecenia Azure rozwiązania Cosmos bazy danych są dostępne w [odwołania 2.0 interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/cosmosdb).

| |  |
|---|---|
|**Tworzenie konta bazy danych Azure rozwiązania Cosmos, bazy danych i kontenerów**||
|[Utwórz konto usługi DocumentDB interfejsu API, wykres lub tabeli interfejsu API](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Tworzy jednego konta Azure rozwiązania Cosmos DB API, bazy danych i kontener do użycia z usługi DocumentDB, wykres lub interfejsów API tabeli. |
| [Tworzenie konta bazy danych MongoDB interfejsu API](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Tworzy jednego interfejsu API Azure rozwiązania Cosmos bazy danych MongoDB konta bazy danych i kolekcji. |
|**Skalować rozwiązania Cosmos Azure DB**||
| [Przepływność kontenera skali](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Zmienia elastycznie througput do kontenera.|
|[Replikację bazy danych Azure rozwiązania Cosmos konta bazy danych w wielu regionach i skonfiguruj priorytetów trybu failover](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Globalny replikuje dane konta w wielu regionach o priorytecie określonego trybu failover.|
|**Zabezpieczanie Azure rozwiązania Cosmos bazy danych**||
| [Uzyskaj klucze konta](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Pobiera klucze podstawowe i pomocnicze zapisu głównego i klucze podstawowe i pomocnicze tylko do odczytu dla konta.|
| [Pobierz ciąg połączenia bazy danych MongoDB](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Pobiera parametry połączenia do łączenie aplikacji z bazy danych MongoDB do swojego konta bazy danych Azure rozwiązania Cosmos.|
|[Wygeneruj ponownie klucze konta](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Ponowne wygenerowanie klucza głównego lub w trybie tylko do odczytu dla konta.|
|[Utworzenie zapory](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Tworzy przychodzących zasad kontroli dostępu w IP można ograniczyć dostęp do konta z zatwierdzonych zbiór maszyny i/lub usług w chmurze.|
|**Wysokiej dostępności, odzyskiwania po awarii, kopia zapasowa i przywracanie**||
|[Konfigurowanie zasad trybu failover](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Ustawia priorytet trybu failover każdego regionu, w którym konta są replikowane.|
|**Łączenie z zasobami Azure DB rozwiązania Cosmos**||
|[Łączenie aplikacji sieci web do bazy danych Azure rozwiązania Cosmos](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Utwórz i połączenia bazy danych Azure rozwiązania Cosmos bazy danych i aplikacji sieci web platformy Azure.|
|||
