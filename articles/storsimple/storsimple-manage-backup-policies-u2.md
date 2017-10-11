---
title: "Zarządzanie zasadami tworzenia kopii zapasowej StorSimple | Dokumentacja firmy Microsoft"
description: "W tym artykule wyjaśniono, jak można użyć usługi Menedżer StorSimple do tworzenia i zarządzania ręcznego tworzenia kopii zapasowych, harmonogramy tworzenia kopii zapasowej i przechowywania kopii zapasowych."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 4a2db707-bbfc-425c-bfeb-bc5c97781873
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2016
ms.author: v-sharos
ms.openlocfilehash: 5448247428ab96887470c6b53f7a9b3dcd9238f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-backup-policies-update-2"></a>Usługę Menedżer StorSimple umożliwia zarządzanie zasadami tworzenia kopii zapasowej (Update 2)
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Omówienie
W tym samouczku wyjaśniono, jak używać usługi Menedżer StorSimple **zasad tworzenia kopii zapasowych** strony do kontroli procesów tworzenia kopii zapasowej i przechowywania kopii zapasowych dla woluminów StorSimple. Zawiera również opis jak zakończyć ręczne kopii zapasowej.

Podczas wykonywania kopii zapasowej woluminu, można utworzyć migawki lokalnej lub migawka w chmurze. W przypadku wykonywania kopii zapasowej woluminu przypiętego lokalnie, zaleca się określenie migawka w chmurze. Biorąc dużą liczbę lokalnych migawek woluminu przypiętego lokalnie, w połączeniu z zestawu danych, który ma wiele zmian spowoduje w sytuacji, w którym można szybko uruchomić poza lokalne miejsce. Jeśli zdecydujesz się na lokalnych migawek, zaleca się zająć mniej migawki codziennych kopii zapasowych ostatniego stanu, należy zachować je na dzień, a następnie usunąć je.

Podczas wykonywania migawki woluminu przypiętego lokalnie chmury kopiujesz tylko zmienione dane w chmurze, gdzie jest deduplikowany i skompresowane. 

## <a name="the-backup-policies-page"></a>Na stronie zasady tworzenia kopii zapasowej
**Zasad tworzenia kopii zapasowych** strona umożliwia zarządzanie zasadami tworzenia kopii zapasowej i zaplanować lokalne i migawki w chmurze. (Zasad tworzenia kopii zapasowych służą do konfigurowania harmonogramy tworzenia kopii zapasowej i przechowywania kopii zapasowych dla kolekcji woluminów). Zasady tworzenia kopii zapasowych umożliwiają utworzenie migawki wiele woluminów jednocześnie. Oznacza to, że kopie zapasowe utworzone przez zasady tworzenia kopii zapasowej będzie spójna w razie awarii kopie. **Zasad tworzenia kopii zapasowych** strona zawiera listę zasad tworzenia kopii zapasowych, ich typy skojarzone woluminy, liczbę kopii zapasowych przechowywane i możliwość włączenia tych zasad.

**Zasad tworzenia kopii zapasowych** strony można też filtrować istniejących zasad tworzenia kopii zapasowych przez jedną lub więcej z następujących pól:

* **Nazwa zasad** — Nazwa skojarzona z zasadami. Różne typy zasad:
  
  * Zaplanowane zasady, które są jawnie utworzone przez użytkownika.
  * Automatyczne zasady, które są tworzone podczas domyślnego tworzenia kopii zapasowej dla tej opcji wolumin został włączony w czasie tworzenia woluminu. Te zasady są nazywane jako *VolumeName*_domyślny gdzie *VolumeName* odwołuje się do nazwy woluminu StorSimple skonfigurowany przez użytkownika w klasycznym portalu Azure. Zasady automatycznego spowodować codzienne migawki w chmurze od chwili urządzenia 22:30.
  * Zasady importowane, które zostały pierwotnie utworzone w programie StorSimple Snapshot Manager. Mają one znacznik hosta StorSimple Snapshot Manager, które zasady zostały zaimportowane z.
* **Woluminy** — woluminy skojarzonych z zasadami. Wszystkie woluminy, które są skojarzone z zasadami tworzenia kopii zapasowej są grupowane razem, gdy kopie zapasowe są tworzone.
* **Ostatnia kopia zapasowa pomyślnie** — Data i godzina ostatniej pomyślnej kopii zapasowej, która została wykonana z tymi zasadami.
* **Następną kopią zapasową** — Data i godzina następnej zaplanowanej kopii zapasowej będą inicjowane przez te zasady.
* **Harmonogramy** — liczba harmonogramów skojarzonych z zasadami tworzenia kopii zapasowej.

Często używane operacje, które można wykonywać na tej stronie są:

* Dodawanie zasad kopii zapasowych 
* Dodaj lub zmodyfikuj harmonogram 
* Usuń zasady tworzenia kopii zapasowej 
* Wykonaj kopię zapasową ręczne 
* Tworzenie niestandardowych zasad tworzenia kopii zapasowej z wieloma woluminami i harmonogramów 

## <a name="add-a-backup-policy"></a>Dodawanie zasad kopii zapasowych
Dodawanie zasad tworzenia kopii zapasowej, można zaplanować automatyczne kopie zapasowe. Wykonaj poniższe kroki w klasycznym portalu Azure, aby dodać zasady tworzenia kopii zapasowej dla urządzenia StorSimple. Po dodaniu zasad można określić harmonogramu (zobacz [Dodaj lub zmodyfikuj harmonogram](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

![Zobacz film](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **Zobacz film**

Aby obejrzeć film wideo przedstawiający sposób tworzenia lokalnie lub w chmurze zasad tworzenia kopii zapasowej, kliknij przycisk [tutaj](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).

## <a name="add-or-modify-a-schedule"></a>Dodaj lub zmodyfikuj harmonogram
Można dodać lub zmodyfikować harmonogram, który jest dołączony do istniejących zasad tworzenia kopii zapasowych w urządzeniu StorSimple. Wykonaj poniższe kroki w klasycznym portalu Azure, aby dodać lub zmodyfikować harmonogram.

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## <a name="delete-a-backup-policy"></a>Usuń zasady tworzenia kopii zapasowej
Wykonaj poniższe kroki w klasycznym portalu Azure, aby usunąć zasady tworzenia kopii zapasowych w urządzeniu StorSimple.

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>Wykonaj kopię zapasową ręczne
Wykonaj poniższe kroki w klasycznym portalu Azure, aby utworzyć na żądanie (ręcznie) kopię zapasową pojedynczego woluminu.

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Tworzenie niestandardowych zasad tworzenia kopii zapasowej z wieloma woluminami i harmonogramów
Wykonaj poniższe kroki w klasycznym portalu Azure, aby utworzyć niestandardowe zasady tworzenia kopii zapasowej, który zawiera wiele woluminów i harmonogramy.

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [przy użyciu usługi Menedżer StorSimple do administrowania urządzeniem StorSimple](storsimple-manager-service-administration.md).

