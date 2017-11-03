---
title: "Obsługa klientów niższych poziomów usługi SQL Data Warehouse inspekcja danych | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat obsługi klientów niższych poziomów usługi SQL Data Warehouse danych inspekcji"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: dfe29ff3-dfeb-4309-83c0-c1a300f4f44e
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: a7ea6141285a0098339f1e071af2592dd4535c12
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>Obsługa klientów niższych poziomów usługi SQL Data Warehouse — inspekcji i dynamicznego maskowania danych
[Inspekcja](sql-data-warehouse-auditing-overview.md) współpracuje z klientom SQL, które obsługują przekierowanie TDS.

Dowolnego klienta, który implementuje TDS 7.4 powinny również obsługiwać przekierowania. Wyjątki od tej reguły obejmują JDBC 4.0, w którym funkcji przekierowania nie jest w pełni obsługiwane i Tedious dla środowiska Node.JS, w których przekierowania nie została zaimplementowana.

Dla "Klientów niższych poziomów" tj. które Obsługa TDS 7.3 i poniżej — nazwa FQDN serwera w połączeniu ciąg wersji powinno zostać zmodyfikowane:

Oryginalna nazwa FQDN serwera w ciągu połączenia: <*nazwy serwera*>. database.windows.net

Nazwa FQDN serwera zmodyfikowane w ciągu połączenia: <*nazwy serwera*> .database. **bezpieczne**. windows.net

Zawiera listę częściowej "Klienci z obniżonym poziomem":

* .NET 4.0 i poniżej,
* ODBC 10.0 i poniżej.
* JDBC (podczas JDBC obsługuje TDS 7.4, funkcji przekierowania TDS nie jest całkowicie obsługiwana)
* Niewygodny (dla środowiska Node.JS)

**Uwaga:** modyfikacji FDQN powyższym serwerze mogą być przydatne także stosowania zasad inspekcji programu SQL Server poziom bez potrzebę konfiguracji kroku w każdej bazie danych (ograniczenie tymczasowe).     

