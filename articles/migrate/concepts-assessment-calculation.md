---
title: "Ocena obliczeń w migracji Azure | Dokumentacja firmy Microsoft"
description: "Zawiera omówienie obliczeń oceny w usłudze Azure migracji."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 39a63769-31eb-49f9-9089-4d3e4e88a412
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: f00825ff9a5018e67672ce452f01130f84919e52
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/27/2017
---
# <a name="assessment-calculations"></a>Obliczenia oceny

[Azure migracji](migrate-overview.md) ocenia obciążeń lokalnych do migracji do usługi Azure. Ten artykuł zawiera informacje o sposobie obliczania oceny.



## <a name="overview"></a>Omówienie

Ocena migracji Azure ma trzy etapy. Ocena rozpoczyna się od analizy przydatności, po której następuje ocen wydajności na podstawie rozmiaru i w końcu co miesiąc kosztów szacowania. Maszyna tylko przenosi na późniejszym etapie Jeśli przekazaniem poprzedni. Na przykład w przypadku niepowodzenia sprawdzania Azure przydatności maszyna jest oznaczony jako nieodpowiednie dla platformy Azure oraz zmiany rozmiaru i kosztów nie można obliczyć. 


## <a name="azure-suitability-analysis"></a>Analiza przydatności Azure

Maszyny, które chcesz przeprowadzić migrację do usługi Azure musi spełniać wymagania dotyczące usługi Azure i ograniczenia. Na przykład jeśli na lokalnym dysku maszyny Wirtualnej jest więcej niż 4 TB, nie może być umieszczona na platformie Azure. Sprawdzanie zgodności Azure podsumowano w poniższej tabeli. 

**Sprawdź** | **Szczegóły**
--- | ---
**Typ rozruchu** | Typ rozruchowy dysk systemu operacyjnego gościa musi być systemu BIOS, a nie z interfejsem UEFI.
**Liczba rdzeni** | Liczba rdzeni na maszynach musi być równa (lub mniej niż) maksymalna liczba rdzeni (32) obsługiwanych na maszynie Wirtualnej platformy Azure.<br/><br/> Jeśli Historia wydajności jest dostępny, Azure migracji uwzględnia rdzenie wykorzystywanych do porównania. Jeśli współczynnik komfort określono w ustawieniach oceny, liczba rdzeni wykorzystywanych jest mnożona przez współczynnik komfort.<br/><br/> Jeśli nie ma żadnych Historia wydajności, Azure migracji używa przydzielonych rdzeni, bez stosowania współczynnik komfort.
**Pamięci** | Rozmiar pamięci maszyny musi być równa (lub mniej niż) dozwolona maksymalna ilość pamięci (448 GB) dla maszyny Wirtualnej platformy Azure. <br/><br/> Jeśli Historia wydajności jest dostępny, Azure migracji uwzględnia pamięci wykorzystywanych do porównania. Jeśli określono współczynnik komfort, wykorzystywanych pamięci jest mnożona przez współczynnik komfort.<br/><br/> Jeśli nie ma Brak historii alokacji pamięci jest używana bez stosowania współczynnik komfort.<br/><br/> 
**Windows Server 2003 – 2008 R2** | Obsługa 32-bitowe i 64-bitowych.<br/><br/> Platforma Azure udostępnia najlepsze Obsługa wysiłku.
**Windows Server 2008 R2 z wszystkich SPs** | Obsługa 64-bitowego.<br/><br/> Azure zapewnia pełną obsługę.
**Windows Server 2012 & wszystkie SPs** | Obsługa 64-bitowego.<br/><br/> Azure zapewnia pełną obsługę.
**Windows Server 2012 R2 & wszystkie SPs** | Obsługa 64-bitowego.<br/><br/> Azure zapewnia pełną obsługę.
**Windows Server 2016 & wszystkie SPs** | Obsługa 64-bitowego.<br/><br/> Azure zapewnia pełną obsługę.
**Klient systemu Windows 7 i nowsze** | Obsługa 64-bitowego.<br/><br/> Azure zapewnia obsługę subskrypcji programu Visual Studio tylko.
**Linux** | Obsługa 64-bitowego.<br/><br/> Azure zapewnia pełną obsługę tych [systemów operacyjnych](../virtual-machines/linux/endorsed-distros.md).
**Dysk magazynu** | Przydzielony rozmiar dysku musi być 4 TB (4096 GB) lub mniej.<br/><br/> Liczba dysków dołączonych do maszyny musi być 65 lub mniej, w tym dysku systemu operacyjnego. 
**Sieć** | Komputer musi mieć 32 lub mniej karty sieciowe podłączone do niego.


## <a name="performance-based-sizing"></a>Na podstawie rozmiaru

Po maszyna jest oznaczona jako odpowiednią dla platformy Azure, Azure migracji mapuje go do rozmiaru maszyny Wirtualnej na platformie Azure, używając następujących kryteriów:

- **Wyboru magazynu**: Migrowanie Azure próbuje zamapować co dysk dołączony do maszyny na dysku na platformie Azure:-Azure migracji mnoży we/wy na sekundę (IOPS) przez współczynnik komfort. On również wielokrotności przepustowość (w MB/s) każdego dysku przez współczynnik komfort. Zapewnia to obowiązującą IOPS i przepływność dysku. Na podstawie tych, Azure migracji map na standard lub premium dysku na platformie Azure.
    - Jeśli usługa nie może znaleźć dysku o wymagane IOPS & przepływności, oznacza maszynę jako nieodpowiednie dla platformy Azure.
    - W przypadku odnalezienia zestaw odpowiednich dysków, Azure migracji wybiera te, które obsługują metody nadmiarowość magazynu i lokalizacji określonej w ustawieniach oceny.
    - W przypadku wielu dysków kwalifikujących się wybiera jeden z najniższy koszt.
- **Przepływność dysku magazynu**: [więcej](../azure-subscription-service-limits.md#storage-limits) o Azure ogranicza na dysku i maszyny Wirtualnej.
- **Typ dysku**: Migrowanie Azure obsługuje tylko dysków zarządzanych.
- **Sieci wyboru**: Migrowanie Azure próbuje znaleźć maszyny Wirtualnej platformy Azure, który może obsługiwać liczbę kart sieciowych na komputerze lokalnym.
    - W tym celu należy go agreguje dane przesyłane na sekundę (MB/s) poza maszyny (sieć out) dla wszystkich kart i dotyczy współczynnik komfort zagregowane numer. Ten numer, jeśli używana do znajdowania obsługujące wydajności wymagana sieć maszyny Wirtualnej Azure.
    - Azure migracji przyjmuje ustawienia sieci z maszyny Wirtualnej, a zakłada się sieci poza centrum danych.
    - Jeśli dane wydajności sieci są niedostępne, tylko z liczbą kart jest uznawany za dla rozmiaru maszyny Wirtualnej.
- **Obliczenia bazy danych sprawdź**: po obliczeniu są wymagania dotyczące magazynu i sieci Azure migracji uwzględnia wymagania obliczeniowe:
    - Jeśli dane wydajności są dostępne dla maszyny Wirtualnej, analizuje wykorzystywanych rdzeni i ilości pamięci i stosuje współczynnik komfort. Na podstawie tego numeru, próbuje odnaleźć odpowiedni rozmiar maszyny Wirtualnej na platformie Azure.
    - Jeśli zostanie znaleziony żaden odpowiedni rozmiar, maszynie jest oznaczony jako nieodpowiednie dla platformy Azure.
    - Jeśli zostanie znaleziony odpowiedni rozmiar, Azure migracji ma zastosowanie obliczenia magazynu i sieci. Następnie stosuje lokalizacji i cenach ustawień warstwę, do końcowego zalecenie rozmiar maszyny Wirtualnej.


## <a name="monthly-cost-estimation"></a>Miesięczne szacowania kosztów

Po zakończeniu zmiany rozmiaru zalecenia dotyczące migracji Azure oblicza koszty obliczeniowej i pamięci masowej po migracji.

- **Obliczanie kosztu**: przy użyciu zalecany rozmiar maszyny Wirtualnej platformy Azure, Azure migracji używa interfejsu API rozliczeń do obliczenia miesięczny koszt dla maszyny Wirtualnej. Obliczenie ma systemu operacyjnego, software assurance, lokalizacji i ustawienia waluty pod uwagę. Agreguje koszt na wszystkich komputerach, aby obliczyć całkowity miesięczny koszt obliczeń.
- **Koszt magazynowania**: Magazyn miesięczny koszt maszyna jest obliczana przez agregowanie miesięczny koszt wszystkich dysków dołączonych do maszyny. Azure migracji oblicza całkowity miesięczny kosztów magazynowania przez agregowanie kosztów magazynowania wszystkich maszyn. Obecnie obliczenia nie przyjmuje oferty określonego w ustawieniach ocenę pod uwagę.

Koszty są wyświetlane w walucie określonego w ustawieniach oceny. 


## <a name="next-steps"></a>Następne kroki

[Konfigurowanie oceny dla lokalnych maszyn wirtualnych VMware](tutorial-assessment-vmware.md)
