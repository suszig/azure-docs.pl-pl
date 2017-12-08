---
title: "Co robić w przypadku platformy Azure usługa przerw w działaniu, który ma wpływ na usługi Azure Key Vault | Dokumentacja firmy Microsoft"
description: "Dowiedz się, co należy zrobić w przypadku Azure zakłócenia, który ma wpływ na usługi Azure Key Vault."
services: key-vault
documentationcenter: 
author: adamglick
manager: mbaldwin
editor: 
ms.assetid: 19a9af63-3032-447b-9d1a-b0125f384edb
ms.service: key-vault
ms.workload: key-vault
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: sumedhb;aglick
ms.openlocfilehash: 6419d54c54e7d19103419262b79e7a5268b2268c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Usługa Azure Key Vault dostępność i nadmiarowość
Usługa Azure Key Vault funkcjami wielu warstw nadmiarowość, aby upewnić się, że kluczy i kluczy tajnych pozostają dostępne dla aplikacji nawet w przypadku awarii poszczególnych składników usługi.

Zawartość magazynu kluczy są replikowane w regionie i w regionie pomocniczym co najmniej 150 mil optymalizacji, ale w tej samej lokalizacji geograficznej. Zapewnia to dostępność wysoką trwałością kluczy i kluczy tajnych. Zobacz [Azure łączyć regionów](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) dokumentu szczegółowe informacje dotyczące konkretnego regionu pary.

W przypadku awarii pojedynczych składników w usłudze magazyn kluczy, alternatywne składniki w regionie kroku obsługiwać Twoje żądanie, aby upewnić się, że nie jest bez spadku funkcji. Nie trzeba wykonywać żadnych czynności w celu wyzwolenia to. Odbywa się automatycznie, a ma być przezroczysty dla użytkownika.

W rzadkich niedostępności całego regionu Azure, automatycznie są kierowane żądań, które należy z usługi Azure Key Vault w tym regionie (*przejścia w tryb failover*) w regionie pomocniczym. Gdy ponownie jest dostępna w regionie podstawowym, żądania są kierowane Wstecz (*nie powiodło się wstecz*) w regionie podstawowym. Ponownie nie ma potrzeby podejmować żadnych działań, ponieważ jest to wykonywane automatycznie.

Istnieje kilka ostrzeżenia pod uwagę:

* W przypadku trybu failover regionu może upłynąć kilka minut, aż usługi do pracy awaryjnej. Żądania, które zostały wprowadzone w tym czasie może zakończyć się niepowodzeniem, dopóki nie zakończy pracy awaryjnej.
* Po zakończeniu pracy w trybie failover magazynu kluczy jest w trybie tylko do odczytu. Żądania, które są obsługiwane w tym trybie są:
  * Lista magazynów kluczy
  * Pobierz właściwości magazynów kluczy
  * Utwórz listę kluczy tajnych
  * Pobierz kluczy tajnych
  * Lista kluczy
  * Pobieranie kluczy (właściwości)
  * Szyfrowanie
  * Odszyfrowywanie
  * Zawijaj
  * Dekodowanie
  * Weryfikuj
  * Zaloguj się
  * Tworzenie kopii zapasowych
* Po przejściu w tryb failover nie jest ponownie, wszystkie żądania typy (w tym odczytu *i* żądań zapisu) są dostępne.

