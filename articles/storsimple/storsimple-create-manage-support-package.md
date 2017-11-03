---
title: "Tworzenie pakietu obsługi StorSimple | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak tworzyć, odszyfrowywania i edytować pakiet obsługi dla urządzenia StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: eac76f5f-5db1-4c92-af8c-54053b91e66c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 32d20e7a8adcfc646c592213fe7395b87a93c985
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-a-storsimple-support-package"></a>Tworzenie i zarządzanie nimi pakietu obsługi StorSimple
## <a name="overview"></a>Omówienie
Pakiet obsługi StorSimple jest mechanizm łatwy w użyciu, która gromadzi wszystkie dzienniki odpowiednich pomagające Microsoft Support żadnych problemów urządzenia StorSimple. Dzienniki zbierane są zaszyfrowane i skompresowane.

Ten samouczek zawiera instrukcje krok po kroku, aby utworzyć i zarządzać pakietu obsługi.

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a>Utworzyć i załadować pakiet obsługi w klasycznym portalu Azure
Możesz utworzyć i załadować pakiet obsługi do witryny Microsoft Support za pomocą **konserwacji** strony usługi w klasycznym portalu Azure.

> [!NOTE]
> Przekazywanie wymaga klucza dostępu pomocy technicznej. Specjalistą pomocy technicznej powinien zawierać to dla Ciebie w wiadomości e-mail.
> 
> 

Obsługa zaszyfrowane i skompresowane pakiet (plik .cab) jest utworzony i przekazać do witryny sieci. Z pracownikiem pomocy technicznej może następnie pobrać tego pakietu z witrynę pomocy technicznej, aby rozwiązać problem.

Wykonaj następujące kroki w klasycznym portalu, aby utworzyć pakiet pomocy technicznej.

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a>Aby utworzyć pakiet pomocy technicznej w klasycznym portalu Azure
1. Wybierz **urządzeń** > **konserwacji**.
2. W **pakietu obsługi** zaznacz **tworzenie i przekazywanie pakietu obsługi**.
3. W **tworzenie i przekazywanie pakietu obsługi** okna dialogowego pole, wykonaj następujące czynności:
   
    ![Utwórz pakiet pomocy technicznej](./media/storsimple-create-manage-support-package/IC740923.png)
   
   * W **klucz dostępu pomocy technicznej** tekst Wprowadź klucz dostępu. Specjalistą pomocy technicznej firmy Microsoft należy wysłać ten klucz dostępu do Ciebie w wiadomości e-mail.
   * Zaznacz pole wyboru, aby zapewnić zgody na automatyczne przekazywanie pakietu obsługi w witrynie Microsoft Support.
   * Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-create-manage-support-package/IC740895.png).

## <a name="manually-create-a-support-package"></a>Ręcznie Utwórz pakiet pomocy technicznej
W niektórych przypadkach należy ręcznie utworzyć pakiet pomocy technicznej za pośrednictwem programu Windows PowerShell dla urządzenia StorSimple. Na przykład:

* Jeśli musisz usunąć poufne informacje z plików dziennika przed udostępnianie Support firmy Microsoft.
* Jeśli masz problemy przekazywanie pakietu z powodu problemów z łącznością.

Możesz udostępniać pakietu ręcznie wygenerowane pomocy technicznej firmy Microsoft Support za pośrednictwem poczty e-mail. Wykonaj poniższe kroki, aby utworzyć pakiet obsługi w programie Windows PowerShell dla StorSimple.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Aby utworzyć pakiet pomocy technicznej w programie Windows PowerShell dla urządzenia StorSimple
1. Aby rozpocząć sesję programu Windows PowerShell jako administrator na komputerze zdalnym, który jest używany do nawiązania połączenia urządzenia StorSimple, wprowadź następujące polecenie:
   
    `Start PowerShell`
2. W sesji środowiska Windows PowerShell połączenie z konsolą SSAdmin urządzenia:
   
   * W wierszu polecenia wpisz:
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   * W otwartym oknie dialogowym Wprowadź hasło administratora urządzenia. Domyślne hasło jest:
     
      `Password1`
     
      ![Okno dialogowe poświadczenie programu PowerShell](./media/storsimple-create-manage-support-package/IC740962.png)
   * Kliknij przycisk **OK**.
   * W wierszu polecenia wpisz:
     
      `Enter-PSSession $MS`
3. W sesji, który zostanie otwarty wprowadź odpowiednie polecenie.
   
   * Udziały sieciowe, które są chronione hasłem wpisz:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       Zostanie wyświetlony monit o podanie hasła, ścieżka do udostępnionego folderu sieciowego i hasło szyfrowania (ponieważ pakietu obsługi jest zaszyfrowany). Pakiet obsługi jest tworzona w określonym folderze.
   * W przypadku udziałów, które nie są chronione hasłem, nie trzeba `-Credential` parametru. Wprowadź następujące informacje:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       Dla obu kontrolerów w udostępnionym folderze sieciowym określonym zostaje utworzony pakiet pomocy technicznej. Jest to plik zaszyfrowanych, skompresowanych, który można wysłać do firmy Microsoft Support do rozwiązywania problemów. Aby uzyskać więcej informacji, zobacz [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).

### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>Parametry polecenia cmdlet Export-HcsSupportPackage
Można używać następujących parametrów, za pomocą polecenia cmdlet Export-HcsSupportPackage.

| Parametr | Wymagane opcjonalne | Opis |
| --- | --- | --- |
| `-Path` |Wymagane |Użyj, aby podać lokalizację folderu udostępnionego sieci, w której znajduje się pakiet pomocy technicznej. |
| `-EncryptionPassphrase` |Wymagane |Użyj, aby podać hasło, aby zaszyfrować pakiet pomocy technicznej. |
| `-Credential` |Optional (Opcjonalność) |Użyj, aby podać poświadczenia dostępu do udostępnionego folderu sieciowego. |
| `-Force` |Optional (Opcjonalność) |Użyj, aby pominąć krok potwierdzenie hasła szyfrowania. |
| `-PackageTag` |Optional (Opcjonalność) |Użyj, aby określić katalog w *ścieżki* w znajduje się pakiet pomocy technicznej. Wartość domyślna to [nazwa]-[bieżącą datę i time:yyyy-MM-dd-HH-mm-ss]. |
| `-Scope` |Optional (Opcjonalność) |Określ jako **klastra** (ustawienie domyślne), aby utworzyć pakiet pomocy technicznej dla obu kontrolerów. Jeśli chcesz utworzyć pakiet tylko dla bieżącego kontrolera, określ **kontrolera**. |

## <a name="edit-a-support-package"></a>Edytuj pakiet pomocy technicznej
Po wygenerowaniu pakietu obsługi może być konieczne Edytuj pakiet, aby usunąć informacje poufne. Mogą to być nazwy woluminów, urządzenia adresów IP i nazwy kopii zapasowej z plików dziennika.

> [!IMPORTANT]
> Można edytować tylko pakietu obsługi, który został wygenerowany za pomocą programu Windows PowerShell dla StorSimple. Nie można edytować pakiet utworzony w klasycznym portalu Azure w usłudze Menedżer StorSimple.
> 
> 

Aby edytować pakiet obsługi przed przekazaniem go w witrynie Microsoft Support, najpierw odszyfrować pakiet pomocy technicznej, Edytuj pliki i następnie ponownie go zaszyfrować. Wykonaj poniższe kroki.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>Aby edytować pakiet obsługi w programie Windows PowerShell dla urządzenia StorSimple
1. Generowanie pakietu obsługi, jak opisano wcześniej, w [do utworzenia pakietu obsługi w programie Windows PowerShell dla urządzenia StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).
2. [Pobierz skrypt](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokalnie na komputerze klienckim.
3. Zaimportuj moduł programu Windows PowerShell. Określ ścieżkę do folderu lokalnego, w której skrypt został pobrany. Aby zaimportować moduł, wpisz:
   
    `Import-module <Path to the folder that contains the Windows PowerShell script>`
4. Wszystkie pliki są *.aes* pliki skompresowane i szyfrowane. Aby zdekompresować i odszyfrowuje pliki, wpisz:
   
    `Open-HcsSupportPackage <Path to the folder that contains support package files>`
   
    Należy pamiętać, że rzeczywisty plik rozszerzenia są teraz wyświetlane dla wszystkich plików.
   
    ![Edytuj pakiet, pomocy technicznej](./media/storsimple-create-manage-support-package/IC750706.png)
5. Gdy zostanie wyświetlony monit o hasło szyfrowania, wprowadź hasło, które są używane podczas tworzenia pakietu obsługi.
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for the following parameters:EncryptionPassphrase: ****
6. Przejdź do folderu, który zawiera pliki dziennika. Ponieważ pliki dziennika są teraz zdekompresować i odszyfrować, te będą mieć oryginalnego rozszerzenia pliku. Zmodyfikuj te pliki, aby usunąć wszelkie informacje specyficzne dla klienta, takich jak nazwy woluminu i adresy IP urządzeń i zapisać pliki.
7. Zamknij pliki, aby kompresować gzip i szyfrowanie AES 256. Jest to szybkości i zabezpieczeń w przesyłania pakietu pomocy technicznej za pośrednictwem sieci. Podczas kompresji i szyfrowania plików, wprowadź następujące informacje:
   
    `Close-HcsSupportPackage <Path to the folder that contains support package files>`
   
    ![Edytuj pakiet, pomocy technicznej](./media/storsimple-create-manage-support-package/IC750707.png)
8. Po wyświetleniu monitu podaj hasło szyfrowania dla pakietu obsługi zmodyfikowane.
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****
9. Zapisz nowe hasło, dzięki czemu można udostępniać Microsoft Support na żądanie.

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Przykład: Edytowanie plików w pakiecie pomocy technicznej w udziale chronione hasłem
Poniższy przykład pokazuje, jak można odszyfrować, edytować i ponownie zaszyfrować pakiet obsługi.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [użycie pakietów pomocy technicznej i dzienniki urządzenia do rozwiązywania problemów z wdrożeniem urządzenia](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).
* Dowiedz się, jak [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

