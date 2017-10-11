---
title: "Planowanie wydajności i skalowania replikacji maszyny Wirtualnej funkcji Hyper-V (bez VMM) na platformie Azure za pomocą usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Użyj w tym artykule Planowanie wydajności i skalowania podczas replikowania maszyn wirtualnych funkcji Hyper-V do platformy Azure z usługą Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8687af60-5b50-481c-98ee-0750cbbc2c57
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: c7891c188c2cecbbf056fa79672a13bb16fa7fcf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-to-azure-replication"></a>Krok 3: Planowanie wydajności i skalowania dla funkcji Hyper-V do platformy Azure replikacji

Niniejszy artykuł służy zorientować się planowanie pojemności i skalowania, podczas replikacji (bez programu System Center VMM), maszynach wirtualnych funkcji Hyper-V lokalnego do platformy Azure z [usługi Azure Site Recovery](site-recovery-overview.md).

Po przeczytaniu tego artykułu, post wszelkie komentarze u dołu i zadawaj pytania techniczne na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="how-do-i-start-capacity-planning"></a>Jak rozpocząć planowanie pojemności


Zbieranie informacji o środowisku replikacji, a następnie planowania pojemności, korzystając z tych informacji, wraz z uwagi wyróżniane w tym artykule.


## <a name="gather-information"></a>Zbieranie informacji

1. Zebrać informacje o środowisku replikacji, w tym o maszynach wirtualnych, liczbie dysków na maszynę wirtualną oraz pojemności dysków.
2. Określ częstotliwość codziennych zmian (przenoszenia) dla replikowanych danych. Pobierz [narzędzia do planowania pojemności funkcji Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) uzyskać szybkość zmian. Zaleca się uruchomić to narzędzie dłużej niż przez tydzień do przechwytywania średnie.
 

## <a name="figure-out-capacity"></a>Ustalenia pojemności

Na podstawie informacji, które zostały zbieranie, uruchom [Planisty wydajności odzyskiwania lokacji](http://aka.ms/asr-capacity-planner-excel) aby przeanalizować środowiska źródłowego i obciążeń, oszacować wymagania dotyczące przepustowości i zasobów serwera do lokalizacji źródłowej i zasobów (maszyn wirtualnych i magazynu itp.), wymagających w lokalizacji docelowej. Narzędzie można uruchomić na kilka metod:

- Planowanie szybkie: Uruchom narzędzie w tym trybie można pobrać projekcje sieci i serwera oparte na średnich liczbach maszyn wirtualnych, dysków, magazynu i szybkość zmian.
- Szczegółowe planowanie: Uruchom narzędzie w tym trybie i podaj szczegóły poszczególnych obciążeń na poziomie maszyny Wirtualnej. Analizowanie zgodności maszyny Wirtualnej i uzyskać projekcje sieci i serwera.

Teraz uruchom narzędzie:

1. Pobierz [narzędzia](http://aka.ms/asr-capacity-planner-excel)
2. Aby uruchomić szybkie planowania, wykonaj [tych instrukcji](site-recovery-capacity-planner.md#run-the-quick-planner)i wybrać scenariusz **funkcji Hyper-V w systemie Azure**.
3. Aby uruchomić szczegółowe planowania, wykonaj [tych instrukcji](site-recovery-capacity-planner.md#run-the-detailed-planner)i wybrać scenariusz **funkcji Hyper-V w systemie Azure**.

## <a name="control-network-bandwidth"></a>Sterowania przepustowością sieci

Po został obliczony przepustowości, które są potrzebne, masz kilka opcji kontrolowania ilość przepustowości używanej w ramach replikacji:

* **Ograniczanie przepustowości**: ruch funkcji Hyper-V, który replikuje Azure przechodzi przez konkretnego hosta funkcji Hyper-V. Możesz ograniczyć przepustowość na serwerze hosta.
* **Wpływ przepustowości**: może mieć wpływ na przepustowość wykorzystywaną podczas replikacji przy użyciu kilku kluczy rejestru.

### <a name="throttle-bandwidth"></a>Ograniczanie przepustowości
1. Otwórz przystawkę MMC usługi Microsoft Azure Backup na serwerze hosta funkcji Hyper-V. Domyślnie skrót do usługi Microsoft Azure Backup jest dostępny na pulpicie lub w folderze C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. W przystawce kliknij przycisk **Zmień właściwości**.
3. Na karcie **Ograniczanie przepływności** wybierz opcję **Włącz ograniczanie użycia przepustowości internetowej dla operacji kopii zapasowych** i ustaw limity dla godzin roboczych i godzin innych niż godziny pracy. Prawidłowy zakres: od 512 Kb/s do 102 Mb/s na sekundę.

    ![Ograniczanie przepustowości](./media/hyper-v-site-walkthrough-capacity/throttle2.png)

Możesz też użyć polecenia cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx), aby ustawić ograniczanie przepływności. Oto przykład:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** wskazuje, że ograniczanie przepływności nie jest wymagane.

### <a name="influence-network-bandwidth"></a>Wywieranie wpływu na przepustowość sieci
1. W rejestrze przejdź do pozycji **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Wpływ na przepustowość ruchu replikacji dysku, zmodyfikuj wartość **UploadThreadsPerVM**, lub Utwórz klucz, jeśli nie istnieje.
   * Wpływ przepustowości dla ruchu powrotu po awarii z platformy Azure, zmodyfikuj wartość **DownloadThreadsPerVM**.
2. Wartość domyślna to 4. W sieci „o nadmiarowych zasobach” należy zmienić wartości domyślne tych kluczy rejestru. Wartość maksymalna to 32. Monitoruj ruch, aby zoptymalizować tę wartość.

## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 4: Planowanie sieci](hyper-v-site-walkthrough-network.md).
