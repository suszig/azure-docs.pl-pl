---
title: "Zarządzanie katalogu kopii zapasowej StorSimple | Dokumentacja firmy Microsoft"
description: "Wyjaśniono, jak używać na stronie katalogu kopii zapasowej usługi Menedżer StorSimple do listy, wybierz i Usuń zestawy kopii zapasowych dla woluminu."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ad81bee9-fe43-40b3-a384-b15fb274ecd9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/28/2016
ms.author: v-sharos
ms.openlocfilehash: 5ee9855e1428c7a2d871d9c215d302c5c3b7101a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>Usługę Menedżer StorSimple umożliwia zarządzanie katalogu kopii zapasowej
## <a name="overview"></a>Omówienie
Usługę Menedżer StorSimple **katalogu kopii zapasowej** na stronie są wyświetlane wszystkie zestawy kopii zapasowych tworzonych podczas ręczne lub zaplanowane kopie zapasowe są pobierane. Ta strona służy do listę wszystkich kopii zapasowych dla zasad tworzenia kopii zapasowej lub woluminem, zaznacz lub usuń kopii zapasowych lub użyj kopii zapasowej do przywrócenia lub sklonować woluminu.

W tym samouczku wyjaśniono, jak listy, wybierz i usuwania zestawu kopii zapasowych. Aby dowiedzieć się, jak przywracanie z kopii zapasowej urządzenia, przejdź do [przywrócenie urządzenia z zestawu kopii zapasowych](storsimple-restore-from-backup-set.md). Aby dowiedzieć się, jak klonowanie woluminu, przejdź do [sklonować woluminu StorSimple](storsimple-clone-volume.md).

![Katalog kopii zapasowej](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

**Katalogu kopii zapasowej** strony zawiera zapytanie, aby zawęzić kopii zapasowej zaznaczenia. Można filtrować zestawy kopii zapasowych, które są pobierane na podstawie następujących parametrów:

* **Urządzenie** — urządzenia, na którym utworzono ten zestaw kopii zapasowych.
* **Wykonaj kopię zapasową zasad lub woluminu** — zasady tworzenia kopii zapasowej lub woluminie skojarzonym z tego zestawu kopii zapasowych.
* **Od i do** — zakres dat i godzin przy tworzeniu zestawu kopii zapasowych.

Następnie filtrowane zestawy kopii zapasowych wyszczególniono na podstawie następujących atrybutów:

* **Nazwa** — Nazwa zasad tworzenia kopii zapasowej lub woluminie skojarzonym z zestawu kopii zapasowych.
* **Rozmiar** — rzeczywisty rozmiar zestawu kopii zapasowych.
* **Utworzone na** — Data i godzina utworzenia kopii zapasowych. 
* **Typ** — zestawy kopii zapasowych można migawki lokalne lub migawki w chmurze. Migawka lokalna jest kopię zapasową wszystkich danych woluminu lokalnie przechowywanych na urządzeniu, migawka w chmurze odnosi się do kopii zapasowej danych woluminu znajdującej się w chmurze. Migawki lokalne zapewnić szybszy dostęp migawki w chmurze są wybrać, aby zachować odporność danych.
* **Zainicjowane przez** — kopie zapasowe można zainicjować automatycznie według harmonogramu lub ręcznie przez użytkownika. Aby zaplanować tworzenie kopii zapasowych, można użyć zasad tworzenia kopii zapasowej. Alternatywnie można użyć **utworzenia kopii zapasowej** opcji, aby utworzyć kopię zapasową ręczne.

## <a name="list-backup-sets-for-a-volume"></a>Zestawy kopii zapasowych listy woluminu
Wykonaj poniższe kroki, aby wyświetlić listę wszystkich kopii zapasowych dla woluminu.

#### <a name="to-list-backup-sets"></a>Do listy zestawów kopii zapasowych
1. Na stronie usługi Menedżer StorSimple, kliknij przycisk **katalog kopii zapasowej** kartę.
2. Filtruj ustawienia w następujący sposób:
   
   1. Wybierz odpowiednie urządzenie.
   2. Na liście rozwijanej wybierz wolumin, aby wyświetlić odpowiednie kopie zapasowe.
   3. Określ zakres czasu.
   4. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) do wykonania tego zapytania.
      
      Tworzenie kopii zapasowych skojarzone z wybranego woluminu powinny być wyświetlane na liście zestawów kopii zapasowych.

## <a name="select-a-backup-set"></a>Wybierz zestaw kopii zapasowych
Wykonaj poniższe kroki, aby wybrać zestaw kopii zapasowych dla woluminu lub zasad tworzenia kopii zapasowej.

#### <a name="to-select-a-backup-set"></a>Aby wybrać zestaw kopii zapasowych
1. Na stronie usługi Menedżer StorSimple, kliknij przycisk **katalog kopii zapasowej** kartę.
2. Filtruj ustawienia w następujący sposób:
   
   1. Wybierz odpowiednie urządzenie.
   2. Na liście rozwijanej wybierz zasady woluminu lub kopii zapasowej do utworzenia kopii zapasowej, który chcesz wybrać.
   3. Określ zakres czasu.
   4. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) do wykonania tego zapytania.
      
      Tworzenie kopii zapasowych skojarzony z wybranego woluminu lub zasad tworzenia kopii zapasowej powinien zostać wyświetlony na liście zestawów kopii zapasowych.
3. Wybierz i rozwiń zestawu kopii zapasowych. **Przywrócić** i **usunąć** opcje są wyświetlane w dolnej części strony. Każda z tych akcji można wykonywać na wybrany zestaw kopii zapasowych.

## <a name="delete-a-backup-set"></a>Usuń zestaw kopii zapasowych
Usunięcie kopii zapasowej, jeśli nie chcesz zachować dane skojarzone z nim. Wykonaj poniższe kroki, aby usunąć zestaw kopii zapasowych.

#### <a name="to-delete-a-backup-set"></a>Aby usunąć zestaw kopii zapasowych
1. Na stronie usługi Menedżer StorSimple, kliknij przycisk **kartę katalog kopii zapasowej**.
2. Filtruj ustawienia w następujący sposób:
   
   1. Wybierz odpowiednie urządzenie.
   2. Na liście rozwijanej wybierz zasady woluminu lub kopii zapasowej do utworzenia kopii zapasowej, który chcesz wybrać.
   3. Określ zakres czasu.
   4. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) do wykonania tego zapytania.
      
      Tworzenie kopii zapasowych skojarzony z wybranego woluminu lub zasad tworzenia kopii zapasowej powinien zostać wyświetlony na liście zestawów kopii zapasowych.
3. Wybierz i rozwiń zestawu kopii zapasowych. **Przywrócić** i **usunąć** opcje są wyświetlane w dolnej części strony. Kliknij polecenie **Usuń**.
4. Użytkownik zostanie powiadomiony, gdy usunięcie jest w toku i kiedy zostało pomyślnie zakończone. Po zakończeniu usuwania Odśwież zapytanie na tej stronie. Usunięto zestaw kopii zapasowych nie będzie dłużej widoczny na liście zestawów kopii zapasowych.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [korzystania z katalogu kopii zapasowej do przywrócenia urządzenia z zestawu kopii zapasowych](storsimple-restore-from-backup-set.md).
* Dowiedz się, jak [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

