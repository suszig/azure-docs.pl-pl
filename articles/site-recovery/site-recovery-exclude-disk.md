---
title: "Wykluczanie dysku z ochrony przy użyciu usługi Azure Site Recovery | Microsoft Docs"
description: "Opisuje dlaczego i jak należy wykluczyć dyski maszyny wirtualnej z replikacji dla scenariuszy replikacji z programu VMware do platformy Azure i funkcji Hyper-V platformy Azure."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 1/24/2017
ms.author: nisoneji
translationtype: Human Translation
ms.sourcegitcommit: af0d66d92ca542f415de779fb638db166ba5f26a
ms.openlocfilehash: 5e0527fb0a41d8892c9e22d6d6d2f252972e69d0


---
#<a name="exclude-disk-from-replication"></a>Wykluczanie dysku z replikacji
W tym artykule opisano, jak wykluczyć dyski z replikacji, aby zoptymalizować zużywaną przepustowość replikacji lub aby zoptymalizować zasoby po stronie docelowej wykorzystywane przez te dyski. Ta funkcja jest obsługiwana w przypadku scenariuszy replikacji z programu VMware do platformy Azure i z funkcji Hyper-V do platformy Azure.

##<a name="prerequisites"></a>Wymagania wstępne

Domyślnie wszystkie dyski na maszynie są replikowane. Aby wykluczyć dysk z replikacji, w przypadku replikacji **z programu VMware do platformy Azure** należy ręcznie zainstalować usługę mobilności na maszynie przed włączeniem replikacji.


## <a name="why-exclude-disks-from-replication"></a>Dlaczego wykluczać dyski z replikacji?
Wykluczenie dysków z replikacji jest często konieczne, ponieważ:

1. Współczynnik zmian danych na wykluczonym dysku nie jest istotny lub nie ma konieczności jego replikacji.

2. Zasoby magazynu i zasoby sieciowe można zapisywać bez replikowania tego współczynnika zmian.

##<a name="what-are-the-typical-scenarios"></a>Jak wyglądają typowe scenariusze?
Istnieją pewne typowe przykłady współczynnika zmian danych, które można łatwo zidentyfikować i które są doskonałymi kandydatami do wykluczenia — na przykład operacje zapisu pliku stronicowania, operacje zapisu bazy danych tempdb programu Microsoft SQL Server itp. W zależności od obciążenia i podsystemu magazynu plik stronicowania może rejestrować znaczącą ilość zmian. Jednak replikowanie tych danych z lokacji głównej do platformy Azure wymagałoby dużej ilości zasobów. Z tego powodu replikację maszyny wirtualnej z jednym dyskiem wirtualnym, na którym znajduje się zarówno system operacyjny, jak i plik stronicowania, można zoptymalizować poprzez:

1. Podzielenie jednego dysku wirtualnego na dwa dyski wirtualne — jeden z systemem operacyjnym i jeden z plikiem stronicowania
2. Wykluczenie dysku z plikiem stronicowania z replikacji.

Podobnie w przypadku programu Microsoft SQL Server z bazą danych tempdb i plikiem systemowej bazy danych na tym samym dysku można dokonać optymalizacji przez:

1. Przechowywanie systemowej bazy danych i bazy danych tempdb na dwóch różnych dyskach
2. Wykluczenie dysku z bazą danych tempdb z replikacji.

##<a name="how-to-exclude-disk-from-replication"></a>Jak wykluczyć dysk z replikacji?

###<a name="vmware-to-azure"></a>Z programu VMware do platformy Azure
Postępuj zgodnie z przepływem pracy [Włączanie replikacji](site-recovery-vmware-to-azure.md#enable-replication), aby chronić maszynę wirtualną z portalu usługi Azure Site Recovery. W 4. kroku przepływu pracy Włączanie replikacji znajduje się kolumna **DYSK DO REPLIKACJI**, za pomocą której można wykluczyć dysk z replikacji. Domyślnie wszystkie dyski są zaznaczone. Usuń zaznaczenie dysku, który chcesz wykluczyć z replikacji, i postępuj zgodnie z instrukcjami, aby włączyć replikację. 

![Włączanie replikacji](./media/site-recovery-exclude-disk/v2a-enable-replication-exclude-disk1.png)
    
    
>[!NOTE]
> 
> * Można wykluczyć tylko te dyski, na których jest już zainstalowana usługa mobilności. Należy ręcznie zainstalować usługę mobilności, ponieważ jest ona instalowana tylko przy użyciu mechanizmu wypychania po włączeniu replikacji.
> * Tylko dyski podstawowe można wyłączyć z replikacji. Nie można wyłączyć dysków systemu operacyjnego ani dysków dynamicznych.
> * Po włączeniu replikacji nie można dodawać ani usuwać dysków do replikacji. Jeśli chcesz dodać lub wykluczyć dysk, musisz wyłączyć ochronę maszyny, a następnie włączyć ją ponownie.
> * Jeśli wykluczysz dysk wymagany do działania aplikacji, po przejściu do trybu failover na platformie Azure konieczne będzie utworzenie go ręcznie na tej platformie, aby można było uruchomić replikowaną aplikację. Można również zintegrować usługę Azure Automation w planie odzyskiwania, aby utworzyć dysk podczas pracy maszyny w trybie failover.
> * Maszyna wirtualna z systemem Windows: Dyski utworzone ręcznie na platformie Azure nie mają możliwości powrotu po awarii. Jeśli na przykład przeniesiesz trzy dyski do trybu failover i utworzysz dwa bezpośrednio na maszynie wirtualnej platformy Azure, powrót po awarii nastąpi tylko dla trzech dysków przeniesionych do trybu failover. Dysków tworzonych ręcznie nie można uwzględnić podczas powrotu po awarii lub ponownego włączania ochrony z zasobów lokalnych do platformy Azure.
> * Maszyna wirtualna z systemem Linux: W przypadku dysków utworzonych ręcznie na platformie Azure nastąpi powrót po awarii. Jeśli na przykład przeniesiesz trzy dyski do trybu failover i utworzysz dwa bezpośrednio na platformie Azure, powrót po awarii nastąpi dla wszystkich pięciu dysków. Nie można wykluczyć dysków utworzonych ręcznie po powrocie po awarii.
> 

###<a name="hyper-v-to-azure"></a>Z funkcji Hyper-V do platformy Azure
Postępuj zgodnie z przepływem pracy [Włączanie replikacji](site-recovery-hyper-v-site-to-azure.md#step-6-enable-replication), aby chronić maszynę wirtualną z portalu usługi Azure Site Recovery. W 4. kroku przepływu pracy Włączanie replikacji znajduje się kolumna **DYSK DO REPLIKACJI**, za pomocą której można wykluczyć dyski z replikacji. Domyślnie do replikacji są wybierane wszystkie dyski. Usuń zaznaczenie dysku, który chcesz wykluczyć z replikacji, i postępuj zgodnie z instrukcjami, aby włączyć replikację. 

![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)
    
>[!NOTE]
> 
> * Tylko dyski podstawowe można wyłączyć z replikacji. Nie można wykluczyć dysku systemu operacyjnego i nie jest zalecane wykluczanie dysków dynamicznych. Usługa ASR nie może ustalić, który wirtualny dysk twardy jest podstawowym, a który dynamicznym dyskiem wewnątrz maszyny wirtualnej gościa.  Jeśli wszystkie zależne dyski woluminu dynamicznego nie zostaną wykluczone, chroniony dysk dynamiczny będzie uszkodzonym dyskiem maszyny wirtualnej w trybie failover, a dane na tym dysku będą niedostępne.    
> * Po włączeniu replikacji nie można dodawać ani usuwać dysków do replikacji. Jeśli chcesz dodać lub wykluczyć dysk, musisz wyłączyć ochronę maszyny wirtualnej, a następnie włączyć ją ponownie.
> * Jeśli wykluczasz dysk wymagany do działania aplikacji, po przejściu do trybu failover na platformie Azure musisz utworzyć go na tej platformie, aby można było uruchomić replikowaną aplikację. Można również zintegrować usługę Azure Automation w planie odzyskiwania, aby utworzyć dysk podczas pracy maszyny w trybie failover.
> * Dyski utworzone ręcznie na platformie Azure nie mają możliwości powrotu po awarii. Jeśli na przykład przeniesiesz trzy dyski do trybu failover i utworzysz dwa bezpośrednio na maszynie wirtualnej platformy Azure, tylko dla trzech dysków przeniesionych do trybu failover nastąpi powrót po awarii z platformy Azure do funkcji Hyper-V. Dysków tworzonych ręcznie nie można uwzględnić podczas powrotu po awarii ani replikacji odwrotnej z funkcji Hyper-V do platformy Azure.
 


##<a name="end-to-end-scenarios-of-exclude-disks"></a>Kompleksowe scenariusze wykluczania dysków
Rozważmy dwa scenariusze, aby lepiej zrozumieć funkcję wykluczania dysku.

1. Dysk bazy danych tempdb programu SQL Server
2. Dysk pliku stronicowania

###<a name="excluding-the-sql-server-tempdb-disk"></a>Wykluczenie dysku bazy danych tempdb programu SQL Server
Rozważmy maszynę wirtualną programu SQL Server z bazą danych tempdb, którą można wykluczyć.

Nazwa maszyny wirtualnej: dyski bazy danych SalesDB na źródłowej maszynie wirtualnej:


**Nazwa dysku** | **Numer dysku systemu operacyjnego gościa** | **Litera dysku** | **Typ danych na dysku**
--- | --- | --- | ---
DB-Disk0-OS | DYSK0 | C:\ | Dysk systemu operacyjnego
DB-Disk1| Dysk1 | D:\ | Systemowa baza danych SQL i baza danych użytkownika&1;
DB-Disk2 (wykluczono dysk z ochrony) | Dysk2 | E:\ | Pliki tymczasowe
DB-Disk3 (wykluczono dysk z ochrony) | Dysk3 | F:\ | Baza danych SQL tempdb — ścieżka folderu (F:\MSSQL\Data\) --> zanotuj ścieżkę folderu przed przejściem do trybu failover
DB-Disk4 | Dysk4 |G:\ |Baza danych użytkownika&2;

Ponieważ współczynnik zmian danych na dwóch dyskach maszyny wirtualnej jest z natury zmienny, podczas ochrony maszyny wirtualnej bazy danych SalesDB wyklucz „Dysk2” i „Dysk3” z replikacji. Usługa Azure Site Recovery nie będzie replikować tych dysków, a w trybie failover te dyski nie będą obecne na maszynie wirtualnej w trybie failover na platformie Azure.

Dyski na maszynie wirtualnej platformy Azure po przejściu do trybu failover:

**Numer dysku systemu operacyjnego gościa** | **Litera dysku** | **Typ danych na dysku**
--- | --- | ---
DYSK0 |    C:\ | Dysk systemu operacyjnego
Dysk1 |    E:\ | Magazyn tymczasowy [platforma Azure dodaje ten dysk i przypisuje mu pierwszą dostępną literę dysku]
Dysk2 | D:\ | Systemowa baza danych SQL i baza danych użytkownika&1;
Dysk3 | G:\ | Baza danych użytkownika&2;

Ponieważ Dysk2 i Dysk3 zostały wykluczone z maszyny wirtualnej bazy danych SalesDB, E: jest pierwszą dostępną literą dysku. Platforma Azure przypisuje literę E: woluminowi magazynu tymczasowego. Litery dysków wszystkich replikowanych dysków pozostają bez zmian.

Dysk3, który był dyskiem bazy danych SQL tempdb (ścieżka folderu bazy danych tempdb F:\MSSQL\Data\) i został wykluczony z replikacji, nie jest dostępny na maszynie wirtualnej w trybie failover. W efekcie usługa SQL przechodzi w stan zatrzymania i potrzebuje ścieżki F:\MSSQL\Data.

Można utworzyć tę ścieżkę na dwa sposoby.

1. Dodaj nowy dysk i przypisz ścieżkę folderu bazy danych tempdb lub
2. Użyj istniejącego dysku magazynu tymczasowego na potrzeby ścieżki folderu bazy danych tempdb

####<a name="add-a-new-disk"></a>Dodaj nowy dysk:

1. Przed przejściem do trybu failover zanotuj ścieżkę plików bazy danych SQL tempdb.mdf i tempdb.ldf.
2. Z poziomu witryny Azure Portal dodaj nowy dysk do maszyny wirtualnej w trybie failover o takim samym lub większym rozmiarze jak źródłowy dysk bazy danych SQL tempdb (Dysk3).
3. Zaloguj się do maszyny wirtualnej platformy Azure. Z poziomu konsoli zarządzania dyskami (diskmgmt.msc) zainicjuj i sformatuj nowo dodany dysk.
4. Przypisz tę samą literę dysku, która była przypisana do dysku bazy danych SQL tempdb (F:).
5. Utwórz folder bazy danych tempdb na woluminie F: (F:\MSSQL\Data).
6. Uruchom usługę SQL z poziomu konsoli usługi.

####<a name="use-existing-temporary-storage-disk-for-sql-tempdb-folder-path"></a>Użyj istniejącego dysku magazynu tymczasowego na potrzeby ścieżki folderu bazy danych SQL tempdb:

1. Otwórz konsolę wiersza polecenia.
2. Z poziomu konsoli wiersza polecenia uruchom serwer SQL w trybie odzyskiwania.

        Net start MSSQLSERVER /f / T3608

3. Uruchom następujące polecenie sqlcmd, aby zmienić ścieżkę bazy danych tempdb na nową ścieżkę.

        sqlcmd -A -S SalesDB        **Use your SQL DBname**
        USE master;        
        GO        
        ALTER DATABASE tempdb        
        MODIFY FILE (NAME = tempdev, FILENAME = 'E:\MSSQL\tempdata\tempdb.mdf');
        GO        
        ALTER DATABASE tempdb        
        MODIFY FILE (NAME = templog, FILENAME = 'E:\MSSQL\tempdata\templog.ldf');        
        GO


4. Zatrzymaj usługę programu Microsoft SQL Server.

        Net stop MSSQLSERVER
5. Uruchom usługę programu Microsoft SQL Server.

        Net start MSSQLSERVER

Zapoznaj się z następującymi wskazówkami dla platformy Azure dotyczącymi dysku magazynu tymczasowego:

* Przechowywanie bazy danych TempDB programu SQL Server i rozszerzeń puli buforów przy użyciu dysków SSD na platformie Azure
* Najlepsze rozwiązania w zakresie wydajności dla programu SQL Server w usłudze Azure Virtual Machines

###<a name="failback-from-azure-to-on-premises"></a>Powrót po awarii (z platformy Azure do zasobów lokalnych)
Teraz omówimy, które dyski zostaną zreplikowane po przejściu do trybu failover z platformy Azure do lokalnych maszyn wirtualnych VMware lub hosta funkcji Hyper-V. Dyski utworzone ręcznie na platformie Azure nie będą replikowane. Jeśli na przykład przeniesiesz trzy dyski do trybu failover i utworzysz dwa bezpośrednio na maszynie wirtualnej platformy Azure, powrót po awarii nastąpi tylko dla trzech dysków przeniesionych do trybu failover. Dysków tworzonych ręcznie nie można uwzględnić podczas powrotu po awarii lub ponownego włączania ochrony z zasobów lokalnych do platformy Azure. Dysk magazynu tymczasowego także nie będzie replikowany do zasobów lokalnych.

####<a name="failback-to-original-location-recovery-olr"></a>Powrót po awarii do odzyskiwania oryginalnej lokalizacji (OLR, original location recovery)

Konfiguracja dysku maszyny wirtualnej platformy Azure w powyższym przykładzie:

**Numer dysku systemu operacyjnego gościa** | **Litera dysku** | **Typ danych na dysku** 
--- | --- | --- 
DYSK0 | C:\ | Dysk systemu operacyjnego
Dysk1 |    E:\ | Magazyn tymczasowy [platforma Azure dodaje ten dysk i przypisuje mu pierwszą dostępną literę dysku]
Dysk2 |    D:\ | Systemowa baza danych SQL i baza danych użytkownika&1;
Dysk3 |    G:\ | Baza danych użytkownika&2;


####<a name="vmware-to-azure"></a>Z programu VMware do platformy Azure
Po powrocie po awarii do oryginalnej lokalizacji konfiguracja dysku maszyny wirtualnej powrotu po awarii nie zawiera wykluczonego dysku. To znaczy, że dyski wykluczone z replikacji z programu VMware do platformy Azure nie będą dostępne na maszynie wirtualnej powrotu po awarii. 

Dyski na maszynie wirtualnej programu VMware (oryginalna lokalizacja) po zaplanowanym powrocie po awarii z platformy Azure do lokalnych zasobów programu VMware:

**Numer dysku systemu operacyjnego gościa** | **Litera dysku** | **Typ danych na dysku** 
--- | --- | --- 
DYSK0 | C:\ | Dysk systemu operacyjnego
Dysk1 |    D:\ | Systemowa baza danych SQL i baza danych użytkownika&1;
Dysk2 |    G:\ | Baza danych użytkownika&2;

####<a name="hyper-v-to-azure"></a>Z funkcji Hyper-V do platformy Azure
Po zakończeniu powrotu po awarii do oryginalnej lokalizacji konfiguracja dysku maszyny wirtualnej powrotu po awarii pozostaje taka sama jak w przypadku oryginalnej konfiguracji dysku maszyny wirtualnej dla funkcji Hyper-V. To znaczy, że dyski wykluczone z replikacji z lokacji funkcji Hyper-V do platformy Azure będą dostępne na maszynie wirtualnej powrotu po awarii.

Dyski na maszynie wirtualnej funkcji Hyper-V (oryginalna lokalizacja) po zaplanowanym powrocie po awarii z platformy Azure do lokalnych zasobów funkcji Hyper-V:

**Nazwa dysku** | **Numer dysku systemu operacyjnego gościa** | **Litera dysku** | **Typ danych na dysku**
--- | --- | --- | ---
DB-Disk0-OS | DYSK0 |    C:\ | Dysk systemu operacyjnego
DB-Disk1 | Dysk1 | D:\ | Systemowa baza danych SQL i baza danych użytkownika&1;
DB-Disk2 (dysk wykluczony) | Dysk2 | E:\ | Pliki tymczasowe
DB-Disk3 (dysk wykluczony) | Dysk3 | F:\ | Baza danych SQL tempdb — ścieżka folderu (F:\MSSQL\Data\)
DB-Disk4 | Dysk4 | G:\ | Baza danych użytkownika&2;


####<a name="exclude-paging-file-disk"></a>Wykluczenie dysku pliku stronicowania

Rozważmy maszynę wirtualną zawierającą dysk z plikiem stronicowania, który można wykluczyć.
Są dwa przypadki:

####<a name="case-1-pagefile-is-configured-on-the-d-drive"></a>Przypadek 1. Plik stronicowania skonfigurowano na dysku D:
Konfiguracja dysku:


**Nazwa dysku** | **Numer dysku systemu operacyjnego gościa** | **Litera dysku** | **Typ danych na dysku**
--- | --- | --- | ---
DB-Disk0-OS | DYSK0 | C:\ | Dysk systemu operacyjnego
DB-Disk1 (wykluczono dysk z ochrony) | Dysk1 | D:\ | pagefile.sys
DB-Disk2 | Dysk2 | E:\ | Dane użytkowników 1
DB-Disk3 | Dysk3 | F:\ | Dane użytkowników 2

Ustawienia pliku stronicowania na źródłowej maszynie wirtualnej:

![Włączanie replikacji](./media/site-recovery-exclude-disk/pagefile-on-d-drive-sourceVM.png)
    

Dyski na maszynie wirtualnej platformy Azure po przełączeniu maszyny wirtualnej w tryb failover z programu VMware do platformy Azure lub z funkcji Hyper-V do platformy Azure:
**Nazwa dysku** | **Numer dysku systemu operacyjnego gościa** | **Litera dysku** | **Typ danych na dysku**
--- | --- | --- | ---
DB-Disk0-OS | DYSK0 | C:\ | Dysk systemu operacyjnego
DB-Disk1 | Dysk1 | D:\ | Magazyn tymczasowy —> pagefile.sys
DB-Disk2 | Dysk2 | E:\ | Dane użytkowników 1
DB-Disk3 | Dysk3 | F:\ | Dane użytkowników 2

Ponieważ Dysk1 (D:) został wykluczony i D: jest pierwszą dostępną literą dysku, platforma Azure przypisuje literę D: do woluminu magazynu tymczasowego.  Ponieważ litera D: jest dostępna na maszynie wirtualnej platformy Azure, ustawienie pliku stronicowania maszyny wirtualnej pozostaje bez zmian.

Ustawienia pliku stronicowania na maszynie wirtualnej platformy Azure:

![Włączanie replikacji](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover.png)

####<a name="case-2-pagefile-file-is-configured-on-any-other-driveother-than-d-drive"></a>Przypadek 2. Plik stronicowania skonfigurowano na dowolnym innym dysku (innym niż D:)

Konfiguracja dysku źródłowej maszyny wirtualnej:

**Nazwa dysku** | **Numer dysku systemu operacyjnego gościa** | **Litera dysku** | **Typ danych na dysku**
--- | --- | --- | ---
DB-Disk0-OS | DYSK0 | C:\ | Dysk systemu operacyjnego
DB-Disk1 (wykluczono dysk z ochrony) | Dysk1 | G:\ | pagefile.sys
DB-Disk2 | Dysk2 | E:\ | Dane użytkowników 1
DB-Disk3 | Dysk3 | F:\ | Dane użytkowników 2

Ustawienia pliku stronicowania na lokalnej maszynie wirtualnej:

![Włączanie replikacji](./media/site-recovery-exclude-disk/pagefile-on-g-drive-sourceVM.png)

Dyski na maszynie wirtualnej platformy Azure po przełączeniu maszyny wirtualnej w tryb failover z programu VMware / funkcji Hyper-V do platformy Azure:

**Nazwa dysku**| **Numer dysku systemu operacyjnego gościa**| **Litera dysku** | **Typ danych na dysku**
--- | --- | --- | ---
DB-Disk0-OS | DYSK0  |C:\ |Dysk systemu operacyjnego
DB-Disk1 | Dysk1 | D:\ | Magazyn tymczasowy —> pagefile.sys
DB-Disk2 | Dysk2 | E:\ | Dane użytkowników 1
DB-Disk3 | Dysk3 | F:\ | Dane użytkowników 2

Ponieważ litera D: jest pierwszą dostępną literą dysku, platforma Azure przypisuje literę D: do woluminu magazynu tymczasowego. Litery dysków wszystkich replikowanych dysków pozostają bez zmian. Ponieważ dysk G: nie jest dostępny, system użyje dysku C: dla pliku stronicowania.

Ustawienia pliku stronicowania na maszynie wirtualnej platformy Azure:

![Włączanie replikacji](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover-2.png)

## <a name="next-steps"></a>Następne kroki
Po skonfigurowaniu i uruchomieniu wdrożenia [dowiedz się więcej](site-recovery-failover.md) o różnych typach trybu failover.



<!--HONumber=Feb17_HO3-->


