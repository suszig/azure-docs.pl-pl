---
title: "Modyfikowanie konfiguracji urządzenia StorSimple | Dokumentacja firmy Microsoft"
description: "Informacje dotyczące używania usługi Menedżer StorSimple do zmiany konfiguracji urządzenia StorSimple, która została już wdrożona."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: bb679264-8d46-429c-9ef7-630dc3b4a423
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: 5025dc2643f9e57c0d8fa919b7d601f184d74431
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>Modyfikowanie konfiguracji urządzenia StorSimple przy użyciu usługi Menedżer StorSimple
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Aby wyświetlić wersję tego artykułu dla nowego portalu Azure, przejdź do [korzystania z usługi Menedżer StorSimple do modyfikowania konfiguracji urządzenia StorSimple](storsimple-8000-modify-device-config.md). Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Omówienie
Klasyczny portal Azure **Konfiguruj** strona zawiera wszystkie parametry urządzenia, które można skonfigurować na urządzeniu StorSimple, który jest zarządzany przez usługę Menedżer StorSimple. W tym samouczku opisano, jak używasz **Konfiguruj** strony, aby wykonać poniższe zadania poziomie urządzeń:

* Zmodyfikuj ustawienia urządzenia 
* Zmodyfikuj ustawienia czasu 
* Zmodyfikuj ustawienia DNS 
* Modyfikowanie interfejsy sieciowe
* Zamień lub ponowne przypisywanie adresów IP

## <a name="modify-device-settings"></a>Zmodyfikuj ustawienia urządzenia
Ustawienia urządzenia obejmują przyjazna nazwa urządzenia i opis urządzenia.

> [!NOTE] 
> Nie można zmodyfikować nazwy urządzenia w klasycznym portalu Azure. Zmiana nazwy urządzenia nie jest obsługiwane.

Urządzenia StorSimple, która jest połączona z usługą Menedżer StorSimple przypisano nazwę domyślną. Domyślna nazwa zazwyczaj odzwierciedla numer seryjny urządzenia. Na przykład domyślna nazwa urządzenia jest 15 znaków, takich jak 8600-SHX0991003G44HT wskazuje następujące czynności:

* **8600** — wskazuje model urządzenia.
* **SHX** — wskazuje miejsce produkcji.
* **0991003** -wskazuje określony produkt.
* **G44HT**-5 ostatnich cyfr numeru są zwiększane, aby utworzyć unikatowe numery seryjne. Może być zestawem sekwencyjnych.

Można określić opis urządzenia. Opis urządzenia zwykle pomaga zidentyfikować właściciela i lokalizacji fizycznej urządzenia. Pole opisu musi zawierać mniej niż 256 znaków.

## <a name="modify-time-settings"></a>Zmodyfikuj ustawienia czasu
Urządzenie musi synchronizować czas w celu uwierzytelniania za pomocą dostawcy usługi magazynu w chmurze. Wybierz strefę czasową z listy rozwijanej i określić maksymalnie dwa serwery protokołu NTP (Network Time). Podstawowy serwer NTP jest wymagana i określono, kiedy skonfigurować urządzenie za pomocą programu Windows PowerShell dla urządzenia StorSimple. Można określić domyślne systemu Windows Server **time.windows.com** jako serwer NTP. Można wyświetlić konfiguracji podstawowego serwera NTP za pośrednictwem klasycznego portalu Azure, ale muszą używać interfejsu programu Windows PowerShell, jego zmiany.

Dodatkowej konfiguracji serwera NTP jest opcjonalne. Klasyczny portal umożliwia konfigurowanie pomocniczy serwer NTP. 

Podczas konfigurowania serwera NTP, upewnij się, że sieć zezwala na ruch NTP z centrum danych do Internetu. Podczas określania publicznego serwera NTP, należy się upewnić zapory sieciowe i inne urządzenia zabezpieczeń skonfigurowano zezwalająca na ruch NTP do i z siecią zewnętrzną. Jeśli ruch NTP dwukierunkowego nie jest dozwolone, należy użyć wewnętrzny serwer NTP (kontrolera domeny systemu Windows zapewnia tę funkcję). Jeśli urządzenie nie może zsynchronizować czasu, nie można mogły komunikować się z dostawcą magazynu chmury.

Aby wyświetlić listę serwerów NTP publicznych, przejdź do [Web Serwery NTP](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Co się stanie, jeśli urządzenie jest wdrażana w innej strefie czasowej?
Jeśli urządzenie jest wdrażana w innej strefie czasowej, strefa czasowa urządzenia zostanie zmieniona. Biorąc pod uwagę, że wszystkie zasady tworzenia kopii zapasowych Użyj strefy czasowej urządzenia, zasad tworzenia kopii zapasowych spowoduje automatyczne dopasowanie zgodnie z nową strefą czasową. Interwencja użytkownika nie jest wymagane.

## <a name="modify-dns-settings"></a>Zmodyfikuj ustawienia DNS
Serwer DNS jest używany, gdy urządzenie próbuje nawiązać połączenia z dostawcą usług magazynu w chmurze. Wysokiej dostępności są wymagane do skonfigurowania serwera podstawowego i pomocniczego serwerów DNS podczas wdrażania wstępna urządzenia. Aby ponownie skonfigurować podstawowy serwer DNS, należy użyć interfejsu programu Windows PowerShell na urządzeniu StorSimple.

Aby zmodyfikować pomocniczy serwer DNS, można użyć klasycznego portalu Azure.

## <a name="modify-network-interfaces"></a>Modyfikowanie interfejsy sieciowe
Urządzenie ma sześć interfejsów sieciowych urządzenia, z których cztery są 1 GbE i dwa z nich są 10 GbE. Te interfejsy są oznaczone jako dane 0 – 5 danych. DANE 0, 1 danych, dane 4 i 5 danych są 1 GbE, a dane 2 i dane 3 10 GbE interfejsów.

Skonfiguruj **ustawienia interfejsu sieciowego** dla poszczególnych interfejsów, które mają być używane. Aby zapewnić wysoką dostępność, zaleca się ma co najmniej dwa interfejsy iSCSI i dwa interfejsy włączoną obsługę chmury na urządzeniu. Firma Microsoft zaleca, ale nie wymagają wyłączone nieużywane interfejsów.

Po skonfigurowaniu wszystkich interfejsów sieciowych, należy skonfigurować wirtualnego adresu IP (VIP).

Interfejs DATA 0 ma włączoną obsługę chmury domyślnie. Podczas konfigurowania dane 0, są również wymagane do skonfigurowania dwie stałe adresy IP, po jednej dla każdego kontrolera. Te stałe adresy IP mogą być używane do dostępu kontrolery urządzeń bezpośrednio i są przydatne podczas instalowania aktualizacji na urządzeniu lub w przypadku, gdy uzyskujesz dostęp do kontrolerów na potrzeby rozwiązywania problemów.

W StorSimple 8000 serii Update 1, wartość metryki routingu danych 0 do najniższego; w związku z tym jeśli urządzenie działa StorSimple 8000 serii Update 1, cały ruch z chmury, zostaną przesłane za pośrednictwem interfejsu dane 0. Zwróć uwagę to, jeśli masz więcej niż jeden interfejs sieciowy z obsługą chmury na urządzeniu StorSimple.

> [!NOTE]
> Stałe adresy IP kontrolera są używane do obsługi aktualizacji urządzenia. W związku z tym stałe adresy IP muszą być routowalne i mieć możliwość łączenia z Internetem.
> 
> 

Dla każdego interfejsu sieciowego są wyświetlane następujące parametry:

* **Szybkość** — nie można skonfigurować użytkownika parametru. DANE 0, 1 danych, dane 4 i 5 danych są zawsze 1 GbE, a dane 2 i dane 3 10 GbE interfejsów.
  
  > [!NOTE]
  > Szybkość i dupleks są zawsze auto negocjowane. Duże ramki nie są obsługiwane.
  > 
  > 
* **Stan interfejsu** — interfejs może być włączona lub wyłączona. Jeśli włączony, urządzenie będzie podejmować próby przy użyciu interfejsu. Zaleca się włączenie tylko te interfejsy, które są podłączone do sieci i używane. Wyłącz wszystkie interfejsy, które nie są używane.
* **Typ interfejsu** — ten parametr służy do izolacji ruchu iSCSI od ruchu magazynu w chmurze. Ten parametr może mieć jedną z następujących czynności:
  
  * **Chmura włączone** — po włączeniu urządzenia będą używać tego interfejsu do komunikowania się z chmurą.
  * **iSCSI włączone** — po włączeniu urządzenia będą używać tego interfejsu do komunikacji z hostem iSCSI.
    
    Zalecamy izolowanie ruchu iSCSI od ruchu magazynu w chmurze. Należy również zauważyć, jeśli host znajduje się w tej samej podsieci co urządzenie, nie trzeba przypisać bramę; Jednak jeśli host znajduje się w innej podsieci niż urządzenia, należy przypisać bramy.
* **Adres IP** — może to być IPv4 lub IPv6 lub oba. Rodziny adresów IPv4 i IPv6 są obsługiwane dla interfejsów sieciowych urządzenia. Przy użyciu protokołu IPv4, należy określić adres IP 32-bitowych (*xxx.xxx.xxx.xxx*) w notacji dziesiętnej kropki. Podczas korzystania z protokołu IPv6, wystarczy podać prefiks 4-cyfrowego i 128-bitowy adres będą generowane automatycznie dla interfejsu sieci urządzenia na podstawie tego prefiksu.
* **Podsieci** — to odwołuje się do maski podsieci i jest skonfigurowana za pośrednictwem interfejsu programu Windows PowerShell.
* **Brama** — to jest brama domyślna, które mają być używane przez ten interfejs, podczas próby komunikacji z węzłów, które nie są w tej samej przestrzeń adresów IP (podsieci). Bramy domyślnej musi być w tej samej przestrzeni adresowej (podsieć) jako interfejs adres IP, zgodnie z ustaleniami maskę podsieci.
* **Stały adres IP** — to pole jest dostępne tylko podczas konfigurowania dane 0 interfejsu. Dla operacji, takich jak aktualizacje i rozwiązywanie problemów z urządzeniem konieczne może połączyć się bezpośrednio z kontrolerem urządzenia. Stały adres IP umożliwia dostęp do aktywnych i pasywnych kontroler na urządzeniu.

Można ponownie skonfigurować kontrolera 0 i kontrolera 1 za pośrednictwem klasycznego portalu Azure.

> [!NOTE]
> * Aby zapewnić poprawne działanie, sprawdź szybkość interfejsu i dupleks na przełącznik, który jest połączony interfejs każdego urządzenia. Interfejsy przełącznika negocjowanych albo z ani można skonfigurować dla Gigabit Ethernet (1000 MB/s) i być pełny dupleks. Interfejsy działających w niższych szybkościach lub półdupleksu spowoduje problemy z wydajnością.
> * Aby zminimalizować zakłócenia i przestoje, zaleca się włączenie portfast dla wszystkich portów przełącznika, które będą łączyć urządzenia interfejsu sieciowego iSCSI. Daje to pewność, że połączenie sieciowe może zostać szybko ustanowiona w przypadku trybu failover.
> 
> 

## <a name="swap-or-reassign-ips"></a>Zamień lub ponowne przypisywanie adresów IP
Obecnie żadnym interfejsem sieciowym na kontrolerze przypisane VIP, który jest w użyciu (w tym samym urządzeniu lub innego urządzenia w sieci) kontrolera zostaną przełączone awaryjnie. W związku z tym należy postępuj zgodnie z prawidłowego, jeśli są wymiany adresów VIP dla urządzenia interfejsu sieciowego, ponieważ spowoduje utworzenie zduplikowanego sytuacji IP.

Wykonaj następujące kroki, aby zamienić lub ponownie przypisać adresy VIP dla wszystkich interfejsów sieciowych:

#### <a name="to-reassign-ips"></a>Ponowne przypisywanie adresów IP
1. Usuń adres IP dla obu interfejsów.
2. Po adresy IP są wyczyszczone, przypisz nowe adresy IP do odpowiednich interfejsów.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [konfigurowanie wielościeżkowego wejścia/wyjścia dla urządzenia StorSimple](storsimple-configure-mpio-windows-server.md).
* Dowiedz się, jak [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

