---
title: "Przykłady programu Azure PowerShell dotyczące rozwiązania Cosmos Azure DB | Dokumentacja firmy Microsoft"
description: "Azure PowerShell próbki — skryptów ułatwiających tworzenie i zarządzanie kontami bazy danych Azure rozwiązania Cosmos."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 10/16/2017
ms.author: mimig
ms.openlocfilehash: f2687369b8e247f00e9de5f3f79d8e44be5b8300
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Przykładów dla platformy Azure środowiska PowerShell dla bazy danych Azure rozwiązania Cosmos

Poniższa tabela zawiera linki do przykładowe skrypty programu PowerShell systemu Azure dla bazy danych Azure rozwiązania Cosmos. W tym momencie można zarządzać tylko accountlayer bazy danych rozwiązania Cosmos Azure za pomocą programu PowerShell; inne zasoby, takie jak bazy danych i kolekcji nie można zarządzać za pomocą programu PowerShell.

| |  |
|---|---|
|**Tworzenie konta bazy danych Azure rozwiązania Cosmos**||
|[Utwórz konto usługi DocumentDB interfejsu API](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tworzy jednego konta Azure rozwiązania Cosmos DB do użycia przy użyciu interfejsu API usługi DocumentDB. |
|**Skalować rozwiązania Cosmos Azure DB**||
|[Replikację bazy danych Azure rozwiązania Cosmos konta w wielu regionach i skonfiguruj priorytetów trybu failover](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Globalny replikuje dane konta w wielu regionach o priorytecie określonego trybu failover.|
|**Zabezpieczanie Azure rozwiązania Cosmos bazy danych**||
| [Uzyskaj klucze konta](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Pobiera klucze podstawowe i pomocnicze zapisu głównego i klucze podstawowe i pomocnicze tylko do odczytu dla konta.|
| [Pobierz ciąg połączenia bazy danych MongoDB](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Pobiera parametry połączenia do łączenie aplikacji z bazy danych MongoDB do swojego konta bazy danych Azure rozwiązania Cosmos.|
|[Wygeneruj ponownie klucze konta](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Ponowne wygenerowanie klucza głównego lub w trybie tylko do odczytu dla konta.|
|[Utworzenie zapory](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tworzy przychodzących zasad kontroli dostępu w IP można ograniczyć dostęp do konta z zatwierdzonych zbiór maszyny i/lub usług w chmurze.|
|**Wysokiej dostępności, odzyskiwania po awarii, kopia zapasowa i przywracanie**||
|[Konfigurowanie zasad trybu failover](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Ustawia priorytet trybu failover każdego regionu, w którym konta są replikowane.|
|||
