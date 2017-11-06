---
title: "Zmienianie haseł za pomocą Menedżera urządzeń StorSimple | Dokumentacja firmy Microsoft"
description: "Informacje dotyczące używania usługi Menedżer StorSimple, aby zmienić swoje hasło administratora StorSimple Snapshot Manager i urządzeń."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: f178509c-f4e1-48a8-90b2-d4ad050eeb30
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: 843d0a4bcc1807a6b1995d067e0f9e572307775d
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>Zmienianie haseł StorSimple przy użyciu usługi Menedżer StorSimple
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Aby wyświetlić wersję tego artykułu dla nowego portalu Azure, przejdź do [usługi Menedżer StorSimple umożliwia zmianę hasła StorSimple](storsimple-8000-change-passwords.md). Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Omówienie
Klasyczny portal Azure **Konfiguruj** strona zawiera wszystkie parametry urządzenia, które można skonfigurować na urządzeniu StorSimple, który jest zarządzany przez usługę Menedżer StorSimple. W tym samouczku opisano, jak używasz **Konfiguruj** strony, aby zmienić administratora urządzenia lub hasło programu StorSimple Snapshot Manager.

## <a name="change-the-device-administrator-password"></a>Zmiana hasła administratora urządzenia
Korzystając z interfejsu programu Windows PowerShell do uzyskania dostępu do urządzenia StorSimple, należy wprowadzić hasło administratora urządzenia. Po zarejestrowaniu pierwszego urządzenia StorSimple z usługą domyślne hasło dla tego interfejsu jest *Password1*. Dla bezpieczeństwa danych należy zmienić hasło po zakończeniu procesu rejestracji. W procesie rejestracji nie może zamknąć bez zmiany hasła. Aby uzyskać więcej informacji, zobacz [krok 3: Konfigurowanie i rejestrowanie urządzenia za pomocą środowiska Windows PowerShell dla StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Następnie można zmienić hasło, które ustawiono najpierw za pośrednictwem interfejsu programu Windows PowerShell podczas rejestracji za pomocą klasycznego portalu Azure. Wykonaj poniższe kroki, aby zmienić hasło administratora urządzenia.

#### <a name="to-change-the-device-administrator-password"></a>Aby zmienić hasło administratora urządzenia
1. W klasycznym portalu, kliknij przycisk **urządzeń** > **Konfiguruj** dla danego urządzenia.
2. Przewiń w dół do **hasło administratora urządzenia** sekcji. Podaj hasło administratora, który zawiera od 8 do 15 znaków. Hasło musi zawierać kombinację 3 lub więcej znaków wielkie litery, małe litery, liczbowych i specjalnych.
3. Potwierdź hasło.
4. Kliknij przycisk **Zapisz** w dolnej części strony.

Teraz należy uaktualnić hasło administratora urządzenia. To hasło modyfikacji umożliwia dostęp do interfejsu programu Windows PowerShell.

## <a name="change-the-storsimple-snapshot-manager-password"></a>Zmień hasło programu StorSimple Snapshot Manager
Oprogramowanie StorSimple Snapshot Manager jest zainstalowane na hoście z systemem Windows i umożliwia administratorom zarządzanie kopiami zapasowymi urządzenia StorSimple przez tworzenie migawek lokalnych i w chmurze.

Podczas konfigurowania urządzenia StorSimple Snapshot Manager, pojawi się monit o podanie adresu IP urządzenia i hasła do uwierzytelnienia urządzenia magazynu. To hasło jest już skonfigurowane za pośrednictwem interfejsu programu Windows PowerShell. Aby uzyskać więcej informacji, zobacz [krok 3: Konfigurowanie i rejestrowanie urządzenia za pomocą środowiska Windows PowerShell dla StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Następnie można zmienić hasło, które ustawiono najpierw za pośrednictwem interfejsu programu Windows PowerShell podczas rejestracji w portalu klasycznym. Wykonaj poniższe kroki, aby zmienić hasło programu StorSimple Snapshot Manager.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>Aby zmienić hasło programu StorSimple Snapshot Manager
1. W klasycznym portalu, kliknij przycisk **urządzeń** > **Konfiguruj** dla danego urządzenia.
2. Przewiń w dół do **StorSimple Snapshot Manager** sekcji. Wprowadź hasło składające się z 14 do 15 znaków. Upewnij się, że hasło zawiera kombinację 3 lub więcej znaków wielkie litery, małe litery, liczbowych i specjalnych.
3. Potwierdź hasło.
4. Kliknij przycisk **Zapisz** w dolnej części strony.

Hasło programu StorSimple Snapshot Manager teraz powinny zostać uaktualnione.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [zabezpieczenia usługi StorSimple](storsimple-security.md).
* Dowiedz się więcej o [modyfikowanie konfiguracji urządzenia](storsimple-modify-device-config.md).
* Dowiedz się więcej o [przy użyciu usługi Menedżer StorSimple do administrowania urządzeniem StorSimple](storsimple-manager-service-administration.md).

