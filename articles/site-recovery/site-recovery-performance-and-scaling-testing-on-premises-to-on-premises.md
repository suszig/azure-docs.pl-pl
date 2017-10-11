---
title: "Wyniki testu dla funkcji Hyper-V replikacji między lokacjami z usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera informacje o testowanie wydajnościowe na potrzeby lokalnej do lokalnej replikacji maszyn wirtualnych funkcji Hyper-V za pomocą usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 96ff404f-0d88-43fa-a00b-2dffde93d192
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/24/2017
ms.author: raynew
ms.openlocfilehash: a9bec774b5482de87eefcd0c87844a2adbd98bbe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="test-results-for-on-premises-to-on-premises-hyper-v-replication-with-site-recovery"></a>Wyniki testu dla lokalnego do lokalnej replikacji funkcji Hyper-V za pomocą usługi Site Recovery

Microsoft Azure Site Recovery umożliwia organizowanie i zarządzać replikacją maszyn wirtualnych i serwerów fizycznych do platformy Azure lub dodatkowego centrum danych. W tym artykule przedstawiono wyniki testów wydajności, które robiliśmy podczas replikacji maszyn wirtualnych funkcji Hyper-V między dwiema lokalnymi centrami danych.

## <a name="test-goals"></a>Cele testu

Celem badania było zbadać, jak usługi Azure Site Recovery wykonuje podczas replikacji stanie stabilności. Stanie stabilności replikacji występuje, gdy maszyny wirtualne ukończenie replikacji początkowej oraz synchronizacji zmiany różnicowe. Należy do pomiaru wydajności przy użyciu stanie stabilności, ponieważ jest on stan pozostają większość maszyn wirtualnych, chyba że wystąpienia nieoczekiwanych awarii.

Testowe wdrożenie obejmowały dwie lokacje lokalne na serwerze programu VMM w każdej lokacji. Tego wdrożenia testowego jest typowy head/oddział wdrożenia pakietu office, z centrali działający jako lokacja główna i oddział firmy jako lokacji dodatkowej lub odzyskiwania.

## <a name="what-we-did"></a>Firma Microsoft może

Oto, co możemy w teście przeszedł:

1. Utworzone za pomocą programu VMM szablonów maszyn wirtualnych.
2. Uruchomiono maszyny wirtualne i metryk wydajności bazowej przechwytywania niż 12 godzin.
3. Utworzony chmury na serwerach VMM podstawowymi i odzyskiwania.
4. Ochrona chmury skonfigurowany w usłudze Azure Site Recovery, łącznie z mapowaniem chmur źródła i odzyskiwania.
5. Włączyć ochronę maszyn wirtualnych i zezwolić im na dokończenie replikacji początkowej.
6. Oczekiwano kilku godzinach stabilizacji systemu.
7. Przechwytywane metryki wydajności niż 12 godzin, zapewnienie pozostały wszystkich maszyn wirtualnych w nieoczekiwany stan replikacji dla tych 12 godzin.
8. Zmierz różnica między metryki wydajności bazowej i metryk wydajności replikacji.


## <a name="primary-server-performance"></a>Wydajność serwera podstawowego

* Funkcji Hyper-V Replica asynchronicznie śledzi zmiany w pliku dziennika z narzut na przechowywanie minimalną na serwerze podstawowym.
* Repliki funkcji Hyper-V korzysta z własnym zapewnienia pamięci podręcznej, aby zminimalizować liczbę IOPS nakładów pracy dla śledzenia. Przechowuje zapisuje VHDX w pamięci i opróżnia je w pliku dziennika, przed upływem terminu, dziennik jest wysyłane do witryny odzyskiwania. Dysk opróżniania odbywa się, jeśli zapisami osiągnęła limit wcześniej.
* Wykres poniżej przedstawia stanie stabilności obciążenie IOPS dla replikacji. Widać, IOPS nakłady pracy związane z powodu replikacji jest około % 5, który jest bardzo mała.

![Podstawowy wyników](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744913.png)

Repliki funkcji Hyper-V korzysta z pamięci na serwerze podstawowym, aby zoptymalizować wydajność dysku. Jak pokazano na wykresie, pamięć, narzut na wszystkich serwerach w klastrze podstawowego jest brzegowych. Pamięć narzut to wartości procentowej pamięci używana przez funkcję replikacji w porównaniu do całkowitej ilości pamięci zainstalowanych na serwerze funkcji Hyper-V.

![Podstawowy wyników](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744914.png)

Funkcja Hyper-V Replica ma minimalną obciążenia Procesora. Jak pokazano na wykresie, obciążenia związanego z replikacją jest z zakresu od 2 do 3%.

![Podstawowy wyników](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744915.png)

## <a name="secondary-recovery-server-performance"></a>Wydajność serwera pomocniczym (odzyskiwania)

Repliki funkcji Hyper-V używa mała ilość pamięci na serwerze odzyskiwania w celu optymalizowania liczby operacji magazynu. Wykres zawiera podsumowanie użycia pamięci na serwerze odzyskiwania. Pamięć narzut to wartości procentowej pamięci używana przez funkcję replikacji w porównaniu do całkowitej ilości pamięci zainstalowanych na serwerze funkcji Hyper-V.

![Wyniki dodatkowej](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744916.png)

Liczba operacji We/Wy w lokacji odzyskiwania jest funkcją liczbę operacji zapisu w lokacji głównej. Załóżmy przyjrzeć się całkowita liczba operacji We/Wy w lokacji odzyskiwania w porównaniu z całkowita liczba operacji We/Wy i zapisu w lokacji głównej. Wykresy pokazują, że łączną liczbę IOPS w lokacji odzyskiwania jest

* Około 1,5 raza zapisu IOPS na serwerze podstawowym.
* Około 37% całkowitej IOPS w lokacji głównej.

![Wyniki dodatkowej](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744917.png)

![Wyniki dodatkowej](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744918.png)

## <a name="effect-on-network-utilization"></a>Wpływ na wykorzystanie sieci

Średnia 275 Mb na sekundę przepustowości sieci zostało użyte między węzłami podstawowymi i odzyskiwania (z włączoną kompresją) względem istniejących przepustowości 5 Gb na sekundę.

![Wykorzystanie sieci wyników](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744919.png)

## <a name="effect-on-vm-performance"></a>Wpływ na wydajność maszyny Wirtualnej

Ważną kwestią jest jego wpływ na replikację w przypadku obciążeń produkcyjnych uruchomionych na maszynach wirtualnych. Jeśli w lokacji głównej można ją było właściwie jest przeznaczona do replikacji, brak nie powinny być ich wpływ na obciążenie. Lightweight funkcji Hyper-V Replica śledzenia mechanizm zapewnia nie dotyczy obciążeń uruchomionych na maszynach wirtualnych podczas replikacji stanie stabilności. Jest to zilustrowane na poniższych wykresach.

Ten wykres pokazuje liczbę IOPS wykonywane przez różnych obciążeń przed maszynami wirtualnymi, po włączeniu replikacji. Można obserwować, czy nie ma żadnej różnicy między nimi.

![Wyniki efekt repliki](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744920.png)

Wykres ukazuje przepływności maszyn wirtualnych uruchomionych w różnych obciążeń przed i po włączeniu replikacji. Można obserwować, czy replikacja nie ma znaczący wpływu.

![Efekty repliki wyników](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744921.png)

## <a name="conclusion"></a>Podsumowanie

Wyraźnie wyniki wskazują, czy usługi Azure Site Recovery, w połączeniu z funkcji Hyper-V Replica skaluje się z co najmniej w czasie dużych klastra.  Usługa Azure Site Recovery zawiera proste wdrożenie, replikacji, zarządzania i monitorowania. Repliki funkcji Hyper-V udostępnia infrastrukturę niezbędne zakończona pomyślnie replikacja skalowania. Do planowania wdrożenia optymalne Sugerujemy, możesz pobrać [funkcji Hyper-V Replica Capacity Planner](https://www.microsoft.com/download/details.aspx?id=39057).

## <a name="test-environment-details"></a>Szczegóły środowiska testowego

### <a name="primary-site"></a>Lokacja główna

* Lokacja główna ma klastrze zawierającym pięć serwerów funkcji Hyper-V uruchomione maszyny wirtualne 470.
* Maszyny wirtualne są uruchamiane różnych obciążeń i mieć włączone ochrony Azure Site Recovery.
* Magazyn dla węzła klastra jest zapewniana przez sieć SAN iSCSI. Model — Hitachi HUS130.
* Każdy serwer klastra ma cztery karty sieciowe (NIC) z jedną GB.
* Dwie karty sieciowe są podłączone do sieci prywatnej iSCSI i dwa są podłączone do sieci zewnętrznej przedsiębiorstwa. Jedną z sieci zewnętrznej jest zarezerwowana dla tylko na komunikację klastra.

![Podstawowe wymagania sprzętowe](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744922.png)

| Serwer | Pamięć RAM | Model | Procesor | Liczba procesorów | KARTA SIECIOWA | Oprogramowanie |
| --- | --- | --- | --- | --- | --- | --- |
| Serwery funkcji Hyper-V w klastrze: <br />ESTLAB HOST11<br />ESTLAB HOST12<br />ESTLAB HOST13<br />ESTLAB HOST14<br />ESTLAB HOST25 |128ESTLAB HOST25 ma 256 |R820 firmy Dell PowerEdge™ |Intel(R) Xeon(R) E5 procesora CPU — 4620 0 @ 2,20 GHz |4 |I GB/s x 4 |Windows Server Datacenter 2012 R2 (x64) + roli funkcji Hyper-V |
| Serwer programu VMM |2 | | |2 |1 Gb/s |Windows Server bazy danych 2012 R2 (x 64) + VMM 2012 R2 |

### <a name="secondary-recovery-site"></a>Lokacja dodatkowa (odzyskiwania)

* Lokacja dodatkowa ma klastra pracy awaryjnej sześciu węzłów.
* Magazyn dla węzła klastra jest zapewniana przez sieć SAN iSCSI. Model — Hitachi HUS130.

![Specyfikacja podstawowy sprzęt](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744923.png)

| Serwer | Pamięć RAM | Model | Procesor | Liczba procesorów | KARTA SIECIOWA | Oprogramowanie |
| --- | --- | --- | --- | --- | --- | --- |
| Serwery funkcji Hyper-V w klastrze: <br />ESTLAB HOST07<br />ESTLAB HOST08<br />ESTLAB HOST09<br />ESTLAB HOST10 |96 |R720 firmy Dell PowerEdge™ |Intel(R) Xeon(R) E5 procesora CPU — 2630 0 @ 2.30GHz |2 |I GB/s x 4 |Windows Server Datacenter 2012 R2 (x64) + roli funkcji Hyper-V |
| ESTLAB HOST17 |128 |R820 firmy Dell PowerEdge™ |Intel(R) Xeon(R) E5 procesora CPU — 4620 0 @ 2,20 GHz |4 | |Windows Server Datacenter 2012 R2 (x64) + roli funkcji Hyper-V |
| ESTLAB HOST24 |256 |R820 firmy Dell PowerEdge™ |Intel(R) Xeon(R) E5 procesora CPU — 4620 0 @ 2,20 GHz |2 | |Windows Server Datacenter 2012 R2 (x64) + roli funkcji Hyper-V |
| Serwer programu VMM |2 | | |2 |1 Gb/s |Windows Server bazy danych 2012 R2 (x 64) + VMM 2012 R2 |

### <a name="server-workloads"></a>Obciążenie serwera

* Do celów testowych możemy pobrać obciążeń często używane w scenariuszach dla przedsiębiorstw klienta.
* Używamy [IOMeter](http://www.iometer.org) z cech obciążenia podsumowane w tabeli na symulacyjnych.
* Wszystkie profile IOMeter są ustawione na zapis losowych bajtów do symulowania najgorszych zapisu wzorce dla obciążeń.

| Obciążenie | We/Wy rozmiar (KB) | % Dostępu | % Odczytu | Oczekujące operacje We/Wy | Wzorzec operacji We/Wy |
| --- | --- | --- | --- | --- | --- |
| Serwer plików |48163264 |60%20%5%5%10% |80%80%80%80%80% |88888 |Wszystkie 100% losowych |
| SQL Server (woluminu 1) programu SQL Server (woluminu 2) |864 |100%100% |70%0% |88 |100% random100% sekwencyjne |
| Exchange |32 |100% |67% |8 |losowe 100% |
| Stacja robocza/VDI |464 |66%34% |70%95% |11 |Losowe zarówno 100% |
| Serwer plików w sieci Web |4864 |33%34%33% |95%95%95% |888 |Wszystkie 75% losowych |

### <a name="vm-configuration"></a>Konfiguracja maszyny Wirtualnej

* 470 maszyny wirtualne w klastrze podstawowego.
* Wszystkie maszyny wirtualne z dysku VHDX.
* Maszyny wirtualne z obciążeniami podsumowane w tabeli. Wszystkie zostały utworzone szablony programu VMM.

| Obciążenie | # Maszyny wirtualne | Minimalna ilość pamięci RAM (GB) | Maksymalna ilość pamięci RAM (GB) | Rozmiar dysku logicznego (GB) dla maszyny Wirtualnej | Maksymalna liczba IOPS |
| --- | --- | --- | --- | --- | --- |
| Oprogramowanie SQL Server |51 |1 |4 |167 |10 |
| Exchange Server |71 |1 |4 |552 |10 |
| Serwer plików |50 |1 |2 |552 |22 |
| VDI |149 |.5 |1 |80 |6 |
| Serwer sieci Web |149 |.5 |1 |80 |6 |
| CAŁKOWITA LICZBA |470 | | |96.83 TB |4108 |

### <a name="site-recovery-settings"></a>Ustawienia odzyskiwania lokacji

* Usługa Azure Site Recovery został skonfigurowany do środowiska lokalnego do ochrony lokalnej
* Serwer VMM ma cztery skonfigurowaną chmurę, zawierającą serwery w klastrze funkcji Hyper-V i maszynami wirtualnymi.

| Podstawowy chmury VMM | Chronione maszyny wirtualne w chmurze | Częstotliwość replikacji | Dodatkowe punkty odzyskiwania |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 minut. |Brak |
| PrimaryCloudRpo30s |47 |30 sekund |Brak |
| PrimaryCloudRpo30sArp1 |47 |30 sekund |1 |
| PrimaryCloudRpo5m |235 |5 minut. |Brak |

### <a name="performance-metrics"></a>Metryki wydajności

W tabeli przedstawiono metryki wydajności i liczniki, które zostały zmierzone w ramach wdrożenia.

| Metryka | Licznik |
| --- | --- |
| Procesor CPU |\Processor(_Total)\% Processor Time |
| Dostępna pamięć |\Memory\Available (MB) |
| Operacje wejścia/wyjścia |Transfery \Disk \PhysicalDisk (_Total) na sekundę |
| Maszyna wirtualna (IOPS) operacje odczytu/s |\Hyper-V wirtualne urządzenie magazynujące (<VHD>) \Read operacji na sekundę |
| Operacje zapisu (IOPS) maszyny Wirtualnej na sekundę |\Hyper-V wirtualne urządzenie magazynujące (<VHD>) \Write operacji/S |
| Przepływność odczytu dla maszyny Wirtualnej |\Hyper-V wirtualne urządzenie magazynujące (<VHD>) \Read bajty/s |
| Przepływność zapisu maszyny Wirtualnej |\Hyper-V wirtualne urządzenie magazynujące (<VHD>) \Write bajty/s |

## <a name="next-steps"></a>Następne kroki

[Konfigurowanie replikacji między dwiema lokacjami programu VMM lokalnie](site-recovery-vmm-to-vmm.md)
