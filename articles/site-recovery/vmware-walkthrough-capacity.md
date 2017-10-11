---
title: "Planowanie wydajności i skalowania dla replikacji maszyn wirtualnych VMware do platformy Azure z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Użyj w tym artykule Planowanie wydajności i skalowania podczas replikowania maszyn wirtualnych VMware do platformy Azure z usługą Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: rayne
ms.openlocfilehash: f5b334e594e3d002e1862b25c4faba7163efa7d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-vmware-to-azure-replication"></a>Krok 3: Planowanie pojemności i skalowanie VMware do platformy Azure replikacji

Użyj w tym artykule, aby zorientować się planowanie pojemności i skalowanie podczas replikowania lokalnych maszyn wirtualnych VMware i serwerów fizycznych do platformy Azure z [usługi Azure Site Recovery](site-recovery-overview.md).

Opublikuj komentarze i pytania w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="how-do-i-start-capacity-planning"></a>Jak rozpocząć planowanie pojemności

Zbieranie informacji o środowisku replikacji, a następnie planowania pojemności, korzystając z tych informacji, wraz z uwagi wyróżniane w tym artykule.


## <a name="gather-information"></a>Zbieranie informacji

1. Pobierz [narzędzie wdrożenia Planistę](https://aka.ms/asr-deployment-planner) dla replikacji maszyn wirtualnych VMware.
2. [Przeczytaj ten artykuł](site-recovery-deployment-planner.md) zrozumienie, jak uruchomić narzędzie.
3. Za pomocą narzędzia zbierać informacje o zgodnych i niezgodnych maszyn wirtualnych, dysków dla maszyny Wirtualnej, a danych churn — na dysku. Narzędzie obejmuje również wymagania dotyczące przepustowości sieci i infrastruktury platformy Azure wymagane do pomyślnego replikacji i testowania trybu failover.

## <a name="replication-considerations"></a>Uwagi dotyczące replikacji

Przed rozpoczęciem wdrażania należy zwrócić uwagę następujące zagadnienia:

**Składnik** | **Szczegóły** |
--- | --- | ---
**Replikacja** | **Maksymalna szybkość codziennych zmian:** chronionej maszyny można używać tylko jednego serwera przetwarzania i serwer pojedynczego procesu może obsłużyć dziennym zmiany szybkości maksymalnie 2 TB. W związku z tym 2 TB jest maksymalną codziennych danych zmienić szybkość, z której jest obsługiwana dla komputera chronionego.<br/><br/> **Maksymalna przepustowość:** zreplikowanej maszyny mogą należeć do jednego konta magazynu na platformie Azure. Konto magazynu w warstwie standardowa może obsługiwać maksymalnie 20 000 żądań na sekundę, i zaleca się utrzymać liczby operacji wejścia/wyjścia na sekundę (IOPS) dla maszyny źródłowej do 20 000. Na przykład jeśli masz maszyny źródłowej z dyskami 5, a każdy dysk generuje 120 IOPS (rozmiarze 8 KB) na maszynie źródłowej, następnie będzie w obrębie platformy Azure limitu IOPS dysku 500. (Liczba kont magazynu wymagana jest równa maszyny źródłowej całkowita IOPS, podzielona przez 20 000).

## <a name="configuration-server-capacity"></a>Pojemność serwera konfiguracji

Serwer konfiguracji powinno być możliwe do obsługi codziennych pojemności szybkość zmian we wszystkich obciążeń uruchomionych na chronionych komputerach i musi wystarczającą przepustowość do ciągłej replikacji danych do usługi Azure Storage.

Najlepszym rozwiązaniem zlokalizować serwera konfiguracji na tej samej sieci, a segment sieci LAN jako maszyn, które chcesz chronić. Może znajdować się w innej sieci, ale maszyny, które chcesz chronić powinny mieć widoczność warstwy 3 sieci do niego.

## <a name="sizing-recommendations"></a>Zalecenia dotyczące zmiany rozmiaru

W tabeli przedstawiono zalecenia dotyczące rozmiaru oparte na Procesorze.

**PROCESOR CPU** | **Pamięci** | **Rozmiar pamięci podręcznej dysku** | **Częstotliwość zmian danych** | **Chronione maszyny**
--- | --- | --- | --- | ---
8 Vcpu (2 sockets * 4 rdzenie @ 2,5 GHz [GHz]) | 16 GB | 300 GB | 500 GB lub mniej | Replikowanie maszyn mniej niż 100.
12 Vcpu (2 sockets * 6 rdzeni @ 2,5 GHz) | 18 GB | 600 GB | 500 GB do 1 TB | Replikują między 100 150 maszyn.
16 Vcpu (2 sockets * 8 rdzeni @ 2,5 GHz) | 32 GB | 1 TB | 1 TB do 2 TB | Replikują między 150 – 200 maszyn.
Wdrażanie inny serwer przetwarzania | | | > 2 TB | Wdrażanie serwerów dodatkowych procesów, Jeśli replikujesz ponad 200 maszyn lub zmiana codziennych danych szybkość przekracza 2 TB.

Gdzie:

* Każda maszyna źródłowa jest skonfigurowany z 3 dyski 100 GB.
* My używamy najlepszymi magazynu 8 dysków SAS 10 k obr. / min, z RAID 10 dla pamięci podręcznej dysku pomiarów.

## <a name="process-server-capacity"></a>Pojemność serwera przetwarzania


Serwer przetwarzania odbiera dane replikacji z chronionych maszyn i optymalizuje je przy użyciu pamięci podręcznej, kompresji i szyfrowania. Następnie wysyła dane na platformie Azure.

- Proces maszyny serwera ma wystarczające zasoby, aby wykonać te zadania.
- Pierwszy serwer przetwarzania jest instalowany domyślnie na serwerze konfiguracji. Można wdrożyć serwery dodatkowych procesów do skalowania środowiska.
- Serwer przetwarzania korzysta z pamięci podręcznej opartej na dysku. Użyj dysku pamięci podręcznej oddzielne 600 GB lub więcej do obsługi zmian danych przechowywanych w przypadku awarii lub wąskich gardeł.
- Jeśli musisz ochrony ponad 200 maszyn lub szybkość codziennych zmian jest większa niż 2 TB, można dodać procesu serwerów do obsługi obciążenia replikacji. Aby skalować w poziomie, można:
    - Zwiększ liczbę serwerów konfiguracji. Na przykład można chronić maksymalnie 400 maszyn przy użyciu dwóch serwerów konfiguracji.
    - Dodawanie kolejnych serwerów procesu i ich używać do obsługi ruchu zamiast (lub oprócz) serwera konfiguracji.


### <a name="example-process-server-scaling"></a>Przykład procesu serwera skalowania

W poniższej tabeli opisano scenariusz, w którym:

* Nie planujesz użyć konfiguracji serwera jako serwera przetwarzania.
* Po skonfigurowaniu serwera dodatkowego przetwarzania.
* Skonfigurowano chronionych maszyn wirtualnych do używania serwera dodatkowych procesów.
* Każda maszyna chronionego źródła jest skonfigurowany z trzech dysków 100 GB.

**Serwer konfiguracji** | **Serwer przetwarzania dodatkowe** | **Rozmiar pamięci podręcznej dysku** | **Częstotliwość zmian danych** | **Chronione maszyny**
--- | --- | --- | --- | ---
8 Vcpu (2 sockets * 4 rdzenie @ 2,5 GHz), 16 GB pamięci | 4 Vcpu (2 sockets * 2 rdzenie @ 2,5 GHz), 8 GB pamięci | 300 GB | 250 GB lub mniej | Replikowanie maszyn 85 lub mniej.
8 Vcpu (2 sockets * 4 rdzenie @ 2,5 GHz), 16 GB pamięci | 8 Vcpu (2 sockets * 4 rdzenie @ 2,5 GHz), 12 GB pamięci RAM | 600 GB | 250 GB do 1 TB | Replikują między 85 150 maszyny.
12 Vcpu (2 sockets * 6 rdzeni @ 2,5 GHz), 18 GB pamięci | 12 Vcpu (2 sockets * 6 rdzeni @ 2,5 GHz) 24 GB pamięci | 1 TB | 1 TB do 2 TB | Replikują między 150 225 komputerów.

Sposób, w którym można skalować serwery zależy od swoich preferencji modelu skalowania w górę i skalowania w poziomie.  Skalowanie w górę przez wdrożenie kilku konfiguracje i serwerów procesu lub skalowanie w poziomie przez wdrożenie więcej serwerów z mniejszą liczbą zasobów. Na przykład jeśli musisz chronić 220 maszyny, wykonaj jedną z następujących czynności:

* Konfigurowanie serwera konfiguracji z 12 vCPU, 18 GB pamięci, a serwerem dodatkowych procesów z 12 vCPU, 24 GB pamięci. Skonfiguruj chronione maszyny do korzystania z serwera dodatkowych procesów.
* Skonfiguruj dwa serwery konfiguracji (vCPU, 16 GB pamięci RAM 2 x 8) i dwa serwery dodatkowych procesów (vCPU 1 x 8 i 4 maszyny vCPU x 1 do uchwytu 135 + 85 [220]). Skonfiguruj chronione maszyny, aby korzystały z serwerów dodatkowych procesów.

## <a name="deploy-additional-process-servers"></a>Wdrażanie serwerów dodatkowych procesów

Wykonaj te instrukcje, aby skonfigurować serwer dodatkowych procesów. Po skonfigurowaniu serwera, można dokonać migracji maszyn źródłowych z niego korzystać.

1. W **serwerów usługi Site Recovery**, kliknij serwer konfiguracji > **+ serwera przetwarzania**.
2. W **typ serwera**, kliknij przycisk **serwera przetwarzania (lokalnego)**.

    ![Serwer przetwarzania](./media/vmware-walkthrough-capacity/migrate-ps2.png)
3. Pobierz plik Instalatora Unified usługi Site Recovery.
4. Uruchom Instalatora, aby zainstalować serwer przetwarzania i zarejestruj go w magazynie.
5. W obszarze **Przed rozpoczęciem** wybierz pozycję **Dodaj dodatkowe serwery przetwarzania w celu zwiększenia skali wdrożenia**.
6. W **szczegóły konfiguracji serwera**, podaj adres IP serwera konfiguracji i hasło. Jeśli nie masz hasła go uzyskać, uruchamiając **[SiteRecoveryInstallationFolder]\home\sysystems\bin\genpassphrase.exe – n** na serwerze konfiguracji.

    ![Serwer konfiguracji](./media/vmware-walkthrough-capacity/add-ps2.png)
7. Wykonaj pozostałe ustawienia w taki sam sposób jak w przypadku podczas konfigurowania serwera konfiguracji.

### <a name="migrate-machines-to-use-the-process-server"></a>Migruj maszyny, aby użyć serwera przetwarzania

1. W **ustawienia** > **serwerów usługi Site Recovery**, kliknij serwer konfiguracji > **przetworzyć serwerów**.
2. Kliknij prawym przyciskiem myszy serwer przetwarzania obecnie w użyciu > **przełącznika**.

    ![Serwer przetwarzania przełącznika](./media/vmware-walkthrough-capacity/migrate-ps3.png)
3. W **wybierz docelowy serwer przetwarzania**, wybierz serwer przetwarzania, którego chcesz użyć i wybierz maszyny wirtualne, które będzie obsługiwać serwer.
4. Kliknij ikonę informacji. Pomaga załadować decyzji, zostanie wyświetlony średni ilości miejsca, która jest potrzebna do replikowania każdej wybranej maszyny Wirtualnej do nowego serwera przetwarzania.
5. Kliknij znacznik wyboru, aby rozpocząć replikację na nowym serwerze procesu.

## <a name="control-network-bandwidth"></a>Sterowania przepustowością sieci

Po uruchomieniu [narzędzie wdrożenia Planistę](site-recovery-deployment-planner.md) do obliczania wartości przepustowości wymagane dla replikacji (Replikacja początkowa i następnie delta) można kontrolować, ilość przepustowości używanej w ramach replikacji przy użyciu kilku opcji:

* **Ograniczanie przepustowości**: VMware ruchu, który replikuje Azure przechodzi przez serwer określonego procesu. Możesz ograniczyć przepustowość na maszynach działających jako serwery procesu.
* **Wpływ przepustowości**: może mieć wpływ na przepustowość wykorzystywaną podczas replikacji przy użyciu kilku kluczy rejestru:
  * **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** wartość rejestru określa liczbę wątków, które są używane do transferu danych (Replikacja początkowa lub przyrostowa) dysku. Wyższa wartość zwiększa przepustowość sieciową używaną podczas replikacji.
  * **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** określa liczbę wątków używanych do transferu danych podczas powrotu po awarii.

### <a name="throttle-bandwidth"></a>Ograniczanie przepustowości

1. Otwieranie przystawki MMC kopia zapasowa Azure na komputerze działającym jako serwer przetwarzania. Domyślnie skrót do utworzenia kopii zapasowej jest dostępny na pulpicie lub w następującym folderze: C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. W przystawce kliknij pozycję **Zmień właściwości**.
3. Na **ograniczania** wybierz opcję **włączyć użycia przepustowości połączenia internetowego do operacji tworzenia kopii zapasowej**.
4. Ustaw limity dla pracy oraz innych niż godziny pracy. Prawidłowy zakres: od 512 Kb/s do 102 Mb/s na sekundę.

    ![Ograniczenie](./media/vmware-walkthrough-capacity/throttle2.png)

Możesz też użyć polecenia cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx), aby ustawić ograniczanie przepływności. Oto przykład:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** wskazuje, że ograniczanie przepływności nie jest wymagane.

### <a name="influence-network-bandwidth-for-a-vm"></a>Wpływ na przepustowość sieci dla maszyny Wirtualnej

1. W rejestrze maszyny wirtualnej, przejdź do **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Wpływ na przepustowość ruchu replikacji dysku, zmodyfikuj wartość **UploadThreadsPerVM**, lub Utwórz klucz, jeśli nie istnieje.
   * Wpływ przepustowości dla ruchu powrotu po awarii z platformy Azure, zmodyfikuj wartość **DownloadThreadsPerVM**.
2. Wartość domyślna to 4. W sieci o nadmiarowych zasobach można zmodyfikować te klucze rejestru. Wartość maksymalna to 32. Monitoruj ruch, aby zoptymalizować tę wartość.




## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 4: Planowanie sieci](vmware-walkthrough-network.md).
