---
title: "Usługa Azure Site Recovery sieci wskazówki dotyczące replikacji maszyn wirtualnych Azure Azure | Dokumentacja firmy Microsoft"
description: "Wskazówki dotyczące replikowanie maszyn wirtualnych platformy Azure Networking"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/31/2017
ms.author: sujayt
ms.openlocfilehash: 99c08a1efbc610959fb4ba824dcb0601efac5877
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="networking-guidance-for-replicating-azure-virtual-machines"></a>Wskazówki dotyczące replikowanie maszyn wirtualnych platformy Azure Networking

>[!NOTE]
> Replikacja lokacji odzyskiwania maszyn wirtualnych platformy Azure jest obecnie w przeglądzie.

W tym artykule szczegółowo sieci wskazówki dotyczące usługi Azure Site Recovery, podczas replikacji i odzyskiwania maszyn wirtualnych platformy Azure z jednego regionu w innym regionie. Aby uzyskać więcej informacji o wymaganiach usługi Azure Site Recovery, zobacz [wymagania wstępne](site-recovery-prereq.md) artykułu.

## <a name="site-recovery-architecture"></a>Architektura odzyskiwania lokacji

Usługa Site Recovery zapewnia prosty i łatwy sposób replikowania aplikacji uruchomionych na maszynach wirtualnych platformy Azure do innego regionu Azure, dzięki czemu mogą zostać odzyskane, jeśli brak przerw w działaniu w regionie podstawowym. Dowiedz się więcej o [tego scenariusza i architekturze usługi Site Recovery](site-recovery-azure-to-azure-architecture.md).

## <a name="your-network-infrastructure"></a>Infrastruktura sieciowa użytkownika

Poniższy diagram przedstawia typowy środowiska platformy Azure dla aplikacji działających na maszynach wirtualnych platformy Azure:

![środowisko klienta](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Jeśli używasz usługi Azure ExpressRoute lub połączenie sieci VPN z sieci lokalnej do platformy Azure środowiska wygląda następująco:

![środowisko klienta](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Zazwyczaj klienci ochrony ich sieci za pomocą zapory i/lub grup zabezpieczeń sieci (NSG). Zapory można użyć na podstawie adresu URL albo opartego na protokole IP listę dozwolonych podobnej do kontrolowania łączność sieciową. Grupy NSG umożliwiają reguły kontrolowania łączność sieciową za pomocą zakresów adresów IP.

>[!IMPORTANT]
> Jeśli korzystasz z uwierzytelnionego serwera proxy do kontrolowania łączności sieciowej, nie jest obsługiwane i nie można włączyć replikacji usługi Site Recovery.

W poniższych sekcjach omówiono zmiany łączność wychodząca sieci, które są wymagane z maszyn wirtualnych platformy Azure Site Recovery replikacji do pracy.

## <a name="outbound-connectivity-for-azure-site-recovery-urls"></a>Łączność wychodząca dla adresy URL platformy Azure Site Recovery

Jeśli używasz dowolnego zapora oparta na adres URL serwera proxy do sterowania łączność wychodząca, upewnij się listą dozwolonych adresów IP, adresy URL usługi Azure Site Recovery, są wymagane:


**ADRES URL** | **Cel**  
--- | ---
*.blob.core.windows.net | Wymagane, aby dane mogą być zapisywane na konto magazynu pamięci podręcznej w regionie źródła z maszyny Wirtualnej.
Login.microsoftonline.com | Wymagane w celu autoryzacji i uwierzytelnianie adresów URL usługi Site Recovery.
*.hypervrecoverymanager.windowsazure.com | Wymagane, aby komunikacja usługi Site Recovery może wystąpić z maszyny Wirtualnej.
*. servicebus.windows.net | Wymagane, dzięki czemu można będzie można zapisywać danych monitorowania i diagnostyki usługi Site Recovery z maszyny Wirtualnej.

## <a name="outbound-connectivity-for-azure-site-recovery-ip-ranges"></a>Łączność wychodząca dla zakresów IP odzyskiwania lokacji platformy Azure

>[!NOTE]
> Aby automatycznie utworzyć wymagane reguły NSG na grupę zabezpieczeń sieci, możesz [pobranie i użycie tego skryptu](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

>[!IMPORTANT]
> * Zaleca się tworzenie wymaganych reguł NSG na testową grupę zabezpieczeń sieci, a następnie sprawdź, czy nie ma żadnych problemów przed utworzeniem reguły na grupę zabezpieczeń sieci produkcyjnych.
> * Aby utworzyć wymaganej liczby elementów reguły NSG, upewnij się, że Twoja subskrypcja jest białej. Skontaktuj się z pomocą techniczną w celu zwiększenia limitu reguły NSG w ramach subskrypcji.

Jeśli używasz serwera proxy oparte na protokole IP zapory lub reguły NSG do kontrolowania łączność wychodząca następujące zakresy IP muszą być białej, w zależności od lokalizacji źródłowej i docelowej maszyn wirtualnych:

- Wszystkie zakresy IP, które odnoszą się do lokalizacji źródłowej. (Możesz pobrać [zakresów IP](https://www.microsoft.com/download/confirmation.aspx?id=41653).) Listę dozwolonych podobnej jest wymagana do danych mogą być zapisywane na koncie magazynu pamięci podręcznej z maszyny Wirtualnej.

- Wszystkie zakresy IP, które odpowiadają usługi Office 365 [uwierzytelnianie i tożsamość punkty końcowe IPv4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

    >[!NOTE]
    > Nowe adresy IP zostaną dodane do pakietu Office 365 IP zakresów w przyszłości, należy utworzyć nowe reguły NSG.

- Punkt końcowy usługi odzyskiwania adresów IP lokacji ([dostępne w pliku XML](https://aka.ms/site-recovery-public-ips)), które zależą od lokalizacji docelowej:

   **Lokalizacja docelowa** | **Usługa Site Recovery adresów IP** |  **Usługa Site Recovery monitorowania IP**
   --- | --- | ---
   Azja Wschodnia | 52.175.17.132 | 13.94.47.61
   Azja Południowo-Wschodnia | 52.187.58.193 | 13.76.179.223
   Indie Środkowe | 52.172.187.37 | 104.211.98.185
   Indie Południowe | 52.172.46.220 | 104.211.224.190
   Środkowo-północne stany USA | 23.96.195.247 | 168.62.249.226
   Europa Północna | 40.69.212.238 | 52.169.18.8
   Europa Zachodnia | 52.166.13.64 | 40.68.93.145
   Wschodnie stany USA | 13.82.88.226 | 104.45.147.24
   Zachodnie stany USA | 40.83.179.48 | 104.40.26.199
   Środkowo-południowe stany USA | 13.84.148.14 | 104.210.146.250
   Środkowe stany USA | 40.69.144.231 | 52.165.34.144
   Wschodnie stany USA 2 | 52.184.158.163 | 40.79.44.59
   Japonia Wschodnia | 52.185.150.140 | 138.91.1.105
   Japonia Zachodnia | 52.175.146.69 | 138.91.17.38
   Brazylia Południowa | 191.234.185.172 | 23.97.97.36
   Australia Wschodnia | 104.210.113.114 | 191.239.64.144
   Australia Południowo-Wschodnia | 13.70.159.158 | 191.239.160.45
   Kanada Środkowa | 52.228.36.192 | 40.85.226.62
   Kanada Wschodnia | 52.229.125.98 | 40.86.225.142
   Środkowo-zachodnie stany USA | 52.161.20.168 | 13.78.149.209
   Zachodnie stany USA 2 | 52.183.45.166 | 13.66.228.204
   Zachodnie Zjednoczone Królestwo | 51.141.3.203 | 51.141.14.113
   Południowe Zjednoczone Królestwo | 51.140.43.158 | 51.140.189.52
   Południowe Zjednoczone Królestwo 2 | 13.87.37.4| 13.87.34.139
   Północne Zjednoczone Królestwo | 51.142.209.167 | 13.87.102.68
   Korea Środkowa | 52.231.28.253 | 52.231.32.85
   Korea Południowa | 52.231.298.185 | 52.231.200.144

## <a name="sample-nsg-configuration"></a>Przykładowa konfiguracja grupy NSG
W tej sekcji opisano kroki konfigurowania reguły NSG, dzięki czemu replikacja usługi Site Recovery może działać na maszynie wirtualnej. Jeśli używasz reguły NSG do kontrolowania łączność wychodząca, za pomocą reguł "Zezwalaj HTTPS wychodzącego" dla wszystkich wymaganych zakresów IP.

>[!Note]
> Aby automatycznie utworzyć wymagane reguły NSG na grupę zabezpieczeń sieci, możesz [pobranie i użycie tego skryptu](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

Na przykład jeśli lokalizacji źródłowej maszyny Wirtualnej jest "Wschodnie nam" i lokalizacji docelowej replikacji jest "Centralnej nam", postępuj zgodnie z instrukcjami w dwóch następnych sekcjach.

>[!IMPORTANT]
> * Zaleca się tworzenie wymaganych reguł NSG na testową grupę zabezpieczeń sieci, a następnie sprawdź, czy nie ma żadnych problemów przed utworzeniem reguły na grupę zabezpieczeń sieci produkcyjnych.
> * Aby utworzyć wymaganej liczby elementów reguły NSG, upewnij się, że Twoja subskrypcja jest białej. Skontaktuj się z pomocą techniczną w celu zwiększenia limitu reguły NSG w ramach subskrypcji.

### <a name="nsg-rules-on-the-east-us-network-security-group"></a>Reguły NSG na grupę zabezpieczeń sieci wschodnie stany USA

* Tworzenie reguł, które odpowiadają [zakresy wschodnie stany USA IP](https://www.microsoft.com/download/confirmation.aspx?id=41653). Jest to wymagane, dzięki czemu dane mogą być zapisywane na koncie magazynu pamięci podręcznej z maszyny Wirtualnej.

* Tworzenie reguł dla wszystkich zakresów IP, które odpowiadają usługi Office 365 [uwierzytelnianie i tożsamość punkty końcowe IPv4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Tworzenie reguł, które odnoszą się do lokalizacji docelowej:

   **Lokalizacja** | **Usługa Site Recovery adresów IP** |  **Usługa Site Recovery monitorowania IP**
    --- | --- | ---
   Środkowe stany USA | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules-on-the-central-us-network-security-group"></a>Reguły NSG na grupę zabezpieczeń sieci środkowe stany USA

Te zasady są niezbędne, dzięki czemu można włączyć replikację z region docelowy do źródła region po pracy w trybie failover:

* Zasady, które odpowiadają [zakresy IP USA centralnej](https://www.microsoft.com/download/confirmation.aspx?id=41653). Te są wymagane do konta magazynu pamięci podręcznej można zapisywać danych z maszyny Wirtualnej.

* Zasady dla wszystkich zakresów IP, które odpowiadają usługi Office 365 [uwierzytelnianie i tożsamość punkty końcowe IPv4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Reguły, które odnoszą się do lokalizacji źródłowej:

   **Lokalizacja** | **Usługa Site Recovery adresów IP** |  **Usługa Site Recovery monitorowania IP**
    --- | --- | ---
   Wschodnie stany USA | 13.82.88.226 | 104.45.147.24


## <a name="guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration"></a>Wytyczne dotyczące istniejącą konfigurację usługi Azure do lokalnymi lub sieć VPN

Jeśli masz połączenie ExpressRoute lub sieci VPN między lokalnymi a lokalizacją źródłową na platformie Azure, postępuj zgodnie z wytycznymi w tej sekcji.

### <a name="forced-tunneling-configuration"></a>W przypadku konfiguracji tunelowania wymuszonego

Typowa konfiguracja klienta jest do definiowania trasy domyślnej (0.0.0.0/0), która wymusza wychodzący ruch internetowy przepływ lokalizacji lokalnej. Firma Microsoft nie jest to zalecane. Ruch związany z replikacją i komunikacji usługi Site Recovery nie opuszczaj Azure granic. Rozwiązanie to dodać trasy zdefiniowane przez użytkownika (Udr) dla [tych zakresów IP](#outbound-connectivity-for-azure-site-recovery-ip-ranges) tak, aby ruch związany z replikacją nie działa lokalnie.

### <a name="connectivity-between-the-target-and-on-premises-location"></a>Łączność między lokalizacji docelowej i lokalnych

Wykonaj te wytyczne dotyczące połączeń między lokalizacji docelowej i lokalizacji lokalnego:
- Jeśli aplikacja wymaga połączyć się z komputerami lokalnymi lub w przypadku klientów, którzy łączą się z lokalnymi za pośrednictwem sieci VPN/ExpressRoute, upewnij się, że masz co najmniej [połączenie lokacja lokacja](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) między urządzenie docelowe region platformy Azure i lokalnego centrum danych.

- Jeśli spodziewasz się dużej ilości na ruch między urządzenie docelowe region platformy Azure i lokalnego centrum danych, należy utworzyć inny [połączenia ExpressRoute](../expressroute/expressroute-introduction.md) między docelowy region platformy Azure i lokalnego centrum danych.

- Jeśli chcesz zachować adresów IP dla maszyn wirtualnych, po ich w tryb failover, Zachowaj połączenia lokacja do witryny/ExpressRoute region docelowy w stanie rozłączenia. To jest upewnienie się, nie zakresu konflikt między region źródła zakresów adresów IP i zakresów adresów IP region docelowy nie istnieje.

### <a name="best-practices-for-expressroute-configuration"></a>Najlepsze rozwiązania dotyczące konfiguracji usługi ExpressRoute
Wykonaj następujące najlepsze rozwiązania dla konfiguracji usługi ExpressRoute:

- Należy utworzyć obwodu usługi ExpressRoute w regionach źródłowych i docelowych. Następnie musisz utworzyć połączenie między usługą:
  - Sieć wirtualna źródła i obwodem usługi ExpressRoute.
  - Sieć wirtualna docelowych i obwodem usługi ExpressRoute.

- W ramach standardowego ExpressRoute możesz utworzyć obwody w tym samym regionie geograficznymi. Aby utworzyć obwody usługi ExpressRoute w różnych regionach geograficznymi, Azure ExpressRoute — wersja Premium jest wymagana, która obejmuje przyrostowe kosztów. (Jeśli już używasz usługi ExpressRoute — wersja Premium, jest nie żadnymi dodatkowymi kosztami.) Aby uzyskać więcej informacji, zobacz [dokumentu lokalizacje ExpressRoute](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) i [cennik usługi ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute/).

- Zalecane jest użycie różnych zakresów IP w regionach źródłowe i docelowe. Obwód usługi expressroute nie można nawiązać połączenia z dwóch sieci wirtualnych platformy Azure z tego samego zakresu adresów IP w tym samym czasie.

- Możesz tworzenie sieci wirtualnych za pomocą tego samego zakresu adresów IP w obu regionach i następnie utwórz obwody usługi ExpressRoute w obu regionach. W przypadku wystąpieniu zdarzenia pracy awaryjnej rozłączyć obwodu źródła sieci wirtualnej, a następnie połącz obwód w docelowej sieci wirtualnej.

 >[!IMPORTANT]
 > Jeśli regionu podstawowego całkowicie jest wyłączony, operację rozłączania może zakończyć się niepowodzeniem. Która uniemożliwi docelowej sieci wirtualnej uzyskiwania połączenia ExpressRoute.

## <a name="next-steps"></a>Następne kroki
Włączyć ochronę obciążeń przez [replikowanie maszyn wirtualnych platformy Azure](site-recovery-azure-to-azure.md).
