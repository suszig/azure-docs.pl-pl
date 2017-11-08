---
title: Przywracanie z kopii zapasowej woluminu StorSimple | Dokumentacja firmy Microsoft
description: "Wyjaśniono, jak strona katalogu kopii zapasowej usługi Menedżer StorSimple służy do przywracania woluminu StorSimple z zestawu kopii zapasowych."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b979782e-3184-4465-ad5f-e4516a5885d2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 0ed433ba93cb08742e3f0b09cc7acaa2cd62461e
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Przywracania woluminu StorSimple z zestawu kopii zapasowych
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Omówienie
**Katalogu kopii zapasowej** na stronie są wyświetlane wszystkie zestawy kopii zapasowych, które są tworzone podczas ręczne lub automatyczne kopie zapasowe są pobierane. Ta strona służy do listę wszystkich kopii zapasowych dla zasad tworzenia kopii zapasowej lub woluminem, zaznacz lub usuń kopii zapasowych lub użyj kopii zapasowej do przywrócenia lub sklonować woluminu.

 ![Strona katalogu kopii zapasowej](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

W tym samouczku wyjaśniono, jak używać **katalogu kopii zapasowej** stronę, aby odzyskać wolumin w urządzeniu z zestawu kopii zapasowych.

## <a name="how-to-use-the-backup-catalog"></a>Jak używać katalogu kopii zapasowej
**Katalogu kopii zapasowej** strona zawiera zaznaczyć kwerendę, która pomaga ograniczyć kopii zapasowej. Można filtrować zestawy kopii zapasowych, które są pobierane na podstawie następujących parametrów:

* **Urządzenie** — urządzenia, na którym utworzono ten zestaw kopii zapasowych.
* **Wykonaj kopię zapasową zasad** lub **woluminu** — zasady tworzenia kopii zapasowej lub woluminie skojarzonym z tego zestawu kopii zapasowych.
* **Z** i **do** — zakres dat i godzin przy tworzeniu zestawu kopii zapasowych.

Następnie filtrowane zestawy kopii zapasowych wyszczególniono na podstawie następujących atrybutów:

* **Nazwa** — Nazwa zasad tworzenia kopii zapasowej lub woluminie skojarzonym z zestawu kopii zapasowych.
* **Rozmiar** — rzeczywisty rozmiar zestawu kopii zapasowych.
* **Utworzony na** — Data i godzina utworzenia kopii zapasowych. 
* **Typ** — zestawy kopii zapasowych można migawki lokalne lub migawki w chmurze. Migawka lokalna jest kopię zapasową wszystkich danych woluminu lokalnie przechowywanych na urządzeniu, migawka w chmurze odnosi się do kopii zapasowej danych woluminu znajdującej się w chmurze. Migawki lokalne zapewnić szybszy dostęp migawki w chmurze są wybrać, aby zachować odporność danych.
* **Zainicjowane przez** — tworzenie kopii zapasowych może zostać zainicjowane automatycznie, zgodnie z harmonogramem lub ręcznie przez użytkownika. (Aby zaplanować tworzenie kopii zapasowych można użyć zasad tworzenia kopii zapasowej. Alternatywnie można użyć **utworzenia kopii zapasowej** opcję, aby wykonać interakcyjne kopii zapasowej.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>Jak przywrócić z kopii zapasowej woluminu StorSimple
Można użyć **katalogu kopii zapasowej** strony, aby przywrócić woluminu StorSimple z określonej kopii zapasowej. 

> [!WARNING]
> Przywracanie z kopii zapasowej spowoduje zastąpienie istniejących woluminów z kopii zapasowej. Może to spowodować utratę wszystkich danych, który został zapisany po wykonaniu kopii zapasowej.
> 
> 

Przed rozpoczęciem przywracania na woluminie, upewnij się, że wolumin jest w trybie offline. Musisz wykonać pierwszy woluminu w trybie offline na hoście, a następnie urządzenia. Postępuj zgodnie z instrukcjami [Przełącz do trybu offline wolumin](storsimple-manage-volumes.md#take-a-volume-offline). Wykonaj poniższe kroki, aby odzyskać wolumin z zestawu kopii zapasowych.

### <a name="to-restore-from-a-backup-set"></a>Przywrócenie z zestawu kopii zapasowych
1. Na stronie usługi Menedżer StorSimple, kliknij przycisk **katalog kopii zapasowej** kartę.
   
    ![Katalog kopii zapasowej](./media/storsimple-restore-from-backup-set/HCS_Restore.png)
2. Wybierz kopię zapasową ustawione w następujący sposób:
   
   1. Wybierz odpowiednie urządzenie.
   2. Na liście rozwijanej wybierz zasady woluminu lub kopii zapasowej do utworzenia kopii zapasowej, który chcesz wybrać.
   3. Określ zakres czasu.
   4. Kliknij ikonę znacznika wyboru ![ikona znacznika wyboru](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) do wykonania tego zapytania.
      
      Tworzenie kopii zapasowych skojarzony z wybranego woluminu lub zasad tworzenia kopii zapasowej powinien zostać wyświetlony na liście zestawów kopii zapasowych.
3. Rozwiń zestaw, aby wyświetlić skojarzone woluminy kopii zapasowych. Te woluminy muszą być pobierane w trybie offline na hoście i urządzenia, zanim będzie można ich przywrócić. Postępuj zgodnie z instrukcjami [Przełącz do trybu offline wolumin](storsimple-manage-volumes.md#take-a-volume-offline).
   
   > [!IMPORTANT]
   > Upewnij się, że została wykonana woluminy w tryb offline na hoście najpierw, przed podjęciem dalszych woluminy w tryb offline na urządzeniu. Jeśli nie przyjmują woluminy w tryb offline na hoście, może prowadzić do uszkodzenia danych.
   > 
   > 
4. Wybierz zestaw kopii zapasowych. Kliknij przycisk **przywrócić** w dolnej części strony.
5. Pojawi się monit o potwierdzenie. 
   
    ![Strona potwierdzenia](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)
6. Przejrzyj informacje przywracania, a następnie kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). Spowoduje to zainicjowanie zadania przywracania, które można wyświetlić, uzyskując dostęp do **zadania** strony. 
7. Po zakończeniu przywracania można sprawdzić, czy zawartość woluminów są zastępowane przez woluminy z kopii zapasowej.

![Zobacz film](./media/storsimple-restore-from-backup-set/Video_icon.png) **Zobacz film**

Aby obejrzeć film wideo przedstawiający sposób użycia klonu i przywrócić funkcji w programie StorSimple, aby odzyskać usunięte pliki, kliknij przycisk [tutaj](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [woluminów StorSimple zarządzanie](storsimple-manage-volumes.md).
* Dowiedz się, jak [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

