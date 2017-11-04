---
title: Przywracanie z kopii zapasowej woluminu StorSimple | Dokumentacja firmy Microsoft
description: "Wyjaśniono, jak strona katalogu kopii zapasowej usługi Menedżer StorSimple służy do przywracania woluminu StorSimple z zestawu kopii zapasowych."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6f289c39-96c7-4d57-b68a-4bc2e99aef9d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/22/2017
ms.author: alkohli
ms.openlocfilehash: 99b76e3bc2939c65654cbf606fda6f8a45e0c44b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Przywracania woluminu StorSimple z zestawu kopii zapasowych (Update 2)
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Omówienie
**Katalogu kopii zapasowej** na stronie są wyświetlane wszystkie zestawy kopii zapasowych, które są tworzone podczas ręczne lub automatyczne kopie zapasowe są pobierane. Ta strona służy do listy i zarządzanie kopiami zapasowymi, Przywróć z zestawu kopii zapasowych lub klonowania woluminu.

 ![Strona katalogu kopii zapasowej](./media/storsimple-restore-from-backup-set-u2/restore.png)

W tym samouczku wyjaśniono, jak używać **katalogu kopii zapasowej** strony, aby przywrócić urządzenia z zestawu kopii zapasowych.

Można przywrócić woluminu z dysku lokalnego lub kopia zapasowa w chmurze. W obu przypadkach operacji przywracania łączy woluminu w trybie online, podczas gdy dane są pobierane w tle. 

## <a name="before-you-restore"></a>Przed przywróceniem
Przed rozpoczęciem operacji przywracania należy zwrócić uwagę z następującymi zastrzeżeniami:

* **Przełącz do trybu offline wolumin** — zająć woluminu w trybie offline na hoście i urządzenia przed rozpoczęciem operacji przywracania. Mimo że operacji przywracania automatycznie go przywrócić do trybu online na urządzeniu, należy ręcznie przełączyć urządzenia w trybie online na hoście. Wolumin w trybie online można przełączyć na hoście, gdy wolumin jest w trybie online na urządzeniu. (Nie trzeba czekać do momentu zakończenia operacji przywracania.) Procedury, przejdź do [Przełącz do trybu offline wolumin](storsimple-manage-volumes-u2.md#take-a-volume-offline).
* **Typ woluminu po przywróceniu** — usunięte woluminy są przywracane na podstawie typu w migawce. Woluminy, które zostały przypięty lokalnie zostaną przywrócone jako woluminów przypiętych lokalnie i woluminów, które zostały do warstwy zostaną przywrócone jako woluminy warstwowe.
  
    Istniejące woluminy bieżący typ użycia woluminu zastępuje typ, który jest przechowywany w migawce. Na przykład jeśli wolumin jest Przywracanie z migawki, która została podjęta, jeśli typ woluminu został warstwowy i typ woluminu jest teraz przypięty lokalnie (z powodu operacji konwersji), wolumin jest przywracany jako woluminu przypiętego lokalnie. Podobnie jeśli istniejącego woluminu przypiętego lokalnie jest rozwinięty, a następnie przywrócić z starsze migawką gdy wolumin był mniejszy, przywróconej woluminu zachowuje bieżący rozmiar po rozwinięciu.
  
    Nie można przekonwertować woluminu z woluminu warstwowego woluminu przypiętego lokalnie lub _odwrotnie_ podczas przywracania woluminu. Poczekaj na zakończenie operacji przywracania, a następnie przekonwertować wolumin do innego typu. Aby uzyskać informacje o konwersji woluminu, przejdź do [zmienić typ woluminu](storsimple-manage-volumes-u2.md#change-the-volume-type). 
* **Rozmiar woluminu jest widoczny w przywróconej woluminu** — to jest ważną kwestią w przypadku przywracania woluminu przypiętego lokalnie, która została usunięta (ponieważ woluminów przypiętych lokalnie są w pełni zaaprowizowanym). Upewnij się, że masz wystarczającą ilość miejsca na przed próbą przywrócenia woluminu przypiętego lokalnie, który został wcześniej usunięty. 
* **Nie można rozszerzyć woluminu, gdy jest przywracana** — poczekaj na zakończenie operacji przywracania przed przystąpieniem do zwiększenia woluminu. Aby uzyskać informacje o rozszerzaniu woluminu, przejdź do [zmodyfikować woluminu](storsimple-manage-volumes-u2.md#modify-a-volume).
* **Podczas przywracania woluminu lokalnego można wykonywać kopii zapasowej** — dla procedury przejdź do [usługi Menedżer StorSimple umożliwia zarządzanie zasadami tworzenia kopii zapasowej](storsimple-manage-backup-policies.md).
* **Możesz anulować operacji przywracania** — Jeśli anulujesz zadania przywracania, a następnie wolumin jest przywracana do stanu sprzed zainicjował przywracania. Procedury, przejdź do [anulować zadanie](storsimple-manage-jobs-u2.md#cancel-a-job).

## <a name="how-does-restore-work"></a>Jak przywrócić pracę
Dla urządzeń z systemem aktualizacji 4 lub nowszym systemem heatmap przywracania jest zaimplementowana. Jak host żądania do dostępu do danych połączyć się z urządzeniem, te żądania są śledzone, i heatmap zostanie utworzony. Wysoka częstość powoduje fragmentów danych z wyższej cieplna natomiast mniejszej szybkości żądania przekłada się na fragmenty o niższych interakcji. Muszą uzyskać dostęp do co najmniej danych dwa razy może być oznaczony jako _gorących_. Plik jest modyfikowany jest również oznaczona jako _gorących_. Po rozpoczęciu przywracania aktywnego dodawaniem danych występuje oparte na heatmap. W wersjach wcześniejszych niż aktualizacja 4, danych pobrano podczas przywracania oparte na dostęp tylko. 

Na podstawie Heatmap śledzenia jest włączona tylko woluminami warstwowymi i przypięty lokalnie woluminy nie są obsługiwane. Przywracanie na podstawie Heatmap również nie jest obsługiwana w przypadku klonowania woluminu na innym urządzeniu. Jeśli istnieje przywracania w miejscu i firma Microsoft nie rehydrate (jak dane są już dostępne w lokalnie), a następnie na urządzeniu, istnieje lokalne migawki woluminu, który ma zostać przywrócony. Domyślnie podczas przywracania, Preparaty nawadniające zadań są inicjowane z wyprzedzeniem rehydrate dane oparte na heatmap. W pakiecie Update 4 poleceń cmdlet programu Windows PowerShell można zbadać uruchomionych zadań Preparaty nawadniające anulować zadanie Preparaty nawadniające i Pobierz stan zadania Preparaty nawadniające.

* `Get-HcsRehydrationJob`— To polecenie cmdlet pobiera stan zadania Preparaty nawadniające. Zadanie pojedynczego Preparaty nawadniające zostanie wywołany dla jednego woluminu.
* `Set-HcsRehydrationJob`— To polecenie cmdlet pozwala na wstrzymanie, Zatrzymaj, Wznów zadanie Preparaty nawadniające, gdy trwa Preparaty nawadniające. 

Aby uzyskać więcej informacji dotyczących poleceń cmdlet Preparaty nawadniające, przejdź do [Dokumentacja poleceń cmdlet programu Windows PowerShell dla StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

Z automatycznego rehdyration zwykle wyższa wydajność odczytu przejściowej oczekuje. Rzeczywiste magniutde ulepszeń zależy od różnych czynników, takich jak wzorca dostępu, przenoszenia danych i typ danych. Aby anulować zadanie Preparaty nawadniające, używając polecenia cmdlet programu PowerShell. Jeśli chcesz trwale wyłączyć Preparaty nawadniające zadania dla wszystkich przyszłych operacji przywracania, skontaktuj się z Microsoft Support.

## <a name="how-to-use-the-backup-catalog"></a>Jak używać katalogu kopii zapasowej
**Katalogu kopii zapasowej** strona zawiera zaznaczyć kwerendę, która pomaga ograniczyć kopii zapasowej. Można filtrować zestawy kopii zapasowych, które są pobierane na podstawie następujących parametrów:

* **Urządzenie** — urządzenia, na którym utworzono ten zestaw kopii zapasowych.
* **Wykonaj kopię zapasową zasad** lub **woluminu** — zasady tworzenia kopii zapasowej lub woluminie skojarzonym z tego zestawu kopii zapasowych.
* **Z** i **do** — zakres dat i godzin przy tworzeniu zestawu kopii zapasowych.

Następnie filtrowane zestawy kopii zapasowych wyszczególniono na podstawie następujących atrybutów:

* **Nazwa** — Nazwa zasad tworzenia kopii zapasowej lub woluminie skojarzonym z zestawu kopii zapasowych.
* **Rozmiar** — rzeczywisty rozmiar zestawu kopii zapasowych.
* **Utworzony na** — Data i godzina utworzenia kopii zapasowych. 
* **Typ** — zestawy kopii zapasowych można migawki lokalne lub migawki w chmurze. Migawka lokalna jest kopię zapasową wszystkich danych woluminu lokalnie przechowywanych na urządzeniu. Migawka w chmurze odnosi się do kopii zapasowej danych woluminu znajdującej się w chmurze. Migawki lokalne zapewnić szybszy dostęp migawki w chmurze są wybrać, aby zachować odporność danych.
* **Zainicjowane przez** — tworzenie kopii zapasowych może zostać zainicjowane automatycznie, zgodnie z harmonogramem lub ręcznie przez użytkownika. (Aby zaplanować tworzenie kopii zapasowych można użyć zasad tworzenia kopii zapasowej. Alternatywnie można użyć **utworzenia kopii zapasowej** opcję, aby wykonać interakcyjne kopii zapasowej.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>Jak przywrócić z kopii zapasowej woluminu StorSimple
Można użyć **katalogu kopii zapasowej** strony, aby przywrócić woluminu StorSimple z określonej kopii zapasowej. Należy jednak pamiętać, Przywracanie woluminu powróci do stanu, w jakim był po wykonaniu kopii zapasowej woluminu. Dane, które zostały dodane po operacji tworzenia kopii zapasowej zostaną utracone.

> [!WARNING]
> Przywracanie z kopii zapasowej zastępuje istniejące woluminy z kopii zapasowej. Może to spowodować utratę wszystkich danych, który został zapisany po wykonaniu kopii zapasowej.
> 
> 

### <a name="to-restore-your-volume"></a>Aby przywrócić woluminu
1. Na stronie usługi Menedżer StorSimple, kliknij przycisk **katalog kopii zapasowej** kartę.
   
    ![Katalog kopii zapasowej](./media/storsimple-restore-from-backup-set-u2/restore.png)
2. Wybierz kopię zapasową ustawione w następujący sposób:
   
   1. Wybierz odpowiednie urządzenie.
   2. Na liście rozwijanej wybierz zasady woluminu lub kopii zapasowej do utworzenia kopii zapasowej, który chcesz wybrać.
   3. Określ zakres czasu.
   4. Kliknij ikonę znacznika wyboru ![ikona znacznika wyboru](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) do wykonania tego zapytania.
      
      Tworzenie kopii zapasowych skojarzony z wybranego woluminu lub zasad tworzenia kopii zapasowej powinien zostać wyświetlony na liście zestawów kopii zapasowych.
3. Rozwiń zestaw, aby wyświetlić skojarzone woluminy kopii zapasowych. Te woluminy muszą być pobierane w trybie offline na hoście i urządzenia, zanim będzie można ich przywrócić. Dostęp do woluminów na **kontenery woluminów** strony, a następnie postępuj zgodnie z instrukcjami [Przełącz do trybu offline wolumin](storsimple-manage-volumes-u2.md#take-a-volume-offline) ich przełączenia do trybu offline.
   
   > [!IMPORTANT]
   > Upewnij się, że została wykonana woluminy w tryb offline na hoście najpierw, przed podjęciem dalszych woluminy w tryb offline na urządzeniu. Jeśli nie przyjmują woluminy w tryb offline na hoście, może prowadzić do uszkodzenia danych.
   > 
   > 
4. Przejdź z powrotem do **katalogu kopii zapasowej** i wybierz zestaw kopii zapasowych.
5. Kliknij przycisk **przywrócić** w dolnej części strony.
6. Zostanie wyświetlony monit o potwierdzenie. Przejrzyj informacje przywracania, a następnie zaznacz pole wyboru potwierdzenia.
   
    ![Strona potwierdzenia](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)
7. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). Rozpoczyna zadanie przywracania. Możesz wyświetlić zadania, uzyskując dostęp do **zadania** strony. 
8. Po zakończeniu przywracania można sprawdzić, czy zawartość woluminów są zastępowane przez woluminy z kopii zapasowej.

![Zobacz film](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Zobacz film**

Aby obejrzeć film wideo przedstawiający sposób użycia klonu i przywrócić funkcji w programie StorSimple, aby odzyskać usunięte pliki, kliknij przycisk [tutaj](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Jeśli jej nie powiedzie się.
Otrzymuj alert, jeśli operacja przywracania nie powiedzie się z dowolnej przyczyny. W takim przypadku należy odświeżyć listy kopii zapasowych, aby sprawdzić, czy kopia zapasowa jest nadal ważny. Jeśli kopia zapasowa jest prawidłowa i że jest przywracana z chmury, następnie problemy z połączeniem mogą być przyczyną problemu. 

Aby ukończyć operację przywracania, zająć woluminu w trybie offline na hoście i spróbuj ponownie wykonać operację przywracania. Wszelkie modyfikacje danych woluminu, które zostały wykonane podczas procesu przywracania zostaną utracone.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [woluminów StorSimple zarządzanie](storsimple-manage-volumes-u2.md).
* Dowiedz się, jak [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

