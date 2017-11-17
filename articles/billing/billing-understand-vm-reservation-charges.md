---
title: "Zrozumienie aplikacji rabat zastrzeżone wystąpień maszyn wirtualnych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak rabat zastrzeżone wystąpienia maszyny Wirtualnej platformy Azure są stosowane do działających maszyn wirtualnych."
services: billing
documentationcenter: 
author: vikramdesai01
manager: vikdesai
editor: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: vikdesai
ms.openlocfilehash: 3fd12bd3c51eeef57c896da030a83e447dc3e8ff
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="understand-how-the-reserved-virtual-machine-instance-discount-is-applied"></a>Zrozumienie, jak stosowany jest rabat zastrzeżone wystąpienie maszyny wirtualnej
Po kupić zastrzeżone wystąpienia maszyny Wirtualnej, rabat rezerwacji jest automatycznie stosowane do dopasowania, atrybuty i ilość zastrzeżenia maszyn wirtualnych. Zastrzeżenie obejmuje kosztów infrastruktury maszyn wirtualnych. W poniższej tabeli przedstawiono kosztów dla maszyny wirtualnej, po dokonaniu zakupu rezerwacji. We wszystkich przypadkach są naliczane opłaty dotyczące magazynu i sieci w normalnych stawek.

| Typ maszyny wirtualnej  | Opłaty za pomocą rezerwacji |    
|-----------------------|--------------------------------------------| 
|Maszyny wirtualne systemu Linux bez dodatkowego oprogramowania | Zastrzeżenie obejmuje koszty infrastruktury maszyny Wirtualnej.|
|Maszyny wirtualne systemu Linux z opłat oprogramowania (na przykład Red Hat) | Zastrzeżenie obejmuje kosztów infrastruktury. Naliczane są opłaty za dodatkowe oprogramowanie.|
|Maszyny wirtualne systemu Windows bez dodatkowego oprogramowania |Zastrzeżenie obejmuje kosztów infrastruktury. Naliczane są opłaty oprogramowania systemu Windows.|
|Maszyny wirtualne systemu Windows z dodatkowego oprogramowania (na przykład SQL server) | Zastrzeżenie obejmuje kosztów infrastruktury. Naliczane są opłaty oprogramowania systemu Windows i dodatkowe oprogramowanie.|
|Maszyny wirtualne systemu Windows z [korzyści hybrydowe platformy Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | Zastrzeżenie obejmuje kosztów infrastruktury. Koszty oprogramowania systemu Windows są objęte korzyści hybrydowe platformy Azure. Dodatkowe oprogramowanie jest pobierana osobno.| 

## <a name="application-of-reservation-discount-to-non-windows-vms"></a>Aplikacja rabatu zastrzeżenia do VMs z systemem innym niż Windows
 Zniżki rezerwacji jest stosowany do uruchomionych wystąpień maszyny Wirtualnej na godzinę. Zastrzeżenia, które zostały zakupione są dopasowywane do użycia emitowane przez uruchomionych maszyn wirtualnych do zastosowania rabat rezerwacji. Wykres przedstawia aplikacji rezerwacji rozliczeniowy użycia maszyny Wirtualnej. Ilustracja opiera się na jednej rezerwacji zakupu i dwa zgodnych wystąpień maszyny Wirtualnej.

![Zarezerwowane aplikacji wystąpienia maszyny Wirtualnej](media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1.  Bez użycia powyżej wiersza zastrzeżone wystąpienia maszyny Wirtualnej pobiera obciążony stawkami płatności obejmujące regularne. Nie masz opłata za bez użycia poniżej wiersza zastrzeżone wystąpień maszyny Wirtualnej, ponieważ ma on już płatnej w ramach zakupu rezerwacji.
2.  W ciągu 1 godziny jest uruchomione wystąpienie 1 godziny 0,75 i uruchamia wystąpienie 2 0,5 godzin. Całkowite użycie godzinę 1 jest 1,25 godzin. Płatności obejmujące stawki są naliczane pozostałych godzin 0,25.
3.  Godzina 2 i 3 godziny oba wystąpienia uruchomiono 1 godziny. Jedno wystąpienie jest objęta zastrzeżenia i innych rozliczany z szybkością.
4.  4 godziny jest uruchomione wystąpienie 1 godziny 0,5 i uruchamia wystąpienie 2 1 godziny. Wystąpienia 1 pełni objęta zastrzeżenia i jest objęte 0,5 godzin wystąpienia 2. Jest obciążony płatności obejmujące współczynnika 0,5 pozostałe godziny.

Aby zrozumieć i Wyświetl aplikację z rezerwacji w rozliczeń raporty użycia, zobacz [użycia zrozumieć zastrzeżone wystąpienia maszyny Wirtualnej](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="application-of-reservation-discount-to-windows-vms"></a>Stosowanie rabatu zastrzeżenia do maszyn wirtualnych systemu Windows
Po uruchomieniu wystąpień maszyny Wirtualnej systemu Windows, rezerwacji jest stosowany do obejmują kosztów infrastruktury. Aplikacja rezerwacji kosztów infrastruktury maszyny Wirtualnej dla maszyn wirtualnych systemu Windows jest taka sama jak w przypadku VMs z systemem innym niż Windows. Naliczane są opłaty oddzielnie oprogramowania systemu Windows na podstawie na vCPU. Zobacz [koszty oprogramowania Windows zastrzeżenia](https://go.microsoft.com/fwlink/?linkid=862756). Może obejmować licencjonowania kosztów [Azure hybrydowego korzyści dla systemu Windows Server] (https://docs.microsoft.com/en-us/azure/virtual-machines/windows/hybrid-use-benefit-licensing) systemu Windows.

## <a name="need-help-contact-support"></a>Potrzebujesz pomocy? Kontakt z pomocą techniczną

Jeśli potrzebujesz pomocy, [się z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) uzyskać szybkie rozwiązanie problemu.
