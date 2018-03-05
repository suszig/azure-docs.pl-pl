---
title: "Rozwiązywanie błędów podczas powrotu po awarii maszyn wirtualnych Azure do programu VMware lokalnych z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposoby rozwiązywania typowych problemów powrotu po awarii i ponowne włączenie ochrony podczas powrotu po awarii do programu VMware z platformy Azure, za pomocą usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 02/27/2017
ms.author: rajanaki
ms.openlocfilehash: 9b1156884a78eb7d68dc9680765b3c1436c0606a
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="troubleshoot-failback-from-azure-to-vmware"></a>Rozwiązywanie problemów z powrotu po awarii z platformy Azure do programu VMware

W tym artykule opisano sposób rozwiązywania problemów, które mogą wystąpić podczas nie zostanie ponownie Azure maszyn wirtualnych o lokalnej infrastrukturze firmy VMware po przejściu w tryb failover Azure przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md).

Powrót po awarii zasadniczo obejmuje dwa podstawowe kroki. Po przejściu w tryb failover należy Włącz ponownie ochronę maszyn wirtualnych platformy Azure do środowiska lokalnego, dzięki czemu rozpoczęciu replikowanie. Drugim krokiem jest do uruchamiania trybu failover z platformy Azure, aby powrót po awarii do lokacji lokalnej. 

## <a name="troubleshoot-reprotection-errors"></a>Rozwiązywanie problemów z błędami zastosowania

Ta sekcja zawiera szczegóły typowe zastosowania błędów i rozwiązania ich.

### <a name="error-code-95226"></a>Kod błędu 95226

**Ponownej ochrony nie powiodła się, ponieważ maszyny wirtualnej platformy Azure nie mógł uzyskać dostęp do lokalnego serwera konfiguracji.**

Ten błąd występuje, gdy:

1. Maszyna wirtualna platformy Azure można nawiązać połączenia z lokalnego serwera konfiguracji. Maszyny Wirtualnej nie można odnaleźć i zarejestrowany na serwerze konfiguracji. 
2. Usługa InMage Scout aplikacji nie jest uruchomiony na maszynie Wirtualnej Azure po pracy awaryjnej. Usługa jest wymagana do komunikacji z lokalnego serwera konfiguracji.

Aby rozwiązać ten problem:

1. Sprawdź, czy sieć wirtualna Azure umożliwia maszyny Wirtualnej platformy Azure do komunikowania się z lokalnego serwera konfiguracji. Aby to zrobić, konfigurowanie sieci VPN lokacja lokacja w lokalnym centrum danych lub skonfiguruj połączenie ExpressRoute z prywatnej komunikacji równorzędnej w sieci wirtualnej maszyny wirtualnej Azure. 
2. Jeśli maszyna wirtualna może komunikować się z serwera konfiguracji lokalnej, następnie logowania się do maszyny Wirtualnej i sprawdź "Usługa aplikacji InMage Scout". Jeśli zobaczysz, że nie jest uruchomiona, uruchom usługę ręcznie, a następnie sprawdź, czy typ uruchomienia usługi jest ustawiony na automatyczny.

### <a name="error-code-78052"></a>Kod błędu 78052

***Nie można ukończyć włączania ochrony dla maszyny wirtualnej.**

Może to nastąpić, jeśli istnieje już maszyna wirtualna o tej samej nazwie na głównym serwerze docelowym z którym powrotu po awarii ponownie.

Aby rozwiązać ten problem, wykonaj następujące czynności:
1. Wybierz innego głównego serwera docelowego na innego hosta, dzięki czemu przełączonej utworzy komputera na inny host, gdzie nie powodują konfliktu nazw. 
2. Można również vMotion główny cel do innego hosta, na którym nie nastąpi kolizję nazw. Jeśli istniejącej maszyny Wirtualnej jest stray maszyny, należy ją zmienić, aby na tym samym hoście ESXi można utworzyć nowej maszyny Wirtualnej.

### <a name="error-code-78093"></a>Kod błędu 78093

**Maszyna wirtualna nie jest uruchomiona, w stanie zawieszone, lub jest niedostępny.**

Włączyć ją ponownie nieudanej za pośrednictwem maszyny Wirtualnej, musi być uruchomiona maszyna wirtualna platformy Azure. Jest to, aby usługa mobilności rejestruje konfigurację serwera lokalnego i rozpocząć replikację komunikując się z serwerem przetwarzania. Jeśli komputer znajduje się w sieci nieprawidłowe lub nie jest uruchomiony (zawiesić lub zamknięcie), serwer konfiguracji nie można osiągnąć usługi mobilności na maszynie Wirtualnej, aby rozpocząć zastosowania. 

1. Ponowne uruchomienie maszyny Wirtualnej tak, aby umożliwić komunikację wstecz lokalnymi.
2. Uruchom ponownie zadanie ponownej ochrony po uruchomieniu maszyny wirtualnej platformy Azure

### <a name="error-code-8061"></a>Kod błędu 8061

**Magazyn danych jest niedostępny z hosta ESXi.**

Sprawdź [wzorca wymagania wstępne docelowej](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server) i [obsługiwane datastores](site-recovery-how-to-reprotect.md#what-datastore-types-are-supported-on-the-on-premises-esxi-host-during-failback) powrotu po awarii.


## <a name="troubleshoot-failback-errors"></a>Rozwiązywanie problemów w przypadku powrotu po awarii

W tej sekcji opisano typowe błędy, które mogą wystąpić podczas powrotu po awarii.

### <a name="error-code-8038"></a>Kod błędu 8038

**Nie można wyświetlić na lokalnej maszynie wirtualnej z powodu błędu**

Dzieje się tak, gdy lokalnej maszyny Wirtualnej jest włączane na hoście, który nie ma wystarczającej ilości pamięci udostępnione. Aby rozwiązać ten problem:

1. Udostępnij więcej pamięci na hoście ESXi.
2. Ponadto można vMotion maszyny Wirtualnej do innego hosta ESXi, który ma wystarczającą ilość pamięci do rozruchu maszyny Wirtualnej.
