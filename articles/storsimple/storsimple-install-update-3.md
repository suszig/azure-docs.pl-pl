---
title: "Instalowanie aktualizacji 3 na urządzeniu StorSimple | Dokumentacja firmy Microsoft"
description: "Wyjaśniono, jak zainstalować StorSimple 8000 serii aktualizacji 3 na urządzeniu z serii StorSimple 8000."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c6c4634d-4f3a-4bc4-b307-a22bf18664e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 72b004a6c2604e0fc20b71b4b69217622f8f9ea0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-3-on-your-storsimple-8000-series-device"></a>Zainstaluj na urządzeniu z serii StorSimple 8000 z aktualizacją Update 3

## <a name="overview"></a>Omówienie

W tym samouczku przedstawiono sposób instalacji aktualizacji 3 na urządzeniu StorSimple uruchomiona starsza wersja oprogramowania za pośrednictwem klasycznego portalu Azure i przy użyciu metody poprawki. Metoda poprawki jest używana, gdy brama jest skonfigurowany w interfejsie sieciowym innym niż dane 0 urządzenia StorSimple i są próby aktualizacji z wersji oprogramowania 1 przed aktualizacją.

Aktualizacja 3 obejmuje oprogramowanie urządzenia, LSI sterowników i oprogramowania układowego, Storport i aktualizuje Spaceport. Jeśli aktualizacja Update 2 lub starszej wersji, będzie również wymagane do zastosowania iSCSI, usługi WMI, a w niektórych przypadkach dysku aktualizacje oprogramowania układowego. Oprogramowanie urządzenia, WMI iSCSI, LSI sterownika, Spaceport i Storport poprawki Brak aktualizacji i mogą być stosowane za pośrednictwem klasycznego portalu Azure. Aktualizacje oprogramowania układowego dysku destrukcyjne aktualizacji i mogą być stosowane tylko za pośrednictwem interfejsu programu Windows PowerShell urządzenia. 

> [!IMPORTANT]
> * Zestaw ręczne i automatyczne wstępne sprawdzanie gotowe czasu zainstalowania w celu określenia kondycji urządzenia pod względem sprzętu stanu i łączność sieciową. Kontrole wstępne są wykonywane tylko wtedy, gdy należy zastosować aktualizacje z klasycznego portalu Azure.
> * Zaleca się zainstalowanie aktualizacji oprogramowania i sterowników za pośrednictwem klasycznego portalu Azure. Tylko powinien możesz przejść do interfejsu programu Windows PowerShell, urządzenia (w celu instalowania aktualizacji), jeśli sprawdzenie przed aktualizacją bramy nie powiedzie się w portalu. W zależności od wersji aktualizowanej z aktualizacji może potrwać godzin w wersji 1.5 — 2.5 do zainstalowania. Aktualizacje trybu konserwacji musi być zainstalowany za pośrednictwem interfejsu programu Windows PowerShell urządzenia. Aktualizacje trybu konserwacji są aktualizacje destrukcyjne, te spowoduje dół czasu dla danego urządzenia.
> * Uruchomiona opcjonalne StorSimple Snapshot Manager, upewnij się, że uaktualniono wersji Snapshot Manager do wersji Update 2 przed zaktualizowaniem urządzenia.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-the-azure-classic-portal"></a>Instalowanie aktualizacji 3 za pośrednictwem klasycznego portalu Azure
Wykonaj poniższe kroki, aby zaktualizować urządzenie do [Update 3](storsimple-update3-release-notes.md).

> [!NOTE]
> Stosowania Update 2 lub nowszym (w tym Update 2.1) firmy Microsoft będzie można ściągnąć dodatkowych informacji diagnostycznych z urządzenia. W związku z tym gdy działu operacji identyfikuje urządzenia, które występują problemy, firma Microsoft mają większe możliwości zbierania informacji z urządzenia i diagnozowanie problemów. Akceptowanie Update 2 lub nowszej, umożliwia firmie Microsoft w celu obsługi aktywne.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

Sprawdź, czy urządzenie działa **StorSimple 8000 serii Update 3 (6.3.9600.17759)**. **Ostatniej aktualizacji daty** również powinien być modyfikowany. 
   - Jeśli aktualizacja z wersji przed Update 2, pojawi się także o dostępnych aktualizacjach tryb konserwacji (ten komunikat może nadal wyświetlane przez 24 godziny, po zainstalowaniu aktualizacji).
     Aktualizacje trybu konserwacji są destrukcyjne aktualizacje, które powoduje przestój urządzenia i mogą być stosowane tylko za pośrednictwem interfejsu programu Windows PowerShell urządzenia. W niektórych przypadkach po uruchomieniu 1.2 aktualizacji oprogramowania układowego dysku mogą być już aktualne, w którym to przypadku nie trzeba instalować żadnych aktualizacji w trybie konserwacji.
   - W przypadku aktualizacji z Update 2 lub nowszej, urządzenie powinno być teraz aktualne. Następny krok można pominąć.

Pobierz aktualizacje tryb konserwacji przy użyciu kroków opisanych w [do pobrania poprawek](#to-download-hotfixes) do wyszukania i pobrania KB3121899, które instaluje aktualizacje oprogramowania układowego dysku (inne aktualizacje powinny być zainstalowane przez teraz). Wykonaj czynności opisane w [zainstalowany i sprawdź poprawki trybu konserwacji](#to-install-and-verify-maintenance-mode-hotfixes) trybu konserwacji do zainstalowania aktualizacji. 

## <a name="install-update-3-as-a-hotfix"></a>Instalowanie aktualizacji 3 jako poprawki
Użyj tej procedury, jeśli nie wyboru bramy, podczas próby zainstalowania aktualizacji za pośrednictwem klasycznego portalu Azure. Sprawdzenie nie powiedzie się, masz przypisane do karty sieciowej 0-DATA bramy i urządzenia z wersją oprogramowania przed Update 1.

Dostępne są następujące wersje oprogramowania, które można uaktualnić za pomocą metody poprawki:

* Zaktualizuj 0,1, 0,2, 0,3
* Aktualizacja 1, 1.1 i 1.2
* Aktualizacja 2, 2.1, 2.2 

> [!IMPORTANT]
> * Jeśli urządzenie korzysta z wersji Release (GA), skontaktuj się z [Microsoft Support](storsimple-contact-microsoft-support.md) pomocne podczas aktualizacji.
> 
> 

Metoda poprawki obejmuje następujące trzy kroki:

1. Pobierz poprawki z wykazu usługi Microsoft Update.
2. Zainstaluj i sprawdź regularne poprawki.
3. Zainstaluj i sprawdź poprawkę trybu konserwacji (tylko wtedy, gdy aktualizacje oprogramowania 2 przed aktualizacją).

#### <a name="download-updates-for-your-device"></a>Pobierz aktualizacje dla urządzenia
**Jeśli urządzenie działa Update 2.1 lub 2.2**, musisz pobrać i zainstalować poniższe poprawki w określonej kolejności:

| Kolejność | KB | Opis | Typ aktualizacji | Godzina instalacji |
| --- | --- | --- | --- | --- |
| 1. |KB3186843 |Aktualizacja oprogramowania &#42; |Regularne <br></br>Bezproblemowa |~ 45 minut. |
| 2. |KB3186859 |LSI sterowników i oprogramowania układowego |Regularne <br></br>Bezproblemowa |~ 20 minut. |
| 3. |KB3121261 |Poprawka Storport i Spaceport </br> Windows Server 2012 R2 |Regularne <br></br>Bezproblemowa |~ 20 minut. |

&#42;  *Należy pamiętać, że aktualizacja oprogramowania składa się z dwóch plików binarnych: Aktualizacja urządzenia poprzedzone znakiem `all-hcsmdssoftwareupdate` i agenta konfiguracji (ci) i usług Mds poprzedzone znakiem `all-cismdsagentupdatebundle`. Urządzenia aktualizacji oprogramowania należy skonfigurować przed agenta konfiguracji (ci) i usług Mds. Należy również ponownie uruchomić kontroler za pośrednictwem usługi active `Restart-HcsController` zaktualizować polecenia cmdlet, po zainstalowaniu agenta konfiguracji (ci) i usług Mds (i przed stosowania pozostałych aktualizacji).* 

**Jeśli urządzenie działa Update 0.1, 0.2, 0.3, 1.0, 1.1, 1.2 lub 2.0**, należy pobrać i zainstalować poniższe poprawki oprócz oprogramowania, LSI sterowników i oprogramowania układowego aktualizacji (przedstawione w powyższej tabeli), w określonej kolejności:

| Kolejność | KB | Opis | Typ aktualizacji | Godzina instalacji |
| --- | --- | --- | --- | --- |
| 4. |KB3146621 |Pakiet iSCSI |Regularne <br></br>Bezproblemowa |~ 20 minut. |
| 5. |KB3103616 |Pakiet usługi WMI |Regularne <br></br>Bezproblemowa |~ 12 minut. |

<br></br>

**Jeśli urządzenie korzysta z wersji 0,2, 0,3, 1.0, 1.1 i 1.2**, należy również zainstalować aktualizacje oprogramowania układowego dysku na wszystkich aktualizacji zamieszczone w poprzednich tabelach. Możesz sprawdzić, czy potrzebujesz aktualizacje oprogramowania układowego dysku, uruchamiając `Get-HcsFirmwareVersion` polecenia cmdlet. Jeśli używasz tych wersji oprogramowania układowego: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, a następnie nie trzeba zainstalować te aktualizacje.

| Kolejność | KB | Opis | Typ aktualizacji | Godzina instalacji |
| --- | --- | --- | --- | --- |
| 6. |KB3121899 |Oprogramowanie układowe dysku |Konserwacji <br></br>Problem |~ 30 minut. |

<br></br>

> [!IMPORTANT]
> * Ta procedura musi zostać wykonana tylko raz do zastosowania aktualizacji 3. Klasyczny portal Azure umożliwia stosowanie kolejnych aktualizacji.
> * Aktualizowania z 2.2 aktualizacji, godzinę instalacji całkowita jest bliski 1.1 godzin.
> * Przed użyciem tej procedury, aby zastosować aktualizację, upewnij się, że kontrolery urządzeń są w trybie online i wszystkie składniki sprzętowe są w dobrej kondycji.
> 
> 

Wykonaj poniższe kroki, aby pobrać i zainstalować poprawki.

[!INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [wersji Update 3](storsimple-update3-release-notes.md).

