---
title: "Wyświetl zadania i zarządzać nimi StorSimple | Dokumentacja firmy Microsoft"
description: "Opis strony zadania usługi Menedżer StorSimple oraz używać go do śledzenia ostatnie, bieżących i zaplanowanych zadań tworzenia kopii zapasowej."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: cdd3e9e8-7ccd-40a3-8c07-0ab1338c0e59
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 6df1b27ce76de7a781ecc40af8430114d80b20d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs-update-2"></a>Użyj usługi Menedżer StorSimple, aby wyświetlić zadania i zarządzać nimi StorSimple (aktualizacja Update 2)
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Omówienie
**Zadania** strony zapewnia jednym portalu centralnej do przeglądania i zarządzania zadaniami, które zostały uruchomione na urządzeniach połączona z usługą Menedżer StorSimple. Można wyświetlić zadań zaplanowanych, uruchomionych, zakończone, anulowane i nie powiodło się dla wielu urządzeń. Wyniki są prezentowane w formie tabeli. 

![Strona zadania](./media/storsimple-manage-jobs-u2/jobs.png)

Można szybko znaleźć zadania, które są zainteresowani przez filtrowanie w polach, takich jak:

* **Stan** — zadania może być uruchomiony, zakończone, anulowane, nie powiodło się, anulowanie lub ukończone z błędami.
* **Od i do** — zadania mogą być filtrowane w oparciu o zakres dat i godzin.
* **Typ** — typ zadania mogą być kopii zapasowej, ręcznego wykonywania kopii zapasowej, przywracania, klonowania, tryb failover urządzeń, tworzenia woluminu przypiętego lokalnie, zmodyfikować woluminu, aktualizowanie i obsługuje pakiet lub inicjowania obsługi urządzenia wirtualnego.
* **Urządzenia** — zadań są inicjowane na niektórych urządzeniach połączona z usługą.
  Następnie wyszczególniono odfiltrowanych zadań na podstawie następujących atrybutów:
  
  * **Typ** — kopia zapasowa, ręcznego wykonywania kopii zapasowej, przywracania, klonowania, tryb failover urządzeń, tworzenia woluminu przypiętego lokalnie, zmodyfikować woluminu, aktualizowanie i obsługuje pakiet lub inicjowania obsługi urządzenia wirtualnego.
  * **Stan** — uruchomiona, zakończone, anulowane, nie powiodło się, anulowanie lub ukończone z błędami.
  * **Jednostka** — zadania może być skojarzony z woluminu, zasad tworzenia kopii zapasowej lub urządzeniu. Na przykład zadanie klonowania jest skojarzony z woluminem, zaplanowane zadanie tworzenia kopii zapasowej jest skojarzone z zasadami tworzenia kopii zapasowej. Zadania urządzenia jest tworzony w wyniku odzyskiwania awaryjnego (DR) lub operacji przywracania.
  * **Urządzenie** — nazwa urządzenia, na którym zadanie zostało uruchomione.
  * **Rozpoczęto w** — czas uruchomienia zadania.
  * **Postęp** — zakończenia procent uruchomionego zadania. Zadanie zostało ukończone, aby uzyskać to zawsze być 100%.

Lista zadań są odświeżane co 30 sekund.

Na tej stronie można wykonywać następujące czynności związanych z pracą:

* Wyświetlanie szczegółów zadania
* Anulowanie zadania

## <a name="view-job-details"></a>Wyświetlanie szczegółów zadania
Wykonaj poniższe kroki, aby wyświetlić szczegóły wszystkie zadania.

#### <a name="to-view-job-details"></a>Aby wyświetlić szczegóły zadania
1. Na **zadania** strony, wyświetlania zadań są zainteresowani, uruchamiając zapytanie o odpowiednie filtry. Można wyszukiwać ukończone, uruchomiona lub anulowane zadania.
2. Wybierz zadanie.
3. W dolnej części strony kliknij **szczegóły**.
4. W **szczegóły zadania tworzenia kopii zapasowej** okno dialogowe, można wyświetlić stan, szczegóły, statystyk czasu i statystyki danych.
   
    ![Strona szczegółów zadania](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a>Anulowanie zadania
Wykonaj poniższe kroki, aby anulować zadanie uruchomione.

> [!NOTE]
> Niektóre zadania, takie jak modyfikowanie woluminu, aby zmienić typ woluminu lub rozszerzenie woluminu, nie można anulować.
> 
> 

### <a name="to-cancel-a-job"></a>Aby anulować zadanie
1. Na **zadania** pozycję Wyświetl uruchomione zadania, które chcesz anulować, uruchamiając zapytanie z filtrami odpowiednie.
2. Wybierz zadanie.
3. W dolnej części strony kliknij **anulować**.
4. Po wyświetleniu monitu o potwierdzenie kliknij przycisk **Tak**.

To zadanie jest teraz anulowane.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [Zarządzanie zasadami tworzenia kopii zapasowej StorSimple](storsimple-manage-backup-policies.md).
* Dowiedz się, jak [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

