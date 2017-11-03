---
title: "Obliczyć wyniki testów porównawczych dla maszyn wirtualnych systemu Linux | Dokumentacja firmy Microsoft"
description: "Porównaj wyniki testu porównawczego obliczeń CoreMark dla maszyn wirtualnych Azure systemem Linux"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 93e812c1-79dd-40c5-b97b-aa79f5cd7d76
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: cynthn
ms.openlocfilehash: 3c7b34652ea4c9340b9fe7f6ada3f9992642aeac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="compute-benchmark-scores-for-linux-vms"></a>Obliczenia bazy danych wyników testów porównawczych dla maszyn wirtualnych systemu Linux
Następujące wyniki testów porównawczych CoreMark Pokaż wydajności obliczeniowej dla zestawienia maszyny Wirtualnej platformy Azure wysokiej wydajności z systemem Ubuntu. Wyniki testu porównawczego obliczeniowe są także dostępne dla [maszyn wirtualnych systemu Windows](../windows/compute-benchmark-scores.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="a-series---compute-intensive"></a>A-series obliczeniowych
| Rozmiar | Vcpu | Węzły NUMA | Procesor CPU | Uruchamia | Iteracje na sekundę | StdDev |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8 |8 |1 |Procesor Intel Xeon E5 procesora CPU — 2670 0 @ 2.6 GHz |179 |110,294 |554 |
| Standard_A9 |16 |2 |Procesor Intel Xeon E5 procesora CPU — 2670 0 @ 2.6 GHz |189 |210,816 |2,126 |
| Standardowa_A10 |8 |1 |Procesor Intel Xeon E5 procesora CPU — 2670 0 @ 2.6 GHz |188 |110,025 |1,045 |
| Standardowa_A11 |16 |2 |Procesor Intel Xeon E5 procesora CPU — 2670 0 @ 2.6 GHz |188 |210,727 |2,073 |

## <a name="dv2-series"></a>Seria Dv2
| Rozmiar | Vcpu | Węzły NUMA | Procesor CPU | Uruchamia | Iteracje na sekundę | StdDev |
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_D1_v2 |1 |1 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |140 |14,852 |780 |
| Standardowa_D2_v2 |2 |1 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |133 |29,467 |1,863 |
| Standardowa_D3_v2 |4 |1 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |139 |56,205 |1,167 |
| Standardowa_D4_v2 |8 |1 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |126 |108,543 |3,446 |
| Standardowa_D5_v2 |16 |2 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |126 |205,332 |9,998 |
| Standardowa_D11_v2 |2 |1 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |125 |28,598 |1,510 |
| Standardowa_D12_v2 |4 |1 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |131 |55,673 |1,418 |
| Standardowa_D13_v2 |8 |1 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |140 |107,986 |3,089 |
| Standardowa_D14_v2 |16 |2 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |140 |208,186 |8,839 |
| Standard_D15_v2 |20 |2 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |28 |268,560 |4,667 |

## <a name="f-series"></a>Seria F
| Rozmiar | Vcpu | Węzły NUMA | Procesor CPU | Uruchamia | Iteracje na sekundę | StdDev |
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_F1 |1 |1 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |154 |15,602 |787 |
| Standardowa_F2 |2 |1 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |126 |29,519 |1,233 |
| Standardowa_F4 |4 |1 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |147 |58,709 |1,227 |
| Standardowa_F8 |8 |1 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |224 |112,772 |3,006 |
| Standardowa_F16 |16 |2 |Procesor Intel Xeon E5-2673 v3 @ 2,4 GHz |42 |218,571 |5,113 |

## <a name="g-series"></a>Seria G
| Rozmiar | Vcpu | Węzły NUMA | Procesor CPU | Uruchamia | Iteracje na sekundę | StdDev |
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_G1 |2 |1 |Procesor Intel Xeon E5-2698B v3 @ 2 GHz |83 |31,310 |2,891 |
| Standardowa_G2 |4 |1 |Procesor Intel Xeon E5-2698B v3 @ 2 GHz |84 |60,112 |3,537 |
| Standardowa_G3 |8 |1 |Procesor Intel Xeon E5-2698B v3 @ 2 GHz |84 |107,522 |4,537 |
| Standardowa_G4 |16 |1 |Procesor Intel Xeon E5-2698B v3 @ 2 GHz |83 |195,116 |5,024 |
| Standard_G5 |32 |2 |Procesor Intel Xeon E5-2698B v3 @ 2 GHz |84 |360,329 |14,212 |

## <a name="gs-series"></a>Seria GS
| Rozmiar | Vcpu | Węzły NUMA | Procesor CPU | Uruchamia | Iteracje na sekundę | StdDev |
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_GS1 |2 |1 |Procesor Intel Xeon E5-2698B v3 @ 2 GHz |84 |28,613 |1,884 |
| Standardowa_GS2 |4 |1 |Procesor Intel Xeon E5-2698B v3 @ 2 GHz |83 |54,348 |3,474 |
| Standardowa_GS3 |8 |1 |Procesor Intel Xeon E5-2698B v3 @ 2 GHz |83 |104,564 |1,834 |
| Standardowa_GS4 |16 |1 |Procesor Intel Xeon E5-2698B v3 @ 2 GHz |84 |194,111 |4,735 |
| Standard_GS5 |32 |2 |Procesor Intel Xeon E5-2698B v3 @ 2 GHz |84 |357,396 |16,228 |

## <a name="h-series"></a>Seria H
| Rozmiar | Vcpu | Węzły NUMA | Procesor CPU | Uruchamia | Iteracje na sekundę | StdDev |
| --- | --- | --- | --- | --- | --- | --- |
| Standardowa_H8 |8 |1 |Procesor Intel Xeon E5-2667 v3 @ 3,2 GHz |28 |140,782 |2,512 |
| Standardowa_H16 |16 |2 |Procesor Intel Xeon E5-2667 v3 @ 3,2 GHz |35 |275,289 |7,110 |
| Standard_H18m |8 |1 |Procesor Intel Xeon E5-2667 v3 @ 3,2 GHz |28 |139,071 |3,988 |
| Standardowa_H16m |16 |2 |Procesor Intel Xeon E5-2667 v3 @ 3,2 GHz |28 |275,988 |6,963 |
| Warstwie standardowa_h16r |16 |2 |Procesor Intel Xeon E5-2667 v3 @ 3,2 GHz |28 |273,982 |6,069 |
| Warstwie standardowa_h16mr |16 |2 |Procesor Intel Xeon E5-2667 v3 @ 3,2 GHz |28 |274,523 |5,698 |

## <a name="about-coremark"></a>O CoreMark
Numery Linux zostały obliczone, uruchamiając [CoreMark](http://www.eembc.org/coremark/faq.php) na Ubuntu. CoreMark został skonfigurowany z liczbę wątków, określ liczbę procesorów wirtualnych i ustaw PThreads współbieżności. Docelowy liczba iteracji została dostosowana oparte na obniżenie wydajności w celu zapewnienia obsługi co najmniej 20 sekund (zwykle znacznie dłużej). Końcowy wynik reprezentuje liczby iteracji ukończone podzielona przez liczbę sekund trwania testu. Każdy test zostało uruchomione co najmniej siedmiu razy na każdej maszynie Wirtualnej. Testuje (z wyjątkiem dla H series_ zostały uruchomione października 2015 wiele maszyn wirtualnych w każdym regionie publicznej Azure maszyny Wirtualnej była obsługiwana w dniu, uruchom.

## <a name="next-steps"></a>Następne kroki
* Pojemność, dysku szczegóły i dodatkowe uwagi dotyczące wybierania rozmiarów maszyn wirtualnych, zobacz [rozmiary maszyn wirtualnych](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Aby uruchamiać skrypty CoreMark na maszynach wirtualnych systemu Linux, Pobierz [CoreMark pakiet skryptu](http://download.microsoft.com/download/3/0/5/305A3707-4D3A-4599-9670-AAEB423B4663/AzureCoreMarkScriptPack.zip).

