---
title: "Plan sieci dla VMware do platformy Azure replikacji z usługą Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano planowanie wymagane sieci podczas replikowania maszyn wirtualnych platformy Azure z usługą Azure Site Recovery"
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
ms.date: 08/01/2017
ms.author: sujayt
ms.openlocfilehash: 7b37e853f6b97ba313111f9201f877846a28fae9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="step-3-plan-networking-for-azure-vm-replication"></a>Krok 3: Planowanie sieci w przypadku replikacji maszyny Wirtualnej Azure

Po zweryfikowaniu [wymagania wstępne dotyczące wdrażania](azure-to-azure-walkthrough-prerequisites.md), przeczytaj ten artykuł, aby zrozumieć zagadnienia sieci podczas replikacji i odzyskiwania maszyn wirtualnych platformy Azure (maszyn wirtualnych) z jednego regionu Azure do innego, przy użyciu usługi Azure Site Recovery. 

- Po zakończeniu tego artykułu, powinien mieć pełną wiedzę na temat sposobu konfigurowania wychodzący dostęp do maszyn wirtualnych platformy Azure, którą chcesz replikować, a także sposób nawiązywania połączenia z lokacji lokalnej do tych maszyn wirtualnych.
- Wszelkie komentarze umieszczaj pod tym artykułem lub zadawaj pytania na [Forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
> Azure replikację maszyny Wirtualnej z usługą Site Recovery jest obecnie w przeglądzie.



## <a name="network-overview"></a>Omówienie sieci

Zwykle maszynach wirtualnych platformy Azure znajdują się w Azure sieci/podsieci wirtualnej, a istnieje połączenie z lokacji lokalnej na platformie Azure przy użyciu usługi Azure ExpressRoute lub połączeń sieci VPN.

Sieci są zwykle chronione przy użyciu zapory lub sieci grup zabezpieczeń (NSG). Zapory można użyć na podstawie adresu URL na listach opartych na protokole IP, do kontroli łączność sieciową. Grupy NSG za pomocą adresu IP na podstawie zakresu reguł. 


Site Recovery pracę w oczekiwanym musisz wprowadzić kilka zmian w łączności wychodzącego z maszyn wirtualnych, które mają być replikowane. Usługa Site Recovery nie obsługuje użycia serwera proxy uwierzytelniania do sterowania łączność sieciową. Jeśli uwierzytelnianie serwera proxy, nie można włączyć replikacji. 


Poniższy diagram przedstawia typowe środowisko dla aplikacji działających na maszynach wirtualnych platformy Azure.

![środowisko klienta](./media/azure-to-azure-walkthrough-network/source-environment.png)

**Środowiska maszyny Wirtualnej platformy Azure**

Może również mieć połączenia z platformą Azure z witryny lokalnej, konfigurowanie za pomocą usługi Azure ExpressRoute lub połączeń sieci VPN. 

![środowisko klienta](./media/azure-to-azure-walkthrough-network/source-environment-expressroute.png)

**Połączenie lokalnej na platformie Azure**



## <a name="outbound-connectivity-for-urls"></a>Łączność wychodząca dla adresu URL

Jeśli używasz serwera proxy zapora oparta na adres URL do sterowania łączność wychodząca, upewnij się, że musisz zezwolić na te adresy URL używane przez usługę Site Recovery

**ADRES URL** | **Szczegóły**  
--- | ---
*.blob.core.windows.net | Umożliwia zapisanie z maszyny Wirtualnej, do konta magazynu pamięci podręcznej w regionie źródła danych.
Login.microsoftonline.com | Umożliwia autoryzację i uwierzytelnianie do adresów URL usługi Site Recovery.
*.hypervrecoverymanager.windowsazure.com | Umożliwia komunikację z usługą Site Recovery z maszyny Wirtualnej.
*. servicebus.windows.net | Wymagane, dzięki czemu można będzie można zapisywać danych monitorowania i diagnostyki usługi Site Recovery z maszyny Wirtualnej.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>Łączność wychodząca dla zakresów adresów IP

- Może automatycznie tworzyć wymagane zasady w grupie NSG, pobieranie i uruchamiając [ten skrypt](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).
- Zaleca się tworzenie wymaganych reguł NSG na test NSG i sprawdź, czy nie ma żadnych problemów, przed utworzeniem reguły produkcyjnych NSG.
- Aby utworzyć wymaganej liczby elementów reguły NSG, upewnij się, że Twoja subskrypcja jest białej. Skontaktuj się z pomocą techniczną w celu zwiększenia limitu reguły NSG w ramach subskrypcji.
Jeśli używasz serwera proxy oparte na protokole IP zapory lub reguły NSG do kontrolowania łączność wychodząca następujące zakresy IP muszą być białej, w zależności od lokalizacji źródłowej i docelowej maszyn wirtualnych:

    - Wszystkie zakresy adresów IP odpowiadające lokalizacji źródłowej. Pobierz [zakresy](https://www.microsoft.com/download/confirmation.aspx?id=41653).) Listę dozwolonych podobnej jest wymagana, dzięki czemu dane mogą być zapisywane na koncie magazynu pamięci podręcznej z maszyny Wirtualnej.
    - Wszystkie zakresy IP, które odpowiadają usługi Office 365 [uwierzytelnianie i tożsamość punkty końcowe IPv4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity). Nowe adresy IP są dodawane do zakresów adresów IP pakietu Office 365, należy utworzyć nowe reguły NSG.
-     Adresy IP punktu końcowego usługi odzyskiwania lokacji ([dostępne w pliku XML](https://aka.ms/site-recovery-public-ips)), które zależą od lokalizacji docelowej: 

   **Lokalizacja docelowa** | **Usługa Site Recovery adresów IP** |  **Usługa Site Recovery monitorowania IP**
   --- | --- | ---
   Azja Wschodnia | 52.175.17.132</br>40.83.121.61 | 13.94.47.61
   Azja Południowo-Wschodnia | 52.187.58.193</br>52.187.169.104 | 13.76.179.223
   Indie Środkowe | 52.172.187.37</br>52.172.157.193 | 104.211.98.185
   Indie Południowe | 52.172.46.220</br>52.172.13.124 | 104.211.224.190
   Środkowo-północne stany USA | 23.96.195.247</br>23.96.217.22 | 168.62.249.226
   Europa Północna | 40.69.212.238</br>13.74.36.46 | 52.169.18.8
   Europa Zachodnia | 52.166.13.64</br>52.166.6.245 | 40.68.93.145
   Wschodnie stany USA | 13.82.88.226</br>40.71.38.173 | 104.45.147.24
   Zachodnie stany USA | 40.83.179.48</br>13.91.45.163 | 104.40.26.199
   Środkowo-południowe stany USA | 13.84.148.14</br>13.84.172.239 | 104.210.146.250
   Środkowe stany USA | 40.69.144.231</br>40.69.167.116 | 52.165.34.144
   Wschodnie stany USA 2 | 52.184.158.163</br>52.225.216.31 | 40.79.44.59
   Japonia Wschodnia | 52.185.150.140</br>13.78.87.185 | 138.91.1.105
   Japonia Zachodnia | 52.175.146.69</br>52.175.145.200 | 138.91.17.38
   Brazylia Południowa | 191.234.185.172</br>104.41.62.15 | 23.97.97.36
   Australia Wschodnia | 104.210.113.114</br>40.126.226.199 | 191.239.64.144
   Australia Południowo-Wschodnia | 13.70.159.158</br>13.73.114.68 | 191.239.160.45
   Kanada Środkowa | 52.228.36.192</br>52.228.39.52 | 40.85.226.62
   Kanada Wschodnia | 52.229.125.98</br>52.229.126.170 | 40.86.225.142
   Środkowo-zachodnie stany USA | 52.161.20.168</br>13.78.230.131 | 13.78.149.209
   Zachodnie stany USA 2 | 52.183.45.166</br>52.175.207.234 | 13.66.228.204
   Zachodnie Zjednoczone Królestwo | 51.141.3.203</br>51.140.226.176 | 51.141.14.113
   Południowe Zjednoczone Królestwo | 51.140.43.158</br>51.140.29.146 | 51.140.189.52

## <a name="example-nsg-configuration"></a>Przykładowa konfiguracja grupy NSG

W tej sekcji przedstawiono sposób konfigurowania reguły NSG, dzięki czemu działa replikacji dla maszyny Wirtualnej. Jeśli używasz reguły NSG do kontrolowania łączność wychodząca, za pomocą reguł "Zezwalaj HTTPS wychodzącego" dla wszystkich wymaganych zakresów IP.

W tym przykładzie lokalizacja źródłowa maszyna wirtualna jest "Wschodnie stany USA". Lokalizacja docelowa replikacji jest środkowe stany USA.


### <a name="example"></a>Przykład

#### <a name="east-us"></a>Wschodnie stany USA

1. Tworzenie reguł, które odpowiadają [zakresy wschodnie stany USA IP](https://www.microsoft.com/download/confirmation.aspx?id=41653). Jest to wymagane, dzięki czemu dane mogą być zapisywane na koncie magazynu pamięci podręcznej z maszyny Wirtualnej.
2. Tworzenie reguł dla wszystkich zakresów IP, które odpowiadają usługi Office 365 [uwierzytelnianie i tożsamość punkty końcowe IPv4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Tworzenie reguł, które odnoszą się do lokalizacji docelowej:

   **Lokalizacja** | **Usługa Site Recovery adresów IP** |  **Usługa Site Recovery monitorowania IP**
    --- | --- | ---
   Środkowe stany USA | 40.69.144.231</br>40.69.167.116 | 52.165.34.144

#### <a name="central-us"></a>Środkowe stany USA

Te zasady są niezbędne, dzięki czemu można włączyć replikację z region docelowy do regionu źródła po pracy awaryjnej.

1. Tworzenie reguł, które odpowiadają [zakresy IP USA centralnej](https://www.microsoft.com/download/confirmation.aspx?id=41653).
2. Tworzenie reguł dla wszystkich zakresów IP, które odpowiadają usługi Office 365 [uwierzytelnianie i tożsamość punkty końcowe IPv4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Tworzenie reguł, które odnoszą się do lokalizacji źródłowej:

   **Lokalizacja** | **Usługa Site Recovery adresów IP** |  **Usługa Site Recovery monitorowania IP**
    --- | --- | ---
   Wschodnie stany USA | 13.82.88.226</br>40.71.38.173 | 104.45.147.24


## <a name="existing-on-premises-connection"></a>Istniejącymi lokalnymi połączenia

Jeśli masz połączenie ExpressRoute lub sieci VPN między lokalnej lokacji a lokalizacją źródłową na platformie Azure, wykonaj następujące wytyczne:

**Konfiguracja** | **Szczegóły**
--- | ---
**Wymuszone tunelowanie** | Zwykle trasa domyślna (0.0.0.0/0) wymusza wychodzący ruch internetowy przepływ lokalizacji lokalnej. Nie zaleca tego. Ruch związany z replikacją i komunikacji usługi Site Recovery powinny pozostać w obrębie platformy Azure.<br/><br/> Jako rozwiązanie, należy dodać trasy zdefiniowane przez użytkownika (Udr) dla [tych zakresów IP](#outbound-connectivity-for-azure-site-recovery-ip-ranges), dzięki czemu ruch nie działa lokalnie.
**Łączność** | Jeśli aplikacje muszą połączyć się z komputerami lokalnymi lub lokalnymi klienci łączą się z aplikacji za pośrednictwem lokalnej za pośrednictwem sieci VPN/ExpressRoute, upewnij się, musisz mieć co najmniej [połączenie lokacja lokacja](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), między docelowy region platformy Azure i lokacji lokalnej.<br/><br/> Jeśli natężeniu ruchu są wysokie między docelowy region platformy Azure i lokalnej lokacji, należy utworzyć inny [połączenia ExpressRoute](../expressroute/expressroute-introduction.md), między lokalnymi i region docelowy.<br/><br/> Jeśli chcesz zachować adresów IP dla maszyn wirtualnych po pracy awaryjnej, Zachowaj połączenia lokacja do witryny/ExpressRoute region docelowy w stanie rozłączenia. Dzięki temu nie ma żadnych kolizji zakresu między zakresów adresów IP źródłowego i docelowego.
**ExpressRoute** | Utworzyć obwodu usługi ExpressRoute w regionach źródłowe i docelowe.<br/><br/> Utwórz połączenie między siecią źródła i obwodem usługi ExpressRoute i między Sieć docelowa i obwodu.<br/><br/> Zalecane jest użycie różnych zakresów IP w regionach źródłowe i docelowe. Obwód nie można nawiązać połączenia z więcej niż jednej sieci platformy Azure z tego samego zakresu adresów IP, w tym samym czasie.<br/><br/> Można tworzenie sieci wirtualnych za pomocą tego samego zakresu adresów IP w obu regionach, a następnie utwórz obwody usługi ExpressRoute w obu regionach. Dla trybu failover rozłączyć obwodu źródła sieci wirtualnej, a następnie połącz obwód w docelowej sieci wirtualnej.<br/><br/> Jeśli regionu podstawowego całkowicie jest wyłączony, operację rozłączania może zakończyć się niepowodzeniem. W takim przypadku sieci wirtualnej docelowych nie będzie otrzymywał połączenie ExpressRoute.
**ExpressRoute — wersja Premium** | Obwody można tworzyć w tym samym regionie geograficznymi.<br/><br/> Aby utworzyć obwody w różnych regionach geograficznymi, należy Azure ExpressRoute — wersja Premium.<br/><br/> Z Premium koszt jest przyrostowe. Jeśli już używasz programu, nie ma żadnych dodatkowych kosztów.<br/><br/> Dowiedz się więcej o [lokalizacje](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) i [cennik](https://azure.microsoft.com/pricing/details/expressroute/).



## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 4: Tworzenie magazynu](azure-to-azure-walkthrough-vault.md)

