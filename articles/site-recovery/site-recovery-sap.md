---
title: "Ochrona za pomocą usługi Azure Site Recovery wdrożenia aplikacji wielowarstwowych SAP NetWeaver | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak chronić SAP NetWeaver wdrożenia aplikacji za pomocą usługi Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: manayar
ms.openlocfilehash: 5a47acab598e113ef7ed968dd3a6429ac3bc1ec3
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="protect-a-multi-tier-sap-netweaver-application-deployment-using-azure-site-recovery"></a>Ochrona wdrażania aplikacji wielowarstwowych SAP NetWeaver przy użyciu usługi Azure Site Recovery

Większości wdrożeń SAP duży i średnie ma rozwiązanie odzyskiwania po awarii.  Znaczenie rozwiązania w zakresie odzyskiwania po awarii niezawodny i testować wzrosło jako więcej podstawowe procesy biznesowe zostaną przeniesione do aplikacji, takich jak SAP.  Usługa Azure Site Recovery został przetestowane i zintegrowane z aplikacje SAP i przekraczają możliwości większości lokalnymi rozwiązaniami odzyskiwania po awarii, w dolnym całkowity koszt posiadania (TCO) niż konkurencyjnych rozwiązania.
Za pomocą usługi Azure Site Recovery można wykonywać następujące czynności:
* Włącz ochronę aplikacji produkcyjnych środowiska SAP NetWeaver i nienależących do środowiska NetWeaver uruchomionych lokalnie, replikując składniki do platformy Azure.
* Włącz ochronę aplikacji produkcyjnych środowiska SAP NetWeaver i nienależących do środowiska NetWeaver uruchomionych na platformie Azure, replikując składniki do innego centrum danych platformy Azure.
* Skorzystaj z uproszczenia migracji do chmury, używając usługi Site Recovery do migrowania wdrożenia SAP do platformy Azure.
* Uprość uaktualnienia, testowanie i tworzenie prototypów projektu SAP, tworząc na żądanie klony środowiska produkcyjnego na potrzeby testowania aplikacji SAP.

W tym artykule opisano, jak chronić SAP NetWeaver wdrożeń aplikacji przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md). W tym artykule omówiono najlepsze rozwiązania dotyczące Zabezpieczanie wdrożenia SAP NetWeaver trójwarstwowa na platformie Azure przez replikację do innego centrum danych Azure, za pomocą usługi Azure Site Recovery, konfiguracji i obsługiwane scenariusze i wykonać przełączenia do trybu failover, zarówno testu Praca awaryjna (testowanie odzyskiwania po awarii), a rzeczywista praca awaryjna.


## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem upewnij się, że należy zrozumieć następujące kwestie:

1. [Replikacja maszyny wirtualnej na platformie Azure](azure-to-azure-walkthrough-enable-replication.md)
2. Jak [projektowania sieci odzyskiwania](site-recovery-azure-to-azure-networking-guidance.md)
3. [Ten test trybu failover na platformie Azure](azure-to-azure-walkthrough-test-failover.md)
4. [Podczas pracy w trybie failover na platformie Azure](site-recovery-failover.md)
5. Jak [replikacji kontrolera domeny](site-recovery-active-directory.md)
6. Jak [replikacji programu SQL Server](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Obsługiwane scenariusze
Z usługą Azure Site Recovery można wdrożyć rozwiązanie odzyskiwania po awarii w następujących scenariuszach:
* SAP komputerów z systemami w jednym centrum danych Azure replikację do innego centrum danych Azure (Azure do platformy Azure DR), ponieważ zaprojektowana [tutaj](https://aka.ms/asr-a2a-architecture).
* Systemie SAP VMWare (lub fizycznych) serwerów lokalnych replikację do odzyskiwania po awarii lokacji w centrum danych Azure (VMware do platformy Azure DR), wymagająca niektóre dodatkowe składniki zaprojektowana [tutaj](https://aka.ms/asr-v2a-architecture).
* Lokacja SAP komputerów z systemami na replikację do odzyskiwania po awarii na lokalnych dla funkcji Hyper-V w centrum danych Azure (funkcji Hyper-V-do Azure DR), wymagająca niektóre dodatkowe składniki zaprojektowana [tutaj](https://aka.ms/asr-h2a-architecture).

Ten dokument używa pierwszym przypadku - Azure-Azure DR — aby zademonstrować funkcje odzyskiwania po awarii SAP usługi Azure Site Recovery. Zgodnie z replikacji usługi Azure Site Recovery jest niezależny od aplikacji, oczekiwano procesu opisanego do przechowywania w innych sytuacjach również.

### <a name="required-foundation-services"></a>Foundation wymagane usługi
W tym scenariuszu dokumentacji wszystkie zostały wdrożone z następującymi usługami foundation wdrożona:
* ExpressRoute lub lokacja lokacja wirtualnej sieci prywatnej (VPN)
* Co najmniej jeden kontroler domeny usługi Active Directory i serwer DNS działające na platformie Azure

Zaleca się, że powyższe infrastruktury zostanie nawiązane przed wdrożeniem usługi Azure Site Recovery.


## <a name="typical-sap-application-deployment"></a>Typowe wdrożenie aplikacji SAP
Duże Klienci SAP zwykle wdrażać od 6 do 20 poszczególne aplikacje SAP.  Większość tych aplikacji są oparte na aparaty SAP NetWeaver ABAP lub Java.  Obsługa tych podstawowych NetWeaver aplikacji są wiele mniejszych aparaty autonomiczny określonego z systemem innym niż SAP NetWeaver - i zwykle niektóre aplikacje z systemem innym niż SAP.  

Warto spis wszystkich aplikacji SAP działających w orientacji poziomej i ustalić wdrożeń w trybie (warstwy 2 lub 3-warstwowej), wersji, poprawki, rozmiary, churn — szybkości przetwarzania oraz wymagań dotyczących trwałości dysku.

![Wzorzec wdrożenia](./media/site-recovery-sap/sap-typical-deployment.png)

Warstwa trwałości bazy danych SAP powinny być chronione za pomocą wewnętrznych narzędzi systemu DBMS AlwaysOn programu SQL Server, Oracle DataGuard lub HANA replikacji systemu. Warstwa klienta nie jest również chroniony za pomocą usługi Azure Site Recovery, ale należy wziąć pod uwagę tematy, które mają wpływ na tę warstwę, takich jak opóźnienie propagacji DNS, zabezpieczeń i dostęp zdalny do odzyskiwania po awarii centrum danych.

Usługa Azure Site Recovery jest zalecanym rozwiązaniem dla warstwy aplikacji, w tym SCS (). Inne aplikacje, takie jak aplikacje NetWeaver SAP i aplikacje z systemem innym niż SAP częścią ogólnego środowiska wdrożenia SAP, a także powinny być chronione przy użyciu usługi Azure Site Recovery.

## <a name="replicate-virtual-machines"></a>Replikowanie maszyn wirtualnych
Postępuj zgodnie z [w tych wskazówkach](azure-to-azure-walkthrough-enable-replication.md) do rozpoczęcia replikacji wszystkich SAP aplikacji maszyn wirtualnych w centrum danych Azure odzyskiwania po awarii.

Jeśli używasz statycznego adresu IP, można określić adres IP, który ma maszynę wirtualną w sekcji karty interfejsu sieciowego w ustawieniach obliczeń i sieci.

![Adres IP obiektu docelowego](./media/site-recovery-sap/sap-static-ip.png)


## <a name="creating-a-recovery-plan"></a>Tworzenie planu odzyskiwania
Plan odzyskiwania umożliwia sekwencjonowania pracę awaryjną różnych warstw w wielowarstwowej aplikacji, w związku z tym zachowaniu spójności aplikacji. Wykonaj kroki opisane [tutaj](site-recovery-create-recovery-plans.md) podczas tworzenia planu odzyskiwania dla aplikacji sieci web w wielowarstwowych.

### <a name="adding-scripts-to-the-recovery-plan"></a>Dodawanie skryptów do planu odzyskiwania
Konieczne może być czy niektóre operacje na maszynach wirtualnych platformy Azure post trybu failover i testowanie trybu failover dla aplikacji do poprawnego działania. Można zautomatyzować operację trybu failover post, takich jak aktualizacja wpisu DNS i zmiana skojarzenia i połączenia, dodając odpowiednie skrypty w planie odzyskiwania, zgodnie z opisem w [w tym artykule](site-recovery-create-recovery-plans.md#add-scripts).

### <a name="dns-update"></a>Aktualizację DNS
Jeśli DNS jest skonfigurowany do dynamicznej aktualizacji DNS, a następnie maszyn wirtualnych zwykle po rozpoczęciu aktualizacji DNS z nowego adresu IP. Jeśli chcesz dodać jawne krok, aby zaktualizować DNS o nowe adresy IP maszyn wirtualnych, a następnie dodaj ją [skrypt w celu zaktualizowania IP w systemie DNS](https://aka.ms/asr-dns-update) akcji post na grupach planu odzyskiwania.  

## <a name="example-azure-to-azure-deployment"></a>Przykład wdrożenia usługi Azure do platformy Azure
Scenariusz jest opisany w diagramie poniżej DR Azure do platformy Azure usługi Azure Site Recovery:
* Podstawowego centrum danych Singapuru (Azja południowo-wschodnia Azure) i odzyskiwania po awarii centrum danych jest Hongkong (Azja Wschodnia Azure).  W tym scenariuszu lokalną wysoką dostępność są udostępniane przez mających dwie maszyny wirtualne uruchomione w trybie synchronicznym w Singapurze AlwaysOn programu SQL Server.
* ASCS udziału pliku może służyć do zapewnienia wysokiej dostępności dla programu SAP pojedynczych punktów awarii. ASCS udziału plików nie wymaga udostępnionego dysku klastra, a aplikacje, takie jak SIOS nie są wymagane.
* Ochrony DR dla warstwy DBMS odbywa się przy użyciu replikacji asynchronicznej.
* W tym scenariuszu pokazano "symetryczne DR" — termin używany do opisywania rozwiązanie odzyskiwania po awarii jest dokładny repliki produkcji, w związku z tym rozwiązanie odzyskiwania po awarii programu SQL Server ma lokalnego wysokiej dostępności. Korzystanie z symetryczne odzyskiwania po awarii nie jest wymagane dla warstwy bazy danych i korzystać z wielu klientów elastyczność wdrożeń chmury do szybkiego tworzenia lokalnego węzła wysokiej dostępności po zdarzeniu odzyskiwania po awarii.
* Diagram przedstawia warstwy serwera SAP NetWeaver ASCS i zastosowania replikowane za pomocą usługi Azure Site Recovery.

![Scenariusz replikacji](./media/site-recovery-sap/sap-replication-scenario.png)

## <a name="doing-a-test-failover"></a>Ten test trybu failover
Postępuj zgodnie z [w tych wskazówkach](azure-to-azure-walkthrough-test-failover.md) przeprowadzić test trybu failover.

1.  Przejdź do portalu Azure i wybierz magazyn usług odzyskiwania.
2.  Polecenie planu odzyskiwania utworzone dla aplikacje SAP (s).
3.  Kliknij na "Test trybu Failover".
4.  Wybierz punkt odzyskiwania i sieć wirtualna platformy Azure, aby rozpocząć proces test trybu failover.
5.  Po skonfigurowaniu dodatkowej środowiska można wykonywać z operacji sprawdzania poprawności.
6.  Po zakończeniu operacji sprawdzania poprawności, kliknij na "Oczyszczania testowy tryb failover" i wyczyścić środowisko pracy awaryjnej.

## <a name="doing-a-failover"></a>Podczas pracy w trybie failover
Postępuj zgodnie z [w tych wskazówkach](site-recovery-failover.md) podczas wprowadzania trybu failover.

1.  Przejdź do portalu Azure i wybierz magazyn usług odzyskiwania.
2.  Polecenie planu odzyskiwania utworzone dla aplikacje SAP.
3.  Kliknij pozycję "Failover".
4.  Wybierz punkt odzyskiwania, aby rozpocząć proces trybu failover.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o tworzeniu rozwiązanie odzyskiwania po awarii dla wdrożenia SAP NetWeaver przy użyciu usługi Azure Site Recovery w [dokumencie](http://aka.ms/asr-sap). Również oficjalnym dokumencie omówiono zalecenia dla różnych architektur SAP, zawiera listę obsługiwanych aplikacji i typów maszyny Wirtualnej dla programu SAP na platformie Azure i opisano możliwe planów testowych do rozwiązania do odzyskiwania po awarii.

Dowiedz się więcej o [replikowanie innych obciążeń](site-recovery-workload.md) przy użyciu usługi Site Recovery.
