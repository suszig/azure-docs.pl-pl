---
title: "Instalowanie portalu klasycznego urządzenia serii StorSimple 8000 z aktualizacją Update 5 | Dokumentacja firmy Microsoft"
description: "Wyjaśniono, jak zainstalować StorSimple 8000 serii aktualizacji 5 na urządzeniu z serii StorSimple 8000 w klasycznym portalu."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/10/2017
ms.author: alkohli
ms.openlocfilehash: 9d8dc7aebbeea7ad428be4af66e4e991f60c8301
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2017
---
# <a name="install-update-5-on-your-storsimple-device"></a>Instalowanie aktualizacji 5 na urządzeniu StorSimple

## <a name="overview"></a>Omówienie

W tym samouczku przedstawiono sposób instalacji aktualizacji 5 na urządzeniu StorSimple uruchomiona starsza wersja oprogramowania za pośrednictwem klasycznego portalu Azure i przy użyciu metody poprawki. Metoda poprawki jest używany podczas próby zainstalowania aktualizacji 5 na urządzeniu z systemem w wersji 3 przed aktualizacją. Metoda poprawki służy także Jeśli brama jest skonfigurowana w interfejsie sieciowym innym niż dane 0 urządzenia StorSimple i są próby aktualizacji z wersji oprogramowania 1 przed aktualizacją.

Aktualizacja 5 obejmuje oprogramowanie urządzenia, Storport i Spaceport, aktualizacje zabezpieczeń systemu operacyjnego i aktualizacje systemu operacyjnego, a aktualizacje oprogramowania układowego dysku.  Oprogramowanie urządzenia, Spaceport Storport, zabezpieczeń i inne aktualizacje systemu operacyjnego są Brak aktualizacji. Brak lub regularne aktualizacje mogą być stosowane za pośrednictwem klasycznego portalu Azure lub za pomocą metody poprawki. Aktualizacje oprogramowania układowego dysku destrukcyjne aktualizacji i są stosowane, gdy urządzenie jest w trybie konserwacji za pomocą metody poprawek za pomocą interfejsu programu Windows PowerShell urządzenia.

> [!IMPORTANT]
> * Zestaw ręczne i automatyczne wstępne sprawdzanie gotowe czasu zainstalowania w celu określenia kondycji urządzenia pod względem sprzętu stanu i łączność sieciową. Kontrole wstępne są wykonywane tylko wtedy, gdy należy zastosować aktualizacje z portalu Azure.
> * Jeśli używasz wersji przed Update 3 zdecydowanie zaleca się zainstalowanie aktualizacji 5 za pomocą metody poprawki. Aby ułatwić obsługę przeprowadzenia aktualizacji, [dziennika biletu pomocy technicznej](storsimple-8000-contact-microsoft-support.md).
> * Jeśli używasz Update 3 i później, zaleca się zainstalowanie oprogramowania i inne regularne aktualizacje za pośrednictwem klasycznego portalu Azure. W zależności od wersji aktualizujesz z aktualizacji może potrwać 4 godziny (lub nowszego) do zainstalowania. Aktualizacje trybu konserwacji musi być zainstalowany za pośrednictwem interfejsu programu Windows PowerShell urządzenia. Aktualizacje trybu konserwacji są destrukcyjne aktualizacje, te spowodować dół czasu dla danego urządzenia.
> * Uruchomiona opcjonalne StorSimple Snapshot Manager, upewnij się, że wersji Snapshot Manager zostały uaktualnione do aktualizacji 5 przed zaktualizowaniem urządzenia.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-5-via-the-azure-classic-portal"></a>Instalowanie aktualizacji 5 za pomocą klasycznego portalu Azure
Wykonaj poniższe kroki, aby zaktualizować urządzenie do [aktualizacji 5](storsimple-update5-release-notes.md).

> [!NOTE]
> Microsoft ściąga dodatkowych informacji diagnostycznych z urządzenia. W związku z tym gdy działu operacji identyfikuje urządzenia, które występują problemy, firma Microsoft mają większe możliwości zbierania informacji z urządzenia i diagnozowanie problemów.

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

Sprawdź, czy urządzenie działa **StorSimple 8000 serii aktualizacji 5 (6.3.9600.17845)**. **Ostatniej aktualizacji daty** powinien być modyfikowany.

* Teraz zobaczysz, że są dostępne aktualizacje tryb konserwacji (ten komunikat może nadal wyświetlane przez 24 godziny, po zainstalowaniu aktualizacji). Aktualizacje trybu konserwacji są destrukcyjne aktualizacje, które powoduje przestój urządzenia i mogą być stosowane tylko za pośrednictwem interfejsu programu Windows PowerShell urządzenia.

* Pobierz aktualizacje tryb konserwacji przy użyciu kroków opisanych w [do pobrania poprawek](#to-download-hotfixes) do wyszukania i pobrania KB4037263, które instaluje aktualizacje oprogramowania układowego dysku (inne aktualizacje powinny być zainstalowane przez teraz). Wykonaj czynności opisane w [zainstalowany i sprawdź poprawki trybu konserwacji](#to-install-and-verify-maintenance-mode-hotfixes) trybu konserwacji do zainstalowania aktualizacji.

## <a name="install-update-5-as-a-hotfix"></a>Instalowanie aktualizacji 5 jako poprawki


Dostępne są następujące wersje oprogramowania, które można uaktualnić za pomocą metody poprawki:

* Zaktualizuj 0,1, 0,2, 0,3
* Aktualizacja 1, 1.1 i 1.2
* Aktualizacja 2, 2.1, 2.2
* Aktualizacja 3 w wersji 3.1
* Aktualizacja 4

> [!NOTE]
> Zaleca się instalowanie aktualizacji 5 odbywa się za pośrednictwem klasycznego portalu Azure. Jednak jeśli używasz wersji przed Update 3 Firma Microsoft zaleca użycie tej metody do zainstalowania aktualizacji 5. Należy również Użyj tej procedury, jeśli nie wyboru bramy, podczas próby zainstalowania aktualizacji za pośrednictwem klasycznego portalu Azure. Sprawdzenie nie powiedzie się, gdy brama przypisany do interfejsu sieciowego 0-DATA urządzeniu jest uruchomiona wersja oprogramowania starszych niż Update 1.

Metoda poprawki obejmuje następujące trzy kroki:

1. Pobierz poprawki z wykazu usługi Microsoft Update.
2. Zainstaluj i sprawdź regularne poprawki.
3. Zainstaluj i sprawdź poprawkę trybu konserwacji.

#### <a name="download-updates-for-your-device"></a>Pobierz aktualizacje dla urządzenia

Należy pobrać i zainstalować poniższe poprawki w określonej kolejności i sugerowane folderów:

| Kolejność | KB | Opis | Typ aktualizacji | Godzina instalacji |Zainstaluj w folderze|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4037264 |Aktualizacja oprogramowania<br> Pobierz oba _HcsSfotwareUpdate.exe_ i _CisMSDAgent.exe_ |Regularne <br></br>Bezproblemowa |~ 25 minut. |FirstOrderUpdate|

Aktualizowania z urządzenia z uruchomioną aktualizacji 4, wystarczy zainstalować aktualizacje zbiorcze systemu operacyjnego jako drugi aktualizacje kolejności.

| Kolejność | KB | Opis | Typ aktualizacji | Godzina instalacji |Zainstaluj w folderze|
| --- | --- | --- | --- | --- | --- |
| 2A. |KB4025336 |Pakiet aktualizacji zbiorczych systemu operacyjnego <br> Pobierz wersję systemu Windows Server 2012 R2 |Regularne <br></br>Bezproblemowa |- |SecondOrderUpdate|

W przypadku instalowania z urządzenia z uruchomioną aktualizacji 3 lub starszym, należy zainstalować następujące oprócz aktualizacje zbiorcze.

| Kolejność | KB | Opis | Typ aktualizacji | Godzina instalacji |Zainstaluj w folderze|
| --- | --- | --- | --- | --- | --- |
| 2B. |KB4011841 <br> KB4011842 |Sterownik LSI i aktualizacje oprogramowania układowego <br> Aktualizacja oprogramowania układowego należy (wersja 3.38) |Regularne <br></br>Bezproblemowa |~ 3 godziny <br> (dotyczy również 2A. + 2B. + 2 C.)|SecondOrderUpdate|
| 2C. |KB3139398 <br> KB3142030 <br> KB3108381 <br> KB3153704 <br> KB3174644 <br> KB3139914   |Pakiet aktualizacji zabezpieczeń systemu operacyjnego <br> Pobierz wersję systemu Windows Server 2012 R2 |Regularne <br></br>Bezproblemowa |- |SecondOrderUpdate|
| 2W. |KB3146621 <br> KB3103616 <br> KB3121261 <br> KB3123538 |Pakiet aktualizacji systemu operacyjnego <br> Pobierz wersję systemu Windows Server 2012 R2 |Regularne <br></br>Bezproblemowa |- |SecondOrderUpdate|


Należy również zainstalować aktualizacje oprogramowania układowego dysku na wszystkich aktualizacji zamieszczone w poprzednich tabelach. Możesz sprawdzić, czy potrzebujesz aktualizacje oprogramowania układowego dysku, uruchamiając `Get-HcsFirmwareVersion` polecenia cmdlet. Jeśli używasz tych wersji oprogramowania układowego: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N003`, `0107`, a następnie nie trzeba zainstalować te aktualizacje.

| Kolejność | KB | Opis | Typ aktualizacji | Godzina instalacji | Zainstaluj w folderze|
| --- | --- | --- | --- | --- | --- |
| 3. |KB4037263 |Oprogramowanie układowe dysku |Konserwacji <br></br>Problem |~ 30 minut. | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Aktualizowania z aktualizacją Update 4, czas instalacji całkowita jest bliski 4 godziny.
> * Przed użyciem tej procedury, aby zastosować aktualizację, upewnij się, że kontrolery urządzeń są w trybie online i wszystkie składniki sprzętowe są w dobrej kondycji.

Wykonaj poniższe kroki, aby pobrać i zainstalować poprawki.

[!INCLUDE [storsimple-install-update5-hotfix](../../includes/storsimple-install-update5-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [wersji aktualizacji 5](storsimple-update5-release-notes.md).

