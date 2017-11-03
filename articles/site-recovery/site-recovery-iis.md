---
title: "Replikacja IIS wielowarstwowych na podstawie aplikacji sieci web przy użyciu usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób replikowania IIS sieci web farmy maszyn wirtualnych za pomocą usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: nisoneji
ms.openlocfilehash: 4ac79df703de00ac009d9845772d8be740e74f29
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="replicate-a-multi-tier-iis-based-web-application-using-azure-site-recovery"></a>Replikowanie aplikacji sieci web usług IIS na podstawie wielowarstwową przy użyciu usługi Azure Site Recovery

## <a name="overview"></a>Omówienie


Oprogramowanie to aparat wydajności biznesowej w organizacji. Różne aplikacje sieci web może obsługiwać różnych celów w organizacji. Niektóre z nich, takie jak lista płac przetwarzania, aplikacje finansowe i klientów witryn sieci Web może być najwyższym krytyczne dla organizacji. Ważne dla organizacji, które mają i uruchomiona na wszystkich razy, aby zapobiec utracie wydajności, a co więcej chronić żadnych obrazu brand organizacji.

Aplikacje sieci web krytyczne są zwykle tworzone jako wielowarstwowe aplikacje sieci web, bazy danych i aplikacji na różnych warstw. Oprócz są rozkładane do różnych warstw, aplikacje mogą również przy użyciu wielu serwerów w poszczególnych warstwach ruch równoważenia obciążenia. Ponadto mapowania między warstwami różnych i na serwerze sieci web może być oparty na statycznych adresów IP. W tryb failover niektóre z tych mapowań będzie muszą zostać zaktualizowane, zwłaszcza, jeśli masz wiele witryn sieci Web, które są skonfigurowane na serwerze sieci web. W przypadku aplikacji sieci web przy użyciu protokołu SSL powiązania certyfikatu będzie muszą zostać zaktualizowane.

Metod tradycyjnych odzyskiwania na podstawie — replikacja obejmują wykonywanie kopii zapasowych różnych plików konfiguracji, ustawienia rejestru, powiązania, niestandardowych składników (COM lub .NET), zawartość i także certyfikaty i odzyskiwanie plików za pomocą zestawu wymagane ręczne wykonanie czynności. Te techniki są jasno obciążeniem, błąd podatnych na błędy i nie skalowalności. Jest na przykład, łatwe w miarę zapomnij wykonywanie kopii zapasowej certyfikatów i pozostać z wyborem nie, ale kupić nowe certyfikaty serwera po pracy awaryjnej.

Dobre rozwiązanie odzyskiwania po awarii, powinno umożliwić modelowania planów odzyskiwania wokół powyższych architekturach aplikacji złożonych, a także mieć możliwość dodawania kroków dostosowane do obsługi aplikacji mapowań między różnych warstw, dlatego zapewnienie pojedynczym kliknięciem czy zrzut rozwiązanie w przypadku awarii, co może prowadzić do dolnej RTO.


W tym artykule opisano sposób chronić usług IIS na podstawie sieci web aplikacji za pomocą [usługi Azure Site Recovery](site-recovery-overview.md). W tym artykule opisano najlepsze rozwiązania dotyczące replikacji trzy warstwy aplikacji sieci web usług IIS na podstawie na platformie Azure, jak wyszczególniania odzyskiwania po awarii oraz sposób pracy awaryjnej aplikacji na platformie Azure.


## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że należy zrozumieć następujące kwestie:

1. [Replikacja maszyny wirtualnej na platformie Azure](site-recovery-vmware-to-azure.md)
1. Jak [projektowania sieci odzyskiwania](site-recovery-network-design.md)
1. [Ten test trybu failover na platformie Azure](./site-recovery-test-failover-to-azure.md)
1. [Podczas pracy w trybie failover na platformie Azure](site-recovery-failover.md)
1. Jak [replikacji kontrolera domeny](site-recovery-active-directory.md)
1. Jak [replikacji programu SQL Server](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Wzorce wdrożenia
Aplikacji sieci web usług IIS na podstawie zwykle obejmuje jedną z następujących wzorców wdrożenia:

** Wdrożenia wzorzec 1 ** usług IIS na podstawie kolektywu serwerów sieci web z Routing(ARR) żądania aplikacji, serwer usług IIS i Microsoft SQL Server.

![Wzorzec wdrożenia](./media/site-recovery-iis/deployment-pattern1.png)

**Wzorzec wdrożenia 2** usług IIS na podstawie kolektywu serwerów sieci web z Routing(ARR) żądania aplikacji, serwer usług IIS, serwera aplikacji i programu Microsoft SQL Server.


![Wzorzec wdrożenia](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Obsługa odzyskiwania lokacji

Na potrzeby tworzenia maszyn wirtualnych VMware tego artykułu z serwerem usług IIS w wersji 7.5 w systemie Windows Server 2012 R2 Enterprise były używane. Ponieważ replikacja z lokacji odzyskiwania jest niezależny od aplikacji, zalecenia zawarte w tym miejscu powinny przechowywane w następujących scenariuszach, jak również i dla różnych wersji programu IIS.

### <a name="source-and-target"></a>Źródłowa i docelowa

**Scenariusz** | **Do lokacji dodatkowej** | **Na platformie Azure**
--- | --- | ---
**Funkcja Hyper-V** | Tak | Tak
**VMware** | Tak | Tak
**Serwer fizyczny** | Nie | Tak

## <a name="replicate-virtual-machines"></a>Replikowanie maszyn wirtualnych

Postępuj zgodnie z [w tych wskazówkach](site-recovery-vmware-to-azure.md) do rozpoczęcia replikacji wszystkich usług IIS sieci web farmy maszyn wirtualnych na platformie Azure.

Jeśli jest używany statyczny adres IP, należy określić adres IP, który ma maszyny wirtualnej w [ **docelowy adres IP** ](./site-recovery-replicate-vmware-to-azure.md#view-and-manage-vm-properties) ustawienie w ustawieniach obliczeń i sieci.

![Adres IP obiektu docelowego](./media/site-recovery-active-directory/dns-target-ip.png)


## <a name="creating-a-recovery-plan"></a>Tworzenie planu odzyskiwania

Plan odzyskiwania umożliwia sekwencjonowania pracę awaryjną różnych warstw w wielowarstwowej aplikacji, w związku z tym zachowaniu spójności aplikacji. Wykonaj następujące czynności, podczas tworzenia planu odzyskiwania dla aplikacji sieci web w wielowarstwowych.  [Dowiedz się więcej o tworzeniu planu odzyskiwania](./site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-to-failover-groups"></a>Dodawanie maszyn wirtualnych do trybu failover grupy
Typowa aplikacja sieci web usług IIS wielowarstwowych będzie składać się z warstwy bazy danych SQL maszyn wirtualnych, warstwa sieci web utworzoną przez serwer usług IIS i warstwy aplikacji. Dodaj wszystkie maszyny wirtualne do innej grupy oparte na warstwę poniżej. [Dowiedz się więcej o dostosowanie planu odzyskiwania](site-recovery-runbook-automation.md#customize-the-recovery-plan).

1. Tworzenie planu odzyskiwania. Dodaj maszyny wirtualne warstwy bazy danych w grupy 1-Sprawdź, czy ostatnie są zamknięcia i włączane pierwszej.

1. Dodaj maszyny wirtualne warstwy aplikacji, w obszarze grupy 2 w taki sposób, że są włączane po włączane warstwy bazy danych.

1. Dodaj maszyny wirtualne warstwy sieci web w grupa 3 w taki sposób, że są włączane po włączane warstwy aplikacji.

1. Dodaj maszyny wirtualne równoważenia obciążenia w grupie 4 taki sposób, że są włączane po włączane warstwa sieci web.


### <a name="adding-scripts-to-the-recovery-plan"></a>Dodawanie skryptów do planu odzyskiwania
Konieczne może być czy niektóre operacje na maszynach wirtualnych platformy Azure post trybu failover i testowanie trybu failover Aby poprawnie funkcja kolektywu serwerów sieci web usług IIS. Można zautomatyzować operację trybu failover post, takich jak aktualizacja wpisu DNS, zmiana powiązania witryny, zmiany w parametrach połączenia, dodając odpowiednie skrypty w planie odzyskiwania, jak pokazano poniżej. [Dowiedz się więcej na temat dodawania skryptu planu odzyskiwania](./site-recovery-create-recovery-plans.md#add-scripts).

#### <a name="dns-update"></a>Aktualizację DNS
Jeśli DNS jest skonfigurowany do dynamicznej aktualizacji DNS, a następnie maszyn wirtualnych zwykle po rozpoczęciu aktualizacji DNS z nowego adresu IP. Jeśli chcesz dodać jawne krok, aby zaktualizować DNS o nowe adresy IP maszyn wirtualnych, a następnie dodaj ją [skrypt w celu zaktualizowania IP w systemie DNS](https://aka.ms/asr-dns-update) akcji post na grupach planu odzyskiwania.  

#### <a name="connection-string-in-an-applications-webconfig"></a>Parametry połączenia w pliku web.config aplikacji
Ciąg połączenia określa witryny sieci web, który komunikuje się z bazy danych.

Jeśli parametry połączenia przyjmuje nazwę maszyny wirtualnej bazy danych, żadne dalsze czynności będą potrzebne post trybu failover, a aplikacja będzie automatycznie komunikować się z bazą danych. Ponadto jeśli adres IP dla maszyny wirtualnej bazy danych jest zachowywana, go nie będzie potrzebna, aby zaktualizować parametry połączenia. Jeśli parametry połączenia odwołuje się do maszyny wirtualnej bazy danych przy użyciu adresu IP, będzie musiała zostać zaktualizowane po pracy awaryjnej. Na przykład poniżej punkty ciągu połączenia z bazą danych z adresem IP 127.0.1.2

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Parametry połączenia w warstwa sieci web można zaktualizować przez dodanie [skryptu aktualizacji połączenia usług IIS](https://aka.ms/asr-update-webtier-script-classic) po grupa 3 w planie odzyskiwania.

#### <a name="site-bindings-for-the-application"></a>Powiązania witryny dla aplikacji
Każda witryna składa się z powiązania informacje, które obejmują typ powiązania, adres IP, jaką serwera IIS nasłuchuje żądań dla witryny, numer portu i nazwy hostów dla tej lokacji. W czasie pracy awaryjnej te powiązania może muszą zostać zaktualizowane w przypadku zmiany adresów IP skojarzonych z nimi.

> [!NOTE]
>
> Po oznaczeniu "wszystkie nieprzypisane" dla powiązania witryny, tak jak w poniższym przykładzie, nie będzie konieczne zaktualizowanie tej pracy awaryjnej post powiązania. Ponadto, jeśli adres IP skojarzony z lokacją nie ulega zmianie post trybu failover, powiązania witryny muszą nie można zaktualizować (przechowywania adresu IP jest zależna od architektury sieci i podsieci przypisane do lokacji głównej i odzyskiwania lokacji i dlatego mogą lub może nie być możliwe dla Twojej organizacji.)

![Powiązanie SSL](./media/site-recovery-iis/sslbinding.png)

Jeśli adres IP został skojarzony z lokacją, należy zaktualizować wszystkie powiązania witryny przy użyciu nowego adresu IP. Możesz dodać [skryptu aktualizacji warstwy sieci Web usług IIS](https://aka.ms/asr-web-tier-update-runbook-classic) po grupa 3 w planie odzyskiwania, aby zmienić powiązania witryny.


#### <a name="update-load-balancer-ip-address"></a>Zaktualizuj adres IP usługi równoważenia obciążenia
Jeśli masz Routing żądań aplikacji maszyny wirtualnej, dodać [skryptu trybu failover IIS ARR](https://aka.ms/asr-iis-arrtier-failover-script-classic) po 4 grupy można zaktualizować adresu IP.

#### <a name="the-ssl-cert-binding-for-an-https-connection"></a>Powiązania certyfikatu SSL dla połączenia https
Witryny sieci Web może mieć skojarzone certyfikatu SSL, która pomaga w celu zapewnienia bezpiecznej komunikacji między serwer sieci Web i przeglądarki użytkownika. Jeśli witryna sieci Web ma połączenie https i powiązanie witryny https skojarzony adres IP serwera IIS wraz z powiązaniem certyfikatu SSL, nowe powiązanie witryny należy można dodać certyfikatu z adresem IP Failover post maszyny wirtualnej usług IIS.

Certyfikat SSL mogą być wystawiane przed-

) w pełni kwalifikowaną nazwę domeny witryny sieci Web<br>
(b) nazwa serwera<br>
c) certyfikat uniwersalny, dla nazwy domeny<br>
d) adres IP — Jeśli certyfikat SSL jest wystawiony na podstawie adresu IP serwera usług IIS, inny certyfikat SSL musi być wystawiony na podstawie adresu IP serwera usług IIS w witrynie platformy Azure i będzie trzeba utworzyć dodatkowe powiązania SSL dla tego certyfikatu. W związku z tym zaleca się nie używać certyfikatu SSL wystawiony na podstawie adresu IP. To jest opcja mniej powszechnie używane i wkrótce zostaną wycofane zgodnie z harmonogramem nowych zmian forum urzędu certyfikacji/przeglądarki.

#### <a name="update-the-dependency-between-the-web-and-the-application-tier"></a>Aktualizuj zależności między sieci web a warstwą aplikacji
Jeśli masz zależność określonych aplikacji na podstawie adresu IP maszyn wirtualnych, należy zaktualizować tej pracy awaryjnej post zależności.

## <a name="doing-a-test-failover"></a>Ten test trybu failover
Postępuj zgodnie z [w tych wskazówkach](site-recovery-test-failover-to-azure.md) przeprowadzić test trybu failover.

1.  Przejdź do portalu Azure i wybierz magazyn usług odzyskiwania.
1.  Polecenie planu odzyskiwania utworzone dla kolektywu serwerów sieci web usług IIS.
1.  Kliknij na "Test trybu Failover".
1.  Wybierz punkt odzyskiwania i sieć wirtualna platformy Azure, aby rozpocząć proces test trybu failover.
1.  Po skonfigurowaniu dodatkowej środowiska można wykonywać z operacji sprawdzania poprawności.
1.  Po zakończeniu operacji sprawdzania poprawności, wybierz opcję "Ukończenie operacji sprawdzania poprawności" i testowe środowisko trybu failover zostaną wyczyszczone.

## <a name="doing-a-failover"></a>Podczas pracy w trybie failover
Postępuj zgodnie z [w tych wskazówkach](site-recovery-failover.md) podczas wprowadzania trybu failover.

1.  Przejdź do portalu Azure i wybierz magazyn usług odzyskiwania.
1.  Polecenie planu odzyskiwania utworzone dla kolektywu serwerów sieci web usług IIS.
1.  Kliknij pozycję "Failover".
1.  Wybierz punkt odzyskiwania, aby rozpocząć proces trybu failover.

## <a name="next-steps"></a>Następne kroki
Użytkownik może dowiedzieć się więcej o [replikacji z innych aplikacji](site-recovery-workload.md) przy użyciu usługi Site Recovery.
