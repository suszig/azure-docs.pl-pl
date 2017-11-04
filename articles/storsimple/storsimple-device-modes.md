---
title: "Zmień tryb urządzenia StorSimple | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano tryby urządzenia StorSimple i wyjaśniono, jak zmienić tryb urządzenia za pomocą programu Windows PowerShell dla urządzenia StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: e9d7d277-8a2f-45eb-9fef-355486e14cbc
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2016
ms.author: alkohli
ms.openlocfilehash: 33c65bf2eecff3914f3227c76f7d638a4507e1f6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="change-the-device-mode-on-your-storsimple-device"></a>Zmień tryb urządzenia na urządzeniu StorSimple
Ten artykuł zawiera krótki opis różnych trybach, w których może działać urządzenia StorSimple. Urządzenia StorSimple może działać w trzech trybów: Normalny, obsługi i odzyskiwania. 

Po przeczytaniu tego artykułu, oznacza to:

* Jakie tryby urządzenia StorSimple są
* Znajduje się w sposób dowiedzieć się, który tryb urządzenia StorSimple
* Jak zmienić z normalnym i tryb konserwacji i *na odwrót*

Powyżej zadań zarządzania można wykonać tylko za pośrednictwem interfejsu programu Windows PowerShell urządzenia StorSimple.

## <a name="about-storsimple-device-modes"></a>Temat trybów urządzenia StorSimple
Urządzenia StorSimple może działać w trybie normalnym, konserwacji lub odzyskiwania. Każdy z tych trybów krótko opisano poniżej.

### <a name="normal-mode"></a>Tryb normalny
To jest zdefiniowana jako normalne tryb operacyjnych urządzenie StorSimple w pełni skonfigurowany. Domyślnie urządzenia powinny być w trybie normalnym.

### <a name="maintenance-mode"></a>Tryb konserwacji
Czasami urządzenia StorSimple może być konieczne umieszczenie w trybie konserwacji. Ten tryb służy do przeprowadzania konserwacji na urządzeniu i zainstaluj aktualizacje destrukcyjne, takich jak powiązane z oprogramowania układowego dysku.

System można umieścić w trybie konserwacji tylko przy użyciu programu Windows PowerShell dla urządzenia StorSimple. Wszystkie żądania We/Wy są wstrzymane w tym trybie. Usługi, takie jak trwałej pamięci (NVRAM) lub usługę klastrowania również zostały zatrzymane. Zarówno kontrolery są ponownie uruchamiane po wprowadzeniu lub opuścić ten tryb. Po wyjściu z trybu konserwacji, wszystkie usługi zostanie wznowiona i powinna być w dobrej kondycji. Może to potrwać kilka minut.

> [!NOTE]
> **Tryb konserwacji jest obsługiwana tylko na urządzeniu działa prawidłowo. Nie jest obsługiwane na urządzeniu, w którym jednego lub obu kontrolerów nie działają.**
> </br>
> 
> 

### <a name="recovery-mode"></a>Tryb odzyskiwania
Tryb odzyskiwania można określić jako "Tryb awaryjny dla systemu Windows z obsługą sieci". Tryb odzyskiwania angażujący zespołu Support firmy Microsoft i umożliwia im wykonywanie diagnostyki w systemie. Podstawowym celem trybu odzyskiwania jest pobrać dzienniki systemu.

System przechodzi do trybu odzyskiwania, należy skontaktować się Microsoft Support dalsze czynności. Aby uzyskać więcej informacji, przejdź do [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).

> [!NOTE]
> **Nie można umieścić urządzenia w trybie odzyskiwania. Jeśli urządzenie jest w złym stanie, tryb odzyskiwania próbuje pobrać urządzenia do stanu, w której personel firmy Microsoft Support można ją zbadać.**
> 
> 

## <a name="determine-storsimple-device-mode"></a>Określić tryb urządzenia StorSimple
#### <a name="to-determine-the-current-device-mode"></a>Aby określić bieżący tryb urządzenia
1. Zaloguj się do konsoli szeregowej urządzenia, wykonując kroki opisane w [przy użyciu programu PuTTY można nawiązać połączenia z konsolą szeregową urządzenia](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Sprawdź komunikat transparentu, w menu konsoli szeregowej urządzenia. Ten komunikat oznacza jawnie, czy urządzenie jest w trybie konserwacji lub odzyskiwania. Jeśli komunikat nie zawiera żadnych określonych informacji dotyczących trybu systemu, urządzenie jest w trybie normalnym.

## <a name="change-the-storsimple-device-mode"></a>Zmień tryb urządzenia StorSimple
Urządzenia StorSimple można umieścić w tryb konserwacji (w trybie normalnym) do wykonania konserwacji lub instalowania aktualizacji tryb konserwacji. Wykonaj poniższe procedury, aby wprowadzić lub wyjść z trybu konserwacji.

> [!IMPORTANT]
> Przed trybu konserwacji, sprawdź, czy zarówno kontrolery urządzeń są w dobrej kondycji po zalogowaniu się do **stan sprzętu** na **konserwacji** strony w klasycznym portalu Azure. Jeśli jeden lub oba kontrolery nie są w dobrej kondycji, skontaktuj się z Microsoft Support następnych kroków. Aby uzyskać więcej informacji, przejdź do [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).
> 
> 

#### <a name="to-enter-maintenance-mode"></a>Aby przejść do trybu konserwacji
1. Zaloguj się do konsoli szeregowej urządzenia, wykonując kroki opisane w [przy użyciu programu PuTTY można nawiązać połączenia z konsolą szeregową urządzenia](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. W menu konsoli szeregowej wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**. Po wyświetleniu monitu podaj **hasło administratora urządzenia**. Domyślne hasło jest: `Password1`.
3. W wierszu polecenia wpisz 
   
    `Enter-HcsMaintenanceMode`
4. Zobaczysz komunikat ostrzegawczy informujący o trybu konserwacji będzie zakłócać wszystkich żądań We/Wy i sever połączenia do klasycznego portalu Azure i zostanie wyświetlony monit o potwierdzenie. Typ **Y** do trybu konserwacji.
5. Zarówno kontrolery zostanie uruchomiony ponownie. Po zakończeniu ponownego uruchomienia innego pojawi się komunikat wskazujący, że urządzenie jest w trybie konserwacji.

#### <a name="to-exit-maintenance-mode"></a>Aby wyjść z trybu konserwacji
1. Zaloguj się do konsoli szeregowej urządzenia. Sprawdź z komunikat transparentu, że urządzenie jest w trybie konserwacji.
2. W wierszu polecenia wpisz:
   
    `Exit-HcsMaintenanceMode`
3. Zostanie wyświetlony komunikat ostrzegawczy i komunikat potwierdzenia. Typ **Y** aby wyjść z trybu konserwacji.
4. Zarówno kontrolery zostanie uruchomiony ponownie. Po zakończeniu ponownego uruchomienia innego pojawi się komunikat wskazujący, że urządzenie jest w trybie normalnym.

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak [stosowania aktualizacji tryb normalny i konserwacja](storsimple-update-device.md) na urządzeniu StorSimple.

