---
title: "Wyświetl zadania i zarządzać nimi StorSimple | Dokumentacja firmy Microsoft"
description: "Opis strony zadania usługi Menedżer StorSimple oraz używać go do śledzenia ostatnie, bieżących i zaplanowanych zadań tworzenia kopii zapasowej."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 55922cd0-d490-48eb-938a-012a67c1c09e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 7d9377bb8f3cb8c587823c2d71d61dfb9b50f52f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs"></a>Użyj usługi Menedżer StorSimple, aby wyświetlić zadania i zarządzać nimi StorSimple
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Omówienie
**Zadania** strony zapewnia jednym portalu centralnej do przeglądania i zarządzania zadaniami, które zostały uruchomione na urządzeniach połączona z usługą Menedżer StorSimple. Można wyświetlić zadań zaplanowanych, uruchomionych, została zakończona i nie powiodło się dla wielu urządzeń. Wyniki są prezentowane w formie tabeli. 

![Strona zadania](./media/storsimple-manage-jobs/HCS_JobsPage.png)

Można szybko znaleźć zadania, które są zainteresowani przez filtrowanie w polach, takich jak:

* **Stan** — zadania może być uruchomiony, zaplanowane, nie powiodło się, ukończonych, anulowanie lub anulowane.
* **Typ** — zadania mogą być tworzone w wyniku zaplanowanego lub kopii zapasowej na żądanie (**wykonać kopii zapasowej**), klonowanie, przywracania urządzenia lub operacji aktualizacji.
* **Urządzenia** — zadań są inicjowane na niektórych urządzeniach połączona z usługą.
* **Od i do** — zadania mogą być filtrowane w oparciu o zakres dat i godzin.

Następnie wyszczególniono odfiltrowanych zadań na podstawie następujących atrybutów:

* **Typ** — kopii zapasowej, sklonowany, przywracania, pracy awaryjnej lub aktualizacji.
* **Stan** — uruchomiona, zaplanowane, nie powiodło się, ukończone, anulowanie lub anulowane.
* **Jednostka** — zadania może być skojarzony z woluminu, zasad tworzenia kopii zapasowej lub urządzeniu. Zadanie klonowania jest skojarzony z woluminem, zaplanowane zadanie tworzenia kopii zapasowej jest skojarzone z zasadami tworzenia kopii zapasowej. Zadania urządzenia jest tworzony w wyniku odzyskiwania awaryjnego (DR) lub operacji przywracania.
* **Urządzenie** — nazwa urządzenia, na którym zadanie zostało uruchomione.
* **Rozpoczęto na** — czas uruchomienia zadania.
* **Postęp** — zakończenia procent uruchomionego zadania. Zadanie zostało ukończone, aby uzyskać to zawsze być 100%.

Lista zadań są odświeżane co 30 sekund.

Na tej stronie można wykonywać następujące czynności związanych z pracą:

* Wyświetl szczegóły zadania
* Anulowanie zadania

## <a name="view-job-details"></a>Wyświetl szczegóły zadania
Wykonaj poniższe kroki, aby wyświetlić szczegóły wszystkie zadania.

#### <a name="to-view-job-details"></a>Aby wyświetlić szczegóły zadania
1. Na **zadania** strony, wyświetlania zadań są zainteresowani, uruchamiając zapytanie o odpowiednie filtry. Można wyszukiwać ukończone, uruchomiona lub anulowane zadania.
2. Wybierz zadanie.
3. W dolnej części strony kliknij **szczegóły**.
4. W **szczegóły zadania tworzenia kopii zapasowej** okno dialogowe, można wyświetlić stan, szczegóły, statystyk czasu i statystyki danych.

## <a name="cancel-a-job"></a>Anulowanie zadania
Wykonaj poniższe kroki, aby anulować zadanie uruchomione.

### <a name="to-cancel-a-job"></a>Aby anulować zadanie
1. Na **zadania** pozycję Wyświetl uruchomione zadania, które chcesz anulować, uruchamiając zapytanie z filtrami odpowiednie.
2. Wybierz zadanie.
3. W dolnej części strony kliknij **anulować**.
4. Po wyświetleniu monitu o potwierdzenie kliknij przycisk **Tak**.

To zadanie jest teraz anulowane.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [Zarządzanie zasadami tworzenia kopii zapasowej StorSimple](storsimple-manage-backup-policies.md).
* Dowiedz się, jak [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

